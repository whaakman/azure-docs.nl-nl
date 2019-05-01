---
title: Enterprise reference architecture voor Azure DevTest Labs
description: Dit artikel bevat een verwijzing architectuurrichtlijnen voor het Azure DevTest Labs in een onderneming.
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
ms.openlocfilehash: 73a3d426e9040525b0c631db273e59c49a6a9eb0
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64705885"
---
# <a name="azure-devtest-labs-reference-architecture-for-enterprises"></a>Azure DevTest Labs-referentiearchitectuur voor ondernemingen
Dit artikel bevat referentie-architectuur om u te helpen bij het implementeren van een oplossing op basis van Azure DevTest Labs in een onderneming. Dit omvat het volgende:
- On-premises connectiviteit via Azure ExpressRoute
- Een extern bureaublad-gateway extern aanmelden bij virtuele machines
- Verbinding met een artefactopslagplaats voor privé-artefacts
- Andere PaaS-services die worden gebruikt in labs

![Diagram van de referentie-architectuur](./media/devtest-lab-reference-architecture/reference-architecture.png)

## <a name="architecture"></a>Architectuur
Dit zijn de belangrijkste elementen van de referentiearchitectuur:

- **Azure Active Directory (Azure AD)**: DevTest Labs gebruikt de [Azure AD-service voor identiteitsbeheer](../active-directory/fundamentals/active-directory-whatis.md). Wanneer u gebruikers toegang tot een omgeving op basis van DevTest Labs verleent, houd rekening met deze twee belangrijke aspecten:
    - **Resourcebeheer**: U krijgt toegang tot de Azure-portal om resources te beheren (virtuele machines maken; omgevingen maken; starten, stoppen, starten, verwijderen, en artefacten; toepassen en enzovoort). Resourcebeheer is gedaan in Azure met behulp van op rollen gebaseerd toegangsbeheer (RBAC). U stelt resource toegangsniveau machtigingen en rollen toewijzen aan gebruikers.
    - **Virtuele machines (op netwerkniveau)**: Virtuele machines gebruiken in de standaardconfiguratie, een lokaal Administrator-account. Als er een domein beschikbaar ([Azure AD Domain Services](../active-directory-domain-services/active-directory-ds-overview.md), een on-premises domein of een cloud-domein), machines kunnen worden toegevoegd aan het domein. Gebruikers kunnen vervolgens hun domein op basis van identiteit verbinding maken met de virtuele machines gebruiken.
- **On-premises connectiviteit**: In onze Architectuurdiagram [ExpressRoute](../expressroute/expressroute-introduction.md) wordt gebruikt. Maar u kunt ook een [site-naar-site VPN](../vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md). Hoewel ExpressRoute is niet vereist voor DevTest Labs, wordt het doorgaans gebruikt in ondernemingen. ExpressRoute is alleen vereist als u toegang tot bedrijfsbronnen nodig. Algemene scenario's zijn:
    - U hebt een on-premises gegevens die naar de cloud kan niet worden verplaatst.
    - U wilt de van de virtuele machines toevoegen aan de on-premises domein.
    - U wilt afdwingen dat alle netwerkverkeer binnen en buiten de cloudomgeving via een on-premises firewall voor beveiliging/naleving.
- **Netwerkbeveiligingsgroepen**: Een veelgebruikte manier voor het beperken van verkeer naar de cloudomgeving (of binnen de cloudomgeving) op basis van de bron en doel-IP-adressen is het gebruik van een [netwerkbeveiligingsgroep](../virtual-network/security-overview.md). Bijvoorbeeld, wilt u toestaan alleen verkeer dat afkomstig van het bedrijfsnetwerk in netwerken van het lab is.
- **Extern bureaublad-gateway**: Ondernemingen blokkeren doorgaans uitgaande verbindingen met extern bureaublad op de firewall van het bedrijf. Er zijn verschillende opties om verbindingen met de cloud-gebaseerde-omgeving in DevTest Labs, met inbegrip van:
  - Gebruik een [extern bureaublad-gateway](/windows-server/remote/remote-desktop-services/desktop-hosting-logical-architecture), en load balancer-whitelist het statische IP-adres van de gateway.
  - [Alle binnenkomende RDP-verkeer te regelen](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) via de ExpressRoute/site-naar-site VPN-verbinding. Deze functionaliteit is een algemene overweging wanneer ondernemingen een DevTest Labs-implementatie plannen.
