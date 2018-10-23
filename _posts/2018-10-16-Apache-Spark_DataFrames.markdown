---
layout: post
title:  "Spark Tutorial Part 4: Getting Started with Spark DataFrames (Reading and Transforming your Data)"
date:   2018-10-16 08:22:00
author: Ayush Vatsyayan
categories: Apache-Spark
tags:	    spark
cover:  "/assets/instacode.png"
---

This article in our Spark Tutorial series demonstrates the reading of data into DataFrame and applying different transformations on it.

Prerequisites: [Set up Spark development environment](https://ayushedu.github.io/apache-spark/2018/06/04/Setting-up-spark-development-environment.html) and review the [Spark Fundamentals](https://ayushedu.github.io/apache-spark/2018/06/09/Apache-Spark_Fundamentals.html).
Objective: To understand [Spark DataFrames](https://spark.apache.org/docs/latest/sql-programming-guide.html#datasets-and-dataframes) and load data into Apache Spark.

# Overview
[Apache Spark](https://spark.apache.org/) is one of the most widely used open source processing engines for big data, with rich language-integrated APIs and a wide range of libraries. It’s much easier to program in Spark due to its rich APIs in [Python](https://www.python.org), [Java](https://docs.oracle.com/en/java/), [Scala](https://www.scala-lang.org), and [R](https://www.r-project.org).

This richness is gained from Apache Spark’s SQL module that integrates the relational processing with Spark's functional programming API.  The Spark SQL lets Spark programmers leverage the benefits of relational processing (e.g. declarative queries and optimized storage), and lets SQL users call complex analytics libraries in Spark (e.g. machine learning). 

Compared to [RDD](https://spark.apache.org/docs/latest/rdd-programming-guide.html#resilient-distributed-datasets-rdds), [Spark SQL](https://spark.apache.org/docs/latest/sql-programming-guide.html#sql) makes two main additions. First, it offers much tighter integration between relational and procedural processing, through a declarative DataFrame API that integrates with procedural Spark code. Second, it includes a highly extensible optimizer, Catalyst, built using features of the Scala programming language, that makes it easy to add composable rules, control code generation, and define extension points. 

In summary Spark SQL is an evolution of both SQL-on-Spark and of Spark itself, offering richer APIs and optimizations while keeping the benefits of the Spark programming model.

# Datasets and DataFrames
A DataFrame is conceptually equivalent to a table in a relational database or a data frame in R/Python, but with richer optimizations. It is a distributed collection of data, like RDD, but organized into named columns (i.e., a collection of structured records). This provides Spark with more information about the structure of both the data and the computation. Such information can be used for extra optimizations.

Unlike the RDD API, which is general and has no information about the data structure, the [DataFrame API](https://spark.apache.org/docs/latest/api/python/pyspark.sql.html#pyspark.sql.DataFrame) can perform relational operations on RDDs and external data sources, and enables rich relational/ functional integration within Spark applications. DataFrames are now the main data representation in Spark’s ML Pipelines API.

Another improvement is the Dataset API which was added in Spark 1.6 that provides the benefits of RDDs (strong typing, ability to use powerful lambda functions) with the benefits of Spark SQL’s optimized execution engine. In the Scala API, DataFrame is simply a type alias of Dataset[Row]. While, in Java API, users need to use Dataset<Row> to represent a DataFrame.

The [Dataset API](https://spark.apache.org/docs/latest/api/scala/index.html#org.apache.spark.sql.Dataset)  is available in Scala and Java. Python and R does not have the support for the Dataset API, but due to their dynamic nature, many of the benefits of the Dataset API are already available.

# Example Data: Kaggle’s House Prices Data
Let’s start by reading the csv file into Spark DataFrame, and then later performing different transformations including creating new features.

For this example we are using the data from  Kaggle’s [House Prices](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/) competition. Once downloaded, we will put the downloaded csv files into HDFS. 

The process is straightforward:
1. Download data as csv file from Kaggle.
2. Load csv file into HDFS.
3. Load data from HDFS into Spark DataFrame.
4. Explore and Transform the dataset.

For the code we are be using Spark’s python API. 

# Download data from kaggle
* Sign in into Kaggle
* Visit Kaggle’s [House Prices: Advanced Regression Techniques](https://www.kaggle.com/c/house-prices-advanced-regression-techniques/data) and download data.
* From the downloaded dataset we will be using only train.csv

## Understanding the dataset
Following are fields descriptions, which is also present in the data description file in downloaded zip.
* **SalePrice**: the property's sale price in dollars. 
* **MSSubClass**: The building class
* **MSZoning**: The general zoning classification
* **LotFrontage**: Linear feet of street connected to property
* **LotArea**: Lot size in square feet
* **Street**: Type of road access
* **Alley**: Type of alley access
* LotShape: General shape of property
* LandContour: Flatness of the property
* Utilities: Type of utilities available
* LotConfig: Lot configuration
* LandSlope: Slope of property
* Neighborhood: Physical locations within Ames city limits
* Condition1: Proximity to main road or railroad
* Condition2: Proximity to main road or railroad (if a second is present)
* BldgType: Type of dwelling
* HouseStyle: Style of dwelling
* OverallQual: Overall material and finish quality
* OverallCond: Overall condition rating
* YearBuilt: Original construction date
* YearRemodAdd: Remodel date
* RoofStyle: Type of roof
* RoofMatl: Roof material
* Exterior1st: Exterior covering on house
* Exterior2nd: Exterior covering on house (if more than one material)
* MasVnrType: Masonry veneer type
* MasVnrArea: Masonry veneer area in square feet
* ExterQual: Exterior material quality
* ExterCond: Present condition of the material on the exterior
* Foundation: Type of foundation
* BsmtQual: Height of the basement
* BsmtCond: General condition of the basement
* BsmtExposure: Walkout or garden level basement walls
* BsmtFinType1: Quality of basement finished area
* BsmtFinSF1: Type 1 finished square feet
* BsmtFinType2: Quality of second finished area (if present)
* BsmtFinSF2: Type 2 finished square feet
* BsmtUnfSF: Unfinished square feet of basement area
* TotalBsmtSF: Total square feet of basement area
* Heating: Type of heating
* HeatingQC: Heating quality and condition
* CentralAir: Central air conditioning
* Electrical: Electrical system
* 1stFlrSF: First Floor square feet
* 2ndFlrSF: Second floor square feet
* LowQualFinSF: Low quality finished square feet (all floors)
* GrLivArea: Above grade (ground) living area square feet
* BsmtFullBath: Basement full bathrooms
* BsmtHalfBath: Basement half bathrooms
* FullBath: Full bathrooms above grade
* HalfBath: Half baths above grade
* Bedroom: Number of bedrooms above basement level
* Kitchen: Number of kitchens
* KitchenQual: Kitchen quality
* TotRmsAbvGrd: Total rooms above grade (does not include bathrooms)
* Functional: Home functionality rating
* Fireplaces: Number of fireplaces
* FireplaceQu: Fireplace quality
* GarageType: Garage location
* GarageYrBlt: Year garage was built
* GarageFinish: Interior finish of the garage
* GarageCars: Size of garage in car capacity
* GarageArea: Size of garage in square feet
* GarageQual: Garage quality
* GarageCond: Garage condition
* PavedDrive: Paved driveway
* WoodDeckSF: Wood deck area in square feet
* OpenPorchSF: Open porch area in square feet
* EnclosedPorch: Enclosed porch area in square feet
* 3SsnPorch: Three season porch area in square feet
* ScreenPorch: Screen porch area in square feet
* PoolArea: Pool area in square feet
* PoolQC: Pool quality
* Fence: Fence quality
* MiscFeature: Miscellaneous feature not covered in other categories
* MiscVal: $Value of miscellaneous feature
* MoSold: Month Sold
* YrSold: Year Sold
* SaleType: Type of sale
* SaleCondition: Condition of sale

# Load data into HDFS

# Load data into Spark from HDFS





