---
title: Overzicht van virtuele-machineschaalsets in Azure | Microsoft Docs
description: Meer informatie over virtuele-machineschaalsets in Azure
services: virtual-machine-scale-sets
documentationcenter: ''
author: gatneil
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 76ac7fd7-2e05-4762-88ca-3b499e87906e
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/01/2017
ms.author: negat
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 64c4b309d214b91c759ff93ad659b9c69320bea7
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2018
ms.locfileid: "30201152"
---
# <a name="what-are-virtual-machine-scale-sets-in-azure"></a>Wat zijn virtuele-machineschaalsets in Azure?
Virtuele-machineschaalsets vormen een compute-resource van Azure die u kunt gebruiken om een set identieke VM's te implementeren en te beheren. Met behulp van schaalsets worden alle virtuele machines op dezelfde manier geconfigureerd en automatisch geschaald. U hoeft de virtuele machines dus niet vooraf in te richten. Hierdoor wordt het gemakkelijker om grootschalige services te ontwikkelen voor Big Compute, grote gegevens en beperkte workloads.

Voor toepassingen die compute-resources in en uit moeten schalen, worden schaalaanpassingen impliciet verdeeld over fout- en updatedomeinen. Raadpleeg de [aankondiging in de Azure-blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/) voor een verdere introductie van schaalsets.

Bekijk voor meer informatie over schaalsets deze video's:

* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-scale-sets"></a>Schaalsets maken en beheren
U kunt een schaalset maken in [Azure Portal](https://portal.azure.com) door **Een resource maken** te selecteren en **schaal** in de zoekbalk te typen. **Virtuele-machineschaalset** wordt vermeld in de resultaten. Daarna vult u de vereiste velden in om uw schaalset aan te passen en te implementeren. Er zijn in de portal ook opties om basisregels voor automatisch schalen in te stellen op basis van CPU-gebruik. Voor het beheren van uw schaalset kunt u Azure Portal, [Azure PowerShell-cmdlets](virtual-machine-scale-sets-windows-manage.md) of de Azure CLI 2.0 gebruiken.

Schaalsets kunnen op [beschikbaarheidszones](virtual-machine-scale-sets-use-availability-zones.md) worden geïmplementeerd.

U kunt schaalsets definiëren en implementeren met behulp van JSON-sjablonen en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx), net als individuele virtuele machines in Azure Resource Manager. U kunt daarom alle standaardimplementatiemethoden van Azure Resource Manager gebruiken. Zie [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over sjablonen.

In de [GitHub-opslagplaats voor Azure Quickstart-sjablonen](https://github.com/Azure/azure-quickstart-templates) kunt u een aantal voorbeeldsjablonen voor virtuele-machineschaalsets vinden. (Zoek naar sjablonen met **vmss** in de titel.)

Voor de Quick Start-sjabloonvoorbeelden is een knop 'Implementeren naar Azure' in het Leesmij-bestand gekoppeld aan de implementatiefunctie van de portal. Als u de schaalset wilt implementeren, klikt u op de knop en vult u vervolgens de vereiste parameters in de portal in. 


## <a name="autoscale"></a>Automatisch schalen
Om consistente toepassingsprestaties te behouden, kunt u het aantal VM-exemplaren in de schaalset automatisch vergroten of verkleinen. Dankzij deze mogelijkheid voor automatisch schalen hoeft u minder te controleren en kunt u de schaalset optimaliseren als de klantvraag in de loop van de tijd verandert. U definieert regels op basis van metrische gegevens voor prestaties, reactietijd van toepassingen of een vast schema, en uw schaalset wordt automatisch vergroot of verkleind indien nodig.

Voor basisregels voor automatisch schalen kunt u hostgebaseerde metrische gegevens voor prestaties gebruiken, zoals CPU-gebruik of schijf-I/O. Deze hostgebaseerde metrische gegevens voor prestaties zijn automatisch beschikbaar, zonder extra agents of extensies te hoeven installeren en configureren. Regels voor automatisch schalen die gebruikmaken van hostgebaseerde metrische gegevens, kunnen worden gemaakt met een van de volgende hulpprogramma's:

- [Azure Portal](virtual-machine-scale-sets-autoscale-portal.md)
- [Azure PowerShell](virtual-machine-scale-sets-autoscale-powershell.md)
- [Azure CLI 2.0](virtual-machine-scale-sets-autoscale-cli.md)

Als u gedetailleerdere metrische gegevens voor prestaties wilt gebruiken, kunt u de diagnostische Azure-extensie installeren en configureren op VM-exemplaren in uw schaalset. Met de diagnostische Azure-extensie kunt u aanvullende metrische gegevens voor prestaties, zoals geheugenverbruik, verzamelen uit elk VM-exemplaar. Deze metrische gegevens voor prestaties worden gestreamd naar een Azure-opslagaccount. U kunt regels voor automatisch schalen maken om deze gegevens te gebruiken. Zie voor meer informatie de artikelen voor het inschakelen van de diagnostische Azure-extensie op een [Linux-VM](../virtual-machines/linux/diagnostic-extension.md) of [Windows-VM](../virtual-machines/windows/ps-extensions-diagnostics.md).

Voor het controleren van de toepassingsprestaties zelf kunt u een klein instrumentatiepakket installeren en configureren in uw toepassing voor App Insights. Gedetailleerde metrische gegevens voor prestaties voor de reactietijd van de toepassing of het aantal sessies kunnen vervolgens terug van uw app worden gestreamd. Vervolgens kunt u regels voor automatisch schalen met gedefinieerde drempelwaarden voor de prestaties op toepassingsniveau zelf maken. Zie [Wat is Application Insights?](../application-insights/app-insights-overview.md) voor meer informatie over App Insights.


## <a name="manually-scaling-a-scale-set-out-and-in"></a>Een schaalset handmatig in- en uitschalen
U kunt de capaciteit van een schaalset in Azure Portal handmatig wijzigen door te klikken op het gedeelte **Schalen** in **Instellingen**. 

Gebruik de opdracht **Schalen** in [Azure CLI](https://github.com/Azure/azure-cli) om de capaciteit van de schaalset op de opdrachtregel te wijzigen. Gebruik bijvoorbeeld deze opdracht om een schaalset in te stellen op een capaciteit van 10 VM's:

```bash
az vmss scale -g resourcegroupname -n scalesetname --new-capacity 10 
```

Gebruik de opdracht **Update-AzureRmVmss** om het aantal VM's in een schaalset in te stellen met behulp van PowerShell:

```PowerShell
$vmss = Get-AzureRmVmss -ResourceGroupName resourcegroupname -VMScaleSetName scalesetname  
$vmss.Sku.Capacity = 10
Update-AzureRmVmss -ResourceGroupName resourcegroupname -Name scalesetname -VirtualMachineScaleSet $vmss
```

Als u het aantal virtuele machines in een schaalset wilt verhogen of verlagen met behulp van een Azure Resource Manager-sjabloon, wijzigt u de eigenschap **capaciteit** en implementeert u de sjabloon opnieuw. Deze gebruiksvriendelijke functie maakt het eenvoudig om schaalsets te implementeren met Automatisch schalen van Azure, of om uw eigen aangepaste schaallaag te schrijven als u aangepaste schaalgebeurtenissen wilt definiëren die niet worden ondersteund met Automatisch schalen van Azure. 

Als u een Azure Resource Manager-sjabloon opnieuw wilt implementeren om de capaciteit te wijzigen, kunt u een veel kleinere sjabloon definiëren die alleen het eigenschappenpakket **SKU** met de bijgewerkte capaciteit bevat. [Hier volgt een voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing).


## <a name="monitoring-your-scale-set"></a>Uw schaalset controleren
In [Azure Portal](https://portal.azure.com) ziet u de schaalsets en de bijbehorende eigenschappen. De portal biedt ook ondersteuning voor beheerbewerkingen. U kunt beheerbewerkingen zowel op schaalsets uitvoeren als op afzonderlijke VM's binnen een schaalset. De portal biedt ook een aanpasbare grafiek over het resourcegebruik. 

Als u de onderliggende JSON-definitie van een Azure-resource wilt weergeven of bewerken, kunt u ook [Azure Resource Explorer](https://resources.azure.com) gebruiken. Schaalsets vormen een resource onder de Microsoft.Compute Azure-resourceprovider. Op deze site kunt u ze zien door op de volgende koppelingen te klikken:

**Abonnementen** > **uw abonnement** > **resourceGroups** > **providers** > **Microsoft.Compute** > **virtualMachineScaleSets** > **uw schaalset** > enzovoort.

## <a name="scale-set-scenarios"></a>Scenario's voor schaalsets
In dit gedeelte wordt een aantal typische scenario's voor schaalsets genoemd. Deze scenario's worden toegepast voor een aantal van de hogere Azure-services, zoals Batch, Service Fabric en Container Service.

* **RDP of SSH gebruiken om verbinding te maken met instanties van schaalset**: binnen een virtueel netwerk wordt een schaalset gemaakt. Er worden standaard geen openbare IP-adressen toegewezen aan afzonderlijke VM's in de schaalset. Met dit beleid voorkomt u de kosten en het overheadbeheer die nodig zijn om afzonderlijke openbare IP-adressen aan alle knooppunten in het rekenraster toe te wijzen. Als u rechtstreekse externe verbindingen met virtuele machines in de schaalset nodig hebt, kunt u een schaalset configureren om automatisch openbare IP-adressen toe te wijzen aan nieuwe virtuele machines. U kunt ook verbinding maken met VM's vanuit andere resources in uw virtuele netwerk waaraan openbare IP-adressen kunnen worden toegewezen, bijvoorbeeld load balancers en zelfstandige virtuele machines. 
* **Verbinding maken met VM's met behulp van NAT-regels:** u kunt een openbaar IP-adres maken, dit toewijzen aan een load balancer en een binnenkomende NAT-pool definiëren. Met deze acties worden poorten in het IP-adres toegewezen aan een poort op een VM in de schaalset. Bijvoorbeeld:
  
  | Bron | Bronpoort | Doel | Doelpoort |
  | --- | --- | --- | --- |
  |  Openbare IP |Poort 50000 |vmss\_0 |Poort 22 |
  |  Openbare IP |Poort 50001 |vmss\_1 |Poort 22 |
  |  Openbare IP |Poort 50002 |vmss\_2 |Poort 22 |
  
   In [dit voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat) worden met behulp van één openbaar IP-adres NAT-regels gedefinieerd voor een SSH-verbinding naar elke VM in een schaalset.
  
   In [dit voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat) gebeurt hetzelfde met RDP en Windows.
* **Verbinding maken met VM's met behulp van een 'jumpbox'**: als u een schaalset en een zelfstandige VM in hetzelfde virtuele netwerk maakt, kunnen de zelfstandige VM en de VM in de schaalset verbinding met elkaar maken via de interne IP-adressen zoals gedefinieerd in het virtuele netwerk of subnet. As u een openbaar IP-adres maakt en dit toewijst aan de zelfstandige VM, kunt u via RDP of SSH verbinding maken met de zelfstandige VM. U kunt vervolgens vanaf deze machine verbinding maken met de exemplaren in uw schaalset. Het valt u misschien op dat een eenvoudige schaalset inherent veiliger is dan een eenvoudige zelfstandige VM met een openbaar IP-adres in de standaardconfiguratie.
  
   [Deze sjabloon](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox) implementeert bijvoorbeeld een eenvoudige schaalset met een zelfstandige VM. 
* **Taken verdelen voor exemplaren in een schaalset:** als u via een round robin-benadering taken aan een rekencluster van VM's wilt toewijzen, kunt u een load balancer van Azure configureren met Layer-4-taakverdelingsregels. U kunt tests definiëren om te verifiëren of uw toepassing wordt uitgevoerd, door poorten te pingen met een opgegeven protocol, interval en aanvraagpad. [Azure Application Gateway](https://azure.microsoft.com/services/application-gateway/) ondersteunt ook schaalsets, naast Layer-7 en complexere scenario's voor taakverdeling.
  
   In [dit voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) wordt een schaalset gemaakt waarmee Apache-webservers worden uitgevoerd en die gebruikmaakt van een load balancer om de taken te verdelen die elke VM ontvangt. (Kijk naar het resourcetype Microsoft.Network/loadBalancers en networkProfile en extensionProfile in virtualMachineScaleSet.)

   In [dit Linux-voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-app-gateway) en [dit Windows-voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-app-gateway) wordt de toepassingsgateway gebruikt.  

* **Een schaalset implementeren als een rekencluster in PaaS-clusterbeheer**: schaalsets worden soms omschreven als de werkrol van de volgende generatie. Dit is een accurate beschrijving, maar deze kan er wel voor zorgen dat schaalsetfuncties worden verward met Azure Cloud Services-functies. In zekere zin bieden schaalsets een echte werkrol of worker-resource. Schaalsets zijn een gegeneraliseerde compute-resource die onafhankelijk is van platform/runtime, aanpasbaar is en kan worden geïntegreerd met Azure Resource Manager-IaaS.
  
   Een worker-rol in Cloud Services is beperkt met betrekking tot platform/runtime-ondersteuning (alleen installatiekopieën van Windows-platform). Maar deze omvat ook services zoals wisselen van VIP, configureerbare instellingen voor upgraden en specifieke instellingen voor runtime/app-implementatie. Deze services zijn *nog niet* beschikbaar voor schaalsets of ze worden geleverd met andere PaaS-services van een hoger niveau, zoals Azure Service Fabric. U kunt schaalsets zien als een infrastructuur die ondersteuning biedt voor PaaS. PaaS-oplossingen zoals [Service Fabric](https://azure.microsoft.com/services/service-fabric/) zijn gebaseerd op deze infrastructuur.
  
   In [dit voorbeeld](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos) van deze benadering wordt met [Azure Container Service](https://azure.microsoft.com/services/container-service/) een cluster geïmplementeerd op basis van schaalsets met een container-orchestrator.

## <a name="scale-set-performance-and-scale-guidance"></a>Richtlijnen voor prestaties en schaal van schaalsets
* Een schaalset biedt ondersteuning voor maximaal 1000 VM's. Als u uw eigen aangepaste VM-installatiekopieën wilt maken en uploaden, is de limiet 300. Zie [Werken met grote schaalsets voor virtuele machines](virtual-machine-scale-sets-placement-groups.md) voor overwegingen bij het gebruik van grote virtuele-machineschaalsets.
* U hoeft vooraf geen Azure-opslagaccounts te maken om schaalsets te kunnen gebruiken. Schaalsets bieden ondersteuning voor beheerde schijven in Azure. Hierdoor hoeft u zich geen zorgen meer te maken over de prestaties als u veel schijven per opslagaccount gebruikt. Zie voor meer informatie [Schaalsets en beheerde schijven voor virtuele Azure-machines](virtual-machine-scale-sets-managed-disks.md).
* Overweeg om Azure Premium-opslag te gebruiken in plaats van Azure-opslag voor snellere, beter te voorspellen VM-inrichting en verbeterde IO-prestaties.
* Het aantal vCPU's dat u kunt maken, is beperkt tot het kernquotum van de regio waarin u ze implementeert. Mogelijk moet u contact opnemen met klantondersteuning om de limiet voor uw rekenquotum te verhogen, zelfs als u nu een hoge limiet hebt voor het aantal vCPU's dat u met Azure Cloud Services gebruikt. Voer de volgende Azure CLI-opdracht uit om uw quotum op te vragen: `az vm list-usage`. Of voer deze PowerShell-opdracht uit: `Get-AzureRmVMUsage`.

## <a name="frequently-asked-questions-for-scale-sets"></a>Veelgestelde vragen over schaalsets
**V:** Hoeveel virtuele machines kan een schaalset bevatten?

**A:** Een schaalset kan 0 tot 1000 virtuele machines bevatten op basis van platforminstallatiekopieën, of 0 tot 300 virtuele machines op basis van aangepaste installatiekopieën. 

**V:** Worden gegevensschijven binnen schaalsets ondersteund?

**A:** Ja. Een schaalset kan een configuratie voor gekoppelde gegevensschijven definiëren die op alle VM's in de set wordt toegepast. Zie [Schaalsets en gekoppelde gegevensschijven in Azure](virtual-machine-scale-sets-attached-disks.md) voor meer informatie. Andere opties voor het opslaan van gegevens zijn:

* Azure-bestanden (gedeelde SMB-stations)
* Station van het besturingssysteem
* Tijdelijk station (lokaal, niet ondersteund met Azure Storage)
* Azure-gegevensservice (bijvoorbeeld Azure-tabellen, Azure-blobs)
* Externe gegevensservice (bijvoorbeeld externe database)

**V:** Welke Azure-regio's ondersteunen schaalsets?

**A:** Alle regio's ondersteunen schaalsets.

**V:** Hoe maak ik een schaalset met behulp van een aangepaste installatiekopie?

**A:** Maak een beheerde schijf op basis van uw aangepaste installatiekopie-VHD en verwijs ernaar in de sjabloon van de schaalset. [Hier volgt een voorbeeld](https://github.com/chagarw/MDPP/tree/master/101-vmss-custom-os).

**V:** Als ik de capaciteit van mijn schaalset verlaag van 20 naar 15, welke virtuele machines worden er dan verwijderd?

**A:** Virtuele machines worden gelijkmatig verwijderd uit updatedomeinen en foutdomeinen van de schaalset om de beschikbaarheid te maximaliseren. VM's met de hoogste id's worden het eerst verwijderd.

**V:** Wat gebeurt er als ik de capaciteit verhoog van 15 naar 18?

**A:** Als u de capaciteit verhoogt naar 18, worden er 3 nieuwe virtuele machines gemaakt. De id van elk VM-exemplaar wordt oplopend gegenereerd vanaf de vorige hoogste waarde (bijvoorbeeld 20, 21, 22). VM's worden verdeeld over foutdomeinen en updatedomeinen.

**V:** Kan ik een uitvoeringsvolgorde toepassen wanneer ik meerdere extensies in een schaalset gebruik?

**A:** Niet direct, maar bij de customScript-extensie kunt u uw script laten wachten op de voltooiing van een andere extensie. Meer informatie over de uitvoeringsvolgorde van extensies vindt u in het blogbericht: [Extensievolgorde bij virtuele-machineschaalsets in Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**V:** Maken schaalsets gebruik van beschikbaarheidssets van Azure?

**A:** Ja. Een schaalset is een impliciete beschikbaarheidsset met vijf foutdomeinen en vijf updatedomeinen. Schaalsets met meer dan 100 virtuele machines omvatten meerdere *plaatsingsgroepen* die gelijkwaardig zijn aan meerdere beschikbaarheidssets. Zie voor meer informatie over de plaatsing van groepen [Werken met grote schaalsets voor virtuele machines](virtual-machine-scale-sets-placement-groups.md). Een beschikbaarheidsset met virtuele machines kan bestaan in hetzelfde virtuele netwerk als een schaalset met virtuele machines. Een veelvoorkomende configuratie is om beheerknooppunt-VM's (waarvoor vaak een unieke configuratie is vereist) in een beschikbaarheidsset te zetten en gegevensknooppunten in de schaalset te zetten.

Meer antwoorden op vragen over schaalsets vindt u in de [Veelgestelde vragen over virtuele-machineschaalsets in Azure](virtual-machine-scale-sets-faq.md).
