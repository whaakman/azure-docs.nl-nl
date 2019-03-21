---
title: Azure Opslaganalyse-Logboeken
description: Leer hoe u zich aanmeldt meer informatie over aanvragen voor Azure Storage.
services: storage
author: fhryo-msft
ms.service: storage
ms.topic: article
ms.date: 03/11/2019
ms.author: fryu
ms.subservice: common
ms.openlocfilehash: ab235c67e3a0e60999a0348d03a6e938944f7030
ms.sourcegitcommit: aa3be9ed0b92a0ac5a29c83095a7b20dd0693463
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58260180"
---
# <a name="azure-storage-analytics-logging"></a>Azure Storage analytics-Logboeken

Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en om problemen met een opslagservice te diagnosticeren. Aanvragen worden geregistreerd op basis van best-effort.

 Storage Analytics logboekregistratie is niet standaard ingeschakeld voor uw storage-account. U kunt deze inschakelen in de [Azure-portal](https://portal.azure.com/); voor meer informatie, Zie [een opslagaccount in Azure portal controleren](/azure/storage/storage-monitor-storage-account). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de [Blob Service-eigenschappen ophalen](https://docs.microsoft.com/rest/api/storageservices/Blob-Service-REST-API), [Queue-Service-eigenschappen ophalen](https://docs.microsoft.com/rest/api/storageservices/Get-Queue-Service-Properties), en [Table Service-eigenschappen ophalen](https://docs.microsoft.com/rest/api/storageservices/Get-Table-Service-Properties) bewerkingen voor het inschakelen van Storage Analytics voor elke service.

 Logboekvermeldingen worden alleen als er aanvragen voor het service-eindpunt gemaakt. Bijvoorbeeld, als een storage-account heeft een activiteit in de Blob-eindpunt, maar niet in de tabel of wachtrij eindpunten, wordt alleen logboeken met betrekking tot de Blob-service gemaakt.

> [!NOTE]
>  Logboekregistratie van opslag Analytics is momenteel alleen beschikbaar voor de services Blob, wachtrijen en tabellen. Premium storage-account wordt echter niet ondersteund.

## <a name="requests-logged-in-logging"></a>Aanvragen die worden geregistreerd de logboekregistratie

### <a name="logging-authenticated-requests"></a>Geverifieerde logboekregistratieaanvragen

 De volgende typen geverifieerde aanvragen worden geregistreerd:

- Geslaagde aanvragen
- Mislukte aanvragen, met inbegrip van time-out, beperking, netwerk, autorisatie en andere fouten
- Aanvragen met een Shared Access Signature (SAS) of met OAuth, met inbegrip van mislukte en geslaagde aanvragen
- Aanvragen voor het analytics-gegevens

  Aanvragen van Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](/rest/api/storageservices/storage-analytics-log-format) onderwerpen.

### <a name="logging-anonymous-requests"></a>Logboekregistratie van anonieme aanvragen

 De volgende typen anonieme aanvragen worden geregistreerd:

- Geslaagde aanvragen
- Server-fouten
- Time-outfouten optreden voor zowel client als server
- GET-aanvragen is mislukt met foutcode 304 (niet gewijzigd)

  Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages) en [Storage Analytics logboekindeling](/rest/api/storageservices/storage-analytics-log-format) onderwerpen.

## <a name="how-logs-are-stored"></a>Hoe de logboeken worden opgeslagen

Alle logboeken worden opgeslagen in blok-blobs in een container met de naam `$logs`, die automatisch wordt gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount. De `$logs` container bevindt zich in de blob-naamruimte van de storage-account, bijvoorbeeld: `http://<accountname>.blob.core.windows.net/$logs`. Deze container kan niet worden verwijderd zodra Opslaganalyse is ingeschakeld, maar de inhoud ervan kunnen worden verwijderd. Als u uw opslag bladeren-hulpprogramma gebruiken om te navigeren naar de container rechtstreeks, ziet u de blobs die uw gegevens voor logboekregistratie bevatten.

