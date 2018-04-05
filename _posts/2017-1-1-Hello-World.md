---
layout: post
title: BarcelonaAccidents 2010-17
---

Yes, I let myself go and get sucked in by my origins. I just had to work on a database that is from my hometown (Barcelona) as a modest way to show my support to the fellow Catalans in these sad days ('Stay strong and together')

I obtained all the information thanks to openData Barcelona. All this information is easily available in [BarcelonaOpenData](http://opendata-ajuntament.barcelona.cat/en/)

![nofrauds](/images/BCN01.jpg){:height="300px" width="600px"}


## Brief description of Barcelona Accidents.

  1. Dataset contains all the accidents that the local police attended from 2010 to 2017.
  2. The information is gathered and spread in different CSV:
    - Accidents.
    This one has all information about the accident including time, location, number of victims and deceased and he number of vehicles. It also includes a column in which it is determined if it was a pedestrian responsible for the event.
    - Causes of the accident.
    This one adds 'initial_street' and 'mediate_cause'. While the meaning of the first added column is unclear and will need further analysis, the second one -'mediate_cause' - indicates the reason - if any - behind the accident.
    - People Involved.
    This one incorporates information about the people involved like age, gender, the degree of injuries, who is responsible etc
    - Types.
    This CSV adds a file named 'accidents_details' and it describes the reason behind the accident.
    - Vehicles involved.
    This file adds a bunch of details about the vehicle involved from a description of it up to its model and brand name. It also includes info about the experience of the driver.

  3. It has been a hard work to combine all different CSV due to different encodings and column's names.
  4. Once I have all data combined, I will analyze all different files to try to make some prediction at the end using the total file.
  5. The link on GitHub for a more detailed and technical explanation is available here:[BarcelonaAccidents](https://github.com/AlexChicote/BarcelonaAccidents). I will keep on updating them as I move forward.


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

![accidents_deaths_per_year](/images/accidents_deaths_per_year.png){:height="400px" width="800px"}

The trend is clear in both cases; while the accidents have been increasing during last years, the number of people that lost their lives in those have clearly diminished.

* Month.

![accidents_deaths_per_month2](/images/accidents_deaths_per_month2.png){:height="400px" width="800px"}

In the graph, we can see on yellow the amount of accidents per month, on red the number of deaths per month and, finally, the average of deaths per accident shown in a blue line.
The months (in my modest opinion) that deserve to be pointed out are the summer ones but essentially  September and August. September is the month with the biggest amount of dead people It actually surprised me and the only explanation I find is that it is the final month of the summer right after August which has the biggest average the deaths per accidents. It all makes me consider the recreational factor of the traffic trips as the main reason of their high number of accidents and sadly deaths.

* Weekday.

![accidents_deaths_per_weekday2](/images/accidents_deaths_per_weekday2.png){:height="450px" width="800px"}

Using the same colors and references than before, we can see how Saturday is the day where the deaths per accidents get the higher rate. It is also important to point out Monday as the day with more accidents and deaths. So I will next check the weekend and this is what I found out.

* Weekend.

![accidents_deaths_per_weekend2](/images/accidents_deaths_per_weekend2.png){:height="450px" width="800px"}

In this case, we can see how the line representing the number of deaths, peaks up on Friday night and not as much during Saturday night. This is important to point out because this explains why Saturday stands out in the number of deaths. Saturday includes the late hours of the Friday night and early hours of Saturday night. But if we understand Friday night as the hours from Friday 10pm to Saturday morning 5am, Friday should stand out as the most dangerous hours of the week.

* Hourly.

![accidents_deaths_per_hour](/images/accidents_deaths_per_hour.png){:height="450px" width="800px"}

This new chart is confirming all that we have been seeing so far. It is that the riskiest time to drive in Barcelona is during the early morning mainly during the weekend. The first chart, similar to the one we had for the weekend, shows that correlation broken between accidents and deaths. In the second one, I try to make that visualization clearer.

![average_per_hour](/images/average_per_hour.png){:height="450px" width="800px"}

* District.

I could not let it go by without having and analysis about the neighborhoods or districts. It is important to say that traffic and accidents have a different geographic relation. The size of every district, its structure (a lot of pedestrian streets where traffic is not allowed) or its location (a district that is placed in the middle of important streets and a lot of traffic is funneled through it) might be introduced to the analysis in order to have a real deep understanding.

What I realized about Barcelona is that the Eixample district is the one that has a lot of the traffic and therefore the accidents and the number of deaths but if you analyze the ratio of the number of deaths per accident it is really low and Les Corts, where there is not a lot of traffic, gets the winning position.

![ratio_per_district](/images/ratio_per_district.png){:height="500px" width="800px"}



* Let's have a closer look at the number of deads and injured per accidents.

  1. The accidents that have deads are very small (less than 1%).

    ![Tauladeathsandaccidents](/images/Tauladeathsandaccidents.png){:height="300px" width="500px"}

  2. It is obvious that it these circumstances one or two accidents with a higher than the normal number of casualties might have a significative impact overall. In this chart we can see what I mean: only counting the accidents with fatalities, the accidents with one are over 97% of the total.
      ![Taula2](/images/Taula2.png){:height="200px" width="500px"}

  3. It is also important to point out that the number of accidents that have people injured (minor, severely or dead) are not that  small compared with the total.

    ![TaulaTotal](/images/TaulaTotal.png){:height="300px" width="500px"}

* What about the vehicles?
