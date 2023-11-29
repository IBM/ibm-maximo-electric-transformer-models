# Electrical Transformer Accelerator for Maximo Monitor Application


## Repository Structure
After cloning this repository, navigate to the AI cookbooks notebooks.   The file structure of the repository is separated based on the receipe.  At the top level, the structure are as follows.

```
├── README.md                           <-- General Introduction of the accelerator
├── onboarding                          <-- Documentation folder for accelerator is here
│   ├── README.md                       <-- Instruction file for onboarding
├── co2e_emission                       <-- Folder for Calculating CO2 Equivent Recipe
├── harmonic_rule                       <-- Folder for Identifying Anomalies Recipe (rule-based Model)
├── energyloss                          <-- Folder for Estimating the Load Energy Loss Recipe
├── anomaly                             <-- Folder for Identifying Anomalies Recipe (ML Model)
├── health_dga                          <-- Folder for Estimating Health Score from DGA (Dissolved Gas Analysis) Recipe (ML Model).  
├── config                              <-- Folder for configuration files cross multiple recipe notebooks
├── utils                               <-- Folder for utility functions cross multiple recipe notebooks
CODE_OF_CONDUCT.md                      <-- Guideline for behavior and interaction within the project's community
CONTRIBUTING.md                         <-- Document that guides contributors on how to effectively participate in the project
LICENSE                                 <-- License File
```

##  Details for Each Recipe Repostory Structure

We have a common structure for all the supported recipes for electrical transformer accelartor.  In general, they are all following this structure.

```
├── README.md
├── config
│   ├── parameter_config.yaml            <-- Configuration for the recipe training
│   ├── train_deploy_score_config.yml          <-- Configuration for the train and deploy for this recipe
├── data
│   ├── train.csv                        <-- Sample dataset used for the model creation
├── cookbooks
│   ├── train.ipynb                      <-- Instruction file for 
│   ├── deploy.ipynb                     <-- Instruction file for ```
│   ├── score.ipynb                      <-- Instruction file for 
```



During the training, deploying, and scoring process.  Two additional configuration files will be generated.

```
├── config
│   ├── model_info.yml                   <-- Output from train, and use by deployment 
│   ├── deployment_info.yml              <-- Output from deployment, and use by score
```



The details of each recipe can be found using the following table. 

|                   Recipe                    |                 Information                 |
| :-----------------------------------------: | :-----------------------------------------: |
|    Calculating CO2 Equivalent Emissions     | [co2e_emission](../co2e_emission/README.md) |
|  Identifying Anomalies (rule-based model)   |                [harmonic_rule](../harmonic_rule/README.md)             |
|       Estimating the Load Energy Loss       |                 [energyloss](../energyloss/README.md)               |
|      Identifying Anomalies (ML model)       |                   [anomaly](../anomaly/README.md)                  |
| Estimating Health Score from DGA (ML model) |                 [health_dga](../health_dga/README.md)              |

