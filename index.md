---
layout: default
---
# Projects

* [Time Series Analysis with ARIMA and SARIMA](#RFG_Time_Series)
  
* [Protein Data Bank Crystallization Conditions Analysis](#PDB_Analysis)

\
\
\
<a name="RFG_Time_Series"></a>
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
     * Line plots displaying in-sample and out-of-sample results for each group in every dataset are created and stored in the cv_plots directory. RMSE and MAPE results are also provided in the graphs.
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

\
\
\
<a name="PDB_Analysis"></a>
## Protein Data Bank Crystallization Conditions Analysis

**Project Overview**: The Protein Data Bank (PDB) is where the archives for biological macromolecule 3D structure data are maintained and stored. These archives are freely available to the public and the data used in this project was downloaded from ftp.wwpdb.org/pub/pdb/data/structures/all. Although the archive contains data for proteins, DNA, and RNA, protein data is solely used in this project. Only 3D structure data that was experimentally determined is used, compared to structure models computed using AI programs such as AlphaFold.

**The main goals of this project are**: 

* To extract the following from the PDB archive (204,170 files, 290 GB) into a Pandas DataFrame:
     * Protein ID
     * FASTA Sequence
     * Solvent Concentration
     * Matthew's Coefficient
     * Crystallography Type
     * Vapor Diffusion Method
     * pH
     * Precipitates: Split on concentration metric into 2 groups (percentage or molarity)
          * Organic Precipitates (percentage)
          * Salt Precipitates (molarity)
          * _Note: Some chemicals may be inappropriately labeled as organic or salt_
        
* To analyze the extracted data and gain insights that could be useful in future crystallization of novel proteins using traditional experimental methodologies. Some of the charts and graphs generated are as follows:
    * Table and bar graphs sumarizing missing data
    * Heatmap showing nullity correlation between extracted conditions, which shows if the presence of one condiditon affects the presence of another
    * Table that contains all unique chemicals found in the PDB along with number of occurences
    * Table and pie chart showing top 10 chemicals found in the PDB
    * Scatter plots that show the top 10 chemicals by number of occurences with concentration of each occurance plotted (shows concentration grouping)
    * Bar chart that displays pH of every crystalliation cocktail found in the PDB, which gives insight to the most used pH values for crystallizing proteins
    * Table and pie chart that groups pH values into whole number groups to better summarize most frequently used pH values

[Data Analysis of the PDB](https://at58474.github.io/another-page)  

* To use various supervised deep learning models such as a convolutional neural network (CNN) to determine the crystallography conditions needed to obtain diffractable protein crystals by learning and mapping those conditions from the FASTA protein sequence. The goal is to then apply this to proteins whose structure has yet to be determined to give insight into what crystallizaiton conditions may be optimal for producing shootable crystals. The target crystallography parameters are pH, temperature, and chemical composition of the mother liquor.

### Crystallography Conditions Extraction Module Overview  

[ccmodule.py](https://github.com/at58474/Crystallization-Conditions/blob/27267c8d78e8d43083900a3c788bc7bc1ba6b38a/ccmodule.py)  (892 lines)

**Data Collection and Preprocessing**: The following describes the process taken to collect the 204,170 *.pdb files.  

* The files were downloaded from the Protein Data Bank's FTP servers at 'ftp.wwpdb.org/pub/pdb/data/structures/all'. This process took roughly 24 hours to complete.

* The files were indivudually compressed as *.enz.gz (gzip) files, and thus needed to be extracted (*.ent.gz -> *.ent). The 7zip command line program was used to extract by using the following command, and took around 8 hours to complete.

```console
(7zip cmd): "C:\Program Files\7-zip\7z.exe" e"*.gz" -o..\Extracted\
```

* The final step was to convert the file extension from *.ent to *.pdb. This was done by simply renaming the files using a Python script. Multiprocessing was utilized and this completed in roughly four hours.

```python
class ConvertToPDB:

    def __init__(self, ent_file_folder):
        self.ent_file_folder = ent_file_folder

    @staticmethod
    def change_file_extension(file):
        base = os.path.splitext(file)[0]
        os.rename(file, base + '.pdb')

    def handle_multiprocessing(self):
        tic = time.time()
        ent_files = Path(self.ent_file_folder).glob('*.ent')
        pool = multiprocessing.Pool()
        pool.map(self.change_file_extension, ent_files)
        pool.close()
        toc = time.time()
        print('Done in {:.4f} seconds'.format(toc - tic))
```

**ccmodule.py Summary**:

* PDBPreprocessingSequences:
  * This class extracts the primary protein sequence from the SEQRES section of each PDB file. The extracted sequences are then converted into FASTA format using the BioPython library. The primary sequences exclude modifications such as cleaved his-tags, chain ends, and mobile loops, so often times the SEQRES sequence will differ from the sequence generated from the ATOM coordinates. This may pose a problem when predicting crystallization conditions since these modifications will not be used during modeling. Cleaving a his-tag, for example, can improve macromolecule packing during crystal growth, so this may couse erraneous predictions. A future module may also exract the ATOM sequence and determine which modifications were performed, but the BioPython library used did this with a high rate of failure.
  * Creates .fasta file for each unique protein, which is then used in the class below to append the fasta sequence to the main dataframe where a protein ID match is found.
  * Utilizes multiprocessing with all available cores.


* PDBCrystallizationConditions:
  * This is the main class where all of the conditions are extracted from the PDB files. The following methods are called for every .pdb files and then each condition is stored in the conditions dataframe.
  * **extract_remark_280()**: This method opens the PDB file and extracts every line in the REMARK 280 section into a list where every item is a string.
  * **extract_solvent_content()**: Extracts the solvent content from the REMARK 280 section, if it exists, and stores the value into the Solvent_Concentration column in the dataframe.
  * **extract_matthews_coefficient()**: If included in the file, this method finds and stores the Matthew's Coefficient into the dataframe.
  * **extract_crystallization_conditions()**: Determines if the data is part of the crystallization conditions section and stores that information into a new list variable which contains chemical composition, pH, temperature, and crystallization method. This is mostly used to reduce the complexity of the original list in order to more easily extract the relevant data.
  * **extract_crystallography_type()**: The crystallography types found in the PDB are VAPOR DIFFUSION, MOLECULAR REPLACEMENT, FIBER SEEDING, DIALYSIS, BATCH, and MICRODIALYSIS. Vapor diffusion is the most widely used method and is further categorized into hanging drop or sitting drop. If one of these types is found then it is stored into the dataframe.
  * **extract_vapor_diffusion_type()**: If the experimental method used was vapor diffusion, then this method determines if the hanging drop or sitting drop method was used and records the finding into the dataframe.
  * **extract_ph()**: Extracts the pH of the mother liquor and stores the value into the dataframe.
  * **extract_organic_precipitates()**: Since hundreds of chemicals can be found within the .pdb files downloaded from the Protein Data Bank, and typos and missing information is prevelant, this method required the most effort to successfully extract the chemicals and their cooresponding concentrations. Reference the docstring in the ccmodule.py file above for more detail into how this was performed. In summary, the organic precipiates were assumed to have concentration measured by percentage, either w/v or v/v, so a REGEX was used to extract this data. If the extracted data contained certain words then the data was erraneous and was removed. If an exact match was not found from comparing the word to a list of all chemicals found in the PDB, then it was replaced with the most similar match. The concentration and chemical were stored into the dataframe as a list wrapped dictionary.
  * **extract_salt_precipitates()**: This method extracts the salt precipitates from the crystallization conditions list. It uses a similar approach as the organic precipitates method, exept the concentration of the salt precipitates were assumed to be measured in molarity. A list of dictionaries were also utilized to store the data into a single location in the dataframe.

* ConvertToPDB:
  * This class converts the .ent files into .pdb files and was summarized in the Data Collection and Preprocessing section above.


**controller.py Summary**:  

* This simple controls which classes will be run and where the data is stored that is required by the module to execute properly.

* This is where the following file paths need to be set:
  * ent_file_folder
  * pdb_file_folder
  * fasta_file_folder

* The following three parameters must also be set to True or False:
  * convert_ent_pdb
  * run_extract_sequence
  * run_extract_conditions

