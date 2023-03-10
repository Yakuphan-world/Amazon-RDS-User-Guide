# Analyzing Oracle execution plans using the Performance Insights dashboard<a name="USER_PerfInsights.UsingDashboard.AccessPlans"></a>

When analyzing DB load on an Oracle Database, you might want to know which plans are contributing the most to DB load\. For example, the top SQL statements at a given time might be using the plans shown in the following table\.


****  

| Top SQL | Plan | 
| --- | --- | 
|  SELECT SUM\(amount\_sold\) FROM sales WHERE prod\_id = 10  |  Plan A  | 
|  SELECT SUM\(amount\_sold\) FROM sales WHERE prod\_id = 521  |  Plan B  | 
|  SELECT SUM\(s\_total\) FROM sales WHERE region = 10  |  Plan A  | 
|  SELECT \* FROM emp WHERE emp\_id = 1000  |  Plan C  | 
|  SELECT SUM\(amount\_sold\) FROM sales WHERE prod\_id = 72  |  Plan A  | 

With the plan feature of Performance Insights, you can do the following:
+ Find out which plans are used by the top SQL queries\. 

  For example, you might find out that most of the DB load is generated by queries using plan A and plan B, with only a small percentage using plan C\.
+ Compare different plans for the same query\. 

  In the preceding example, three queries are identical except for the product ID\. Two queries use plan A, but one query uses plan B\. To see the difference in the two plans, you can use Performance Insights\.
+ Find out when a query switched to a new plan\. 

  You might see that a query used plan A and then switched to plan B at a certain time\. Was there a change in the database at this point? For example, if a table is empty, the optimizer might choose a full table scan\. If the table is loaded with a million rows, the optimizer might switch to an index range scan\.
+ Drill down to the specific steps of a plan with the highest cost\.

  For example, the for a long\-running query might show a missing a join condition in an equijoin\. This missing condition forces a Cartesian join, which joins all rows of two tables\.

You can perform the preceding tasks by using the plan capture feature of Performance Insights\. Just as you can slice Oracle queries by wait events and top SQL, you can slice them by the plan dimension\.

**To analyze Oracle execution plans using the console**

1. Open the Amazon RDS console at [https://console\.aws\.amazon\.com/rds/](https://console.aws.amazon.com/rds/)\.

1. In the navigation pane, choose **Performance Insights**\.

1. Choose an Oracle DB instance\. The Performance Insights dashboard is displayed for that DB instance\.

1. In the **Database load \(DB load\)** section, choose **Plans** next to **Slice by**\.

   The Average active sessions chart shows the plans used by your top SQL statements\. The plan hash values appear to the right of the color\-coded squares\. Each hash value uniquely identifies a plan\.  
![\[Slice by plans\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/./images/pi-slice-by-plans.png)

1. Scroll down to the **Top SQL** tab\.

   In the following example, the top SQL digest has two plans\. You can tell that it's a digest by the question mark in the statement\.   
![\[Choose a digest plan\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/./images/top-sql-plans-unselected.png)

1. Choose the digest to expand it into its component statements\.

   In the following example, the `SELECT` statement is a digest query\. The component queries in the digest use two different plans\. The colors of the plans correspond to the database load chart\. The total number of plans in the digest is shown in the second column\.  
![\[Choose a digest plan\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/./images/pi-digest-plan.png)

1. Scroll down and choose two **Plans** to compare from **Plans for digest query** list\.

   You can view either one or two plans for a query at a time\. The following screenshot compares the two plans in the digest, with hash 2032253151 and hash 1117438016\. In the following example, 62% of the average active sessions running this digest query are using the plan on the left, whereas 38% are using the plan on the right\.  
![\[Compare the plans side by side\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/./images/pi-compare-plan.png)

   In this example, the plans differ in an important way\. Step 2 in plan 2032253151 uses an index scan, whereas plan 1117438016 uses a full table scan\. For a table with a large number of rows, a query of a single row is almost always faster with an index scan\.  
![\[Compare the plans side by side\]](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/./images/pi-table-access.png)

1. \(Optional\) Choose **Copy** to copy the plan to the clipboard, or **Download** to save the plan to your hard drive\. 