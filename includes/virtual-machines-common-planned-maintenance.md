---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: shants123
ms.service: virtual-machines
ms.topic: include
ms.date: 4/30/2019
ms.author: shants
ms.custom: include file
ms.openlocfilehash: c2931fa410cf92755a5df5b7129dcf93de900930
ms.sourcegitcommit: 778e7376853b69bbd5455ad260d2dc17109d05c1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/23/2019
ms.locfileid: "66155915"
---
Azure werkt periodiek het platform voor het verbeteren van de betrouwbaarheid, prestaties en beveiliging van de host-infrastructuur voor virtuele machines. Het doel van deze updates kan variëren van het patchen van software-onderdelen in de hostomgeving upgraden netwerkonderdelen of hardware buiten gebruik stellen. 

Updates van zelden invloed op de gehoste virtuele machines. Wanneer updates wel van invloed is, kiest Azure de minste krachtige methode voor updates:

- Als de update niet opnieuw opstarten vereist, wordt de virtuele machine wordt onderbroken terwijl de host is bijgewerkt, of de virtuele machine live-gemigreerd naar een reeds bijgewerkte host wordt.

- Als u onderhoud moet worden opgestart, krijgt u een melding van het geplande onderhoud. Azure biedt ook een bepaalde periode waarin kun u het onderhoud zelf op een tijdstip die bij u past. Het selfservice-onderhoudsvenster is doorgaans 30 dagen, tenzij het onderhoud urgent is. Azure investeert in technologieën te verminderen van het aantal cases waarin geplande platform onderhoud vereist dat de virtuele machines opnieuw worden opgestart. 

Deze pagina wordt beschreven hoe Azure presteert voor beide typen onderhoud. Zie voor meer informatie over niet-geplande gebeurtenissen (uitval) [beheren van de beschikbaarheid van virtuele machines voor Windows](../articles/virtual-machines/windows/manage-availability.md) of het bijbehorende artikel voor [Linux](../articles/virtual-machines/linux/manage-availability.md).

Binnen een virtuele machine, krijgt u meldingen over toekomstig onderhoud door [met behulp van geplande gebeurtenissen voor Windows](../articles/virtual-machines/windows/scheduled-events.md) of voor [Linux](../articles/virtual-machines/linux/scheduled-events.md).

