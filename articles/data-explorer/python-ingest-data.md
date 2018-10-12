---
title: 'Snelstart: gegevens opnemen met behulp van de Python-bibliotheek voor Azure Data Explorer'
description: In deze snelstart leert u hoe u gegevens opneemt (laadt) in Azure Data Explorer met behulp van Python.
services: data-explorer
author: mgblythe
ms.author: mblythe
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 09/24/2018
ms.openlocfilehash: 52be08006985ee2f2e1ea4427e0f63ebbeb6e8b2
ms.sourcegitcommit: 7824e973908fa2edd37d666026dd7c03dc0bafd0
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "48900493"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-python-library"></a>Snelstart: gegevens opnemen met behulp van de Python-bibliotheek voor Azure Data Explorer

Azure Data Explorer is een snelle en zeer schaalbare service om gegevens in logboeken en telemetrie te verkennen. Azure Data Explorer biedt twee clientbibliotheken voor Python: een [ingest-bibliotheek](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-ingest) en [een data-bibliotheek](https://github.com/Azure/azure-kusto-python/tree/master/azure-kusto-data). Met deze bibliotheken kunt u gegevens opnemen (laden) in een cluster en gegevens bevragen vanuit uw code. In deze snelstart maakt u eerst een tabel en gegevenstoewijzing in een testcluster. Vervolgens plaatst u op te nemen gegevens in de wachtrij en valideert u de resultaten.

Deze snelstart is ook beschikbaar als een [Azure Notebook](https://notebooks.azure.com/ManojRaheja/libraries/KustoPythonSamples/html/QueuedIngestSingleBlob.ipynb).

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Naast een Azure-abonnement hebt u het volgende nodig om deze snelstart te voltooien:

* [Een cluster en database voor testdoeleinden](create-cluster-database-portal.md)

* [Python](https://www.python.org/downloads/) geïnstalleerd op uw ontwikkelcomputer

## <a name="install-the-data-and-ingest-libraries"></a>De bibliotheken data en ingest installeren

Installeer *azure-kusto-data* en *azure-kusto-ingest*.

```python
pip install azure-kusto-data
pip install azure-kusto-ingest
```

## <a name="add-import-statements-and-constants"></a>Importinstructies en constanten toevoegen

Importeer klassen uit de bibliotheek, evenals *dataetime* en *pandas*, een bibliotheek voor gegevensanalyse.

```python
from azure.kusto.data.request import KustoClient, KustoConnectionStringBuilder
from azure.kusto.data.exceptions import KustoServiceError
import pandas as pd
import datetime
```

Azure Data Explorer maakt gebruik van de id van uw AAD-tenant om een toepassing te verifiëren. Om uw tenant-id te vinden, gebruikt u de volgende URL, waarbij u *YourDomain* vervangt door uw domeinnaam.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Dus als uw domein *contoso.com* is, wordt de URL bijvoorbeeld: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Klik op deze URL om de resultaten weer te geven. De eerste regel is als volgt. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

De tenant-id is `6babcaad-604b-40ac-a9d7-9fd97c0b779f` in dit voorbeeld. Stel de waarden voor AAD_TENANT_ID, KUSTO_URI, KUSTO_INGEST_URI en KUSTO_DATABASE in voordat deze code wordt uitgevoerd.

```python
AAD_TENANT_ID = "<TenantId>"
KUSTO_URI = "https://<ClusterName>.<Region>.kusto.windows.net:443/"
KUSTO_INGEST_URI = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/"
KUSTO_DATABASE  = "<DatabaseName>"
```

Stel nu de verbindingsreeks samen. In dit voorbeeld wordt apparaatverificatie gebruikt voor toegang tot het cluster. U kunt ook een AAD-toepassingscertificaat, een AAD-toepassingssleutel en een AAD-gebruiker en -wachtwoord gebruiken.

U maakt de doeltabel en toewijzing in een latere stap.

```python
KCSB_INGEST = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_INGEST_URI, AAD_TENANT_ID)

KCSB_DATA = KustoConnectionStringBuilder.with_aad_device_authentication(KUSTO_URI, AAD_TENANT_ID)

DESTINATION_TABLE = "StormEvents"
DESTINATION_TABLE_COLUMN_MAPPING = "StormEvents_CSV_Mapping"
```

## <a name="set-source-file-information"></a>Gegevens van bronbestand instellen

Importeer aanvullende klassen en stel constanten in voor het gegevensbronbestand. In dit voorbeeld wordt een voorbeeldbestand gebruikt dat wordt gehost in Azure Blob Storage. De set met voorbeeldgegevens **StormEvents** bevat gegevens van het weer afkomstig van de [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

```python
from azure.storage.blob import BlockBlobService
from azure.kusto.ingest import KustoIngestClient, IngestionProperties, FileDescriptor, BlobDescriptor, DataFormat, ReportLevel, ReportMethod

CONTAINER = "samplefiles"
ACCOUNT_NAME = "kustosamplefiles"
SAS_TOKEN = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D"
FILE_PATH = "StormEvents.csv"
FILE_SIZE = 64158321    # in bytes

BLOB_PATH = "https://" + ACCOUNT_NAME + ".blob.core.windows.net/" + CONTAINER + "/" + FILE_PATH + SAS_TOKEN
```

## <a name="create-a-table-on-your-test-cluster"></a>Een tabel maken in het testcluster

Maak een tabel die overeenkomt met het schema van de gegevens in het bestand StormEvents.csv. Wanneer deze code wordt uitgevoerd, wordt er een bericht als het volgende geretourneerd: *Als u zich wilt aanmelden, opent u de pagina https://microsoft.com/devicelogin met een webbrowser. Voer de code F3W4VWZDM in om te verifiëren*. Volg de stappen om u aan te melden en ga vervolgens terug om het volgende codeblok uit te voeren. Als volgende codeblokken verbinding moeten maken, moet u zich opnieuw aanmelden.

```python
KUSTO_CLIENT = KustoClient(KCSB_ENGINE)
CREATE_TABLE_COMMAND = ".create table StormEvents (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)"

df_table_create_output = KUSTO_CLIENT.execute_mgmt(KUSTO_DATABASE, CREATE_TABLE_COMMAND).primary_results[0].to_dataframe()

df_table_create_output
```

## <a name="define-ingestion-mapping"></a>Toewijzing van opname definiëren

Wijs binnenkomende CSV-gegevens toe aan de kolomnamen en gegevenstypen die zijn gebruikt bij het maken van de tabel.

```python
CREATE_MAPPING_COMMAND = """.create table StormEvents ingestion csv mapping 'StormEvents_CSV_Mapping' '[{"Name":"StartTime","datatype":"datetime","Ordinal":0}, {"Name":"EndTime","datatype":"datetime","Ordinal":1},{"Name":"EpisodeId","datatype":"int","Ordinal":2},{"Name":"EventId","datatype":"int","Ordinal":3},{"Name":"State","datatype":"string","Ordinal":4},{"Name":"EventType","datatype":"string","Ordinal":5},{"Name":"InjuriesDirect","datatype":"int","Ordinal":6},{"Name":"InjuriesIndirect","datatype":"int","Ordinal":7},{"Name":"DeathsDirect","datatype":"int","Ordinal":8},{"Name":"DeathsIndirect","datatype":"int","Ordinal":9},{"Name":"DamageProperty","datatype":"int","Ordinal":10},{"Name":"DamageCrops","datatype":"int","Ordinal":11},{"Name":"Source","datatype":"string","Ordinal":12},{"Name":"BeginLocation","datatype":"string","Ordinal":13},{"Name":"EndLocation","datatype":"string","Ordinal":14},{"Name":"BeginLat","datatype":"real","Ordinal":16},{"Name":"BeginLon","datatype":"real","Ordinal":17},{"Name":"EndLat","datatype":"real","Ordinal":18},{"Name":"EndLon","datatype":"real","Ordinal":19},{"Name":"EpisodeNarrative","datatype":"string","Ordinal":20},{"Name":"EventNarrative","datatype":"string","Ordinal":21},{"Name":"StormSummary","datatype":"dynamic","Ordinal":22}]'"""

df_mapping_create_output = KUSTO_CLIENT.execute_mgmt(KUSTO_DATABASE, CREATE_MAPPING_COMMAND).primary_results[0].to_dataframe()

df_mapping_create_output
```

## <a name="queue-a-message-for-ingestion"></a>Bericht in wachtrij zetten voor opname

Zet een bericht in de wachtrij om gegevens op te halen uit blob-opslag en die gegevens op te nemen in Azure Data Explorer.

```python
INGESTION_CLIENT = KustoIngestClient(KCSB_INGEST)

# All ingestion properties are documented here: https://docs.microsoft.com/en-us/azure/kusto/management/data-ingest#ingestion-properties
INGESTION_PROPERTIES  = IngestionProperties(database=KUSTO_DATABASE, table=DESTINATION_TABLE, dataFormat=DataFormat.csv, mappingReference=DESTINATION_TABLE_COLUMN_MAPPING, additionalProperties={'ignoreFirstRecord': 'true'})
BLOB_DESCRIPTOR = BlobDescriptor(BLOB_PATH, FILE_SIZE)  # 10 is the raw size of the data in bytes
INGESTION_CLIENT.ingest_from_blob(BLOB_DESCRIPTOR,ingestion_properties=INGESTION_PROPERTIES)

print('Done queuing up ingestion with Azure Data Explorer')

```

## <a name="validate-that-data-was-ingested-into-the-table"></a>Controleren of er gegevens in de tabel zijn opgenomen

Wacht vijf tot tien minuten totdat de opname in de wachtrijde opname heeft gepland en de gegevens in Azure Data Explorer zijn geladen. Voer vervolgens de volgende code uit om het aantal records in de tabel StormEvents te bepalen.

```python
QUERY = "StormEvents | count"

df = KUSTO_CLIENT.execute_query(KUSTO_DATABASE, QUERY).primary_results[0].to_dataframe()

df
```

## <a name="run-troubleshooting-queries"></a>Query's voor probleemoplossing uitvoeren

Meld u aan bij [https://dataexplorer.azure.com](https://dataexplorer.azure.com) en maak verbinding met uw cluster. Voer de volgende opdracht uit in uw database om te zien of er in de afgelopen vier uur fouten zijn opgetreden tijdens het opnemen van gegevens. Vervang de naam van de database voordat u de opdracht uitvoert.
    
```Kusto
    .show ingestion failures
    | where FailedOn > ago(4h) and Database == "<DatabaseName>"
```

Voer de volgende opdracht uit om de status op te vragen van alle bewerkingen voor het opnemen van gegevens van de afgelopen vier uur. Vervang de naam van de database voordat u de opdracht uitvoert.

```Kusto
.show operations
| where StartedOn > ago(4h) and Database == "<DatabaseName>" and Operation == "DataIngestPull"
| summarize arg_max(LastUpdatedOn, *) by OperationId
```

## <a name="clean-up-resources"></a>Resources opschonen

Als u onze andere snelstarts en zelfstudies wilt volgen, behoudt u de gemaakte resources. Voer anders de volgende opdracht uit in uw database om de tabel StormEvents op te schonen.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Query's schrijven](write-queries.md)
