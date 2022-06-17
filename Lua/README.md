# Lua
----------

## Iterate over the elements in a table

    tbl = { "Star Trek", "Star Wars", "Babylon 5" }
      for idx, val in ipairs(tbl) do
        print(idx, val)
    end
