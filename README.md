## Purpose

To make it easy for you to get started with split and combine large files, here's a list of steps. Including,

1. Split files steps
2. Combine files steps
3. Validate the file consistance 


## Split your files

Please replace your environment variables.

```
export FILE_NAME=15G.img
export REGION=us-east-1
export SOURCE_BUCKET=erica-splitfile-source-bucket
export SPLIT_TARGET_BUCKET=erica-splitfile-target-bucket


./s3_linux split --region=${REGION} --sourceBucketName=${SOURCE_BUCKET} --sourceFileName=${FILE_NAME} --targetBucketName=${SPLIT_TARGET_BUCKET} --targetFileName=${FILE_NAME}  > split.log 2>&1 &

# Please monitor the splitting status
vim split.log 

```

If needed, you can custom every file's size.

```

export FILE_NAME=15G.img
export REGION=us-east-1
export SOURCE_BUCKET=erica-splitfile-source-bucket
export SPLIT_TARGET_BUCKET=erica-splitfile-target-bucket
export PART_SIZE_IN_MB=200
export SPLITTED_FILE_SIZE_IN_MB=200
export PART_COUNT_IN_ONE_SPLITTED_FILE=0

./s3_linux split --region=${REGION} --sourceBucketName=${SOURCE_BUCKET} --sourceFileName=${FILE_NAME} --targetBucketName=${SPLIT_TARGET_BUCKET} --targetFileName=${FILE_NAME}  --splittedFileSizeInMB ${SPLITTED_FILE_SIZE_IN_MB} --partSizeInMB ${PART_SIZE_IN_MB}  --partCountInOneSplittedFile ${PART_COUNT_IN_ONE_SPLITTED_FILE} > split.log 2>&1 &

```

## Combine your files

Please replace your environment variables.

```
export COMBINE_FILE_NAME=15G-combine.img
export SPLIT_FILE_NAME=15G.img
export REGION=us-east-1
export SPLIT_TARGET_BUCKET=erica-splitfile-target-bucket
export COMBINE_TARGET_BUCKET_NAME=erica-splitfile-source-bucket


./s3_linux combine --region=${REGION} --sourceBucketName=${SPLIT_TARGET_BUCKET} --sourceFileName=${SPLIT_FILE_NAME} --targetBucketName=${COMBINE_TARGET_BUCKET_NAME} --targetFileName=${COMBINE_FILE_NAME}  > combine.log 2>&1 &


# Please monitor the splitting status
vim combine.log 

```


## Validate the file consistance 

Comparing md5 or E-tag for the source and combined files. 

1. md5 comparing 

```
md5sum 15G-combine.img

```

2. E-tag comparing 

Please replace your environment variables.

```

aws s3api head-object --bucket ${SOURCE_BUCKET} --key ${SPLIT_FILE_NAME} --checksum-mode ENABLED --region=${REGION} | jq .ETag

aws s3api head-object --bucket ${COMBINE_TARGET_BUCKET_NAME} --key ${COMBINE_FILE_NAME} --checksum-mode ENABLED --region=${REGION} | jq .ETag

```

