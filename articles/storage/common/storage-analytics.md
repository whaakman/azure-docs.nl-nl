---
title: Azure Storage Analytics gebruiken om gegevens in Logboeken en metrische gegevens te verzamelen | Microsoft Docs
description: Storage Analytics kunt u voor het bijhouden van metrische gegevens voor alle storage-services en het verzamelen van Logboeken voor Blob, Queue en Table storage.
services: storage
author: roygara
ms.service: storage
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: rogarana
ms.component: common
ms.openlocfilehash: 0f237b4d742c0c7de1e836e2b9d83502cfe1a30d
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51230983"
---
# <a name="storage-analytics"></a>Storage Analytics

Met Azure Storage Analytics wordt logboekregistratie uitgevoerd en kunnen metrische gegevens voor een opslagaccount worden geleverd. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt dit inschakelen via de [Azure Portal](https://portal.azure.com). Zie voor meer informatie, [een opslagaccount in de Azure Portal controleren](storage-monitor-storage-account.md). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de [Blob Service-eigenschappen ophalen](https://msdn.microsoft.com/library/hh452239.aspx), [ophalen van de eigenschappen van de Queue-Service](https://msdn.microsoft.com/library/hh452243.aspx), [ophalen van de tabel-Service-eigenschappen](https://msdn.microsoft.com/library/hh452238.aspx), en [ophalen File Service Properties](https://msdn.microsoft.com/library/mt427369.aspx)bewerkingen voor het inschakelen van Storage Analytics voor elke service.

De samengevoegde gegevens worden opgeslagen in een bekende blob (voor logboekregistratie) en in bekende tabellen (voor metrische gegevens), die kunnen worden geopend met behulp van de Blob-service en service-tabel API's.

Storage Analytics heeft een limiet van 20 TB voor de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw storage-account. Zie voor meer informatie over facturering en bewaarbeleid voor gegevens, [Storage Analytics en facturering](https://msdn.microsoft.com/library/hh360997.aspx). Zie voor meer informatie over limieten voor opslagaccounts [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).

Zie voor een uitgebreide handleiding over het gebruik van Storage Analytics en andere hulpprogramma's om te identificeren, onderzoeken en oplossen van problemen met betrekking tot Azure Storage, [bewaken, problemen vaststellen en oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Over Opslaganalyse-Logboeken
Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en om problemen met een opslagservice te diagnosticeren. Aanvragen worden geregistreerd op basis van best-effort.

Logboekvermeldingen worden alleen gemaakt als er storage service-activiteit wordt. Bijvoorbeeld, als een storage-account heeft een activiteit in de Blob-service, maar niet in de tabel of wachtrij-services, wordt alleen logboeken met betrekking tot de Blob-service gemaakt.

Storage Analytics logboekregistratie is niet beschikbaar voor Azure Files.

### <a name="logging-authenticated-requests"></a>Geverifieerde logboekregistratieaanvragen
De volgende typen geverifieerde aanvragen worden geregistreerd:

* Voltooide aanvragen.
* Mislukte aanvragen, met inbegrip van time-out, beperking, netwerk, autorisatie en andere fouten.
* Aanvragen met behulp van een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen.
* Aanvragen voor het analytics-gegevens.

Aanvragen van Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx) onderwerpen.

### <a name="logging-anonymous-requests"></a>Logboekregistratie van anonieme aanvragen
De volgende typen anonieme aanvragen worden geregistreerd:

* Voltooide aanvragen.
* Serverfouten.
* Time-outfouten optreden voor zowel client als server.
* Mislukte aanvragen ophalen met foutcode 304 (niet gewijzigd).

Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst van de gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx) onderwerpen.

### <a name="how-logs-are-stored"></a>Hoe de logboeken worden opgeslagen
Alle logboeken worden opgeslagen in blok-blobs in een container met de naam $logs, die automatisch wordt gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount. De container met $logs bevindt zich in de blob-naamruimte van de storage-account, bijvoorbeeld: `http://<accountname>.blob.core.windows.net/$logs`. Deze container kan niet worden verwijderd zodra Opslaganalyse is ingeschakeld, maar de inhoud ervan kunnen worden verwijderd.

> [!NOTE]
> De container met $logs wordt niet weergegeven als een container met de bewerking wordt uitgevoerd, zoals de [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) methode. Deze moet rechtstreeks worden geopend. Bijvoorbeeld, kunt u de [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) methode voor toegang tot de blobs in de `$logs` container.
> Als aanvragen worden vastgelegd, wordt Storage Analytics tussenliggende resultaten als blokken uploaden. Storage Analytics wordt periodiek, deze blokken doorvoeren en beschikbaar stellen als een blob.
> 
> 

