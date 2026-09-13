# Wind Energy Analysis

## About the Project

This project focuses on investigating the development of wind energy across different countries and exploring the factors that may be associated with its development.

The main research question is:

> **Why does wind energy develop rapidly in some countries while remaining limited or almost absent in others?**

To investigate this question, data on wind electricity generation are combined with economic, demographic, social, and institutional indicators for different countries.

The project is not limited to searching for simple linear relationships. It also explores potential non-linear relationships, time effects, and combinations of multiple factors that may not be apparent when individual variables are considered separately.

The analysis does not include the natural and geographical potential of different territories, such as terrain characteristics, climatic conditions, or actual wind resources. Therefore, the results should be interpreted as an analysis of statistical patterns rather than a complete causal explanation of wind energy development.

---

## Research Objectives

The main objective of the project is to investigate the relationship between wind energy development and the socioeconomic characteristics of countries.

The analysis addresses the following questions:

* Is wind electricity generation associated with economic development?
* How is wind energy development related to human development?
* Is there a relationship between wind energy development and population density?
* Is wind energy development associated with urbanization?
* Is there a relationship with life expectancy?
* Is wind energy development associated with internet usage?
* Is there a relationship between wind energy development and the quality of governance?
* Do non-linear relationships exist between the variables under consideration?
* Can changes in some indicators precede changes in wind energy development?

The project therefore does not aim to identify a single factor that explains wind energy development. Instead, it aims to identify a combination of patterns and relationships in international data.

---

## Data

The main data source is **Our World in Data (OWID)**.

The primary wind energy indicator used in the analysis is:

* `wind_electricity` — electricity generation from wind, measured in TWh.

The OWID dataset also provides:

* `GDP`;
* `population`.

Additional indicators were collected from other sources:

GDP — Our World in Data
population — Our World in Data
life_expectancy — World Bank
population_density — World Bank
urbanization — World Bank
control_of_corruption — World Bank Worldwide Governance Indicators
HDI — United Nations Development Programme
internet_users — World Bank

The resulting dataset combines wind electricity generation with economic, demographic, social, and institutional characteristics of countries.

---

## Data Preparation

The initial structure of the raw dataset was explored using Python.

DuckDB / SQL was then used for more detailed data inspection and preprocessing.

Particular attention was given to missing values in `wind_electricity`.

A missing value in wind electricity generation does not necessarily mean zero generation. It may instead indicate that the corresponding observation is unavailable.

Therefore, missing values were not automatically replaced with zeros.

To determine when such a replacement could reasonably be made, the first observed positive value of wind electricity generation was identified for each country, together with its share of global wind generation in the corresponding year.

A working heuristic threshold of **0.1% of global generation** was then selected.

Countries whose first observed positive value accounted for less than 0.1% of global generation in the corresponding year were considered candidates for replacing preceding missing values with zero.

The 0.1% threshold is not a statistically derived universal criterion. It was selected after inspecting the distribution of the data as a practical boundary below which the first observed value was likely to represent a very small amount of generation.

For other ambiguous cases, missing values were retained rather than introducing potentially unjustified assumptions into the dataset.

---

## Historical and Special Entities

During data preparation, several countries and historical entities were identified for which `iso_code` was missing from the original dataset.

The following entities were handled separately:

* Kosovo;
* Czechoslovakia;
* East Germany;
* West Germany;
* USSR;
* Yugoslavia;
* Serbia and Montenegro.

Appropriate identifiers were assigned to these entities to allow them to be matched with additional datasets.

---

## Data Integration

To combine the different sources, the datasets were converted to a common country-year format.

`iso_code` was primarily used as the country identifier, allowing indicators from different sources to be matched without relying on differences in country naming conventions.

The resulting integrated dataset was stored as `analysis_data`.

The main variables include:

country — country name
iso_code — country code
year — year
wind_electricity — wind electricity generation
gdp — GDP
population — population
hdi — Human Development Index
population_density — population density
urbanization — urbanization rate
life_expectancy — life expectancy
network — share of the population using the internet
corruption — Control of Corruption indicator

---

## Workflow

The project is being developed in several stages.

### 1. Python — Initial Exploration

Python was used to initially inspect the structure of the raw dataset, the number of countries and years, missing values, and the main characteristics of the data.

### 2. SQL / DuckDB — Data Processing

DuckDB was used for:

* detailed exploration of the raw data;
* checking missing values;
* working with country-year observations;
* identifying cases where missing values could be replaced with zero;
* creating cleaned datasets;
* integrating multiple data sources.

