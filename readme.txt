In this version, I have added one more micro-service which gets the data of playlist and creates an xml. This xml when saved as .xspf file, can be executed using VLC player to play that particular song.
Also, introduces API gateway, minIO server and sharing of the track database.

API Gateway:
In this I have used Kong API to consolidate all the services behind this gateway. XSPF microservice accessed the other micro services through Kong's port 8000.

Installation of Kong API can be found in installation.txt 
For more information visit https://docs.konghq.com/install/ubuntu/

MinIO:
MinIO has its own browser where we can login and create buckets to store data it will run on port 9000. I have uploaded all the songs in this bucket so that it can be accessed using URL http://localhost:9000/tracks/musicFile.mp3 instead of calling tracks from flaskAPI.

Installation of minIO can be found in installation.txt 
For more information visit https://docs.konghq.com/install/ubuntu/  
https://docs.min.io/ 

Database Sharding: 

In previous version I had only one database with four tables. As the track database will have more data and the hits to this table will be more than that of others. Hence, I have separated tracks database and one database containing rest of the tables.

For sharing, I have created 3 different databases of tracks with same table structure and insertion of the records is done sharing track_id by modulo 3. To insert a row in track table, concept of UUID is used. Incoming insert request will go to which table is decided at the run time by using modulo function logic. Similar process is followed to fetch a particular track from database.
Changed database scripts can be found in DB.txt

For UUID: https://docs.python.org/3/library/uuid.html


To generate xml for xspf,it is necessary to download the supporting files for it. You can download these supporting files in src (feedparser.py, xspfparser.py, xspf.py) 
For information on xspf visit - https://xspf.org/

This project is developed using FlaskAPI, python, Sqlite3, foreman, tavern

Micro-services can be tested using Postman. Details about request/response details cab ne found in the file "Music_Microservices_Part-2_Documentation". 

Before running the project make sure installation is done as per installation.txt

To Run all Micro-services execute below command on terminal after navigating to the folder you have downloaded the project:
-----------------------------
foreman start


To Test all micro services:
-------------------------
python3 app.py test