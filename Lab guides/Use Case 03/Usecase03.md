# Use Case 3 - Building Dataflows in Microsoft Fabric with Snowflake 

## Introduction

In this lab, you will learn how to create a Dataflow (Gen2) in Microsoft
Fabric. Dataflows (Gen2) are powerful tools that allow you to connect to
various data sources and perform transformations using Power Query
Online.

These dataflows can be integrated into Data Pipelines for data ingestion
into a Lakehouse or other analytical stores, or to define datasets for
Power BI reports. This exercise is designed to introduce you to the
essential elements of Dataflows (Gen2).

This lab will guide you through the process of connecting to a Snowflake
data warehouse using Power Query Desktop and Power Query Online. By the
end of this lab, you will be able to import data from Snowflake,
transform it using Power Query, and load it into Power BI for analysis
and visualization. This exercise will help you understand how to
integrate Snowflake with Power BI to leverage powerful data analytics
capabilities.

You will learn how to create a data pipeline in Microsoft Fabric's Data
Factory and copy data from a sample dataset into a Lakehouse. By the end
of this lab, you will have successfully configured and executed a data
pipeline that moves data from Snowflake to the Lakehouse, leveraging
various Snowflake features and options.

**Objective**

The objective of this lab is to:

1.  Create a workspace and a lakehouse in Microsoft Fabric.

2.  Create a Dataflow (Gen2) to ingest and transform data.

3.  Add a Dataflow (Gen2) to a pipeline to automate data ingestion and
    processing.

4.  Explore and verify the ingested data in the lakehouse.

5.  Connect to a Snowflake data warehouse from Power Query Desktop.

6.  Sign in and authenticate your Snowflake credentials.

7.  Import and transform data from Snowflake in Power BI Desktop.

8.  Explore and manipulate the imported data using Power Query Editor.

9.  Create and configure a data pipeline.

10. Set up a source connection to Snowflake.

11. Define the destination as a Lakehouse.

12. Execute the copy activity and validate the data transfer.

**Prerequisites**

Before you begin this lab, ensure that you have:

