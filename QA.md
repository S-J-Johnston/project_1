What are your risk areas? Identify and describe them.
- The data contained in the Analytics table and the All_sessions table are from different time periods but common keys (fullvisitorId) that can be used to bridge data, however the product and sales data do not contain time stamps or an indication of the date they were pulled or the period they reflect. Therefore they don't have much use for comparing sales figures across the different tables.

QA Process:
- Validation of record counts by running sections of code in isolation
- Manually calculating % figures from simplified sections of code
- Running every iteration of the join function to see what it does to the answer.

