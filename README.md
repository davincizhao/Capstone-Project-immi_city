# Capstone-Project-immi_city

The project follows the follow steps:

Step 1: Scope the Project and Gather Data
Step 2: Explore and Assess the Data
Step 3: Define the Data Model
Step 4: Run ETL to Model the Data
Step 5: Complete Project Write Up

### Step 1: Scope the Project and Gather Data
I try to find out in 2006, which cities have TOP 10 population move in ? So I need combine immigration date and iata_code which have location relative to iata code. I choose immigration data from udacity data set sample and extrat IATA code from website:https://www.nationsonline.org/oneworld/IATA_Codes/airport_code_list.htm and get the city name from IATA code that in the immigration. But I found above "airport_code_csv" is not include metranpolitan code like "NYC". and i search iata_code in wiki website:"https://en.wikipedia.org/wiki/List_of_airports_by_IATA_code:_" add get all the new iata code and combine all of them to one table: this table have three columns: iata_id,airport,location
add get all the new iata code and combine all of them to one table:
this table have three columns: iata_id,airport,location
### 'iata_id' is 3-alpha letter , string type.

### 'airport' is airport name, string type.

### 'location' is country name or city name, string type.
using python script :"get_iata_code_from_wiki.py" that written by myself.

 ### I extracted i94yr: as year,i94mon: as month,i94cit: as from_city,i94port: as to_city, admnum: as total_immi_num.


### Step 2: Explore and Assess the Data
#### Explore the Data 
Identify data quality issues, like missing values, duplicate data, etc.

#### Cleaning Steps
Document steps necessary to clean the data

### Step 3: Define the Data Model
#### 3.1 Conceptual Data Model
Map out the conceptual data model and explain why you chose that model

#### 3.2 Mapping Out Data Pipelines
List the steps necessary to pipeline the data into the chosen data model

#### Fact Table
1.immigration population in world.
              _c0 AS id,
              i94yr AS year, 
              i94mon AS month,
              i94cit AS from_city,
              i94port AS iata_id,
              CAST(admnum AS bigint) AS total_immi_num 
#### Dimension Tables
2.location
iata_id,airport,location

### Step 4: Run Pipelines to Model the Data 
#### 4.1 Create the data model
Build the data pipelines to create the data model.

#### 4.2 Data Quality Checks
Explain the data quality checks you'll perform to ensure the pipeline ran as expected. These could include:
 * Integrity constraints on the relational database (e.g., unique key, data type, etc.)
 * Unit tests for the scripts to ensure they are doing the right thing
 * Source/Count checks to ensure completeness
 
### Perform quality checks here
### As you can see above, sorted_city top 10 citits is different from the answer top 10 cities

### from immigration data : top 10 is 
+-------+--------------+
|to_city|         total|
+-------+--------------+
|    NYC|10846125450235|
|    MIA| 8922547681245|
|    LOS| 6965071195933|
|    SFR| 3591412487120|
|    CHI| 3020403711605|
|    ORL| 2736974886745|
|    HOU| 2728330237345|
|    ATL| 2669580186622|
|    NEW| 2589234201821|
|    HHW| 2461775306030|
+-------+--------------+

### but after combine the two tables: fact table and demension. the result is :
+-------+--------------------+--------------------+--------------+
|to_city|            location|             airport|         total|
+-------+--------------------+--------------------+--------------+
|    NYC|New York City, Ne...|  metropolitan area2|10846125450235|
|    MIA|Miami, Florida, U...|Miami Internation...| 8922547681245|
|    LOS|      Lagos, Nigeria|Murtala Muhammed ...| 6965071195933|
|    CHI|Chicago, Illinois...|  metropolitan area2| 3020403711605|
|    ORL|Orlando, Florida,...|Orlando Executive...| 2736974886745|
|    HOU|Houston, Texas, U...|William P. Hobby ...| 2728330237345|
|    ATL|Atlanta, Georgia,...|Hartsfield–Jackso...| 2669580186622|
|    NEW|New Orleans, Loui...|   Lakefront Airport| 2589234201821|
|    WAS|Washington, D.C.,...|  metropolitan area1| 1765844128444|
|    DAL|Dallas, Texas, Un...|   Dallas Love Field| 1317476983970|
+-------+--------------------+--------------------+--------------+

the result is wrong ,because there is miss city "SFR" and "HHW". So more iata code data need to be collected, and then Append to "./output/location.parquet".


### As you can see above, the 10th city is wrong ,it must be "HHW", not "WAS" Washington, D.C.
But I haven't found the result when I searched "HHW" in iata code in wiki websit,
So I infer the "HHW" is wrong data ,  maybe it is "KHHW "in ICAO CODE. It represents "KHHW Stan Stamper Municipal Airport (FAA: HHW) Hugo, Oklahoma, United States"


#### 4.3 Data dictionary 
Create a data dictionary for your data model. For each field, provide a brief description of what the data is and where it came from. You can include the data dictionary in the notebook or in a separate file.

## Fact table: immigration data set, from udacity data set sample.

### I extracted 
### i94yr: as year, can be cast as datetime type
### i94mon: as month, can be cast as datetime type
### i94cit: as from_city, "3 digit Numeric code ISO 3166 codes for each country." 
### i94port: as to_city, "alpha-3 code character,IATA CODE" string type
### admnum: as total_immi_num. bigint type

## Demenstion table: using python script :"get_iata_code_from_wiki.py" extract from iata code wiki website.

### 'iata_id' is 3-alpha letter , string type.

### 'airport' is airport name, string type.

### 'location' is country name or city name, string type.

#### Step 5: Complete Project Write Up
* Clearly state the rationale for the choice of tools and technologies for the project.
* Propose how often the data should be updated and why.
* Write a description of how you would approach the problem differently under the following scenarios:
 * The data was increased by 100x.
 * The data populates a dashboard that must be updated on a daily basis by 7am every day.
 * The database needed to be accessed by 100+ people.
 
 1. tool sets: pandas ,pyspark,pyspark.sql, bs4, BeautifulSoup,requests, csv,time
2. I think the data should be updated every 6 months, because more people immigration frequently.
3. if the data was increased by 100x. use AWS S3 as stage store. and run the spark in the AWS cloud cluster to accelerate the loading data speed and caculation.
4. if the data populates a dashboard that must be updated on a daily basis by 7am every day, it is better to use airflow to make populates automative.
5. if The database needed to be accessed by 100+ people. it is better to use Cassandra database.
