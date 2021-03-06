

    import sqlite3 as sl
    import pandas as pd
    import numpy as np

## Problem 11.2. To and From Chicago.

In this problem, we will use the databse we have created in week 10
  and use SQLite and Pandas to extract some summary information
  for flights to and from Chicago (ORD).

The database we have created in week 10 has a table named `flights`,
  which comes from importing `2001.csv`.
  We will use only the `flights` table, but not the `iata` table.
  So, even if you couldn't complete week 10 assignment,
  you should be able to do this assignment,
  if you know how to use the provided schema
  [flights.sql](https://github.com/INFO490/spring2015/blob/master/week10/flights
.sql)
  to import `2001.csv`.


    # edit this path to point to your database
    db_path = "/data/week11.db"

### Function: get\_from\_ord()

Write a function named `get_from_ord()` that takes a string (the path to the
database)
   and returns a Pandas DataFrame.
   This function calculates the average departure delays at each destination
airport
   when the origin airport was the O'Hare airport.
   You should
   - use Pandas `read_sql()` function to read the SQL database,
   - find all flights that departed from Chicago (all rows where `originCode` is
`ORD`),
   - group by `destinationCode`,
   - calculate (for each airport) the average `departureDelay` of all flights
that departed from `ORD`,
   - sort the result in descending order of `avgDepartureDelay`, and
   - return a DataFrame with two columns `destinationCode` and
`avgDepartureDelay`.

Maybe that was confusing. When you run the following code,

```python
from_ord = get_from_ord(db_path)
print(from_ord)
```
you should get

```text
   destinationCode  avgDepartureDelay
0              JAC          26.968254
1              SBN          24.555556
2              MQT          24.419355
3              SRQ          23.250000
4              ONT          22.614828
5              JFK          20.711111
6              BOI          17.600484
7              SMF          17.458277
8              ABE          17.077313
9              COS          16.761834
10             BTV          16.755924
11             GEG          16.532067
12             BTR          16.362620
13             OAK          15.542587
14             GSP          15.537936
15             SFO          15.449473
16             ABQ          15.197390
17             ORF          15.150380
18             SJU          14.944480
19             MHT          14.940486
20             GSO          14.768665
21             TOL          14.632429
22             PHX          14.581101
23             PDX          14.497236
24             CMH          14.453364
25             LNK          14.367847
26             TVC          14.254834
27             GRR          14.194545
28             XNA          14.182957
29             ORH          14.168605
30             DLH          14.136410
31             LAS          14.089014
32             JAX          13.933687
33             SEA          13.852752
34             RIC          13.832230
35             LAX          13.756401
36             IND          13.728835
37             MDT          13.722720
38             CMI          13.686726
39             GRB          13.633869
40             SLC          13.613793
41             TPA          13.598235
42             DAY          13.595689
43             ICT          13.580737
44             DSM          13.502440
45             OMA          13.456912
46             SAN          13.384536
47             AZO          13.291948
48             CHA          13.255556
49             BMI          13.223060
50             EVV          13.162393
51             HSV          13.074033
52             MSN          13.033333
53             PVD          12.931463
54             LGA          12.888844
55             ANC          12.862963
56             CID          12.851332
57             CLE          12.710920
58             PIA          12.564820
59             PWM          12.562319
               ...                ...

[117 rows x 2 columns]
```


    def get_from_ord(database):
        '''
        Takes a string and returns a pandas dataframe.
        
        Parameters
        ----------
        database: A str. The path and/or the file name to the SQL database.
        
        Returns
        -------
        A pandas dataframe.
        '''
        
        #### your code goes here
        
        return from_ord


    from_ord = get_from_ord(db_path)
    print(from_ord)

### Function: get\_to\_ord()

The `get_to_ord()` function is similar to the `get_from_ord()` function
  but it now calculates the average **arrival** delays at each airport
  when the **destination** airport was `ORD`.

You should get

