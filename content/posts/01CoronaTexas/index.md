+++
title = "Coronavirus in Texas: A County Level Map"
description = "Something should go in here."
author = "Frank Ceballos"
date = 2020-04-04T18:39:29-05:00
tags = ["coronavirus", "Texas"]
categories = [""]
draft = false
+++


The first case of coronavirus in Texas was reported on February 12, and according
to [data from The New York Times, based on reports from state and local health agencies](https://github.com/nytimes/covid-19-data)
as of May 06 there have been a total of 34,325 cases reported and 957 deaths.
<!--more-->

{{< 01map "images/texas_counties.html" >}}


Note: The map shows the coronavirus cases for each county. Counties are colored
based on the number of confirm cases. Counties colored in
gray have not reported any cases yet. Hover over the map to
see specific county details.
Sources: [New York Times](https://github.com/nytimes/covid-19-data)

{{< 02bar "images/02BarPlot.html" >}}

{{< 02bar "images/03BarPlot.html" >}}

The number of cases and deaths in Texas continue to increase but at lower rate.
It's important that you wash your hands
frequently, maintain social distance, and practice respiratory hygiene in order
to slow down or maintain the current rate the virus is the spreading.

Next, we show the counties with more than 10 cases. By inspecting the number of
cases at the county level, we can compare how the virus is affecting different
counties.
{{< 01bar "images/01BarPlot.html" >}}

It's also interesting to investigate how the virus has spread with time at the
county level. To show that, we create heatmaps (see Figures below) to keep track on how
the number of cases and deaths have changed with time. Days where no cases or
deaths were reported are shown in gray. Hover the mouse over the heatmap to learn specific
details about each county.

{{< 04bar_heat_tex1 "images/04CasesHeatMap.html" >}}
{{< 04bar_heat_tex2 "images/05DeathsHeatMap.html" >}}

**When is the information updated?**
The information on this page is updated every morning.

**Definitions**:
* A **confirmed case** is defined as a patient that has tested positive for coronavirus.
Only cases that have been reported by federal, state, territorial or local government
agencies are considered.

{{< buymeacoffee "" >}}
