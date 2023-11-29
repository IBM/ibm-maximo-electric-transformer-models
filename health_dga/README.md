# DGA Health Index

The Predictive DGA (Dissolved Gas Analysis) Health Index Model serves as a predictive maintenance tool primarily in the electrical power transformer domain. It examines the gases dissolved in the transformer oil, which often reflect the transformer's physical health.

This model's core objective is to evaluate the transformer's health and foresee potential system failures. Leveraging advanced machine learning algorithms and statistical methodologies, the Predictive DGA Health Index Model provides an accurate assessment of the transformer's state based on the gas composition in the transformer oil and additional features. Notably, this model can be used in situations characterized by data **sparsity** where not all features are required.

It focuses on analyzing key metrics, including gases like hydrogen, methane, acetylene, ethylene, and ethane, as well as dielectric measurements and moisture levels. Fluctuations in these gas concentrations, changes in dielectric quality, or shifts in moisture content can hint at various fault types, including overheating, arcing, or insulation degradation. The culmination of this model's evaluation is a computed health index.

For utilities and industries utilizing transformers, the Predictive DGA Health Index Model becomes indispensable. It empowers them to plan preventative maintenance activities, thereby sidestepping expensive and unanticipated equipment failures.

## Input

In this example, we predict the health index while omitting the age of the asset.

| **AssetID** | **Timestamps** | **DGAR-H2** | **DGAR-O2** | **DGAR-N2** | **DGAR-CH4** | **DGAR-CO** | **DGAR-CO2** | **DGAR-C2H4** | **DGAR-C2H6** | **DGAR-C2H2** | **DBDS** | **POWER_FACT** | **INTER_V** | **DI_RIG** | **H2O** | **Health index** |
| ----------- | -------------- | ----------- | ----------- | ----------- | ------------ | ----------- | ------------ | ------------- | ------------- | ------------- | -------- | -------------- | ----------- | ---------- | ------- | ---------------- |
| 9010        | 53:22.1        | 2845        | 5860        | 27842       | 7406         | 32          | 1344         | 16684         | 5467          | 7             | 19.0     | 1.0            | 45          | 55         | 0       | 95.2             |

Note: This dataset can reside either on local storage or in a COS (Cloud Object Storage) bucket.

## Output Definitions

| **AssetID** | **Timestamps** | **DGAR-H2** | **DGAR-O2** | **DGAR-N2** | **DGAR-CH4** | **DGAR-CO** | **DGAR-CO2** | **DGAR-C2H4** | **DGAR-C2H6** | **DGAR-C2H2** | **DBDS** | **POWER_FACT** | **INTER_V** | **DI_RIG** | **H2O** | **Health index** | **prediction** |
| ----------- | -------------- | ----------- | ----------- | ----------- | ------------ | ----------- | ------------ | ------------- | ------------- | ------------- | -------- | -------------- | ----------- | ---------- | ------- | ---------------- | -------------- |
| 9010        | 53:22.1        | 2845        | 5860        | 27842       | 7406         | 32          | 1344         | 16684         | 5467          | 7             | 19.0     | 1.0            | 45          | 55         | 0       | 95.2             | 84.561         |