- **Netwerkservices (virtuele netwerken, subnetten)**: De [Azure-netwerken](../networking/networking-overview.md) topologie is een andere belangrijke elementen in de DevTest Labs-architectuur. Deze Hiermee bepaalt u of de resources van het lab kunnen communiceren en toegang tot on-premises en het internet hebben. Onze Architectuurdiagram bevat de meest gebruikte manieren die klanten DevTest Labs gebruiken: Alle labs maken verbinding via [peering op virtueel netwerk](../virtual-network/virtual-network-peering-overview.md) met behulp van een [hub-spoke-model](/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) aan de ExpressRoute/site-naar-site VPN-verbinding met on-premises. Maar DevTest Labs gebruikt Azure Virtual Network direct, dus er geen beperkingen zijn voor hoe u de netwerkinfrastructuur instellen.
- **DevTest Labs**:  DevTest Labs is een belangrijk onderdeel van de algehele architectuur. Zie voor meer informatie over de service, [over DevTest Labs](devtest-lab-overview.md).
- **Virtuele machines en andere bronnen (SaaS, PaaS, IaaS)**:  Virtuele machines zijn een belangrijke werkbelasting die ondersteuning biedt voor DevTest Labs, samen met andere Azure-resources. DevTest Labs kunt u eenvoudig en snel voor een enterprise voor toegang tot Azure-resources (inclusief virtuele machines en andere Azure-resources). Meer informatie over toegang tot Azure voor [ontwikkelaars](devtest-lab-developer-lab.md) en [testers](devtest-lab-test-env.md).

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen
Hoewel DevTest Labs niet beschikt over ingebouwde quotums of limieten, andere Azure-resources die worden gebruikt in de normale werking van een lab hebt [abonnementsniveau quota](../azure-subscription-service-limits.md). Dus in een standaard enterprise-implementatie moet u meerdere Azure-abonnementen voor een grote implementatie van DevTest Labs. De quota die bedrijven meestal bereiken zijn:

- **Resourcegroepen**: In de standaardconfiguratie DevTest Labs wordt een resourcegroep voor elke nieuwe virtuele machine of de gebruiker maakt een omgeving met behulp van de service. Abonnementen kunnen bevatten [tot 980 resourcegroepen](../azure-subscription-service-limits.md#subscription-limits---azure-resource-manager). En is dat de limiet van virtuele machines en omgevingen in een abonnement. Er zijn twee andere configuraties die u moet overwegen:
    - **[Alle virtuele machines gaat u naar dezelfde resourcegroep](resource-group-control.md)**: Hoewel deze instelling u voldoen aan de groep resourcelimiet kunt, het is van invloed op de resource-type-per-resource-group-limiet.
    - **Openbare IP-adressen met behulp van gedeelde**: Alle virtuele machines van dezelfde grootte en hetzelfde regio gaan in dezelfde resourcegroep bevinden. Deze configuratie is een 'midden' tussen groep resourcequota en resource-type-per-resource-group-quota, als de virtuele machines zijn toegestaan voor het openbare IP-adressen hebben.
- **Resources per resource per resourcetype groep**: De standaardlimiet voor [resources per resourcegroep per resourcetype is 800](../azure-subscription-service-limits.md#resource-group-limits).  Wanneer u gebruikt de *alle virtuele machines gaat u naar dezelfde resourcegroep* configuratie, drukt u op gebruikers dit abonnement veel eerder beperken, met name als de virtuele machines hebben veel extra schijven.
- **Storage-accounts**: Een lab in DevTest Labs wordt geleverd met een opslagaccount. De Azure-quotum voor [aantal storage-accounts per regio per abonnement is 250](../azure-subscription-service-limits.md#storage-limits). Het maximum aantal DevTest Labs in dezelfde regio is ook 250.
- **Roltoewijzingen**: Een roltoewijzing is hoe u een gebruiker of -principal toegang verlenen tot een resource (eigenaar, resource, machtigingsniveau). In Azure, er is een [limiet van 2000 roltoewijzingen per abonnement](../azure-subscription-service-limits.md#role-based-access-control-limits). De DevTest Labs-service maakt standaard een resourcegroep voor elke virtuele machine. De eigenaar is verleend *eigenaar* machtiging voor de DevTest Labs-VM en *lezer* machtigingen voor de resourcegroep. Op deze manier maakt gebruik van elke nieuwe VM die u maakt twee roltoewijzingen naast de toewijzingen die worden gebruikt wanneer u gebruikers voor het lab machtigen.
- **Lees-/ schrijfbewerkingen API**: Er zijn verschillende manieren voor het automatiseren van Azure en DevTest Labs, met inbegrip van de REST-API's, PowerShell, Azure CLI en Azure SDK. U kunt een andere limiet op API-aanvragen mogelijk bereikt door middel van automatisering: Elk abonnement kan maximaal [12.000 lezen schrijfaanvragen aanvragen en 1200 per uur](../azure-resource-manager/resource-manager-request-limits.md). Houd rekening met deze limiet bij het automatiseren van DevTest Labs.

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen
DevTest Labs is een geweldige gebruiker met beheerdersrechten-interface voor het werken met een enkele lab. Maar in een onderneming, waarschijnlijk hebt u meerdere Azure-abonnementen en veel labs. Consistent wijzigingen aanbrengen in alle uw labs vereist scripting/automation. Hier volgen enkele voorbeelden en aanbevolen beheerprocedures voor een DevTest Labs-implementatie:

- **Wijzigingen in de instellingen van het lab**: Een gebruikelijk scenario is het bijwerken van een specifieke labomgeving in alle labs in de implementatie. Bijvoorbeeld, een nieuwe VM-instantiegrootte is beschikbaar, en alle labs moeten worden bijgewerkt zodat deze. Het is raadzaam deze wijzigingen automatiseren met behulp van PowerShell-scripts, CLI of REST-API's.  
- **Artefact opslagplaats persoonlijk toegangstoken**:  Normaal gesproken verlopen persoonlijke toegangstokens voor een Git-opslagplaats in 90 dagen, één, of twee jaar. Om continuïteit te waarborgen, is het belangrijk voor het uitbreiden van het persoonlijke toegangstoken of een nieuwe maken. Vervolgens gebruiken automation voor het nieuwe persoonlijke toegangstoken van toepassing op alle labs.
- **Wijzigingen beperken tot een lab-instelling**: Een bepaalde instelling moet vaak worden beperkt (zoals het toestaan van het gebruik van marketplace-installatiekopieën). U kunt Azure Policy gebruiken om te voorkomen dat wijzigingen aan een brontype. Of u kunt een aangepaste rol maken en gebruikers verlenen die rol in plaats van de *eigenaar* rol voor de testomgeving. U kunt dit doen voor de meeste instellingen in het lab (ondersteuning voor interne, lab-aankondiging, toegestane VM-grootten, enzovoort).
- **Vereisen dat VM's naar een naamconventie volgen**: Managers wilt vaak om eenvoudig virtuele machines die deel van een cloud-gebaseerde ontwikkelings- en testomgeving uitmaken te identificeren. U kunt dit doen met behulp van [Azure Policy](https://github.com/Azure/azure-policy/tree/master/samples/TextPatterns/allow-multiple-name-patterns).

Het is belangrijk te weten dat gebruikmaakt van DevTest Labs onderliggende Azure-resources die op dezelfde wijze worden beheerd: netwerk, schijven, compute, enzovoort. Bijvoorbeeld, Azure Policy is van toepassing op virtuele machines die zijn gemaakt in een testomgeving. Azure Security Center kan rapporteren dat op naleving van de virtuele machine. En de Azure Backup-service krijgt u een regelmatige back-ups voor de virtuele machines in het lab.

## <a name="security-considerations"></a>Beveiligingsoverwegingen
Azure DevTest Labs maakt gebruik van bestaande resources in Azure (compute, netwerken, enzovoort). Dus profiteren automatisch van de beveiligingsfuncties die zijn ingebouwd in het platform. Bijvoorbeeld, om te vereisen dat binnenkomende verbindingen met extern bureaublad afkomstig te zijn alleen van het bedrijfsnetwerk bevinden, Voeg een netwerkbeveiligingsgroep naar het virtuele netwerk op de extern bureaublad-gateway. De enige extra uit veiligheidsoverwegingen is het niveau van de machtigingen die u toewijst aan teamleden die gebruikmaken van de labs op dagelijks. De meest voorkomende machtigingen zijn [ *eigenaar* en *gebruiker*](devtest-lab-add-devtest-user.md). Zie voor meer informatie over deze rollen, [eigenaars en gebruikers toevoegen in Azure DevTest Labs](devtest-lab-add-devtest-user.md).

## <a name="next-steps"></a>Volgende stappen
Zie het volgende artikel in deze reeks: [Schalen van uw infrastructuur voor Azure DevTest Labs](devtest-lab-guidance-scale.md).
