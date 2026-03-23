Model Calculation using DAX

Part 1: Introduction to DAX & Calculated Tables

Objective: Understand DAX and create a calculated table to return distinct values.

Key Concept: DAX (Data Analysis Expressions) is the formula language used in Power BI. It is a collection of functions, operators, and constants used in formulas. DAX contexts enable dynamic analysis.

Action Items:

1. Open File: Open the "Sample Superstore" desktop file (built in Module 2).
2. Create a Calculated Table:
   · Go to the Modeling tab on the ribbon.
   · In the Calculations group, click New Table.
   · In the formula bar, rename the table from "Table" to Distinct Order Count.
   · After the = sign, build the DAX function: DISTINCT(Orders[Order ID])
   · Press Enter.
3. Verify Result:
   · Switch to Data View (second view button on the left).
   · In the Fields pane, click on the new Distinct Order Count table. The status bar should show 1,746 rows (distinct order IDs).
   · Click on the Orders table to see it has 2,428 rows (total orders).

---

Part 2: Creating Calculated Columns

Objective: Add new columns to an existing table using DAX.

Lesson 2.1: Calculate Days to Ship

Action Items:

1. Create a New Column:
   · In the Fields pane, right-click the Orders table.
   · Select New column.
2. Build the Formula:
   · Rename the column from "Column" to Days to Ship.
   · Use the DATEDIFF function: DATEDIFF(Orders[Order Date], Orders[Ship Date], DAY)
   · Press Enter. The new column will appear at the end of the Orders table.

Lesson 2.2: Create a Sales Ranking (Highest to Lowest)

Action Items:

1. Create Another Column:
   · With the Orders table selected, go to the Column tools tab and click New column.
2. Build the Formula:
   · Rename the column to Sales Ranking Highest.
   · Use the RANKX function with its required arguments: RANKX(Orders, Orders[Sales])
   · Press Enter. This ranks the highest sales as "1".

Lesson 2.3: Edit the Ranking (Lowest to Highest & Handling Ties)

Action Items:

1. Modify the Ranking:
   · In the formula bar, rename the column to Sales Ranking Lowest.
   · Add the optional ASC argument to change the order: RANKX(Orders, Orders[Sales], , ASC)
   · Press Enter. Now the lowest sales are ranked "1".
2. Simulate Ties to Understand RankX:
   · Open the source Excel file (Sample Superstore.xlsx) and change a sales value (e.g., cell W369) to match another row (e.g., 52.97). Save and close the file.
   · In Power BI, on the Home tab, click Refresh.
   · Filter the Orders table by the two order IDs to see they have the same sales value and the same rank (e.g., 546). The next rank (e.g., 547) is skipped.
3. Prevent Skipping Ranks:
   · In the formula bar, rename the column to Sales Ranking Lowest No Skip.
   · Add the Dense argument to the RANKX function: RANKX(Orders, Orders[Sales], , ASC, Dense)
   · Press Enter. Now, when ties occur, the next number is not skipped.

---

Part 3: Measures and Quick Measures

Objective: Create virtual calculations that do not increase dataset size.

Lesson 3.1: Create a Quick Measure

Action Items:

1. Create Quick Measure:
   · In the Fields pane, right-click the Orders table and select New quick measure.
2. Configure Calculation:
   · In the Calculation dropdown, under Total, select Total for category filters not applied.
   · For Base value, drag the Sales field into the box. Click the ellipsis (...) next to it and change "Sum" to Average.
   · For Category, drag the Customer Segment field into the box.
   · Click OK.
3. View Result:
   · Go to Report View.
   · Drag the new measure Average of Sales total for customer segment onto the canvas.
   · In the Visualizations pane, drag Customer Segment to the Axis box and Product Category to the Small multiples box. Resize the chart to see the results.

Lesson 3.2: Create a Measure from Scratch

Action Items:

1. Create New Measure:
   · In the Fields pane, right-click the Orders table and select New measure.
2. Build the Formula:
   · Name the measure Average Sales per Product Category.
   · Enter the formula:
     ```
     Average Sales per Product Category =
     CALCULATE(
         AVERAGE(Orders[Sales]),
         ALLSELECTED(Orders[Product Category])
     )
     ```
   · Press Enter.

---

Part 4: Time Intelligence Functions

Objective: Create a date table and use time intelligence functions.

Lesson 4.1: Create a Date Table

Action Items:

1. Create Table with Calendar Auto:
   · In the Fields pane, select the Orders table.
   · On the Table tools tab, click New table.
   · Rename the table to Dates and enter the formula: CALENDARAUTO().
   · Press Enter.
2. Add Columns to Date Table:
   · Edit the formula to nest CALENDARAUTO() within an ADDCOLUMNS function to create Year, Quarter, Month, and Month Number columns.
     ```
     Dates =
     ADDCOLUMNS(
         CALENDARAUTO(),
         "Year", YEAR([Date]),
         "Quarter", "Q" & QUARTER([Date]),
         "Month", FORMAT([Date], "mmmm"),
         "Month Number", MONTH([Date])
     )
     ```
   · Press Enter.
