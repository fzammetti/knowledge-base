# Oracle

**NOTE: Some of this may apply to other RDBMS's too, but for the most part this is Oracle-specific stuff.

---

* [Get version info](#a07b4e77-4b93-4ce5-a47c-6eec0434295e)
* [Get instance info](#2545cb62-df6d-4ec7-afbc-c40d313dac6c)
* [Get license info](#58291cd5-e939-4b06-ab65-2980cb3e36df)
* [Get host name info](#6a19d280-22c2-43b9-9060-0034c9888b8a)
* [Find duplicate records on a given column](#f0a2c7ac-a276-4c43-89a4-817bc867fdf6)
* [Get list of all tables in schema](#e9ba0c24-d92f-4d82-bf6e-258dbb4768db)
* [Query for count of rows with a populated BLOB field](#e486eded-ac3f-4a83-bd5d-321c82bee1c3)
* [Get number of days in the current month](#2db2802b-63cd-4bd0-aab4-5e4abf3808ec)
* [Get number of days left in the current month](#434afc2e-3662-4722-85ca-826623b7cc40)
* [Get number of seconds left today](#4c3630e2-7931-46c6-b626-7982112a52a8)
* [Check if a table exists in the current schema](#e96b9bd2-3635-4ffd-a93e-2799899a17e3)
* [Check if a column exists in a table](#3ed824dc-d62f-424c-ab58-895e8e752f9e)
* [Get DDL for a table](#53cb015f-0460-4881-acdf-7d19c7674b1d)
* [Get name of the current schema](#d88ea749-5f5d-45db-bace-ba8edd33028e)
* [Get database character set information](#b87aad4c-7bde-474e-8d81-b59c09c2982d)
* [Get size of a database (total across all schemas, all object types)](#37e108b7-bb49-4a2e-8b4c-c8abac3ee134)
* [Get size of just the data in a database](#336da71b-4315-4a74-88d3-9b05ef4e4953)
* [Get size of a schema/user](#ac005dcf-462e-4fc3-ace9-eeed9f6674f4)
* [Get last SQL statement executed by each user](#ad4bc520-3b0f-4662-a0f7-10fbeb407098)
* [Get connections grouped by app](#f66a2bd7-a4c2-44a5-acf5-f0803e94a942)
* [Get connected users and number of sessions](#0f4bc0ef-be08-4f68-8a45-7f8414cbecc1)
* [Get number of objects per owner](#86b16d47-0f3a-4800-800d-aa853860650e)
* [Convert numbers to words](#f259286c-82ab-431b-9377-daf2d97ad272)
* [Convert CSV to table (each element becomes a row)](#3fa8608b-465d-4771-b42c-11b144e92311)
* [Generate a random number](#005c9ebb-1ed1-4ac1-a8dd-bb18c4c6e4a6)
* [Test if a table has data](#7fc2020e-f066-4223-8c8b-e08848f0e54b)
* [List tables with number of rows and comments for each](#c8f25619-abe2-44fb-9478-45ca0f55a531)
* [Swap the value of two columns in a table](#38b6c7c6-df56-450a-a5b0-3481d3033188)
* [Select unique values in a column](#b8f600ab-a4ef-40d4-9c58-1cd310c3edc7)

---




<div id="a07b4e77-4b93-4ce5-a47c-6eec0434295e">

## Get version info

</div>

    SELECT * FROM v$version (banner, banner_full, banner_legacy, con_id)

...or, for just the simple Oracle version number...

    SELECT VALUE FROM v$system_parameter WHERE name = 'compatible'

Note: you must be logged in as the admin proxy account for this to work (probably true for all of these Oracle "info"-type queries, and you most likely can’t execute any of these in QA or Prod




<div id="2545cb62-df6d-4ec7-afbc-c40d313dac6c">

## Get instance info

</div>

    SELECT * FROM v$instance (instance_number, instance_name, host_name, version, version_legacy, version_full, startup_time, status, parallel, thread#, archiver, log_switch_wait, logins, shutdown_pending, database_status, instance_role, active_state, blocked, con_id, instance_mode, edition, family, database_type)




<div id="58291cd5-e939-4b06-ab65-2980cb3e36df">

## Get license info

</div>

    SELECT * FROM v$license (sessions_max, sessions_warning, sessions_current, sessions_highwater, users_max, cpu_count_current, cpu_core_count_current, cpu_socket_count_current, cpu_count_highwater, cpu_core_count_highwater, cpu_socket_count_highwater, con_id)




<div id="6a19d280-22c2-43b9-9060-0034c9888b8a">

## Get host name info

</div>

    SELECT host_name FROM v$instance WHERE instance_number=userenv('instance')

...or...

    SELECT SYS_CONTEXT ('USERENV', 'SERVER_HOST') FROM DUAL;




<div id="f0a2c7ac-a276-4c43-89a4-817bc867fdf6">

## Find duplicate records on a given column

</div>

    SELECT <field>, COUNT(*) AS cnt FROM Mtable> GROUP BY <field> ORDER BY cnt DESC




<div id="e9ba0c24-d92f-4d82-bf6e-258dbb4768db">

## Get list of all tables in schema

</div>

    SELECT owner || '.' || table_name FROM all_tables WHERE LOWER(owner) IN ('<schema_name>');




<div id="e486eded-ac3f-4a83-bd5d-321c82bee1c3">

## Query for count of rows with a populated BLOB field

</div>

    SELECT COUNT(*) FROM <table> WHERE DBMS_LOB.GETLENGTH(<field>) > 0




<div id="2db2802b-63cd-4bd0-aab4-5e4abf3808ec">

## Get number of days in the current month

</div>

    SELECT CAST(TO_CHAR(LAST_DAY (SYSDATE), 'dd') AS INT) number_of_days FROM DUAL




<div id="434afc2e-3662-4722-85ca-826623b7cc40">

## Get number of days left in the current month

</div>

    SELECT SYSDATE, LAST_DAY(SYSDATE) "Last", LAST_DAY(SYSDATE) - SYSDATE "Days left" FROM DUAL




<div id="4c3630e2-7931-46c6-b626-7982112a52a8">

## Get number of seconds left today

</div>

    SELECT (TRUNC(SYSDATE + 1) - SYSDATE) * 24 * 60 * 60 num_of_sec_left FROM DUAL




<div id="e96b9bd2-3635-4ffd-a93e-2799899a17e3">

## Check if a table exists in the current schema

</div>

    SELECT table_name FROM user_tables WHERE table_name = '<TABLE_NAME>'




<div id="3ed824dc-d62f-424c-ab58-895e8e752f9e">

## Check if a column exists in a table

</div>

    SELECT column_name AS found FROM user_tab_cols WHERE table_name = '<TABLE_NAME>' AND column_name = '<COLUMN_NAME>'




<div id="53cb015f-0460-4881-acdf-7d19c7674b1d">

## Get DDL for a table

</div>

    SELECT DBMS_METADATA.get_ddl('TABLE', '<TABLE_NAME>', '<SCHEMA_NAME>') FROM DUAL




<div id="d88ea749-5f5d-45db-bace-ba8edd33028e">

## Get name of the current schema

</div>

    SELECT SYS_CONTEXT('userenv', 'current_schema') FROM DUAL




<div id="b87aad4c-7bde-474e-8d81-b59c09c2982d">

## Get database character set information

</div>

    SELECT * FROM nls_database_parameters




<div id="37e108b7-bb49-4a2e-8b4c-c8abac3ee134">

## Get size of a database (total across all schemas, all object types)

</div>

    SELECT SUM (bytes) / 1024 / 1024 / 1024 AS GB FROM dba_data_files




<div id="336da71b-4315-4a74-88d3-9b05ef4e4953">

## Get size of just the data in a database

</div>

    SELECT SUM (bytes) / 1024 / 1024 / 1024 AS GB FROM dba_segments




<div id="ac005dcf-462e-4fc3-ace9-eeed9f6674f4">

## Get size of a schema/user

</div>

    SELECT SUM(bytes / 1024 / 1024) "size" FROM dba_segments WHERE owner = '<OWNER>'




<div id="ad4bc520-3b0f-4662-a0f7-10fbeb407098">

## Get last SQL statement executed by each user

</div>

    SELECT
      S.USERNAME || '(' || s.sid || ')-' || s.osuser UNAME,
      s.program || '-' || s.terminal || '(' || s.machine || ')' PROG,
      s.sid || '/' || s.serial# sid,
      s.status "Status",
      p.spid,
      sql_text sqltext
    FROM
      v$sqltext_with_newlines t, V$SESSION s, v$process p
    WHERE
      t.address = s.sql_address AND p.addr = s.paddr(+) AND t.hash_value = s.sql_hash_value
    ORDER
      BY s.sid, t.piece
    Status of long-running queries
    SELECT
      a.sid,
      a.serial#,
      b.username,
      opname OPERATION,
      target OBJECT,
      TRUNC(elapsed_seconds, 5) "ET (s)",
      TO_CHAR (start_time, 'HH24:MI:SS') start_time,
      ROUND((sofar / totalwork) * 100, 2) "COMPLETE (%)"
    FROM
      v$session_longops a, v$session b
    WHERE
      a.sid = b.sid AND b.username NOT IN ('SYS', 'SYSTEM') AND totalwork > 0
    ORDER BY
      elapsed_seconds
    Current session ID/PID/client process ID
    SELECT
      b.sid, b.serial#, a.spid processid, b.process clientpid
    FROM
      v$process a, v$session b WHERE a.addr = b.paddr AND b.audsid = USERENV('sessionid')
    Top 10 SQL queries by reads per execution
    SELECT * FROM (
      SELECT
        ROWNUM,
        SUBSTR (a.sql_text, 1, 200) sql_text,
        TRUNC(a.disk_reads / DECODE(a.executions, 0, 1, a.executions)) reads_per_execution,
        a.buffer_gets,
        a.disk_reads,
        a.executions,
        a.sorts,
        a.address
      FROM
        v$sqlarea a
      ORDER BY
        3 DESC
    ) WHERE ROWNUM < 10
    Current number of connections
    SELECT osuser, username, machine, program FROM v$session ORDER BY osuser




<div id="f66a2bd7-a4c2-44a5-acf5-f0803e94a942">

## Get connections grouped by app

</div>

    SELECT program application, COUNT(program) number_of_sessions FROM v$session GROUP BY program ORDER BY number_of_sessions DESC




<div id="0f4bc0ef-be08-4f68-8a45-7f8414cbecc1">

## Get connected users and number of sessions

</div>

    SELECT username, COUNT(username) number_of_sessions FROM v$session GROUP BY username ORDER BY number_of_sessions DESC




<div id="86b16d47-0f3a-4800-800d-aa853860650e">

## Get number of objects per owner

</div>

    SELECT owner, COUNT(owner) number_of_objects FROM dba_objects GROUP BY owner ORDER BY number_of_objects DESC




<div id="f259286c-82ab-431b-9377-daf2d97ad272">

## Convert numbers to words

</div>

    SELECT TO_CHAR(TO_DATE (1526, 'j'), 'jsp') FROM DUAL




<div id="3fa8608b-465d-4771-b42c-11b144e92311">

## Convert CSV to table (each element becomes a row)

</div>

    WITH csv AS (
      SELECT 'AA,BB,CC,DD,EE,FF' AS csvdata FROM DUAL
    )
    SELECT
      REGEXP_SUBSTR(csv.csvdata, '[^,]+', 1, LEVEL) pivot_char FROM DUAL,
      csv CONNECT BY REGEXP_SUBSTR(csv.csvdata,'[^,]+', 1, LEVEL) IS NOT NULL
    Generate a table of random data (non-persistent)
    SELECT
      LEVEL empl_id,
      MOD(ROWNUM, 50000) dept_id,
      TRUNC(DBMS_RANDOM.VALUE(1000, 500000), 2) salary,
      DECODE(ROUND(DBMS_RANDOM.VALUE(1, 2)), 1, 'M', 2, 'F') gender,
      TO_DATE(ROUND(DBMS_RANDOM.VALUE(1, 28)) || '-'|| ROUND(DBMS_RANDOM.VALUE(1, 12)) || '-'|| ROUND(DBMS_RANDOM.VALUE(1900, 2010)), 'DD-MM-YYYY') dob,
      DBMS_RANDOM.STRING('x', DBMS_RANDOM.VALUE (20, 50)) address
    FROM DUAL
    CONNECT BY LEVEL < 10000




<div id="005c9ebb-1ed1-4ac1-a8dd-bb18c4c6e4a6">

## Generate a random number

</div>

    SELECT ROUND(DBMS_RANDOM.VALUE () * 100) + 1 AS random_num FROM DUAL




<div id="7fc2020e-f066-4223-8c8b-e08848f0e54b">

## Test if a table has data

</div>

    SELECT 'yes' AS does_table_have_data FROM <TABLE_NAME> WHERE ROWNUM = 1




<div id="c8f25619-abe2-44fb-9478-45ca0f55a531">

## List tables with number of rows and comments for each

</div>

    SELECT
      tab.owner AS schema_name,
      tab.table_name AS table_name,
      obj.created,
      obj.last_ddl_time AS last_modified,
      tab.num_rows,
      tab.last_analyzed,
      comm.comments
    FROM
      all_tables tab INNER JOIN all_objects obj ON obj.owner = tab.owner
      AND obj.object_name = tab.table_name LEFT OUTER JOIN all_tab_comments comm ON tab.table_name = comm.table_name
      AND tab.owner = comm.owner
    WHERE
      tab.owner IN ('<SCHEMA_NAME>')
    ORDER BY
      tab.owner, tab.table_name
    Show table column details
    SELECT
      col.owner AS schema_name,
      col.table_name,
      col.column_name,
      col.data_type,
      DECODE(char_length, 0, data_type,
      data_type || '(' || char_length || ')') AS data_type_ext,
      col.data_length,
      col.data_precision,
      col.data_scale,
      col.nullable,
      col.data_default AS default_value,
      NVL(pk.primary_key, ' ') AS primary_key,
      NVL(fk.foreign_key, ' ') AS foreign_key,
      NVL(uk.unique_key, ' ') AS unique_key,
      NVL(check_const.check_constraint, ' ') check_constraint,
      comm.comments
    FROM
      all_tables tab INNER JOIN all_tab_columns col ON col.owner = tab.owner
      AND col.table_name = tab.table_name LEFT JOIN all_col_comments comm ON col.owner = comm.owner
      AND col.table_name = comm.table_name
      AND col.column_name = comm.column_name
      LEFT JOIN (
        SELECT
          constr.owner, col_const.table_name, col_const.column_name, 'PK' primary_key
        FROM
          all_constraints constr INNER JOIN all_cons_columns col_const ON constr.constraint_name = col_const.constraint_name
          AND col_const.owner = constr.owner
        WHERE
          constr.constraint_type = 'P'
      ) pk ON col.table_name = pk.table_name
      AND col.column_name = pk.column_name
      AND col.owner = pk.owner
      LEFT JOIN (
        SELECT
          constr.owner, col_const.table_name, col_const.column_name, 'FK' foreign_key
        FROM
          all_constraints constr INNER JOIN all_cons_columns col_const ON constr.constraint_name = col_const.constraint_name
          AND col_const.owner = constr.owner
        WHERE
          constr.constraint_type = 'R'
        GROUP BY
          constr.owner, col_const.table_name, col_const.column_name
      ) fk ON col.table_name = fk.table_name
      AND col.column_name = fk.column_name
      AND col.owner = fk.owner
      LEFT JOIN (
        SELECT
          constr.owner, col_const.table_name, col_const.column_name, 'UK' unique_key
        FROM
          all_constraints constr INNER JOIN all_cons_columns col_const ON constr.constraint_name = col_const.constraint_name
        AND
          constr.owner = col_const.owner
        WHERE
          constr.constraint_type = 'U' UNION
          SELECT
            ind.owner, col_ind.table_name, col_ind.column_name, 'UK' unique_key
          FROM
            all_indexes ind
          INNER JOIN all_ind_columns col_ind ON ind.index_name = col_ind.index_name
          WHERE
            ind.uniqueness = 'UNIQUE'
      ) uk ON col.table_name = uk.table_name
      AND col.column_name = uk.column_name
      AND col.owner = uk.owner
      LEFT JOIN (
        SELECT
          constr.owner, col_const.table_name, col_const.column_name, 'Check' check_constraint
        FROM
          all_constraints constr INNER JOIN all_cons_columns col_const ON constr.constraint_name = col_const.constraint_name
          AND col_const.owner = constr.owner
        WHERE
          constr.constraint_type = 'C'
        GROUP BY
          constr.owner, col_const.table_name, col_const.column_name
      ) check_const ON col.table_name = check_const.table_name
      AND col.column_name = check_const.column_name
      AND col.owner = check_const.owner
    WHERE
      col.owner IN ('<schema_name>')
      AND LOWER(tab.table_name) = '<table_name>'
    ORDER BY
      col.owner, col.table_name, col.column_name
    List all tables by number of columns
    SELECT
      col.owner AS schema_name,
      col.table_name,
      COUNT(*) columns
    FROM
      all_tab_columns col INNER JOIN all_tables tab ON col.owner = tab.owner
      AND col.table_name = tab.table_name
    WHERE
      col.owner NOT IN (
        'ANONYMOUS', 'CTXSYS', 'DBSNMP', 'EXFSYS', 'LBACSYS', 'MDSYS', 'MGMT_VIEW', 'OLAPSYS', 'OWBSYS', 'ORDPLUGINS',
        'ORDSYS', 'OUTLN', 'SI_INFORMTN_SCHEMA', 'SYS', 'SYSMAN', 'SYSTEM', 'TSMSYS', 'WK_TEST', 'WKSYS', 'WKPROXY',
        'WMSYS', 'XDB', 'APEX_040000', 'APEX_PUBLIC_USER', 'DIP', 'FLOWS_30000', 'FLOWS_FILES', 'MDDATA',
        'ORACLE_OCM', 'SPATIAL_CSW_ADMIN_USR', 'SPATIAL_WFS_ADMIN_USR', 'XS$NULL', 'PUBLIC'
      )
    GROUP BY col.owner, col.table_name
    ORDER BY COUNT(*) DESC;




<div id="38b6c7c6-df56-450a-a5b0-3481d3033188">

## Swap the value of two columns in a table

</div>

    UPDATE <table_name> SET <column_1_name>=<column_2_name>, <column_2_name>=<column_1_name>




<div id="b8f600ab-a4ef-40d4-9c58-1cd310c3edc7">

## Select unique values in a column

</div>

    SELECT DISTINCT <column_name> FROM <table_name>
