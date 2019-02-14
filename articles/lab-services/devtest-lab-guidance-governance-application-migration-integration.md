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
ms.date: 02/11/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: c57385bc398544e786aec77dae9886784e3a0b3c
ms.sourcegitcommit: b3d74ce0a4acea922eadd96abfb7710ae79356e0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/14/2019
ms.locfileid: "56243617"
---
# <a name="governance-of-azure-devtest-labs-infrastructure---application-migration-and-integration"></a>Beheer van infrastructuur voor Azure DevTest Labs - integratie en toepassingen migreren
Zodra uw ontwikkelen/testen-testomgeving is tot stand is gebracht, moet u echter nadenken over de volgende vragen:

- Hoe u de omgeving gebruiken binnen uw team?
- Hoe u ervoor zorgen dat u al de vereiste organisatie-beleidsregels en onderhouden van de flexibiliteit om toe te voegen waarde aan uw toepassing?

## <a name="azure-marketplace-images-vs-custom-images"></a>Azure Marketplace-installatiekopieën en aangepaste installatiekopieën

### <a name="question"></a>Vraag
Wanneer moet ik een Azure Marketplace-installatiekopie gebruiken versus mijn eigen aangepaste installatiekopie van de organisatie?

### <a name="answer"></a>Antwoord
Azure Marketplace moet worden gebruikt standaard, tenzij u specifieke belangen of organisatorische vereisten hebt. Enkele algemene voorbeelden zijn;

- Setup van complexe software waarvoor een toepassing moet worden opgenomen als onderdeel van de basisinstallatiekopie.
- Installatie en configuratie van een toepassing kunnen vele uren, die niet een efficiënt gebruik van de tijd om te worden toegevoegd aan een Azure Marketplace-installatiekopie krijgen.
- Ontwikkelaars en testers snel toegang tot een virtuele machine vereist en wilt minimaliseren, de insteltijd van een nieuwe virtuele machine.
- Nalevings- of regelgeving voorwaarden (bijvoorbeeld beveiligingsbeleid) die voldaan voor alle machines worden moeten.

Met behulp van aangepaste installatiekopieën mogen niet worden beschouwd als licht. Ze voeren extra complexiteit, zoals u nu hebt voor het beheren van VHD-bestanden voor deze onderliggende basisinstallatiekopieën. U moet ook regelmatig patch die basisinstallatiekopieën met software-updates. Deze updates bevatten nieuwe updates van het besturingssysteem (OS), en eventuele updates of wijzigingen in de configuratie die nodig zijn voor de software-updatepakket zelf.

## <a name="formula-vs-custom-image"></a>Formule versus aangepaste installatiekopie

### <a name="question"></a>Vraag
Wanneer moet ik een formule versus aangepaste installatiekopie gebruiken?

### <a name="answer"></a>Antwoord
Normaal gesproken de beslissende factor in dit scenario zijn kosten verbonden en opnieuw te gebruiken.

Als u een scenario waarbij veel gebruikers/labs een afbeelding met een groot aantal software boven op de basisinstallatiekopie nodig hebt, kan u kosten verlagen door een aangepaste installatiekopie maken. Dit betekent dat de installatiekopie van het eenmaal is gemaakt. Dit vermindert de insteltijd van de virtuele machine en de kosten in rekening gebracht omdat de virtuele machine uitvoeren bij de instelling.

Een extra factor te weten is echter de frequentie van wijzigingen in uw softwarepakket. Als u wordt dagelijks bouwt en vereist dat software op uw gebruikers virtuele machines, kunt u overwegen een formule in plaats van een aangepaste installatiekopie.

## <a name="use-custom-organizational-images"></a>Gebruik aangepaste installatiekopieën van de organisatie

### <a name="question"></a>Vraag
Hoe kan ik een eenvoudig herhaalbaar proces instellen om mijn aangepaste installatiekopieën van de organisatie in een omgeving met DevTest Labs?

### <a name="answer"></a>Antwoord
Zie [in deze video van afbeelding Factory patroon](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/). Dit scenario is een geavanceerd scenario en de geleverde scripts zijn alleen voorbeelden van scripts. Als wijzigingen vereist zijn, die u wilt beheren en onderhouden van de scripts in uw omgeving gebruikt.

