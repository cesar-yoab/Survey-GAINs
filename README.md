# Survey Generative Adversarial Imputation Networks.
In this repository we implement [GAINs](https://arxiv.org/abs/1806.02920) with the 
to be used in survey data and/or
data sets comprised of only categorical varibles. All the experiments are conveniently inside
the jupyter notebook vailable in this repository. T
here is button at the top of the notebook to open it on the Google Colab Platform. We 
recomend doing this to leverage the free GPU available at Google Colab. More information
on how to reproduce our results below.

## Reproducing Results
There are two parts to this project: the actual experiment and the report that goes with it.
Before running any of the code make sure you have the correct pre-requisites.

NOTE: All our code was created in Ubuntu 19 and therefore the following instructions are for UNIX (Linux, Mac) 
like systems, please use the equivalent Windows commands if that is your operating system.

### Kagge Data
The Kaggle data used in the experiments can be found [here](https://www.kaggle.com/c/kaggle-survey-2020). You will need a Kaggle account to download the data set. This data is
part of an ongoing Kaggle competition.

### ACS Data
To download this data go to the [IPUMS USA website](https://usa.ipums.org/usa/)
create an account if you don't have one, login and follow the next steps:

1. After you login click on the tab **SELECT DATA**
2. Click **SELECT SAMPLES** uncheck "Default sample from each year" and only check **2018 ACS**
3. Click on **Submit Sample Selection** 
4. Under "SELECT HARMONIZED VARIABLES" go to HOUSEHOLD>GEOGRAPHIC and add **STATEFIP** to the cart
5. Go to PERSON>DEMOGRAPHIC and add **SEX** and **AGE** to the cart
6. Go to PERSON>RACE, ETHNICITY, AND NATIVITY and add **RACE** and **HISPAN** to the cart
7. Click on "View Cart" and clik on **CREATE DATA EXTRACT**
8. Download the file (It will be large file!)

After you download the file move it to the cloned repository. If the file you downloaded has
the termination ".csv.gz" then run the following command first

```bash
find . -name '*.csv.gz' -print 0 | xargs -0 -n1 gzip -d
```

This should create a csv file. Then to run python cleaning script we recommend creating
a virtual environment. This can be done by running:

```bash
python -m venv .venv
source .venv/bin/activate
python -m pip install --upgrade pip
pip install -r requirements.txt
```

Then run the scrip with:

```bash
mv ACS_FILENAME ./data
source .venv/bin/activate
python3 clean_ipums.py --data='./data/ACS_FILENAME'
```
The script should automatically clean the data and output a file with the name **post-strat.csv**. Make sure
you replace "ACS_FILENAME" with the name of the file in your computer.

### Reproducing Experiment Results
Click on the "Open in Colab" button at the top of the notebook. Once Google Colab opens, make
sure that you have GPU enabled. To do this go to _Runtime>Change Runtime type_ and select **GPU** as 
the hardware accelarator. The second chunk of code in the notebook checks that you have GPU enabled,
make sure the ouput of this block is

```
device(type='cuda')
```

In order to run the code you will now need to upload the data sets to colab. On the side panel click
on "Files" and then click on "Upload to session storage". To make this process simples we compressed
the data set into zip files. The colab notebook includes code to unzip and process the data.

Our implementation is based on:
- [jsyoon0823/GAIN](https://github.com/jsyoon0823/GAIN)
- [dhanajitb/GAIN-Pytorch](https://github.com/dhanajitb/GAIN-Pytorch)


### (OPTIONAL) Create Report
The report is created using R Mark Down. We recommend you install R studio which comes with 
Rmd installed by default. Open the R project in this repository and install the following
packages:

1. tidyverse
2. cowplot

you can install them by typing the following into an R console

```R
install.packages("PACKAGENAME")
```

The colab notebook should contain code that saves the results to a csv file. Download this files
by going to "Files" selecting the generated csv files and click on "Download". Open the Rproject
on Rstudio and go to the report.Rmd file, click **knit**. This should generate a pdf file however
the full report can be found in the report folder. 