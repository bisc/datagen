# Prospect tool for probability specification and data generation
Prospect (a PRObability SPECification Tool) generates time-series data from probabilistic constraints and independence specifications.


## Running Prospect

Requirements: Wolfram Mathematica, version 12.1+

1) Open the Request_input.nb notebook, and run it
2) When prompted to enter file name for input, select a valid input file from the same directory as the Request_input.nb notebook. In the comments of the file exist several examples that can be copied/pasted to observe the functionality.
3) The experiment files can be found in experiments/specs/

## Specification File Format

A valid Prospect file is a txt of the following form, containing three sections separated by blank lines:

```casetype: <one of "static", "timeinvariant", or "timevariant">  
variables: <list of variable name here>  
values: <list of value lists for each variable>  
timesteps: <list of time steps for the Markov blanket shape for each variable> // Not in static case  
numsamples: <natural number of samples to generate>  

independence  
indep[{X, Y, Z...}] // an unconditional independence statement, where X, Y, Z, etc. are all variables previously specified  
condIndep[{X, Y, Z...} | {A, B, C...}] // a conditional independence statement, where X, Y, Z, A, B, C, etc. are all variables previously specified

base // only in time-variant case

< probability specifications line-by-line: algebraic equations with left- and right-hand sides over terms P[event] or P[event | event]  
- These equations can only contain variables with absolute time steps
- Events are predicates over variables taking values. NOT, OR, AND boolean operations supported. Variable equality is also supported (ex. P[X = Y]) if X and Y have the same value set>

main

< probability specifications line-by-line: algebraic equations with left- and right-hand sides over terms P[event] or P[event | event]  
- These equations can only contain variables with relative times and no earlier than those at t-max(timesteps for that variable)
- NOT, OR, AND boolean operations supported. Variable equality is also supported (ex. P[X = Y]) if X and Y have the same value set.>
```

## Running Baseline Pyro Scripts
Included in this repository are a collection of scripts written in Pyro v1.5.1 (based on Python v3.8.5). They may be found in experiments -> baseline. The "accurate" baseline scripts imperatively generate data from several example specifications found in experiments -> specs. The "naive" scripts highlight potential pitfalls of imperatively coding the specifications in this way. You can run the scripts using the follow steps.
1) Install Python and Pyro using the instructions here: https://pyro.ai/.
2) In your terminal, navigate to the directory containing the script(s).
3) For the static and time-invariant scripts, type "python <filename.py> <# of time steps desired>". For the time-variant scripts, type "python <filename.py> <# of time steps desired> <# of time series desired>".
4) After running a script, one or more .csv files with the generated data will appear in your directory. In the time-variant case, each file represents data for a variable, each table row is a time series, and each column is a time step.
