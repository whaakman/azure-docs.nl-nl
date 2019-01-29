---
title: Azure File Sync controleren | Microsoft Docs
description: Klik hier voor meer informatie over het bewaken van Azure File Sync.
services: storage
author: jeffpatt24
ms.service: storage
ms.topic: article
ms.date: 01/28/2019
ms.author: jeffpatt
ms.component: files
ms.openlocfilehash: 7b03860fbcfd3380b92343575e88bd9cc7be5c13
ms.sourcegitcommit: d3200828266321847643f06c65a0698c4d6234da
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55199068"
---
# <a name="monitor-azure-file-sync"></a>Monitor voor Azure File Sync

Gebruik Azure File Sync te centraliseren bestandsshares van uw organisatie in Azure Files, terwijl de flexibiliteit, prestaties en compatibiliteit van een on-premises bestandsserver. Azure File Sync transformeert Windows Server naar een snelle cache van uw Azure-bestandsshare. U kunt elk protocol dat beschikbaar is op Windows Server voor toegang tot uw gegevens lokaal, met inbegrip van SMB, NFS en FTPS gebruiken. U kunt zoveel caches hebben als u nodig hebt over de hele wereld.

Dit artikel wordt beschreven hoe u het bewaken van uw Azure File Sync-implementatie met behulp van de Azure-portal en Windows Server.

De volgende controle-opties zijn momenteel beschikbaar:

## <a name="azure-portal"></a>Azure Portal

U kunt geregistreerde serverstatus, status van eindpunt (health sync) en metrische gegevens weergeven in de Azure-portal.

### <a name="storage-sync-service"></a>Opslagsynchronisatieservice

Als u wilt weergeven van geregistreerde server en de gezondheid van server-eindpunt, gaat u naar de Opslagsynchronisatieservice in Azure portal. Status van de server geregistreerd is kan worden weergegeven in de blade van de servers geregistreerd. Serverstatus-eindpunt is in de blade voor het synchroniseren van groepen kan worden weergegeven.

Status van de geregistreerde Server
- Als de status van de geregistreerde server Online is, wordt de server met succes communiceert met de service.
- Als de status van de server geregistreerd Offline wordt weergegeven is, controleert u of dat de Monitor van de synchronisatie-opslag (AzureStorageSyncMonitor.exe)-proces op de server wordt uitgevoerd. Als de server zich achter een firewall of proxy, configureert u de firewall en proxy per [documentatie](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy).

