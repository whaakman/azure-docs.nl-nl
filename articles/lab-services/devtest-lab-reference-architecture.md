---
title: Reference architecture voor Azure DevTest Labs voor een onderneming
description: Dit artikel bevat een verwijzing architectuurrichtlijnen voor Azure DevTest Labs in een onderneming.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/12/2019
ms.author: spelluru
ms.reviewer: christianreddington,anthdela,juselph
ms.openlocfilehash: bcb154f7cffb92ef23fc2606e1f604bb12f8d1a3
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59996608"
---
# <a name="azure-devtest-labs---reference-architecture-for-an-enterprise"></a>Azure DevTest Labs - referentiearchitectuur voor een onderneming
In dit artikel biedt een referentiearchitectuur voor het implementeren van een oplossing op basis van Azure DevTest Labs in een onderneming. Het bevat on-premises connectiviteit via Expressroute, een extern bureaublad-gateway voor virtuele machines op afstand aan te melden, verbinding met een artefactopslagplaats voor privé-artefacts en andere PaaS-services die worden gebruikt in een testomgeving.

![Referentiearchitectuur](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Architectuur
De belangrijkste elementen in de referentiearchitectuur zijn:

- **Azure Active Directory (AAD)**: Azure DevTest Labs gebruikt de [service van Azure Active Directory voor identiteitsbeheer](../active-directory/fundamentals/active-directory-whatis.md). Er zijn twee belangrijke aandachtspunten voor toegang tot een omgeving op basis van DevTest Labs voor gebruikers:
    - **Resourcebeheer**:  U krijgt toegang tot de Azure-portal om resources te beheren (virtuele machines maken, het maken van omgevingen, starten/stoppen/opnieuw opstarten/verwijderen/applyartifacts, enzovoort). Dit wordt gedaan in Azure met behulp van Roble gebaseerd toegangsbeheer (RBAC) en door het toepassen van een roltoewijzing voor de gebruiker, de instelling resource en de machtigingen op het niveau van toegang.
    - **Virtuele machines (op netwerkniveau)**:  Virtuele machines gebruiken in de standaardconfiguratie, een lokaal Administrator-account.  Als er een domein beschikbaar ([AAD Domain services](../active-directory-domain-services/active-directory-ds-overview.md), een on-premises domein of een cloud-domein), machines kunnen worden toegevoegd aan het domein. Eenmaal is toegevoegd, kunnen gebruikers hun identiteit op basis van een domein zou gebruiken verbinding maken met de virtuele machines.
- **On-Premises connectiviteit**: In de bovenstaande Architectuurdiagram [Express Route](../expressroute/expressroute-introduction.md) i gebruikt, maar u kunt ook een [Site-naar-Site VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md). Hoewel het geen vereiste voor DevTest Labs is, het vaak zichtbaar in ondernemingen. Het is alleen vereist als er een reden is voor het verbinding maken met bedrijfsbronnen. Veelvoorkomende oorzaken zijn: 
    - On-premises gegevens die nog naar de cloud kan niet worden verplaatst
    - Voorkeur van het lab-virtuele machines toevoegen aan de on-premises domein
    - Alle netwerkverkeer binnen en buiten de cloudomgeving via een on-premises firewall voor beveiliging/nalevingsredenen forceren
- **Netwerkbeveiligingsgroepen**: Een veelgebruikte manier voor het beperken van verkeer naar de cloudomgeving (of binnen de cloudomgeving) op basis van de bron en doel-IP-adressen is het gebruik van een [netwerkbeveiligingsgroep](../virtual-network/security-overview.md). Bijvoorbeeld, zodat alleen het netwerkverkeer dat afkomstig is van het bedrijfsnetwerk in netwerken van het lab.
- **Extern bureaublad-gateway**:  Ondernemingen blokkeren doorgaans uitgaande verbindingen met extern bureaublad op de firewall van het bedrijf. Om in te schakelen verbinding met de cloud gebaseerde omgeving in DevTest Labs, er zijn verschillende opties, zoals het gebruik een [extern bureaublad-gateway](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture) (whitelist het statische IP-adres voor de gateway load balancer) of [zodat alle binnenkomende RDP-verkeer](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) via de Express Route/Site-naar-Site VPN-verbinding. Het is een algemene overweging bij het plannen van een implementatie van DevTest Labs in de onderneming.
- **Azure-netwerken (VNets, subnetten)**:  De [Azure-netwerken](../networking/networking-overview.md) topologie is een ander belangrijk onderdeel voor de algehele architectuur van DevTest Labs. Hiermee kunt resources van labs om te communiceren (of niet), toegang tot on-premises (of niet) en toegang tot internet (of niet). Het architectuurdiagram bevat de meest voorkomende manier klanten maken gebruik van DevTest Labs (alle labs die zijn verbonden via [VNet-Peering](../virtual-network/virtual-network-peering-overview.md) met behulp van een [hub-spoke-model](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) naar de Express Route/Site-naar-Site VPN-verbinding met on-premises), maar sinds DevTest Labs maakt gebruik van Azure Networking direct er geen beperkingen voor het instellen van de netwerkinfrastructuur.
- **DevTest Labs**:  DevTest Labs is een belangrijk onderdeel van de algehele architectuur. Zie voor meer informatie over de service, [over DevTest Labs](devtest-lab-overview.md).
- **Virtuele machines en andere bronnen (SaaS, PaaS, IaaS)**:  Een van de belangrijke werkbelastingen, ondersteund door DevTest Labs zijn virtuele machines samen met andere Azure-resources.  DevTest Labs kunt u eenvoudig en snel een onderneming toegang geven tot Azure-resources (inclusief virtuele Machines en andere Azure-Resources).  Meer informatie over toegang tot Azure voor [ontwikkelaars](devtest-lab-developer-lab.md) en [testers](devtest-lab-test-env.md).

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen
Hoewel DevTest Labs niet beschikt over een ingebouwde quotums of limieten, andere Azure-resources worden gebruikt in de normale bewerkingen van een lab hebt [abonnementsniveau quota](../azure-subscription-service-limits.md). Als gevolg hiervan in een standaard enterprise-implementatie, moet u meerdere Azure-abonnementen voor een grote implementatie van DevTest Labs hebben. De quota die meestal wordt bereikt door ondernemingen zijn:

- **Resourcegroepen**:  In de standaardconfiguratie wordt een resourcegroep voor elke nieuwe virtuele machine of een omgeving die een gebruiker maakt gebruik van de service gemaakt met DevTest Labs. Abonnementen kunnen bevatten een [maximaal 980 resourcegroepen](../azure-subscription-service-limits.md#subscription-limits---azure-resource-manager), zodat deze limiet de bovengrens van virtuele machines en omgevingen in een abonnement is. Er zijn twee andere configuraties die u moet overwegen:
    - **[Alle virtuele machines gaat u naar dezelfde resourcegroep](resource-group-control.md)**:  Hoewel deze u bij de groep resourcelimiet helpt, dit het resourcetype per groep resourcelimiet invloed heeft op.
    - **Openbare IP-adressen met behulp van gedeelde**:  Alle virtuele machines van dezelfde grootte en dezelfde regio gaan in dezelfde resourcegroep bevinden. Het is een 'midden' tussen resourcequota groep en het resourcetype per groep resourcequota als virtuele machines zijn toegestaan voor het openbare IP-adressen hebben. 
- **Resources per resource per resourcetype groep**: De standaardlimiet voor [bronnen per resource groep per resourcetype is 800](../azure-subscription-service-limits.md#resource-group-limits).  Wanneer met behulp van de alle virtuele machines gaat u naar de configuratie van de dezelfde resource, gebruikers wordt wordt deze abonnementslimiet bereikt veel sneller, met name als de virtuele machines veel extra schijven hebben.
- **Storage-accounts**: Een lab in DevTest Labs wordt geleverd met een opslagaccount en de Azure-quotum voor [aantal storage-accounts per regio per abonnement is 250](../azure-subscription-service-limits.md#storage-limits). Dit betekent dat de bovenste limiet voor het aantal DevTest Labs in dezelfde regio ook 250.
- **Roltoewijzingen**: Een roltoewijzing wordt beschreven hoe u een gebruiker of -principal toegang te verlenen tot een resource (eigenaar, resource, machtigingsniveau). In Azure, er een [limiet van 2000 roltoewijzingen per abonnement](../azure-subscription-service-limits.md#role-based-access-control-limits). De DevTest Labs-service (in de standaardconfiguratie) wordt een resourcegroep gemaakt voor elke virtuele machine en de eigenaar wordt verleend de **eigenaar** machtiging voor de DevTest Labs-VM en **lezer** machtiging voor de resourcegroep.  Elke nieuwe VM gebruikt op deze manier kunnen twee extra roltoewijzingen te roltoewijzingen gemaakt wanneer gebruikers toestemming om het lab te geven.
- **Lees-/ schrijfbewerkingen API**: Er zijn verschillende manieren (REST-API's, PowerShell, CLI, Azure SDK, enzovoort) in Azure en DevTest Labs te automatiseren en via de automatisering is het mogelijk om een andere limiet voor API-aanvragen. Elk abonnement kan maximaal [12000 lezen schrijfaanvragen aanvragen en 1200 per uur](../azure-resource-manager/resource-manager-request-limits.md).  Er is een limiet rekening mee moet houden bij het automatiseren van DevTest Labs.

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen
DevTest Labs heeft een interface geweldige gebruiker met beheerdersrechten als u werkt met een enkele lab. In een onderneming zijn er waarschijnlijk meerdere Azure-abonnementen en veel labs. Dit betekent dat de wijzigingen aanbrengen in alle labs consistent is vereist voor scripting/automation.  Hier volgen enkele voorbeelden en aanbevolen methoden voor het beheren van de DevTest Labs-implementatie:

- **Wijzigingen in de instellingen van het lab**: Een gebruikelijk scenario is het bijwerken van een specifieke lab-instelling in de labs in de implementatie. Bijvoorbeeld, een nieuwe VM-instantiegrootte is beschikbaar en alle labs moeten worden bijgewerkt zodat deze.  Het is raadzaam deze wijzigingen automatiseren met behulp van Azure PowerShell-scripts, Azure CLI of REST-API's.  
- **Artefact opslagplaats persoonlijk toegangstoken**:  Normaal gesproken verlopen persoonlijke toegangstokens voor een Git-opslagplaats (90 dagen, 1 Jr 2 jaar). Om continuïteit te waarborgen, is het belangrijk dat u het persoonlijke toegangstoken uitbreiden of maak een nieuwe, en om automatisering naar de nieuwe persoonlijke toegangstoken van toepassing op alle labs gebruiken.
- **Beperken van de wijzigingen voor een testomgeving**:  Het is vaak het geval waarbij een bepaalde instelling (bijvoorbeeld, waardoor Marketplace-installatiekopieën moet worden gebruikt) die beperkt worden moet. Kan worden gedaan via de Azure policy (geen wijzigingen aan dit brontype) of door het maken van een aangepaste rol en het verlenen van die rol in plaats van 'eigenaar' aan het lab. Het kan worden gedaan voor het merendeel van de instellingen in het lab (ondersteuning voor interne, Lab-aankondiging, toegestaan VM-grootten, enzovoort)
- **Vereisen van virtuele machines een naamconventie volgen**: Het is een algemene aanvraag voor het herkennen van virtuele machines die deel van de cloud gebaseerde ontwikkel- en testomgeving uitmaken. U kunt dit doen [met Azure Policy](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns).

Het is belangrijk te weten dat DevTest Labs onderliggende resources in Azure (netwerk, schijven, berekenen, enzovoort) die worden beheerd in Azure op dezelfde manier gebruikt.  Bijvoorbeeld, Azure policy is van toepassing op virtuele Machines die zijn gemaakt in een testomgeving. Het Azure Security center kan rapporteren dat op naleving van de virtuele machine. De Azure Backup-Service kan bieden van regelmatige back-ups voor virtuele machines in het lab, enzovoort. 

## <a name="security-considerations"></a>Beveiligingsoverwegingen
Azure DevTest Labs maakt gebruik van bestaande resources in Azure (compute, netwerken, enzovoort) en dus automatisch profiteren van alle de uitstekende geïntegreerde beveiligingsfuncties voor het platform. Als u wilt beveiligen alle binnenkomende verbindingen met extern bureaublad alleen afkomstig te zijn van het bedrijfsnetwerk bevinden, is het bijvoorbeeld net zo eenvoudig als een netwerkbeveiligingsgroep toe te voegen aan het virtuele netwerk op de extern bureaublad-gateway. De enige extra beveiliging voor Azure DevTest Labs is het niveau van de machtigingen die zijn opgegeven aan teamleden die de labs wordt gebruikt op basis van de dag tot dag.  De algemene machtigingen die zijn ['Eigenaar' en 'DevTest Labs-gebruiker'](devtest-lab-add-devtest-user.md). Zie voor meer informatie over deze rollen, [eigenaars en gebruikers toevoegen in Azure DevTest Labs](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel in deze reeks: [Schalen van uw infrastructuur voor Azure DevTest Labs](devtest-lab-guidance-scale.md)
