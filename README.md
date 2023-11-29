# Electrical Transformer Accelerator
---

## Overview

An electrical transformer is a crucial device that transfers electrical energy from one circuit to another through electromagnetic induction, making it an essential component of electrical power systems.

The accelerator demonstrates to end-users the leverage of the IBM solution to generate and deploy the analytic models, including machine learning, without coding requirements.  Furthermore, it can integrate with the IBM Maximo Monitor to display the analytic results over the dashboard.  To leverage the data sets collected from distribution electrical transformers, we can provide a set of examples that convert this data into business or operation values. 

Here we include five examples:
1. CO2 Equaivent Emissions - The energy loss to CO2 equaivent estimation provides an approach for taking energy  loss calculations and the location of the electrical transformer expressed as a zip code and using that information and CO2  energy conversion data from the EPA to generate an estimate of CO2  emissions. By calculating CO2 emissions, power companies can leverage  this information in various ways to create business value and ensure  their operations are sustainable and aligned with market demands and  regulatory requirements.

2. Rule-based Haromic Anomaly Detection Detection - Voltage Total Harmonic Distortion (THD) in the context of transformers refers to the distortion of the voltage waveform due to multiple harmonic frequencies in addition to the fundamental frequency.  The rule-based anomaly detection using the sensors's maximum and minimum threshold set  points is a simple and effective method for monitoring sensor.  When the observed values exceed these thresholds, it  generates a tag (or indicator) for that timestamp. Additional rules are  applied to count the indicators in a specific amount of time for  different metrics or KPIs to trigger an alert or anomaly.  Those  additional rules are used to avoid overflooding the anomalies in the  system.

3. Energy Loss - Energy loss estimation calculates the amount of energy lost in a system due to various factors such as inefficiencies due to the outdated transformer, too high or too low of capacities, and cooling oil overheating.   Energy loss estimation can help identify areas of the system where  energy loss is particularly high, allowing for targeted improvements to  be made to reduce these losses. By minimizing energy loss in the system, it is possible to increase its efficiency, reduce costs, and minimize  environmental impact.

4. Health Score - The health score is a quantitative measure of the electrical transformer's health condition, often ranging from 0 to 100 or some other scale. By continuously monitoring and analyzing the health score, it is possible to identify trends and patterns in the machine's behavior and make adjustments to maximize its operational efficiency and lifespan.

5. Anomaly Detection - Anomaly detection is an essential aspect of transformer monitoring. It enables identifying abnormal behavior that may indicate a potential failure or fault in the transformer. Early detection of such anomalies can facilitate preventative maintenance, reducing the risk of costly downtime or damage to the transformer. 

## Onboarding Guide
This repository includes a comprehensive guide for building and deploying a conformal prediction-based model using AI Model Factory's - Electrical Transformer Accelerator Recipe (available as API endpoint) for Maximo Monitor Application. It provides all the necessary resources, including. 
1. Sample data sets for the `distribution of electrical transformer` Asset and a set of `Dissolved Gas Analysis (DGA)` data
2. AI Cookbook to facilitate model training using `AI Model Factory` Service, 
3. Creating an IoT device on `Maximo Monitor`, 
4. Deploying and configuring the `KPI` endpoints to the created device, 
5. Conducting inference with real-time data streaming into `Maximo Monitor`,  
6. `Visualizing` the results through an intuitive dashboard

Navigate to the **[onboarding](onboarding)** folder to discover all the resources for utilizing our electrical transformer accelerator. We have provided a detailed outline of the required steps to streamline and automate the entire procedure. 

Completing the walkthrough should take around 15 minutes, considering the time needed for training, deployment, and inference on the dataset spanning nine months during the demonstration.

## Security
See [**CONTRIBUTING**](./CONTRIBUTING.md) for more information.

## License
This collection of AI cookbooks is licensed under Apache license. See the [**LICENSE**](LICENSE) file.

## Questions
Please contact [**Dhaval Patel or raise an issue on this repository.
