# Issue tracker：GitHub / Issue tracker: GitHub

> 本文件中英並列：上半為繁體中文，下半為英文。修改時請同步更新兩區。
>
> This document is bilingual: Traditional Chinese first, English below. Update both sections together.
>
> 本文件改寫自 [mattpocock/skills](https://github.com/mattpocock/skills) 的 `setup-matt-pocock-skills` 範本（commit `4ae2317`），授權條款見 [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)。
>
> Adapted from the `setup-matt-pocock-skills` templates in [mattpocock/skills](https://github.com/mattpocock/skills) (commit `4ae2317`). See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for the license.

[繁體中文](#繁體中文) | [English](#english)

## 繁體中文

本 repo 的 Issue 與規格都放在 GitHub Issues。所有操作都使用 `gh` CLI。

### 慣例

- **建立 Issue**：`gh issue create --title "..." --body "..."`。內文有多行時，用 heredoc。
- **讀取 Issue**：`gh issue view <number> --json number,title,body,labels,comments`。
- **列出 Issue**：`gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'`。依需要加上 `--label` 與 `--state` 篩選。
- **把 Issue 設為父 Issue 的子 Issue**：`gh issue create --parent <parent> ...`，或建立後執行 `gh issue edit <parent> --add-sub-issue <child>`（`gh` 2.94 以上）。舊版 `gh`：`gh api --method POST repos/<owner>/<repo>/issues/<parent>/sub_issues -F sub_issue_id=<child-db-id>`（database id，見下方「阻擋」）。無法使用子 Issue 時，在子 Issue 內文最上方寫 `Part of #<parent>`。
- **阻擋（Blocking）**：使用 GitHub 原生的 issue dependencies。這是標準做法，也會顯示在 UI 上。用 `gh issue create --blocked-by <n>,<n> ...` 或 `gh issue edit <child> --add-blocked-by <blocker>` 加入相依關係（`gh` 2.94 以上）。舊版 `gh`：`gh api --method POST repos/<owner>/<repo>/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-db-id>`。`<blocker-db-id>` 是阻擋者的數字 **database id**（`gh api repos/<owner>/<repo>/issues/<n> --jq .id`），**不是** `#number` 或 `node_id`。無法使用 dependencies 時，在子 Issue 內文最上方寫 `Blocked by: #<n>, #<n>`。所有阻擋者都關閉後，該 Issue 才算解除阻擋。
- **計算尚未關閉的阻擋者**：`gh api repos/<owner>/<repo>/issues/<n> --jq .issue_dependencies_summary.blocked_by`（只算未關閉的；0 表示可以開始）。`gh issue view <n> --json blockedBy` 也會列出已關閉的阻擋者，所以要篩選：`--jq '[.blockedBy.nodes[] | select(.state == "OPEN")] | length'`。
- **在 Issue 留言**：`gh issue comment <number> --body "..."`
- **套用／移除 label**：`gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **關閉**：先用 `gh issue comment <number> --body "..."` 寫出說明，再執行 `gh issue close <number>`。對已關閉的 Issue 執行 `gh issue close --comment` 時，留言會被丟棄，但指令仍然回傳 0。

從 `git remote -v` 判斷目標 repo。在 clone 的目錄內執行時，`gh` 會自動判斷。

只在使用者自己的 repo 建立 Issue。除非使用者擁有 `mattpocock/skills`，否則不要在那裡建立 Issue。在 fork 裡，`gh` 可能預設使用上游的父 repo。所以每個工作階段第一次執行 `gh issue create` 前，先用 `gh repo view --json nameWithOwner` 確認目標。目標不是使用者的 repo 時，先向使用者確認。

### 把 PR 當作 triage 來源

**把 PR 當作需求來源：否。**（本 repo 若把外部 PR 當作功能需求，改成「是」。`/triage` 會讀取這個設定。）

設為「是」時，PR 使用和 Issue 相同的 label 與狀態，並改用對應的 `gh pr` 指令：

- **讀取 PR**：`gh pr view <number>`，接著 `gh pr view <number> --comments`（不在 TTY 時，`--comments` 只會印出留言，不會印出內文），再用 `gh pr diff <number>` 看 diff。
- **列出要 triage 的外部 PR**：`gh api --paginate 'repos/{owner}/{repo}/pulls?state=open' --jq '.[] | select(.author_association | IN("OWNER","MEMBER","COLLABORATOR") | not) | {number, title, author: .user.login, author_association, labels: [.labels[].name]}'`。
- **留言／label／關閉**：`gh pr comment`、`gh pr edit --add-label`/`--remove-label`、`gh pr close`。

GitHub 的 Issue 與 PR 共用同一組編號，所以單獨的 `#42` 可能是兩者之一。先用 `gh pr view 42` 判斷，失敗時再用 `gh issue view 42`。

### skill 要求「發布到 issue tracker」時

建立一個 GitHub Issue。

### skill 要求「取得相關 ticket」時

照上方「讀取 Issue」的方式讀取。

### Wayfinding 操作

供 `/wayfinder` 使用。**Map** 是一個 Issue，**child** 是它底下的子 Issue。

- **Map**：一個加上 `wayfinder:map` label 的 Issue。內文包含 Notes／Decisions-so-far／Fog。指令：`gh issue create --label wayfinder:map`。
- **Child ticket**：以 GitHub 子 Issue 連到 map 的 Issue（見「把 Issue 設為父 Issue 的子 Issue」）。無法使用子 Issue 時，把 child 加到 map 內文的 task list，並在 child 內文最上方寫 `Part of #<map>`。Label：`wayfinder:<type>`（`research`/`prototype`/`grilling`/`task`）。認領後，指派給負責的開發者。
- **阻擋**：見「慣例」中的「阻擋」。
- **Frontier 查詢**：列出 map 底下未關閉的 child（`gh issue list --state open`，範圍限定在 map 的子 Issue 或 task list）。排除有未關閉阻擋者（見「計算尚未關閉的阻擋者」，或 `Blocked by` 行中未關閉的 Issue）或已有負責人的項目。依 map 的順序，取第一個。
- **認領**：`gh issue edit <n> --add-assignee @me`。這是工作階段的第一個寫入動作。
- **完成**：執行 `gh issue comment <n> --body "<answer>"`，再執行 `gh issue close <n>`，然後在 map 的 Decisions-so-far 加上一行重點與連結。

---

## English

Issues and specs for this repo live as GitHub issues. Use the `gh` CLI for all operations.

### Conventions

- **Create an issue**: `gh issue create --title "..." --body "..."`. Use a heredoc for multi-line bodies.
- **Read an issue**: `gh issue view <number> --json number,title,body,labels,comments`.
- **List issues**: `gh issue list --state open --json number,title,body,labels,comments --jq '[.[] | {number, title, body, labels: [.labels[].name], comments: [.comments[].body]}]'` with appropriate `--label` and `--state` filters.
- **Make an issue a sub-issue of a parent**: `gh issue create --parent <parent> ...`, or `gh issue edit <parent> --add-sub-issue <child>` afterwards (`gh` 2.94+). Older `gh`: `gh api --method POST repos/<owner>/<repo>/issues/<parent>/sub_issues -F sub_issue_id=<child-db-id>` (database id, as in **Blocking** below). Without sub-issues, put `Part of #<parent>` at the top of the child body.
- **Blocking**: GitHub's **native issue dependencies**, the canonical, UI-visible representation. Add an edge with `gh issue create --blocked-by <n>,<n> ...` or `gh issue edit <child> --add-blocked-by <blocker>` (`gh` 2.94+). Older `gh`: `gh api --method POST repos/<owner>/<repo>/issues/<child>/dependencies/blocked_by -F issue_id=<blocker-db-id>`, where `<blocker-db-id>` is the blocker's numeric **database id** (`gh api repos/<owner>/<repo>/issues/<n> --jq .id`, _not_ the `#number` or `node_id`). Where dependencies aren't available, fall back to a `Blocked by: #<n>, #<n>` line at the top of the child body. A ticket is unblocked when every blocker is closed.
- **Count open blockers**: `gh api repos/<owner>/<repo>/issues/<n> --jq .issue_dependencies_summary.blocked_by` (open blockers only; 0 means startable). `gh issue view <n> --json blockedBy` also lists closed blockers, so filter it: `--jq '[.blockedBy.nodes[] | select(.state == "OPEN")] | length'`.
- **Comment on an issue**: `gh issue comment <number> --body "..."`
- **Apply / remove labels**: `gh issue edit <number> --add-label "..."` / `--remove-label "..."`
- **Close**: post the explanation with `gh issue comment <number> --body "..."`, then `gh issue close <number>`. On an already-closed issue, `gh issue close --comment` drops the comment and still exits 0.

Infer the repo from `git remote -v`; `gh` does this automatically when run inside a clone.

File issues only into the user's own repo. Never file into `mattpocock/skills` (the repo these skills come from) unless the user owns it. In a fork, `gh` can default to the upstream parent, so before the first `gh issue create` of a session, check the target with `gh repo view --json nameWithOwner` and confirm it with the user if it is not their repo.

### Pull requests as a triage surface

**PRs as a request surface: no.** _(Set to `yes` if this repo treats external PRs as feature requests; `/triage` reads this flag.)_

When set to `yes`, PRs run through the same labels and states as issues, using the `gh pr` equivalents:

- **Read a PR**: `gh pr view <number>`, then `gh pr view <number> --comments` (off a TTY, `--comments` prints only the comments, not the body), and `gh pr diff <number>` for the diff.
- **List external PRs for triage**: `gh api --paginate 'repos/{owner}/{repo}/pulls?state=open' --jq '.[] | select(.author_association | IN("OWNER","MEMBER","COLLABORATOR") | not) | {number, title, author: .user.login, author_association, labels: [.labels[].name]}'`.
- **Comment / label / close**: `gh pr comment`, `gh pr edit --add-label`/`--remove-label`, `gh pr close`.

GitHub shares one number space across issues and PRs, so a bare `#42` may be either: resolve with `gh pr view 42` and fall back to `gh issue view 42`.

### When a skill says "publish to the issue tracker"

Create a GitHub issue.

### When a skill says "fetch the relevant ticket"

Read it as in **Read an issue** above.

### Wayfinding operations

Used by `/wayfinder`. The **map** is a single issue with **child** issues as tickets.

- **Map**: a single issue labelled `wayfinder:map`, holding the Notes / Decisions-so-far / Fog body. `gh issue create --label wayfinder:map`.
- **Child ticket**: an issue linked to the map as a GitHub sub-issue (see **Make an issue a sub-issue of a parent**). Where sub-issues aren't enabled, add the child to a task list in the map body and put `Part of #<map>` at the top of the child body. Labels: `wayfinder:<type>` (`research`/`prototype`/`grilling`/`task`). Once claimed, the ticket is assigned to the driving dev.
- **Blocking**: see **Blocking** under Conventions.
- **Frontier query**: list the map's open children (`gh issue list --state open`, scoped to the map's sub-issues / task list), drop any with an open blocker (see **Count open blockers**, or an open issue in the `Blocked by` line) or an assignee; first in map order wins.
- **Claim**: `gh issue edit <n> --add-assignee @me`, the session's first write.
- **Resolve**: `gh issue comment <n> --body "<answer>"`, then `gh issue close <n>`, then append a context pointer (gist + link) to the map's Decisions-so-far.
