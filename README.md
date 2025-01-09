# Spark Setup Guide on AWS EC2
### This guide provides step-by-step instructions to set up Apache Spark on AWS EC2 with one master node and one worker node.

## Master Node Setup
### 1. Install Java
Update the system and install Java:
```bash
sudo yum update -y
sudo yum install -y java-11-amazon-corretto 
```
### 2. Download and Install Spark
Download Spark:
```bash
wget https://archive.apache.org/dist/spark/spark-3.5.2/spark-3.5.2-bin-hadoop3.tgz
```
Extract the downloaded file:
```bash
tar xvf spark-3.5.2-bin-hadoop3.tgz
```
Move Spark to the /opt directory:
```bash
sudo mv spark-3.5.2-bin-hadoop3 /opt/spark
```

### 3. Set Environment Variables
Set the necessary environment variables:
```bash
echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
echo "export PATH=\$PATH:\$SPARK_HOME/bin" >> ~/.bashrc
echo "export JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64" >> ~/.bashrc
source ~/.bashrc
```

### 4. Configure Spark (Master Node)
Navigate to Spark configuration directory:
```bash
cd $SPARK_HOME/conf
```
Copy the template for the Spark environment file:
```bash
cp spark-env.sh.template spark-env.sh
```
Edit the spark-env.sh file:
```bash
nano spark-env.sh
```

Add the following line:
```bash
export SPARK_MASTER_HOST='<Master-Node-Private-IP>'
```
### 5. Start the Spark Master Node
Start the Spark master node:
```bash
$SPARK_HOME/sbin/start-master.sh
```
### 6. Install AWS CLI
Install the AWS CLI to manage datasets:
```bash
sudo yum install awscli -y
```
### 7. Create a Directory for Data
Create a directory to store your data:

```bash
mkdir -p /home/ec2-user/data
```
8. Download Data from S3
Download the dataset from Amazon S3:

```bash
aws s3 cp s3:put_your_dataset_path_in_s3
```
 for example: aws s3 cp s3://amazon-bucket23/amazon_reviews.csv /home/ec2-user/data 

 
### 9. Start Spark Shell
Start the interactive Spark shell:


```bash
$SPARK_HOME/bin/spark-shell
```
## Notes

Ensure that you replace <Master-Node-Private-IP> with the private IP address of your master EC2 instance.


Use the Spark Web UI
```bash
http://<Master-Node-IP>:8080
```
 to monitor your cluster.
---
## Worker Node Setup


To complete the Spark cluster, the worker nodes need to be set up as follows:

### 1. Install Java
Update the system and install Java:

```bash
sudo yum update -y
sudo yum install -y java-11-amazon-corretto
```
### 2. Download and Install Spark
Download Spark:

```bash
wget https://archive.apache.org/dist/spark/spark-3.5.2/spark-3.5.2-bin-hadoop3.tgz
```
Extract the downloaded file:
```bash
tar xvf spark-3.5.2-bin-hadoop3.tgz
```
Move Spark to the /opt directory:
```bash
sudo mv spark-3.5.2-bin-hadoop3 /opt/spark
```
### 3. Set Environment Variables
Configure the environment variables for Spark and Java:

```bash
echo "export SPARK_HOME=/opt/spark" >> ~/.bashrc
echo "export PATH=\$PATH:\$SPARK_HOME/bin" >> ~/.bashrc
echo "export JAVA_HOME=/usr/lib/jvm/java-11-amazon-corretto.x86_64" >> ~/.bashrc
source ~/.bashrc
```
### 4. Start the Worker Node
Start the worker node:

```bash
$SPARK_HOME/sbin/start-slave.sh spark://<Master-Node-Private-IP>:7077
```
Replace <Master-Node-Private-IP> with the private IP address of the Spark master node.

Verify that the worker node is successfully connected to the master by accessing the Spark Web UI at:

```bash
http://<Master-Node-IP>:8080
```
---
### Verification

The worker node's status should appear as ALIVE on the Spark Web UI.
This indicates that the master and worker nodes are successfully communicating.
