# ROMANTIG_OEE_CALCULATOR

This ROSE-AP is intended as a microservice for automatic OEE, and related metrics, calculation. The service works by connecting to a [Crate database](https://crate.io/) on port 4200, where information about the status of your target process are stored by [Orion](https://fiware-orion.readthedocs.io/en/master/) and [QuantumLeap](https://quantumleap.readthedocs.io/en/latest/) services. 


## Configuration

In order to compute the OEE, the service must know if each possible process state that is found on CrateDB has to be considered an up-time or a down-time state. To do so, please change the `oee_conf.config` file found in the `oee_service` folder, prior to building the image of the service. You have to set the following variables:

```
[PROCESS]
upTimeStates = ["upTimeStateName1","upTimeStateName2",...,"upTimeStateNameN"] #These represents the states to be considered as up-time
downTimeStates = ["downTimeStateName1","downTimeStateName2",...,"downTimeStateNameN"]#These represents the states to be considered as down-time
endStates =["upTimeStateNameN","downTimeStateNameN",...] #Subset of either upTimeStates, downTimeStates or both. These represents the possibly multiple final states of the process
goodEnd =["endStatesName1",...] #Subset of endStates, these represents the final states in which an item is successfully created
badEnd =["endStatesName2",...] #Subset of endStates, these represents the final states in which an item is defective or faulty and has to be discarded

[OEE]
timestep = 300 #The timestep granularity (integer) for the resulting OEE metrics, in seconds
idealTime_ppm = 5 #The theoretical maximum piece per minute rate (integer) that the process is able to deliver in ideal conditions (note: this ppm value cannot be lower than the actual process output rate as this represents a theoretical upper bound) 
```

Be sure that the name of the states written in the config file perfectly match those that are written by your process to the CrateDB, so that the microservice can correctly identify them.

## Setup

navigate to oee-service folder and build the image for the OEE microservice:

```
  cd oee-service

  docker build -t roseap_oee .
```

See usage of this microservice [here](https://github.com/claret-srl/romantig_RAMP_ROSEAP).
