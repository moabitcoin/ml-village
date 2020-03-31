# Installation
You can skip to the next step if you already have your account setup. Attach power to your workstation & hook it up with the ethernet cable. Boot your machine for the first time and you'd be asked to make an account. Select your username/password & machine name. You can use the machine name later to ssh into your workstation from your MacBook | Linux machine. Logout from OEM temporary account and login with your username / password. Check if your network is up
```
ping 8.8.8.8
```
Update & upgrade your package manager
```
sudo apt update
sudo apt-get -y upgrade
```

## Libs
Install the following useful libs and if you see something missing please do add them here. 
```
sudo apt-get install libcurl4-openssl-dev
sudo apt-get install libssl-dev
sudo apt install -y libkrb5-dev
sudo apt install -y libcublas-dev
sudo apt-get install libavutil-dev
sudo apt-get install libavcodec-dev
sudo apt-get install libavcformat-dev
sudo apt-get install libswscale-dev
sudo apt install cifs-utils
sudo apt-get install nfs-common
sudo apt install screen
sudo apt install awscli
sudo apt install htop
```

## SSH server
To be able to SSH into you machine you'd need to run the following on your workstation. 
```
sudo apt install ssh
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
sudo ufw allow ssh
```

##  Mounting storage
Your workstation has a 1T SSD (on which ubuntu is installed) & a 2T HDD which needs to mounted before you can use it. 
```
sudo mkdir /data
sudo chown <you-user-name>:<you-user-name> -R /data
sudo mount /dev/sda /data
df -h
```

## Mounting NAS
```
sudo mkdir /nas
sudo chown <user-name>:<user-name> -R /nas
sudo mount.cifs //perception_nas.local/data /nas -o user=<your-nas-user-name>,vers=1.0
# when prompted for password provide password
ls -a /nas
```
We haven't been able to successfully do this step. `Please help!`

## Setting up python/ipython
Install the minimal Python/IPython. Our recommendation is Python3.6.5 or later. 

