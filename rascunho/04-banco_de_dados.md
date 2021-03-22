## Postgresal criação banco.

```sql
--- C:\Program Files\PostgreSQL\10\bin>psql.exe -U postgres 
-- sudo -u postgres psql
-- Cria um banco no postgres
CREATE DATABASE dj_pmovel_db_teste;
CREATE USER dj_pmovel_user_teste WITH PASSWORD 'xpto';


ALTER ROLE dj_pmovel_user_teste SET client_encoding TO 'utf8';
ALTER ROLE dj_pmovel_user_teste SET default_transaction_isolation TO 'read committed';
ALTER ROLE dj_pmovel_user_teste SET timezone TO 'America/Sao_Paulo';

GRANT ALL PRIVILEGES ON DATABASE dj_pmovel_db TO dj_pmovel_user_teste;
\q

-- altera a senha do usuario postgres
-- pgadmin III
-- name: localhost
-- host: localhost
-- port: 5432
-- maintenance DB: postgres
-- username: postgres
-- password: xpto
sudo -u postgres psql postgres
alter user postgres with password 'xpto';
\q


-- saber versao do banco
SELECT version();
```


### Criando base de exemplo postgresl

```bash
sudo -i -u postgres
psql -h 127.0.0.1 -U postgres -p 15432

#psql -h 127.0.0.1 -U pagila_user -p 15432 -c "CREATE DATABASE pagila;"

CREATE DATABASE pagila;
CREATE USER pagila_user WITH PASSWORD 'xpto';

ALTER ROLE pagila_user SET client_encoding TO 'utf8';
ALTER ROLE pagila_user SET default_transaction_isolation TO 'read committed';
ALTER ROLE pagila_user SET timezone TO 'America/Sao_Paulo';

GRANT ALL PRIVILEGES ON DATABASE pagila TO pagila_user;


psql -h 127.0.0.1 -U pagila_user -p 15432 -d pagila -f /home/lc/Downloads/pagila/pagila-schema.sql 
psql -h 127.0.0.1 -U pagila_user -p 15432 -d pagila -f /home/lc/Downloads/pagila/pagila-data.sql

https://github.com/xzilla/pagila
``` 

### Criando usario para aplicacao pagila
```bash

psql -h 127.0.0.1 -U pagila_user -p 15432 -d pagila
```