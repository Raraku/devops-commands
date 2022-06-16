# Migrate an on-premise vm to cloud sql for postgres (GCP)

- Install pglogical on the onpremise db with:

  `sudo apt install postgresql-13-pglogical`

- Download and apply some additions to the PostgreSQL configuration files (to enable pglogical extension) and restart the postgresql service:

  ````sudo su - postgres -c "gsutil cp gs://cloud-training/gsp918/pg_hba_append.conf ."

  sudo su - postgres -c "gsutil cp gs://cloud-training/gsp918/postgresql_append.conf ."

  sudo su - postgres -c "cat pg_hba_append.conf >> /etc/postgresql/13/main/pg_hba.conf"

  sudo su - postgres -c "cat postgresql_append.conf >> /etc/postgresql/13/main/postgresql.conf"

  sudo systemctl restart postgresql@13-main```
  ````

  these commands allow access to all hosts and configures pglogical.

- launch psql
- Add the pglogical database extension to the databases you wish to replicate (ex. orders, postgres)

  ```
  \c postgres;
  CREATE EXTENSION pglogical;
  \c orders;
  CREATE EXTENSION pglogical;
  \c gmemegen_db;
  CREATE EXTENSION pglogical;
  ```

- Create a user on the primary db with replication permissions

  ```
  CREATE USER migration_admin PASSWORD 'DMS_1s_cool!';
  ALTER DATABASE orders OWNER TO migration_admin;
  ALTER ROLE migration_admin WITH REPLICATION;
  ```

- Grant permission to pglogical schemas for the migration user on each database.

  ```
  \c postgres;
  GRANT USAGE ON SCHEMA pglogical TO migration_admin;
  GRANT ALL ON SCHEMA pglogical TO migration_admin;
  GRANT SELECT ON pglogical.tables TO migration_admin;
  GRANT SELECT ON pglogical.depend TO migration_admin;
  GRANT SELECT ON pglogical.local_node TO migration_admin;
  GRANT SELECT ON pglogical.local_sync_status TO migration_admin;
  GRANT SELECT ON pglogical.node TO migration_admin;
  GRANT SELECT ON pglogical.node_interface TO migration_admin;
  GRANT SELECT ON pglogical.queue TO migration_admin;
  GRANT SELECT ON pglogical.replication_set TO migration_admin;
  GRANT SELECT ON pglogical.replication_set_seq TO migration_admin;
  GRANT SELECT ON pglogical.replication_set_table TO migration_admin;
  GRANT SELECT ON pglogical.sequence_state TO migration_admin;
  GRANT SELECT ON pglogical.subscription TO migration_admin;
  ```

- Grant permissions to the public schema and tables for the databases
  ```
  GRANT USAGE ON SCHEMA public TO migration_admin;
  GRANT ALL ON SCHEMA public TO migration_admin;
  GRANT SELECT ON public.distribution_centers TO migration_admin;
  GRANT SELECT ON public.inventory_items TO migration_admin;
  GRANT SELECT ON public.order_items TO migration_admin;
  GRANT SELECT ON public.products TO migration_admin;
  GRANT SELECT ON public.users TO migration_admin;
  ```

# On GCP

- Create a connection profile for a standalone postgresql database (follow documentation)
- Create a continuous migration job
- Create source and destination instances.
- Choose a connectivity method (VPC peering recommended)
- Allow access to the destination ip range of the vpc peering `VPC Network > VPC Network Peering` to get the ip-range.
- edit the pg_hba.conf file of your on-premise postgres instance to allow access to only hosts using the destination ip-range.
- Restart your postgres db with `sudo systemctl start postgresql@13-main` (This command will change for different versions).
- Test and start the continuous migration job

Congrats, you're done!
