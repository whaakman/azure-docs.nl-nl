---
title: Gegevens wetenschap proces rollen en taken - Azure in een team | Microsoft Docs
description: Een overzicht van de belangrijke onderdelen, personeel rollen en bijbehorende taken voor een teamproject voor wetenschap van gegevens.
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/04/2017
ms.author: bradsev;
ms.openlocfilehash: b502a586cdb7351d5b22c6b0db966723b8ebb7b4
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="team-data-science-process-roles-and-tasks"></a>Team gegevens wetenschap proces rollen en taken

Het Team gegevens wetenschap proces is een framework dat is ontwikkeld door Microsoft waarmee een gestructureerde methodologie voor het efficiënt predictive analytics-oplossingen en intelligente toepassingen te ontwikkelen. In dit artikel bevat een overzicht van de belangrijkste personeel rollen en de bijbehorende taken die worden verwerkt door een wetenschappelijke gegevens in een team standaardiseren op dit proces. 

Deze inleiding koppelingen naar zelfstudies waarin instructies over het instellen van de omgeving TDSP voor de hele gegevensgroep voor wetenschap, gegevens wetenschappelijke teams en projecten. We bieden gedetailleerde richtlijnen met behulp van Visual Studio Team Services (VSTS) in de zelfstudies als onze platform voor hosten van code en flexibele hulpprogramma teamtaken beheren, toegang controleren en beheren van de opslagplaatsen. 

Ook is mogelijk gebruik van deze informatie TDSP implementeren op uw eigen code-hosting en flexibele planning hulpprogramma. 

## <a name="structures-of-data-science-groups-and-teams"></a>Structuur van de wetenschap gegevensgroepen en teams
Functies van wetenschappelijke gegevens in ondernemingen kunnen vaak worden ingedeeld in de volgende hiërarchie:

1. ***Gegevens wetenschappelijke groep/s***

2. ***Gegevens wetenschappelijke team/s in de groep/s***

Er wordt groep zijn in deze structuur en potentiële klanten in een team. Normaal gesproken een wetenschappelijke gegevensproject wordt uitgevoerd door een team wetenschappelijke gegevens, die kan bestaan uit het project leads (voor beheer en toezicht projecttaken) en gegevenswetenschappers of engineers (individuele gebruikers / technische personeel) die de wetenschappelijke gegevens wordt uitgevoerd en de gegevens engineering delen van het project. De installatie en beheer wordt uitgevoerd, wordt uitgevoerd door de groep, team of project leads.

## <a name="definition-of-four-tdsp-roles"></a>Definitie van de vier TDSP rollen
Met de bovenstaande veronderstelling hebben we vier verschillende rollen voor ons team personeel opgegeven:

1. ***Groepsbeheerder***. Groep Manager is de manager van de gehele wetenschappelijke eenheid in een onderneming. Een wetenschappelijke gegevenseenheid wellicht meerdere teams, die elk meerdere gegevens wetenschap-projecten in twee afzonderlijke business verticalen wordt gewerkt. Groepsbeheerder mogelijk hun taken delegeren aan een vervanging, maar de taken die zijn gekoppeld aan de functie niet te wijzigen.

2. ***In een team Lead***. Een team lead wordt beheerd door een team in de wetenschap gegevenseenheid van een onderneming. Een team bestaat uit meerdere gegevenswetenschappers. Voor wetenschap gegevenseenheid met een klein aantal gegevenswetenschappers Manager van de groep en het Team leiden mogelijk dezelfde persoon.

3. ***Project Lead***. Een project lead beheert de dagelijkse activiteiten van afzonderlijke gegevenswetenschappers op een specifieke gegevens wetenschappelijke project.

4. ***Afzonderlijke Inzender project***. Gegevens wetenschappelijk, bedrijfsanalist, gegevens engineering, Architect, enzovoort. Een project afzonderlijke inzender voert een wetenschappelijke gegevensproject. 


**[AZURE.NOTE]**: Één persoon kan meer dan één rol spelen afhankelijk van de structuur in een onderneming, of kan er meer dan één persoon werkt aan een rol. Dit komt vaak het geval in kleine bedrijven of ondernemingen met een klein aantal medewerkers in hun organisatie van de wetenschappelijke gegevens mogelijk.

## <a name="tasks-to-be-completed-by-four-personnel"></a>Taken worden voltooid door de medewerkers van de vier

