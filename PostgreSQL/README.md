# PostgreSQL
----------

## If when setting up Confluence you get an error like “org.postgresql.util.PSQLException: FATAL: Ident authentication failed for user conflueneuser” then look in the pg_hba.conf file (probably in something like /var/lib/pgsql/data) and make sure local and remote connections use method TRUST and not IDENT.
