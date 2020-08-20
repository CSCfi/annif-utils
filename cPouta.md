
# Minimal instructions for running Annif on cPouta

## Prerequisites

Make sure that you have installed [docker Engine](https://docs.docker.com/engine/install/) and [docker-compose](https://docs.docker.com/compose/install/) on your cPouta virtual machine.

## Prepare data for Annif

Please train, test and evaluate relevant ML and NLP models using the instructions as as nicely described on Annif [wiki page](https://github.com/NatLibFi/Annif/wiki). You can for example use docker-based tutorials for training your models by using annif container as below:

```
sudo docker run  -it -v /path/of/Annif-data:/Annif-data  --name annif -u $(id -u):$(id -g) quay.io/natlibfi/annif:latest bash
```

## Run Annif web-app with docker-compose

Clone Annif github repository and launch compose version of Annif as below:

```
git clone https://github.com/NatLibFi/Annif.git
cd Annif/
sudo ANNIF_PROJECTS=~/annif-projects MY_UID=$(id -u) MY_GID=$(id -g) docker-compose up

````

Above docker-compose deployment assumes that all the information related to projects and vocabularies after training ML/NLP models are  available inside , e.g.,  `annif-projects` folder and configuration file for respective projects and their backends are stored inside `projects.cfg` file. The annif-projects should also include `mauidata`  folder to be able to integrate properly with Maui Server.

## Access Annif application via browser

Once Annif is successfully deployed, Navigate to http://externalIP:8080/ on web browser to access Annif webpage.

## Funding:

<img src="./EU_logo.png" width="30%">
