---
title: Problemen oplossen voor automatisch schalen met Virtual Machine Scale Sets | Microsoft Docs
description: Problemen met automatisch schalen met Virtual Machine Scale Sets. Informatie over veelvoorkomende problemen aangetroffen en hoe u ze op te lossen.
services: virtual-machine-scale-sets
documentationcenter: ''
author: mayanknayar
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: manayar
ms.openlocfilehash: 3308b22606e87853aad7e3d3a3995aab8d1b5401
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58005313"
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Oplossen van problemen met automatisch schalen met Virtual Machine Scale Sets
**Probleem** : u hebt gemaakt een infrastructuur voor automatisch schalen in Azure Resource Manager met behulp van de virtuele-machineschaalsets, bijvoorbeeld door het implementeren van een sjabloon zoals deze: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale : u hebt uw schaalregels die zijn gedefinieerd en deze kan uitstekend, met uitzondering van Nee hoeveel belasting van de virtuele machines uit, dat niet het geval voor automatisch schalen.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
Er zijn enkele aandachtspunten voor:

* Het aantal vcpu's heeft elke virtuele machine, en u elke vCPU laden?
  Het voorgaande voorbeeld Azure Quickstart-sjabloon is een script do_work.php, die een enkele vCPU wordt geladen. Als u een virtuele machine die groter zijn dan een enkele vCPU VM-grootte, zoals standaard_a1 of D1, moet u deze belasting meerdere keren uitvoeren. Controleert het aantal vcpu's voor uw VM's aan de hand [grootten voor Windows virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Hoeveel virtuele machines in de virtuele-machineschaalset, gaat het werk voor elke service?
  
    Een uitbreidbare gebeurtenis alleen vindt plaats wanneer de gemiddelde CPU voor **alle** de virtuele machines in een schaalset is groter dan de drempelwaarde, na verloop van de interne tijd gedefinieerd in de regels voor automatisch schalen.
* Hebt u alle schaalgebeurtenissen gemist?
  
    Raadpleeg dat de auditlogboeken beschikbaar zijn in de Azure-portal voor schaalgebeurtenissen. Mogelijk is er een schaal omhoog en omlaag die een schalen is overgeslagen. U kunt filteren op 'Schaal'.
  
    ![Controlelogboeken][audit]
* Zijn uw schaal en scale-out drempels lijkt te veel?
  
    Stel dat u een regel om uit te schalen wanneer Gemiddeld CPU groter is dan 50% meer dan vijf minuten en op schaal in wanneer Gemiddeld CPU is minder dan 50% instellen. Deze instelling wordt een "op en neer" probleem veroorzaken wanneer CPU-gebruik is dicht bij de drempelwaarde met schaalacties voortdurend vergroten of verkleinen van de grootte van de set. Vanwege deze instelling kan probeert de service voor automatisch schalen om te voorkomen dat 'op en neer', die u kunt het manifest als niet schalen. Zorg daarom dat uw scale-out en schaal drempelwaarden zijn lijkt te veel om toe te staan enkele ruimte tussen de schaal.
* U uw eigen JSON-sjabloon voor schrijven?
  
    Het is eenvoudig voor fouten, dus beginnen met een sjabloon, zoals hierboven die is een bewezen oplossing om te werken en kleine incrementele wijzigingen aanbrengen. 
* Kunt u handmatig opschalen in of uit?
  
    Probeer het opnieuw distribueren van de virtuele-resource met een andere 'capaciteit machineschaalset' instellen op het aantal virtuele machines handmatig wijzigen. Een van de voorbeeldsjabloon is hier: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing : mogelijk moet u de sjabloon om te controleren of deze heeft dezelfde grootte van de machine omdat uw Schaalset gebruikmaakt van bewerken. Als u het aantal virtuele machines is handmatig wijzigen kunt, weet u vervolgens dat het probleem is geïsoleerd voor automatisch schalen.
* Controleer uw Microsoft.Compute/virtualMachineScaleSet en Microsoft.Insights resources in de [Azure Resource Explorer](https://resources.azure.com/)
  
    Azure Resource Explorer is een onmisbaar hulpprogramma waarin u de status van uw Azure Resource Manager-resources voor probleemoplossing. Klik op uw abonnement en kijken naar de resourcegroep die u wilt oplossen. Bekijk de virtuele-machineschaalset u gemaakt en controleer of de weergave-exemplaar, waarin u de status van een implementatie onder de Compute-resourceprovider. Controleer ook of de instantieweergave van virtuele machines in de virtuele-machineschaalset. Vervolgens gaat u naar de resourceprovider Microsoft.Insights en controleer de regels voor automatisch schalen uitziet.
* Is de diagnostische extensie werken en prestatiegegevens verzenden?
  
    **Update:** Automatisch schalen van Azure is uitgebreid voor het gebruik van een pijplijn hostgebaseerde metrische gegevens, die niet langer vereist een extensie voor diagnostische gegevens worden geïnstalleerd. De volgende alinea zijn niet langer van toepassing als u een automatisch schalende toepassing met behulp van de nieuwe pijplijn maakt. Een voorbeeld van Azure-sjablonen die zijn geconverteerd voor het gebruik van de pijplijn host is hier beschikbaar: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Met behulp van hostgebaseerde metrische gegevens voor automatisch schalen is het beter om de volgende redenen:
  
  * Minder bewegende onderdelen als er worden geen extensies diagnostische gegevens moeten worden geïnstalleerd.
  * Eenvoudiger sjablonen. Alleen inzichten voor automatisch schalen regels toevoegen aan een bestaande sjabloon voor schaalsets.
  * Melden van betrouwbaarder en sneller starten van de nieuwe virtuele machines.
    
    De enige redenen die u wilt blijven gebruiken een diagnostische extensie is als u geheugen diagnostische gegevens over reporting/schalen. Hostgebaseerde metrische gegevens rapporteren niet geheugen.
    
    Met die in gedachten, alleen volgt u de rest van dit artikel als u diagnostische extensies voor het automatisch schalen.
    
    Automatisch schalen in Azure Resource Manager kan worden gebruikt (maar niet meer heeft tot) met behulp van een virtuele machine een extensie van de extensie voor diagnostische gegevens aangeroepen. Het verzendt gegevens naar een opslagaccount dat u in de sjabloon definieert. Deze gegevens vervolgens gesorteerd op basis van de Azure Monitor-service.
    
    Als de Insights-service kan geen gegevens uit de virtuele machines lezen, wordt het verwacht u een e-mail verzenden. Bijvoorbeeld, ontvangt u een e-mail als de virtuele machines niet beschikbaar zijn. Zorg dat uw e-mail, op het e-mailadres dat u hebt opgegeven tijdens het maken van uw Azure-account te controleren.
    
    U kunt ook zoeken op de gegevens zelf. Bekijk de Azure storage-account met behulp van een cloud explorer in. Bijvoorbeeld, met behulp van de [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), log in en kies het Azure-abonnement u gebruikt. Bekijk vervolgens de diagnostische gegevens over naam van het opslagaccount waarnaar wordt verwezen in de definitie van de extensie voor diagnostische gegevens in uw sjabloon voor de implementatie.
    
    ![Cloud Explorer][explorer]
    
    U ziet een aantal tabellen waarbij de gegevens van elke virtuele machine worden opgeslagen. Linux- en de metriek CPU als voorbeeld nemen, zoek de meest recente rijen. De Visual Studio cloud explorer ondersteunt een querytaal, zodat u een query kunt uitvoeren. Bijvoorbeeld, u kunt geen query uitvoeren voor ' tijdstempel gt datum/tijd ' 2016-02-02T21:20:00Z' ' om ervoor te zorgen dat u de meest recente gebeurtenissen. De tijdzone komt overeen met de UTC. Voert de gegevens die u ziet in er komen overeen met de schaalregels instellen? In het volgende voorbeeld wordt de CPU voor machine 20 gestart verhogen tot 100% in de afgelopen vijf minuten.
    
    ![Storage-tabellen][tables]
    
    Als de gegevens niet aanwezig, impliceert dat het probleem is met de diagnostische extensie die wordt uitgevoerd in de virtuele machines. Als de gegevens aanwezig is, impliceert dat er is een probleem met uw schaalregels of met de Insights-service. Controleer [Azure Status](https://azure.microsoft.com/status/).
    
    Nadat u tijdens de volgende stappen zijn hebt als u nog steeds problemen met automatisch schalen, kunt u proberen de volgende bronnen: 
    * De forums lezen op [MSDN](https://social.msdn.microsoft.com/forums/azure/home?forum=WAVirtualMachinesforWindows), of [stackoverloop](https://stackoverflow.com/questions/tagged/azure) 
    * Meld u aan een telefoongesprek met de ondersteuning. Worden voorbereid om de sjabloon en een weergave van uw prestatiegegevens te delen.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
