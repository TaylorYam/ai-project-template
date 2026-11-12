# Domain docs / Domain Docs

> 本文件中英並列：上半為繁體中文，下半為英文。修改時請同步更新兩區。
>
> This document is bilingual: Traditional Chinese first, English below. Update both sections together.
>
> 本文件改寫自 [mattpocock/skills](https://github.com/mattpocock/skills) 的 `setup-matt-pocock-skills` 範本（commit `4ae2317`），授權條款見 [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)。
>
> Adapted from the `setup-matt-pocock-skills` templates in [mattpocock/skills](https://github.com/mattpocock/skills) (commit `4ae2317`). See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for the license.

[繁體中文](#繁體中文) | [English](#english)

## 繁體中文

本文件規定 engineering skill 在探索程式碼時，如何使用本 repo 的領域文件。

**本 repo 的結構：single-context。**

### 探索之前，先讀這些文件

- 根目錄的 **`GLOSSARY.md`**。如果根目錄有 **`GLOSSARY-MAP.md`**，改讀它；它會指向每個 context 各自的 `GLOSSARY.md`，讀和主題相關的那幾份。
- **`docs/adr/`**：讀和這次要改的範圍有關的 ADR。multi-context 的 repo 也要檢查 `src/<context>/docs/adr/` 裡的 context 專屬決策。

以上檔案不存在時，**直接繼續，不要提出**。不要提醒檔案不存在，也不要建議先建立。`/domain-modeling` skill（透過 `/grill-with-docs` 與 `/improve-codebase-architecture` 呼叫）會在術語或決策確定時才建立這些檔案。

### 檔案結構

Single-context repo（大多數 repo，包括本 repo）：

```
/
├── GLOSSARY.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

Multi-context repo（根目錄有 `GLOSSARY-MAP.md`）：

```
/
├── GLOSSARY-MAP.md
├── docs/adr/                          ← 整個系統的決策
└── src/
    ├── ordering/
    │   ├── GLOSSARY.md
    │   └── docs/adr/                  ← context 專屬的決策
    └── billing/
        ├── GLOSSARY.md
        └── docs/adr/
```

### 使用 glossary 的用詞

產出內容提到領域概念時（例如 Issue 標題、重構提案、假設、測試名稱），使用 `GLOSSARY.md` 定義的詞。不要改用 glossary 明確避免的同義詞。

需要的概念不在 glossary 裡時，代表兩種情況之一：你在發明專案沒有使用的詞（請重新考慮），或 glossary 真的缺少這個詞（記下來，交給 `/domain-modeling`）。

### 標示和 ADR 的衝突

產出內容和既有 ADR 衝突時，要明確指出，不要默默推翻：

> _和 ADR-0007（event-sourced orders）衝突，但值得重新討論，因為……_

---

## English

How the engineering skills should consume this repo's domain documentation when exploring the codebase.

**Layout for this repo: single-context.**

### Before exploring, read these

- **`GLOSSARY.md`** at the repo root, or
- **`GLOSSARY-MAP.md`** at the repo root if it exists: it points at one `GLOSSARY.md` per context. Read each one relevant to the topic.
- **`docs/adr/`**: read ADRs that touch the area you're about to work in. In multi-context repos, also check `src/<context>/docs/adr/` for context-scoped decisions.

If any of these files don't exist, **proceed silently**. Don't flag their absence; don't suggest creating them upfront. The `/domain-modeling` skill (reached via `/grill-with-docs` and `/improve-codebase-architecture`) creates them lazily when terms or decisions actually get resolved.

### File structure

Single-context repo (most repos, including this one):

```
/
├── GLOSSARY.md
├── docs/adr/
│   ├── 0001-event-sourced-orders.md
│   └── 0002-postgres-for-write-model.md
└── src/
```

Multi-context repo (presence of `GLOSSARY-MAP.md` at the root):

```
/
├── GLOSSARY-MAP.md
├── docs/adr/                          ← system-wide decisions
└── src/
    ├── ordering/
    │   ├── GLOSSARY.md
    │   └── docs/adr/                  ← context-specific decisions
    └── billing/
        ├── GLOSSARY.md
        └── docs/adr/
```

### Use the glossary's vocabulary

When your output names a domain concept (in an issue title, a refactor proposal, a hypothesis, a test name), use the term as defined in `GLOSSARY.md`. Don't drift to synonyms the glossary explicitly avoids.

If the concept you need isn't in the glossary yet, that's a signal: either you're inventing language the project doesn't use (reconsider) or there's a real gap (note it for `/domain-modeling`).

### Flag ADR conflicts

If your output contradicts an existing ADR, surface it explicitly rather than silently overriding:

> _Contradicts ADR-0007 (event-sourced orders), but worth reopening because…_
