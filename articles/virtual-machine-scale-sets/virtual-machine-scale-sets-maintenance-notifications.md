---
title: Afhandeling van onderhoudsmeldingen voor virtuele-Machineschaalsets in Azure | Microsoft Docs
description: Weergeven onderhoudsmeldingen voor virtuele-Machineschaalsets in Azure en selfservice-onderhoud starten.
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
ms.openlocfilehash: 4ce984686c2bb320d5d32771d31b81cdec1153af
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38314404"
---
# <a name="handling-planned-maintenance-notifications-for-virtual-machine-scale-sets"></a>Meldingen gepland onderhoud verwerken voor virtuele-Machineschaalsets

Van tijd tot tijd voert Azure updates uit om de betrouwbaarheid, prestaties en veiligheid te verbeteren van de host-infrastructuur voor virtuele machines. Updates zijn wijzigingen, zoals de hostomgeving patches of bijwerken en buiten gebruik stellen van hardware. Een meerderheid van deze updates worden uitgevoerd zonder dat dit van invloed is op de gehoste virtuele machines. Er zijn echter gevallen waar updates indruk hebben:

- Als het onderhoud niet opnieuw worden opgestart vereist, gebruikt Azure in-place migratie voor de virtuele machine wordt onderbroken terwijl de host is bijgewerkt. Deze onderhoudsbewerkingen niet rebootful zijn toegepast foutdomein door foutdomein en voortgang is gestopt als er health waarschuwingssignalen worden ontvangen.

- Als u onderhoud moet worden opgestart, krijgt u een kennisgeving van wanneer het onderhoud is gepland. In dergelijke gevallen krijgt u een bepaalde periode waar u het onderhoud zelf beginnen kunt, wanneer het voor u werkt.


