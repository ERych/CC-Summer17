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
$ ssh -i "MyKeyPair.pem" ubuntu@ec2-18-220-74-31.us-east-2.compute.amazonaws.com
```
2. Run the follow commands to update and upgrade the instance
```
$ sudo apt-get update && sudo apt-get upgrade
```
##### Step 3. Install and Test Cuda
1. Install Nvidia Drivers using the following commands:
```
$ sudo add-apt-repository ppa:graphics-drivers/ppa
$ sudo apt update
$ sudo apt-get install nvidia- (press tab to see latest)
```
Make sure to restart your server after installing.

2. Next, install Cuda. Go here for the official documentation:https://developer.nvidia.com/rdp/cudnn-download.
```
$ sudo dpkg -i cuda-repo-ubuntu1404_7.5-18_amd64.deb
$ sudo apt-get update
$ sudo apt-get install -y cuda
$ sudo apt-get upgrade cuda
```
Make sure it works:
```
$ sudo apt-get install linux-headers-generic linux-headers-virtual linux-image-virtual linux-virtual -y

3. Install cuDNN

Visit https://developer.nvidia.com/rdp/cudnn-download and create a NVIDIA Developer Program account. This is necessary to download cuDNN.

Now, go to the toolkit default install location at /usr/local/cuda. Next, run the following:
```
$ sudo dpkg -i libcudnn7_7.0.1.13-1+cuda9.0_amd64.deb
```
Install the developer library
```
$ sudo dpkg -i libcudnn7-dev_7.0.1.13-1+cuda9.0_amd64.deb
```
Install the code samples and the cuDNN Library User Guide:
```
$ sudo dpkg -i libcudnn7-doc_7.0.1.13-1+cuda9.0_amd64.deb
```
Now you need to update your bash file
$ gedit ~/.bashrc
Scroll to the bottom and insert this line:
```
export LD_LIBRARY_PATH="$LD_LIBRARY_PATH:/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64"
export CUDA_HOME=/usr/local/cuda
```




#. Install Python
```
sudo apt-get install python3-pip
```
Once installed, test it's working. Exit() will quit Python once it runs successfully.
![python image](/images/python.jpeg)

#. Next, install

5.sudo pip3 install --upgrade pip
6.sudo apt-get install python3-tk
7.sudo pip3 install numpy
8.sudo pip3 install seaborn
9.sudo pip3 install scipy
10.sudo pip3 install sklearn
11.sudo pip3 install matplotlib
12.sudo pip3 install seaborn
13.sudo pip3 install tensorflow
14.sudo pip3 install keras
