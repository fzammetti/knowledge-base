# Lua

---

* [Iterate over the elements in a table](#0d0b59e0-e0b0-4dca-8fc6-47540b4193e0)

---




<div id="0d0b59e0-e0b0-4dca-8fc6-47540b4193e0">

## Iterate over the elements in a table

</div>

    tbl = { "Star Trek", "Star Wars", "Babylon 5" }
      for idx, val in ipairs(tbl) do
        print(idx, val)
    end
