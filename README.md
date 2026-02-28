![](https://raw.githubusercontent.com/marcoalopez/GrainSizeTools/master/FIGURES/new_header.webp)

_Maintained by [Marco A. Lopez-Sanchez](https://marcoalopez.github.io/) - This website was last modified: 2024-06-26_

[GrainSizeTools](https://doi.org/10.21105/joss.00863) is a free, open-source, cross-platform script written in [Python](https://www.python.org/) that provides tools for (1) quantifying and visualising grain size populations, (2) applying stereological methods to approximate the true grain size distribution from 2D sections and (3) estimating differential stress for different mineral phases via paleopiezometry. The script has been designed to be **accessible to users with no previous experience of the Python programming language** and focuses on scientific reproducibility by including **several ready-made templates for the different use cases**. For users with programming skills, the script is organised in a modular (functional) way to facilitate reuse and code extension.

**Latest release: v3.2.0**  
**Date: 2024-06-26**  
See notes at https://github.com/marcoalopez/GrainSizeTools/releases/tag/v3.2.0
[View project on GitHub](https://github.com/marcoalopez/GrainSizeTools)


## Features at a glance

- Import and manipulate tabular data sets including text, CSV, or Excel formats via Pandas.

- Fully automated descriptive statistics of grain size populations including:

  - Multiple averages (arithmetic and geometric means, median, and frequency peak ("mode") via Gaussian Kernel Density Estimator)
  - Estimation of robust confidence intervals (including some specific methods for lognormal populations such as the modified Cox or the GCI method)
  - Measures of dispersion and population shape
  - Normality and lognormality tests

- Estimation of differential stress using palaeopiezometers. It includes piezometric relations for quartz, olivine, calcite and feldspar (more to come!)

- Ready-to-publish plots in bitmap or vector format (see screenshots below), including:

  - Histograms and kernel density estimates
  - Area or volume-weighted plots
  - Normalized plots
  - Quantile-quantile plots, and more

- Stereology methods (approximating the true 3D grain size distribution from data collected in flat sections):

  - Saltykov method
  - Two-step method (log-normal fitting, shape characterization)
  - Estimate the volume fraction occupied by any grain size range

## Download

https://github.com/marcoalopez/GrainSizeTools/releases

## Installation

### Option A: Install from PyPI (recommended)

```bash
# core package only
pip install grainsizetools-jwestraadt

# with JupyterLab for running notebooks
pip install "grainsizetools-jwestraadt[jupyter]"
```

Then launch JupyterLab:

```bash
jupyter lab
```

### Option B: Install from source using uv

GrainSizeTools uses [uv](https://docs.astral.sh/uv/) for dependency and environment management.

#### 1. Install uv

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Or via pip: `pip install uv`

#### 2. Clone the repository

```bash
git clone https://github.com/jwestraadt/GrainSizeTools.git
cd GrainSizeTools
```

#### 3. Create the environment and install dependencies

```bash
uv sync --all-groups
```

This creates a `.venv` virtual environment with Python 3.11 and installs all dependencies (numpy, scipy, matplotlib, pandas, pyyaml) along with JupyterLab for running notebooks.

#### 4. Launch JupyterLab

```bash
uv run jupyter lab
```

Then open any of the template notebooks in `grain_size_tools/` or the examples in `grain_size_tools/example_notebooks/`.

### Running scripts directly

Any Python script or command can be run inside the managed environment using `uv run`:

```bash
uv run python your_script.py
```

## Usage

GrainSizeTools is a proper Python package and can be imported directly:

```python
import grain_size_tools as gst

# or import submodules individually
from grain_size_tools import summarize, plot, stereology, piezometers
```

### Describe a grain size population

```python
import numpy as np
import grain_size_tools as gst

# load your grain diameter data (in microns)
data = np.loadtxt('my_data.txt')

# print descriptive statistics with confidence intervals
gst.summarize(data)

# visualize the distribution
fig, ax = gst.plot.distribution(data)
```

### Stereological correction (2D → 3D)

```python
# Saltykov method
fig, axes = gst.stereology.Saltykov(data, numbins=12)

# Two-step method (returns lognormal parameters)
fig, ax = gst.stereology.two_step(data)
```

### Paleopiezometry

```python
# load the piezometric database
version, metadata, db = gst.piezometers.load_piezometers_from_yaml(None)

# list available piezometers
gst.piezometers.list_piezometers(db)

# estimate differential stress (MPa) from a grain size (microns)
gst.piezometers.calc_diffstress(db.quartz.__dict__['Stipp_Tullis'], grain_size=40.0)
```

## Documentation

https://github.com/marcoalopez/GrainSizeTools/wiki

## Screenshots

![](https://raw.githubusercontent.com/marcoalopez/GrainSizeTools/master/FIGURES/screenshots-01.webp)

## Citation guidelines

If you have used the script, please consider citing the following paper:

> Lopez-Sanchez, Marco A. (2018). GrainSizeTools: a Python script for grain size analysis and paleopiezometry based on grain size. *Journal of Open Source Software* 3, 863, https://doi.org/10.21105/joss.00863

By citing this paper, you are giving proper credit to the author and acknowledging his work.

## License

GrainSizeTools script is licensed under the [Apache License, Version 2.0 (the "License")](http://www.apache.org/licenses/LICENSE-2.0)

The documentation of GrainSizeTools script is licensed under a [Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License (CC BY-NC-SA 4.0)](https://creativecommons.org/licenses/by-nc-sa/4.0/). 

## Community guidelines

The GitHub site where the project is hosted offers several options (you'll need a GitHub account, it's free!)::

- Open a [discussion](https://github.com/marcoalopez/GrainSizeTools/discussions): This is a place to:
  - Ask questions you are wondering about.
  - Requests for specific features or share ideas.
  - Interact with the developers.
- Open an [issue](https://github.com/marcoalopez/GrainSizeTools/issues): This is a place to report or track bugs.
- Make a [pull request](https://github.com/marcoalopez/GrainSizeTools/pulls): You have modified, corrected or added a feature to one of the notebooks and send it to one of the developers to review it and add it to the main page.

---
*Copyright © 2017-2024 Marco A. Lopez-Sanchez*  

> [!WARNING]
> Please note that the information on this website and in the script documentation is provided without any warranty of any kind, either expressed or implied. It may therefore include technical inaccuracies or typographical errors. The author reserves the right to make changes or improvements to the content of this website and the script documentation at any time without notice. This website and its documentation are not responsible for the content of external links. 

*Hosted on GitHub Pages — This website was created with [Typora](https://typora.io/)*

![](https://raw.githubusercontent.com/marcoalopez/GrainSizeTools/master/FIGURES/footer.webp)