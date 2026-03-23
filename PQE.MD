Lesson 1: Data Shaping

Objective: Learn to transform data by renaming, removing rows/columns, and promoting headers.

· Action Items for the "Users" Table:
  1. Rename Table: In the Fields pane, right-click the "Users" table, select Rename, and change it to "Managers".
  2. Open Power Query Editor: Go to the Home tab and click Transform Data.
  3. Promote Headers: Select the "Managers" query. In the Transform group, click Use First Row as Headers. This replaces "Column1" and "Column2" with "Region" and "Manager".
  4. Duplicate Column: Right-click the "Manager" column heading and choose Duplicate Column.
  5. Rename Columns: Double-click the original "Manager" column and rename it to "Manager First". Double-click the copy and rename it to "Manager Last".
  6. Replace Values: Right-click each cell in the "Manager Last" column, select Replace Values, and enter the last names:
     · Chris → Evans
     · Aaron → Rogers
     · Sam → Smith
     · William → Jackson
· Action Items for the "Orders" Table:
  1. Remove Columns: In the "Orders" query, go to the Home tab, click Choose Columns. Uncheck the following columns: Row ID, Order Priority, Product Base, Margin, and Quantity Ordered New. Click OK.
  2. Filter Data: Click the auto-filter arrow on the "State or Province" column. If the list is incomplete, click Load More. Uncheck Select All, then check only Arizona, California, Florida, New Jersey, and New York. Click OK.
  3. Rename Column: Double-click the "State or Province" heading and rename it to "State".
  4. Sort Data: Click the auto-filter arrow on the "State" column and select Sort Ascending.
· Final Step:
  1. Apply Changes: In the Home tab, click Close & Apply to load the changes into the Power BI data model.
  2. Save: Save the Power BI Desktop file.

Lesson 2: Enhancing the Data Model (Merging Queries)

Objective: Combine data from the Managers table into the Orders table using a merge operation.

· Action Items for Merging Managers into Orders:
  1. Open Power Query Editor: Click Transform Data on the Home tab.
  2. Perform Merge: Go to the Home tab, in the Combine group, click Merge Queries and select Merge Queries as New.
  3. Configure Merge:
     · Ensure "Orders" is the top table. Select the "Region" column.
     · Select "Managers" as the second table. Select its "Region" column.
     · Change the "Join Kind" dropdown from Left Outer to Right Outer.
     · Click OK.
  4. Expand New Table: In the new "Merge1" query, click the expand button (double arrow) on the "Managers" column. Uncheck Use original column name as prefix (or leave it, but ensure all columns are selected). Click OK.
  5. Rename Columns: Rename the new columns:
     · "Managers.manager first" → "Manager First"
     · "Managers.manager last" → "Manager Last"
  6. Filter and Sort:
     · Click the auto-filter on the "Managers.region" column, uncheck "Central" (to remove null records), and click OK.
     · Sort the "Managers.region" column in ascending order.
  7. Rename Query: In the Query Settings pane, rename "Merge1" to "Orders by Region".
  8. Apply and Save: Click Close & Apply and save the file.
· Practice Task:
  · Objective: Merge the Orders and Returns tables.
  · Action: Create a new query that merges these two tables. Include the return status in the merged table and filter out "No" values for a cleaner dataset.

Lesson 3: Data Profiling

Objective: Use Power Query Editor's profiling tools to assess data quality.

· Action Items:
  1. Open Power Query Editor: Click Transform Data.
  2. Enable Profiling Tools: Go to the View tab in the ribbon. In the Data Preview group, check the boxes:
     · Column Quality: Shows the percentage of valid, error, and empty values in each column.
     · Column Distribution: Shows the count of distinct and unique values in each column.
     · Column Profile: Opens a pane at the bottom to display detailed statistics and value distribution for the selected column.
  3. Exit: Click the Close & Apply dropdown and select Close (to exit without applying the profiling view changes).

---

Module 4: Data Modeling

This module covers designing a data model for performance and scalability, including creating dimensions, relationships, and row-level security.

Lesson 1: Working with Tables

Objective: Learn to format data and categorize fields for visualizations.

