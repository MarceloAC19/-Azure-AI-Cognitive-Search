# Insights and Knowledge Acquired

This document records the main observations and learnings obtained during the exploration of **Azure AI Search**.

## 1. The Power of AI Enrichment (Cognitive Skills)

The "Add cognitive skills" step proved to be the most impactful feature of the process. Initially, the data was just simple text files (reviews). After applying skills such as:

* **Sentiment Analysis**: It was possible to quantify customer opinions. A query like `$filter=sentiment lt 0.3` allowed for the quick isolation of all negative reviews, something that would be extremely time-consuming to do manually.
* **Key Phrase Extraction**: This skill transformed unstructured text into organized metadata. Instead of just searching for words, we could identify the most discussed *topics*, such as "customer service," "coffee price," or "venue atmosphere."
* **Entity Recognition**: Automatically identifying locations (`london`, `paris`) in the reviews creates a powerful capability for geospatial search and aggregation by city.

**Key Insight**: Azure AI Search is not just a "search tool." It is a knowledge ETL (Extract, Transform, Load) pipeline that transforms raw data into a graph of structured and interconnected information.

## 2. The Flexibility of Query Syntax

The use of OData syntax (`$filter`, `$select`, `$orderby`) offers granular and powerful control over search results.

* **Filters vs. Search**: Understanding the difference between `search` and `$filter` is fundamental.
