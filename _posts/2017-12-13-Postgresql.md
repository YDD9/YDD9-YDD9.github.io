---
layout: post
title:  "Postgresql"
date:   2017-12-13 14:58:39 +0100
comments: true 
categories: Postgresql
---


- [work with command line with](#work-with-command-line-with)
- [Login with cmd](#login-with-cmd)
- [backup your database:](#backup-your-database)
- [import from dump via cmd](#import-from-dump-via-cmd)
- [import from dump via pgAdmin SQL](#import-from-dump-via-pgadmin-sql)
- [Auth issues](#auth-issues)


# work with command line with   
add env path C:\Program Files\PostgreSQL\9.6\bin   
for linux download `sudo apt-get install postgresql-client`   

# Login with cmd
https://code.i-harness.com/en/q/756e5a    
```
# psql -U postgres -h <your IPv4> -W
postgres# \c <database name>
```

# backup your database WITHOUT login as above:   
https://www.postgresql.org/docs/9.1/static/backup-dump.html   
https://stackoverflow.com/questions/37984733/postgresql-database-export-to-sql-file    

https://www.postgresql.org/docs/9.1/static/app-pgdump.html     

`pg_dump dbname > outfile`</br>
`pg_dump -U username databasename >> sqlfile.sql`   

`pg_dump -U username -h <your IPv4> > sqlfile.sql`     

To dump a single table named mytab:   
`$ pg_dump -t mytab mydb > db.sql`    

https://stackoverflow.com/questions/42567030/psql-errors-when-importing-from-a-pg-dump   
without owner `--no-owner --no-acl`

**To dump all schemas whose names start with east or west and end in gsm, excluding any schemas whose names contain the word test:  
`$ pg_dump -n 'east*gsm' -n 'west*gsm' -N '*test*' mydb > db.sql`   **

**PS**:  mydb should always be the end of command:
download DB postgres_sh, with only schema mydb_testing, with user postgres, with server IP 10.16.187.100 into file.   
`pg_dump -U postgres -h 10.16.187.100 -n mydb_schema postgres_sh> back.sql`

```
output schema-only
C:\tmp>pg_dump --host localhost --port 5432 --username postgres -d DATABASE --schema-only --table test1 > test.sql


output data-only
C:\tmp>pg_dump --host localhost --port 5432 --username postgres -d DATABASE --data-only --table test1 > test.sql


output all
C:\tmp>pg_dump --host localhost --port 5432 --username postgres -d DATABASE --table test1 > test.sql
C:\tmp>pg_dump --host localhost --port 5432 --username postgres -d DATABASE -t test1 > test.sql
```

# import from dump via cmd
https://www.postgresql.org/docs/8.1/static/backup.html#BACKUP-DUMP-RESTORE   
https://stackoverflow.com/questions/6842393/import-sql-dump-into-postgresql-database   
`psql databasename < data_base_dump`  databasename DB must be created before you import your dump.sql   
` psql -h 10.16.187.100 -U postgres postgres_sh < back.sql`   

# import from dump via pgAdmin SQL
https://stackoverflow.com/questions/32271378/in-postgresql-how-to-insert-data-with-copy-command   
if you want to use your dump.sql from pgAdmin, when you dump, you need add --inserts, this way you won't get a sql plain text file with copy table from stdin;     
`pg_dump -U postgres -h 10.16.187.100 --inserts -n mydbschema postgres_sh> back.sql`    
  
# Auth issues  
https://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge  

The problem is still your pg_hba.conf file (/etc/postgresql/9.1/main/pg_hba.conf). This line:  
`local   all             postgres                                peer`

Should be   
`local   all             postgres                                md5`
