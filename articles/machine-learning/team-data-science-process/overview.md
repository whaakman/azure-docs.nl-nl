---
title: Wat is de procedure voor wetenschappelijke gegevens Team? | Microsoft Docs
description: Biedt een gegevens wetenschappelijke methodologie voor het leveren van predictive analytics-oplossingen en intelligente toepassingen.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: b1f677bb-eef5-4acb-9b3b-8a5819fb0e78
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/20/2017
ms.author: bradsev;
ms.openlocfilehash: 75e170f29f9a6abeeb3393e43e6a4c355749044d
ms.sourcegitcommit: cf4c0ad6a628dfcbf5b841896ab3c78b97d4eafd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/21/2017
---
# <a name="what-is-the-team-data-science-process"></a>Wat is de procedure voor wetenschappelijke gegevens Team?

Het Team gegevens wetenschap proces (TDSP) is een flexibele, herhalende gegevens wetenschappelijke methodologie predictive analytics-oplossingen en intelligente toepassingen efficiënt te leveren. TDSP verbetert teamsamenwerking en leren. Het bevat een distillatie van de aanbevolen procedures en structuren van Microsoft en anderen in de branche die de geslaagde implementatie van de gegevens wetenschappelijke initiatieven vergemakkelijken. Het doel is om de voordelen van het programma analytics altijd bewust bedrijven.

Dit artikel bevat een overzicht van TDSP en de belangrijkste onderdelen. We bieden een algemene beschrijving van het proces dat kan worden geïmplementeerd met tal van hulpprogramma's. Een gedetailleerde beschrijving van de projecttaken en functies die zijn betrokken bij de levenscyclus van het proces is beschikbaar in extra gekoppelde onderwerpen. Richtlijnen voor het implementeren van de TDSP met behulp van een specifieke set Microsoft-programma's en infrastructuur die we gebruiken voor het implementeren van de TDSP in onze teams is ook beschikbaar.

## <a name="key-components-of-the-tdsp"></a>Belangrijke onderdelen van de TDSP

TDSP bestaat uit de volgende hoofdonderdelen:

- Een **gegevens wetenschappelijke lifecycle** definitie
- Een **gestandaardiseerd projectstructuur**
- **Infrastructuur en resources** voor gegevens wetenschappelijke projecten
- **Hulpprogramma's** voor de projectuitvoering van


## <a name="data-science-lifecycle"></a>Levenscyclus van wetenschappelijke gegevens

Het Team gegevens wetenschap proces (TDSP) biedt een levenscyclus voor het ontwikkelen van projecten wetenschappelijke gegevens structuur. De levenscyclus van bevat de stappen, van begin tot eind, projecten meestal volgen wanneer ze worden uitgevoerd.

