---
title: Afhandeling van meldingen van onderhoud voor Windows-machines in Azure | Microsoft Docs
description: Weergeven meldingen onderhoud voor Windows virtuele machines in Azure wordt uitgevoerd en start selfservice onderhoud.
services: virtual-machines-windows
documentationcenter: 
author: zivraf
manager: timlt
editor: 
tags: azure-service-management,azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 12/15/2017
ms.author: zivr
ms.openlocfilehash: d6d8507508ef1946c1dfa41c47ae81f51c0ad4ef
ms.sourcegitcommit: 8fc9b78a2a3625de2cecca0189d6ee6c4d598be3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/29/2017
---
# <a name="handling-planned-maintenance-notifications-for-windows-virtual-machines"></a>Verwerken geplande onderhoud meldingen voor virtuele machines van Windows

Azure voert regelmatig updates ter verbetering van de betrouwbaarheid, prestaties en beveiliging van de infrastructuur van de host voor virtuele machines. Updates zijn wijzigingen, zoals de hostomgeving patchen of bijwerken en buiten gebruik stellen van hardware. Een meerderheid van deze updates worden uitgevoerd zonder nadelige gevolgen voor de gehoste virtuele machines. Er zijn echter gevallen bekend waarbij updates gevolgen:

- Als het onderhoud niet opnieuw worden opgestart hoeft, gebruikt Azure in-place migratie voor het onderbreken van de virtuele machine, terwijl de host is bijgewerkt.

- Als u onderhoud moet worden opgestart, krijgt u een bericht wanneer het onderhoud is gepland. In dergelijke gevallen krijgt u een tijdvenster op waar u het onderhoud uzelf beginnen kunt, wanneer het voor u werkt.


