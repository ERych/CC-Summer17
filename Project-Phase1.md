## DATS 6450: Cloud Computing
### Phase 1: Setting up an virtual machine for machine learning programs

*Machine Learning Environment with AWS*

##### Step 1. Launch EC2 Instance
1. Go to http://console.aws.com and login using your AWS Credentials
2. Under Compute, select EC2 to visit the EC2 Dashboard
3. Select Launch Instance and create an instance with a GPU graphics card. I selected g3.8xlarge. Follow the configuration pages to setup. Add 20 GiB of storage, and make sure to select your existing key pair or create one.
4. Your EC2 instance will launch after completing setup and the indicator will turn green once it is running.

##### Step 2. Connect using terminal (MAC) or bash (Windows)
1. Launch your instance using SSH and your terminal of choice

Example:
```
$ ssh -i "MyKeyPair.pem" ubuntu@ec2-18-220-90-33.us-east-2.compute.amazonaws.com
```
2. Run the follow commands to update and upgrade the instance
```
$ sudo apt-get update && sudo apt-get upgrade
$ sudo apt install gcc
```
##### Step 3. Install and Test Cuda
1. Install Nvidia Drivers using the following commands:
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo apt-get install nvidia-384
```
Make sure to restart your server after installing.

2. Next, install Cuda. Go here for the official documentation:https://developer.nvidia.com/rdp/cudnn-download.
```
$ wget https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb
$ cd cuda-repo-ubuntu1604-8-0-local-ga2_8.0.61-1_amd64-deb
$ sudo apt-get update
$ sudo apt-get install -y cuda
$ sudo apt-get upgrade cuda
```
Make sure it works:
```
$ nvidia-smi
```
##### Step 4. Install and Test cudaNN

CudaNN is a GPU-accelerated library of primitives for deep neural networks. This library provides highly tuned implementations for standard routines such as forward and backward convolution, pooling, normalization, and activation layers.

1. You must first visit https://developer.nvidia.com/rdp/cudnn-download and create a NVIDIA Developer Program account. This is necessary to download cudaNN

2. A good way to install this library is to download it from Chrome inside your EC2 Instance. Install Google Chrome using the following commands:

```
#Add Key:
wget -q -O - https://dl-ssl.google.com/linux/linux_signing_key.pub | sudo apt-key add -
```
```
#Set repository:
sudo sh -c 'echo "deb [arch=amd64] http://dl.google.com/linux/chrome/deb/ stable main" >> /etc/apt/sources.list.d/google-chrome.list'
```
```
#Install package:
sudo apt-get update
sudo apt-get install google-chrome-stable
```

Test Chrome is working by running ``` google-chrome ``` in the command line.

2. Next, download the Linux library for Cuda 8.0 in your newly installed Chrome browser from this website:
https://developer.nvidia.com/rdp/cudnn-download

You must sign in using the credentials you created in Part 1.

3. After downloading completes, return to the terminal and unzip the file.
```
$ cd ~
$ cd Downloads
$ tar -zxf cudnn-8.0-linux-x64-v7.tgz
```
```
4. Copy the following files into the Cuda Toolkit Directory
$ ubuntu@ip-172-31-33-60:~/Downloads$ cd cuda
$ ubuntu@ip-172-31-33-60:~/Downloads/cuda$ sudo cp lib64/* /usr/local/cuda/lib64/
$ ubuntu@ip-172-31-33-60:~/Downloads/cuda$ sudo cp include/* /usr/local/cuda/include/
```

5. Test the installation:
```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2    
```

##### Step 5: Install Python and it's related libraries
1. Install Python 3
```
$ sudo apt-get install python3-pip
```
Test if Python 3 is installed: ```python3 --version```.

2. Install the following python libraries:
```
$ sudo pip3 install --upgrade pip
$ sudo apt-get install python3-tk
$ sudo pip3 install numpy
$ sudo pip3 install seaborn
$ sudo pip3 install pandas
$ sudo pip3 install sklearn
$ sudo pip3 install matplotlib
$ sudo pip3 install seaborn
```
3. Install tensorflow (This version is for python 3 with GPU support)
```
$ pip3 install tensorflow-gpu
```

Run a short TensorFlow program to see if it's working:
```python
# Python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```
The system should output the following:
```
Hello, TensorFlow!
```
Installation continued....
```
$ sudo pip3 install keras
$ sudo pip3 install caffe
$ sudo pip3 install theano
$ sudo pip3 install torch
```
