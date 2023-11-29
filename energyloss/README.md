# Estimation of Energy Efficiency/Loss Using Load Factor
Energy efficiency and loss in electrical transformers are critical KPIs directly influencing power systems' overall performance and cost-effectiveness.  They are also important indicators of the electrical transformer's health and the making of the decision for the replacement.  There are non-load loss and load loss for electrical transformers.   This document provides the empirical approaches to estimate the load loss, the significant portion of energy loss.

Load loss refers to the energy loss in the transformer when it supplies power to a load. This loss is directly proportional to the square of the current passing through the transformer's windings. The primary cause of load loss is the resistive heating of the transformer's windings. When current flows through the windings, it encounters resistance, generating heat. This heat is the energy being lost.

Energy loss estimation can help identify areas where energy loss is exceptionally high, allowing for targeted improvements to reduce these losses. By minimizing energy loss in the system, it is possible to increase its efficiency, reduce costs, and minimize environmental impact.


## Three KPIs for Energy Loss Estimation

The estimation process involves three KPIs, they are `Load Factor`(unit less),  `Loss Factor`(unit less), and `Energy Loss` rate (Kilo Watt KW or Watt W) or `Energy Loss` over a unit time (Kilo-Watt Hour KWH, or Watt-Hour WH). 

1. `Load Factor` for an electrical transformer is a measure of the efficiency of its usage over a period of time. It is calculated as the ratio of the average load to the maximum load during a specific period. The formula to calculate the load factor is:

$$
\text{Load Factor}=\frac{Average Load}{Maximum Load}
$$
- `Average Load`: This is the average power (in kilowatts, kW) that the transformer carries over a specific period,  say one hour
- `Maximum Load`: This is the peak or highest power (in kW) that the transformer carries for the operation time period.  It can be calculated or entered as an input for our calculation.

2. `Loss Factor` (unit less) is a measure used to estimate the energy losses in the transformer over a period of time.  In our calculation, we estimate this from the transformer's load (see the following two methods). A high `loss factor indicates inefficiencies in the transformer's operation, potentially leading to higher operational costs and a need for more frequent maintenance or replacement. 
2. `Energy Loss` refers to the energy that is not converted into useful output but dissipated as heat, including both no-load loss and load loss.   Our estimation focuses on the load loss, that varies with the load on the transformer. 




## Input 

Time Series Power Data must be a .csv and contain at least the two following columns:
* Timestamps
* Active Power (i.e. Load, Typically KW)

| Timestamp           | Active Power (KW) |
| ------------------- | ----------------- |
| 2023-04-25 22:00:00 | 54.722            |
| 2023-04-25 22:15:00 | 51.263            |
| 2023-04-25 22:30:00 | 52.74             |
| 2023-04-25 22:45:00 | 57.983            |
| 2023-04-25 23:00:00 | 57.136            |
| 2023-04-25 23:15:00 | 49.598            |


This dataset can be stored either locally or in a `COS` (Cloud Object Storage) bucket.

## Preparation of `parameter_config.yaml`

The following is an illustrated example of the parameters required to create the energy loss service.   We will explain the details of each section as follows.

    dataset_para:
      timestamp_col_name: 'Timestamp' # the dataset column specifies the sampling time
      power_col_name: 'Active Power (KW)' # the dataset column specifies the power of the electrical transformer
    transformer:
      power_unit: 'KW'  # Active Power, the unit of energy loss is based on this unit
      transformer_rating: 100 # The maximum capacity to handle power without exceeding its design temperature
      peak_power: 104.45  # The peak value of the power transferred
    estimate_loss_factor_method:  # estimate the power loss factor
      smooth_time_period: '1H'   # Smooth time range to allow the loss factor to be smoothed (medium is used), say '1H' or '2H'
      method: 'buller_woodrow'  # Please choose  either 'gustafson' or 'buller_woodrow'
      buller_woodrow_parameter: 0.7 # specific for 'buller_woodrow', say buller_woodrow_parameter = 0.7
    energyloss_output:
      full_load_winding_loss: 3.5  # the energy loss in the electrical transformer's windings when operating at full load.
      type: 'energyloss_rate'  # Can be 'energyloss_rate' with output as 'w', 'kw' or 'mw' or 'energyloss' with output as 'wh', 'kwh', or 'mwh')
      aggregation_time_period: '1H'  # This field is required only if type is 'energyloss', say '1H', '2H' or '1D'
    list_of_outputs:   # Please select from the following list load_factor, loss_factor and energyloss
      - load_factor
      - loss_factor
      - energyloss

A comprehensive `parameter_config.yaml` file must be provided to ensure the calculation functions appropriately.   The configuration has to be asset-specific.  

### Dataset Parameters - `dataset_para`
We need the following two columns for a dataset for the `load energy loss` estimation. 

