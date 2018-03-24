---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: cynthn
ms.service: virtual-machines
ms.topic: include
ms.date: 03/09/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: e484dac645ff2e5867d2e652c389a9950e8bac12
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
---
Azure voert regelmatig updates ter verbetering van de betrouwbaarheid, prestaties en beveiliging van de infrastructuur van de host voor virtuele machines. Deze updates tussen softwareonderdelen in de hosting-omgeving (zoals het besturingssysteem, hypervisor en verschillende agents die zijn geïmplementeerd op de host) patching bijwerken netwerkonderdelen, naar hardware buiten gebruik stellen. De meeste van deze updates worden uitgevoerd zonder nadelige gevolgen voor de gehoste virtuele machines. Er zijn echter gevallen bekend waarbij updates gevolgen:

- Als een update opnieuw opstarten minder mogelijk is, gebruikt Azure geheugen behouden onderhoud onderbreken van de virtuele machine, terwijl de host is bijgewerkt of de virtuele machine wordt verplaatst naar een host al bijgewerkte helemaal.

- Als u onderhoud moet worden opgestart, krijgt u een bericht wanneer het onderhoud is gepland. In dergelijke gevallen u ook krijgt een tijdvenster op waar u kunt beginnen het onderhoud zelf tegelijk die geschikt is voor u.

Deze pagina wordt beschreven hoe Microsoft Azure uitvoert voor beide typen van onderhoud. Zie voor meer informatie over niet-geplande gebeurtenissen (uitval), de beschikbaarheid van virtuele machines beheren voor [Windows] (... / articles/virtual-machines/windows/manage-availability.md) of [Linux](../articles/virtual-machines/linux/manage-availability.md).

Toepassingen die worden uitgevoerd in een virtuele machine kunnen informatie verzamelen over toekomstige updates met behulp van de Service Azure metagegevens voor [Windows](../articles/virtual-machines/windows/instance-metadata-service.md) of [Linux] (... / articles/virtual-machines/linux/instance-metadata-service.md).

