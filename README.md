# Movies_ETL
Data Pipeline Process: Extract, Transform, and Load. Extract from JSON and CSV files, transform data in python and pandas using jupyter notebooks, then load cleaned data into PostgreSQL database.

## Overview
### ETL Purpose
The purpose of this repository is to perform an ETL from start to finish on data originating from JSON wikipedia and CSV formats. We will extract the data (pull the data from the sources), Transform the data (use regular expressions to parse data and to transform text into numbers, clean the data, eliminate unnecessary columns, merge dataframes, and load the data into PostgreSQL. ETL is the workhorse for moving information between databases to improve performance and comprehension. ETL is a core concept in data engineering in that it helps organizations have consistent data with maintained integrity. The goal is to create a pipeline that automates as much data processing as it can. 

![data-8-1-1-1-extract-transform-load](https://user-images.githubusercontent.com/73972332/105554938-34d6d180-5cbd-11eb-9882-1c37d69511bb.png)

### Project Scope
Amazing Prime Video is a platform for streaming movies and tv shows. The Amazing Prime Video team would like to develop an algorithm to predict which low budget movies being released will become popular so that they can buy the streaming rights at a bargain. Amazing Prime has decided to sponsor a hackathon; providing a clean set of movie data and asking participants to predict the popular pictures. We are tasked with creating the datasets for the hackathon from unmerged and uncleaned data. We are tasked with creating a data pipeline process for this data, from origination, to finished cleaned tables in SQL. 
### Data Origination
There are two data sources. A scrape of Wikipedia for all movies released since 1990 and rating data from MovieLand's website. We need to extract the data from the two sources, transform it into one clean dataset, and finally load that dataset into a SQL table. 

## ETL Implementation
Once we have gathered all the source data, we need to perform the most challenging aspect of the ETL process: the Transform phase. In order to transform the data, we will need to perform three separate steps. 
* Write an ETL Function to Read Three Data Files
    * This function is fairly straight forward; we use python and pandas to open and read all three files into three separate dataframes. The two csv files can be read with the pd.read_CSV function in pandas while the JSON file can be opened and read using a "with open ( ) as file" statement and then using the method json.load inside the "with" function.  
* Extract and Transform the Wikipedia Data
    * The process to clean the wikipedia data is more complex and will take quite a few steps. We first want to create a non-destructive copy so that we do not destroy any of our original data. This is just good coding practice. Upon first glance of our dataframe, we see there are a lot of columns that don't relate in any way to movie data. Since this data won't help our hackathon goal, we can remove it from our non-destructive copy. Let's modify our JSON data by restricting it to only those entries that have a director and an IMDb link and also removing any tv shows. We can do this with a list comprehension.  We just want to focus on one title in english so we can eliminate all the columns for alternate titles. We can consolidate columns with the same data into one column. We can also remove columns with any imdb_id duplicates, drop columns with null values, and use regex to filter by box office data 
* Extract and Transform the Kaggle data
After transforming the data, we will finally create one Movie Database by loading the information into PostgreSQL. 
## Results

