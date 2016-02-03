pycrits
=======

Python interface to the CRITs API.

This is currently very minimal. Fetching data is pretty stable but the API for
submitting data to CRITs is still alpha.

I'll write docs once things become stable, but for now here is some basic
usage:

You will need the Requests Python module to use this.

```
from pycrits import pycrits

crits = pycrits('http://localhost:8000', 'wxs', '<api_key>')
for obj in crits.indicators():
    print(obj['value']) 
```

Here's an example of how to fetch a PCAP. If nothing is found you will
get an empty list back. These are all fetching the same file.

```
>>> from pycrits import pycrits
>>> crits = pycrits('http://localhost:8000', 'wxs', '<api_key>')
>>> x = crits.fetch_pcap(md5='67cc75e608b4f37ed993bf84fafafb9d')
>>> print(len(x[0]['data'])) 
22279
>>> x = crits.fetch_pcap(id_='51ac0abcd6fa25ca9d2d277f')
>>> print(len(x[0]['data'])) 
22279
>>> x = crits.fetch_pcap(params={'c-filename': 'sedtest.pcap'})
>>> print(len(x[0]['data'])) 
22279
>>>
```

Here's an example of using sample_count() to get a count of number of samples
that are over 1MB in size:

```
from pycrits import pycrits
crits = pycrits('http://localhost:8000', 'wxs', '<api_key>')
params = {'c-size__gte': 1024 * 1024}
print(crits.sample_count(params=params)) 
```

Here is an example of using crits.domains to query mongodb and write all domains to a text file.

```
from pycrits import pycrits
from datetime import datetime
import requests
import sys

crits = pycrits('Your CRITS server', 'CRITS USERNAME', 'API KEY')
rundate = datetime.now().strftime('-%H%M-%m-%d-%y')
sys.stdout = open("results" +".txt", "w")


# Disables SSL verification.
# Accepts [True, False, and path to certificate file]
crits.verify = False

# Supresses SSL certification verification
requests.packages.urllib3.disable_warnings()

for obj in crits.domains():
    print obj['domain']
```
