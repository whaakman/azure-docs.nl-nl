---
title: Overzicht van virtuele-machineschaalsets in Azure | Microsoft Docs
description: Meer informatie over virtuele-machineschaalsets in Azure
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/10/2017
ms.author: guybo
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 97acd09d223e59fbf4109bc8a20a25a2ed8ea366
ms.openlocfilehash: 2071debe24ef2705b2ee05ce1210a813234cf277
ms.lasthandoff: 03/10/2017


---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Wat zijn virtuele-machineschaalsets in Azure?
Virtuele-machineschaalsets vormen een compute-resource van Azure die u kunt gebruiken om een set identieke VM's te implementeren en te beheren. Bij schaalsets worden alle virtuele machines op dezelfde manier geconfigureerd, waardoor de schaalsets echt automatisch schalen ondersteunen. De virtuele machines hoeven niet vooraf te worden ingericht, zodat het eenvoudiger wordt om services op grote schaal te bouwen voor big compute, big data en workloads met containers.

Voor toepassingen die compute-resources in en uit moeten schalen, worden schaalaanpassingen impliciet verdeeld over fout- en updatedomeinen. Raadpleeg de [aankondiging in de Azure-blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/) voor een introductie over schaalsets.

Bekijk deze video's voor meer informatie over schaalsets:

* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Schaalsets maken en beheren
U kunt een schaalset maken in [Azure Portal](https://portal.azure.com) door *nieuw* te selecteren en 'schaal' in de zoekbalk te typen. U ziet 'Virtuele-machineschaalset' staan in de resultaten. Daarna vult u de vereiste velden in om uw schaalset aan te passen en te implementeren. Er zijn ook opties in de portal om basisregels voor automatisch schalen in te stellen op basis van CPU-gebruik.

Schaalsets kunnen ook worden gedefinieerd en geïmplementeerd met behulp van JSON-sjablonen en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx), net als individuele virtuele machines in Azure Resource Manager. Dit betekent dat alle standaardimplementatiemethoden van Azure Resource Manager kunnen worden toegepast. Zie [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over sjablonen.

[Hier](https://github.com/Azure/azure-quickstart-templates) in de GitHub-opslagplaats voor Azure Quickstart-sjablonen vindt u een aantal voorbeeldsjablonen voor VM-schaalsets (zoek de sjablonen met *vmss* in de titel).

Op de specifieke pagina's voor de sjablonen ziet u een knop voor portalimplementatie. Als u de schaalset wilt implementeren, klikt u op de knop en vult u vervolgens de vereiste parameters in de portal in. Als u niet zeker weet of een resource hoofdletters of hoofdletters en kleine letters door elkaar ondersteunt, kunt u het beste kleine letters en cijfers gebruiken in parameterwaarden. U kunt ook deze handige video bekijken waarin de sjabloon voor schaalsets wordt toegelicht:

[Toelichting sjabloon voor VM-schaalset](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-scale-set-out-and-in"></a>Een schaalset in- en uitschalen
U kunt de capaciteit van een schaalset in Azure Portal wijzigen door te klikken op het gedeelte _Schalen_ in _Instellingen_. 

[Azure CLI](https://github.com/Azure/azure-cli) biedt de opdracht _Schalen_ om de capaciteit van de schaalset op de opdrachtregel te wijzigen. Ga bijvoorbeeld als volgt te werk om een schaalset in te stellen op een capaciteit van 10 virtuele machines:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

U kunt het aantal virtuele machines in een schaalset instellen met behulp van PowerShell met de opdracht _Update-AzureRmVmss_:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Als u het aantal virtuele machines in een schaalset wilt verhogen of verlagen met een Azure Resource Manager-sjabloon, wijzigt u de eigenschap *capaciteit* en implementeert u de sjabloon opnieuw. Deze gebruiksvriendelijke functie maakt het eenvoudig om schaalsets te implementeren met Automatisch schalen van Azure, of uw eigen aangepaste schaallaag te schrijven als u aangepaste schaalgebeurtenissen wilt definiëren die niet door Automatisch schalen van Azure worden ondersteund. 

Als u een Azure Resource Manager-sjabloon opnieuw wilt implementeren om de capaciteit te wijzigen, kunt u een veel kleinere sjabloon definiëren dat alleen het SKU-eigenschappenpakket met de bijgewerkte capaciteit bevat. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) kunt u een voorbeeld bekijken.

## <a name="autoscale"></a>Automatisch schalen

Een schaalset kan eventueel worden geconfigureerd met instellingen voor automatisch schalen wanneer deze is gemaakt in Azure Portal. Zo kan het aantal virtuele machines worden vergroot of verkleind op basis van het gemiddelde CPU-gebruik. Veel van de schaalsetsjablonen in [Azure Quick Start-sjablonen](https://github.com/Azure/azure-quickstart-templates) definiëren de instellingen voor automatisch schalen. U kunt instellingen voor automatisch schalen ook toevoegen aan een bestaande schaalset. Dit is bijvoorbeeld een Azure PowerShell-script waarmee u automatisch schalen op basis van CPU toevoegt aan een schaalset:

```PowerShell

$subid = "yoursubscriptionid"
$rgname = "yourresourcegroup"
$vmssname = "yourscalesetname"
$location = "yourlocation" # e.g. southcentralus

$rule1 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator GreaterThan -MetricStatistic Average -Threshold 60 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Increase -ScaleActionValue 1
$rule2 = New-AzureRmAutoscaleRule -MetricName "Percentage CPU" -MetricResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -Operator LessThan -MetricStatistic Average -Threshold 30 -TimeGrain 00:01:00 -TimeWindow 00:05:00 -ScaleActionCooldown 00:05:00 -ScaleActionDirection Decrease -ScaleActionValue 1
$profile1 = New-AzureRmAutoscaleProfile -DefaultCapacity 2 -MaximumCapacity 10 -MinimumCapacity 2 -Rules $rule1,$rule2 -Name "autoprofile1"
Add-AzureRmAutoscaleSetting -Location $location -Name "autosetting1" -ResourceGroup $rgname -TargetResourceId /subscriptions/$subid/resourceGroups/$rgname/providers/Microsoft.Compute/virtualMachineScaleSets/$vmssname -AutoscaleProfiles $profile1
```

 Hier vindt u een lijst met geldige metrische gegevens die u kunt schalen: [Ondersteunde metrische gegevens met Azure Monitor](../monitoring-and-diagnostics/monitoring-supported-metrics.md) onder de kop _Microsoft.Compute/virtualMachineScaleSets_. Er zijn ook meer geavanceerde opties voor automatisch schalen beschikbaar, waaronder automatisch schalen op basis van een planning en het gebruik van webhooks om te integreren met waarschuwingssystemen.

## <a name="monitoring-your-scale-set"></a>Uw schaalset controleren
In [Azure Portal](https://portal.azure.com) ziet u de schaalsets en de bijbehorende eigenschappen. De portal ondersteunt bovendien beheerbewerkingen die kunnen worden uitgevoerd op schaalsets en op afzonderlijke virtuele machines in een schaalset. De portal biedt ook een aanpasbare grafiek over het resourcegebruik. Als u de onderliggende JSON-definitie van een Azure-resource wilt weergeven of bewerken, kunt u ook de [Azure Resource Explorer](https://resources.azure.com) gebruiken. Schaalsets zijn een resource onder de Azure Resource Provider Microsoft.Compute. U kunt ze vanaf deze site bekijken door de volgende koppelingen uit te vouwen:

**Abonnementen -> uw abonnement -> resourceGroups -> providers > Microsoft.Compute -> virtualMachineScaleSets -> uw schaalset -> enz.**

## <a name="scale-set-scenarios"></a>Scenario's voor schaalsets
In dit gedeelte wordt een aantal typische scenario's voor schaalsets genoemd. Deze scenario's worden toegepast voor een aantal van de hogere Azure-services, zoals Batch, Service Fabric en Azure Container Service.

* **RDP / SSH naar instanties van schaalset**: een schaalset wordt binnen een VNET gemaakt en er worden geen openbare IP-adressen toegewezen aan individuele virtuele machines in de schaalset. Met dit beleid voorkomt u de kosten en het overheadbeheer die nodig zijn om afzonderlijke openbare IP-adressen aan alle knooppunten in het rekenraster toe te wijzen. U kunt verbinding maken met deze virtuele machines vanuit andere bronnen in uw VNET, bijvoorbeeld load balancers en zelfstandige virtuele machines, waaraan openbare IP-adressen kunnen worden toegewezen.
* **Verbinding maken met virtuele machines met behulp van NAT-regels:** u kunt een openbaar IP-adres maken, dit toewijzen aan een load balancer en een binnenkomende NAT-pool definiëren die poorten van het IP-adres toewijst aan een poort van een virtuele machine in de schaalset. Bijvoorbeeld:
  
  | Bron | Bronpoort | Doel | Doelpoort |
  | --- | --- | --- | --- |
  |  Openbare IP |Poort 50000 |vmss\_0 |Poort 22 |
  |  Openbare IP |Poort 50001 |vmss\_1 |Poort 22 |
  |  Openbare IP |Poort 50002 |vmss\_2 |Poort 22 |
  
   In dit voorbeeld zijn NAT-regels gebruikt om een SSH-verbinding met elke virtuele machine in een schaalset mogelijk te maken via een enkel openbaar IP-adres: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Hier volgt een voorbeeld van hoe u hetzelfde doet met RDP en Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Verbinding maken met virtuele machines met behulp van een 'jumpbox'**: als u een schaalset en een losstaande virtuele machine in hetzelfde VNET maakt, kunnen de losse virtuele machine en de virtuele machines in de schaalset met elkaar verbinding maken via de interne IP-adressen zoals gedefinieerd in het VNET/Subnet. As u een openbaar IP-adres maakt en dit toewijst aan de losstaande virtuele machine, kunt u via RDP of SSH verbinding maken met de losse virtuele machine en vervolgens vanaf die machine verbinding maken met de instanties van uw schaalset. Het valt u misschien op dat een eenvoudige schaalset inherent veiliger is dan een eenvoudige losse virtuele machine met een openbaar IP-adres in de standaardconfiguratie.
  
   Deze sjabloon implementeert bijvoorbeeld een eenvoudige schaalset met een losse VM: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Taken verdelen voor instanties van een schaalset:** als u middels een round robin-benadering taken aan een rekencluster van virtuele machines wilt toewijzen, kunt u een Azure Load Balancer configureren met Layer-4-taakverdelingsregels. U kunt tests definiëren om te verifiëren of uw toepassing wordt uitgevoerd, door poorten te pingen met een opgegeven protocol, interval en aanvraagpad. De Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) ondersteunt ook schaalsets, naast Layer-7 en complexere scenario's voor taakverdeling.
  
   In dit voorbeeld wordt een schaalset gemaakt die wordt uitgevoerd op Apache-webservers en die een load balancer gebruikt om de taken te verdelen die elke virtuele machine ontvangt: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (kijk naar het resourcetype Microsoft.Network/loadBalancers en het networkProfile en extensionProfile in de virtualMachineScaleSet)

   In dit voorbeeld wordt een Application Gateway gebruikt. Linux:  [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway). Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) 

* **Een schaalset implementeren als een rekencluster in PaaS-clusterbeheer**: schaalsets worden soms omschreven als de werkrol van de volgende generatie. Dit is een accurate beschrijving, maar deze kan er wel voor zorgen dat schaalsetfuncties worden verward met Azure Cloud Services-functies. Schaalsets bieden een echte werkrol of werkrol-resource in dat opzicht dat ze een algemene rekenresource zijn die losstaat van platform/runtime, aanpasbaar is en integreert met Azure Resource Manager IaaS.
  
   Een Clous Services-werkrol is beperkt in het kader van ondersteuning voor platform/runtime (alleen Windows-platforminstallatiekopieën), maar omvat wel services als wisselen van VIP, configureerbare upgrade-instellingen en specifieke instellingen voor runtime/toepassingsimplementatie die of *nog* niet beschikbaar zijn in schaalsets of worden geleverd voor andere, hogere PaaS-services zoals Service Fabric. U kunt schaalsets zien als een infrastructuur die ondersteuning biedt voor PaaS. PaaS-oplossingen zoals [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) zijn gebaseerd op deze infrastructuur.
  
   Voor een voorbeeld van deze benadering implementeert de [Azure Container Service](https://azure.microsoft.com/services/container-service/) een cluster op basis van de schaalsets met een container orchestrator: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="scale-set-performance-and-scale-guidance"></a>Richtlijnen voor prestaties en schaal van schaalsets
* Schaalsets ondersteunen maximaal 1000 virtuele machines in een schaalset. Als u uw eigen aangepaste VM-installatiekopieën wilt maken en uploaden, is de limiet 100. Zie voor overwegingen bij het gebruik van grote schaalsets [Werken met grote schaalsets voor virtuele machines](virtual-machine-scale-sets-placement-groups.md).
* U hoeft vooraf geen Azure-opslagaccounts te maken om schaalsets te kunnen gebruiken. Schaalsets bieden ondersteuning voor Azure Managed Disks, waardoor u zich geen zorgen meer hoeft te maken over de prestaties als u veel schijven per opslagaccount gebruikt. Zie voor meer informatie [Schaalsets en beheerde schijven voor virtuele Azure-machines](virtual-machine-scale-sets-managed-disks.md).
* Overweeg om Azure Premium-opslag te gebruiken in plaats van Standard-opslag voor snellere, beter te voorspellen VM-inrichting en verbeterde IO-prestaties.
* Het aantal VM's dat u kunt maken, is beperkt tot de kernquota van de regio waarin u ze implementeert. Mogelijk moet u contact opnemen met de klantenservice om uw limiet voor rekenquota te verhogen, zelfs als u nu een hoge limiet voor het aantal cores voor gebruik met Azure-cloudservices hebt. Als u uw quota wilt opvragen, kunt u de volgende Azure CLI-opdracht uitvoeren: `azure vm list-usage`, of de volgende PowerShell-opdracht: `Get-AzureRmVMUsage` (als u een versie van PowerShell lager dan 1.0 gebruikt, gebruikt u `Get-AzureVMUsage`).

## <a name="scale-set-frequently-asked-questions"></a>Veelgestelde vragen over schaalsets
**V:** Hoeveel virtuele machines kan een schaalset bevatten?

**A:** Een setschaal kan tussen 0 en 1000 virtuele machines op basis van platforminstallatiekopieën bevatten of 0-100 virtuele machines gebaseerd op aangepaste installatiekopieën. 

**V:** Worden gegevensschijven binnen schaalsets ondersteund?

**A:** Ja. Een schaalset kan een configuratie voor gekoppelde gegevensstations definiëren die op alle virtuele machines in de set wordt toegepast. Zie voor meer informatie (Azure-schaalsets en gekoppelde gegevensschijven)[virtual-machine-scale-sets-attached-disks.md]. Andere opties voor het opslaan van gegevens zijn:

* Azure-bestanden (gedeelde SMB-stations)
* Station van het besturingssysteem
* Tijdelijk station (lokaal, niet ondersteund door Azure Storage)
* Azure-gegevensservice (bijvoorbeeld Azure-tabellen, Azure-blobs)
* Externe gegevensservice (bijvoorbeeld externe database)

**V:** Welke Azure-regio's ondersteunen schaalsets?

**A:** Alle regio's ondersteunen schaalsets.

**V:** Hoe maak ik een schaalset met behulp van een aangepaste installatiekopie?

**A:** Maak een beheerde schijf op basis van uw aangepaste installatiekopie-VHD en verwijs ernaar in de sjabloon van de schaalset. Hier volgt een voorbeeld: [https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**V:** Als ik de capaciteit van mijn schaalset verlaag van 20 naar 15, welke virtuele machines worden er dan verwijderd?

**A:** Virtuele machines worden gelijkmatig uit upgradedomeinen en foutdomeinen van de schaalset verwijderd om de beschikbaarheid te maximaliseren. De VM's met de hoogste id's worden het eerst verwijderd.

**V:** Wat gebeurt er dan als ik de capaciteit verhoog van 15 naar 18?

**A:** Als u de capaciteit verhoogt naar 18, worden er 3 nieuwe virtuele machines gemaakt. De ID van elke VM-instantie wordt oplopend gegenereerd vanaf de vorige hoogste waarde (bijvoorbeeld 20, 21, 22). De VM's worden verdeeld over upgrade- en foutdomeinen.

**V:** Kan ik een uitvoeringsvolgorde toepassen wanneer ik meerdere extensies in een schaalset gebruik?

**A:** Niet direct, maar bij de CustomScript-extensie kunt u uw script laten wachten op de voltooiing van een andere extensie ([bijvoorbeeld door het extensielogboek te controleren](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Meer informatie over de uitvoeringsvolgorde van extensies vindt u in dit blogartikel: [Extensievolgorde bij VM-schaalsets in Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**V:** Maken schaalsets gebruik van beschikbaarheidssets van Azure?

**A:** Ja. Een schaalset is een impliciete beschikbaarheidsset met vijf foutdomeinen en vijf upgradedomeinen. Schaalsets met meer dan 100 virtuele machines omvatten meerdere 'plaatsingsgroepen' die gelijkwaardig aan meerdere beschikbaarheidssets zijn. Zie voor meer informatie over de plaatsing van groepen [Werken met grote schaalsets voor virtuele machines](virtual-machine-scale-sets-placement-groups.md). Een beschikbaarheidsset van virtuele machines kan bestaan in hetzelfde VNET als een schaalset van virtuele machines. Een veelvoorkomende configuratie is om beheerknooppunt-VM's, waarvoor vaak unieke configuratie in een beschikbaarheidsset nodig is, en gegevensknooppunten in de schaalset te zetten.

Meer veelgestelde vragen over schaalsets vindt u in de [Veelgestelde vragen over virtuele-machineschaalsets in Azure](virtual-machine-scale-sets-faq.md).

