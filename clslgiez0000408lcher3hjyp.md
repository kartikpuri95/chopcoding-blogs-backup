---
title: "Using clickhouse to transform tick data into OHLC"
datePublished: Wed Feb 14 2024 07:14:34 GMT+0000 (Coordinated Universal Time)
cuid: clslgiez0000408lcher3hjyp
slug: using-clickhouse-to-transform-tick-data-into-ohlc
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1707894722793/2da7fb10-5216-44e9-9d7e-cff02c0e9ea1.png
tags: newbie, sql, clickhouse, dataanalytics

---

### Introduction

In the world of finance, tick data represents the heartbeat of the market, capturing every price change and transaction that occurs. This granular data is invaluable for high-frequency trading strategies, market analysis, and understanding liquidity and volatility. Processing and analyzing tick data, however, requires powerful data management solutions. This is where ClickHouse, an open-source columnar database optimized for fast, real-time analytics, shines. In this article, we'll explore how to leverage ClickHouse to process and analyze tick data to extract Open, High, Low, Close (OHLC) values—a fundamental component of financial analysis.

### Understanding Tick Data

Tick data is composed of individual trades or quote changes including details such as price, volume, and transaction time. Unlike aggregated data, such as daily summaries, tick data provides a microscopic view of market behavior, enabling analysts to uncover patterns and trends not visible at higher levels of aggregation.

### Generating Sample Tick Data

To demonstrate the process of analyzing tick data with ClickHouse, we first generate a sample dataset. Our dataset simulates a single trading day's activity, capturing the price and volume for each second of an 8-hour trading session. This results in approximately 28,800 data points, offering a detailed canvas to illustrate our analysis.  
You can download the file [here](https://gist.github.com/kartikpuri95/93ec6b6683b794fda603559f53b0233f)

### Loading Data into ClickHouse

With our sample tick data in hand, the next step is to load it into ClickHouse for analysis. First, we define a table schema and create the table that matches our data's structure:

```sql
CREATE TABLE IF NOT EXISTS tick_data 
( Time DateTime, Price Float64, Volume UInt32 ) 
ENGINE = MergeTree() ORDER BY Time;
```

This schema is designed to efficiently store and query our tick data. To import the data, we use the following command, which pipes our CSV file directly into the ClickHouse client, connecting securely to our ClickHouse instance:

```sql
cat sample_tick_data_updated.csv | ./clickhouse client --host <your-cloud-url>.aws.clickhouse.cloud --secure --password 'your_password'
```

### Querying OHLC Values

Once the data is loaded, we can proceed to extract OHLC values for desired time intervals. The following SQL query calculates these values for each minute of trading activity:

```sql
SELECT
  toStartOfMinute(Time) as data_captured_datetime,
  argMin(Price, Time) as open,
  max(Price) as high,
  min(Price) as low,
  argMax(Price, Time) as close,
  sum(Volume) as total_volume
FROM tick_data
GROUP BY data_captured_datetime
ORDER BY data_captured_datetime;
```

This query groups the data into one-minute intervals, then calculates the opening price, highest price, lowest price, closing price, and total volume for each interval.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1707894442246/9bb3ebe9-c56c-45e5-a2bf-d2a46a8cddac.png align="center")

### Understanding the Query

* `toStartOfMinute(Time)`: This function rounds down the transaction time to the start of the minute, serving as our grouping mechanism.
    
* `argMin` and `argMax`: These functions identify the opening and closing prices within each minute by finding the earliest and latest transactions, respectively.
    
* `max` and `min`: These aggregate functions calculate the highest and lowest prices within each interval.
    
* `sum(Volume)`: Summarizes the total trading volume for each minute, providing insight into market activity.
    

### Conclusion

Analyzing tick data with ClickHouse provides a powerful toolkit for financial analysts and traders. By leveraging ClickHouse's efficient data storage and fast querying capabilities, we can uncover detailed insights into market dynamics, support decision-making processes, and develop sophisticated trading strategies. This article has demonstrated the process from data preparation to analysis, highlighting the ease and efficiency of using ClickHouse for financial data analytics.  
  
You can connect me on :

💼 Linkedin: [https://www.linkedin.com/in/kartikchop/](https://www.linkedin.com/in/kartikchop/)

🐦 Twitter: [https://twitter.com/chopcoding](https://twitter.com/chopcoding)