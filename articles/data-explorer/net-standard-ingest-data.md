---
title: 'Quickstart: Gegevens opnemen met behulp van de .NET Standard SDK voor Azure Data Explorer (preview)'
description: In deze quickstart leert u hoe u gegevens opneemt (laadt) in Azure Data Explorer met behulp van .NET Standard SDK.
services: data-explorer
author: orspod
ms.author: v-orspod
ms.reviewer: mblythe
ms.service: data-explorer
ms.topic: quickstart
ms.date: 11/18/2018
ms.openlocfilehash: b0e8c4dabea6aeae8d93d64d97b598ec97b2d18a
ms.sourcegitcommit: 8d88a025090e5087b9d0ab390b1207977ef4ff7c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2018
ms.locfileid: "52277073"
---
# <a name="quickstart-ingest-data-using-the-azure-data-explorer-net-standard-sdk-preview"></a>Quickstart: Gegevens opnemen met behulp van de .NET Standard SDK voor Azure Data Explorer (preview)

Azure Data Explorer (ADX) is een snelle en zeer schaalbare service om gegevens in logboeken en telemetrie te verkennen. ADX biedt twee clientbibliotheken voor .NET Standard: een [opnamebibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Ingest.NETStandard) en een [gegevensbibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Kusto.Data.NETStandard). Met deze bibliotheken kunt u gegevens opnemen (laden) in een cluster en gegevens bevragen vanuit uw code. In deze snelstart maakt u eerst een tabel en gegevenstoewijzing in een testcluster. Vervolgens plaatst u op te nemen gegevens in de wachtrij en valideert u de resultaten.

## <a name="prerequisites"></a>Vereisten

