
# Automation table partitioning in Postgres

## Install
https://github.com/pgpartman/pg_partman?tab=readme-ov-file#installation

- Ubuntu system package `postgresql-16-partman` - PostgreSQL Partition Manager
- modify `postgresql.conf` probably in the directory `/etc/postgresql/16/main/`
    ```editorconfig
    # Nový řádek, pokud jde o pravní rozšíření
    shared_preload_libraries = 'pg_partman_bgw'
    
    # nebo pokud už tam něco je, tak udělat seznam
    shared_preload_libraries = 'pg_stat_statements,pg_partman_bgw'
    ```
- restart Postgres database systemd service `sudo systemctl restart postgresql-16`
- run command in the database
  - connect to the database `psql -d moje_databaze`
  - sdvsdvsd
```sql
-- 1. Vytvoříme schéma pro partmanovy interní tabulky a funkce
CREATE SCHEMA partman;

-- 2. Vytvoříme rozšíření a řekneme mu, aby použilo toto schéma
CREATE EXTENSION pg_partman SCHEMA partman;

-- 3. (Volitelné) Dejte oprávnění uživateli, který bude spravovat partition.
CREATE ROLE partman_user WITH LOGIN;
GRANT ALL ON SCHEMA partman TO partman_user;
GRANT ALL ON ALL TABLES IN SCHEMA partman TO partman_user;


GRANT ALL ON SCHEMA partman TO partman_user;
GRANT ALL ON ALL TABLES IN SCHEMA partman TO partman_user;
GRANT EXECUTE ON ALL FUNCTIONS IN SCHEMA partman TO partman_user;
GRANT EXECUTE ON ALL PROCEDURES IN SCHEMA partman TO partman_user;
GRANT ALL ON SCHEMA my_partition_schema TO partman_user;
GRANT TEMPORARY ON DATABASE mydb to partman_user;
```


sources: https://github.com/pgpartman/pg_partman?tab=readme-ov-file#installation
