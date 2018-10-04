---
title: Aan de slag met Azure DevTest Labs
description: Dit artikel bevat de primaire scnearios voor het gebruik van Azure DevTest Labs en twee algemene paden om te beginnen met de service in uw organisatie.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/03/2018
ms.author: spelluru
ms.openlocfilehash: 1d00dbe23fbd724893ba2b964fc54a3ea9066dcb
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48251018"
---
# <a name="get-started-with-using-azure-devtest-labs"></a>Aan de slag met Azure DevTest Labs
Als u DevTest Labs verkennen besluit, er zijn twee algemene paden voorwaarts – Proof of Concept vs schaal implementatie. 

Een **uitgebreide implementatie** bestaat uit weken/maanden van controleren en planning met de opzet van de implementatie van DevTest Labs voor de hele onderneming met honderden of duizenden ontwikkelaars. 

Een **bewijs van concept implementatie** richt zich op een geconcentreerd inspanningen van één team tot stand brengen van organisatie-waarde. Terwijl het verleidelijk zijn kan om na te denken van een uitgebreide implementatie, doorgaans de aanpak kan niet vaker dan het bewijs van concept-optie. Daarom raden wij aan dat u klein beginnen, leer van de eerste team, herhaalt u de dezelfde benadering met twee of drie extra teams en vervolgens te plannen voor een uitgebreide implementatie op basis van de kennis opgedaan uit. Voor een geslaagde bewijs van concept, wordt aangeraden dat u een of twee teams kiest, en identificeren van de scenario's (dev-omgeving vs testomgevingen), hun huidige gebruiksvoorbeelden document, en DevTest Labs implementeert. 

## <a name="scenarios"></a>Scenario's
Er zijn drie primaire scenario's voor een DevTest Labs-implementatie: 

- Developer bureaubladen
- Testomgevingen
- Training-Labs/hackathon

## <a name="developer-desktops"></a>Developer bureaubladen
Ontwikkelaars hebben vaak verschillende vereisten voor de ontwikkeling van machines voor verschillende projecten. Met DevTest Labs kunnen ontwikkelaars hebben altijd toegang tot on-demand virtuele machines die vooraf zijn geconfigureerd aan de behoeften van hun meest voorkomende scenario's. DevTest Labs biedt de volgende voordelen:

- Organisaties krijgt u een gemeenschappelijk ontwikkelings- en testomgevingen ervoor te zorgen dat de consistentie tussen teams
- Ontwikkelaars kunnen snel inrichten voor hun ontwikkeling-machines op aanvraag
- Ontwikkelaars kunnen resources inrichten op een manier Self-service zonder abonnement machtigingen
- IT of beheerders kunnen de netwerktopologie vooraf gedefinieerde en ontwikkelaars kunnen rechtstreeks gebruikmaken van deze op een eenvoudige en intuïtieve manier zonder speciale toegang
- Ontwikkelaars kunnen hun ontwikkeling machines gemakkelijk aanpassen indien nodig
- Beheerders kunnen kosten beheren door ervoor te zorgen dat:
    - Ontwikkelaars kunnen niet meer virtuele machines bevat dan ze nodig hebt voor ontwikkeling ophalen
    - VM's worden afgesloten wanneer deze niet in gebruik
    - Waarbij alleen een subset van de VM-exemplaargrootten voor de specifieke werkbelastingen

## <a name="test-environments"></a>Testomgevingen
Het maken en beheren van testomgevingen binnen een onderneming kunnen een aanzienlijke inspanning vereisen. Met DevTest Labs, kunnen testomgevingen worden eenvoudig gemaakt, bijgewerkt of wordt gedupliceerd, waarmee teams voor toegang tot een volledig geconfigureerde omgeving als dat nodig. In dit scenario biedt DevTest Labs de volgende voordelen:

- Testers kunnen de nieuwste versie van de toepassing testen door snel Windows en Linux-omgevingen inrichten met behulp van herbruikbare sjablonen en artefacten.
- Testers kunnen hun belastingtests op via het inrichten van meerdere testagents opschalen
- Beheerders verbinding kunnen maken van de testomgeving met VSTS DevOps-scenario's inschakelen
- Beheerders kunnen kosten beheren door ervoor te zorgen dat:
    - Testers kunnen niet meer virtuele machines bevat dan ze nodig hebben ophalen
    - VM's worden afgesloten wanneer deze niet in gebruik
    - Waarbij alleen een subset van de VM-exemplaargrootten voor de specifieke werkbelastingen

## <a name="labs-workshops-trainings-and-hackathons"></a>Labs, workshops, trainingen en hackathons  
Een lab in Azure DevTest Labs fungeert als een geweldige container voor tijdelijke activiteiten zoals workshops, praktijksessies, training of hackathons. De service kunt u het maken van een lab waarin u aangepaste sjablonen die elke leerling gebruiken kunt voor het maken van identieke en geïsoleerde omgevingen voor training kunt opgeven. U kunt beleid om ervoor te zorgen dat training omgevingen beschikbaar voor elke leerling zijn alleen wanneer ze deze nodig hebt en voldoende resources - zoals virtuele machines - vereiste voor de training bevatten toepassen. Ten slotte kunt u de testomgeving eenvoudig delen met stagiairs, die ze in één klik kunnen openen. Nadat de klasse heeft gesloten, is het gemakkelijk in het lab en alle gerelateerde resources te verwijderen.


## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel in deze serie: [schalen van uw implementatie DevTest Labs](devtest-lab-guidance-scale.md)
