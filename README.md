# easyformat.nvim
a simply and powerful plugin make format easy in neovim with third party command and lsp format

<center>
<img src="https://user-images.githubusercontent.com/41671631/218993459-aeaf79fe-c77f-4d4f-a820-57e1a6464af4.gif" width=40% height=40%>
</center>

## Features

- async
- fast
- minimalist and light weight

## Install

- Lazy.nvim

```lua
require('lazy').setup({
 {'glepnir/easyformat.nvim', ft = {filetypes}, config = function()
    require('easyformat').setup({})
 end},
})
```

- packer.nvim

```lua
use({
 {'glepnir/easyformat.nvim', ft = {filetypes}, config = function()
    require('easyformat').setup({})
 end},
})
```

## Config

```lua
fmt_on_save = true -- default is true when is true it will run format on BufWritePre
--filetype config
filetype = {
    cmd  -- string type the third party format command.
    args -- table type command arguments.
    stdin -- boolean type when is true will send the buffer contents to stdin
    ignore_patterns --table type when file name match one of it will ignore format
    find  -- string type search the config file that command used. if not find will not format
    hook -- function type a hook run after async fmt invoked
    lsp  -- boolean type if enable it will run vim.lsp.buf.format with async = true
}
```

also you can use a command `EasyFormat` to format file.

## Example configs

for c cpp go lua

```lua
  require('easyformat').setup({
    fmt_on_save = true,
    c = {
      cmd = 'clang-format',
      args = { '-style=file', vim.api.nvim_buf_get_name(0) },
      ignore_patterns = { 'neovim/*' },
      find = '.clang-format',
      stdin = false,
      lsp = false,
    },
    cpp = {
      cmd = 'clang-format',
      args = { '-style=file', vim.api.nvim_buf_get_name(0) },
      find = '.clang-format',
      stdin = false,
      lsp = false,
    },
    go = {
      cmd = 'golines',
      args = { '--max-len=80', vim.api.nvim_buf_get_name(0) },
      stdin = false,
      hook = function()
        vim.lsp.buf.code_action({ context = { only = { 'source.organizeImports' } }, apply = true })
      end,
      lsp = true,
    },
    lua = {
      cmd = 'stylua',
      ignore_patterns = { '%pspec', 'neovim/*' },
      find = '.stylua.toml',
      args = { '-' },
      stdin = true,
      lsp = false,
    },
  })

```

## License
