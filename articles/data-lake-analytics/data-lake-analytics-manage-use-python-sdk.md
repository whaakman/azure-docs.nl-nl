---
title: Azure Data Lake Analytics met behulp van Python beheren
description: In dit artikel wordt beschreven hoe u Python gebruikt voor het beheren van Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken.
services: data-lake-analytics
ms.service: data-lake-analytics
author: matt1883
ms.author: saveenr
ms.reviewer: jasonwhowell
ms.assetid: d4213a19-4d0f-49c9-871c-9cd6ed7cf731
ms.topic: conceptual
ms.date: 06/08/2018
ms.openlocfilehash: f73ef118efbdfc94d8cb9b7d81717bd13511c785
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048279"
---
# <a name="manage-azure-data-lake-analytics-using-python"></a>Azure Data Lake Analytics met behulp van Python beheren
[!INCLUDE [manage-selector](../../includes/data-lake-analytics-selector-manage.md)]

In dit artikel wordt beschreven hoe u Azure Data Lake Analytics-accounts, gegevensbronnen, gebruikers en taken beheren met behulp van Python.

## <a name="supported-python-versions"></a>Ondersteunde versies van Python

* Gebruik een 64-bits versie van Python.
* U kunt de standaard Python-distributie die is gevonden op  **[Python.org downloads](https://www.python.org/downloads/)**. 
* Veel ontwikkelaars vindt u het gemakkelijk te gebruiken de  **[Anaconda Python-distributie](https://www.anaconda.com/download/)**.  
* In dit artikel is geschreven met behulp van Python 3.6-versie van de standaard Python-distributie

## <a name="install-azure-python-sdk"></a>Azure Python-SDK installeren

Installeer de volgende modules:

* De **azure-mgmt-resource** -module bevat andere Azure-modules voor Active Directory, enzovoort.
* De **azure-datalake-store** -module bevat de bestandssysteembewerkingen voor Azure Data Lake Store. 
* De **azure-mgmt-datalake-store** -module bevat de accountbeheerbewerkingen voor Azure Data Lake Store.
* De **azure-mgmt-datalake-analytics** -module bevat de Azure Data Lake Analytics-bewerkingen. 

Eerst, zorg ervoor dat u hebt de meest recente `pip` door het uitvoeren van de volgende opdracht uit:

```
python -m pip install --upgrade pip
```

Dit document is geschreven met `pip version 9.0.1`.

Gebruik de volgende `pip` opdrachten de modules installeren vanaf de opdrachtregel:

```
pip install azure-mgmt-resource
pip install azure-datalake-store
pip install azure-mgmt-datalake-store
pip install azure-mgmt-datalake-analytics
```

## <a name="create-a-new-python-script"></a>Maak een nieuwe Python-script

Plak de volgende code in het script:

```python
## Use this only for Azure AD service-to-service authentication
#from azure.common.credentials import ServicePrincipalCredentials

## Use this only for Azure AD end-user authentication
#from azure.common.credentials import UserPassCredentials

## Required for Azure Resource Manager
from azure.mgmt.resource.resources import ResourceManagementClient
from azure.mgmt.resource.resources.models import ResourceGroup

## Required for Azure Data Lake Store account management
from azure.mgmt.datalake.store import DataLakeStoreAccountManagementClient
from azure.mgmt.datalake.store.models import DataLakeStoreAccount

## Required for Azure Data Lake Store filesystem management
from azure.datalake.store import core, lib, multithread

## Required for Azure Data Lake Analytics account management
from azure.mgmt.datalake.analytics.account import DataLakeAnalyticsAccountManagementClient
from azure.mgmt.datalake.analytics.account.models import DataLakeAnalyticsAccount, DataLakeStoreAccountInfo

## Required for Azure Data Lake Analytics job management
from azure.mgmt.datalake.analytics.job import DataLakeAnalyticsJobManagementClient
from azure.mgmt.datalake.analytics.job.models import JobInformation, JobState, USqlJobProperties

## Required for Azure Data Lake Analytics catalog management
from azure.mgmt.datalake.analytics.catalog import DataLakeAnalyticsCatalogManagementClient

## Use these as needed for your application
import logging, getpass, pprint, uuid, time
```

Voer dit script om te controleren dat de modules kunnen worden geïmporteerd.

## <a name="authentication"></a>Verificatie

### <a name="interactive-user-authentication-with-a-pop-up"></a>Interactieve gebruikersverificatie met een pop-upvenster

Deze methode wordt niet ondersteund.

### <a name="interactive-user-authentication-with-a-device-code"></a>Interactieve gebruikersverificatie met de apparaatcode van een

```python
user = input('Enter the user to authenticate with that has permission to subscription: ')
password = getpass.getpass()
credentials = UserPassCredentials(user, password)
```

### <a name="noninteractive-authentication-with-spi-and-a-secret"></a>Niet-interactieve verificatie met SPI en een geheim

```python
credentials = ServicePrincipalCredentials(client_id = 'FILL-IN-HERE', secret = 'FILL-IN-HERE', tenant = 'FILL-IN-HERE')
```

### <a name="noninteractive-authentication-with-api-and-a-certificate"></a>Niet-interactieve verificatie met API en een certificaat

Deze methode wordt niet ondersteund.

## <a name="common-script-variables"></a>Algemene scriptvariabelen

Deze variabelen worden gebruikt in de voorbeelden.

```python
subid= '<Azure Subscription ID>'
rg = '<Azure Resource Group Name>'
location = '<Location>' # i.e. 'eastus2'
adls = '<Azure Data Lake Store Account Name>'
adla = '<Azure Data Lake Analytics Account Name>'
```

## <a name="create-the-clients"></a>De clients maken

```python
resourceClient = ResourceManagementClient(credentials, subid)
adlaAcctClient = DataLakeAnalyticsAccountManagementClient(credentials, subid)
adlaJobClient = DataLakeAnalyticsJobManagementClient( credentials, 'azuredatalakeanalytics.net')
```

## <a name="create-an-azure-resource-group"></a>Een Azure-resourcegroep maken

```python
armGroupResult = resourceClient.resource_groups.create_or_update( rg, ResourceGroup( location=location ) )
```

## <a name="create-data-lake-analytics-account"></a>Een Data Lake Analytics-account maken

Maak eerst een store-account.

```python
adlsAcctResult = adlsAcctClient.account.create(
    rg,
    adls,
    DataLakeStoreAccount(
        location=location)
    )
).wait()
```
Vervolgens maakt u een ADLA-account dat gebruikmaakt van dat archief.

```python
adlaAcctResult = adlaAcctClient.account.create(
    rg,
    adla,
    DataLakeAnalyticsAccount(
        location=location,
        default_data_lake_store_account=adls,
        data_lake_store_accounts=[DataLakeStoreAccountInfo(name=adls)]
    )
).wait()
```

## <a name="submit-a-job"></a>Een taak verzenden

```python
script = """
@a  = 
    SELECT * FROM 
        (VALUES
            ("Contoso", 1500.0),
            ("Woodgrove", 2700.0)
        ) AS 
              D( customer, amount );
OUTPUT @a
    TO "/data.csv"
    USING Outputters.Csv();
"""

jobId = str(uuid.uuid4())
jobResult = adlaJobClient.job.create(
    adla,
    jobId,
    JobInformation(
        name='Sample Job',
        type='USql',
        properties=USqlJobProperties(script=script)
    )
)
```

## <a name="wait-for-a-job-to-end"></a>Wachten op een taak te beëindigen

```python
jobResult = adlaJobClient.job.get(adla, jobId)
while(jobResult.state != JobState.ended):
    print('Job is not yet done, waiting for 3 seconds. Current state: ' + jobResult.state.value)
    time.sleep(3)
    jobResult = adlaJobClient.job.get(adla, jobId)

print ('Job finished with result: ' + jobResult.result.value)
```

## <a name="list-pipelines-and-recurrences"></a>Lijst met pijplijnen en herhalingen
Afhankelijk van of uw taken pijplijn of terugkeerpatroon metagegevens die zijn gekoppeld hebben, kunt u pijplijnen en herhalingen weergeven.

```python
pipelines = adlaJobClient.pipeline.list(adla)
for p in pipelines:
    print('Pipeline: ' + p.name + ' ' + p.pipelineId)

recurrences = adlaJobClient.recurrence.list(adla)
for r in recurrences:
    print('Recurrence: ' + r.name + ' ' + r.recurrenceId)
```

## <a name="manage-compute-policies"></a>Compute-beleid beheren

Het object DataLakeAnalyticsAccountManagementClient biedt methoden voor het beheren van de compute-beleidsregels voor een Data Lake Analytics-account.

### <a name="list-compute-policies"></a>Lijst met compute-beleid

De volgende code haalt een lijst met compute-beleidsregels voor een Data Lake Analytics-account.

```python
policies = adlaAccountClient.computePolicies.listByAccount(rg, adla)
for p in policies:
    print('Name: ' + p.name + 'Type: ' + p.objectType + 'Max AUs / job: ' + p.maxDegreeOfParallelismPerJob + 'Min priority / job: ' + p.minPriorityPerJob)
```

### <a name="create-a-new-compute-policy"></a>Maak een nieuw beleid voor compute

De volgende code maakt een nieuw beleid voor compute voor een Data Lake Analytics-account, het maximumaantal AU's beschikbaar stellen voor de opgegeven gebruiker tot 50 en de prioriteit van de minimale job tot 250 tekens.

```python
userAadObjectId = "3b097601-4912-4d41-b9d2-78672fc2acde"
newPolicyParams = ComputePolicyCreateOrUpdateParameters(userAadObjectId, "User", 50, 250)
adlaAccountClient.computePolicies.createOrUpdate(rg, adla, "GaryMcDaniel", newPolicyParams)
```

## <a name="next-steps"></a>Volgende stappen

- Als u dezelfde zelfstudie wilt bekijken met een ander hulpprogramma, klikt u op de tabselectors boven aan de pagina.
- Zie [Aan de slag met de Azure Data Lake Analytics U-SQL-taal](data-lake-analytics-u-sql-get-started.md) om U-SQL te leren.
- Zie [Azure Data Lake Analytics beheren met Azure Portal](data-lake-analytics-manage-use-portal.md) voor informatie over beheertaken.

