# LTR

A general PyTorch based framework for learning tracking representations. The repository contains the code for training the [**ATOM**](https://arxiv.org/pdf/1811.07628.pdf) tracker.

## Table of Contents

* [Quick Start](#quick-start)
* [Overview](#overview)
* [Train Settings](#train-settings)
* [Training your own networks](#training-your-own-networks)

## Quick Start
The installation script will automatically generate a local configuration file  "admin/local.py". In case the file was not generated, run ```admin.environment.create_default_local_file()``` to generate it. Next, set the paths to the training workspace, 
i.e. the directory where the checkpoints will be saved. Also set the paths to the datasets you want to use. If all the dependencies have been correctly installed, you can train a network using the run_training.py script in the correct conda environment.  
```bash
conda activate pytracking
python run_training train_module train_name
```
Here, ```train_module``` is the sub-module inside ```train_settings``` and ```train_name``` is the name of the train setting file to be used.

For example, you can train using the included default ATOM settings by running:
```bash
python run_training bbreg atom_default
```


## Overview
The framework consists of the following sub-modules.  
 - ```actors```: Contains the actor classes for different trainings. The actor class is responsible for passing the input data through the network can calculating losses.  
 - ```admin```: Includes functions for loading networks, tensorboard etc. and also contains environment settings.  
 - ```dataset```: Contains integration of a number of training datasets, namely [TrackingNet](https://tracking-net.org/), [GOT-10k](http://got-10k.aitestunion.com/), [LaSOT](https://cis.temple.edu/lasot/), 
 [ImageNet-VID](http://image-net.org/), and [COCO](http://cocodataset.org/#home).  
 - ```data_specs```: Information about train/val splits of different datasets.   
 - ```data```: Contains functions for processing data, e.g. loading images, data augmentations, sampling frames from videos.  
 - ```external```: External libraries needed for training. Added as submodules.  
 - ```models```: Contains different layers and network definitions.  
 - ```trainers```: The main class which runs the training.  
 - ```train_settings```: Contains settings files, specifying the training of a network.   
 
## Train Settings
 The framework currently contains the following training settings:  
 - [```bbreg.atom_default```](train_settings/bbreg/atom_default.py): The default settings used for training the network in [ATOM](https://arxiv.org/pdf/1811.07628.pdf).
 
 
## Training your own networks
To train a custom network using the toolkit, the following components need to be specified in the train settings. For reference, see [atom_default.py](train_settings/bbreg/atom_default.py).  
- Datasets: The datasets to be used for training. A number of standard tracking datasets are already available in ```dataset``` module.  
- Processing: This function should perform the necessary post-processing of the data, e.g. cropping of target region, data augmentations etc.  
- Sampler: Determines how the frames are sampled from a video sequence to form the batches.  
- Network: The network module to be trained.  
- Objective: The training objective.  
- Actor: The trainer passes the training batch to the actor who is responsible for passing the data through the network correctly, and calculating the training loss.  
- Optimizer: Optimizer to be used, e.g. Adam.  
- Trainer: The main class which runs the epochs and saves checkpoints. 
 

 