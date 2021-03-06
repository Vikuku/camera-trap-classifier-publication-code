[![DOI](https://zenodo.org/badge/146035349.svg)](https://zenodo.org/badge/latestdoi/146035349)
# _Identifying Animal Species in Camera Trap Images using Deep Learning and Citizen Science_

This repo provides the research code used in _Identifying Animal Species in Camera Trap Images using Deep Learning and Citizen Science_. We strongly recommend to use the following repo
https://github.com/marco-willi/camera-trap-classifier to train new models. The main purpose of this repo is to provide reproducability of our results and to provide the models trained in the study which can be used to classify images.

# Pre-Requisites

The following pre-requisites are necessary to run the code.

## Software Installation (non GPU version)

We have used Python 3.5 on Ubuntu to train and apply the models. To train a model we strongly recommend using computers with GPUs (see https://github.com/marco-willi/camera-trap-classifier for more details). The following instructions install the non-GPU Tensorflow version which is suitable to apply the models on new images but very slow for model training.

### Unix installation

The required modules are defined in the requirements.txt and can be installed using pip.
```
pip install -r requirements.txt
```

Example installation on Unix systems using virtual environments (named ctc):
```
virtualenv -p python3.5 --no-site-packages ctc
source ctc/bin/activate
git clone https://github.com/marco-willi/camera-trap-classifier-publication-code.git
cd camera-trap-classifier-publication-code/
pip install -r requirements.txt
```

### Anaconda

Anaconda (https://www.anaconda.com) is a popular software to manage Python installations. We recommend to use it if the above installation procedure results in errors or if using Windows (Unix/Mac is also possible). Pre-requisite is an Anaconda installation (https://conda.io/docs/user-guide/install/index.html). The commands can be executed using, for example, Git BASH (https://gitforwindows.org) or the terminal if using Unix/Mac.

#### Anaconda on Windows

The easiest way to exactly reproduce our environment is to use the provided conda_env_windows.yml file:
```
conda env create -f conda_env_windows.yml
```

If the above script does not work (i.e. if certain packages are not available anymore) the following script can be used to generate our environment:
```
conda create --no-default-packages -n ctc python=3.5
source activate ctc
pip install tensorflow==1.3.0
conda install scikit-learn=0.18.1 keras=2.0.8 dill requests  pillow aiohttp scikit-image pandas
pip install panoptes_client
```

#### Anaconda on Unix

The easiest way to install is to exactly reproduce our environment using the provided conda_env_unix.yml:
```
conda env create -f conda_env_unix.yml
```

If the above script does not work (i.e. if certain packages are not available anymore) the following script can be used to generate our environment on Unix:
```
conda create --no-default-packages -n ctc python=3.5
source activate ctc
conda install tensorflow=1.3.0 scikit-learn=0.18.1 keras=2.0.8 dill requests  pillow aiohttp scikit-image pandas
pip install panoptes_client
```



# Apply a Model

The following code shows an example of how to classify images using the snapshot serengeti species model:

```
python predict.py \
-images_dir /my_data/new_images/ \
-path_to_model /my_data/save/ss/ss_species_51_201708072308.hdf5 \
-model_cfg_json /my_data/save/ss/ss_species_51_201708072308_cfg.json \
-export_dir /my_data/save/ss/
```
Note that 'images_dir' must contain 1 to N sub-directories with images.

The result will be a csv with following entries:
- file_name: the image name
- predicted_class: the class with the highest probability
- predicted_probability: the probability of the predicted class
- predictions_all: a json with keys of each class and their probabilities
- image_path: the full path of the image

Example classification from the snapshot serengeti species model:
```
file_name,predicted_class,predicted_probability,predictions_all,image_path
ASG0004cla_1.jpeg,elephant,1.0,"{'human': 1.2061459e-10, 'cheetah': 7.6320991e-17, 'secretaryBird': 9.2653534e-19, 'hyenaStriped': 1.2458685e-16, 'eland': 4.5333959e-14, 'hare': 1.2339816e-17, 'dikDik': 8.7492429e-17, 'hartebeest': 2.926076e-14, 'gazelleGrants': 2.1007439e-13, 'leopard': 3.0884748e-16, 'insectSpider': 4.3569909e-17, 'zorilla': 2.033801e-17, 'hippopotamus': 3.4702596e-13, 'hyenaSpotted': 3.2176124e-16, 'buffalo': 1.6195639e-11, 'impala': 3.7534744e-14, 'giraffe': 4.5546412e-13, 'warthog': 4.011828e-13, 'bushbuck': 3.1397468e-15, 'ostrich': 4.3003787e-13, 'batEaredFox': 1.3257945e-17, 'reedbuck': 7.9458289e-15, 'honeyBadger': 2.4897844e-17, 'koriBustard': 5.6870173e-15, 'gazelleThomsons': 2.3575537e-14, 'otherBird': 1.4772053e-15, 'wildcat': 1.1637649e-16, 'aardvark': 1.5080654e-15, 'guineaFowl': 2.7299814e-16, 'waterbuck': 3.4258154e-13, 'vulture': 4.1541407e-17, 'genet': 1.7523444e-16, 'wildebeest': 2.1406713e-12, 'aardwolf': 1.3755561e-17, 'elephant': 1.0, 'duiker': 6.224699e-16, 'zebra': 1.278188e-11, 'reptiles': 7.7722187e-16, 'vervetMonkey': 1.8021384e-15, 'lionFemale': 2.4059594e-15, 'porcupine': 2.438521e-15, 'jackal': 2.5444938e-18, 'baboon': 5.8241938e-14, 'lionMale': 5.7171429e-15, 'rhinoceros': 3.5210663e-12, 'civet': 2.7837817e-17, 'rodents': 1.3789448e-17, 'caracal': 5.1348895e-16, 'mongoose': 3.5725514e-17, 'topi': 4.7060778e-15, 'serval': 1.8760704e-18}",/my_data/new_images/ASG0004cla_1.jpeg
```

Example of applying a model on Windows using GitBash:

```
python predict.py \
-images_dir D:\\images\\ss\\ \
-path_to_model D:\\save\\ss\\ss_species_51_201708072308.hdf5 \
-model_cfg_json D:\\save\\ss\\ss_species_51_201708072308_cfg.json \
-export_dir D:\\save\\ss\\
```


# Train a Model

To train a model we recommend using a computer with GPU support as mentioned above. For training new models on new data we recommend using the following code: https://github.com/marco-willi/camera-trap-classifier which is a curated and updated version. To reproduce the results of our study the following steps are required:

## Directory Structure

To train models a specific directory structure is required. The 'root_directory' is an arbitrary directory which must contain the following directory structure (shown is an example for snapshot_wisconsin, other datasets can be added likewise):

```
root_directory/
  images/
    snapshot_wisconsin/
      all/
        deer/
          deer1.jpg
  db/
    snapshot_wisconsin/
      subject_set_used.json
  save/
    snapshot_wisconsin/
  models/
    snapshot_wisconsin/
  logs/
    snapshot_wisconsin
```

- images/: This is for storing the images in class-specific directories.
- db/: This is for storing the dataset and experiment meta-data.
- save/: This is for storing final models and predictions on test/validation data.
- models/: This is for storing intermediate models and other files.
- logs/: This is for storing log files for each model run.

## Training Configuration

Model training relies on the specifications in the [config.ini](config/config.ini) file.

1. Open the [config.ini](config/config.ini) file and make the following adjustments:

- Define the dataset in section [projects]: Adjust the 'panoptes_id' entry and choose the dataset for which to train the model. For example for training a model on the Snapshot Wisconsin dataset use this:
```
[projects]
panoptes_id: snapshot_wisconsin
```

- Define the paths in section [paths]: define the absolute paths as required above. For Example:
```
[paths]
images: /root_directory/images/
models: /root_directory/models/
logs: /root_directory/logs/
db: /root_directory/db/
save: /root_directory/save/
```

- Select the experiment: Go to the selected 'panoptes_id' project entry. For example go to entry [snapshot_wisconsin] if the selected dataset is from Snapshot Wisconsin:
```
[snapshot_wisconsin]
subject_mode: disk_used
experiment_data: disk
...
```
In this section adjust the 'experiment_id'. For example:
```
experiment_id: sw_species_uncropped
```

This leads to the selection of the 'experiment_id' configuration of the model run. For example:
```
[sw_species_uncropped]
model_file: resnet18
load_model:
class_mapping: sw_species
pre_processing: ss
balanced_classes: 0
train_size: 0.9
optimizer: sgd
num_epochs: 70
batch_size: 128
```

2. Create folders for the specific project in each of the paths as defined in the config.ini. For example: create the following directories if training a model for Snapshot Winsconsin (id: snapshot_wisconsin):
- ../db/snapshot_wisconsin
- ../images/snapshot_wisconsin
- ../save/snapshot_wisconsin
- ../model/snapshot_wisconsin
- ../logs/snapshot_wisconsin

3. Make sure the images are in the /images/<project_name>/all/ directory in class specific folders.

4. Make sure the meta-data (.json) are in the /db/<project_name>/ directory.

5. Run the code:
```
python main.py
```


# Available Models used in the Study

 Each of the models below is defined by its name as referred to in the study. Additionally, we  provide the id as referred to in the config.ini (experiment_id) and the names of the model/config files (.hdf5 and .json) as available for download. The format is as follows 'name used in the study / experiment_id / model file name'.

## Snapshot Serengeti

- Empty model - from scratch /  ss_blank_vs_non_blank_small / ss_blank_vs_non_blank_small_201711150811
- Species model - from scratch / ss_species_51 / ss_species_51_201708072308

## Snapshot Wisconsin

- Empty model - from scratch / sw_blank_vs_nonblank_uncropped /sw_blank_vs_nonblank_uncropped_201709150309.hdf5
- Empty model - transfer-learning / sw_blank_vs_nonblank_uncropped_blank_last_layer_only / sw_blank_vs_nonblank_uncropped_blank_last_layer_only_201712171812.hdf5
- Species model - from scratch / sw_species_uncropped / sw_species_uncropped_201709120509.hdf5
- Species model - transfer-learning / sw_species_ss51_last_layer_only / sw_species_ss51_last_layer_only_201710290510.hdf5

## Camera CATalogue

- Empty model / cc_blank_vehicle_species_v2 / cc_blank_vehicle_species_v2_201708200608.hdf5
- Empty model - transfer-learning / cc_blank_vehicle_species_v2_ss_last_layer / cc_blank_vehicle_species_v2_ss_last_layer_201712160112.hdf5
- Species model - from scratch / cc_species_v2 /  cc_species_v2_201708210308.hdf5
- Species model - transfer-learning / cc_species_ss51_last_layer_only / cc_species_ss51_last_layer_only_201708221508.hdf5
- Species model - from scratch - 75% / cc_species_75p_train / cc_species_75p_train_201710150710.hdf5
- Species model - transfer-learning - 75% / cc_species_75p_train_ss_last_layer / cc_species_75p_train_ss_last_layer_201710161810.hdf5
- Species model - from scratch - 50% / cc_species_50p_train / cc_species_50p_train_201710152110.hdf5
- Species model - transfer-learning - 50% / cc_species_50p_train_ss_last_layer / cc_species_50p_train_ss_last_layer_201710170510.hdf5
- Species model - from scratch - 25% / cc_species_25p_train / cc_species_25p_train_201710160310.hdf5
- Species model - transfer-learning - 25% / cc_species_25p_train_ss_last_layer / cc_species_25p_train_ss_last_layer_201710171410.hdf5
- Species model - from scratch - 12.5% / cc_species_12_5p_train / cc_species_12_5p_train_201710171810.hdf5
- Species model - transfer-learning - 12.5% / cc_species_12_5p_train_ss_last_layer / cc_species_12_5p_train_ss_last_layer_201710180310.hdf5

## Elephant Expedition

- Empty model / ee_blank_vs_nonblank_v /  ee_blank_vs_nonblank_v2_201708231608.hdf5
- Empty model - transfer-learning / ee_blank_vs_nonblank_v2_blank_last_layer_only / ee_blank_vs_nonblank_v2_blank_last_layer_only_201712151812.hdf5
- Species model - from scratch / ee_nonblank_no_cannotidentify_new_subject / ee_nonblank_no_cannotidentify_new_subject_201708180508.hdf5
- Species model - transfer-learning / ee_nonblank_no_ci_ss51_last_layer_only_v2 / ee_nonblank_no_ci_ss51_last_layer_only_v2_201709180209.hdf5


# Code Structure

## [config](config/)
Configuration file: models and paths (credentials not necessary).

## [learning](learning/)
Files to run the training (learning) process.

## [tools](tools/)
Different helper functions, e.g. fetching data, classes for projects, experiments and subjects.


# Acknowledements

This code was used in the following study:

*Identifying Animal Species in Camera Trap Images using Deep Learning and Citizen Science, 2018, Methods in Ecology and Evolution*

Authors: Marco Willi, Ross Tyzack Pitman, Anabelle W. Cardoso, Christina Locke, Alexandra Swanson, Amy Boyer, Marten Veldthuis, Lucy Fortson


The ResNet models are based on the implementation provided here:
https://github.com/raghakot/keras-resnet
