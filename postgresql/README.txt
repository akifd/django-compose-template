To create user and database, run with psql:

    CREATE USER tuntikirjaus PASSWORD 'yourpassword' CREATEDB;
    CREATE DATABASE tuntikirjaus OWNER tuntikirjaus TEMPLATE template0 ENCODING 'UTF8' LC_COLLATE 'fi_FI.utf8' LC_CTYPE 'fi_FI.utf8';
