# JupyterLab user guide

JupyterLab is a web-based user interface for <a href="https://jupyter.org/" target="_blank">Project Jupyter</a> and is tightly integrated into Adobe Experience Platform. It provides an interactive development environment for data scientists to work with Jupyter notebooks, code, and data.

This document provides an overview of JupyterLab and its features as well as instructions to perform common actions:
-   [JupyterLab on Adobe Experience Platform](#jupyterlab-on-adobe-experience-platform)
-   [Integration with other Platform services](#integration-with-other-platform-services)
-   [Key features and common operations](#key-features-and-common-operations)
    -   [Access JupyterLab](#access-jupyterlab)
    -   [JupyterLab interface](#jupyterlab-interface)
    -   [Code cells](#code-cells)
    -   [Kernels](#kernels)
    -   [Kernel sessions](#kernel-sessions)
    -   [PySpark/Spark execution resource](#pysparkspark-execution-resource)
    -   [Launcher](#launcher)
-   [Access Platform data using Notebooks](#access-platform-data-using-notebooks)
    -   [Read from a dataset in Python/R](#read-from-a-dataset-in-pythonr)
    -   [Read from a dataset in PySpark/Spark](#read-from-a-dataset-in-pysparkspark)
    -   [Query data using Query Service in Python](#query-data-using-query-service-in-python)
    -   [Filter ExperienceEvent data in Python/R](#filter-experienceevent-data-in-pythonr)
    -   [Filter ExperienceEvent data in PySPark/Spark](#filter-experienceevent-data-in-pysparkspark)

The [appendix](#appendix) to this document includes additional useful resources related to JupyterLab, including:
-   [Supported libraries](#supported-libraries)
-   [Optional SQL flags for Query Service](#optional-sql-flags-for-query-service)

## JupyterLab on Adobe Experience Platform

The JupyterLab integration for Experience Platform is accompanied with architectural changes, design considerations, customized notebook extensions, pre-installed libraries, and an Adobe themed interface.

The following list outlines some of the features that are unique to JupyterLab on Platform:

*   **Kernels**
    *   Kernels provide notebook and other JupyterLab front-ends the ability to execute and introspect code in different programming languages. Experience Platform provides additional kernels to support development in Python, R, PySpark, and Spark. See the [kernels](#kernels) section for more details.
*   **Data access**
    *   Access existing datasets directly from within JupyterLab with full support for read and write capabilities.
*   **Platform service integration**
    *   Built-in integrations allows you to utilize other Platform services directly from within JupyterLab. A complete list of supported integrations is provided in the section on [Integration with other Platform services](#integration-with-other-platform-services).
*   **Authentication**
    *   In addition to <a href="https://jupyter-notebook.readthedocs.io/en/latest/security.html" target="_blank">JupyterLab's built-in security model</a>, every interaction between your application and Experience Platform, including Platform service-to-service communication is encrypted and authenticated through the <a href="https://www.adobe.io/authentication/auth-methods.html" target="_blank">Adobe Identity Management System (IMS)</a>.
*   **Development libraries**
    *   In Experience Platform, JupyterLab provides pre-installed libraries for Python, R, and PySpark. See the [appendix](#appendix-supported-libraries) for a complete list of supported libraries.
*   **Library controller**
    *   When the the pre-installed libraries are lacking for your needs, additional libraries can be installed for Python and R, and are temporarily stored in isolated containers to maintain the integrity of Platform and keep your data safe. See the [kernels](#kernels) section for more details.
        >   **Note:** Additional libraries are only available for the session in which they were installed. You must reinstall any additional libraries you require when starting new sessions.

## Integration with other Platform services

Standardization and interoperability are key concepts behind Experience Platform. The integration of JupyterLab on Platform as an embedded IDE allows it to interact with other Platform services, enabling you to utilize Platform to its full potential. The following Platform services are available in JupyterLab:

*   **Data Catalog:** Access and explore datasets with read and write functionalities.
*   **Query Service:** Access and explore datasets using SQL, providing lower data access overheads when dealing with large amounts of data.
*   **Sensei ML Framework:** Model development with the ability to train and score data, as well as recipe creation with a single click.

>   **Note:** Some Platform service integrations on JupyterLab are limited to specific kernels. Refer to the section on [kernels](#kernels) for more details.

## Key features and common operations

Information regarding key features of JupyterLab and instructions on performing common operations are provided in the sections below:

-   [Access JupyterLab](#access-jupyterlab)
-   [JupyterLab interface](#jupyterlab-interface)
-   [Code cells](#code-cells)
-   [Kernels](#kernels)
-   [Kernel sessions](#kernel-sessions)
-   [PySpark/Spark execution resource](#pysparkspark-execution-resource)
-   [Launcher](#launcher)
-   [GPU and memory configuration](#gpu-and-memory-server-configuration-in-pythonr)

### Access JupyterLab

In <a href="https://platform.adobe.com" target="_blank">Adobe Experience Platform</a>, click **Models** from the left navigation column, then click **Notebooks** found in the top navigation to access JupyterLab. Allow some time for JupyterLab to fully initialize.

![](./images/access_jupyterlab.png)

### JupyterLab interface

The JupyterLab interface consists of a menu bar, a collapsible left sidebar, and the main work area containing tabs of documents and activities.

**Menu bar**

The menu bar at the top of the interface has top-level menus that expose actions available in JupyterLab with their keyboard shortcuts:

*   **File:** Actions related to files and directories
*   **Edit:** Actions related to editing documents and other activities
*   **View:** Actions that alter the appearance of JupyterLab
*   **Run:** Actions for running code in different activities such as notebooks and code consoles
*   **Kernel:** Actions for managing kernels
*   **Tabs:** A list of open documents and activities
*   **Settings:** Common settings and an advanced settings editor
*   **Help:** A list of JupyterLab and kernel help links

**Left sidebar**

The left sidebar contains clickable tabs that provides access to the following features:

*   **File browser:** A list of saved notebook documents and directories
*   **Data explorer:** Browse, access, and explore datasets and schemas
*   **Running kernels and terminals:** A list of active kernel and terminal sessions with the ability to terminate
*   **Commands:** A list of useful commands
*   **Cell inspector:** A cell editor that provides access to tools and metadata useful for setting up a notebook for presentation purposes
*   **tabs:** A list of open tabs

Click on a tab to expose its features, or click on an expanded tab to collapse the left sidebar as demonstrated below:

![](./images/left_sidebar_collapse.gif)

**Main work area**

The main work area in JupyterLab enables you to arrange documents and other activities into panels of tabs that can be resized or subdivided. Drag a tab to the center of a tab panel to migrate the tab. Divide a panel by dragging a tab to the left, right, top, or bottom of the panel:

![](./images/main_work_area.gif)

### Code cells

Code cells are the primary content of notebooks. They contain source code in the language of the notebook's associated kernel and the output as a result of executing the code cell. An execution count is displayed to the right of every code cell which represents its order of execution.

![](./images/code_cell.png)

Common cell actions are described below:

*   **Add a cell:** Click the plus symbol (**+**) from the notebook menu to add an empty cell. New cells are placed under the cell that is currently being interacted with, or at the end of the notebook if no particular cell is in focus.

*   **Move a cell:** Place your cursor to the right of the cell you wish to move, then click and drag the cell to a new location. Additionally, moving a cell from one notebook to another replicates the cell along with its contents.

*   **Execute a cell:** Click on the body of the cell you wish to execute and then click the **play** icon (**▶**) from the notebook menu. An asterisk (**\***) is displayed in the cell's execution counter when the kernel is processing the execution, and is replaced with an integer upon completion.

*   **Delete a cell:** Click on the body of the cell you wish to delete and then click the **scissor** icon.

### Kernels

Notebook kernels are the language-specific computing engines for processing notebook cells. In addition to Python, JupyterLab provides additional language support in R, PySpark, and Spark. When you open a notebook document, the associated kernel is launched. When a notebook cell is executed, the kernel performs the computation and produces results which may consume significant CPU and memory resources. Note that allocated memory is not freed until the kernel is shut-down.

>   **Note:** PySpark and Spark functionalities are supported by <a href="https://github.com/jupyter-incubator/sparkmagic" target="_blank">Sparkmagic</a>.

Certain features and functionalities are limited to particular kernels as described in the table below:

| Kernel | Library installation support | Platform integrations |
| :----: | :--------------------------: | :-------------------- |
| **Python** | yes | <ul><li>Sensei ML Framework</li><li>Data Catalog</li><li>Query Service</li></ul> |
| **R** | yes | <ul><li>Sensei ML Framework</li><li>Data Catalog</li></ul> |
| **PySpark** | no | <ul><li>Sensei ML Framework</li><li>Data Catalog</li></ul> |
| **Spark** | no | <ul><li>Sensei ML Framework</li><li>Data Catalog</li></ul> |

### Kernel sessions

Each active notebook or activity on JupyterLab utilizes a kernel session. All active sessions can be found by expanding the **Running terminals and kernels** tab from the left sidebar. The type and state of the kernel for a notebook can be identified by observing the top right of the notebook interface. In the diagram below, the notebook's associated kernel is **Python 3** and the its current state is represented by a grey circle to the right. A hollow circle implies an idling kernel and a solid circle implies a busy kernel.

![](./images/kernel_and_state_1.png)

If the kernel is shut-down or inactive for a prolonged period, then **No Kernel!** with a solid circle is shown. Activate a kernel by clicking the kernel status and selecting the appropriate kernel type as demonstrated below:

![](./images/switch_kernel.gif)

### PySpark/Spark execution resource

PySpark and Spark kernels allows you to configure Spark cluster resources within your PySpark or Spark notebook by using the configure command (`%%configure`) and providing a list of configurations. Ideally, these configurations are defined before the Spark application is initialized. Modifying the configurations while the Spark application is active requires an additional force flag after the command (`%%configure -f`) which will restart the application in order for the changes to be applied, as shown below:

```python
%%configure -f 
{
    "numExecutors": 10,
    "executorMemory": "8G",
    "executorCores":4,
    "driverMemory":"2G",
    "driverCores":2,
    "conf": {
        "spark.cores.max": "40"
    }
}
```

>   **Note:** Use the help command (`%%help`) to view all commands available.

All configurable properties are listed in the table below:

| Property | Description | Type |
| :------- | :---------- | :-----:|
| kind | The session kind (required) | `session kind`_ |
| proxyUser | The user to impersonate that will run this session (For example bob) | string |
| jars | Files to be placed on the java `classpath` | list of paths |
| pyFiles | Files to be placed on the `PYTHONPATH` | list of paths |
| files | Files to be placed in executor working directory | list of paths |
| driverMemory | Memory for driver in megabytes or gigabytes (For example 1000M, 2G) | string |
| driverCores | Number of cores used by driver (YARN mode only) | int |
| executorMemory | Memory for executor in megabytes or gigabytes (For example 1000M, 2G) | string |
| executorCores | Number of cores used by executor | int |
| numExecutors | Number of executors (YARN mode only) | int |
| archives | Archives to be uncompressed in the executor working directory (YARN mode only) | list of paths |
| queue | The YARN queue to submit too (YARN mode only) | string |
| name | Name of the application | string |
| conf | Spark configuration property | Map of key=val |

### Launcher

[//]: # (Talk about the different Notebooks, introduce that certain starter notebooks are limited to particular kernels)

The customized *Launcher* provides you with useful notebook templates for their supported kernels to help you kickstart your task, including:

*   **Blank:** An empty notebook file.
*   **Starter:** A pre-filled notebook demonstrating data exploration using sample data.
*   **Retail Sales:** A pre-filled notebook featuring the <a href="https://adobe.ly/2wOgO3L" target="_blank">Retail Sales Recipe</a> using sample data.
*   **Recipe Builder:** A notebook template for creating a recipe in JupyterLab. It is pre-filled with code and commentary that demonstrates and describes the recipe creation process. Refer to the <a href="https://www.adobe.com/go/data-science-create-recipe-notebook-tutorial-en" target="_blank">notebook to recipe tutorial</a> for a detailed walkthrough.
*   **Query Service:** A pre-filled notebook demonstrating the usage of Query Service directly in JupyterLab with provided sample workflows that analyzes data at scale.
*   **XDM Events:** A pre-filled notebook demonstrating data exploration on postvalue Experience Event data, focusing on features common across the data structure.
*   **XDM Queries:** A pre-filled notebook demonstrating sample business queries on Experience Event data.
*   **Aggregation:** A pre-filled notebook demonstrating sample workflows to aggregate large amounts of data into smaller, manageable chunks.
*   **Clustering:** A pre-filled notebook demonstrating the end-to-end machine learning modeling process using clustering algorithms.

Some notebook templates are limited to certain kernels. Template availability for each kernel is mapped in the following table:

<table>
    <tr>
        <td></td>
        <th scope="col"><strong>Blank</strong></th>
        <th scope="col"><strong>Starter</strong></th>
        <th scope="col"><strong>Retail Sales</strong></th>
        <th scope="col"><strong>Recipe Builder</strong></th>
        <th scope="col"><strong>Query Service</strong></th>
        <th scope="col"><strong>XDM Events</strong></th>
        <th scope="col"><strong>XDM Queries</strong></th>
        <th scope="col"><strong>Aggregation</strong></th>
        <th scope="col"><strong>Clustering</strong></th>
    </tr>
    <tr>
        <th scope="row" style="text-align:left"><strong>Python</strong></th>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
    </tr>
    <tr>
        <th scope="row" style="text-align:left"><strong>R</strong></th>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
    </tr>
    <tr>
        <th scope="row" style="text-align:left"><strong>PySpark</strong></th>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
    </tr>
    <tr>
        <th scope="row" style="text-align:left"><strong>Spark</strong></th>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#ff6666">no</td>
        <td style="text-align:center; background-color:#66ff66">yes</td>
    </tr>
</table>

To open a new *Launcher*, click **File > New Launcher**. Alternatively, expand the **File browser** from the left sidebar and click the plus symbol (**+**):

![](./images/new_launcher.gif)

### GPU and memory server configuration in Python/R

In JupyterLab select the gear icon in the top-right corner to open *Notebook server configuration*. You can toggle on GPU and allocate the amount of memory you need by using the slider. The amount of memory you can allocate depends on how much your organization has provisioned. Select **Update configs** to save.

> **Note:** Only one GPU is provisioned per organization for Notebooks. If the GPU is in use, you need to wait for the user that has currently reserved the GPU to release it. This can be done by logging out or leaving the GPU in an idle state for four or more hours.

![](./images/notebook-gpu-config.png)

## Access Platform data using Notebooks

Each supported kernel provides built-in functionalities that allow you to read Platform data from a dataset within a notebook. However, support for paginating data is limited to Python and R notebooks.

### Read from a dataset in Python/R

Python and R notebooks allow you to paginate data when accessing datasets. Sample code to read data with and without pagination is demonstrated below.

[//]: # (In the following samples, the first step is currently required but once the SDK is complete, users are no longer required to explicitly define client_context)

**Read from a dataset in Python/R without pagination**

Executing the following code will read the entire dataset. If the execution is successful, then data will be saved as a Pandas dataframe referenced by the variable `df`.

```python
# Python

client_context = PLATFORM_SDK_CLIENT_CONTEXT
from platform_sdk.dataset_reader import DatasetReader
dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.read()
df.head()
```

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT
DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$read() 
df
```

*   `{DATASET_ID}`: The unique identity of the dataset to be accessed

**Read from a dataset in Python/R with pagination**

Executing the following code will read data from the specified dataset. Pagination is achieved by limiting and offsetting data through the functions `limit()` and `offset()` respectively. Limiting data refers to the maximum number of data points to be read, while offsetting refers to the number of data points to skip prior to reading data. If the read operation executes successfully, then data will be saved as a Pandas dataframe referenced by the variable `df`.

```python
# Python

client_context = PLATFORM_SDK_CLIENT_CONTEXT
from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.limit(100).offset(10).read()
```

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT

DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$limit(100L)$offset(10L)$read() 
```

*   `{DATASET_ID}`: The unique identity of the dataset to be accessed

### Read from a dataset in PySpark/Spark

With an an active PySpark or Spark notebook opened, expand the **Data Explorer** tab from the left sidebar and double click **Datasets** to view a list of available datasets. Right click on the dataset listing you wish to access and click **Explore Data in Notebook**. The following code cells are generated:

```python
# PySpark

pd0 = spark.read.format("com.adobe.platform.dataset").\
    option('orgId', "YOUR_IMS_ORG_ID@AdobeOrg").\
    load("{DATASET_ID}")
pd0.describe()
pd0.show(10, False)
```

```scala
// Spark

import com.adobe.platform.dataset.DataSetOptions
val dataFrame = spark.read.
    format("com.adobe.platform.dataset").
    option(DataSetOptions.orgId, "YOUR_IMS_ORG_ID@AdobeOrg").
    load("{DATASET_ID}")
dataFrame.printSchema()
dataFrame.show()
```

### Query data using Query Service in Python

JupyterLab on Platform allows you to use SQL in a Python notebook to access data through <a href="https://www.adobe.com/go/query-service-home-en" target="_blank">Adobe Experience Platform Query Service</a>. Accessing data through Query Service can be useful for dealing with large datasets due to its superior running times. Be advised that querying data using Query Service has a processing time limit of ten minutes.

Before you use Query Service in JupyterLab, ensure you have a working understanding of the <a href="https://www.adobe.com/go/query-service-sql-syntax-en" target="_blank">Query Service SQL syntax</a>.

Querying data using Query Service requires you to provide the name of the target dataset. You can generate the necessary code cells by finding the desired dataset using the **Data explorer**. Right click on the dataset listing and click **Query Data in Notebook** to generate the following two code cells in your notebook:

<ol>
<li>

In order to utilize Query Service in JupyterLab, you must first create a connection between your working Python notebook and Query Service. This can be achieved by executing the first generated cell.

```python
qs_connect()
```

</li>
<li>

In the second generated cell, the first line must be defined before the SQL query. By default, the generated cell defines an optional variable (`df0`) which saves the query results as a Pandas dataframe. <br>The `-c QS_CONNECTION` argument is mandatory and tells the kernel to execute the SQL query against Query Service. See the [appendix](#appendix-optional-sql-flags-for-query-service) for a list of additional arguments.

```python
%%read_sql df0 -c QS_CONNECTION
SELECT *
FROM name_of_the_dataset
LIMIT 10
/* Querying table "name_of_the_dataset" (datasetId: {DATASET_ID})*/
```

</li>
</ol>

Python variables can be directly referenced within a SQL query by using string-formatted syntax and wrapping the variables in curly brackets (`{}`), as shown in the following example:

```python
table_name = 'name_of_the_dataset'
table_columns = ','.join(['col_1','col_2','col_3'])
```

```python
%%read_sql demo -c QS_CONNECTION
SELECT {table_columns}
FROM {table_name}
```

### Filter ExperienceEvent data in Python/R

In order to access and filter an ExperienceEvent dataset in a Python or R notebook, you must provide the ID of the dataset (`{DATASET_ID}`) along with the filter rules that define a specific time range using logical operators. When a time range is defined, any specified pagination is ignored and the entire dataset is considered. 

A list of filtering operators are described below: 

*   `eq()`: Equal to
*   `gt()`: Greater than
*   `ge()`: Greater than or equal to
*   `lt()`: Less than
*   `le()`: Less than or equal to
*   `And()`: Logical AND operator
*   `Or()`: Logical OR operator

The following cells filter an ExperienceEvent dataset to data existing exclusively between January 1, 2019 and the end of December 31, 2019.

```python
# Python

client_context = PLATFORM_SDK_CLIENT_CONTEXT
from platform_sdk.dataset_reader import DatasetReader

dataset_reader = DatasetReader(client_context, "{DATASET_ID}")
df = dataset_reader.\
    where(dataset_reader["timestamp"].gt("2019-01-01 00:00:00").\
    And(dataset_reader["timestamp"].lt("2019-12-31 23:59:59"))\
).read()
```

```R
# R

library(reticulate)
use_python("/usr/local/bin/ipython")
psdk <- import("platform_sdk")
py_run_file("../.ipython/profile_default/startup/platform_sdk_context.py")
client_context <- py$PLATFORM_SDK_CLIENT_CONTEXT

DatasetReader <- psdk$dataset_reader$DatasetReader
dataset_reader <- DatasetReader(client_context, "{DATASET_ID}") 
df <- dataset_reader$
    where(dataset_reader["timestamp"]$gt("2019-01-01 00:00:00")$
    And(dataset_reader["timestamp"]$lt("2019-12-31 23:59:59"))
)$read()
```

### Filter ExperienceEvent data in PySpark/Spark

Accessing and filtering an ExperienceEvent dataset in a PySpark or Spark notebook requires you to provide the dataset identity (`{DATASET_ID}`), your organization's IMS identity, and the filter rules defining a specific time range. A Filtering time range is defined by using the function `spark.sql()`, where the function parameter is a SQL query string.

The following cells filter an ExperienceEvent dataset to data existing exclusively between January 1, 2019 and the end of December 31, 2019.

```python
# PySpark

pd = spark.read.format("com.adobe.platform.dataset").\
    option("orgId", "YOUR_IMS_ORG_ID@AdobeOrg").\
    load("{DATASET_ID}")

pd.createOrReplaceTempView("event")
timepd = spark.sql("""
    SELECT *
    FROM event
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
```

```scala
// Spark

import com.adobe.platform.dataset.DataSetOptions
val dataFrame = spark.read.
    format("com.adobe.platform.dataset").
    option(DataSetOptions.orgId, "YOUR_IMS_ORG_ID@AdobeOrg").
    load("{DATASET_ID}")

dataFrame.createOrReplaceTempView("event")
val timedf = spark.sql("""
    SELECT * 
    FROM event 
    WHERE timestamp > CAST('2019-01-01 00:00:00.0' AS TIMESTAMP)
    AND timestamp < CAST('2019-12-31 23:59:59.9' AS TIMESTAMP)
""")
```
## Next steps 

For additional information, such as the workflow involved in building and deploying a model, please begin by reading the [Creating a Recipe using Jupyter notebooks](https://www.adobe.io/apis/experienceplatform/home/tutorials/data-science-workspace/dsw-tutorials/dsw-all-tutorials.html#!api-specification/markdown/narrative/tutorials/dsw/author_a_model/create_a_recipe_using_jupyter_notebooks/create_a_recipe_using_jupyter_notebooks.md) documentation.

## Appendix

The following sections contain supplemental information for working with the JupyterLab integration for Experience Platform.

### Supported libraries

Click the kernels below to show their supported libraries:

<details>
<summary>Python / R</summary>

Library | Version
:------ | :------
notebook | 6.0.0
requests | 2.22.0
plotly | 4.0.0
folium | 0.10.0
ipywidgets | 7.5.1
bokeh | 1.3.1
gensim | 3.7.3
ipyparallel | 0.5.2
jq | 1.6
keras | 2.2.4
nltk | 3.2.5
pandas | 0.22.0
pandasql | 0.7.3
pillow | 6.0.0
scikit-image | 0.15.0
scikit-learn | 0.21.3
scipy | 1.3.0
scrapy | 1.3.0
seaborn | 0.9.0
statsmodels | 0.10.1
elastic | 5.1.0.17
ggplot | 0.11.5
py-xgboost | 0.90
opencv | 3.4.1
pyspark | 2.4.3
pytorch | 1.0.1
wxpython | 4.0.6
colorlover | 0.3.0
geopandas | 0.5.1
pyshp | 2.1.0
shapely | 1.6.4
rpy2 | 2.9.4
r-essentials | 3.6
r-arules | 1.6_3
r-fpc | 2.2_3
r-e1071 | 1.7_2
r-gam | 1.16.1
r-gbm | 2.1.5
r-ggthemes | 4.2.0
r-ggvis | 0.4.4
r-igraph | 1.2.4.1
r-leaps | 3.0
r-manipulate | 1.0.1
r-rocr | 1.0_7
r-rmysql | 0.10.17
r-rodbc | 1.3_15
r-rsqlite | 2.1.2
r-rstan | 2.19.2
r-sqldf | 0.4_11
r-survival | 2.44_1.1
r-zoo | 1.8_6
r-stringdist | 0.9.5.2
r-quadprog | 1.5_7
r-rjson| 0.2.20
r-forecast | 8.7
r-rsolnp | 1.16
r-reticulate | 1.12
r-mlr | 2.14.0
r-viridis | 0.5.1
r-corrplot | 0.84
r-fnn | 1.1.3
r-lubridate | 1.7.4
r-randomforest | 4.6_14
r-tidyverse | 1.2.1
r-tree | 1.0_39
pymongo | 3.8.0
pyarrow | 0.14.1
boto3 | 1.9.199
ipyvolume | 0.5.2
fastparquet | 0.3.2
python-snappy | 0.5.4
ipywebrtc | 0.5.0
jupyter_client | 5.3.1
wordcloud | 1.5.0
graphviz | 2.40.1
python-graphviz | 0.11.1
azure-storage | 0.36.0
jupyterlab | 1.0.4
pandas_ml | 0.6.1
tensorflow-gpu | 1.14.0
nodejs | 12.3.0
mock | 3.0.5
ipympl | 0.3.3
fonts-anacond1
psycopg2 | 2.8.3
nose | 1.3.7
autovizwidget | 0.12.9
altair | 3.1.0
vega_datasets | 0.7.0
papermill | 1.0.1
sql_magic | 0.0.4
iso3166 | 1.0
nbimporter | 0.3.1

</details>

<details>
<summary>PySpark</summary>

Library | Version
:------ | :------
requests | 2.18.4
gensim | 2.3.0
keras | 2.0.6
nltk | 3.2.4
pandas | 0.20.1
pandasql | 0.7.3
pillow | 5.3.0
scikit-image | 0.13.0
scikit-learn | 0.19.0
scipy | 0.19.1
scrapy | 1.3.3
statsmodels | 0.8.0
elastic | 4.0.30.44
py-xgboost | 0.60
opencv | 3.1.0
pyarrow | 0.8.0
boto3 | 1.5.18
azure-storage-blob | 1.4.0
python | 3.6.7
mkl-rt | 11.1

</details>


### Optional SQL flags for Query Service

<table>
    <tr>
        <th style="text-align:left"><strong>Flag</strong></th>
        <th style="text-align:left"><strong>Description</strong></th>
    </tr>
    <tr>
        <td style="text-align:left" nowrap>
            <code>-h</code>, <code>--help</code>
        </td>
        <td style="text-align:left">
            Show the help message and exit.
        </td>
    </tr>
    <tr>
        <td style="text-align:left" nowrap>
            <code>-n</code>, <code>--notify</code>
        </td>
        <td style="text-align:left">
            Toggle option for notifying query results.
        </td>
    </tr>
    <tr>
        <td style="text-align:left" nowrap>
            <code>-a</code>, <code>--async</code>
        </td>
        <td style="text-align:left">
            Using this flag will execute the query asynchonously and can free up the kernel while the query is executing. Be cautious when assigning query results to variables as it may be undefined if the query is not complete.
        </td>
    </tr>
    <tr>
        <td style="text-align:left" nowrap>
            <code>-d</code>, <code>--display</code>
        </td>
        <td style="text-align:left">
            Using this flag will prevent results from being displayed.
        </td>
    </tr>
</table>

## Additional resources 

The following video outlines how to load Adobe Analytics data, select features, and explore data using the JupyterLab integration for Adobe Experience Platform.

<a href="https://docs.adobe.com/content/help/en/platform-learn/tutorials/data-science-workspace/build-and-deploy-a-model.html" target="_blank"><img src="./images/using_dsw_build_and_deploy.png"></a>