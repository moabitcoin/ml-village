Here you will find instructions on how to boot-up a machine with a cloud compute vendor. We currently use AWS as our cloud compute provider. First you'd need AWS credentials. Ping your buddy to set them up for you. Once you have logged into AWS console follow the instructions below 

1. Allocate [EBS storage](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-creating-volume.html) for your machine. EBS is your personal storage which you can mount to any machine you boot now and in the future without losing your work. 

2. Boot up a GPU machine you need following the instructions [here](https://aws.amazon.com/getting-started/tutorials/get-started-dlami/). AWS will ask you to create a new key-pair for SSH logins into your machine. Keep it handy for future or reuse if you already have one.  

3. [Attach](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html) your EBS storage on your machine. [Mount](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html) your EBS volume

TL;DR version
```
ssh -L localhost:8888:localhost:8888 -i <your key-pair>.pem.txt ubuntu@<Public DNS of your machine>
# Execute mkfs only for the first time you initialise your storage
sudo mkfs -t xfs /dev/xvdf
# here we mount your EBS volume on /data but you can chose 
sudo mkdir /data
sudo mount /dev/xvdf /data
df -h | grep /dev/xvd
/dev/xvda1      75G   75G   71G   94% /
/dev/xvdf       500G  298G  203G  60% /data
```

4. To write to our S3 buckets this function comes in handy. Add it to your bashrc file
```
setAWSidentity() {
  export AWS_REGION=eu-west-1
  export S3_VERIFY_SSL=1
  export S3_USE_HTTPS=1
  export S3_ENDPOINT=s3.eu-west-1.amazonaws.com
  export AWS_SECRET_ACCESS_KEY=`aws configure get aws_secret_access_key --profile $1`
  export AWS_ACCESS_KEY_ID=`aws configure get aws_access_key_id --profile $1`
}
source ~/.bashrc
setAWSidentity perception
```

5. Your machine is ready for use. To prevent loss of work, have your code/data in /data. 