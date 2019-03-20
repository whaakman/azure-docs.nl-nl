---
title: Inschakelen van metrische opslaggegevens in Azure portal | Microsoft Docs
description: Het inschakelen van metrische gegevens van storage voor de services Blob, wachtrij, tabel en bestand
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: rogarana
ms.subservice: common
ms.openlocfilehash: 770a494e93a51fc018b2bfe803ac15ba543f35d4
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58011344"
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Metrische gegevens van Azure Storage inschakelen en weergeven van metrische gegevens
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Overzicht
Metrische opslaggegevens is standaard ingeschakeld wanneer u een nieuw opslagaccount maakt. U kunt configureren met het bewaken via de [Azure-portal](https://portal.azure.com) of Windows PowerShell, of via een programma via een van de opslagclientbibliotheken.

U kunt configureren voor de metrische gegevens een retentieperiode: dit wordt bepaald hoe lang de opslag van de service blijft kosten voor de metrische gegevens en u voor de ruimte nodig is om op te slaan ze. Normaal gesproken moet u een kortere retentieperiode voor één minuut metrische gegevens dan per uur metrische gegevens vanwege de aanzienlijke extra ruimte die nodig zijn voor één minuut metrische gegevens. Kies een bewaarperiode zodat er voldoende tijd voor het analyseren van de gegevens en downloaden van alle metrische gegevens die u wilt behouden voor offline analyse en rapportage. Houd er rekening mee dat u ook kosten in rekening gebracht worden voor het downloaden van metrische gegevens van uw opslagaccount.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Het inschakelen van metrische gegevens met behulp van de Azure portal
Volg deze stappen voor het inschakelen van metrische gegevens in de [Azure-portal](https://portal.azure.com):

1. Ga naar uw opslagaccount.
1. Selecteer **Diagnostics** uit de **Menu** deelvenster.
1. Zorg ervoor dat **Status** is ingesteld op **op**.
1. Selecteer de metrische gegevens voor de services die u wilt bewaken.
1. Geef een bewaarbeleid om aan te geven hoe lang bewaren van metrische gegevens en logboekgegevens.
1. Selecteer **Opslaan**.

De [Azure-portal](https://portal.azure.com) wordt niet op dit moment kunt u configureren minuut metrische gegevens in uw storage-account; u moet inschakelen minuut metrische gegevens met behulp van PowerShell of via een programma.

## <a name="how-to-enable-metrics-using-powershell"></a>Het inschakelen van metrische gegevens met behulp van PowerShell

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U kunt PowerShell gebruiken op uw lokale machine metrische opslaggegevens in uw storage-account configureren met behulp van de Azure PowerShell-cmdlet Get-AzStorageServiceMetricsProperty de huidige instellingen op te halen en de cmdlet Set-AzStorageServiceMetricsProperty de huidige instellingen wijzigen.

De cmdlets waarmee metrische opslaggegevens gebruik de volgende parameters:

* MetricsType: mogelijke waarden zijn uur en minuut.
* ServiceType: mogelijke waarden zijn Blob, wachtrijen en tabellen.
* MetricsLevel: mogelijke waarden zijn geen, Service en ServiceAndApi.

De volgende opdracht wordt bijvoorbeeld met de periode is ingesteld op vijf dagen bewaren op minuut metrische gegevens voor de Blob-service in uw storage-standaardaccount:

```powershell
Set-AzStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

De volgende opdracht haalt de huidige per uur metrische gegevens niveau en retentie dagen voor de blob-service in uw storage-standaardaccount:

```powershell
Get-AzStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

Zie voor informatie over het configureren van de Azure PowerShell-cmdlets om te werken met uw Azure-abonnement en het selecteren van het standaardopslagaccount te gebruiken: [Hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Programmatisch metrische opslaggegevens inschakelen
De volgende C#-fragment toont hoe u metrische gegevens en logboekregistratie voor de Blob-service met behulp van de storage-clientbibliotheek voor .NET inschakelen:

```csharp
//Parse the connection string for the storage account.
const string ConnectionString = "DefaultEndpointsProtocol=https;AccountName=account-name;AccountKey=account-key";
CloudStorageAccount storageAccount = CloudStorageAccount.Parse(ConnectionString);

// Create service client for credentialed access to the Blob service.
CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

// Enable Storage Analytics logging and set retention policy to 10 days.
ServiceProperties properties = new ServiceProperties();
properties.Logging.LoggingOperations = LoggingOperations.All;
properties.Logging.RetentionDays = 10;
properties.Logging.Version = "1.0";

// Configure service properties for metrics. Both metrics and logging must be set at the same time.
properties.HourMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.HourMetrics.RetentionDays = 10;
properties.HourMetrics.Version = "1.0";

properties.MinuteMetrics.MetricsLevel = MetricsLevel.ServiceAndApi;
properties.MinuteMetrics.RetentionDays = 10;
properties.MinuteMetrics.Version = "1.0";

// Set the default service version to be used for anonymous requests.
properties.DefaultServiceVersion = "2015-04-05";

// Set the service properties.
blobClient.SetServiceProperties(properties);
```

## <a name="viewing-storage-metrics"></a>Weergeven van metrische opslaggegevens
Na het configureren van Storage Analytics metrics voor het bewaken van uw storage-account registreert Storage Analytics de metrische gegevens in een set met bekende tabellen in uw opslagaccount. U kunt grafieken voor het weergeven van metrische gegevens van per uur in configureren de [Azure-portal](https://portal.azure.com):

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Selecteer **metrische gegevens** in de **Menu** deelvenster voor de service waarvan metrische gegevens die u wilt weergeven.
1. Selecteer **bewerken** op de grafiek die u wilt configureren.
1. In de **grafiek bewerken** venster de **tijdsbereik**, **grafiektype**, en de metrische gegevens die u weergeven in de grafiek wilt.
1. Selecteer **OK**

Als u wilt voor het downloaden van de metrische gegevens voor langetermijnopslag of deze lokaal analyseert, moet u naar:

* Gebruik een hulpprogramma dat op de hoogte van deze tabellen en kunt u weergeven en deze te downloaden.
* Een aangepaste toepassing of script om te lezen en sla de tabellen worden geschreven.

Veel opslag bladeren hulpprogramma's van derden zijn van deze tabellen op de hoogte en kunnen u ze rechtstreeks weergeven.
Zie [Azure Storage-clienthulpprogramma's](storage-explorers.md) voor een lijst van beschikbare hulpprogramma's.

> [!NOTE]
> Vanaf versie 0.8.0 van de [Microsoft Azure Storage Explorer](https://storageexplorer.com/), u kunt bekijken en downloaden van de metrische gegevens analytics tabellen.
>
>

Als u de tabellen analytics via een programma opent, houd er rekening mee dat de analytics-tabellen niet weergegeven worden als u een lijst met alle tabellen uw storage-account. U kunt toegang tot deze rechtstreeks door de naam of gebruik de [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in de .NET-clientbibliotheek om op te vragen van de tabelnamen van de.

### <a name="hourly-metrics"></a>Per uur metrische gegevens
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuut metrische gegevens
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Capaciteit
* $MetricsCapacityBlob

Volledige details van de schema's vindt u voor deze tabellen aan [tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343264.aspx). Het onderstaande voorbeeldrijen slechts een subset van de beschikbare kolommen weergeven, maar illustratie van enkele belangrijke functies van de manier waarop die metrische opslaggegevens Hiermee slaat u deze metrische gegevens:

| PartitionKey | RowKey | Tijdstempel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Beschikbaarheid | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |user;All |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |user;QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |user;QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |user;UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

In dit voorbeeld minuut metrische gegevens gebruikt de partitiesleutel het tijd resolutie van één minuut. De rijsleutel identificeert het type informatie die is opgeslagen in de rij. De rijsleutel bestaat uit twee soorten informatie, het toegangstype en het aanvraagtype:

* Het toegangstype is gebruiker of systeem, waar gebruikers verwijst naar alle aanvragen van gebruikers met de storage-service en system verwijst naar aanvragen van Storage Analytics.
* Het aanvraagtype is in dat geval moeten er is een samenvatting regel of de specifieke API zoals QueryEntity of UpdateEntity wordt aangeduid.

De bovenstaande voorbeeldgegevens bevat alle records voor één minuut (vanaf 11:00 uur), zodat het aantal aanvragen QueryEntities plus het aantal QueryEntity aanvragen plus het aantal UpdateEntity toevoegen tot zeven, dit het totale aantal is aanvragen wordt weergegeven op de rij van de gebruiker: All. Op dezelfde manier de gemiddelde end-to-end-latentie 104.4286 op de rij van de gebruiker: All kunnen worden afgeleid door het berekenen van ((143.8 * 5) + 3 + 9) / 7.

Houd er rekening mee dat **per uur metrische instellingen Blob** worden toegepast op beide **Blob capaciteit metriek** ($MetricsCapacityBlob) en **per uur Blob metrische gegevens die transactie** ($ MetricsHourPrimaryTransactionsBlob). Beide zijn ingeschakeld of uitgeschakeld samen en gebruiken dezelfde bewaarbeleid.

## <a name="metrics-alerts"></a>Waarschuwingen voor metrische gegevens
Overweeg het instellen van waarschuwingen in de [Azure-portal](https://portal.azure.com) zodat metrische opslaggegevens kan automatisch een melding van belangrijke wijzigingen in het gedrag van uw storage-services. Als u een hulpprogramma met storage explorer gebruiken om te downloaden van deze metrische gegevens in een door tekens gescheiden indeling, kunt u Microsoft Excel gebruiken om de gegevens te analyseren. Zie [Azure Storage-clienthulpprogramma's](storage-explorers.md) voor een lijst van beschikbare opslag explorer-hulpprogramma's. U kunt waarschuwingen in configureren de **waarschuwingsregels** deelvenster toegankelijk is onder **bewaking** in het menu Storage-account.

> [!IMPORTANT]
> Er is mogelijk een vertraging tussen een opslag-gebeurtenis en wanneer de bijbehorende per uur of minuut metrische gegevens wordt vastgelegd. Wanneer u zich minuut metrische gegevens, kunnen enkele minuten aan gegevens in één keer worden geschreven. Transacties van eerdere minuten kunnen vervolgens worden samengevoegd in de transactie voor de huidige minuut. Als dit gebeurt, de waarschuwings-service mogelijk geen gegevens voor alle beschikbare metrische gegevens voor de geconfigureerde waarschuwingen interval, wat kan leiden tot onverwacht cursorparameter waarschuwingen.
>

## <a name="accessing-metrics-data-programmatically"></a>Openen van metrische gegevens via een programma
De volgende vermelding toont een voorbeeld van C#-code die toegang heeft tot de minuut metrische gegevens voor een aantal minuten en de resultaten worden weergegeven in een console-venster. De Azure Storage-bibliotheek versie 4 met de CloudAnalyticsClient-klasse die het vereenvoudigt de toegang tot de tabellen voor metrische gegevens in de opslag wordt gebruikt.

```csharp
private static void PrintMinuteMetrics(CloudAnalyticsClient analyticsClient, DateTimeOffset startDateTime, DateTimeOffset endDateTime)
{
    // Convert the dates to the format used in the PartitionKey
    var start = startDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");
    var end = endDateTime.ToUniversalTime().ToString("yyyyMMdd'T'HHmm");

    var services = Enum.GetValues(typeof(StorageService));
    foreach (StorageService service in services)
    {
        Console.WriteLine("Minute Metrics for Service {0} from {1} to {2} UTC", service, start, end);
        var metricsQuery = analyticsClient.CreateMinuteMetricsQuery(service, StorageLocation.Primary);
        var t = analyticsClient.GetMinuteMetricsTable(service);
        var opContext = new OperationContext();
        var query =
          from entity in metricsQuery
          // Note, you can't filter using the entity properties Time, AccessType, or TransactionType
          // because they are calculated fields in the MetricsEntity class.
          // The PartitionKey identifies the DataTime of the metrics.
          where entity.PartitionKey.CompareTo(start) >= 0 && entity.PartitionKey.CompareTo(end) <= 0
        select entity;

        // Filter on "user" transactions after fetching the metrics from Table Storage.
        // (StartsWith is not supported using LINQ with Azure table storage)
        var results = query.ToList().Where(m => m.RowKey.StartsWith("user"));
        var resultString = results.Aggregate(new StringBuilder(), (builder, metrics) => builder.AppendLine(MetricsString(metrics, opContext))).ToString();
        Console.WriteLine(resultString);
    }
}

private static string MetricsString(MetricsEntity entity, OperationContext opContext)
{
    var entityProperties = entity.WriteEntity(opContext);
    var entityString =
      string.Format("Time: {0}, ", entity.Time) +
      string.Format("AccessType: {0}, ", entity.AccessType) +
      string.Format("TransactionType: {0}, ", entity.TransactionType) +
      string.Join(",", entityProperties.Select(e => new KeyValuePair<string, string>(e.Key.ToString(), e.Value.PropertyAsObject.ToString())));
    return entityString;
}
```

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Op welke kosten moet u rekenen wanneer u metrische opslaggegevens inschakelen?
Aanvragen voor het maken van tabelentiteiten voor metrische gegevens in rekening gebracht tegen de standaardtarieven van toepassing op alle Azure Storage-bewerkingen worden geschreven.

Lees- en delete-aanvragen door een client naar metrische gegevens zijn ook te factureren tegen standaardtarieven. Als u een beleid voor Gegevensretentie hebt geconfigureerd, u betaalt geen Azure Storage verwijdert de oude metrische gegevens. Echter, als u analytics-gegevens verwijdert, uw account wordt in rekening gebracht voor de delete-bewerkingen.

De capaciteit die wordt gebruikt door de tabellen voor metrische gegevens is ook factureerbare: u kunt het volgende gebruiken om te schatten van de hoeveelheid capaciteit die wordt gebruikt voor het opslaan van metrische gegevens:

* Als elk uur een service gebruikmaakt van elke API in elke service, wordt ongeveer 148KB aan gegevens per uur in de tabellen van de transactie metrische gegevens opgeslagen als u zowel service als API-niveau samenvatting hebt ingeschakeld.
* Als elk uur een service gebruikmaakt van elke API in elke service, wordt ongeveer 12KB aan gegevens per uur in de tabellen van de transactie metrische gegevens opgeslagen als u alleen serviceniveau samenvatting hebt ingeschakeld.
* De gegevenstabel voor capaciteit voor blobs bevat twee rijen toegevoegd van elke dag (in de opgegeven gebruiker heeft aangemeld voor Logboeken): dit betekent dat elke dag de grootte van deze tabel wordt verhoogd door maximaal ongeveer 300 bytes.

## <a name="next-steps"></a>Volgende stappen
[Inschakelen van Storage logboekregistratie en toegang krijgen tot logboekgegevens](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
