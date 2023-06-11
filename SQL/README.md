# SQL

---

* [Return all rows where a field contains a substring](#5e5edfd8-51b1-406c-9a43-40f56b663210)
* [Replace a substring in a field on all rows in a table with a new string](#7c2760e1-830d-4e3f-a909-414fc80fcddf)

---




<div id="5e5edfd8-51b1-406c-9a43-40f56b663210">

## Return all rows where a field contains a substring

</div>

    SELECT <field_name> FROM <table_name> WHERE (UPPER(<field_name>)) LIKE '%' || (UPPER('<substring>')) || '%'




<div id="7c2760e1-830d-4e3f-a909-414fc80fcddf">

## Replace a substring in a field on all rows in a table with a new string

</div>

    UPDATE <table_name> SET <field_name> = REPLACE(<field_name>, '<substring>', '<new_string>')
