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
ms.date: 10/26/2017
ms.author: zivr
ms.openlocfilehash: 80c029866f3d28712be823692f3bf4ce6e210405
ms.sourcegitcommit: adf6a4c89364394931c1d29e4057a50799c90fc0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="handling-planned-maintenance-notifications-for-windows-virtual-machines"></a>Verwerken geplande onderhoud meldingen voor virtuele machines van Windows

Azure voert regelmatig updates ter verbetering van de betrouwbaarheid, prestaties en beveiliging van de infrastructuur van de host voor virtuele machines. Updates zijn wijzigingen, zoals de hostomgeving patchen of bijwerken en buiten gebruik stellen van hardware. Een meerderheid van deze updates worden uitgevoerd zonder nadelige gevolgen voor de gehoste virtuele machines. Er zijn echter gevallen bekend waarbij updates gevolgen:

- Als het onderhoud niet opnieuw worden opgestart hoeft, gebruikt Azure in-place migratie voor het onderbreken van de virtuele machine, terwijl de host is bijgewerkt.

- Als u onderhoud moet worden opgestart, krijgt u een bericht wanneer het onderhoud is gepland. In dergelijke gevallen krijgt u een tijdvenster op waar u het onderhoud uzelf beginnen kunt, wanneer het voor u werkt.


