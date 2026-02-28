# Getting started

> [!CAUTION]
> This wiki is incomplete and will be the official documentation for the new version of the script to be released in 2024.

In this section, we will learn

- How to install GrainSizeTools using uv.
- How to use Python through Jupyter Notebook or JupyterLab.
- How to interact with the package and import data.

> [!NOTE]
Although this script does not require any prior knowledge of Python programming language, a general introduction to Python can be found at the following web link: https://marcoalopez.github.io/Python_course/

## Step 1. Install GrainSizeTools

GrainSizeTools uses [uv](https://docs.astral.sh/uv/) for dependency and environment management. It requires Python 3.9 or higher.

**1a. Install uv**

```bash
# macOS / Linux
curl -LsSf https://astral.sh/uv/install.sh | sh

# Windows (PowerShell)
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```

Or via pip: `pip install uv`

**1b. Clone the repository**

```bash
git clone https://github.com/marcoalopez/GrainSizeTools.git
cd GrainSizeTools
```

**1c. Create the environment and install dependencies**

```bash
uv sync --all-groups
```

This creates a `.venv` virtual environment with Python 3.11 and installs all dependencies (numpy, scipy, matplotlib, pandas, pyyaml) along with JupyterLab.

**1d. Launch JupyterLab**

```bash
uv run jupyter lab
```

The GrainSizeTools documentation is written assuming that you will be working using Jupyter Notebooks.

> [!TIP]
> **Using a dedicated application to work with Jupyter Notebooks**
>
> If you prefer to use a dedicated application instead of opening Jupyter Notebooks in your browser, there are several alternatives:
>
> - **JupyterLab desktop**: https://github.com/jupyterlab/jupyterlab-desktop/releases
>
> - **Visual Studio Code** (a.k.a. Vscode):  https://code.visualstudio.com/ (supports Jupyter Notebooks via extensions)

## Step 2. Download GrainSizeTools

The preferred method is cloning via git as shown in Step 1b above. Alternatively, you can download a release zip from:

https://github.com/marcoalopez/GrainSizeTools/releases

Unzip the file and save the GrainSizeTools folder to a location of your choice. Then run `uv sync --all-groups` inside that folder to set up the environment.

## Step 3. Understanding Jupyter Notebooks

To improve the reproducibility of the grain size studies, we suggest working with the Jupyter Notebooks templates provided within the script, especially if you have no previous experience with the Python language. A [Jupyter Notebook](https://jupyter.org/) is a document that can contain executable code, equations, visualisations and narrative text. This is ideal for generating reports and including them as supplementary material in your publications so that any researcher can reproduce your results.

TODO -> Figure showing a Jupyter Notebook  
_An example of a Jupyter Notebook with code, equations (using Latex), visualizations and narrative text._

> [!NOTE]
> Explaining how Jupyter Notebooks work in detail is beyond the scope of this wiki page. The aim here is to teach the minimum for a user the templates that come with the script. In any case, the user is advised to familiarise himself with the use of the notebooks. Fortunately, there are excellent tutorials available explaining how Jupyter Notebooks work, we recommend the following:
>
> - https://www.youtube.com/watch?v=HW29067qVWk This is a video by Corey Schafer that explains in a clear, entertaining and concise way how to install and use a Jupyter Notebook, the usage part starts at about 4:20. For the tutorial, Corey uses the classic Jupyter Notebook application instead of JupyterLab, but it works similarly.
>
> - TODO

### 3.1 The JupyterLab interface

When you first open JupyterLab, you should see something similar to this

![](https://raw.githubusercontent.com/marcoalopez/GrainSizeTools/master/imgs/JupyterLab_interface.png)

with the following elements:

- The **Menu Bar** at the top of the screen, which has familiar drop-down menus like: “File”, “Edit”, “View”, etc.

- The **Workspace Area** currently displaying the **Launcher**. This allows users to access the Console and Terminal or create new Notebooks, Python, Text, or Markdown files.

- The **Collapsible Sidebar** on the left contains a File Browser, along with other elements such as running tabs and kernels information, notebook table of contents, and the Extensions Manager.

- The **Information Bar** located below.

At this point, use the launcher to create your first Jupyter notebook.

#### Jupyter cells

When you create a notebook, you'll find an empty cell at the top. These cells can be used for writing code, text (which is called markdown), or raw content. You can see the type of cell at the top of the notebook tab and change it anytime. When you click inside a cell, it turns white with a blinking cursor, showing it's ready for editing.

![](https://raw.githubusercontent.com/marcoalopez/GrainSizeTools/master/imgs/notebook_topMenu.png)  
_The JupyterLab interface...TODO. More info at_ https://jupyterlab.readthedocs.io/en/latest/user/interface.html#the-jupyterlab-interface

If it's a code cell, you can write Python code inside. When you run the cell by pressing ``Shift+Enter``, the code gets executed, and any results appear just below the cell. This action, pressing ``Shift+Enter``, will also create a new cell just below it.

If it is a text cell instead, you can use [Markdown](https://www.markdownguide.org/cheat-sheet/) to write down what you think you need. When you're ready, press ``Shift+Enter``, and a new cell will be created below it. Repeat this as many times as you need.

If you open an already edited notebook or template, using the collapsible sidebar or ``File > Open from Path``, the cells will already be created, and you can edit them by clicking on the cell you want to edit and executing it when you are finished.



## Step 4: Understanding the package structure and the workflow

GrainSizeTools is a proper Python package located in the ``grain_size_tools/`` directory. It consists of the following submodules:

- ``__init__.py``: Package entry point that exposes `summarize`, `get_filepath`, and `function_list` at the top level and imports all submodules.
- ``GrainSizeTools_script.py``: Contains the top-level functions `summarize()`, `get_filepath()`, and `function_list()`.
- ``averages.py``: Functions for calculating different types of averages and confidence intervals.
- ``plot.py``: Functions for generating distribution plots, q-q plots, area-weighted plots, and normalized plots.
- ``stereology.py``: Functions to approximate true 3D grain size distributions from 2D sectional measurements (Saltykov and two-step methods).
- ``piezometers.py``: Paleopiezometry functions, loading from `piezometric_database.yaml`.
- ``template.py``: Default Matplotlib parameters used by the script to generate plots.

To import the package and access any method, use:

```python
import grain_size_tools as gst

# top-level functions
gst.summarize(data)
gst.get_filepath()

# submodule methods
gst.averages.amean(data)
gst.plot.distribution(data)
gst.stereology.Saltykov(data)
gst.piezometers.calc_diffstress(piezometer, grain_size=40.0)
```

We will see the functionality of all the methods in more detail in the examples shown in this wiki.

#### The DATA folder

In the GrainSizeTools folder, you'll find a subfolder named DATA. This  is where you should store all the grain size data you want to analyze. While it's not strictly necessary, it is a desirable action in the interest of reproducibility.

> [!IMPORTANT]
> The GrainSizeTools script is not intended to deal with microscopic images but to quantify and visualize grain size populations and estimate stresses via paleopiezometers. **It is, therefore, necessary to measure the grain diameters or the cross-sectional of the grains in advance and store them in a txt/CSV/excel file**. For this task, we strongly recommend using the [*ImageJ](http://rsbweb.nih.gov/ij/) application or one of its different flavours (see [here](http://fiji.sc/ImageJ)). ImageJ-type applications are public domain image processing programs widely used in scientific research and run on all operating systems. This wiki includes a short tutorial on how to measure the areas of the grain profiles with ImageJ. The combined use of **ImageJ** and **GrainSizeTools script** is intended to ensure that all data processing steps are done through free and open-source programs/scripts that run under any operating system. If you are dealing with EBSD data instead, we encourage you to use the [MTEX toolbox](https://mtex-toolbox.github.io/) for grain reconstruction.

#### The notebook templates

Inside the GrainSizeTools folder, there are three different notebook templates depending on the type of study you want to perform:

- quantification of grain size distributions (``grainsize_pop_template.ipynb``)
-  approximation of grain size distributions using stereological methods (``stereology_template.ipynb``),
- paleopiezometry (``paleopiezometry_template.ipynb``).

These notebooks contain all the necessary instructions on how to use them through practical examples. The user will only have to change some parameters within the cells to adapt them to their study case and delete everything that is not needed. Once this is done, all the data, figures and the procedures will be contained within a single folder and in a single fully reproducible document that can be exported to other formats (PDF, HTML, etc.) if needed. If you use this workflow, you should ideally copy the entire GrainSizeTool folder (<<1 MB), i.e. the code, the subfolder structure and the notebooks, for each grain size study you conduct.

Once you have everything installed, open JupyterLab or Jupyter Notebooks and in the file manager go to the location where the script and templates are. Open the Jupyter template you want to use and follow the instructions in the template. That's it.

> [!CAUTION]
> In your notebook, you can run cells in any order by selecting them and pressing ``Shift+Enter``. But, when you share your notebook, it's best to run cells in the order they're shown. To do this, you can restart the kernel and then go to the menu bar and click on ``Run > Run All``. This helps prevent unexpected issues if you're using variables between cells and improve reproducibility. Just be cautious when changing the order.

#### Importing data using the Pandas library

TODO