Dubbele records kunnen bestaan voor logboeken die zijn gemaakt in hetzelfde uur. U kunt bepalen of een record een duplicaat door het controleren van is de **RequestId** en **bewerking** getal.

### <a name="log-naming-conventions"></a>Meld u naamgevingsregels
Elk logboek wordt in de volgende indeling worden geschreven.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

De volgende tabel beschrijft elke kenmerk in de naam van het logboek.

| Kenmerk | Beschrijving |
| --- | --- |
| <service-name> |De naam van de storage-service. Bijvoorbeeld: blob, tabel en wachtrij. |
| JJJJ |Het jaar in vier cijfers voor het logboek. Bijvoorbeeld: 2011. |
| MM |De maand in twee cijfers voor het logboek. Bijvoorbeeld: 07. |
| DD |De maand in twee cijfers voor het logboek. Bijvoorbeeld: 07. |
| hh |Het uur van twee cijfers die aangeeft van het eerste uur voor de logboeken, in 24-uurs UTC-notatie. Bijvoorbeeld: 18. |
| mm |Het getal van twee cijfers die vanaf minuut voor de logboeken aangeeft. Deze waarde wordt niet ondersteund in de huidige versie van Storage Analytics en de waarde is altijd 00. |
| <counter> |Een op nul gebaseerde item met zes cijfers die het aantal log-blobs die zijn gegenereerd voor de storage-service in de periode van een uur geeft. Deze teller begint bij 000000. Bijvoorbeeld: 000001. |

Hier volgt een compleet voorbeeld logboeknaam waarin de eerdere voorbeelden worden gecombineerd.

    blob/2011/07/31/1800/000001.log

