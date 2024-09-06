# zenhan

Switch the mode of input method editor from terminal. This is a tool similar to im-select.

see https://github.com/VSCodeVim/Vim#input-method

## Setting example

```
"vim.autoSwitchInputMethod.enable": true,
"vim.autoSwitchInputMethod.defaultIM": "0",
"vim.autoSwitchInputMethod.obtainIMCmd": "D:\\bin\\zenhan.exe",
"vim.autoSwitchInputMethod.switchIMCmd": "D:\\bin\\zenhan.exe {im}"
```

## see also

[Qiita (Japanese)](https://qiita.com/iuchi/items/9ddcfb48063fc5ab626c)


---
モード変更時は変更前の状態を標準出力に出すように修正した。
オリジナルのzenhanを使う場合はインサートモードを抜けるときに、状態の検出と変更の2回zenhanを実行する必要がある。

インサートモードでのIMEの状態を覚えておき、再度インサートモードに入ったときに復元する。
```
IM = 0
local zenhan = os.getenv("LOCALAPPDATA") .. '/zenhan/bin64/zenhan.exe'
vim.api.nvim_create_autocmd({ 'InsertEnter' }, {
    pattern = { "*" },
    callback = function() os.execute(zenhan .. ' ' .. IM) end
})
vim.api.nvim_create_autocmd({ 'InsertLeave' }, {
    pattern = { "*" },
    callback = function()
        local handle = io.popen(zenhan .. ' 0', "r")
        if (handle) then
            IM = handle:read("*all")
            handle:close()
        end
    end
})
```
