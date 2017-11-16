---
title: Scenario's en voorbeelden voor abonnement toezicht | Microsoft Docs
description: Bevat voorbeelden van hoe Azure-abonnement governance voor veelvoorkomende scenario's implementeren.
services: azure-resource-manager
documentationcenter: na
author: rdendtler
manager: timlt
editor: tysonn
ms.assetid: e8fbeeb8-d7a1-48af-804f-6fe1a6024bcb
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/03/2017
ms.author: rodend;karlku;tomfitz
ms.openlocfilehash: 4ab816d0392816c2293f9d70eb249bbcfa09bfba
ms.sourcegitcommit: 9a61faf3463003375a53279e3adce241b5700879
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2017
---
# <a name="examples-of-implementing-azure-enterprise-scaffold"></a>Voorbeelden van de implementatie van Azure enterprise scaffold
Dit onderwerp vindt u voorbeelden van hoe de aanbevelingen voor in een onderneming implementeren kunt een [Azure enterprise scaffold](resource-manager-subscription-governance.md). Een fictief bedrijf met de naam Contoso wordt gebruikt ter illustratie van best practices voor algemene scenario's.

## <a name="background"></a>Achtergrond
Contoso is een wereldwijd bedrijf dat levering keten oplossingen voor klanten in alles uit een model 'Software as a Service' naar een ingepakte model biedt lokale geïmplementeerd.  Ze ontwikkelen software overal ter wereld met aanzienlijke development centers in India, de Verenigde Staten en Canada.

De ISV-gedeelte van het bedrijf is onderverdeeld in meerdere onafhankelijke business units die producten in een grote onderneming beheren. Elk bedrijfsonderdeel heeft een eigen ontwikkelaars, productmanagers en -architecten.

De Services voor Enterprise-technologie (ETS) business unit biedt de mogelijkheid van centrale IT en beheert de verschillende datacenters waar bedrijfseenheden hun toepassingen hosten. Samen met het beheren van de datacenters, de organisatie ETS biedt en beheert centraal samenwerking (zoals e-mail en websites) en netwerk/telephony-services. Ze ook beheren klantgerichte werkbelastingen voor kleinere business units die operationele medewerkers geen.

De volgende Persona's worden gebruikt in dit onderwerp:

* Dave is de ETS Azure-beheerder.
* Alice is van Contoso directeur van ontwikkeling in de levering keten business unit.

Contoso moet een line-of-business-app en een app klantgerichte bouwen. Zij heeft besloten de apps in Azure uitvoeren. Dave leest de [prescriptieve abonnement governance](resource-manager-subscription-governance.md) onderwerp, en is nu gereed voor het implementeren van de aanbevelingen.

## <a name="scenario-1-line-of-business-application"></a>Scenario 1: line-of-business-toepassing
Contoso is bezig met een beheersysteem van de bron-code (BitBucket) moet worden gebruikt door ontwikkelaars over de hele wereld.  De toepassing maakt gebruik van infrastructuur als een Service (IaaS) voor het hosten van, en bestaat uit de webservers en een databaseserver. Ontwikkelaars toegang tot de servers in hun ontwikkelomgevingen, maar ze geen toegang nodig tot de servers in Azure. Contoso ETS wenst te maken voor de eigenaar van de toepassing en het team voor het beheren van de toepassing. De toepassing is alleen beschikbaar bij in het bedrijfsnetwerk van Contoso. Dave moet voor het instellen van het abonnement voor deze toepassing. Het abonnement wordt ook als host fungeren andere software developer-gerelateerde in de toekomst.  

### <a name="naming-standards--resource-groups"></a>Naamgevingsstandaarden & resourcegroepen
Maakt een abonnement ter ondersteuning van hulpprogramma's voor ontwikkelaars die voor alle business units gelden Dave. Hij moet betekenisvolle namen voor het abonnement en de resource-groepen (voor de toepassing en de netwerken) maken. Hij maakt de volgende abonnement en de resource-groepen:

| Item | Naam | Beschrijving |
| --- | --- | --- |
| Abonnement |Contoso ETS DeveloperTools productie |Biedt ondersteuning voor algemene hulpprogramma's voor ontwikkelaars |
| Resourcegroep |bitbucket-prod-rg |Bevat de webserver van de toepassing en de database-server |
| Resourcegroep |corenetworks-prod-rg |Bevat de virtuele netwerken en site-naar-site gatewayverbinding |