Zie voor 'procedures' informatie over het beheren van gepland onderhoud 'Verwerken geplande onderhoud meldingen' voor [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of [Windows](../articles/virtual-machines/windows/maintenance-notifications.md).

## <a name="memory-preserving-maintenance"></a>Geheugen onderhoud behouden

Als updates niet volledig opnieuw opstarten vereist, wordt geheugen behouden onderhoud mechanismen worden gebruikt voor het beperken van de impact op de virtuele machine. De virtuele machine is onderbroken voor maximaal 30 seconden, het geheugen in RAM, behouden terwijl de hosting-omgeving van toepassing de vereiste updates en patches is of de virtuele machine naar een host al bijgewerkt verplaatst. De virtuele machine wordt vervolgens hervat en de klok van de virtuele machine automatisch wordt gesynchroniseerd. 

Voor virtuele machines in beschikbaarheidssets, worden bijgewerkt een voor een update-domeinen. Alle virtuele machines in één updatedomein (UD) worden onderbroken, bijgewerkt en hervat vervolgens voordat gepland onderhoud verder met de volgende UD gaat.

Sommige toepassingen worden beïnvloed door deze typen updates. Toepassingen die realtime-gebeurtenissen verwerken, zoals mediastreaming of transcodering of hoge doorvoersnelheid scenario's, toegang uitvoeren kunnen niet worden ontworpen voor een pauze van 30 seconde tolereren. <!-- sooooo, what should they do? --> Als de virtuele machine wordt verplaatst naar een andere host, betekent dit dat sommige gevoelige werkbelastingen wellicht opgevallen dat er een lichte prestatievermindering in de paar minuten aan de virtuele Machine onderbreken geleid. 


## <a name="maintenance-requiring-a-reboot"></a>Onderhoud vereisen opnieuw opstarten

Wanneer virtuele machines opnieuw worden opgestart voor gepland onderhoud, u een melding van tevoren. Gepland onderhoud heeft twee fasen: het venster selfservice en een geplande onderhoudsvenster.

De **selfservice venster** kunt u het onderhoud op uw virtuele machines starten. U kunt elke virtuele machine om te zien van hun status en controleer het resultaat van uw laatste verzoek in de onderhoudsmodus query tijdens deze periode.

Wanneer u onderhoud selfservice start, wordt uw virtuele machine wordt verplaatst naar een knooppunt dat al is bijgewerkt en wordt deze vervolgens weer ingeschakeld. Omdat de virtuele machine opnieuw is opgestart, is de tijdelijke schijf verloren en dynamische IP-adressen die zijn gekoppeld aan virtuele netwerkinterface worden bijgewerkt.

Als u onderhoud selfservice start en er een fout opgetreden tijdens het proces is, de bewerking is gestopt, de virtuele machine is niet bijgewerkt en wordt deze ook verwijderd uit de herhaling gepland onderhoud. U wordt verbinding gemaakt met in een later tijdstip met een nieuw schema en krijgt een nieuwe mogelijkheid selfservice onderhoud doen. 

Wanneer het venster selfservice is verstreken, de **geplande onderhoudsvenster** begint. Tijdens dit tijdvenster kunt u nog steeds een query voor het onderhoudsvenster, maar niet langer mogelijk om te starten van het onderhoud zelf.

Zie voor informatie over het beheren van een programma onderhoud 'Verwerken geplande onderhoud meldingen' voor [Linux](../articles/virtual-machines/linux/maintenance-notifications.md) of [Windows](../articles/virtual-machines/windows/maintenance-notifications.md). 

## <a name="availability-considerations-during-planned-maintenance"></a>Overwegingen voor beschikbaarheid tijdens gepland onderhoud 

Als u besluit om te wachten tot het geplande onderhoudsvenster, moet u er een paar aandachtspunten voor het onderhouden van de hoogste beschikbaarheid van uw virtuele machines zijn. 

### <a name="paired-regions"></a>Gekoppelde regio 's

Elke Azure-regio is gekoppeld aan een andere regio binnen de dezelfde Geografie, samen een combinatie van regionale indienen. Azure wordt alleen de virtuele machines in één regio uit een combinatie van regio bijgewerkt tijdens gepland onderhoud. Wanneer bijvoorbeeld de virtuele machines in Noord-centraal VS worden bijgewerkt, worden er tegelijkertijd geen virtuele machines in Zuid-centraal VS bijgewerkt. Tegelijkertijd met VS - oost kan er echter wel onderhoud plaatsvinden in andere regio's, zoals Noord-Europa. Uw VM's begrijpen hoe regio paren werken, kunt u beter verdelen over regio's. Zie voor meer informatie [Azure-regio paren](https://docs.microsoft.com/azure/best-practices-availability-paired-regions).

### <a name="availability-sets-and-scale-sets"></a>Beschikbaarheidssets en schaalsets

Bij het implementeren van een werklast op Azure Virtual machines, kunt u de virtuele machines binnen een beschikbaarheidsset voor maximale beschikbaarheid voor uw toepassing. Dit zorgt ervoor dat bij een stroomstoring of een onderhoud gebeurtenissen, ten minste één virtuele machine beschikbaar is.

Afzonderlijke virtuele machines worden in een beschikbaarheidsset verdeeld over maximaal 20 update domeinen (UDs). Tijdens gepland onderhoud één updatedomein is dit van invloed op een bepaald moment. Let erop dat de volgorde van update-domeinen die worden beïnvloed noodzakelijkerwijs niet opeenvolgend gebeurt. 

Virtuele-machineschaalsets zijn een Azure compute resource waarmee u kunt implementeren en beheren van een set van identieke virtuele machines als één bron. De schaalaanpassingsset wordt automatisch geïmplementeerd in meerdere domeinen van de update, zoals virtuele machines in een beschikbaarheidsset. Net als met beschikbaarheidssets met schaalsets één updatedomein is dit van invloed op een bepaald moment.

Zie voor meer informatie over het configureren van uw virtuele machines voor hoge beschikbaarheid, de beschikbaarheid van uw virtuele machines beheren voor [Windows](../articles/virtual-machines/windows/manage-availability.md) of [Linux](../articles/virtual-machines/linux/manage-availability.md).
