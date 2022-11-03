# Download data from M<sup>3</sup>G

In the Bash shell, one could use a command-line such `curl` to access [M<sup>3</sup>G API](intro.md): it acts as an HTTP client and allows you to send requests and display/store the corresponding responses.

## `GET` the sitelog of a given station <a id='sitelog'></a>
Let's say you have identified a site log (i.e. information that you also get in the M<sup>3</sup>G website) by the station 9-characters id aka `id` e.g. BRUX00BEL, and you want to download it. M3G allows you to do this and get the output in various formats: JSON, [GeodesyML](http://geodesyml.org/) or [IGS site log format](https://kb.igs.org/hc/en-us/articles/203402393-IGS-Site-Log-Manager-User-Guide).<br>
To get the JSON is the format:
```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog/view?id=BRUX00BEL" -H  "accept: application/json"
```
Here we used `curl` to send an HTTP GET request to return the resource located by the given URL. In practice you can omit the `-X GET` (which is the default) and you might want to 'nicely' format the output with `json_pp`:

```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog/view?id=BRUX00BEL" -H  "accept: application/json" | json_pp
```
and get the following output:
```
{
   "_links" : {
      "geodesyML" : {
         "href" : "https://gnss-metadata.eu/__test/v1/sitelog/exportxml?id=BRUX00BEL"
      },
      "self" : {
         "href" : "https://gnss-metadata.eu/__test/v1/sitelog/view?id=BRUX00BEL"
      },
      "sitelog" : {
         "href" : "https://gnss-metadata.eu/__test/v1/sitelog/exportlog?id=BRUX00BEL"
      }
   },
   "dateUpdate" : "2020-09-30T09:39Z",
   "geodesymlName" : "BRUX00BEL.xml",
   "id" : "BRUX00BEL",
   ...
```
For the GeodesyML or the IGS site log format:
```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog/exportxml?id=BRUX00BEL" -H  "accept: application/geodesyML0.4"
```
```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog/exportlog?id=BRUX00BEL" -H  "accept: application/sitelog"
```


## `GET` the sitelogs of all stations matching a search criterion<a id='sitelogs'></a>
Let's say you want to get the site log for entries that fit a give search criterion, e.g. stations having the string "PRAG" in their 9-char id:

```bash
curl -g -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog?page=1&filter[id][like]=PRAG" -H  "accept: text/plain"
```
As you can notice by looking at the command line above, you can select which page of results should be returned (`?page=1`). This is one of the "pagination" parameters. See, for example, a [REST API handbook](https://developer.wordpress.org/rest-api/using-the-rest-api/pagination/) for more info.
More conveniently, one can select one of the columns of the output and get:

```Bash
curl -g -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog?page=1&filter[id][like]=PRAG" -H  "accept: text/plain" | awk '{print $1}'
```
and get:
```
id
PRAG00CZE
PRAG00REU
...
```
aka the site logs of the entries match your search criterion: `filter[id][like]=PRAG`. There are various other query parameters. You might find some others in the generated [swagger dashboard of our API](https://gnss-metadata.eu/__test/site/api-docs#/Metadata/get_sitelog){:target="_blank"}. Note that, on most terminals, if `[]`and/or `{}` are present in the URL, one needs to add `-g` to the command line to disable the "URL globbing parser" and escape parentheses.


## `GET` the list of stations included in a given network<a id='list_netw'></a>
It could be useful to simply get a list of stations (9-char id) included in a given network e.g. EPN, EPOS.
Here we'll need to use `curl` and a bit of Python, but no special libraries:
```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/network/view?id=EPN" -H  "accept: application/json" | python -c "import sys,json; print(json.load(sys.stdin)['included'])"
```
You'll get a list of stations e.g.
```
['BRUX00BEL', 'AUBG00DEU', 'NOA100GRC', 'GOR200DEU', 'BORJ00DEU', 'HEL200DEU',...
```
Similarly, to get the list of stations proposed to a given network e.g. EPOS:
```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/network/view?id=EPOS" -H  "accept: application/json" | python -c "import sys,json; print(json.load(sys.stdin)['proposed'])"
```
## `GET` list of satellite systems that a given station is currently tracking<a id='list_sat'></a>

```bash
curl -s -X GET "https://gnss-metadata.eu/__test/v1/sitelog/view?id=BRUX00BEL" -H  "accept: application/json" | python -c "import sys,json; print(json.load(sys.stdin)['sitelog']['receivers'][-1]['satelliteSystem'])"
```
```
['GPS', 'GLO', 'GAL', 'BDS', 'QZSS', 'IRNSS', 'SBAS']
```