Serverstatus-eindpunt
- De status van de server-eindpunt in de portal is gebaseerd op de gebeurtenissen synchroniseren die zijn vastgelegd in het gebeurtenislogboek telemetrie op de server (ID 9102 en 9302). Als een synchronisatiesessie is mislukt vanwege een tijdelijke fout (bijvoorbeeld fout geannuleerd), kan synchronisatie nog steeds in orde in de portal als de huidige synchronisatiesessie is uitgevoerd (gebeurtenis-ID 9302 wordt gebruikt om te bepalen als bestanden die worden toegepast), zodat. Zie de volgende documentatie voor meer informatie: [Synchroniseren van de gezondheid van](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [synchroniseren voortgang](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Als in de portal ziet een synchronisatiefout verschuldigd is om te synchroniseren niet die wordt uitgevoerd, controleert u de [documentatie voor probleemoplossing](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) voor hulp.

### <a name="azure-monitor"></a>Azure Monitor

Gebruik [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) sync bewaken, cloud-opslaglagen en connectiviteit. Metrische gegevens voor Azure File Sync zijn standaard ingeschakeld en worden verzonden naar Azure Monitor om de 15 minuten.

Als u wilt weergeven van metrische gegevens over Azure File Sync in Azure Monitor, selecteer het type van de resource Opslagsynchronisatieservices.

De volgende metrische gegevens voor Azure File Sync zijn beschikbaar in Azure Monitor:

| Naam van de meetwaarde | Description |
|-|-|
| Bytes die worden gesynchroniseerd | Grootte van gegevens die worden overgedragen (uploaden en downloaden).<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |
| Warmtemeting intrekken in de cloud | Grootte van gegevens ingetrokken.<br><br>Eenheid: Bytes<br>Aggregatietype: Sum<br>Dimensie van toepassing: Servernaam |
| Bestanden die niet worden gesynchroniseerd | Het aantal bestanden die niet worden gesynchroniseerd.<br><br>Eenheid: Count<br>Aggregatietype: Sum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |
| Bestanden die zijn gesynchroniseerd | Het aantal bestanden geüpload en gedownload.<br><br>Eenheid: Count<br>Aggregatietype: Sum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |
| Serverheartbeat | Het aantal heartbeats ontvangen van de server.<br><br>Eenheid: Count<br>Aggregatietype: Maximum<br>Dimensie van toepassing: Servernaam |
| Synchronisatie-sessie resultaat | Synchroniseren van resultaat van de sessie (1 = voltooide synchronisatie sessie; 0 = mislukt synchronisatiesessie)<br><br>Eenheid: Count<br>Aggregatietypen: Maximum<br>Van toepassing afmetingen: Server-eindpunt naam, synchronisatie richting, naam van Synchronisatiegroep |

## <a name="windows-server"></a>Windows Server

U kunt op de Windows-Server, cloud-opslaglagen, geregistreerde server bekijken en status te synchroniseren.

### <a name="event-logs"></a>Gebeurtenislogboeken

De telemetrie-gebeurtenislogboek op de server gebruiken voor het bewaken van de geregistreerde server, synchronisatie en cloud-opslaglagen health. Het gebeurtenislogboek telemetrie bevindt zich onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken.

Synchronisatie-status
- Gebeurtenis-ID 9102 wordt geregistreerd wanneer een synchronisatiesessie is voltooid. Deze gebeurtenis moet worden gebruikt om te bepalen als synchronisatiesessies worden voltooid (HResult = 0) en of er per item zijn synchroniseren fouten. Zie de volgende documentatie voor meer informatie: [Synchroniseren van de gezondheid van](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) & [fouten Per Item](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

  > [!Note]  
  > Soms synchronisatiesessies algemene mislukken of een niet-nul-PerItemErrorCount hebt maar nog steeds aanbrengen voortgang, met enkele bestanden is gesynchroniseerd. Dit kunt u zien in de toegepaste velden (AppliedFileCount, AppliedDirCount, AppliedTombstoneCount en AppliedSizeBytes) waaraan u kunt zien hoeveel van de sessie is voltooid. Als u meerdere synchronisatiesessies in een rij die mislukken, maar een toenemend aantal toegepast ziet, moet u synchronisatietijd om te proberen het opnieuw voordat u een ondersteuningsticket openen geven.

- Gebeurtenis-ID 9302 wordt elke 5 tot 10 minuten geregistreerd als er een actieve synchronisatiesessie. Deze gebeurtenis moet worden gebruikt om te bepalen als de huidige synchronisatiesessie ervoor dat wordt uitgevoerd zorgt (AppliedItemCount > 0). Als synchronisatie niet wordt uitgevoerd, wordt de synchronisatiesessie moet uiteindelijk mislukt en wordt een gebeurtenis-ID 9102 worden geregistreerd met de fout. Zie de volgende documentatie voor meer informatie: [Voortgang van de synchronisatie](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session)

Status van de geregistreerde Server
- Gebeurtenis-ID 9301 wordt elke 30 seconden wanneer een server een query uit de service voor taken geregistreerd. Als GetNextJob is voltooid met status = 0, de server kan communiceren met de service. Als GetNextJob is voltooid met een fout, controleert u de [documentatie voor probleemoplossing](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) voor hulp.

Warmtemeting Health in de cloud
- U kunt controleren cloudlagen activiteit op een server met gebeurtenis-ID 9003, 9016 en 9029 in het logboek voor telemetrie (te vinden onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken).

  - Gebeurtenis-ID 9003 biedt foutverdeling voor een servereindpunt. Totaal aantal fouten, ErrorCode, enzovoort. Let op: één gebeurtenis wordt geregistreerd per foutcode.
  - Gebeurtenis-ID 9016 bevat de resultaten van het dupliceren van een volume. Vrije ruimte procent is, bijvoorbeeld ghosted aantal bestanden in de sessie voor het aantal bestanden kan niet ghost, enzovoort.
  - Gebeurtenis-ID 9029 biedt de sessiegegevens dupliceren van een servereindpunt. Bijvoorbeeld, aantal bestanden die worden uitgevoerd in de sessie, aantal bestanden in de sessie, aantal bestanden gelaagd al lagen, enzovoort.
  
- U kunt controleren intrekken-activiteit op een server met gebeurtenis-ID 9005 9006, 9009 en 9059 in het logboek voor telemetrie (te vinden onder logboeken toepassingen en Services\Microsoft\FileSync\Agent in Logboeken).

  - Gebeurtenis-ID 9005 biedt betrouwbaarheid van terugroeping voor een servereindpunt. Bijvoorbeeld: totaal aantal unieke bestanden zijn geopend, totale aantal unieke bestanden met mislukte toegang, enzovoort.
  - Gebeurtenis-ID 9006 biedt foutverdeling intrekken voor een servereindpunt. Totaal aantal mislukte aanvragen, ErrorCode, enzovoort. Let op: één gebeurtenis wordt geregistreerd per foutcode.
  - Gebeurtenis-ID 9009 biedt intrekken sessie-informatie voor een servereindpunt. Bijvoorbeeld, DurationSeconds, CountFilesRecallSucceeded, CountFilesRecallFailed, enzovoort.
  - Gebeurtenis-ID 9059 biedt distributie van de toepassing het intrekken van een servereindpunt. Bijvoorbeeld, ShareId, toepassingsnaam en TotalEgressNetworkBytes.

### <a name="performance-counters"></a>Prestatiemeteritems

Gebruik de Azure File Sync-prestatiemeteritems op de server voor het bewaken van activiteiten.

Azure File Sync-prestaties weergeven items op de server, start de Prestatiemeter (Perfmon.exe) en de prestatiemeteritems kunnen u vinden onder de AFS Bytes overgedragen en synchronisatiebewerkingen AFS-objecten.

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
