# Accelerator Demo for Maximo Monitor Application
---

## 1. Prepare data

1.  Prepare dataset with `timestamp` column, `asset id` column and `feature` columns
2.  Split it for `training` and `inference`
3.  Refer it to create the [yaml](../config/yt_kpi.yml) file for input params - upload to cp4d as well as notebook folder

    [<img src="../images/yaml_config.png" width="600"/>](image.png)
    
    In this `config` file, below are the details of the parameters that need to be set:
    
    | Parameter           | Description                                                                                                             |
    |---------------------|-------------------------------------------------------------------------------------------------------------------------|
    | asset_id_column     | The column in your data which represents the ID of the asset or device name.                                          |
    | data_name           | The file name of the data that is to be used for training. If you are uploading on Watson Studio, the file name remains the same, but the prefix would change. |
    | device_description  | A description of your device type to publish to Monitor, while creating a device.                                      |
    | device_type         | The name of the asset group or device type, which is used to create a device type in Monitor.                         |
    | feature_columns     | The feature columns to use for training and inference.                                                                 |
    | feature_names       | Human readable names of the feature columns to be used to display them on Monitor.                                    |
    | target_columns      | The target column used for training. In supervised anomaly recipe, this is the column that will be used to predict upper and lower bound for it, based on the feature columns. |
    | target_names        | Human readable names of the target columns to be used to display them on Monitor.                                    |
    | timestamp_column    | Name of the column which contains the timestamps for your records.                                                    |
    | timestamp_format    | Format of the timestamp to be used to interpret them.                                                                  |
    | inference_data_name | File name of the dataset to be used for inference.                                                                     |


    ### Recommendations on data preparation
    1. Ensure the data has no null values, as streaming data to monitor expects non-null values. You may either drop the null rows, or impute them.
    2. For the sample data size of 10k, the overall training process takes around 5 minutes, and the inference takes around 5 minutes as well. The larger the data, the longer it would take to run training and inference.
    3. The timestamp column should be in a standard format, which is also to be provided in the config file. 
    4. If multiple assets are present in the data, they will be sent under different device names to monitor in terms of setup. However, the same KPI model will be deployed to each of these devices for inference.

## 2. Bring your own data
    
To explore our capabilities with your custom data, adhere to the following steps:

1. Put your data in the `data/` folder.
2. Revise the `KPI yaml` config file to include the pertinent information related to your data.
3. Continue with the subsequent steps in a similar manner.
4. While creating a device on `Monitor`, ensure to furnish a new device name.