```python
to_ord = get_to_ord(db_path)
print(to_ord)
```
```text
   originCode  avgArrivalDelay
0         JAC        34.629032
1         JFK        23.977778
2         SBN        20.000000
3         LNK        14.125341
4         DEN        13.412446
5         PHX        12.892532
6         EGE        12.062069
7         OAK        11.700980
8         SLC        11.562592
9         ICT        11.351094
10        LAS        11.010512
11        RNO        11.007338
12        IAH        10.784456
13        TVC        10.425784
14        XNA        10.255620
15        CLT        10.220210
16        ATL        10.058884
17        ORF         9.996966
18        PHL         9.953525
19        DAY         9.922723
20        DTW         9.869015
21        STL         9.864484
22        ONT         9.830571
23        IAD         9.793168
24        SMF         9.626153
25        BOS         9.513652
26        RIC         9.407848
27        CMI         9.387269
28        GSO         9.329825
29        BWI         9.291394
30        DCA         9.228687
31        MEM         9.163850
32        CVG         9.130196
33        MQT         9.062044
34        BTV         8.887435
35        JAX         8.874667
36        MIA         8.677982
37        ABE         8.588571
38        RDU         8.554770
39        SJC         8.495950
40        CMH         8.449122
41        IND         8.357289
42        PDX         8.339041
43        MSP         8.101055
44        SNA         8.015702
45        FLL         7.996124
46        HPN         7.878896
47        PWM         7.792250
48        GSP         7.523444
49        EVV         7.363748
50        LGA         7.357276
51        PIT         7.306919
52        TYS         7.072993
53        FWA         7.049932
54        DFW         7.024954
55        SAN         6.936231
56        GRR         6.932374
57        BDL         6.850943
58        LAX         6.803611
59        MCI         6.714703
          ...              ...

[116 rows x 2 columns]
```


    def get_to_ord(database):
        '''
        Takes a string and returns a pandas dataframe.
        
        Parameters
        ----------
        database: A str. The path and/or the file name to the SQL database.
        
        Returns
        -------
        A pandas dataframe.
        '''
       
        #### your code goes here
    
        return to_ord


    to_ord = get_to_ord(db_path)
    print(to_ord)

### Function: pd\_to\_sql()

The `pd_to_sql()` function takes a Pandas DataFrame (`to_ord` or `from_ord`),
  a string (path to the database),
  and another string (name of the SQL table).
  You should use Pandas `to_sql()` function to create in the SQL database
  a table that matches `df`, the DataFrame that is passed as the first
  argument to the function.


    def pd_to_sql(df, database, table):
        '''
        Converts a dataframe to a table in an SQL database.
        
        Parameters
        ----------
        df: A pandas.DataFrame, e.g. to_ord or from_ord.
        database: A str. The path and/or the file name to the SQL database
                  where the table is to be created.
        table: A str. The name of SQL table where df will be stored.
        '''
    
        #### your code goes here
        
        return None

When you run the following code cells,

```python
pd_to_sql(from_ord[:10], db_path, 'topFromORD')
```
```python
%%bash
sqlite3 /data/sql/test "SELECT * FROM topFromORD"
```

you should get

```text
JAC|26.968253968254
SBN|24.5555555555556
MQT|24.4193548387097
SRQ|23.25
ONT|22.6148282097649
JFK|20.7111111111111
BOI|17.6004842615012
SMF|17.4582772543742
ABE|17.0773130544994
COS|16.7618343195266
```


    pd_to_sql(from_ord[:10], db_path, 'topFromORD')


    %%bash
    sqlite3 /data/sql/test "SELECT * FROM topFromORD"

We use the same funtion, `pd_to_sql()`, to create a table for `to_ord`.

```python
pd_to_sql(database, to_ord[:10], "topToORD")
```
```python
%%bash
sqlite3 /data/sql/test "SELECT * FROM topToORD"
```
```text
JAC|34.6290322580645
JFK|23.9777777777778
SBN|20.0
LNK|14.125340599455
DEN|13.4124455507156
PHX|12.8925318761384
EGE|12.0620689655172
OAK|11.7009803921569
SLC|11.5625915080527
ICT|11.3510941960038
```


    pd_to_sql(to_ord[:10], db_path, "topToORD")


    %%bash
    sqlite3 /data/sql/test "SELECT * FROM topToORD"

### Function: join\_the\_worst()

Now we do a simple JOIN in our database to return
  the airports that exist in both the `topFromORD` and `topToORD` tables.
  The `join_the_worst()` function takes three strings:
  the path to the SQL database,
  the name of the first table to be joined,
  and the name of the second table to be joined.
  It should return a Pandas DataFrame
  that is indexed by the `destinationCode` column in `table1` (`topFromORD`)
  and returns two columns:
  `avgDepartureDelay` from `table1` (`topFromORD`) and
  `avgArrivalDelay` from `table2` (`topToORD`).
  And two tables should of course be joined on
  the IATA codes (`destinationCode` and `originCode`).


    def join_the_worst(database, table1, table2):
        '''
        Joins two tables, table1 and table2, in database on IATA codes.
        Returns the result in a Pandas DataFrame.
        
        Parameters
        ----------
        database: A str. The path and/or the file name of the SQL database.
        table1: A str. The name of the first table to be joined.
        table2: A str. The name of the second table to be joined.
        
        Returns
        -------
        A pandas Dataframe.
        '''
    
        #### your code goes here
        
        return df

When I run,

```python
join_the_worst(db_path, 'topFromORD', 'topToORD')
print(the_worst)
```

I get

```text
                 avgDepartureDelay  avgArrivalDelay
destinationCode
JAC                      26.968254        34.629032
SBN                      24.555556        20.000000
JFK                      20.711111        23.977778

[3 rows x 2 columns]
```


    the_worst = join_the_worst(db_path, 'topFromORD', 'topToORD')
    print(the_worst)


    