DevTest Labs gebruiken voor het maken van een aangepaste installatiekopie-pijplijn in Azure pijplijnen:

- [Introductie: Bereid u voor virtuele machines in enkele minuten door het instellen van een installatiekopie factory in Azure DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2016/09/14/introduction-get-vms-ready-in-minutes-by-setting-up-image-factory-in-azure-devtest-labs/)
- [Afbeelding Factory – deel 2. Azure pijplijnen en Factory Lab voor het maken van virtuele machines instellen](https://blogs.msdn.microsoft.com/devtestlab/2017/10/25/image-factory-part-2-setup-vsts-to-create-vms-based-on-devtest-labs/)
- [Afbeelding Factory – deel 3: Aangepaste installatiekopieën opslaan en distribueren naar meerdere Labs](https://blogs.msdn.microsoft.com/devtestlab/2018/01/10/image-factory-part-3-save-custom-images-and-distribute-to-multiple-labs/)
- [Video: Aangepaste installatiekopie Factory met Azure DevTest Labs](https://blogs.msdn.microsoft.com/devtestlab/2017/04/17/video-custom-image-factory-with-azure-devtest-labs/)

## <a name="patterns-to-set-up-network-configuration"></a>Patronen voor het instellen van de netwerkconfiguratie

### <a name="question"></a>Vraag
Hoe zorg ervoor dat ontwikkeling en testen van virtuele machines ik weet kan niet worden bereikt van het openbare internet? Zijn er aanbevolen patronen voor het instellen van de configuratie van het netwerk?

### <a name="answer"></a>Antwoord
Ja. Er zijn twee aspecten om te overwegen: binnenkomend en uitgaand verkeer.

**Binnenkomend verkeer** : als de virtuele machine geen openbaar IP-adres, heeft en vervolgens deze kan niet worden bereikt door het internet. Er is een veelgebruikte manier om ervoor te zorgen dat een beleid op abonnementsniveau is ingesteld, zodat er geen gebruiker kan een openbaar IP-adres maken.

**Uitgaand verkeer** : als u wilt voorkomen dat virtuele machines gaan direct naar het openbare internet en afdwingen dat verkeer via een firewall van het bedrijf, en u verkeer on-premises via expressroute versturen kunt of VPN, met behulp van routering geforceerde.

> [!NOTE]
> Hebt u een proxyserver die verkeer zonder proxy-instellingen wordt geblokkeerd, vergeet niet om toe te voegen uitzonderingen op van het lab artefact storage-account.

U kunt ook netwerkbeveiligingsgroepen voor virtuele machines of subnetten. Deze stap voegt een extra beschermingslaag toestaan / blokkeren van verkeer.

## <a name="new-vs-existing-virtual-network"></a>Nieuw of bestaand virtueel netwerk

### <a name="question"></a>Vraag
Wanneer moet ik een nieuw virtueel netwerk maken voor mijn DevTest Labs-omgeving versus met behulp van een bestaand virtueel netwerk?

### <a name="answer"></a>Antwoord
Als uw VM's nodig hebt om te communiceren met de bestaande infrastructuur, moet klikt u vervolgens u overwegen een bestaand virtueel netwerk in uw omgeving DevTest Labs. Bovendien, als u ExpressRoute gebruikt, kunt u Beperk het aantal vnet's / subnetten zodat u uw IP-adresruimte die wordt toegewezen voor gebruik in de abonnementen niet fragment. U moet ook overwegen om gebruik van het VNet peering patroon hier (Hub-Spoke-model). Deze aanpak maakt vnet/subnet communicatie mogelijk tussen abonnementen binnen een bepaalde regio, hoewel peering met andere regio's een functie van afkomstig in Azure-netwerken is.

Elke DevTest Labs-omgeving kan anders zijn eigen virtueel netwerk hebt. Echter, houd er rekening mee dat er [limieten](../azure-subscription-service-limits.md) op het aantal virtuele netwerken per abonnement. De standaardinstelling is 50, hoewel deze limiet kan worden verhoogd tot 100.

## <a name="shared-public-or-private-ip"></a>Gedeelde, openbare of privé-IP

### <a name="question"></a>Vraag
Wanneer moet ik een gedeelde IP-adres versus openbaar IP-adres versus privé IP-adres gebruiken?

### <a name="answer"></a>Antwoord
Als u een site-naar-site VPN of Express Route gebruikt, overweeg het gebruik van privé IP-adressen, zodat uw machines toegankelijk via het interne netwerk en toegankelijk zijn via openbare internet zijn.

> [!NOTE]
> Lab-eigenaars kunnen dit subnetbeleid om ervoor te zorgen dat niemand openbare IP-adressen per ongeluk worden gemaakt voor hun VM's wijzigen. Eigenaar van het abonnement moet maken van een beleid van het abonnement zo wordt voorkomen dat openbare IP-adressen die worden gemaakt.

Bij het gebruik van gedeelde openbare IP-adressen, delen de virtuele machines in een testomgeving een openbaar IP-adres. Deze methode kan nuttig zijn wanneer u nodig hebt om te voorkomen dat inbreuk op de limieten voor openbare IP-adressen voor een bepaald abonnement.

## <a name="limits-of-number-of-virtual-machines-per-user-or-lab"></a>Grenzen van het aantal virtuele machines per gebruiker of het lab

### <a name="question"></a>Vraag
Is er een regel in termen van het aantal virtuele machines dat moet ik per gebruiker of per lab instellen?

### <a name="answer"></a>Antwoord
Wanneer u overweegt het aantal virtuele machines per gebruiker of per lab, zijn er drie belangrijkste zorgen:

- De **totale kosten** die het team kunt besteden aan resources in het lab. Het is gemakkelijk om te veel computers activeren. Voor het beheren van kosten, is een mechanisme om te beperken van het aantal virtuele machines per gebruiker en/of per lab
- Het totale aantal virtuele machines in een lab wordt beïnvloed door de [niveau abonnementquota](../azure-subscription-service-limits.md) beschikbaar. Een van de bovenste limieten is 800 resourcegroepen per abonnement. DevTest Labs wordt momenteel een nieuwe resourcegroep voor elke virtuele machine (tenzij gedeelde openbare IP-adressen worden gebruikt). Als er 10 labs in een abonnement, labs ongeveer 79 virtuele machines in elke lab (800 bovengrens – 10 resourcegroepen voor de 10 labs zelf) kunnen passen = 79 virtuele machines per lab.
- Als het lab is verbonden met on-premises via de Express-Route (bijvoorbeeld), zijn er **gedefinieerd IP-adresruimten beschikbaar** voor het VNet/Subnet. Om ervoor te zorgen dat virtuele machines in de testomgeving niet mislukken moet worden gemaakt (fout: kan de IP-adres niet ophalen), lab-eigenaren de maximale VM's per lab uitgelijnd met de beschikbare IP-adresruimte kunnen opgeven.

## <a name="use-resource-manager-templates"></a>Resource Manager-sjablonen gebruiken

### <a name="question"></a>Vraag
Hoe kan ik Resource Manager-sjablonen gebruiken in mijn DevTest Labs-omgeving?

### <a name="answer"></a>Antwoord
Implementeren van uw Resource Manager-sjablonen in een DevTest Labs met behulp van de stappen in de [omgevingen-functie in DevTest labs](devtest-lab-test-env.md) artikel. In principe u uw Resource Manager-sjablonen worden ingecheckt bij een Git-opslagplaats (Azure-opslagplaatsen of GitHub) en voegt een [privé-opslagplaats voor uw sjablonen](devtest-lab-test-env.md) aan het lab.

In dit scenario mogelijk niet handig als u DevTest Labs voor ontwikkeling hostmachines gebruikt, maar zijn mogelijk nuttig als u bouwt een faseringsomgeving die representatief voor productie is.

Het is ook het vermelden waard dat het aantal virtuele machines per lab of per Gebruikersoptie alleen het aantal machines systeemeigen die zijn gemaakt in de testomgeving zelf en niet door alle omgevingen (Resource Manager-sjablonen beperkt).

## <a name="next-steps"></a>Volgende stappen
Zie [omgevingen gebruiken in DevTest Labs](devtest-lab-test-env.md).