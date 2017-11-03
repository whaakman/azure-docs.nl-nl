---
title: Problemen met automatisch schalen met virtuele-Machineschaalsets | Microsoft Docs
description: Problemen oplossen met virtuele-Machineschaalsets voor automatisch schalen. Begrijpen typische problemen en hoe u deze kunt oplossen.
services: virtual-machine-scale-sets
documentationcenter: 
author: gbowerman
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: c7d87b72-ee24-4e52-9377-a42f337f76fa
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: windows
ms.devlang: na
ms.topic: article
ms.date: 10/28/2016
ms.author: guybo
ms.openlocfilehash: bd45a0fb99a77851aa7b91d23bd4b830b6f5cc7b
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="troubleshooting-autoscale-with-virtual-machine-scale-sets"></a>Het oplossen van problemen met virtuele-Machineschaalsets voor automatisch schalen
**Probleem** : u hebt gemaakt een infrastructuur voor automatisch schalen in Azure Resource Manager met behulp van de VM-Schaalsets – bijvoorbeeld door het implementeren van een sjabloon als volgt: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale: u hebt uw scale-regels gedefinieerd en werkt geweldig, behalve dat ongeacht hoeveel belasting u op de virtuele machines, het automatisch schalen won't.

## <a name="troubleshooting-steps"></a>Stappen voor probleemoplossing
Enkele overwegingen zijn onder andere:

* Het aantal kernen heeft elke virtuele machine en laadt u elke core?
  Het bovenstaande voorbeeld Azure Quickstart sjabloon heeft een script do_work.php, die één kern wordt geladen. Als u een virtuele machine die groter is dan één kern VM-grootte zoals Standard_A1 of D1 moet u deze laadbewerking meerdere keren uitvoeren. Controleren hoeveel cores uw virtuele machines aan de hand van [grootten voor Windows virtuele machines in Azure](../virtual-machines/windows/sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* Hoeveel virtuele machines in de VM-Schaalset, gaat het werk voor elk criterium?
  
    Een scale-out gebeurtenis worden pas plaats wanneer de gemiddelde CPU over **alle** de virtuele machines in een set scale overschrijdt de drempelwaarde, via de interne tijd gedefinieerd in de regels voor automatisch schalen.
* Hebt u geen gebeurtenissen scale hoofd gezien?
  
    Controleer dat de auditlogboeken in de Azure portal voor scale-gebeurtenissen. Misschien er is een schaal en een schaal omlaag dat is overgeslagen. U kunt filteren op 'Schaal'...
  
    ![Controlelogboeken][audit]
* De schaal in en scale-out drempelwaarden voldoende verschillend zijn?
  
    Stel dat u een regel moet worden uitgebreid gemiddelde CPU is groter dan 50% meer dan 5 minuten en schaal in wanneer de gemiddelde CPU minder dan 50% is ingesteld. Hierdoor zou een probleem 'flapping' als het CPU-gebruik bijna deze drempelwaarde, met de schaalacties voortdurend vergroten of verkleinen van de grootte van de set. Als gevolg hiervan probeert de service automatisch schalen om te voorkomen dat 'op en neer', die licht kunnen als niet schalen. Controleer daarom dat de drempelwaarden voor scale-out en de schaal verschillen voldoende ruimte vrij Between schalen zodat.
* U uw eigen JSON-sjabloon schrijft?
  
    Het is gemakkelijk fouten maken, dus beginnen met een sjabloon zoals hierboven die is bewezen werken en klein incrementele wijzigingen aanbrengen. 
* Kunt u handmatig schalen in- of?
  
    Probeer het opnieuw distribueren van de VM-Schaalset resource met een andere 'capaciteit' instelling het aantal virtuele machines handmatig wijzigen. Een van de voorbeeldsjabloon om dit te doen is hier: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-scale-existing – mogelijk moet u de sjabloon om ervoor te zorgen dat deze dezelfde machinegrootte heeft, zoals uw Schaalset maakt gebruik van bewerken. Als u het aantal virtuele machines is handmatig wijzigen kunt, weet u dat het probleem is geïsoleerd, zodat automatisch schalen.
* Controleer uw Microsoft.Compute/virtualMachineScaleSet en Microsoft.Insights bronnen in de [Azure Resource Explorer](https://resources.azure.com/)
  
    Dit is een onmisbaar voor probleemoplossing hulpmiddel waarin u de status van uw Azure Resource Manager-resources kunt zien. Klik op uw abonnement en kijk in de resourcegroep die u wilt oplossen. Onder de resourceprovider voor Compute kijken naar de VM-Schaalset u hebt gemaakt en de exemplaar-weergave, waarin u de status van een implementatie controleren. Controleer ook de instantieweergave van virtuele machines in de VM-Schaalset. Vervolgens gaat u naar de resourceprovider Microsoft.Insights en controleer dat de regels voor automatisch schalen goed.
* Is de extensie voor diagnostische werkt en prestatiegegevens verzenden?
  
    **Update:** Azure automatisch schalen is uitgebreid voor het gebruik van een host gebaseerde metrische gegevens pijplijn die niet langer vereist een extensie voor diagnostische gegevens worden geïnstalleerd. Dit betekent dat de volgende die paragrafen zijn niet langer van toepassing als u een toepassing automatisch schalen op basis van de nieuwe pijplijn maakt. Een voorbeeld van Azure-sjablonen die zijn geconverteerd naar de host-pijplijn gebruiken is: https://github.com/Azure/azure-quickstart-templates/tree/master/201-vmss-bottle-autoscale. 
  
    Met behulp van de host op basis van metrische gegevens voor automatisch schalen is het beter om de volgende redenen:
  
  * Minder bewegende onderdelen als er worden geen extensies diagnostische gegevens moeten worden geïnstalleerd.
  * Eenvoudiger sjablonen. NET insights automatisch schalen regels toevoegen aan een bestaande sjabloon voor scale set.
  * Betrouwbaarder rapportage en snellere starten van de nieuwe virtuele machines.
    
    De enige redenen die u wilt blijven gebruiken een extensie voor diagnostische zou zijn als u geheugen diagnostics reporting/schalen moet. Host op basis van metrische gegevens rapporteren niet geheugen.
    
    Met daarom alleen volgen op de rest van dit artikel als u nog steeds diagnostische uitbreidingen voor uw automatisch schalen.
    
    Automatisch schalen in Azure Resource Manager kunt werken (maar niet meer heeft tot) met behulp van een virtuele machine een extensie van de extensie voor diagnostische gegevens aangeroepen. Hiermee plaatst u prestatiegegevens naar een opslagaccount dat u in de sjabloon definieert. Deze gegevens wordt vervolgens geaggregeerd met de Azure-Monitor-service.
    
    Als de Insights-service kan geen gegevens uit de virtuele machines lezen, moet deze sturen je een e-mail: bijvoorbeeld als de virtuele machines zijn niet actief, dus Controleer uw e-mailadres (de computer hebt opgegeven toen u het Azure-account).
    
    U kunt ook gaan en bekijk de gegevens zelf. De Azure storage-account met behulp van een cloud explorer kijken. Bijvoorbeeld met behulp van de [Visual Studio Cloud Explorer](https://visualstudiogallery.msdn.microsoft.com/aaef6e67-4d99-40bc-aacf-662237db85a2), aanmelden en kies het Azure-abonnement u en de diagnostische gegevens voor de opslagaccountnaam waarnaar wordt verwezen in de definitie van de extensie diagnostische gegevens in uw implementatiesjabloon...
    
    ![Cloud Explorer][explorer]
    
    Hier ziet u een aantal tabellen waarbij de gegevens van elke virtuele machine wordt opgeslagen. Linux- en de metriek CPU als voorbeeld duurt, zoek de meest recente rijen. De Visual Studio cloud explorer biedt ondersteuning voor een querytaal zodat u kunt een query zoals uitvoeren ' tijdstempel gt datetime'2016-02-02T21:20:00Z' ' om ervoor te zorgen dat u de meest recente gebeurtenissen (uitgaan van tijd in UTC). Ondersteunt de gegevens die u ziet in er komen overeen met de scale-regels instellen? In het onderstaande voorbeeld is de CPU voor machine 20 gestart verhogen tot 100% gedurende de laatste vijf minuten...
    
    ![Storage-tabellen][tables]
    
    Als de gegevens niet er, vervolgens wordt verwezen naar dat het probleem is met de diagnostische extensie uitgevoerd in de virtuele machines. Als de gegevens aanwezig is, betekent dit dat er is een probleem met uw scale-regels of met de Insights-service. Controleer [Azure Status](https://azure.microsoft.com/status/).
    
    Zodra u via deze stappen zijn hebt als u steeds automatisch schalen problemen nog kunt u de forums op [MSDN](https://social.msdn.microsoft.com/forums/azure/home?category=windowsazureplatform%2Cazuremarketplace%2Cwindowsazureplatformctp), of [Stack-overloop](http://stackoverflow.com/questions/tagged/azure), of in te loggen van een aanroep van ondersteuning. Wees voorbereid om u te delen van de sjabloon en een weergave van de prestatiegegevens.

[audit]: ./media/virtual-machine-scale-sets-troubleshoot/image3.png
[explorer]: ./media/virtual-machine-scale-sets-troubleshoot/image1.png
[tables]: ./media/virtual-machine-scale-sets-troubleshoot/image4.png