Gepland onderhoud is gepland in vaste blokken die moet worden opgestart. Elk blok heeft een ander bereik (regio's).

- Een golf begint met een melding aan klanten. Standaard wordt melding verzonden naar de eigenaar van het abonnement en mede-eigenaren. U kunt meer ontvangers en opties zoals e-mail, SMS en Webhooks, toevoegen aan de meldingen met behulp van Azure [waarschuwingen voor activiteitenlogboeken](../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- Op het moment van de melding een *selfservice venster* beschikbaar wordt gesteld. Tijdens dit venster kunt u zoeken die van uw virtuele machines zijn opgenomen in deze wave en proactief onderhoud starten op basis van de behoeften van uw eigen planning.
- Nadat het venster zelf een *geplande onderhoudsvenster* begint. Op een bepaald moment tijdens dit venster Azure plant en het vereiste onderhoud is van toepassing op uw virtuele machine. 

Het doel voor het hebben van twee windows is dat u voldoende tijd is voor onderhoud starten en opnieuw opstarten van uw virtuele machine en als Azure automatisch onderhoud wordt gestart.


Kunt u de Azure portal, PowerShell, REST-API en CLI om op te vragen de onderhoudsvensters voor de VM-schaalset VM's en selfservice-onderhoud starten.

  
## <a name="should-you-start-maintenance-during-the-self-service-window"></a>Moet u onderhoud starten tijdens het venster selfservice?  

De volgende richtlijnen kunt u beslissen of u deze mogelijkheid gebruik en onderhoud starten op uw eigen tijd.

> [!NOTE] 
> Selfservice-onderhoud is mogelijk niet beschikbaar voor al uw VM's. Om te bepalen als proactieve opnieuw implementeren beschikbaar voor uw virtuele machine is, zoek de **nu starten** in de onderhoudsstatus weergegeven. Selfservice-onderhoud is momenteel niet beschikbaar voor Cloudservices (Web-/ Werkrol) en Service Fabric.


Selfservice-onderhoud wordt niet aanbevolen voor implementaties met **beschikbaarheidssets** omdat deze maximaal beschikbare installaties waarbij slechts één updatedomein is van invloed op een bepaald moment. 

- Laat Azure activeren van het onderhoud. Voor het onderhoud waarvoor opnieuw opstarten, er rekening mee dat het onderhoud updatedomein per updatedomein dat de update-domeinen niet per se het onderhoud sequentieel worden verwerkt wordt, wordt uitgevoerd en of er een pauze van 30 minuten tussen updatedomeinen.
- Als een tijdelijk verlies van een aantal van de capaciteit van de (aantal 1/bijwerken) een probleem is, kan het eenvoudig worden gecompenseerd door het toewijzen van extra exemplaren tijdens de onderhoudsperiode.
- Voor het onderhoud van besturingssysteemsoftware die zonder opnieuw opstarten, updates worden toegepast op het niveau van de fout-domein. 
    
**Geen** selfservice-onderhoud gebruiken in de volgende scenario's: 

- Als u uw VM's vaak, hetzij handmatig, met DevTest Labs, met behulp van automatisch afsluiten of volgens een planning, het kan de onderhoudsstatus zijn en dus meer downtime veroorzaken.
- Op de tijdelijke virtuele machines dat u weet verwijderd vóór het einde van de onderhoudsgolf. 
- Voor workloads met een grote staat die zijn opgeslagen in de lokale (tijdelijke) schijf dat nodig is om te worden onderhouden tijdens het bijwerken. 
- Voor gevallen waarin u de grootte van uw virtuele machine vaak, zoals deze kan herstellen de onderhoudsstatus weergegeven. 
- Als u geplande gebeurtenissen waarmee proactieve failover of het correct afsluiten van uw werkbelasting, 15 minuten voor het begin van het onderhoud afsluiten van zijn vastgesteld.

**Gebruik** selfservice-onderhoud, als u van plan bent om uit te voeren van uw virtuele machine ononderbroken tijdens de fase gepland onderhoud en geen van de tegenpartij vermeldingen hierboven genoemde van toepassing zijn. 

Het is raadzaam te gebruiken van selfservice-onderhoud in de volgende gevallen:

- U moet een exacte onderhoudsvenster om uw beheer of de eindklant communiceren. 
- U moet de onderhoud voltooien door een bepaalde datum. 
- U moet voor het beheren van de volgorde van onderhoud, bijvoorbeeld, toepassing met meerdere lagen te garanderen veilig kunnen worden hersteld.
- U moet meer dan 30 minuten van de hersteltijd virtuele machine tussen twee updatedomeinen (ud's). Voor het beheren van de tijd tussen het update-domeinen, moet u onderhoud activeren op uw VM's één updatedomein (UD) tegelijkertijd.

 
## <a name="view-virtual-machine-scale-sets-impacted-by-maintenance-in-the-portal"></a>Virtuele-machineschaalsets beïnvloed door onderhoud in de portal weergeven

Wanneer een geplande onderhoudsgolf is gepland, kunt u de lijst met schaalsets voor virtuele machines die worden beïnvloed door de geplande onderhoudsgolf met behulp van de Azure-portal weergeven. 

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer **alle services** in de Linkernavigatie en kies **virtuele-machineschaalsets**.
3. Op de **virtuele-machineschaalsets** pagina, kies de **kolommen bewerken** optie aan de bovenkant om de lijst van beschikbare kolommen te openen.
4. In de **beschikbare kolommen sectie**, selecteer de **selfservice-onderhoud** item en gebruik de pijltjesknoppen om te verplaatsen naar de **kolommen geselecteerd** lijst. Kunt u de vervolgkeuzelijst in schakelen de **beschikbare kolommen** rubriek van **alle** naar **eigenschappen** waarmee de **selfservice-onderhoud** item gemakkelijker te vinden. Zodra u hebt de **selfservice-onderhoud** item in de **kolommen geselecteerd** sectie, selecteer de **toepassen** knop aan de onderkant van de pagina. 

Nadat u de bovenstaande stappen het **selfservice-onderhoud** kolom wordt weergegeven in de lijst met virtuele-machineschaalsets. Elke virtuele-machineschaalset kan een van de volgende waarden voor de selfservice-onderhoud-kolom hebben:

| Waarde | Beschrijving |
|-------|-------------|
| Ja | Er is ten minste één virtuele machine in uw virtuele-machineschaalset in een venster selfservice. U kunt onderhoud starten op elk gewenst moment tijdens dit venster selfservice. | 
| Nee | Er zijn geen virtuele machines die in een selfservice-venster in de betrokken virtuele-machineschaalset. | 
| - | Uw schaalsets van virtuele machines maken geen deel uit van een geplande onderhoudsgolf.| 

## <a name="notification-and-alerts-in-the-portal"></a>Meldingen en waarschuwingen in de portal

Een schema voor gepland onderhoud communiceert Azure met een e-mailbericht verzenden naar de abonnement-eigenaar en mede-eigenaars-groep. U kunt extra ontvangers en kanalen toevoegen aan deze communicatie met het maken van Azure-activiteit waarschuwingen. Zie voor meer informatie [abonnementsactiviteiten controleren met de Azure-activiteitenlogboek] (.. / monitoring-and-diagnostics/monitoring-overview-activity-logs.md)

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu aan de linkerkant, **Monitor**. 
3. In de **Monitor - waarschuwingen (klassiek)** deelvenster, klikt u op **+ waarschuwing voor activiteitenlogboek toevoegen**.
4. Vul de gegevens in de **waarschuwing voor activiteitenlogboek toevoegen** pagina en zorg ervoor dat u het volgende instellen in de **Criteria**:
   - **Gebeurteniscategorie**: servicestatus
   - **Services**: Virtual Machine Scale Sets en virtuele Machines
   - **Type**: gepland onderhoud 
    
Zie voor meer informatie over het configureren van waarschuwingen voor activiteitenlogboeken, [waarschuwingen voor activiteitenlogboek maken](../monitoring-and-diagnostics/monitoring-activity-log-alerts.md)
    
    
## <a name="start-maintenance-on-your-virtual-machine-scale-set-from-the-portal"></a>Onderhoud starten op de virtuele-machineschaalset vanuit de portal

Tijdens het kijken naar het overzicht van schaalsets voor virtuele machines, kunt u zich om te zien van meer onderhoud gerelateerde details. Als ten minste één virtuele machine in de virtuele-machineschaalset is opgenomen in de geplande onderhoudsgolf, wordt een nieuwe melding-lint toegevoegd aan de bovenkant van de pagina. U kunt klikken op het lint melding naar de **onderhoud** pagina om te zien welk exemplaar van de virtuele machine wordt beïnvloed door het geplande onderhoud. 

Van daaruit kunt u zich aan het onderhoud starten door het controleren van het vak overeenkomt met de betreffende virtuele machine en vervolgens te klikken op **onderhoud starten** optie.

Als u onderhoud gestart, wordt de betrokken virtuele machines in uw virtuele-machineschaalset onderhoud ondergaan en zijn tijdelijk niet beschikbaar. Als u het venster selfservice hebt gemist, kunt u zich nog steeds het venster zien wanneer uw virtuele-machineschaalset wordt onderhouden door Azure.
 
## <a name="check-maintenance-status-using-powershell"></a>Onderhoudsstatus controleren met PowerShell

U kunt Azure Powershell gebruiken om te zien wanneer virtuele machines in uw virtuele machine-schaalsets zijn gepland voor onderhoud. Gepland onderhoudsinformatie is beschikbaar in de [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss) cmdlet als u de `-InstanceView` parameter.
 
Alleen als er onderhoud gepland is onderhoudsgegevens geretourneerd. Als geen onderhoud is gepland die van invloed is op het VM-exemplaar, retourneert de cmdlet geen onderhoudsinformatie. 

```powershell
Get-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -InstanceView
```

De volgende eigenschappen worden geretourneerd bij MaintenanceRedeployStatus: 
| Waarde | Beschrijving   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Geeft aan of u onderhoud op de virtuele machine op dit moment starten kunt ||
| PreMaintenanceWindowStartTime         | Het begin van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt ||
| PreMaintenanceWindowEndTime           | Het einde van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt ||
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoud waarin Azure onderhoud op de virtuele machine start ||
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine start ||
| LastOperationResultCode               | Het resultaat van de laatste poging tot het starten van onderhoud op de virtuele machine ||



### <a name="start-maintenance-on-your-vm-instance-using-powershell"></a>Onderhoud starten op uw VM-exemplaar met behulp van PowerShell

U kunt onderhoud starten op een virtuele machine als **IsCustomerInitiatedMaintenanceAllowed** is ingesteld op true via [Set-AzureRmVmss](/powershell/module/azurerm.compute/set-azurermvmss) cmdlet met `-PerformMaintenance` parameter.

```powershell
Set-AzureRmVmss -ResourceGroupName rgName -VMScaleSetName vmssName -InstanceId id -PerformMaintenance 
```

## <a name="check-maintenance-status-using-cli"></a>Onderhoudsstatus controleren met CLI

Gepland onderhoudsinformatie kunt weergeven aan de hand [az vmss list-instances](/cli/azure/vmss?view=azure-cli-latest#az-vmss-list-instances).
 
Alleen als er onderhoud gepland is onderhoudsgegevens geretourneerd. Als er is geen onderhoud gepland die van invloed is op het VM-exemplaar, de opdracht heeft geen onderhoudsgegevens als resultaat. 

```azure-cli
az vmss list-instances -g rgName -n vmssName --expand instanceView
```

De volgende eigenschappen zijn onder MaintenanceRedeployStatus geretourneerd voor elk VM-exemplaar: 
| Waarde | Beschrijving   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Geeft aan of u onderhoud op de virtuele machine op dit moment starten kunt ||
| PreMaintenanceWindowStartTime         | Het begin van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt ||
| PreMaintenanceWindowEndTime           | Het einde van het selfservice onderhoudsvenster wanneer u onderhoud op de virtuele machine starten kunt ||
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoud waarin Azure onderhoud op de virtuele machine start ||
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine start ||
| LastOperationResultCode               | Het resultaat van de laatste poging tot het starten van onderhoud op de virtuele machine ||


### <a name="start-maintenance-on-your-vm-instance-using-cli"></a>Onderhoud starten op uw VM-exemplaar met behulp van CLI

De volgende oproep verzenden onderhoud op een VM-exemplaar worden gestart als `IsCustomerInitiatedMaintenanceAllowed` is ingesteld op true.

```azure-cli
az vmss perform-maintenance -g rgName -n vmssName --instance-ids id
```

## <a name="faq"></a>Veelgestelde vragen


**V: Waarom moet u nu opnieuw opstarten van mijn virtuele machines?**

**A:** het merendeel van de updates en upgrades van het Azure-platform, zijn niet van invloed op de beschikbaarheid van de virtuele machine, maar er zijn gevallen waar we kan niet worden vermeden opnieuw opstarten van virtuele machines die worden gehost in Azure. We hebben diverse wijzigingen waarvoor hostupdate met herstarten van onze servers die in virtuele machines opnieuw opstarten resulteren bij elkaar opgeteld.

**Vraag: als ik volg de aanbevelingen voor hoge beschikbaarheid met behulp van een Beschikbaarheidsset, ben ik veilig?**

**A:** virtuele machines die zijn geïmplementeerd in een beschikbaarheidsset of virtuele-machineschaalsets hebben het begrip van de Update-domeinen (UD). Wanneer u onderhoud uitvoert, wordt Azure zich houdt aan de UD-beperkingen en wordt niet opnieuw op virtuele machines van verschillende UD (binnen dezelfde beschikbaarheidsset).  Azure is ook moet wachten gedurende ten minste 30 minuten voordat u naar de volgende groep van virtuele machines. 

Zie voor meer informatie over hoge beschikbaarheid, [regio's en beschikbaarheid voor virtuele machines in Azure](../virtual-machines/windows/regions-and-availability.md).

**V: hoe ik te worden geïnformeerd over gepland onderhoud?**

**A:** een geplande onderhoudsgolf wordt gestart door een planning in te stellen op een of meer Azure-regio's. Direct nadat een e-mailmelding verzonden naar de eigenaars van abonnementen (één e-mailadres per abonnement). Extra kanalen en ontvangers voor deze melding kunnen worden geconfigureerd met behulp van waarschuwingen voor activiteitenlogboeken. Als u een virtuele machine naar een regio waar al gepland onderhoud is gepland implementeren, kunt u de melding niet ontvangt maar in plaats daarvan moet de onderhoudsstatus van de virtuele machine controleren.

**V: Ik zie geen melding over gepland onderhoud in de portal, Powershell of CLI. Wat is er aan de hand?**

**A:** informatie met betrekking tot gepland onderhoud tijdens een geplande onderhoudsgolf alleen voor de virtuele machines die worden beïnvloed door deze beschikbaar is. Met andere woorden, als u geen gegevens ziet, kan het zijn dat de onderhoudsgolf is al voltooid (of niet gestart) of dat uw virtuele machine al wordt gehost op een bijgewerkt server.

**Vraag: is er een manier om te weten precies zien wanneer mijn virtuele machine worden beïnvloed?**

**A:** bij het instellen van de planning, definiëren we een tijdvenster van enkele dagen. Echter, de exacte volgorde van servers (en virtuele machines) binnen dit venster is onbekend. Klanten die graag weten de precieze tijd voor hun VM's kunnen gebruiken [geplande gebeurtenissen](../virtual-machines/windows/scheduled-events.md) query uitvoert met een van de virtuele machine en een melding van 15 minuten voordat een virtuele machine opnieuw wordt opgestart.

**V: hoe lang duurt het voordat mijn virtuele machine opnieuw is opgestart?**

**A:** afhankelijk van de grootte van uw virtuele machine opnieuw opstarten kan duren enkele minuten tijdens het selfservice-onderhoudsvenster. Tijdens de door Azure geïnitieerde opnieuw opstarten in het geplande onderhoudsvenster, het opnieuw opstarten duurt doorgaans ongeveer 25 minuten. Houd er rekening mee dat in het geval u Cloudservices (Web-/ Werkrol), Virtual Machine Scale Sets of beschikbaarheidssets gebruikt, u 30 minuten tussen elke groep van virtuele machines (UD) tijdens het geplande onderhoudsvenster krijgt. 

**V: Ik zie niet alle onderhoudsinformatie op mijn virtuele machines. Wat er mis ging?**

**A:** er zijn diverse redenen waarom u niet alle onderhoudsgegevens op uw virtuele machines ervaart nog:
   - U kunt een abonnement dat is gemarkeerd als Microsoft interne worden gebruikt.
   - Uw VM's zijn niet gepland voor onderhoud. Kan het zijn dat de onderhoudsgolf is beëindigd, geannuleerd of gewijzigd, zodat uw virtuele machines niet meer worden beïnvloed door deze.
   - U hoeft niet de **onderhoud** kolom toegevoegd aan uw VM-lijstweergave. Hoewel we deze kolom aan de standaardweergave hebt toegevoegd, klanten die geconfigureerd om te zien van niet-standaard kolommen handmatig moeten toevoegen de **onderhoud** kolom aan de VM-lijstweergave.

**V: Mijn VM is gepland voor onderhoud voor de tweede keer. Waarom is dat?**

**A:** er zijn verschillende gebruiksvoorbeelden hier u de virtuele machine waarvoor onderhoud is gepland vindt nadat u al uw onderhoud-opnieuw implementeren hebt:
   - We hebben de onderhoudsgolf geannuleerd en deze opnieuw gestart met een andere nettolading. Het kan zijn dat we hebben de nettolading van de mislukte gedetecteerd en moet u een extra nettolading implementeren.
   - Uw virtuele machine werd *uw* naar een ander knooppunt vanwege een hardwarefout.
   - U hebt geselecteerd om te stoppen (toewijzing ongedaan maken) en start de VM opnieuw.
   - U hebt **automatisch afsluiten** ingeschakeld voor de virtuele machine.

## <a name="next-steps"></a>Volgende stappen

Leer hoe u zich kunt registreren voor onderhoudsgebeurtenissen binnen de virtuele machine met behulp [geplande gebeurtenissen](../virtual-machines/windows/scheduled-events.md).
