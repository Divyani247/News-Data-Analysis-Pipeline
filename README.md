# News-Data-Analysis-Pipeline
## 📋 Project Overview

This project demonstrates a comprehensive **news data extraction and analysis pipeline** using **NewsAPI**, **Apache Airflow**, **Google Cloud Storage**, and **Snowflake**. The system automatically fetches news articles, processes them, and creates analytical tables for news source and author activity analysis.

## Architecture

```
┌─────────────────┐    ┌──────────────────┐    ┌─────────────────┐    ┌─────────────────┐
│   NewsAPI       │───▶│  Apache Airflow  │───▶│ Google Cloud    │───▶│   Snowflake     │
│                 │    │                  │    │ Storage (GCS)   │    │                 │
│ • Real-time     │    │ • Daily DAG      │    │ • Parquet Files │    │ • Data Warehouse│
│   News Data     │    │ • Data Pipeline  │    │ • Raw Storage   │    │ • Analytics     │
│ • Pagination    │    │ • Orchestration  │    │ • Auto-cleanup  │    │ • Summary Tables│
└─────────────────┘    └──────────────────┘    └─────────────────┘    └─────────────────┘
```

## 🎯 Key Features

- **🔄 Automated Data Extraction**: Daily news fetching with pagination support
- **📊 Real-time Processing**: Live news data from NewsAPI with comprehensive coverage
- **☁️ Cloud Integration**: Seamless GCS and Snowflake integration
- **🎛️ Airflow Orchestration**: Scheduled data pipeline with error handling
- **📈 Analytics Ready**: Pre-built summary tables for news source and author analysis
- **🔧 Schema Inference**: Automatic table creation from Parquet files
- **📝 Data Quality**: Content cleaning and validation

## 📁 Project Structure

```
News-Data-Analysis-Project/
├── fetch_news.py                    # News API data extraction script
├── news_api_airflow_job.py          # Airflow DAG for orchestration
├── snowflake_commands.sql           # Snowflake setup and configuration
├── airflow_snowflake_connection.png # Connection setup reference
└── README.md                        # This documentation
```
### **Analytical Tables:**

#### **1. Main Data Table: `news_api_data`**
- **Purpose**: Raw news articles with full content
- **Schema**: Auto-inferred from Parquet files
- **Content**: All article fields including title, content, source, author, timestamps

#### **2. News Source Summary: `summary_news`**
- **Purpose**: News source analytics and statistics
- **Metrics**: Article count, date range, source popularity
- **Use Cases**: Source performance analysis, content volume tracking

#### **3. Author Activity: `author_activity`**
- **Purpose**: Author productivity and distribution analysis
- **Metrics**: Article count, latest activity, source diversity
- **Use Cases**: Author performance tracking, content creator analysis

## 🔧 Technical Components

### **1. News Data Extraction (`fetch_news.py`)**

**Core Functions:**
- **`get_api_key_from_airflow()`**: Secure API key retrieval from Airflow Variables
- **`fetch_news_from_api()`**: Paginated news fetching with comprehensive coverage
- **`process_articles_to_dataframe()`**: Data cleaning and structure conversion
- **`upload_to_gcs()`**: Cloud storage integration with automatic cleanup

**Key Features:**
- **Pagination Support**: Fetches all available articles (not just first 100)
- **Content Cleaning**: Intelligent content trimming and validation
- **Error Handling**: Comprehensive exception handling and logging
- **Airflow Integration**: Seamless integration with Airflow Variables

### **2. Airflow Orchestration (`news_api_airflow_job.py`)**

**DAG Configuration:**
- **Schedule**: Daily execution
- **Start Date**: October 3, 2025
- **Retry Logic**: Configurable retry attempts
- **Error Handling**: Comprehensive failure management

**Task Pipeline:**
```
newsapi_data_to_gcs → snowflake_create_table → snowflake_copy_from_stage
                                                      ↓
                                    [news_summary_task, author_activity_task]
```

**Task Details:**
1. **`newsapi_data_to_gcs`**: Extract and upload news data
2. **`snowflake_create_table`**: Auto-create table with inferred schema
3. **`snowflake_copy_from_stage`**: Load data from GCS to Snowflake
4. **`create_or_replace_news_summary_tb`**: Generate source analytics
5. **`create_or_replace_author_activity_tb`**: Generate author analytics