Hier volgt een voorbeeld van een URI die kan worden gebruikt voor toegang tot het vorige logboek.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Wanneer een aanvraag voor opslag wordt geregistreerd, wordt de naam van het resulterende logboek overeenkomt met het uur waarop de aangevraagde bewerking is voltooid. Bijvoorbeeld, als een aanvraag GetBlob om 18:30 uur is voltooid op 7/31/2011, zou het logboek worden geschreven met het voorvoegsel van de volgende: `blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metagegevens van het synchronisatielogboek
Alle logboekbestanden blobs worden opgeslagen met metagegevens die kan worden gebruikt om te identificeren welke logboekgegevens bevat van de blob. De volgende tabel beschrijft elke metagegevenskenmerk.

| Kenmerk | Beschrijving |
| --- | --- |
| LogType |Hierin wordt beschreven of het logboek informatie over als u wilt lezen bevat, schrijven of verwijderen van bewerkingen. Deze waarde kan een type of een combinatie van alle drie, gescheiden door komma's bevatten. Voorbeeld 1: schrijven. Voorbeeld 2: lezen, schrijven; Voorbeeld 3: lezen, schrijven, verwijderen. |
| startTime |De vroegste tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ddTHH. Bijvoorbeeld: 2011-07-31T18:21:46Z. |
| Eindtijd |De laatste tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ddTHH. Bijvoorbeeld: 2011-07-31T18:22:09Z. |
| LogVersion |De versie van de indeling voor logboekbestanden. De enige ondersteunde waarde is momenteel 1.0. |

De volgende lijst bevat een compleet voorbeeld metagegevens met behulp van de eerdere voorbeelden.

* LogType=write
* StartTime=2011-07-31T18:21:46Z
* EndTime=2011-07-31T18:22:09Z
* LogVersion=1.0

### <a name="accessing-logging-data"></a>Toegang tot gegevens voor logboekregistratie
Alle gegevens in de `$logs` container kunnen worden benaderd met behulp van de Blob-service API's, met inbegrip van de .NET-API's geleverd door Azure beheerde bibliotheek. De beheerder van de storage-account kunt lezen en verwijderen van Logboeken, maar niet maken of bijwerken van deze. Zowel de metagegevens van het logboek en naam van het logboek kunnen worden gebruikt bij het uitvoeren van query's voor een logboek. Het is mogelijk voor een bepaald uur Logboeken niet de juiste volgorde worden weergegeven, maar de metagegevens van altijd Hiermee geeft u het interval van vermeldingen in een logboek. Daarom kunt u een combinatie van logboeknamen en metagegevens bij het zoeken naar een bepaald logboek.

## <a name="about-storage-analytics-metrics"></a>Over Storage Analytics metrics
Storage Analytics kan metrische gegevens opslaan die samengevoegde transactiestatistieken en capaciteitsgegevens gegevens over aanvragen voor een opslagservice. Transacties worden gerapporteerd op zowel de API bewerking-niveau, evenals op het niveau van de service opslag en capaciteit wordt gerapporteerd op het niveau van de storage-service. Metrische gegevens kan worden gebruikt storage service-gebruik analyseren, problemen diagnosticeren met aanvragen voor de storage-service en voor het verbeteren van de prestaties van toepassingen die gebruikmaken van een service.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt dit inschakelen via de [Azure Portal](https://portal.azure.com). Zie voor meer informatie, [een opslagaccount in de Azure Portal controleren](storage-monitor-storage-account.md). U kunt ook Storage Analytics via een programma via de REST-API of de clientbibliotheek inschakelen. Gebruik de **Service-eigenschappen ophalen** bewerkingen voor het inschakelen van Storage Analytics voor elke service.

### <a name="transaction-metrics"></a>Metrische gegevens de transactie
Een robuuste set gegevens wordt vastgelegd met intervallen van uren of minuten voor elke service-opslag en de aangevraagde API-bewerking, waaronder Inkomend/uitgaand verkeer, beschikbaarheid, fouten, en aanvraag percentages gecategoriseerd. Ziet u een volledige lijst van de transactiedetails van de in de [tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/hh343264.aspx) onderwerp.

Transactiegegevens wordt vastgelegd bij twee niveaus: het serviceniveau van de en het niveau van de API-bewerking. Op het serviceniveau van de aangevraagd statistieken samenvatting van alle API-bewerkingen worden geschreven naar een Tabelentiteit elk uur, zelfs als er geen aanvragen zijn aangebracht in de service. Op het niveau van de API-bewerking, worden statistieken alleen geschreven naar een entiteit als de bewerking binnen dat uur is aangevraagd.

Als u bijvoorbeeld een **GetBlob** bewerking op uw Blob-service, metrische gegevens in Storage Analytics wordt de aanvraag zich en deze opnemen in de cumulatieve gegevens voor zowel de Blob-service, evenals de **GetBlob** de bewerking. Echter, als er geen **GetBlob** bewerking wordt aangevraagd tijdens het uur, een entiteit, niet worden geschreven naar de `$MetricsTransactionsBlob` voor die bewerking.

Metrische gegevens die transactie worden geregistreerd voor zowel de aanvragen van gebruikers en de aanvragen van Storage Analytics zelf. Bijvoorbeeld worden aanvragen door Storage Analytics voor het schrijven logboeken en tabelitems vastgelegd. Zie voor meer informatie over hoe deze aanvragen worden in rekening gebracht, [Storage Analytics en facturering](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Capaciteit metrische gegevens
> [!NOTE]
> Metrische gegevens over capaciteit zijn momenteel alleen beschikbaar voor de Blob-service. Capaciteit metrische gegevens voor de Table-service en de Queue-service zijn beschikbaar in toekomstige versies van Storage Analytics.
> 
> 

Capaciteit gegevens per dag worden geregistreerd voor de Blob-service van het opslagaccount en twee tabelitems worden geschreven. Een entiteit voorziet in statistieken voor gebruikersgegevens en de andere voorziet in statistieken over de `$logs` blobcontainer die wordt gebruikt door Storage Analytics. De `$MetricsCapacityBlob` tabel bevat de volgende statistische gegevens:

* **Capaciteit**: de hoeveelheid opslag die wordt gebruikt door de storage-account Blob-service, in bytes.
* **ContainerCount**: het aantal blob-containers in het opslagaccount-Blob-service.
* **ObjectCount**: het aantal vastgelegde en niet-doorgevoerde blok of pagina-blobs in Blob-service van het storage-account.

Zie voor meer informatie over de metrische gegevens over de capaciteit, [tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Hoe metrische gegevens worden opgeslagen
Alle metrische gegevens voor elk van de storage-services worden opgeslagen in drie tabellen die zijn gereserveerd voor de service: één tabel voor transactie-informatie, een tabel voor één minuut transactie-informatie en een andere tabel voor informatie over capaciteit. Transactie en de minuut transactie-informatie bestaat uit gegevens van aanvragen en reacties en capaciteitsgegevens van gebruiksgegevens van de opslag bestaat. Metrische gegevens uur, minuut metrische gegevens en capaciteit voor een opslagaccount-Blob-service kunnen worden benaderd in tabellen die zijn met de naam zoals beschreven in de volgende tabel.

| Het niveau van metrische gegevens | Tabelnamen | Ondersteunde versies |
| --- | --- | --- |
| Metrische gegevens per uur, primaire locatie |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |Versies vóór 2013-08-15 alleen. Terwijl deze namen worden nog steeds ondersteund, wordt het wordt aanbevolen dat u overstappen naar de onderstaande tabellen. |
| Metrische gegevens per uur, primaire locatie |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |Alle versies, met inbegrip van 15-08-2013. |
| Minuut metrische gegevens, primaire locatie |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |Alle versies, met inbegrip van 15-08-2013. |
| Metrische gegevens per uur, secundaire locatie |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |Alle versies, met inbegrip van 15-08-2013. Leestoegang geografisch redundante replicatie moet zijn ingeschakeld. |
| Minuut metrische gegevens, secundaire locatie |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |Alle versies, met inbegrip van 15-08-2013. Leestoegang geografisch redundante replicatie moet zijn ingeschakeld. |
| Capaciteit (alleen voor Blob-service) |$MetricsCapacityBlob |Alle versies, met inbegrip van 15-08-2013. |

Deze tabellen worden automatisch gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount. Ze toegankelijk zijn via de naamruimte van de storage-account, bijvoorbeeld: `https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Toegang tot metrische gegevens
Alle gegevens in de tabellen voor metrische gegevens kunnen worden benaderd met behulp van de service-API's van de tabel, met inbegrip van de .NET-API's geleverd door Azure beheerde bibliotheek. De beheerder van de storage-account kunt lezen en verwijderen van de tabelentiteiten, maar niet maken of bijwerken ze.

