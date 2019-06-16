---
title: Gedistribueerde gezamenlijke ontwikkeling van resources in Azure DevTest Labs | Microsoft Docs
description: Bevat de aanbevolen procedures voor het instellen van een gedistribueerde en gezamenlijke ontwikkelomgeving voor het ontwikkelen van DevTest Labs-resources.
services: devtest-lab,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: d8892b2d00008c9d67f8bc28d1abb7d562dfd95c
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67079884"
---
# <a name="best-practices-for-distributed-and-collaborative-development-of-azure-devtest-labs-resources"></a>Aanbevolen procedures voor het ontwikkelen van gedistribueerde en samenwerking van Azure DevTest Labs-resources
Gedistribueerde gezamenlijke ontwikkeling kunt u verschillende teams of personen te ontwikkelen en onderhouden van een code basis. Om succesvol te zijn, is het ontwikkelingsproces afhankelijk van de mogelijkheid om te maken, delen en gegevens integreren. Dit principe belangrijke ontwikkeling kan worden gebruikt in Azure DevTest Labs. Er zijn verschillende soorten resources in een testomgeving die vaak worden verdeeld over verschillende labs in een onderneming. De verschillende soorten resources zijn gericht in twee gebieden:

- Resources die intern zijn opgeslagen in het lab (lab-indeling)
- Resources die zijn opgeslagen in [externe opslagplaatsen die zijn verbonden met het lab](devtest-lab-add-artifact-repo.md) (code op basis van-opslagplaats). 

Dit document beschrijft enkele aanbevolen procedures waarmee samenwerking en distributie tussen meerdere teams en tegelijkertijd het aanpassen en de kwaliteit op alle niveaus.

## <a name="lab-based-resources"></a>Resources op basis van een Lab

### <a name="custom-virtual-machine-images"></a>Aangepaste VM-installatiekopieën
U kunt een veelvoorkomende oorzaak van aangepaste installatiekopieën die zijn geïmplementeerd voor labs op basis van elke nacht hebben. Zie voor gedetailleerde informatie [installatiekopie factory](image-factory-create.md).    

### <a name="formulas"></a>Formules
[Formules](devtest-lab-manage-formulas.md) lab-specifieke zijn en niet een distributiemechanisme voor. De leden van het lab doen de ontwikkeling van formules. 

## <a name="code-repository-based-resources"></a>Code-opslagplaats op basis van resources
Er zijn twee verschillende functies die zijn gebaseerd op code-opslagplaatsen, artefacten en omgevingen. In dit artikel gaat over de functies en hoe u het meest effectief instellen van opslagplaatsen en werkstroom om toe te staan de mogelijkheden voor het aanpassen van de beschikbare artefacten en omgevingen op het organisatieniveau of team.  Deze werkstroom is gebaseerd op standaard [broncode beheren vertakkingen strategie](/devops/repos/tfvc/branching-strategies-with-tfvc?view=azure-devops). 

### <a name="key-concepts"></a>Belangrijkste concepten
De brongegevens voor artefacten bevat metagegevens, scripts. De brongegevens voor omgevingen bevat metagegevens en Resource Manager-sjablonen met ondersteunende bestanden, zoals PowerShell-scripts, DSC-scripts, Zip-bestanden, enzovoort.  

### <a name="repository-structure"></a>Structuur van de opslagplaats  
De meest voorkomende configuratie voor broncodebeheer (SCC), is het instellen van een structuur met meerdere lagen voor het opslaan van codebestanden (Resource Manager-sjablonen, metagegevens en scripts) die worden gebruikt voor het in de labs. Specifiek, opslagplaatsen voor het opslaan van de resources die worden beheerd door de verschillende niveaus van het bedrijf maken:   

- Resources voor het hele bedrijf.
- Zakelijke eenheid/deling-wide bronnen
- Team-specifieke resources.

Elk van deze niveaus koppelen aan een andere opslagplaats waar de master-vertakking is vereist om te worden van de kwaliteit van de productie. De [vertakkingen](/devops/repos/git/git-branching-guidance?view=azure-devops) in elke opslagplaats zou zijn voor de ontwikkeling van deze specifieke resources (artefacten of sjablonen). Deze structuur goed wordt uitgelijnd met DevTest Labs zoals u meerdere opslagplaatsen en meerdere vertakkingen gemakkelijk op hetzelfde moment tot labs van de organisatie verbinden kunt. Naam van de opslagplaats is opgenomen in de gebruikersinterface (UI) om verwarring te voorkomen wanneer er dezelfde naam, beschrijving en uitgever.
     
Het volgende diagram toont twee opslagplaatsen: een bedrijf-opslagplaats die wordt beheerd door de IT-afdeling en de opslagplaats van een deling onderhouden door de afdeling R & D.

![Een voorbeeld distributieve en gezamenlijke ontwikkelomgeving](./media/best-practices-distributive-collaborative-dev-env/distributive-collaborative-dev-env.png)
   
Deze gelaagde structuur profiteert u van ontwikkeling, terwijl een hogere kwaliteit op de master-vertakking met meerdere opslagplaatsen die zijn verbonden met een testomgeving kunt u voor nog meer flexibiliteit.

## <a name="next-steps"></a>Volgende stappen    
Zie de volgende artikelen:

- Een opslagplaats toevoegen aan een lab met behulp van de [Azure-portal](devtest-lab-add-artifact-repo.md) of via [Azure Resource Manager-sjabloon](add-artifact-repository.md)
- [DevTest Labs-artefacten](devtest-lab-artifact-author.md)
- [DevTest Labs-omgevingen](devtest-lab-create-environment-from-arm.md).
