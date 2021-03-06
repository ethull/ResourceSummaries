# exmaple scripts to create a psql table
```sql
--Account example
    CREATE TABLE account(
       user_id serial PRIMARY KEY,
       username VARCHAR (50) UNIQUE NOT NULL,
       password VARCHAR (50) NOT NULL,
       email VARCHAR (355) UNIQUE NOT NULL,
       created_on TIMESTAMP NOT NULL,
       last_login TIMESTAMP
    );


    CREATE TABLE role(
       role_id serial PRIMARY KEY,
       role_name VARCHAR (255) UNIQUE NOT NULL
    );

    CREATE TABLE account_role
    (
      user_id integer NOT NULL,
      role_id integer NOT NULL,
      grant_date timestamp without time zone,
      PRIMARY KEY (user_id, role_id),
      CONSTRAINT account_role_role_id_fkey FOREIGN KEY (role_id)
          REFERENCES role (role_id) MATCH SIMPLE
          ON UPDATE NO ACTION ON DELETE NO ACTION,
      CONSTRAINT account_role_user_id_fkey FOREIGN KEY (user_id)
          REFERENCES account (user_id) MATCH SIMPLE
          ON UPDATE NO ACTION ON DELETE NO ACTION
    );
--Customer data example
CREATE SEQUENCE user_id_seq2;
    CREATE TABLE public.customer_data
    (
        --Integer type, cant be null, attach sequence
        tally integer NOT NULL DEFAULT nextval('user_id_seq2'::regclass),
        --Takes up to 32 char, COLLATE organises the text in the column,
        user_id character(32) COLLATE pg_catalog."default" NOT NULL,
        address text COLLATE pg_catalog."default",
        neutralised_b BOOL,
        fleas integer,
        --JSON notation
        ObjectInJSON json,
        other_behaviour text,
        --Date format 'YYYY-MM-DD'
        date_current date,
        --Can add all keys at the end, names constraints
        --Apply primary key constraint
        CONSTRAINT customerdata_pk PRIMARY KEY (user_id)
        --Add foriegn key restraint to own column (generally foriegn key child shouldnt be a primary key (but the parent normally is))
        CONSTRAINT users_fkey FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
			)
			WITH (
				OIDS = FALSE
			)
			TABLESPACE pg_default;
			--Establish sequence ownership
			ALTER SEQUENCE user_id_seq2 OWNED BY customer_data.tally;
			--Change table owner to superuser account
			ALTER TABLE public.customer_data
				OWNER to postgres;
			COMMENT ON TABLE public.customer_data
				IS 'Contains main data on JPP users';
```
