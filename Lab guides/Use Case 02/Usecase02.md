## Use Case 2 - Building a Data Warehouse using pipelines

## Introduction

In this lab, you will start by rapidly generating real-time data and
understanding how that data can be processed and visualized in Microsoft
Fabric. With the initial reporting in place, multiple modules are
available that explore data warehousing, data lakehouse architecture,
data activator, data science, and of course, real-time analytics. The
modules are designed to be cohesive but flexible -- they all involve the
same core scenario but have limited dependencies so you can consume the
modules that make the most sense for you.

The basic architecture of the solution is illustrated below. The app
deployed in the beginning of this lab (either as a docker container or
running in Jupyter notebook) will publish events to our Fabric
environment. The data is ingested into a KQL database for real-time
reporting in Power BI.

In this lab, you’ll get hands-on with a fictitious financial company
"AbboCost." AbboCost would like to set up a stock monitoring platform to
monitor price fluctuations and report on historical data. Throughout the
workshop, we'll look at how every aspect of Microsoft Fabric can be
incorporated as part of a larger solution -- by having everything in an
integrated solution, you'll be able to quickly and securely integrate
data, build reports, create data warehouses and lakehouses, forecast
using ML models, and more.

![Data Lakehouse with Azure Synapse Analytics](./media/image1.png)

Now that our data is streaming into our KQL database, we can begin to
query and explore the data, leveraging KQL to gain insights into the
data. A KQL queryset is used to run queries, view, and transform data
from a KQL database. Like other artifacts, a KQL queryset exists within
the context of a workspace. A queryset can contain multiple queries each
stored in a tab. In this exercise, we'll create several KQL queries of
increasing complexity to support different business uses.

You will build a Synapse Data Warehouse inside Microsoft Fabric to
aggregate data from the KQL database. In Microsoft Fabric, there are two
primary ways to build a data warehouse: using a Synapse Data Warehouse,
the focus of this module, and a lakehouse.

A Synapse Data Warehouse stores its data in OneLake in Delta/Parquet
format similar to lakehouse tables. However, only Synapse Data Warehouse
offers read/write on the T-SQL endpoint. If you are migrating a data
warehouse or more familiar with T-SQL development, using a Synapse Data
Warehouse is a logical choice.

# Objectives

- To sign up for the free Microsoft Fabric trial, redeem Azure Pass, and
  configure necessary permissions within the Azure portal.

- To create fabric capacity and workspace, storage account, and fabric
  workspace.

- To deploy the stock generator app via Azure Container Instance using
  an ARM template.

- To configure Eventstream in Microsoft Fabric for ingesting real-time
  data from Azure Event Hubs, ensuring seamless integration and data
  preview for subsequent analysis.

- To create a KQL database within Microsoft Fabric and send data from
  Eventstream to the

- KQL database.

- To explore stock price data using KQL, progressively developing
  queries to analyze trends, calculate price differentials, and
  visualize data for actionable insights.

- To leverage Power BI to create dynamic, real-time reports based on
  analyzed stock data, configuring auto-refresh settings for timely
  updates and enhancing visualization for informed decision-making.

- Create a Synapse Data Warehouse within the Fabric workspace and create
  essential staging and ETL objects to facilitate data processing and
  transformation.

- Build a data pipeline for efficiently extracting, transforming, and
  loading (ETL) data from source systems into the Synapse Data
  Warehouse, ensuring data accuracy and consistency.

- Create dimension and fact tables within the data warehouse to organize
  and store structured data efficiently for analytical purposes.

- Implement procedures to incrementally load data into the data
  warehouse, ensuring efficient handling of large datasets while
  maintaining data integrity.

- Create views to support data aggregation during the ETL process,
  optimizing data processing and improving pipeline performance.

- Create semantic model in Synapse Data Warehouse, define table
  relationships, and generate a Power BI report for data visualization.

# Exercise 1: Environment Setup

To follow the lab exercises, a set of resources must be provisioned. At
the heart of the scenario is the real-time stock price generator script
that generates a continuous stream of stock prices that are used
throughout the workshop.

We recommend deploying the stock price generator via Azure Container
Instance because the default Spark cluster will consume a large number
of resources.

## Task 1: Sign in to Power BI account and sign up for the free [Microsoft Fabric trial](https://learn.microsoft.com/en-us/fabric/get-started/fabric-trial)

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL: <https://app.fabric.microsoft.com/> then press
    the **Enter** button.

> ![A search engine window with a red box Description automatically
> generated with medium confidence](./media/image2.png)

2.  In the **Microsoft Fabric** window, enter your Microsoft 365
    credentials and click on the **Submit** button.

> ![A screenshot of a computer Description automatically
> generated](./media/image3.png)
>
> ![A close up of a white and green object Description automatically
> generated](./media/image4.png)

3.  Enter the **Administrative password** from the **Resources** tab and
    click on the **Sign in** button**.**

> ![A login screen with a red box and blue text Description
> automatically generated](./media/image5.png)

4.  In **Stay signed in?** window, click on the **Yes** button.

> ![A screenshot of a computer error Description automatically
> generated](./media/image6.png)

5.  You’ll be directed to Power BI Home page.

![A screenshot of a computer Description automatically
generated](./media/image7.png)

## Task 2: Start the Microsoft Fabric trial

1.  On **Power BI Home** page, click on the **Account manager** **for
    MOD Administrator** icon on the top right corner of the page. In the
    Account manager blade, navigate and select **Start trial** as shown
    in the below image**.**

![A screenshot of a computer Description automatically
generated](./media/image8.png)

2.  On **Upgrade to a free Microsoft Fabric** trial dialog box, click on
    **Start trial** button**.**

> ![](./media/image9.png)

3.  You will see a **Successfully upgraded to a free Microsoft Fabic
    trial** notification dialog box. In the dialog box, click on
    **Fabric Home Page** button.

> ![](./media/image10.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image11.png)

## Task 3: Redeem Azure Pass

1.  Open a new tab on your browser and enter the following link in the
    address bar: <https://www.microsoftazurepass.com/>

2.  Then, click on the **Start** button**.**

![A person using a computer Description automatically
generated](./media/image12.png)

**Note**: Do not use your Company/Work Account to login to redeem the
Azure Pass, another Azure Pass will not be issued.

3.  Click on the **Resources** tab of the Lab VM and enter the **Office
    365 tenant credentials** to **Sign In**.

> ![A screenshot of a computer Description automatically
> generated](./media/image13.png)

![Graphical user interface, application Description automatically
generated](./media/image14.png)

4.  Crosscheck the Email ID and then click on the **Confirm Microsoft
    Account**.

![Text Description automatically generated](./media/image15.png)

5.  Click on the **Resources** tab and copy the **Promo Code**. Navigate
    to **Enter Promo code** box and paste the Promo Code that have you
    copied, then click on the **Claim Promo Code** button**.**

> ![](./media/image16.png)

![Graphical user interface, text, application Description automatically
generated](./media/image17.png)

6.  Enter correct details in **Your Profile** page, tick all the check
    boxes, and then click on **Sign up** button.

![A screenshot of a computer Description automatically generated with
medium confidence](./media/image18.jpeg)

![Graphical user interface, text, application Description automatically
generated](./media/image19.png)

7.  On **Are you satisfied with your signup experience** window, enter
    your feedback and click on the **Submit** button.

![](./media/image20.png)

8.  It would automatically redirect you to the Azure Portal and now you
    are ready to use Azure services. On **Welcome to Microsoft Azure**
    dialog box, click on the **Maybe later** button.

![A screenshot of a computer Description automatically
generated](./media/image21.png)

## Task 4: Log Analytics Contributor role assignment 

1.  In Azure portal search box, type +++**Subscriptions**+++, then
    navigate and click on **Subscriptions** under **Services**.

![A screenshot of a computer Description automatically
generated](./media/image22.png)

2.  In **Subscriptions page**, navigate and click on **Azure Pass –
    Sponsorship**.

![A screenshot of a computer Description automatically
generated](./media/image23.png)

3.  In the Azure Pass – Sponsorship page, navigate and click on **Access
    control (IAM)** as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image24.png)

4.  On the **Access control (IAM)** page, click on +**Add** and then
    navigate and select **Add role assignment.**

![A screenshot of a computer Description automatically
generated](./media/image25.png)

5.  In the **Add role assignment – Role** page, click on **Job function
    roles** tab, navigate to the search box and type +++**Log Analytics
    Contributor**+++ and select it as shown in the below image, then
    click on the **Next** button.

![](./media/image26.png)

6.  In the **Members** tab, click on **+Select members**.

![](./media/image27.png)

7.  On the **Select members** pane that appears on the right side side,
    search and select your O365 tenant credentials and click on the
    **Select** button.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

8.  Then, click on **Review + Assign** button.

![](./media/image29.png)

![](./media/image30.png)

9.  You will see a notification – that you O365 Tenant ID was added as
    **Log Analytics Contributor** role for Azure Pass-Sponsorship.

> ![](./media/image31.png)

