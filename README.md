# Movies_ETL
Data Pipeline Process: Extract, Transform, and Load. Extract from JSON and CSV files, transform data in python and pandas using jupyter notebooks, then load cleaned data into PostgreSQL database.

## Overview
### ETL Purpose
The purpose of this repository is to perform an ETL from start to finish on data originating from JSON wikipedia and CSV formats. We will extract the data (pull the data from the sources), Transform the data (use regular expressions to parse data and to transform text into numbers, clean the data, eliminate unnecessary columns, merge dataframes, and load the data into PostgreSQL. ETL is the workhorse for moving information between databases to improve performance and comprehension. ETL is a core concept in data engineering in that it helps organizations have consistent data with maintained integrity. The goal is to create a pipeline that automates as much data processing as it can. 

![data-8-1-1-1-extract-transform-load](https://user-images.githubusercontent.com/73972332/105554938-34d6d180-5cbd-11eb-9882-1c37d69511bb.png = 250x250)

### Project Scope
Amazing Prime Video is a platform for streaming movies and tv shows. The Amazing Prime Video team would like to develop an algorithm to predict which low budget movies being released will become popular so that they can buy the streaming rights at a bargain. Amazing Prime has decided to sponsor a hackathon; providing a clean set of movie data and asking participants to predict the popular pictures. We are tasked with creating the datasets for the hackathon from unmerged and uncleaned data. We are tasked with creating a data pipeline process for this data, from origination, to finished cleaned tables in SQL. 

## ETL Implementation
### Extract 
There are two data sources. A scrape of Wikipedia for all movies released since 1990 and rating data from MovieLand's website. We need to extract the data from the two sources, transform it into one clean dataset, and finally load that dataset into a SQL table. 
### Transform
Once we have gathered all the source data, we need to perform the most challenging aspect of the ETL process: the Transform phase. In order to transform the data, we will need to perform four main steps. 
* Write an ETL Function to Read Three Data Files
    * This function is fairly straight forward; we use python and pandas to open and read all three files into three separate dataframes. The two csv files can be read with the pd.read_CSV function in pandas while the JSON file can be opened and read using a "with open ( ) as file" statement and then using the method json.load inside the "with" function.  
* Extract and Transform the Wikipedia Data
    * The process to clean the wikipedia data is more complex and will take quite a few steps. We first want to create a non-destructive copy so that we do not destroy any of our original data. This is just good coding practice. 
    * Upon first glance of our dataframe, we see there are a lot of columns that don't relate in any way to movie data. Since this data won't help our hackathon goal, we can remove it from our non-destructive copy. Let's modify our JSON data by restricting it to only those entries that have a director and an IMDb link and also removing any tv shows. We can do this with a list comprehension.  
    * We just want to focus on one title in english so we can eliminate all the columns for alternate titles. 
    * We can consolidate columns with the same data into one column. 
    * We can also remove columns with any imdb_id duplicates, drop columns with null values, and use regex to clean by box office data, budget, release date, and running time. 
* Extract and Transform the Kaggle data
    * Because the Kaggle data came in as a CSV, one of the first things we want to check is that all of the columns came in as the correct data types.
    * We use the .value_counts() method to check that all values are either true or false. Since we find they aren't, we remove the bad data by dropping columns that are false. 
    * We convert the data types for each of the six columns that need to be converted to their proper type. 
    * We run reasonability checks on the ratings data to make sure dates don't seem outlandish. 
    * Finally, we look at some statistics of the actual ratings and see if there are any glaring errors. A quick, easy way to do this is to look at a histogram of the rating distributions, and then use the describe() method to print out some stats on central tendency and spread.
* Merge the Wikipedia and Kaggle Data
    * Now that the Wikipedia data and Kaggle data are cleaned up and in tabular formats with the right data types for each column, we can join them together. However, after they're joined, the data still needs to be cleaned up a bit, especially where Kaggle and Wikipedia data overlap. We remove columns that are redundant. 
### Load
After transforming the data, we will finally create one Movie Database by loading the information into PostgreSQL. We are now onto the final stage of our ETL Process: the Load Phase. 
* Here is where we connect Pandas and SQL. Amazing Prime has decided the easiest way to make the data accessible for the hackathon is to provide a SQL database to the participants. We need to move the data from Pandas into a PostgreSQL database.
    * We create a new database and use the built-in to_sql() method in Pandas to create a table for our merged movie data. We also import the raw ratings data into its own table.
    * We create the database engine in our python jupyter notebook file, and then run the code to import the movie and ratings data into PostgreSQL while printing out the number of rows that import along with the elapsed time. 
## Results
After running two queries, we arrive at the following results:

![movies_query](https://user-images.githubusercontent.com/73972332/105557871-1b845400-5cc2-11eb-8671-1017c3a4bda5.png)
![ratings_query](https://user-images.githubusercontent.com/73972332/105557919-3e166d00-5cc2-11eb-9821-d771553b4b3b.png)
![final_run_time](https://user-images.githubusercontent.com/73972332/105558123-baa94b80-5cc2-11eb-9b01-c51f0e4b4bf8.png)

We can see that both our movies and ratings tables have loaded successfully into our PostgreSQL Database and the final runtime to load the data was 2944.77 seconds, or 49 minutes. After all that parsing, cleaning, and merging, we now have a full and final dataset in one merged database to use for our Amazing Prime Hackathon!

