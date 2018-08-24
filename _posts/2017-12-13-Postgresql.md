---
layout: post
title:  "Postgresql"
date:   2017-12-13 14:58:39 +0100
comments: true 
categories: Postgresql
---


- [work with command line](#work-with-command-line)
- [Login with cmd](#login-with-cmd)
- [work with python](#work-with-python)
- [backup your database:](#backup-your-database)
- [import from dump via cmd](#import-from-dump-via-cmd)
- [import from dump via pgAdmin SQL](#import-from-dump-via-pgadmin-sql)
- [Auth issues](#auth-issues)
- [Docker postgres](#docker-postgres)
- [Replace text](#replace-text)
- [Query pattern match](#patter-match)  


# work with command line   
add env path C:\Program Files\PostgreSQL\9.6\bin   
for linux download `sudo apt-get install postgresql-client`   

# Login with cmd
https://code.i-harness.com/en/q/756e5a    
```
# psql -U postgres -h <your IPv4> -W
postgres# \c <database name>
```

# work with python
```
import psycopg2
import os

# Connect to an existing database
try:
    conn = psycopg2.connect("dbname='postgres' user='xxx' host='127.0.0.1' port='5432' password='xxx'")
except:
    print("I am unable to connect to the database")

# Open a cursor to perform database operations
cur = conn.cursor()

# Execute a command: this creates a new table
cur.execute("""CREATE TABLE IF NOT EXISTS schema.table
(
    id serial PRIMARY KEY,
    analytic_name character varying COLLATE pg_catalog."default" NOT NULL,
    analytic bytea NOT NULL,
    CONSTRAINT analytics_storage_analytic_name_key UNIQUE (analytic_name)
);""")

# Pass binary to fill a query placeholders and let Psycopg perform
# the correct conversion (no more SQL injections!)
# http://initd.org/psycopg/docs/usage.html#binary-adaptation
filename = 'test.zip'
file_dir = os.path.join('temp', filename)
analytic_binary = open(file_dir, 'rb').read()
try:
    cur.execute("INSERT INTO schema.table (analytic_name, analytic) VALUES (%s, %s) ON CONFLICT DO NOTHING", (filename, psycopg2.Binary(analytic_binary),))
except Exception as e:
    print(e)
    # https://stackoverflow.com/questions/2979369/databaseerror-current-transaction-is-aborted-commands-ignored-until-end-of-tra
    conn.rollback() # go to the normal status and ignore this transaction exception


# Query the database and obtain binary as Python objects
# http://www.postgresqltutorial.com/postgresql-python/blob/
try:
    # cur.execute("SELECT COUNT(*) FROM common.analytics_storage;")
    cur.execute("SELECT analytic_name FROM schema.table;")
    names = cur.fetchall()
except Exception as e:
    print(e)
    conn.rollback()

for i in names:
    cur.execute("""SELECT analytic FROM schema.table
                   WHERE analytic_name=%s""",
                (i[0],))
    analytic_binary = cur.fetchone()[0]
    des_dir = os.path.join('temp',i[0])
    open(des_dir, 'wb').write(analytic_binary)

# Make the changes to the database persistent
conn.commit()

# Close communication with the database
cur.close()
conn.close()
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

# Docker postgres
[blog example use volume container as volume](https://ryaneschinger.com/blog/dockerized-postgresql-development-environment/)
postgres docker images: https://hub.docker.com/_/postgres/

[official example using volume](https://docs.docker.com/storage/volumes/#create-and-manage-volumes)

Use a dockerized persistant volume to store data
```
docker create -v /var/lib/postgresql/data --name postgres-data busybox
```
the path should be the same as:
Dockerfile for postgres:10.2-alpine
ENV PGDATA /var/lib/postgresql/data

start postgres instance
```
docker run --name Mypostgres -e POSTGRES_PASSWORD=postgres -d --volumes-from postgres-data postgres:10.2-alpine
```

expose the port to host(Linux VM in virtualbox)
```
docker run --name Mypostgres -p 5433:5432 -e POSTGRES_PASSWORD=postgres  -d --volumes-from postgres-data postgres:10.2-alpine
```

expose to psql
```
docker run -it --rm --link Mypostgres:postgres postgres:10.2-alpine psql -h postgres -U postgres

# or
docker run -it --link Mypostgres:postgres --rm postgres:10.2-alpine sh -c 'exec psql -h "$POSTGRES_PORT_5432_TCP_ADDR" -p "$POSTGRES_PORT_5432_TCP_PORT" -U postgres'
```

expose to an app
```
docker run --name some-app --link some-postgres:postgres -d application-that-uses-postgres
```

expose to pgadmin4 docker
```
# first install pgadmin4 docker
docker pull dpage/pgadmin4
docker run -p 80:80 \
-e "PGADMIN_DEFAULT_EMAIL=user@domain.com" \
-e "PGADMIN_DEFAULT_PASSWORD=SuperSecret" \
-d dpage/pgadmin4
```

Open browser in Linux VM `localhost:80` and then login with email and pass, finally add the server connection, use network interface enp0s3 IP:5433 from the Linux VM.

If your Docker host is a Linux VM by virtualbox NAT, then you want access pgadmin4 from host PC:
```
ssh -N -L localhost:8888:localhost:80 user@127.0.1.1
```
![port forward with NAT network](https://raw.githubusercontent.com/YDD9/YDD9.github.io/master/images/VirtualBoxPortForwarding.png)
remote_host: 127.0.1.1 special loopback IP!!!</br>
ssh -N -L localhost:8888:localhost:8889 user@remote_host</br>
From <https://github.com/YDD9/YDD9.github.io/blob/master/_posts/2017-12-07-Spark-Python3-Linux.md> </br>


# [Replace text](https://stackoverflow.com/questions/5060526/postgresql-replace-all-instances-of-a-string-within-text-field)

You want to use postgresql's `replace` function:
```
replace(string text, from text, to text)
```

for instance :
```
# simply replace inside a string
replace( 'abcdefabcdef', 'cd', 'XX')

# text from a column in a table
UPDATE <table> SET <field> = replace(<field>, 'small', 'big')
```

`regexp_replace ` function is better in controller what text to be replaced.</br>
```
regexp_replace(string text, pattern text, replacement text [,flags text])

regexp_replace('Thomas', '.[mN]a.', 'M')
```
flags i and g for case-insensitive and global matching, respectively. </br>
\m and \M to match the beginning and the end of a word.</br>
```
SELECT regexp_replace('Cat bobcat cat cats catfish', 'cat', 'dog');
-->                    Cat bobdog cat cats catfish

SELECT regexp_replace('Cat bobcat cat cats catfish', '\mcat\M', 'dog', 'gi');
-->                    dog bobcat dog cats catfish

UPDATE <table> SET <field> = regexp_replace(<field>, '\mcat\M', 'dog', 'gi');

```

# Query pattern match
[SIMILAR TO Regular Expressions](https://www.postgresql.org/docs/8.3/static/functions-matching.html)

LIKE pattern matches always cover the entire string. To match a sequence anywhere within a string, the pattern must therefore **start and end with a percent sign**.

the SIMILAR TO operator succeeds only if its pattern matches the entire string; this is unlike common regular expression practice, wherein the pattern can match any part of the string. Also like LIKE, SIMILAR TO uses _ and % as wildcard characters denoting any single character and any string, respectively (these are comparable to . and .* in POSIX regular expressions).

```
string SIMILAR TO pattern [ESCAPE escape-character]
string NOT SIMILAR TO pattern [ESCAPE escape-character]

select * from schema.table
-- where 'B1_AB_C50XYZ' like '%' || signal_name || '%';
where signal_name similar to 'B[1-4]_AB_C[0-9]+XYZ'
```