- `timestamp_col_name` is the column name in the dataset containing the timestamp information for each data record.   The format selected is `%Y-%m-%d %H:%M:%S

- `power_col_name` is the column name in the dataset containing the power values (in kW) for each data record.


### Electrical Transformer - `transformer`
This part of the configuration is related to the specification of the electrical transformer.  

- _rating refers to the maximum power capacity of a transformer, expressed in kVA or MVA.

### Estimation Method - `estimate_loss_factor_method`
Method indicates the chosen approach for calculating energy losses, in this service, you can choose either `gustafson` or `buller_woodrow` method.

- Gustafson's equation is more accurate when only load factors are available and detailed load cycle data is not accessible. It simplifies the calculation and provides a practical approach for estimating energy losses based on empirical analysis.

- Buller Woodrow's equation is more accurate when detailed load cycle data is available. This method takes into account the variations in load. It adjusts the contribution of the linear and quadratic terms of the load factor in the loss calculation using the Buller-Woodrow Parameter (x).

In summary, Gustafson's equation is more accurate for situations with limited data, while Buller Woodrow's equation is more accurate when detailed load cycle data is available. The choice between the two methods depends on the data available and the desired accuracy of the energy loss estimation in a specific scenario.




#### Gustafson's Equation

Gustafson's equation is a method for calculating the Loss Factor in power distribution systems that simplifies the calculation by using load factors instead of detailed load cycle data. The equation is expressed as:

$$
\text{Loss Factor} = \text{Load Factor}^{1.912}
$$

where:
- `Loss Factor` = Energy loss factor
- `LoadFactor` = Ratio of the average power demand to the maximum power demand over a specified time period

This method is based on empirical data and provides a more practical and simplified approach for estimating energy losses, especially when only load factors are available.


#### Buller and Woodrow's Equation
Buller and Woodrow's equation is a method for calculating the Loss Factor in power distribution systems that considers the nonlinear relationship between load factor and energy loss. The equation is expressed as:

$$
\text{Loss Factor} = (1 - x) \times \text{Load Factor} + x \times (\text{Load Factor})^2
$$

where:
- `Loss Factor` = Energy loss factor
- `Load Factor` = Ratio of the average power demand to the maximum power demand over a specified time period
- `x` = Buller_Woodrow_Parameter, a constant between 0.7 and 0.85

This method requires detailed load cycle data and adjusts the contribution of the linear and quadratic terms of the load factor in the loss calculation using the Buller Woodrow Parameter (x). It is particularly useful when more accurate energy loss estimations based on variations in load are needed.

### Parameters for Energy Loss  - `energyloss_output`

The output for the energy loss is used as the output KPIs for the additional analysis, such as CO2e estimation and anomaly detection.   We estimate the energy loss using the `Loss Factor` and a parameter called `Full Load Winding Loss`. 

The `Full Load Winding Loss` (`full_load_winding_loss)` is the energy loss (in kW) in the transformer's windings when operating at full load.  The full_load_winding_loss has the power unit.   It is a parameter required from the configuration file.   A typical value for `full_load_winding_loss`  is 3.5.   For transformer 100 KVA, it is 3.5 KW (estimated). 


$$
\text{Energy Loss}_{rate} = \text{Full Load Winding Loss}*\text{Loss Factor}
$$

In the case, we want to calculate the energy loss for a given time period, say `T` (say one hour), then the energy loss can be computed as: 

$$
\text{Energy Loss} = \text{Energy Loss}_{rate}*T
$$



### Model KPIs output - `list_of_outputs`

Load Factor: The ratio of the transformer's average load to its maximum rated capacity, indicating how efficiently it is utilized.

Loss Factor: A measure of the transformer's energy loss during operation, accounting for factors such as resistance, core, and stray load losses as per either Gustafson's or Buller & Woodrow's method.

Estimated Load Loss: A calculated value representing the anticipated energy loss in the transformer during a given period, based on its load factor.




| asset_id | period | load_factor | loss_factor | load_loss |
|-------------|-------------|-------------|-------------|-------------|
|90150| 2019-06-25  | 0.353592    | 0.137005    | 345.253615 |
|90150| 2019-07-25  | 0.471900    | 0.237904    | 599.518190 |
|90150| 2019-08-24  | 0.512072    | 0.278126    | 700.876400 |
|90150| 2019-09-23  | 0.525415    | 0.292146    | 736.208374 |



# References:

 

1. [https://siemens.coursewebs.com/Courses/CommonFiles/Course%20Notes/USL%20TABS%201-6.pdf](https://siemens.coursewebs.com/Courses/CommonFiles/Course Notes/USL TABS 1-6.pdf)

   Gustafson (page  19) and buller_woodrow. (page 20)



# Appendix

### Buller & Woodrow Parameter (only required if the method is "buller_woodrow")

Buller_Woodrow_Parameter (x) is a constant between 0.7 and 0.85 used in the Buller-Woodrow loss equation to calculate the Loss Factor, which accounts for the nonlinear relationship between load factor and energy loss in a distribution system.

#### Setting the Buller Woodrow Parameter (x)

1. **Analyze historical energy loss data** from your power distribution system, focusing on the relationship between load factors and energy losses.
2. **Compare your system's data** to reference values from the literature or industry standards to validate your findings.
3. **Perform a regression analysis** using your historical data to determine the optimal value for the Buller-Woodrow parameter (x) that minimizes the difference between the estimated and actual energy losses. You can use software tools or statistical methods to perform the regression analysis.
4. **Ensure that the chosen value of x is within the range** of 0.7 to 0.85, as recommended in the literature. If the value is outside this range, consider re-evaluating your data or consulting with experts to identify any potential issues.

