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

To help answer these questions I've conceptualized a visualization dashboard in the following sketch. In the top left I hope to create an interactive map of the US using the geopath library. On this map I would like to show the overall vaccination level of that state using doughnut charts. The inner ring of the doughnut chart will indicate the overall vaccination level (which is also shown in text in the center of the doughnut) and the outer ring will show the vaccine utilization for the state. The map will also act as filter tool allowing the user to select one or more states to visualize the data of. Below the map I will embed a bar chart visualization of the number of vaccines distributed in the given day. On the right I will provide the user filter controls to select the time range to observe and help the user find and select states for consideration.

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

### 4.3 Interactive Bar

The next major iteration of this was to add several interactive features. The picture below shows a revised version which allows the user to select the state to plot from the drop down, animated bar replacement when the selected state changes, and a hover interaction which highlights the selected bar and adds a tooltip. Critically this allows the user access to the data for all states compared to previous versions which were hard coded to filter to one state

[![image](/img/interactive_bar_1.png)](https://vizhub.com/PeterVanNostrand/32dd78ca3b604a989206ed054a43f00f)

[![image](/img/interactive_bar_2.png)](https://vizhub.com/PeterVanNostrand/32dd78ca3b604a989206ed054a43f00f)

### 4.4 Prototype Map

To build towards the map with doughnut charts I started by creating a simple map with circles plot where the radius of each circle is proportional to the fraction of the eligible population who are fully vaccinated as of the latest available data (09/04/2021). I have built this visualization with future filtering in mind and so have written the implementation to take the date to visualize as a parameter. This will make future iterations simpler and make for easier connection to the filter box and bar chart.

[![image](/img/map_with_circles.png)](https://vizhub.com/PeterVanNostrand/54d07b8746334cae8694b1687cc8e204)

## 4.5 Iterated Map

For this weeks assignment I updated my prototype map of COVID-19 vaccine data to use doughnut charts rather than simple circles. Getting these working was a bit of a challenge, but I think it was worth the effort. Each doughnut consists of two arc elements and an outer circle. The inner arc in blue is the percentage of state residents fully vaccinated, while the outer arc in green represents the percentage of COVID-19 vaccine doses used. To ensure that the charts don't overlap which each other I'm using the d3 force function to move each chart subtly until they no longer overlap. In the center of each chart it a value reading the percentage of residents fully vaccinated. For some reason there is a bug when brushing that causes this number to be newly appended rather than updated. I think this has to do with how I'm filtered the data on the brush update, but I'll have to spend some time to debug this. Any suggestions would be appreciated. Brushing seems to be updating the arcs successfully to match the vaccination level at the end of the selected range.I also plan on adding a legend for reading these charts in the near future.

[![image](/img/map_with_doughnuts.png)](https://vizhub.com/PeterVanNostrand/197316bcd3424f2e98de60ca9985beae)
## 5. Schedule of Deliverables

- Improve map visualization 10/27
  - Convert visualization of vaccination percent from map with circles into map with doughnut charts
  - Add second ring to doughnuts to encode the fraction of vaccination doses used
  - Add mouse zooming with scroll wheel
- Add map interaction 11/03
  - On mouse hover highlight that state
  - Display tooltip with detailed data: percent fully vaccinated, percent partially vaccinated, percent doses used, population
- Dashboard prototype 11/10
  - Combine bar chart and map into a single visualization
  - Create line chart of the fraction of doses used
- Advanced filtering 11/17
  - Add filter control box with start date and end date
  - Add toggle for visualizing fully vaccinated vs partially vaccinated
  - These should affect data in all three visualizations
- Cross-visualization interaction 12/01
  - Connect hover filtering from bar chart to map
  - Connect hover filtering from map to bar chart and line chart
- Debugging and cleanup 12/08
