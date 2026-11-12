# Triage label / Triage Labels

> 本文件中英並列：上半為繁體中文，下半為英文。修改時請同步更新兩區。
>
> This document is bilingual: Traditional Chinese first, English below. Update both sections together.
>
> 本文件改寫自 [mattpocock/skills](https://github.com/mattpocock/skills) 的 `setup-matt-pocock-skills` 範本（commit `4ae2317`），授權條款見 [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md)。
>
> Adapted from the `setup-matt-pocock-skills` templates in [mattpocock/skills](https://github.com/mattpocock/skills) (commit `4ae2317`). See [`THIRD_PARTY_NOTICES.md`](THIRD_PARTY_NOTICES.md) for the license.

[繁體中文](#繁體中文) | [English](#english)

## 繁體中文

skill 用 5 個標準 triage 角色描述 Issue 的狀態。本表把這些角色對應到本 repo issue tracker 實際使用的 label。

| mattpocock/skills 的 label | 本 repo 的 label | 意義 |
| --- | --- | --- |
| `needs-triage` | `needs-triage` | 維護者需要評估這個 Issue |
| `needs-info` | `needs-info` | 等待回報者補充資訊 |
| `ready-for-agent` | `ready-for-agent` | 規格完整，可以交給 agent 獨立實作 |
| `ready-for-human` | `ready-for-human` | 需要由人實作 |
| `wontfix` | `wontfix` | 不會處理 |

skill 提到某個角色時（例如「套用可交給 agent 的 triage label」），使用本表對應的 label 字串。

要改用其他 label 名稱時，修改「本 repo 的 label」欄。

新 repo 可能還沒有這些 label。建立方式見 `README.md` 的「AI skills」段落。

---

## English

The skills speak in terms of five canonical triage roles. This file maps those roles to the actual label strings used in this repo's issue tracker.

| Label in mattpocock/skills | Label in our tracker | Meaning |
| --- | --- | --- |
| `needs-triage` | `needs-triage` | Maintainer needs to evaluate this issue |
| `needs-info` | `needs-info` | Waiting on reporter for more information |
| `ready-for-agent` | `ready-for-agent` | Fully specified, ready for an AFK agent |
| `ready-for-human` | `ready-for-human` | Requires human implementation |
| `wontfix` | `wontfix` | Will not be actioned |

When a skill mentions a role (e.g. "apply the AFK-ready triage label"), use the corresponding label string from this table.

Edit the "Label in our tracker" column to match whatever vocabulary you actually use.

A new repository may not have these labels yet. See the "AI skills" section in `README.md` to create them.
