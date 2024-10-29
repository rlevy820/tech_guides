# Postgresql Cheet Sheet
## Service management commands
```bash
sudo service postgresql stop
sudo service postgresql start
sudo service postgresql restart
```
## Start postgresql in terminal
```bash
psql -U postgres
```
## Connect/switch to database
```bash
\c dbname
```
## List all databases
```bash
\l
```
##  List all tables in current database
```bash
\dt
```
## Show table structure/schema
```bash
\d tablename
```
## SQL Help
```bash
\h
```
## PSQL help
```bash
\?
```
## Exit postgresql
```bash
\q
```