## Task 5: Create Fabric Capacity and Workspace

Microsoft Fabric is deployed to an Azure Active Directory tenant. Within
each Fabric tenant, Fabric capacities can be created to group resources
for various purposes -- this might be done organizationally (sales,
marketing, development), geographically, or other logical grouping.

If a Fabric Trial is available, we recommend taking advantage of that
opportunity to try Microsoft Fabric for a period of time (currently 60
days) with no commitment. To see if you are in a trial or eligible for a
trial, visit the [Fabric portal](https://app.fabric.microsoft.com/). If
you are able to log in or presented the option to start a trial, you
should be all set!

To create a Fabric capacity outside of a trial environment, create a new
resource from the Azure portal, and search for Fabric.

1.  From the Azure portal home page, click on **Azure portal menu**
    represented by three horizontal bars on the left side of the
    Microsoft Azure command bar as shown in the below image.

> ![A screenshot of a computer Description automatically
> generated](./media/image32.png)

2.  Navigate and click on **+ Create a resource**.

> ![A screenshot of a computer Description automatically
> generated](./media/image33.png)

3.  On **Create a resource** page, in the **Search services and
    marketplace** search bar, type+++ **Fabric+++**, then select
    **Microsoft fabric**.

> ![A screenshot of a computer Description automatically
> generated](./media/image34.png)

4.  In the **Marketplace** page, navigate to the **Microsoft Fabric**
    section, click on the Create button dropdown, then select
    **Microsoft Fabric** as shown in the image.

> ![A screenshot of a computer Description automatically
> generated](./media/image35.png)

5.  In the **Create Fabric capacity** window, under the **Basics** tab,
    enter the following details and click on the **Review+create**
    button.

[TABLE]

> ![A screenshot of a computer screen Description automatically
> generated](./media/image36.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image37.png)
>
> ![](./media/image38.png)
>
> ![](./media/image39.png)

6.  In the **Review+submit** tab, once the Validation is Passed, click
    on the **Create** button.

> ![A screenshot of a computer Description automatically
> generated](./media/image40.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image41.png)

7.  After the deployment is completed, click on the **Go to resource**
    button.

> ![A screenshot of a computer Description automatically
> generated](./media/image42.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image43.png)

## Task 6: Create Storage Account 

1.  Click on the **Portal Menu**, then select **+ Create a resource**

![Graphical user interface, application Description automatically
generated](./media/image44.png)

2.  In the **Create a resource** window search box, type +++**Storage
    account**+++ and then click on the **storage account**.

![Graphical user interface, application Description automatically
generated](./media/image45.png)

3.  In the **Marketplace** page, click on the **Storage account**
    section.

![A screenshot of a computer Description automatically
generated](./media/image46.png)

4.  In the **Storage account** window, click on the **Create** button.

![A screenshot of a computer Description automatically
generated](./media/image47.png)

5.  On **Create a storage account** window, under the **Basics** tab,
    enter the below details to create a storage account and then click
    on the **Next** button.

[TABLE]

![](./media/image48.png)

6.  Under the **Advanced** tab, select the check box **Enable
    hierarchical namespace** and then click on **Review + create.**

![](./media/image49.png)

7.  On the **Review+create** tab, click on the **Create** button.

![](./media/image50.png)

8.  The new Azure Storage account is now set up to host data for an
    Azure Data Lake. Click on the **Go to resource** button.

![A screenshot of a computer Description automatically
generated](./media/image51.png)

![A screenshot of a computer Description automatically
generated](./media/image52.png)

9.  In the **Storage account** (storagefabricXXX) page left-sided
    navigation menu, click on the **Access control (IAM).**

![](./media/image53.png)

10. On the **Access control (IAM)** page, click on +**Add** and then
    select **Add role assignments.**

![](./media/image54.png)

11. In the **Add role assignment – Role** page, click on **Job function
    roles** tab. In the search bar, type +++**Storage Blob Data
    Contributor+++** and select it, as shown in the below image. Then,
    click on the **Next** button.

![](./media/image55.png)

12. In the **Members** tab, click on **+Select members**.

![](./media/image56.png)

13. On the **Select members** pane that appears on the right side,
    search and select your O365 tenant credentials, then click on the
    **Select** button.

![](./media/image28.png)

14. Click on **Review + assign** button.

![](./media/image57.png)

![](./media/image58.png)

15. You will see a notification – that you tenant ID was added as
    Storage Blob Data Contributor role for Azure Pass-Sponsorship.

![A screenshot of a computer Description automatically
generated](./media/image59.png)

16. Click again on the **Access control (IAM).**

![A screenshot of a computer Description automatically
generated](./media/image60.png)

17. On the **Access control (IAM)** page, click on +**Add** and select
    **Add role assignments.**

![A screenshot of a computer Description automatically
generated](./media/image54.png)

18. Click on **Privileged administrator roles** tab, select **Owner
    role,** then click on the **Next** button.

![](./media/image61.png)

19. In the **Members** tab, click on **+Select members**.

![A screenshot of a computer Description automatically
generated](./media/image56.png)

20. On the **Select members** pane that appears on the right side,
    search and select your O365 tenant credentials and click on the
    **Select** button.

![A screenshot of a computer Description automatically
generated](./media/image28.png)

21. In the **Add role assignment** page, Click **Next**, you will get a
    notification once the role assignment is complete.

![](./media/image62.png)

22. Select the check box –**Allow user to assign all roles (highly
    privileged)** and click on **Review + assign** button.

![](./media/image63.png)

![](./media/image64.png)

![A screenshot of a computer Description automatically
generated](./media/image58.png)

23. You will see a notification – that your O365 tenant ID was added as
    Owner for storagefabricXXX.

![A white box with black text Description automatically
generated](./media/image65.png)

## **Task 7: Create a Fabric workspace**

In this task, you create a Fabric workspace. The workspace contains all
the items needed for this lakehouse tutorial, which includes lakehouse,
dataflows, Data Factory pipelines, the notebooks, Power BI datasets, and
reports.

1.  Open your browser, navigate to the address bar, and type or paste
    the following URL:
    [https://app.fabric.microsoft.com/](https://app.fabric.microsoft.com/,)
    then press the **Enter** button. In the **Microsoft Fabric Home**
    page, navigate and click on **Power BI** tile.

> ![A screenshot of a computer Description automatically
> generated](./media/image66.png)

2.  In the **Power BI Home** page left-sided navigation menu, navigate
    and click on **Workspaces** as shown in the below image.

> ![](./media/image67.png)

3.  In the Workspaces pane, click on **+** **New workspace button**

> ![A screenshot of a computer Description automatically
> generated](./media/image68.png)

4.  In the **Create a workspace** pane that appears on the right side,
    enter the following details, and click on the **Apply** button.

[TABLE]

> ![](./media/image69.png)
>
> ![](./media/image70.png)

## **Task 8: Deploy the app via Azure Container Instance**

This task deploys the stock generator app to an Azure Container Instance
using an ARM template. The app will generate stock data that publishes
the data to an Azure Event Hub, which is also configured during the
deployment of the ARM template.

To auto-deploy the resources, use these steps below.

1.  Open a new address bar and enter the following URL. If prompted to
    Sign in, then use your O365 tenant credentials.

> [https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2Ffabricrealtimelab%2Fmain%2Fresources%2Fmodule00%2Ffabricworkshop_arm_managedid.json](https://portal.azure.com/%23create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmicrosoft%2Ffabricrealtimelab%2Fmain%2Fresources%2Fmodule00%2Ffabricworkshop_arm_managedid.json)

2.  In the **Custom deployment** window, under the **Basics** tab, enter
    the following details and click on the **Review+create** button.

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image71.png)

![A screenshot of a computer Description automatically
generated](./media/image72.png)

3.  In the **Review + create** tab, navigate and click on the **Create**
    button**.**

![](./media/image73.png)

5.  Wait for the deployment to complete. The deployment will take around
    10-15 minutes.

6.  After the deployment is completed, click on the **Go to resource**
    button.

![A screenshot of a computer Description automatically
generated](./media/image74.png)

4.  In **realtimeworkshop** **Resource group**, verify that the **Event
    Hub Namespace **and **Azure Container Instance (ACI)** are
    successfully deployed.

![](./media/image75.png)

5.  Open the **Event Hub** **namespace**, which will have a name similar
    to** *ehns-XXXXXX-fabricworkshop***.

![](./media/image76.png)

6.  In **Event Hub** **namespace** page left-sided navigation menu,
    navigate to **Settings** section and click on **Shared access
    policies**.

> ![A screenshot of a computer Description automatically
> generated](./media/image77.png)

7.   In the ***Shared access policies*** page, click on
    ***stockeventhub_sas*** .**SAS Policy: stockeventhub_sas** pane
    appear on the right side, copy the **primary key** and **Event Hub
    namespace** (such as *ehns-XXXXXX-fabricworkshop*) and paste them on
    a notepad, as you need them in the upcoming task. In short, you'll
    need the following:

![A screenshot of a computer Description automatically
generated](./media/image78.png)

![A screenshot of a computer Description automatically
generated](./media/image79.png)

## **Task 9: Get data with Eventstream**

1.  Go back to the Microsoft Fabric, navigate and click on **Power BI**
    at the bottom of the page, then select **Real-Time Intelligence**.

![A screenshot of a computer Description automatically
generated](./media/image80.png)

2.  On the **Synapse Real-Time Analytics** home page,
    select **Eventstream**. Name the Eventstream
    +++ ***StockEventStream**+++, check the **Enhanced Capabilities
    (preview)*** and click on the **Create** button.

![A screenshot of a computer Description automatically
generated](./media/image81.png)

![A screenshot of a computer Description automatically
generated](./media/image82.png)

3.  On the Eventstream, select **Add external source**

![](./media/image83.png)

4.  On the Add source, select **Azure *Event Hubs.***

![A screenshot of a chat Description automatically
generated](./media/image84.png)

5.  On the **Azure Event Hubs** configuration page, enter the below
    details and click on **Add** button.

&nbsp;

1.  Configure connection settings: Click on the **New connection** and
    enter the below details then click on **Create** button.

&nbsp;

1.  In Event Hub namespace-Enter Event Hub name (the values that you
    have saved in your notepad**)**

2.  Event Hub : **+++StockEventHub+++**

3.  Shared Access Key Name:+++**stockeventhub_sas+++**

4.  Shared Access Key- Enter Primary Key (the value that you have saved
    in your notepad in the **Task 8)**

&nbsp;

2.  Consumer group: ***$Default*** 

3.  Data format: **JSON** and click on **Next** button

![A screenshot of a computer Description automatically
generated](./media/image85.png)

![A screenshot of a computer Description automatically
generated](./media/image86.png)

![A screenshot of a computer Description automatically
generated](./media/image87.png)

![A screenshot of a computer Description automatically
generated](./media/image88.png)

![A screenshot of a computer Description automatically
generated](./media/image89.png)

8.  You will see a notification stating **Successfully added The source
    “StockEventHub,Azure Event Hubs”** was added.

> ![A screenshot of a computer Description automatically
> generated](./media/image90.png)

9.  With the Event Hub configured, click on ***Test result***. You
    should see events including the stock symbol, price, and timestamp.

> ![](./media/image91.png)

10. On the Eventstream, select **Publish.**

![](./media/image92.png)

![](./media/image93.png)

11. On the Eventstream, select **AzureEventHub** and click on
    **Refresh** button.

![A screenshot of a computer Description automatically
generated](./media/image94.png)

![A screenshot of a computer Description automatically
generated](./media/image95.png)

# Exercise 2: KQL Database Configuration and Ingestion

Now that our environment is fully configured, we will complete the
ingestion of the Eventstream, so that the data is ingested into a KQL
database. This data will also be stored in Fabric OneLake.

## Task 1: Create KQL Database

Kusto Query Language (KQL) is the query language used by Real-Time
analytics in Microsoft Fabric along with several other solutions, like
Azure Data Explorer, Log Analytics, Microsoft 365 Defender, etc. Similar
to Structured Query Language (SQL), KQL is optimized for ad-hoc queries
over big data, time series data, and data transformation.

To work with the data, we'll create a KQL database and stream data from
the Eventstream into the KQL DB.

1.  In the left-sided navigation menu, navigate and click on **RealTime
    workspaceXXX**, as shown in the below image.

> ![](./media/image96.png)

2.  In the **Real-Time Intelligence** page, navigate to **New** section
    and click on, select **Eventhouse** to create Eventhouse.

> ![A screenshot of a computer Description automatically
> generated](./media/image97.png)

3.  In the **New Eventhouse** dialog box, enter +++**StockDB+++**in
    the **Name** field, click on the **Create** button and open the new
    Eventhouse.

> ![A screenshot of a computer Description automatically
> generated](./media/image98.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image99.png)

4.  Click on the **pencil icon** as shown in the below image to change
    the setting and select the **Active**, then click on the **Done**
    button to enable OneLake access.

![A screenshot of a computer Description automatically
generated](./media/image100.png)

5.  After enabling OneLake, you may need to refresh the page to verify
    the OneLake folder integration is active.

![A screenshot of a computer Description automatically
generated](./media/image101.png)

![A screenshot of a computer Description automatically
generated](./media/image102.png)

## Task 2: Send data from the Eventstream to the KQL database

1.  In the left-sided navigation menu, navigate and click on
    **StockEventStream** created in the previous task, as shown in the
    below image.

> ![A screenshot of a computer Description automatically
> generated](./media/image103.png)

2.  On the Eventstream, click on the **Edit** button.

> ![A screenshot of a computer Description automatically
> generated](./media/image104.png)

3.  Our data should be arriving into our Eventstream, and we'll now
    configure the data to be ingested into the KQL database we created
    in the above task. On the Eventstream, click on *Transform events or
    add destination,* then navigate and click on **KQL Database**.

> ![A screenshot of a computer Description automatically
> generated](./media/image105.png)

4.  On the KQL settings, select *Direct ingestion*. While we have the
    opportunity to process event data at this stage, for our purposes,
    we will ingest the data directly into the KQL database. Set the
    destination name to +++*KQL+++*, then select your **workspace** and
    KQL database created in the above task, then click on **Save**
    button.

![](./media/image106.png)

5.  Click on the **Publish** button

![](./media/image107.png)

![A screenshot of a computer Description automatically
generated](./media/image108.png)

![A screenshot of a computer Description automatically
generated](./media/image109.png)

6.  On the Eventstream pane, select **configure** in the **KQL**
    destination.

![A screenshot of a computer Description automatically
generated](./media/image110.png)

7.  On the first settings page, select **+New table** and enter the
    name +++*StockPrice+++* for the table to hold the data in StockDB.
    Click on the **Next** button.

![A screenshot of a computer Description automatically
generated](./media/image111.png)

![A screenshot of a computer Description automatically
generated](./media/image112.png)

8.  The next page allows us to inspect and configure the schema. Be sure
    to change the format from TXT to **JSON**, if necessary. The default
    columns of *symbol*, *price*, and *timestamp* should be formatted as
    shown in the below image; then click on the *Finish* button.

![A screenshot of a computer Description automatically
generated](./media/image113.png)

2.  On the **Summary** page, if there are no errors, you’ll see a
    **green checkmark** as shown in the below image, then click on the
    *Close* button to complete the configuration.

![A screenshot of a computer Description automatically
generated](./media/image114.png)

![A screenshot of a computer Description automatically
generated](./media/image115.png)

9.  Click on the **Refresh** button

![A screenshot of a computer Description automatically
generated](./media/image116.png)

10. Select the **KQL** destination and click on the **Refresh** button.

![A screenshot of a computer Description automatically
generated](./media/image117.png)

![A screenshot of a computer Description automatically
generated](./media/image118.png)

# Exercise 3: Exploring the Data

In this exercise, you'll create several KQL queries of increasing
complexity to support different business uses.

## Task 1: Create KQL queryset: StockQueryset

1.  Click on **RealTimeWorkspace** on the left-sided navigation pane.

![A screenshot of a computer Description automatically
generated](./media/image119.png)

2.  From your workspace, click on ***+** **New \> KQL Queryset** as
    shown in the below image. In the **New KQL Queryset** dialog box,*
    enter +++***StockQueryset**+++*, then click on the **Create**
    button.

![](./media/image120.png)

![A screenshot of a computer Description automatically
generated](./media/image121.png)

3.  Select the ***StockDB*** and click on the **Connect** button. ![A
    screenshot of a computer Description automatically
    generated](./media/image122.png)

4.  The KQL query window will open, allowing you to query the data.

> ![A screenshot of a computer Description automatically
> generated](./media/image123.png)

5.  The default query code will look like the code shown in the below
    image; it contains 3 distinct KQL queries. You may
    see *YOUR_TABLE_HERE* instead of the ***StockPrice*** table. Select
    and delete them.

6.  ![A screenshot of a computer Description automatically
    generated](./media/image123.png)

7.  In the query editor, copy and paste the following code. Select the
    entire text and click on *the **Run*** button to execute the query.
    After the query is executed, you will see the results.

**Copy**

// Use "take" to view a sample number of records in the table and check
the data.

StockPrice

| take 100;

// See how many records are in the table.

StockPrice

| count;

// This query returns the number of ingestions per hour in the given
table.

StockPrice

| summarize IngestionCount = count() by bin(ingestion_time(), 1h);

***Note:** To run a single query when there are multiple queries in the
editor, you can highlight the query text or place your cursor so the
cursor is in the context of the query (for example, at the beginning or
end of the query) -- the current query should highlight in blue. To run
the query, click Run in the toolbar. If you'd like to run all 3 to
display the results in 3 different tables, each query will need to have
a semicolon (;) after the statement, as shown below.*

![](./media/image124.png)

8.  The results will be displayed in 3 different tables as shown in the
    below image. Click on each table tab to review the data.

![A screenshot of a computer Description automatically
generated](./media/image125.png)

> ![](./media/image126.png)
>
> ![](./media/image127.png)

## Task 2: New Query of StockByTime

1.  Create a new tab within the queryset by clicking on the ***+* icon**
    as shown in the below image. Rename this tab as
    +++***StockByTime***+++

> ![](./media/image128.png)
>
> ![](./media/image129.png)
>
> ![](./media/image130.png)

2.  We can begin to add our own calculations, such as calculating the
    change over time. For example,
    the [prev()](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/prevfunction) function,
    a type of windowing function, allows us to look at values from
    previous rows; we can use this to calculate the change in price. In
    addition, as the previous price values are stock symbol specific, we
    can [partition](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/partition-operator) the
    data when making calculations.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

Copy

StockPrice

| where timestamp \> ago(75m)

| project symbol, price, timestamp

| partition by symbol

(

order by timestamp asc

| extend prev_price = prev(price, 1)

| extend prev_price_10min = prev(price, 600)

)

| where timestamp \> ago(60m)

| order by timestamp asc, symbol asc

| extend pricedifference_10min = round(price - prev_price_10min, 2)

| extend percentdifference_10min = round(round(price - prev_price_10min,
2) / prev_price_10min, 4)

| order by timestamp asc, symbol asc

![](./media/image131.png)

4.  In this KQL query, the results are first limited to the most recent
    75 minutes. While we ultimately limit the rows to the last 60
    minutes, our initial dataset needs enough data to lookup previous
    values. The data is then partitioned to group the data by symbol,
    and we look at the previous price (from 1 second ago) as well as the
    previous price from 10 minutes ago. Note that this query assumes
    data is generated at 1 second intervals. For the purposes of our
    data, subtle fluctuations are acceptable. However, if you need
    precision in these calculations (such as exactly 10 minutes ago and
    not 9:59 or 10:01), you'd need to approach this differently.

## Task 3: StockAggregate

1.  Create another new tab within the queryset by clicking on
    the ***+* icon** as shown in the below image. Rename this tab as
    **+++*StockAggregate*+++**

> ![](./media/image132.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image133.png)

2.  This query will find the biggest price gains over a 10-minute period
    for each stock, and the time it occurred. This query uses
    the [summarize](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/summarizeoperator) operator,
    which produces a table that aggregates the input table into groups
    based on the specified parameters (in this case, *symbol*),
    while [arg_max](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/arg-max-aggregation-function) returns
    the greatest value.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> **Copy**
>
> StockPrice
>
> | project symbol, price, timestamp
>
> | partition by symbol
>
> (
>
> order by timestamp asc
>
> | extend prev_price = prev(price, 1)
>
> | extend prev_price_10min = prev(price, 600)
>
> )
>
> | order by timestamp asc, symbol asc
>
> | extend pricedifference_10min = round(price - prev_price_10min, 2)
>
> | extend percentdifference_10min = round(round(price -
> prev_price_10min, 2) / prev_price_10min, 4)
>
> | order by timestamp asc, symbol asc
>
> | summarize arg_max(pricedifference_10min, \*) by symbol

![A screenshot of a computer Description automatically
generated](./media/image134.png)

![A screenshot of a computer Description automatically
generated](./media/image135.png)

## Task 4: StockBinned

1.  Create another new tab within the queryset by clicking on
    the ***+* icon** as shown in the below image. Rename this tab as
    +++***StockBinned+++***

![](./media/image136.png)

![](./media/image137.png)

2.  KQL also has a [bin()
    function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/bin-function),
    which can be used to bucket results based on the bin parameter. In
    this case, by specifying a timestamp of 1 hour, the result is
    aggregated for each hour. The time period can be set to minute,
    hour, day, and so on.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> **Copy**

StockPrice

| summarize avg(price), min(price), max(price) by bin(timestamp, 1h),
symbol

| sort by timestamp asc, symbol asc

![A screenshot of a computer Description automatically
generated](./media/image138.png)

4.  This is particularly useful when creating reports that aggregate
    real-time data over a longer time period.

## Task 5: Visualizations

1.  Create a final new tab within the queryset by clicking on
    the ***+* icon** as shown in the below image. Rename this tab as
    +++***Visualizations+++*.** We'll use this tab to explore
    visualizing data.

> ![](./media/image139.png)
>
> ![](./media/image140.png)

2.  KQL supports a large number
    of [visualizations](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/render-operator?pivots=fabric) by
    using the *render* operator. Run the below query, which is the same
    as the StockByTime query, but with an additional *render* operation
    added:

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> Copy

StockPrice

| where timestamp \> ago(75m)

| project symbol, price, timestamp

| partition by symbol

(

order by timestamp asc

| extend prev_price = prev(price, 1)

| extend prev_price_10min = prev(price, 600)

)

| where timestamp \> ago(60m)

| order by timestamp asc, symbol asc

| extend pricedifference_10min = round(price - prev_price_10min, 2)

| extend percentdifference_10min = round(round(price - prev_price_10min,
2) / prev_price_10min, 4)

| order by timestamp asc, symbol asc

| render linechart with (series=symbol, xcolumn=timestamp,
ycolumns=price)

![A screenshot of a computer Description automatically
generated](./media/image141.png)

4.  This will render a line chart as shown in the below image.

![A screenshot of a graph Description automatically
generated](./media/image142.png)

# Exercise 4: Optimizing Power BI Reporting Efficiency

With the data loaded in the database and our initial KQL queryset
complete, we can begin to craft visualizations for real-time dashboards.

## Task 1: Configuring Refresh Rate

Our Power BI tenant needs to be configured to allow for more frequent
updating.

1.  To configure this setting, navigate to the Power BI admin portal by
    clicking on the ***Settings* **icon in the upper right corner of the
    **Fabric portal**. Navigate to Governance and insights section, then
    click on **Admin portal**.

![](./media/image143.png)

2.  In the **Admin portal** page, navigate and click on ***Capacity
    settings**,* then click on **Trial** tab. Click on your capacity
    name.

> ![](./media/image144.png)

3.  Scroll down and click on ***Power BI workloads***, and
    under ***Semantic Models*** (recently renamed from *Datasets*),
    configure ***Automatic page refresh*** to ***On***, with a **minimum
    refresh interval** of **1 Seconds**. Then, click on the ***Apply***
    button.

**Note**: Depending on your administrative permissions, this setting may
not be available. Note that this change may take several minutes to
complete.

![](./media/image145.png)

![](./media/image146.png)

4.  On **Update your capacity workloads** dialog box, click on the
    **Yes** button.

![](./media/image147.png)

## Task 2: Creating a basic Power BI report

1.  In the **Microsoft Fabric** page menu bar on the left side, select
    **StockQueryset**.

![A screenshot of a computer Description automatically
generated](./media/image148.png)

2.  From the ***StockQueryset*** queryset used in the previous module,
    select the ***StockByTime*** query tab.

![](./media/image149.png)

3.  Select the query and run to view the results. Click** **on *the
    **Build Power BI report*** button in the command bar to bring this
    query into Power BI.

![](./media/image150.png)

![A screenshot of a computer Description automatically
generated](./media/image151.png)

4.  On the report preview page, we can configure our initial chart,
    select a **line chart** to the design surface, and configure the
    report as follows. See the image below as a reference.

- Legend: **symbol**

- X-axis: **timestamp**

- Y-axis**: price**

![](./media/image152.png)

5.  In the Power BI (preview) page, from the ribbon, click on
    **File** and select **Save**.

> ![A screenshot of a graph Description automatically
> generated](./media/image153.png)

6.  On **Just a few details first** dialog box, in **Name your file in
    Power BI** field, enter +++***RealTimeStocks**+++*. In **Save it to
    a workspace** field, click on the dropdown and select
    ***RealTimeWorkspace***. Then, click on the **Continue** button**.**

![A screenshot of a computer Description automatically
generated](./media/image154.png)

7.  In the Power BI (preview) page, click on **Open the file in Power BI
    to view, edit and get a shareable link.**

![](./media/image155.png)

8.  On the **RealTimeStock** page, click on the **Edit** button in the
    command bar to open the report editor.

> ![A graph of different colored lines Description automatically
> generated](./media/image156.png)

9.  Select the line chart on the report. Configure a **Filter**
    for ***timestamp*** to display data for the last 5 minutes using
    these settings:

- Filter type: Relative time

- Show items when the value: is in the last 5 minutes

Click on ***Apply filter*** to enable the filter. You will see a similar
type of output as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image157.png)

## Task 3: Creating a second visual for percent change

1.  Create a second line chart, under **Visualizations**, select **Line
    chart**.

2.  Instead of plotting the current stock price, select
    the ***percentdifference_10min*** value, which is a positive or
    negative value based off the difference between the current price
    and the value of the price from 10 minutes ago. Use these values for
    the chart:

- Legend: **symbol**

- X-axis: **timestamp**

- Y-axis: **average of percentdifference_10min**

![](./media/image158.png)

![A screenshot of a computer Description automatically
generated](./media/image159.png)

3.  Under the **Visualization,** select the **Analytics** represented by
    a magnifier-like icon as shown in the below image, then click on
    **Y-Axis Constant Line(1).** In the **Apply settings to**
    section**,** click on **+Add line,** then enter **Value 0.**

![](./media/image160.png)

4.  Select the line chart on the report. Configure a **Filter**
    for ***timestamp*** to display data for the last 5 minutes using
    these settings:

- Filter type: Relative time

- Show items when the value: is in the last 5 minutes

![](./media/image161.png)

## Task 4: Configuring the report to auto-refresh

1.  Deselect the chart. On the ***Visualizations* settings**,
    enable ***Page refresh*** to automatically refresh every second or
    two, based on your preference. Of course, realistically we need to
    balance the performance implications of refresh frequency, user
    demand, and system resources.

2.  Click on **Format your report** **page** icon, navigate and click on
    **Page refresh**. Turn on the toggle. Set the Auto page refresh
    value as **2 Seconds** as shown in the below image.

![](./media/image162.png)

3.  In the Power BI (preview) page, from the ribbon, click on
    **File** and select **Save**.

![](./media/image163.png)

# Exercise 5: Setup Warehouse and Pipeline

## Task 1: Create a Synapse Data Warehouse in the Fabric workspace

To get started, we'll first create the Synapse Data Warehouse in our
workspace.

1.  Click on the **Real Time Analytics  icon** at the bottom of the page
    on the left side, navigate and click on **Data Warehouse** as shown
    in the below image.

![A screenshot of a computer Description automatically
generated](./media/image164.png)

2.  Select the ***Warehouse* **tile to create a new Synapse Data
    Warehouse.

![](./media/image165.png)

3.  On the **New warehouse** dialog box, enter +++***StocksDW+++*** as
    the name and click on the **Create** button.

![](./media/image166.png)

4.  The warehouse is largely empty.

![A screenshot of a computer Description automatically
generated](./media/image167.png)

5.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next task.

![](./media/image168.png)

## Task 2: Create the staging and ETL objects

1.  Run the following query that creates the staging tables that will
    hold the data during the ETL (Extract, Transform, and Load) process.
    This will also create the two schemas used -- *stg* and *ETL*;
    schemas help group workloads by type or function. The *stg* schema
    is for staging and contains intermediate tables for the ETL process.
    The *ETL* schema contains queries used for data movement, as well as
    a single table for tracking state.

2.  Note that the begin date for the watermark is arbitrarily chosen as
    some previous date (1/1/2022), ensuring all data is captured -- this
    date will be updated on each successful run.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> **Copy**

/\* 1 - Create Staging and ETL.sql \*/

-- STAGING TABLES

CREATE SCHEMA stg

GO

CREATE TABLE stg.StocksPrices

(

symbol VARCHAR(5) NOT NULL

,timestamp VARCHAR(30) NOT NULL

,price FLOAT NOT NULL

,datestamp VARCHAR(12) NOT NULL

)

GO

-- ETL TABLES

CREATE SCHEMA ETL

GO

CREATE TABLE ETL.IngestSourceInfo

(

ObjectName VARCHAR(50) NOT NULL

,WaterMark DATETIME2(6)

,IsActiveFlag VARCHAR(1)

)

INSERT \[ETL\].\[IngestSourceInfo\]

SELECT 'StocksPrices', '1/1/2022 23:59:59', 'Y'

![](./media/image169.png)

![](./media/image170.png)

4.  Rename the query for reference. Right-click on **SQL query 1** in
    **Explorer** and select **Rename**.

![](./media/image171.png)

5.  In the **Rename** dialog box, under the **Name** field, enter
    +++**Create stocks and metadata+++**, then click on the **Rename**
    button. 

> ![A screenshot of a computer Description automatically
> generated](./media/image172.png)

6.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next step:

![A screenshot of a computer Description automatically
generated](./media/image173.png)

7.  The *sp_IngestSourceInfo_Update* procedure updates the watermark;
    this ensures we are keeping track of which records have already been
    imported

8.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

**Copy**

/\* 1 - Create Staging and ETL.sql \*/

CREATE PROC \[ETL\].\[sp_IngestSourceInfo_Update\]

@ObjectName VARCHAR(50)

,@WaterMark DATETIME2(6)

AS

BEGIN

UPDATE \[ETL\].\[IngestSourceInfo\]

SET WaterMark = @WaterMark

WHERE

ObjectName = @ObjectName

END

GO

![](./media/image174.png)

![A screenshot of a computer Description automatically
generated](./media/image175.png)

6.  Rename the query for reference later. Right-click on **SQL query 1**
    in **Explorer** and select **Rename**.

![A screenshot of a computer Description automatically
generated](./media/image176.png)

7.  In the **Rename** dialog box, under the **Name** field, enter
    +++**ETL.sql_IngestSource+++**, then click on the **Rename**
    button. 

![A screenshot of a computer Description automatically
generated](./media/image177.png)

This should look similar to:

![DW First Queries](./media/image178.png)

## Task 3: Create the data pipeline

1.  On the **StockDW** page, click on **RealTimeWorkspace** Workspace on
    the left-sided navigation menu.

![A screenshot of a computer Description automatically
generated](./media/image179.png)

2.  On the **Synapse Data Warehouse RealTimeWorkhouse** home page, under
    **RealTimeWorkhouse**, click on **+New**, then select **Data
    pipeline.**

![](./media/image180.png)

3.  A **New pipeline** dialog box will appear, in the **Name**  field,
    enter +++ ***PL_Refresh_DWH***+++ and click on the **Create**
    button**.**

![A screenshot of a computer Description automatically
generated](./media/image181.png)

4.  In the ***PL_Refresh_DWH*** page, navigate to **Build a data
    pipeline to organize and move your data** section and click on
    **P**i**peline activity**.

![](./media/image182.png)

5.  Then, navigate and select ***Lookup*** activity as shown in the
    below image.

![A screenshot of a computer Description automatically
generated](./media/image183.png)

6.  On the **General** tab, in the **Name field,** enter +++***Get
    WaterMark***+++

![](./media/image184.png)

7.  Click on the **Settings** tab, enter the following details as shown
    in the below image.

[TABLE]

![](./media/image185.png)

## Task 4: Build ForEach activity

This task focuses on building multiple activities within a single
ForEach activity. The ForEach activity is a container that executes
child activities as a group: in this case, if we had multiple sources to
pull data from, we'd repeat these steps for each data source.

1.  In the **Lookup - Get WaterMark** box, navigate and click on the
    right arrow to **Add an activity**. Then, navigate and
    select ***ForEach*** activity as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image186.png)

