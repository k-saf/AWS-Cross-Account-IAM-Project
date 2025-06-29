#Download and install the latest version of the AWS CLI
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

#test its been properly installed
aws --version

#configure aws cli for the RF_Engineer IAM user
aws configure --profile RF_Engineer
#enter Access key id, secret key, region and output format

#assume the role
aws sts assume-role \
  --role-arn arn:aws:iam::<DEV_ACCOUNT_ID>:role/RF_Engineer_Role_Weekends  \
  --role-session-name testRF \
  --profile RF_Engineer \
  --tags Key=Team,Value=RF

  #verify role assumption worked
  aws sts get-caller-identity

#export the temporary credentials
export AWS_ACCESS_KEY_ID=<copied AccessKeyId>
export AWS_SECRET_ACCESS_KEY=<copied SecretAccessKey>
export AWS_SESSION_TOKEN=<copied SessionToken>

#run the ABAC tests
aws ec2 describe-instances --region us-east-1
aws ec2 reboot-instances --instance-ids <RF-instance-id>