Gepland onderhoud die vereist een herstart is gepland in golven. Elke golf heeft een ander bereik (regio's).

- Een golf begint met een melding aan klanten. Standaard wordt melding verzonden naar de eigenaar van het abonnement en mede-eigenaren. U kunt meer ontvangers en messaging opties zoals e-mail, SMS en Webhooks, toevoegen aan de meldingen met Azure [activiteit logboek waarschuwingen](../../monitoring-and-diagnostics/monitoring-overview-activity-logs.md).  
- Op het moment van de melding een *venster selfservice* beschikbaar wordt gesteld. Tijdens dit venster kunt u zoeken die van uw virtuele machines zijn opgenomen in deze wave en proactief onderhoud starten volgens de behoeften van uw eigen planning.
- Na het venster selfservice een *geplande onderhoudsvenster* begint. Op een bepaald moment tijdens dit venster Azure gepland en vereist onderhoud is van toepassing op uw virtuele machine. 

Het doel voor het hebben van twee windows is zodat u voldoende tijd is voor de onderhoudsmodus te starten en de virtuele machine opstarten achterhoofd wanneer Azure automatisch onderhoud wordt gestart.


U kunt de Azure-portal, PowerShell, REST-API en CLI zoeken naar de onderhoudsvensters voor uw virtuele machines en selfservice onderhoudsmodus te starten.

 > [!NOTE]
 > Als u probeert te starten, onderhoud en de aanvraag is mislukt, Azure uw virtuele machine als markeert **overgeslagen**. Niet meer mogelijk gebruik van de optie klant geïnitieerd onderhoud. Uw virtuele machine moet opnieuw worden opgestart door Azure tijdens de fase gepland onderhoud.


 
## <a name="should-you-start-maintenance-using-during-the-self-service-window"></a>Moet u beginnen met behulp van onderhoud tijdens het venster selfservice?  

De volgende richtlijnen moeten u helpen te beslissen of u moet deze functie gebruiken en onderhoud om uw eigen tijd gestart.

> [!NOTE] 
> Selfservice onderhoud is mogelijk niet beschikbaar voor alle uw virtuele machines. Om te bepalen of proactieve opnieuw distribueren voor uw virtuele machine beschikbaar is, zoekt u naar de **nu starten** in de onderhoudsstatus. Selfservice onderhoud is momenteel niet beschikbaar voor Cloudservices (Web/Worker-rol), Service Fabric en virtuele-Machineschaalsets.


Selfservice onderhoud wordt niet aanbevolen voor implementaties met **beschikbaarheidssets** aangezien deze maximaal beschikbare instellingen zijn waar slechts één updatedomein is van invloed op een bepaald moment. 
    - Laat het onderhoud van Azure trigger, maar houd er rekening mee dat de volgorde van update-domeinen die worden beïnvloed noodzakelijkerwijs niet opeenvolgend gebeurt en of er een pauze van 30 minuten tussen domeinen van de update.
    - Als een tijdelijke verlies van een deel van de capaciteit van de (aantal 1-update) een probleem is, het kan eenvoudig worden gecompenseerd door toevoeging exemplaren toewijzen tijdens de onderhoudsperiode **niet** onderhoud selfservice gebruiken in de volgende scenario's: 
    - Als u uw VM's vaak afsluit hetzij handmatig, DevTest labs, met behulp van automatisch afsluiten of een planning te volgen met deze kan herstel de onderhoudsstatus en daarom leiden tot extra uitvaltijd.
    - Op de tijdelijke virtuele machines waarvan u weet dat vóór het einde van het onderhoud wave worden verwijderd. 
    - Voor werkbelastingen met een grote status opgeslagen in de lokale (tijdelijke) schijf dat nodig is om te worden gehouden tijdens het bijwerken. 
    - Voor gevallen waarin u de grootte van uw virtuele machine vaak omdat dit kan het onderhoudsstatus herstellen. 
    - Als u geplande gebeurtenissen waarmee proactieve failover of correct afsluiten van uw werkbelasting 15 minuten voor het begin van het onderhoud afsluiten van hebben vastgesteld

**Gebruik** selfservice-onderhoud, als u van plan bent om uit te voeren van uw virtuele machine onderbroken tijdens de fase gepland onderhoud en geen van de tegenpartij vermeldingen bovengenoemde van toepassing zijn. 

Het is raadzaam te onderhoud selfservice gebruiken in de volgende gevallen:
    - U moet een exacte onderhoudsvenster om uw beheer of de einde-klant communiceren. 
    - U moet het onderhoud voltooien door een bepaalde datum. 
    - U moet de volgorde van onderhoud, bijvoorbeeld toepassing met meerdere lagen te garanderen veilig kunnen worden hersteld.
    - U moet meer dan 30 minuten aan de hersteltijd VM tussen twee update domeinen (UDs). Voor het beheren van de tijd tussen de domeinen van de update, moet u onderhoud op uw virtuele machines één updatedomein (UD) activeren op een tijdstip.

 

[!INCLUDE [virtual-machines-common-maintenance-notifications](../../../includes/virtual-machines-common-maintenance-notifications.md)]

## <a name="check-maintenance-status-using-powershell"></a>Controleer de onderhoudsstatus van met behulp van PowerShell

U kunt ook Azure Powershell gebruiken om te zien wanneer virtuele machines zijn gepland voor onderhoud. Gepland onderhoudsinformatie is beschikbaar via de [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) cmdlet wanneer u de `-status` parameter.
 
Alleen als er onderhoud gepland onderhoudsinformatie geretourneerd. Als er dat geen onderhoud gepland dat invloed op de virtuele machine, wordt de cmdlet geen onderhoudsgegevens als resultaat. 

```powershell
Get-AzureRmVM -ResourceGroupName rgName -Name vmName -Status
```

De volgende eigenschappen worden onder MaintenanceRedeployStatus geretourneerd: 
| Waarde | Beschrijving   |
|-------|---------------|
| IsCustomerInitiatedMaintenanceAllowed | Hiermee wordt aangegeven of u onderhoudsmodus op de virtuele machine op dit moment starten kunt ||
| PreMaintenanceWindowStartTime         | Het begin van het selfservice onderhoudsvenster wanneer u de onderhoudsmodus op de virtuele machine starten kunt ||
| PreMaintenanceWindowEndTime           | Het einde van het selfservice onderhoudsvenster wanneer u de onderhoudsmodus op de virtuele machine starten kunt ||
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoud waarin Azure onderhoud op de virtuele machine initieert ||
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster waarin Azure onderhoud op de virtuele machine initieert ||
| LastOperationResultCode               | Het resultaat van de laatste poging tot het onderhoud op de virtuele machine starten ||



U kunt ook de onderhoudsstatus ophalen voor alle VM's in een resourcegroep met behulp van [Get-AzureRmVM](/powershell/module/azurerm.compute/get-azurermvm) en een virtuele machine niet op te geven.
 
```powershell
Get-AzureRmVM -ResourceGroupName rgName -Status
```

De volgende PowerShell-functie duurt uw abonnements-ID en een lijst met virtuele machines die zijn ingepland voor onderhoud wordt afgedrukt.

```powershell

function MaintenanceIterator
{
    Select-AzureRmSubscription -SubscriptionId $args[0]

    $rgList= Get-AzureRmResourceGroup 

    for ($rgIdx=0; $rgIdx -lt $rgList.Length ; $rgIdx++)
    {
        $rg = $rgList[$rgIdx]        
    $vmList = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName 
        for ($vmIdx=0; $vmIdx -lt $vmList.Length ; $vmIdx++)
        {
            $vm = $vmList[$vmIdx]
            $vmDetails = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName -Name $vm.Name -Status
              if ($vmDetails.MaintenanceRedeployStatus )
            {
                Write-Output "VM: $($vmDetails.Name)  IsCustomerInitiatedMaintenanceAllowed: $($vmDetails.MaintenanceRedeployStatus.IsCustomerInitiatedMaintenanceAllowed) $($vmDetails.MaintenanceRedeployStatus.LastOperationMessage)"               
            }
          }
    }
}

```

### <a name="start-maintenance-on-your-vm-using-powershell"></a>Onderhoudsmodus starten op de virtuele machine met behulp van PowerShell

Met behulp van informatie van de functie in de vorige sectie, het volgende begint onderhoud op een virtuele machine als **IsCustomerInitiatedMaintenanceAllowed** is ingesteld op true.

```powershell
Restart-AzureRmVM -PerformMaintenance -name $vm.Name -ResourceGroupName $rg.ResourceGroupName 
```

## <a name="classic-deployments"></a>Klassieke implementaties

Als er nog steeds verouderde virtuele machines die zijn kunt geïmplementeerd met behulp van het klassieke implementatiemodel u PowerShell gebruiken voor de query voor virtuele machines en onderhoud initiëren.

Als u de onderhoudsstatus van een virtuele machine, typt u:

```
Get-AzureVM -ServiceName <Service name> -Name <VM name>
```

Voor het onderhoud op de klassieke virtuele machine starten, typt u:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```


## <a name="faq"></a>Veelgestelde vragen


**V: Waarom moet u de virtuele machines nu opnieuw opstarten?**

**A:** het merendeel van updates en upgrades naar de Azure-platform hebben geen invloed op de beschikbaarheid van virtuele machine, er zijn ook gevallen waarbij we kan niet worden vermeden opnieuw opstarten van virtuele machines die worden gehost in Azure. We hebben enkele wijzigingen die we onze servers waardoor de virtuele machines opnieuw opstarten opnieuw opstarten vereist verzameld.

**V: als ik volg de aanbevelingen voor hoge beschikbaarheid met behulp van een Beschikbaarheidsset, kan ik veilig?**

**A:** virtuele machines die worden geïmplementeerd in een beschikbaarheidsset ingesteld of de virtuele-machineschaalsets het begrip van de Update-domeinen (UD). Bij het uitvoeren van onderhoud wordt Azure zich houdt aan de beperking UD en niet opnieuw wordt opgestart van virtuele machines van verschillende UD (binnen dezelfde beschikbaarheidsset).  Azure wacht ook ten minste 30 minuten voordat u doorgaat naar de volgende groep van virtuele machines. 

Zie voor meer informatie over hoge beschikbaarheid, [regio's en de beschikbaarheid van virtuele machines in Azure](regions-and-availability.MD).

**V: hoe ik blijf op de hoogte over gepland onderhoud?**

**A:** een golf gepland onderhoud wordt gestart door een planning in te stellen op een of meer Azure-regio's. Kort nadat een e-mailmelding verzonden naar de abonnement-eigenaars (één e-mail per abonnement). Extra kanalen en geadresseerden voor deze melding kunnen worden geconfigureerd met behulp van de activiteit logboek waarschuwingen. Als u een virtuele machine in een regio waar is al gepland onderhoud gepland implementeert, kunt u de melding niet ontvangt maar in plaats daarvan moet de onderhoudsstatus van de virtuele machine niet controleren.

**V: ik een aanwijzing van gepland onderhoud in de portal, Powershell of CLI, wat is het probleem niet zien?**

**A:** informatie met betrekking tot gepland onderhoud is beschikbaar tijdens een gepland onderhoud wave alleen voor de virtuele machines die worden beïnvloed door deze. Met andere woorden, als er geen gegevens, kan het zijn dat de wave onderhoud is al voltooid (of niet is gestart) of dat uw virtuele machine al wordt gehost in een server bijgewerkt.

**V: is er een manier om te weten precies zien wanneer mijn virtuele machine worden beïnvloed?**

**A:** bij het instellen van de planning, definiëren we een tijdvenster van enkele dagen. Echter, de exacte sequentiëren van servers (en virtuele machines) in dit venster is onbekend. Klanten die willen graag weten de precieze tijd voor hun virtuele machines kunt [gepland gebeurtenissen](scheduled-events.md) en query uit vanuit de virtuele machine en ontvangt een melding 15 minuten voordat u een VM opnieuw wordt opgestart.

**V: hoe lang duurt het voordat u Mijn virtuele machine opnieuw opstarten?**

**A:** afhankelijk van de grootte van uw virtuele machine opnieuw opstarten kan enkele minuten duren tijdens het Self-service onderhoudsvenster. Tijdens de Azure gestart opnieuw wordt opgestart in het venster voor geplande onderhoud, de opnieuw opstarten wordt typicall nemen ongeveer 25 minuten. Houd er rekening mee dat als u Cloud-Services (Web/Worker-rol), virtuele Machine Scale ingesteld of beschikbaarheidssets gebruikt, u 30 minuten tussen elke groep van virtuele machines (UD) tijdens het geplande onderhoudsvenster krijgt. 

**V: Wat is de ervaring in het geval van Cloud-Services (Web/Worker-rol), Service Fabric en virtuele-Machineschaalsets?**

**A:** tijdens gepland onderhoud is van invloed op deze platforms, klanten die gebruikmaken van deze platformen worden beschouwd als veilig gegeven die alleen virtuele machines in een enkel Upgrade Domain (UD) is van invloed op een bepaald moment. Selfservice onderhoud is momenteel niet beschikbaar voor Cloudservices (Web/Worker-rol), Service Fabric en virtuele-Machineschaalsets.

**V: ik een e-mail over de hardware buiten gebruik stellen hebt ontvangen, is dit hetzelfde als gepland onderhoud?**

**A:** hardware buiten gebruik stellen, is een gebeurtenis gepland onderhoud, hebben we nog niet vrijgegeven deze gebruiksvoorbeeld naar de nieuwe ervaring.  

**V: Ik zie niet alle onderhoudsgegevens op mijn VM's. Wat er mis ging?**

**A:** er zijn diverse redenen waarom u niet, eventuele onderhoudsinformatie op uw virtuele machines ziet:
1.  U gebruikt een abonnement dat is gemarkeerd als Microsoft interne.
2.  Uw VM's zijn niet gepland voor onderhoud. Kan het zijn dat de wave onderhoud is beëindigd, geannuleerd of gewijzigd, zodat uw virtuele machines niet meer worden beïnvloed door deze.
3.  U hoeft niet de **onderhoud** kolom die wordt toegevoegd aan uw VM-lijstweergave. Terwijl we deze kolom aan de standaardweergave hebt toegevoegd, moeten handmatig klanten die zijn geconfigureerd om te zien van niet-standaard kolommen toevoegen de **onderhoud** kolom naar de lijstweergave van de virtuele machine.

**V: Mijn VM is gepland voor onderhoud voor de tweede keer. Waarom is dat?**

**A:** er zijn enkele gebruiksvoorbeelden waar u uw VM gepland onderhoud ziet nadat u hebt al uw onderhoud-implementatie opnieuw uitgevoerd:
1.  We hebben de wave onderhoud geannuleerd en wordt opnieuw gestart met een andere nettolading. Kan zijn dat we hebben vastgesteld dat de nettolading van de fout en moet u een extra nettolading implementeren.
2.  Uw VM is *service moeten worden hersteld* naar een ander knooppunt veroorzaakt door een hardwarefout
3.  U hebt geselecteerd om te stoppen (ongedaan gemaakt) en start de virtuele machine
4.  U hebt **automatisch afsluiten** ingeschakeld voor de virtuele machine


**V: onderhoud van mijn beschikbaarheidsset kan lang duren en er nu 'overgeslagen' status voor een aantal van mijn beschikbaarheid exemplaren instellen. Waarom is dat?** 

**A:** op voor het bijwerken van meerdere exemplaren in een beschikbaarheidsset korte achter elkaar hebt geklikt, Azure deze aanvragen in wachtrij wordt geplaatst en wordt alleen de virtuele machines in één updatedomein (UD) tegelijk bijwerken is gestart. Omdat het is mogelijk een pauze tussen domeinen van de update, kan echter de update langer duurt weergegeven. Als de wachtrij update langer dan 60 minuten duurt duurt, wordt weergegeven in sommige gevallen de **overgeslagen** status, zelfs als ze zijn bijgewerkt. Om te voorkomen dat deze onjuiste status, update uw beschikbaarheidssets door te klikken op exemplaar binnen één beschikbaarheid alleen ingesteld en wacht u totdat de update op die virtuele machine te voltooien voordat u op de volgende virtuele machine in een andere update-domein.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u kunt registreren voor onderhoud gebeurtenissen uit binnen de virtuele machine met behulp van [gepland gebeurtenissen](scheduled-events.md).
