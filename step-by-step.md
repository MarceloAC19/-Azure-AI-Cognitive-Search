# Step-by-Step: Exploring an Azure AI Search Index (UI)

This document details the process of creating, configuring, and exploring an index in Azure AI Search, using the Azure portal interface. The guide was adapted from the [Explore an Azure AI Search index](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html) lab by Microsoft.

## Prerequisites

* An Azure subscription.
* A provisioned **Azure AI Search** resource.
* An **Azure Blob Storage** resource with data for indexing (e.g., documents, images).

---

### **Step 1: Provision an Azure AI Search Resource**

1.  In the Azure portal, click **+ Create a resource**.
2.  Search for "Azure AI Search" and click **Create**.
3.  Configure the resource details:
    * **Subscription**: Select your subscription.
    * **Resource Group**: Create a new one or select an existing one (e.g., `rg-ai-search-lab`).
    * **Service Name**: Provide a globally unique name (e.g., `my-search-service-unique`).
    * **Location**: Choose the region closest to you.
    * **Pricing tier**: Select `Basic` or `Free (F)` for this lab.
4.  Click **Review + create** and then **Create**.

### **Step 2: Create a Storage Account and Upload Data**

1.  Create a **Storage Account** resource in Azure.
2.  After its creation, navigate to the storage account and select **Containers** from the left menu.
3.  Create a new container (e.g., `coffee-reviews`).
4.  Upload the sample data files to this container. The Microsoft lab provides a set of coffee shop reviews.

### **Step 3: Index the Data**

Indexing is the process of ingesting content into Azure AI Search.

1.  Navigate to your **Azure AI Search** resource.
2.  On the overview page, click **Import data**.
3.  **Connect to your data**:
    * **Data Source**: Select **Azure Blob Storage**.
    * **Data source name**: Give it a name, such as `coffee-reviews-datasource`.
    * **Connection**: Choose your storage account and the `coffee-reviews` container.
4.  **Add cognitive skills (Optional, but recommended)**:
    * In this step, you can enrich the data with AI. Enable **OCR** to extract text from images and add other skills, such as:
        * **Extract key phrases**: To identify the main topics.
        * **Sentiment Analysis**: To determine if the text is positive, negative, or neutral.
        * **Entity Recognition**: To identify places, people, etc.
    * Save the enrichments to a new field in the index.
5.  **Customize target index**:
    * The **Index** is the structure that stores the searchable data.
    * **Index name**: Give it a name, such as `coffee-reviews-index`.
    * **Key**: Define a unique field for each document (usually `metadata_storage_path`).
    * Review the index fields. Mark the fields as `Retrievable`, `Filterable`, `Sortable`, and `Searchable` as needed.
6.  **Create an indexer**:
    * The **Indexer** is the process that runs the data ingestion.
    * **Name**: Give it a name, such as `coffee-reviews-indexer`.
    * **Schedule**: Set to `Once`.
7.  Click **Submit** to start the indexer creation and execution process.

### **Step 4: Explore the Index and Run Queries**

1.  Within your Azure AI Search resource, select **Search explorer** from the menu.
2.  Make sure that **`coffee-reviews-index`** is selected in the **Index** tab.

#### **Query Examples:**

* **Simple Text Search**:
    * In the **Query string** box, type `london` and click **Search**. The results will include all documents that mention the word "london".

    ```json
    search=london
    ```

* **Filtering with OData Syntax**:
    * To find reviews with a sentiment score greater than 0.8 (very positive), use the `$filter` parameter.

    ```
    search=*&$filter=sentiment gt 0.8
    ```

* **Combining Search and Filter**:
    * To find positive reviews about `london`:

    ```
    search=london&$filter=sentiment gt 0.7
    ```

* **Selecting Specific Fields**:
    * To return only the key phrases and sentiment score from the results, use the `$select` parameter.

    ```
    search=london&$select=keyphrases,sentiment
    ```

This practical guide demonstrates how Azure AI Search can be used to efficiently ingest, enrich, and explore unstructured data, transforming it into valuable and searchable information.