2.  Click on the **Settings** tab, enter the items as +++
    **@activity('Get WaterMark').output.value**+++

This should look similar to the below image:

![](./media/image187.png)

3.  In the *ForEach*  box, click on the plus (+) symbol to add a new
    activity.

> ![](./media/image188.png)

4.  Select and add a ***Copy Data*** activity within *ForEach.*

> ![A screenshot of a computer Description automatically
> generated](./media/image189.png)

5.  Select **Copy data1** Activity icon, on the **General** tab, in the
    **Name field,** enter +++***Copy KQL***+++

> ![A screenshot of a computer Description automatically
> generated](./media/image190.png)

6.  Click on the **Source** tab, enter the following settings.

[TABLE]

The *Source* tab of the activity should look similar to:

![](./media/image191.png)

7.  Click on the **Destination** tab, enter the following settings

[TABLE]

- Under the *Advanced* section, enter the following ***Pre-copy
  script*** to truncate the table before loading the staging table:

> **+++delete** stg**.**StocksPrices+++

This step first deletes old data from the staging table, and then copies
the data from the KQL table, selecting data from the last watermark and
inserting it into the staging table. Using a watermark is important to
avoid processing the entire table; additionally, KQL queries have a
maximum rowcount of 500,000 rows. Given the current rate of data
ingested, this equates to about 3/4 of one day.