1.  Microsoft Fabric Tenant Account: You need an active subscription.
    [Create a free
    account](https://www.microsoft.com/microsoft-365/microsoft-fabric).

2.  Microsoft Fabric Enabled Workspace: Make sure you have a workspace
    with Microsoft Fabric enabled. [Create a
    workspace](https://docs.microsoft.com/microsoft-fabric/workspace).

3.  Downloaded and installed the OneLake file explorer.

4.  Access to the WideWorldImportersDW dataset or any suitable CSV data
    for the exercise.

5.  Power BI Desktop installed on your computer.

6.  Access to a Snowflake data warehouse, including the server name and
    warehouse name.

7.  Valid Snowflake credentials (username and password) to authenticate
    the connection.

# Exercise 1: Create a Dataflow (Gen2) in Microsoft Fabric

## Task 1: Create a workspace

1.  Log in to your Microsoft Fabric account. On the Microsoft Fabric
    **home page**, select **Synapse Data Engineering**.

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  In the menu bar on the left, select **Workspaces**. Then select **+
    new workspace.**

![A screenshot of a computer Description automatically
generated](./media/image2.png)

3.  Name workspace as **DataflowWorkspace_Tutorial** or with a name of
    your choice, select a licensing mode that includes Fabric capacity
    (Trial, Premium, or Fabric). Then select **Apply**.

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  Your new workspace is ready.

![A screenshot of a computer Description automatically
generated](./media/image4.png)

## Task 2: Create a Lakehouse

Now that you have a workspace, it’s time to create a data lakehouse into
which you’ll ingest data.

1.  In the menu bar on the left, select **Create**. create a
    new **Lakehouse** with a name of your choice.

![A screenshot of a computer Description automatically
generated](./media/image5.png)

2.  On the **New** page, select **Lakehouse** under Data Engineering
    section.

![A screenshot of a computer Description automatically
generated](./media/image6.png)

3.  Name your Lakehouse as **LakehouseTutorial** and select the
    **Create** button.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

4.  Your new Lakehouse is ready.

![A screenshot of a computer Description automatically
generated](./media/image8.png)

## Task 3: Create a Dataflow (Gen2) to ingest data

Now that you have a lakehouse, you need to ingest some data into it. One
way to do this is to define a dataflow that encapsulates an *extract,
transform, and load* (ETL) process.

1.  In the menu bar on the left, select your Lakehouse,
    **LakehouseTutorial.**

> ![](./media/image9.png)

2.  In the home page for your workspace, select **+ New** dropdown.

> ![A screenshot of a computer Description automatically
> generated](./media/image10.png)

3.  From the dropdown menu, select **New Dataflow Gen2**.

![A screenshot of a computer Description automatically
generated](./media/image11.png)

4.  This will open the Power Query editor for your new dataflow.

![A screenshot of a computer Description automatically
generated](./media/image12.png)

5.  In the Power Query editor, Select **Import from a Text/CSV file**.

![A screenshot of a computer Description automatically
generated](./media/image13.png)

6.  Create a new data source with the following settings and
    Select **Next** to preview the file data.

- **Link to file**: *Selected*

- **File path or
  URL**: https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/orders.csv

- **Connection**: Create new connection

- **data gateway**: (none)

- **Authentication kind**: Anonymous

Then select, **Next**.

> ![A screenshot of a computer Description automatically
> generated](./media/image14.png)

7.  Select **Create** and the **Publish** the data source. The Power
    Query editor shows the data source and an initial set of query steps
    to format the data, as shown here:

![A screenshot of a computer Description automatically
generated](./media/image15.png)

![](./media/image16.png)

8.  On the toolbar ribbon, select the **Add column** tab.

![A screenshot of a computer Description automatically
generated](./media/image17.png)

9.  Then select **Custom column** to create a new column.

![A screenshot of a computer Description automatically
generated](./media/image18.png)

10. Set the **New column name** to MonthNo , select the **Data type**
    dropdown and set the **Data type** to **Whole Number** and then add
    the following formula: Date.Month(\[OrderDate\]).

![A screenshot of a computer Description automatically
generated](./media/image19.png)

11. Select **OK** to create the column and notice how the step to add
    the custom column is added to the query. The resulting column is
    displayed in the data pane:

![](./media/image20.png)

Check and confirm that the data type for the OrderDate column is set to
Date and the data type for the newly created column MonthNo is set to
Whole Number.

**Tip:** In the Query Settings pane on the right side, notice
the **Applied Steps** include each transformation step. At the bottom,
you can also toggle the **Diagram flow** button to turn on the Visual
Diagram of the steps.

Steps can be moved up or down, edited by selecting the gear icon, and
you can select each step to see the transformations apply in the preview
pane.

## Task 4: Add data destination for Dataflow

1.  On the toolbar ribbon, select the **Home** tab.

![](./media/image21.png)

2.  Then in the **Add data destination** drop-down menu,
    select **Lakehouse**.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

> **Note:** If this option is grayed out, you may already have a data
> destination set. Check the data destination at the bottom of the Query
> settings pane on the right side of the Power Query editor. If a
> destination is already set, you can change it using the gear.

3.  In the **Connect to data destination** dialog box, set the
    connections as **Create new connection**, and add the
    **Authentication kind** as **Organizational account.**

> Next, **Sign in** using your Power BI organizational account to set
> the identity that the dataflow uses to access the Lakehouse.
> Select **Next** .

![A screenshot of a computer Description automatically
generated](./media/image23.png)

4.  In the list of available workspaces, find your workspace and select
    the Lakehouse you created in it at the start of this exercise. Then
    specify a new table named **orders**:

![A screenshot of a computer Description automatically
generated](./media/image24.png)

5.  On the **Choose destination settings** page, select **Append** and
    then **Save settings**.

![A screenshot of a computer Description automatically
generated](./media/image25.png)

**Note:** We suggest using the *Power query* editor for updating data
types, but you can also do so from this page, if you prefer.

6.  On the Menu bar, open **View** and select **Diagram view**. Notice
    the **Lakehouse** destination is indicated as an icon in the query
    in the Power Query editor.

![A screenshot of a computer Description automatically
generated](./media/image26.png)

![A screenshot of a computer Description automatically
generated](./media/image27.png)

7.  Select **Publish** to publish the dataflow. Then wait for
    the **Dataflow 1** dataflow to be created in your workspace.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

8.  Once published, you can click on the ellipsis **(…)** next to the
    dataflow in your workspace.

![A screenshot of a computer Description automatically
generated](./media/image29.png)

9.  Select **Properties**.

![A screenshot of a computer Description automatically
generated](./media/image30.png)

10. Rename your dataflow as **Dataflow_Connection** and click **Save**.

![A screenshot of a computer Description automatically
generated](./media/image31.png)

11. Your dataflow is now renamed.

![A screenshot of a computer Description automatically
generated](./media/image32.png)

## Task 5: Add a dataflow to a pipeline

You can include a dataflow as an activity in a pipeline. Pipelines are
used to orchestrate data ingestion and processing activities, enabling
you to combine dataflows with other kinds of operation in a single,
scheduled process. Pipelines can be created in a few different
experiences, including Data Factory experience.

1.  From your Fabric-enabled workspace, make sure you’re still in
    the **Data Engineering** experience. Select **New** dropdown.

![](./media/image33.png)

2.  From the dropdown menu, select **Data pipeline.**

> ![](./media/image34.png)

3.  Then when prompted, create a new pipeline named **Load data** and
    click **Create**.

> ![](./media/image35.png)

4.  The pipeline editor opens.

![](./media/image36.png)

**Tip**: If the Copy Data wizard opens automatically, close it!

5.  Select **Add pipeline activity** and add a **Dataflow** activity to
    the pipeline.

![A screenshot of a computer Description automatically
generated](./media/image37.png)

6.  With the new **Dataflow** activity selected, on
    the **Settings** tab, in the **Workspace** dropdown, select your
    workspace **DataflowWorkspace_Tutorial.** Then,
    in **Dataflow** drop-down list, select **Dataflow_Connection** (the
    data flow you created previously).

> ![A screenshot of a computer Description automatically
> generated](./media/image38.png)

7.  On the **Home** tab, save the pipeline using the **🖫** (*Save*)
    icon.

![A screenshot of a computer Description automatically
generated](./media/image39.png)

8.  Use the **▷ Run** button to run the pipeline, and wait for it to
    complete. It may take a few minutes.

![A screenshot of a computer Description automatically
generated](./media/image40.png)

![A screenshot of a computer Description automatically
generated](./media/image41.png)

9.  In the menu bar on the left edge, select your Lakehouse.

![A screenshot of a computer Description automatically
generated](./media/image42.png)

10. In the **…** menu for **Tables**, select **refresh**.

> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

11. Then expand **Tables** and select the **orders** table, which has
    been created by your dataflow.

![](./media/image44.png)

**Tip**: Use the Power BI Desktop *Dataflows connector* to connect
directly to the data transformations done with your dataflow.

You can also make additional transformations, publish as a new dataset,
and distribute with intended audience for specialized datasets.

To get data in Data Factory:

1.  On the left side of Data Factory, select **Workspaces** (but
    not **My Workspace**).

2.  From your Data Factory workspace, select **New** \> **Dataflow Gen2
    (Preview)** to create a new dataflow.

![Screenshot showing the workspace where you choose to create a new
dataflow.](./media/image45.png)

3.  In Power Query, either select **Get data** in the ribbon or
    select **Get data from another source** in the current view.

4.  In the **Choose data source** page, use **Search** to search for the
    name of the connector, or select **View more** on the right hand
    side the connector to see a list of all the connectors available in
    Power BI service.

![Screenshot of the Data Factory Choose data source page with the search
box and the view more selection emphasized.](./media/image46.png)

5.  If you choose to view more connectors, you can still
    use **Search** to search for the name of the connector, or choose a
    category to see a list of connectors associated with that category.

![Screenshot of the Data Factory Choose data source page displayed after
selecting view more, including the categories at the top and then the
list of connectors.](./media/image47.png)

# Exercise 2: Create Snowflake trial account

## Task 1: Create a Snowflake account using your Microsoft email account.

1.  Fill in the required details and select **Continue**.

Note: A [free
trial](https://signup.snowflake.com/?utm_cta=quickstarts_&_fsi=NfWsJmXg&_fsi=NfWsJmXg&_fsi=NfWsJmXg&_fsi=NfWsJmXg&_fsi=NfWsJmXg&_fsi=NfWsJmXg) will
suffice. [Standard
Edition](https://docs.snowflake.com/en/user-guide/intro-editions#standard-edition) will
work for most of this lab, but if you'd like to try governance features
covered in section 4, you will
need [Enterprise](https://docs.snowflake.com/en/user-guide/intro-editions#enterprise-edition) or [Business
Critical
Edition](https://docs.snowflake.com/en/user-guide/intro-editions#business-critical-edition).

![A screenshot of a blue screen Description automatically
generated](./media/image48.png)

2.  Once you have logged in you will receive the Snowflake credentials
    in the email.

![A screenshot of a computer Description automatically
generated](./media/image49.png)

3.  Using the credential, log into your Snowflake account.

> ![A screenshot of a login form Description automatically
> generated](./media/image50.png)

![A blue and white website with text Description automatically
generated](./media/image51.png)

4.  On the Homepage, select **My profile**.

![A screenshot of a computer Description automatically
generated](./media/image52.png)

5.  Here, you will find the Warehouse name in **Default role &
    warehouse** section. Note down the warehouse name to establish
    connect later.

![A screenshot of a computer Description automatically
generated](./media/image53.png)

# Exercise 3: Make the connection to a Snowflake.

## Task 1: Make the connection to a Snowflake computing warehouse in Power BI Desktop app

1.  Select **Get Data** from the **Home** ribbon in Power BI Desktop and
    Select **More**.

![A screenshot of a computer Description automatically
generated](./media/image54.png)

2.  Select **Database** from the categories on the left,
    select **Snowflake**, and then select **Connect**.

![A screenshot of a computer Description automatically
generated](./media/image55.png)

3.  In the **Snowflake** window that appears, enter the name of your
    Snowflake server in **Server** (you can copy this from URL from the
    snowflake credentials email that you received) and the name of your
    Snowflake computing warehouse in **Warehouse**. Then select **OK.**

Optionally, enter values in any advanced options that you want to use to
modify the connection query, such as a text value to use as a Role name
or a command timeout

![A screenshot of a computer Description automatically
generated](./media/image56.png)

4.  To sign in to your Snowflake computing warehouse, enter your
    username and password, and then select **Connect**.

![A screenshot of a login box Description automatically
generated](./media/image57.png)

**Note:** Once you enter your username and password for a
particular **Snowflake** server, Power BI Desktop uses those same
credentials in subsequent connection attempts. You can modify those
credentials by going to **File \> Options and settings \> Data source
settings**.

If you want to use the Microsoft account option, the Snowflake Microsoft
Entra ID integration must be configured on the Snowflake side.

5.  In **Navigator**, select one or multiple elements to import and use
    in Power BI Desktop. Then select either **Load** to load the table
    in Power BI Desktop, then load that refined set of data into Power
    BI Desktop.

![A screenshot of a computer Description automatically
generated](./media/image58.png)

6.  Select **Import** to import data directly into Power BI, then
    select **OK**.

![A screenshot of a computer Description automatically
generated](./media/image59.png)

7.  Your Snowflake data is now loaded into the Power BI Desktop. You can
    now utilize the **Visualization** panel to drag and drop the visuals
    for your data.

![A screenshot of a computer Description automatically
generated](./media/image60.png)

# Exercise 4: Creating and Configuring a Data Pipeline

## Task 1: Navigate to Power BI and Data Factory

1.  Open Power BI: Navigate to the **Power BI portal**.

![A screenshot of a computer Description automatically
generated](./media/image61.png)

2.  Access Data Factory: Select the Power BI icon in the bottom left
    corner, then select "**Data Factory**" to open the Data Factory
    homepage.

![A screenshot of a computer Description automatically
generated](./media/image62.png)

**Step 2: Create a Data Pipeline**

1.  Go to Your Workspace: Navigate to the **DataEngineeringWorkspace**
    in Microsoft Fabric.

![A screenshot of a computer Description automatically
generated](./media/image63.png)

2.  Create New Pipeline: Select "Data pipeline" and enter
    **SampleDataToLakehousePipeline** as the name for your new pipeline.

![](./media/image64.png)

![A screenshot of a computer Description automatically
generated](./media/image65.png)

![A screenshot of a computer Description automatically
generated](./media/image66.png)

## Task 2: Configure Your Source

1.  Choose Sample Data: In the data source browser, select **Copy data
    assistant.**

![A screenshot of a computer Description automatically
generated](./media/image67.png)

2.  "Sample data" tab, then choose the "Public Holidays" sample data.

![A screenshot of a computer Description automatically
generated](./media/image68.png)

3.  Review the preview of the data from Public_Holidays and proceed by
    clicking "Next".

![A screenshot of a computer Description automatically
generated](./media/image69.png)

## Task 3: Configure Your Destination

1.  Set the destination to "**Lakehouse**".

![A screenshot of a computer Description automatically
generated](./media/image70.png)

2.  Create Lakehouse: Enter PublicHolidaysLakehouse as the Lakehouse
    name and select "Create and connect".

![A screenshot of a computer Description automatically
generated](./media/image71.png)

3.  Map Data: Configure the mapping of source data to the destination
    Lakehouse table. Choose "Tables" for the root folder and "Load to a
    new table" for load settings. Provide PublicHolidaysTable as the
    table name and click "Next".

![](./media/image72.png)

![A screenshot of a computer Description automatically
generated](./media/image73.png)

## Task 4: Review and Create Your Copy Activity

1.  Check all settings configured in the previous steps. Select
    "**Save + run**" to execute the pipeline.

![A screenshot of a computer Description automatically
generated](./media/image74.png)

![A screenshot of a computer Description automatically
generated](./media/image75.png)

2.  The Copy activity is added to your new data pipeline canvas. All
    settings including advanced settings for the activity are available
    in the tabs below the pipeline canvas when the created **Copy
    data** activity is selected.

![A screenshot of a computer Description automatically
generated](./media/image76.png)

## Summary 

By following these steps, you have learned how to create and configure a
workspace and lakehouse, design a Dataflow (Gen2) for data ingestion and
transformation, and integrate it into a data pipeline for automated data
processing. Additionally, you have explored how to connect to Snowflake
from Power Query Desktop, import and transform data, and load it into
Power BI for analysis.

You also learned how to set up a data pipeline in Microsoft Fabric's
Data Factory, which involved copying data from Snowflake into a
Lakehouse, configuring the pipeline's copy activity, and verifying the
data transfer. By completing these tasks, you have developed an
understanding of the entire data ingestion, transformation, and analysis
process using Microsoft Fabric and Snowflake.

.
