# Unsupervised Anomaly Detection

Anomaly Detection - Anomaly detection is an essential aspect of transformer monitoring. It enables the identification of abnormal behavior that may indicate a potential failure or fault in the transformer. Early detection of such anomalies can facilitate preventative maintenance, reducing the risk of costly downtime or damage to the transformer. 


## Configuration File

For this receipt, there is no requirement for the configuration.  


## Required Inputs

We use the performance of the distribution electrical transformer as an example.  For example,  the list of features selected is as 

| load_factor                                                  | loss_factor                                                  | energy_loss                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| A measure of the efficiency of its usage over a period of time. It is calculated as the ratio of the average load to the maximum load during a specific period. | A measure used to estimate the energy losses in the transformer over a period of time | Load loss refers to the energy loss in the transformer when it supplies power to a load. This loss is directly proportional to the square of the current passing through the transformer's windings. |

We utilize a method called SROM (Smarter Resources and Operations Management) to get the classification for anomaly using the best available models supported by SROM.  The output predicts and provides a binary classification. 

We can use the anomaly tags from the sensors combined with the binary classification.  

| tag_OTI_A                       | tag_OTI_T                      | tag_MOG_A                    |
| ------------------------------- | ------------------------------ | ---------------------------- |
| Oil Temperature Indicator Alarm | Oil Temperature Indicator Trip | Magnetic oil gauge indicator |



## Output

The output is a list of prediction classifications with value of [-1, 1].  It can be combined with the input to get the proper outputs. 



| date time | load factor| loss factor | energy loss |tag_WTI|tag OTI_A| tag OTI_T|tag MOG_A|Anomaly Output|
| :------------: | :-------: | :------: | :----: | :----: | :-----: | :-----: | :-----: | :--: |
| 2020-04-13 15:00:00 | 0.443362    | 0.211155    | 0.687995 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 16:00:00 | 0.414162    | 0.185366    | 0.600356 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 17:00:00 | 0.408793    | 0.180799    | 0.584892 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 18:00:00 | 0.500329    | 0.266058    | 0.876152 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 19:00:00 | 0.495787    | 0.261460    | 0.860318 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 20:00:00 | 0.517957    | 0.284269    | 0.938979 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 21:00:00 | 0.517996    | 0.284310    | 0.939119 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 22:00:00 | 0.521028    | 0.287500    | 0.950144 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-13 23:00:00 | 0.498605    | 0.264309    | 0.870125 | 1         | 0         | 0         | 0              | 1    |
| 2020-04-14 00:00:00 | 0.443020    | 0.210844    | 0.686933 | 1         | 0         | 0         | 0              | 1    |
