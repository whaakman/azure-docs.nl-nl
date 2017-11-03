---
title: Azure Storage Analytics gebruiken voor het verzamelen van Logboeken en metrische gegevens | Microsoft Docs
description: Storage Analytics kunt u voor het bijhouden van metrische gegevens voor alle storage-services en voor het verzamelen van Logboeken voor de opslag van Blob, wachtrijen en tabellen.
services: storage
documentationcenter: 
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 7894993b-ca42-4125-8f17-8f6dfe3dca76
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 03/03/2017
ms.author: tamram
ms.openlocfilehash: 9ae9dd0b078911a695d441cd3891be720dc204ac
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="storage-analytics"></a>Storage Analytics

Met Azure Storage Analytics wordt logboekregistratie uitgevoerd en kunnen metrische gegevens voor een opslagaccount worden geleverd. Deze gegevens kunt u gebruiken om aanvragen te traceren, gebruikstrends te analyseren en een diagnose uit te voeren voor problemen met uw opslagaccount.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt dit inschakelen via de [Azure Portal](https://portal.azure.com). Zie voor meer informatie [bewaken van een opslagaccount in de Azure Portal](storage-monitor-storage-account.md). U kunt ook Opslaganalyse programmatisch via de REST-API of de clientbibliotheek inschakelen. Gebruik de [Blob-Service-eigenschappen ophalen](https://msdn.microsoft.com/library/hh452239.aspx), [wachtrij Service-eigenschappen ophalen](https://msdn.microsoft.com/library/hh452243.aspx), [tabel Service-eigenschappen ophalen](https://msdn.microsoft.com/library/hh452238.aspx), en [ophalen File Service Properties](https://msdn.microsoft.com/library/mt427369.aspx) bewerkingen voor het inschakelen van Storage Analytics voor elke service.

De samengevoegde gegevens wordt opgeslagen in een bekende blob (voor logboekregistratie) en in een bekende tabellen (voor metrische gegevens) waartoe toegang kunnen worden verkregen met behulp van de Blob-service en de API's van de tabel-service.

Storage Analytics mag maximaal 20 TB van de hoeveelheid opgeslagen gegevens die onafhankelijk is van de totale limiet voor uw opslagaccount. Zie voor meer informatie over facturering en bewaarbeleid voor gegevens [Storage Analytics en facturering](https://msdn.microsoft.com/library/hh360997.aspx). Zie voor meer informatie over opslagaccountlimieten [Azure Storage Scalability and Performance Targets](storage-scalability-targets.md).

Zie voor een gedetailleerde uitleg over het gebruik van Storage Analytics en andere hulpprogramma's om te bepalen, onderzoeken en oplossen van problemen met Azure Storage met [Monitor, vaststellen en oplossen van Microsoft Azure Storage](storage-monitoring-diagnosing-troubleshooting.md).

## <a name="about-storage-analytics-logging"></a>Over Opslaganalyse logboekregistratie
Storage Analytics wordt gedetailleerde informatie over geslaagde en mislukte aanvragen in een storage-service geregistreerd. Deze informatie kan worden gebruikt voor het bewaken van afzonderlijke aanvragen en voor het vaststellen van problemen met de storage-service. Aanvragen worden geregistreerd op basis van best-effort.

Logboekvermeldingen worden alleen gemaakt als opslag service-activiteit is. Bijvoorbeeld, als een opslagaccount activiteit in de Blob-service, maar niet in de tabel- of -services heeft, wordt alleen de logboeken met betrekking tot de Blob-service gemaakt.

Storage Analytics logboekregistratie is niet beschikbaar voor Azure Files.

### <a name="logging-authenticated-requests"></a>Geverifieerde logboekregistratieaanvragen
De volgende soorten geverifieerde aanvragen worden geregistreerd:

* Geslaagde aanvragen.
* Mislukte aanvragen, met inbegrip van de time-outperiode, beperking, netwerk, autorisatie en andere fouten.
* -Aanvragen via een Shared Access Signature (SAS), met inbegrip van mislukte en geslaagde aanvragen.
* Aanvragen voor analytische gegevens.

Aanvragen door Storage Analytics zelf, zoals logboekbestanden worden gemaakt of verwijderd, worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx) onderwerpen.

### <a name="logging-anonymous-requests"></a>Logboekregistratie anonieme aanvragen
De volgende soorten anonieme aanvragen worden geregistreerd:

* Geslaagde aanvragen.
* Serverfouten.
* Time-outfouten voor zowel client als server.
* Mislukte aanvragen ophalen met foutcode 304 (niet gewijzigd).

Alle andere mislukte anonieme aanvragen worden niet geregistreerd. Een volledige lijst met gegevens in het logboek wordt beschreven in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx) en [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx) onderwerpen.

