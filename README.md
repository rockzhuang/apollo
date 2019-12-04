# Sync ApolloAuto & LGSVL Apollo
This repository is to sync LGSVL fork with ApolloAuto upstream, and make some modifications and examples. 

**The software and source code in this repository are intended only for use with LG Automotive Simulator and *should not* be used in a real vehicle.**

## Table of Contents

1. [Getting Started](#getting-started)
2. [Prerequisites](#prerequisites)
3. [Setup](#setup)
    - [Docker](#docker)
    - [Cloning the Repository](#cloning-the-repository)
    - [Building Apollo and Bridge](#building-apollo-and-bridge)
4. [Launching Apollo Alongside the Simulator](#launching-apollo-alongside-the-simulator)

## Getting Started
The guide outlines the steps required to setup Apollo for use with the LG Automotive Simulator. If you have not already set up the simulator, please do so first by following the instructions [here](https://github.com/lgsvl/simulator).

## Prerequisites
* Linux operating system (preferably Ubuntu 14.04 or later)
* Nvidia graphics card (required for Perception)
    - Nvidia proprietary driver must be installed
    - The current version of Apollo does not support Volta and Turing architectures (this includes Titan V and RTX 2080 GPUs).



## Setup

### Docker
Apollo is designed to run out of docker containers. The image will mount this repository as a volume so the image will not need to be rebuilt each time a modification is made.

#### Installing Docker 19.03 or later
Make sure to install Docker 19.03 or later because NVIDIA GPUs are natively supported as devices in the Docker runtime.

### Cloning the Repository
This repository includes a couple of submodules for HD Maps and lgsvl msgs. To make sure that the submodules are also cloned use the following command:

    git clone --recurse-submodules https://github.com/rockzhuang/apollo.git


### Building Apollo and bridge
Now everything should be in place to build apollo. Apollo must be built from the container. To launch the container navigate to the directory where the repository was cloned and enter:

    bash docker/scripts/dev_start.sh

This should launch the container and mount a few volumes. It could take a few minutes to pull the latest volumes on the first run.

To get into the container:

    bash docker/scripts/dev_into.sh

Build Apollo:

    ./apollo.sh build_gpu


## Launching Apollo alongside the simulator

[![](images/apollo_simulator.png)](images/full_size_images/apollo_simulator.png)

Here we only describe only a simple case of driving from point A to point B using Apollo and the simulator. 

To launch apollo, first launch and enter a container as described in the previous steps.

* To start Apollo:

        bootstrap.sh

    Note: you may receive errors about dreamview not being build if you do not run the script from the `/apollo` directory.

* Launch bridge (inside docker container):

        bridge.sh

* Run the LG SVL Simulator outside of docker. See instructions in the [simulator repository](https://github.com/lgsvl/simulator)
    - Select the `BorregasAve` map and the `Lincoln2017MKZ (Apollo 5.0)` vehicle.
    - Enable GPS, IMU, LIDAR, Main Camera, and Telephoto Camera.
    - (optional) Enable Sensor Effects, Traffic and Pedestrian.

[![](images/apollo.png)](images/full_size_images/apollo.png)


* Open Apollo dreamview in a browser by navigating to: `localhost:8888`
    - Select the `Lincoln2017MKZ` vehicle and `BorregasAve` map in the top right corner.
    - Open the **Module Controller** tab (on the left bar).
    - Enable **Localization**, **Transform**, **Perception**, **Traffic Light**, **Planning**, **Prediction**, **Routing**.
    - Navigate to the **Route Editing** tab.
    - Select a destination by clicking on a lane line and clicking **Send Routing Request**.
    - Enable **Control** in **Module Controller** tab.
    - Watch the vehicle navigate to the destination.
    - To stop the docker container run the `bash docker/scripts/dev_start.sh stop` script in a new terminal (not in the docker container).