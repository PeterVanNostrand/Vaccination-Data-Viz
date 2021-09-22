# Vaccination-Data-Viz

A visualization of US daily vaccinations in part of 2021. Created for CS573 Data Visualization

## Data: US Vaccination Data by State/Territory

The data I propose to visualize for my project is [Data on COVID-19 Vaccinations by Our World in Data](https://github.com/owid/covid-19-data/tree/master/public/data/vaccinations). This dataset contains information about the number of COVID-19 vaccinations given per day by US State/Territory as well as information about the total vaccination level of each state, number of doses available, and total dose utilization. I have filtered this dataset to time period 04/01/2021 - 09/04/2021 to reduce the quantity of data to process for this visualization.

## Prototypes

So far I’ve created two proof of concept visualization of this data. The first is a simple scatterplot which shows the daily number of vaccinations given. For this demonstration I have filtered the data to only show the vaccines distributed in the state of Massachusetts. This is because the amount of data for every day in every state is too large to reasonably display on a single plot raw. In the future I would like to aggregate the data over a set of states to show total doses administered in that region.

[![image](/img/wk3_scatterplot.png)](https://vizhub.com/PeterVanNostrand/56fccd1bb8924f7e93071a64a7cfd67d)

As I felt that this scatterplot was not displaying the data as clearly as possible I elected to reformat the scatterplot into a bar chart, with each bar representing the number of doses given on each day. The result is shown below.

[![image](/img/wk4_barchart.png)](https://vizhub.com/PeterVanNostrand/433774e3a12845a48a3bb98b683ba708)

Here we can see that the use of bars significantly clarifies the visualization as area is one of the most significant channels for human interpretation. This also prevent the overlap of mark which was present in the previous figure and allows us to see that there are a few missing days of data around June 1st and July 4th. We may need to impute this data in the future for operations such as computing a moving average.

## Questions

- How has the number of vaccines given changed over time?
- Which state has the highest per capita vaccinate rate for a given period?
- Which state has most efficiently distributed its vaccines for a given period?
- Which state has the fastest vaccination rate for a given period?

## Sketches

To help answer these questions I've conceptualized a visualization dashboard in the following sketch. In the top left I hope to create an interactive map of the US using the geopath library as demonstrated in the `Circles on Map` demo from Professor Kelleher. A source I have identified for the US shape data is the [US Cartographic Boundary Files](https://www.census.gov/geographies/mapping-files/time-series/geo/carto-boundary-file.html) which provides shape files for several options for state and municipal breakdowns of the country. I intend to use [MapShaper](https://mapshaper.org/) to convert and simplify this data. On this map I would like to show the overall vaccination level of that state using doughnut charts. The inner ring of the doughnut chart will indicate the overall vaccination level (which is also shown in text in the center of the doughnut) and the outer ring will show the vaccine utilization for the state. The map will also act as filter tool allowing the user to select one or more states to visualize the data of.

![image](/img/vaccination_viz_sketch.png)

Below the map I will embed a the bar chart visualization from above and add a rolling average line to help smooth out daily fluctuations. On the right I will provide the user filter controls to select the time range to observe and help the user find and select states for consideration.

If possible I would also like to merge in data on COVID cases such that the user could analyze the connection between the number of vaccines given in a state and the number of COVID cases. If I find a suitable source for daily COVID number it would be possible to merge this into the existing dataset. I would then visualize the number of COVID cases in each state using a fill color for the state, and add a line chart of COVID cases to the bar chart below.
## Open Questions

I believe the path to creating the map visualization is rather straightforward, the open question is the multi-state aggregation and COVID data. In order to perform these operations I need to learn how to better manipulate the data array that the CSV loader returns. In particular I need to determine how to perform some database like operations such as GROUP BY for the aggregation and JOIN for adding the COVID data.
