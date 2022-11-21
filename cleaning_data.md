What issues will you address by cleaning the data?

In general as part of the cleaning process I am looking to address the following:
- Removal of duplicate information
- Removal of non pertinent or sparse information
- Trim and simplify data to provide clarity
See the section below for specific changes in each table.

### sales_report review
- Product SKU format changes around row 200. Will compare accross products and sales_by_sku to see if change is common. sku starting with 9 carry 0 orders. zero or null values for ratio. Ratio is a measure of total_ordered / stock level. 
- Null values replaced with '0' in ratio column.

        select case
            when ratio is null then '0'
            else ratio
            end ratio
        from sales_report
    
- No duplicates for sku found in sales_report
- Name updated to lowercase to standardise strings as lowercase.

        update sales_report
        SET name = lower(name)

### sales_by_sku
- No duplicates observed.
- 8 SKUs missing from sales report vs sales_by_sku. No action taken.
- No missing or Null values observed.
- Compared total_ordered between sales report and by sku to see if they contain the    same totals. They do.
- Considering sales_by_sku contains all duplicate information the table can either be disregarded or deleted.

        select  sbs.productsku,
                sum(sbs.total_ordered - sr.total_ordered) order_diff
       
        from sales_by_sku sbs 
        
        join sales_report sr
        on sr.productsku = sbs.productsku

	    group by 1
	    having sum(sbs.total_ordered - sr.total_ordered) > '0' 
	    order by 2 desc
        

### products review
- Name updated to lowercase to standardise strings as lowercase.
- Total count (1092), sku (count) no duplicates observed.
- 1 instance of null values in sentiment and sentiment magnitude columns for "GGADFBSBKS42347". Single instance will be ignored. 

### analytics review
- Removed userid as all values are null
- Removed units sold
- Removed timeonsite as duplicated by visitId
- Removed revenue as all values are null
- VisitId is a duplicated record for visitstarttime. Can be removed.
- timeonsite was assumed to be in seconds and was converted to minutes
- visitstarttime is recorded in Unix timestamp and was converted to timestamp using the query below and inserted in column "visitstarttime_new" with data type timestamp. The orginal viststarttime was dropped:
    
        update analytics_clean
        set visitstarttime_new = to_timestamp(visitstarttime) 
        

- unit_price was reduced by factor of 1,000,000 and rounded to 2 decimal places

        update analytics_clean
        set unit_price = round(unit_price / 1000000, 2)
    

### all_sessions review
- 'time' has been recorded as an INT. Assumption that time is in sec. To be converted to minutes for clarity

        update all_sessions
        set "time" = round("time" / 60, 2)

- ProductRefundAmount is null for every row. Will be removed. We can also infere that in this dataset there were no refunds processed.
- City is an unreliable field. 8674 out of 15134 rows where city is not available in dataset or not set. >50% missing. Column removed.
- itemQuantity removed as all values = null
- itemRevevue removed as redunent 
- transactionRevenue removed as redunent
- searchKeyword removed as allvalues = null
- currencyCode removed as all USD bar a few null values.
- Only 81 transactions recorded
- Review changing text columns to lowercase
- TotalTransactionRevenue was reduced by factor of 1,000,000 and rounded to 2 decimal places

        update all_sessions
        set "totalTransactionRevenue" = round("totalTransactionRevenue" / 1000000, 2)
    
- productPrice, productRevenue was also reduced and rounded as above.





Queries:
Refer to the above.
