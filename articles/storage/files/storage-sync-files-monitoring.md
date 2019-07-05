---
title: Azure File Sync controleren | Microsoft Docs
description: Klik hier voor meer informatie over het bewaken van Azure File Sync.
services: storage
author: roygara
ms.service: storage
ms.topic: article
ms.date: 06/28/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 86c4bf328430bbc623d8e493eec5db520d50ef82
ms.sourcegitcommit: 9b80d1e560b02f74d2237489fa1c6eb7eca5ee10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/01/2019
ms.locfileid: "67485980"
---
# <a name="monitor-azure-file-sync"></a>Azure File Sync bewaken

Gebruik Azure File Sync te centraliseren bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB, NFS en FTPS gebruiken. U kunt zoveel caches hebben als u nodig hebt over de hele wereld.

In dit artikel wordt beschreven hoe u de implementatie van uw Azure File Sync controleren met behulp van Azure Monitor, Opslagsynchronisatieservice en Windows Server.

De volgende controle-opties zijn momenteel beschikbaar.

## <a name="azure-monitor"></a>Azure Monitor

Gebruik [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) om metrische gegevens weer te geven en het configureren van waarschuwingen voor synchronisatie, cloud tiering en connectiviteit.  

### <a name="metrics"></a>Metrische gegevens

Metrische gegevens voor Azure File Sync zijn standaard ingeschakeld en worden verzonden naar Azure Monitor om de 15 minuten.

Voor Azure File Sync metrische gegevens weergeven in Azure Monitor, selecteer de **Opslagsynchronisatieservices** resourcetype.

De volgende metrische gegevens voor Azure File Sync zijn beschikbaar in Azure Monitor:

| Naam van de meetwaarde | Description |
|-|-|
| Bytes die worden gesynchroniseerd | Grootte van gegevens die worden overgedragen (uploaden en downloaden).<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |
| Warmtemeting intrekken in de cloud | Grootte van gegevens ingetrokken.<br><br>**Opmerking**: Met deze metriek wordt in de toekomst verwijderd. Gebruik de Cloud cloudlagen intrekken grootte metrische gegevens voor het bewaken van de grootte van gegevens die zijn ingetrokken.<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Dimensie van toepassing: De naam van server |
| Warmtemeting intrekken-grootte in de cloud | Grootte van gegevens ingetrokken.<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Dimensie van toepassing: Server naam, naam van Synchronisatiegroep |
| Warmtemeting intrekken-grootte in de cloud door toepassing | Grootte van gegevens door de toepassing is ingetrokken.<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Dimensie van toepassing: Toepassing de naam, Server-naam, naam van Synchronisatiegroep |
| Warmtemeting intrekken doorvoer in de cloud | Grootte van gegevens terughalen doorvoer.<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Dimensie van toepassing: Server naam, naam van Synchronisatiegroep |
| Bestanden niet synchroniseren | Het aantal bestanden die niet worden gesynchroniseerd.<br><br>Eenheid: Count<br>Aggregatietype: Sum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |
| Bestanden die zijn gesynchroniseerd | Aantal bestanden overgedragen (uploaden en downloaden).<br><br>Eenheid: Count<br>Aggregatietype: Sum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |
| Status van de server online | Het aantal heartbeats ontvangen van de server.<br><br>Eenheid: Count<br>Aggregatietype: Maximum<br>Dimensie van toepassing: De naam van server |
| Synchronisatie-sessie resultaat | Synchroniseren van resultaat van de sessie (1 = voltooide synchronisatie sessie; 0 = mislukt synchronisatiesessie)<br><br>Eenheid: Count<br>Aggregatietypen: Maximum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |

### <a name="alerts"></a>Waarschuwingen