The *Destination* tab of the activity should look like:

![](./media/image192.png)

8.  In the *ForEach*  box, click on the plus **(+)** symbol, navigate
    and select **Lookup** activity.

![](./media/image193.png)

9.  Click on **Lookup1** icon, in the **General** tab, **Name field,**
    enter +++***Get New WaterMark*** +++

> ![A screenshot of a computer Description automatically
> generated](./media/image194.png)

10. Click on the **Settings** tab, enter the following settings

[TABLE]

![](./media/image195.png)

11. In the *ForEach* box, click on the plus **(+)** symbol, navigate and
    select ***Stored Procedure***  activity.

> ![A screenshot of a computer Description automatically
> generated](./media/image196.png)

12. Click on the **Stored procedure** icon. On the **General** tab, in
    the **Name field,** enter +++ ***Update WaterMark*** +++

> ![A screenshot of a computer Description automatically
> generated](./media/image197.png)

13. Click on the **Settings** tab, enter the following settings.

[TABLE]

- Parameters (click *Import* to automatically add the parameter names):

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image198.png)

## Task 5: Test the Pipeline

1.  From the ***Home*** tab in the pipeline, select ***Run***.

> ![A screenshot of a computer Description automatically
> generated](./media/image199.png)

2.  In the **Save and run?** dialog box, click on **Save and run**
    button

