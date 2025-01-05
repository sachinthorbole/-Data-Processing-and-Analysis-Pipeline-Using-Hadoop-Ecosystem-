# -Data-Processing-and-Analysis-Pipeline-Using-Hadoop-Ecosystem-
Ingested data to HDFS.  • Filtered and transformed data using Pig.  • Stored results in HDFS. Analyzed and sorted data in Hive.  • Exported results to HDFS.
 1.Add the dataset to HDFS:
 • Make a new directory on HDFS:
 hdfs dfs-mkdir /input
 • Send the data from the local file system to HDFS:
 hadoop fs-put /path/to/Fitness_trackers.csv
 /Home/hadoop/data/Fitness_trackers.csv
 2. Load the data in Pig and filter for Device Type
 SmartWatch:
 • Start Pig:
 Pig
 • Load the data into Pig:
 data = LOAD '/Home/hadoop/data/Fitness_trackers.csv' USING PigStorage(',')
 AS(BrandName: chararray, DeviceType: chararray, ModelName: chararray,
 Color: chararray, SellingPrice: float, OriginalPrice: float, Display: chararray,
 Rating: float, StrapMaterial: chararray, BatteryLife: chararray);
 • Filter command to get the device type SmartWatch:
 smartwatch_data = FILTER data BY DeviceType == 'SmartWatch';
 3.Store the result in HDFS:.
 STORE smartwatch_data INTO '/output/smartwatch_data' USING
 PigStorage(',');
 4. Load the output of Pig in Hive. Display the data sorted by
 Rating:
hive
 • Create a database:
 CREATEDATABASEmydatabase;
 • Select the database:
 USEmydatabase;
 • Create a table with the correct schema:
 CREATETABLEmytable (
 `Brand Name` STRING,
 `Device Type` STRING,
 `Model Name` STRING,
 `Color` STRING,
 `Selling Price` FLOAT,
 `Original Price` FLOAT,
 `Display` STRING,
 `Rating` INT,
 `Strap Material` STRING,
 `Average Battery Life` INT
 );
  #Load the output of Pig that you got after filtering into that table:
 LOADDATAINPATH'/output/smartwatch_data' INTO TABLE mytable;
 • Write a select query to sort the data by Rating (order by clause):
 SELECT *FROMmytable ORDERBYRating;
 5.Store the results in HDFS:
 • Create an external table:
  CREATEEXTERNALTABLEmyexternaltable (ID INT, Name
 STRING, Rating INT, Price FLOAT, DeviceType STRING)
 ROWFORMATDELIMITEDFIELDSTERMINATEDBY','
 LOCATION '/output/sorted_data';
• Insert the result of the select query to sort the data by rating into that
 external table:
 INSERT INTO TABLE myexternaltable
 SELECT *FROMmytable ORDERBYRating