### <a name="how-logs-are-stored"></a>Hoe logboeken worden opgeslagen
Alle logboeken worden opgeslagen in een blok-blobs in een container met de naam $logs die automatisch wordt gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount. De container $logs bevindt zich in de blob-naamruimte van de storage-account, bijvoorbeeld: `http://<accountname>.blob.core.windows.net/$logs`. Deze container kan niet worden verwijderd zodra Opslaganalyse is ingeschakeld, maar de inhoud ervan kunnen worden verwijderd.

> [!NOTE]
> De container $logs wordt niet weergegeven wanneer een container met de bewerking wordt uitgevoerd, zoals de [ListContainers](https://msdn.microsoft.com/library/azure/dd179352.aspx) methode. Deze moet rechtstreeks worden geopend. U kunt bijvoorbeeld de [ListBlobs](https://msdn.microsoft.com/library/azure/dd135734.aspx) methode voor toegang tot de blobs in de `$logs` container.
> Als aanvragen worden geregistreerd, gaat u Storage Analytics tussenliggende resultaten als blokken uploaden. Storage Analytics wordt regelmatig deze blokken doorvoeren en zodat ze beschikbaar zijn als een blob.
> 
> 

Dubbele records kunnen bestaan voor logboeken gemaakt in hetzelfde uur. U kunt bepalen of een record een duplicaat door het controleren van is de **RequestId** en **bewerking** getal.

### <a name="log-naming-conventions"></a>Meld u naamgevingsregels
Elk logboek wordt geschreven in de volgende indeling.

    <service-name>/YYYY/MM/DD/hhmm/<counter>.log

De volgende tabel beschrijft elk kenmerk in de naam van het logboek.

| Kenmerk | Beschrijving |
| --- | --- |
| < Servicenaam > |De naam van de storage-service. Bijvoorbeeld: blob, table of wachtrij. |
| JJJJ |Het jaar vier cijfers voor het logboek. Bijvoorbeeld: 2011. |
| MM |De maand van de twee cijfers voor het logboek. Bijvoorbeeld: 07. |
| DD |De maand van de twee cijfers voor het logboek. Bijvoorbeeld: 07. |
| hh |Het uur jaartallen met twee betekent dit het eerste uur voor de logboeken, in 24-uurs UTC-notatie. Bijvoorbeeld: 18. |
| mm |Het nummer van twee cijfers die aangeeft van de eerste minuut voor de logboeken. Deze waarde wordt niet ondersteund in de huidige versie van Storage Analytics wordt, waarbij de waarde altijd 00. |
| <counter> |Een op nul gebaseerde item met zes cijfers die het aantal logboek BLOB's gegenereerd voor de storage-service in een uur periode geeft. Deze teller begint bij 000000. Bijvoorbeeld: 000001. |

Hier volgt een compleet codevoorbeeld logboeknaam waarin de eerdere voorbeelden worden gecombineerd.

    blob/2011/07/31/1800/000001.log

Hier volgt een voorbeeld van een URI die kan worden gebruikt voor toegang tot het vorige logboek.

    https://<accountname>.blob.core.windows.net/$logs/blob/2011/07/31/1800/000001.log

Wanneer een aanvraag van de opslag is geregistreerd, wordt de naam van het resulterende logboek correleert met het uur waarop de aangevraagde bewerking is voltooid. Bijvoorbeeld, als een GetBlob-aanvraag is voltooid op 18:30 uur op 7/31/2011, zou het logboek zijn geschreven met de volgende prefix:`blob/2011/07/31/1800/`

### <a name="log-metadata"></a>Metagegevens van het logboek
Alle logboekbestanden blobs worden opgeslagen met metagegevens die kan worden gebruikt om te bepalen welke logboekgegevens bevat van de blob. De volgende tabel beschrijft elke metagegevenskenmerk.

| Kenmerk | Beschrijving |
| --- | --- |
| LogType |Hierin wordt beschreven of het logboek informatie over lezen bevat, schrijven of verwijderen van bewerkingen. Deze waarde kan een type of een combinatie van alle drie, gescheiden door komma's bevatten. Voorbeeld 1: schrijven; Voorbeeld 2: lezen, schrijven; Voorbeeld 3: lezen, schrijven, verwijderen. |
| StartTime |De vroegste tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ssZ. Bijvoorbeeld: 2011-07-31T18:21:46Z. |
| Eindtijd |De laatste tijd van een vermelding in het logboek, in de vorm van jjjj-MM-ssZ. Bijvoorbeeld: 2011-07-31T18:22:09Z. |
| LogVersion |De versie van de indeling voor logboekbestanden. De enige ondersteunde waarde is momenteel 1.0. |

De volgende lijst bevat een compleet codevoorbeeld metagegevens met behulp van de eerdere voorbeelden.

* LogType = schrijven
* StartTime = 2011-07-31T18:21:46Z
* EndTime = 2011-07-31T18:22:09Z
* LogVersion = 1.0

### <a name="accessing-logging-data"></a>Toegang tot gegevens voor logboekregistratie
Alle gegevens in de `$logs` container toegankelijk zijn via de API's van de Blob-service, inclusief de .NET API's die is geleverd door de Azure beheerde bibliotheek. Beheerder van het opslagaccount kunt lezen en verwijderen van Logboeken, maar niet maken of bijwerken van deze. Zowel de metagegevens van het logboek als de naam van het logboek kunnen worden gebruikt bij het opvragen van een logboek. Het is mogelijk voor een bepaalde tijd worden in volgorde worden weergegeven, maar de metagegevens van altijd Hiermee geeft u het interval van logboekvermeldingen in een logboek. Daarom kunt u een combinatie van logboeknamen en metagegevens bij het zoeken naar een bepaald logboek.

## <a name="about-storage-analytics-metrics"></a>Over Opslaganalyse metrische gegevens
Storage Analytics metrische gegevens die samengevoegde statistieken en capaciteit transactiegegevens over aanvragen met storage-service zijn kunnen worden opgeslagen. Transacties worden gerapporteerd op beide API bewerking niveau en op het niveau van de service storage en capaciteit op het niveau van de service storage wordt gerapporteerd. Metrische gegevens kan worden gebruikt om storage-service analyseren, onderzoeken van problemen met aanvragen ten opzichte van de storage-service en verbeteren de prestaties van toepassingen die gebruikmaken van een service.

Voor het gebruik van Storage Analytics, moet u deze inschakelen afzonderlijk voor elke service die u wilt bewaken. U kunt dit inschakelen via de [Azure Portal](https://portal.azure.com). Zie voor meer informatie [bewaken van een opslagaccount in de Azure Portal](storage-monitor-storage-account.md). U kunt ook Opslaganalyse programmatisch via de REST-API of de clientbibliotheek inschakelen. Gebruik de **Service-eigenschappen ophalen** bewerkingen voor het inschakelen van Storage Analytics voor elke service.

### <a name="transaction-metrics"></a>Transactie metrische gegevens
Een set krachtige gegevens wordt vastgelegd bij per uur of minuut intervallen voor elke service-opslag en API heeft bewerking aangevraagd, met inbegrip van inkomend en uitgaand, beschikbaarheid, fouten, en gecategoriseerd percentages van de aanvraag. Ziet u een volledige lijst met de transactiedetails in de [Storage Analytics metrische gegevens tabelschema](https://msdn.microsoft.com/library/hh343264.aspx) onderwerp.

Transactiegegevens wordt vastgelegd op twee niveaus – het serviceniveau en het niveau van de API-bewerking. Op het serviceniveau aangevraagd statistieken samenvatten alle API-bewerkingen worden geschreven naar de Tabelentiteit van een om het uur zelfs als er geen aanvragen zijn aangebracht in de service. Op het niveau van de bewerking API zijn statistieken alleen geschreven naar een entiteit als de bewerking is aangevraagd binnen dat uur.

Als u bijvoorbeeld een **GetBlob** bewerking in de Blob-service, metrische gegevens Storage Analytics wordt de aanvraag te melden en deze opnemen in de cumulatieve gegevens voor zowel de Blob-service, evenals de **GetBlob** bewerking. Echter, als er geen **GetBlob** bewerking wordt aangevraagd tijdens het uur, een entiteit wordt niet worden geschreven naar de `$MetricsTransactionsBlob` voor de bewerking.

Transactie metrische gegevens worden geregistreerd voor aanvragen van gebruikers en aanvragen door Opslaganalyse zelf. Bijvoorbeeld worden aanvragen door Storage Analytics schrijven logboeken en tabelentiteiten geregistreerd. Zie voor meer informatie over hoe deze aanvragen worden gefactureerd [Storage Analytics en facturering](https://msdn.microsoft.com/library/hh360997.aspx).

### <a name="capacity-metrics"></a>Capaciteit metrische gegevens
> [!NOTE]
> Capaciteit metrische gegevens zijn momenteel alleen beschikbaar voor de Blob-service. Capaciteit metrische gegevens voor de tabel-service en de Queue-service zijn beschikbaar in toekomstige versies van Storage Analytics.
> 
> 

Capaciteit gegevens worden dagelijks geregistreerd voor een opslagaccount Blob-service en twee tabelentiteiten worden geschreven. Een entiteit voorziet in statistieken voor gebruikersgegevens en de andere voorziet in statistieken over de `$logs` blob-container door Storage Analytics gebruikt. De `$MetricsCapacityBlob` tabel bevat de volgende statistische gegevens:

* **Capaciteit**: de hoeveelheid opslagruimte die wordt gebruikt door de storage-account Blob-service, in bytes.
* **ContainerCount**: het aantal blob-containers in het opslagaccount Blob-service.
* **ObjectCount**: het aantal vastgelegde en niet-doorgevoerde blok of pagina blobs in de Blob-service de storage-account.

Zie voor meer informatie over de capaciteitsmetrieken [Storage Analytics metrische gegevens tabelschema](https://msdn.microsoft.com/library/hh343264.aspx).

### <a name="how-metrics-are-stored"></a>Hoe metrische gegevens worden opgeslagen
Alle metrische gegevens voor elk van de storage-services wordt opgeslagen in drie tabellen die zijn gereserveerd voor de service: één tabel voor transactie-informatie, één tabel voor de minuut transactie-informatie en een andere tabel voor informatie over capaciteit. Transactie en minuut transactie-informatie bestaat uit gegevens aanvraag en -antwoord en capaciteitsgegevens van gebruiksgegevens van de opslag bestaat. Metrische gegevens uur, minuut meetgegevens en capaciteit voor een opslagaccount Blob-service kunnen worden benaderd in tabellen die worden genoemd, zoals beschreven in de volgende tabel.

| Niveau van de metrische gegevens | Namen van tabellen | Ondersteunde versies |
| --- | --- | --- |
| Elk uur maatstaven voor primaire locatie |$MetricsTransactionsBlob <br/>$MetricsTransactionsTable <br/> $MetricsTransactionsQueue |Versies vóór 2013-08-15 alleen. Deze namen worden wel nog steeds ondersteund, het raadzaam dat u overschakelt naar de onderstaande tabellen gebruiken. |
| Elk uur maatstaven voor primaire locatie |$MetricsHourPrimaryTransactionsBlob <br/>$MetricsHourPrimaryTransactionsTable <br/>$MetricsHourPrimaryTransactionsQueue |Alle versies, inclusief 2013-08-15. |
| Minuut maatstaven voor primaire locatie |$MetricsMinutePrimaryTransactionsBlob <br/>$MetricsMinutePrimaryTransactionsTable <br/>$MetricsMinutePrimaryTransactionsQueue |Alle versies, inclusief 2013-08-15. |
| Elk uur maatstaven voor secundaire locatie |$MetricsHourSecondaryTransactionsBlob <br/>$MetricsHourSecondaryTransactionsTable <br/>$MetricsHourSecondaryTransactionsQueue |Alle versies, inclusief 2013-08-15. Leestoegang geografisch redundante replicatie moet zijn ingeschakeld. |
| Minuut maatstaven voor secundaire locatie |$MetricsMinuteSecondaryTransactionsBlob <br/>$MetricsMinuteSecondaryTransactionsTable <br/>$MetricsMinuteSecondaryTransactionsQueue |Alle versies, inclusief 2013-08-15. Leestoegang geografisch redundante replicatie moet zijn ingeschakeld. |
| Capaciteit (alleen voor Blob-service) |$MetricsCapacityBlob |Alle versies, inclusief 2013-08-15. |

Deze tabellen worden automatisch gemaakt wanneer Opslaganalyse is ingeschakeld voor een opslagaccount. Ze toegankelijk zijn via de naamruimte van de storage-account, bijvoorbeeld:`https://<accountname>.table.core.windows.net/Tables("$MetricsTransactionsBlob")`

### <a name="accessing-metrics-data"></a>Toegang tot metrische gegevens
Alle gegevens in de tabellen metrische gegevens toegankelijk zijn via de API's van de tabel-service, inclusief de .NET API's die is geleverd door de Azure beheerd bibliotheek. Beheerder van het opslagaccount kunt lezen en verwijderen van de tabelentiteiten, maar niet maken of bijwerken ze.

## <a name="billing-for-storage-analytics"></a>Facturering voor Storage Analytics
Alle gegevens van de metrische gegevens worden geschreven door de services van een opslagaccount. Elke schrijfbewerking uitgevoerd door Opslaganalyse is als gevolg hiervan factureerbare. Bovendien is de hoeveelheid opslagruimte die wordt gebruikt door metrische gegevens ook factureerbare.

De volgende acties uitgevoerd door Storage Analytics zijn factureerbare:

* Aanvragen voor het maken van blobs voor logboekregistratie. 
* Aanvragen voor het maken van de tabelentiteiten voor de metrische gegevens.

Als u een bewaarbeleid voor gegevens hebt geconfigureerd, u worden niet in rekening gebracht voor verwijdertransacties wanneer Opslaganalyse oude logboekregistratie en metrische gegevens worden verwijderd. Er zijn echter verwijdertransacties van een client factureerbare. Zie voor meer informatie over het bewaarbeleid [instellen van een bewaarbeleid voor gegevens van Storage Analytics](https://msdn.microsoft.com/library/azure/hh343263.aspx).

### <a name="understanding-billable-requests"></a>Understanding factureerbare aanvragen
Elk verzoek aan de storage-service een account is factureerbare of niet-factureerbaar. Storage Analytics registreert elke afzonderlijke aanvraag aan een service, inclusief een statusbericht dat geeft aan hoe de aanvraag is verwerkt. Storage Analytics worden op dezelfde manier metrische gegevens voor een service en de API-bewerkingen van die service, inclusief de percentages en de telling van bepaalde statusberichten die zijn opgeslagen. Samen kunt deze functies u uw factureerbare aanvragen analyseren, verbeteringen op uw toepassing maken en analyseren van problemen met verzoeken tot uw services. Zie voor meer informatie over facturering [wat Azure Storage facturering - bandbreedte, transacties en capaciteit](http://blogs.msdn.com/b/windowsazurestorage/archive/2010/07/09/understanding-windows-azure-storage-billing-bandwidth-transactions-and-capacity.aspx).

Als u Opslaganalyse gegevens bekijkt, kunt u de tabellen in de [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/azure/hh343260.aspx) onderwerp om te bepalen welke aanvragen factureerbare zijn. Vervolgens kunt u uw logboeken en metrische gegevens om de statusberichten om te zien als u aangerekend voor een bepaalde aanvraag te vergelijken. U kunt ook de tabellen in het vorige onderwerp gebruiken voor het onderzoeken van de beschikbaarheid van een service-opslag of afzonderlijke API-bewerking.

## <a name="next-steps"></a>Volgende stappen
### <a name="setting-up-storage-analytics"></a>Instellen van Storage Analytics
* [Een opslagaccount in de Azure Portal controleren](storage-monitor-storage-account.md)
* [Inschakelen en configureren van opslag Analytics](https://msdn.microsoft.com/library/hh360996.aspx)

### <a name="storage-analytics-logging"></a>Storage Analytics logboekregistratie
* [Over Storage Analytics logboekregistratie](https://msdn.microsoft.com/library/hh343262.aspx)
* [Storage Analytics logboekindeling](https://msdn.microsoft.com/library/hh343259.aspx)
* [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)

### <a name="storage-analytics-metrics"></a>Metrische gegevens Storage Analytics
* [Over metrische gegevens Storage Analytics](https://msdn.microsoft.com/library/hh343258.aspx)
* [Storage Analytics metrische gegevens tabelschema](https://msdn.microsoft.com/library/hh343264.aspx)
* [Storage Analytics geregistreerde bewerkingen en statusberichten](https://msdn.microsoft.com/library/hh343260.aspx)  

