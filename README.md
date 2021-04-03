# Steps to Run TF2 object detection on GPU in Ubuntu=18.04 

## Run these commands on Ubuntu Terminal to first install CUDA and Cudnn

#### Note -  It works perfectly on 1050 ti but if you have other graphics card then first check CUDA compatibility
```
# Remove CUDA pakages to avoid conflicts 

sudo apt-get purge nvidia* 
sudo apt remove nvidia-* 
sudo rm /etc/apt/sources.list.d/cuda* 
sudo apt-get autoremove && sudo apt-get autoclean 
sudo rm -rf /usr/local/cuda*

# Install Nvidia-driver

sudo ubuntu-drivers autoinstall

# Update and install supporting packages

lspci | grep -i nvidia

gcc --version

# system update
sudo apt-get update
sudo apt-get upgrade


# install other import packages
sudo apt-get install g++ freeglut3-dev build-essential libx11-dev libxmu-dev libxi-dev libglu1-mesa libglu1-mesa-dev

# Install CUDA 10.1

sudo add-apt-repository ppa:graphics-drivers/ppa
sudo apt-key adv --fetch-keys http://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
echo "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64 /" | sudo tee /etc/apt/sources.list.d/cuda.list
sudo apt-get update

sudo apt-get -o Dpkg::Options::="--force-overwrite" install cuda-10-1 cuda-drivers

# Setup path vars

echo 'export PATH=/usr/local/cuda-10.0/bin:$PATH' >> ~/.bashrc
echo 'export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64:$LD_LIBRARY_PATH' >> ~/.bashrc
source ~/.bashrc
sudo ldconfig
```

#### Now go to NVIDIA Cudnn download  [here](https://developer.nvidia.com/cudnn)
Sign In and dowload Cudnn version 7.6.5.32 for CUDA 10.1 tar gz file

```
# Unzip

CUDNN_TAR_FILE="cudnn-10.0-linux-x64-v7.6.5.32.tgz"
tar -xzvf ${CUDNN_TAR_FILE}

# Copy Cudnn files to CUDA folder

sudo cp -P cuda/include/cudnn.h /usr/local/cuda-10.0/include
sudo cp -P cuda/lib64/libcudnn* /usr/local/cuda-10.0/lib64/
sudo chmod a+r /usr/local/cuda-10.0/lib64/libcudnn*

# At last reboot
```

#### Now verify installations
```
nvidia-smi
nvcc -V
```
#### If GPU cant be found the turn of safe boot of ubunbtu and install cudatoolkit
```
sudo apt install nvidia-cuda-toolkit
```

### Now clone this repo because I made some changes for easy installation and running code.

#### Create new environment in conda with python 3.7.10 
```
conda create -n tf2 python=3.7.10
conda activate tf2

# Install libraries and dependencies

conda install -c conda-forge opencv 
cd ./Monk_Object_Detection/13_tf_obj_2/installation
pip install requirements_cuda10.txt 

sudo apt-get install cuda-cudart-10-1
```
And now check tensorflow GPU is running or not

```
python3
import tensorflow as tf
tf.test.is_gpu_available()
```
This should give output as True
