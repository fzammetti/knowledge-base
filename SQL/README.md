# SQL
----------

## Return all rows where a field contains a substring:

    SELECT <field_name> FROM <table_name> WHERE (UPPER(<field_name>)) LIKE '%' || (UPPER('<substring>')) || '%'

## Replace a substring in a field on all rows in a table with a new string:

    UPDATE <table_name> SET <field_name> = REPLACE(<field_name>, '<substring>', '<new_string>')
