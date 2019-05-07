---
title: Azure bestanden prestaties, problemen oplossen met
description: Bekende problemen met prestaties met Azure premium-bestandsshares (preview) en de bijbehorende tijdelijke oplossingen.
services: storage
author: gunjanj
ms.service: storage
ms.topic: article
ms.date: 04/25/2019
ms.author: gunjanj
ms.subservice: files
ms.openlocfilehash: 5ae0bb736a7cc0bbc38df5905abc5d8a71f60eb9
ms.sourcegitcommit: 0568c7aefd67185fd8e1400aed84c5af4f1597f9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65190051"
---
# <a name="troubleshoot-azure-files-performance-issues"></a>Azure Files-prestatieproblemen oplossen

Dit artikel worden enkele veelvoorkomende problemen met betrekking tot premium-Azure-bestandsshares (preview). Het biedt mogelijke oorzaken en oplossingen als deze problemen zijn opgetreden.

## <a name="high-latency-low-throughput-and-general-performance-issues"></a>Hoge latentie en lage doorvoer algemene prestatieproblemen

### <a name="cause-1-share-experiencing-throttling"></a>1 oorzaak: Delen ondervindt beperking

Het standaardquotum op een share is 100 GiB, waarmee u 100 basislijn IOPS (met een kans om uit te breiden tot 300 een uur). Zie voor meer informatie over het inrichten en de relatie tussen IOPS, de [ingericht shares](storage-files-planning.md#provisioned-shares) sectie van de handleiding voor capaciteitsplanning.

Om te bevestigen als uw bestandsshare wordt beperkt, kunt u gebruikmaken van Azure metrische gegevens in de portal.

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

1. Selecteer **alle services** en zoek vervolgens **metrische gegevens**.

1. Selecteer **Metrische gegevens**.

1. Selecteer uw storage-account als de resource.

1. Selecteer **bestand** als de metrische naamruimte.

1. Selecteer **transacties** als de metriek.

1. Voeg een filter voor **ResponseType** en controleert u of alle aanvragen die een Antwoordcode van hebt **SuccessWithThrottling**.

![Metrische gegevens over opties voor het premium-bestandsshares](media/storage-troubleshooting-premium-fileshares/metrics.png)

### <a name="solution"></a>Oplossing

- Ingerichte capaciteit vergroten delen door een hogere quotum op te geven op de share.

### <a name="cause-2-metadatanamespace-heavy-workload"></a>2 oorzaak: Metagegevens/naamruimte zware werkbelasting

Als het merendeel van de aanvragen voor metagegevens centraal (zoals createfile/openfile/closefile/queryinfo/querydirectory) wordt de latentie slechter vergelijking voor bewerkingen voor lezen/schrijven.

Om te bevestigen of de meeste van de aanvragen voor metagegevens die gericht zijn, kunt u dezelfde stappen als hierboven. Behalve in plaats van het toevoegen van een filter voor **ResponseType**, Voeg een filter voor **API-naam**.

![Filter voor de naam van de API in uw metrische gegevens](media/storage-troubleshooting-premium-fileshares/MetadataMetrics.png)

### <a name="workaround"></a>Tijdelijke oplossing

- Controleer als de toepassing kan worden gewijzigd om te beperken van het aantal bewerkingen voor metagegevens.

### <a name="cause-3-single-threaded-application"></a>3 oorzaak: Single-threaded toepassingen

Als de toepassing die wordt gebruikt door de klant één thread is, kan dit resulteren in aanzienlijk lagere IOPS/doorvoer dan de maximaal mogelijke op basis van uw ingerichte sharegrootte.

### <a name="solution"></a>Oplossing

- Toepassing parallelle uitvoering verhogen door het aantal threads te verhogen.
- Schakel over naar de toepassingen waar parallelle uitvoering mogelijk is. Bijvoorbeeld: voor kopieerbewerkingen, klanten kunnen gebruiken AzCopy of RoboCopy van Windows-clients of de **parallelle** opdracht op Linux-clients.

## <a name="very-high-latency-for-requests"></a>Zeer hoge latentie voor aanvragen

### <a name="cause"></a>Oorzaak

De client virtuele machine is gevonden in een andere regio dan de premium-bestandsshare.

### <a name="solution"></a>Oplossing

- Voer de toepassing van een virtuele machine die zich in dezelfde regio als de premium-bestandsshare.

## <a name="client-unable-to-achieve-maximum-throughput-supported-by-the-network"></a>Client niet kan bereiken van maximale doorvoer die worden ondersteund door het netwerk

Een mogelijke oorzaak van deze onvoldoende is voor SMB meerdere kanalen ondersteuning. Azure-bestandsshares ondersteunen momenteel alleen één kanaal, zodat er slechts één verbinding van de client virtuele machine met de server. Deze één verbinding is vastgemaakt aan een enkele kern op de client virtuele machine, zodat de maximale doorvoer van een virtuele machine haalbare is gebonden aan een enkele kern.

### <a name="workaround"></a>Tijdelijke oplossing

- Het ophalen van een virtuele machine met een grotere core kan u helpen de doorvoer verbeteren.
- De clienttoepassing uit meerdere virtuele machines uitvoert, wordt de doorvoer verhoogd.
- REST API's gebruiken waar mogelijk.

## <a name="throughput-on-linux-clients-is-significantly-lower-when-compared-to-windows-clients"></a>Doorvoer op Linux-clients is aanzienlijk lager in vergelijking tot Windows-clients.

### <a name="cause"></a>Oorzaak

Dit is een bekend probleem met de implementatie van SMB-client op Linux.

### <a name="workaround"></a>Tijdelijke oplossing

- De belasting verdeeld over meerdere virtuele machines
- Op dezelfde VM, gebruikmaken van meerdere koppelpunten met **nosharesock** optie en de belasting tussen deze verspreiding van koppelpunten.

## <a name="high-latencies-for-metadata-heavy-workloads-involving-extensive-openclose-operations"></a>Hoge latentie voor metagegevens zware werklasten met betrekking tot uitgebreide bewerkingen voor openen en sluiten.

### <a name="cause"></a>Oorzaak

Geen ondersteuning voor directory-leases.

### <a name="workaround"></a>Tijdelijke oplossing

- Vermijd overmatige openen/sluiten ingang op dezelfde map indien mogelijk, binnen een korte periode.
- Voor virtuele Linux-machines door time-out voor de cache van de directory-vermelding te verhogen door op te geven **actimeo =<sec>**  als een optie voor het koppelen. Standaard is deze één seconde, zodat een grotere waarde, zoals drie of vijf kan helpen.
- Voor virtuele Linux-machines door de kernel te upgraden naar 4.20 of hoger.

## <a name="low-iops-on-centosrhel"></a>Lage IOP's op CentOS/RHEL

### <a name="cause"></a>Oorzaak

I/o-diepte van meer dan één wordt niet ondersteund op CentOS/RHEL.

### <a name="workaround"></a>Tijdelijke oplossing

- Een upgrade uitvoeren naar CentOS 8 / RHEL 8.
- Wijzig in Ubuntu.

## <a name="jitterysaw-tooth-pattern-for-iops"></a>Schokkend/saw-kantoor in tooth patroon voor IOPS

### <a name="cause"></a>Oorzaak

Clienttoepassing overschrijdt consistent basislijn IOPS. Er is momenteel geen servicezijde-aanpassing van de aanvraagbelasting, dus als de client groter is dan basislijn IOP's, beperkingen op door de service. Deze beperking kan resulteren in de client ondervindt een schokkend/saw-kantoor in tooth IOPS-patroon. In dit geval gemiddelde IOPS bereikt door de client mogelijk lager dan de basislijn IOPS.

### <a name="workaround"></a>Tijdelijke oplossing

- De aanvraagbelasting te verminderen van de clienttoepassing, zodat de share heeft geen beperkingen op.
- Verhoog het quotum van de share, zodat de share heeft geen beperkingen op.

## <a name="excessive-directoryopendirectoryclose-calls"></a>Buitensporig aantal aanroepen van DirectoryOpen/DirectoryClose

### <a name="cause"></a>Oorzaak

Als het aantal aanroepen DirectoryOpen/DirectoryClose tot de belangrijkste API-aanroepen behoort en u niet verwacht de client dat te zijn dat veel aanroepen mogelijk een probleem met de antivirussoftware op de Azure VM-client is geïnstalleerd.

### <a name="workaround"></a>Tijdelijke oplossing

- Een oplossing voor dit probleem is beschikbaar in de [April Platform Update voor Windows](https://support.microsoft.com/help/4052623/update-for-windows-defender-antimalware-platform).

## <a name="file-creation-is-slower-than-expected"></a>Maken van het bestand is langzamer dan verwacht

### <a name="cause"></a>Oorzaak

Werkbelastingen die afhankelijk zijn van het maken van een groot aantal bestanden zichtbaar niet voor een aanzienlijk verschil tussen de prestaties van bestandsshares van premium en standard-bestandsshares.

### <a name="workaround"></a>Tijdelijke oplossing

- Geen.

## <a name="slow-performance-from-windows-81-or-server-2012-r2"></a>Trage prestaties van Windows 8.1 of Server 2012 R2

### <a name="cause"></a>Oorzaak

Hoger is dan verwacht latentie toegang tot Azure Files voor i/o-intensieve workloads.

### <a name="workaround"></a>Tijdelijke oplossing

- Installeer de beschikbare [hotfix](https://support.microsoft.com/help/3114025/slow-performance-when-you-access-azure-files-storage-from-windows-8-1).