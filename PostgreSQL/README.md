# PostgreSQL

---

* [Confluence setup error like “org.postgresql.util.PSQLException: FATAL: Ident authentication failed for user conflueneuser”](#ff9d276b-0231-4e42-9812-2c1ea679aa8d)

---




<div id="ff9d276b-0231-4e42-9812-2c1ea679aa8d">

## Confluence setup error like “org.postgresql.util.PSQLException: FATAL: Ident authentication failed for user conflueneuser”

</div>

If when setting up Confluence you get an error like “org.postgresql.util.PSQLException: FATAL: Ident authentication failed for user conflueneuser” then look in the pg_hba.conf file (probably in something like /var/lib/pgsql/data) and make sure local and remote connections use method TRUST and not IDENT.
