Answer the following questions and provide the SQL queries used to find the answer.

    
**Question 1: Which cities and countries have the highest level of transaction revenues on the site?**


SQL Queries:


    select  country, 
            sum("totalTransactionRevenue"), 
            count("totalTransactionRevenue") 
        from all_sessions
    
        group by country
    
        having sum("totalTransactionRevenue") > 0
    
        order by 2 desc
        limit 1


Answer: The USA $13,154.17
I had removed the city column as it contained mostly incomplete values. the above query can be used to calculate the values for cities if city is substituted for country.




**Question 2: What is the average number of products ordered from visitors in each city and country?**


SQL Queries:
    

    SELECT al."country", round(AVG(an."units_sold"), 2) avg_units_sold
	        FROM (
	        analytics_clean an
		
		    JOIN all_sessions al
		    ON an."fullvisitorId" = al."fullVisitorId")
		
	    GROUP BY 1
	    ORDER BY round(AVG(an."units_sold"), 2) ASC


Answer: Orginally I looked at the productQuantity from the all_sessions table but the quanity data is extremely sparse and looks to be useless. Instead I decided to join the units sold data from analytics to the country data in all_sessions. 
42 countries where returned where average units were greater than 0 or Null. Ranging from Guatamala (1) to USA (19.24)





**Question 3: Is there any pattern in the types (product categories) of products ordered from visitors in each city and country?**


SQL Queries:

    SELECT 	q.product_cat,
		    q.rank,
		    COUNT(q.rank) 
		    FROM 
		    (
 			    SELECT r.* 
			    FROM 
			    (
				    SELECT al."country",
				    al."v2ProductCategory" product_cat,
				    count("v2ProductCategory") total_orders, 
 				    RANK() OVER (partition by al."country" ORDER BY count("v2ProductCategory") DESC) rank
	                FROM 
				    (
	        		analytics_clean an
		
		            JOIN all_sessions al
		            ON an."fullvisitorId" = al."fullVisitorId")
		
	    	    GROUP BY 1, 2
	   		    ORDER BY 1, 3 desc) r
		WHERE RANK <= 3) q
		GROUP BY 2, 1
		ORDER BY 3 DESC



Answer: There is probably an easier way of pulling out these results but the triple nested query above counts the top 3 ordered product categories by country and counts how often they were in a given rank. It was difficult to pull out a trend that fits all countries but it was clear from the table that Home/Shop by Brand/YouTube and Home/Apparel/Mes/Men's-T-Shirits were common amoung the top 3 product categories for most countries. Products from Home/Shop by Brand/YouTube were the top ranked product category in 45 countries and second in 21 countries.


**Question 4: What is the top-selling product from each city/country? Can we find any pattern worthy of noting in the products sold?**


SQL Queries:
   
   
    SELECT r.* FROM (
                SELECT al."country",
                al."v2ProductName" product_name,
                count("v2ProductName") total_orders, 
                RANK() OVER (partition by al."country" ORDER BY count("v2ProductName") DESC) rank
	        
                FROM (
	            analytics_clean an
		
		        JOIN all_sessions al
		        ON an."fullvisitorId" = al."fullVisitorId")
		
	        GROUP BY 1, 2
	        ORDER BY 1, 3 desc) r
		where rank <= 3 



Answer: It is difficult to pick up a trend that is common across all countries but if we look at a sample of countries with high orders we can see that the products are a mix of mens apparal and stationary products like notebooks or journals. 


**Question 5: Can we summarize the impact of revenue generated from each city/country?**

SQL Queries:

        Select r.* 
            from 
            (
                select al."country", 
                sum(an.units_sold * an.unit_price) revenue
                from
                    analytics_clean an
                    join all_sessions al
                    on an."fullvisitorId" = al."fullVisitorId"
        
                group by 1
                order by 2 desc ) r
                where revenue is not null



Answer: The total revenue figures from all_sessions were sparse so instead the revenue generated was assumed to be the number of units sold by the units price from the analytics table. The US generates more revenue than all other countries combined with $11.9 M generated from 2017-05-01 -> 2017-08-01. 







