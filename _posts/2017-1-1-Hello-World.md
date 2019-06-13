---
layout: post
title: BarcelonaAccidents 2010-18
---

Yes, I let myself go and get sucked in by my origins. I just had to work on a database that is from my hometown (Barcelona) as a modest way to show my support to the fellow Catalans in these sad days ('Stay strong and together')

I obtained all the information thanks to openData Barcelona. All these file are easily available in [BarcelonaOpenData](http://opendata-ajuntament.barcelona.cat/en/)

![nofrauds](/images/BCN01.jpg){:height="300px" width="600px"}


## Brief description of Barcelona Accidents.

  1. Dataset contains all the accidents that the local police attended from 2010 to 2018.
  2. The information is gathered and spread in different files:
    - Accidents.
    This one has all information about every single accident and its time (date and time) and location (street, district, longitude and latitude). It also gives detailed information about the victims of the accident: how many in total were involved and what type of injuries they had: minor, severe or death. It also gives us the number of vehicles involved in the accident. Finally, there is a column that explains what the role of the pedestrian in the accident if in any case it was one involved.
    - Causes of the accident.
    This one adds 'initial_street' and 'mediate_cause'. While the meaning of the first added column is unclear and will need further analysis, the second one -'mediate_cause' - indicates the reason - if any - behind the accident (alcohol, drugs, weather, etc).
    - People.
    This file contains the information for every single person involved in the accident. What vehicle they were in (car, motorcycle, etc), gender, age, his/her role in the accident (driver, passenger or pedestrian), if he/she was present or not and lastly what injuries suffer and if he/she was treated or not.
    - Types.
    This CSV adds a file named 'accidents_description' and it describes the type of accident: tun over, collision, crash, etc
    - Vehicles.
    This one adds a more detailed information about the vehicle: apart from type (car, bus,etc), it considers the brand name, model, color, what type fo license was holding the driver and finally for how long that driver has been holding the license.

  3. It has been a hard work to combine all different CSV due to different encodings and column's names.
  4. Once I have all data combined, I will analyze the accidents as a whole and, at the same time, the accidents in which people died. I will compare both (all accidents and deadly accidents) to see if there is something we can learn about them and to see if they are correlated somehow.
  5. Important consideration: the accidents where people died represent 0.27% (just I did not miss any comma) of the total. It haas two conclusions: this is very imbalanced dataset and any change in the number of deaths even by one accident can change its statistics significantly.
  6. The link on GitHub for a more detailed and technical explanation is available here:[BarcelonaAccidents](https://github.com/AlexChicote/BarcelonaAccidents). I will keep on updating them as I move forward.


## Non-technical description of BarcelonaAccidents.

## Gathering the data.

First I combine all files for accidents, causes, people, types and vehicles from 2010 to 2018. Once done, I merged them all together using the number of case file as a common index. You can see the coding in [BarcelonaAccidents](https://github.com/AlexChicote/BarcelonaAccidents/BarcelonaAccidents2010-18.ipynb)

DATA ANALYSIS


I will start with the most basic information contained in the first file, time, location, number f vehicles, number of people affected and its injuries to finish with the pedestrian cause (reminder: only those in which Barcelona local police is involved) like location, time, number of vehicles, number of people implied and the degree of its injuries.

Time Based Analysis


* Year.

![accidents_deaths_per_year](/images/accidents_deaths_per_year_2018.png){:height="400px" width="800px"}

The trend is clear in both cases; while the accidents have been increasing during last years up until 2018 where the were reduced, the number of people that lost their lives in those have clearly diminished except for 3 years: 2014, 2016 and 2018. A more detailed analysis show how the ratio deaths per accidents spiked for 2018.

![accidents_deaths_per_year](/images/avg_deaths_accident_per_year_2018.png){:height="400px" width="800px"}

* Month.

![accidents_deaths_per_month2](/images/accidents_deaths_per_month_2018.png){:height="400px" width="800px"}

In the graph, we can see how the number of accidents keep steadily during most part of the year except for July and August where they clearly go down.
When it comes to analyze deaths, the picture changes. While it looks like the previous months to summer it slows down ( less people dying), it peaks in summer to then go down again.

* Weekday.

![accidents_deaths_per_weekday2](/images/accidents_deaths_per_weekday_2018.png){:height="450px" width="800px"}

Do you see how the accidents grow constantly during the week to reach its maximum on Friday and then go down during the weekend? Well, when it comes to deaths the analysis is that weekends are the days with the highest numbers of deaths while Monday... What does it mean? Isn't it strange to see Monday so high out of nowhere?

* Weekend.

![accidents_deaths_per_weekend2](/images/accidents_deaths_per_weekend2_2018.png){:height="450px" width="800px"}

In this case, we can see how the line representing the number of deaths, peaks up on Friday night and also on Saturday night but not as much. This is important to point out because this explains why Saturday stands out in the number of deaths. Saturday includes the late hours of the Friday night and early hours of Saturday night. But if we understand Friday night as the hours from Friday 10pm to Saturday morning 5am, Friday should stand out as the most dangerous hours of the week.

* Hour.

![accidents_deaths_per_hour](/images/accidents_deaths_per_hour_2018.png){:height="450px" width="800px"}

This new chart is confirming all that we have been seeing so far. It is that the riskiest time to drive in Barcelona is during the early morning mainly during the weekend. The first chart, similar to the one we had for the weekend, shows that correlation broken between accidents and deaths.

I love this chart. it shows exactly the non-correlation of the accidents as a total and the deadly ones.

![accidents_deaths_per_year](/images/average_per_hour.png){:height="400px" width="800px"}

* Shift

Doing the analysis by shift, it is a little bit overdoing it but I think it will give a clearer picture of the non-correlation in hour for accidents as a whole and the ones with people dying. As a reminder: morning goes from 6am to 2pm, afternoon up until 10 and the rest is night.

![accidents_deaths_per_year](/images/accidents_deaths_by_shift_2018.png){:height="400px" width="800px"}

Conclusion: while accidents seem to be more related to traffic (more traffic more accidents) the accidents with deaths look like they are more associated to the nigh oriented activities.


Location Based Analysis

* District.

I could not let it go by without having and analysis about the neighborhoods or districts. It is important to say that traffic and accidents have a different geographic relation. The size of every district, its structure (a lot of pedestrian streets where traffic is not allowed) or its location (a district that is placed in the middle of important streets and a lot of traffic is funneled through it) might be introduced to the analysis in order to have a real deep understanding.

What I realized about Barcelona is that the Eixample district is the one that has a lot of the traffic and therefore the accidents and the number of deaths but if you analyze the ratio of the number of deaths per accident it is really low and Les Corts and Nou Bariis, where there is not a lot of traffic, gets the winning position.

![ratio_per_district](/images/ratio_per_district_2018.png){:height="500px" width="800px"}



* Let's have a closer look at the number of deaths and injured per accidents.

  1. The accidents that have deaths are very small (less than 1%).

    ![Tauladeathsandaccidents](/images/Tauladeathsandaccidents.png){:height="300px" width="500px"}

  2. It is obvious that it these circumstances one or two accidents with a higher than the normal number of casualties might have a significative impact overall. In this chart we can see what I mean: only counting the accidents with fatalities, the accidents with one are over 97% of the total.
      ![Taula2](/images/Taula2.png){:height="200px" width="500px"}

  3. It is also important to point out that the number of accidents that have people injured (minor, severely or dead) are not that  small compared with the total.

    ![TaulaTotal](/images/TaulaTotal.png){:height="300px" width="500px"}

* What about the vehicles?

First of all, it is important to mention that the accidents that involved more vehicles are more likely to have more people affected and therefore victims. In other words: for an accident to occur you at least need a driver per vehicle.
Obviously, the probability of having more vehicles involved in an accident decreases but at the same time when it happens it is more likely to affect more individuals.

Said that, I just show a plot with the ratio of deaths per accident per number of vehicles. I stopped at 9 because there is an outlier: one accident with deaths and 9 vehicle involved that throws off everything as you can see in the following chart.

  ![ratio_deaths_accidents_vehicles](/images/ratio_deaths_accidents_vehicles.png){:height="300px" width="600px"}

  ![TaulaVehicles](/images/TaulaVehicles.png){:height="300px" width="600px"}

  * What about the the field "pedestrian cause"?

  In this dataset, we have a field with the information related to the cause behind the accident when a pedestrian is involved. As I show in the chart, it has over 50% of 'Unknown" what makes it hard to analyze but let's have a look at the causes when they are known.

  ![TaulaPedestrians](/images/pedestrian_cause_accidents_2018.png){:height="200px" width="600px"}
