## 2025-10-21 15:45:00

### 1. 修改 interactive_feedback 工具的默認超時時間

**Change Type**: improvement

> **Purpose**: 將 interactive_feedback 工具的默認超時時間從 10 分鐘改為 24 小時
> **Detailed Description**:
>   1. 修改 `src/mcp_feedback_enhanced/server.py` 第 659 行，將 timeout 默認值從 600 秒改為 86400 秒
>   2. 更新函數文檔字符串，說明新的默認值為 24 小時
>   3. 更新相關文檔文件中的 timeout 默認值說明
> **Reason for Change**:
>   - 用戶需要更長的超時時間來完成複雜的反饋和測試
>   - 24 小時的默認值更符合實際使用場景
> **Impact Scope**:
>   - `src/mcp_feedback_enhanced/server.py` - 主要修改
>   - `docs/architecture/api-reference.md` - 文檔更新
>   - `docs/architecture/deployment-guide.md` - 文檔更新
>   - `docs/architecture/interaction-flows.md` - 文檔更新
> **API Changes**:
>   - 舊：timeout 默認值 = 600 秒（10 分鐘）
>   - 新：timeout 默認值 = 86400 秒（24 小時）
> **Configuration Changes**: 無
> **Performance Impact**: 無負面影響，只是改變了默認超時時間

   ```
   root
   - src/mcp_feedback_enhanced
    - server.py  // {type: refact} 修改 interactive_feedback 工具的默認 timeout 值
   - docs/architecture
    - api-reference.md  // {type: docs} 更新 API 文檔中的 timeout 默認值
    - deployment-guide.md  // {type: docs} 更新部署指南中的 timeout 默認值
    - interaction-flows.md  // {type: docs} 更新交互流程文檔中的 timeout 示例
   ```

## 2025-10-21 14:30:00

### 2. 修復 Augment 客戶端圖片識別功能

**Change Type**: fix

> **Purpose**: 修復 Augment AI 客戶端無法識別上傳圖片的問題
> **Detailed Description**:
>   1. 修改 `create_feedback_text_with_base64()` 函數，將圖片嵌入為 base64 編碼的數據
>   2. 圖片格式從 `{"path": "/tmp/..."}` 改為 `{"data": "data:image/png;base64,...", "type": "image/png"}`
>   3. 在文本中嵌入圖片數據，使用 `data:mime/type;base64,<data>` 格式和 `---END_IMAGE_N---` 標記
>   4. 添加環境變數別名支持：`is_augment_client` 和 `IS_AUGMENT_CLIENT` 現在可以作為 `MCP_AI_CLIENT` 的別名
> **Reason for Change**:
>   - Augment 是 JavaScript 應用，無法訪問服務器文件系統
>   - 需要通過 JSON 傳輸 base64 編碼的圖片數據
>   - 用戶設置 `is_augment_client: "true"` 但代碼只讀取 `MCP_AI_CLIENT`，導致配置無效
> **Impact Scope**:
>   - `src/mcp_feedback_enhanced/server.py` - 主要修改
>   - 所有使用 Augment 客戶端的用戶
> **API Changes**:
>   - 舊：圖片返回文件路徑 `{"path": "/tmp/image_xxx.png", "type": "png"}`
>   - 新：圖片返回 base64 數據 `{"data": "data:image/png;base64,...", "type": "image/png", "name": "..."}`
> **Configuration Changes**:
>   - 現在支持三種環境變數設置 AI 客戶端類型：
>     - `MCP_AI_CLIENT=augment` (主要方式)
>     - `is_augment_client=true` (別名)
>     - `IS_AUGMENT_CLIENT=true` (別名)
> **Performance Impact**: 無負面影響，base64 編碼略增加數據大小但改善了兼容性

   ```
   root
   - src/mcp_feedback_enhanced
    - server.py  // {type: refact} 修改 create_feedback_text_with_base64() 函數和啟動配置
   - tests/unit
    - test_image_fix.py  // {type: add} 新增圖片修復功能的單元測試
   ```

### 2. {function simple description}

**Change Type**: {type: feature/fix/improvement/refactor/docs/test/build}

> **Purpose**: {function purpose}
> **Detailed Description**: {function detailed description}
> **Reason for Change**: {why this change is needed}
> **Impact Scope**: {other modules or functions that may be affected by this change}
> **API Changes**: {if there are API changes, detail the old and new APIs}
> **Configuration Changes**: {changes to environment variables, config files, etc.}
> **Performance Impact**: {impact of the change on system performance}

   ```
   root
   - pkg    // {type: add/del/refact/-} {The role of a folder}
    - utils // {type: add/del/refact} {The function of the file}
   - xxx    // {type: add/del/refact} {The function of the file}
   ```

...