Zie voor instructies over het beheren van gepland onderhoud [meldingen gepland onderhoud verwerken voor Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of het bijbehorende artikel voor [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="maintenance-that-doesnt-require-a-reboot"></a>Onderhoud dat geen herstart vereist

Zoals eerder vermeld, zijn de meeste platform-updates niet van invloed op klant-VM's. Wanneer een update zonder gevolgen niet mogelijk is, kiest Azure het mechanisme voor updates die minimaal impactful klant virtuele machines. 

De meeste andere impact onderhoud pauzeert de virtuele machine minder dan 10 seconden. In bepaalde gevallen gebruikt Azure geheugenbehoud onderhoud mechanismen. Deze mechanismen voor de virtuele machine onderbreken gedurende maximaal 30 seconden en het geheugen in het RAM behouden. De virtuele machine wordt vervolgens hervat, en wordt de klok automatisch gesynchroniseerd. 

Geheugenbehoud onderhoud werkt voor meer dan 90 procent van de Azure-VM's. Werkt niet voor G, M, N en H-serie. Azure maakt gebruik van live migratie technologieën steeds en verbetert de geheugenbehoud onderhoud mechanismen voor het verminderen van de duur onderbreken.  

Deze onderhoudsbewerkingen waarvoor opnieuw opstarten niet zijn toegepast, een foutdomein op een tijdstip. Proces wordt gestopt als ze signalen van de status waarschuwing ontvangen. 

Deze typen updates kunnen sommige toepassingen beïnvloeden. Wanneer de virtuele machine live-gemigreerd naar een andere host is, kunnen sommige gevoelige werkbelastingen een lichte prestatievermindering weergeven in de aanloop naar de virtuele machine onderbreken minuten. Als u wilt voorbereiden voor het onderhoud van de virtuele machine en de gevolgen tijdens het onderhoud van Azure, probeer [met behulp van geplande gebeurtenissen voor Windows](../articles/virtual-machines/windows/scheduled-events.md) of [Linux](../articles/virtual-machines/linux/scheduled-events.md) voor zulke toepassingen. Azure is bezig met onderhoud-control-functies voor deze gevoelige toepassingen. 

### <a name="live-migration"></a>Live migratie

Livemigratie is een bewerking die niet moeten worden opgestart en met geheugen voor de virtuele machine. Dit zorgt ervoor dat een onderbreking of stilzetten, duurt meestal niet meer dan 5 seconden. Met uitzondering van G, M, N en H-serie, alle infrastructuur als een service (IaaS) virtuele machines, in aanmerking komen voor livemigratie. In aanmerking komende virtuele machines zijn meer dan 90 procent van de IaaS-VM's die zijn geïmplementeerd voor de Azure vloot. 

Het Azure-platform wordt live migratie begint in de volgende scenario's:
- Gepland onderhoud
- Hardware-uitval
- Optimalisaties van toewijzing

Sommige scenario's voor gepland onderhoud gebruikmaken van live migratie en kunt u geplande gebeurtenissen van tevoren weten wanneer live migratie wordt gestart.

Livemigratie kan ook worden gebruikt om te verplaatsen van virtuele machines als Azure Machine Learning-algoritmen voorspellen een aanstaande hardware-uitval of als u wilt optimaliseren toewijzingen van virtuele machine. Zie voor meer informatie over de voorspellende modellen die exemplaren van verminderde hardware detecteert, [verbetering van Azure VM-tolerantie met voorspellende machine learning en livemigratie](https://azure.microsoft.com/blog/improving-azure-virtual-machine-resiliency-with-predictive-ml-and-live-migration/?WT.mc_id=thomasmaurer-blog-thmaure). Live migratie meldingen worden weergegeven in de logboeken controleren en de status van de Service ook zoals geplande gebeurtenissen in de Azure Portal als u deze services.

## <a name="maintenance-that-requires-a-reboot"></a>Onderhoud dat worden opgestart moet

In het zeldzame geval waarbij virtuele machines opnieuw worden opgestart voor gepland onderhoud, u krijgt een bericht van tevoren. Gepland onderhoud heeft twee fasen: de selfservice-fase en een fase gepland onderhoud.

Tijdens de *selfservice fase*, die meestal vier weken duurt, u het onderhoud starten op uw virtuele machines. U kunt elke virtuele machine om te zien van de status en het resultaat van de laatste aanvraag voor onderhoud opvragen als onderdeel van de selfservicegebruikersrol.

Als u selfservice-onderhoud start, wordt uw virtuele machine opnieuw geïmplementeerd naar een reeds bijgewerkte knooppunt. Omdat de virtuele machine opnieuw is opgestart, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan de virtuele netwerkinterface worden bijgewerkt.

Als een fout tijdens selfservice-onderhoud, de bewerking stopt optreden, de virtuele machine wordt niet bijgewerkt en krijgt u de mogelijkheid om opnieuw te proberen de selfservice-onderhoud. 

Wanneer de fase selfservice is beëindigd, de *fase gepland onderhoud* begint. U kunt nog steeds een query voor de onderhoudsfase tijdens deze fase, maar u niet het onderhoud zelf starten.

Zie voor meer informatie over het beheren van onderhoud dat worden opgestart moet, [meldingen gepland onderhoud verwerken voor Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of het bijbehorende artikel voor [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

### <a name="availability-considerations-during-scheduled-maintenance"></a>Beschikbaarheidsoverwegingen tijdens gepland onderhoud 

Als u besluit om te wachten tot de fase gepland onderhoud, zijn er enkele dingen die u moet rekening houden met de hoogst mogelijke beschikbaarheid van uw VM's onderhouden. 

#### <a name="paired-regions"></a>Gekoppelde regio's

Elke Azure-regio is gekoppeld aan een andere regio binnen de dezelfde geografische omgeving bevindt. Samen maken ze een combinatie van de regio. Tijdens de fase gepland onderhoud werkt Azure alleen de virtuele machines in één regio van een paar regio. Bijvoorbeeld bij het bijwerken van de virtuele machine in Noord-centraal VS, Azure niet worden bijgewerkt alle virtuele machines in Zuid-centraal VS op hetzelfde moment. Tegelijkertijd met US - oost kan er echter wel onderhoud plaatsvinden in andere regio's, zoals Europa - noord. Uw VM's inzicht in de werking van gekoppelde regio's, kunt u beter verdelen over regio's. Zie voor meer informatie, [Azure regioparen](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

#### <a name="availability-sets-and-scale-sets"></a>Beschikbaarheidssets en schaalsets

Bij het implementeren van een workload op virtuele Azure-machines, kunt u de virtuele machines binnen een *beschikbaarheidsset* voor maximale beschikbaarheid voor uw toepassing. Met behulp van beschikbaarheidssets, kunt u ervoor zorgen dat tijdens een storing of een onderhoud gebeurtenissen die worden opgestart moeten, ten minste één virtuele machine beschikbaar is.

Afzonderlijke virtuele machines worden in een beschikbaarheidsset verdeeld over maximaal 20 updatedomeinen (ud's). Tijdens het geplande onderhoud is op een bepaald moment slechts één UD bijgewerkt. Ud's niet noodzakelijkerwijs sequentieel zijn bijgewerkt. 

Virtuele machine *schaalsets* een Azure compute-resource die u kunt gebruiken om te implementeren en beheren van een set identieke VM's als één resource zijn. De schaalset wordt automatisch geïmplementeerd op ud's, zoals virtuele machines in een beschikbaarheidsset. Als met beschikbaarheidssets, wordt wanneer u schaalsets, met slechts één UD bijgewerkt op een bepaald moment tijdens gepland onderhoud.

Zie voor meer informatie over het instellen van uw VM's voor hoge beschikbaarheid [beheren van de beschikbaarheid van uw virtuele machines voor Windows](../articles/virtual-machines/windows/manage-availability.md) of het bijbehorende artikel voor [Linux](../articles/virtual-machines/linux/manage-availability.md).
