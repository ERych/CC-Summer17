## DATS 6450: Cloud Computing
### Phase 1: Setting up an virtual machine for machine learning programs

*Machine Learning Environment with AWS*

##### Launch EC2 Instance
* Go to http://console.aws.com and login using your AWS Credentials
* Under Compute, select EC2 to visit the EC2 Dashboard
* Select Launch Instance and create an instance with a GPU graphics card. I selected g3.8xlarge. Follow the configuration pages to setup. Add 20 GiB of storage, and make sure to select your existing key pair or create one.
* Your EC2 instance will launch after completing setup and the indicator will turn green once it is running.

##### Connect using terminal (MAC) or bash (Windows)
* Launch your instance using SSH and your terminal of choice
Example:
```
ssh -i "MyKeyPair.pem" ubuntu@ec2-18-220-74-31.us-east-2.compute.amazonaws.com
```
* Run the follow commands to update and upgrade the instance
```
sudo apt-get update && sudo apt-get upgrade
```





1. Connect to Your EC2 Instance using terminal(MAC) or Bash(Windows). Once you are in Logged in your E2C Instance. Follow the following commands.
2.sudo apt update
3.sudo apt upgrade
4.sudo apt-get install python3-pip
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
