---
title: Indelen van de implementatie van Azure DevTest Labs
description: Dit artikel bevat richtlijnen voor het organiseren van implementatie van Azure DevTest Labs in uw organisatie.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: christianreddington
manager: femila
editor: spelluru
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/11/2019
ms.author: chredd
ms.reviewer: anthdela,juselph
ms.openlocfilehash: 1e657260eeb36dc813f856ab172882a396324450
ms.sourcegitcommit: e69fc381852ce8615ee318b5f77ae7c6123a744c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/11/2019
ms.locfileid: "56004431"
---
# <a name="orchestrate-the-implementation-of-azure-devtest-labs"></a>Indelen van de implementatie van Azure DevTest Labs
In dit artikel biedt een aanbevolen aanpak voor snelle implementatie en de implementatie van Azure DevTest Labs. De volgende afbeelding ligt de nadruk op het algehele proces als richtlijnen terwijl u flexibiliteit voor het ondersteunen verschillende industriële eisen en scenario's.

![Stappen voor het implementeren van Azure DevTest Labs](./media/devtest-lab-guidance-orchestrate-implementation/implementation-steps.png)

## <a name="assumptions"></a>Veronderstellingen
In dit artikel wordt ervan uitgegaan dat u de volgende items in plaats hebt voordat u implementeert een DevTest Labs-test:

- **Azure-abonnement**: Het proefteam heeft toegang tot het implementeren van resources in een Azure-abonnement. Als de werkbelastingen zijn alleen ontwikkelen en testen, is het raadzaam om te selecteren van de Enterprise-DevTest-aanbieding voor aanvullende beschikbare installatiekopieën en lagere tarieven voor virtuele machines van Windows.
- **On-Premises Access**: Indien nodig, is toegang tot on-premises al geconfigureerd. De on-premises toegang kan worden bereikt via een Site-naar-site VPN-verbinding of Express Route. Connectiviteit via een Express Route kunnen doorgaans veel weken tot stand brengen, het is raadzaam om te beschikken over de Express-Route voordat u begint met het project.
- **Test Teams**: De ontwikkelingsfase project teams die gebruikmaakt van DevTest Labs is geïdentificeerd, samen met de toepasselijke ontwikkeling of tests activiteiten en vereisten/doelstellingen voor deze teams tot stand brengen.

## <a name="milestone-1-establish-initial-network-topology-and-design"></a>Mijlpaal 1: Eerste netwerktopologie en -ontwerp maken
Het eerste gedeelte van de focus bij het implementeren van een Azure DevTest Labs-oplossing is om de geplande verbinding voor de virtuele machines te maken. De volgende stappen beschrijven de vereiste procedures:

1. Definieer **IP-adresbereiken voor de eerste** die zijn toegewezen aan de DevTest Labs-abonnement in Azure. Deze stap moet voorspellen van het verwachte gebruik van het aantal virtuele machines, zodat u kunt een groot genoeg is voor toekomstige uitbreiding.
2. Identificeer **gewenste toegangsmethoden** in de DevTest Labs (bijvoorbeeld externe / interne toegang). Een belangrijk punt in deze stap is om te bepalen of virtuele machines openbare IP-adressen (dat wil zeggen, via internet rechtstreeks toegankelijk).
3. Identificeren en tot stand brengen **methoden van de verbinding** cloud met de rest van de Azure-omgeving en on-premises. Als de geforceerde routering met Expressroute is ingeschakeld, is het waarschijnlijk dat de virtuele machines moet juiste proxyconfiguraties om door te gaan van de firewall van het bedrijf.
4. Als VM's die moeten worden **toegevoegd aan een domein**, bepalen of ze lid maken van een domein cloud-gebaseerde (Directory Services AAD bijvoorbeeld) of een on-premises domein. Bepalen welke organisatie-eenheid (OE) in active directory die lid wordt van de virtuele machines voor on-premises. Controleer bovendien of dat gebruikers toegang hebben tot join (of tot stand brengen van een service-account met de mogelijkheid om machine om records te maken in het domein)

## <a name="milestone-2-deploy-the-pilot-lab"></a>Mijlpaal 2: De test lab implementeren
Zodra de netwerktopologie ingesteld is, het eerste/test lab kan worden gemaakt door de volgende stappen:

1. Een eerste DevTest Labs-omgeving maken (Stapsgewijze instructies hiervoor vindt [hier](https://github.com/Azure/fta-devops/blob/master/devtest-labs/articles/devtest-labs-walkthrough-it.md))
2. Toegestane VM-installatiekopieën en grootten voor gebruik met lab bepalen. Bepaal of aangepaste installatiekopieën kunnen worden geüpload naar Azure voor gebruik met DevTest Labs.
3. Beveiligde toegang tot de testomgeving door het maken van de initiële Role Base Access besturingselementen (RBAC) voor de testomgeving (lab eigenaars en gebruikers van de testomgeving). U wordt aangeraden de gesynchroniseerde active directory-accounts te gebruiken met Azure Active Directory voor de identiteit met DevTest Labs.
4. DevTest Labs voor gebruik van beleid, zoals schema's, kosten beheer, claimbare virtuele machines, aangepaste installatiekopieën of formules configureren.
5. Stel een online, zoals Azure opslagplaatsen/Git-opslagplaats.
6. Besluiten over het gebruik van openbare of particuliere opslagplaatsen of combinatie van beide. JSON-sjablonen indelen voor implementaties en op lange termijn sustainment.
7. Indien nodig, kunt u aangepaste artefacten maken. Deze stap is optioneel. 

## <a name="milestone-3-documentation-support-learn-and-improve"></a>Mijlpaal 3: Documentatie, ondersteuning, leren en verbeteren
De eerste pilot teams mogelijk uitgebreide ondersteuning voor het aan de slag. Gebruik van hun ervaringen om te controleren of de juiste documentatie en ondersteuning voor doorlopende implementatie van Azure DevTest Labs is.

1. Introductie van de pilot teams hun nieuwe DevTest Labs-resources (demo's, documentatie)
2. Op basis van de pilot teams ervaringen, plannen en leveren van documentatie, indien nodig
3. Formaliseren proces voor onboarding van nieuwe teams (het maken en configureren van labs, bieden toegang, enzovoort)
4. Op basis van de eerste opname, Controleer of het oorspronkelijke prognose van IP-adresruimte wordt nog steeds redelijke en nauwkeurige
5. Zorg ervoor dat de juiste nalevings- en beoordelingen zijn voltooid

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel in deze reeks: [Beheer van Azure DevTest Labs-infrastructuur](devtest-lab-guidance-governance-resources.md)
