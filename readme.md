## Automatic data extraction & export of categorized data from JHU COVID-19 Dashboard

### Introduction
[Johns-Hopkins University](https://github.com/CSSEGISandData/COVID-19) has put in a tremendous effort in gathering all the COVID-19 data from reliable sources and bringing to us in the form of an ArcGIS Dashboard. They have also open-sourced all the feature layers to public, for using in their own dashboards or analysis.

### Challenge
Even though most of the data is open sourced, for people who want get all numbers, it would be tedious to look for the service layers, add/connect them in their ArcGIS Online Organization, download the data set and aggregate them county-wise.Hence, this is an attempt to automatically extract the numbers (Active, Confirmed, Deaths) both Countrywise,US-Countywise and finally categorize,aggregate,style and export them to a neatly formatted spreadsheet from their dashboard. This is performed by leveraging Pandas and ArcGIS API for Python.

### Data Source
Here is the first hosted feature service that was open-sourced by John Hopkins

[Covid19-WorldWide-Cases](https://www.arcgis.com/home/item.html?id=c0b356e20b30490c8b8b4c7bb9554e7c)

This feature service has 3 feature layers in it namely

* [Deaths](https://services1.arcgis.com/0MSEUqKaxRlEPj5g/arcgis/rest/services/ncov_cases/FeatureServer/0)
* [Cases](https://services1.arcgis.com/0MSEUqKaxRlEPj5g/arcgis/rest/services/ncov_cases/FeatureServer/1)
* [Cases_Country](https://services1.arcgis.com/0MSEUqKaxRlEPj5g/arcgis/rest/services/ncov_cases/FeatureServer/2)

Upon a huge demand, JHU also came up with US-Countywise feature service on March 23rd, 2020.

[COVID19-USCountyCases](https://www.arcgis.com/home/item.html?id=628578697fb24d8ea4c32fa0c5ae1843)


### Extraction Workflow

* Get the feature services published in ArcGIS Online using their Item IDs for both worldwide & US.
* Get the Cases_Country feature layer in the World Wide dataset and Cases feature layer in the US Countywise dataset.
* Convert both the feature layers into spatial data frames for data extraction.

#### Countrywide cases
* We extract only the required fields from the dataset namely
    * Country_Region
    * Deaths
    * Confirmed
    * Recovered
    * Active
* Now apply the styling using Pandas Styling API. 
* Here Bar Style and Gradient Styles were used for visualizing the data.
* After visualization, export the styled dataframe into a neatly formatted spread sheet.

#### US-CountyWise cases
* Since JHU stated that they havent found a reliable source for Active and Recovered cases at county level, this was not included. But this may change over time once reliable data is found.

* In addition, as mentioned by JHU, all the cases for which exact county location is not known but a state is known are assigned a Lat Long of 0,0 and named as **Unassigned** in the county column.

* From the USCountywise spatial dataframe we extract only **State**,**County**,**Confirmed**,**Deaths**.

* Even, through the extraction is straight-forward, we wanted to have the data grouped by state and county for which pandas pivotting is performed.

* Once that is completed, styling is applied on the dataframe to quickly identify counties where deaths occurred, State-wide death totals and finally US-wide totals.

* After we visualize the dataframe, we can export it to a neatly formatted spreadsheet like above.

#### Discrepancies:
You might observe a small difference in Confirmed and Active numbers for the US when we compare the US-Countywise cases dataset and the Country wide dataset. This is due to the time of update of the feature layers. The country-wide feature layer update is ahead of the US-Countywise update.

#### Notebook Viewer:
Due to [limitation](https://help.github.com/en/github/managing-files-in-a-repository/working-with-jupyter-notebook-files-on-github) of rendering interactive Javascript plots,stylings in your notebook inside the github repository, I have included the notebook along with dataframe stylings in the nbviewer **[here](https://nbviewer.jupyter.org/github/rvg296/Covid19-JHUDashboard-Data-Extraction/blob/master/Covid19-Numbers-Extraction.ipynb#)**


#### Sample Screenshots:

*The numbers quoted below may vary depending on the time you run the script*
*In Jupyter Notebook*
![](/images/Country-BarStyle.PNG "CountryWiseCases-BarStyle")
![](/images/Country-GradientStyle.PNG "CountryWiseCases-GradientStyle")
![](/images/US-County-Part1.PNG "CountryWiseCases-GradientStyle")
![](/images/US-County-Part2.PNG "CountryWiseCases-GradientStyle")
![](/images/US-County-Part3.PNG "CountryWiseCases-GradientStyle")


*In Excel*
![](/images/Country-Gradient-Excel.PNG "CountryWiseCases-GradientStyle-Excel")
![](/images/US-County-Excel.PNG "CountryWiseCases-GradientStyle-Excel")


#### References:
* [ArcGIS Python API Spatial Dataframes](https://developers.arcgis.com/python/guide/introduction-to-the-spatial-dataframe/)
* [Pandas-Styling](https://pandas.pydata.org/pandas-docs/stable/user_guide/style.html)
* [Pandas-Pivoting](https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.pivot.html)
* [Color-Formatting](https://www.rapidtables.com/web/color/html-color-codes.html)
* [JHU-Git-Hub](https://github.com/CSSEGISandData/COVID-19)

