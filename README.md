# RME

This repo contains source code for our paper: "Regularizing Matrix Factorization with User and Item Embeddings for Recommendation" published in CIKM 2018. We implemented using multi-threads, so it is very fast to run with big datasets.

# DATA FORMAT 


#### Data format:
- First line: the header "userId,movieId"
- Second line --> last line: [userId],[movieId]

### data for running our source code: ml10m.
We preprocessed it and splitted into train, vad/dev, test. Their paths are:

- data/ml10m/train.csv

- data/ml10m/test.csv

- data/ml10m/validation.csv

### format of user and disliked items: same as previous format: 
- First line: the header "userId,movieId"
- Second line --> last line: [userId],[movieId]

### When we have available users and dislike items:
do 2 steps:
- saved it to data/ml10m/train_neg.csv
- build the disliked item-item co-occurrence by running (assume that the dataset is ml10m):
**produce_negative_cooccurrence.py --dataset ml10m**


# RUNNING:
### Step 1: produce user-user co-occurrence matrix and item-item co-occurrence matrix
**python produce_positive_cooccurrence.py --dataset ml10m**

### Step 2: run RME with our user-oriented EM-like algorithm to infer disliked items for users:
**python rme_rec.py --dataset ml10m --model rme --neg_item_inference 1 --n_factors 40 --reg 1.0 --reg_embed 1.0**

where:
- <code>model<code>: the model to run. There are 3 choices: <code>rme<code> (our model), <code>wmf<code>, <code>cofactor<code>.
- <code>reg<code>: is the regularization hyper-parameter for user and item latent factors (alpha and beta).
- <code>reg_emb<code>: is the regularization hyper-parameter for user and item context latent factors (gamma, theta, delta).
- <code>n_factors<code>: number of latent factors (or embedding size). Default: n_factors = 40.
- <code>neg_item_inference<code>: whether or not running our user-oriented EM like algorithm for sampling disliked items for users.
- <code>neg_item_inference<code>: negative sample ratio per user. If a user consumed 10 items, and this neg_sample_ratio = 0.2 --> randomly sample 2 negative items for the user. Default: 0.2.

#### other hyper-parameters:
- s: the shifted constant, which is a hyper-parameter to control density of SPPMI matrix. Default: s = 1.
- data_path: path to the data. Default: data.
- saved_model_path: path to saved the optimal model using validation/development dataset. Default: MODELS.

### running some baselines: Cofactor, WMF:
- Running cofactor:

**python rme_rec.py --dataset ml10m --model cofactor --n_factors 40 --reg 1.0 --reg_embed 1.0**

You may get the results like:

- Running WMF:

**python rme_rec.py --dataset ml10m --model wmf --n_factors 40 --reg 1.0 --reg_embed 1.0**

You may get the results like:




# CITATION:

If you use this Caser in your paper, please cite the paper:

```
@inproceedings{tran2018regularizing,
  title={Regularizing Matrix Factorization with User and Item Embeddings for Recommendation},
  author={Tran, Thanh and Lee, Kyumin and Liao, Yiming and Lee, Dongwon},
  booktitle={Proceedings of the 27th ACM International Conference on Information and Knowledge Management},
  pages={687--696},
  year={2018},
  organization={ACM}
}
```



