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
Exit `mongsh` to import CSV data into MongoDB.
```
$ mongoimport --db hotel --collection listings --type csv --headerline --file 7007_data/merged_listings.csv
$ mongoimport --db hotel --collection reviews --type csv --headerline --file 7007_data/merged_reviews.csv
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
