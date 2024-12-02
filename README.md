
# Building and Training a Distributed Large Language Model (LLM) with Spark and AWS EMR

**Demo Video**: https://youtu.be/H-hUezj_hX0

**Author**: Bhavya Batta

**Email**: bbatta@uic.edu



## Introduction

This project is the second part of a three-part series in CS441, where we develop a Large Language Model (LLM) from scratch. This assignment focuses on training the decoder for the LLM using Spark and a neural network library in a cloud-based computational environment, specifically on AWS EMR. This trained model will later be used to generate text in response to client queries. Building on our previous work, we will leverage distributed processing to train the model with an attention mechanism, a key component for effective LLMs.

---

## Environment

- **Operating System**: Mac
- **IDE**: IntelliJ IDEA 2022 (Ultimate Edition)
- **Scala Version**: 2.13.12
- **SBT Version**: 1.8.3
- **Hadoop Version**: 3.3.6
- **Distributed Platform**: Spark on AWS EMR

---

## Project Overview

### Workflow

1. **Tokenization**: Using Byte Pair Encoding (BPE), tokenized data generated in Homework 1 will be transformed into sliding windows.
2. **Vector Embedding**: A Word2Vec model is applied to generate embeddings, adding positional embeddings as required.
3. **Attention Mechanism**: Implements an attention mechanism with the sliding window data to predict subsequent tokens.
4. **Training**: Utilizes Spark and DL4J to train the LLM on the AWS EMR cluster.
5. **Text Generation**: The trained model will later be able to generate text based on client queries.

---

## Running the Project

### Setting Up the Environment

Ensure that the following components are installed and configured:

- **IntelliJ IDE** with Scala plugin
- **Java Development Kit** (JDK 8-22)
- **Apache Hadoop** (v3.3.6)
- **AWS CLI** for EMR integration

### Configurations

All configuration parameters should be specified in `application.conf` using the Typesafe Configuration Library. Sample parameters include:

```hocon
# Paths
inputFilePath = "/local/path/to/input"
outputFilePath = "/local/path/to/output"
modelPath = "/local/path/to/model.zip"

# AWS S3 Paths
s3InputPath = "s3a://bucket-name/input"
s3OutputPath = "s3a://bucket-name/output"
s3ModelPath = "s3a://bucket-name/model.zip"

# Spark Configuration
spark.appName = "Distributed LLM Training"
spark.master = "yarn"
spark.executor.memory = "4g"
spark.driver.memory = "4g"

# Sliding Window Parameters
windowSize = 15
overlap = 7
```

### Building the Project

To compile and package the project, run:
```
sbt clean compile
sbt assembly
```
Running Tests
```
Test files are located in src/test. Run tests with:
sbt test
```
## Project Components

### Sliding Window Generation

The sliding window algorithm generates training samples with positional embeddings. This 3D tensor structure (batch size, sequence length, embedding size) is optimized for parallel processing on Spark.

### Word2Vec and Attention Mechanism

Uses Word2Vec for vector embeddings and integrates an attention mechanism to improve prediction accuracy. Spark processes sliding windows across a distributed cluster, leveraging parameter averaging or shared parameter server for distributed model training.

### AWS EMR Deployment

This project is designed to be executed on AWS EMR for scalable processing. Setup instructions for EMR deployment:

	1.	Create an EMR Cluster with Spark.
	2.	Upload JAR and configuration files to HDFS.
	3.	Submit the job using spark-submit:
```
spark-submit \
  --class MainApp \
  --master yarn \
  --deploy-mode cluster \
  s3a://bucket-name/path-to-jar.jar
```
### Usage

To execute the project, follow these steps:

	1.	Data Preparation: Ensure input text files are correctly formatted and placed in the specified input directory.
	2.	Tokenization: Execute the sliding window generation job.
	3.	Training: Run the training job on EMR, using the configurations in application.conf.
	4.	Result Analysis: Check output files in the specified output directory.

### Results and Output

The program outputs:

	1.	Model File: Saved as a .zip file.
	2.	Statistics File: Includes gradient norms, training loss, accuracy, and time per epoch.
	3.	Generated Text: The trained model is used to generate text based on a seed query.

### Logging and Configuration

	•	Logging: Uses SLF4J with Logback to manage logging levels (TRACE, INFO, WARN, ERROR).
	•	Configuration: Parameters are managed through the Typesafe Configuration Library to ensure maintainability and flexibility.


## Conclusion

This project demonstrates the use of Spark and AWS EMR for distributed training of a large language model. It provides hands-on experience with cloud-based neural network training, focusing on scalability and efficiency using Spark’s distributed framework.