Voor waarschuwingen configureren in Azure Monitor, selecteert u de Opslagsynchronisatieservice en selecteer vervolgens de [Azure File Sync-metriek](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#metrics) moet worden gebruikt voor de waarschuwing.  

De volgende tabel bevat enkele voorbeeldscenario voor het bewaken van en de juiste metriek moet worden gebruikt voor de waarschuwing:

| Scenario | Metrische gegevens moet worden gebruikt voor waarschuwing |
|-|-|
| Serverstatus-eindpunt in de portal fout = | Synchronisatie-sessie resultaat |
| Bestanden niet kunnen synchroniseren met een server of cloud-eindpunt | Bestanden niet synchroniseren |
| Geregistreerde server kan niet communiceren met de Opslagsynchronisatieservice | Status van de server online |
| Cloud cloudlagen intrekken grootte heeft 500GiB in één dag overschreden  | Warmtemeting intrekken-grootte in de cloud |

Zie voor meer informatie over het configureren van waarschuwingen in Azure Monitor, [overzicht van waarschuwingen in Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="storage-sync-service"></a>Opslagsynchronisatieservice

Als u wilt geregistreerde serverstatus, de serverstatus eindpunt en metrische gegevens weergeven, gaat u naar de Opslagsynchronisatieservice in Azure portal. Vindt u de status van de geregistreerde server in de **servers geregistreerd** blade en server eindpunt-status in de **groepen synchroniseren** blade.

### <a name="registered-server-health"></a>Status van de geregistreerde server

- Als de **geregistreerde server** staat **Online**, de-server communiceert met de service.
- Als de **geregistreerde server** staat **Offline weergegeven**, controleren of de Monitor van de synchronisatie-opslag (AzureStorageSyncMonitor.exe)-proces op de server wordt uitgevoerd. Als de server zich achter een firewall of proxy, Zie [in dit artikel](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy) de firewall en proxy configureren.

### <a name="server-endpoint-health"></a>Serverstatus-eindpunt

- De status van de server-eindpunt in de portal is gebaseerd op de gebeurtenissen synchroniseren die zijn vastgelegd in het gebeurtenislogboek telemetrie op de server (ID 9102 en 9302). Als een synchronisatiesessie is mislukt vanwege een tijdelijke fout, zoals fout geannuleerd, synchronisatie mogelijk nog steeds weergegeven in orde in de portal als de huidige synchronisatiesessie is uitgevoerd, zodat. Gebeurtenis-ID 9302 wordt gebruikt om te bepalen als bestanden die worden toegepast. Zie voor meer informatie, [health synchroniseren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) en [synchroniseren voortgang](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Als de portal ziet u een synchronisatiefout omdat synchronisatie is niet beschikbaar wordt uitgevoerd, raadpleegt u de [documentatie voor probleemoplossing](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) voor hulp.

### <a name="metric-charts"></a>Grafieken met metrische gegevens

- De volgende grafieken met metrische gegevens worden weergegeven in de Opslagsynchronisatieservice-portal:

  | Naam van de meetwaarde | Description | De naam van de blade |
  |-|-|-|
  | Bytes die worden gesynchroniseerd | Grootte van gegevens die worden overgedragen (uploaden en downloaden) | Groep voor synchronisatie, servereindpunt |
  | Warmtemeting intrekken in de cloud | Grootte van gegevens die zijn ingetrokken | Geregistreerde servers |
  | Bestanden niet synchroniseren | Aantal bestanden die niet worden gesynchroniseerd | Servereindpunt |
  | Bestanden die zijn gesynchroniseerd | Aantal bestanden overgedragen (uploaden en downloaden) | Groep voor synchronisatie, servereindpunt |
  | Status van de server online | Het aantal heartbeats ontvangen van de server | Geregistreerde servers |

- Zie voor meer informatie, [Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor).

  > [!Note]  
  > De grafieken in de portal Opslagsynchronisatieservice hebben een tijdsbereik van 24 uur. Als u wilt weergeven van verschillende tijdsbereik of dimensies, moet u Azure Monitor gebruiken.

## <a name="windows-server"></a>Windows Server

U kunt op Windows Server, cloud-opslaglagen, geregistreerde server bekijken en status te synchroniseren.

### <a name="event-logs"></a>Gebeurtenislogboeken

De telemetrie-gebeurtenislogboek op de server gebruiken voor het bewaken van de geregistreerde server, synchronisatie en cloud-opslaglagen health. Het gebeurtenislogboek telemetrie bevindt zich in Logboeken onder *toepassingen en Services\Microsoft\FileSync\Agent*.

Synchronisatie-status:

- Gebeurtenis-ID 9102 wordt geregistreerd wanneer een synchronisatiesessie is voltooid. Gebruik deze gebeurtenis om te bepalen of synchronisatiesessies geslaagd (**HResult = 0**) en of er per item zijn synchroniseren fouten. Zie voor meer informatie de [health synchroniseren](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) en [fouten per item](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing) documentatie.

  > [!Note]  
  > Soms synchronisatiesessies algemene mislukt of een niet-nul-PerItemErrorCount hebben. Echter, ze nog steeds voortgang maken en sommige bestanden worden gesynchroniseerd. U kunt dit zien in de toegepaste velden, zoals AppliedFileCount, AppliedDirCount AppliedTombstoneCount en AppliedSizeBytes. Deze velden laat u weten welk deel van de sessie is voltooid. Als er meerdere synchronisatiesessies in een rij mislukt en ze een toenemend aantal toegepast hebben, geeft u synchronisatietijd en probeer het opnieuw voordat u een ondersteuningsticket opent.

- Gebeurtenis-ID 9302 wordt elke 5 tot 10 minuten geregistreerd als er een actieve synchronisatiesessie. Gebruik deze gebeurtenis om te bepalen als de huidige synchronisatiesessie ervoor dat wordt uitgevoerd zorgt (**AppliedItemCount > 0**). Synchronisatie is niet beschikbaar wordt uitgevoerd, de synchronisatiesessie mocht uiteindelijk als een gebeurtenis-ID 9102 worden geregistreerd met de fout. Zie voor meer informatie de [synchroniseren voortgang documentatie](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).

Status van de geregistreerde server:

- Gebeurtenis-ID 9301 wordt elke 30 seconden wanneer een server een query uit de service voor taken geregistreerd. Als GetNextJob is voltooid met **status = 0**, de server kan communiceren met de service. Als GetNextJob is voltooid met een fout, controleert u de [documentatie voor probleemoplossing](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) voor hulp.

Warmtemeting health in de cloud:

- U kunt controleren cloudlagen activiteit op een server met gebeurtenis-ID 9003, 9016 en 9029 in het gebeurtenislogboek telemetrie, dat zich bevindt in Logboeken onder *toepassingen en Services\Microsoft\FileSync\Agent*.

  - Gebeurtenis-ID 9003 biedt foutverdeling voor een servereindpunt. Bijvoorbeeld: Totaal aantal fouten en ErrorCode. Een gebeurtenis wordt geregistreerd per foutcode.
  - Gebeurtenis-ID 9016 bevat de resultaten van het dupliceren van een volume. Bijvoorbeeld: Percentage vrije ruimte is, aantal bestanden in de sessie, ghosted en het aantal bestanden niet ghost.
  - Gebeurtenis-ID 9029 biedt de sessiegegevens dupliceren van een servereindpunt. Bijvoorbeeld: Aantal bestanden die worden uitgevoerd in de sessie, aantal bestanden in de sessie lagen en aantal van de bestanden al lagen.
  
- U kunt controleren intrekken-activiteit op een server met gebeurtenis-ID 9005 9006, 9009 en 9059 in het gebeurtenislogboek telemetrie, dat zich bevindt in Logboeken onder *toepassingen en Services\Microsoft\FileSync\Agent*.

  - Gebeurtenis-ID 9005 biedt betrouwbaarheid van terugroeping voor een servereindpunt. Bijvoorbeeld: Totale aantal unieke bestanden geopend en totale aantal unieke bestanden met mislukte toegang.
  - Gebeurtenis-ID 9006 biedt foutverdeling intrekken voor een servereindpunt. Bijvoorbeeld: Totaal aantal mislukte aanvragen en ErrorCode. Een gebeurtenis wordt geregistreerd per foutcode.
  - Gebeurtenis-ID 9009 biedt intrekken sessie-informatie voor een servereindpunt. Bijvoorbeeld: DurationSeconds, CountFilesRecallSucceeded en CountFilesRecallFailed.
  - Gebeurtenis-ID 9059 biedt distributie van de toepassing het intrekken van een servereindpunt. Bijvoorbeeld: ShareId, toepassingsnaam en TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Prestatiemeteritems

Gebruik de Azure File Sync-prestatiemeteritems op de server voor het bewaken van activiteiten.

Voor Azure File Sync-prestatiemeteritems op de server, opent u de Prestatiemeter (Perfmon.exe). U vindt de tellers onder de **AFS Bytes overgedragen** en **AFS-synchronisatiebewerkingen** objecten.

De volgende prestatiemeteritems voor Azure File Sync zijn beschikbaar in Prestatiemeter:

| De naam van de prestatie-object item | Description |
|-|-|
| AFS Bytes Transferred\Downloaded Bytes per seconde | Het aantal bytes per seconde wordt gedownload. |
| AFS Bytes Transferred\Uploaded Bytes per seconde | Het aantal bytes per seconde wordt geüpload. |
| AFS Bytes Transferred\Total Bytes/sec | Totaal aantal bytes per seconde (uploaden en downloaden). |
| AFS synchronisatie Operations\Downloaded Sync bestanden per seconde | Het aantal bestanden die zijn gedownload per seconde. |
| AFS synchronisatie Operations\Uploaded Sync bestanden per seconde | Het aantal bestanden die zijn geüpload per seconde. |
| AFS synchronisatie Operations\Total synchronisatie bestand bewerkingen/sec | Totale aantal bestanden gesynchroniseerd (uploaden en downloaden). |

## <a name="next-steps"></a>Volgende stappen
- [Planning voor de implementatie van een Azure File Sync](storage-sync-files-planning.md)
- [Houd rekening met firewall en proxy-instellingen](storage-sync-files-firewall-and-proxy.md)
- [Azure Files Sync implementeren](storage-sync-files-deployment-guide.md)
- [Azure Files Sync oplossen](storage-sync-files-troubleshoot.md)
- [Veelgestelde vragen over Azure Files](storage-files-faq.md)