> ![A screenshot of a computer Description automatically
> generated](./media/image200.png)

3.  This will prompt to first save the pipeline, and then validate to
    find any configuration errors. This initial run will take a few
    moments and will copy the data into the staging table.

![A screenshot of a computer Description automatically
generated](./media/image201.png)

4.  On the **PL_Refresh_DWH** page, click on **RealTimeWorkspace**
    Workspace on the left-sided navigation menu.

![A screenshot of a computer Description automatically
generated](./media/image202.png)

5.  Click on the **Refresh** button.

![A screenshot of a computer Description automatically
generated](./media/image203.png)

6.  In the data warehouse, data should be visible in the staging table.
    Within the data warehouse, selecting a table will show a preview of
    the data in the table. Click on StocksDW on the left-sided
    navigation menu, then click o **Schemas** in Explorer. Under
    Schemas, navigate and click on **stg**, then click on
    **StocksPrices** as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image204.png)

9.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next step:

![](./media/image205.png)

8.  While we're in the data warehouse, run the script below in new SQL
    query window to reset the ingestion process. It's often handy in
    development to have a reset script to allow for incremental testing.
    This will reset the date and delete the data from the staging table.

> ***Note:** We haven't created the fact or dimensions table yet, but
> the script should still work.*

9.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> **Copy**

