# Vaccination-Data-Viz

A visualization of US daily vaccinations in part of 2021. Created for CS573 Data Visualization

## 1. Data: US Vaccination Data by State/Territory

The data I propose to visualize for my project is [Data on COVID-19 Vaccinations by Our World in Data](https://github.com/owid/covid-19-data/tree/master/public/data/vaccinations). This dataset contains information about the number of COVID-19 vaccinations given per day by US State/Territory as well as information about the total vaccination level of each state, number of doses available, and total dose utilization. I have filtered this dataset to time period 04/01/2021 - 09/04/2021 to reduce the quantity of data to process for this visualization.

## 2. Questions

- How has the number of vaccines given changed over time?
- Which state has the highest per capita vaccinate rate for a given period?
- Which state has most efficiently distributed its vaccines for a given period?
- Which state has the fastest vaccination rate for a given period?

## 3. Sketches

To help answer these questions I've conceptualized a visualization dashboard in the following sketch. In the top left I hope to create an interactive map of the US using the geopath library as demonstrated in the `Circles on Map` demo from Professor Kelleher. On this map I would like to show the overall vaccination level of that state using doughnut charts. The inner ring of the doughnut chart will indicate the overall vaccination level (which is also shown in text in the center of the doughnut) and the outer ring will show the vaccine utilization for the state. The map will also act as filter tool allowing the user to select one or more states to visualize the data of. Below the map I will embed a bar chart visualization of the number of vaccines distributed in the given day. On the right I will provide the user filter controls to select the time range to observe and help the user find and select states for consideration.

![image](/img/dashboard_sketch_v2.png)

### Interaction

This dashboard will have several interactive components to allow the user to select data for visualization

1. Selecting a state in the filter section will show only data for that state on all three plots
2. Hovering a state will highlight that state and show daily doses and percent doses used for only that state. This will allow the user to hover around the map and take quick peeks into the data for each state. It will also display a tooltip with full number for the fully vaccinated, partially vaccinated, the percent doses used and population numbers.
3. Hovering a bar in the daily doses administered bar chart will filter the above map to show the state of every state on that date allowing the user to scrub through time and observe how the doughnut charts change. It will also show a tooltip will the date and exact number of doses administered.
4. Using the start date and end date filters will filter the data used in the bar chart and line chart to only the dates in that range and update the map to show the national state of vaccinations on the end date
5. Lastly the `vaccinations` toggle in the filter box allows the user to switch between plotting full and partial vaccination level on the map.

## 4. Prototypes

### 4.1 Scatterplot

So far I’ve created two proof of concept visualization of this data. The first is a simple scatterplot which shows the daily number of vaccinations given. For this demonstration I have filtered the data to only show the vaccines distributed in the state of Massachusetts. This is because the amount of data for every day in every state is too large to reasonably display on a single plot raw. In the future I would like to aggregate the data over a set of states to show total doses administered in that region.

[![image](/img/wk3_scatterplot.png)](https://vizhub.com/PeterVanNostrand/56fccd1bb8924f7e93071a64a7cfd67d)

### 4.2 Basic Bar

As I felt that this scatterplot was not displaying the data as clearly as possible I elected to reformat the scatterplot into a bar chart, with each bar representing the number of doses given on each day. The result is shown below.

[![image](/img/wk4_barchart.png)](https://vizhub.com/PeterVanNostrand/433774e3a12845a48a3bb98b683ba708)

Here we can see that the use of bars significantly clarifies the visualization as area is one of the most significant channels for human interpretation. This also prevent the overlap of mark which was present in the previous figure and allows us to see that there are a few missing days of data around June 1st and July 4th. We may need to impute this data in the future for operations such as computing a moving average.
## 5. Open Questions

I believe the path to creating the map visualization is rather straightforward, the open question is the multi-state aggregation and COVID data. In order to perform these operations I need to learn how to better manipulate the data array that the CSV loader returns. In particular I need to determine how to perform some database like operations such as GROUP BY for the aggregation and JOIN for adding the COVID data.
