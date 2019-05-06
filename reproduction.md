# Reproduction Instructions

Our source code can be found at:

https://github.com/Duuuuuu/Large-Scale-Distributed-Sentiment-Analysis-with-RNNs

## I. Data Preprocessing 

### I.1 MapReduce

#### Uploading Files to S3 Bucket

Create a new bucket with no special setting and upload the following files:

- `install_boto3_h5py.sh`: installs boto3 and h5py packages on each node in the cluster
- `complete.json`: raw data
- `mapper.py` and `reducer.py`: python files for the MapReduce task

#### Deploying CPU Cluster on AWS

Please go to EMR dashboard and select `Create cluster`, and then `Go to advanced options`.

- **Step 1: Software and Steps**: Choose `emr-5.8.0` Release and leave the rest as default

- **Step 2: Hardware**: At the bottom of the page, change instance type as `m4.xlarge` for Master and Core, and change to `8 Core Instances`, or however many needed

- **Step 3: General Cluster Settings**: Add `custom bootstrap action` by calling the script `install_boto3_h5py.sh`. This bash file will install boto3 and h5py packages on each node in the cluster. Could also rename cluster or enable logging, but these are not required

- **Step 4: Security**: Select `EC2 key pair` and create cluster

#### Running MapReduce

After the cluster is started and bootstrapped, go to `Steps` tab and `Add step`:

- **Step type**: `Streaming program`
- **Name**: optional
- **Mapper**: path to your mapper file, e.g.`s3://BucketName/mapper.py`
- **Reducer**: path to your reducer file, e.g.`s3://BucketName/reducer.py`
- **Input S3 location**: path to your data file, e.g.`s3://BucketName/complete.json`
- **Output S3 location**: input a non-existing folder name, e.g.`s3://BucketName/new_folder`


### I.2 Combine Generate h5 Files

#### Launching Instance

Please go to EC2 dashboard and select `Launch Instance`.

- **Step 1: Choose AMI**: Launch `Ubuntu Server 16.04 LTS (HVM), SSD Volume Type`

- **Step 2: Choose an Instance Type**: Choose `m4.2xlarge`

- **Step 3: Launch**: Launch with your own key pair, such as `course-key`.


#### Installing Essential Packages

After connecting to the instance, you need to first install boto3 and h5py with the following commands:

`sudo apt update`

`sudo apt install python-pip`

`pip install boto3`

`pip install h5py`

#### Modify Instance Volume

Go to the EC2 dashboard, select `Volumes`, and modify the instance volume to 64GB.

Run `sudo growpart /dev/xvda 1` on the instance and restart it.

If you run `df -h`, you should find that the disk space has been expanded.

#### Running Python File

Specify bucket name and the names of the files to be combined in `combine_h5.py`. You can find the file names from the S3 bucket page. You may also adjust the output file name.

Next, upload the python file to instance and run it with `python combine_h5.py`

#### Final Setting

Make the preprocessed h5 data file public, in order for the following process to access it with its `Object URL`.

Go to the S3 bucket, select the `Permissions` tab, and set all options under `Public access settings` to False.

Find the combined h5 file and `Make public` under the `Actions` tab.



## II. RNN with Distributed SGD
