# trello-status
This script gets cards and comments from a trello board and writes an HTML page as a basis for a status report.  I'm using a docker container to run this, but you can just as easily do it outside of docker.  

* Clone py-trello 
```
docker run -it fedora /bin/bash
# In the container:
yum -y install git python-pip python-requests 
git clone https://github.com/sarumont/py-trello.git
pip install py-trello
```

* Get your API keys 
  * Login to trello.com
  * Go to https://trello.com/app-key to find your developer API keys
    * API Key: <Save your key>
    * API Secret: <Save your secret>

* Request a Token for your app using the API key
  * In a browser go to https://trello.com/1/connect?key=<insert key here>&name=StatusReport&response_type=token
NOTE: Add "&scope=read,write" if you want a read/write token. 

* Configure trello-status
```
git clone https://github.com/jonjozwiak/trello-status
cd trello-status
```
  * Add your API Key, API Secret, and Token from above into status_report.conf
```
api_key=...
api_secret=...
token=...
```
  * Add your board name that you want to report from 
```
board_name=...
```
  * By default it will report the last 7 days from today 

* Generate a report
```
./status_report.py
```
NOTE: This will create a report called status.html in your current directory


