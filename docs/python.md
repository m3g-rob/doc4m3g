# Use <i>python requests</i> library

One could integrate the REST API with different languages, for example Python. Similar to `curl` in the shell, you can use *requests* in Python (quickly installable via `pip install requests`). It's a generic HTTP client library that allows you to send HTTP requests.

Let's look at the test example mentioned above, on how to update/change the firmware section of a station. To do this, one needs to get authorization via the ["Application access token"](authorization.md):

```python
import requests

headers={'accept': 'application/json', 'Authorization': 'Bearer xxx..'}
# where 'xxx..' is your "Application access token"

# authorization is not needed for GET requests:
#headers={'accept': 'application/json'}
```
## Examples of some basic functions to interact M<sup>3</sup>G API via <i>requests</i>
As mentioned in the example with `curl`, to perform this specific task, the M<sup>3</sup>G API [requires](https://gnss-metadata.eu/__test/site/api-docs#/Update/put_sitelog_firmware_change){:target="_blank"} the `id` aka the station 9-characters id and the records that will be changed as key/values pairs: the name of the person who is allowed to change the site log (`updateMadeBy`), the new firmware version (`changedTo`) and the date.
One could write a couple of functions on the fly using the *requests* module we have just loaded:

```python
#store the M3G API URL:
def url(path):
    return 'https://gnss-metadata.eu/v1/' + path
#get info about a site log giventhe 9-char id:
def get_sitelog(station_id):
    return requests.get(url('sitelog/view?id={}'.format(station_id)),headers=headers)
#update a record in the site log (the firmware version in this case) given the 9-char id:
def update_firmware(station_id, updateMadeBy, changedTo, datetimeISO, datetimeISOfuture):
    url = url('sitelog/firmware-change?id={}'.format(station_id))
    return requests.put(url, json={
        'updateMadeBy': updateMadeBy,
        'changedTo': changedTo,
        'endOfTheLastSection': datetimeISO,
        'startOfTheNewSection': datetimeISOfuture,
        }, headers=headers)
```
## Update the sitelog with a new firmware
We then send the request to update the site log with the mandatory information:
```python
my_update=update_firmware('BRUX00BEL','Carine Bruyninx', '5.3.1', '2020-09-04T11:50Z', '2020-09-04T11:51Z')
my_update.status_code
```
The latter will return as a response one of HTTP status codes, as we mentioned before:
```
200
```
## Retrieve the sitelog
We can now check if the record now stored in  M<sup>3</sup>G is what we expect.<br>
We might want to get the link to the updated site log:
```python
my_update.url
```
```
'https://gnss-metadata.eu/__test/v1/sitelog/view?id=BRUX00BEL'
```
or retrieve the info about the sitelog of our station (BRUX00BEL)
```python
my_station=get_sitelog('BRUX00BEL')
my_station.status_code
```
and indeed the request is successful:
```
200
```
## Get information about the firmware version stored in the sitelog
We can now have a look at the content of the record we're interested in aka `firmwareVersion`:
```python
my_station.json()['sitelog']['receivers'][-1]['firmwareVersion']
```
and get:
```
'5.3.1'
```
## Download the sitelog of a given station at a specific day
As an alternative, we can use another available `GET` request `/sitelog/day-view`.
This allows you to download metadata corresponding to a specific day (in the YYYY-MM-DD format) for a given station. For example:
```python
 #get info about a site log at a given date YYYY-MM-DD) and given the 9-char id:
 def get_sitelog_date(station_id, date):
     return requests.get(url('sitelog/day-view?id={}&date={}'.format(station_id,date)),headers=headers)

my_station=get_sitelog_date('BRUX00BEL','2019-06-06')
my_station.json()['sitelog']['receivers'][-1]['firmwareVersion']
```
and get:
```
'2.9.6-extref3'
```
.
