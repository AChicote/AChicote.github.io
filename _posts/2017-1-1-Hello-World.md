---
layout: post
title: BarcelonaAccidents 2010-17
---

Yes, I let myself go and get sucked in by my origins. I just had to work on a database that is from my hometown (Barcelona) as a modest way to show my support to the fellow Catalans in these sad days ('Stay strong and together')

I obtained all the information thanks to openData Barcelona. All this information is easily available in [BarcelonaOpenData]http://opendata-ajuntament.barcelona.cat/en/

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
  5. The link in github for a more detailed and technical explanation is available:[BarcelonaAccidents](https://github.com/AlexChicote/BarcelonaAccidents). I will keep on updating them as I move forward.


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

1. Year.

![year](/users/fcbnyc/dsi/datasets/BarcelonaAccidents/charts/accidents_deads_per_year.png){:height="300px" width="400px"}

The trend is clear in both cases; while the accidents have been increasing during last years, the number of people that lost their life in those have clearly diminished.
