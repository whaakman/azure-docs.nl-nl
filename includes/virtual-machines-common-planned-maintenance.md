---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 07/05/2018
ms.author: shants
ms.custom: include file
ms.openlocfilehash: abf10177f8ce86309043da92d1f2b690775b6d89
ms.sourcegitcommit: a1e1b5c15cfd7a38192d63ab8ee3c2c55a42f59c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37909980"
---
Van tijd tot tijd voert Azure updates uit om de betrouwbaarheid, prestaties en veiligheid te verbeteren van de host-infrastructuur voor virtuele machines. Deze updates kunnen betrekking hebben op het patchen van software-onderdelen in de hostingomgeving (zoals besturingssysteem, hypervisor en verschillende agents die zijn geïmplementeerd op de host), het bijwerken van netwerkonderdelen en het buiten gebruik stellen van hardware. De meeste van deze updates worden uitgevoerd zonder dat dit van invloed is op de gehoste virtuele machines. Er zijn echter gevallen waar updates indruk hebben:

- Als een update opnieuw opstarten zonder mogelijk is, gebruikt Azure onderhoud met statusbehoud geheugen op de virtuele machine wordt onderbroken terwijl de host is bijgewerkt of de virtuele machine wordt verplaatst naar een reeds bijgewerkte host kan worden overgeslagen.

- Als u onderhoud moet worden opgestart, krijgt u een kennisgeving van wanneer het onderhoud is gepland. In dergelijke gevallen u ook krijgt een bepaalde periode waar u kunt beginnen het onderhoud zelf op een tijdstip die bij u past.

Deze pagina wordt beschreven hoe beide typen onderhoud wordt uitgevoerd in Microsoft Azure. Zie voor meer informatie over niet-geplande gebeurtenissen (uitval), de beschikbaarheid van virtuele machines beheren voor [Windows] (.. / articles/virtual-machines/windows/manage-availability.md) of [Linux](../articles/virtual-machines/linux/manage-availability.md).

Toepassingen die worden uitgevoerd in een virtuele machine kunnen informatie verzamelen over toekomstige updates met behulp van de Azure Metadata-Service voor [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) of [Linux] (.. / articles/virtual-machines/linux/instance-metadata-service.md).

Zie voor 'procedures' informatie over het beheren van gepland onderhoud 'Meldingen gepland onderhoud verwerken' voor [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="memory-preserving-maintenance"></a>Geheugen onderhoud met statusbehoud

Wanneer updates volledig opnieuw worden opgestart vereisen, wordt geheugen behouden onderhoud mechanismen worden gebruikt voor het beperken van de impact op de virtuele machine. De virtuele machine wordt onderbroken tot 30 seconden, het geheugen in het RAM behouden terwijl de hostomgeving van toepassing de vereiste updates en patches is, of de virtuele machine naar een reeds bijgewerkte host verplaatst. De virtuele machine wordt vervolgens hervat en wordt de klok van de virtuele machine automatisch gesynchroniseerd. 

Deze onderhoudsbewerkingen niet rebootful zijn toegepast foutdomein door foutdomein en voortgang is gestopt als er health waarschuwingssignalen worden ontvangen.

Sommige toepassingen wordt mogelijk beïnvloed door deze typen updates. Toepassingen die gebeurtenissen in realtime verwerkt, zoals media streamen of transcodering of hoge doorvoer scenario's, netwerken kunnen niet worden ontworpen voor een pauze van 30 seconde tolereren. <!-- sooooo, what should they do? --> Als de virtuele machine wordt verplaatst naar een andere host, kunnen een lichte prestatievermindering in de aanloop naar de virtuele Machine onderbreken minuten ziet u enkele gevoelige werkbelastingen. 


## <a name="maintenance-requiring-a-reboot"></a>Onderhoud vereisen van opnieuw opstarten

Wanneer virtuele machines nodig hebt voor gepland onderhoud opnieuw worden opgestart, wordt u van tevoren gewaarschuwd. Gepland onderhoud heeft twee fasen: het selfservice-venster en een geplande onderhoudsvenster.

De **selfservice venster** kunt u het onderhoud op uw virtuele machines starten. Gedurende deze tijd kunt u een query elke virtuele machine om te zien wat de status en controleer het resultaat van de laatste aanvraag voor onderhoud.

Als u selfservice-onderhoud start, wordt uw virtuele machine wordt verplaatst naar een knooppunt dat al is bijgewerkt en wordt vervolgens gebruikt door terug op. Omdat de virtuele machine opnieuw is opgestart, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt.

Als u selfservice-onderhoud starten en er een fout opgetreden tijdens het proces is, de bewerking is gestopt, de virtuele machine is niet bijgewerkt en krijgt u de mogelijkheid om opnieuw te proberen de selfservice-onderhoud. 

Wanneer het venster selfservice is verstreken, de **geplande onderhoudsvenster** begint. Tijdens deze periode, kunt u nog steeds op te vragen voor het onderhoudsvenster, maar niet langer starten van het onderhoud zelf.

Zie voor meer informatie over het beheren van onderhoud vereisen van opnieuw opstarten 'Meldingen gepland onderhoud verwerken' voor [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Beschikbaarheidsoverwegingen tijdens gepland onderhoud 

Als u besluit om te wachten tot het geplande onderhoudsvenster, zijn er enkele aandachtspunten voor het onderhouden van de hoogst mogelijke beschikbaarheid van uw VM's. 

#### <a name="paired-regions"></a>Gekoppelde regio 's

Elke Azure-regio is gekoppeld aan een andere regio binnen hetzelfde geografische gebied, samen ze een regionaal paar maken. Azure tijdens gepland onderhoud, worden alleen de virtuele machines in één regio van een paar regio bijgewerkt. Wanneer bijvoorbeeld de virtuele machines in Noord-centraal VS worden bijgewerkt, worden er tegelijkertijd geen virtuele machines in Zuid-centraal VS bijgewerkt. Tegelijkertijd met VS - oost kan er echter wel onderhoud plaatsvinden in andere regio's, zoals Noord-Europa. Uw VM's inzicht in de werking van gekoppelde regio's, kunt u beter verdelen over regio's. Zie voor meer informatie, [Azure regioparen](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Beschikbaarheidssets en schaalsets

Bij het implementeren van een workload op virtuele Azure-machines, kunt u de virtuele machines in een beschikbaarheidsset voor maximale beschikbaarheid voor uw toepassing kunt maken. Dit zorgt ervoor dat tijdens een storing of rebootful onderhoud, ten minste één virtuele machine beschikbaar is.

Afzonderlijke virtuele machines worden in een beschikbaarheidsset verdeeld over maximaal 20 updatedomeinen (ud's). Tijdens het geplande onderhoud is slechts één updatedomein van invloed op een bepaald moment. Let erop dat de volgorde van update-domeinen wordt beïnvloed niet per se is gebeurd sequentieel worden verwerkt. 

Virtuele-machineschaalsets vormen een Azure compute-resource waarmee u kunt implementeren en beheren van een set identieke VM's als één bron. De schaalset wordt automatisch geïmplementeerd in meerdere updatedomeinen, zoals virtuele machines in een beschikbaarheidsset. Net als met beschikbaarheidssets, met schaalsets voor slechts één updatedomein is van invloed op een bepaald moment tijdens gepland onderhoud.

Zie voor meer informatie over het configureren van uw virtuele machines voor hoge beschikbaarheid, de beschikbaarheid van uw virtuele machines beheren voor [Windows](../articles/virtual-machines/windows/manage-availability.md) of [Linux](../articles/virtual-machines/linux/manage-availability.md).
