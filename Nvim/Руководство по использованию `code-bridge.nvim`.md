Вот пошаговое руководство по использованию `code-bridge.nvim` — от установки до ежедневной работы.

---

## 📦 Шаг 1: Установка

Добавьте в вашу конфигурацию (например, для `lazy.nvim`):
```lua
{
  "samir-roy/code-bridge.nvim",
  config = function()
    require('code-bridge').setup()
  end
}
```
Плагин работает с Neovim 0.7+ и требует наличия установленного AI-агента (Claude Code, OpenCode и т.д.) [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim).

---

## 🚀 Шаг 2: Настройка окружения (рекомендуемый способ)

Для максимальной эффективности создайте tmux-сессию с отдельным окном для AI-агента [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim):
```bash
# Создаём сессию
tmux new-session -d -s coding
# Создаём окно для Claude Code
tmux new-window -t coding -n claude
# Запускаем агента
tmux send-keys -t coding:claude 'claude' Enter
```
Теперь у вас есть два окна в tmux:

- Окно с Neovim (где вы пишете код)
    
- Окно с AI-агентом (куда будут отправляться запросы)
    

Плагин сам найдёт окно `claude` и отправит туда контекст [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim).

---

## ⌨️ Шаг 3: Основные команды для ежедневной работы

### 📤 Отправка кода агенту

|Команда|Что делает|Режим|
|---|---|---|
|`:CodeBridgeTmux`|Отправляет текущий файл (или выделенный фрагмент)|Normal/Visual [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim)|
|`:CodeBridgeTmuxAll`|Отправляет **все открытые буферы**|Normal|
|`:CodeBridgeTmuxDiff`|Отправляет незакоммиченные изменения (git diff)|Normal|
|`:CodeBridgeTmuxRecent`|Отправляет недавно изменённые файлы|Normal|
|`:CodeBridgeTmuxDiagnostics`|Отправляет ошибки LSP из текущего буфера|Normal|

### 💬 Чат внутри Neovim

Если вы не хотите переключаться в окно с агентом, можно общаться прямо из Neovim [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim):

|Команда|Что делает|
|---|---|
|`:CodeBridgeQuery`|Открывает чат-буфер с автоматическим добавлением контекста|
|`:CodeBridgeChat`|Открывает чат-буфер **без** контекста (для общих вопросов)|
|`:CodeBridgeHide`|Скрывает чат-буфер (история сохраняется)|
|`:CodeBridgeShow`|Показывает скрытый чат-буфер|
|`:CodeBridgeWipe`|Полностью очищает историю чата|
|`:CodeBridgeCancelQuery`|Отменяет выполняющийся запрос|

### ✏️ Интерактивные промпты

Если вам нужно отредактировать промпт перед отправкой [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim):

|Команда|Что делает|
|---|---|
|`:CodeBridgeTmuxInteractive`|Открывает редактор для написания промпта перед отправкой|
|`:CodeBridgeResumePrompt`|Возвращает скрытый редактор промпта|

**В редакторе промпта:**

- `<Ctrl-s>` или `<Leader-s>` — отправить промпт
    
- `<Ctrl-x>` или `<Leader-x>` — скрыть редактор (можно будет вернуться)
    
- `<Ctrl-c>` — отменить и закрыть
    

---

## 🔧 Шаг 4: Настройка горячих клавиш

Добавьте в ваш `init.lua` удобные маппинги [](https://explore.market.dev/ecosystems/neovim/projects/code-bridge-nvim):
```lua
-- Базовые команды
vim.keymap.set("n", "<leader>ct", ":CodeBridgeTmux<CR>", { desc = "Send file to agent" })
vim.keymap.set("v", "<leader>ct", ":CodeBridgeTmux<CR>", { desc = "Send selection to agent" })
vim.keymap.set("n", "<leader>ca", ":CodeBridgeTmuxAll<CR>", { desc = "Send all buffers to agent" })
-- Расширенные
vim.keymap.set("n", "<leader>ci", ":CodeBridgeTmuxInteractive<CR>", { desc = "Interactive prompt" })
vim.keymap.set("n", "<leader>cd", ":CodeBridgeTmuxDiff<CR>", { desc = "Send git diff" })
vim.keymap.set("n", "<leader>cr", ":CodeBridgeTmuxRecent<CR>", { desc = "Send recent files" })
vim.keymap.set("n", "<leader>ce", ":CodeBridgeTmuxDiagnostics<CR>", { desc = "Send diagnostics" })
-- Чат внутри Neovim
vim.keymap.set("n", "<leader>cq", ":CodeBridgeQuery<CR>", { desc = "Query with context" })
vim.keymap.set("v", "<leader>cq", ":CodeBridgeQuery<CR>", { desc = "Query with selection" })
vim.keymap.set("n", "<leader>cc", ":CodeBridgeChat<CR>", { desc = "Chat without context" })
vim.keymap.set("n", "<leader>ch", ":CodeBridgeHide<CR>", { desc = "Hide chat" })
vim.keymap.set("n", "<leader>cs", ":CodeBridgeShow<CR>", { desc = "Show chat" })
vim.keymap.set("n", "<leader>cx", ":CodeBridgeWipe<CR>", { desc = "Clear chat history" })
```
> 💡 `

[[code-bridge.nvim]]