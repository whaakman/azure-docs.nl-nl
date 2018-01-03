---
title: Problemen met automatisch schalen met virtuele-Machineschaalsets | Microsoft Docs
description: Problemen oplossen met virtuele-Machineschaalsets voor automatisch schalen. Begrijpen typische problemen en hoe u deze kunt oplossen.
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 11/16/2017
ms.author: negat
ms.openlocfilehash: 02a3acf818bfca31a56b364f7abab97551e0d3f0
ms.sourcegitcommit: f46cbcff710f590aebe437c6dd459452ddf0af09
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Het oplossen van problemen met virtuele-Machineschaalsets voor automatisch schalen
**Probleem** : u hebt gemaakt een infrastructuur voor automatisch schalen in Azure Resource Manager met behulp van de virtuele-machineschaalsets – bijvoorbeeld door het implementeren van een sjabloon zoals deze: https://github.com/Azure/azure-quickstart-templates/tree/master/201- vmss-bottle-automatisch schalen: u hebt uw scale-regels gedefinieerd en werkt geweldig, behalve ongeacht hoeveel belasting u op de virtuele machines, dat niet het geval automatisch schalen.

## <a name="troubleshooting-steps"></a>Stappen voor het oplossen van problemen
Enkele overwegingen zijn onder andere:

* Hoeveel Vcpu elke virtuele machine heeft en u elke vCPU laden?
  Het voorgaande voorbeeld Azure Quickstart-sjabloon is een script do_work.php, die een enkele vCPU wordt geladen. Als u een virtuele machine die groter is dan een VM-grootte voor één vCPU, zoals Standard_A1 of D1 gebruikt, moet u zou deze laadbewerking meerdere keren uitvoeren. Controleren hoeveel vcpu's voor uw virtuele machines aan de hand van [grootten voor Windows virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Hoeveel virtuele machines in de virtuele-machineschaalset gaat het werk voor elk criterium?
  
    Een uitbreidbare gebeurtenis alleen vindt plaats wanneer de gemiddelde CPU over **alle** de virtuele machines in een set scale overschrijdt de drempelwaarde, via de interne tijd gedefinieerd in de regels voor automatisch schalen.
* Hebt u geen gebeurtenissen scale hoofd gezien?
  
    Controleer dat de auditlogboeken in de Azure portal voor scale-gebeurtenissen. Misschien er is een schaal en een schaal dat is overgeslagen. U kunt filteren op 'Schaal'.
  
    ![Controlelogboeken][audit]
* De schaal in en scale-out drempelwaarden voldoende verschillend zijn?
  
    Stel dat u een regel moet worden uitgebreid gemiddelde CPU is groter dan 50% gedurende vijf minuten en schaal in wanneer de gemiddelde CPU minder dan 50% is ingesteld. Deze instelling kan een 'flapping' probleem zou ontstaan als CPU-gebruik bijna de drempelwaarde, met de schaalacties voortdurend vergroten of verkleinen van de grootte van de set. Vanwege deze instelling kan probeert de service automatisch schalen om te voorkomen dat 'op en neer', die licht kunnen als niet schalen. Zorg er daarom dat de drempelwaarden voor scale-out en de schaal verschillen voldoende ruimte vrij Between schalen zodat.
* U uw eigen JSON-sjabloon schrijft?
  
    Het is gemakkelijk fouten maken, dus beginnen met een sjabloon zoals hierboven die is bewezen werken en klein incrementele wijzigingen aanbrengen. 
* Kunt u handmatig schalen in- of?
  
    Probeer het opnieuw distribueren van die de virtuele-machineschaalset resource ingesteld met een andere 'capaciteit' instellen voor het aantal virtuele machines handmatig wijzigen. Een van de voorbeeldsjabloon is hier: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – mogelijk moet u de sjabloon om ervoor te zorgen dat deze dezelfde machinegrootte heeft, zoals maakt gebruik van uw Schaalset bewerken. Als u het aantal virtuele machines is handmatig wijzigen kunt, kent u vervolgens dat het probleem is geïsoleerd, zodat automatisch schalen.
