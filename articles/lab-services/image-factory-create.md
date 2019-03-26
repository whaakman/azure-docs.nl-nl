---
title: De fabrieksinstellingen van een installatiekopie maken in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over het maken van een aangepaste installatiekopie-factory in Azure DevTest Labs.
services: devtest-lab, lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/25/2019
ms.author: spelluru
ms.openlocfilehash: c7923282396481dcfd6aa19b75d110fa03e50931
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58439820"
---
# <a name="create-a-custom-image-factory-in-azure-devtest-labs"></a>De fabrieksinstellingen van een aangepaste installatiekopie maken in Azure DevTest Labs
Dit artikel leest u over het instellen van de fabrieksinstellingen van een aangepaste installatiekopie met behulp van voorbeelden van scripts beschikbaar in de [Git-opslagplaats](https://github.com/Azure/azure-devtestlab/tree/master/Scripts/ImageFactory).

## <a name="whats-an-image-factory"></a>Wat is een installatiekopie factory?
Een afbeelding factory is een configuratie als code-oplossing die u bouwt en distribueert afbeeldingen automatisch met de gewenste configuraties op een geregelde. De afbeeldingen in de fabriek installatiekopie altijd up-to-date zijn en het voortdurende onderhoud is bijna nul wanneer het hele proces is geautomatiseerd. En omdat de vereiste configuraties al in de afbeelding, bespaart u de tijd van het systeem handmatig configureren nadat een virtuele machine is gemaakt met het Basisbesturingssysteem.

De aanzienlijke accelerator om op te halen van een ontwikkelaar bureaublad naar een status gereed in DevTest Labs maakt gebruik van aangepaste installatiekopieën. Het nadeel van aangepaste installatiekopieën is dat er iets extra te houden in het lab. Bijvoorbeeld, proefversies van producten verlopen na verloop van tijd (of) nieuw uitgebrachte beveiligingsupdates worden niet toegepast, die we de aangepaste installatiekopie periodiek vernieuwen forceren. Met een installatiekopie fabriek hebt u een definitie van de installatiekopie van het code-besturingselement voor de gegevensbron en een geautomatiseerd proces voor het produceren van aangepaste installatiekopieën op basis van de definitie ingecheckt.

De oplossing kunt de snelheid van het maken van virtuele machines vanuit aangepaste installatiekopieën geen kosten voor extra voortdurende onderhoud meer nodig. Met deze oplossing kunt u automatisch maken van aangepaste installatiekopieën, deze toewijzen aan andere DevTest Labs en buiten gebruik stellen van de oude afbeeldingen. In de volgende video leert u over de factory afbeelding, en hoe deze geïmplementeerd met DevTest Labs.  Alle Azure Powershell-scripts zijn vrij beschikbaar is en zich hier: [ http://aka.ms/dtlimagefactory ](http://aka.ms/dtlimagefactory).

<br/>

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Custom-Image-Factory-with-Azure-DevTest-Labs/player]


## <a name="high-level-view-of-the-solution"></a>Weergave op hoog niveau van de oplossing
De oplossing kunt de snelheid van het maken van virtuele machines vanuit aangepaste installatiekopieën geen kosten voor extra voortdurende onderhoud meer nodig. Met deze oplossing kunt u automatisch maken van aangepaste installatiekopieën en distribueer ze naar andere DevTest Labs. U kunt Azure DevOps (voorheen Visual Studio Team Services) gebruikt als de orchestration-engine voor het automatiseren van alle bewerkingen in de DevTest Labs.

![Weergave op hoog niveau van de oplossing](./media/create-image-factory/high-level-view-of-solution.png)

Er is een [VSTS-extensie voor DevTest Labs](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) waarmee u kunt afzonderlijke stappen uitvoeren: 

- Aangepaste installatiekopie maken
- VM maken
- Virtuele machine verwijderen
- Omgeving maken
- Omgeving verwijderen
- Omgeving vullen

Met de extensie DevTest Labs is een eenvoudige manier om te beginnen met het automatisch maken van aangepaste installatiekopieën in DevTest Labs.

Er is een alternatieve implementatie met behulp van PowerShell-script voor een complexere scenario. Met behulp van PowerShell, kunt u volledig automatiseren de fabrieksinstellingen van een installatiekopie op basis van DevTest Labs die kunnen worden gebruikt in uw continue integratie en continue levering (CI/CD) hulpprogrammaketen. De principes die worden gevolgd in deze alternatieve oplossing zijn:

- Algemene updates moeten geen wijzigingen aangebracht in de fabriek installatiekopie vereisen. (bijvoorbeeld verhoging van een nieuw type aangepaste afbeelding, automatisch buiten gebruik stellen van oude afbeeldingen toe te voegen een nieuwe 'eindpunt' DevTest Labs te ontvangen van aangepaste installatiekopieën, enzovoort.)
- Algemene wijzigingen worden ondersteund door broncodebeheer (infrastructuur als code)
- DevTest Labs ontvangen van aangepaste installatiekopieën mogelijk niet in hetzelfde Azure-abonnement (labs omvatten abonnementen)
- PowerShell-scripts moeten worden ingezet, zodat we aanvullende fabrieken instellen kunt indien nodig

## <a name="next-steps"></a>Volgende stappen
Naar het volgende artikel in deze sectie gaan: [Een gegevensfactory installatiekopie uitvoeren vanaf Azure DevOps](image-factory-set-up-devops-lab.md)