### <a name="role-based-access-control"></a>Op rollen gebaseerd toegangsbeheer
Na het maken van diens abonnement wil Dave om ervoor te zorgen dat de juiste teams en toepassingseigenaars toegang hun bronnen tot hebben. Dave herkent dat elk team verschillende vereisten heeft. Hij maakt gebruik van de groepen die aan Azure Active Directory zijn gesynchroniseerd van Contoso on-premises Active Directory (AD) en biedt het juiste niveau van toegang tot de teams.

Dave wijst de volgende rollen voor het abonnement:

| Rol | Toegewezen aan | Beschrijving |
| --- | --- | --- |
| [Eigenaar](../active-directory/role-based-access-built-in-roles.md#owner) |ID van Contoso beheerde AD |Deze ID wordt beheerd met Just in Time (Just in time) toegang via de Contoso Identity Management-hulpprogramma en zorgt ervoor dat er volledig abonnement eigenaar toegang wordt gecontroleerd |
| [Beveiligingsbeheer](../active-directory/role-based-access-built-in-roles.md#security-manager) |Gegevensbeveiliging en risicobeheer management afdeling |Deze rol kan gebruikers om te kijken naar het Azure Security Center en de status van de resources |
| [Inzender voor netwerken](../active-directory/role-based-access-built-in-roles.md#network-contributor) |Netwerkteam |Deze rol kan team van de Contoso netwerk voor het beheren van de Site naar Site VPN-verbinding en de virtuele netwerken |
| *Aangepaste rol* |De eigenaar van de toepassing |Dave maakt een rol die de mogelijkheid om te wijzigen van resources binnen de resourcegroep verleent. Zie voor meer informatie [aangepaste rollen in Azure RBAC](../active-directory/role-based-access-control-custom-roles.md) |

### <a name="policies"></a>Beleidsregels
Dave gelden de volgende vereisten voor het beheren van resources in het abonnement:

* Omdat de ontwikkelprogramma's ontwikkelaars overal ter wereld ondersteunen, wilt hij niet verhinderen dat gebruikers maken van resources in elke regio. Hij moet echter weten waar de bronnen worden gemaakt.
* Hij is betrokken zijn bij de kosten. Daarom hij om te voorkomen dat toepassingseigenaars onnodig dure virtuele machines maken.  
* Omdat deze toepassing ontwikkelaars in vele business units fungeert, wil hij tag van elke resource met de eigenaar van het bedrijf eenheid en de toepassing. Met behulp van deze tags kunnen ETS factureren de juiste teams.

Hij maakt de volgende [Azure beleid](../azure-policy/azure-policy-introduction.md):

| Veld | Effect | Beschrijving |
| --- | --- | --- |
| location |Audit |Het maken van de resources in elke regio controleren |
| type |weigeren |Maken van virtuele machines van G-reeks weigeren |
| tags |weigeren |Toepassing eigenaar tag vereisen |
| tags |weigeren |De tag center kosten vereisen |
| tags |toevoegen |Toevoeg-labelnaam **Business Unit** en tagwaarde **ETS** tot alle bronnen |

### <a name="resource-tags"></a>Resourcetags
Dave begrijpt dat hij zijn van specifieke informatie over de factuur moet voor het identificeren van de kostenplaats voor de BitBucket-implementatie. Daarnaast wil Dave de resources waartoe ETS weten.

Hij voegt de volgende [labels](resource-group-using-tags.md) aan de resourcegroepen en resources.

| Codenaam | Tagwaarde |
| --- | --- |
| ApplicationOwner |De naam van de persoon die deze toepassing beheert |
| CostCenter |De kostenplaats van de groep die u voor de Azure-verbruik betaalt |
| Business Unit |**ETS** (de business unit die is gekoppeld aan het abonnement) |

### <a name="core-network"></a>Basisnetwerk
Het Contoso ETS informatie gegevensbeveiliging en risicobeheer management-team onderzoekt van Dave voorgestelde plan te verplaatsen van de toepassing in Azure. Ze willen om ervoor te zorgen dat de toepassing geen toegang heeft tot internet.  Dave heeft ook developer-apps die in de toekomst wordt verplaatst naar Azure. Deze apps vereisen openbare interfaces.  Om te voldoen aan deze vereisten, biedt hij zowel interne als externe virtuele netwerken en een netwerkbeveiligingsgroep om toegang te beperken.

Hij maakt de volgende bronnen:

| Brontype | Naam | Beschrijving |
| --- | --- | --- |
| Virtual Network |interne-vnet |Met de BitBucket-toepassing gebruikt en met het bedrijfsnetwerk van Contoso is verbonden via ExpressRoute.  Een subnet (`bitbucket`) biedt de toepassing met een specifiek IP-adresruimte |
| Virtual Network |externe-vnet |Beschikbaar voor toekomstige toepassingen waarvoor openbare eindpunten |
| Netwerkbeveiligingsgroep |bitbucket nsg |Zorgt ervoor dat de kwetsbaarheid voor aanvallen van deze workload wordt geminimaliseerd doordat verbindingen alleen via poort 443 voor het subnet waar de toepassing woont (`bitbucket`) |

### <a name="resource-locks"></a>Resource-vergrendelingen
Dave herkent dat de connectiviteit van het bedrijfsnetwerk van Contoso met het interne virtuele netwerk moet worden beveiligd tegen wayward script of per ongeluk verwijderen.

Hij maakt de volgende [resource vergrendeling](resource-group-lock-resources.md):

| Type van de vergrendeling | Resource | Beschrijving |
| --- | --- | --- |
| **CanNotDelete** |interne-vnet |Voorkomen dat gebruikers verwijderen van het virtuele netwerk of subnetten, maar voorkomt niet dat voor het toevoegen van nieuwe subnetten |

### <a name="azure-automation"></a>Azure Automation
Dave heeft niets te automatiseren voor deze toepassing. Hoewel hij een Azure Automation-account hebt gemaakt, niet hij het in eerste instantie gebruikt.

### <a name="azure-security-center"></a>Azure Security Center
Er moet een Contoso IT-servicebeheer om snel te identificeren en bedreigingen te verwerken. Ze willen ook om te begrijpen welke problemen kunnen bestaan.  

Om te voldoen aan deze vereisten, Dave kan de [Azure Security Center](../security-center/security-center-intro.md), en biedt toegang tot de rol Security Manager.

## <a name="scenario-2-customer-facing-app"></a>Scenario 2: klantgerichte app
De leidinggevende bedrijven in de levering keten business unit geconstateerd verschillende mogelijkheden om de betrokkenheid bij Contoso klanten via een loyaliteitskaart te vergroten. Els van team maken van deze toepassing en besluit dat Azure verhoogt de mogelijkheid om te voldoen aan de zakelijke behoeften. Els werkt met Dave van ETS twee abonnementen voor het ontwikkelen en gebruiken van deze toepassing te configureren.

### <a name="azure-subscriptions"></a>Azure-abonnementen
Dave zich aanmeldt bij de Azure Enterprise Portal en ziet dat de keten levering afdeling al bestaat.  Als dit project de eerste project voor de ontwikkeling voor het team van de keten levering in Azure is, herkent Dave de noodzaak van een nieuw account voor ontwikkelteam van Els.  Hij maakt van het account "R & D" voor haar team en toegang wijst naar Els. Els zich aanmeldt via de Azure-portal en maakt u twee abonnementen: één waarin de van Ontwikkelingsservers en één voor het opslaan van de productieservers.  Ze volgt de eerder vastgestelde naamgevingsstandaarden bij het maken van de volgende abonnementen:

| Abonnement gebruiken | Naam |
| --- | --- |
| Ontwikkeling |Contoso Supply Chain ResearchDevelopment LoyaltyCard ontwikkeling |
| Productie |Contoso Supply Chain Operations LoyaltyCard productie |

### <a name="policies"></a>Beleidsregels
Dave en Els bespreken van de toepassing en dat deze toepassing alleen klanten in de regio Noord-Amerika fungeert identificeren.  Els en haar team van plan bent om Azure toepassing Service-omgeving en Azure SQL te maken van de toepassing. Ze willen maken van virtuele machines tijdens de ontwikkeling.  Alice wil om ervoor te zorgen dat haar ontwikkelaars de resources die ze nodig hebben om te verkennen en onderzoeken van problemen zonder binnenhalen in ETS hebben.

Voor de **ontwikkeling abonnement**, maken van het volgende beleid:

| Veld | Effect | Beschrijving |
| --- | --- | --- |
| location |Audit |Het maken van de resources in elke regio controleren |

Ze vormen geen beperking van het type van een gebruiker in ontwikkeling maken kan sku en tags is niet vereist voor alle resourcegroepen en resources.

Voor de **productie abonnement**, ze de volgende beleidsregels maken:

| Veld | Effect | Beschrijving |
| --- | --- | --- |
| location |weigeren |Het maken van alle resources buiten de Verenigde Staten-datacenters weigeren |
| tags |weigeren |Toepassing eigenaar tag vereisen |
| tags |weigeren |Afdeling tag vereisen |
| tags |toevoegen |Code toevoegen aan elke resourcegroep die productie-omgeving aangeeft |

Ze beperken niet het type van een gebruiker in productie maken kan sku.

### <a name="resource-tags"></a>Resourcetags
Dave begrijpt dat hij zijn van specifieke informatie moet voor het identificeren van de juiste bedrijfsgroepen voor facturerings- en eigendom. Hij definieert resourcetags voor resourcegroepen en resources.

| Codenaam | Tagwaarde |
| --- | --- |
| ApplicationOwner |De naam van de persoon die deze toepassing beheert |
| Afdeling |De kostenplaats van de groep die u voor de Azure-verbruik betaalt |
| EnvironmentType |**Productie** (zelfs als het abonnement bevat **productie** in de naam, met inbegrip van dit label kan eenvoudige identificatie bij het onderzoeken van bronnen in de portal of op de factuur) |

### <a name="core-networks"></a>Core-netwerken
Het Contoso ETS informatie gegevensbeveiliging en risicobeheer management-team onderzoekt van Dave voorgestelde plan te verplaatsen van de toepassing in Azure. Ze willen Zorg ervoor dat de toepassing loyaliteitskaart correct is geïsoleerd en beveiligd in een Perimeternetwerk-netwerk.  Maak een extern virtueel netwerk en een netwerkbeveiligingsgroep voor het isoleren van de toepassing loyaliteit-kaart van het bedrijfsnetwerk van Contoso om te voldoen aan deze eis, Dave en Els.  

Voor de **ontwikkeling abonnement**, ze maken:

| Brontype | Naam | Beschrijving |
| --- | --- | --- |
| Virtual Network |interne-vnet |De ontwikkelomgeving Contoso loyaliteitskaart fungeert en is verbonden via ExpressRoute Contoso netwerk van bedrijf |

Voor de **productie abonnement**, ze maken:

| Brontype | Naam | Beschrijving |
| --- | --- | --- |
| Virtual Network |externe-vnet |Als host fungeert voor de toepassing loyaliteitskaart en niet rechtstreeks is verbonden met ExpressRoute van Contoso. Code wordt doorgeschoven, is via hun broncode-systeem rechtstreeks aan de PaaS-services |
| Netwerkbeveiligingsgroep |loyaltycard nsg |Zorgt ervoor dat de kwetsbaarheid voor aanvallen van deze workload door alleen in gebonden communicatie toe te staan op TCP 443 wordt geminimaliseerd.  Contoso is ook het onderzoeken van het gebruik van een Web Application Firewall voor extra beveiliging |

### <a name="resource-locks"></a>Resource-vergrendelingen
Dave en Els verlenen en besluit toe te voegen van bronvergrendelingen op sommige van de belangrijkste resources in de omgeving om te voorkomen dat de onopzettelijke verwijderingen tijdens een onjuiste code push.

Ze maken de volgende vergrendeling:

| Type van de vergrendeling | Resource | Beschrijving |
| --- | --- | --- |
| **CanNotDelete** |externe-vnet |Om te voorkomen dat mensen verwijderen van het virtuele netwerk of subnetten. De vergrendeling voorkomt niet dat voor het toevoegen van nieuwe subnetten |

### <a name="azure-automation"></a>Azure Automation
Els en haar ontwikkelingsteam hebben uitgebreide runbooks voor het beheren van de omgeving voor deze toepassing. De runbooks kunt u de toevoeging/verwijdering van de knooppunten voor de toepassing en andere DevOps-taken.

Voor het gebruik van deze runbooks inschakelen [Automation](../automation/automation-intro.md).

### <a name="azure-security-center"></a>Azure Security Center
Er moet een Contoso IT-servicebeheer om snel te identificeren en bedreigingen te verwerken. Ze willen ook om te begrijpen welke problemen kunnen bestaan.  

Om te voldoen aan deze vereisten voldoet, kunt Dave Azure Security Center. Hij zorgt ervoor dat het Azure Security Center wordt bewaakt door de bronnen en toegang tot de teams DevOps en beveiliging biedt.

## <a name="next-steps"></a>Volgende stappen
* Zie voor meer informatie over het maken van Resource Manager-sjablonen, [aanbevolen procedures voor het maken van Azure Resource Manager-sjablonen](resource-manager-template-best-practices.md).