-- Run this to 'RESET' the ingestion tables

exec ETL.sp_IngestSourceInfo_Update 'StocksPrices', '2022-01-01
23:59:59.000000'

GO

IF (EXISTS (SELECT \* FROM INFORMATION_SCHEMA.TABLES

WHERE TABLE_SCHEMA = 'stg' AND TABLE_NAME = 'StocksPrices'))

BEGIN

delete stg.StocksPrices

END

GO

IF (EXISTS (SELECT \* FROM INFORMATION_SCHEMA.TABLES

WHERE TABLE_SCHEMA = 'dbo' AND TABLE_NAME = 'fact_Stocks_Daily_Prices'))

BEGIN

delete dbo.fact_Stocks_Daily_Prices

END

GO

IF (EXISTS (SELECT \* FROM INFORMATION_SCHEMA.TABLES

WHERE TABLE_SCHEMA = 'dbo' AND TABLE_NAME = 'dim_Symbol'))

BEGIN

delete dbo.dim_Symbol

END

GO

![A screenshot of a computer Description automatically
generated](./media/image206.png)

![A screenshot of a computer Description automatically
generated](./media/image207.png)

# Exercise 6: Build Star Schema

For the date dimension, we'll load enough values for the foreseeable
future. Date dimensions are fairly similar across all implementations
and typically hold specific date details: the day of week, month,
quarter, etc.

For the symbol dimension, we'll incrementally load that during the
pipeline -- this way, if new stocks are added at some point, they will
get added to the Symbol dimension table during the execution of the
pipeline. The symbol dimension holds additional details about each
symbol, such as company name, the stock market it trades on, etc.

We'll also create views to support the pipeline by making it easier to
load data from the staging table by aggregating the min, max, and
closing price of the stock.

## Task 1: Create the dimension and fact tables

1.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next step.

![](./media/image205.png)

2.  In our data warehouse, run the following SQL to create the fact and
    dimension tables. As in the previous step, you can run this ad-hoc
    or create a SQL query to save the query for future use.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> **Copy**

/\* 2 - Create Dimension and Fact tables.sql \*/

-- Dimensions and Facts (dbo)

CREATE TABLE dbo.fact_Stocks_Daily_Prices

(

Symbol_SK INT NOT NULL

,PriceDateKey DATE NOT NULL

,MinPrice FLOAT NOT NULL

,MaxPrice FLOAT NOT NULL

,ClosePrice FLOAT NOT NULL

)

GO

CREATE TABLE dbo.dim_Symbol

(

Symbol_SK INT NOT NULL

,Symbol VARCHAR(5) NOT NULL

,Name VARCHAR(25)

,Market VARCHAR(15)

)

GO

CREATE TABLE dbo.dim_Date

(

\[DateKey\] DATE NOT NULL

,\[DayOfMonth\] int

,\[DayOfWeeK\] int

,\[DayOfWeekName\] varchar(25)

,\[Year\] int

,\[Month\] int

,\[MonthName\] varchar(25)

,\[Quarter\] int

,\[QuarterName\] varchar(2)

)

GO

![A screenshot of a computer Description automatically
generated](./media/image208.png)

![A screenshot of a computer Description automatically
generated](./media/image209.png)

4.  Rename the query for reference. Right-click on **SQL query** in
    Explorer and select **Rename**.

![A screenshot of a computer Description automatically
generated](./media/image210.png)

5.  In the **Rename** dialog box, under the **Name** field, enter +++
    Create Dimension and Fact tables**+++**, then click on the
    **Rename** button. 

![A screenshot of a computer Description automatically
generated](./media/image211.png)

## Task 2: Load the date dimension

1.  Click ***New SQL query*** at the top of the window. Click on ***New
    SQL query*** dropdown in the command bar, then select **New SQL
    query** under **Blank** section. We'll start building our schema in
    the next step:

![A screenshot of a computer Description automatically
generated](./media/image212.png)

2.  The date dimension is differentiated; it can be loaded once with all
    the values we'd need. Run the following script, which creates a
    procedure to populate the date dimension table with a broad range of
    values.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. After the query is
    executed, you will see the results.

> **Copy**

/\* 3 - Load Dimension tables.sql \*/

CREATE PROC \[ETL\].\[sp_Dim_Date_Load\]

@BeginDate DATE = NULL

,@EndDate DATE = NULL

AS

BEGIN

SET @BeginDate = ISNULL(@BeginDate, '2022-01-01')

SET @EndDate = ISNULL(@EndDate, DATEADD(year, 2, GETDATE()))

DECLARE @N AS INT = 0

DECLARE @NumberOfDates INT = DATEDIFF(day,@BeginDate, @EndDate)

DECLARE @SQL AS NVARCHAR(MAX)

DECLARE @STR AS VARCHAR(MAX) = ''

WHILE @N \<= @NumberOfDates

BEGIN

SET @STR = @STR + CAST(DATEADD(day,@N,@BeginDate) AS VARCHAR(10))

IF @N \< @NumberOfDates

BEGIN

SET @STR = @STR + ','

END

SET @N = @N + 1;

END

SET @SQL = 'INSERT INTO dbo.dim_Date (\[DateKey\]) SELECT CAST(\[value\]
AS DATE) FROM STRING_SPLIT(@STR, '','')';

EXEC sys.sp_executesql @SQL, N'@STR NVARCHAR(MAX)', @STR;

UPDATE dbo.dim_Date

SET

\[DayOfMonth\] = DATEPART(day,DateKey)

,\[DayOfWeeK\] = DATEPART(dw,DateKey)

,\[DayOfWeekName\] = DATENAME(weekday, DateKey)

,\[Year\] = DATEPART(yyyy,DateKey)

,\[Month\] = DATEPART(month,DateKey)

,\[MonthName\] = DATENAME(month, DateKey)

,\[Quarter\] = DATEPART(quarter,DateKey)

,\[QuarterName\] = CONCAT('Q',DATEPART(quarter,DateKey))

END

GO

![A screenshot of a computer Description automatically
generated](./media/image213.png)

![A screenshot of a computer Description automatically
generated](./media/image214.png)

4.  From same query window, execute the above procedure by running the
    following script.

> **Copy**

/\* 3 - Load Dimension tables.sql \*/

Exec ETL.sp_Dim_Date_Load

![A screenshot of a computer Description automatically
generated](./media/image215.png)

![A screenshot of a computer Description automatically
generated](./media/image216.png)

5.  Rename the query for reference. Right-click on **SQL query** in
    Explorer and select **Rename**.

![A screenshot of a computer Description automatically
generated](./media/image217.png)

6.  In the **Rename** dialog box, under the **Name** field, enter +++
    **Load Dimension tables+++**, then click on the **Rename** button. 

![A screenshot of a computer Description automatically
generated](./media/image218.png)

## Task 3: Create the procedure to load the Symbol dimension

1.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next step.

![A screenshot of a computer Description automatically
generated](./media/image212.png)

2.  Similar to the date dimension, each stock symbol corresponds to a
    row in the Symbols dimension table. This table holds details of the
    stock, such as company name, and the market the stock is listed
    with.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query. This will create the
    procedure that will load the stock symbol dimension. We'll execute
    this in the pipeline to handle any new stocks that might enter the
    feed.

> **Copy**

/\* 3 - Load Dimension tables.sql \*/

CREATE PROC \[ETL\].\[sp_Dim_Symbol_Load\]

AS

BEGIN

