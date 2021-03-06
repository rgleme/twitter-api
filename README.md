# TWITTER API
This App collects infos about hashtags and users from twitter, stores in a DataBase and shows in a webserver hosted by Flask

## Prerequisites
- Softwares\
   Docker\
   Docker-compose
- Local configurations\
  For manipulating Twitter API, it is necessary credentials, that are created in [twitter api](https://developer.twitter.com/en/docs/basics/authentication/oauth-1-0a)\
  After creating them, export them locally in your system. Eg: Linux Environment:
   ```bash
   export cons_tok="YOUR CREDDENTIALS"
   export cons_sec="YOUR CREDDENTIALS"
   export app_tok="YOUR CREDDENTIALS"
   export app_sec="YOUR CREDDENTIALS"
   ```
## Usage
After cloning the repo locally. Run:
```bash
docker-compose up
```
## Architecture
This project runs with:
- Flask APP ( serving webserver ) - Port 80
- Logstash ( collecting logs ) - Port 5001
- ElasticSearch ( indexing logs ) - Port 9200
- Kibana ( showing logs and dashboards ) - Port 5601
## Testing
There are two unit tests created for this environment
```bash
docker exec -ti docker-compose_flask_api_1 python test_frontend.py
docker exec -ti docker-compose_flask_api_1 python test_backend.py
```
## Webserver
![Api Home](/pictures/home.png)\
http://localhost
- Generate a New List\
  Collects a brand-new list, containing up to 100 tweets by given hashtags
- Top Users And Followers\
  Shows TOP 5 Users with followers count from all the list
- Top Tweets by Tags\
  After typing a hashtag, it will bring all tweets grouped by language and location
- All tweets grouped by hour\
  It shows all Tweets grouped by day and hour ( UTC Timezone !!! ) 
## Logging
After acessing Kibana for the very first time, it is necessary to create a index pattern:\
Send some requests to the API, so the logstash will start to consume it.\
Access http://localhost:5601 ( Credentials were kept default for an example usage. username: elastic password: changeme Change it as soon as possible ) \
On the left panel, click on management, then "Create index pattern"\
![Index Patterns](/pictures/index.png)\
In Index Pattern type "logstash-*" , then click Next Step\
![Index Patterns](/pictures/create_index.png)\
Select "@timestamp" in Time Filter field name, then create it !\
![Index Patterns](/pictures/timestamp.png)\
On the left panel, click "Discover" and all the log will be shown.\
![Kibana Query](/pictures/query.png)\
## Dashboard
On the left panel, click on management, then Saved Objects\
Click Import and choose "export.ndjson" that was cloned with the project\
On the left panel, click on dashboards, and choose Requests\
![Kibana Dashboard](/pictures/dashboard.png)\
This dashboard shows total amount of requests, and requests by status code OK ( 200 ) and not OK ( not 200 )