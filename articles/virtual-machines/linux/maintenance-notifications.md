---
title: Afhandeling van onderhoudsmeldingen voor virtuele Linux-machines in Azure | Microsoft Docs
description: Onderhoudsmeldingen voor Linux virtuele machines van Azure weergeven en starten van selfservice-onderhoud.
services: virtual-machines-linux
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/02/2018
ms.author: shants
ms.openlocfilehash: 860cb2bee902c6559b7851eb05fa9c5270876fe9
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58084722"
---
# <a name="handling-planned-maintenance-notifications-for-linux-virtual-machines"></a>Meldingen gepland onderhoud verwerken voor virtuele Linux-machines

Van tijd tot tijd voert Azure updates uit om de betrouwbaarheid, prestaties en veiligheid te verbeteren van de host-infrastructuur voor virtuele machines. Updates zijn wijzigingen, zoals de hostomgeving patches of bijwerken en buiten gebruik stellen van hardware. Een meerderheid van deze updates worden uitgevoerd zonder dat dit van invloed is op de gehoste virtuele machines. Er zijn echter gevallen waar updates indruk hebben:

- Als het onderhoud niet opnieuw worden opgestart vereist, gebruikt Azure in-place migratie voor de virtuele machine wordt onderbroken terwijl de host is bijgewerkt. Deze onderhoudsbewerkingen niet rebootful zijn toegepast foutdomein door foutdomein en voortgang is gestopt als er health waarschuwingssignalen worden ontvangen.

- Als u onderhoud moet worden opgestart, krijgt u een kennisgeving van wanneer het onderhoud is gepland. In dergelijke gevallen krijgt u een bepaalde periode waar u het onderhoud zelf beginnen kunt, wanneer het voor u werkt.


