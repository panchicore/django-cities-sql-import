# django-cities-sql-import

It was originally imported from `django-cities`, due to the quantity limitation on the high ammount of cities, command neved completed the entire process succesfully. I quickly split the task by splitting the complete geonames into 500k cities files and ran the importer for each one, managing to collect all the cities without problems.

How was the files created?

The geonames database was imported (importing countries, regions, subregions and cities; postal codes and alternative names in the next opportunity)

```
python manage.py cities --import=country,region,subregion,city
```

Created the SQL dump:

```
pg_dump -a -t 'cities_*' dbname > cities_P2.sql
```


Splitted the file so it complies with github limits

```
split -l 2400000 cities_P2.sql
#resulting on xaa and xab
```

gziped

```
gzip -9 xa*
```

So all you need from here, is to clone this repo, unzip the files,

```
gzip -d xa*
```

merge

```
cat xaa xab > cities.sql
```

run django-cities migration and import the dump

```
psql myprojectdb < cities.sql 
```

and there are the 4M cities in the database in just a couple of minutes, pretty quickly to start hacking with the cities.


output should looks like this one:

```
SET
SET
SET
SET
SET
 set_config 
------------
 
(1 row)

SET
SET
SET
COPY 0
ERROR:  duplicate key value violates unique constraint "cities_continent_pkey"
DETAIL:  Key (id)=(6255146) already exists.
CONTEXT:  COPY cities_continent, line 1
COPY 250
COPY 3970
COPY 0
COPY 4693296
COPY 0
COPY 0
COPY 0
COPY 646
COPY 0
COPY 0
COPY 0
COPY 0
COPY 0
COPY 0
 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
    646
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

 setval 
--------
      1
(1 row)

```