## <a name="billing-for-storage-analytics"></a>Facturering voor Storage Analytics
Alle metrische gegevens is geschreven door de services van een storage-account. Als gevolg hiervan wordt elke schrijfbewerking is uitgevoerd door Storage Analytics gefactureerd. Daarnaast is de hoeveelheid opslag die wordt gebruikt door metrische gegevens ook factureerbaar.

De volgende acties uitgevoerd door Storage Analytics zijn factureerbaar:

* Aanvragen voor het maken van blobs voor logboekregistratie. 
* Aanvragen voor de tabelentiteiten voor metrische gegevens maken.

Als u een beleid voor Gegevensretentie hebt geconfigureerd, u betaalt geen voor verwijdertransacties wanneer Storage Analytics oude gegevens voor logboekregistratie en metrische gegevens worden verwijderd. Verwijdertransacties van een client zijn echter factureerbaar. Zie voor meer informatie over het bewaarbeleid [instellen van een bewaarbeleid voor gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Understanding factureerbare aanvragen
Elk verzoek aan de opslag van een serviceaccount is factureerbare of niet-factureerbaar. Opslaganalyse-logboeken elke afzonderlijke aanvraag aan een service, inclusief een statusbericht dat geeft aan hoe de aanvraag is verwerkt. Op deze manier in Storage Analytics metrische gegevens voor zowel een service en de API-bewerkingen van die service, inclusief de percentages en de telling van bepaalde statusberichten worden opgeslagen. Samen vormen kunt deze functies u analyseren van uw factureerbare aanvragen, breng verbeteringen aan op uw toepassing en onderzoeken van problemen met aanvragen voor uw services. Zie voor meer informatie over facturering [inzicht in Azure Storage Billing - bandbreedte, transacties en capaciteit](https://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Wanneer Storage Analytics-gegevens bekijkt, kunt u de tabellen in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) onderwerp om te bepalen welke aanvragen zijn factureerbaar. U kunt vervolgens uw logboeken en metrische gegevens om de statusberichten om te zien als u in de rekening zijn gebracht voor een bepaalde aanvraag te vergelijken. U kunt ook de tabellen in het vorige onderwerp gebruiken voor het onderzoeken van de beschikbaarheid van een storage-service of een afzonderlijke API-bewerking.

## <a name="next-steps"></a>Volgende stappen
### <a name="setting-up-storage-analytics"></a>Instellen van Storage Analytics
* [Een opslagaccount in de Azure Portal controleren](storage-monitor-storage-account.md)
* [Inschakelen en configureren van Storage Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Logboekregistratie van opslag Analytics
* [Over Storage Analytics logboekregistratie](https://msdn.microsoft.com/library/hh343262.aspx)
* [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx)
* [Storage Analytics vastgelegd Operations en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Metrische gegevens in Storage Analytics
* [Over Storage Analytics Metrics](https://msdn.microsoft.com/library/hh343258.aspx)
* [Tabelschema van metrische gegevens van Storage Analytics](https://msdn.microsoft.com/library/hh343264.aspx)
* [Storage Analytics vastgelegd Operations en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)  