> [!NOTE]
>  De `$logs` container wordt niet weergegeven als een container met de bewerking wordt uitgevoerd, zoals de lijst met Containers-bewerking. Deze moet rechtstreeks worden geopend. U kunt bijvoorbeeld de lijst met Blobs-bewerking gebruiken voor toegang tot de blobs in de `$logs` container.

Als aanvragen worden vastgelegd, wordt Storage Analytics tussenliggende resultaten als blokken uploaden. Storage Analytics wordt periodiek, deze blokken doorvoeren en beschikbaar stellen als een blob. Het kan een uur duren voor de logboekgegevens worden weergegeven in de blobs in de **$logs** container omdat de frequentie waarmee de storage-service de schrijvers log leegmaken. Dubbele records kunnen bestaan voor logboeken die zijn gemaakt in hetzelfde uur. U kunt bepalen of een record een duplicaat door het controleren van is de **RequestId** en **bewerking** getal.

Als u een groot aantal logboekgegevens met meerdere bestanden voor elk uur hebt, kunt u de metagegevens van de blob gebruiken om te bepalen welke gegevens het logboek door te controleren van de blob-metagegevensvelden bevat. Dit is ook handig omdat er soms een vertraging optreden terwijl gegevens worden geschreven naar de logboekbestanden: de metagegevens van de blob biedt een meer nauwkeurige indicatie van de inhoud van de blob dan de naam van de blob.

De meeste opslag bladeren's kunt u de metagegevens van de blobs; weergeven u kunt ook deze informatie met behulp van PowerShell lezen of via een programma. De volgende PowerShell-codefragment wordt een voorbeeld van de lijst met blobs logboek filteren door de naam opgeven van een tijd en metagegevens voor het identificeren van alleen de logboeken die bevatten **schrijven** bewerkingen.  

 ```  
 Get-AzureStorageBlob -Container '$logs' |  
 where {  
     $_.Name -match 'table/2014/05/21/05' -and   
     $_.ICloudBlob.Metadata.LogType -match 'write'  
 } |  
 foreach {  
     "{0}  {1}  {2}  {3}" –f $_.Name,   
     $_.ICloudBlob.Metadata.StartTime,   
     $_.ICloudBlob.Metadata.EndTime,   
     $_.ICloudBlob.Metadata.LogType  
 }  
 ```  