* Als u nog geen abonnement op Azure hebt, maak dan een [gratis Azure-account](https://azure.microsoft.com/free/) aan voordat u begint.

* [Een cluster en database voor testdoeleinden](create-cluster-database-portal.md)

## <a name="install-the-ingest-library"></a>De opnamebibliotheek installeren

```
Install-Package Microsoft.Azure.Kusto.Ingest.NETStandard
```

## <a name="authentication"></a>Verificatie

Azure Data Explorer maakt gebruik van de id van uw AAD-tenant om een toepassing te verifiëren. Om uw tenant-id te vinden, gebruikt u de volgende URL, waarbij u *YourDomain* vervangt door uw domeinnaam.

```
https://login.windows.net/<YourDomain>/.well-known/openid-configuration/
```

Dus als uw domein *contoso.com* is, wordt de URL bijvoorbeeld: [https://login.windows.net/contoso.com/.well-known/openid-configuration/](https://login.windows.net/contoso.com/.well-known/openid-configuration/). Klik op deze URL om de resultaten weer te geven. De eerste regel is als volgt. 

```
"authorization_endpoint":"https://login.windows.net/6babcaad-604b-40ac-a9d7-9fd97c0b779f/oauth2/authorize"
```

De tenant-id is `6babcaad-604b-40ac-a9d7-9fd97c0b779f` in dit voorbeeld.

In dit voorbeeld worden een gebruikersnaam en wachtwoord van ADD gebruikt voor toegang tot het cluster. U kunt ook een AAD-toepassingscertificaat en een AAD-toepassingssleutel gebruiken. Stel de waarden in voor `tenantId`, `user` en `password` voordat u deze code uitvoert.

```csharp
var tenantId = "<TenantId>";
var user = "<User>";
var password = "<Password>";
```

## <a name="construct-the-connection-string"></a>De verbindingsreeks samenstellen
Stel nu de verbindingsreeks samen. U maakt de doeltabel en toewijzing in een latere stap.

```csharp
var kustoUri = "https://<ClusterName>.<Region>.kusto.windows.net:443/";
var database = "<DatabaseName>";

var kustoConnectionStringBuilder =
    new KustoConnectionStringBuilder(kustoUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };
```

## <a name="set-source-file-information"></a>Gegevens van bronbestand instellen

Stel de constanten in voor het gegevensbronbestand. In dit voorbeeld wordt een voorbeeldbestand gebruikt dat wordt gehost in Azure Blob Storage. De set met voorbeeldgegevens **StormEvents** bevat gegevens van het weer afkomstig van de [National Centers for Environmental Information](https://www.ncdc.noaa.gov/stormevents/).

```csharp
var blobPath = "https://kustosamplefiles.blob.core.windows.net/samplefiles/StormEvents.csv?st=2018-08-31T22%3A02%3A25Z&se=2020-09-01T22%3A02%3A00Z&sp=r&sv=2018-03-28&sr=b&sig=LQIbomcKI8Ooz425hWtjeq6d61uEaq21UVX7YrM61N4%3D";
```

## <a name="create-a-table-on-your-test-cluster"></a>Een tabel maken in het testcluster
Maak een tabel die overeenkomt met het schema van de gegevens in het bestand `StormEvents.csv`. Wanneer deze code wordt uitgevoerd, wordt er een bericht als het volgende geretourneerd: *Als u zich wilt aanmelden, opent u de pagina https://microsoft.com/devicelogin met een webbrowser. Voer de code F3W4VWZDM in om te verifiëren*. Volg de stappen om u aan te melden en ga vervolgens terug om het volgende codeblok uit te voeren. Als volgende codeblokken verbinding moeten maken, moet u zich opnieuw aanmelden.

```csharp
var table = "StormEvents";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCreateCommand(
            table,
            new[]
            {
                Tuple.Create("StartTime", "System.DateTime"),
                Tuple.Create("EndTime", "System.DateTime"),
                Tuple.Create("EpisodeId", "System.Int32"),
                Tuple.Create("EventId", "System.Int32"),
                Tuple.Create("State", "System.String"),
                Tuple.Create("EventType", "System.String"),
                Tuple.Create("InjuriesDirect", "System.Int32"),
                Tuple.Create("DeathsDirect", "System.Int32"),
                Tuple.Create("DeathsIndirect", "System.Int32"),
                Tuple.Create("DamageProperty", "System.Int32"),
                Tuple.Create("DamageCrops", "System.Int32"),
                Tuple.Create("Source", "System.String"),
                Tuple.Create("BeginLocation", "System.String"),
                Tuple.Create("EndLocation", "System.String"),
                Tuple.Create("BeginLat", "System.Double"),
                Tuple.Create("BeginLon", "System.Double"),
                Tuple.Create("EndLat", "System.Double"),
                Tuple.Create("EndLon", "System.Double"),
                Tuple.Create("EpisodeNarrative", "System.String"),
                Tuple.Create("EventNarrative", "System.String"),
                Tuple.Create("StormSummary", "System.Object"),
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="define-ingestion-mapping"></a>Toewijzing van opname definiëren

Wijs binnenkomende CSV-gegevens toe aan de kolomnamen en gegevenstypen die zijn gebruikt bij het maken van de tabel.
Richt een [CSV-kolomtoewijzingsobject](/azure/kusto/management/tables#create-ingestion-mapping) in voor deze tabel

```csharp
var tableMapping = "StormEvents_CSV_Mapping";
using (var kustoClient = KustoClientFactory.CreateCslAdminProvider(kustoConnectionStringBuilder))
{
    var command =
        CslCommandGenerator.GenerateTableCsvMappingCreateCommand(
            table,
            tableMapping,
            new[]
            {
                new CsvColumnMapping { ColumnName = "StartTime", Ordinal = 0 },
                new CsvColumnMapping { ColumnName = "EndTime", Ordinal = 1 },
                new CsvColumnMapping { ColumnName = "EventId", Ordinal = 3 },
                new CsvColumnMapping { ColumnName = "State", Ordinal = 4 },
                new CsvColumnMapping { ColumnName = "InjuriesDirect", Ordinal = 6 },
                new CsvColumnMapping { ColumnName = "InjuriesIndirect", Ordinal = 7 },
                new CsvColumnMapping { ColumnName = "DeathsDirect", Ordinal = 8 },
                new CsvColumnMapping { ColumnName = "DeathsIndirect", Ordinal = 9 },
                new CsvColumnMapping { ColumnName = "DamageProperty", Ordinal = 10 },
                new CsvColumnMapping { ColumnName = "DamageCrops", Ordinal = 11 },
                new CsvColumnMapping { ColumnName = "Source", Ordinal = 12 },
                new CsvColumnMapping { ColumnName = "BeginLocation", Ordinal = 13 },
                new CsvColumnMapping { ColumnName = "EndLocation", Ordinal = 14 },
                new CsvColumnMapping { ColumnName = "BeginLat", Ordinal = 16 },
                new CsvColumnMapping { ColumnName = "BeginLon", Ordinal = 17 },
                new CsvColumnMapping { ColumnName = "EndLat", Ordinal = 18 },
                new CsvColumnMapping { ColumnName = "EndLon", Ordinal = 19 },
                new CsvColumnMapping { ColumnName = "EpisodeNarrative", Ordinal = 20 },
                new CsvColumnMapping { ColumnName = "EventNarrative", Ordinal = 21 },
                new CsvColumnMapping { ColumnName = "StormSummary", Ordinal = 22 },
            });

    kustoClient.ExecuteControlCommand(command);
}
```

## <a name="queue-a-message-for-ingestion"></a>Bericht in wachtrij zetten voor opname

Zet een bericht in de wachtrij om gegevens op te halen uit blob-opslag en deze gegevens op te nemen in ADX.

```csharp
var ingestUri = "https://ingest-<ClusterName>.<Region>.kusto.windows.net:443/";
var ingestConnectionStringBuilder =
    new KustoConnectionStringBuilder(ingestUri)
    {
        FederatedSecurity = true,
        InitialCatalog = database,
        UserID = user,
        Password = password,
        Authority = tenantId
    };

using (var ingestClient = KustoIngestFactory.CreateQueuedIngestClient(ingestConnectionStringBuilder))
{
    var properties =
        new KustoQueuedIngestionProperties(database, table)
        {
            Format = DataSourceFormat.csv,
            CSVMappingReference = tableMapping,
            IgnoreFirstRecord = true
        };

    ingestClient.IngestFromSingleBlob(blobPath, deleteSourceOnSuccess: false, ingestionProperties: properties);
}
```

## <a name="validate-data-was-ingested-into-the-table"></a>Controleren of gegevens zijn opgenomen in de tabel

Wacht vijf tot tien minuten totdat de opname in de wachtrij de opname heeft gepland en de gegevens zijn geladen in ADX. Voer vervolgens de volgende code uit om het aantal records in de tabel StormEvents te bepalen.

```csharp
using (var cslQueryProvider = KustoClientFactory.CreateCslQueryProvider(kustoConnectionStringBuilder))
{
    var query = "StormEvents | count";

    var results = cslQueryProvider.ExecuteQuery<long>(query);
    Console.WriteLine(results.Single());
}
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