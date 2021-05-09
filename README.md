### PostgreSQL for IoT

PostgreSQL can be used to store data in IoT. I have followed the following methods to convert the time (UNIT time to dd-mm-yy) format, store the data in postgresql and then to print the data in the table.

1. As the first step, I have installed TimescaleDB database in my computer.
2. Then, I have created a database with name '\IoTdata'.
3. After that, I have created a hypertable with a name '\powerdata' within '\IoTdata'.
4. Then I have converted UNIX time format to proper date-time format using python and saved it in another csv file name 'Time.csv'.
5. After that I have inserted the new converted csv file in the powerdata and printed all the 4000 rows.
6. In the last step, I have printed first and last five rows of the powerdata.

Microsoft Windows [Version 10.0.19042.928] </br>
(c) Microsoft Corporation. All rights reserved.</br>
</br>
C:\Users\supri>psql -U postgres -h localhost</br>
Password for user postgres:</br>
psql (11.11)</br>
WARNING: Console code page (437) differs from Windows code page (1252)</br>
         8-bit characters might not work correctly. See psql reference</br>
         page "Notes for Windows users" for details.</br>
Type "help" for help.</br>
</br>
</br>
postgres=# create database iotdata;</br>
CREATE DATABASE</br>
</br>
postgres=# \c iotdata</br>
You are now connected to database "iotdata" as user "postgres".</br>
</br>
iotdata=# create extension if not exists timescaledb;</br>
WARNING:</br>
WELCOME TO </br>
</br>
```
 _____ _                               _     ____________
|_   _(_)                             | |    |  _  \ ___ \
  | |  _ _ __ ___   ___  ___  ___ __ _| | ___| | | | |_/ /
  | | | |  _ ` _ \ / _ \/ __|/ __/ _` | |/ _ \ | | | ___ \
  | | | | | | | | |  __/\__ \ (_| (_| | |  __/ |/ /| |_/ /
  |_| |_|_| |_| |_|\___||___/\___\__,_|_|\___|___/ \____/
```
               Running version 2.1.1</br>
For more information on TimescaleDB, please visit the following links:</br>
</br>
 1. Getting started: https://docs.timescale.com/getting-started</br>
 2. API reference documentation: https://docs.timescale.com/api</br>
 3. How TimescaleDB is designed: https://docs.timescale.com/introduction/architecture</br>
</br>
Note: TimescaleDB collects anonymous reports to better understand and assist our users.</br>
For more information and how to disable, please see our docs https://docs.timescaledb.com/using-timescaledb/telemetry.</br>
</br>
CREATE EXTENSION</br>
</br>
iotdata=# create table powerdata(time TIMESTAMPTZ NOT NULL, power DOUBLE PRECISION NULL, current TEXT NULL, voltage DOUBLE PRECISION NULL, frequency DOUBLE PRECISION NULL, power_factor DOUBLE PRECISION NULL);</br>
CREATE TABLE</br>
</br>
iotdata=# select create_hypertable('powerdata','time');</br>
   create_hypertable
------------------------
 (1,public,powerdata,t)
(1 row)

</br>
iotdata=# \copy powerdata from 'C:\Users\supri\Downloads\IoT_Data.csv' DELIMITER ',' csv header;</br>
COPY 4000</br>
</br>
iotdata=#  select * from powerdata fetch first 5 row only;</br>

          time                   |    power    | current |  voltage  | frequency | power_factor
          -----------------------+-------------+---------+-----------+-----------+--------------
          2013-09-08 18:30:00-05 | 28368.60645 | NA      | 240.99646 |  50.32322 |      0.93397
          2013-09-08 18:31:00-05 | 28411.98828 | NA      | 241.19509 |  50.35931 |      0.93536
          2013-09-08 18:32:00-05 | 28752.53809 | NA      | 241.38574 |  50.39784 |      0.93675
          2013-09-08 18:33:00-05 | 29031.44629 | NA      | 241.72626 |  50.44616 |      0.93685
          2013-09-08 18:34:00-05 | 28487.45801 | NA      | 241.94244 |  50.44468 |      0.93448
(5 rows)

</br>
iotdata=# </br>
</br>
iotdata=# select * from powerdata order by time desc limit 5;</br>

          time                   |    power    | current |  voltage  | frequency | power_factor
          -----------------------+-------------+---------+-----------+-----------+--------------
          2013-12-08 13:09:00-06 | 48596.77344 | NA      | 239.12647 |  50.02576 |      0.95946
          2013-12-08 13:08:00-06 | 46434.46875 | NA      | 239.44028 |  50.06322 |      0.95489
          2013-12-08 13:07:00-06 | 48676.02344 | NA      | 239.73373 |  50.14679 |      0.95049
          2013-12-08 13:06:00-06 |  46271.4707 | NA      | 240.04881 |  50.22065 |      0.95461
          2013-12-08 13:05:00-06 | 46893.51562 | NA      | 240.30111 |  50.28455 |      0.95621
(5 rows)