· Action Items:
  1. Format Discount as Percentage:
     · Go to Data View.
     · Select the "Discount" column in the Orders table.
     · On the Column Tools tab, click the % icon and set decimal places to 0.
  2. Format Columns as Currency:
     · Select the "Unit Price" column. Click the $ icon.
     · Select the "Shipping Cost" column. Click the $ icon.
  3. Format Date Columns:
     · Select "Order Date". In the Formatting group, use the dropdown to select March 14, 2001.
     · Select "Ship Date". Apply the same format.
  4. Categorize Location Fields (for Maps):
     · Select "Region". In the Properties group, set Data Category to Region.
     · Select "State". Set Data Category to State or Province.
     · Select "City". Set Data Category to City.
     · Select "Postal Code". Set Data Category to Postal Code.

Lesson 2: Dimensions and Hierarchies

Objective: Break down a large table into dimensions and create a hierarchy.

· Action Items for Creating Dimension Tables:
  1. Create DimCustomers:
     · Open Power Query Editor. Right-click "Orders" and select Copy. Right-click in the pane and select Paste.
     · With the pasted query selected, click Choose Columns. Uncheck Select All and select only: Customer ID, Customer Name, Customer Segment, Region, State, City, Postal Code, Discount. Click OK.
     · Drag "Customer ID" to be the first column.
     · Rename the query to "DimCustomers".
  2. Create DimProducts:
     · Repeat the copy/paste process for "Orders".
     · Use Choose Columns to select only the four columns starting with "Product": Product Category, Product Sub-Category, Product Name, Product Container.
     · Create Key Field:
       · Go to the Add Column tab.
       · Click Index Column > Custom. Set start to 1 and increment to 1. Click OK.
       · Drag the new index column to be the first column.
     · Rename the query to "DimProducts".
  3. Apply and Rename: Click Close & Apply. In the Fields pane, right-click the index column in DimProducts and rename it to "Product ID". Save.
· Action Items for Creating a Hierarchy:
  1. Create Hierarchy: In the Fields pane under the Orders table, right-click "Region" and select Create hierarchy.
  2. Add Fields:
     · Right-click "State", hover over Add to hierarchy, and select Region hierarchy.
     · Repeat for "City" and "Postal Code`. This creates a drill-down structure (Region > State > City > Postal Code).
  3. Save the file.

Lesson 3: Creating Model Relationships

Objective: Learn about relationships (cardinality) and use Model View to manage them.

· Action Items:
  1. Review Relationship Settings: Go to File > Options and Settings > Options > Current File > Data Load. Ensure relationship detection settings are configured.
  2. Navigate to Model View: Click the Model view icon.
  3. Hide Unused Tables: Click the eye icon on the "DimCustomers", "DimProducts", "Orders by Region", and "Returned Orders" tables to hide them from Report view.
  4. Create a New Relationship:
     · From the "Orders" table, click and drag the "Order ID" field and drop it onto the "Order ID" field in the "Returns" table.
     · Power BI automatically creates the relationship (default is one-to-many).

Lesson 4: Row Level Security (RLS)

Objective: Restrict data access for specific users by creating roles and defining filters.

· Action Items:
  1. Create Roles in Desktop:
     · Go to the Modeling tab and click Manage Roles.
     · Click Create. Name the role "East".
     · Select the "Orders" table. In the filter DAX expression box, enter: [Region] = "East"
     · Click Create again. Name the role "West". Enter: [Region] = "West"
     · Click Save.
  2. Test Roles in Desktop:
     · On the Modeling tab, click View as.
     · Check the "East" role and click OK. The yellow bar confirms you are viewing as that role.
     · Go to Data View and click the Orders table. Only East region data should appear.
     · Click Stop Viewing in the yellow bar.
  3. Publish to Service:
     · Click Publish on the Home tab. Select your workspace (e.g., "My Workspace") and click Select.
     · After publishing, click the link to open in the Power BI service.
  4. Assign Users in Service:
     · In the Power BI service, go to your workspace.
     · Hover over the dataset, click the More options (vertical ellipsis), and select Security.
     · For the "East" role, add an email address and click Add, then Save.
     · Repeat for the "West" role.
