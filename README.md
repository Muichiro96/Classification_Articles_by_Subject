# Article Classification with Apache Mahout

This project demonstrates how to classify articles using Apache Mahout. The steps include data transformation, TF-IDF vectorization, dataset splitting, model training, and testing with the Naive Bayes classifier.

---

## Prerequisites

- **Apache Mahout** installed on your system.
- **Hadoop** configured and running (if using HDFS).
- A dataset organized into folders, where each folder represents a category.

---

## Process Steps

### 1. Convert data to sequence format
Organize your data into directories, where each directory represents a category. Use the following command to convert the data into a sequence format that Mahout can process:

```bash
mahout seqdirectory -i Articles_data -o Articles_data_seq
```

### 2. Vectorize data using TF-IDF
Transform the sequential data into sparse vectors using TF-IDF weighting:

```bash
mahout seq2sparse -i Articles_data_seq -o Articles_data_vectors -lnorm -nv -wt tfidf
```

### 3. Split data into training and testing sets
Split the vectorized data into training and testing datasets, with 25% reserved for testing:

```bash
mahout split -i Articles_data_vectors/tfidf-vectors \
  --trainingOutput Articles-train-vectors \
  --testOutput Articles-test-vectors \
  --randomSelectionPct 25 \
  --overwrite \
  --sequenceFiles -xm sequential
```

### 4. Upload data to HDFS (if applicable)
If using Hadoop, upload the project data to HDFS:

```bash
hadoop fs -put projet/ /projet-big-data/
```

### 5. Train the Naive Bayes model
Train the Naive Bayes classifier using the training dataset:

```bash
mahout trainnb -i hdfs://localhost:9000/projet-big-data/projet/Articles-train-vectors \
  -o hdfs://localhost:9000/projet-big-data/projet/model \
  -li hdfs://localhost:9000/projet-big-data/projet/labelindex \
  -ow -c
```

### 6. Test the model
Evaluate the model on the testing dataset and output the results:

```bash
mahout testnb -i hdfs://localhost:9000/projet-big-data/projet/Articles-test-vectors \
  -m hdfs://localhost:9000/projet-big-data/projet/model \
  -l hdfs://localhost:9000/projet-big-data/projet/labelindex \
  -ow -o hdfs://localhost:9000/resultats_model
```

---

## Outputs

![metrics](https://github.com/user-attachments/assets/26ade644-fbc7-4d41-8641-517def697a4e)


