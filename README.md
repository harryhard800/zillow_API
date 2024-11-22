First create account on rapid api inorder to get the data
url = https://rapidapi.com/apimaker/api/zillow-com1
got to search and look for zillow by sabri open it and select language and you will get the api endpoint [/search (Search for properties by neighborhood, city, or ZIP code)] also attached screenshots. click on test endpoint.
if you get the result as list of dict you are done as shown in response data file.

step 1 
install python on AWS ec2 instance
create virtual environment with python.
intall aws cli and configure it 
install apace-airflow

step 2
make change in zilowrapidapi.py
a) add you registered email id with rapid_api.com
b) create s3 bucket with name 
    1) zillow-api-bucket-1st
    2) copy-of-json-data-2nd-bucket
    3) zillow-api-third-bucket
c) add rules and permission in ec2 to access the lambda, redshift and s3 bucket and aloow ports of apache airflow to access the airflow web ui

Step 3
open airflow installation folder and modify the airflow.cfg
a) add (dags_folder = /home/ubuntu/airflow/dags) config file in airflow.cfg 
b) disbale the load_examples = True to load_examples = False
c) start apace-airflow (airflow standalone)

step 4
* if you dont need the default schema of data you can change it in lambda function.
a) create table (zillowdata) in redsfit cluster using create commnad , for schema download the file from s3 bucket name  'zillow-api-bucket-1st' and change the schema defined in lambda function. 
default schema will be (
        CREATE TABLE IF NOT EXISTS zillowdata(
        bathrooms NUMERIC,
        bedrooms NUMERIC,
        city VARCHAR(225),
        homeStatus VARCHAR(225),
        homeType VARCHAR(225),
        zipcode INT)
    )
b) set the connection of redshift in airflow using the 'redshift_conn_id' mentioned in 'task_transfer_s3_to_redshift' dag
c) run the dag, After completion of last dag you can query the table of redshift.

step5
a) for the analytics create the quicksight account on aws and configure the redshift with quicksight and choose the graph or create the custome graph based on table data.

trigger the dag on airflow ui and each time table of redshift will get updated by new data and previous data.
