+++
title = "Political Individual Contributions in the Rio Grande Valley"
date = 2020-04-12T17:41:01-05:00
tags = ["money", "politics"]
author = "Frank Ceballos"
categories = ["politics"]
draft = false
summary = "Who is donating money in the Rio Grande Valley?"
+++
{{< image "images/brownsvilleCity.jpg" >}}

## Source of Data and  Data Processing
The data of **contributions from individuals** was retrieved from the Federal Election Commission (FEC) of the
United States-of-America using the [bulk downloads](https://www.fec.gov/data/browse-data/?tab=bulk-data)
feature on 04/12/2020. The FEC using provides the following description for this file:

***The individual contributions file contains each contribution
from an individual to a federal committee. It includes the ID number of the
committee receiving the contribution, the name, city, state, zip code, and
place of business of the contributor along with the date and amount of the contribution.***

To focus on the contributions of individuals from the Rio Grande Valley,
the data was first filtered using the 81 zip codes that were obtained
from [www.usa.com](http://www.usa.com/956-area-code--covered-zip-codes.htm). Then
transactions that contained a negative or a zero value were removed. Finally,
the data was first group by the contributors name, city, occupation, or employer
and then aggregated for the maximum, mean, median, minimum, standard deviation,
sum, count of the transaction amount.

All the processing was done using Python and the following packages: `pyspark`,
`pandas`, and `numpy`. The visualizations was done with Python using: `plotly`.

You are welcome to contact me on [LinkedIn](https://www.linkedin.com/in/frank-ceballos/).

Until next time, take care, and code everyday!

{{< buymeacoffee "" >}}
