---
title: Overzicht van virtuele-machineschaalsets | Microsoft Docs
description: Meer informatie over virtuele-machineschaalsets
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
ms.date: 1/25/2017
ms.author: guybo
translationtype: Human Translation
ms.sourcegitcommit: 868d65642ab9ca3e1c35d33a7cb9e9dd8c31c430
ms.openlocfilehash: f945ac0357c11c70780dea8e62f094457a213d96


---
# <a name="virtual-machine-scale-sets-overview"></a>Overzicht van virtuele-machineschaalsets
Virtuele-machineschaalsets vormen een compute-resource van Azure die u kunt gebruiken om een set identieke VM's te implementeren en te beheren. Bij VM-schaalsets worden alle VM's op dezelfde manier geconfigureerd, waardoor de schaalsets echt automatisch schalen ondersteunen. VM’s hoeven niet vooraf te worden ingericht, waardoor het eenvoudiger wordt om services op grote schaal te bouwen voor big compute, big data en workloads met containers.

Voor toepassingen die compute-resources in en uit moeten schalen, worden schaalaanpassingen impliciet verdeeld over fout- en updatedomeinen. Raadpleeg de [aankondiging in het Azure-blog](https://azure.microsoft.com/blog/azure-virtual-machine-scale-sets-ga/) voor een introductie over VM-schaalsets.

Bekijk deze video's voor meer informatie over VM-schaalsets:

* [Mark Russinovich vertelt over Azure-schaalsets](https://channel9.msdn.com/Blogs/Regular-IT-Guy/Mark-Russinovich-Talks-Azure-Scale-Sets/)  
* [Guy Bowerman over virtuele-machineschaalsets](https://channel9.msdn.com/Shows/Cloud+Cover/Episode-191-Virtual-Machine-Scale-Sets-with-Guy-Bowerman)

## <a name="creating-and-managing-vm-scale-sets"></a>VM-schaalsets maken en beheren
U kunt een VM-schaalset maken in [Azure Portal](https://portal.azure.com) door *nieuw* te selecteren en ‘schaal’ in de zoekbalk te typen. U ziet ‘Virtuele-machineschaalset’ staan in de resultaten. Daarna vult u de vereiste velden in om uw schaalset aan te passen en te implementeren. Er zijn ook opties in de portal om basisregels voor automatisch schalen in te stellen op basis van CPU-gebruik.

VM-schaalsets kunnen ook worden gedefinieerd en geïmplementeerd met behulp van JSON-sjablonen en [REST API's](https://msdn.microsoft.com/library/mt589023.aspx), net als individuele VM's in Azure Resource Manager. Dit betekent dat alle standaardimplementatiemethoden van Azure Resource Manager kunnen worden toegepast. Zie [Azure Resource Manager-sjablonen samenstellen](../azure-resource-manager/resource-group-authoring-templates.md) voor meer informatie over sjablonen.

[Hier](https://github.com/Azure/azure-quickstart-templates) in de GitHub-opslagplaats voor Azure Quickstart-sjablonen vindt u een aantal voorbeeldsjablonen voor VM-schaalsets. (Zoek naar sjablonen met *vmss* in de naam)

Op de specifieke pagina's voor de sjablonen ziet u een knop voor portalimplementatie. Als u de VM-schaalset wilt implementeren, klikt u op de knop en vult u vervolgens de vereiste parameters in de portal in. Als u niet zeker weet of een resource hoofdletters of hoofdletters en kleine letters door elkaar ondersteunt, kunt u het beste kleine letters en cijfers gebruiken in parameterwaarden. U kunt ook deze handige video bekijken waarin de sjabloon voor VM-schaalsets wordt toegelicht:

[Toelichting sjabloon voor VM-schaalset](https://channel9.msdn.com/Blogs/Azure/VM-Scale-Set-Template-Dissection/player)

## <a name="scaling-a-vm-scale-set-out-and-in"></a>Een VM-schaalset in- en uitschalen
Als u het aantal virtuele machines in een VM-schaalset wilt verhogen of verlagen, wijzigt u de eigenschap *capaciteit* en implementeert u de sjabloon opnieuw. Deze gebruiksvriendelijke functie maakt het eenvoudig om uw eigen aangepaste schaallaag te schrijven als u aangepaste schaalgebeurtenissen wilt definiëren die niet door Automatisch schalen van Azure worden ondersteund.

Als u een sjabloon opnieuw wilt implementeren om de capaciteit te wijzigen, kunt u een veel kleinere sjabloon definiëren dat alleen het SKU-eigenschappenpakket met de bijgewerkte capaciteit bevat. [Hier](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing) kunt u een voorbeeld bekijken.

Raadpleeg [Automatisch machines schalen in een virtuele-machineschaalset](virtual-machine-scale-sets-windows-autoscale.md) voor een stapsgewijze uitleg van het maken van een schaalset die automatisch wordt geschaald

## <a name="monitoring-your-vm-scale-set"></a>De VM-schaalset controleren
[Azure Portal](https://portal.azure.com) toont de schaalsets en basiseigenschappen en -bewerkingen, met inbegrip van de VM's in de set en een grafiek van het resourcegebruik. Gebruik de [Azure Resource Explorer](https://resources.azure.com) om VM-schaalsets te bekijken voor meer details. VM-schaalsets zijn een resource onder Microsoft.Compute. U kunt ze vanaf deze site bekijken door de volgende koppelingen uit te vouwen:

**Abonnementen -> uw abonnement -> resourceGroups -> providers > Microsoft.Compute -> virtualMachineScaleSets -> Uw VM-schaalset -> enz.**

## <a name="vm-scale-set-scenarios"></a>Scenario's voor VM-schaalsets
In dit gedeelte wordt een aantal typische scenario's voor VM-schaalsets genoemd. Deze scenario's worden toegepast voor een aantal van de hogere Azure-services, zoals Batch, Service Fabric en Azure Container Service.

* **RDP / SSH naar instanties van VM-schaalset**: een VM-schaalset wordt binnen een VNET gemaakt en er worden geen openbare IP-adressen toegewezen aan individuele VM’s in de schaalset. Zo vermijdt u de kosten en overhead van het toewijzen van aparte openbare IP-adressen aan alle staatloze resources in uw compute-raster. Bovendien kunt u gemakkelijk verbinding maken met deze VM's vanaf andere resources in uw VNET, inclusief resources met openbare IP-adressen, zoals load balancers of losstaande virtuele machines.
* **Verbinding maken met virtuele machines met behulp van NAT-regels:** u kunt een openbaar IP-adres maken, dit toewijzen aan een load balancer en een binnenkomende NAT-pool definiëren die poorten van het IP-adres toewijst aan een poort van een VM in de VM-schaalset. Bijvoorbeeld:
  
  | Bron | Bronpoort | Doel | Doelpoort |
  | --- | --- | --- | --- |
  |  Openbare IP |Poort 50000 |vmss\_0 |Poort 22 |
  |  Openbare IP |Poort 50001 |vmss\_1 |Poort 22 |
  |  Openbare IP |Poort 50002 |vmss\_2 |Poort 22 |
  
   Hier volgt een voorbeeld van het maken van een VM-schaalset waarbij NAT-regels zijn gebruikt om een SSH-verbinding met elke VM in een schaalset mogelijk te maken via een enkel openbaar IP-adres: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-nat)
  
   Hier volgt een voorbeeld van hoe u hetzelfde doet met RDP en Windows: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-nat)
* **Verbinding maken met VM's met behulp van een 'jumpbox'**: als u een VM-schaalset en losstaande VM in hetzelfde VNET maakt, kunnen de losse VM en de VM's in de VM-schaalset met elkaar verbinding maken via de interne IP-adressen zoals gedefinieerd in het VNET/Subnet. As u een openbaar IP-adres maakt en dit toewijst aan de losstaande VM, kunt u via RDP of SSH verbinding maken met de losse VM en vervolgens vanaf die machine verbinding maken met de instanties van uw VM-schaalset. Het valt u misschien op dat een eenvoudige VM-schaalset inherent veiliger is dan een eenvoudige losse VM met een openbaar IP-adres in de standaardconfiguratie.
  
   Deze sjabloon implementeert bijvoorbeeld een eenvoudige schaalset met een losse VM: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-linux-jumpbox)
* **Taken verdelen voor instanties van een VM-schaalset:** als u middels een round robin-benadering taken aan een rekencluster van virtuele machines wilt toewijzen, kunt u een Azure Load Balancer configureren met Layer-4-taakverdelingsregels. U kunt tests definiëren om te verifiëren of uw toepassing wordt uitgevoerd, door poorten te pingen met een opgegeven protocol, interval en aanvraagpad. De Azure [Application Gateway](https://azure.microsoft.com/services/application-gateway/) ondersteunt ook schaalsets, naast Layer-7 en complexere scenario's voor taakverdeling.
  
   Hier volgt een voorbeeld dat een VM-schaalset maakt die wordt uitgevoerd op Apache-webservers en een load balancer gebruikt om de taken te verdelen die elke VM ontvangt: [https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-ubuntu-web-ssl) (kijk naar het resourcetype Microsoft.Network/loadBalancers en het networkProfile en extensionProfile in de virtualMachineScaleSet)
* **Een VM-schaalset implementeren als een rekencluster in PaaS-clusterbeheer**: VM-schaalsets worden soms omschreven als de werkrol van de volgende generatie. Dit is een accurate beschrijving, maar deze kan er wel voor zorgen dat schaalsetfuncties worden verward met PaaS v1-werkrolfuncties. VM-schaalsets bieden een echte werkrol of werkrol-resource in dat opzicht dat ze een algemene rekenresource bieden die losstaat van platform/runtime, aanpasbaar is en integreert met Azure Resource Manager IaaS.
  
   Een PaaS v1-werkrol is beperkt in het kader van ondersteuning voor platform/runtime (alleen Windows-platforminstallatiekopieën), maar omvat wel services als wisselen van VIP, configureerbare upgrade-instellingen en specifieke instellingen voor runtime/toepassingsimplementatie die of *nog* niet beschikbaar zijn in VM-schaalsets of worden geleverd voor andere, hogere PaaS-services zoals Service Fabric. Dit betekent dat u VM-schaalsets kunt beschouwen als infrastructuur die PaaS ondersteunt. Dit betekent dat PaaS-oplossingen zoals Service Fabric of clusterbeheer zoals Mesos op VM-schaalsets kunnen bouwen als een schaalbare rekenlaag.
  
   Voor een voorbeeld van deze benadering implementeert de Azure Container Service een cluster op basis van de schaalsets met een container orchestrator: [https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos](https://github.com/Azure/azure-quickstart-templates/tree/master/101-acs-dcos).

## <a name="vm-scale-set-performance-and-scale-guidance"></a>Richtlijnen voor prestaties en schaal van VM-schaalsets
* Zorg ervoor dat u maximaal 20 virtuele machines per opslagaccount hebt wanneer u schaalsets met onbeheerde schijven gebruikt (tenzij u de eigenschap *overprovision* heb ingesteld op 'false', dan is 40 het maximum). Bij beheerde schijven gelden deze limieten per opslagaccount niet.
* Spreid de eerste letters van de opslagaccountnamen zo veel mogelijk wanneer u schaalsets met onbeheerde schijven gebruikt. De VMSS-sjablonen in [Azure-snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates/) bieden voorbeelden van hoe u dit doet.
* Plan niet meer dan 40 virtuele machines in per VM-schaalset voor een enkel opslagaccount als u aangepaste virtuele machines met onbeheerde schijven gebruikt. De installatiekopie dient vooraf naar het opslagaccount gekopieerd te zijn voordat u de VM-schaalset kunt implementeren. Schaalsets met beheerde schijven ondersteunen maximaal 100 aangepaste virtuele machines met installatiekopieën. Bekijk de veelgestelde vragen voor meer informatie.
* Plan niet meer dan 4096 VM's per VNET.
* Het aantal VM's dat u kunt maken, is beperkt tot de kernquota van de regio waarin u ze implementeert. Mogelijk moet u contact opnemen met de klantenservice om uw limiet voor rekenquota te verhogen, zelfs als u nu een hoge limiet voor het aantal cores voor gebruik met cloudservices of IaaS v1 hebt. Als u uw quota wilt opvragen, kunt u de volgende Azure CLI-opdracht uitvoeren: `azure vm list-usage`, en de volgende PowerShell-opdracht: `Get-AzureRmVMUsage` (als u een versie van PowerShell lager dan 1.0 gebruikt, gebruik dan `Get-AzureVMUsage`).

## <a name="vm-scale-set-frequently-asked-questions"></a>Veelgestelde vragen over VM-schaalsets
**V:** Hoeveel VM's kan een VM-schaalset bevatten?

**A:** Met beheerde schijven kan uw setschaal tussen 0 en 1000 virtuele machines op basis van platforminstallatiekopieën bevatten of 0-100 virtuele machines gebaseerd op aangepaste installatiekopieën. Voor niet-beheerde schijven (waarbij u uw eigen opslagaccounts definieert) is de limiet is 100 virtuele machines voor platforminstallatiekopieën en 40 voor aangepaste installatiekopieën (als de eigenschap *overprovision* is ingesteld op 'false', is de limiet standaard 20), omdat aangepaste installatiekopieën met niet-beheerde schijven zijn beperkt tot één opslagaccount.

**V:** Worden gegevensschijven binnen VM-schaalsets ondersteund?

**A:** Ja. Een schaalset die is gedefinieerd met beheerde opslag, kan een configuratie voor gekoppelde gegevensstations definiëren die op alle virtuele machines in de set wordt toegepast. Schaalsets die niet zijn gedefinieerd met beheerde opslag hebben geen gekoppelde gegevensstations. Andere opties voor het opslaan van gegevens zijn:

* Azure-bestanden (gedeelde SMB-stations)
* Station van het besturingssysteem
* Tijdelijk station (lokaal, niet ondersteund door Azure Storage)
* Azure-gegevensservice (bijvoorbeeld Azure-tabellen, Azure-blobs)
* Externe gegevensservice (bijvoorbeeld externe database)

**V:** Welke Azure-regio's ondersteunen VM-schaalsets?

**A:** Alle regio's ondersteunen VM-schaalsets.

**V:** Hoe maak ik een VM-schaalset met behulp van een aangepaste installatiekopie?

**A:** Laat de eigenschap vhdContainers leeg (of achterwege) en geef de URI van de eigenschappen van de installatiekopie op. Bijvoorbeeld: [201-vmss-windows-customimage].(https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-windows-customimage)


**V:** Als ik de capaciteit van mijn schaalset verlaag van 20 naar 15, welke VM's worden er dan verwijderd?

**A:** Virtuele machines worden gelijkmatig uit upgradedomeinen en foutdomeinen van de schaalset verwijderd om de beschikbaarheid te maximaliseren. De VM's met de hoogste id's worden het eerst verwijderd.

**V:** Wat gebeurt er dan als ik de capaciteit verhoog van 15 naar 18?

**A:** Als u de capaciteit verhoogd naar 18, worden er 3 nieuwe VM's gemaakt. De id van elke VM-instantie wordt oplopend gegenereerd vanaf de vorige hoogste waarde (bijvoorbeeld 20, 21, 22). De VM's worden verdeeld over upgrade- en foutdomeinen.

**V:** Kan ik een uitvoeringsvolgorde toepassen wanneer ik meerdere extensies in een VM-schaalset gebruik?

**A:** Niet direct, maar bij de CustomScript-extensie kunt u uw script laten wachten op de voltooiing van een andere extensie ([bijvoorbeeld door het extensielogboek te controleren](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vmss-lapstack-autoscale/install_lap.sh)). Meer informatie over de uitvoeringsvolgorde van extensies vindt u in dit blogartikel: [Extensievolgorde bij VM-schaalsets in Azure](https://msftstack.wordpress.com/2016/05/12/extension-sequencing-in-azure-vm-scale-sets/).

**V:** Maken VM-schaalsets gebruik van beschikbaarheidssets van Azure?

**A:** Ja. Een VM-schaalset is een impliciete beschikbaarheidsset met vijf foutdomeinen en vijf upgradedomeinen. U hoeft onder virtualMachineProfile niets te configureren. VM-schaalsets met meer dan 100 virtuele machines omvatten meerdere 'plaatsingsgroepen' die gelijkwaardig aan meerdere beschikbaarheidssets zijn. Een beschikbaarheidsset van virtuele machines kan bestaan in hetzelfde VNET als een schaalset van virtuele machines. Een veelvoorkomende configuratie is om beheerknooppunt-VM's, waarvoor vaak unieke configuratie in de beschikbaarheidsset nodig is, en gegevensknooppunten in de schaalset te zetten.



<!--HONumber=Feb17_HO1-->


