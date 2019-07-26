---
title: Een proef van concept-Azure DevTest Labs leveren | Microsoft Docs
description: Leer hoe u een proef concept kunt leveren, zodat Azure DevTest Labs met succes kan worden opgenomen in een bedrijfs omgeving.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/23/2018
ms.author: takamath
ms.openlocfilehash: 6739679b1687393737dda1ded4265a95c4fce169
ms.sourcegitcommit: a0b37e18b8823025e64427c26fae9fb7a3fe355a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2019
ms.locfileid: "68501544"
---
# <a name="delivering-a-proof-of-concept"></a>Een testen van het concept 

Een van de belangrijkste scenario's voor Azure DevTest Labs is het inschakelen van ontwikkel-en test omgevingen in de Cloud. Voorbeelden zijn:

* Ontwikkel aars Bureau bladen maken in de Cloud
* omgevingen configureren voor testen,
* toegang tot virtuele machines en andere Azure-bronnen inschakelen
* het instellen van een sandboxgebied voor ontwikkel aars om te leren en te experimenteren.

DevTest Labs biedt ook de beleids regels voor de Guardrails en kosten om de onderneming in staat te stellen om "zelf bedienstende Azure" te bieden aan ontwikkel aars die voldoen aan de beveiligings-, regelgevings-en nalevings beleidsregels van het bedrijf. 

Elke onderneming heeft verschillende vereisten voor de manier waarop Azure DevTest Labs kunnen worden opgenomen in hun omgeving. In dit artikel worden de meest voorkomende stappen beschreven die ondernemingen doorgaans moeten volt ooien om te zorgen voor een geslaagde test van het concept. Dit is de eerste stap naar een geslaagde end-to-end implementatie. 

## <a name="getting-started"></a>Aan de slag 

Om aan de slag te gaan met het leveren van een concept. Het is belang rijk om enige tijd te best Eden aan meer informatie over Azure en DevTest Labs.  Hier volgen enkele start bronnen: 

