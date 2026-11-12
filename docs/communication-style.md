# 寫作規範 / Writing Style

> 本文件中英並列：上半為繁體中文，下半為英文。修改時請同步更新兩區。
>
> This document is bilingual: Traditional Chinese first, English below. Update both sections together.

[繁體中文](#繁體中文) | [English](#english)

## 繁體中文

本規範以 ASD-STE100（Simplified Technical English，簡化技術英文）的寫作規則為基礎。ASD-STE100 是為英文設計的受控語言。它的核准詞彙表無法直接用在中文，所以本專案只採用它的結構規則，並改寫成中文版本。本規範不代表符合 ASD-STE100 標準。

### 適用範圍

AI 產出的以下文字要遵守本規範：

- 與使用者的對話。
- Issue 與 PR 內文。
- commit message。
- 程式註解與文件。

規則只用在新寫或修改的文字。不需要回頭改寫既有內容。

### 句子

1. 一句只寫一個重點。
2. 步驟句（告訴讀者做什麼）最多 40 個字。描述句（說明事實或原因）最多 50 個字。
3. 計算字數時，一個程式碼、路徑、指令或英文術語算一個字。
4. 用主動語態。寫出由誰做這件事。執行者不重要時，才可以省略。
5. 不用雙重否定、成語、譬喻與口語。

字數上限是本專案自訂的換算。STE 原本的規定是英文步驟句最多 20 個英文字，描述句最多 25 個英文字。

### 段落

1. 一個段落只談一個主題。
2. 一個段落最多 6 句。

### 步驟

1. 步驟用編號清單。
2. 一個步驟只寫一個動作。
3. 每個步驟用動詞開頭，例如「執行」、「開啟」、「刪除」。
4. 有條件時，先寫條件，再寫動作。例如：「若測試失敗，檢查 log。」

### 用詞

1. 同一個概念固定用同一個詞。不要為了變化而換成同義詞。
2. 用意思明確的動詞。避免「處理」、「優化」、「調整」這類意思太廣的詞，除非後面接具體說明。
3. 「這個」、「它」、「上述」要能清楚指到一個對象。指不清楚時，直接重寫名詞。
4. 有確切數字時，寫出數字與單位。不要用「一些」、「大約」代替。
5. 不常見的術語第一次出現時，簡短解釋。

### 警告與注意事項

1. 先寫要做或不要做的動作。
2. 再寫原因或可能的後果。

例如：「不要把 `.env` 加入 Git。檔案裡的金鑰會外洩。」

### 例外

以下內容保留原文，不依本規範改寫：

- 程式碼、指令、路徑、設定值。
- 錯誤訊息與 log。
- 引用的外部文字。
- 函式、類別、API 與函式庫名稱。

### 範例

不符合：

> 這個問題我們應該可以透過稍微調整一下設定檔來處理，不過要注意不要讓測試被影響到。

符合：

> 修改 `config.yml` 的 `timeout` 值，改成 30 秒。然後執行測試。確認測試全部通過。

---

## English

This guide is based on the writing rules of ASD-STE100 (Simplified Technical English). ASD-STE100 is a controlled language for English. Its approved dictionary does not apply to Chinese. Thus, this project uses only its structural rules and adapts them for Chinese. This guide does not claim compliance with the ASD-STE100 standard.

### Scope

AI-written text of these types must follow this guide:

- Conversations with the user.
- Issue and PR text.
- Commit messages.
- Code comments and documentation.

Apply these rules to new or changed text only. Do not rewrite existing text for this reason.

### Sentences

1. Write only one topic in each sentence.
2. Procedural sentences (tell the reader what to do) have a maximum of 40 Chinese characters. Descriptive sentences (give facts or reasons) have a maximum of 50 Chinese characters.
3. When you count characters, count one code item, path, command, or English term as one character.
4. Use the active voice. Write who does the action. Omit the person who does the action only when that person is not important.
5. Do not use double negatives, idioms, metaphors, or slang.

The Chinese length limits are a conversion that this project defines. The original STE rule for English is a maximum of 20 words in procedural sentences and 25 words in descriptive sentences. English text in this repository uses these STE word limits.

### Paragraphs

1. Write only one topic in each paragraph.
2. A paragraph has a maximum of 6 sentences.

### Procedures

1. Use a numbered list for steps.
2. Write only one action in each step.
3. Start each step with a verb, for example "Run", "Open", or "Delete".
4. If there is a condition, write the condition first and the action second. Example: "If the test fails, examine the log."

### Words

1. Use the same term for the same concept every time. Do not use synonyms for variation.
2. Use verbs with a clear meaning. Do not use broad verbs such as "handle", "optimize", or "adjust" without a specific description.
3. Make sure that "this", "it", and "the above" refer to one clear item. If the reference is not clear, write the noun again.
4. When you know the exact number, write the number and the unit. Do not use "some" or "about" instead.
5. When you use an unusual term for the first time, give a short explanation.

### Warnings and cautions

1. Write the action to do or not to do first.
2. Then write the reason or the possible result.

Example: "Do not add `.env` to Git. The keys in the file can become public."

### Exceptions

Keep the original text for these items. Do not change them to follow this guide:

- Code, commands, paths, and configuration values.
- Error messages and logs.
- Quoted external text.
- Function, class, API, and library names.

### Example

Incorrect:

> We can probably handle this issue by tweaking the config file a bit, but be careful not to let the tests get affected.

Correct:

> Change the `timeout` value in `config.yml` to 30 seconds. Then run the tests. Make sure that all tests pass.
