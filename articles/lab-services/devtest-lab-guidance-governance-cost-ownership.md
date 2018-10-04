---
title: Beheer van Azure DevTest Labs-infrastructuur
description: Dit artikel bevat richtlijnen voor het beheer van Azure DevTest Labs-infrastructuur.
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
ms.openlocfilehash: 623e4392ff8ff75d09da92c54b9e4dc620146432
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48251041"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---manage-cost-and-ownership"></a>Beheer van de infrastructuur van Azure DevTest Labs - kosten en eigendom beheren
Kosten en het eigendom zijn primaire problemen wanneer u Houd rekening met het bouwen van uw ontwikkel- en testomgevingen. In deze sectie vindt u informatie waarmee u kunt optimaliseren voor kosten en eigendom uitlijnen in uw omgeving.

## <a name="optimize-for-cost"></a>Optimaliseren voor kosten

### <a name="question"></a>Vraag
Hoe kan ik optimaliseren voor kosten binnen mijn DevTest Labs-omgeving?

### <a name="answer"></a>Antwoord
Er zijn een aantal ingebouwde functies van DevTest Labs die u helpen optimaliseren voor kosten. Zie [kostenbeheer, drempels](devtest-lab-configure-cost-management.md) [, en het beleid](devtest-lab-set-lab-policy.md) artikelen activiteiten van uw gebruikers te beperken. 

Als u DevTest Labs voor een ontwikkelings- en workloads gebruiken, kunt u overwegen om gebruik van de [Enterprise Dev/Test-abonnement voordeel](https://azure.microsoft.com/offers/ms-azr-0148p/), als onderdeel van uw Enterprise Agreement. U kunt ook als u een betalen-klant bent, kunt u rekening houden met de [betaalt u Ga DevTest aanbieding](https://azure.microsoft.com/offers/ms-azr-0023p/).

Deze aanpak kunt u met profiteren van talrijke voordelen:

- Speciale lagere tarieven voor ontwikkelen/testen op Windows-virtuele machines, cloudservices, HDInsight, App Service en Logic Apps
- Geweldige Enterprise Agreement (EA)-tarieven op andere Azure-services
- Toegang tot exclusieve Dev/Test-installatiekopieën in de galerie, waaronder Windows 8.1 en Windows 10
 
Actieve Visual Studio-abonnees (standaardabonnementen, cloudjaarabonnementen en cloudmaandabonnementen) kunnen alleen Azure-bronnen in een enterprise Dev/Test-abonnement gebruiken. Eindgebruikers kunnen wel toegang tot de toepassing feedback geven of acceptatietesten uit te voeren. Gebruik van bronnen binnen dit abonnement is beperkt tot het ontwikkelen en testen van toepassingen en er wordt geen garantie voor actieve tijdsduur gegeven.

Als u wilt gebruiken de DevTest-aanbieding, houd er rekening mee dat dit voordeel is uitsluitend bedoeld voor ontwikkeling en testen van uw toepassingen. Gebruik binnen het abonnement wordt niet geleverd met een SLA met financiële garantie, behalve voor het gebruik van Visual Studio-teamservices en HockeyApp.

## <a name="define-a-role-based-access-across-your-organization"></a>Een op rollen gebaseerde toegang in uw organisatie definiëren
### <a name="question"></a>Vraag
Hoe definieer ik op rollen gebaseerd toegangsbeheer voor mijn DevTest Labs omgevingen om ervoor te zorgen dat kan IT beheren terwijl ontwikkelaars en testen hun werk kunnen doen? 

### <a name="answer"></a>Antwoord
Er is een brede patroon, maar de details, is afhankelijk van uw organisatie.

Centrale IT moet alleen wat nodig is de eigenaar en inschakelen van het project en toepassing teams om het benodigde beheerniveau. Normaal gesproken betekent dit dat centrale IT eigenaar is van het abonnement en de grepen essentiële IT-functies, zoals netwerkconfiguraties. De set **eigenaren** voor een abonnement moet zo klein zijn. Deze eigenaren kunnen extra eigenaren benoemen als nodig is, of beleidsregels op abonnementsniveau, bijvoorbeeld "Er zijn geen openbare IP' toepassen.

Het is mogelijk dat er een subset van gebruikers die toegang nodig voor een abonnement, zoals Tier1 of laag 2-ondersteuning hebben. In dit geval wordt aangeraden dat u deze gebruikers geeft de **Inzender** toegang zodat ze kunnen de bronnen beheren, maar niet gebruiker toegang krijgen of beleid.

De DevTest Labs-bron moet eigendom zijn van eigenaren die zich dicht bij de toepassing/project-team. Dit is omdat ze hun vereisten wat betreft de machines en de vereiste software begrijpen. De eigenaar van deze resource DevTest Labs is in de meeste organisaties vaak de lead project/ontwikkeling. De eigenaar van deze gebruikers en het beleid in de testomgeving kunt beheren en alle virtuele machines in de DevTest Labs-omgeving kunt beheren.

De teamleden project/toepassing moeten worden toegevoegd aan de rol van DevTest Labs-gebruikers. Deze gebruikers kunnen maken van virtuele machines (inline-lab en het abonnementsniveau). Ook kunnen ze hun eigen virtuele machines beheren. Ze kunnen geen virtuele machines die deel uitmaken van andere gebruikers beheren.

Zie voor meer informatie, [Azure enterprise-platform-prescriptieve abonnementsgovernance](/architecture/cloud-adoption/appendix/azure-scaffold) documentatie.


## <a name="next-steps"></a>Volgende stappen
Zie [zakelijke beleid en naleving](devtest-lab-guidance-governance-policy-compliance.md).