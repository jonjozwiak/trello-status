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
* Patch py-trello
```
vi /usr/lib/python2.7/site-packages/trello/card.py
# Edit line 158 to read: return sorted(comments).  
# If not, you will see the following error when executing:

Traceback (most recent call last):
  File "./status_report.py", line 48, in <module>
    c.fetch()
  File "/usr/lib/python2.7/site-packages/trello/card.py", line 149, in fetch
    self._comments = self.fetch_comments() if eager else None
  File "/usr/lib/python2.7/site-packages/trello/card.py", line 158, in fetch_comments
    return sorted(comments, key=lambda comment: checklist['date'])
  File "/usr/lib/python2.7/site-packages/trello/card.py", line 158, in <lambda>
    return sorted(comments, key=lambda comment: checklist['date'])
NameError: global name 'checklist' is not defined
```

* Get your API keys 
  * Login to trello.com
  * Go to https://trello.com/app-key to find your developer API keys
    * API Key: 
    * API Secret: 

* Request a Token for your app using the API key
  * In a browser go to https:// trello.com/1/connect?key=__insert key here__&name=StatusReport&response_type=token
    * Add "&scope=read,write" if you want a read/write token. 

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