### **3. Snowflake Setup (`snowflake_commands.sql`)**

**Database Setup:**
- **Database**: `news_api` dedicated database
- **File Format**: Parquet format for efficient storage
- **Storage Integration**: GCS integration for external data access
- **External Stage**: Direct access to GCS bucket

**Key Components:**
- **Storage Integration**: Secure GCS connection
- **External Stage**: `gcs_raw_data_stage` for data access
- **File Format**: Optimized Parquet format
- **Query Examples**: Sample queries for data exploration

## 📊 Data Processing Pipeline

### **1. Data Extraction:**
- **API Call**: Fetch news articles with pagination
- **Content Processing**: Clean and validate article content
- **DataFrame Creation**: Structure data for analysis
- **File Generation**: Save as Parquet format

### **2. Cloud Storage:**
- **Upload**: Transfer Parquet files to GCS
- **Cleanup**: Remove local files after upload
- **Organization**: Structured file naming with timestamps

### **3. Data Loading:**
- **Schema Inference**: Automatic table structure detection
- **Data Transfer**: Copy from GCS to Snowflake
- **Validation**: Ensure data integrity

### **4. Analytics Generation:**
- **Source Analysis**: News source performance metrics
- **Author Analysis**: Author activity and distribution
- **Real-time Updates**: Daily refresh of analytical tables

## 📈 Analytics & Insights

### **News Source Analytics:**
```sql
-- Top news sources by article count
SELECT news_source, article_count, latest_article_date
FROM summary_news
ORDER BY article_count DESC;
```

### **Author Activity Analysis:**
```sql
-- Most prolific authors
SELECT author, article_count, distinct_sources
FROM author_activity
WHERE author IS NOT NULL
ORDER BY article_count DESC;
```

### **Content Analysis:**
```sql
-- Recent articles by source
SELECT "source", "newsTitle", "timestamp"
FROM news_api_data
WHERE "timestamp" >= CURRENT_DATE() - 1
ORDER BY "timestamp" DESC;
```

## 🔍 Data Quality & Validation

### **Content Processing:**
- **Text Cleaning**: Remove extra whitespace and format content
- **Length Validation**: Intelligent content trimming at sentence boundaries
- **Null Handling**: Comprehensive null value management
- **Data Types**: Proper type conversion and validation

## 📚 Learning Objectives

This project demonstrates:

1. **API Integration**: NewsAPI integration with pagination
2. **Data Processing**: Content cleaning and validation
3. **Cloud Storage**: GCS integration and file management
4. **Orchestration**: Airflow DAG design and task dependencies
5. **Data Warehousing**: Snowflake setup and data loading
6. **Analytics**: Summary table creation and insights
7. **Error Handling**: Comprehensive error management
8. **Security**: Secure API key management

## 🎯 Business Value

- **Content Intelligence**: Automated news monitoring and analysis
- **Source Tracking**: News source performance and reliability analysis
- **Author Insights**: Content creator activity and distribution analysis
- **Trend Analysis**: Daily news pattern identification
- **Data Quality**: Clean, structured news data for analysis
- **Scalability**: Cloud-native architecture for growth
- **Cost Efficiency**: Automated processing reduces manual effort

## 🔮 Future Enhancements

### **Potential Improvements:**
- **Sentiment Analysis**: Add sentiment scoring to articles
- **Topic Modeling**: Automatic topic classification
- **Real-time Alerts**: Notifications for specific news topics
- **Advanced Analytics**: More sophisticated reporting and dashboards
- **Multi-source Integration**: Support for additional news APIs

### **Technical Enhancements:**
- **Streaming Processing**: Real-time news processing
- **Machine Learning**: Predictive analytics for news trends
- **API Rate Limiting**: Intelligent API usage optimization
- **Data Lake Integration**: Connect to additional data sources
- **Performance Monitoring**: Enhanced monitoring and alerting

---

**📰 News Data Analysis - Automated News Intelligence Pipeline** 
**🔧 Built with:** NewsAPI, Apache Airflow, Google Cloud Storage, Snowflake, Python
