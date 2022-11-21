# Final-Project-Transforming-and-Analyzing-Data-with-SQL

## Project/Goals
- Learn to create tables and import data into a database 
- Clean data and in the process understand what the contents of each table are and how they relate to each other
- Analyse the database to draw conclusions about the information it contains

## Process
1. Review
2. Import
3. Clean
4. Analyse
5. QA

## Results
(fill in what you discovered this data could tell you and how you used the data to answer those questions)
- The primary country of business is the United States. We saw that the majority of purchases come from the US. $11M in revenue.
- The average time spent on the site for people who purchase a product is almost twice as long as that of browsers. 11.03 vs 19.86 minutes.
- Refer to cleaning_data, staring_with_data and starting_with_questions for further insights.

## Challenges 
- ### Loading sales_report.csv into a table:

    "Ratio" was trimmed to 7 decimals. Could be trimmed further. No value observed in a the 7th decimal place versus the 4th.

- ### Loading all_sessions.csv into a table:

    This was a bit of a nightmare to load into a table. I ended up changing the "Time" field to BIGINTs with the intention of formatting them to time later because it did not conform to the TIME format. I orginally started with VARCHAR(50) as a standard but found that was not long enough for some of the fields and defaulted to (200) and the max for "pageTitle". Then to top off the issues with getting this in there was a lot of adhoc inputting of "" and changing upper to lowercase and viceversa. I don't fully understand why "" is required in somecases and not others but going forward I will use them as default for column names when creating tables. I feel many of these issues could have been caused by my careless formatting in setting up column names and referencing.

- ### Loading Analytics.csv into a table:

    Time: time appears to be in epoch time. the number of seconds since january 1 1970. Needs to be entered as a BIGINT and then converted to DATETIME.

- ### Deciding how to interprete the intended use of the all_sessions table:

    The all_sessions table contained a large amount of sparse information. Without having the benefit of an SME who can provide context around the definition of fields and in what circumstances one field populates over another it was tricky to draw many meaningful conclusions using that dataset with regards to sales. However where it was useful was in tying product information and location data to visitorIds from the Analytics table.


## Future Goals
- In general I was short on time for most of the activities so could revise and add to much of the work I completed. I would have liked to have spent more time looking at the relationship between the analytics table and the product table. Specifically investigating at risk stock levels in the products table based on the rate of sales from the analytics table. 
- I would have liked to spend more time on the QA process to validate some of the figures that were calculated.