### 3. Excel — Exploratory Analysis

Excel was used for preliminary exploration of the resulting dataset.

This stage focused on:

* differences between countries;
* major trends;
* simple relationships between variables;
* potential non-linear relationships;
* identifying directions for further statistical analysis.

Excel is used as a tool for preliminary exploration and hypothesis generation rather than as the primary statistical analysis tool.

### 4. Python — Advanced Analysis

Python will be used for more detailed statistical analysis.

The next stage will investigate both simple and more complex relationships between wind energy development and the selected factors.

Planned methods include:

* correlation analysis;
* linear models;
* polynomial and other non-linear models;
* residual analysis;
* multivariable regression;
* accounting for differences between countries and years;
* analysis of time effects and lags;
* Random Forest;
* XGBoost;
* SHAP for model interpretation.

### 5. Power BI — Final Visualization

Power BI will be used to present the final results as an interactive dashboard.

---

## Preliminary Analysis

A preliminary exploratory analysis was conducted in Excel using scatter plots and polynomial trendlines.

The results suggest that the relationship between wind electricity generation and several socioeconomic and demographic variables may have a pronounced non-linear pattern.

For **GDP**, **life expectancy**, and **HDI**, the relationship appears strongly curved and increasing. The polynomial trendlines produce the following R² values:

| Indicator       | Polynomial model R² |
| --------------- | ------------------: |
| GDP             |               0.947 |
| Life expectancy |               0.914 |
| HDI             |               0.940 |

This suggests that a simple linear model may not adequately describe the observed relationship between these indicators and wind electricity generation.

For **population density**, the relationship appears closer to linear. The quadratic model provides only a small improvement in fit, with an R² of **0.909**. Therefore, both linear and non-linear specifications should be compared for this variable during the next stage of the analysis.

**Urbanization** shows a considerably weaker relationship with wind electricity generation, with an R² of approximately **0.229**. The plot also shows a pronounced increase at lower levels of urbanization, suggesting that the overall relationship may be influenced by a particular range of observations.

Overall, the preliminary analysis suggests that **a single linear model is unlikely to adequately describe all of the relationships under investigation**.

These results are preliminary. R² indicates how well a particular model describes variation in the given dataset, but it does not by itself establish a causal relationship between the corresponding variables.

---

## Planned Statistical Analysis

The next stage of the project will include:

* calculating correlations between variables;
* comparing linear and polynomial models;
* performing residual analysis and assessing model adequacy;
* building multivariable regression models;
* accounting for differences between countries and years;
* investigating potential time effects and lags;
* comparing classical statistical models with machine learning approaches;
* using SHAP to interpret model results.

Particular attention will be given to the fact that observations from the same country across different years are not fully independent. Therefore, the country-year structure of the data and potential differences between countries and time periods will need to be considered in the subsequent analysis.

All results from this stage will be treated as **exploratory rather than causal**.

---

## Limitations

The study has several important limitations.

First, a statistical association between variables does not imply a causal relationship. Observed relationships should therefore be interpreted as statistical patterns rather than evidence of causation.

Second, the analysis does not include many physical and geographical factors that directly affect the potential for wind energy development. These include terrain characteristics, climatic conditions, and the actual wind resource of a territory.

Third, the final dataset combines several different sources, so the completeness and comparability of individual indicators may vary across countries and years.

In addition, the absolute amount of wind electricity generation may be related to the size of a country and its economy. Therefore, additional normalized indicators may be required during the analysis, such as wind generation per capita or the share of wind power in total electricity generation.

The purpose of the project is therefore not to provide a complete causal explanation of wind energy development, but to identify statistical patterns and potential relationships in international data.

---

## Project Status

The project is currently under development.

### Completed

* explored the structure of the raw dataset;
* cleaned the wind electricity generation data;
* handled missing `iso_code` values;
* developed an approach for handling missing wind generation values;
* prepared the `wind_data` table;
* prepared additional indicators from external sources;
* integrated the datasets into `analysis_data`;
* conducted preliminary exploratory analysis in Excel;
* identified preliminary signs of non-linear relationships between wind generation and several indicators.

### Next Steps

* conduct advanced statistical analysis in Python;
* compare linear and non-linear models;
* investigate country and year effects;
* evaluate machine learning models;
* interpret model results;
* build the final Power BI dashboard.

---

## Project Structure

The repository structure will be expanded as the project develops.

The main components of the project include:

* raw data;
* cleaned data;
* Jupyter Notebook for data processing;
* analysis results;
* Python analysis;
* Power BI dashboard;
* project documentation.
