# AWS

show files in s3 bucket
```bash
$ aws --region us-west-1 s3 ls s3://<bucketname>
```
push local file to s3 bucket
```bash
$ aws --region us-west-1 s3 cp index.html s3://<bucketname>/index.html
```
copy aws directory to local
```bash
$ aws --region us-west-1 s3 cp s3://<bucketname>/<directory>/ . --recursive
```
create new aws bucket
```bash
$ aws --region us-west-1 s3 mb s3://<newbucketname>
```