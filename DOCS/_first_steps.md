# Getting started: first steps using the GrainSizeTools script

Installing GrainSizeTools
-------------

GrainSizeTools uses [uv](https://docs.astral.sh/uv/) for dependency and environment management. It requires Python 3.9 or higher.

### 1. Install uv

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Or via pip: `pip install uv`

### 2. Clone the repository

```bash
git clone https://github.com/marcoalopez/GrainSizeTools.git
cd GrainSizeTools
```

### 3. Create the environment and install dependencies

```bash
uv sync --all-groups
```

This creates a `.venv` virtual environment with Python 3.11 and installs all dependencies (numpy, scipy, matplotlib, pandas, pyyaml) along with JupyterLab for running notebooks.

### 4. Launch JupyterLab

```bash
uv run jupyter lab
```

Then open any of the template notebooks in `grain_size_tools/` or the examples in `DOCS/`.

> [!IMPORTANT]
> Scope: The GrainSizeTools script is not designed to deal with microscopic images but to analyse and visualize grain size populations and estimate stresses via paleopiezometers. **It is therefore necessary to measure the grain diameters or the sectional areas/volumes of the grains in advance and store them in a txt/csv/excel file**. For this task, we highly encourage you to use the [*ImageJ*](http://rsbweb.nih.gov/ij/) application or one of their different flavours (see [here](http://fiji.sc/ImageJ)). ImageJ-type applications are public-domain image processing programs widely used for scientific research that runs on Windows, macOS, and Linux platforms. The documentation contains a quick tutorial on how to measure the areas of the grain profiles with ImageJ, see *Table of Contents*. The combined use of **ImageJ** and **GrainSizeTools script** is intended to ensure that all data processing steps are done through free and open-source programs/scripts that run under any operating system. If you are dealing with EBSD data instead, we encourage you to use the [MTEX toolbox](https://mtex-toolbox.github.io/) for grain reconstruction (a tutorial on this will be available soon).

## Importing GrainSizeTools

GrainSizeTools is a proper Python package. In any Jupyter notebook or Python script, import it as follows:

```python
import numpy as np
import pandas as pd
import grain_size_tools as gst
```

After import, the following messages will appear in the console confirming each submodule was loaded:

```
module averages imported
module plot imported
module stereology imported
module template imported
```

All functionality is accessed through the `gst` namespace:

```python
gst.summarize(data)          # describe grain size population
gst.plot.distribution(data)  # visualize distribution
gst.stereology.Saltykov(data)  # stereological correction
gst.piezometers.calc_diffstress(piezometer, grain_size=40.0)  # paleopiezometry
```

The documentation is written assuming you will work in **JupyterLab**. Ready-to-use template notebooks are provided in `grain_size_tools/` (see `grain_size_analysis_template.ipynb`, `stereology_analysis_template.ipynb`, and `paleopizometry_template.ipynb`).

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/Jupyter_lab.png?raw=true)

*The JupyterLab development environment: an interactive data science environment for creating documents mixing code, equations, visualizations and narrative text.*



## Get information on the GrainSizeTools methods

You can get a list of the main methods by typing in the console:

```python
gst.function_list()
```

![](https://raw.githubusercontent.com/marcoalopez/GrainSizeTools/master/FIGURES/function_list.png)

The package is organised around several submodules. To access a method within a submodule you write the package name, the submodule name and then the method name separated by dots. For example, to access the method ``qq_plot`` of the plot submodule:

```python
gst.plot.qq_plot()
```
and provide the required parameters within the parenthesis.

To explore the available methods within a submodule, type `gst.plot.` and hit the tab key — a complete list of methods will pop up.

#### Get detailed information on methods

You can get detailed information about any method or function using `?` after the method name:

```python
gst.averages.conf_interval?
```

In JupyterLab you can also use `Shift+Tab` inside the function parentheses to display the docstring inline.

## Importing data using the Spyder data importer

Spyder allows importing the data through a graphical interface named the Spyder data importer. To do this, select the variable explorer and then click on the import data icon in the upper left (Fig. 3). A new window will pop up showing different import options (Fig. 4).

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/new_variable_explorer.png?raw=true)

*Figure 3. The variable explorer window in Spyder. Note that the variable explorer label is selected at the bottom (indicated with an arrow). To launch the data importer click on the top left icon (indicated by a circle).*

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/import_data.png?raw=true)

