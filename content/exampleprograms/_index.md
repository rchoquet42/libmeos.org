---
title: "Example Programs"
date: 2022-07-29T14:28:47+02:00
draft: false
---

Example programs showing the usage of the MEOS library 


- [meos_hello_world.c](https://github.com/estebanzimanyi/MobilityDB/blob/bbox/meos/include/meos_hello_world.c)

  This program creates some temporal values from their WKT format and output them in MF-JSON format. The output of the program is given next.
```
--------------------
| Temporal Instant |
--------------------

WKT:
----
POINT(1 1)@2000-01-01

MF-JSON:
--------
{
   "type": "MovingGeomPoint",
   "stBoundedBy": {
     "bbox": [
       [
         1,
         1
       ],
       [
         1,
         1
       ]
     ],
     "period": {
       "begin": "2000-01-01T00:00:00+01",
       "end": "2000-01-01T00:00:00+01"
     }
   },
   "coordinates": [
     1,
     1
   ],
   "datetimes": "2000-01-01T00:00:00+01",
   "interpolations": [
     "Discrete"
   ]
 }

------------------------
| Temporal Instant Set |
------------------------

WKT:
----
{POINT(1 1)@2000-01-01, POINT(2 2)@2000-01-02}

MF-JSON:
--------
{
   "type": "MovingGeomPoint",
   "stBoundedBy": {
     "bbox": [
       [
         1,
         1
       ],
       [
         2,
         2
       ]
     ],
     "period": {
       "begin": "2000-01-01T00:00:00+01",
       "end": "2000-01-02T00:00:00+01"
     }
   },
   "coordinates": [
     [
       1,
       1
     ],
     [
       2,
       2
     ]
   ],
   "datetimes": [
     "2000-01-01T00:00:00+01",
     "2000-01-02T00:00:00+01"
   ],
   "interpolations": [
     "Discrete"
   ]
 }

---------------------
| Temporal Sequence |
---------------------

WKT:
----
[POINT(1 1)@2000-01-01, POINT(2 2)@2000-01-02]

MF-JSON:
--------
{
   "type": "MovingGeomPoint",
   "stBoundedBy": {
     "bbox": [
       [
         1,
         1
       ],
       [
         2,
         2
       ]
     ],
     "period": {
       "begin": "2000-01-01T00:00:00+01",
       "end": "2000-01-02T00:00:00+01"
     }
   },
   "coordinates": [
     [
       1,
       1
     ],
     [
       2,
       2
     ]
   ],
   "datetimes": [
     "2000-01-01T00:00:00+01",
     "2000-01-02T00:00:00+01"
   ],
   "lower_inc": true,
   "upper_inc": true,
   "interpolations": [
     "Linear"
   ]
 }

-------------------------
| Temporal Sequence Set |
-------------------------

WKT:
----
{[POINT(1 1)@2000-01-01, POINT(2 2)@2000-01-02],[POINT(3 3)@2000-01-03, POINT(3 3)@2000-01-04]}

MF-JSON:
--------
{
   "type": "MovingGeomPoint",
   "stBoundedBy": {
     "bbox": [
       [
         1,
         1
       ],
       [
         3,
         3
       ]
     ],
     "period": {
       "begin": "2000-01-01T00:00:00+01",
       "end": "2000-01-04T00:00:00+01"
     }
   },
   "sequences": [
     {
       "coordinates": [
         [
           1,
           1
         ],
         [
           2,
           2
         ]
       ],
       "datetimes": [
         "2000-01-01T00:00:00+01",
         "2000-01-02T00:00:00+01"
       ],
       "lower_inc": true,
       "upper_inc": true
     },
     {
       "coordinates": [
         [
           3,
           3
         ],
         [
           3,
           3
         ]
       ],
       "datetimes": [
         "2000-01-03T00:00:00+01",
         "2000-01-04T00:00:00+01"
       ],
       "lower_inc": true,
       "upper_inc": true
     }
   ],
   "interpolations": [
     "Linear"
   ]
 }
```

- [meos_read_ais.c](https://github.com/estebanzimanyi/MobilityDB/blob/bbox/meos/include/meos_read_ais.c)

  This program reads AIS data from a CSV file and outputs a few of these records converted into temporal values. The output of the program is given next.
```
MMSI: 566948000, Location: POINT(4.62 55.57)@2021-01-08 01:10:46+01 SOG : 0.5@2021-01-08 01:10:46+01
MMSI: 566948000, Location: POINT(4.62 55.57)@2021-01-08 01:21:25+01 SOG : 0.1@2021-01-08 01:21:25+01
MMSI: 219001559, Location: POINT(9.98 57.59)@2021-01-08 01:33:48+01 SOG : 0.1@2021-01-08 01:33:48+01
...
MMSI: 219001559, Location: POINT(9.98 57.59)@2021-01-09 00:43:35+01 SOG : 0@2021-01-09 00:43:35+01
MMSI: 566948000, Location: POINT(4.49 55.64)@2021-01-09 00:52:34+01 SOG : 0.3@2021-01-09 00:52:34+01

156837 records read.
0 incomplete records ignored.
```

- [meos_process_ais.c](https://github.com/estebanzimanyi/MobilityDB/blob/bbox/meos/include/meos_process_ais.c)

  This program reads AIS data from a CSV file, constructs trips from these records, and outputs for each trip the MMSI, the number of instants, and the distance travelled. The output of the program is given next.
```
156837 records read.
0 incomplete records ignored.
5 trips read.
MMSI: 265513270, Number of input instants: 21799, Number of instants: 6, Distance travelled 11.695976
MMSI: 219027804, Number of input instants: 38326, Number of instants: 3285, Distance travelled 64865.704162
MMSI: 566948000, Number of input instants: 26619, Number of instants: 2969, Distance travelled 14932.332003
MMSI: 219001559, Number of input instants: 48323, Number of instants: 1597, Distance travelled 5202.381849
MMSI: 257136000, Number of input instants: 21770, Number of instants: 14811, Distance travelled 647535.585339
```

- [meos_store_ais.c](https://github.com/estebanzimanyi/MobilityDB/blob/bbox/meos/include/meos_store_ais.c)

  This program reads AIS data from a CSV file, converts them into temporal values, and stores them in MobilityDB. The program uses the [libpq](https://www.postgresql.org/docs/current/libpq.html) library to connect to PostgreSQL. The output of the program is given next.
```
Creating the table in the database
NOTICE:  table "meos_demo" does not exist, skipping
Start processing the file
156837 records read.
0 incomplete records ignored.
Query 'SELECT COUNT(*) FROM public.MEOS_demo' returned 156837
```

- [meos_read_berlinmod.c](https://github.com/estebanzimanyi/MobilityDB/blob/bbox/meos/include/meos_read_berlinmod.c)

  This program reads from a CSV file synthetic trip data in Brussels generated by the [MobilityDB-BerlinMOD](https://github.com/MobilityDB/MobilityDB-BerlinMOD) generator and generate statics about the Brussels communes (or municipalities) traversed by the trips. The output of the program is given next.
```
19 commune records read.
Brussels region record read.
64 trip records read.
                -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
                | Commmunes
    -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
Veh | Distance |     2       4       6       7       8       9      10      11      12      14      15      16      17      18      19   | Outside | Inside
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
  1 |  393.448 |  32.536   1.289   0.000   0.000   5.702   0.000   0.000   0.000   0.000   0.000   0.000  29.265  17.872   0.000   4.634 | 302.149 |  91.299
  2 |  133.156 |   0.000   0.000   0.000   0.000   0.000   0.000   0.000   0.000   0.000   0.000  77.281   4.583  28.809   0.000   0.000 |  22.482 | 110.673
  3 |  297.831 |   0.000   5.194  56.414   0.000   0.000   0.000   0.000   0.000  17.132   9.017   0.000  48.214   0.000  51.660   1.951 | 108.249 | 189.583
  4 |   82.236 |   0.000   0.000   0.000  16.609   0.000  29.140  10.832  11.738   0.000   0.000   0.000  13.917   0.000   0.000   0.000 |   0.000 |  82.236
  5 |  172.900 |   0.000   0.000   0.000   0.000   0.000   0.000   0.000   4.170   0.000   0.000   0.000 112.290   0.000   0.000   0.000 |  56.439 | 116.461
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
    | 1079.571 |  32.536   6.484  56.414  16.609   5.702  29.140  10.832  15.908  17.132   9.017  77.281 208.270  46.682  51.660   6.585 | 489.320 | 590.251
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
```