DECLARE @MaxSK INT = (SELECT ISNULL(MAX(Symbol_SK),0) FROM
\[dbo\].\[dim_Symbol\])

INSERT \[dbo\].\[dim_Symbol\]

SELECT

Symbol_SK = @MaxSK + ROW_NUMBER() OVER(ORDER BY Symbol)

, Symbol

, Name

,Market

FROM

(SELECT DISTINCT

sdp.Symbol

, Name = 'Stock ' + sdp.Symbol

, Market = CASE SUBSTRING(Symbol,1,1)

WHEN 'B' THEN 'NASDAQ'

WHEN 'W' THEN 'NASDAQ'

WHEN 'I' THEN 'NYSE'

WHEN 'T' THEN 'NYSE'

ELSE 'No Market'

END

FROM

\[stg\].\[vw_StocksDailyPrices\] sdp

WHERE

sdp.Symbol NOT IN (SELECT Symbol FROM \[dbo\].\[dim_Symbol\])

) stg

END

GO

![A screenshot of a computer Description automatically
generated](./media/image219.png)

![A screenshot of a computer Description automatically
generated](./media/image220.png)

7.  Rename the query for reference. Right-click on **SQL query** in
    Explorer and select **Rename**.

![A screenshot of a computer Description automatically
generated](./media/image221.png)

8.  In the **Rename** dialog box, under the **Name** field, enter +++
    **Load the stock symbol dimension** **+++**, then click on the
    **Rename** button. 

![A screenshot of a computer Description automatically
generated](./media/image222.png)

## **Task 4: Create the views**

1.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next step.

![A screenshot of a computer Description automatically
generated](./media/image212.png)

2.  Create views that support the aggregation of the data during the
    load. When the pipeline runs, data is copied from the KQL database
    into our staging table, where we'll aggregate all of the data for
    each stock into a min, max, and closing price for each day.

3.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query.

/\* 4 - Create Staging Views.sql \*/

CREATE VIEW \[stg\].\[vw_StocksDailyPrices\]

AS

SELECT

Symbol = symbol

,PriceDate = datestamp

,MIN(price) as MinPrice

,MAX(price) as MaxPrice

,(SELECT TOP 1 price FROM \[stg\].\[StocksPrices\] sub

WHERE sub.symbol = prices.symbol and sub.datestamp = prices.datestamp

ORDER BY sub.timestamp DESC

) as ClosePrice

FROM

\[stg\].\[StocksPrices\] prices

GROUP BY

symbol, datestamp

GO

/\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*/

CREATE VIEW stg.vw_StocksDailyPricesEX

AS

SELECT

ds.\[Symbol_SK\]

,dd.DateKey as PriceDateKey

,MinPrice

,MaxPrice

,ClosePrice

FROM

\[stg\].\[vw_StocksDailyPrices\] sdp

INNER JOIN \[dbo\].\[dim_Date\] dd

ON dd.DateKey = sdp.PriceDate

INNER JOIN \[dbo\].\[dim_Symbol\] ds

ON ds.Symbol = sdp.Symbol

GO

![A screenshot of a computer Description automatically
generated](./media/image223.png)

![A screenshot of a computer Description automatically
generated](./media/image224.png)

4.  Rename the query for reference. Right-click on **SQL query** in
    Explorer and select **Rename**.

![A screenshot of a computer Description automatically
generated](./media/image225.png)

5.  In the **Rename** dialog box, under the **Name** field, enter +++
    **Create Staging Views** **+++**, then click on the **Rename**
    button. 

![A screenshot of a computer screen Description automatically
generated](./media/image226.png)

## Task 5: Add activity to load symbols

1.  On the **StockDW** page, click on **PL_Refresh_DWH** on the
    left-sided navigation menu.

![](./media/image227.png)

2.  In the pipeline, add a new ***Stored Procedure*** activity
    named ***Populate Symbols Dimension*** that executes the procedure,
    which loads the stock symbols.

3.  This should be connected to the success output of the ForEach
    activity (not within the ForEach activity).

> ![A screenshot of a computer Description automatically
> generated](./media/image228.png)

4.  On the **General** tab, in the **Name field,** enter +++**Populate
    Symbols Dimension** +++

> ![A screenshot of a computer Description automatically
> generated](./media/image229.png)

5.  Click on the **Settings** tab, enter the following settings.

[TABLE]

![](./media/image230.png)

## Task 6: Create the procedure to load daily prices

1.  On the **PL_Refresh_DWH** page, click on **StockDW** on the
    left-sided navigation menu.

![A screenshot of a computer Description automatically
generated](./media/image231.png)

2.  Click on ***New SQL query*** dropdown in the command bar, then
    select **New SQL query** under **Blank** section. We'll start
    building our schema in the next step.

![A screenshot of a computer Description automatically
generated](./media/image212.png)

3.  Next, run the below script to create the procedure that builds the
    fact table. This procedure merges data from staging into the fact
    table. If the pipeline is running throughout the day, the values
    will be updated to reflect any changes in the min, max, and closing
    price.

> **Note**: Currently, Fabric data warehouse does not support the T-SQL
> merge statement; therefore, data will be updated and then inserted as
> needed.

4.  In the query editor, copy and paste the following code. Click on
    the **Run** button to execute the query.

/\* 5 - ETL.sp_Fact_Stocks_Daily_Prices_Load.sql \*/

CREATE PROCEDURE \[ETL\].\[sp_Fact_Stocks_Daily_Prices_Load\]

AS

BEGIN

BEGIN TRANSACTION

UPDATE fact

SET

fact.MinPrice = CASE

WHEN fact.MinPrice IS NULL THEN stage.MinPrice

ELSE CASE WHEN fact.MinPrice \< stage.MinPrice THEN fact.MinPrice ELSE
stage.MinPrice END

END

,fact.MaxPrice = CASE

WHEN fact.MaxPrice IS NULL THEN stage.MaxPrice

ELSE CASE WHEN fact.MaxPrice \> stage.MaxPrice THEN fact.MaxPrice ELSE
stage.MaxPrice END

END

,fact.ClosePrice = CASE

WHEN fact.ClosePrice IS NULL THEN stage.ClosePrice

WHEN stage.ClosePrice IS NULL THEN fact.ClosePrice

ELSE stage.ClosePrice

END

FROM \[dbo\].\[fact_Stocks_Daily_Prices\] fact

INNER JOIN \[stg\].\[vw_StocksDailyPricesEX\] stage

ON fact.PriceDateKey = stage.PriceDateKey

AND fact.Symbol_SK = stage.Symbol_SK

INSERT INTO \[dbo\].\[fact_Stocks_Daily_Prices\]

(Symbol_SK, PriceDateKey, MinPrice, MaxPrice, ClosePrice)

SELECT

Symbol_SK, PriceDateKey, MinPrice, MaxPrice, ClosePrice

FROM

\[stg\].\[vw_StocksDailyPricesEX\] stage

WHERE NOT EXISTS (

SELECT \* FROM \[dbo\].\[fact_Stocks_Daily_Prices\] fact

WHERE fact.PriceDateKey = stage.PriceDateKey

AND fact.Symbol_SK = stage.Symbol_SK

)

COMMIT

END

GO

![A screenshot of a computer Description automatically
generated](./media/image232.png)

![A screenshot of a computer Description automatically
generated](./media/image233.png)

6.  Rename the query for reference. Right-click on **SQL query** in
    Explorer and select **Rename**.

![A screenshot of a computer Description automatically
generated](./media/image234.png)

7.  In the **Rename** dialog box, under the **Name** field, enter +++
    ETL.sp_Fact_Stocks_Daily_Prices_Load**+++**, then click on the
    **Rename** button. 

![A screenshot of a computer Description automatically
generated](./media/image235.png)

## Task 7: Add activity to the pipeline to load daily stock prices

1.  On the **StockDW** page, click on **PL_Refresh_DWH** on the
    left-sided navigation menu.

![A screenshot of a computer Description automatically
generated](./media/image236.png)

2.  Add another ***Stored Procedure*** activity to the pipeline
    named ***Populate Fact Stocks Daily Prices*** that loads the stocks
    prices from staging into the fact table. Connect the success output
    of the *Populate Symbols Dimension* to the new *Populate Fact Stocks
    Daily Prices* activity.

> ![A screenshot of a computer Description automatically
> generated](./media/image237.png)
>
> ![A screenshot of a computer Description automatically
> generated](./media/image238.png)

3.  Click on the **Settings** tab, enter the following settings.

[TABLE]

![A screenshot of a computer Description automatically
generated](./media/image239.png)

## Task 8. Run the pipeline

1.  Run the pipeline by clicking on the ***Run*** button, and verify the
    pipeline runs and fact and dimension tables are being loaded.

![A screenshot of a computer Description automatically
generated](./media/image240.png)

2.  In the **Save and run?** dialog box, click on **Save and run**
    button

> ![A screenshot of a computer Description automatically
> generated](./media/image200.png)

![A screenshot of a computer Description automatically
generated](./media/image241.png)

## Task 9: Schedule the pipeline

