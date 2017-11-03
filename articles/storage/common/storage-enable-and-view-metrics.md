---
title: Inschakelen van metrische gegevens voor opslag in de Azure portal | Microsoft Docs
description: Het inschakelen van metrische gegevens voor de services Blob, Queue, Table en bestand storage
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 0407adfc-2a41-4126-922d-b76e90b74563
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 02/14/2017
ms.author: tamram
ms.openlocfilehash: 8abb4f968c1fa84e03c8cc807826d3684713847a
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enabling-azure-storage-metrics-and-viewing-metrics-data"></a>Inschakelen van Azure Storage metrische gegevens en metrische gegevens weergeven
[!INCLUDE [storage-selector-portal-enable-and-view-metrics](../../../includes/storage-selector-portal-enable-and-view-metrics.md)]

## <a name="overview"></a>Overzicht
Metrische gegevens Storage is standaard ingeschakeld wanneer u een nieuw opslagaccount maken. U kunt configureren bewaking de [Azure-portal](https://portal.azure.com) of Windows PowerShell of via een programma via een van de clientbibliotheken storage.

U kunt een bewaarperiode voor de metrische gegevens configureren: deze periode bepaalt hoe lang de opslag van de service houdt de metrische gegevens en de kosten die u voor de ruimte vereist voor het opslaan van deze. Normaal gesproken moet u een kortere bewaartermijn voor minuut metrieken dan elk uur metrische gegevens vanwege de aanzienlijke extra ruimte nodig zijn voor de minuut metrische gegevens. Kies een bewaarperiode zodanig dat er voldoende tijd om de gegevens analyseren en eventuele metrische gegevens die u wilt behouden voor offline-analyse en rapportage te downloaden. Houd er rekening mee dat u ook wordt gefactureerd voor het downloaden van metrische gegevens van uw opslagaccount.

## <a name="how-to-enable-metrics-using-the-azure-portal"></a>Het inschakelen van metrische gegevens met Azure portal
Volg deze stappen voor het inschakelen van metrische gegevens in de [Azure-portal](https://portal.azure.com):

1. Navigeer naar uw storage-account.
1. Selecteer **Diagnostics** van de **Menu** deelvenster.
1. Zorg ervoor dat **Status** is ingesteld op **op**.
1. Selecteer de metrische gegevens voor de services die u wilt bewaken.
1. Geef een bewaarbeleid om aan te geven hoe lang metrische gegevens behouden en gegevens aanmelden.
1. Selecteer **Opslaan**.

De [Azure-portal](https://portal.azure.com) momenteel kunnen niet u minuut metrieken configureren in uw opslagaccount, moet u minuut metrische gegevens met behulp van PowerShell inschakelen of programmatisch.

## <a name="how-to-enable-metrics-using-powershell"></a>Het inschakelen van metrische gegevens met behulp van PowerShell
U kunt PowerShell gebruiken op uw lokale machine opslag metrische gegevens in uw opslagaccount configureren via de Azure PowerShell-cmdlet Get-AzureStorageServiceMetricsProperty de huidige instellingen op te halen en de cmdlet Set-AzureStorageServiceMetricsProperty de huidige instellingen te wijzigen.

De cmdlets waarmee de metrische gegevens Storage gebruiken de volgende parameters:

* MetricsType: mogelijke waarden zijn uren en minuten.
* ServiceType: mogelijke waarden zijn Blob, wachtrijen en tabellen.
* MetricsLevel: mogelijke waarden zijn geen, Service en ServiceAndApi.

De volgende opdracht wordt bijvoorbeeld met de periode is ingesteld op vijf dagen bewaren op minuut metrische gegevens voor de Blob-service in uw storage-standaardaccount:

```powershell
Set-AzureStorageServiceMetricsProperty -MetricsType Minute -ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5`
```

De volgende opdracht wordt de huidige per uur metrische gegevens niveau en retentie dagen voor de blob-service in uw storage-standaardaccount opgehaald:

```powershell
Get-AzureStorageServiceMetricsProperty -MetricsType Hour -ServiceType Blob
```

Zie voor informatie over het configureren van de Azure PowerShell-cmdlets voor het werken met uw Azure-abonnement en hoe u het standaardopslagaccount moet worden gebruikt, selecteert: [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

## <a name="how-to-enable-storage-metrics-programmatically"></a>Metrische gegevens Storage programmatisch inschakelen
Het volgende C#-fragment toont metrische gegevens en logboekregistratie in voor de Blob-service met de storage-clientbibliotheek voor .NET inschakelen:

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

## <a name="viewing-storage-metrics"></a>Opslag metrische gegevens weergeven
Nadat u opslag Analytics metrische gegevens voor het bewaken van uw storage-account configureert, registreert Opslaganalyse de metrische gegevens in een verzameling van bekende tabellen in uw opslagaccount. U kunt grafieken om weer te geven per uur metrische gegevens in de [Azure-portal](https://portal.azure.com):

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Selecteer **metrische gegevens** in de **Menu** deelvenster voor de service waarvan metrische gegevens die u wilt weergeven.
1. Selecteer **bewerken** op de grafiek die u wilt configureren.
1. In de **grafiek bewerken** deelvenster, selecteer de **tijdsbereik**, **grafiektype**, en de metrische gegevens die u weergeven in de grafiek wilt.
1. Selecteer **OK**

Als u wilt downloaden van de metrische gegevens voor langdurige opslag of ze lokaal te analyseren, moet u naar:

* Gebruik een hulpprogramma waarmee de hoogte is van deze tabellen en kunt u bekijken en deze te downloaden.
* Schrijven van een aangepaste toepassing of script om te lezen en sla de tabellen.

Veel opslag bladeren hulpprogramma's van derden zich bewust bent van deze tabellen en kunnen u ze rechtstreeks weergeven.
Zie [hulpprogramma's voor Azure Storage Client](storage-explorers.md) voor een lijst met beschikbare hulpprogramma's.

> [!NOTE]
> Vanaf versie 0.8.0 van de [Microsoft Azure Storage Explorer](http://storageexplorer.com/), u kunt weergeven en downloaden van de metrische gegevens analytics tabellen.
> 
> 

Om de tabellen analytics programmatisch toegang, te Houd er rekening mee dat de analytics tabellen niet weergegeven worden als u alle tabellen weergeven die in uw opslagaccount. U kunt toegang tot deze rechtstreeks met de naam of gebruik de [CloudAnalyticsClient API](https://msdn.microsoft.com/library/azure/microsoft.windowsazure.storage.analytics.cloudanalyticsclient.aspx) in de .NET-clientbibliotheek query uitvoeren op de tabelnamen van de.

### <a name="hourly-metrics"></a>Elk uur metrische gegevens
* $MetricsHourPrimaryTransactionsBlob
* $MetricsHourPrimaryTransactionsTable
* $MetricsHourPrimaryTransactionsQueue

### <a name="minute-metrics"></a>Minuut metrische gegevens
* $MetricsMinutePrimaryTransactionsBlob
* $MetricsMinutePrimaryTransactionsTable
* $MetricsMinutePrimaryTransactionsQueue

### <a name="capacity"></a>Capaciteit
* $MetricsCapacityBlob

U vindt alle details van de schema's voor deze tabellen op [Storage Analytics metrische gegevens tabelschema](https://msdn.microsoft.com/library/azure/hh343264.aspx). Het onderstaande voorbeeld rijen alleen een subset van de beschikbare kolommen weergeven, maar illustratie van enkele belangrijke functies van de manier waarop die metrische gegevens Storage Hiermee slaat u deze metrische gegevens:

| PartitionKey | RowKey | tijdstempel | TotalRequests | TotalBillableRequests | TotalIngress | TotalEgress | Beschikbaarheid | AverageE2ELatency | AverageServerLatency | PercentSuccess |
| --- |:---:| ---:| --- | --- | --- | --- | --- | --- | --- | --- |
| 20140522T1100 |gebruiker. Alle |2014-05-22T11:01:16.7650250Z |7 |7 |4003 |46801 |100 |104.4286 |6.857143 |100 |
| 20140522T1100 |gebruiker. QueryEntities |2014-05-22T11:01:16.7640250Z |5 |5 |2694 |45951 |100 |143.8 |7.8 |100 |
| 20140522T1100 |gebruiker. QueryEntity |2014-05-22T11:01:16.7650250Z |1 |1 |538 |633 |100 |3 |3 |100 |
| 20140522T1100 |gebruiker. UpdateEntity |2014-05-22T11:01:16.7650250Z |1 |1 |771 |217 |100 |9 |6 |100 |

In dit voorbeeld minuut metrische gegevens gebruikt de partitiesleutel de tijd in minuten resolutie. De rijsleutel identificeert het type informatie die is opgeslagen in de rij. De rijsleutel bestaat uit twee delen van gegevens, het toegangstype en het aanvraagtype:

* Het toegangstype is gebruiker of systeem, waarbij gebruiker verwijst naar alle aanvragen van gebruikers voor de storage-service en system verwijst naar aanvragen door Storage Analytics.
* Het aanvraagtype is in dat geval moeten er wordt een samenvatting lijn, of het de specifieke API zoals QueryEntity of UpdateEntity identificeert.

De bovenstaande voorbeeldgegevens toont alle records voor één minuut (te beginnen om 11:00 A.M.), zodat het aantal aanvragen QueryEntities plus het aantal QueryEntity aanvragen plus het aantal UpdateEntity toevoegen tot zeven, dit het totale aantal is aanvragen wordt weergegeven op de rij voor gebruiker: All. U kunt ook de gemiddelde end-to-end-latentie 104.4286 op de rij voor gebruiker: All afleiden door te berekenen ((143.8 * 5) + 3 + 9) / 7.

## <a name="metrics-alerts"></a>Waarschuwingen van de metrische gegevens
U moet overwegen om waarschuwingen in de [Azure-portal](https://portal.azure.com) zodat metrische gegevens Storage kan automatisch een melding van belangrijke wijzigingen in het gedrag van uw storage-services. Als u een storage explorer-hulpprogramma gebruiken om de gegevens van deze metrische gegevens in een indeling met scheidingstekens te downloaden, kunt u Microsoft Excel gebruiken om de gegevens te analyseren. Zie [hulpprogramma's voor Azure Storage Client](storage-explorers.md) voor een lijst met beschikbare opslag explorer-hulpprogramma's. Kunt u waarschuwingen in de **waarschuwing regels** deelvenster toegankelijk is onder **bewaking** in het deelvenster Storage account menu.

> [!IMPORTANT]
> Er is mogelijk een vertraging tussen een gebeurtenis voor opslag en wanneer de bijbehorende per uur of minuut metrische gegevens worden geregistreerd. Tijdens de registratie van minuut metrische gegevens kunnen enkele minuten van gegevens in één keer worden geschreven. Transacties uit eerdere minuten kunnen vervolgens worden samengevoegd in de transactie voor de huidige minuut. Als dit gebeurt, kan waarschuwing heeft de service niet alle gegevens van de beschikbare metrische gegevens voor de geconfigureerde waarschuwingen interval, wat kan leiden tot onverwacht geactiveerd waarschuwingen.
>

## <a name="accessing-metrics-data-programmatically"></a>Programmatisch toegang krijgen tot gegevens van de metrische gegevens
De volgende code toont voorbeeld C#-code die toegang heeft tot de minuut metrische gegevens voor een aantal minuten en de resultaten weergeven in een venster-console. De Azure Storage-bibliotheek versie 4 met de CloudAnalyticsClient-klasse die vereenvoudigt de toegang tot de metrische gegevens tabellen in de opslag wordt gebruikt.

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

## <a name="what-charges-do-you-incur-when-you-enable-storage-metrics"></a>Welke kosten worden u wanneer u opslag metrische gegevens inschakelen?
Schrijven naar aanvragen voor het maken van de tabelentiteiten voor de metrische gegevens worden in rekening gebracht tegen standaardtarieven van toepassing op alle Azure Storage-bewerkingen.

Lees- en delete-aanvragen door een client naar de metrische gegevens zijn ook factureerbare tegen standaardtarieven. Als u een bewaarbeleid voor gegevens hebt geconfigureerd, er zijn niet in rekening gebracht wanneer Azure Storage oude metrische gegevens worden verwijderd. Echter, als u analytische gegevens verwijdert, uw account wordt in rekening gebracht voor de delete-bewerkingen.

De capaciteit die wordt gebruikt door de tabellen metrische gegevens is ook factureerbare: u kunt de volgende schatten hoeveel capaciteit die wordt gebruikt voor het opslaan van metrische gegevens:

* Als u elk uur een service gebruikmaakt van elke API in elke service, wordt ongeveer 148KB aan gegevens elk uur in de tabellen van de transactie metrische gegevens opgeslagen als u zowel de service en de API-niveau samenvatting hebt ingeschakeld.
* Als u elk uur een service gebruikmaakt van elke API in elke service, wordt ongeveer 12KB aan gegevens elk uur in de tabellen van de transactie metrische gegevens opgeslagen als u alleen serviceniveau samenvatting hebt ingeschakeld.
* De tabel capaciteit voor blobs heeft twee rijen per dag wordt toegevoegd (mits de gebruiker heeft gekozen Logboeken): dit betekent dat elke dag de grootte van deze tabel met verhoogd tot ongeveer 300 bytes.

## <a name="next-steps"></a>Volgende stappen
[Opslag en het benaderen van logboekgegevens inschakelen](/rest/api/storageservices/Enabling-Storage-Logging-and-Accessing-Log-Data)
