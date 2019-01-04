---
title: 'Snelstart: Gegevens opnemen met behulp van de Node-bibliotheek voor Azure Data Explorer'
description: In deze snelstart leert u hoe u gegevens opneemt (laadt) in Azure Data Explorer met behulp van Node.js.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 10/25/2018
ms.openlocfilehash: a7ebceff18016a5aa527a032a644d2723aceee7a
ms.sourcegitcommit: b767a6a118bca386ac6de93ea38f1cc457bb3e4e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/18/2018
ms.locfileid: "53558563"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-node-library"></a>Snelstart: Gegevens opnemen met behulp van de Node-bibliotheek voor Azure Data Explorer

Azure Data Explorer is een snelle en zeer schaalbare service om gegevens in logboeken en telemetrie te verkennen. Azure Data Explorer biedt twee clientbibliotheken voor Node: een [ingest library](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-ingest) en een [data library](https://github.com/Azure/azure-kusto-node/tree/master/azure-kusto-data) (beide Engelstalig). Met deze bibliotheken kunt u gegevens opnemen (laden) in een cluster en gegevens bevragen vanuit uw code. In deze snelstart maakt u eerst een tabel en gegevenstoewijzing in een testcluster. Vervolgens plaatst u op te nemen gegevens in de wachtrij en valideert u de resultaten.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Naast een Azure-abonnement hebt u het volgende nodig om deze snelstart te voltooien:

* [Een cluster en database voor testdoeleinden](create-cluster-database-portal.md)

* [Node.js](https://nodejs.org/en/download/) geïnstalleerd op uw ontwikkelcomputer

## <a name="install-the-data-and-ingest-libraries"></a>De bibliotheken data en ingest installeren

Installeer *azure-kusto-ingest* en *azure-kusto-data*

```bash
npm i --save azure-kusto-ingest azure-kusto-data
```

## <a name="add-import-statements-and-constants"></a>Importinstructies en constanten toevoegen

Importeer klassen uit de bibliotheken

```javascript

const KustoConnectionStringBuilder = require("azure-kusto-data").KustoConnectionStringBuilder;
const KustoClient = require("azure-kusto-data").Client;
const KustoIngestClient = require("azure-kusto-ingest").IngestClient;
const IngestionProperties = require("azure-kusto-ingest").IngestionProperties;
const { DataFormat } = require("azure-kusto-ingest").IngestionPropertiesEnums;
const { BlobDescriptor } = require("azure-kusto-ingest").IngestionDescriptors;

```
Azure Data Explorer maakt gebruik van de id van uw Azure Active Directory-tenant om een toepassing te verifiëren. Volg [Find your Office 365 tenant ID](https://docs.microsoft.com/onedrive/find-your-office-365-tenant-id) (Uw Office 365-tenant-id zoeken) om de tenant-id te vinden.

Stel de waarden voor `authorityId`, `kustoUri`, `kustoIngestUri` en `kustoDatabase` in voordat u deze code uitvoert.

```javascript
const authorityId = "<TenantId>";
const kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443";
const kustoIngestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443";
const kustoDatabase  = "<DatabaseName>";
```

Stel nu de verbindingsreeks samen. In dit voorbeeld wordt apparaatverificatie gebruikt voor toegang tot het cluster. U kunt ook een Azure Active Directory-toepassingscertificaat, een Azure Active Directory-toepassingssleutel en een Azure Active Directory-gebruiker en -wachtwoord gebruiken.

U maakt de doeltabel en toewijzing in een latere stap.

```javascript
const kcsbIngest = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoIngestUri, authorityId);
const kcsbData = KustoConnectionStringBuilder.withAadDeviceAuthentication(kustoUri, authorityId);
const destTable = "StormEvents";
const destTableMapping = "StormEvents_CSV_Mapping";
```

## <a name="set-source-file-information"></a>Gegevens van bronbestand instellen

Importeer aanvullende klassen en stel constanten in voor het gegevensbronbestand. In dit voorbeeld wordt een voorbeeldbestand gebruikt dat wordt gehost in Azure Blob Storage. De set met voorbeeldgegevens **StormEvents** bevat gegevens van het weer afkomstig van de [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

```javascript
const container = "samplefiles";
const account = "kustosamplefiles";
const sas = "?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
const filePath = "StormEvents.csv";
const blobPath = `https://${account}.blob.core.windows.net/${container}/${filePath}${sas}`;
```

## <a name="create-a-table-on-your-test-cluster"></a>Een tabel maken in het testcluster

Maak een tabel die overeenkomt met het schema van de gegevens in het bestand `StormEvents.csv`. Wanneer deze code wordt uitgevoerd, wordt een bericht als het volgende geretourneerd: *Als u zich wilt aanmelden, opent u de pagina https://microsoft.com/devicelogin met een webbrowser. Voer de code XXXXXXXXX in om te verifiëren*. Volg de stappen om u aan te melden en ga vervolgens terug om het volgende codeblok uit te voeren. Als volgende codeblokken verbinding moeten maken, moet u zich opnieuw aanmelden.

```javascript
const kustoClient = new KustoClient(kcsbData);
const createTableCommand = `.create table ${destTable} (StartTime: datetime, EndTime: datetime, EpisodeId: int, EventId: int, State: string, EventType: string, InjuriesDirect: int, InjuriesIndirect: int, DeathsDirect: int, DeathsIndirect: int, DamageProperty: int, DamageCrops: int, Source: string, BeginLocation: string, EndLocation: string, BeginLat: real, BeginLon: real, EndLat: real, EndLon: real, EpisodeNarrative: string, EventNarrative: string, StormSummary: dynamic)`;

kustoClient.executeMgmt(kustoDatabase, createTableCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="define-ingestion-mapping"></a>Toewijzing van opname definiëren

Wijs binnenkomende CSV-gegevens toe aan de kolomnamen en gegevenstypen die zijn gebruikt bij het maken van de tabel.

```javascript
const createMappingCommand = `.create table ${destTable} ingestion csv mapping '${destTableMapping}' '[{"Name":"StartTime","datatype":"datetime","Ordinal":0}, {"Name":"EndTime","datatype":"datetime","Ordinal":1},{"Name":"EpisodeId","datatype":"int","Ordinal":2},{"Name":"EventId","datatype":"int","Ordinal":3},{"Name":"State","datatype":"string","Ordinal":4},{"Name":"EventType","datatype":"string","Ordinal":5},{"Name":"InjuriesDirect","datatype":"int","Ordinal":6},{"Name":"InjuriesIndirect","datatype":"int","Ordinal":7},{"Name":"DeathsDirect","datatype":"int","Ordinal":8},{"Name":"DeathsIndirect","datatype":"int","Ordinal":9},{"Name":"DamageProperty","datatype":"int","Ordinal":10},{"Name":"DamageCrops","datatype":"int","Ordinal":11},{"Name":"Source","datatype":"string","Ordinal":12},{"Name":"BeginLocation","datatype":"string","Ordinal":13},{"Name":"EndLocation","datatype":"string","Ordinal":14},{"Name":"BeginLat","datatype":"real","Ordinal":16},{"Name":"BeginLon","datatype":"real","Ordinal":17},{"Name":"EndLat","datatype":"real","Ordinal":18},{"Name":"EndLon","datatype":"real","Ordinal":19},{"Name":"EpisodeNarrative","datatype":"string","Ordinal":20},{"Name":"EventNarrative","datatype":"string","Ordinal":21},{"Name":"StormSummary","datatype":"dynamic","Ordinal":22}]'`;

kustoClient.executeMgmt(kustoDatabase, createMappingCommand, (err, results) => {
    console.log(result.primaryResults[0]);
});
```

## <a name="queue-a-message-for-ingestion"></a>Bericht in wachtrij zetten voor opname

Zet een bericht in de wachtrij om gegevens op te halen uit blob-opslag en die gegevens op te nemen in Azure Data Explorer.

```javascript
const defaultProps  = new IngestionProperties(kustoDatabase, destTable, DataFormat.csv, null,destTableMapping, {'ignoreFirstRecord': 'true'});
const ingestClient = new KustoIngestClient(kcsbIngest, defaultProps);
// All ingestion properties are documented here: https://docs.microsoft.com/azure/kusto/management/data-ingest#ingestion-properties

const blobDesc = new BlobDescriptor(blobPath, 10);
ingestClient.ingestFromBlob(blobDesc,null, (err) => {
    if (err) throw new Error(err);
});
```

## <a name="validate-that-table-contains-data"></a>Valideren dat de tabel gegevens bevat

Valideer dat er gegevens in de tabel zijn opgenomen. Wacht vijf tot tien minuten totdat de opname in de wachtrijde opname heeft gepland en de gegevens in Azure Data Explorer zijn geladen. Voer vervolgens de volgende code uit om het aantal records in de tabel `StormEvents` te bepalen.

```javascript
const query = `${destTable} | count`;

kustoClient.execute(kustoDatabase, query, (err, results) => {
    if (err) throw new Error(err);  
    console.log(results.primaryResults[0].toString());
});
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

Als u onze andere snelstarts en zelfstudies wilt volgen, behoudt u de gemaakte resources. Voer anders de volgende opdracht uit in uw database om de tabel `StormEvents` op te schonen.

```Kusto
.drop table StormEvents
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Query's schrijven](write-queries.md)
