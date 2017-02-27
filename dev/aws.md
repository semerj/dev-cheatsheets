# AWS

## [AWS CLI](https://docs.aws.amazon.com/cli/latest/reference/index.html#cli-aws)

[Inspect AWS credentials](https://docs.aws.amazon.com/cli/latest/reference/configure/index.html) (access/secret keys should be visible through EC2 IAM role):

```sh
$ aws configure list
```

## [S3](https://docs.aws.amazon.com/cli/latest/reference/s3/index.html)

Show files in bucket:

```sh
$ aws s3 ls s3://BUCKET
```

Copy file:

```sh
$ aws s3 cp file.txt s3://BUCKET/
```

Copy S3 bucket to local:

```sh
$ aws s3 cp s3://PATH/TO/BUCKET/ . --recursive
```

Create new bucket:

```sh
$ aws s3 mb s3://BUCKET
```

Check the amount of disk space in bucket:

```sh
$ aws s3 ls s3://<bucketname> --recursive  | grep -v -E "(Bucket: |Prefix: |LastWriteTime|^$|--)" | awk 'BEGIN {total=0}{total+=$3}END{print total/1024/1024" MB"}'
```

## [EC2](https://docs.aws.amazon.com/cli/latest/reference/ec2/index.html)

Launch Tensorflow from Docker container on EC2 free tier instance (`t2.micro`):

1. [Launch an instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/launching-instance.html) and log in.

  ```sh
  $ ssh -i <PEM_FILE>.pem ec2-user@<INSTANCE_DNS>
  ```

2. Update instance and install Docker.

  ```sh
  $ sudo yum update -y && sudo yum install -y docker
  ```

3. Start Docker service.

  ```sh
  $ sudo service docker start
  ```

4. Add `ec2-user` to `docker` group so you can execute Docker commands without `sudo`.

  ```sh
  $ sudo usermod -a -G docker ec2-user
  ```

5. Log out and log back in using an SSH tunnel to (1) pick up the new `docker` group permissions and (2) access `jupyter` notebooks from local machine.

  ```sh
  $ ssh –L localhost:8888:localhost:8888 –i <PEM_FILE>.pem ec2-user@<INSTANCE_DNS>
  ```

6. Run Tensorflow Docker image on AWS instance.

  ```sh
  $ docker run -it -p 8888:8888 tensorflow/tensorflow:latest-py3
  ```

### EBS

[Make an EBS volume available for use on EC2 Linux instance](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html):

1. Use `lsblk` to view available disk devices and their mount points (output of `lsblk` removes the `/dev/` prefix from full device paths).

  ```sh
  $ lsblk
  ```

2. Determine whether you need to create a file system on the volume. If the output of the command below shows simply data for the device, then there is no file system on the device and you need to create one.

  ```sh
  $ sudo file -s <DEVICE_NAME>  # e.g. /dev/xvdf
  ```

3. (Conditional) Create an ext4 file system on the volume. This step assumes that you're mounting an empty volume.

  ```sh
  $ sudo mkfs -t ext4 <DEVICE_NAME>
  ```

4. Create a mount point directory for the volume.

  ```sh
  $ sudo mkdir <MOUNT_POINT>  # e.g. /data
  ```

5. Mount the volume at the location you just created.

  ```sh
  $ sudo mount <DEVICE_NAME> <MOUNT_POINT>
  ```

6. (Optional) To mount this EBS volume on every system reboot, see [AWS docs](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ebs-using-volumes.html)

7. Review the file permissions of your new volume mount to make sure that your users and applications can write to the volume.

8. To mount EBS volume in Docker container, use `-v` flag.

  ```sh
  $ docker run -it -p 8888:8888 -v /data:/data tensorflow/tensorflow:latest-py3 bash
  ```

Attach an existing EBS snapshot to EC2 instance:

```sh
$ sudo mkdir /data
$ sudo mount /dev/xvdf /data
```

[Describe volumes on instance](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-volumes.html):

```sh
$ aws ec2 describe-volumes --region us-west-1
{
    "Volumes": [
        {
            "AvailabilityZone": "us-west-1c",
            "Attachments": [],
            "Encrypted": true,
            "VolumeType": "gp2",
            "VolumeId": "vol-xxxxxxxxxxxxxxxxx",
            "State": "available",
            "Iops": 100,
            "KmsKeyId": "arn:aws:kms:us-west-1:xxxxxxxxxxxx:key/xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
            "SnapshotId": "snap-xxxxxxxxxxxxxxxxx",
            "CreateTime": "2017-02-26T00:00:00.000Z",
            "Size": 1
        }
    ]
}
```

[Create volume snapshot](https://docs.aws.amazon.com/cli/latest/reference/ec2/create-snapshot.html) (get `volume-id` from `describe-volumes`):

```sh
$ aws ec2 create-snapshop --volume-id vol-xxxxxxxxx --description "creating snapshot" --region us-west-1
```
