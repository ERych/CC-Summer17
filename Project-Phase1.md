# DATS 6450: Cloud Computing
## Phase 1: Setting up an virtual machine for machine learning programs

*Machine Learning Environment with AWS*

### Step 1. Launch EC2 Instance
1. Go to http://console.aws.com and login using your AWS Credentials

2. Under Compute, select EC2 to visit the EC2 Dashboard

3. Select Launch Instance and create an instance with a GPU graphics card. I selected g3.8xlarge. Follow the configuration pages to setup. Add 20 GiB of storage, and make sure to select your existing key pair or create one.

4. Your EC2 instance will launch after completing setup and the indicator will turn green once it is running.

### Step 2. Connect using terminal (MAC) or bash (Windows)
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

### Step 3. Install and Test Cuda
1. Install Nvidia Drivers using the following commands:
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo apt-get install nvidia-384
```
Make sure to restart your server after installing.

2. Next, install Cuda. Go here for the official documentation and choose the correct installation:https://developer.nvidia.com/rdp/cudnn-download.
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

### Step 4. Install and Test cudaNN

CudaNN is a GPU-accelerated library of primitives for deep neural networks. This library provides highly tuned implementations for standard routines such as forward and backward convolution, pooling, normalization, and activation layers.

1. You must first visit https://developer.nvidia.com/rdp/cudnn-download and create a NVIDIA Developer Program account. This is necessary to download cudaNN

2. An easy way to install this library is to download it from Chrome *inside your EC2 Instance*.
Install Google Chrome using the following commands:

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

4. Copy the following files into the Cuda Toolkit Directory
```
$ ubuntu@ip-172-31-33-60:~/Downloads$ cd cuda
$ ubuntu@ip-172-31-33-60:~/Downloads/cuda$ sudo cp lib64/* /usr/local/cuda/lib64/
$ ubuntu@ip-172-31-33-60:~/Downloads/cuda$ sudo cp include/* /usr/local/cuda/include/
```

5. Test the installation:
```
cat /usr/local/cuda/include/cudnn.h | grep CUDNN_MAJOR -A 2    
```

### Step 5: Install Python and it's related libraries
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
$ sudo pip3 install keras
$ sudo pip3 install theano
```

### Step 6. Install Tensorflow
```
$ sudo pip3 install tensorflow
```

Run a short TensorFlow program to see if it's working:
```
vim tftest.py
```
```python
# Python
import tensorflow as tf
hello = tf.constant('Hello, TensorFlow!')
sess = tf.Session()
print(sess.run(hello))
```
```
python3 tftest.py
```
The system should output the following:
```
Hello, TensorFlow!
```

### Step 7. Install Caffe
This instructions are based on this helpful resource: https://github.com/BVLC/caffe/wiki/Install-Caffe-on-EC2-from-scratch-(Ubuntu,-CUDA-7,-cuDNN-3)

1. Install the dependencies with all of these installs:
```
sudo apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev libopencv-dev libboost-all-dev libhdf5-serial-dev protobuf-compiler gfortran libjpeg62 libfreeimage-dev libatlas-base-dev git python-dev  libgoogle-glog-dev libbz2-dev libxml2-dev libxslt-dev libffi-dev libssl-dev libgflags-dev liblmdb-dev python-yaml
```
Then run:
```
$ sudo easy_install pillow
```
2. Clone Caffe.git
```
$ cd ~
$ git clone https://github.com/BVLC/caffe.git
```
3. Cd to Caffe folder and run the follwing:
```
$ cat python/requirements.txt | xargs -L 1 sudo pip install
```

4. Next, copy the Makefile.config.example to create our own Makefile.config
```
$ cp Makefile.config.example Makefile.config
$ vi Makefile.config
```
Uncomment the line: USE_CUDNN := 1
Make sure the CUDA_DIR correctly points to our CUDA installation.

5. Now we build Caffe. Set X to the number of CPU threads (or cores) on your machine. For me, this was 8.

```
$ sudo apt-get install htop
```
```
$ make pycaffe -jX
$ make all -jX
$ make test -jX
```
Now test Caffe:
```
$ ./data/mnist/get_mnist.sh
```

### Step 8. Install Torch
This instructions are based on this helpful resource:
http://torch.ch/docs/getting-started.html

Run the following in the terminal:
```
$ git clone https://github.com/torch/distro.git ~/torch --recursive
$ cd ~/torch; bash install-deps;
$ ./install.sh
```
The last command will take some time to install.

Then run:
```
source ~/.bashrc
```
Test if it's working:
```
$ th
```
If successful, Torch will start:
```

  ______             __   |  Torch7                                   
 /_  __/__  ________/ /   |  Scientific computing for Lua.         
  / / / _ \/ __/ __/ _ \  |                                           
 /_/  \___/_/  \__/_//_/  |  https://github.com/torch   
                          |  http://torch.ch       
```

### Step 9. Install pycharm
1. Run ```google-chrome``` to open the Chrome browser

2. Navigate to https://www.jetbrains.com/pycharm/download/download-thanks.html?platform=linux&code=PCC and download the Linux version of PyCharm Community

3. Cd to Downloads folder in the terminal and unzip:
```
$ cd ~
$ cd Downloads
$ tar -zxf pycharm-community-2017.2.tar.gz
```

4. To open PyCharm community:
```
$ cd pycharm-community-2017.2
$ cd bin
$ cd ./pycharm.sh
```
### You have now made a deep learning virtual machine! Congrats!
