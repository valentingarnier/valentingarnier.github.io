---
layout: page
title: Analyzing travel patterns
subtitle: Analyzing long distance plane travel and predicting user home area based on their long distance travels
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

## How Sherlock and his team (we) has proceeded to solve the mystery

### Background
#### Visualize how people are distributed over the world

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

#### How to detect plane travels 

We now want to detect plane travels from the check-ins we have. To do so we look at all the check-ins of each user and consider all pairs of consecutive check-ins that are far enough as a plane trip. An important parameter we need to consider is what distance to use as a threshold to determine if two check-ins are distant enough. After trying different values we decide to use 500km as minimum distance. Indeed, we think that it is really likely that a majority of trips longer than this are done by plane. 
To make our analysis even more precise we decide to detect the airports used at the beginning and end of each trip. To do so we take a list of all airports in the world and determine the closest one to the each check-in right before and after the detected long-distance trips. As our list of airports contains very precise data (airports, small airports, aerodromes, heliports, military bases, etc...) we decide to only consider large airports as these are the ones used by most commercial airline companies for their regular trips.  

Below we show the distribution of distances between our estimated airports and the check-ins that happen just before and after a trip.

<iframe src="assets/histogram_distances_airports.html" width="100%" height="500" frameborder="0" style="border:0" allowfullscreen></iframe>

We see that our estimation works well, indeed most check-ins before and after a long distance trip are usually close to airports. However we see there are some outliers after 1000km which are very far from airports for their long-distance trips. After analyzing those we realize they are mainly a few trips to really remote part of the world such as Antarctica and the Arctic and some remote islands without large airports. Because these represent only a small proportion of detected trips we safely ignore them.

#### Which countries travel the most?

Now that we have detected plane travels, we want to see which countries have residents travelling the most. As this question can be interpreted differently, we will use two metrics, travelled distance and number of flights to be as thorough as possible. To have comparable data, we normalize it temporally to have the flown distance and number of flights over a year and then we divide the results by the amount of users per country, so that our metrics correspond to an average year for an average user of each country.

Below, we show the top 10 countries according to each metric:

 Top 10 countries flying the biggest distance per year
:---------------------------------------------------------------------------------------:|
|N°   |Country          | Travelled distance over a year| GDP per capital ranking (IMF)  |
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

 Top 10 countries flying the most trips per year
:---------------------------------------------------------------------------------------:|
|N°   |Country          | Average number of trips over a year| GDP per capital ranking (IMF)  |
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


#### Vizualize the pattern of how people move over the world

Still using the check-ins of the possible guilties, we only kept pairs of check-ins which represents a travel by plane. To do so, firsty we only kept the one which are less than 1000km from an airport. Then we had to order chronologically all the check-in by person. Finally we kept all the pair of a check-in with the following one only if the distance between their respective closest airports was greater of 500km: we assumed that this threshold is large enough to consider that the person took the plane between them.

In the following gif, we showed the air trafic according to the different months of the year. We can see that there are severaly big nodes which are mainly in the east North American coast, and in the center of Europe.

<p align="center">
  <img src="assets/img/animated-2.gif" alt="animated" />
</p>

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/global_distance_travelled.html" width="60%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/distance_travelled.html" width="90%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/global_number_trips_travelled.html" width="60%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>

<p align="center">
  <iframe style="margin:auto;display:block;" src="assets/number_trips_travelled.html" width="90%" height="600" frameborder="0" style="border:0" allowfullscreen></iframe>
</p>
Ajouter la courbe travel / mois (total et par country) --> que doit faire Apo + description

At this point, we have a good background of where could live the real guilties and we know to what looks like the air traffic during those terrible months.

### Analysis
#### Analyse which are the top 10 countries to which people travels based on their own country

TODO

<iframe src="assets/top10visited.html" width="100%" height="500" frameborder="0" style="border:0" allowfullscreen></iframe>


#### Analyse which are the top 10 countries where live the contacts of the people based on their own country

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

 

   
