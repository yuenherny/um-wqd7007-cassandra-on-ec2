# Benchmarking MongoDB Performance on Hospitality Dataset

## Install `gdown` for downloading files from Google Drive
By default, Ubuntu AMI on AWS comes with `python3` installed. However you will still need to install `python3-pip` in order to use `pip` to install `gdown`.
```
$ sudo apt install python3-pip
```
Install `gdown`
```
$ pip3 install gdown
```
Use `gdown` to download the folder containing the dataset.
```
$ gdown --folder https://drive.google.com/drive/folders/1vAml9cJ0B7p3RhaMR3rxpV19EcJgzFOb
```

## Install MongoDB Community Edition
Follow this [official tutorial](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#install-mongodb-community-edition) to install MongoDB on Ubuntu.

 ## Launch MongoDB
Follow the [steps](https://www.mongodb.com/docs/manual/tutorial/install-mongodb-on-ubuntu/#run-mongodb-community-edition) here to start MongoDB.

## Create a MongoDB database with `listings` and `reviews` table
Launch `mongosh`, the MongoDB interactive shell.
```
$ mongosh
```
Show existing databases.
```
test> show dbs
```
Create a new database named `hotel`.
```
test> use hotel
```
Exit `mongsh` to import CSV data into MongoDB. Remember to time these commands as it is important to know about dataset import time too. Just drop the `time` utility if execution time not needed. Importing `listings.csv`:
```
$ time mongoimport --db hotel --collection listings --type csv --headerline --file 7007_data/merged_listings.csv
```
in which the following results were obtained:
```
real	1m26.883s
user	1m36.463s
sys	    0m25.200s
```
Importing `reviews.csv`:
```
$ time mongoimport --db hotel --collection reviews --type csv --headerline --file 7007_data/merged_reviews.csv
```
in which the following results were obtained:
```
real	18m59.104s
user	14m18.680s
sys	    3m0.393s
```
Launch `mongosh` again and show created tables. You should see `listings` and `reviews` listed.
```
test> use hotel
hotel> show tables
```

## Simple operations in MongoDB
Count how many rows (or documents) in the table.
```
hotel> db.listings.count()
```
List the first 2 rows (or documents).
```
hotel> db.listings.find().limit(2)
```

## Queries related to hospitality: Reviews dataset
Retrieve "top 10 listings with most reviews":
```
db.reviews.aggregate([
  {
    $group: {
      _id: "$listing_id",
      count: { $sum: 1 }
    }
  },
  {
    $sort: { count: -1 }
  },
  {
    $limit: 10
  }
])
```
Retrieve "top 10 listings with most reviews and were recently reviewed":
```
db.reviews.aggregate([
  {
    $group: {
      _id: "$listing_id",
      count: { $sum: 1 },
      lastReviewDate: { $max: "$date" }
    }
  },
  {
    $sort: { count: -1, lastReviewDate: -1 }
  },
  {
    $limit: 10
  }
])
```
Retrieve "top 10 reviewers with most reviews":
```
db.reviews.aggregate([
  {
    $group: {
      _id: "$reviewer_id",
      count: { $sum: 1 }
    }
  },
  {
    $sort: { count: -1 }
  },
  {
    $limit: 10
  }
])
```
Retrieve "top 10 reviewers with most reviews and recently commented":
```
db.reviews.aggregate([
  {
    $group: {
      _id: "$reviewer_id",
      count: { $sum: 1 },
      lastCommentDate: { $max: "$date" }
    }
  },
  {
    $sort: { count: -1, lastCommentDate: -1 }
  },
  {
    $limit: 10
  }
])
```
