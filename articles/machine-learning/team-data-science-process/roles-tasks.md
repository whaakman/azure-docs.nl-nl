---
title: Team Data Science Process rollen en taken - Azure | Microsoft Docs
description: Een overzicht van de belangrijke onderdelen, het personeel functies en de bijbehorende taken voor een data science-teamproject.
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
services: machine-learning
ms.assetid: ''
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: deguhath
ms.openlocfilehash: 8cec2c2b72b88a27c4a6c15b197e859b879bef43
ms.sourcegitcommit: cfff72e240193b5a802532de12651162c31778b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/27/2018
ms.locfileid: "39308521"
---
# <a name="team-data-science-process-roles-and-tasks"></a>Team Data Science Process rollen en taken

Het Team Data Science Process is een framework ontwikkeld door Microsoft en die een gestructureerde methode voor het bouwen van predictive analytics-oplossingen en intelligente toepassingen efficiënt biedt. In dit artikel bevat een overzicht van de belangrijkste personeel-rollen en de bijbehorende taken die worden beheerd door een data science-team standaardiseren over dit proces. 

Deze inleiding koppelingen naar zelfstudies die instructies voor het bevatten instellen van de TDSP-omgeving voor de groep van volledige data science, data-scienceteams en projecten. Het biedt gedetailleerde richtlijnen met behulp van Visual Studio Team Services (VSTS) in de zelfstudies.  VSTS biedt een platform voor hosten van code en flexibele planning hulpprogramma teamtaken beheren, toegangsbeheer en beheren van de opslagplaatsen. 

U kunt deze informatie gebruiken voor het implementeren van TDSP op uw eigen code-hosting en flexibele planning hulpprogramma. 

## <a name="structures-of-data-science-groups-and-teams"></a>Structuur van de data science-groepen en teams

Data science-functies in ondernemingen kunnen vaak worden ingedeeld in de volgende hiërarchie:

1. ***Data science groep/s***

2. ***Data science-team/s in de groep/s***

In deze structuur zijn er groep en team leads. Een data science-project wordt gewoonlijk gedaan door een team data science, die kan bestaan uit het project leads (voor beheer en beheer van projecttaken) en gegevenswetenschappers of technici (afzonderlijke inzenders / technisch personeel) die de data science wordt uitgevoerd en data engineering-onderdelen van het project. De installatie en beheer wordt uitgevoerd, wordt uitgevoerd door de groep, team of project leads.

## <a name="definition-of-four-tdsp-roles"></a>Definitie van de vier TDSP-rollen
Met de bovenstaande veronderstelling zijn er vier verschillende rollen voor het personeel team:

1. ***Groepsbeheerder***. Groepsbeheerder is de manager van de volledige data science-eenheid in een onderneming. Een data science-eenheid mogelijk meerdere teams, die elk is bezig met meerdere data science-projecten in afzonderlijk zakelijk verticalen. De Manager van een groep mogelijk hun taken delegeren aan een vervangend, maar de taken die zijn gekoppeld aan de rol niet te wijzigen.

2. ***Team Lead***. Een lead team is een team in de data science-eenheid van een onderneming beheren. Een team bestaat uit meerdere gegevenswetenschappers. Voor data science-eenheid met slechts een beperkt aantal gegevenswetenschappers, Manager van de groep en het Team leiden mogelijk dezelfde persoon.

3. ***Project Lead***. Een lead project beheert de dagelijkse werkzaamheden van afzonderlijke datawetenschappers in een specifieke data science-project.

4. ***Individuele Inzender project***. Gegevenswetenschapper, bedrijfsanalist, Data engineering, Architect, enzovoort. De individuele Inzender van een project wordt uitgevoerd een data science-project. 


> [!NOTE]
> Afhankelijk van de structuur in een onderneming, één persoon kan meer dan één rol spelen of kunnen er meer dan één persoon die aan een rol werkt. Dit kan vaak het geval in kleine ondernemingen of ondernemingen met een klein aantal medewerkers in de data science-organisatie zijn.

## <a name="tasks-to-be-completed-by-four-personnel"></a>Taken worden voltooid door vier personeel

De volgende afbeelding ziet u de taken op het hoogste niveau voor personeel dat door de rol bij de goedkeuring en implementatie van het Team Data Science Process als conceptualized door Microsoft. 

![Overzicht van functies en taken](./media/roles-tasks/overview-tdsp-top-level.png)