```
sudo apt-get install -y python3-pip
sudo apt-get install -y ipython3
sudo ln -s /usr/bin/python3 /usr/bin/python
sudo ln -s /usr/bin/ipython3 /usr/bin/ipython
sudo ls -s /usr/bin/pip3 /usr/bin/pip
pip install -U pip
```
Optional : Install the following [useful python packages](https://gitlab.mobilityservices.io/am/roam/perception/data-catalogue/blob/master/requirements.txt)
```
curl -O https://gitlab.mobilityservices.io/am/roam/perception/data-catalogue/blob/master/requirements.txt
pip install -r requirements.txt
```

## Setting up Conda/Miniconda
Use virtual environments or docker images to keep your sanity. 

```
cd ~/Downloads
curl -O https://repo.anaconda.com/archive/Anaconda3-2019.03-Linux-x86_64.sh
sudo bash Anaconda3-2019.03-Linux-x86_64.sh
echo -e "\n# export local Conda binaries\nexport PATH=\$HOME/anaconda3/condabin:\$PATH" >> $HOME/.profile
```

## Identity file
You'd need to copy your RSA keys `~/.ssh/id_rsa.pub` to your workstation & add it to `~/.ssh/authorized_keys` to be able to ssh into the workstation without typing your password every time. Your RSA key is your signature use one consistent key for all logins (f.ex Gitlab) to keep sanity. 

```
ssh-copy-id -i ~/.ssh/id_rsa user@hostname
```

## Disable Password-based SSH Logins

Once your ssh key is on the machine disable password-based ssh logins for security reasons.

In `/etc/ssh/sshd_config` change `PasswordAuthentication` to `no` and then restart the openssh-server via `sudo service ssh restart`.

## TensorFlow
Your workstations comes with CUDA 10.1 pre-installed. CUDA 10.1 is not formally supported on TF for Ubuntu 18.04 :see_no_evil: so you'd need to run the following in a Conda environment to sleep peacefully and use TensorFlow. More information [here](https://github.com/tensorflow/tensorflow/issues/26182) and some [benchmarks](https://github.com/tensorflow/benchmarks) & [here](https://github.com/IntelAI/models/tree/master/benchmarks)
```
conda create -n me-working
conda activate me-working
conda install cudatoolkit
conda install cudnn
conda install tensorflow-gpu
```

Alternatively [here](https://dmitry.ai/t/topic/33) is recipe for downgrading to CUDA 10.0:

```
apt-get --purge remove "*cublas*" "cuda*"

reboot

wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/cuda-repo-ubuntu1804_10.0.130-1_amd64.deb
dpkg -i cuda-repo-ubuntu1804_10.0.130-1_amd64.deb
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu1804/x86_64/7fa2af80.pub
apt install cuda-10-0

reboot
```


## Docker it up

It's best to package up our apps and all its dependencies into a self-contained and reproducible docker image. For some example Dockerfiles and a convenience Makefile see e.g. [this project](https://gitlab.mobilityservices.io/am/roam/perception/prototypes/semantic-frame-index).

To be able to run docker without root privileges add yourself to the docker group and log out 

    sudo usermod -aG docker $USER

Then try

    docker run -it --rm hello-world


To push images

    docker image save das/sfi:gpu | bzip2 | ssh user@workstation 'bunzip2 | docker image load'

## Accessing workstation from home

On the workstation, get the shared ec2 ssh key. Then open a reverse ssh gateway to the ec2 instance.
Once the reverse ssh gateway is established traffic to the ec2 instance gets tunnled to your workstation allowing you to ssh into it from the outside.

```
# ssh into your workstation
ssh <username>@<workstation_name>
# get the shared ec2 key once and set permissions
cp /nas/team-space/AWS/mirco-instance/ec2-dl-playground.pem.txt ~/.ssh/id_rsa_ec2
chmod 400 ~/.ssh/id_rsa_ec2
```

Then for establishing the remote ssh gateway:

```
# ssh into your workstation
ssh <username>@<workstation_name>
# start a screen session 
screen -S remote-forward
# remote forward to our dedicated micro ec2 instances 
# chose port-number from 5001 to 5010 (see port list below)
ssh -R 5xxx:localhost:22 -o ServerAliveInterval=60 -i ~/.ssh/id_rsa_ec2 ec2-user@63.34.172.253
# leave screen session
# ssh out of workstation
```

Now to connect to your workstation from the outside (e.g. from home)

```
ssh -p 5xxx -i <your private key to login into your workstation> <username>@63.34.172.253
```

or for example for tunneling port 6006 (for tensorboard) from your workstation to your laptop

```
ssh -p <port-number> -L localhost:6006:localhost:6006 -i <your private key to login into your workstation> <username>@63.34.172.253
```

### Port & Hostnames
Assigned port list, user & hostnames (only available from `DAS_NW`)

Port               | User      | Hostname 
:-------------------------:|:-------------------------:|:-------------------------
5004 | Mo | mo-workstation
5005 | Dani | glados
5007 | Nico | marvin
5008 | Ilan | ilan-kingdom
5009 | Harsi | walle
5006 | Emna | hal

## Useful tools

Install following tools which are handy with GPU
```
sudo apt install cuda-nvml-dev-10-1
```
1. [nvtop](https://github.com/Syllo/)

## Perception team account
If your machine is idle (you are on [vacation](http://forgifs.com/gallery/d/80770-5/Kayak-pool-dive-fail.gif) or busy reading up [papers](https://66.media.tumblr.com/tumblr_m5ezckIAL81r2hf4ro1_500.gif)). To allow your peers to use your compute please make a `perception` account as follow

```
# ssh into your workstation
sudo adduser perception
sudo adduser -G docker perception
sudo mkdir /home/perception/.ssh
sudo chmod 0700 /home/perception/.ssh
sudo cp ~/.ssh/id_rsa.pub /home/perception/.ssh/authorized_keys
sudo chmod 0600 /home/perception/.ssh/authorized_keys
sudo chown -R perception /home/perception
```

Ask your peers for their public keys and add them to authorised keys as follow 

```
# ssh out of your workstation 
ssh-copy-id -i "<public-key-file>" perception@<machine_name>
```