
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
-- create schema for partman
CREATE SCHEMA partman;

-- activate extension for schema
CREATE EXTENSION pg_partman SCHEMA partman;

-- (optional) grant permission to role/user
CREATE ROLE partman_user WITH LOGIN;
GRANT ALL ON SCHEMA partman TO partman_user;
GRANT ALL ON ALL TABLES IN SCHEMA partman TO partman_user;
GRANT EXECUTE ON ALL FUNCTIONS IN SCHEMA partman TO partman_user;
GRANT EXECUTE ON ALL PROCEDURES IN SCHEMA partman TO partman_user;

-- grant rights for places where are my data 
GRANT ALL ON SCHEMA public TO partman_user;
GRANT TEMPORARY ON DATABASE test to partman_user;
```
- create a table for demonstration purpose
```sql
-- create parent table
CREATE TABLE public.time_taptest_table
(col1 int,
 col2 text default 'stuff',
 col3 timestamptz NOT NULL DEFAULT now())
    PARTITION BY RANGE (col3);

-- create index for partitioning
CREATE INDEX ON public.time_taptest_table (col3);

-- create template table for partitioning
CREATE TABLE public.time_taptest_table_template (LIKE public.time_taptest_table);

-- add primary key to template table
ALTER TABLE public.time_taptest_table_template ADD PRIMARY KEY (col1);

-- enable configuration pro BGW (background worker)
SELECT partman.create_parent(
             p_parent_table := 'public.time_taptest_table',
             p_template_table := 'public.time_taptest_table_template',
             p_control := 'col3',              -- column to use for partitioning 
             p_interval := '1 day',            -- granularity of partitions
             p_premake := 4                    -- how much to the future
       );

-- change retention
UPDATE partman.part_config
SET
    retention = '5 days',            -- how long keep old data
    retention_keep_table = false,    -- keep old partitions?
    infinite_time_partitions = true  -- make new empty partitions?
WHERE parent_table = 'public.time_taptest_table';
```

Show partitions
```sql
SELECT partman.show_partitions('public.time_taptest_table', 'ASC', true);
```
or Postgres console `\d+ public.time_taptest_table` should show the partitions of the table.

Note: role/user who adds and deletes partitions must be the same as the owner of the table
