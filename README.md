Hackathon 2019
==============

Welcome to hackathonCLT 2019!

Download the kickoff deck for more information on the business problem, the hackathonCLT agenda, the data, and more! Find it in the data folder: [HackathonCLT 2018 kickoff deck](https://github.com/tresata/hackathonclt2018/blob/master/data/hackathonCLT%20MMXVIII%20-%20KICK%20OFF%20.pdf)

Table of Content: 

* [IMPORTANT INFORMATION](#important-information)
* [SLACK](#slack)
* [Getting Started](#getting-started)
* [Machines](#machines)
* [HDFS](#hdfs)
* [Hive](#hive)
* [Spark](#spark)
* [pySpark](#pyspark)
* [Anaconda Python](#anaconda-python)
* [Tresata Software](#tresata-software)
* [YARN Resource Manager](#yarn-resource-manager)
* [PuTTY](#putty)
* [SAMBA](#samba)
* [FTP](#ftp)
* [Elasticsearch](#elasticsearch)
* [Data](#data)
* [Data Links](#data-links)

## IMPORTANT INFORMATION

* Please make sure you spread out on all the boxes. There are 4 servers available for login, make sure you spreadout and not all login to 1 box. 
* Use tmux once you login into the server. If not, your session could get terminated and you can lose your work. Just type "tmux" once you login. To re-attach a tmux session "tmux attach". https://tmux.github.io/

## SLACK

Please make sure you connect with your fellow hackers on [SLACK](https://hackathonclt2019.slack.com/). You can also ask any questions here on any of the available channels. 

## Getting Started

You can obtain a username and login information from command central (in the kids university downstairs).

ssh into a server where you can access the data.

    $ ssh <username>@hack02.nscom.com OR
    $ ssh <username>@hack03.nscom.com OR
    $ ssh <username>@hack04.nscom.com OR
    $ ssh <username>@hack05.nscom.com
    

and enter the password you were given.

We made Hive, Spark, pySpark, R and Anaconda Python command-line interfaces available.

## Machines

We have a Hadoop cluster with one master and four workers. The workers have 32 cores, 7 X 1TB data drives, and 128GB of RAM each. You will have ssh access to the workers.

Please spread yourselves out across the machines.

The /home directory on every machine is limited to 170G, and shared between everyone logged in to the server. If you need disk space to work please use one of the following directories on a 1TB data disk:

    /data/0/work
    /data/1/work
    /data/2/work
    /data/3/work
    /data/4/work
    /data/5/work
    /data/6/work

Do not work inside these directories directly, but instead create a subdirectory. For example:
    mkdir /data/3/work/hacker123
    # in case you want privacy
    chmod og-rwx /data/3/work/hacker123
    cd /data/3/work/hacker123

## HDFS
To access your HDFS location, you need to use hadoop fs commands (some reference: http://www.folkstalk.com/2013/09/hadoop-fs-shell-command-example-tutorial.html). For example, to take a look at your home directory on HDFS, use

    $ hadoop fs -ls

or

    $ hadoop fs -ls /user/username

**HDFS**

You can find the pre-downloaded data on HDFS in the /data/hackathon folder:

    /data/hackathon/housing
    /data/hackathon/education
    /data/hackathon/economics
    /data/hackathon/health
    /data/hackathon/transporation
    /data/hackathon/demographics
    /data/hackathon/crime

## Hive

Give Hive a whirl and run a sample query:

    $ hive

Try pasting the following query into the hive command-line interface:

    hive> show tables;
    OK
  
    economics_county_cb1500a11_business_patterns
    economics_qol_economy
    economics_zip_by_business_class_cb1500cz21_business_patterns
    economics_zip_cb1500cz11_business_patterns
    education_edge_geocode_postsecsch_nc_1516
    education_edge_geocode_publicsch_nc_1516
    education_qol_education
    health_coulwood_a_primary_01_01_2017_02_28_2018
    health_coulwood_a_secondary_01_01_2017_02_28_2018
    health_coulwood_b_primary_01_01_2017_02_28_2018
    health_coulwood_b_secondary_01_01_2017_02_28_2018
    health_matthews_a_primary_01_01_2017_02_28_2018
    health_matthews_a_secondary_01_01_2017_02_28_2018
    health_matthews_b_primary_01_01_2017_02_28_2018
    health_matthews_b_secondary_01_01_2017_02_28_2018
    health_mceniry_a_primary_01_01_2017_02_28_2018
    health_mceniry_a_secondary_01_01_2017_02_28_2018
    health_mceniry_b_primary_01_01_2017_02_28_2018
    health_mceniry_b_secondary_01_01_2017_02_28_2018
    health_mosquito_data_dictionary
    health_myers_park_a_primary_01_01_2017_02_28_2018
    health_myers_park_a_secondary_01_01_2017_02_28_2018
    health_myers_park_b_primary_01_01_2017_02_28_2018
    health_myers_park_b_secondary_01_01_2017_02_28_2018
    housing_qol_housing
    
    hive> set hive.cli.print.header=true;
    hive> select * from housting_qol_housing limit 10;

This will return all the fields for the first ten items in the active_match_details_new table.

If you'd like to create a file from the command line, you can use a create table command:

    hive> create table test row format delimited fields terminated by '|' stored as textfile as select * from housing_qol_housing limit 10;
    
We are also running hive-server on hack01.nscom.com. You can connect to them with JDBC/ODBC. For example to connect to hack01 with JDBC you would use this connect string:

    jdbc:hive2://hack01.nscom.com:10000
    
If you need to provide a username and password, use the username we provided for SSH login and leave the password blank. 

## Spark

**Spark-shell** can be found at /usr/local/lib/spark/bin

Now give the Spark-shell a test:

    $ /usr/local/lib/spark/bin/spark-shell --executor-cores 1 --executor-memory 1G


Read in the data and run a simple query that calcuates the unique count of ChildZip:

    val df = spark.sqlContext.read.parquet("/data/hackathon/education/mecklenburg-quality-of-life-survey/csv/qol-education.csv")
    df.groupBy("NPA").count().collect()


Note that for your "production" run on the dataset you might want to increase resources used on the cluster:

   --executor-memory 4G --executor-cores 4

Keep in mind that a spark-shell takes up these resources on the cluster even when you do not use them so please do not keep a spark-shell with "production" resources open unused.  


## pySpark

**pySpark** can be found at /usr/local/lib/spark/bin


You can also do the same query using a python version of the Spark shell.

    $ /usr/local/lib/spark/bin/pyspark --executor-cores 1 --executor-memory 1G

Read in the data and run a simple query that calcuates the unique count of ChildZip:

    df = sqlContext.read.parquet("data/hackathon/education/mecklenburg-quality-of-life-survey/csv/qol-education.csv")
    df.groupBy("NPA").count().collect()

Note that for your "production" run on the dataset you might want to increase resources used on the cluster:

    --executor-memory 4G --executor-cores 4

Keep in mind that a pyspark takes up these resources on the cluster even when you do not use them so please do not keep a pyspark shell (interpreter) with "production" resources open unused.  


## Anaconda Python
Anaconda is a completely free Python distribution from [Continuum Analytics](https://www.continuum.io). It includes more than 400 of the most popular Python packages for science, math, engineering, and data analysis. See [the packages included with Anaconda](https://docs.anaconda.com/anaconda/packages/pkg-docs/).

**Anaconda** can be found here:

    /usr/local/lib/anaconda

Getting familiar with conda: https://conda.io/projects/conda/en/latest/commands.html

An example of how to start anaconda python:
```
ssh hacker001@hack02.nscom.com
hacker001@hack02.nscom.com's password:
Last login: Fri Mar 23 21:42:20 2018 from 107.14.49.67
[hacker001@hack02 ~]$ source /usr/local/lib/anaconda/bin/activate
(base) [hacker001@hack02 ~]$ python
Python 3.6.4 |Anaconda, Inc.| (default, Jan 16 2018, 18:10:19)
[GCC 7.2.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```


## Tresata Software

#### TREK
Data Inventory Engine built specifically to catalog, profile and report data ontology, quality and format attributes for all data in Hadoop. TREK rapidly profiles and inventories “as-is” data stored in Hadoop across all rows and columns to create an informed view of all valuable enterprise data feeds stored in a single Hadoop cluster.

TREK can be accessed via http://hack01.nscom.com:5603

For login, it is user "tresata" and password "admin". After loggin in click on the menu icon next to the name "tresata" on the top left corner and select TREK.
Once you select a dataset in TREK click on partitions and select a partition (typically there is only one). You should now see a summary of all the columns in the dataset. Click on a column to get the statistics for that one column.

## YARN Resource Manager

This is where you can track jobs that run on the hadoop cluster:

http://hack01.nscom.com:8088/

## PuTTY
Here is the link to download puTTY for remote access to the data files. This is useful if you have a Windows computer. The download link is:

https://www.putty.org/

## Samba 

You can also use Samba to connect to the servers and download the data locally. The samba share is:

    smb://hack01.nscom.com/data

on windows this is:

    \\hack01.nscom.com\data

Instructions for how to use Samba for apple devices can be found [here](https://support.apple.com/en-us/HT204445). Help for connecting to a Samba share on a windows device can be found [here](https://www.techrepublic.com/article/how-to-connect-to-linux-samba-shares-from-windows-10/). 

## FTP

The data is also accessible via FTP on hack01.nscom.com. In a web browser:
    ftp://hack01.nscom.com/hackathon

## Elasticsearch

An Elasticsearch 5 cluster is available at port 9200 on all servers (hack01.nscom, hack02.nscom, hack03.nscom, hack04.nscom.com, hack05.nscom.com). There is no security enabled so you can create indices if you need to, but please **do not delete or modify other peoples indices**. 

## Data

**Data Dictionary** : The links to the public source data & the corresponding data dictionaries can be found in the next section. Additionally, descriptions of the pre-downloaded files (already stored locally and on HDFS) can be found [here](https://github.com/tresata/hackathonclt2018/tree/master/datadictionary) For any further questions please use the DATA slack channel.

**LOCAL**
You can find the data on local (all machines) in the /srv/data directory. We have provided the csv and parquet versions of the pre-downloaded files. 

```
├── health_outcomes
│   ├── 500-cities
│   │   ├── csv
│   │   └── parq
│   └── aids-vu
│       ├── csv
│       └── parq
├── medlink_partners_services
│   ├── camino_clinic
│   │   ├── csv
│   │   └── parq
│   ├── charlotte_center_for_legacy_advocacy
│   │   ├── csv
│   │   └── parq
│   ├── charlotte_community_health_clinic
│   │   ├── csv
│   │   └── parq
│   └── nc_medassist
│       ├── csv
│       └── parq
├── social_determinants_of_health
│   ├── census
│   │   ├── csv
│   │   └── parq
│   ├── charlotte_housing_authority
│   │   ├── csv
│   │   ├── parq
│   │   └── README.txt
│   ├── consumer_financial_protection_bureau
│   │   ├── consumer-complaints
│   │   │   ├── csv
│   │   │   └── parq
│   │   ├── financial-well-being-survey
│   │   │   ├── csv
│   │   ├── home-mortgage-disclosure-act
│   │   │   ├── csv
│   │   │   └── parq
│   │   └── national-mortgage-rates
│   │       ├── csv
│   │       └── parq
│   ├── epa
│   │   └── clean-air-carolinas
│   │       ├── csv
│   │       └── parq
│   ├── geo-mappings
│   │   ├── csv
│   │   └── parq
│   ├── housing_urban_development
│   │   ├── affh
│   │   │   ├── mecklenburg
│   │   │   │   ├── csv
│   │   │   │   └── parq
│   │   │   ├── national
│   │   │   │   ├── csv
│   │   │   │   └── parq
│   │   │   └── raw-affh-data
│   │   │       └── csv
│   │   └── fair-market-value
│   │       └── parq
│   ├── mecklenburg_public_services_geojson
│   │   └── demographics
│   │       └── mecklenburg-public-services-geojson
│   ├── mecklenburg_quality_of_life
│   │   ├── crime
│   │   │   ├── csv
│   │   │   ├── data_dictionary
│   │   │   ├── npa_mapping_geojson
│   │   │   └── parq
│   │   ├── demographics
│   │   │   ├── csv
│   │   │   ├── data_dictionary
│   │   │   ├── npa_mapping_geojson
│   │   │   └── parq
│   │   ├── economics
│   │   │   ├── csv
│   │   │   ├── data_dictionary
│   │   │   ├── npa_mapping_geojson
│   │   │   └── parq
│   │   ├── education
│   │   │   ├── csv
│   │   │   ├── data_dictionary
│   │   │   ├── npa_mapping_geojson
│   │   │   └── parq
│   │   ├── health
│   │   │   ├── csv
│   │   │   ├── data_dictionary
│   │   │   ├── npa_mapping_geojson
│   │   │   └── parq
│   │   ├── housing
│   │   │   ├── csv
│   │   │   ├── data_dictionary
│   │   │   ├── npa_mapping_geojson
│   │   │   └── parq
│   │   └── transportation
│   │       ├── csv
│   │       ├── data_dictionary
│   │       └── parq
│   ├── nc_board_of_education
│   │   ├── graduation-counts-including-summer-school
│   │   │   ├── csv
│   │   │   └── parq
│   │   ├── nc-student-counts-grade-race-sex
│   │   │   ├── csv
│   │   │   └── parq
│   │   ├── principles-report-nc
│   │   │   ├── csv
│   │   │   └── parq
│   │   └── school-to-geocode-mapping
│   │       ├── csv
│   │       └── parq
│   └── zillow
│       ├── csv
│       └── parq
```

## Helpful Data Links - Social Determinants of Health

**Census**

[Census](https://factfinder.census.gov/faces/nav/jsf/pages/download_center.xhtml)

**Environmental Protection Agency (EPA)**

[Environmental Protection Agency](https://catalog.data.gov/dataset/walkability-index)

**Health**

[500 Cities Healthcare Data](https://catalog.data.gov/dataset/500-cities-census-tract-level-data-gis-friendly-format)

[Healthcare Cost & Utilization Project](https://www.ahrq.gov/research/data/hcup/index.html)

[Medicare Provider Utlization & Payment Data](https://www.cms.gov/Research-Statistics-Data-and-Systems/Statistics-Trends-and-Reports/Medicare-Provider-Charge-Data/Inpatient.html)

**Housing and Urban Development**

[Housing and Urban Development](https://www.hudexchange.info/resource/4868/affh-raw-data/)

[Housing Choice Vouchers](https://egis-hud.opendata.arcgis.com/datasets/8d45c34f7f64433586ef6a448d00ca12_0?geometry=97.375%2C12.813%2C71.887%2C54.725)

[National Housing Preservation Database](http://preservationdatabase.org/)

**Mecklenburg Quality of Life**

[Mecklenburg Quality of Life](https://mcmap.org/qol/)

**NC Board of Education**

[North Carolina School Board](http://www.ncpublicschools.org/fbs/accounting/data/)

[Survey on US Public Schools](https://catalog.data.gov/dataset/2010-school-survey-on-crime-and-safety)

**Zillow**

[Zillow](https://www.zillow.com/research/data/)
