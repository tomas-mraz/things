
# PostgreSQL

```
sudo -u postgres psql

CREATE DATABASE mydb;
CREATE USER myuser WITH ENCRYPTED PASSWORD 'mypass';
GRANT ALL privileges on DATABASE mydb TO myuser;
```

switch to database `\CONNECT mydb`

granting SELECT right
`GRANT ALL ON ALL TABLES IN SCHEMA schema_name TO myuser;`


grant rights to use sequences
`GRANT ALL ON ALL SEQUENCES IN SCHEMA schema_name TO myuser;`

note: "schema_name" is most likely "public"


