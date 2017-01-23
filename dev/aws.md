# AWS

## s3

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