Dit schema en de volgende, meer gedetailleerd overzicht van taken die zijn toegewezen aan elke rol in de TDSP kunt u de relevante zelfstudie op basis van uw verantwoordelijkheden in de organisatie kiezen.

> [!NOTE]
> De volgende instructies laten zien stappen over het instellen van een TDSP-omgeving en andere data science-taken in Visual Studio Team Services (VSTS) uit te voeren. We geven over het uitvoeren van deze taken met VSTS omdat dit is wat we gebruiken voor het implementeren van TDSP bij Microsoft. VSTS vergemakkelijkt de samenwerking door de integratie van het beheer van de werkitems die deze taken volgen en een code hosting service die wordt gebruikt voor het delen van hulpprogramma's, versies organiseren en beveiliging op basis van rollen. U zijn kunt andere platforms, als u liever, voor het implementeren van de taken die worden beschreven door de TDSP. Maar afhankelijk van uw platform, sommige functies van VSTS gebruikt mogelijk niet beschikbaar. 
>
>Instructies hier wordt ook gebruikt de [Data Science Virtual Machine (DSVM)](http://aka.ms/dsvm) op de Azure cloud-als de analysebureaublad met verschillende populaire data science-hulpprogramma's vooraf is geconfigureerd en geïntegreerd met diverse Microsoft-software en Azure Services. U kunt de DSVM of andere ontwikkelomgeving gebruiken voor het implementeren van TDSP. 


## <a name="group-manager-tasks"></a>Groepsbeheerder taken

De volgende taken worden uitgevoerd door de Manager van de groep (of een aangewezen TDSP-systeembeheerder) die zich conformeerde aan de TDSP:

- Maak een **groepsaccount** op een code die als host fungeert platform (zoals Github, Git, VSTS of anderen)
- Maak een **project sjabloonopslagplaats** op het groepsaccount en seeding uit de projectopslagplaats van de sjabloon die is ontwikkeld door Microsoft-TDSP-team. De opslagplaats voor sjablonen TDSP-project van Microsoft 
    - biedt een **mapstructuur gestandaardiseerd** mappen voor gegevens, code en -documenten, inclusief 
    - biedt een set **documentsjablonen gestandaardiseerd** als richtlijn voor een efficiënte datatechnologisch proces. 
- Maak een **hulpprogramma opslagplaats**, en het seeden vanuit de opslagplaats van het hulpprogramma die zijn ontwikkeld door Microsoft-TDSP-team. De opslagplaats van de TDSP hulpprogramma van Microsoft biedt 
    - een set van nuttige hulpprogramma's voor het maken van het werk van een gegevenswetenschapper efficiënter, waaronder hulpprogramma's voor interactieve gegevensverkenning, analyse en rapportage en basislijn gegevensmodellering en -rapportage.
- Instellen van de **beveiligingsbeleid van het besturingselement** van de volgende twee opslagplaatsen op uw groepsaccount.  

Zie voor gedetailleerde stapsgewijze instructies, [groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md). 


## <a name="team-lead-tasks"></a>Team Lead taken

De volgende taken worden uitgevoerd door het Team leiden (of een beheerder aangewezen team project) die zich conformeerde aan de TDSP:

- Als VSTS is geselecteerd als de code hostingplatform voor versiebeheer en samenwerking, maakt u een **teamproject** op van de groep VSTS-server. Anders wordt deze taak kan worden overgeslagen.
- Maak de **team project sjabloonopslagplaats** onder het teamproject en seeding van de groep sjabloon projectopslagplaats ingesteld door de manager van uw groep of het delegeren van de manager. 
- Maak de **team hulpprogramma opslagplaats**, en de hulpprogramma's voor team-specifieke toevoegen aan de opslagplaats. 
- (Optioneel) Maak **[Azure bestandsopslag](https://azure.microsoft.com/services/storage/files/)** moet worden gebruikt voor het opslaan van de gegevensassets die nuttig voor het hele team zijn kunnen. Andere teamleden kunnen deze bestandsarchief gedeelde cloud koppelen op hun eigen bureaublad analytics.
- (Optioneel) Koppelen van de Azure file storage om de **Data Science Virtual Machine** (DSVM) van het team te leiden en gegevensassets toevoegen op het.
- Instellen van de **beveiligingscontrole** door toe te voegen leden van een team en zijn bevoegdheden te configureren. 

Zie voor gedetailleerde stapsgewijze instructies, [teamleider taken voor een team van gegevenswetenschappers](team-lead-tasks.md).  


## <a name="project-lead-tasks"></a>Projecttaken Lead

De volgende taken worden uitgevoerd door het Project leiden vast te stellen de TDSP:

- Maak een **projectopslagplaats** onder het teamproject en seed project van het Team van-opslagplaats voor sjablonen. 
- (Optioneel) Maak **Azure bestandsopslag** moet worden gebruikt voor het opslaan van gegevensassets van het project. 
- (Optioneel) Koppelen van de Azure file storage om de **Data Science Virtual Machine** (DSVM) van het Project leiden en project gegevensassets erop toevoegen.
- Instellen van de **beveiligingscontrole** door projectleden toe te voegen en zijn bevoegdheden te configureren. 

Zie voor gedetailleerde stapsgewijze instructies, [Project leiden taken voor een team van gegevenswetenschappers](project-lead-tasks.md). 

## <a name="project-individual-contributor-tasks"></a>Individuele Inzender van projecttaken

De volgende taken worden uitgevoerd door een Project individuele Inzender (meestal een Gegevenswetenschapper) voor het uitvoeren van de data science-project van de TDSP gebruiken:

- Kloon de **projectopslagplaats** instellen door de projectleider. 
- (Optioneel) Koppel de gedeelde **Azure bestandsopslag** van het team en het project op hun **Data Science Virtual Machine** (DSVM).
- Voer het project. 

 
Zie voor gedetailleerde stapsgewijze instructies voor het toevoegen aan een project, [afzonderlijke inzenders Project voor een team van gegevenswetenschappers](project-ic-tasks.md). 


## <a name="data-science-project-execution"></a>Data science-project uitvoeren
 
Door de betreffende set instructies te volgen, kunnen gegevenswetenschappers, projectleider en team leads werkitems voor het volgen van alle taken en -fasen die een project moet aan het einde van het begin maken. Met behulp van git ook bevordert samenwerking tussen gegevenswetenschappers en zorgt ervoor dat de artefacten die zijn gegenereerd tijdens het uitvoeren van project versie beheerd en gedeeld door alle projectleden.

De instructies voor de projectuitvoering van het zijn ontwikkeld op basis van de veronderstelling dat beide werkitems en git-opslagplaatsen op VSTS zijn-project. Met behulp van VSTS voor beide, kunt u het koppelen van uw werkitems met de Git-vertakkingen van uw project-opslagplaatsen. Op deze manier kunt u eenvoudig bijhouden wat voor een werkitem is voltooid. 

De volgende afbeelding geeft een overzicht van deze werkstroom voor de projectuitvoering van het met behulp van de TDSP.

![Typische Data Science-Project kan worden uitgevoerd](./media/roles-tasks/overview-project-execute.png)

De werkstroom bevat de stappen die kunnen worden gegroepeerd in drie activiteiten:

- Sprint plannen (Project leiden)
- Het ontwikkelen van artefacten op git vertakkingen om werkitems (Gegevenswetenschapper)
- Code bekijken en samenvoegen van branches weergegeven met de master vertakkingen (Project leiden of andere teamleden)

Zie voor gedetailleerde stapsgewijze instructies voor het project uitvoeringswerkstroom, [uitvoering van wetenschappelijke gegevensprojecten](project-execution.md).

## <a name="project-structure"></a>Projectstructuur

Gebruik deze [project sjabloonopslagplaats](https://github.com/Azure/Azure-TDSP-ProjectTemplate) ter ondersteuning van efficiënte projectuitvoering en samenwerking. Deze opslagplaats beschikt u over een gestandaardiseerde directory-structuur en het document sjablonen die u kunt gebruiken voor uw eigen TDSP-project.

## <a name="next-steps"></a>Volgende stappen

Meer gedetailleerde beschrijvingen van de functies en taken die zijn gedefinieerd door het Team Data Science Process verkennen:

- [Groepsbeheerder taken voor een team van gegevenswetenschappers](group-manager-tasks.md)
- [Team Lead taken voor een team van gegevenswetenschappers](team-lead-tasks.md)
- [Project Lead taken voor een team van gegevenswetenschappers](project-lead-tasks.md)
- [Afzonderlijke inzenders project voor een team van gegevenswetenschappers](project-ic-tasks.md)