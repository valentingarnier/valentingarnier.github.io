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
Early 2000s, fews crimes were commited inside airports. To elucidate this mystery and finally find who is responsable of those terrible acts, the mondial government called the most qualified team: the one and only Sherlock Holmes along with his little sister Enola Holmes, and his best friend the Doctor Watson.

To solve it, they dispose of the homes, the travels and the contacts of all the people which are considerate as "possible guilties".

Will this be enough to discover who's the one?


## How Sherlock and his team has proceeded to solve the mytery

### Background
#### Visualize how the possible guilties are distributed over the world : their homes

Ajouter la carte des homes

htmltools::includeHTML("/assets/homes_map.html")


#### Vizualize the pattern of how do the possible guilties move over the world: their travels

![Alt Text](assets/img/animated-2.gif)

Ajouter la courbe travel / mois (total et par country) --> que doit faire Apo


### Analysis
#### Analyse which are the top 10 countries to which the possible guilties travel based on their own country

Ajouter top10countries

htmltools::includeHTML("/assets/top10visited.html")

#### Analyse which are the top 10 countries where live the contacts of the possible guilties based on their own country

Ajouter top10friendships

{{ $r := resources.Get (printf "plotly/%s.html" ($.Get 0)) }}

{{ $r.Content | safeHTML }}

{{< plotly /assets/top10friends >}}

### Prediction

... notre modele est mauvais



