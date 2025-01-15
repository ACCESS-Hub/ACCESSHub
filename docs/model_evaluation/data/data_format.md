# Data Formats and Standards

<!-- For this content, I have used a lot of text from this website: https://pro.arcgis.com/en/pro-app/latest/help/data/multidimensional/fundamentals-of-netcdf-data-storage.htm -->

Model evaluation often requires comparison across different models, such as for the [Coupled Model Intercomparison Project (CMIP)](https://wcrp-cmip.org). However, comparing output from different models can be tricky due to the multiple data formats and standards used across models. This is why ACCESS-NRI supports and encourages the use of common, community-supported data formats and variables.

## Data Standards
Data standards are agreed-upon guidelines for the "representation, format, definition, structuring, tagging, transmission, manipulation, use, and management" of datasets (definition from [Geoscience Australia](https://www.ga.gov.au/data-pubs/datastandards)). Abiding by these standardized guidelines allow for, among other things, easier sharing and combining of data, as well as the ability to better understand which quantities can be compared across datasets - very important for model evaluation.

An example data standard in climate models is the use of [Climate and Forecast metadata conventions (CF conventions)](http://cfconventions.org). These are designed to promote the processing and sharing of _NetCDF_ files (described in more detail below). The conventions specify metadata that provide a definitive description of what the data in each variable represents, and the spatial and temporal properties of the data. 

Metadata is information about the data, which can include variable names, dimension names, units, grid information and many others. Standardized metadata can also be more easily made machine readable, allowing software packages to interpret, for example, variable names automatically and making data analysis more efficient and less error prone. The machine readability of standardized formats thus facilitates building software applications with powerful extraction, regridding and display capabilities.

Currently, many models do not abide by the CF conventions by default. However, there is a software library called [CMOR (Climate Model Output Rewriter)](https://cmor.llnl.gov) that translates native climate model output into output that complies with the CF conventions. The process of CMORizing is specifically designed for model intercomparison projects, like CMIP.

## Network Common Data Format (NetCDF)

Numerous organisations and scientific groups worldwide have adopted a file format called [_NetCDF_](https://www.unidata.ucar.edu/software/netcdf/) as a standard way to store multidimensional scientific data.

<i>NetCDF</i>, which has the file extension <i>*.nc</i>, is a self-describing, machine-independent data format of array-oriented scientific data.

<ul>
<li><b>Self-describing</b>
    <br>
    <i>*.nc</i> files include not only the data, but also a header with metadata that describes the data layout.

<li><b>Machine-independent</b>
    <br>
    <i>*.nc</i> files can be accessed by computers with different ways of storing integers, characters and floating-point numbers.

<li><b>Array-oriented</b>
    <br>
    <i>*.nc</i> data typically spans multiple dimensions with the same lengths (e.g., latitude, longitude and time) and variables (e.g., temperature and humidity), which are stored in arrays.
    <br>
    <br>
    <div style="text-align: center;">
        <img src="../../../assets/model_evaluation/xarray2.png" alt="Schematic of a NetCDF file with data (temperature and pressure as variables stored over the dimensions latitude, longitude, and time) and metadata" title="xarray https://xarray.dev/" width="75%"/>
    </div>
</ul>

Data in a *NetCDF* file is stored in the form of arrays, where each *NetCDF* dimension has a name and a length. NetCDF variables and coordinates can also have a different number of dimensions.

For example, surface temperature variation over time at a fixed location would be stored as a one-dimensional array (with dimension *time*), whereas surface temperature that varies over a region at a fixed point in time would be stored as a two-dimensional array (with dimensions *longitude, latitude*). An example of three-dimensional (3D) data would be surface temperature varying with time over a region (with dimensions *longitude, latitude, time*), and four-dimensional (4D) data would be temperature varying with time over a region with varying altitude (with dimensions *longitude, latitude, altitude, time*).

## Loading NetCDF files

There are many ways of reading files, though a common way is via the Python package *xarray*. 
<br>
For more information, refer to a <a href="https://docs.xarray.dev/en/stable/getting-started-guide/quick-overview.html" target="_blank">quick overview of xarray</a> and <a href="https://tutorial.xarray.dev/intro.html" target="_blank">xarray tutorials</a>.

*xarray* is a python package avaliable through the conda environment on NCI. 
<br>
Hence, you can either use it directly (as shown below) or through the dataset capabilities of the [ACCESS-NRI Model Intake Catalog Tool](/model_evaluation/data/model_catalogs).

```
import xarray as xr
dataset = xr.open_dataset("example.nc")
dataset
```

<div style="text-align: center;">
     <img src="../../../assets/model_evaluation/netcdf_example.jpg" alt="Example of an actual NetCDF file with data (precipitation/rainfall over the dimensions latitude, longitude, and time) and metadata." title="Picture from https://pro.arcgis.com/en/pro-app/latest/help/data/multidimensional/fundamentals-of-netcdf-data-storage.htm" width="60%"/>
 </div>

## Other Data formats

NetCDF has been described in detail here as it is the most common format for climate data and then for comparison and optimizing evaluation workflows all data would be in the same format. [Observational data](/model_evaluation/data/observations) can come from different institutions and measured with various instruments. These institutions can manage their data for users other than climate researchers, therefore the data can come in other formats including plain text formats. This data can be [_CMORised_](#data-standards), for evaluation frameworks. Reach out on the [Hive Forum](https://forum.access-hive.org.au) for assistance and suggestions of any datasets that may be missing or could be useful.


<h6>References</h6>
<ul class="references">
    <li>
        <a href = "https://pro.arcgis.com/en/pro-app/latest/help/data/multidimensional/fundamentals-of-netcdf-data-storage.htm" target="_blank">https://pro.arcgis.com/en/pro-app/latest/help/data/multidimensional/fundamentals-of-netcdf-data-storage.htm</a>
    </li>