De volgende afbeelding ziet u de taken op het hoogste niveau voor personeel dat door de rol bij de goedkeuring en implementatie van het Team gegevens wetenschap proces als conceptualized door Microsoft. 

![Overzicht van rollen en taken](./media/roles-tasks/overview-tdsp-top-level.png)

Dit schema en de volgende, meer gedetailleerde omtrek van taken die zijn toegewezen aan elke rol in de TDSP kunt u de juiste zelfstudie op basis van uw verantwoordelijkheden in de organisatie kiezen.

>[AZURE.NOTE] In de volgende instructies, laten we zien stappen van het instellen van een omgeving TDSP en andere gegevens wetenschappelijke taken in Visual Studio Team Services (VSTS) te voltooien. We opgeven hoe u deze taken met VSTS omdat wat we gebruiken voor het implementeren van TDSP bij Microsoft. VSTS samenwerking vereenvoudigt door integratie van het beheer van de werkitems die taken te volgen en een code-hostingservice gebruikt voor het delen van hulpprogramma's, organiseert versies en op rollen gebaseerde beveiliging bieden. U zijn kunt andere platforms kiezen als u liever, voor het implementeren van de taken die worden beschreven door de TDSP. Maar, afhankelijk van uw platform sommige functies die we van VSTS gebruikmaken mogelijk niet beschikbaar. 
>
>We gebruiken ook het [gegevens wetenschappelijke virtuele Machine (DSVM)](http://aka.ms/dsvm) cloud op Azure als het bureaublad analytics met verschillende populaire gegevens wetenschappelijke hulpmiddelen vooraf is geconfigureerd en geïntegreerd met diverse Microsoft-software en Azure-services. U kunt de DSVM of andere ontwikkelomgeving TDSP implementeren. 


## <a name="group-manager-tasks"></a>Groep Manager-taken

De volgende taken worden uitgevoerd door de groepsbeheerder (of een aangewezen TDSP systeembeheerder) vast te stellen de TDSP:

- Maak een **groepsaccount** op een code die als host fungeert platform (zoals Github, Git, VSTS of anderen)
- Maak een **project sjabloon opslagplaats** voor het groepsaccount en de seed in de opslagplaats project-sjabloon die is ontwikkeld door Microsoft TDSP team. De opslagplaats TDSP project sjabloon van Microsoft biedt een **mapstructuur gestandaardiseerd** inclusief mappen voor gegevens, code en documenten en bevat een reeks **gestandaardiseerd documentsjablonen** om te helpen een efficiënte gegevens wetenschap proces. 
- Maak een **hulpprogramma opslagplaats**, en seed-vanuit de opslagplaats hulpprogramma is ontwikkeld door Microsoft TDSP team. De opslagplaats TDSP hulpprogramma van Microsoft biedt een reeks nuttige hulpprogramma's voor het werk van een wetenschappelijk gegevens om efficiënter te maken, met inbegrip van hulpprogramma's voor interactieve gegevensverkenning, analyse en rapportage en voor de basislijn modelleren en rapportage.
- Instellen van de **beveiligingsbeleid van het besturingselement** van deze twee opslagplaatsen voor je groepsaccount.  

Zie voor gedetailleerde stapsgewijze instructies [groepsbeheerder taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md). 


## <a name="team-lead-tasks"></a>Team Lead taken

De volgende taken zijn voltooid door het Team leiden (of een beheerder van het project aangewezen team) vast te stellen de TDSP:

- Als VSTS moet de code host-platform voor versiebeheer en samenwerking is ingeschakeld, maakt u een **teamproject** op van de groep VSTS server. Anders wordt kan deze taak worden overgeslagen.
- Maak de **team project sjabloon opslagplaats** onder het teamproject en seed in de groep project sjabloon opslagplaats ingesteld door uw groepmanager of de gemachtigde van de manager. 
- Maak de **team hulpprogramma opslagplaats**, en de team-specifieke hulpprogramma's toevoegen aan de opslagplaats. 
- (Optioneel) Maak  **[Azure file storage](https://azure.microsoft.com/services/storage/files/)**  moet worden gebruikt voor het opslaan van gegevensassets die nuttig is voor het hele team kunnen zijn. Andere teamleden kunnen dit archief voor gedeelde cloud koppelen op hun eigen bureaublad analytics.
- (Optioneel) Koppel de bestandsopslag van de Azure-naar de **gegevens wetenschappelijke virtuele Machine** (DSVM) van het team te leiden en gegevensassets toevoegen op het.
- Instellen van de **beveiligingscontrole** door teamleden toe te voegen en hun bevoegdheden configureren. 

Zie voor gedetailleerde stapsgewijze instructies [Team leiden taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md).  


## <a name="project-lead-tasks"></a>Projecttaken Lead

De volgende taken zijn voltooid door het Project ertoe leiden dat de TDSP nemen:

- Maak een **project opslagplaats** onder het teamproject en seed uit het Team sjabloon opslagplaats project. 
- (Optioneel) Maak **Azure file storage** moet worden gebruikt voor het opslaan van gegevensassets van het project. 
- (Optioneel) Koppel de bestandsopslag van de Azure-naar de **gegevens wetenschappelijke virtuele Machine** (DSVM) van het Project leiden en gegevensassets project toevoegen op het.
- Instellen van de **beveiligingscontrole** door projectleden toe te voegen en hun bevoegdheden configureren. 

Zie voor gedetailleerde stapsgewijze instructies [Project leiden taken voor een wetenschappelijke Gegevensteam](project-lead-tasks.md). 

## <a name="project-individual-contributor-tasks"></a>Afzonderlijke Inzender projecttaken

De volgende taken worden uitgevoerd door een Project afzonderlijke Inzender (meestal een wetenschappelijk gegevens) voor het uitvoeren van het project in de wetenschap die is via de TDSP:

- Kloon de **project opslagplaats** is ingesteld door de lead project. 
- (Optioneel) Koppelen van de gedeelde **Azure file storage** van het team en het project op hun **gegevens wetenschappelijke virtuele Machine** (DSVM).
- Het project worden uitgevoerd. 

 
Zie voor gedetailleerde stapsgewijze instructies voor het op twee locaties op een project, [Project individuele gebruikers voor een wetenschappelijke Gegevensteam](project-ic-tasks.md). 


## <a name="data-science-project-execution"></a>Gegevens wetenschappelijke project uitvoeren
 
Door de relevante reeks instructies te volgen, maken gegevenswetenschappers, project lead en team leads werkitems om bij te houden van alle taken en fasen die een project moet van het begin tot het einde. Met git ook bijdraagt aan de samenwerking tussen gegevenswetenschappers en zorgt ervoor dat de artefacten die zijn gegenereerd tijdens het uitvoeren van een project versie beheerd en gedeeld door alle leden zijn.

De instructies voor de projectuitvoering van het zijn ontwikkeld op basis van de veronderstelling dat beide werkitems en git opslagplaatsen op VSTS zijn project. VSTS gebruiken voor beide, kunt u uw werkitems met Git vertakkingen van uw project-opslagplaatsen koppelen. Op deze manier kunt u eenvoudig bijhouden wat is gedaan voor een werkitem. 

De volgende afbeelding bevat een overzicht van deze werkstroom voor de projectuitvoering van het met behulp van de TDSP.

![Typische gegevens wetenschappelijke Project uitvoeren](./media/roles-tasks/overview-project-execute.png)

De werkstroom bevat de stappen beschreven die kunnen worden gegroepeerd in drie activiteiten:

- Sprint plannen (Project leiden)
- Ontwikkeling van artefacten op git vertakkingen voor het oplossen van werkitems (wetenschappelijk gegevens)
- Code-revisie en het samenvoegen van filialen met master vertakkingen (leiden Project of andere teamleden)

Zie voor gedetailleerde stapsgewijze instructies voor het project uitvoeringswerkstroom, [uitvoering van de gegevens wetenschappelijke projecten](project-execution.md).

## <a name="next-steps"></a>Volgende stappen

Hier vindt u koppelingen naar de meer gedetailleerde beschrijvingen van de rollen en taken die zijn gedefinieerd door het Team gegevens wetenschap proces:

- [Groep Manager-taken voor een wetenschappelijke Gegevensteam](group-manager-tasks.md)
- [Team Lead taken voor een wetenschappelijke Gegevensteam](team-lead-tasks.md)
- [Projecttaken Lead voor een wetenschappelijke Gegevensteam](project-lead-tasks.md)
- [Project individuele gebruikers voor een wetenschappelijke Gegevensteam](project-ic-tasks.md)