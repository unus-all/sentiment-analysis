# Sentiment Analysis (WiG_Dataset)

## Introduction

This repository contains the train and test datasets used for sentiment analysis of public comments related to the war in Gaza. The datasets are in CSV format and include comment IDs, and labels. Additionally, instructions are provided on how to use the PRAW library to extract comments based on their IDs.

## Datasets

The datasets can be downloaded using git command:

```bash
git clone https://github.com/unus-all/sentiment-analysis.git
```

Each dataset consists of two columns:
- `comment_id`: The ID of the comment.
- `label`: The sentiment label of the comment (positive, negative, and neutral).

## Prerequisites

To run the scripts in this repository, you need to have the following installed:

- Python 3.x
- PRAW Python library

You can install the PRAW library using pip:

```bash
pip install praw
```

## Extracting Comments Using PRAW

PRAW (Python Reddit API Wrapper) is a Python package that allows for simple access to Reddit's API. Below are the steps to extract comments based on their IDs.

### Step 1: Setting Up PRAW

First, you need to set up a Reddit application to get your `client_id`, `client_secret`, `user_agent`, and `username`. You can create an application on Reddit by following these [instructions](https://praw.readthedocs.io/en/latest/getting_started/authentication.html).

### Step 2: Extracting Comments

Here is an example script to extract comments from the datasets using PRAW:

```python
import pandas as pd
import praw

# Load the datasets
train_df = pd.read_csv('./path/to/train_dataset.csv')
test_df = pd.read_csv('./path/to/test_dataset.csv')

# Configure PRAW
reddit = praw.Reddit(
    client_id='YOUR_CLIENT_ID',
    client_secret='YOUR_CLIENT_SECRET',
    user_agent='YOUR_USER_AGENT',
    username='YOUR_USERNAME'
)

def get_comment_by_id(comment_id):
    try:
        comment = reddit.comment(id=comment_id)
        return comment.body
    except Exception as e:
        print(f"Error retrieving comment {comment_id}: {e}")
        return None

# Extract comments from the train dataset
train_df['comment'] = train_df['comment_id'].apply(get_comment_by_id)

# Extract comments from the test dataset
test_df['comment'] = test_df['comment_id'].apply(get_comment_by_id)

# Save the updated datasets with comments
train_df.to_csv('./path/to/updated_train_dataset.csv', index=False)
test_df.to_csv('./path/to/updated_test_dataset.csv', index=False)
```

### Step 3: Running the Script

Run the script to download the comments based on their IDs:

```bash
python extract_comments.py
```

This script will read the `comment_id` from both the train and test datasets, use PRAW to fetch the actual comments from Reddit, and save the updated datasets with the comments included.


## Citation
In case you used this dataset in you research, please cite it as follows:
```bibtex
@misc{wigyual2024,
  author={Allal, Younes},
  title={WiG Dataset: Public Comments From Reddit Regarding The War In Gaza},
  url={https://github.com/unus-all/sentiment-analysis},
  year={2024}
}
```
