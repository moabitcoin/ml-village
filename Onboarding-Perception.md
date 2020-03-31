# Onboarding - Perception

Hey ! Hey Welcome to our team :) So glad you join us in our adventure to build technology powering the [future of mobility](https://66.media.tumblr.com/tumblr_mdv51hvvCe1ry1tp9.gif). Here’s all the information you’d need to get started. Incase you are blocked, feel free to ping your [buddy](https://i.chzbgr.com/maxW500/7333828608/hE3E4FEA4/) or your teammates - [Say hi!](https://66.media.tumblr.com/tumblr_mcg6eeCije1ry8teo.gif) ! But first, add your [photo](https://i.imgur.com/jZhsTc7.gif) in the table below :fireworks: 

## Your Team 

Mo                | Dani      | Harsi | Emna | Francesco | Nicolai |
:-------------------------:|:-------------------------:|:-------------------------:|:-------------------------|:-----------------------------:|:-----------------------------
<img src="https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/raw/master/imgs/mo.png" width="480">  |  <img src="https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/raw/master/imgs/dani.jpeg" width="480"> | <img src="https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/raw/master/imgs/harsi.jpg" width="480"> | <img src="https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/raw/master/imgs/emna.jpg" width="480"> | <img src="https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/raw/master/imgs/avatar.png" width="480"> | <img src="https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/raw/master/imgs/nico.jpeg" width="480"> |

## Your Workstation
You have your very one ML workstation ! Ask your buddy to provide you with your credentials (which you should change ASAP). Only you have access to your workstation. Each workstation is fitted with at least 1 Titan V which is yours for [running inference/training locally](https://66.media.tumblr.com/tumblr_mdv51hvvCe1ry1tp9.gif). For training on larger datasets we have AWS credentials for you (more on this below), [ping your buddy](https://66.media.tumblr.com/tumblr_m5ezckIAL81r2hf4ro1_500.gif) for more information. Workstations are pre-installed with 1. `Ubuntu 18.04` 2. `Cuda 10.1` 3. `v418.67` Nvidia Drivers. You'll have to install standard libraries and python packages before starting running cool stuff. Follow the [instructions](https://gitlab.mobilityservices.io/am/roam/perception/team-space/cv-ml-resources/wikis/Workstation-installation) here to get started. 

## Our perception@gitlab
All perception related repositories are [here](https://gitlab.mobilityservices.io/am/roam/perception). If you can read this you already have access to our repositories:) A good place to start is our data [catalogue](https://gitlab.mobilityservices.io/am/roam/perception/data-catalogue). If you need access to our drive data you can use scripts within the [catalogue](https://gitlab.mobilityservices.io/am/roam/perception/data-catalogue). As our data sources keep growing, we'd like to make sure everyone has access to all the datasets they need :dancer: . If you plan to use a [public dataset](https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources#datasets) please discuss with your team member regarding the LICENSE. 

## Your AWS account

Its easy to work your AWS account with `aws cli` set up both on your workstation and your notebook
For Unix machine skip the steps below. For Mac you'd need to [jump through few hoops](https://66.media.tumblr.com/tumblr_mcythlotza1ry8teo.gif) before setting up awscli. 

Install `developer tools` + `Homebrew` + `python-pip` on MacBook. Skip this step for your workstation.
```
/usr/bin/xcode-select --install
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
brew install python
```
Install [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/index.html)
```
# Both Unix and Mac
pip install awscli
```

Follow the instructions in the [infrastructure wiki](https://gitlab.mobilityservices.io/infra/infra-docs/wikis/FAQ#aws) to login to AWS with KeyCloak.

Now test your AWS set-up
```
# Both Unix and Mac
aws s3 ls s3://das-perception/perception_onboarding/ --profile DASPerceptionDev

2019-04-12 16:18:49      16756 dani.jpeg
2019-04-12 16:16:36     278860 harsi.jpg
2019-04-12 16:18:39     135149 mo.png
2019-04-12 16:18:24          6 team.txt
```

## Our S3 Buckets

Location                | Usage      | Example
:-------------------------|:-------------------------|:-------------------------
s3://das-perception/perception_data | Drive Data/Open source Data | Videos, GPS, Lidar
s3://das-perception/perception_experiments | Training Data/Docker Images | tf-records etc
s3://das-perception/perception_logs | Logging your experiments | tensorflow summary events 
s3://das-perception/perception_models | Pre-trained models/your models | ResNet101 pre-trained on ImageNet

Now you can push data to S3 and start [training your models](http://s.pikabu.ru/post_img/2013/04/07/7/1365327582_998102211.gif) ! Need an 8 GPU machine ? [Here's](https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/wikis/cloud-compute) how to get one !

## Our Dockers ourselves 
Our gitlab repositories have their dedicated docker registry. F.ex on the left side panel (after expanding) you'll   see `Registry`. You can [build and push docker](https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/container_registry) images (tagged obviously) to the registry. 

For accessing the registry follow the instructions below Here you can find [instructions](https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/container_registry) on accessing gitlab docker registry from you workstation or an AWS machine. You'd need to setup an [personal access token](https://gitlab.mobilityservices.io/help/user/profile/account/two_factor_authentication#personal-access-tokens) and run the following for the first time

Clone this repo 
```
git clone ssh://git@ssh.gitlab.mobilityservices.io:443/am/roam/perception/cv-ml-resources.git
git checkout docker-demo
```
Get your [personal access token](https://gitlab.mobilityservices.io/help/user/profile/account/two_factor_authentication#personal-access-tokens) and save it at a secure location. You can use this token from your workstation or MacBook. When prompted for password below provide your personal access token. 
```
docker login registry.mobilityservices.io -u <your_gitlab_username>
```

Build your docker image and push it to our registry as below. You can tag your docker image `latest`. Best practice would be to use tag from the commit history as `commit_tag`.
```
# We use the following conventions for our docker containers.
# docker build -t registry.mobilityservices.io/am/roam/perception/<repository>/<branch>:<commit_tag> .
docker build -t registry.mobilityservices.io/am/roam/perception/cv-ml-resources/docker-demo:<your_name> .
docker push docker push registry.mobilityservices.io/am/roam/perception/cv-ml-resources/docker-demo:<your_name>
```
You can see the docker file in our registry [here](https://gitlab.mobilityservices.io/am/roam/perception/cv-ml-resources/container_registry).

## Our [Coding Conventions](http://i.imgur.com/rnU38.gif)
Within python environment, we ([try to](https://i.imgur.com/W4oNHsD.gif)) follow [PEP8](https://www.python.org/dev/peps/pep-0008/) coding conventions [wherever possible](https://66.media.tumblr.com/tumblr_mdcdp6BhhW1ry1tp9.gif). Easiest way to enforce this into your favourite text editor is to install a PEP8 [package/plugins](https://pypi.org/project/pycodestyle/). Inconsistent coding practices between team members can lead to messy PRs (tabs v spaces etc) and [code reviews](https://66.media.tumblr.com/tumblr_meqd87LELl1ry1tp9.gif). Here are the detailed [`guidelines`](https://gitlab.mobilityservices.io/am/roam/perception/team-space/cv-ml-resources/wikis/Python-coding-conventions)

## Our NAS storage
We have our own Network Attached Storage (NAS). This is one stop location for accessing all the drive data, annotations, results, experiments etc. You have an account to mount the drive onto your workstation and/or MacBook. You'd need to connect to our dedicated Wifi: `DAS_NW`. Pwd : `N85qaDL9uXeKm5RG26m7TZNdErSLSb2ZB73T`

For Macbook : 
Connect to our NAS with a ethernet cable. 
```
Finder -> Network -> perception_nas -> connect (top left) -> Enter credentials and wait a bit
```

From Linux (CLI Only) : Follow the following [steps](https://gitlab.mobilityservices.io/am/roam/perception/team-space/cv-ml-resources/wikis/Workstation-installation#mounting-nas)

You can also mount the NFS share from the command line; if you do this you are on your own.


## Our Roadmap
You can find our mission statement and roadmap [here](https://gitlab.mobilityservices.io/am/roam/perception/team-space/perception/blob/team/README.md)
## Your first day
* [ ] Team Photo : Add your photo above
* [ ] Gitlab : Get access ping @peter
* [ ] Meet the team : #das-perception slack channel
* [ ] Workstation Setup : Login and setup 
* [ ] AWS Setup : Credentials + your EBS storage

## Your first week
* [ ] Intro to Drive Data catalogue
* [ ] Aloha @ OKRs : Align progress 
* [ ] Hey - Your first task :) 
* [ ] Meet and Greet - Mapping
* [ ] Say Hi to Routing
* [ ] Salut Simulation 

## Your first month
* [ ] Solve Computer Vision :dancer\_tone2: 
* [ ] Help us improve the on-boarding wiki :)