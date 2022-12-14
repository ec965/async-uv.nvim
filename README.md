# async-uv.nvim

Every libuv function that has a callback, now wrapped in async/await syntax.
Other libuv functions are also exported for your convenience.

## Installation

With [Packer](https://github.com/wbthomason/packer.nvim)

```lua
 use { "ec965/async-uv.nvim", requires = { "ms-jpq/lua-async-await" }}
```

## Usage

```lua
local uv = require "async-uv"
local a = require "async"

function readFile(path)
    return a.sync(function()
        local err, fd = a.wait(uv.fs_open(path, "r", 438))
        assert(not err, err)
        local err, stat = a.wait(uv.fs_fstat(fd))
        assert(not err, err)
        local err, data = a.wait(uv.fs_read(fd, stat.size, 0))
        assert(not err, err)
        return data
    end)
end

a.sync(function()
    local data = a.wait(readFile(vim.fn.expand "~/.config/nvim/init.lua"))
    vim.schedule(function()
        vim.notify(data)
    end)
end)()
```

## Related

- [ms-jpq/lua-async-await](https://github.com/ms-jpq/lua-async-await)
- [lewis6991/async.nvim](https://github.com/lewis6991/async.nvim)
- [nvim-lua/plenary.nvim](https://github.com/nvim-lua/plenary.nvim#plenaryasync): includes an async wrapper for some libuv functions
- [wbthomason/packer.nvim](https://github.com/wbthomason/packer.nvim/blob/502a89f72ee5db3907dd0c7ee36287d49cfa56a0/lua/packer/async.lua): the plugin implements it's own async library
