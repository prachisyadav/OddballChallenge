# OddballChallenge
Customer Support Challenge Repo for Prachi Yadav

Welcome to my README File for the Oddball Customer Support Challenge!

## Downloading Data and Associated R Files
1. Navigate to https://github.com/prachisyadav/OddballChallenge/tree/main and under the green button labeled "Code" either clone the repo or download Zip.
2. If downloaded zip, unzip.
3. Open OddballCustomerSupport.Rmd using RStudio.
4. Run the Chunks in RStudio (Ctrl+Shift+Enter to run current chunk or Ctrl+Alt+R to run all) according to the descriptions below:

## Chunk Descriptions 
### Chunk 1:
Installs all necessary packages (if not already installed) on your machine. Library(packagename) locates and loads the specific package.

### Chunk 2:
Ingests initial data. This includes the initial csv files `agents.csv`, `contact_centers.csv`. `interactions.csv`, and `service_categories.csv`. The ingesting of initial files relies on variable `subdir`, which is set to "Initial", so it is important to maintain the original folder structure. 
`dfs_initial` is created as a list of 4 dataframes of the original data. Function `list2env` transfers the components of the list to the global environment. This si what creates the 4 dataframes.

### Chunk 3:
Trims white spaces from initial data column names. This is good practice and to avoid mismnaming in the future chunks.

### Chunk 4:
Ingests February delta data. In this case there are 3 February delta files (`agents_delta_202502.csv`, `service_categories_202502.csv`, and `interactions_202502.csv`). The ingestion relies on a pattern in which "_delta" is followed by "_202502" and must be a .csv to ensure the correctly named files are ingested.

### Chunk 5:
Trims white spaces from February delta column names. 
Also executes the `add` function on February delta data by using `plyr` package and specifically function `rbind.fill`.
Also executes the `update` function on February delta data by using the `dplyr` package and specifically function `rows_update`.
Also executes the `delete` function on February delta data by using the `dplyr` package and specifically function  `rows_delete`.
Pushes latest dataframes to global environment.

### Chunk 6:
Ingests March delta data. In this case there are 4 March delta files (`agents_delta_202503.csv`, `contact_centers_delta_202503.csv`, `service_categories_202503.csv`, and `interactions_202503.csv`). The ingestion relies on a pattern in which "_delta" is followed by "_202503" and must be a .csv to ensure the correctly named files are ingested.

### Chunk 7:
Trims white spaces from March delta column names. 
Also executes the `add` function on March delta data by using `plyr` package and specifically function `rbind.fill`.
Also executes the `update` function on March delta data by using the `dplyr` package and specifically function `rows_update`.
Also executes the `delete` function on March delta data by using the `dplyr` package and specifically function  `rows_delete`.
Pushes latest dataframes to global environment.

### Chunk 8:
Assigns `dfs_final` as the final list of dataframes with the most updated data. Removes the obsolete "action" column and sets deleted dimensions as `Unknown`.
Transforms timestamps from UTC timezone to EST timezone.
Creates `month` column in `interactions` dataframe.
Builds `support_report` using left joins and groups by `month`, `contact_center_name`, and `department`. Also includes calculated columns `total_interactions`, `total_calls`, and `total_call_duration`.
Option to export to `.csv`, `.json`, or `.parquet`.

### Chunk 9:
Supplemental data outputs for business questions.

## Business Questions:
### What were the total number of interactions handled by each contact center in Q1 2025?

_Business_ _Answer_: During Q1 of 2025, the total number of interactions by "Atlanta GA SE" was 8. The total number of interactions by "Boston MA NE" was 13. The total number of interactions by "Richmond VA E" was 7.

_Technical_ _Explanation_: I filtered support_report so that the month column only included January, February, and March (even though that was all the data), then grouped by `contact_center_name`, and finally used the summarise() function to sum total interactions per contact center.

### Which month (Jan, Feb, or Mar) had the highest total interaction volume?

_Business_ _Answer_: February 2025 had the highest total interaction volume with 11 interactions. Second was March 2025 with 10 interactions and third was January 2025 with 7 interactions.

_Technical_ _Explanation_: I used support_report to group by month and then the summarise() function to sum total interactions by month. `slice_max` can optionally be used to retain only the max value in the output.

### Which contact center had the longest average phone call duration (total_call_duration)?

_Business_ _Answer_: "Richmond VA E" had the longest average phone call duration with an average of 15.5 minutes. Second was "Boston MA NE" with 14.0 minutes and third was "Atlanta GA SE" with  13.5 minutes.

_Technical_ _Explanation_: I created a data frame `modified_for_calls` that filtered records so that `total_calls` had to not be equal to 0. This is to remove records that were web interactions. I then grouped by `contact_center_name` and took the mean of `total_call_duration`.

_Follow_-_up_: Why might this be the case based on the interactions data? Upon further examination, I found that in the interactions data, when I filtered for `category_ID` to be "TECH" and `channel` to be "phone", contact center "Richmond VA E" was responsible for 50% of the technical support interactions. This makes sense, because technical support calls can arguably take longer than benefits, eligibility, or even scheduling calls.

_Follow_-_up_: What approach would you recommend to measure agent work time more accurately? To inspect further, I created dataframe `inspect` with time-related data. I created variable `time_until_resolution` as a better way to measure agent work time. This is because `agent_resolution_timestemp` is not always the same as `interaction_end`. For example, some agents may still stay on a phone call several minutes after the issue is marked as resolved. My new variable also accounts for non-phone interactions like the web, since `call_duration_minutes` can be misleading with its current state of including several zeroes.

--
## That's the end! 
### I hope you've enjoyed my analysis!


