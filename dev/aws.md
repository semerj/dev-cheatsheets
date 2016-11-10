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
