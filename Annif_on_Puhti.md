
# Running Annif on Puhti Super Computer at CSC

## Prerequisites

 As Puhti users are not granted previlized root access for running docker containers, Annif can not be deployed as a docker application. One has to use HPC-compliant singularity containers instead.

Prepare Annif singularity image from docker image (for example, on cPouta VM or any linux environment) as below:

```
sudo docker tag quay.io/natlibfi/annif localhost:5000/annif:latest
sudo docker run -d -p 5000:5000 --restart=always --name registry registry:2
sudo docker push localhost:5000/annif:latest
singularity build annif.simg docker://localhost:5000/annif:latest

# or simply

singularity build annif.simg  docker://quay.io/natlibfi/annif

```
and then copy the singularity image (annif.simg) to your work space (for example, to scratch directory) on Puhti

Alternatively, it is possible to convert annif docker image to singularity image using sbatch script on puhti as below:
```
#!/bin/bash
#SBATCH --time=0:30:00
#SBATCH --partition=small
#SBATCH --account=project_xxx
#SBATCH --mem-per-cpu=2000

export SINGULARITY_CACHEDIR=$PWD
export TMPDIR=$PWD
singularity build annif.simg  docker://quay.io/natlibfi/annif

```
and then submit sbatch job. Once job is finished successfully, you can see annif.simg file in the job directory. 

## Run Annif singularity application as a batch job

One can find instructions for training, testing and evaluating relevant ML and NLP models on Annif [wiki page](https://github.com/NatLibFi/Annif/wiki).  

Singularity was installed on compute nodes on Puhti and Annif singularity application can be deployed as a batch job. A toy example batch script for training a model (e.g., omikuji parabel model) inside Puhti as a singularity application is shown below:

```
#!/bin/bash
#SBATCH --time=0:30:00
#SBATCH --partition=small
#SBATCH --account=project_xxxx
#SBATCH --mem-per-cpu=8000
export TMPDIR=$PWD
singularity -s exec -B $PWD:$PWD    annif.simg ./test_omikuji-parabel-en.sh

```

cat test_omikuji-parabel-en.sh # check the contents of file 

```
annif loadvoc yso-omikuji-parabel-en /path/data-sets/yso-nlf/yso-skos.ttl
annif train yso-omikuji-parabel-en /path/data-sets/yso-nlf/yso-finna-small.tsv.gz
echo "random fortunes written on strips of paper at Shinto shrines and Buddhist temples in Japan." | annif suggest yso-omikuji-parabel-en

```

## (Optionally) upload trained models on Puhti to Allas

Allas is a objects storage environment at CSC. One can upload `annif_projects` to allas and can later be deployed as a web application , e.g., on Rahti/cPouta. `annif_projects` folder should include a data folder with trained projects and vocabs,  a configuration file with  projects and their backends stored inside projects.cfg file and optionally a mauidata folder.

```
module load allas
allas-conf --mode s3cmd
tar -xavf annif_yso_singularity.tar.gz annif_yso_singularity/
# s3cmd mb s3://annif_singularity # First time when you create a bucket
s3cmd put annif_yso_singularity.tar.gz s3://annif_singularity --acl-public

```
## Funding:

<img src="./EU_logo.png" width="30%">
