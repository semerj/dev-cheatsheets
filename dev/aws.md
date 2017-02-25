# AWS

## S3

show files in bucket

```sh
$ aws s3 ls s3://BUCKET
```

copy file

```sh
$ aws s3 cp file.txt s3://BUCKET/
```

copy s3 bucket to local

```sh
$ aws s3 cp s3://PATH/TO/BUCKET/ . --recursive
```

create new bucket

```sh
$ aws s3 mb s3://BUCKET
```

check the amount of disk space in bucket

```sh
aws s3 ls s3://<bucketname> --recursive  | grep -v -E "(Bucket: |Prefix: |LastWriteTime|^$|--)" | awk 'BEGIN {total=0}{total+=$3}END{print total/1024/1024" MB"}'
```

## EC2

### EC2 + Docker + Tensorflow + Jupyter Notebooks

Launch Tensorflow within Docker on EC2 free tier instance (`t2.micro`)

1. after [launching an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html), log in:

  ```sh
  ssh -i <PEM_FILE>.pem ec2-user@<INSTANCE_DNS>
  ```

2. update instance and install Docker

  ```sh
  sudo yum update -y && yum install -y docker
  ```

3. start Docker service

  ```sh
  sudo service docker start
  ```

4. add `ec2-user` to `docker` group so you can execute Docker commands without `sudo`

  ```sh
  sudo usermod -a -G docker ec2-user
  ```

5. log out and log back in using an SSH tunnel to (1) pick up the new `docker` group permissions and (2) access `jupyter` notebooks from local machine

  ```sh
  ssh –L localhost:8888:localhost:8888 –i <PEM_FILE>.pem ec2-user@<INSTANCE_DNS>
  ```

6. run Tensorflow Docker image on AWS instance

  ```sh
  docker run -it -p 8888:8888 tensorflow/tensorflow:latest-py3
  ```
