# concepts around psql statements
```sql
--postgreSQL
	--aggregate functions
		SUM(), COUNT(), AVG(), MAX(), MIN()
		--ARRAY_AGG(), Takes set of values and returns an array
			ARRAY_AGG(expression [ORDER BY [sort_expression {ASC | DESC}], [...])
			SELECT ARRAY_AGG (first_name || ' ' || last_name) actors FROM film
		--STRING_AGG(), concatenates a list of strings and places a separator between them
			STRING_AGG ( expression, separator [ORDER BY expression1 {ASC | DESC}, [...]] )
			STRING_AGG (first_name || ' ' || last_name, ',' ORDER BY first_name, last_name) actors
	--postgres Functions
		pg_typeof([columnName])
	--Utility operators
		OR, IS, IN, NOT, 
	--Value operators
		NULL, DEFAULT
	--Data types
		Boolean, Bool
		Character
			char, varchar, text
		Numeric
			integer, float
				-- 2 byte signed integer 
				SMALLINT
				-- 4 byte integer
				INT
				--SQLite: AUTOINCREMENT, MySQL: AUTO_INCREMENT
				SERIAL
				--Double precision float (8 byte)
				realor float
				--Real number, p digits with s number after deciaml point
				numeric(p,s)
					
		Temporal
			date, time, 
			-- Stores date and time
			timestamp
			--Timezone aware timestamp (postgreSQL only)
			TIMESTAMPTZ
			--Periods of time
			interval
		UUID
		Array
			array strings, numbers
		JSON
		hstore
			key-value pair
		Special types
				network address
					--IPv4, MAC
						inet, macaddr
				geometric data
					--rectangular box, set of points, geometric pair of numbers, line segment, closed geometric 
					box, line, point, lseg, polygon
--Create DB
    CREATE DATABASE name
    [ [ WITH ] [ OWNER [=] user_name ]
           [ TEMPLATE [=] template ]
           [ ENCODING [=] encoding ]
           [ LC_COLLATE [=] lc_collate ]
           [ LC_CTYPE [=] lc_ctype ]
           [ TABLESPACE [=] tablespace ]
           [ CONNECTION LIMIT [=] connlimit ] ]
    CREATE DATABASE JPP OWNER postgres 
					
		
		
--Creating a table
	--Syntax, inherit in postgreSQL only
		CREATE TABLE table_name (
		 column_name TYPE column_constraint, table_constraint table_constraint) INHERITS existing_table_name;
		
	--Examples
		
		--Copy anouther table	
			CREATE TABLE link_tmp (LIKE link);
		--Create temp table
			--Deleted by postgreSQL after DB session
			CREATE TEMP TABLE temp_table (column_list);
			
	--Column constraints
		--syntax: column_name TYPE column_constraint
			--Value cant be null, Value of column must be unqiue across entire table (doesnt include null (unlike SQL)),
			 --check a condition when inserting or updating data,
			 --Constrains the value of the column that exists in a column in another table (Use to define foriegn key), NOT NULL + UNQIUE
			NOT NULL, UNQIUE, CHECK, REFERENCES  
			PRIMARY KEY: 
		--EG
			username VARCHAR (50) UNIQUE NOT NULL
		--GENERATED AS IDENTITY constraint, postgreSQL only
			--Automatically assign a unique value to a column, err raised if you try to insert
			--Can be smallint, bigint or int
			GENERATED { ALWAYS | BY DEFAULT } AS IDENTITY[ ( sequence_option ) ]
			--can overide sequence
				INSERT INTO color (color_id, color_name) OVERRIDING SYSTEM VALUE VALUES (2, 'Green');
				--or can use  
				 GENERATED BY DEFAULT AS IDENTITY,
	--Table level constraints (appplied to entire table rather than one column)
		UNQIUE(column_list), PRIMARY KEY(column_list), CHECK (condition), REFERENCES
	
	--Advanced creation statements syntax
		CREATE DATABASE db_name 
		 OWNER =  role_name TEMPLATE = template ENCODING = encoding LC_COLLATE = collate LC_CTYPE = ctype
		 TABLESPACE = tablespace_name CONNECTION LIMIT = max_concurrent_connection
		--template: is the name of the database template from which the new database creates (default =template1), most other options default to the templates options
		--collate: Collation for entire DB, this specifies the sort order of strings (affects ORDER BY clause results)
		--ctype: Character classification for the new database.
		--EG
			CREATE DATABASE hrdb WITH ENCODING='UTF8'OWNER=hr CONNECTION LIMIT=25;
	
	--SELECT INTO
		--Create table and insert data via query, note use CREATE TABLE AS instead for PL/pgSQL or ECPG, doesnt return anything
		SELECT column_list INTO [ TEMPORARY | TEMP | UNLOGGED ] [ TABLE ] new_table_name FROM
		 table_name WHERE condition;
	--CREATE TABLE AS 
		CREATE TABLE new_table_name ( column_name_list) AS query;
	
	
--Foriegn keys
	--Parent/referenced table, Sales order headers
	CREATE TABLE so_headers (
	   --Serial so automatically increments by 1 for each row, cant be edited unless row is removed
	   id SERIAL PRIMARY KEY,
	   customer_id INTEGER,
	   ship_to VARCHAR (255)
		);
	
	--Child/referencing table, Sales order line items
	CREATE TABLE so_items (
	  item_id INTEGER NOT NULL,   
	  --Foriegn key constraint
	  so_id INTEGER REFERENCES so_headers(id) ON DELETE RESTRICT,
	  product_id INTEGER,
	  qty INTEGER,
	  net_price numeric,
	  PRIMARY KEY (item_id,so_id)
	);
	
	--Each line item of a sales order must belong to a specific sales order (in the so_headers parent table)
	--Each sales order can have one or many line items.
		--So its a one-to-many relationship
		
		--As a result cant insert into so_items without referencing a valid so_id that matches the parent column
		--hence FK ensures the table contains data of sales items that exist
		
	--Parameter after constraint decides what happens when a row in the parent table is deleted.
		--ON DELETE RESTRICT: Doesnt allow the row to be deleted from parent table (so_headers) until all the referenced rows in child table are deleted (so_items)
		--ON DELETE CASCADE: delete all rows in the so_items table that are referenced to the rows that are being deleted in the so_headers table.
		--ON DELETE NO ACTION: DO nothing
		--Can also apply to updates: ON UPDATE RESTRICT, ON UPDATE CASCADE and ON UPDATE NO ACTION.
	
	--Define group of columns as the foriegn key
	FOREIGN KEY (c2, c3) REFERENCES parent_table (p1, p2)

--Copy DB
	--Within same server
	CREATE DATABASE targetdb WITH TEMPLATE sourcedb;
	--Between different servers
		--dump the source database to a file ->  copy the dump file to the remote server
		-- -> create DB on remote server -> restore dump file on the remote server
			pg_dump -U postgres -O sourcedb sourcedb.sql
			
			CREATE DATABASE targetdb;
			psql -U postgres -d targetdb -f sourcedb.sql
		--If small DB and good connection can instead use
			pg_dump -C -h local -U localuser sourcedb | psql -h remote -U remoteuser targetdb
--Sequences
	--user-defined schema-bound object that generates a sequence of integers 
	--syntax
		--cache: specifies number of sequence numbers preallocated to memory, CYCLE: Restart value if limit reached (after passing default limit err is raised)
		CREATE SEQUENCE [ IF NOT EXISTS ] sequence_name
		 [ AS { SMALLINT | INT | BIGINT } ] [ INCREMENT [ BY ] increment ] [ MINVALUE minvalue | NO MINVALUE ] 
		 [ MAXVALUE maxvalue | NO MAXVALUE ] [ START [ WITH ] start ] [ CACHE cache ] 
		 [ [ NO ] CYCLE ]  [ OWNED BY { table_name.column_name | NONE } ]	
	--Unassociated sequence
		CREATE SEQUENCE mysequence INCREMENT 5 START 100;
		--Increment by 5
		SELECT nextval('mysequence');
	--Sequence associated with column
		CREATE SEQUENCE order_item_id START 10 INCREMENT 10 MINVALUE 10 OWNED BY order_details.item_id;
		--Insert using nextval sequence, can also insert null and will auto-increment
		INSERT INTO order_details(order_id, item_id, item_text, price) VALUES (100, nextval('order_item_id'),'DVD Player',100);
	--List sequences in the database
		SELECT c.relname sequence_name FROM pg_class WHERE relkind = 'S';
	--Del sequence
		DROP SEQUENCE [ IF EXISTS ] sequence_name [, ...] [ CASCADE | RESTRICT ];
	--EG where tally keeps track of the row, but user_id is the primary key, allows for a flexible tally
			--Sequence controls progression of tally
			CREATE SEQUENCE user_id_seq;
			CREATE TABLE customer_data
			(
				--Integer type, cant be null, attach sequence
				tally integer NOT NULL DEFAULT nextval('user_id_seq2'::regclass),
				--Takes up to 32 char, COLLATE organises the text in the column,
				user_id character(32) COLLATE NOT NULL
				--Can add all keys at the end, names constraints
				--Apply primary key constraint
				CONSTRAINT customerdata_pk PRIMARY KEY (user_id)
				--Add foriegn key restraint to own column (generally foriegn key child shouldnt be a primary key (but the parent normally is))
				CONSTRAINT users_fkey FOREIGN KEY (user_id) REFERENCES users(id) ON DELETE CASCADE
			)
			ALTER SEQUENCE user_id_seq2 OWNED BY customer_data.tally;
		--If incrementing via one can use SERIAL instead, also PK setup locally
			user_id serial PRIMARY KEY,		
				
	
--Alter existing tables
	--Syntax: ALTER TABLE [itemName] [action] [itemSubset] [itemSubsetName]  
	--Add foriegn key
		ALTER TABLE child_table 
		ADD CONSTRAINT constraint_name FOREIGN KEY (c1) REFERENCES parent_table (p1);	
	--Edit columns
		--Add column																					 
			ALTER TABLE table_name, can add multiple columns via chaining multiple statements using comments
			ADD COLUMN new_column_name data_type constraint;		
			--Adding a column with a NOT NULL restraint will violate the rule
				--create the data normally -> update values -> set NOT NULL
					ALTER TABLE customers
					ALTER COLUMN contact_name SET NOT NULL;																				 
				--or add column with default value
		--Del column
			ALTER TABLE table_name DROP COLUMN column_name;
		--Rename column, remove COLUMN column_name to rename table				 
			ALTER TABLE table_name RENAME COLUMN column_name TO new_column_name; 	
		--Change default value
			ALTER TABLE table_name ALTER COLUMN column_name [SET DEFAULT value | DROP DEFAULT];
		--Change datatype
			ALTER TABLE table_name ALTER COLUMN column_name [SET DATA] TYPE new_data_type;
		--Add constraint
			ALTER TABLE table_name ADD CONSTRAINT constraint_name constraint_definition (targetColumn);
	--Ownership
		--If role doesnt exist, create new role and assign ownership
		CREATE ROLE HR VALID UNTIL 'infinity';
		ALTER TABLE customer_data OWNER TO HR;
		--Use existing roles
		ALTER TABLE customer_data OWNER TO postgres;
	--Tablespace
		--If tablespace doesnt exist create it
			CREATE TABLESPACE hr_default OWNER hr LOCATION E'C:\\pgdata\\hr';
		--Attach to table
			ALTER DATABASE testhrdb SET TABLESPACE hr_default;
	
	--Comment on table	
		COMMENT ON TABLE customer_data
				IS 'DEscription of customer_data table';
	--Change session defaults for run-time configuration variables
	 --Default configuration variables presented in the postgresql.conf	
		ALTER DATABASE target_database SET configuration_parameter = value;
		--Set escape_string_warning configuration variable to off
			ALTER DATABASE testhrdb SET escape_string_warning TO off;
--Schema
	--namespace that contains named database objects such as tables, views, indexes, data types, functions, and operators.
	--Organise obj and allow concurrent connections
	--Access obj
	schema_name.object_name
	--schema search path is the list of schemas postgres looks in to find requested tables, the first schema = current schema
	 --Default schema is called public, its automatically the current schema so you can access it without specifying schema 
	CREATE TABLE public.table_name(...); = CREATE TABLE table_name(...);
	
	 --First schema = current_schema
		SELECT current_schema();
		SHOW search_path;
		--Create schema
			CREATE SCHEMA sales;
			--Move to front of search path
			SET search_path TO sales, public;
		--Users dont have access to un-owned schemas, give people access
			GRANT USAGE ON SCHEMA schema_name TO user_name;
		--Use ALTER to rename or del schema, use DROP to del
		--Create schema in depth
			CREATE SCHEMA [IF NOT EXISTS] AUTHORIZATION user_name;
			CREATE SCHEMA schema_name
			 CREATE TABLE table_name1 (...) CREATE TABLE table_name2 (...) CREATE VIEW view_name1
			 SELECT select_list FROM table_name1;
		--Create schema for anouther user
			CREATE USER john WITH ENCRYPTED PASSWORD 'Postgr@s321!';
			--Create schema they can use
			CREATE SCHEMA AUTHORIZATION john;
			--Create schema they own
			CREATE SCHEMA IF NOT EXISTS doe AUTHORIZATION john;
		--Selected all schemas
			SELECT * FROM pg_catalog.pg_namespace ORDER BY nspname;
		--Select user created schema
			SELECT * FROM pg_catalog.pg_namespace WHERE nspacl is NULL AND nspname NOT LIKE 'pg_%' ORDER BY nspname;
		--Del schema
			DROP SCHEMA [IF EXISTS] schema_name [ CASCADE | RESTRICT ];
			
		
		


--Get the size of different objects in postgreSQL
	--Specific table size
		SELECT pg_relation_size('actor');
		--Convert bytes output to more readable format
			SELECT pg_size_pretty (pg_relation_size('actor'));
	--Total size of table (including dependancies)
		SELECT pg_size_pretty (pg_total_relation_size ('actor'));
	--Get DB size
		SELECT pg_size_pretty (pg_database_size ('dvdrental'));
	--Get DB size of each DB in current server
		SELECT pg_database.datname, pg_size_pretty(pg_database_size(pg_database.datname)) AS size
		 FROM pg_database;
	--Get size of all indexed atached to a table
		SELECT pg_size_pretty (pg_indexes_size('actor'));
	--Tablespace size	
	SELECT pg_size_pretty (pg_tablespace_size ('pg_default'));	
	--Column size
		SELECT pg_column_size(column_name);
		--EG with passed datatype
		SELECT pg_column_size(5::smallint);
		
					
									
--Edit table	
	--Inserting into a table 
		syntax
			INSERT INTO table(column1, column2, …) VALUES (value1, value2, …);
			--Multiple rows at once
			INSERT INTO table (column1, column2, …) VALUES (value1, value2, …), (value1, value2, …) ,...;
			--Insert data that comes from anoutehr table
			INSERT INTO table(column1,column2,...) SELECT column1,column2,... FROM another_table WHERE condition;
		--Inserting date
		INSERT INTO link (url, name, date_current)
		 VALUES
		   ('https://www.tumblr.com/','Tumblr', DEFAULT);
		--Get the last insert id from the table after inserting a new row
		INSERT INTO link (url, NAME, last_update) VALUES('http://www.postgresql.org','PostgreSQL',DEFAULT) 
		 RETURNING id;
		
		INSERT INTO public.customer_data(
							  count, user_id, address, mobile, dob, booleanItem
							  )
							  VALUES (1, '81fcbdf3-ad18-493a-a9bc-6a22512fa13c', 'test', '343444', '2001-07-19', 1);
		--React to constraint errors
		syntax
			INSERT INTO table_name (column_list) VALUES (value_list) ON CONFLICT target action;
		ON CONFLICT ON CONSTRAINT customers_name_key DO NOTHING;
		ON CONFLICT (column_name) DO NOTHING;
		--Upsert data, EG concatenate the new email with the old email when inserting a customer that already exists
		ON CONFLICT (name) DO UPDATE SET email = EXCLUDED.email || ';' || customers.email;
		
	--UPDATE syntax
		syntax
			--Leave out WHERE keyword to edit entire row, add RETURNING keyword to get changes
			UPDATE table SET column1 = value1, column2 = value2 ,... WHERE condition;
			--Use update join to update with data from other tables
			UPDATE A SET A.c1 = expresion FROM B WHERE A.c2 = B.c2;
	--DELETE	
		--syntax
			DELETE FROM table WHERE condition;
		--Consider anouther table
			DELETE FROM table USING another_table WHERE table.id = another_table.id AND …
			--or
			DELETE FROM table WHERE table.id = (SELECT id FROM another_table);
		--Delete tables
			DROP DATABASE [IF EXISTS] name;
			--If connection error occurs 
				--Query connections
					SELECT * FROM pg_stat_activity WHERE datname = 'testdb1';
				--Terminate connections	
					SELECT pg_terminate_backend (pg_stat_activity.pid) FROM pg_stat_activity
					 WHERE pg_stat_activity.datname = 'testdb1';
		--Truncate
			--removes all rows from a table without scanning it (much faster), reclaims storage right away
			TRUNCATE TABLE table_name;
			--Reset associated sequence generators
			TRUNCATE TABLE table_name RESTART IDENTITY;
			--By default doesnt remove data from table that has foriegn key references, to override  
			TRUNCATE TABLE table_name CASCADE;
			--To fire the trigger when the  TRUNCATE TABLE command applied to a table, you must define  BEFORE TRUNCATE and/or  AFTER TRUNCATE triggers for that table
						
--Copy table

--Select (queries)
	SELECT column_name, columnName2 FROM table_name;
	SELECT [pre_keyword] column_name FROM table_name [condition_keyword] [condition_data] [condition_keyword2] [condition_data2];
	--Can put NOT operator before most condition_keywords to reverse effect
																						 																				 
	--Only use expressions
	SELECT 5 * 3 AS result;
	
	--pre_keywords
		--Ignore rows with duplicate data 
			SELECT DISTINCT column_1 FROM table_name;																					 
			--With 2 columns with look for duplicates based on their combination
			--Keep first row of each group of duplicates
				--DISTINCT ON (column_1) column_alias, column_2																				 
				SELECT DISTINCT ON (column_1) column_alias, column_2 FROM table_name;
																						 																				 
	--condition_keywords
		--WHERE, combine clause and operators (AND, OR, < (and all variants))
			SELECT last_name, first_name FROM customer WHERE [condition_keyword] 
			
			first_name = 'Jamie' AND last_name = 'Rice';
			first_name IN ('Ann','Anne','Annie');																		 
			--% is the wildcard that matches any string								
			first_name LIKE 'Ann%'
			first_name LIKE 'A%' AND LENGTH(first_name) BETWEEN 3 AND 5						
		--ORDER BY, order column contents
			ORDER BY column_1 ASC, column_2 DESC;		
		--GROUP BY, divide query results into groups
		 --can use a column_list or an expression instead of one column, apply AF (sum(), count()) to get info on group (not applying AF can be used to remove duplicate columns only)
			SELECT column_1, aggregate_function(column_2) FROM tbl_name GROUP BY column_1;
			--Use having clause to sort columns, WHERE acts before GROUP BY but HAVING acts after
			SELECT column_1, aggregate_function (column_2) FROM tbl_name GROUP BY column_1 HAVING condition_;									 
			--GROUPING SETS allows you to define multiple grouping sets in the same query.
			 --EG  for table with 3 columns, query to create a single result set with the aggregates for all grouping sets.
				SELECT c1, c2, aggregate_function(c3) FROM table_name GROUP BY
				 GROUPING SETS ((c1, c2), (c1), (c2), ());
				--EG For table with columns brand, segment, quantity
				SELECT brand, segment, SUM (quantity) FROM sales GROUP BY
				 GROUPING SETS ((brand, segment), (brand), (segment), ());
			--Grouping function, returns 0 (is), 1 (isn't) relative to whether the passed column name is a member of the current grouping set (uses aliases to display the result)
				SELECT GROUPING(brand) grouping_brand, GROUPING(segment) grouping_segement, brand, segment,
				 SUM (quantity) FROM sales GROUP BY
				  GROUPING SETS ((brand, segment), (brand), (segment), ()) ORDER BY brand, segment;
			--CUBE allows you to generate multiple grouping sets 
				GROUP BY CUBE (c1, c2, c3);				
				--which = all possible grouping sets, hence =
				GROUPING SETS ((c1,c2,c3), (c1,c2), (c1,c3), (c2,c3), (c1), (c2), (c3), ()) 	
				--Partial cube to reduce number of aggregates calculated
				GROUP BY c1, CUBE (c1, c2);
			--ROLLUP, assumes a hierarchy among the input columns and generates all grouping sets that fit that hierarchy
				 --EG if c1 > c2 > c3
				 ROLLUP(c1,c2,c3)
				  --=
				  GROUPING SETS ((c1, c2, c3), (c1, c2), (c1), ())
			--Date example
				SELECT EXTRACT (YEAR FROM rental_date) y, EXTRACT (MONTH FROM rental_date) M, 
				 EXTRACT (DAY FROM rental_date) d, COUNT (rental_id) FROM rental GROUP BY
				 ROLLUP (EXTRACT (YEAR FROM rental_date), EXTRACT (MONTH FROM rental_date),
				  EXTRACT (DAY FROM rental_date));
				
		--LIMIT, reduce number of comlums selected
			--Skip first m rows, then take n rows																			 								
			LIMIT n OFFSET m;
			--Normally order is unpredictable so order how u want first																			 	
			ORDER BY film_id LIMIT 5; 
		--Fetch (SQLite compatible alt to LIMIT)
			syntax
				OFFSET start { ROW | ROWS }
				FETCH { FIRST | NEXT } [ row_count ] { ROW | ROWS } ONLY
			ORDER BY title FETCH FIRST ROW ONLY;
			
			
				
				
																					 
	--Aliases, assign alias to a query result, AS keyword is optional
		SELECT column_name AS alias_name FROM table;																				 
			--Merge results into one column (using expressions)
			SELECT first_name || ' ' || last_name AS full_name FROM customer;		
		SELECT column_list FROM table_name AS alias_name;
			--Shorten statements
				SELECT t.column_name FROM a_very_long_table_name t;			
				--Long statement that queries data from multiple tables that have the same column names																		 
				SELECT t1.column_name, t2.column_name FROM table_name1 t1 INNER JOIN table_name2 t2 ON join_predicate;																		 
			--To reference a table multiple times within a query, EG when self-joining tables
				SELECT colum_list FROM table_name table_alias INNER JOIN table_name ON join_predicate;																			 
	
	
	
	--Joins
		--Combine columns from one (self-join) or more tables based on values of common columns between these tables
		--Generally fk columns from first table and sk columns from the second table
		--Inner join, returns rows in the left table that match with rows from the right table
			SELECT a.id id_a, a.fruit fruit_a, b.id id_b, b.fruit fruit_b
			 FROM basket_a a INNER JOIN basket_b b ON a.fruit = b.fruit;	
			 --USING keyword, can use as a shortcut when columns have the same name
			 SELECT * FROM countries INNER JOIN economies USING(code)
			 					
		--Outer joins
			--Left join, all rows from left table query with avaliable matching rows right table (Some left rows may get a null match)																				 
				 LEFT JOIN basket_b b ON a.fruit = b.fruit;																		
				--To ignore matching rows (only get rows in the left table)
				WHERE b.id IS NULL;
			--Right join, reversed left join
				RIGHT JOIN basket_b b ON a.fruit = b.fruit;
			--full join, all rows from both left and right table query results, with matching rows where avaliable 	
				FULL JOIN basket_b b ON a.fruit = b.fruit;																		 
			 	--Ignore values that match
			 	WHERE a.id IS NULL OR b.id IS NULL;																			 
			--Could think of joins as a venn diagram, inner = AnB, left =  A, left where B is null = AnB', right = B, full = AuB, full where A or B is null = (AnB)'
		--Cross join, row that consists of all columns in the T1 table followed by all columns in the T2 table.
			SELECT * FROM T1 CROSS JOIN T2;				
			T1: A, B, T2: 1, 2, 3																			 
		--Natural join, implciit join based on same column names, effective for foriegn keys
		 	syntax
				SELECT * FROM T1 NATURAL [INNER, LEFT, RIGHT, *] JOIN T2;		
				--Though dont need to specify join clause as it uses an implicit join clause based on the common column, however may have unexpected results with multiple common columns																	 
				--Where categories is a fk in the products table																		 
				SELECT * FROM products NATURAL JOIN categories;	
				--The result is that columns are sorted around the fk's parent 
																					 
	--Combine results of two select statements
		SELECT column_1, column_2 FROM tbl_name_1 [combine_keyword] [select_statement2]
			--Union statement, requires same number of columns and the same data types within them
			 --removes dupes automatically unless UNION ALL used instead
				
				UNION SELECT column_1, column_2 FROM tbl_name_2; 
			--Intersect statement, returns rows that are the same in both queries  
				INTERSECT SELECT column_list FROM B; 
			--Except operator, returns distinct rows from the first query not in the output of the second query. 
				EXCEPT SELECT column_list FROM B WHERE condition_b;
	--Subqueires, query nested inside anouther query, inner query run -> result passed to outer query
		SELECT first_name, last_name FROM customer WHERE customer_id IN (
		 SELECT customer_id FROM rental WHERE CAST (return_date AS DATE) = '2005-05-27');
		SELECT film_id, title, rental_rate FROM film WHERE rental_rate > (
		 SELECT AVG (rental_rate) FROM film);
		--EXISTS operator, returns true if subquery returns any row
			--EG returns customers who have paid at least a rental which is greater than 11
			SELECT first_name, last_name FROM customer c WHERE EXISTS (
			 SELECT 1 FROM payment p WHERE p.customer_id = c.customer_id AND amount > 11 )
			  ORDER BY first_name, last_name;
		  																	 
		--ANY operator, compares a value to a set of values returned by a subquery, checks that if any value of the subquery meets the condition		
		SELECT title FROM film WHERE length >= ANY( SELECT MAX( length ) FROM film
		 INNER JOIN film_category USING(film_id) GROUP BY  category_id );
		--Note, = ANY does the same as IN, though != ANY doesnt equal NOT IN 
		--ALL operator, comparing a value with a list of values returned by a subquery, comapares outer query value against extreme value from subquery
		 --column_name > ALL (subquery) the expression evaluates to true if a value is greater than the biggest value returned by the subquery.
			--EG Return all films whose lengths are greater than the biggest value in the average length list returned by the subquery
			SELECT film_id, title, length FROM film WHERE length > ALL (
			 SELECT ROUND(AVG (length),2) FROM film GROUP BY rating) ORDER BY length;
	--Conditional operators
		--Case
			CASE WHEN condition_1  THEN result_1 WHEN condition_2  THEN result_2
			 [WHEN ...] [ELSE result_n] END
			
			SELECT SUM (CASE WHEN rental_rate = 0.99 THEN 1 ELSE 0 END) AS "Mass",
			 SUM (CASE WHEN rental_rate = 2.99 THEN 1 ELSE 0 END) AS "Economic",
			 SUM (CASE WHEN rental_rate = 4.99 THEN 1 ELSE 0 END) AS "Luxury" FROM film;
		--COALESCE
			--Accepts an unlimited number of arguments. It returns the first argument that is not null
			SELECT COALESCE (1, 2, "");
			--EG when applying a discount for a price, if there is no discount it could be null or 0
			SELECT product, (price - COALESCE(discount, 0)) AS net_price FROM items;
		--NULLIF
			--Rrturns null if arg1 = arg2, otherwise returns arg1
			SELECT NULLIF(argument_1,argument_2);
			--EG return first 40 text characters if the excerpt is empty
			SELECT id, title, COALESCE (NULLIF (excerpt, ''), LEFT (body, 40)) FROM posts;
	--CAST
		--Convert one datatype into anouther
		CAST ( expression AS target_type );
		--or
		expression::type
		SELECT '100'::INTEGER, '01-OCT-2015'::DATE;
		SELECT CAST ('10.2' AS DOUBLE PRECISION);
		SELECT CAST('T' as BOOLEAN), '2019-06-15 14:30:20'::timestamp, '15 minute'::interval,
```