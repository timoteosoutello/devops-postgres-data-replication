# DevOps Postgres Data Replication

Postgres 13 Dockerized with Replication
Master/Slave Postgres Replication through logical method

**Quickstart**: docker-compose up

# HOW-TO Test

Run in all instances:

*CREATE TABLE customers (*
  *id int GENERATED ALWAYS AS IDENTITY PRIMARY KEY,*
  *first_name varchar(255) NOT NULL,*
  *last_name varchar(255) NOT NULL,*
  *email varchar(255) NOT NULL*
*);*

Run in **db-origin**(Master):

*CREATE PUBLICATION PUBLICATION_TO_ALL FOR ALL TABLES;*

Run in **db-intermediate**(Slave 1):

*CREATE PUBLICATION PUBLICATION_TO_ALL FOR ALL TABLES;* (just to test if the db-final-destination will be read to read through replica)

*CREATE SUBSCRIPTION SUBSCRIBE_TO_ALL_2 CONNECTION 'host=db-origin dbname=postgres user=postgres password=admin' PUBLICATION PUBLICATION_TO_ALL;* (To read from Master)

Run in **db-destination**(Slave 2):

*CREATE SUBSCRIPTION SUBSCRIBE_TO_ALL CONNECTION 'host=db-intermediate dbname=postgres user=postgres password=admin' PUBLICATION PUBLICATION_TO_ALL_2;* (To read from Slave)



Test Insert / Update commands:

*INSERT INTO customers (first_name, last_name, email) VALUES ('Timoteo', 'Soutello', 'tsoutello@gmail.com);*

# Useful commands

* Get the default config
  * docker run -i --rm postgres cat /usr/share/postgresql/postgresql.conf.sample > my-postgres.conf
  * docker run -i --rm postgres cat /usr/share/postgresql/13/pg_hba.conf.sample > pg_hba.conf

# References

* https://www.postgresql.org/docs/13/logical-replication.html
* https://www.postgresql.org/docs/13/logical-replication-config.html
* https://www.postgresql.org/docs/13/logical-replication-quick-setup.html
* https://www.postgresql.org/docs/current/runtime-config-wal.html
* https://www.enterprisedb.com/postgres-tutorials/logical-replication-postgresql-explained
* https://blog.raveland.org/post/postgresql_lr_en/