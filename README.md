Project Steps 


I.Create Tables

1. We created the sql_queries.py to create and drop
    a.the fact table: songlays
    b. dimension tables: songs, artists, users and time
    
    
    

2. WE ran the file create_tables.py to create the database Sparkifydb and tables
    # create sparkify database with UTF8 encoding
    cur.execute("DROP DATABASE IF EXISTS sparkifydb")
    cur.execute("CREATE DATABASE sparkifydb WITH ENCODING 'utf8' TEMPLATE template0")
    conn.set_session(autocommit=True)
    # close connection to default database
    conn.close() 
    # connect to sparkify database
    dbname = "sparkifydb"
    conn = psycopg2.connect("host=127.0.0.1 dbname=sparkifydb user=student password=student")
    conn.set_session(autocommit=True)
    cur = conn.cursor()
    return cur, conn
    
3. We created functions drop_tables and create_tables to process queries written sql_queries.py

    def drop_tables(cur, conn):
    """
    Drops each table using the queries in `drop_table_queries` list.
    """
    for query in drop_table_queries:
        cur.execute(query)
        conn.commit()

    def create_tables(cur, conn):
    """
    Creates each table using the queries in `create_table_queries` list. 
    """
    for query in create_table_queries:
        cur.execute(query)
        conn.commit()

4. We ran th Run test.ipynb to confirm the creation of your tables with the correct columns and made sure to close the connection. 



II. Build ETL Processes(etl.ipynb and etl.py)
After connecting to SPARKIFYDB, we started the ETL processes by create the get_files() function to get the file path of json files.

    def get_files(filepath):
    all_files = []
    for root, dirs, files in os.walk(filepath):
        files = glob.glob(os.path.join(root,'*.json'))
        for f in files :
            all_files.append(os.path.abspath(f)) 
    return all_files
 
1. Process song_data files by extrating and inserting the data for songs and artists Tables
2. Process log_data files by exracting and inseting the data into songplays(fact table), time and user tables.
3. Close the connection
Then add all the ETL processes from the etl.ipynb file to etl.py to insert all the data into the tables successfully.



III. Test results(test.ipynb)
#Run songplays table
%sql SELECT * FROM songplays LIMIT 10;
* postgresql://student:***@127.0.0.1/sparkifydb
10 rows affected.

#Run users table
%sql SELECT * FROM users LIMIT 5;
 * postgresql://student:***@127.0.0.1/sparkifydb
5 rows affected.

#Run songs table
%sql SELECT * FROM songs LIMIT 5;
 * postgresql://student:***@127.0.0.1/sparkifydb
1 rows affected.
song_id	title	artist_id	year	duration
SOMZWCG12A8C13C480	I Didn't Mean To	ARD7TVE1187B99BFB1	0	218.93179

#Run artists table
%sql SELECT * FROM artists LIMIT 5;
 * postgresql://student:***@127.0.0.1/sparkifydb
5 rows affected.
artist_id	artist_name	artist_location	artist_latitude	artist_longitude
ARD7TVE1187B99BFB1	Casual	California - LA	NaN	NaN
ARNTLGG11E2835DDB9	Clp		NaN	NaN
AR8ZCNI1187B9A069B	Planet P Project		NaN	NaN
AR10USD1187B99F3F1	Tweeterfriendly Music	Burlington, Ontario, Canada	NaN	NaN
ARMJAGH1187FB546F3	The Box Tops	Memphis, TN	35.14968	-90.04892

#Run time table
%sql SELECT * FROM time LIMIT 5;
 * postgresql://student:***@127.0.0.1/sparkifydb
5 rows affected.
start_time	hour	day	week	month	year	weekday
2018-11-30 00:22:07.796000	0	30	48	11	2018	4
2018-11-30 01:08:41.796000	1	30	48	11	2018	4
2018-11-30 01:12:48.796000	1	30	48	11	2018	4
2018-11-30 01:17:05.796000	1	30	48	11	2018	4
2018-11-30 01:20:56.796000	1	30	48	11	2018	4

