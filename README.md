# Overview
Columbia Data Science Module 8

In this module, I created an ETL function to read three disparate data files containing movie data. I then extracted and transformed the data from Wikipedia and Kaggle before creating a database in PGAdmin. 

Please note: I tried to upload the .csv files to the resources folder, but even when compressed, they were too large for Github to accept. 

## Tools

Software: Jupyter Notebook, Python, PostgreSQL, SQL, 

Python packages: json, pandas, numpy, re, sqlalchemy, time

Data: movies_metadata.csv and ratings.csv from [Kaggle](https://www.kaggle.com/rounakbanik/the-movies-dataset?select=ratings.csv), [wikipedia-movies.json](https://github.com/perryabdulkadir/Movies-ETL/blob/main/Resources/wikipedia-movies.json)


## Analysis

### Writing an ETL Function to Read Three Data Files

I started in [ETL_function_test.ipynb](https://github.com/perryabdulkadir/Movies-ETL/blob/main/ETL_function_test.ipynb), writing a function that takes three arguments. It loads in the two .csv files (movies_metadata.csv and ratings.csv) and the JSON file (wikipedia-movies.json) and turns them all into data frames (kaggle_metadata, ratings, and wiki_movies_df).

```
def extract_transform_load(wikidata, ratings, movielens):
    # 2. Read in the kaggle metadata and MovieLens ratings CSV files as Pandas DataFrames.
    kaggle_metadata = pd.read_csv(movielens)
    ratings = pd.read_csv(ratings)

    # 3. Open the read the Wikipedia data JSON file.
    with open(wikidata, mode='r') as file:
        wiki_movies_raw = json.load(file)
    # 4. Read in the raw wiki movie data as a Pandas DataFrame.
    wiki_movies_df =  pd.DataFrame(wiki_movies_raw)
    # 5. Return the three DataFrames
    return wiki_movies_df, kaggle_metadata, ratings
   ```
   
  I made variables representing file directories for each of the data sources. 
  
  ```
  file_dir = 'C:/Users/Perry/Movies-ETL' 
# Wikipedia data
wiki_file = f'{file_dir}/wikipedia-movies.json'
# Kaggle metadata
kaggle_file = f'{file_dir}/movies_metadata.csv'
# MovieLens rating data.
ratings_file = f'{file_dir}/ratings.csv'
```

Then, I set the file directory variables equal to the extract_transform_load function. 

```
wiki_file, kaggle_file, ratings_file = extract_transform_load(wiki_file, ratings_file, kaggle_file)
```
Next, I set the data fames equal to the file variables. 

```
wiki_movies_df = wiki_file
kaggle_metadata = kaggle_file
ratings = ratings_file
```
I checked each data frame to verify it had been properly populated. 

* **wiki_movies_df**
![wiki_movies_df](Resources/wiki_movies_df.PNG)

* **kaggle_metadata_df**
![kaggle_metadata_df](Resources/election_results_printout.PNG)

* **ratings**
![ratings_df](Resources/ratings_df.PNG)

## Extracting and Transforming the Wikipedia Data
Unfortunately, due to numerous issues of inconsistent formatting, it was necessary to clean and transform the Wikipedia data before I could proceed with analysis. I began coding in a new notebook, [ETL_clean_wiki_movies.ipynb](https://github.com/perryabdulkadir/Movies-ETL/blob/main/ETL_clean_wiki_movies.ipynb).

The clean_movie function takes the argument 'movie' and combines alternate titles into one list, then combines synonymous words and phrases (e.g. 'Directed by' and 'Director'). 

![clean_movie_function](Resources/clean_movie_function.PNG)

I then began constructing a large function, also called extract_transform_load, that takes in the arguments wikidata, ratings, and movielens. The first section of the function reads in the JSON and CSV files as data frames. A list comprehension removes TV shows from the Wikipedia data by removing entries that have 'No. of episodes' in them. Another list comprehension iterates through the wiki_movies list and applies the clean_movie function to each movie. Then, the resulting clean_movies list is turned into a data frame, wiki_movies_df. 




![etl_function_1](Resources/etl_function_1.PNG)
