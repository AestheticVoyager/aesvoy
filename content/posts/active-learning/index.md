---
title: "Active Learning"
date: 2025-05-09
draft: false
summary: "Active learning is a powerful technique that can help us automate the labeling process for large datasets. By selecting a subset of the data that is most relevant to the task at hand, active learning can be more efficient than manually labeling every example in a dataset. This can lead to better results and more accurate predictions. In this blog post, I'll walk through the concept of active learning, how it works, and share a step-by-step implementation of how to automate dataset labeling for a text classification task using this method."
tags: ["machine learning", "data science", "active learning", "text classification", "image classification", "time series classification", "Python", "classification"]
---

# Active Learning

A few years ago, training AI models required massive amounts of labeled data. Manually collecting and labeling this data was both time-consuming and expensive. But thankfully, we’ve come a long way since then, and now we have much more powerful tools and techniques to help us automate this labeling process. One of the most effective ways? 

**Active Learning**.

In this blog post, I'll walk through the concept of active learning, how it works, and share a step-by-step implementation of how to automate dataset labeling for a text classification task using this method.

## What is Active Learning?

Active learning is a machine learning technique that allows us to automatically label data that is not labeled by humans. Instead of labeling every example in a dataset, active learning focuses on a small subset of the data and uses it to train a model. As the model learns from the unlabeled data, it can then be used to predict labels for the remaining data.

The idea behind active learning is that it can be more efficient than manually labeling every example in a dataset. By focusing on a small subset of the data, the model can learn from the unlabeled data more quickly and accurately, leading to better results.

## How does Active Learning Work?

Active learning works by selecting a subset of the data that is most relevant to the task at hand. This subset is then used to train a model, which can then be used to predict labels for the remaining data. The process is repeated until all the data is labeled.

There are two main types of active learning:

1. **Active Learning with Labeled Data**: In this approach, the model is trained on a small subset of the data that is labeled by humans. The model learns from this labeled data and can then be used to predict labels for the remaining unlabeled data.

2. **Active Learning without Labeled Data**: In this approach, the model is trained on a small subset of the data that is not labeled by humans. The model learns from this unlabeled data and can then be used to predict labels for the remaining labeled data.

## Key Concepts in Active Learning

Let's quickly go over some of the key concepts in active learning:

- **Labeled Data**: This refers to the data that has already been labeled by humans. It is used to train the model and is used to predict labels for the remaining unlabeled data.

- **Unlabeled Data**: This refers to the data that has not been labeled by humans. It is used to train the model and is used to predict labels for the remaining labeled data.

- **Labeling Oracle**: The external source or human expert who provides lables for the selected data points. This is used to train the model and is used to predict labels for the remaining unlabeled data.

- **Query Strategy**: The strategy used to select the data points that are most relevant to the task at hand. This can be based on various criteria such as the frequency of labeling, the relevance of the data points to the task, or the diversity of the data points. Common strategies include:
    - **Uncertainty Sampling**: Selects the instances where the model is most uncertain about the labels.(i.e., where the model has prediction with high entropy)

    - **Random Sampling**: Randomly selects data points from the unlabeled data.

    - **Diversity Sampling**: Chooses samples that are diverse from the existing labeled data to improve coverage of the feature space. 

    - **Query-by-committee**: A group of experts or users provide labels for the selected data points. Uses multiple models to vote on samples where disagreement is highest.

    - **Expected Model Change**: Identifies samples that would cause the greatest change to the current model parameters if labeled.

    - **Expected Error Reduction**: Selects samples that would minimize expected error on the unlabeled data pool.


## Step-by-Step Implementation of Active Learning

Let's walk through the step-by-step process of implementing active learning for a text classification task using Python.

### Step 1: Define the Data

First, we need to define the data that we want to use for active learning. This data should include the text of the documents, as well as the labels that we want to predict. For example, if we're classifying movie reviews as positive or negative, we would have a dataset with the text of the reviews and the corresponding labels.

```python
import pandas as pd

# Define the data
data = {
    'text': ['This movie was amazing!', 'This movie was terrible!', 'This movie was okay.'],
    'label': ['positive', 'negative', 'positive']
}

df = pd.DataFrame(data)
```

### Step 2: Define the Model

Next, we need to define the model that we want to use for active learning. This model should be able to take in the text of the documents and predict the corresponding labels. For example, if we're using a simple text classification model like Naive Bayes, we would define the model as follows:

