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
\d tablename
```bash
```
## Help
```bash
# sql help
\h

# psql help
\?
```
## Exit postgresql
```bash
\q
```
