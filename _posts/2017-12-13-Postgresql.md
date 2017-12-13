# work with command line with   
add env path C:\Program Files\PostgreSQL\9.6\bin   
for linux download `sudo apt-get install postgresql-client`   

# Login with cmd
https://code.i-harness.com/en/q/756e5a  
`psql -U postgres -h <your IPv4> -W`  

# backup your database:  
https://www.postgresql.org/docs/9.1/static/backup-dump.html   
https://stackoverflow.com/questions/37984733/postgresql-database-export-to-sql-file    

https://www.postgresql.org/docs/9.1/static/app-pgdump.html     

`pg_dump dbname > outfile`
`pg_dump -U username databasename >> sqlfile.sql`   

`pg_dump -U username -h <your IPv4> > sqlfile.sql`     

To dump a single table named mytab:   
`$ pg_dump -t mytab mydb > db.sql`    

https://stackoverflow.com/questions/42567030/psql-errors-when-importing-from-a-pg-dump   
without owner `--no-owner --no-acl`

**To dump all schemas whose names start with east or west and end in gsm, excluding any schemas whose names contain the word test:  
`$ pg_dump -n 'east*gsm' -n 'west*gsm' -N '*test*' mydb > db.sql`   **

**PS**:  mydb should always be the end of command:
download DB postgres_sh, with only schema task_controller_testing, with user postgres, with server IP 10.16.187.100 into file.   
`pg_dump -U postgres -h 10.16.187.100 -n task_controller_testing postgres_sh> back.sql`

# import from dump
https://www.postgresql.org/docs/8.1/static/backup.html#BACKUP-DUMP-RESTORE   
https://stackoverflow.com/questions/6842393/import-sql-dump-into-postgresql-database   
`psql databasename < data_base_dump`  databasename DB must be created before you import your dump.sql   
  
# Auth issues  
https://stackoverflow.com/questions/18664074/getting-error-peer-authentication-failed-for-user-postgres-when-trying-to-ge  

The problem is still your pg_hba.conf file (/etc/postgresql/9.1/main/pg_hba.conf). This line:  
`local   all             postgres                                peer`

Should be   
`local   all             postgres                                md5`