Gepland onderhoud die worden opgestart moet is in vaste blokken gepland. Elk blok heeft een ander bereik (regio's).

- Een golf begint met een melding aan klanten. Standaard wordt melding verzonden naar de eigenaar van het abonnement en mede-eigenaren. U kunt meer ontvangers en opties zoals e-mail, SMS en webhooks, toevoegen aan de meldingen met behulp van Azure [waarschuwingen voor activiteitenlogboeken](../../azure-monitor/platform/activity-logs-overview.md).  
- Op het moment van de melding een *selfservice venster* beschikbaar wordt gesteld. Tijdens dit venster kunt u zoeken die van uw virtuele machines zijn opgenomen in deze wave en proactief onderhoud starten op basis van de behoeften van uw eigen planning.
- Nadat het venster zelf een *geplande onderhoudsvenster* begint. Op een bepaald moment tijdens dit venster Azure plant en het vereiste onderhoud is van toepassing op uw virtuele machine. 

Het doel voor het hebben van twee windows is dat u voldoende tijd is voor onderhoud starten en opnieuw opstarten van uw virtuele machine en als Azure automatisch onderhoud wordt gestart.


U kunt de Azure portal, PowerShell, REST-API en CLI gebruiken voor uw VM's op te vragen voor de onderhoudsvensters en selfservice-onderhoud starten.

 
## <a name="should-you-start-maintenance-using-during-the-self-service-window"></a>Moet u beginnen met behulp van onderhoud tijdens het venster selfservice?  

De volgende richtlijnen kunt u beslissen of u deze mogelijkheid gebruik en onderhoud starten op uw eigen tijd. 

> [!NOTE] 
> Selfservice-onderhoud is mogelijk niet beschikbaar voor al uw VM's. Om te bepalen als proactieve opnieuw implementeren beschikbaar voor uw virtuele machine is, zoek de **nu starten** in de onderhoudsstatus weergegeven. Selfservice-onderhoud is momenteel niet beschikbaar voor Cloudservices (Web-/ Werkrol) en Service Fabric.


Selfservice-onderhoud wordt niet aanbevolen voor implementaties met **beschikbaarheidssets** omdat deze maximaal beschikbare installaties waarbij slechts één updatedomein is van invloed op een bepaald moment. 
- Laat Azure activeren van het onderhoud. Voor het onderhoud waarvoor opnieuw opstarten, er rekening mee dat het onderhoud updatedomein per updatedomein dat de update-domeinen niet per se het onderhoud sequentieel worden verwerkt wordt, wordt uitgevoerd en of er een pauze van 30 minuten tussen updatedomeinen. 
- Als een tijdelijk verlies van een aantal van de capaciteit van de (aantal 1/bijwerken) een probleem is, kan het eenvoudig worden gecompenseerd door toevoeging instanties toewijzen tijdens de onderhoudsperiode. 
- Voor het onderhoud van besturingssysteemsoftware die zonder opnieuw opstarten, updates worden toegepast op het niveau van de fout-domein. 

**Geen** selfservice-onderhoud gebruiken in de volgende scenario's: 
- Als u uw VM's vaak, hetzij handmatig, met DevTest Labs, met behulp van automatisch afsluiten of volgens een planning, het kan de onderhoudsstatus zijn en dus meer downtime veroorzaken.
- Op de tijdelijke virtuele machines dat u weet verwijderd vóór het einde van de onderhoudsgolf. 
- Voor workloads met een grote staat die zijn opgeslagen in de lokale (tijdelijke) schijf dat nodig is om te worden onderhouden tijdens het bijwerken. 
- Voor gevallen waarin u de grootte van uw virtuele machine vaak, zoals deze kan herstellen de onderhoudsstatus weergegeven. 
- Als u geplande gebeurtenissen waarmee proactieve failover of het correct afsluiten van uw werkbelasting, 15 minuten voor het begin van de onderhoudsmodus afsluiten is

**Gebruik** selfservice-onderhoud, als u van plan bent om uit te voeren van uw virtuele machine ononderbroken tijdens de fase gepland onderhoud en geen van de tegenpartij vermeldingen hierboven genoemde van toepassing zijn. 

Het is raadzaam te gebruiken van selfservice-onderhoud in de volgende gevallen:
- U moet een exacte onderhoudsvenster om uw beheer of de eindklant communiceren. 
- U moet de onderhoud voltooien door een bepaalde datum. 
- U moet voor het beheren van de volgorde van onderhoud, bijvoorbeeld, toepassing met meerdere lagen te garanderen veilig kunnen worden hersteld.
- U moet meer dan 30 minuten van de hersteltijd virtuele machine tussen twee updatedomeinen (ud's). Voor het beheren van de tijd tussen het update-domeinen, moet u onderhoud activeren op uw VM's één updatedomein (UD) tegelijkertijd.



## <a name="find-vms-scheduled-for-maintenance-using-cli"></a>Virtuele machines die zijn gepland voor onderhoud met behulp van CLI zoeken

Gepland onderhoudsinformatie kunt weergeven aan de hand [azure vm get-instance-view](/cli/azure/vm?view=azure-cli-latest).
 
Alleen als er onderhoud gepland is onderhoudsgegevens geretourneerd. Als er dat geen onderhoud gepland die gevolgen van de virtuele machine is, retourneert de opdracht geen onderhoudsinformatie. 

```azure-cli
az vm get-instance-view -g rgName -n vmName
```

De volgende waarden worden onder MaintenanceRedeployStatus geretourneerd: 

| Value | Description   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Geeft aan of u onderhoud op de virtuele machine op dit moment starten kunt |
| PreMaintenanceWindowStartTime         | Het begin van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt |
| PreMaintenanceWindowEndTime           | Het einde van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt |
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine start |
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine start |
| LastOperationResultCode               | Het resultaat van de laatste poging tot het starten van onderhoud op de virtuele machine |




## <a name="start-maintenance-on-your-vm-using-cli"></a>Onderhoud starten op de virtuele machine met behulp van CLI

De volgende oproep verzenden onderhoud op een virtuele machine worden gestart als `IsCustomerInitiatedMaintenanceAllowed` is ingesteld op true.

```azure-cli
az vm perform-maintenance -g rgName -n vmName 
```

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="classic-deployments"></a>Klassieke implementaties

Als u hebt nog steeds verouderde virtuele machines die zijn kunt geïmplementeerd met behulp van het klassieke implementatiemodel, u klassieke Azure CLI om query te gebruiken voor virtuele machines en starten van onderhoud.

Zorg ervoor dat u zich in de juiste modus voor klassieke virtuele machine door te typen:

```
azure config mode asm
```

De onderhoudsstatus van een virtuele machine met de naam ophalen *myVM*, type:

```
azure vm show myVM 
``` 

Onderhoud starten op de klassieke virtuele machine met de naam *myVM* in de *MijnService* service en *myDeployment* implementatie, type:

```
azure compute virtual-machine initiate-maintenance --service-name myService --name myDeployment --virtual-machine-name myVM
```


## <a name="faq"></a>Veelgestelde vragen


**V: Waarom moet u nu opnieuw opstarten van mijn virtuele machines?**

**A:** Het merendeel van de updates en upgrades van het Azure-platform, zijn niet van invloed op de beschikbaarheid van de virtuele machine, maar er zijn gevallen waar we kan niet worden vermeden opnieuw opstarten van virtuele machines die worden gehost in Azure. We hebben diverse wijzigingen waarvoor hostupdate met herstarten van onze servers die in virtuele machines opnieuw opstarten resulteren bij elkaar opgeteld.

**V: Als ik volg de aanbevelingen voor hoge beschikbaarheid met behulp van een Beschikbaarheidsset, ben ik veilig?**

**A:** Voor virtuele machines die worden geïmplementeerd in een beschikbaarheidsset of een schaalset voor virtuele machines is het begrip Updatedomein (UD) van toepassing. Wanneer u onderhoud uitvoert, wordt Azure zich houdt aan de UD-beperkingen en wordt niet opnieuw op virtuele machines van verschillende UD (binnen dezelfde beschikbaarheidsset).  Azure is ook moet wachten gedurende ten minste 30 minuten voordat u naar de volgende groep van virtuele machines. 

Zie voor meer informatie over hoge beschikbaarheid, [regio's en beschikbaarheid voor virtuele machines in Azure](regions-and-availability.MD).

**V: Hoe ik te worden geïnformeerd over gepland onderhoud?**

**A:** Een geplande onderhoudsgolf wordt gestart door een planning in te stellen op een of meer Azure-regio's. Direct nadat een e-mailmelding verzonden naar de eigenaars van abonnementen (één e-mailadres per abonnement). Extra kanalen en ontvangers voor deze melding kunnen worden geconfigureerd met behulp van waarschuwingen voor activiteitenlogboeken. Als u een virtuele machine naar een regio waar al gepland onderhoud is gepland implementeren, kunt u de melding niet ontvangt maar in plaats daarvan moet de onderhoudsstatus van de virtuele machine controleren.

**V: Ik zie geen melding over gepland onderhoud in de portal, Powershell of CLI. Wat is er aan de hand?**

**A:** Informatie met betrekking tot gepland onderhoud is beschikbaar tijdens een geplande onderhoudsgolf alleen voor de virtuele machines die worden beïnvloed. Met andere woorden, als u geen gegevens ziet, kan het zijn dat de onderhoudsgolf is al voltooid (of niet gestart) of dat uw virtuele machine al wordt gehost op een bijgewerkt server.

**V: Is er een manier om te weten precies zien wanneer mijn virtuele machine worden beïnvloed?**

**A:** Bij het instellen van de planning, definiëren we een tijdvenster van enkele dagen. Echter, de exacte volgorde van servers (en virtuele machines) binnen dit venster is onbekend. Klanten die graag weten de precieze tijd voor hun VM's kunnen gebruiken [geplande gebeurtenissen](scheduled-events.md) query uitvoert met een van de virtuele machine en een melding van 15 minuten voordat een virtuele machine opnieuw wordt opgestart.

**V: Hoe lang duurt het voordat mijn virtuele machine opnieuw is opgestart?**

**A:**  Afhankelijk van de grootte van uw virtuele machine kan opnieuw opstarten duren enkele minuten tijdens het selfservice-onderhoudsvenster. Tijdens de door Azure geïnitieerde opnieuw opstarten in het geplande onderhoudsvenster, het opnieuw opstarten duurt doorgaans ongeveer 25 minuten. Houd er rekening mee dat in het geval u Cloudservices (Web-/ Werkrol), Virtual Machine Scale Sets of beschikbaarheidssets gebruikt, u 30 minuten tussen elke groep van virtuele machines (UD) tijdens het geplande onderhoudsvenster krijgt.

**V: Wat is de ervaring in het geval van Virtual Machine Scale Sets?**

**A:** Gepland onderhoud is nu beschikbaar voor Virtual Machine Scale Sets. Voor instructies over het starten van selfservice-onderhoud verwijzen [gepland onderhoud voor VMSS](../../virtual-machine-scale-sets/virtual-machine-scale-sets-maintenance-notifications.md) document.

**V: Wat is de ervaring in het geval van Cloudservices (Web-/ Werkrol) en Service Fabric?**

**A:** Hoewel deze platformen worden beïnvloed door gepland onderhoud, worden klanten die gebruikmaken van deze platformen als veilig beschouwd, omdat alleen virtuele machines binnen een enkel Upgrade Domain (UD) tegelijkertijd worden beïnvloed. Selfservice-onderhoud is momenteel niet beschikbaar voor Cloudservices (Web-/ Werkrol) en Service Fabric.

**V: Ik zie niet alle onderhoudsinformatie op mijn virtuele machines. Wat er mis ging?**

**A:** Er zijn diverse redenen waarom u niet alle onderhoudsgegevens op uw virtuele machines ervaart nog:
1.  U kunt een abonnement dat is gemarkeerd als Microsoft interne worden gebruikt.
2.  Uw VM's zijn niet gepland voor onderhoud. Kan het zijn dat de onderhoudsgolf is beëindigd, geannuleerd of gewijzigd, zodat uw virtuele machines niet meer worden beïnvloed door deze.
3.  U hoeft niet de **onderhoud** kolom toegevoegd aan uw VM-lijstweergave. Hoewel we deze kolom aan de standaardweergave hebt toegevoegd, klanten die geconfigureerd om te zien van niet-standaard kolommen handmatig moeten toevoegen de **onderhoud** kolom aan de VM-lijstweergave.

**V: Mijn virtuele machine is gepland voor onderhoud voor de tweede keer. Waarom is dat?**

**A:** In bepaalde gevallen is er onderhoud voor uw VM gepland terwijl dit al is uitgevoerd en uw VM opnieuw is geïmplementeerd:
1.  We hebben de onderhoudsgolf geannuleerd en deze opnieuw gestart met een andere nettolading. Het kan zijn dat we hebben de nettolading van de mislukte gedetecteerd en moet u een extra nettolading implementeren.
2.  Uw virtuele machine werd *uw* naar een ander knooppunt vanwege een hardwarefout.
3.  U hebt geselecteerd om te stoppen (toewijzing ongedaan maken) en start de VM opnieuw.
4.  U hebt **automatisch afsluiten** ingeschakeld voor de virtuele machine.


## <a name="next-steps"></a>Volgende stappen

Leer hoe u zich kunt registreren voor onderhoudsgebeurtenissen binnen de virtuele machine met behulp [geplande gebeurtenissen](scheduled-events.md).
