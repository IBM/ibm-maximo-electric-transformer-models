# Train and deploy a model thorugh  API

# Train a Model for Model Factory using the  API



This document allows us to training, deploying and score the five services and potentially others.  The process always starts with the training, followed by the deployment.  Then, the service is ready for the end-user.

As a separate effort, a scoring notebook is created to allow us to digest the service from WML (not very clear yet, as we are using the C4PD end-point; it should be used from the Swagger API). 



## Prerequisite: 

- All the services should have their codes committed into the Model Factory code base and with proper routing setup.  The details of the such Swagger definition can be found at:
- 
- Swagger Server:
  - Early Beat Swagger Server:  https://tenant1.predict.masinst1.ibmmam.com/ibm/modelfactory/service
  - Research Swagger Server: https://api.modelfactory-9ca4d14d48413d18ce61b80811ba4308-0000.us-south.containers.appdomain.cloud/ibm/modelfactory/service/apidocs



This document will use the CO2e emission from energy loss of eletrical transformer as illustrated example. 



## Training Process:



URL 

Data 

Status 

model _link

## Deployment Process 



model_link 

status of return 

Validation

Question?  How do I know the end-point? (we view in the C4PD the scoring end-point, how we know)



## Scoring Process

Link 

Data 

Scoring





# Setting up for the training process

First begin by importing necessary packages:

```
#Imports
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
import time
```

Next cells will define functions needed to display information about the job and training process:

```
# Print functions
def print_job_status(job_id, endpoint_url):
    # Extract the job ID and construct the URL
    url = endpoint_url + "/summary/" + job_id
    
    # Send a GET request to fetch the job status
    get_response = requests.get(url, headers={})
    status_data = get_response.json()
    
    # Print the job status
    print("The status of job {} is {}.".format(job_id, status_data['status']))
    
    return status_data['status']
    
def print_all_status(job_id, endpoint_url):
    # Extract the job ID and construct the URL
    url = endpoint_url + "/summary/" + job_id
    
    # Send a GET request to fetch the job status
    get_response = requests.get(url, headers={})
    status_data = get_response.json()
    
    # Print the job status
    print(status_data)
```

```
# More print functions using display 
from IPython.display import display, HTML
import requests

def print_job_details(job_id, endpoint_url):
    # Extract the job ID and construct the URL
    url = endpoint_url + "/summary/" + job_id
    
    # Send a GET request to fetch the job status
    get_response = requests.get(url, headers={})
    summary_data = get_response.json()
    
    # Display the job status
    display(HTML(print_keys_and_values(summary_data)))

    
def print_keys_and_values(json_data):
    # Start the HTML code
    html_code = "<div style='font-family: Arial; font-size: 1.2em;'>"
    
    # Add the job details to the HTML code
    html_code += f"<p>job_id: {json_data['job_id']}</p>"
    html_code += f"<p>status: {json_data['status']}</p>"
    html_code += "<br>"
    
    for summary in json_data['detailed_summary']:
        html_code += f"<p>run_id: {summary['run_id']}</p>"
        html_code += f"<p>experiment_id: {summary['experiment_id']}</p>"
        html_code += f"<p>status: {summary['status']}</p>"
        html_code += f"<p>artifact_uri: {summary['artifact_uri']}</p>"
        html_code += f"<p>artifact_name: {summary.get('tags.artifact_name', 'No artifact_name found')}</p>"
        html_code += "<br>"
    
    # Close the HTML code
    html_code += "</div>"
    
    return html_code
```

check that your directory is set to the correct location. In the sample case of CO2 emission calculation similar to the below example:

```
'/electrical_transformer_accelerator/cookbooks/co2_emission'
```

## Define Data Paths for Files

Define paths to the data and configuration files required for the endpoint you are making use of. The following example is for CO2 emissions but the exaction files will vary depending on use case. The relevent endpoint_url is defined here for later use.

```
# Define the file paths
endpoint_url = "https://tenant1.predict.masinst1.ibmmam.com/ibm/modelfactory/service"
data_file_path =  "../../data/CO2e_from_energy_model_training_99557.csv"
co2_conversion_file_path = "../../config/co2_emission/co2e_conversion.csv"
config_file_path =  "../../config/co2_emission/EEL_to_CO2_kwh_config.yaml"
```

# Post Response

Next we import the requests package and create the files dictionary which will be used to input the appropriate files into the api. In the example case this includes three files, the data_file, the co2_conversion_file, and the config_file. Then the url is created by combining the endpoint url from earlier and the api url. Finally we post response using the files dictionary and url we defined.

```
import requests

files = {
    "data_file": ("CO2e_from_energy_model_training_99557.csv", open(data_file_path, 'rb')),
    "co2_conversion_file": ("co2e_conversion.csv", open(co2_conversion_file_path, 'rb')), 
    "config_file": ("EEL_to_CO2_kwh_config.csv", open(config_file_path, 'rb')),
   
}

url = endpoint_url + "/recipe/electrical-transformer/kpi/emission/train"
post_response = requests.request("POST", url, headers={}, data={}, files=files)
```

Next we retrieve the job_id for the job we have created and get an initial status of the job run.

```
post_r_json = post_response.json()

anomaly_service_jobId = None

if 'jobId' in post_r_json:
    anomaly_service_jobId = post_r_json['jobId']
    print ('submitted successfully job : ', post_r_json['jobId'])
else:
    print (post_r_json)
```

# GET Response

Next we will determine how to retrieve information about the job we have created. The status of the job may be running, flagged by INITALIZING or EXECUTING
After a while the model recipe training is complete, and the STATUS changes to DONE. 

Start by creating the log url which will take you to the job log and retrieve the job_id. 

```
log_url = endpoint_url + "/log/"
job_id = post_r_json['job_id']
```

Next we print the log url. You may click on this link to observe the log to retrieve information about the job run.

```
print(log_url + job_id)
```

Finally we print job details. The details generated by this cell will update as the job runs and the cell may return an error if it is run too quickly after the job is created. If the error occurs wait several seconds and re-run the cell.

```
print_job_details(job_id,endpoint_url)
```
