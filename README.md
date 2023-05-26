## Overview
The UK Food Standards Agency evaluates various establishments across the United Kingdom, and gives them a food hygiene rating. In this project, I evaluated some of their ratings data which might be useful to journalists and food critics to decide where to focus future articles. This project implements CRUD operations (see diagram below), which are commonly used in database systems and APIs to manage data. CRUD stands for Create, Read, Update, and Delete, representing the basic operations performed on data. I utilized the CRUD concept to create, view, modify and delete data from the database with the help of <a href=https://pymongo.readthedocs.io/en/stable/>PyMongo</a>.

<p align="center">
 
 <img  width="350" src =https://github.com/Jayplect/nosql-challenge/assets/107348074/caf307b5-7dd0-4bf5-936d-fbdc860445bb>
 
</p>

## Technologies/Libraries
The following technologies/libraries are used in this project:

<p >
 <img  width="300" src =https://github.com/Jayplect/nosql-challenge/assets/107348074/72ec540f-c313-46a3-b5ad-1c3c965cd0ad>

 <img  width="200" src = https://user-images.githubusercontent.com/107348074/236379825-80dc02bc-46c1-46fa-9634-dc28cdcb5704.png>
</p>

## Project Steps
### Part 1: Database and Jupyter Notebook Set Up- CRUD Operations

- Create

To begin, I imported the data from my Terminal. The database, collection and path represents any specified database name, collection name and relative path of the data file. 

    ! mongoimport --type json -d <database> -c <collection> --drop --jsonArray <path>
 
To access the database I `created` an instance of the Mongo Client using a default port of 27017. The server then processed the request and returned the resource. To confirm that the instance was returned, I used `mongo.list_database_names()` to list the representation of the created resource (in this case the databases).
 
    # Create an instance of MongoClient
    mongo = MongoClient(port=27017)

    # confirm that the new database was created
    print(mongo.list_database_names())

    # assign the database to a variable name
    db = mongo['database']

- Read

Under this operation, I retrieved a list of all collections associated with the database of interest using `db.list_collection_names()`. Typically, the server will respond with a list of all available collections. Clearly, I have only one collection and so I assigned this collection to a variable.

    # review the collections in our new database
    print(db.list_collection_names())

    # assign the collection to a variable
    collection = db['collection']

- Update

An exciting new halal restaurant ("Penang Flavours") just opened in Greenwich, but hasn't been rated yet. Thus, I inserted this new restaurant with all its information (i.e., {new_dict}) into the collection using `insert_one` function.  However, to update specific fields, I made a PUT request using PyMongo `update_one()` function to the resource. The {id} represents the unique identifier of the resource to be updated while the {dict} represent the new updates. The server will then process the request and update the resource with the provided id. The response included a representation of the updated resource in an object form which I iterated upon. In addition, I observed that some of the number values were stored as strings, when they should in fact be stored as numbers. Therefore, I had to change the data type from String to Decimal for `longitude` and  `lattitude`. For `RatingValue`i set the non 1-5 Rating Values to Null before updating data type to integer.
 
    #insert a new dictionary
    collection.insert_one({new_dict})

    #update field
    collection.update_one({id},{$set:{dict}})

- Delete

For this process, I assumed that most Journalists and food critics may not be interested in any establishments within the Dover Local Authority from the database. So I checked how many documents contained the Dover Local Authority and then made a delete request to the resource using the {id} endpoint, where {id} represents the unique identifier of the resource to be deleted.
         
    #delete fields
    collection.delete_many({id})

### Part 2: Exploratory Analysis
 
In this part of the project, I queried the database using the PyMongo `find()` function to answer specific questions along the way. In order to analyze data for a group of documents I used MongoDB Aggregation operations. The questions and the approach used for the analysis are discussed below:
 
#1 The first analysis queried the establishments that had a hygiene score equal to 20.

#2 Secondly, I searched the establishments in London with a RatingValue greater than or equal to 4. To carry out this search, I used `$regex` to do an exact word match for London.

#3 I was inquisitive to know the top 5 establishments with a RatingValue of 5, sorted by lowest hygiene score. More particularly I wanted to know which of these top establishments was nearest to the new restaurant ("Penang Flavours") added. To perform this analysis, I compared the geocode to find the nearest locations wihtin a search limit of 0.01 degree on either side of the latitude and longitude.

#4 Lastly, I figured that it might be worthwhile to know how many establishments in each Local Authority area have a hygiene score of 0. To execute this analysis, I use the <a href="https://www.mongodb.com/docs/manual/core/aggregation-pipeline/">aggregation method</a> to answer this and then sorted the results from highest to lowest to reveal the top ten local authority areas.

## Conclusion
This readme provided an overview of the CRUD operations implemented in this project. By utilizing the specified endpoints, you can create, read, update, and delete resources as per your requirements. Refer to the <a href="https://github.com/Jayplect/nosql-challenge/tree/main">main page</a> to explore my codes and for detailed information about the request and response structures.

## References
UK Food Standards AgencyLinks to an external site. (2022). UK food hygiene rating data API. https://ratings.food.gov.uk/open-data/en-GBLinks to an external site.. Contains public sector information licensed under the Open Government Licence v3.0Links to an external site.
Accessed Sept 9, 2022 and Sept 12, 2022 with the establishment settings as follows: longitude=51.5072, latitude=-0.1276, maxdistancelimit=4567, pagesize=10000, sortoptionkey=distance, pagenumber=(1,2,3,4,5,6,7,8).
