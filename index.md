---
layout: page
title: Analyzing travel patterns
subtitle: Analyzing plane travels and predicting people's home area based on their flight patterns
cover-img: /assets/img/travels.png
thumbnail-img: /assets/img/thumb.png
share-img: /assets/img/travels.png
order: 1
---

## Introduction

Early 2000s, all the front pages of the news papers are covered by the same story: crimes are commited different airports all around the world: John F. Kennedy International Airport, Sydney Kingsford Smith Airport, Cointrin Geneva Airport.. and the list is long. It seems some criminal dark mind is flying around the world to murder people. 

It has been months now that the first crimes have been comitted and that the tragedies continue. People start being scared to take planes and the airline companies are worrying about their futur. 

October 14st, 2010, all airlines companies gather together to find a solution and stop this macabre trend. They decide to take rational actions and call the most qualified team to elucidate this mystery and finally find who is responsable of those terrible acts. They obviously call the most qualified known team in terms of discovering mysteries: the one and only Sherlock Holmes along with his little sister Enola Holmes, and his best friend Doctor Watson.

Fortunately, the team has taken the ADA course from EPFL and they have a large collection of data about people's movements around the world. Enola Holmes proposes to analyze this data to get insights about plane travels globally and use it to help them catch the murderer and his potential associates before they strike again.

Will this be enough to discover who's the one?

## How Sherlock and his team (we) have proceeded to solve the mystery

### Goals

### Key findings

### Data at our disposal

The data we have at our disposal represents over 10 million check-ins of 1.2 million users of the apps Gowalla and Brightkite. Each check-in represents a time a user has used the app and his or her GPS coordinates were recorded along with a timestamp.

Thanks to these check-ins, we have estimated their homes, by discretizing the world into 25km2 cells, and taking the average of the check-ins inside the cell which contains most of them. We do this in two steps as there can be users who have been to multiple places around the world for holidays or work and we do not want to just take the avergae of all their check-ins. For example, if we just did the gloabl average, a user that lives in Switzerland and has been to a long duration trip in Tahiti for the holidays would have his home estimated somewhere in the ocean around South America. By first taking the cell where the user has been the most, we can then confidently take the average without having too much of a bias. 

The following map shows all estimated homes, with a color scale representing their density. Warm colors represent areas with a high density of homes, whereas cold colors represent areas with a scarse density of homes. 

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/homes_map.html" width="90%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>


By looking at the interactive map above, we can see that the density of our data is far from being uniformly spread around the world. There are some areas of the world such as Central Africa and Central Asia which are almost not covered. We see that most users we are considering are living in North America, Europe and Japan. 

