---
title: Python-script voor het ophalen van gegevens uit Azure Log Analytics | Microsoft Docs
description: Log Analytics logboek Search API kan een REST-API-client gegevens ophalen van een werkruimte voor logboekanalyse.  Dit artikel bevat een voorbeeld van pythonscript met de Search-API van het logboek.
services: log-analytics
documentationcenter: 
author: bwren
manager: carmonm
editor: tysonn
ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: bwren
ms.openlocfilehash: a8a4ec7a6ddf2daeca6ead11460fa076a7eb5c94
ms.sourcegitcommit: 3df3fcec9ac9e56a3f5282f6c65e5a9bc1b5ba22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2017
---
# <a name="retrieve-data-from-log-analytics-with-a-python-script"></a>Gegevens ophalen van logboekanalyse met een pythonscript
De [Log Analytics Log-API van zoekservice](log-analytics-log-search-api.md) kunnen een REST-API-clientcomputers gegevens ophalen van een werkruimte voor logboekanalyse.  Dit artikel bevat een voorbeeldscript Python die gebruikmaakt van de Log Analytics logboek zoeken-API.  

>[!NOTE]
> In dit artikel gebruikt de Search-API van het logboek voor de verouderde querytaal in logboekanalyse.  Een update aan dit artikel worden verstrekt voor werkruimten die zijn geüpgraded naar de [querytaal van nieuwe logboekanalyse](log-analytics-log-search-upgrade.md).

## <a name="authentication"></a>Authentication
Dit script maakt gebruik van een service-principal in Azure Active Directory om te verifiëren naar de werkruimte.  Service-principals toestaan dat een clienttoepassing om aan te vragen of de service een account verifiëren, zelfs als de client beschikt niet over de accountnaam. Voordat u dit script uitvoert, moet u een service-principal met behulp van het proces op [portal gebruik maken van een Azure Active Directory-toepassing en service-principal die toegang bronnen tot](../azure-resource-manager/resource-group-create-service-principal-portal.md).  U moet de toepassings-ID, Tenant-ID en verificatiesleutel bieden aan het script. 

> [!NOTE]
> Wanneer u [maken van een Azure Automation-account](../automation/automation-create-standalone-account.md), een service-principal dat geschikt is voor gebruik met dit script wordt gemaakt.  Als u al een service-principal gemaakt door Azure Automation hebt en vervolgens u kunnen moet gebruiken in plaats van een nieuw wachtwoord maken, hoewel u wellicht [maken van een verificatiesleutel](../azure-resource-manager/resource-group-create-service-principal-portal.md#get-application-id-and-authentication-key) als deze nog niet hebt.

## <a name="script"></a>Script
``` python
import adal
import requests
import json
import datetime
from pprint import pprint

# Details of workspace.  Fill in details for your workspace.
resource_group = 'xxxxxxxx'
workspace = 'xxxxxxxx'

# Details of query.  Modify these to your requirements.
query = "Type=Event"
end_time = datetime.datetime.utcnow()
start_time = end_time - datetime.timedelta(hours=24)
num_results = 100  # If not provided, a default of 10 results will be used.

# IDs for authentication.  Fill in values for your service principal.
subscription_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx'
tenant_id = 'xxxxxxxx-xxxx-xxxx-xxx-xxxxxxxxxxxx'
application_id = 'xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxx'
application_key = 'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'

# URLs for authentication
authentication_endpoint = 'https://login.microsoftonline.com/'
resource  = 'https://management.core.windows.net/'

# Get access token
context = adal.AuthenticationContext('https://login.microsoftonline.com/' + tenant_id)
token_response = context.acquire_token_with_client_credentials('https://management.core.windows.net/', application_id, application_key)
access_token = token_response.get('accessToken')

# Add token to header
headers = {
    "Authorization": 'Bearer ' + access_token,
    "Content-Type":'application/json'
}

# URLs for retrieving data
uri_base = 'https://management.azure.com'
uri_api = 'api-version=2015-11-01-preview'
uri_subscription = 'https://management.azure.com/subscriptions/' + subscription_id
uri_resourcegroup = uri_subscription + '/resourcegroups/'+ resource_group
uri_workspace = uri_resourcegroup + '/providers/Microsoft.OperationalInsights/workspaces/' + workspace
uri_search = uri_workspace + '/search'

# Build search parameters from query details
search_params = {
        "query": query,
        "top": num_results,
        "start": start_time.strftime('%Y-%m-%dT%H:%M:%S'),
        "end": end_time.strftime('%Y-%m-%dT%H:%M:%S')
        }

# Build URL and send post request
uri = uri_search + '?' + uri_api
response = requests.post(uri,json=search_params,headers=headers)

# Response of 200 if successful
if response.status_code == 200:

    # Parse the response to get the ID and status
    data = response.json()
    search_id = data["id"].split("/")
    id = search_id[len(search_id)-1]
    status = data["__metadata"]["Status"]

    # If status is pending, then keep checking until complete
    while status == "Pending":

        # Build URL to get search from ID and send request
        uri_search = uri_search + '/' + id
        uri = uri_search + '?' + uri_api
        response = requests.get(uri,headers=headers)

        # Parse the response to get the status
        data = response.json()
        status = data["__metadata"]["Status"]

else:

    # Request failed
    print (response.status_code)
    response.raise_for_status()

print ("Total records:" + str(data["__metadata"]["total"]))
print ("Returned top:" + str(data["__metadata"]["top"]))
pprint (data["value"])
```
## <a name="next-steps"></a>Volgende stappen
- Meer informatie over de [Log Analytics Log-API van zoekservice](log-analytics-log-search-api.md).