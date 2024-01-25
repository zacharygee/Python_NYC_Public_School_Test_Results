# NYC Public School Test Results
As I continue to learn Python, this project aims to identify NYC's public schools with top math results on the SAT, how performance varies by borough, and the city's top 10 performing schools overall. You can find the CSV file to the dataset used in this project in this repository.

## Table of Contents
1. [Finding the Schools with the Best Math Scores](#finding-the-schools-with-the-best-math-scores)
2. [Identifying the Top 10 Schools](#identifying-the-top-10-schools)
3. [Locating the NYC Borough with the Largest Variation](#locating-the-nyc-borough-with-the-largest-variation)

## Finding the Schools with the Best Math Scores
To start off, I load in the pandas package, read in the data, and investigate the first few rows of the dataset.
```
schools = pd.read_csv("schools.csv")

schools.head()
```
To find the schools with the best math scores, I create a dataframe titled "best_math_scores" that ranks the schools with an average SAT math score of at least 80% of the maximum score of 800 in descending order. I then print and view the results.
```
best_math_schools = schools[schools["average_math"] / 800 >= 0.8][["school_name", "average_math"]].sort_values("average_math", ascending=False)

print(best_math_schools)
```

## Identifying the Top 10 Schools
To identify the top 10 performing schools based on SAT scores, I first create a new column in our schools dataset titled "total_SAT" that sums the average math, reading, and writing scores. I then create a dataframe called "top_10_schools" that takes the mean of this column and group it by school, ranking by the total SAT score column in descending order.
```
schools["total_SAT"] = schools["average_math"] + schools["average_reading"] + schools["average_writing"]
top_10_schools = schools.groupby("school_name")["total_SAT"].mean().reset_index().sort_values("total_SAT", ascending=False).head(10)

print(top_10_schools)
```

## Locating the NYC Borough with the Largest Variation
Lastly, I locate the NYYC with the largest variation. In other words, which borough has the largest standard deviation. This is done by first creating a dataframe titled "boroughs" and groupying by the "borough" column and aggregating the count, mean, and standard deviation of the "total_SAT" column created before, rounding to 2 decimal places.
```
boroughs = schools.groupby("borough")["total_SAT"].agg(["count", "mean", "std"]).round(2)
```
To find the largest standard deviation, I create a new dataframe titled "largest_std_dev" which takes the max "std" out of the boroughs dataframe.
```
largest_std_dev = boroughs[boroughs["std"] == boroughs["std"].max()]
```
Finally, I update the "largest_std_dev" dataframe to display the count, mean, and standard deviation.
```
largest_std_dev = largest_std_dev.rename(columns={"count": "num_schools", "mean": "average_SAT", "std": "std_SAT"})

print(largest_std_dev)
```
