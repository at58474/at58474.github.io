---
layout: default
---
# Projects

Below is a list of projects showcasing some of the skills I have developed through undergraduate and masters level coursework, certifications, and self study.

## Time Series Analysis with ARIMA and SARIMA

**Project Overview**: The primary goal of this project was to accurately forecast river flow for recreational purposes by using historical observations collected from the United States Geological Survey (USGS). Another goal was to develop a Python package for performing time series analysis and forecasting on a given dataset, and organizing the results to be easily accessed. The final objective of this project was to determine which model attributes had the greatest impact on forecasting performance. The attributes that were analyzed were the time interval of the data, method of aggregation, SARIMAX forecast() vs predict(), and which evaluation metric was the best for determining model order when running the auto-ARIMA module. The main features of this module are summarized below:

_All plots and tables created by the module are saved into the data directory. The structure of this directory is explained in the readme file_

[README.md](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/README.md)

_A detailed composition of this project can be found here:_

[Time Series Model Performance Analysis with ARIMA and SARIMA for Streamflow Forecasting in the Appalachia](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/TS_Analysis_RFG.pdf)

### Time Series Module Overview (tsmodule.py)

[tsmodule.py](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/tsmodule.py)

* Preprocessing
     * Merges multiple datasets
     * Imputes missing data
     * Removes duplicate rows
     * Renames and deletes columns defined by list_col_del and dict_col_rename parameters
     * Reads raw data with a user defined delimiter
* Dataframe Creation
     * Resamples the dataset into time intervals denoted by the parameter frequency_list, then by using the first(), mean(), and max() aggregation methods. This results in the number of datasets equating to the number of items contained in frequency_list multiplied by 3.
     * A row cap parameter is included here to reduce the time to run the ARIMA/SARIMA models, if needed
* Parameter Estimation
     * Checks for stationarity with ADF test and automatically differences any non-stationary datasets
     * Creates ACF and PACF plots for all stationary data
     * Creates decomposition plots for each dataset that show trend, seasonality, and residual attributes
* Model Fitting
     * Performs in-sample non-rolling, in-sample 1-step-ahead rolling, and out-of-sample rolling forcasts for each dataset using either ARIMA or SARIMA
     * Creates plots for each of the forecasting methods
     * Creates tables for each forecasting method that displays AIC, MAPE, and RMSE metric values for the predict() and forecast() functions for each dataset 
* Diagnostics
     * Plots for each forecasting method are created using results from SARIMAX predict() and SARIMAX forecast() for each dataset
     * The diagnostic plots included are standardized residuals, histogram, normal Q-Q, and ACF plot of residuals
     * Two sets of plots are generated using both statsmodels plot_diagnostics() and custom methods
* Cross Validation
     * The number of cross validation groups and the size of each cross validaiton testing set can be set by the user
     * The datasets used can be specified by providing the dictionary key in the configuraiton file
     * Line plots displaying in-sample and out-of-sample resutls for each group in every dataset are created and stored in the cv_plots directory. RMSE and MAPE results are also provided in the graphs.
     * A table that contains every cross validaiton group for each dataset along with MAPE and RMSE values is created for comparing how well each model performed
* Auto ARIMA
     * The Auto-ARIMA class can be passed 1 time interval dataset at a time and will iterate through each parameter combination in the user-defined ranges provided in the configuration file
     * The resulting outputs are three pdf files containing tables that include the top 20 results sorted by AIC, MAPE, and RMSE. For example, file 1 contains results sorted by AIC, file 2 contains results sorted by MAPE, and file 3 contains results sorted by RMSE
* File Handling
     * File handling can be turned on or off in the configuration file. When set to True it automatically deletes any non-archived files before running the program. File archiving is not currently implemented and must be done by hand, but the folder structure for archiving the results is included.

### Configuration File (config.py)

[config.py](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/config.py)

The configuration file contains customizable settings and parameters that can be used to run analysis and forecasting with ARIMA or SARIMA on any given time series dataset. A detailed explanation of each paramater can be found in the readme file.

[README.md](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/README.md)

### Configuration File Validation (config_validation.py)

[config_validation.py](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/config_validation.py)

This is a parameter validation module that ensures the user-defined settings meet the requirements of the program. If an error is found an assertion will be thrown and the program will terminate until the parameter has been corrected.

### Time Series Module Controller (controller.py)

[controller.py](https://github.com/at58474/Time-Series-ARIMA-SARIMA/blob/main/controller.py)

This file imports the validated program settings and controls which classes from the time series module will be initialized. After modifying the configuration file to include the desired parameters, run this file to create the results that can be found in the plots directory.


[Link to another page](./another-page.html)



Text can be **bold**, _italic_, or ~~strikethrough~~.

[README.md](./Time-Series-ARIMA-SARIMA/README.md).

There should be whitespace between paragraphs.

There should be whitespace between paragraphs. We recommend including a README, or a file with information about your project.

# Header 1

This is a normal paragraph following a header. GitHub is a code hosting platform for version control and collaboration. It lets you and others work together on projects from anywhere.

## Header 2

> This is a blockquote following a header.
>
> When something is important enough, you do it even if the odds are not in your favor.

### Header 3

```js
// Javascript code with syntax highlighting.
var fun = function lang(l) {
  dateformat.i18n = require('./lang/' + l)
  return true;
}
```

```ruby
# Ruby code with syntax highlighting
GitHubPages::Dependencies.gems.each do |gem, version|
  s.add_dependency(gem, "= #{version}")
end
```

#### Header 4

*   This is an unordered list following a header.
*   This is an unordered list following a header.
*   This is an unordered list following a header.

##### Header 5

1.  This is an ordered list following a header.
2.  This is an ordered list following a header.
3.  This is an ordered list following a header.

###### Header 6

| head1        | head two          | three |
|:-------------|:------------------|:------|
| ok           | good swedish fish | nice  |
| out of stock | good and plenty   | nice  |
| ok           | good `oreos`      | hmm   |
| ok           | good `zoute` drop | yumm  |

### There's a horizontal rule below this.

* * *

### Here is an unordered list:

*   Item foo
*   Item bar
*   Item baz
*   Item zip

### And an ordered list:

1.  Item one
1.  Item two
1.  Item three
1.  Item four

### And a nested list:

- level 1 item
  - level 2 item
  - level 2 item
    - level 3 item
    - level 3 item
- level 1 item
  - level 2 item
  - level 2 item
  - level 2 item
- level 1 item
  - level 2 item
  - level 2 item
- level 1 item

### Small image

![Octocat](https://github.com/at58474/at58474.github.io/blob/master/assets/img/manuscript_cover.png)

### Large image

![Branching](https://github.com/at58474/at58474.github.io/blob/master/assets/img/manuscript_cover.png)

### Definition lists can be used with HTML syntax.

<dl>
<dt>Name</dt>
<dd>Godzilla</dd>
<dt>Born</dt>
<dd>1952</dd>
<dt>Birthplace</dt>
<dd>Japan</dd>
<dt>Color</dt>
<dd>Green</dd>
</dl>

```
Long, single-line code blocks should not wrap. They should horizontally scroll if they are too long. This line should be long enough to demonstrate this.
```

```
The final element.
```
