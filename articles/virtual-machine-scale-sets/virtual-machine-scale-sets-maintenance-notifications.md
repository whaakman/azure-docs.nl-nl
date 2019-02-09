---
title: Hiermee stelt u onderhoudsmeldingen voor virtuele-machineschaalsets in Azure | Microsoft Docs
description: Onderhoudsmeldingen weergeven en starten van selfservice-onderhoud voor virtuele-machineschaalsets in Azure.
services: virtual-machine-scale-sets
documentationcenter: ''
author: shants123
editor: ''
tags: azure-service-management,azure-resource-manager
ms.assetid: ''
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/09/2018
ms.author: shants
ms.openlocfilehash: d82e0aa1f803001cf3bab5ec133a59f1fe19e4aa
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55981414"
---
# <a name="planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Meldingen voor gepland onderhoud voor virtuele-machineschaalsets


Azure voert regelmatig updates ter verbetering van de betrouwbaarheid, prestaties en beveiliging van de host-infrastructuur voor virtuele machines (VM's). Updates bevatten mogelijk de hostomgeving patches of bijwerken en buiten gebruik stellen van hardware. De meeste updates hebben geen invloed op de gehoste virtuele machines. Echter updates een invloed hebben op virtuele machines in deze scenario's:

- Als het onderhoud niet opnieuw opstarten vereist, gebruikt Azure in-place migratie voor de virtuele machine wordt onderbroken terwijl de host is bijgewerkt. Onderhoudsbewerkingen waarvoor opnieuw opstarten niet zijn toegepast foutdomein door foutdomein. Voortgang is gestopt als er health waarschuwingssignalen worden ontvangen.

- Als u onderhoud moet worden opgestart, ontvangt u een melding waarin wordt weergegeven wanneer het onderhoud is gepland. In dergelijke gevallen krijgt u een venster waarin u het onderhoud zelf beginnen kunt wanneer u het beste werkt voor u.


Gepland onderhoud die worden opgestart moet is in vaste blokken gepland. Elk blok heeft een ander bereik (regio's):

- Een golf begint met een melding aan klanten. Standaard wordt melding verzonden naar de abonnement-eigenaar en mede-eigenaren. U kunt ontvangers en opties zoals e-mail, SMS en webhooks voor meldingen toevoegen met behulp van Azure [waarschuwingen voor activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md).  
- Met de melding een *selfservice venster* beschikbaar wordt gesteld. Tijdens dit venster kunt u vinden die van uw VM's zijn opgenomen in de wave. U kunt onderhoud proactief te starten op basis van de behoeften van uw eigen planning.
- Nadat het venster zelf een *geplande onderhoudsvenster* begint. Op een bepaald moment tijdens dit venster Azure plant en het vereiste onderhoud is van toepassing op uw virtuele machine. 

Het doel voor het hebben van twee windows is dat u voldoende tijd is voor onderhoud starten en opnieuw opstarten van uw virtuele machine en als Azure automatisch onderhoud wordt gestart.

U kunt de Azure-portal, PowerShell, de REST-API en de Azure CLI gebruiken om op te vragen voor onderhoudsvensters voor uw virtuele-machineschaalset VM's en selfservice-onderhoud starten.

## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Moet u onderhoud starten tijdens het venster selfservice?  

De volgende richtlijnen kunt u beslissen of u onderhoud starten op een tijdstip dat u kiest.

> [!NOTE] 
> Selfservice-onderhoud is mogelijk niet beschikbaar voor al uw VM's. Om te bepalen of proactieve opnieuw implementeren beschikbaar voor uw virtuele machine is, zoek naar **nu starten** in de onderhoudsstatus weergegeven. Selfservice-onderhoud is momenteel niet beschikbaar voor Azure-Cloudservices (Web-/ Werkrol) en Azure Service Fabric.


Selfservice-onderhoud wordt niet aanbevolen voor implementaties die gebruikmaken van *beschikbaarheidssets*. Beschikbaarheidssets zijn maximaal beschikbare installaties in welke update slechts één domein op elk gewenst moment wordt beïnvloed. Voor beschikbaarheidssets:

- Laat Azure activeren van het onderhoud. Voor het onderhoud dat worden opgestart moet, gebeurt onderhoud updatedomein per updatedomein. Update-domeinen ontvangt niet per se sequentieel het onderhoud. Er is een pauze van 30 minuten tussen updatedomeinen.
- Als een tijdelijk verlies van een aantal van de capaciteit van de (aantal 1/bijwerken) een probleem is, kunt u eenvoudig compenseren voor het verlies van gegevens door het toewijzen van extra exemplaren tijdens de onderhoudsperiode.
- Voor onderhoud die niet worden opgestart moet, worden de updates toegepast op het niveau van de fout-domein. 
    
**Geen** selfservice-onderhoud gebruiken in de volgende scenario's: 

- Als u uw VM's vaak wordt afgesloten handmatig met behulp van DevTest Labs, met behulp van automatisch afsluiten of door een schema te volgen. Selfservice-onderhoud in deze scenario's mogelijk de onderhoudsstatus terugkeren en aanvullende downtime veroorzaken.
- Op de tijdelijke virtuele machines dat u weet verwijderd vóór het einde van de onderhoudsgolf. 
- Voor workloads met een grote staat die zijn opgeslagen in de lokale (tijdelijke) schijf die u wilt behouden na-update. 
- Als u de grootte van uw virtuele machine vaak. In dit scenario kan de onderhoudsstatus herstellen. 
- Als u geplande gebeurtenissen waarmee proactieve failover of het correct afsluiten van uw workload 15 minuten voordat de onderhoudsmodus afsluiten begint hebt vastgesteld.

**Voer** selfservice-onderhoud gebruiken als u van plan bent om uit te voeren van uw virtuele machine ononderbroken tijdens de fase gepland onderhoud en geen van de voorgaande counterindications toe te passen. 

Het is raadzaam te gebruiken van selfservice-onderhoud in de volgende gevallen:

- U moet een exacte onderhoudsvenster management of uw klanten communiceren. 
- U moet de onderhoud uitvoeren op een specifieke datum. 
- U moet voor het beheren van de volgorde van onderhoud, bijvoorbeeld in een toepassing met meerdere lagen, om te waarborgen veilig kunnen worden hersteld.
- U moet meer dan 30 minuten van de hersteltijd virtuele machine tussen twee updatedomeinen. Voor het beheren van de tijd tussen het update-domeinen, moet u onderhoud activeren op uw VM's één updatedomein tegelijk.

 
## <a name="view-virtual-machine-scale-sets-that-are-affected-by-maintenance-in-the-portal"></a>Weergave virtuele-machineschaalsets die worden beïnvloed door onderhoud in de portal

Wanneer een geplande onderhoudsgolf is gepland, kunt u de lijst met schaalsets voor virtuele machines die worden beïnvloed door de geplande onderhoudsgolf met behulp van de Azure-portal weergeven. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **alle services**, en selecteer vervolgens **virtuele-machineschaalsets**.
3. Onder **virtuele-machineschaalsets**, selecteer **kolommen bewerken** om de lijst van beschikbare kolommen te openen.
4. In de **beschikbare kolommen** sectie, selecteer **selfservice-onderhoud**, en verplaats deze naar de **kolommen geselecteerd** lijst. Selecteer **Toepassen**.  

    Om de **selfservice-onderhoud** item gemakkelijker te vinden, kunt u de optie-omlaag in de **beschikbare kolommen** rubriek van **alle** naar  **Eigenschappen van**.

De **selfservice-onderhoud** kolom wordt nu weergegeven in de lijst met virtuele-machineschaalsets. Elke virtuele-machineschaalset kan een van de volgende waarden voor de selfservice-onderhoud-kolom hebben:

| Value | Description |
|-------|-------------|
| Ja | Er is ten minste één virtuele machine in uw virtuele-machineschaalset in een venster selfservice. U kunt onderhoud starten op elk gewenst moment tijdens dit venster selfservice. | 
| Nee | Er zijn geen virtuele machines in een selfservice-venster in de betrokken virtuele-machineschaalset ingesteld. | 
| - | De schaal van uw virtuele machines wordt geen deel uitmaken van een geplande onderhoudsgolf.| 

## <a name="notification-and-alerts-in-the-portal"></a>Meldingen en waarschuwingen in de portal

Een schema voor gepland onderhoud communiceert Azure met een e-mailbericht verzenden naar de abonnement-eigenaar en mede-eigenaars-groep. U kunt ontvangers en kanalen toevoegen aan deze communicatie met het maken van waarschuwingen voor activiteitenlogboek. Zie voor meer informatie, [abonnement activiteiten met de Azure-activiteitenlogboek](../azure-monitor/platform/activity-logs-overview.md).

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu links **Monitor**. 
3. In de **Monitor - waarschuwingen (klassiek)** venster **+ waarschuwing voor activiteitenlogboek toevoegen**.
4. Op de **waarschuwing voor activiteitenlogboek toevoegen** pagina, selecteer of Voer de gevraagde informatie. In **Criteria**, zorg ervoor dat u de volgende waarden ingesteld:
   - **Gebeurteniscategorie**: Selecteer **servicestatus**.
   - **Services**: Selecteer **Schaalsets voor virtuele machines en virtuele Machines**.
   - **Type**: Selecteer **gepland onderhoud**. 
    
Zie voor meer informatie over het configureren van waarschuwingen voor activiteitenlogboek, [waarschuwingen voor activiteitenlogboek maken](../azure-monitor/platform/activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Onderhoud starten op de virtuele-machineschaalset vanuit de portal

U kunt meer onderhoud gerelateerde details in het overzicht van schaalsets voor virtuele machines weergeven. Als ten minste één virtuele machine in de virtuele-machineschaalset is opgenomen in de geplande onderhoudsgolf, wordt een nieuwe melding-lint toegevoegd aan de bovenkant van de pagina. Selecteer het lint melding naar de **onderhoud** pagina. 

Op de **onderhoud** pagina, kunt u zien welke VM-exemplaar wordt beïnvloed door het geplande onderhoud. Schakel het selectievakje dat overeenkomt met de betreffende virtuele machine voor het starten van onderhoud. Selecteer **onderhoud starten**.

Nadat u hebt onderhoud gestart, wordt de betrokken VM's in uw virtuele-machineschaalset onderhoud ondergaan en zijn tijdelijk niet beschikbaar. Als u het venster selfservice hebt gemist, kunt u het tijdvenster kunt nog steeds zien wanneer uw virtuele-machineschaalset wordt onderhouden door Azure.
 
## <a name="check-maintenance-status-by-using-powershell"></a>De onderhoudsstatus controleren met behulp van PowerShell

U kunt Azure PowerShell gebruiken om te zien wanneer virtuele machines in uw virtuele machine-schaalsets zijn gepland voor onderhoud. Gepland onderhoudsinformatie is beschikbaar via de [Get-AzVmss](https://docs.microsoft.com/powershell/module/az.compute/get-azvmss) cmdlet als u de `-InstanceView` parameter.
 
Alleen als het onderhoud is gepland onderhoudsgegevens geretourneerd. Als geen onderhoud is gepland dat betrekking heeft op het VM-exemplaar, niet de cmdlet geen onderhoudsgegevens geretourneerd. 

```powershell
Get-AzVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

De volgende eigenschappen worden geretourneerd bij **MaintenanceRedeployStatus**: 
| Value | Description   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Geeft aan of u onderhoud op de virtuele machine op dit moment starten kunt. ||
| PreMaintenanceWindowStartTime         | Het begin van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt. ||
| PreMaintenanceWindowEndTime           | Het einde van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt. ||
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoud waarin Azure onderhoud op de virtuele machine start. ||
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine start. ||
| LastOperationResultCode               | Het resultaat van de laatste poging tot het starten van onderhoud op de virtuele machine. ||



### <a name="start-maintenance-on-your-vm-instance-by-using-powershell"></a>Onderhoud starten op uw VM-exemplaar met behulp van PowerShell

U kunt onderhoud starten op een virtuele machine als **IsCustomerInitiatedMaintenanceAllowed** is ingesteld op **waar**. Gebruik de [Set AzVmss](/powershell/module/az.compute/set-azvmss) cmdlet met `-PerformMaintenance` parameter.

```powershell
Set-AzVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-by-using-the-cli"></a>De onderhoudsstatus controleren met behulp van CLI

U kunt ook gepland onderhoudsinformatie weergeven met behulp van [az vmss list-instances](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances).
 
Alleen als het onderhoud is gepland onderhoudsgegevens geretourneerd. Als geen onderhoud dat betrekking heeft op het VM-exemplaar is gepland, niet de opdracht geen onderhoudsgegevens geretourneerd. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

De volgende eigenschappen worden geretourneerd bij **MaintenanceRedeployStatus** voor elk VM-exemplaar: 
| Value | Description   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Geeft aan of u onderhoud op de virtuele machine op dit moment starten kunt. ||
| PreMaintenanceWindowStartTime         | Het begin van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt. ||
| PreMaintenanceWindowEndTime           | Het einde van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt. ||
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoud waarin Azure onderhoud op de virtuele machine start. ||
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine start. ||
| LastOperationResultCode               | Het resultaat van de laatste poging tot het starten van onderhoud op de virtuele machine. ||


### <a name="start-maintenance-on-your-vm-instance-by-using-the-cli"></a>Onderhoud starten op uw VM-exemplaar met behulp van CLI

De volgende oproep verzenden initieert onderhoud op een VM-exemplaar als `IsCustomerInitiatedMaintenanceAllowed` is ingesteld op **waar**:

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>Veelgestelde vragen

**V: Waarom moet u nu opnieuw opstarten van mijn VM's?**

**A:** Hoewel de meeste updates en upgrades naar het Azure-platform geen invloed op een VM-beschikbaarheid, in sommige gevallen vermijden we kan niet opnieuw opstarten van virtuele machines die worden gehost in Azure. We hebben diverse wijzigingen waarvoor hostupdate met herstarten van onze servers die in de VM opnieuw wordt opgestart resulteren bij elkaar opgeteld.

**V: Als ik Ga als volgt wordt uw aanbevelingen voor hoge beschikbaarheid met behulp van een beschikbaarheidsset instellen, ben ik veilig?**

**A:** Virtuele machines die zijn geïmplementeerd in een beschikbaarheidsset instellen of in virtuele-machineschaalsets gebruiken updatedomeinen. Bij het uitvoeren van onderhoud Azure zich houdt aan de beperking van update-domein en virtuele machines van een ander updatedomein (binnen dezelfde beschikbaarheidsset) wordt niet opnieuw op. Azure is ook moet wachten gedurende ten minste 30 minuten voordat u naar de volgende groep van virtuele machines. 

Zie voor meer informatie over hoge beschikbaarheid, [regio's en beschikbaarheid voor virtuele machines in Azure](../virtual-machines/windows/regions-and-availability.md).

**V: Hoe kan ik gewaarschuwd over gepland onderhoud?**

**A:** Een geplande onderhoudsgolf wordt gestart door een planning in te stellen op een of meer Azure-regio's. Direct nadat een e-mailmelding verzonden naar de eigenaars van abonnementen (één e-mailadres per abonnement). U kunt de kanalen en ontvangers voor deze melding toevoegen met behulp van waarschuwingen voor activiteitenlogboek. Als u een virtuele machine in een regio waarin gepland onderhoud is gepland implementeert, kunt u de melding niet ontvangen. In plaats daarvan, Controleer de onderhoudsstatus van het van de virtuele machine.

**V: Ik zie geen melding over gepland onderhoud in de portal, PowerShell of de CLI. Wat is er aan de hand?**

**A:** Gegevens die betrekking hebben op het geplande onderhoud is beschikbaar tijdens een geplande onderhoudsgolf alleen voor de virtuele machines die worden beïnvloed door het geplande onderhoud. Als er geen gegevens, de onderhoudsgolf al kan worden voltooid (of niet gestart), of uw virtuele machine is mogelijk al ondergebracht op een bijgewerkt server.

**V: Is er een manier om te weten precies wanneer mijn virtuele machine worden beïnvloed?**

**A:** Als we de planning instelt, definiëren we een tijdvenster van enkele dagen. De exacte volgorde van servers (en virtuele machines) binnen dit venster is onbekend. Als u weten de precieze tijd dat uw virtuele machines wordt bijgewerkt wilt, kunt u [geplande gebeurtenissen](../virtual-machines/windows/scheduled-events.md). Wanneer u geplande gebeurtenissen, kunt u query uitvoert met een van de virtuele machine en een melding van 15 minuten voordat een virtuele machine opnieuw wordt opgestart.

**V: Hoe lang duurt het voordat mijn virtuele machine opnieuw is opgestart?**

**A:**  Afhankelijk van de grootte van uw virtuele machine opnieuw opstarten mogelijk enkele minuten duren tijdens het selfservice-onderhoudsvenster. Het opnieuw opstarten duurt meestal ongeveer 25 minuten tijdens de door Azure geïnitieerde opnieuw opgestart in het geplande onderhoudsvenster. Als u gebruik van Cloudservices (Web-/ Werkrol), virtuele-machineschaalsets of beschikbaarheidssets, krijgt u 30 minuten tussen elke groep van virtuele machines (updatedomein) tijdens het geplande onderhoudsvenster. 

**V: Ik zie niet alle onderhoudsinformatie op mijn virtuele machines. Wat er mis ging?**

**A:** Er zijn diverse redenen waarom u niet alle onderhoudsinformatie op uw virtuele machines ziet mogelijk:
   - U gebruikt een abonnement dat is gemarkeerd als *Microsoft Internal*.
   - Uw VM's worden niet gepland voor onderhoud. Het is mogelijk dat de onderhoudsgolf is beëindigd, is geannuleerd of is gewijzigd, zodat uw virtuele machines niet meer worden beïnvloed door deze.
   - U hoeft niet de **onderhoud** kolom toegevoegd aan uw VM-lijstweergave. Hoewel we deze kolom toegevoegd aan de standaardweergave, als u uw weergave om te zien van niet-standaard kolommen, moet u handmatig toevoegen configureert de **onderhoud** kolom naar uw VM-lijstweergave.

**V: Mijn virtuele machine is gepland voor onderhoud voor de tweede keer. Waarom is dat?**

**A:** Uw virtuele machine is gepland voor onderhoud in bepaalde gevallen, nadat u hebt al uw onderhoud voltooid en opnieuw geïmplementeerd:
   - We hebben de onderhoudsgolf geannuleerd en deze opnieuw gestart met een andere nettolading. Kan het zijn dat we hebben een mislukte nettolading gedetecteerd en moet u een extra nettolading implementeren.
   - Uw virtuele machine werd *uw* naar een ander knooppunt vanwege een hardwarefout.
   - U hebt geselecteerd om te stoppen (toewijzing ongedaan maken) en start de VM opnieuw.
   - U hebt **automatisch afsluiten** ingeschakeld voor de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het registreren voor onderhoudsgebeurtenissen binnen de virtuele machine met behulp van [geplande gebeurtenissen](../virtual-machines/windows/scheduled-events.md).