* Controleer uw Microsoft.Compute/virtualMachineScaleSet en Microsoft.Insights bronnen in de [Azure Resource Explorer](https://resources.azure.com/)
  
    De Azure Resourceverkenner is een onmisbaar voor probleemoplossing hulpprogramma dat u de status van uw Azure Resource Manager-resources toont. Klik op uw abonnement en kijk in de resourcegroep die u wilt oplossen. Bekijk de virtuele-machineschaalset u gemaakt en controleer of de weergave-exemplaar, waarin u de status van een implementatie onder de Compute-resourceprovider. Controleer ook de instantieweergave van virtuele machines in de virtuele-machineschaalset. Vervolgens gaat u naar de resourceprovider Microsoft.Insights en controleer de regels voor automatisch schalen uitziet.
* Is de extensie voor diagnostische werkt en prestatiegegevens verzenden?
  
    **Update:** Azure automatisch schalen is uitgebreid voor het gebruik van een pijplijn metrische gegevens op de host, die niet langer vereist een extensie voor diagnostische gegevens worden geïnstalleerd. De volgende paragrafen zijn niet langer van toepassing als u een toepassing automatisch schalen op basis van de nieuwe pijplijn maakt. Een voorbeeld van een Azure-sjablonen die zijn geconverteerd voor gebruik van de host-pijplijn is hier beschikbaar: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Het gebruik van metrische gegevens op de host voor automatisch schalen is beter om de volgende redenen:
  
  * Minder bewegende onderdelen als er worden geen extensies diagnostische gegevens moeten worden geïnstalleerd.
  * Eenvoudiger sjablonen. NET insights automatisch schalen regels toevoegen aan een bestaande sjabloon voor scale set.
  * Betrouwbaarder rapportage en snellere starten van de nieuwe virtuele machines.
    
    De enige redenen die u wilt blijven gebruiken een extensie voor diagnostische is als u moet geheugen diagnostics reporting/schalen. Metrische gegevens op de host rapporteren niet geheugen.
    
    Met daarom alleen volgen op de rest van dit artikel als u diagnostische uitbreidingen voor uw automatisch schalen.
    
    Automatisch schalen in Azure Resource Manager kunt werken (maar niet meer heeft tot) met behulp van een virtuele machine een extensie van de extensie voor diagnostische gegevens aangeroepen. Hiermee plaatst u prestatiegegevens naar een opslagaccount dat u in de sjabloon definieert. Deze gegevens wordt vervolgens geaggregeerd met de Azure-Monitor-service.
    
    Als de Insights-service kan geen gegevens uit de virtuele machines lezen, moet deze sturen je een e-mail. Bijvoorbeeld, ophalen u een e-mailbericht als de virtuele machines zijn niet beschikbaar. Zorg ervoor dat Controleer je e-mail, op het e-mailadres dat u hebt opgegeven toen u uw Azure-account hebt gemaakt.
    
    U kunt ook zoeken op de gegevens zelf. De Azure storage-account met behulp van een cloud explorer kijken. Bijvoorbeeld, met behulp van de [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), aanmelden en het Azure-abonnement u kiest. Controleer bij de diagnostische gegevens opslagaccountnaam waarnaar wordt verwezen in de definitie van de extensie diagnostische gegevens in de implementatiesjabloon voor.
    
    ![Cloud Explorer][explorer]
    
   Ziet u een aantal tabellen waarbij de gegevens van elke virtuele machine wordt opgeslagen. Linux- en de metriek CPU als voorbeeld duurt, zoek de meest recente rijen. De Visual Studio cloud explorer ondersteunt een querytaal, zodat u kunt een query uitvoeren. Bijvoorbeeld, u kunt geen query uitvoeren voor ' tijdstempel gt datetime'2016-02-02T21:20:00Z' ' om ervoor te zorgen dat u de meest recente gebeurtenissen. De tijdzone overeenkomt met de UTC. Ondersteunt de gegevens die u ziet in er komen overeen met de scale-regels instellen? In het volgende voorbeeld wordt de CPU voor machine 20 gestart verhogen tot 100% in de afgelopen vijf minuten.
    
    ![Storage-tabellen][tables]
    
    Als de gegevens er niet in staat, wordt verwezen naar dat het probleem is met de diagnostische extensie uitgevoerd in de virtuele machines. Als de gegevens aanwezig is, betekent dit dat er is een probleem met uw scale-regels of met de Insights-service. Controleer [Azure Status](https://azure.microsoft.com/status/).
    
    Zodra u via deze stappen zijn hebt als u steeds automatisch schalen problemen nog, kunt u proberen de volgende bronnen: 
    * Lees de forums op [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), of [Stack-overloop](http://stackoverflow.com/questions/tagged/azure) 
    * Meld u aan een aanroep van ondersteuning. Zorg ervoor dat u kunt de sjabloon en een weergave van uw gegevens delen.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