Zie voor meer informatie over het aanbieden van blobs programmatisch [Blobbronnen inventariseren](http://msdn.microsoft.com/library/azure/hh452233.aspx) en [instelling en het ophalen van eigenschappen en metagegevens voor Blobbronnen](http://msdn.microsoft.com/library/azure/dd179404.aspx).  

### <a name="log-naming-conventions"></a>Meld u naamgevingsregels

 Elk logboek wordt geschreven in de volgende indeling:

 `<service-name>/YYYY/MM/DD/hhmm/<counter>.log`

 De volgende tabel beschrijft elke kenmerk in de naam van het logboek:

|Kenmerk|Description|
|---------------|-----------------|
|`<service-name>`|De naam van de storage-service. Bijvoorbeeld: `blob`, `table`, of `queue`|
|`YYYY`|Het jaar in vier cijfers voor het logboek. Bijvoorbeeld: `2011`|
|`MM`|De maand in twee cijfers voor het logboek. Bijvoorbeeld: `07`|
|`DD`|De dag van de twee cijfers voor het logboek. Bijvoorbeeld: `31`|
|`hh`|Het uur van twee cijfers die aangeeft van het eerste uur voor de logboeken in 24-uurs UTC-notatie. Bijvoorbeeld: `18`|
|`mm`|Het nummer met twee cijfers die vanaf minuut voor de logboeken aangeeft. **Opmerking:**  Deze waarde wordt niet ondersteund in de huidige versie van Storage Analytics en de waarde is altijd `00`.|
|`<counter>`|Een op nul gebaseerde item met zes cijfers die het aantal log-blobs die zijn gegenereerd voor de storage-service in de periode van een uur geeft. Deze teller begint bij `000000`. Bijvoorbeeld: `000001`|

 Hier volgt een compleet voorbeeld logboeknaam waarin de bovenstaande voorbeelden worden gecombineerd:

 `blob/2011/07/31/1800/000001.log`

 Hier volgt een voorbeeld van een URI die kan worden gebruikt voor toegang tot het bovenstaande logboek:

 `https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log`

 Wanneer een aanvraag voor opslag wordt geregistreerd, wordt de naam van het resulterende logboek overeenkomt met het uur waarop de aangevraagde bewerking is voltooid. Bijvoorbeeld, als een aanvraag GetBlob op 18:30:00 uur op 7/31/2011 is voltooid, kan het logboek zouden worden geschreven met het voorvoegsel van de volgende: `blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metagegevens van het synchronisatielogboek

 Alle logboekbestanden blobs worden opgeslagen met metagegevens die kan worden gebruikt om te identificeren welke logboekgegevens bevat van de blob. De volgende tabel beschrijft elke metagegevenskenmerk:

|Kenmerk|Description|
|---------------|-----------------|
|`LogType`|Hierin wordt beschreven of het logboek informatie over als u wilt lezen bevat, schrijven of verwijderen van bewerkingen. Deze waarde kan een type of een combinatie van alle drie, gescheiden door komma's bevatten.<br /><br /> Voorbeeld 1: `write`<br /><br /> Voorbeeld 2: `read,write`<br /><br /> Voorbeeld 3: `read,write,delete`|
|`StartTime`|De vroegste tijd van een vermelding in het logboek, in de vorm van `YYYY-MM-DDThh:mm:ssZ`. Bijvoorbeeld: `2011-07-31T18:21:46Z`|
|`EndTime`|De laatste tijd van een vermelding in het logboek, in de vorm van `YYYY-MM-DDThh:mm:ssZ`. Bijvoorbeeld: `2011-07-31T18:22:09Z`|
|`LogVersion`|De versie van de indeling voor logboekbestanden.|

 De volgende lijst bevat een compleet voorbeeld metagegevens met behulp van de bovenstaande voorbeelden:

-   `LogType=write`
-   `StartTime=2011-07-31T18:21:46Z`
-   `EndTime=2011-07-31T18:22:09Z`
-   `LogVersion=1.0`

## <a name="enable-storage-logging"></a>Inschakelen van logboekregistratie van opslag

U kunt logboekregistratie van opslag met Azure portal, PowerShell and Storage-SDK's inschakelen.

### <a name="enable-storage-logging-using-the-azure-portal"></a>Inschakelen van logboekregistratie van opslag met behulp van de Azure portal  

In de Azure-portal, gebruikt u de **diagnostische instellingen (klassiek)** blade om te bepalen, opslag-logboekregistratie, toegankelijk is vanaf de **bewaking (klassiek)** gedeelte van een storage-account **Menu-blade** .

U kunt de storage-services die u wilt vastleggen, evenals de bewaarperiode (in dagen) voor de gegevens in het logboek.  

### <a name="enable-storage-logging-using-powershell"></a>Inschakelen van logboekregistratie van opslag met behulp van PowerShell  

 U kunt PowerShell gebruiken op uw lokale machine logboekregistratie van opslag configureren in uw opslagaccount met behulp van de Azure PowerShell-cmdlet **Get-AzureStorageServiceLoggingProperty** om op te halen van de huidige instellingen en de cmdlet  **Set-AzureStorageServiceLoggingProperty** om de huidige instellingen te wijzigen.  

 De cmdlets waarmee de logboekregistratie van opslag gebruiken een **LoggingOperations** parameter die is een tekenreeks met een door komma's gescheiden lijst van aanvraagtypen om aan te melden. Er zijn drie mogelijke aanvraagtypen **lezen**, **schrijven**, en **verwijderen**. Als u wilt logboekregistratie uitschakelen, gebruikt u de waarde **geen** voor de **LoggingOperations** parameter.  

 De volgende opdracht schakelt logboekregistratie voor lezen, schrijven en verwijderen van aanvragen in de Queue-service in uw storage-standaardaccount met een bewaarperiode van ingesteld op vijf dagen:  

```  
Set-AzureStorageServiceLoggingProperty -ServiceType Queue   
-LoggingOperations read,write,delete -RetentionDays 5  
```  

 De volgende opdracht schakelt uit logboekregistratie voor de table-service in uw storage-standaardaccount:  

```  
Set-AzureStorageServiceLoggingProperty -ServiceType Table   
-LoggingOperations none  
```  

 Zie voor informatie over het configureren van de Azure PowerShell-cmdlets om te werken met uw Azure-abonnement en het selecteren van het standaardopslagaccount te gebruiken: [Hoe u Azure PowerShell installeren en configureren](https://azure.microsoft.com/documentation/articles/install-configure-powershell/).  

### <a name="enable-storage-logging-programmatically"></a>Opslag programmatisch logboekregistratie inschakelen  
 Naast het gebruik van Azure portal of de Azure PowerShell-cmdlets voor het besturingselement logboekregistratie van opslag, kunt u ook een van de Azure Storage-API's gebruiken. Als u een .NET-taal kunt u bijvoorbeeld de Storage-clientbibliotheek gebruiken.  

 De klassen **CloudBlobClient**, **CloudQueueClient**, en **CloudTableClient** hebben alle methoden, zoals **SetServiceProperties** en **SetServicePropertiesAsync** die nemen een **ServiceProperties** object als parameter. U kunt de **ServiceProperties** object logboekregistratie van opslag wilt configureren. Bijvoorbeeld, de volgende C# fragment toont hoe u wat wordt geregistreerd en de bewaarperiode voor logboekregistratie van de wachtrij te wijzigen:  

```  
var storageAccount = CloudStorageAccount.Parse(connStr);  
var queueClient = storageAccount.CreateCloudQueueClient();  
var serviceProperties = queueClient.GetServiceProperties();  

serviceProperties.Logging.LoggingOperations = LoggingOperations.All;  
serviceProperties.Logging.RetentionDays = 2;  

queueClient.SetServiceProperties(serviceProperties);  
```  

 Zie voor meer informatie over logboekregistratie van opslag configureren met behulp van een .NET-taal [naslaginformatie over Storage-clientbibliotheek](https://msdn.microsoft.com/library/azure/dn261237.aspx).  

 Raadpleeg voor algemene informatie over het configureren van opslag logboekregistratie met behulp van de REST-API, [inschakelen en configureren van Storage Analytics](https://msdn.microsoft.com/library/azure/hh360996.aspx).  

## <a name="download-storage-logging-log-data"></a>Storage logging logboekgegevens downloaden

 Als u wilt weergeven en analyseren van uw logboekgegevens, moet u de blobs die bevatten de logboekgegevens die naar een lokale computer wilt downloaden. Veel opslag bladeren's kunt u downloaden van blobs vanuit uw storage-account; u kunt ook het opdrachtregelprogramma voor Azure-exemplaar opgegeven Azure Storage-team (**AzCopy**) om de gegevens van uw logboekbestanden te downloaden.  

 Zorg ervoor dat u de logboekgegevens die u geïnteresseerd bent in downloaden en te voorkomen dat dezelfde gegevens meerdere keren downloaden:  

-   Gebruik de datum en tijd naamgevingsregels voor partities voor blobs met logboekgegevens om bij te houden welke blobs u al hebt gedownload voor analyse om te voorkomen dat dezelfde gegevens meerdere keren opnieuw te downloaden.  

-   De metagegevens op de blobs met logboekgegevens gebruiken om u te identificeren van de specifieke periode waarvoor bevat de blob logboekgegevens voor het identificeren van de exacte blob die u wilt downloaden.  

> [!NOTE]
>  AzCopy is onderdeel van de Azure SDK, maar u kunt altijd de meest recente versie van downloaden [ https://aka.ms/AzCopy ](https://aka.ms/AzCopy). AzCopy is standaard geïnstalleerd in de map **C:\Program Files (x86) \Microsoft SDKs\Windows Azure\AzCopy**, en moet u deze map toevoegen aan uw pad voordat u het hulpprogramma uitvoeren in een opdrachtprompt of PowerShell-venster.  

 Het volgende voorbeeld ziet hoe u de logboekgegevens van de queue-service voor de uren vanaf 09 AM, 10 uur en 11 uur op 20 mei 2014 kunt downloaden. De **/S** parameter zorgt ervoor dat AzCopy om te maken van de structuur van een lokale map op basis van de datums en tijden in de namen van logboekbestanden; de **/V** parameter zorgt ervoor dat AzCopy voor het produceren van uitgebreide uitvoer; de **/Y** parameter zorgt ervoor dat AzCopy alle lokale bestanden overschreven. Vervang **< yourstorageaccount\>**  met de naam van uw opslagaccount en vervang **< yourstoragekey\>**  met sleutel van uw opslagaccount.  

```  
AzCopy 'http://<yourstorageaccount>.blob.core.windows.net/$logs/queue'  'C:\Logs\Storage' '2014/05/20/09' '2014/05/20/10' '2014/05/20/11' /sourceKey:<yourstoragekey> /S /V /Y  
```  

 AzCopy heeft ook enkele nuttige parameters die bepalen hoe de laatst gewijzigd wordt ingesteld op de bestanden die worden gedownload en of er wordt geprobeerd te downloaden van bestanden die ouder of nieuwer is dan de bestanden die al bestaan op uw lokale computer. U kunt deze ook uitvoeren in de modus voor opnieuw starten. Voor meer informatie, kunt u de help weergeven door het uitvoeren van de **AzCopy /?** de opdracht.  

 Voor een voorbeeld van hoe u kunt uw logboekgegevens programmatisch downloaden, Zie het blogbericht [logboekregistratie van Windows Azure Storage: Met behulp van Logboeken op de opslag-aanvragen bijhouden](https://blogs.msdn.com/b/windowsazurestorage/archive/2011/08/03/windows-azure-storage-logging-using-logs-to-track-storage-requests.aspx) en zoek naar het woord 'DumpLogs' op de pagina.  

 Wanneer u uw logboekgegevens hebt gedownload, kunt u de logboekvermeldingen weergeven in de bestanden. Deze logboekbestanden gebruiken een gescheiden tekstopmaak dat veel log lezen van de hulpprogramma's zijn kan worden geparseerd, met inbegrip van Microsoft Message Analyzer (Zie voor meer informatie de handleiding [bewaking, diagnose en het oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md)). Verschillende hulpprogramma's beschikken over andere voorzieningen voor opmaak, filteren, sorteren, te zoeken naar de inhoud van uw logboekbestanden ad. Zie voor meer informatie over de logboekregistratie van opslag-indeling en inhoud [Storage Analytics logboekindeling](/rest/api/storageservices/storage-analytics-log-format) en [Storage Analytics geregistreerde bewerkingen en statusberichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages).

## <a name="next-steps"></a>Volgende stappen
* [Storage Analytics logboekindeling](/rest/api/storageservices/storage-analytics-log-format)
* [Storage Analytics vastgelegd Operations en statusberichten](/rest/api/storageservices/storage-analytics-logged-operations-and-status-messages)
* [Storage Analytics Metrics (klassiek)](storage-analytics-metrics.md)