* [Wat is de Azure Portal?](https://azure.microsoft.com/features/azure-portal/)
* [Basis beginselen van DevTest Labs](devtest-lab-overview.md)
* [Ondersteunde Scenario's van DevTest Labs](devtest-lab-guidance-get-started.md)
* [Enter prise-documentatie voor DevTest Labs](devtest-lab-guidance-prescriptive-adoption.md)
* [Inleiding tot Azure-netwerken](../virtual-network/virtual-networks-overview.md)

## <a name="prerequisites"></a>Vereisten 

Als u een pilot of concept wilt volt ooien met DevTest Labs, zijn er enkele vereiste vereisten: 

* **Azure-abonnement**: Ondernemingen hebben vaak een bestaande [Enterprise Agreement](https://azure.microsoft.com/pricing/purchase-options/enterprise-agreement/) op locatie waarmee toegang tot Azure en een bestaand of nieuw abonnement kunnen worden gebruikt voor DevTest Labs. U kunt ook een [Visual Studio Subscription](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) gebruiken tijdens de pilot (met behulp van de gratis Azure-tegoed). Als geen van deze opties beschikbaar is, kan een [gratis Azure-account](https://azure.microsoft.com/free/search/?&OCID=AID719825_SEM_g4lyBqgB&lnkd=Bing_Azure_Brand&msclkid=ecc4275a31b61375749e7a5322c20de8&dclid=CMGW5-m78-ICFaLt4QodmUwGtQ) worden gemaakt en gebruikt. Als er een Enterprise Agreement is, is het gebruik van een [Enter prise dev/test-abonnement](https://azure.microsoft.com/offers/ms-azr-0148p/) een uitstekende optie om toegang te krijgen tot Windows 10/Windows 8,1-client besturingssystemen en kortings tarieven voor werk belastingen voor ontwikkelen en testen. 
* **Azure Active Directory Tenant**:  Als u gebruikers wilt beheren (bijvoorbeeld het toevoegen van gebruikers of het toevoegen van Lab-eigen aars), moeten die gebruikers deel uitmaken van de [Azure Active Directory Tenant](https://azure.microsoft.com/services/active-directory/) die wordt gebruikt in het Azure-abonnement voor de pilot. Vaak wordt een [hybride identiteit](../active-directory/hybrid/whatis-hybrid-identity.md) ingesteld om gebruikers in staat te stellen hun on-premises identiteit in de cloud te gebruiken, maar dit is niet vereist voor de DevTest Labs-pilot. 

## <a name="scoping-of-the-pilot"></a>Bereik van de pilot 

Het is belang rijk dat u een goede pilot plant voordat u de implementatie start. Als u van tevoren weet dat de resources niet voor onbepaalde tijd blijven, worden de juiste verwachtingen ingesteld voor gebruikers van de pilot. 

> [!IMPORTANT]
> Het is niet mogelijk om te benadrukken dat het belang rijk is dat de pilot en het instellen van verwachting vooraf worden gebereikd.

Er zijn belang rijke vragen om te beantwoorden voordat u de pilot kunt starten: 

* Wat wilt u weten en wat is het succes van de pilot? 
* Welke werk belasting (en) of scenario ('s) worden in de pilot behandeld? Het is belang rijk dat u slechts een kleine set definieert, om ervoor te zorgen dat de pilot kan worden beperkt en snel kan worden voltooid. 
* Welke resources moeten beschikbaar zijn in het lab? Bijvoorbeeld: aangepaste installatie kopieën, installatie kopieën van Marketplace, beleids regels, netwerk topologie, enzovoort. 
* Wie zijn de eind gebruikers/teams die bij de pilot betrokken zijn, om de ervaring te controleren?  
* Wat is de duur van de pilot? Kies een tijds bestek dat goed wordt uitgelijnd op het geplande bereik, zoals twee weken of één maand. 
* Wanneer de test is voltooid, wat gebeurt er dan met de toegewezen resources die tijdens de test zijn gebruikt? Wilt u de test bronnen verwijderen?
   
   Als we de virtuele machines en Labs aan het einde van de pilot kunnen gaan doen, kunnen we een enkel abonnement voor de pilot instellen en al onze werkzaamheden uitvoeren terwijl we de vragen over de schaal implementatie parallel oplossen. 

Er is een algemene tendens om de Pilot ' perfect ' te maken, zodat het exact overeenkomt met wat de uiteindelijke status is zodra de service bij het bedrijf is getotaliseerd. Dit is een onjuiste hypo these. Hoe dichter u bij ' perfect ', hoe meer u moet doen *voordat* u aan de slag gaat met de pilot. Het doel van de pilot is om de juiste beslissingen te nemen over het omhoog en uitrollen van de eind service. 

De focus van de pilot moet worden opgenomen in de absolute mini maal vereiste werk belastingen en afhankelijkheden om de vraag te beantwoorden of Azure DevTest Labs de juiste service voor uw bedrijf is. Het is raadzaam om de eenvoudigste werk belasting met de minste afhankelijkheden te kiezen om sneller en schoon te kunnen slagen. Als dat niet mogelijk is, kunt u het beste een representatieve werk belasting kiezen waarmee mogelijke complexe resultaten worden weer gegeven, zodat het succes van de test fase kan worden gerepliceerd in de scale-out-fase. 

In het volgende voor beeld ziet u een goed bereik van het concept. 

## <a name="example-proof-of-concept-plan"></a>Voor beeld: testen van het concept plan 

In deze sectie wordt een voor beeld weer gegeven dat u kunt gebruiken voor het bereik van een concept van de pilot voor DevTest Labs. 

> [!TIP]
> Om het project zo klein mogelijk te maken, kunt u het beste het voor beeld dat in deze sectie wordt beschreven, niet overs Laan. 

### <a name="overview"></a>Overzicht 

We plannen voor het ontwikkelen van een nieuwe omgeving in azure op basis van DevTest Labs voor leveranciers om te gebruiken als een geïsoleerde omgeving van het bedrijfs netwerk. Om te bepalen of de oplossing aan de vereisten voldoet, zullen we een proef versie van het concept ontwikkelen om de end-to-end-oplossing te valideren en meerdere leveranciers hebben opgenomen om de ervaring uit te proberen en te verifiëren. 

### <a name="outcomes"></a>Resultaten 

Bij het opstellen van een concept kunt u het beste de resultaten bekijken (wat we proberen te verkrijgen). deze worden hier weer gegeven.  Aan het einde van het concept testen verwachten we het volgende: 

* Een werkend end-to-end-oplossing voor leveranciers om gast accounts in Azure Active Directory (Azure AD) te gebruiken om toegang te krijgen tot een geïsoleerde omgeving in azure, met de resources die nodig zijn om productief te zijn. 
* Mogelijke blokkerings problemen die invloed hebben op grotere schaal baarheid en goed keuring, worden opgesomd en begrepen.
* De personen die betrokken zijn bij het ontwikkelen van het concept testen, hebben een goed begrip van alle code. Ze begrijpen ook het betrokken onderpand en zijn in ruimere goed keuring.

### <a name="open-questions--prerequisites"></a>Vragen openen & vereisten 

* Is er een abonnement gemaakt dat kan worden gebruikt voor dit project? 
* Hebben we een Azure AD-Tenant en een Azure AD Global-beheerder geïdentificeerd wie hulp kunnen bieden & hulp bij Azure AD-gerelateerde vragen? 
* Een plaats om samen te werken voor de personen die aan het project werken: 

   * Bron code en scripts (zoals Azure opslag plaatsen) 
   * Documenten (zoals teams of share point)  
   * Discussies (zoals micro soft-teams) 
   * Werk items (zoals Azure-boards) 
* Wat zijn de vereiste resources voor leveranciers? Dit omvat zowel lokaal op de virtuele machines als andere vereiste servers, toepassingen die beschikbaar zijn op het netwerk. 
* Worden de virtuele machines gekoppeld aan een domein in azure? Als dat het geval is, wordt dit Azure AD Domain Services of iets anders? 
* Zijn het team/de leveranciers geïdentificeerd dat het doel is van het concept testen? Wie zijn de klanten voor de omgeving?
* Welke Azure-regio wordt gebruikt voor het testen van het concept? 
* Hebben we een lijst met services die de leveranciers mogen gebruiken via DevTest Labs naast IaaS (Vm's)? 
* Hoe kunnen we leveranciers of gebruikers plannen voor het gebruik van het lab? 

### <a name="proof-of-concept-solution-components"></a>Testen van onderdelen van de concept oplossing 

We verwachten dat de oplossing de volgende onderdelen bevat: 

* Een set DevTest Labs in azure voor verschillende leveranciers teams.
* De laboratoria zijn verbonden met een netwerk infrastructuur die ondersteuning biedt voor de vereisten.
* De leveranciers hebben toegang tot de Labs via Azure AD en rollen toewijzen aan het lab.
* Een manier waarop leveranciers verbinding met hun resources kunnen maken. Met name een site-naar-site-VPN waarmee u rechtstreeks toegang kunt krijgen tot virtuele machines zonder open bare IP-adressen.
* Een verzameling artefacten die betrekking hebben op de vereiste software die de leveranciers op de virtuele machines nodig hebben.

## <a name="additional-planning-and-design-decisions"></a>Aanvullende plannings-en ontwerp beslissingen 

Voordat u een volledige DevTest Labs-oplossing uitbrengt, moeten er enkele belang rijke plannings-en ontwerp beslissingen worden genomen. De ervaring van het werken met een concept kan u helpen bij het nemen van deze beslissingen. Verdere overwegingen zijn onder andere: 

* **Topologie van abonnement**: De vereisten op ondernemings niveau voor resources in azure kunnen buiten de [beschik bare quota's worden uitgebreid binnen één abonnement](https://docs.microsoft.com/azure/azure-subscription-service-limits), waardoor meerdere Azure-abonnementen en/of service aanvragen nodig zijn om de initiële limieten voor abonnementen te verhogen. Het is belang rijk om te bepalen hoe u resources over verschillende abonnementen distribueert, een waardevolle resource is de [beslissings handleiding](https://docs.microsoft.com/azure/architecture/cloud-adoption/decision-guides/subscriptions/) , omdat het lastig is om resources later te verplaatsen naar een ander abonnement. Een DevTest-Lab kan bijvoorbeeld niet worden verplaatst naar een ander abonnement nadat het is gemaakt.  
* **Netwerk topologie**: De [standaard netwerk infrastructuur](../app-service/networking-features.md) die automatisch door DevTest Labs wordt gemaakt, voldoet niet aan de vereisten en beperkingen voor de zakelijke gebruikers. Het is gebruikelijk om [ExpressRoute Connected VNets](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/), [hub en spoke](https://docs.microsoft.com/azure/architecture/reference-architectures/hybrid-networking/hub-spoke) te zien voor connectiviteit tussen abonnementen, en zelfs [geforceerde route ring](../vpn-gateway/vpn-gateway-forced-tunneling-rm.md) waarbij alleen on-premises connectiviteit wordt gegarandeerd. Met DevTest Labs kunnen bestaande virtuele netwerken worden verbonden met het lab, zodat ze hun gebruik kunnen maken wanneer ze nieuwe virtuele machines in het lab worden gemaakt. 
* **Externe toegang van virtual machines**: Er zijn veel opties voor externe toegang tot de virtuele machines die zich bevinden in DevTest Labs. Het eenvoudigste is om open bare Ip's of gedeelde open bare Ip's te gebruiken. Dit zijn de [instellingen die beschikbaar zijn in het lab](devtest-lab-shared-ip.md). Als deze opties niet voldoende zijn, is het gebruik van een RAS-gateway ook een optie die wordt weer gegeven in de [DevTest Labs Enter prise-referentie architectuur](devtest-lab-reference-architecture.md) en die verder wordt beschreven in de [documentatie over de extern bureau blad-gateway van DevTest Labs](configure-lab-remote-desktop-gateway.md). Ondernemingen kunnen ook Express route of een site-naar-site-VPN gebruiken om hun DevTest Labs te verbinden met hun on-premises netwerk. Hierdoor kunnen direct extern bureau blad-of SSH-verbindingen naar het Virtual Machines op basis van hun privé-IP-adres zonder bloot stelling aan Internet. 
* **Verwerkings machtigingen**: De twee belangrijkste machtigingen die doorgaans worden gebruikt in DevTest Labs is [eigenaar en Lab-gebruiker](devtest-lab-add-devtest-user.md). Het is belang rijk om te beslissen voordat u DevTest Labs globaal uitrollen, die met elk toegangs niveau in het lab wordt toevertrouwd. Een gemeen schappelijk model is de budget eigenaar (bijvoorbeeld team lead) als Lab-eigenaar en de team leden als Lab-gebruikers. Hierdoor kan de persoon (team leider) die verantwoordelijk is voor het budget, de beleids instellingen aanpassen zodat het team binnen het budget blijft.  

## <a name="completing-the-proof-of-concept"></a>Het testen van het concept volt ooien 

Zodra de verwachte informatie is behandeld, is het tijd om de pilot uit te voeren en te volt ooien. Dit is de tijd om feedback van de gebruikers te verzamelen, te bepalen of de test is geslaagd en te beslissen of de organisatie vooruit gaat op een scale-out van DevTest Labs in de onderneming. Het is ook een goed idee om de implementatie van DevTest Labs en bijbehorende resources te automatiseren om consistentie te garanderen. 

## <a name="next-steps"></a>Volgende stappen 

* [Enter prise-documentatie voor DevTest Labs](devtest-lab-guidance-prescriptive-adoption.md)
* [Referentie architectuur voor een onderneming](devtest-lab-reference-architecture.md)
* [Uw DevTest Labs-implementatie omhoog schalen](devtest-lab-guidance-orchestrate-implementation.md)
* [De implementatie van Azure DevTest Labs organiseren](devtest-lab-guidance-orchestrate-implementation.md)
