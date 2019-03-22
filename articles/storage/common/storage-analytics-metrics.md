---
title: Azure Storage analytics metrics (klassiek)
description: Informatie over het gebruik van metrische gegevens in Azure Storage.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: 9b4bceba53658cb8ac3c73e75e0d19faf3fe3f0b
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58259751"
---
# <a name="azure-storage-analytics-metrics-classic"></a>Azure Storage analytics metrics (klassiek)

Storage Analytics kan metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens gegevens over aanvragen voor een opslagservice. Transacties worden gerapporteerd op zowel de API bewerking-niveau, evenals op het niveau van de service opslag en capaciteit wordt gerapporteerd op het niveau van de storage-service. Metrische gegevens kan worden gebruikt storage service-gebruik analyseren, problemen diagnosticeren met aanvragen voor de storage-service en voor het verbeteren van de prestaties van toepassingen die gebruikmaken van een service.  

 Storage Analytics metrics zijn standaard ingeschakeld voor nieuwe storage-accounts. U kunt metrische gegevens in de [Azure-portal](https://portal.azure.com/); voor meer informatie Zie [een opslagaccount in Azure portal controleren](/azure/storage/storage-monitor-storage-account). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de bewerkingen van de Service-eigenschappen instellen voor het inschakelen van Storage Analytics voor elke service.  

> [!NOTE]
> Storage Analytics metrische gegevens zijn beschikbaar voor de services Blob, wachtrij, tabel en bestand.
> Storage Analytics metrische gegevens zijn nu klassieke metrische gegevens. Microsoft adviseert om [metrische opslaggegevens in Azure Monitor](storage-metrics-in-azure-monitor.md) in plaats van Storage Analytics metrics.

## <a name="transaction-metrics"></a>Metrische gegevens de transactie  
 Een robuuste set gegevens wordt vastgelegd met intervallen van uren of minuten voor elke service-opslag en de aangevraagde API-bewerking, waaronder Inkomend/uitgaand verkeer, beschikbaarheid, fouten, en aanvraag percentages gecategoriseerd. Ziet u een volledige lijst van de transactiedetails van de in de [tabelschema van metrische gegevens van Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema) onderwerp.  

 Transactiegegevens wordt vastgelegd bij twee niveaus: het serviceniveau van de en het niveau van de API-bewerking. Op het serviceniveau van de aangevraagd statistieken samenvatting van alle API-bewerkingen worden geschreven naar een Tabelentiteit elk uur, zelfs als er geen aanvragen zijn aangebracht in de service. Op het niveau van de API-bewerking, worden statistieken alleen geschreven naar een entiteit als de bewerking binnen dat uur is aangevraagd.  

 Als u bijvoorbeeld een **GetBlob** bewerking op uw Blob-service, metrische gegevens in Storage Analytics wordt de aanvraag zich en deze opnemen in de cumulatieve gegevens voor zowel de Blob-service, evenals de **GetBlob** de bewerking. Echter, als er geen **GetBlob** bewerking wordt aangevraagd tijdens het uur, een entiteit, niet worden geschreven naar de *$MetricsTransactionsBlob* voor die bewerking.  

 Metrische gegevens die transactie worden geregistreerd voor zowel de aanvragen van gebruikers en de aanvragen van Storage Analytics zelf. Bijvoorbeeld worden aanvragen door Storage Analytics voor het schrijven logboeken en tabelitems vastgelegd.

## <a name="capacity-metrics"></a>Capaciteit metrische gegevens  

> [!NOTE]
>  Metrische gegevens over capaciteit zijn momenteel alleen beschikbaar voor de Blob-service.

 Capaciteit gegevens per dag worden geregistreerd voor de Blob-service van het opslagaccount en twee tabelitems worden geschreven. Een entiteit voorziet in statistieken voor gebruikersgegevens en de andere voorziet in statistieken over de `$logs` blobcontainer die wordt gebruikt door Storage Analytics. De *$MetricsCapacityBlob* tabel bevat de volgende statistische gegevens:  

- **Capaciteit**: De hoeveelheid opslag die wordt gebruikt door de storage-account Blob-service, in bytes.  
- **ContainerCount**: Het aantal blob-containers in het opslagaccount-Blob-service.  
- **ObjectCount**: Het aantal vastgelegde en niet-doorgevoerde blok of pagina-blobs in Blob-service van het storage-account.  

  Zie voor meer informatie over de metrische gegevens over de capaciteit, [tabelschema van metrische gegevens van Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema).  

## <a name="how-metrics-are-stored"></a>Hoe metrische gegevens worden opgeslagen  

 Alle metrische gegevens voor elk van de storage-services worden opgeslagen in drie tabellen die zijn gereserveerd voor de service: één tabel voor transactie-informatie, een tabel voor één minuut transactie-informatie en een andere tabel voor informatie over capaciteit. Transactie en de minuut transactie-informatie bestaat uit gegevens van aanvragen en reacties en capaciteitsgegevens van gebruiksgegevens van de opslag bestaat. Metrische gegevens uur, minuut metrische gegevens en capaciteit voor een opslagaccount van het account Blob-service kan worden geopend in de tabellen die zijn met de naam zoals beschreven in de volgende tabel.  

|Het niveau van metrische gegevens|Tabelnamen|Ondersteunde versies|  
|-------------------|-----------------|----------------------------|  
|Metrische gegevens per uur, primaire locatie|-$MetricsTransactionsBlob<br />-$MetricsTransactionsTable<br />-   $MetricsTransactionsQueue|Versies vóór 2013-08-15 alleen. Terwijl deze namen worden nog steeds ondersteund, wordt het wordt aanbevolen dat u overstappen naar de onderstaande tabellen.|  
|Metrische gegevens per uur, primaire locatie|-   $MetricsHourPrimaryTransactionsBlob<br />-   $MetricsHourPrimaryTransactionsTable<br />-   $MetricsHourPrimaryTransactionsQueue<br />-   $MetricsHourPrimaryTransactionsFile|Alle versies. Ondersteuning voor de metrische servicegegevens bestand is alleen beschikbaar in versie 2015-04-05 en hoger.|  
|Minuut metrische gegevens, primaire locatie|-   $MetricsMinutePrimaryTransactionsBlob<br />-   $MetricsMinutePrimaryTransactionsTable<br />-   $MetricsMinutePrimaryTransactionsQueue<br />-   $MetricsMinutePrimaryTransactionsFile|Alle versies. Ondersteuning voor de metrische servicegegevens bestand is alleen beschikbaar in versie 2015-04-05 en hoger.|  
|Metrische gegevens per uur, secundaire locatie|-   $MetricsHourSecondaryTransactionsBlob<br />-   $MetricsHourSecondaryTransactionsTable<br />-   $MetricsHourSecondaryTransactionsQueue|Alle versies. Leestoegang geografisch redundante replicatie moet zijn ingeschakeld.|  
|Minuut metrische gegevens, secundaire locatie|-$MetricsMinuteSecondaryTransactionsBlob<br />-$MetricsMinuteSecondaryTransactionsTable<br />-   $MetricsMinuteSecondaryTransactionsQueue|Alle versies. Leestoegang geografisch redundante replicatie moet zijn ingeschakeld.|  
|Capaciteit (alleen voor Blob-service)|$MetricsCapacityBlob|Alle versies.|  

 Deze tabellen worden automatisch gemaakt wanneer Opslaganalyse is ingeschakeld voor een storage-service-eindpunt. Ze zijn toegankelijk via de naamruimte van de storage-account, bijvoorbeeld: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`. De tabellen voor metrische gegevens worden niet weergegeven in de bewerking van een aanbieding en rechtstreeks via de naam van de tabel moeten worden geopend.  

## <a name="enable-metrics-using-the-azure-portal"></a>Metrische gegevens met behulp van de Azure-portal inschakelen
Volg deze stappen voor het inschakelen van metrische gegevens in de [Azure-portal](https://portal.azure.com):

1. Ga naar uw opslagaccount.
1. Selecteer **diagnostische instellingen (klassiek)** uit de **Menu** deelvenster.
1. Zorg ervoor dat **Status** is ingesteld op **op**.
1. Selecteer de metrische gegevens voor de services die u wilt bewaken.
1. Geef een bewaarbeleid om aan te geven hoe lang bewaren van metrische gegevens en logboekgegevens.
1. Selecteer **Opslaan**.

De [Azure-portal](https://portal.azure.com) wordt niet op dit moment kunt u configureren minuut metrische gegevens in uw storage-account; u moet inschakelen minuut metrische gegevens met behulp van PowerShell of via een programma.

> [!NOTE]
>  Houd er rekening mee dat de Azure-portal is niet op dit moment kunt u minuut metrische gegevens in uw storage-account configureren. U moet inschakelen minuut metrische gegevens met behulp van PowerShell of via een programma.  

## <a name="enable-storage-metrics-using-powershell"></a>Inschakelen van metrische gegevens van Storage met behulp van PowerShell  
U kunt PowerShell gebruiken op uw lokale machine metrische opslaggegevens in uw storage-account configureren met behulp van de Azure PowerShell-cmdlet **Get-AzureStorageServiceMetricsProperty** om op te halen van de huidige instellingen en de cmdlet  **Set-AzureStorageServiceMetricsProperty** om de huidige instellingen te wijzigen.  

De cmdlets waarmee metrische opslaggegevens gebruik de volgende parameters:  

* **ServiceType**, mogelijke waarden zijn **Blob**, **wachtrij**, **tabel**, en **bestand**.
* **MetricsType**, mogelijke waarden zijn **uur** en **minuut**.  
* **MetricsLevel**, mogelijke waarden zijn:
* **Geen**: Schakelt de bewaking.
* **Service**: Verzamelt metrische gegevens zoals Inkomend/uitgaand verkeer, beschikbaarheid, latentie en succes percentages, die voor de blob, wachtrij, tabel en Bestandsservices worden samengevoegd.
* **ServiceAndApi**: Naast de metrische gegevens van services, verzamelt dezelfde set metrische gegevens voor elke opslagbewerking in de API van Azure Storage.

De volgende opdracht wordt bijvoorbeeld met de periode is ingesteld op vijf dagen bewaren op minuut metrische gegevens voor de blob-service in uw storage-standaardaccount:  

```  
Set-AzureStorageServiceMetricsProperty -MetricsType Minute   
-ServiceType Blob -MetricsLevel ServiceAndApi  -RetentionDays 5  
```  

De volgende opdracht haalt de huidige per uur metrische gegevens niveau en retentie dagen voor de blob-service in uw storage-standaardaccount:  

```  
Get-AzureStorageServiceMetricsProperty -MetricsType Hour   
-ServiceType Blob  
```  

Zie voor informatie over het configureren van de Azure PowerShell-cmdlets om te werken met uw Azure-abonnement en het selecteren van het standaardopslagaccount te gebruiken: [Hoe u Azure PowerShell installeren en configureren](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

## <a name="enable-storage-metrics-programmatically"></a>Metrische opslaggegevens inschakelen via een programma  
Naast het gebruik van Azure portal of de Azure PowerShell-cmdlets voor het besturingselement metrische opslaggegevens, kunt u ook een van de Azure Storage-API's gebruiken. Als u een .NET-taal kunt u bijvoorbeeld de Storage-clientbibliotheek gebruiken.  

De klassen **CloudBlobClient**, **CloudQueueClient**, **CloudTableClient**, en **CloudFileClient** alle methoden, zoals hebben **SetServiceProperties** en **SetServicePropertiesAsync** die nemen een **ServiceProperties** object als parameter. U kunt de **ServiceProperties** object metrische opslaggegevens configureren. Bijvoorbeeld, de volgende C# fragment toont hoe u de metrische gegevens niveau en retentie dagen voor de uurtarieven wachtrij metrische gegevens te wijzigen:  

```csharp
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.HourMetrics.MetricsLevel = MetricsLevel.Service;  
serviceProperties.HourMetrics.RetentionDays = 10;  

queueClient.SetServiceProperties(serviceProperties);  
```  

Zie voor meer informatie over het gebruik van een .NET-taal voor het configureren van metrische opslaggegevens [Storage-clientbibliotheek voor .NET](https://msdn.microsoft.com/library/azure/mt347887.aspx).  

Raadpleeg voor algemene informatie over het configureren van metrische gegevens met behulp van de REST-API voor Storage [inschakelen en configureren van Storage Analytics](/rest/api/storageservices/Enabling-and-Configuring-Storage-Analytics).  

##  <a name="viewing-storage-metrics"></a>Weergeven van metrische opslaggegevens  
Na het configureren van Storage Analytics metrics voor het bewaken van uw storage-account registreert Storage Analytics de metrische gegevens in een set met bekende tabellen in uw opslagaccount. U kunt grafieken voor het weergeven van metrische gegevens van per uur in configureren de [Azure-portal](https://portal.azure.com):

1. Navigeer naar uw opslagaccount in de [Azure-portal](https://portal.azure.com).
1. Selecteer **metrische gegevens (klassiek)** in de **Menu** blade voor de service waarvan metrische gegevens die u wilt weergeven.
1. Klik op de grafiek die u wilt configureren.
1. In de **grafiek bewerken** blade, selecteer de **tijdsbereik**, **grafiektype**, en de metrische gegevens die u weergeven in de grafiek wilt.

In de **bewaking (klassiek)** sectie van uw opslagaccount menu-blade in Azure portal, kunt u configureren [waarschuwingsregels](#metrics-alerts), zoals het verzenden van e-mailbericht waarschuwingen u waarschuwen wanneer een bepaalde meetwaarde bereikt een bepaalde waarde.

Als u wilt voor het downloaden van de metrische gegevens voor langetermijnopslag of deze lokaal analyseert, moet u een hulpprogramma of de code schrijven om de tabellen lezen. U moet de minuut metrische gegevens voor analyse downloaden. De tabellen worden niet weergegeven als u een lijst met alle tabellen uw storage-account, maar u kunt ze rechtstreeks met de naam benaderen. Veel hulpprogramma's voor opslag bladeren op de hoogte van deze tabellen en kunt u ze rechtstreeks weergeven (Zie [Azure Storage-clienthulpprogramma's](/azure/storage/storage-explorers) voor een lijst van beschikbare hulpprogramma's).

||||  
|-|-|-|  
|**Metrische gegevens**|**Tabelnamen**|**Opmerkingen**|  
|Per uur metrische gegevens|$MetricsHourPrimaryTransactionsBlob<br /><br /> $MetricsHourPrimaryTransactionsTable<br /><br /> $MetricsHourPrimaryTransactionsQueue<br /><br /> $MetricsHourPrimaryTransactionsFile|Deze tabellen zijn in eerdere versies 2013-08-15 bekend als:<br /><br /> $MetricsTransactionsBlob<br /><br /> $MetricsTransactionsTable<br /><br /> $MetricsTransactionsQueue<br /><br /> Metrische gegevens voor de File-service zijn beschikbaar vanaf versie 2015-04-05.|  
|Minuut metrische gegevens|$MetricsMinutePrimaryTransactionsBlob<br /><br /> $MetricsMinutePrimaryTransactionsTable<br /><br /> $MetricsMinutePrimaryTransactionsQueue<br /><br /> $MetricsMinutePrimaryTransactionsFile|Kan alleen worden ingeschakeld met behulp van PowerShell of via een programma.<br /><br /> Metrische gegevens voor de File-service zijn beschikbaar vanaf versie 2015-04-05.|  
|Capaciteit|$MetricsCapacityBlob|Alleen voor BLOB-service.|  

Volledige details van de schema's vindt u voor deze tabellen aan [tabelschema van metrische gegevens van Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema). Het onderstaande voorbeeldrijen slechts een subset van de beschikbare kolommen weergeven, maar illustratie van enkele belangrijke functies van de manier waarop die metrische opslaggegevens Hiermee slaat u deze metrische gegevens:  

||||||||||||  
|-|-|-|-|-|-|-|-|-|-|-|  
|**partitionKey**|**RowKey**|**Timestamp**|**TotalRequests**|**TotalBillableRequests**|**TotalIngress**|**TotalEgress**|**Beschikbaarheid**|**AverageE2ELatency**|**AverageServerLatency**|**PercentSuccess**|  
|20140522T1100|user;All|2014-05-22T11:01:16.7650250Z|7|7|4003|46801|100|104.4286|6.857143|100|  
|20140522T1100|user;QueryEntities|2014-05-22T11:01:16.7640250Z|5|5|2694|45951|100|143.8|7.8|100|  
|20140522T1100|user;QueryEntity|2014-05-22T11:01:16.7650250Z|1|1|538|633|100|3|3|100|  
|20140522T1100|user;UpdateEntity|2014-05-22T11:01:16.7650250Z|1|1|771|217|100|9|6|100|  

In dit voorbeeld minuut metrische gegevens gebruikt de partitiesleutel het tijd resolutie van één minuut. De rijsleutel identificeert het type informatie die is opgeslagen in de rij en deze bestaat uit twee soorten informatie, het toegangstype en het aanvraagtype:  

-   Het toegangstype is een **gebruiker** of **system**, waarbij **gebruiker** verwijst naar alle aanvragen van gebruikers met de storage-service en **system** verwijst naar aanvragen van Storage Analytics.  

-   Het aanvraagtype is **alle** in dat geval een samenvatting regel is, of deze de specifieke API, zoals wordt aangegeven **QueryEntity** of **UpdateEntity**.  

De bovenstaande voorbeeldgegevens bevat alle records voor één minuut (vanaf 11:00 uur), zodat het aantal **QueryEntities** plus het aantal aanvragen **QueryEntity** plus het aantal aanvragen  **UpdateEntity** aanvragen tot zeven, dit het totale aantal is toevoegen wordt weergegeven op de **gebruiker: All** rij. Op deze manier kunt u de gemiddelde end-to-end-latentie 104.4286 afleiden op de **gebruiker: All** rij door het berekenen van ((143.8 * 5) + 3 + 9) / 7.  

## <a name="metrics-alerts"></a>Waarschuwingen voor metrische gegevens
Overweeg het instellen van waarschuwingen in de [Azure-portal](https://portal.azure.com) , zodat u wordt automatisch de hoogte van belangrijke wijzigingen in het gedrag van uw storage-services. Als u een hulpprogramma met storage explorer gebruiken om te downloaden van deze metrische gegevens in een door tekens gescheiden indeling, kunt u Microsoft Excel gebruiken om de gegevens te analyseren. Zie [Azure Storage-clienthulpprogramma's](/azure/storage/storage-explorers) voor een lijst van beschikbare opslag explorer-hulpprogramma's. U kunt waarschuwingen in configureren de **waarschuwing (klassiek)** blade toegankelijk is onder **bewaking (klassiek)** in het menu-blade van het Storage-account.

> [!IMPORTANT]
> Er is mogelijk een vertraging tussen een opslag-gebeurtenis en wanneer de bijbehorende per uur of minuut metrische gegevens wordt vastgelegd. In het geval van één minuut metrische gegevens, kunnen enkele minuten aan gegevens in één keer worden geschreven. Dit kan leiden tot transacties uit eerdere minuten worden samengevoegd in de transactie voor de huidige minuut. Als dit gebeurt, de waarschuwings-service mogelijk geen gegevens voor alle beschikbare metrische gegevens voor de geconfigureerde waarschuwingen interval, wat kan leiden tot onverwacht cursorparameter waarschuwingen.
>

## <a name="accessing-metrics-data-programmatically"></a>Openen van metrische gegevens via een programma  
De volgende vermelding toont een voorbeeld van C#-code die toegang heeft tot de minuut metrische gegevens voor een aantal minuten en de resultaten worden weergegeven in een console-venster. De voorbeeldcode gebruikt de Azure Storage Client Library-versie 4.x of hoger, inclusief de **CloudAnalyticsClient** klasse die het vereenvoudigt de toegang tot de tabellen voor metrische gegevens in de opslag.  

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
     // (StartsWith is not supported using LINQ with Azure Table storage)  
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

## <a name="billing-on-storage-metrics"></a>Facturering op metrische opslaggegevens  
Aanvragen voor het maken van tabelentiteiten voor metrische gegevens in rekening gebracht tegen de standaardtarieven van toepassing op alle Azure Storage-bewerkingen worden geschreven.  

Lees- en delete-aanvragen van metrische gegevens door een client zijn ook te factureren tegen standaardtarieven. Als u een beleid voor Gegevensretentie hebt geconfigureerd, u betaalt geen Azure Storage verwijdert de oude metrische gegevens. Echter, als u analytics-gegevens verwijdert, uw account wordt in rekening gebracht voor de delete-bewerkingen.  

De capaciteit die wordt gebruikt door de tabellen voor metrische gegevens is ook te factureren. U kunt het volgende gebruiken om te schatten van de hoeveelheid capaciteit die wordt gebruikt voor het opslaan van metrische gegevens:  

-   Als elk uur een service gebruikmaakt van elke API in elke service, wordt ongeveer 148KB aan gegevens per uur in de tabellen van de transactie metrische gegevens opgeslagen als u zowel service - en API-niveau samenvatting hebt ingeschakeld.  
-   Als u zich binnen elk uur, een service maakt gebruik van elke API in de service en vervolgens ongeveer 12KB aan gegevens per uur in de tabellen van de transactie metrische gegevens worden opgeslagen als u alleen serviceniveau-overzicht hebt ingeschakeld.  
-   De gegevenstabel voor capaciteit voor blobs bevat twee rijen toegevoegd van elke dag, mits u hebt ervoor gekozen-in voor Logboeken. Dit betekent dat elke dag, de grootte van deze tabel wordt verhoogd door maximaal ongeveer 300 bytes.

## <a name="next-steps"></a>Volgende stappen
* [Een Opslagaccount controleren](https://www.windowsazure.com/manage/services/storage/how-to-monitor-a-storage-account/)   
* [Tabelschema van metrische gegevens van Storage Analytics](/rest/api/storageservices/storage-analytics-metrics-table-schema)   
* [Storage Analytics vastgelegd Operations en statusberichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)   
* [Logboekregistratie van opslag Analytics](storage-analytics-logging.md)