This fact should be considered in the subsequent analysis we do as it is based on data mainly covering specific and one can even argue similar areas of the world. 
Indeed, we might, as a lot of research in the scientific litterature, suffer from a [WEIRD (Western, educated, industrialized, rich and democratic) bias](https://www.apa.org/monitor/2010/05/weird). For example, it is quite probable that potential users that are not covered in our data and do not come from WEIRD countries will have other patterns in their long distance travels.
The results we state should thus be taken with a grain of salt. 

### How to detect plane travels 

We now want to detect plane travels from the check-ins we have. To do so we look at all the check-ins of each user and consider all pairs of consecutive check-ins that are far enough as a plane trip. An important parameter we need to consider is what distance to use as a threshold to determine if two check-ins are distant enough. After trying different values we decide to use 500km as minimum distance. Indeed, we think that it is really likely that a majority of trips longer than this are done by plane. 
To make our analysis even more precise we decide to detect the airports used at the beginning and end of each trip. To do so we take a list of all airports in the world and determine the closest one to the each check-in right before and after the detected long-distance trips. As our list of airports contains very precise data (airports, small airports, aerodromes, heliports, military bases, etc...) we decide to only consider large airports as these are the ones used by most commercial airline companies for their regular trips.  

Below we show the distribution of distances between our estimated airports and the check-ins that happen just before and after a trip.

<iframe src="assets/histogram_distances_airports.html" width="100%" height="500" frameborder="0" style="border:0" allowfullscreen></iframe>

We see that our estimation works well, indeed most check-ins before and after a long distance trip are usually close to airports. However we see there are some outliers after 1000km which are very far from airports for their long-distance trips. After analyzing those we realize they are mainly a few trips to really remote part of the world such as Antarctica and the Arctic and some remote islands without large airports. Because these represent only a small proportion of detected trips we safely ignore them.

After doing this we get the information for 24'243 trips made by plane between 606 airports worldwide.

### Which countries travel the most?

Now that we have detected plane travels, we want to see which countries have residents travelling the most. As this question can be interpreted differently, we will use two metrics, travelled distance and number of flights to be as thorough as possible. To have comparable data, we normalize it temporally to have the flown distance and number of flights over a year and then we divide the results by the amount of users per country, so that our metrics correspond to an average year for an average user of each country.

Below, we show the top 10 countries according to each metric:


**<u>Top 10 countries flying the biggest distance per year</u>**

|N°   |Country          | Travelled distance over a year| GDP per capita ranking (IMF)   |
|:---:|:---------------:|:-----------------------------:|:------------------------------:|
|  1  | Sweden          | 21'537                        | 12                             |
|  2  | Singapor        | 10'064                        | 6                              |
|  3  | Switzerland     | 8'903                         | 2                              |
|  4  | Norway          | 7'971                         | 4                              |
|  5  | Austria         | 6'476                         | 13                             |
|  6  | The Philippines | 6'243                         | 119                            |
|  7  | Belgium         | 6'214                         | 16                             |
|  8  | United States   | 5'549                         | 5                              |
|  9  | China           | 4'931                         | 59                             |
|  10 | Australia       | 4'919                         | 10                             |


 **<u>Top 10 countries flying the most trips per year</u>**

|N°   |Country          | Average number of trips over a year| GDP per capita ranking (IMF)   |
|:---:|:---------------:|:----------------------------------:|:------------------------------:|
|  1  | Sweden          | 10.1                               | 12                             |
|  2  | Norway          | 3.8                                | 4                              |
|  3  | Switzerland     | 3.1                                | 2                              |
|  4  | United States   | 2.6                                | 5                              |
|  5  | Austria         | 2.5                                | 13                             |
|  6  | Belgium         | 2.4                                | 16                             |
|  7  | Singapor        | 2.2                                | 6                              |
|  8  | Germany         | 1.9                                | 15                             |
|  9  | Denmark         | 1.8                                | 7                              |
|  10 | Mexico          | 1.7                                | 71                             |


We notice something interesting when looking at the tables above, most present countries are also in the top 20 of GDP per capita according to the [International Monetary Fund (IMF)](https://en.wikipedia.org/wiki/List_of_countries_by_GDP_(nominal)_per_capita). This illustrates a correlation between the their economic well-being and their capacity to travel by plane. However, there are some outliers, in the first table we see that people living in The Philliines, even though they are ranked low in GDP per capita, are flying a lot of milleage. This is not surprising since their country is made of 7'000 islands in the ocean, so every time they travel they need to cross the waters around them in order to reach any destination. This intrinsic way of travelling will naturally increase the flown distance more rapidely than a landlock country such as Switzerland, even though they travel a bit less frequently (N° 14 in most trips per year). We also see that people living in China, are not in the top travellers by number of flights (N°15  in most trips per year) but tend to fly further,  similarly to people from The Philippines. The average Chinese user is thus flying less frequently but further. For Mexico, we see the opposite trend, they fly more frequently but shorter distances (N° 20 in furtest distance by year). This might be due to the fact that their country is large enough so that they take short domestic flights which are cheaper than long international flights.

### Vizualize the pattern of how people move over the world over time

We now want to see how these two metrics evolve throughout the year. We look at the variation between months of the year in this section.

First, we show the following GIF that depicts all airports and the number of flights connecting them throughout a month. The darker the link between them, the more connected the airports for the given month.

<p align="center">
  <img src="assets/img/animated-2.gif" alt="animated" />
</p>

We can see that there are three big clusters for the global air traffic, North America, Europe and Eastern Asia. The airports in these clusters are strongly connected amongst themselves and the three big clusters are also well connected amongst themselves. This network can thus be considered as one with three big communities that are very dense and that are connected with so called *weak ties* which are significant.  
We also notice some varying trends throughout the year. It seems that there is more traffic in January and during the summer, but to be more certain about this claim we make a more granular analysis.

We first look at the distance travelled by month. The following bar plot shows how an average user travels throughout the year. We should note that this might be a bit biased towards the US as it is the country with the most users.

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/global_distance_travelled.html" width="60%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

We see that there is a significant increase in flown distance between May and October as we saw in the animation above. This might be explained because it is usually a time of the year when  lot of people we have represented in the data (North America and Europe) have summer holidays andthus the time to go on longer trips further. However we do not see the increase of traffic in January on this gloabl trend. To look into more details we analyze the trend per country:

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/distance_travelled.html" width="90%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

Here we see something interesting, there is a divide between countries in the Northern and Southern hemisphere of the globe. People living in Europe and North America tend to have a peak of travels in the summer months (June to September), while users from countries in the Southern hemisphere such as Australia, New Zealand, Brazil, Argentina or South Africa tend to have a peak in the first months of the year. This might be due to the fact that this corresponds to their summer and is when schools are on holidays and families tend to have more time to travel. This can also explain why we are seeing more traffic in the animated gif af air traffic above.

We now do the same analysis for the number of trips. We first show the gloabl trend:

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/global_number_trips_travelled.html" width="60%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

The trend we observe here is very similar to the one for the flown distance, meaning that the periods when user travel further are the same ones as the periods when user travel frequently. To see if we also see a North/South divide we show the data per country:

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/number_trips_travelled.html" width="90%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

As one could expect, we see the same trend appearing for the number of travels. 

### Analysis of the gloabal network of airports

We now would like to see how the airports are connected amongst themselves. The 606 form a graph which is interesting to analyze. First we note that all airports form one connected component, this means that it is possible from any airport to reach any other airport on the world. It is sometimes needed to go through other airports but it is always possible. We do not have a scenario where some subset of airports are fully separated from the rest. We use different metrics to measure how well tight the airports are. We see that the average shortest path between two airports is 1.96. This means that going from any airport A to any airport B on the globe takes on average just below two trips. 

We are now interested to see what are the top 10 most used airports. For this we will use two metrics.  
First we look at the most connected airports, i.e. the airports from which you can reach the most other airports:

 **<u>Top 10 countries flying the most trips per year</u>**

|N°   |Airport (Country)                            | Connected to |
|:---:|:-------------------------------------------:|:------------:|
|  1  | La Guardia (US)                             | 404          |
|  2  | London Heathrow (GB)                        | 379          |
|  3  | San Francisco International (US)            | 361          |
|  4  | Los Angeles International (US)              | 359          |
|  5  | Denver International (US)                   | 343          |
|  6  | Stockholm-Arlanda (SW)                      | 331          |
|  7  | Norman Y. Mineta San Jose International (US)| 328          |
|  8  | Metropolitan Oakland International (US)     | 323          |
|  9  | Austin Bergstrom International (US)         | 312          |
|  10 | Tokyo Haneda International (JP)             | 290          |

We see that each one of the three big clusters we entionned in air traffic (Nort America, Europe and Eastern Asia) has at least one airport that is really well connected to the rest of the network. We note that the is the country with the most connected airports.

Then we look at the betweeness of airports, this measure tells us which airports are used the most often to travel between any two random airports worldwide. Airports in the following list are the most frequently used when travelling between two airports with the least amounts of layovers:

|N°   |Airport (Country)                             | Betweeness score |
|:---:|:--------------------------------------------:|:----------------:|
|  1  | London Heathrow (GB)                         | 0.044            |
|  2  | La Guardia (US)                              | 0.039            |
|  3  | Stockholm-Arlanda (SW)                       | 0.031            |
|  4  | Los Angeles International (US)               | 0.027            |
|  5  | San Francisco International (US)             | 0.025            |
|  6  | Tokyo Haneda International (JP)              | 0.025            |
|  7  | Denver International (US)                    | 0.022            |
|  8  | Norman Y. Mineta San Jose International (US) | 0.022            |
|  9  | Austin Bergstrom International (US)          | 0.016            |
|  10 | Paris-Orly (FR)                              | 0.016            |

In the table above when the airport has a high betweeness score, it is used a lot in routes between other airports. Not surprisingly, we see that most of the airport that have a very high betweeness score are also some of the most connected. We also see that each of the three clusters are represented in this table.


### Analyse which are the top 10 countries to which people travels based on their own country

TODO

<iframe src="assets/top10visited.html" width="100%" height="500" frameborder="0" style="border:0" allowfullscreen></iframe>


### Analyse which are the top 10 countries where live the contacts of the people based on their own country

Now that we have the top 10 countries to which the possible guilties travel based on their own country, we tought to find some informations about where live the contacts of the possible guilties in order to guess who are the partners in crime of the guilty once they will find him.
To do so, we found for each person the country where he lives based on his home. Then, thanks to the contact network we have, we manage to obtained the top 10 countries where live their contacts based on their own country. 

<iframe src="assets/top10friendships.html" width="100%" height="500" frameborder="0" style="border:0" allowfullscreen></iframe>


### Prediction

Here they applied three different classification algorithms for a problem of 3 balanced classes. Since classes are balanced and having false positives or negatives does not impact differently the results, they consider that accuracy is a good measure here of how the algorithms perform.

Predicting at random would yield an accuracy of 33%. Here they obtained an accuracy of 47% which is slightly better than random predictions. This implies that predicting the continent where a user lives is possible using travel patterns over a year, but our data does not mark enough differences between continents. In order to validate if it is possible, they would need more balanced data between continents in the checkins.


<center> <h2>About the team </h2> </center>



| Sherlock Holmes          | Enola Holmes              | Doctor Watson            |
:-------------------------:|:-------------------------:|:-------------------------:
![](/assets/img/apo.png)   |![](/assets/img/maina.png) |![](/assets/img/val.png)  |
| Alexander Apostolov      | Maina Orchampt-Mareschal  | Valentin Garnier         |                   

 

   
