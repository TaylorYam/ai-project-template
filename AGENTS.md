# AI 開發指南 / AI Development Guide

> 本文件中英並列：上半為繁體中文，下半為英文，兩者內容相同。修改時兩區必須同步更新；若內容不一致，以英文版為準。
>
> This document is bilingual: Traditional Chinese first, English below, with the same content. Update both sections together; if they differ, the English version prevails.

[繁體中文](#繁體中文) | [English](#english)

## 繁體中文

這份位於根目錄的檔案是本 repository 共用的 AI 開發政策。它定義**何時**使用各個工作流程；skill 則定義**如何**執行。若明確的任務要求縮小或擴充本指南的範圍，以該要求為準。

### 工作分流

建立 Issue 之前，先選定路線。每條路線由下方「步驟」組成。

#### 步驟

每個步驟都能手動執行。有安裝 [mattpocock/skills](https://github.com/mattpocock/skills) 時，使用「skill」欄的 skill。沒有安裝時，照「做法」欄執行。不要因為缺少 skill 就跳過步驟。使用者沒有要求時，不要自行安裝 skill。

| 步驟 | 做法 | 產出 | skill |
|---|---|---|---|
| 釐清需求 | 一次問一個問題，只問真正不確定的地方。每個問題附上建議答案。能從程式碼查到的事，不要問使用者。 | 已確認的目標、範圍、驗收條件與決策 | `grill-with-docs` |
| 寫成規格 | 把已確認的結論寫成一個可直接實作的 GitHub Issue。寫明目標、範圍、驗收條件與不在範圍內的事。 | 一個 GitHub Issue | `to-spec` |
| 拆分工作 | 只在一個 Issue／PR 裝不下時使用。把工作拆成可以各自 merge 的子 Issue，並寫明相依關係。 | 多個子 Issue | `to-tickets` |
| 診斷 bug | 1. 建立重現方式。2. 縮小範圍。3. 列出假設。4. 加入觀測。5. 找出根本原因。6. 確認正確行為。假設與已確認的原因分開記錄。 | 重現方式、根本原因、正確行為 | `diagnosing-bugs` |
| Triage | 確認 Issue 的內容成立；bug 要先重現。替 Issue 分類。資訊不足時，提出具體的問題。依 [`docs/agents/triage-labels.md`](docs/agents/triage-labels.md) 套用 label。 | 已分類的 Issue | `triage` |
| 實作 | 依 Issue 實作。行為改變時，先新增或更新測試。執行相關檢查。 | 通過檢查的變更 | `implement` |
| 審查 | 開 PR 之前，檢查 diff 的正確性與範圍。修正發現的問題。 | 審查過的 diff | `code-review` |

#### 路線

- 新功能、重要需求不明確：釐清需求 → 寫成規格 → 拆分工作（只在需要時）→ 實作 → 審查。
- 新功能、需求明確：檢查相關程式碼 → 寫成規格 → 實作 → 審查。
- Bug、原因與正確行為都明確：檢查相關程式碼 → 寫成規格 → 實作 → 審查。合理時加入回歸測試（regression test）。
- Bug、原因不明：診斷 bug → 寫成規格 → 實作 → 審查。
- 外部或他人提出的 Issue：Triage → 套用 `ready-for-agent` → 實作 → 審查。

Triage 主要用於他人建立的 Issue。本 agent 在「寫成規格」或「拆分工作」步驟建立的 Issue，已經可以直接實作，不需要再 triage。

判斷方式：不知道**要做什麼** → 釐清需求；知道**要做什麼** → 寫成規格 → 實作；不知道 **bug 為什麼發生** → 診斷 bug；明確的 bug → 寫成規格 → 實作；外部 Issue → Triage；工作量過大 → 拆分工作。

#### Agent skills 設定

本樣板已經預先完成 `/setup-matt-pocock-skills` 的設定。不需要再執行這個 skill。

- Issue tracker：GitHub Issues。見 [`docs/agents/issue-tracker.md`](docs/agents/issue-tracker.md)。
- Triage label：使用預設的 5 個 label。見 [`docs/agents/triage-labels.md`](docs/agents/triage-labels.md)。
- Domain docs：single-context。見 [`docs/agents/domain.md`](docs/agents/domain.md)。

### 修改檔案之前

- 檢查目前的 branch 與工作目錄狀態。一般專案工作應從最新的 `main` 開始，並在編輯前建立任務 branch 或 worktree。
- 有附上 Issue 時先閱讀。閱讀 `README.md`、`docs/architecture.md` 與相關 ADR，並檢查相關的實作與測試。
- 較大的工作在編輯前先說明預期成果、可能修改的檔案、做法、風險與驗證方式。遵守使用者明確設定的核准界線。
- 若 Issue 或規格（spec）留下未決定、且會實質影響行為、資料正確性、使用者體驗、架構、資安、付款或真實交易、刪除、API 行為或向下相容性的重要選擇，應停在規劃階段，用淺白的繁體中文詢問使用者。低風險、可回復的實作選擇可直接決定，不必打斷。

### GitHub 優先工作流程

`main` 保持為穩定的整合 branch。規劃中的工作依照以下流程：

`Issue → branch 或 worktree → 規劃 → 實作 → 驗證 → commit 與 push → pull request → review 與 CI → merge`

- 實作前先建立可直接實作的 Issue，寫清楚目標、範圍與可觀察的驗收條件。Agent 建立的 Issue 標題與內文所有段落預設使用繁體中文（台灣用語），包含「寫成規格」建立的 Issue 與「拆分工作」建立的子 Issue。正式技術識別名稱保留英文。既有的英文 Issue 不需改寫；直接與說英文的外部貢獻者溝通時使用英文。
- Branch 名稱要能看出用途，例如 `feat/export-report` 或 `fix/timezone-boundary`。
- 變更聚焦在對應的 Issue，並在 PR 中記錄重要的使用者可見或架構上的取捨。
- 執行相關檢查；行為改變時新增或更新測試，並回報無法執行的檢查。
- 在 PR 中連結 Issue。必要的 review 與檢查通過後才 merge。
- 若是尚未建立預設 branch 的初始（bootstrap）repository，可依需要直接建立基準內容；之後的一般工作改用 branch 與 PR。

### 溝通

- 與使用者的一般對話（包含「釐清需求」步驟）使用簡潔、具體、淺白的繁體中文（台灣用語）。用淺白的語言提出深入的問題。
- 有幫助時使用技術術語。Git、Branch、PR、API、Database、SQL、CI 等常見術語不需強行翻譯。
- 對話、Issue、PR、commit message、程式註解與文件，都依 [`docs/communication-style.md`](docs/communication-style.md) 撰寫。這份規範改寫自 ASD-STE100 的結構規則：一句一個重點、限制句長、用主動語態、步驟用編號清單、同一個概念固定用同一個詞。

### 設計與實作

- 遵循既有的架構、命名、格式與相依套件，除非任務本身要改變它們。以符合驗收條件的最小且一致的變更為原則；避免無關的重構與新增相依套件。
- 行為、設定方式或介面改變時更新文件。持久的架構決策依 `docs/adr/` 的格式記錄。
- 中英並列的文件（`AGENTS.md`、`CLAUDE.md`、`README.md`、`docs/`）修改時，中文與英文兩區必須同步更新。

### 機密與本機資料

- 憑證、token、金鑰、密碼與填好值的 `.env` 檔不得進入 Git、log 或範例。`.env.example` 只放名稱與安全的佔位值；真實值放在核准的機密儲存處或本機環境。
- 產生的資料、快取、建置產出與特定機器的設定不要 commit，除非專案把該產出視為原始碼。
- 若在工作目錄或歷史紀錄中發現機密，立即停止並回報，不要複製或重述其內容。

### 完成

交回工作前，檢查 diff、執行適用的驗證，並總結變更內容、測試結果與限制。不要動到與任務無關的使用者變更。

---

## English

This root-level file is the repository's shared AI development policy. It defines **when** to use each workflow; skills define **how** to carry it out. Follow an explicit task request when it narrows or extends this guide.

### Work routing

Choose the route before you create an Issue. Each route is a sequence of the steps below.

#### Steps

You can do each step manually. If [mattpocock/skills](https://github.com/mattpocock/skills) is installed, use the skill in the "Skill" column. If it is not installed, do the step as the "Method" column describes. Do not skip a step because a skill is missing. Do not install skills unless the user asks.

| Step | Method | Output | Skill |
|---|---|---|---|
| Clarify | Ask one question at a time. Ask only about genuine uncertainties. Give a recommended answer with each question. Do not ask the user about facts that the code shows. | Confirmed goal, scope, acceptance criteria, and decisions | `grill-with-docs` |
| Spec | Write the confirmed result as one implementation-ready GitHub Issue. Include the goal, scope, acceptance criteria, and out-of-scope items. | One GitHub Issue | `to-spec` |
| Tickets | Use only when one Issue / PR cannot hold the work. Split the work into child Issues that can merge separately. Record the dependencies. | Child Issues | `to-tickets` |
| Diagnose | 1. Make a reproduction. 2. Make the scope smaller. 3. List hypotheses. 4. Add instrumentation. 5. Find the root cause. 6. Confirm the correct behavior. Keep hypotheses separate from confirmed causes. | Reproduction, root cause, correct behavior | `diagnosing-bugs` |
| Triage | Make sure that the Issue is valid; reproduce a bug first. Categorize the Issue. If information is missing, ask specific questions. Apply labels as [`docs/agents/triage-labels.md`](docs/agents/triage-labels.md) specifies. | Categorized Issue | `triage` |
| Implement | Implement the Issue. If behavior changes, add or update tests first. Run the relevant checks. | A change that passes the checks | `implement` |
| Review | Before you open a PR, examine the diff for correctness and scope. Fix the problems that you find. | A reviewed diff | `code-review` |

#### Routes

- New feature, important requirements unclear: Clarify → Spec → Tickets (only when necessary) → Implement → Review.
- New feature, requirements clear: inspect relevant code → Spec → Implement → Review.
- Bug, cause and correct behavior clear: inspect relevant code → Spec → Implement → Review. Add a regression test when reasonable.
- Bug, cause unclear: Diagnose → Spec → Implement → Review.
- External or incoming Issue: Triage → apply `ready-for-agent` → Implement → Review.

Use Triage mainly for Issues that other people create. An Issue that this agent creates in the Spec or Tickets step is ready for implementation. It does not need triage.

Decision guide: unknown **what to build** → Clarify; known **what to build** → Spec → Implement; unknown **why a bug occurs** → Diagnose; clear bug → Spec → Implement; external Issue → Triage; oversized work → Tickets.

#### Agent skills configuration

This template already contains the `/setup-matt-pocock-skills` configuration. Do not run that skill again.

- Issue tracker: GitHub Issues. See [`docs/agents/issue-tracker.md`](docs/agents/issue-tracker.md).
- Triage labels: the 5 default labels. See [`docs/agents/triage-labels.md`](docs/agents/triage-labels.md).
- Domain docs: single-context. See [`docs/agents/domain.md`](docs/agents/domain.md).

### Before changing files

- Check the current branch and working tree. For normal project work, start from an up-to-date `main` and create a task branch or worktree before editing.
- Read the linked Issue when one is provided. Read `README.md`, `docs/architecture.md`, and relevant ADRs. Inspect related implementation and tests.
- For substantial work, explain the outcome, likely files, approach, risks, and validation before editing. Follow explicit user approval boundaries.
- If an Issue or spec leaves an important choice unresolved that materially affects behavior, data correctness, user experience, architecture, security, payments or real transactions, deletion, API behavior, or backward compatibility, pause at planning and ask the user in plain Traditional Chinese. Make low-risk, reversible implementation choices without interruption.

### GitHub-first workflow

Keep `main` as the stable integration branch. For planned work, follow:

`Issue → branch or worktree → plan → implementation → validation → commit and push → pull request → review and CI → merge`

- Create an implementation-ready Issue before implementation, with a clear goal, scope, and observable acceptance criteria. Agent-created Issue titles and all body sections default to Traditional Chinese (Taiwan usage), including Issues from the Spec step and child Issues from the Tickets step. Keep formal technical identifiers in English. Existing English Issues need no rewrite; use English when communicating directly with an English-speaking external contributor.
- Give branches descriptive names, such as `feat/export-report` or `fix/timezone-boundary`.
- Keep changes focused on their Issue and record significant user-visible or architectural tradeoffs in the PR.
- Run relevant checks; add or update tests when behavior changes and report checks that could not run.
- Link the Issue in the PR. Merge after required review and checks pass.
- For a bootstrap repository without an established default branch, initialize the baseline as needed; use branches and PRs for regular work afterward.

### Communication

- Use concise, concrete, plain Traditional Chinese (Taiwan usage) in ordinary conversations with the user, including the Clarify step. Ask deep questions in plain language.
- Use technical terms when helpful. Common terms such as Git, Branch, PR, API, Database, SQL, and CI need no forced translation.
- Write conversations, Issues, PRs, commit messages, code comments, and documentation as [`docs/communication-style.md`](docs/communication-style.md) specifies. This guide adapts the structural rules of ASD-STE100: one topic in each sentence, limited sentence length, active voice, numbered steps, and the same term for the same concept.

### Design and implementation

- Follow existing architecture, naming, formatting, and dependencies unless the task changes them. Make the smallest coherent change that meets acceptance criteria; avoid unrelated refactors and dependencies.
- Update documentation when behavior, setup, or interfaces change. Record lasting architecture decisions in `docs/adr/` using its format.
- When editing a bilingual document (`AGENTS.md`, `CLAUDE.md`, `README.md`, `docs/`), update the Chinese and English sections together.

### Secrets and local data

- Keep credentials, tokens, keys, passwords, and populated `.env` files out of Git, logs, and examples. Put only names and safe placeholders in `.env.example`; use the approved secret store or local environment for real values.
- Keep generated data, caches, build output, and machine-specific configuration out of commits unless the project treats an artifact as source.
- If a secret appears in the working tree or history, stop and report it without copying or repeating it.

### Completion

Review the diff, run applicable validation, and summarize changes, test results, and limitations before handing work back. Leave unrelated user changes untouched.
