
# CKAN Downloader for Amsterdam Data
This is a simple downloader for all data hosted through the Amsterdam city data portal (<http://data.amsterdam.nl>). Since this is a CKAN instance, we will use the CKAN API to retrieve the JSON description of the data, and use the `url` of the JSON object to download the data (if possible)


    import json
    import requests


* Do an HTTP GET against the API to retrieve all datasets (limited to 10000, which is way more than the CKAN contains). 
* Take the JSON representation of the response and convert it to a Python dictionary.
* Take the `results` element of the JSON object (a Python dictionary)

&nbsp;

    ckan_response = requests.get('http://data.amsterdam.nl/api/search/dataset?all_fields=1&offset=0&limit=10000')
    ckan_json = ckan_response.json()
    results = ckan_json['results']

* Loop over all results
* For every result, check whether it is in a format that we can understand
* If so, retrieve it by doing a GET against the `url` of the resource
* ... and save it to the current directory.

&nbsp;

    for r in results:
        rj = json.loads(r['data_dict'])
        
        for resource in rj['resources']:
            if resource['format'] in ['JSON','api','XLS','CSV','ZIP'] :
                print u"Retrieving {}".format(resource['name'])
                resource_data_response = requests.get(resource['url'])
                resource_filename = u"{}-{}.{}".format(resource['id'],resource['name'].replace(' ','_'),resource['format'].lower())
                
                try :
                    with open(resource_filename,'wb') as resource_file:
                        print u"Writing to {}".format(resource_filename)
                        resource_file.write(resource_data_response.content)
                except:
                    print u"Error while writing {}".format(resource_filename)