```python
from sklearn.naive_bayes import MultinomialNB

# Define the model
model = MultinomialNB()
```

### Step 3: Define the Active Learning Algorithm

Now, we need to define the active learning algorithm that we want to use. This algorithm should be able to select a subset of the data that is most relevant to the task at hand. For example, if we're using the Least-Squares method, we would define the algorithm as follows:

```python
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import LeaveOneOut

# Define the active learning algorithm
algorithm = LeaveOneOut()
```

### Step 4: Train the Model

Finally, we can train the model using the active learning algorithm. This involves selecting a subset of the data that is most relevant to the task at hand, training the model on this subset, and then using the model to predict labels for the remaining data.

```python
# Train the model
for train_index, test_index in algorithm.split(df):
    X_train = df.loc[train_index, 'text']
    y_train = df.loc[train_index, 'label']
    X_test = df.loc[test_index, 'text']
    y_test = df.loc[test_index, 'label']
    model.fit(X_train, y_train)
    predictions = model.predict(X_test)
    print(f"Accuracy: {model.score(X_test, y_test)}")
```

### Step 5: Evaluate the Model

After training the model, we can evaluate its performance using the test data. This involves predicting the labels for the remaining data and comparing them to the true labels.

```python
# Evaluate the model
X_test = df.loc[test_index, 'text']
y_test = df.loc[test_index, 'label']
predictions = model.predict(X_test)
print(f"Accuracy: {model.score(X_test, y_test)}")
```


## Conclusion and Further Reading

Active learning is a powerful technique that can help us automate the labeling process for large datasets. By selecting a subset of the data that is most relevant to the task at hand, active learning can be more efficient than manually labeling every example in a dataset. This can lead to better results and more accurate predictions.

I hope this blog post has provided a good overview of active learning and how to implement it using Python. If you have any questions or comments, feel free to leave them in the comments below.

For more information on active learning, I recommend checking out the following resources:

- [Active Learning for Text Classification](https://www.kdnuggets.com/2020/07/active-learning-text-classification.html)
- [Active Learning for Image Classification](https://www.kdnuggets.com/2020/07/active-learning-image-classification.html)
- [Active Learning for Time Series Classification](https://www.kdnuggets.com/2020/07/active-learning-time-series-classification.html)


## Another Example

I've implemented active learning with a different dataset and model. You can find the code and results in this [GitHub repository](https://github.com/AestheticVoyager/ActiveLearning-Example).

```python
def run_experiment(sampling_strategy, X_pool, y_pool, X_test, y_test, 
                  initial_samples=20, batch_size=10, iterations=10):
    X_pool_copy = X_pool.copy()
    y_pool_copy = y_pool.copy()
    X_pool_tfidf = vectorizer.transform(X_pool_copy)
    initial_indices = np.random.choice(len(X_pool_copy), initial_samples, replace=False)
    X_labeled = X_pool_tfidf[initial_indices]
    y_labeled = np.array(y_pool_copy)[initial_indices]
    labeled_mask = np.zeros(len(X_pool_copy), dtype=bool)
    labeled_mask[initial_indices] = True
    model = LogisticRegression(random_state=42)
    accuracies = []
    num_labeled = []
    model.fit(X_labeled, y_labeled)
    accuracies.append(evaluate_model(model, X_test_tfidf, y_test))
    num_labeled.append(initial_samples)
    for i in range(iterations):
        X_unlabeled = X_pool_tfidf[~labeled_mask]
        if sampling_strategy == 'uncertainty':
            indices_to_label_relative = uncertainty_sampling(model, X_unlabeled, batch_size)
            unlabeled_indices = np.where(~labeled_mask)[0]
            indices_to_label = unlabeled_indices[indices_to_label_relative]
        else:  
            unlabeled_indices = np.where(~labeled_mask)[0]
            indices_to_label = np.random.choice(unlabeled_indices, batch_size, replace=False)
        labeled_mask[indices_to_label] = True
        X_labeled = X_pool_tfidf[labeled_mask]
        y_labeled = np.array(y_pool_copy)[labeled_mask]
        model.fit(X_labeled, y_labeled)
        accuracy = evaluate_model(model, X_test_tfidf, y_test)
        accuracies.append(accuracy)
        num_labeled.append(np.sum(labeled_mask))
    return num_labeled, accuracies
```