1.  Next, schedule the pipeline to run periodically. This will vary by
    business case, but this could be run frequently (every few minutes)
    or throughout the day.

> **Note**: In this specific case, as there are roughly 700k rows per
> day, and KQL limits the query results to 500k, the pipeline must run
> at least twice per day to stay current.

2.  To schedule the pipeline, click the ***Schedule* **button (next to
    the *Run* button) and set up a recurring schedule, such as hourly or
    every few minutes.

![A screenshot of a computer Description automatically
generated](./media/image242.png)

![A screenshot of a computer Description automatically
generated](./media/image243.png)

# Exercise 7: Semantic Modeling

One last step is to operationalize the data by creating a semantic model
and viewing the data in Power BI. 

## Task 1: Create a semantic model

A semantic model, conceptually, provides an abstraction of our data for
consumption in business analytics. Typically, we expose data in our data
warehouse via semantic model that is used in Power BI. At a basic level,
they will include the relationships between tables.

*Note: Power BI Datasets have recently been renamed to Semantic Models.
In some cases, labels may not have been updated. The terms can be used
interchangeably. Read more about this change [on the Power BI
Blog](https://powerbi.microsoft.com/en-us/blog/datasets-renamed-to-semantic-models/).*

When we created our data warehouse, a default semantic model was created
automatically. We can leverage this in Power BI, but it also includes
many artifacts of the table we may not need. So, we'll create a new
semantic model with just our fact and two-dimension tables.

1.  On the **PL_Refresh_DWH** page, click on **StockDW** on the
    left-sided navigation menu.

![A screenshot of a computer Description automatically
generated](./media/image244.png)

2.  Click on the **refresh** icon as shown in the below image.

![A screenshot of a computer Description automatically
generated](./media/image245.png)

![A screenshot of a computer Description automatically
generated](./media/image246.png)

3.  In StockDW page, select the ***Reporting* **tab and then
    select ***New semantic model***.

> ![A screenshot of a computer Description automatically
> generated](./media/image247.png)

4.  In the New semantic model tab, enter the name as ***StocksModel*,**
    and select only the fact and dimensions table, as we are concerned
    with ***fact_Stocks_Daily_Prices*, *dim_Date*, and *dim_Symbol***.
    Click on the **Confirm** button.

![A screenshot of a computer Description automatically
generated](./media/image248.png)

## Task 2. Add relationships

1.  On the **StockDW** page, click on **RealTimeWorkspace** on the
    left-sided navigation menu and select **StockModel**.

![A screenshot of a computer Description automatically
generated](./media/image249.png)

2.  The model designer should automatically open after creating the
    semantic model above. If it doesn't, or if you'd like to return to
    the designer at a later time, you can do so by opening the model
    from the list of resources in the workspace, and then
    selecting ***Open Data Model*** from the semantic model item.

![A screenshot of a computer Description automatically
generated](./media/image250.png)

![A screenshot of a computer Description automatically
generated](./media/image251.png)

3.  To create relationships between the fact and dimension tables, drag
    the key from the fact table to the corresponding key in the
    dimension table.

4.  For this data model, you need to define the relationship between
    different tables so that you can create reports and visualizations
    based on data coming across different tables. From
    the **fact_Stocks_Daily_Prices** table, drag
    the **PriceDateKey** field and drop it on the **DateKey** field in
    the **dim_Date** table to create a relationship. The **New
    relationship** dialog box appears.

> ![A screenshot of a computer Description automatically
> generated](./media/image252.png)

5.  In the **New relationship** dialog box:

- **From table** is populated with **fact_Stocks_Daily_Prices** and the
  column of **PriceDateKey.**

- **To table** is populated with **dim_Date**  and the column of
  **DateKey**

- Cardinality: **Many to one (\*:1)**

- Cross filter direction: **Single**

- Leave the box next to **Make this relationship active** selected.

- Select **Ok.**

![A screenshot of a computer Description automatically
generated](./media/image253.png)

![A screenshot of a computer Description automatically
generated](./media/image254.png)

6.  From the **fact_Stocks_Daily_Prices** table, drag
    the **Symbol_SK** field and drop it on the **Symbol_SK**  field in
    the **dim_Symbol** table to create a relationship. The **New
    relationship** dialog box appears.

![A screenshot of a computer Description automatically
generated](./media/image255.png)

7.  In the **New relationship** dialog box:

- **From table** is populated with **fact_Stocks_Daily_Prices** and the
  column of **Symbol_Sk.**

- **To table** is populated with **dim_Symabol**  and the column of
  **Symbol_Sk**

- Cardinality: **Many to one (\*:1)**

- Cross filter direction: **Single**

- Leave the box next to **Make this relationship active** selected.

- Select **Ok.**

![A screenshot of a computer Description automatically
generated](./media/image256.png)

![A screenshot of a computer Description automatically
generated](./media/image257.png)

## Task 3. Create a simple report

1.  Click on ***New Report*** to load the semantic model in Power BI.

> ![A screenshot of a computer Description automatically
> generated](./media/image258.png)

2.  While we won't have much data yet to make much of a report,
    conceptually, we can build a report similar to below, which shows a
    report after the lab has been running for a week or so (The
    lakehouse module will import additional history to allow for more
    interesting reports). The top chart shows the closing price for each
    stock on each day, while the bottom one shows the high/low/close of
    the WHO stock.

3.  In the **Power BI** page, under **Visualizations**, click to the
    **Line chart** icon to add a **Column chart** to your report.

- On the **Data** pane, expand **fact_Stocks_Daily_Prices**  and check
  the box next to **PriceDateKey**. This creates a column chart and adds
  the field to the **X-axis**.

- On the **Data** pane, expand **fact_Stocks_Daily_Prices** and check
  the box next to **ClosePrice**. This adds the field to the **Y-axis.**

- On the **Data** pane, expand **dim_Symbol** and check the box next
  to **Symbol**. This adds the field to the **Legend**.

![A screenshot of a computer Description automatically
generated](./media/image259.png)

![A screenshot of a computer Description automatically
generated](./media/image260.png)

4.  From the ribbon, select **File** \> **Save.**

![A screenshot of a computer Description automatically
generated](./media/image261.png)

5.  In the Save your report dialog box, enter +++ **semantic
    report** +++ as the name of your report and select **your
    workspace**. Click on the **Save button**.

![A screenshot of a computer Description automatically
generated](./media/image262.png)

![A screenshot of a computer Description automatically
generated](./media/image263.png)

## **Summary**

In this lab, you’ve signed up for the Microsoft Fabric trial and
redeemed Azure Pass, followed by configuring permissions and creating
necessary resources within the Azure portal such as Fabric Capacity,
Workspace, and Storage Account. Then, you’ve deployed the stock
generator app via Azure Container Instance using an ARM template to
generate real-time stock data. Additionally, you’ve configured
Eventstream in Microsoft Fabric to ingest data from Azure Event Hubs and
prepared the KQL Database to store this data efficiently. In this lab,
you’ve established a fully functional environment to proceed with
subsequent lab exercises related to real-time analytics and data
processing.

You embarked on a comprehensive exploration of stock price data using
Kusto Query Language (KQL). Starting with the creation of a KQL queryset
named "StockQueryset," you executed a series of increasingly complex
queries to analyze various facets of the data. From viewing sample
records to calculating price differentials over time and identifying
significant price gains, each query unveils valuable insights into the
dynamics of stock prices. By leveraging windowing functions, aggregation
techniques, and data partitioning, you gained a deeper understanding of
stock price trends and fluctuations.

Then, you’ve shifted the focus to optimizing Power BI reporting
efficiency, where you configured refresh rates and crafted dynamic
visualizations for real-time dashboards. By configuring refresh rates in
the Power BI admin portal and creating Power BI reports based on the
previously defined KQL queries, you ensured timely updates and enabled
insightful visualization of stock price data. Through tasks like
creating visualizations for percent change and configuring auto-refresh
settings, you harnessed the full potential of Power BI to drive informed
decision-making and enhanced business intelligence capabilities.

You’ve configured a Synapse Data Warehouse in the Fabric workspace and
established a robust data pipeline for data processing. You’ve begun
this lab with creating the Synapse Data Warehouse and then proceeded to
create staging and ETL objects necessary for data transformation. You’ve
created schemas, tables, stored procedures, and pipelines to manage data
flow efficiently.

Then, you’ve delved into building dimension and fact tables essential
for organizing data effectively for analytical purposes. You’ve created
tables for storing daily stock prices, symbol details, and date
information. Additionally, procedures are developed to load dimension
tables with relevant data and populate fact tables with daily stock
prices.

You’ve created a semantic model in Synapse Data Warehouse, focusing on
essential fact and dimension tables. After establishing the semantic
model named "StocksModel," you’ve established relationships between the
fact_Stocks_Daily_Prices table and the dim_Date and dim_Symbol tables to
enable cohesive data analysis. Overall, this lab provides a
comprehensive understanding of setting up a data warehouse environment
and building a reliable data pipeline for analytics.
