# Vaccination-Data-Viz

This repository is a live document chronicling the development of a dashboard visualization for daily US COVID-19 vaccinations in part of 2021. This project was created for CS573 Data Visualization at Worcester Polytechnic Institute in Fall 2021. For the completed visualization see [Section 5, Final Visualization](#5-final-dashboard). Clicking on any of the images will take you to a VizHub page where you can explore each version of the dashboard or fork it for your own project!

A video walkthrough of the completed dashboard can be found [here](https://youtu.be/iJh7S_nWXp0).

## 1. Data: US Vaccination Data by State/Territory

The data I propose to visualize for this project is [Data on COVID-19 Vaccinations by Our World in Data](https://github.com/owid/covid-19-data/tree/master/public/data/vaccinations). This dataset contains information about the number of COVID-19 vaccinations given per day by US State/Territory as well as information about the total vaccination level of each state, number of doses available, and total dose utilization. I have filtered this dataset to time period 04/01/2021 - 09/04/2021 to reduce the quantity of data to process for this visualization.

## 2. Research Questions

- How has the number of vaccines given changed over time?
- Which state has the highest per capita vaccinate rate for a given period?
- Which state has most efficiently distributed its vaccines for a given period?
- Which state has the fastest vaccination rate for a given period?

## 3. Sketches

To help answer these questions I've conceptualized a visualization dashboard in the following sketch. In the top left I hope to create an interactive map of the US using the `geopath` library. On this map I would like to show the overall vaccination level of that state using doughnut charts. The inner ring of the doughnut chart will indicate the overall vaccination level (which is also shown in text in the center of the doughnut) and the outer ring will show the vaccine utilization for the state. These doughnut charts were inspired by the Apple Fit workout rings and should be understandable by a lay user. The map will also act as filter tool allowing the user to select one or more states to visualize the data of. Below the map I will embed a bar chart visualization of the number of vaccines distributed in the given day. On the right I will provide the user filter controls to select the time range to observe and help the user find and select states for consideration.

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

The first proof of concept visualization I created for this data is a simple scatterplot which shows the daily number of vaccinations given. For this demonstration I have filtered the data to only show the vaccines distributed in the state of Massachusetts. This is because the amount of data for every day in every state is too large to reasonably display on a single plot raw. In the future I would like to aggregate the data over a set of states to show total doses administered in that region.

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

## 4.5 First Dashboard

For this weeks assignment I updated my prototype map of COVID-19 vaccine data to use doughnut charts rather than simple circles. Getting these working was a bit of a challenge, but I think it was worth the effort. Each doughnut consists of two arc elements and an outer circle. The inner arc in blue is the percentage of state residents fully vaccinated, while the outer arc in green represents the percentage of COVID-19 vaccine doses used. To ensure that the charts don't overlap which each other I'm using the d3 force function to move each chart subtly until they no longer overlap. In the center of each chart it a value reading the percentage of residents fully vaccinated. For some reason there is a bug when brushing that causes this number to be newly appended rather than updated. I think this has to do with how I'm filtered the data on the brush update, but I'll have to spend some time to debug this. Any suggestions would be appreciated. Brushing seems to be updating the arcs successfully to match the vaccination level at the end of the selected range.I also plan on adding a legend for reading these charts in the near future.

[![image](/img/map_with_doughnuts.png)](https://vizhub.com/PeterVanNostrand/197316bcd3424f2e98de60ca9985beae)

## 4.6 Debugged Dashboard

For this weeks assignment I worked on ironing out some bugs in my COVID-19 vaccination dashboard. Last week I mentioned how when brushing on this plot there was a set of bugs which caused the text fields and marker components (i.e. the paths and circles that make the doughnuts) to be duplicated when updating as part of brushing the lower plot. This resulted in these elements stacking on top of each other and lagged the page a bit when sequential brushing lead to a buildup of these elements. To fix this I've refactored my map code to draw all the elements for a state in one pass as part of a group, with the elements within the group being updated and redrawn as necessary for brushing. As a result this cross plot interaction is now working as corrected! I also moved the JSON file for each state location to a GitHub gist. One task I investigated heavily but have not solved yet is being able to both brush and handle mouse events for the bar chart. My previous iterations included a tooltip and callout when hovering the histogram bars, but the d3 brush library interferes with this by placing its brush box over these elements. I investigate several solutions which involve dynamically showing/hiding the brush box on mouse down, but haven't found a good solution yet. At the moment I have a toggleable line which switches between the two. Any pointers would appreciated. Pre and post fix images are below.

[![image](/img/map_with_doughnuts_bugged.png)](https://vizhub.com/PeterVanNostrand/197316bcd3424f2e98de60ca9985beae)
[![image](/img/map_with_doughnuts_debugged.png)](https://vizhub.com/PeterVanNostrand/7b144d21b3634dbab08c6721cd402d2f)

## 4.7 Interactive Filtering

Now that brushing the histogram correctly updates the map I worked to allow the reverse direction and have the map update the histogram. To do this I implemented an interaction which allows the user to click on a state or a doughnut chart in the map and switch the histogram to visualize the historical data for vaccinations in that state. This augments the dropdown list to allow the user to more quickly switch between different states. To allow the user to easily locate the state they are looking for in the map I implemented panning and zooming of the map element and added a tooltip to the map which displays the name of the state currently being hovered. The result of these changes can be seen below. The map state selection integrates well with the histogram brushing, allowing both filters to be applied at once or independently. The map selection also updates the selected drop down item for consistency.

[![image](/img/interactive_dashboard_zoomed.png)](https://vizhub.com/PeterVanNostrand/81ecde3d496f4bc6b819602ad836c34c)

## 4.8 Line Chart and User Experience

This week I began work on the line chart of COVID vaccine utilization which can be seen in the upper right corner of the figure below. This line plot shows the percentage of available doses used over time for the selected state. I also took some time to add clear axes labels for the bar chart and an appropriate legend for the map plot. Next week I plan to connect the line chart to state filtering from the map and the date filtering from the bar plot so that it is responsive to all interactive elements. With a bit of polish this will completed the dashboard.

[![image](/img/dashboard_labels.png)](https://vizhub.com/PeterVanNostrand/e366297bd4704e70b3690f402148151b)

## 5. Final Dashboard

Below is the final result of this visualization project, a fully interactive COVID-19 Vaccination dashboard. The dashboard consists of three main components: a national map with doughnut markers, a bar chart of daily doses administered, and a line chart of available dose utilization.

[![image](/img/dashboard_final.png)](https://vizhub.com/PeterVanNostrand/b0e60911c76d4548810a1873b7bb66c2)

The map and bar chart elements feature interactions which change the data to be visualized. Using the dropdown or by clicking on a state or marker in the map the user can select which region they would like to see vaccination data for with the bar and line charts updating accordingly. By brushing the bar chart the user can also select the time period they would like to visualize. After brushing the line chart will adjust to match the brushed period and the map markers will update to show the national state of vaccination as of the last day of the brushed range.
