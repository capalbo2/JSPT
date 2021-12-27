# JSPT
Javascript  Strategic Planning Tool with graph.js and D3.js


This strategic planner allows the user to design and allocate resouces in order to achieve desired outcomes.
It is based on a graph structure, supported by graph.js that over time evolves to produce final quantities over a time-horizon (from 1 month to 30 years).
VIsual representations of graph through time are made using D£
It can be used for planning losing weight over 6 months, the production needs of a company over 5 years , or the achievement of financial goals over 20 years.


User must model  his plan by defining these three objects:
1.Resources: available resources.
2.Processes: actions that consume/create resources and contribute or detract to a goal.
3.Goals: the final objective, achieved by a series of resources used and processes completed.
(will call these «R/P/G»)

Once defined the objects user can run simulations over a «time horizon» and see the state of his resource/processes/goals
The efectiveness of this tool is determined of the quality and realistic nature of the R/P/G parameters.
Once defined my R/P/G I can save it , modify it and confront the simulations of  different R/P/G configurations.

Reports: 
Time to achieve goal(s).
Used resources per goal.
Free resources at achieved goal.
(differential reports of these between different R/P/G configurations)


Definition of Objects
=============
R/P do not have the time dimension, they are implied as daily resources.  JSPT should not be used on timeframes < 2 days. unless you re-interpret  «days» as «minutes»! but the function of this tool is long-time planning so we cannot guarantee a good functionality.
G has a time dimension.

   1---Resources (name, state, maximum_day_amount)
     -name: name of the resource.
     -state: occupied or free.
     -Maximum_day_amount :  how many nodes with this name can i have. (any further resource creation with this name will be stopped once the limit is reached. limits the nodes on the graph of this name examples: time, mental energy, physical energy, money, emotional energy, square feet, liters of water, calories, KWatt.

  2---Processes (name, req_resources_day, state)
  name: name of the process
  req_resources_day: 1....n resources ( can be different)
  state: allocated/nonallocated
  effect: create resources, delete resources , contiribute to goals, detract from goals.
Processes done over time consume resources daily and complete goals.


3 --- goals (name, time, req_processes_day, state, effect)

    req_processes_day:
    effect: can create or delete processes, can contribute o detract to 					other goals, can create or delete resources!
               examples:
                 ex1.   goal «pay off mortgage early» will consume money, needs n «payment» processes to be completed, but once done will delete fixed payments processes, therefore leaving resources unallocated.
                ex2.  goal «lift 200lbs» once completed may add 20 units of physical_energy to your daily resources. to be used in whatever process after that date.



4 --- horizon: 1mo, .....10year....30year.


Edit:  resources , processes, goals can be edited.
           name, req, effect can all be edited.
         state can never be edited, because it depends on the calcultations

Simulation: you can run over a horizon your current setup and see if realistcally you will achieve goals X and what resource will it take

Implementation
===========
Implemented through a graph. 
Resources, processes and goals are vertices,  req fields create arcs.
-- look for cyclical and warn for cyclical 
--- look for positive feedback!
--- analyze performance.


Future features
=========

TEMPLATES
=======
Typical R/P/G graphs, with realistic parameters, for a quick start in simulating. user can load a template, and edit it according to need.


ERROR CALCULATION OF SIMILAR PROCESSES FEATURE
------------------------------------------------------------------------------

Once defined my R/P/G simulation, i can then track results over time , the difference between «real world» values of a process (real resources occupied, real contribution to goal achievement) can then be inserted in the process 

Every process will have a «reality_type» , the significance of this field is that all processes have similar behaviour in the real world.
If process A has a reality_type of +10% used resources and +20% contribution to goal. all processes of that same type will be modified with the same proportion once the calculated error is applied.

Example, let's say  I want to learn 5 technologies in 1 year. these are 5 processes, that are evaluated to consume 2 hours day for 6 months.
the goal of learning tech A requires process «learn sql» to be done 60 times. «learn sql» process has type «giovanni_learning_coding».
If i track the learning of one process and see goal of learning tech A takes -33% time, i can apply that error on a new simulation and all processes of type «giovanni_learning_coding» will be updated in proportion to the new info.


OVER TIME COEFFICIENT.
WIll add a parameter, that augments or decreases performance over time to reflect how some of the parameters changed as applied through time.
(examples: 
-as a student learns, he becomes more proficient, i can model a +10% results and -10% occupied resources every 6 months
- some machines reduce their efficiency as they get older, i can add a +1% resource consuming every year.