*Figure 4. The two-step process for importing data with the Spyder data importer. At left, the first step where the main options are the (1) the variable name, (2) the type of data to import (set to data), (3) the column separator (set to Tab), and (4) the rows to skip (set to 0 as this assumes that the first row is the column names). At right, the second step where you can preview the data and set the variable type. In this case, choose import as DataFrame, which is the best choice for tabular data.*

Once you hit "Done" (in the bottom right) the dataset will appear in the variable explorer window as shown in figure 3. Note that it provides information about the type of variable (a Dataframe), the number of rows and columns (2661 x 11), and the column names. If you want to get more details or edit something, double-click on this variable and a new window will pop up (Fig. 5). Also, you can do a right-click on this variable and several options will appear.

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/variable_explorer02.png?raw=true)

*Figure 5. Representation of the dataset in the Spyder variable explorer. Note that the colours change with values.*

> 👉 More info on the Spyder variable explorer here: https://docs.spyder-ide.org/variableexplorer.html



## Importing tabular data using the console

An alternative option is to import the data using the console. For this, [Pandas](https://pandas.pydata.org/about/index.html) is the de facto standard Python library for data analysis and manipulation of table-like datasets (CSV, excel or text files among others). The library includes several tools for reading files and handling of missing data and when running the GrainSizeTools script pandas is imported as ```pd``` for its general use.

All Pandas methods to read data are all named ```pd.read_*``` where * is the file type. For example:

```python
pd.read_csv()          # read csv or txt files, default delimiter is ','
pd.read_table()        # read general delimited file, default delimiter is '\t' (TAB)
pd.read_excel()        # read excel files
pd.read_html()         # read HTML tables
# etc.
```

For other supported file types see https://pandas.pydata.org/pandas-docs/stable/user_guide/io.html

The only mandatory argument for the reading methods is to define the path (local or URL) with the location of the file to be imported. For example:


```python
# set the filepath, note that is enclosed in quotation marks
filepath = 'C:/Users/marco/Documents/GitHub/GrainSizeTools/grain_size_tools/DATA/data_set.txt'

# import the data
dataset = pd.read_table(filepath)

#display the data
dataset
```

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/dataframe_output.png?raw=true)

Some important things to note about the code snippet used above are:

- We used the ``pd.read_table()`` method to import the file. By default, this method assumes that the data to import is stored in a text file separated by tabs. Alternatively you can use the ``pd.read_csv()`` method (note that csv means comma-separated values) and set the delimiter to ``'\t'`` as follows: ``pd.read_csv(filepath, sep='\t')``.
- When calling the variable ``dataset`` it returs a visualization of the dataset imported, which is a tabular-like dataset with 2661 entries and 11 columns with different grain properties.

In Python, this type of tabular-like objects are called (Pandas) *DataFrame* and allow a flexible and easy to use data analysis. Just for checking:

```python
# show the variable type
type(dataset)

pandas.core.frame.DataFrame
```

Pandas' reading methods give you a lot of control over how a file is read. To keep things simple, I list the most commonly used arguments:

```python
sep         # Delimiter/separator to use.
header      # Row number(s) to use as the column names. By default it takes the first row as the column names (header=0). If there is no columns names in the file you must set header=None
skiprows    # Number of lines to skip at the start of the file (an integer).
na_filter   # Detect missing value markers. False by default.
sheet_name  # Only for excel files, the excel sheet name either a number or the full name of the sheet.

```

An example using several optional arguments might be:

```python
dataset = pd.read_csv('DATA/data_set.csv', sep=';', skiprows=5, na_filter=True)
```

which in plain language means that we are importing a (fictitious) ``csv`` file named ``data_set`` that is located in the folder ``DATA``. The data is delimited by a semicolon and we ignore the first five lines of the file (*i.e.* column names are supposed to appear in the sixth row). Last, we want all missing values to be handled during the import. 

> 👉 more details on Pandas csv read method: https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.read_csv.html

The package includes a method named ```gst.get_filepath()``` to get the path of a file through a file selection dialog instead of directly writing it. This can be used in two ways:

```python
# store the path in a variable (here named filepath for convenience) and then use it when calling the read method
filepath = gst.get_filepath()
dataset = pd.read_csv(filepath, sep='\t')

# use gst.get_filepath() directly within the read method
dataset = pd.read_csv(gst.get_filepath(), sep='\t')
```

Lastly, Pandas also allows to directly import tabular data from the clipboard (i.e. data copied using copy-paste commands). For example, after copying the table from a text file, excel spreadsheet or a website using: 

