---
title: Integratie van Azure DevTest Labs en DevOps | Microsoft Docs
description: Informatie over het gebruik van labs van Azure DevTest Labs binnen een continue integratie (CI) / continue levering (CD) in een bedrijfsomgeving pijplijnen.
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2019
ms.author: spelluru
ms.openlocfilehash: 62c44bfea28d47d7c32aa7ef440a40d45c314683
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67078922"
---
# <a name="integration-of-azure-devtest-labs-and-azure-devops"></a>Integratie van Azure DevTest Labs en Azure DevOps
DevOps is een methodologie voor het ontwikkelen van software die softwareontwikkeling (Dev) kan worden geïntegreerd met bewerkingen (Ops) voor een systeem. Dit systeem mogelijk nieuwe functies, updates en oplossingen leveren in overeenstemming met de bedrijfsdoelen. Deze methodologie omvat alles van het ontwerpen van nieuwe functies op basis van de doelen, gebruikspatronen en feedback van klanten; oplossen, herstellen en beperken van het systeem wanneer er problemen optreden. Een eenvoudig geïdentificeerde onderdeel van deze methodologie is de continue integratie (CI) / pipeline voor continue levering (CD). Een CI/CD-pijplijn wordt informatie, code en bronnen van een doorvoer via een reeks stappen, waaronder het ontwikkelen, testen en implementeren voor het produceren van het systeem. In dit artikel richt zich op verschillende manieren effectief labs binnen een pijplijn gebruikt in een bedrijfsomgeving. 

## <a name="benefits-of-using-labs-in-devops-workflow"></a>Voordelen van het gebruik van labs in de DevOps-werkstroom 

### <a name="focused-access"></a>Gerichte toegang 
Als een onderdeel met behulp van een lab, kunt een specifieke ecosysteem om te koppelen aan een beperkte groep gebruikers. Normaal gesproken heeft een team of groep die in een algemene gebied of een specifieke functie werkt een lab aan hen toegewezen.   

### <a name="infrastructure-replication-in-the-cloud"></a>Replicatie van de infrastructuur in de cloud 
Een ontwikkelaar kan snel instellen van een ecosysteem van ontwikkeling met een DEV-machine met broncode en hulpprogramma's. Ontwikkelaars kunnen ook een omgeving die is bijna identiek aan de productieconfiguratie maken. Dit helpt bij de ontwikkeling sneller binnenste lus. 

### <a name="pre-production"></a>Pre-productie 
Een lab in de CI/CD-pijplijn met maakt het gemakkelijker om meerdere verschillende Pre-productie-omgevingen of machines die worden uitgevoerd op hetzelfde moment voor het testen van asynchrone. Ondersteuning voor verschillende-infrastructuren zoals de build-agents kunnen worden geïmplementeerd en beheerd in een testomgeving. 

## <a name="devops-with-devtest-labs"></a>DevOps met DevTest Labs 

### <a name="development--operation"></a>Ontwikkeling / bewerking 
Een lab moet worden gericht op een team dat in een functiegebied werkt. Deze algemene focus kan voor het delen van gebied-specifieke resources, zoals de hulpprogramma's en scripts en het Resource Manager-sjablonen. Hierdoor kan sneller wijzigingen tijdens de negatieve effecten tot een kleinere groep beperken. Deze gedeelde bronnen toe dat de ontwikkelaar ontwikkeling van virtuele machines maken met de benodigde code, hulpprogramma's en configuratie. Ze kunnen worden gemaakt dynamisch of een systeem dat basisinstallatiekopieën met de aanpassingen maakt. Niet alleen kunt de ontwikkelaar van de virtuele machines maken, maar ze kunnen ook DevTest Labs-omgevingen op basis van de sjablonen die nodig zijn om te maken van de juiste Azure-resources in het lab maken. Eventuele wijzigingen of schadelijke werk kan worden gedaan op basis van de testomgeving zonder dat iemand anders. Houd rekening met het scenario waarin het product een zelfstandige system geïnstalleerd op de computer van de klant is. In dit scenario is DevTest Labs maken van VM dat omvat het installeren van extra software met behulp van artefacten en vooraf het bouwen van configuraties van de klant voor het testen van sneller binnenste lus van de code verbeterd. 
  
## <a name="cicd-pipeline"></a>CI/CD-pijplijn 
De CI/CD-pijplijn is een van de belangrijke onderdelen in DevOps verplaatsen van de code van een pull-aanvraag van de ontwikkelaar, deze kan worden geïntegreerd met de bestaande code en implementeert dit bij de productie-ecosysteem. Alle resources hoeft te worden in een lab. Bijvoorbeeld, kan een Jenkins-host worden ingesteld buiten lab als meer permanente resource. Hier volgen enkele specifieke voorbeelden van labs integreren in de pijplijn. 

### <a name="build"></a>Ontwikkelen 
De build-pijplijn is gericht op het maken van een pakket met onderdelen die samen worden getest om te worden doorgegeven aan de release-pijplijn. Labs kunnen deel uitmaken van de build-pijplijn als de locatie waar de build-agents en andere bronnen voor ondersteuning. De mogelijkheid om te bouwen dynamisch de infrastructuur kunt u meer controle. Met de mogelijkheid om meerdere omgevingen in een testomgeving hebt, kan elke build asynchroon worden uitgevoerd tijdens het gebruik van de build-ID als onderdeel van de omgevingsgegevens voor het aanduiden van de resources naar de specifieke build.   

Build-agents, mogelijkheid toegang te beperken van het testlab wordt de beveiliging en vermindert de kans op onbedoeld beschadigingen.  

### <a name="test"></a>Testen 
DevTest Labs kunt een CI/CD-pijplijn voor het automatiseren van het maken van Azure-Resource (VM's, -omgevingen) die kan worden gebruikt om voor het testen van automatische en handmatige. De virtuele machines zouden worden gemaakt met behulp van artefacten of formules die gegevens van het bouwproces gebruiken om de verschillende aangepaste configuraties die nodig zijn voor het testen van maken.   

### <a name="release"></a>Release 
DevTest Labs wordt doorgaans gebruikt voor verificatie in de sectie release voordat de code wordt geïmplementeerd. Het is vergelijkbaar met het testen in de sectie Build. Productieresources mag niet worden geïmplementeerd in DevTest Labs. 

### <a name="customization"></a>Aanpassing 
Er zijn in Azure DevOps, bestaande taken waarmee manipulatie van VM's en omgevingen binnen specifieke labs. Hoewel Azure DevOps-Services een manier zijn voor het beheren van de CI/CD-pijplijn, kunt u de testomgeving kunt integreren met elk systeem dat ondersteuning biedt voor de mogelijkheid voor het aanroepen van REST-API's, PowerShell-scripts uit te voeren of Azure CLI gebruiken. 

Hoewel sommige CI/CD-pipeline-beheerders bestaande open-source-invoegtoepassingen die in Azure en DevTest Labs-resources beheren hebben kunt. U moet mogelijk bepaalde aangepaste scripts gebruiken voor de specifieke behoeften van de pijplijn.  Met die Denk eraan dat u bij het uitvoeren van een taak, een service-principal gebruikt met de juiste rol voor toegang tot het lab. De service-principal moet meestal de toegang van de rol Inzender aan het lab. Zie voor meer informatie, [Azure DevTests Labs integreren in uw Azure DevOps continue integratie en levering pijplijn](devtest-lab-integrate-ci-cd-vsts.md). 
 