## [What to look for in code review](https://google.github.io/eng-practices/review/reviewer/)
#### 設計
- 此次更動了什麼程式
#### 功能性
- 此更動的影響，是否有bug
- 可能的極端案例、找出併發問題、嘗試作為一個使用者去思考
- deadlock跟race condition問題須留意
#### 複雜性
- 此次更動是否複雜難以閱讀
- 無需過度設計。*鼓勵開發者解決`現在`需要解決的問題，而`非`猜測未來可能需要被解決的問題*
#### 測試 
- 測試程式的必要性
#### 命名
- 變數或函式名稱是否合理且足以表達、不含糊
#### 註釋
- 解釋why而不是what
- 若需解釋what，就回到`複雜性`的review
- 除了正規表達式及演算法，可直接加註釋
#### 風格
- 風格需一致 ex. eslint
#### 文件
- 更動內容是否同步在文件上
#### 每一行程式
- 每行程式碼都需要掃過

## how to do in code review efficiently
1. 先大致瀏覽整個 CL
2. 檢視這個 CL 最重要的部份
3. 依照適當的順序看其餘的部份

## how to write code review comments
- 說明原因
- 提供指引
