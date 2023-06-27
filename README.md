## creditfault


#### Problem Statement:
  Financial threats are displaying a trend about the credit risk of commercial banks as the
  incredible improvement in the financial industry has arisen. In this way, one of the
  biggest threats faces by commercial banks is the risk prediction of credit clients. The
  goal is to predict the probability of credit default based on credit card owner's
  characteristics and payment history.
  
  
  
  #### Data Description
  
  The client will send data in multiple sets of files in batches at a given location. The data has been extracted from the census bureau. 
The data contains 32561 instances with the following attributes:
     Features:
1.	LIMIT_BAL: continuous Credit Limit of the person.
2.	SEX: Categorical: 1 = male; 2 = female
3.	EDUCATION: Categorical: 1 = graduate school; 2 = university; 3 = high school; 4 = others
4.	MARRIAGE: 1 = married; 2 = single; 3 = others
5.	AGE-num: continuous. 
6.	PAY_0 to PAY_6: History of past payment. We tracked the past monthly payment records (from April to September, 2005)
7.	BILL_AMT1 to BILL_AMT6: Amount of bill statements.
Target Label:
Whether a person shall default in the credit card payment or not.


9.	default payment next month:  Yes = 1, No = 0.


Apart from training files, we also require a "schema" file from the client, which contains all the relevant information about the training files such as:
Name of the files, Length of Date value in Filename, Length of Time value in Filename, Number of Columns, Name of the Columns, and their datatype.



#### Data Validation
    
    In This step, we perform different sets of validation on the given set of training files.
    
    Name Validation: We validate the name of the files based on the given name in the schema file. We have 
    created a regex patterg as per the name given in the schema fileto use for validation. After validating 
    the pattern in the name, we check for the length of the date in the file name as well as the length of time 
    in the file name. If all the values are as per requirements, we move such files to "Good_Data_Folder" else
    we move such files to "Bad_Data_Folder."
    
    Number of Columns: We validate the number of columns present in the files, and if it doesn't match with the
    value given in the schema file, then the file id moves to "Bad_Data_Folder."
    
    Name of Columns: The name of the columns is validated and should be the same as given in the schema file. 
    If not, then the file is moved to "Bad_Data_Folder".
    
    The datatype of columns: The datatype of columns is given in the schema file. This is validated when we insert
    the files into Database. If the datatype is wrong, then the file is moved to "Bad_Data_Folder."
    
    Null values in columns: If any of the columns in a file have all the values as NULL or missing, we discard such
    a file and move it to "Bad_Data_Folder".
    
#### Data Insertion in Database
     
     Database Creation and Connection: Create a database with the given name passed. If the database is already created,
     open the connection to the database.
     
     Table creation in the database: Table with name - "Good_Data", is created in the database for inserting the files 
     in the "Good_Data_Folder" based on given column names and datatype in the schema file. If the table is already
     present, then the new table is not created and new files are inserted in the already present table as we want 
     training to be done on new as well as old training files.
     
     Insertion of file in the table: All the files in the "Good_Data_Folder" are inserted in the above-created table. If
     any file has invalid data type in any of the columns, the file is not loaded in the table and is moved to 
     "Bad_Data_Folder".
     
#### Model Training
    
     Data Export from Db: The data in a stored database is exported as a CSV file to be used for model training.
     
     Data Preprocessing: 
        Check for null values in the columns. If present, impute the null values using the KNN imputer.
        
        Check if any column has zero standard deviation, remove such columns as they don't give any information during 
        model training.
        
     Clustering: KMeans algorithm is used to create clusters in the preprocessed data. The optimum number of clusters 
     is selected


## Create a "Procfile" with following content
```
web: gunicorn main:app
```

## create a file ".github/workflows/main_creditcardf01.yml" with following content
```
# Docs for the Azure Web Apps Deploy action: https://github.com/Azure/webapps-deploy
# More GitHub Actions for Azure: https://github.com/Azure/actions
# More info on Python, GitHub Actions, and Azure App Service: https://aka.ms/python-webapps-actions

name: Build and deploy Python app to Azure Web App - creditcardf01

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2

      - name: Set up Python version
        uses: actions/setup-python@v1
        with:
          python-version: '3.7'

      - name: Create and start virtual environment
        run: |
          python -m venv venv
          source venv/bin/activate
      
      - name: Install dependencies
        run: pip install -r requirements.txt
        
      # Optional: Add step to run tests here (PyTest, Django test suites, etc.)
      
      - name: Upload artifact for deployment jobs
        uses: actions/upload-artifact@v2
        with:
          name: python-app
          path: |
            . 
            !venv/

  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: 'Production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v2
        with:
          name: python-app
          path: .
          
      - name: 'Deploy to Azure Web App'
        uses: azure/webapps-deploy@v2
        id: deploy-to-webapp
        with:
          app-name: 'creditcardf01'
          slot-name: 'Production'
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE_909B911F7CC04D048A7DF7552412C72E }}
        
```
## to create requirements.txt

```buildoutcfg
pip freeze>requirements.txt
```

## initialize git repo

```
git init
git add .
git commit -m "first commit"
git branch -M main
git remote add origin <github_url>
git push -u origin main
```

## to update the modification

```
git add .
git commit -m "proper message"
git push 
```


### deployment link:-


    Azure link :-https://creditcardf01.azurewebsites.net
     
     
