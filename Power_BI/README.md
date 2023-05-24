# Power BI

## Defined Terms

`Power BI`: A business intelligence and data visualization tool developed by Microsoft. It allows users to connect to various data sources, transform and shape the data, create interactive visualizations, and share insights with others. Power BI provides a user-friendly interface and powerful analytical capabilities to help users explore and analyze their data, make informed business decisions, and create visually appealing reports and dashboards.

`Calendar Table`: In Power BI, a calendar table is a table that contains dates and related information, such as day, month, year, quarter, and other relevant attributes. It is commonly used in data modeling to support time-based analysis and calculations. A calendar table provides a comprehensive set of dates, which enables users to easily slice and dice their data based on different time periods, such as daily, weekly, monthly, or yearly. It serves as a foundation for time intelligence calculations and facilitates advanced analytics in Power BI.

`M`:  A functional language used in Power Query, the data transformation and data preparation component of Power BI. Power Query allows users to connect to various data sources, perform data transformations, and shape the data before loading it into Power BI for analysis and visualization. M is the underlying language used to write the queries and expressions in Power Query. It provides a flexible and expressive syntax for data manipulation, including operations such as filtering, merging, pivoting, and aggregating data. With M, users can define custom data transformations and perform complex data cleansing tasks to prepare their data for analysis in Power BI.

`DAX`: DAX stands for Data Analysis Expressions. It is a formula language used in Microsoft Power BI, as well as other Microsoft products such as Power Pivot and SQL Server Analysis Services (SSAS). DAX is designed to perform calculations and create custom formulas within data models. DAX functions and expressions are used to manipulate and analyze data in Power BI. It provides a wide range of functions for data aggregation, filtering, calculation, and data modeling. With DAX, users can create measures, calculated columns, and calculated tables to define custom calculations and business logic based on their data. DAX is particularly powerful for performing calculations and creating complex formulas in Power BI. It supports advanced functions such as time intelligence functions, statistical functions, logical functions, and text functions. DAX formulas can be written to calculate totals, perform mathematical operations, apply conditional logic, and perform calculations across different dimensions or hierarchies. DAX expressions are written using a combination of functions, operators, and constants. The expressions are evaluated within the context of the data model and can reference columns, tables, and other measures within the model.

## Calendar Table Example

When making a calendar table, I like to add as many options that I can possibly think of, as someone will likely request the option to filter by some random time interval such a `season`. Here is an example of how to write the logic for a calendar table, using the propritary `M` language.

```m
let
    StartDate = #date(2023, 1, 1), // Start date of the calendar
    EndDate = #date(2023, 12, 31), // End date of the calendar
    NumberOfDays = Duration.Days(EndDate - StartDate) + 1, // Total number of days in the calendar
    Source = List.Dates(StartDate, NumberOfDays, #duration(1, 0, 0, 0)), // Generate a list of dates from StartDate to EndDate
    Table = Table.FromList(Source, Splitter.SplitByNothing(), {"Date"}, null, ExtraValues.Error), // Convert the list into a table
    #"Changed Type" = Table.TransformColumnTypes(Table, {{"Date", type date}}), // Change the data type of the "Date" column to Date
    #"Inserted Year" = Table.AddColumn(#"Changed Type", "Year", each Date.Year([Date])), // Add a column for the year
    #"Inserted Month" = Table.AddColumn(#"Inserted Year", "Month", each Date.Month([Date])), // Add a column for the month
    #"Inserted Day" = Table.AddColumn(#"Inserted Month", "Day", each Date.Day([Date])), // Add a column for the day
    #"Inserted Quarter" = Table.AddColumn(#"Inserted Day", "Quarter", each Date.QuarterOfYear([Date])), // Add a column for the quarter
    #"Inserted Weekday" = Table.AddColumn(#"Inserted Quarter", "Weekday", each Date.DayOfWeek([Date])) // Add a column for the weekday
in
    #"Inserted Weekday"
```
