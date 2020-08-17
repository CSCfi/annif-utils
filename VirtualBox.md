###  VirtualBox for Annif

If cPouta and Rahti environment at CSC is not familiar to explore Annif, you can use Oracle VM VirtualBox which is a free and  open source software for virtualization. The software  has to be installed on your work station in order to explore capabilities of Annif. More information about the VirtualBox can be found [here](https://www.virtualbox.org/manual/ch01.html).

The  VirtualBox image for Annif was built on ubuntu (64 bit) operating system and  can be downloaded from  from CSC's object storage system, called Allas. It  is a ~9GB zip file so it can take a while to download.

The following are minimum instructions to get started with Annif VirtualBox:

- Download and install Oracle VirtualBox on your local machine
  from [here](https://www.virtualbox.org/wiki/Downloads)
- Download OVA file of Annif VM from Allas object storage [here](www)
- Start up the VirtualBox software on your computer.
- Go to the menu bar of main window and then import OVA file as below: Select File -> Import Appliances -> select 'local file system' (from source panel on the left) -> import .ova file
- Verify the settings in the center window and make any changes if needed
- Click import button at the bottom to allow VirtualBox to import the file and configure it for use

The annif machine should now appear at the list of VMs on the left side. Select it and press Start. A new window should appear and after a while, you should see a Linux (Xubuntu) desktop. Open a terminal window (by double-clicking on the Terminal desktop icon). You can also change the keyboard layout now from the upper right corner.

Open terminal and navigate to the folder where annif folder including tutorial data is located.

```
cd  /home/annif/Desktop/Annif
```
activate annif virtual environment

```
source venv/bin/activate

```
The `maui` backend is integrated with Annif with Maui Server, which is a RESTful microservice wrapper around the Maui automated indexing tool. You can start mauiserver as below:

```
./mauiserver_start.sh

```
It will ask for admin password which is  `annif`.  You should now have all the  environment ready for testing Annif with different ML/NLP backend algorithms.

Try checking the different models using the following command:

```
annif list-projects
```
To start with, trained examples are provided with yso-en dataset. You can add any other dataset and train different models according to  instructions as provides in Annif [wiki page](https://github.com/NatLibFi/Annif/wiki).

You can for example test the existing models inside the VirtualBox as below:

```
echo "Machine learning algorithms build a mathematical model based on sample data" | annif suggest yso-tfidf-en

echo "frequently occurring or otherwise salient terms in the document are matched with terms in the vocabulary" | annif suggest yso-maui-en

echo "random fortunes written on strips of paper at Shinto shrines and Buddhist temples in Japan." | annif suggest yso-omikuji-parabel-en

echo "ensemble methods use multiple learning algorithms to obtain better predictive performance" | annif suggest yso-ensemble-en

echo "ensemble methods use multiple learning algorithms to obtain better predictive performance" | annif suggest yso-nn-ensemble-en


echo "ensemble methods use multiple learning algorithms to obtain better predictive performance" | annif suggest yso-fasttext-en

echo "ensemble methods use multiple learning algorithms to obtain better predictive performance" | annif suggest vw-multi-en


echo "ensemble methods use multiple learning algorithms to obtain better predictive performance" | annif suggest pav-en

```

some tips:
- if you can't run pav-en backend as the process is killed during execution, try increasing RAM memory
- if you need to install any sofwtare/tools, us admin password as 'annif'

References:
- https://github.com/NatLibFi/Annif/wiki

Funding:

![](./EU_logo.png)