```python
dataset = pd.read_clipboard()
```



## Basic data manipulation (using Pandas)

Let's first see how the data set looks like. Instead of calling the variable (as in the example before) we now use the ``head()`` and ``tail()`` methods so that it only shows us the first (or last) rows of the data set

```python
dataset.head()  # returns 5 rows by default, you can define any number within the parenthesis
```

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/dataframe_output_head5.png?raw=true)

The example dataset has 11 different columns (one without a name). To interact with one of the columns we must call its name in square brackets with the name in quotes as follows:

```python
# get the column 'Area' and multiplied by two
dataset['Area'] * 2
```

```
0         314.5
1        4119.5
2        3923.0
3       10857.0
4         748.0
         ...   
2656      905.0
2657     2162.5
2658     1027.0
2659      555.5
2660     1450.0
Name: Area, Length: 2661, dtype: float64
```

If you want to remove one or more columns, you can do it with the ``drop()`` method. For example, let's remove the column without a name.

```python
# Remove the column without a name from the DataFrame
dataset.drop(columns=' ', inplace=True)
dataset.head(3)
```

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/dataframe_output_head3.png?raw=true)

If you want to remove more than one column pass a list of columns instead as in the example below:

```python
dataset.drop(columns=['FeretX', 'FeretY'], inplace=True)
```

### Create new columns

The example dataset does not contain any column with the grain diameters and therefore we have to estimate them. For example, assuming the data comes from a thin section, you can estimate the apparent diameters from the section areas using the equivalent circular diameter (ECD) formula which is

ECD = 2 * √(area / π)

we will call the new column ``'diameters'``


```python
# Estimate the ECDs and store them in a column named 'diameters'
dataset['diameters'] = 2 * np.sqrt(dataset['Area'] / np.pi)
dataset.head()
```

![](https://github.com/marcoalopez/GrainSizeTools/blob/master/FIGURES/dataframe_output_newcol.png?raw=true)

You can see a new column named diameters.

> 👉 In the examples above we define the square root as ``np.sqrt``, the arithmetic mean as ``np.mean``, and pi as  ``np.pi``. In this case, ``np.`` stems for NumPy or numerical Python, a core package for scientific computing with Python, and the keyword after the dot is the method or the scientific value to be applied. If you write in the console ``np.`` and then press the TAB key, you will see a large list of available methods. In general, the method names are equivalent to those used in MATLAB or R but always by adding the ``np.`` first.

### A list of useful Pandas methods

Some things you might want to try (just copy-paste in interactive cells below and explore):

```python
# Reduction
dataset.mean()          # estimate the mean for all columns
dataset['Area'].mean()  # estimate the mean only for the column Area
dataset.std()           # estimate the (Bessel corrected) standard deviation
dataset.median()        # estimate the median
dataset.mad()           # estimate the mean absolute deviation
dataset.var()           # estimate the unbiased variace
dataset.sem()           # estimate the standard error of the mean
dataset.skew()          # estimate the sample skewness
dataset.kurt()          # estimate the sample kurtosis
dataset.quantile()      # estimate the sample quantile

# Information
dataset.describe()   # generate descriptive statistics
dataset.info()       # display info of the DataFrame
dataset.shape()      # (rows, columns)
dataset.count()      # number of non-null values

# Data cleaning
dataset.dropna()        # remove missing values from the data

# Writing to disk
dataset.to_csv(filename)    # save as csv file, the filename must be within quotes
dataset.to_excel(filename)  # save as excel file
```



***Well, I'm afraid you've come to the end. Where do you want to go?***

[return me to the home page](https://marcoalopez.github.io/GrainSizeTools/)  

[take me to “Getting started: first steps using the GrainSizeTools script”](https://github.com/marcoalopez/GrainSizeTools/blob/master/DOCS/_first_steps.md)

[take me to “Describing the population of grain sizes”](https://github.com/marcoalopez/GrainSizeTools/blob/master/DOCS/_describe.md)

[take me to “The plot module: visualizing grain size distributions”](https://github.com/marcoalopez/GrainSizeTools/blob/master/DOCS/_Plot_module.md)

[take me to “Paleopiezometry based on dynamically recrystallized grain size”](https://github.com/marcoalopez/GrainSizeTools/blob/master/DOCS/_Paleopizometry.md)

[take me to “The stereology module”](https://github.com/marcoalopez/GrainSizeTools/blob/master/DOCS/_Stereology_module.md)