---
layout: post
title: BarcelonaAccidents 2010-17
---

Yes, I let myself go and get sucked in by my origins. I just had to work on a database that is from my hometown (Barcelona) as a modest way to show my support to the fellow Catalans in these sad days ('Stay strong and together')

I obtained all the information thanks to openData Barcelona. All this information is easily available in [BarcelonaOpenData](http://opendata-ajuntament.barcelona.cat/en/)

![nofrauds](/images/BCN01.jpg){:height="300px" width="400px"}


## Brief description of Barcelona Accidents.

  1. Dataset contains all the accidents that the local police attended from 2010 to 2017.
  2. The information is gathered is spread in different CSV:
    - Accidents.
    This one has all information about the accident including time, location, number of victims and deceased and he number of vehicles. It also includes a column in which it is determined
    if it was a pedestrian responsible for the event.
    - Causes of the accident.
    This one adds 'initial_street' and 'mediate_cause'. While the meaning of the first added column is unclear and will need further analysis, the second one -'mediate_cause' - indicates the reason - if any - behind the accident.
    - People Involved.
    This one incorporates information about the people involved like age, gender, degree of injuries, who is responsible etc
    - Types.
    This CSV adds a filed named 'accidents_details' and it describes the reason behind the accident.
    - Vehicles involved.
    This file adds a bunch of details about the vehicle involved from a description of it up to its model and brand name. It also includes info about the experience of the driver.

  3. It has been a hard work to combine all different CSV due to different encodings and column's
  names.
  4. Once I have all data combined, I will analyze all different files to try to make some prediction at the end using the total file.
  5. The link in github for a more detailed and technical explanation is available here:[BarcelonaAccidents](https://github.com/AlexChicote/BarcelonaAccidents). I will keep on updating them as I move forward.


## Non-technical description of BarcelonaAccidents.

## Gathering the data.

First I combine all files for accidents, causes, people, types and vehicles from 2010 to 2017. Once done, I merged them all together using the number of case file as a common index.

```
## ACCIDENTS.
accidents = combinant_csv('accidents', 2010, 2017)

## CAUSES.
causes = combinant_csv('causes', 2010, 2017)
total = merging_df(causes, accidents)

##PEOPLE.
people = combinant_csv('persones', 2010, 2017)
total = merging_df(people, total)

##TYPES.
types = combinant_csv('tipus', 2010, 2017)
total = merging_df(types, total)

##VEHICLES. (147473, 20). ((210297, 34)
vehicles = combinant_csv('vehicles', 2010, 2017)
total = merging_df(vehicles, total)

print('Acabat')
```

Secondly, I analyze every different group of data to try to figure out where the information is hidden. I start with the file accidents.

## ACCIDENTS

In this file, we have all information concerning the accidents (reminder: only those in which Barcelona local police is involved) like location, time, number of vehicles, number of people implied and the degree of its injuries.

As a starter, I will consider mainly the number of accidents and the number of people that die in the event


* Year.

![accidents_deads_per_year](/images/accidents_deads_per_year.png){:height="400px" width="800px"}

The trend is clear in both cases; while the accidents have been increasing during last years, the number of people that lost their life in those have clearly diminished.

* Month.

![accidents_deads_per_month2](/images/accidents_deads_per_month2.png){:height="400px" width="800px"}

In the graph we can see on yellow the amount of accidents per month, on red the amount of deads per month and, finally, the average of deads per accident shown in a blue line.
The months (in my modest opinion) to point out is September and August. September is the month was the biggest amount of dead people what surprised unless explained by being the month right after August which has the biggest average the deads per accidents that might be associated to the  recreational factor.

* Week day.

![accidents_deads_per_weekday2](/images/accidents_deads_per_weekday2.png){:height="450px" width="800px"}

Using the same colors and references than before, we can see how Saturday is the day where the deads per accidents get the higher rate. It is also important to point out Monday as the day with more accidents and deads. So I had to check the weekend and this is what I did.

* Weekend.

![accidents_deads_per_weekday2](/images/accidents_deads_per_weekday2.png){:height="450px" width="800px"}

In this case, we can see how the line representing the number of deads, peaks up on Friday night and not as much during Saturday night. This is important to point out, I believe, beacuse this explains why Saturday stands out in number of deads. It is obvious that what we call Friday night is way more active when it comes to accidents and deads that at least myself expected.

* Hourly.

![accidents_deads_per_hour](/images/accidents_deads_per_hour.png){:height="450px" width="800px"}

This new chart is confirming all what we have been seeing so far. It is that the most risky time to drive in Barcelona is during the early morning. The first chart, similar to the one we had for the weekend, shows that correlation broken between accidents and deads. In the second one, I try to make that visualization clearer.

![average_per_hour](/images/average_per_hour.png){:height="450px" width="800px"}

* District.

I could not let it go by without having and analysis about the neighborhoods or districts. It is important to say that traffic and accidents have a different geographic relation. The size of every district, its structure (a lot of pedestrian streets where traffic is not allowed) or its position (a district that is located in middles of important streets and a lot of traffic is guided through it) might be introduced to the analysis in order to have a real deep understanding. I do not have the data so I will just show the chart without adding more info.

![deads_accidents_per_district](/images/deads_accidents_per_district.png){:height="450px" width="800px"}

* Accidents with bigger number of casualties.

####need to export tables