Als u een andere gegevens wetenschappelijke levensduur, zoals gebruikt [heldere DM](https://wikipedia.org/wiki/Cross_Industry_Standard_Process_for_Data_Mining), [KDD](https://wikipedia.org/wiki/Data_mining#Process) of uw organisatie eigen aangepaste proces, kunt u de TDSP op basis van een taak in de context van de levenscycli van de ontwikkeling. Op een hoog niveau hebben deze verschillende methoden veel gemeen. 

Deze levenscyclus beheren is ontworpen voor wetenschap-projecten voor gegevens die worden geleverd als onderdeel van intelligente toepassingen. Deze toepassingen implementeren machine learning of kunstmatige intelligence modellen voor predictive analytics. Experimentele gegevens wetenschap of ad-hoc analytics projecten kunnen ook profiteren van dit proces. Maar in dergelijke gevallen enkele van de beschreven stappen mogelijk niet nodig.    

De levenscyclus geeft een overzicht van de belangrijke fasen die projecten doorgaans worden uitgevoerd, vaak iteratief:

* **Wat voor bedrijven**
* **Gegevensverzameling en begrijpen**
* **Modeling**
* **Implementatie**
* **Acceptatie van de klant**

Hier volgt een visuele representatie van het **Team gegevens wetenschap proces lifecycle**. 

![TDSP Lifecycle2](./media/overview/tdsp-lifecycle2.png) 

De doelstellingen, taken en documentatie artefacten voor elke fase van de levenscyclus in TDSP worden beschreven in de [Team gegevens wetenschap proces lifecycle](lifecycle.md) onderwerp. Deze taken en de artefacten zijn gekoppeld aan Projectrollen:

- Oplossing architect
- Projectmanager
- Gegevenswetenschapper
- Projectleider 

Het volgende diagram biedt een rasterweergave van de taken (in blauw) en de artefacten (in groen) die zijn gekoppeld aan elke fase van de levenscyclus (op de horizontale as) voor deze rollen (op de verticale as). 

![TDSP-functies-en-taken](./media/overview/tdsp-tasks-by-roles.png)

## <a name="standardized-project-structure"></a>De structuur gestandaardiseerde project

Met alle projecten een mapstructuur delen en het gebruik van sjablonen voor de project-documenten, kunt u gemakkelijk de teamleden voor meer informatie over hun projecten. Alle code en documenten worden opgeslagen in een versiebeheersysteem (VCS) zoals Git, TFS of Subversion teamsamenwerking inschakelen. Bijhouden van taken en functies in een flexibele project traceringssysteem zoals Jira Rally, Visual Studio Team Services Hiermee kan dichter bij het bijhouden van de code voor de afzonderlijke onderdelen. Dergelijke bijhouden kan ook teams verkrijgen beter geschatte kosten. TDSP raadt aan om te maken van een afzonderlijke opslagplaats voor elk project in de VCS voor versiebeheer, informatiebeveiliging en samenwerking. De gestandaardiseerde structuur voor alle projecten helpt institutionele kennis bouwen in de hele organisatie.

We bieden sjablonen voor de mapstructuur en de vereiste documenten op standaardlocaties. Deze mapstructuur organiseert de bestanden die code voor gegevensverkenning en uitpakken van de functie bevatten en die model iteraties opnemen. Deze sjablonen eenvoudiger teamleden om te begrijpen werk dat door anderen en nieuwe leden toevoegen aan teams. Het is gemakkelijk weergeven en bijwerken van de documentsjablonen in markdown-indeling. Sjablonen gebruiken om te voorzien van belangrijke vragen voor elk project om ervoor te zorgen dat het probleem goed gedefinieerde is en dat de kwaliteit verwacht voldoen aan de producten controlelijsten. Voorbeelden zijn:

- een freelance project te maken voor het zakelijke probleem en het bereik van het project
- rapporten met gegevens naar het document de structuur en statistieken van de onbewerkte gegevens
- model rapporten om vast te leggen van de afgeleide functies
- model maatstaven voor prestaties zoals ROC curven of muis


![TDSP-mappen](./media/overview/tdsp-dir-structure.png)

De mapstructuur uit kan worden gekloond [Github](https://github.com/Azure/Azure-TDSP-ProjectTemplate).

## <a name="infrastructure-and-resources-for-data-science-projects"></a>Infrastructuur en resources voor gegevens wetenschappelijke projecten  

TDSP bevat aanbevelingen voor het beheren van gedeelde analytics en opslaginfrastructuur zoals:

- cloud-bestandssystemen voor het opslaan van gegevenssets 
- databases
- BIG data (Hadoop of Spark) clusters 
- machine learning-services. 

De infrastructuur van de analyses en opslag kan zich in de cloud of on-premises. Dit is waar de onbewerkte gegevens en verwerkte gegevenssets worden opgeslagen. Deze infrastructuur kunt reproduceerbare analyse. Ook voorkomt u dubbele, wat tot inconsistenties en onnodige infrastructuurkosten leiden kan. Hulpprogramma's worden geleverd om te leveren van de gedeelde resources, ze bijhouden, zodat elk teamlid veilig verbinding kunnen maken met deze bronnen. Het is ook een goede gewoonte projectleden die u maakt een consistente compute-omgeving hebben. Andere teamleden kunnen repliceren en experimenten te valideren.

Hier volgt een voorbeeld van een team werkt aan meerdere projecten en delen van de verschillende onderdelen van de cloud analytics infrastructuur.

![TDSP-infrastructuur](./media/overview/tdsp-analytics-infra.png)


## <a name="tools-and-utilities-for-project-execution"></a>Hulpprogramma's voor de projectuitvoering van

Inleiding tot processen in de meeste organisaties is lastig. Hulpprogramma's te implementeren de gegevens wetenschap proces en de levenscyclus van help lager de belemmering voor het verhogen van de consistentie van de vaststelling. TDSP biedt een eerste set hulpprogramma's en scripts te acceptatie van TDSP binnen een team. Het helpt tevens een gedeelte van de algemene taken in de levenscyclus van de wetenschap zoals gegevensverkenning en basislijn modellering automatiseren. Er is een goed gedefinieerde structuur opgegeven voor personen om bij te dragen gedeelde hulpprogramma's in hun team gedeelde code opslagplaats. Deze resources kunnen vervolgens worden gebruikt door andere projecten binnen het team dat of de organisatie. TDSP wil ook bijdragen van hulpprogramma's en hulpprogramma's voor de hele community inschakelen. De hulpprogramma's voor TDSP kunnen worden gekloond van [Github](https://github.com/Azure/Azure-TDSP-Utilities).


## <a name="next-steps"></a>Volgende stappen

[Team gegevens wetenschappelijke processen: De rollen en taken](https://github.com/Azure/Microsoft-TDSP/blob/master/Docs/roles-tasks.md) geeft een overzicht van de belangrijkste personeel-rollen en de bijbehorende taken voor een wetenschappelijke-team voor gegevens die op dit proces standaardiseert. 
