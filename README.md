# MUBI Dataset Analysis

This project analyzes a dataset from MUBI, a curated streaming service for independent, international, classic, and arthouse films. The analysis is performed using a Jupyter notebook, and includes steps for loading, cleaning, and visualizing the data. Additionally, the project includes code for querying a MongoDB database containing the same dataset.

## Getting Started

### Prerequisites

- Python 3.11+
- Jupyter Notebook
- MongoDB

### Installation

1. Clone the repository:
   ```bash
   git clone https://github.com/Nikwiza/MUBI_dataset.git
   cd MUBI-Dataset
   ```

2. Create a virtual environment and activate it::
   ```bash
    python -m venv venv
    source venv/bin/activate   # On Windows use `venv\Scripts\activate`
   ```

3. Install the required packages:
   ```bash
    pip install -r requirements.txt
   ```

4. Ensure MongoDB is installed and running on your machine. Follow the instructions on the MongoDB official website if needed.



# Dataset

The dataset consists of multiple CSV files:

    movies.csv: Contains information about the movies.
    ratings.csv: Contains user ratings for the movies.
    users.csv: Contains information about the users.

## Logical Schema

The logical schema of the dataset is represented in the image below:

## Jupyter Notebook

The Jupyter notebook MUBI_Analysis.ipynb contains the following sections:

- Data Loading: Load the CSV files into pandas DataFrames.
- Data Cleaning: Perform data cleaning and preprocessing.
- Exploratory Data Analysis (EDA): Generate visualizations and perform basic statistical analysis.
- Advanced Analysis: Implement advanced analysis techniques (e.g., machine learning models).

## MongoDB Queries

The mongo_queries.py script contains code to perform various queries on the MongoDB database containing the MUBI dataset. This includes:

- Inserting data from CSV files into MongoDB collections.
- Querying the database to retrieve specific information.
- Aggregating data to perform complex analyses.


## How to Run

1. Run the Jupyter Notebook:
   ```bash
    jupyter notebook notebooks/MUBI_Analysis.ipynb
   ```

2. Run MongoDB Queries:
   ```bash
    python mongodb/mongo_queries.py 
   ```