Gepland onderhoud die vereist een herstart is gepland in golven. Elke golf heeft een ander bereik (regio's).

- Een golf begint met een melding aan klanten. Standaard wordt melding verzonden naar de eigenaar van het abonnement en mede-eigenaren. U kunt meer ontvangers en messaging opties zoals e-mail, SMS en Webhooks, toevoegen aan de meldingen.  
- Snel na de melding is een selfservice-venster ingesteld. Tijdens dit venster kunt u vinden die van uw virtuele machines is opgenomen in dit wave en begin onderhoud met proactieve opnieuw distribueren. 
- Na het venster selfservice begint een geplande onderhoudsvenster. Op dit moment worden Azure gepland en vereist onderhoud is van toepassing op uw virtuele machine. 

Het doel voor het hebben van twee windows is zodat u voldoende tijd is voor de onderhoudsmodus te starten en de virtuele machine opstarten achterhoofd wanneer Azure automatisch onderhoud wordt gestart.


U kunt de Azure-portal, PowerShell, REST-API en CLI zoeken naar de onderhoudsvensters voor uw virtuele machines en selfservice onderhoudsmodus te starten.

 > [!NOTE]
 > Als u probeert te beginnen met het onderhoud en mislukken, Azure worden gemarkeerd voor uw virtuele machine als **overgeslagen** en niet tijdens het geplande onderhoudsvenster opstarten. In plaats daarvan contact met u worden opgenomen in een later tijdstip met een nieuwe planning. 


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
| MaintenanceWindowStartTime            | Het begin van het geplande onderhoudsvenster wanneer u de onderhoudsmodus op de virtuele machine starten kunt ||
| MaintenanceWindowEndTime              | Het einde van het geplande onderhoudsvenster wanneer u de onderhoudsmodus op de virtuele machine starten kunt ||
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
        $rg = $rgList[$rgIdx]        $vmList = Get-AzureRMVM -ResourceGroupName $rg.ResourceGroupName 
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

Voor het onderhoud op uw klassieke VM starten, typt u:

```
Restart-AzureVM -InitiateMaintenance -ServiceName <service name> -Name <VM name>
```


## <a name="faq"></a>Veelgestelde vragen


**V: Waarom moet u de virtuele machines nu opnieuw opstarten?**

**A:** het merendeel van updates en upgrades naar de Azure-platform hebben geen invloed op de beschikbaarheid van virtuele machine, er zijn ook gevallen waarbij we kan niet worden vermeden opnieuw opstarten van virtuele machines die worden gehost in Azure. We hebben enkele wijzigingen die we onze servers waardoor de virtuele machines opnieuw opstarten opnieuw opstarten vereist verzameld.

**V: als ik volg de aanbevelingen voor hoge beschikbaarheid met behulp van een Beschikbaarheidsset, kan ik veilig?**

**A:**virtuele machines die worden geïmplementeerd in een beschikbaarheidsset ingesteld of de virtuele-machineschaalsets het begrip van de Update-domeinen (UD). Bij het uitvoeren van onderhoud wordt Azure zich houdt aan de beperking UD en niet opnieuw wordt opgestart van virtuele machines van verschillende UD (binnen dezelfde beschikbaarheidsset).  Azure wacht ook ten minste 30 minuten voordat u doorgaat naar de volgende groep van virtuele machines. 

Raadpleeg de beschikbaarheid van Windows virtuele machines in Azure beheren voor meer informatie over hoge beschikbaarheid of de beschikbaarheid van Linux virtuele machines in Azure beheren.

**V: ik heb herstel na noodgevallen instellen in een andere regio. Kan ik veilig?**

**A:** elke Azure-gebied is gekoppeld aan een andere regio binnen de dezelfde Geografie (zoals Verenigde Staten, Europa of Azië). Geplande Azure-updates worden voor gepaarde regio’s één voor één geïmplementeerd, om downtime en het risico op niet-beschikbare toepassingen te beperken. Tijdens gepland onderhoud plannen Azure een vergelijkbare venster voor gebruikers om te beginnen het onderhoud echter het geplande onderhoudsvenster verschil is tussen de gekoppelde regio's.  

Raadpleeg voor meer informatie over Azure-regio's, regio's en de beschikbaarheid van virtuele machines in Azure.  Hier ziet u de volledige lijst met regionale paren hier.

**V: hoe ik blijf op de hoogte over gepland onderhoud?**

**A:** een golf gepland onderhoud wordt gestart door een planning in te stellen op een of meer Azure-regio's. Kort nadat een e-mailmelding verzonden naar de abonnement-eigenaars (één e-mail per abonnement). Extra kanalen en geadresseerden voor deze melding kunnen worden geconfigureerd met behulp van de activiteit logboek waarschuwingen. Als u een virtuele machine in een regio waar is al gepland onderhoud gepland implementeert, kunt u de melding niet ontvangt maar in plaats daarvan moet de onderhoudsstatus van de virtuele machine niet controleren.

**V: ik een aanwijzing van gepland onderhoud in de portal, Powershell of CLI, wat is het probleem niet zien?**

**A:** informatie met betrekking tot gepland onderhoud is beschikbaar tijdens een gepland onderhoud wave alleen voor de virtuele machines die worden beïnvloed door deze. Met andere woorden, als er geen gegevens, kan het zijn dat de wave onderhoud is al voltooid (of niet is gestart) of dat uw virtuele machine al wordt gehost in een server bijgewerkt.

**V: moet ik het onderhoud op mijn virtuele machine starten?**

**A:** In het algemeen werkbelastingen die zijn geïmplementeerd in een cloudservice, beschikbaarheidsset of scale set van virtuele machines, zijn tegen gepland onderhoud.  Tijdens gepland onderhoud één updatedomein is dit van invloed op een bepaald moment. Let erop dat de volgorde van update-domeinen die worden beïnvloed noodzakelijkerwijs niet opeenvolgend gebeurt.

U wilt onderhoud zelf starten in de volgende gevallen:
- Uw toepassing wordt uitgevoerd op een enkele virtuele machine en u moet alle onderhoud buiten kantooruren toepassen
- U moet de tijd van het onderhoud coördineren als onderdeel van uw SERVICEOVEREENKOMST
- U moet meer dan 30 minuten tussen elke virtuele machine opnieuw opstarten zelfs binnen een beschikbaarheid instellen.
- Wilt u nemen de gehele toepassing (meerdere lagen, meerdere update domeinen) om te kunnen voltooien van het onderhoud sneller af.

**V: is er een manier om te weten precies zien wanneer mijn virtuele machine worden beïnvloed?**

**A:** bij het instellen van de planning, definiëren we een tijdvenster van enkele dagen. Echter, de exacte sequentiëren van servers (en virtuele machines) in dit venster is onbekend. Klanten die willen graag weten de precieze tijd voor hun virtuele machines kunnen u geplande gebeurtenissen en query vanuit de virtuele machine gebruiken en ontvangt een melding 10 minuten voordat een virtuele machine opnieuw wordt opgestart.
  
**V: hoe lang duurt het voordat u Mijn virtuele machine opnieuw opstarten?**

**A:** afhankelijk van de grootte van uw virtuele machine opnieuw opstarten kan enkele minuten duren. Let op: als u cloudservices gebruikt, worden geschaald ingesteld of beschikbaarheidsset, krijgt u 30 minuten tussen elke groep van virtuele machines (UD). 

**V: wat de ervaring in het geval van cloudservices en -schaalsets Service Fabric worden?**

**A:** tijdens gepland onderhoud is van invloed op deze platforms, klanten die gebruikmaken van deze platformen worden beschouwd als veilig gegeven die alleen virtuele machines in een enkel Upgrade Domain (UD) is van invloed op een bepaald moment.  

**V: ik een e-mail over de hardware buiten gebruik stellen hebt ontvangen, is dit hetzelfde als gepland onderhoud?**

**A:** hardware buiten gebruik stellen, is een gebeurtenis gepland onderhoud, hebben we nog niet vrijgegeven deze gebruiksvoorbeeld naar de nieuwe ervaring.  We verwachten dat klanten nogal verward als ze twee vergelijkbare e-mailberichten over twee verschillende gepland onderhoud golven ontvangen.

**V: Ik zie niet alle onderhoudsgegevens op mijn VM's. Wat er mis ging?**

**A:** er zijn diverse redenen waarom u niet, eventuele onderhoudsinformatie op uw virtuele machines ziet:
1.  U gebruikt een abonnement dat is gemarkeerd als Microsoft interne.
2.  Uw VM's zijn niet gepland voor onderhoud. Kan het zijn dat de wave onderhoud is beëindigd, geannuleerd of gewijzigd, zodat uw virtuele machines niet meer worden beïnvloed door deze.
3.  U hebt de kolom "Onderhoud" is toegevoegd aan uw weergave van de lijst met virtuele machine. Terwijl we deze kolom aan de standaardweergave hebt toegevoegd, moeten handmatig klanten die zijn geconfigureerd om te zien van niet-standaard kolommen toevoegen de **onderhoud** kolom naar de lijstweergave van de virtuele machine.

**V: Mijn VM is gepland voor onderhoud voor de tweede keer. Waarom is dat?**

**A:** er zijn enkele gebruiksvoorbeelden waar u uw VM gepland onderhoud ziet nadat u hebt al uw onderhoud-implementatie opnieuw uitgevoerd:
1.  We hebben de wave onderhoud geannuleerd en wordt opnieuw gestart met een andere nettolading. Kan zijn dat we hebben vastgesteld dat de nettolading van de fout en moet u een extra nettolading implementeren.
2.  Uw VM is *service moeten worden hersteld* naar een ander knooppunt veroorzaakt door een hardwarefout
3.  U hebt geselecteerd om te stoppen (ongedaan gemaakt) en start de virtuele machine
4.  U hebt **automatisch afsluiten** ingeschakeld voor de virtuele machine


## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u kunt registreren voor onderhoud gebeurtenissen uit binnen de virtuele machine met behulp van [gepland gebeurtenissen](scheduled-events.md).