3. Mark as Date Table:
   · In the Fields pane, right-click the Dates table.
   · Hover over Mark as date table and click Mark as date table.
   · In the dialog, select the Date column and click OK.
4. Create Relationship:
   · Switch to Model View.
   · Drag Order Date from the Orders table and drop it on the Date column in the Dates table to create a relationship.

Lesson 4.2: Resolve Date Errors & Add Time Intelligence Columns

Action Items:

1. Fix Days to Ship Column:
   · Switch to Data View.
   · In the formula bar for the Days to Ship column, remove the .Date qualifier from the Order Date and Ship Date references (e.g., change Orders[Order Date].Date to Orders[Order Date]). Press Enter to recalculate.
2. Add End of Month and Quarter Columns:
   · Right-click the Orders table and select New column.
   · Name the column End of Month and use the formula: ENDOFMONTH(Orders[Order Date]).
   · Right-click Orders again and select New column.
   · Name the column End of Quarter and use the formula: ENDOFQUARTER(Orders[Order Date]).
   · Format both new columns as Date (e.g., March 14, 2001) using the Column tools tab.

---

Part 5: Key Performance Indicators (KPIs)

Objective: Create a KPI visualization.

Action Items:

1. Open File: Close the Sample Superstore file and open the Retail Analysis Sample desktop file.
2. Create a Base Visualization:
   · Go to Report View and create a new page.
   · From the Sales table, drag Total Units This Year onto the canvas.
   · From the Time table, drag Fiscal Month onto the canvas.
3. Prepare and Sort Data:
   · In the chart's more options (...), go to Sort by and select Fiscal Month. Then sort Ascending.
   · Resize the chart as needed.
4. Convert to KPI:
   · With the chart selected, in the Visualizations pane, click the KPI icon.
   · From the Sales table, drag Total Units Last Year into the Target Goals box.
5. Finalize:
   · Rename the page from "Page 1" to KPI.

---

Module 6: Optimizing Model Performance

Objective: Learn optimization techniques like DirectQuery and using variables.

Part 1: Using DirectQuery

Key Concept: DirectQuery connects directly to the data source instead of importing data, reducing file size and ensuring real-time data.

Action Items:

1. Publish Dataset to Service:
   · In the Retail Analysis Sample file, click Publish on the Home tab. Choose a workspace (e.g., "My Workspace").
   · Click the link in the success dialog to see it in the Power BI service.
2. Create a New File Using DirectQuery:
   · In Power BI Desktop, go to File -> New to start a new file.
   · On the Home tab, click Power BI datasets.
   · Select the Retail Analysis Sample dataset and click Create.
   · Save this new file as Direct Query.
3. Observe Differences:
   · Note that Data View is missing because no data was imported. The file size of Direct Query will be much smaller than the original Retail Analysis Sample file.
   · Recreate the KPI from the previous module (Total Units This Year by Fiscal Month, sorted, then converted to KPI with Total Units Last Year as the target).

---

Part 2: Using Variables in DAX

Key Concept: Variables (VAR) store the result of an expression to improve performance, readability, and ease of debugging.

Action Items:

1. Open File: Launch the Sample Superstore desktop file.
2. Create a Base Measure:
   · Right-click the Orders table and select New measure. Name it Total Sales.
   · Enter the formula: Total Sales = CALCULATE(SUMX(Orders, Orders[Sales])).
3. Create a Measure with Variables:
   · Right-click Orders and select New measure. Name it 2010 Total Corporate Sales.
   · Enter the following formula (use Shift+Enter for new lines):
     ```
     2010 Total Corporate Sales =
     VAR CorporateSales =            // Declare variable for Corporate segment
         FILTER(
             ALL(Orders[Customer Segment]),
             Orders[Customer Segment] = "Corporate"
         )
     VAR IncludedDates =             // Declare variable for year 2010
         FILTER(
             ALL(Dates[Year]),
             Dates[Year] = 2010
         )
     RETURN
         CALCULATE(
             [Total Sales],          // Use the measure created in step 2
             CorporateSales,
             IncludedDates
         )                           // This calculation is only for 2010 Corporate sales
     ```
   · Press Enter.
4. Duplicate for Other Years:
   · Right-click Orders and select New measure.
   · Paste the formula (Ctrl+V) and rename it to 2011 Total Corporate Sales. Change the IncludedDates variable filter to = 2011 and update the comment.
   · Repeat for 2012 and 2013.
5. Display Results:
   · In Report View, select the Multi-row card visualization.
   · Drag the four new measures (2010 Total Corporate Sales, etc.) into the card.

---

Part 3: Other Optimization Techniques

· Apply restrictive filters to visuals.
· Limit the number of visuals on a single report page.
· Evaluate custom visuals for performance.
· Optimize the environment by managing capacity settings, gateway sizing, and network latency (typically handled by IT).
