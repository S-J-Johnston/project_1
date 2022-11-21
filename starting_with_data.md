Question 1: find all duplicate records

SQL Queries:

Answer: 


Question 2: Find the total number of unique visitors (`fullVisitorID`)

SQL Queries:
''' 
{ 
    select count("fullvisitorId") from (
	    select "fullvisitorId" from analytics_clean
	    union
	    Select "fullVisitorId" from all_sessions) subq
}
,,,

Answer: 
It depends which table you consult. There were 14223 unique visitors from all_sessions. All sessions contains data from 2016-08-01 -> 2017-08-01. The difference in the purpose of all_sessions versus analytics is unknown. By contrast analytics contains 120018 unique visitors from 2017-05-01 -> 2017-08-01. Based on the 2 wildly different figures and date ranges we can look at the total number of unique visitors across both tables which is 130,345.


Question 3: find the total number of unique visitors by referring sites

SQL Queries: 
   

    SELECT COUNT(DISTINCT "fullvisitorId"), channelgrouping 
    
    FROM (
    
            select "fullvisitorId", channelgrouping
            from analytics_clean
            union
            Select "fullVisitorId", "channelGrouping" 
            from all_sessions
            ) subq

        where channelgrouping = 'Referral'

        GROUP BY 2
        ORDER BY 1 desc
    

Answer: 20129

Question 4:
Find each unique product viewed by each visitor

SQL Queries:

	select "fullVisitorId", "productSKU", "v2ProductName" 
        from all_sessions
    order by 1
        

Answer: This question seemed so straightforward to the point it seemed too simple and  I began to overthink it. In the end I took it to mean "What products did each member view?" The query above yields 15134 rows showing which products each memeber viewed.

Question 5: compute the percentage of visitors to the site that actually makes a purchase

SQL Queries:

Answer: 95147/4301122 * 100 = 2.2%. I could not get my code to work but working it out manually the target answers is approximately 2.2%.
