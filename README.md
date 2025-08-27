# OddballChallenge
Customer Support Challenge Repo for Prachi Yadav
Welcome to my README File for the Oddball Customer Support Challenge!

##Downloading Data and Associated R Files
1. Navigate to https://github.com/prachisyadav/OddballChallenge/tree/main and under the green button labeled "Code" either clone the repo or download Zip.
2. If downloaded zip, unzip.
3. Open OddballCustomerSupport.Rmd using RStudio.
4. Run the Chunks in RStudio (Ctrl+Shift+Enter to run current chunk or Ctrl+Alt+R to run all) according to the descriptions below:

###Chunk 1:
Installs all necessary packages (if not already installed) on your machine. Library(packagename) locates and loads the specific package.

###Chunk 2:
Ingests initial data. This includes the initial csv files `agents.csv`, `contact_centers.csv`. `interactions.csv`, and `service_categories.csv`. The ingesting of initial files relies on variable `subdir`, which is set to "Initial", so it is important to maintain the original folder structure. 
`dfs_initial` is created as a list of 4 dataframes of the original data. Function `list2env` transfers the components of the list to the global environment. This si what creates the 4 dataframes.

###Chunk 3:
Trims white spaces from initial data column names. This is good practice and to avoid mismnaming in the future chunks.

###Chunk 4:
Ingests February delta data. In this case there are 3 February delta files (`agents_delta_202502.csv`, `service_categories_202502.csv`, and `interactions_202502.csv`). The ingestion relies on a pattern in which "_delta" is followed by "_202502" and must be a .csv to ensure the correctly named files are ingested.

###Chunk 5:
Trims white spaces from February delta column names. 
Also executes the `add` function on February delta data by using `plyr` package and specifically function `rbind.fill`.
Also executes the `update` function on February delta data by using the `dplyr` package and specifically function `rows_update`.
Also executes the `delete` function on February delta data by using the `dplyr` package and specifically function  `rows_delete`.
Pushes latest dataframes to global environment.

###Chunk 6:
Ingests March delta data. In this case there are 4 March delta files (`agents_delta_202503.csv`, `contact_centers_delta_202503.csv`, `service_categories_202503.csv`, and `interactions_202503.csv`). The ingestion relies on a pattern in which "_delta" is followed by "_202503" and must be a .csv to ensure the correctly named files are ingested.

###Chunk 7:
Trims white spaces from March delta column names. 
Also executes the `add` function on March delta data by using `plyr` package and specifically function `rbind.fill`.
Also executes the `update` function on March delta data by using the `dplyr` package and specifically function `rows_update`.
Also executes the `delete` function on March delta data by using the `dplyr` package and specifically function  `rows_delete`.
Pushes latest dataframes to global environment.

###Chunk 8:
Assigns `dfs_final` as the final list of dataframes with the most updated data. Removes the obsolete "action" column and sets deleted dimensions as `Unknown`.
Transforms timestamps from UTC timezone to EST timezone.
Creates `month` column in `interactions` dataframe.
Builds `support_report` using left joins and groups by `month`, `contact_center_name`, and `department`. Also includes calculated columns `total_interactions`, `total_calls`, and `total_call_duration`.
Option to export to `.csv`, `.json`, or `.parquet`.

###Chunk 9:
Supplemental data outputs for business questions.

##Business Questions:


