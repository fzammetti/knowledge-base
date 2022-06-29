# Oracle
----------

Some of this may apply to other RDBMS's too, but for the most part this is Oracle-specific stuff.

## Get information about the Oracle instance

### Version info

    SELECT * FROM v$version (banner, banner_full, banner_legacy, con_id)

...or, for just the simple Oracle version number...

    SELECT VALUE FROM v$system_parameter WHERE name = 'compatible'

Note: you must be logged in as the admin proxy account for this to work (probably true for all of these Oracle "info"-type queries, and you most likely can’t execute any of these in QA or Prod

### Instance info

    SELECT * FROM v$instance (instance_number, instance_name, host_name, version, version_legacy, version_full, startup_time, status, parallel, thread#, archiver, log_switch_wait, logins, shutdown_pending, database_status, instance_role, active_state, blocked, con_id, instance_mode, edition, family, database_type)

### License info

    SELECT * FROM v$license (sessions_max, sessions_warning, sessions_current, sessions_highwater, users_max, cpu_count_current, cpu_core_count_current, cpu_socket_count_current, cpu_count_highwater, cpu_core_count_highwater, cpu_socket_count_highwater, con_id)

### Host name info

    SELECT host_name FROM v$instance WHERE instance_number=userenv('instance')

...or...

    SELECT SYS_CONTEXT ('USERENV', 'SERVER_HOST') FROM DUAL;

## Find duplicate records on a given column

    SELECT <field>, COUNT(*) AS cnt FROM Mtable> GROUP BY <field> ORDER BY cnt DESC

## Get list of all tables in schema

    SELECT owner || '.' || table_name FROM all_tables WHERE LOWER(owner) IN ('<schema_name>');

## Query for count of rows with a populated BLOB field

    SELECT COUNT(*) FROM <table> WHERE DBMS_LOB.GETLENGTH(<field>) > 0

## Number of days in the current month

    SELECT CAST(TO_CHAR(LAST_DAY (SYSDATE), 'dd') AS INT) number_of_days FROM DUAL

## Number of days left in the current month

    SELECT SYSDATE, LAST_DAY(SYSDATE) "Last", LAST_DAY(SYSDATE) - SYSDATE "Days left" FROM DUAL

## Number of seconds left today

    SELECT (TRUNC(SYSDATE + 1) - SYSDATE) * 24 * 60 * 60 num_of_sec_left FROM DUAL

##Check if a table exists in the current schema

    SELECT table_name FROM user_tables WHERE table_name = '<TABLE_NAME>'

## Check if a column exists in a table

    SELECT column_name AS found FROM user_tab_cols WHERE table_name = '<TABLE_NAME>' AND column_name = '<COLUMN_NAME>'

## DDL for a table

    SELECT DBMS_METADATA.get_ddl('TABLE', '<TABLE_NAME>', '<SCHEMA_NAME>') FROM DUAL

## Name of the current schema

    SELECT SYS_CONTEXT('userenv', 'current_schema') FROM DUAL

## Database character set information

    SELECT * FROM nls_database_parameters

## Size of a database (total across all schemas, all object types)

    SELECT SUM (bytes) / 1024 / 1024 / 1024 AS GB FROM dba_data_files

## Size of just the data in a database

    SELECT SUM (bytes) / 1024 / 1024 / 1024 AS GB FROM dba_segments

## Size of a schema/user

    SELECT SUM(bytes / 1024 / 1024) "size" FROM dba_segments WHERE owner = '<OWNER>'

## Last SQL statement executed by each user

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

## Connections grouped by app

    SELECT program application, COUNT(program) number_of_sessions FROM v$session GROUP BY program ORDER BY number_of_sessions DESC

## Connected users and number of sessions

    SELECT username, COUNT(username) number_of_sessions FROM v$session GROUP BY username ORDER BY number_of_sessions DESC

## Number of objects per owner

    SELECT owner, COUNT(owner) number_of_objects FROM dba_objects GROUP BY owner ORDER BY number_of_objects DESC

## Convert numbers to words

    SELECT TO_CHAR(TO_DATE (1526, 'j'), 'jsp') FROM DUAL

## Convert CSV to table (each element becomes a row)

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

## Generate a random number

    SELECT ROUND(DBMS_RANDOM.VALUE () * 100) + 1 AS random_num FROM DUAL

## Test if a table has data

    SELECT 'yes' AS does_table_have_data FROM <TABLE_NAME> WHERE ROWNUM = 1

## List tables with number of rows and comments for each

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

## Swap the value of two columns in a table

    UPDATE <table_name> SET <column_1_name>=<column_2_name>, <column_2_name>=<column_1_name>

## Select unique values in a column

    SELECT DISTINCT <column_name> FROM <table_name>
