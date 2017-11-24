---
title: Azure betaling verwerking blauwdruk - firewallvereisten
description: PCI-DSS vereiste 1
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: b1935d88-acae-42f9-bc25-bb0766f876ab
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 995ecd5ef876695145fc6313aba2a46d2cc085cc
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="firewall-requirements-for-pci-dss-compliant-environments"></a>Firewallvereisten voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-1"></a>PCI-DSS vereiste 1

**Installeren en onderhouden van een firewallconfiguratie om kaarthouder gegevens te beveiligen**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Firewalls zijn apparaten waarmee het verkeer van de computer toegestaan tussen netwerken voor een entiteit (intern) en niet-vertrouwde netwerken (extern), evenals verkeer van en naar meer gevoelige gebieden in een entiteit interne netwerken worden vertrouwd. De gegevens kaarthouder omgeving is een voorbeeld van een gevoeliger gebied binnen een entiteit vertrouwd netwerk.
Een firewall alle netwerkverkeer moet worden gecontroleerd en deze gegevensoverdrachten die niet voldoen aan de criteria voor de opgegeven security blokkeert.
Alle systemen moeten worden beveiligd tegen onbevoegde toegang van niet-vertrouwde netwerken, of het systeem via het Internet als e-commerce, toegang tot Internet via pc-browsers, toegang tot e-mail van werknemers, specifieke verbindingen, zoals werknemer invoeren Business-to-business-verbindingen via draadloze netwerken, of een andere bronnen. Vaak kunnen schijnbaar verwaarlozen paden naar en van niet-vertrouwde netwerken bieden onbeveiligde paden in de belangrijkste systemen. Firewalls zijn een mechanisme sleutelbeveiliging voor een computernetwerk.
Andere onderdelen van het systeem kunnen firewall-functionaliteit bieden, zolang ze voldoen aan de minimale vereisten voor firewalls zoals gedefinieerd in vereiste 1. Als andere onderdelen van het systeem worden gebruikt binnen de omgeving van de gegevens kaarthouder om firewall-functionaliteit te bieden, kunnen deze apparaten moeten zijn opgenomen in het bereik en de evaluatie van de vereiste 1.

## <a name="pci-dss-requirement-11"></a>PCI-DSS vereiste 1.1

**1.1** maken en implementeren firewall- en configuratie normen die zijn onder andere de volgende (Zie 1.1.1 via 1.1.7).


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore beschikken over gebruik van het CDE met het PaaS-isolatie en een App Service-omgeving implementatie zorgt ervoor dat CDE inkomende en uitgaande van gegevens is beveiligd.<br /><br />Een [as-omgeving (App Service omgeving)](/azure/app-service-web/app-service-app-service-environment-intro) is een Premium-service-abonnement gebruikt om wettelijke redenen. Zie voor meer informatie over besturingselementen as-omgeving en configuratie [richtlijnen PCI - App Service-omgeving](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-111"></a>PCI-DSS eis 1.1.1

**1.1.1** een formele proces voor het goedkeuren en testen van alle netwerkverbindingen en -wijzigingen op de firewall- en configuraties


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Een exemplaar van Contoso Webstore stelt u een CI/CD DevOps-model om ervoor te zorgen dat alle wijzigingen correct worden beheerd. [Operations Management Suite (OMS)](/azure/operations-management-suite/) biedt uitgebreide logboekregistratie van wijzigingen. Wijzigingen worden beoordeeld en gecontroleerd op juistheid. Zie voor meer specifieke instructies [richtlijnen PCI - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).<br /><br />[Azure Security Center](https://azure.microsoft.com/services/security-center/) biedt een gecentraliseerde weergave van de beveiligingsstatus van alle Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u alle bronnen die aandacht vereisen snel kan identificeren.|



### <a name="pci-dss-requirement-112"></a>PCI-DSS eis 1.1.2

**1.1.2** huidige netwerkdiagram waarmee alle verbindingen tussen de kaarthouder gegevensomgeving en andere netwerken, inclusief alle draadloze netwerken

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Raadpleeg de Contoso Webstore architectuur en het ontwerp naslagdocumentatie geleverd als onderdeel van het patroon van de installatie van de oplossing.|



### <a name="pci-dss-requirement-113"></a>PCI-DSS eis 1.1.3

**1.1.3** Huidig diagram waarin alle gegevens van de kaarthouder loopt verschillende systemen en netwerken

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Raadpleeg de Contoso-webarchief GSD en de risicomodel.|



### <a name="pci-dss-requirement-114"></a>PCI-DSS eis 1.1.4

**1.1.4** vereisten voor een firewall op elke internetverbinding en tussen een gedemilitariseerde zone (DMZ genoemd) en de zone van het interne netwerk

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | De veiligheidsmaatregelen grens beveiliging apparaten zoals gateways, netwerk-ACL's en application firewalls om communicatie besturingselement op de externe en interne grenzen op het niveau van het platform voor Microsoft Azure. De klant configureert deze voor hun specificaties en vereisten. Microsoft Azure filtert communicatie als in het platform. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een DMZ met PaaS-isolatie en een App Service-omgeving implementatie zorgt ervoor dat CDE inkomende en uitgaande van gegevens is beveiligd.<br /><br />Een [as-omgeving (App Service omgeving)](/azure/app-service-web/app-service-app-service-environment-intro) is een Premium-service-abonnement gebruikt om wettelijke redenen. Zie voor meer informatie over besturingselementen as-omgeving en configuratie [richtlijnen PCI - App Service-omgeving](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-115"></a>PCI-DSS eis 1.1.5

**1.1.5** beschrijving van groepen, rollen en verantwoordelijkheden voor het beheer van netwerkonderdelen

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Maakt gebruik van de Contoso Webstore [gebaseerd toegangsbeheer (RBAC)](/azure/active-directory/role-based-access-control-configure) voor het isoleren van gebruikersrollen. RBAC kunt nauwkeurig gerichte toegangsbeheer voor Azure. Specifieke configuraties bestaan voor toegang via abonnement en Azure Sleutelkluis.|



### <a name="pci-dss-requirement-116"></a>PCI-DSS vereiste 1.1.6

**1.1.6** documentatie van zakelijke rechtvaardiging en goedkeuring voor gebruik van alle services, protocollen en poorten die zijn toegestaan, inclusief de documentatie over beveiligingsfuncties voor deze protocollen die als onveilig wordt geïmplementeerd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore wordt alleen de vereiste poorten en protocollen overal in het ontwerp RA geopend. Meer informatie over de gegevensstroom zichtbaar in het model GSD en bedreigingen.|



### <a name="pci-dss-requirement-117"></a>PCI-DSS vereiste 1.1.7

**1.1.7** vereiste controleren softwarerouter en firewall-regel wordt ingesteld ten minste om de zes maanden

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | In de Contoso Webstore, worden de regelsets firewall gecontroleerd om ervoor te zorgen dat geen overbodige of ongebruikte regels opgenomen worden. Standaard wordt de demo geïmplementeerd met een minimale bevoegdheden, waarbij de kleinste pad footprint.|



## <a name="pci-dss-requirement-12"></a>PCI-DSS eis 1.2

**1.2** bouwen firewall- en configuraties die verbindingen tussen de niet-vertrouwde netwerken en alle onderdelen van het systeem in de kaarthouder gegevensomgeving beperken. 

> [!NOTE]
> Een 'niet-vertrouwd netwerk' is een netwerk die zich buiten de netwerken die horen bij de entiteit onder revisie en/of ligt buiten de mogelijkheid om te bepalen of het beheren van de entiteit.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Van de Contoso Webstore CDE is gedefinieerd in de documentatie van de RA en de implementatie. Niet-vertrouwde netwerken zijn standaard geweigerd.|



### <a name="pci-dss-requirement-121"></a>PCI-DSS vereiste 1.2.1

**1.2.1** binnenkomend en uitgaand verkeer te beperken tot die welke nodig is voor de omgeving van de gegevens kaarthouder en al het andere verkeer expliciet weigert.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Van de Contoso Webstore CDE is gedefinieerd in de documentatie van de RA en de implementatie. Niet-vertrouwde netwerken zijn standaard geweigerd. De demo Contoso Webstore configureert u de firewall van de toepassing Microsoft Azure zodat alleen opgegeven bereiken van IP-adressen voor toegang tot Microsoft Azure-services. De Contoso Webstore biedt een firewall weigeren alle helemaal CDE grenzen. Alle configuraties wordt uitgevoerd tijdens de eerste installatie van de implementatie.

> [!NOTE]
> As-omgeving (App Service omgeving) wordt gebruikt in deze oplossing voor het isoleren van CDE echter is het essentieel dat uw gekwalificeerde beveiliging beoordelaar (QSA) deze oplossing wordt geëvalueerd als as-omgeving een DMZ isolatie waarmee uitgaande verbindingen implementeert moeten worden uitgevoerd door de as-omgeving. PCI-DSS vereist dat alle binnenkomende en uitgaande verbindingen die niet vereist, moeten worden geblokkeerd. Voor de as-omgeving goed te laten werken, as-omgeving tot stand uitgaande verbindingen zoals brengen die nodig zijn zoals gedefinieerd in ['Overwegingen voor een App Service-omgeving netwerken'](/azure/app-service/app-service-environment/network-info). Klanten dienen de uitgaande verbindingen met uw QSA voordat u de oplossing implementeert in een productieomgeving om te controleren of deze voldoet aan de vereisten te evalueren. |



### <a name="pci-dss-requirement-122"></a>PCI-DSS vereiste 1.2.2

**1.2.2** Secure en configuratiebestanden van de router te synchroniseren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt configuraties voor besturingselementen van Microsoft Azure systeemeigen netwerk is gesynchroniseerd.|



### <a name="pci-dss-requirement-123"></a>PCI-DSS vereiste 1.2.3

**1.2.3** perimeterfirewalls tussen alle draadloze netwerken en de gegevens kaarthouder omgeving installeren en configureren van firewalls wilt weigeren, of, als verkeer nodig is voor zakelijke doeleinden toestaan alleen verkeer tussen het draadloze geautoriseerd omgeving en de gegevens kaarthouder omgeving.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore beschikt niet over draadloze oplossingen of mogelijkheden die zijn ingeschakeld.|



## <a name="pci-dss-requirement-13"></a>PCI-DSS vereiste 1.3

**1.3** direct openbare toegang tussen het Internet en een systeemonderdeel in de omgeving van de gegevens kaarthouder verbieden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure maakt gebruik van netwerk- en host grens beveiliging apparaten zoals firewalls, netwerktaakverdelers en ACL's. Deze apparaten gebruikgemaakt van mechanismen zoals isolatie, NAT en pakketfilters aparte klantverkeer van Internet en beheer van verkeer van VLAN. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt op het moment van implementatie van de configuraties van de Azure-toepassing firewall zodat alleen opgegeven bereiken van IP-adressen voor toegang tot de site, de bastionomgeving Azure VM's opnemen in hun CDE.|



### <a name="pci-dss-requirement-131"></a>PCI-DSS eis 1.3.1

**1.3.1** implementeren een DMZ voor binnenkomend verkeer beperkt tot alleen systeemonderdelen waarmee geautoriseerde openbaar toegankelijk services, protocollen en poorten.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De implementatie van Contoso Webstore van de DMZ zorgt ervoor dat alleen geautoriseerde services verbinding met het CDE maken kunnen.|



### <a name="pci-dss-requirement-132"></a>PCI-DSS eis 1.3.2

**1.3.2** limiet Binnenkomend internetverkeer voor IP-adressen in het Perimeternetwerk.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De implementatie van Contoso Webstore van de DMZ zorgt ervoor dat alleen geautoriseerde services verbinding met het CDE maken kunnen.|



### <a name="pci-dss-requirement-133"></a>PCI-DSS vereiste 1.3.3

**1.3.3** anti-adresvervalsing metingen implementeren om te detecteren en blokkeren forged bron-IP-adressen in te voeren van het netwerk. (Bijvoorbeeld blokkeren voor verkeer dat afkomstig is van het Internet met een interne bronadres.)

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure implementeert netwerk filteren om te voorkomen dat vervalste verkeer en binnenkomende en uitgaande verkeer te beperken tot vertrouwde platform-onderdelen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-134"></a>PCI-DSS vereiste 1.3.4

**1.3.4** staan geen niet-geautoriseerde uitgaand verkeer van de kaarthouder gegevensomgeving met Internet.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De architectuur van Contoso Webstore wordt voorkomen dat onbevoegde uitgaand verkeer van de omgeving in het bereik tot het Internet. Dit wordt bereikt door het uitgaande verkeer ACL's voor goedgekeurde poorten en protocollen configureren in Microsoft Azure. Toegang tot het CDE opnemen deze besturingselementen in de SQL Server-database. <br /><br />Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-135"></a>PCI-DSS vereiste 1.3.5

**1.3.5** toestaan alleen 'verbindingen hebben gemaakt' in het netwerk.


**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure implementeert netwerk filteren om te voorkomen dat vervalste verkeer en binnenkomende en uitgaande verkeer te beperken tot vertrouwde platform-onderdelen. Het Microsoft Azure-netwerk wordt gescheiden voor het scheiden van klantverkeer van beheer van verkeer. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-136"></a>PCI-DSS vereiste 1.3.6

**1.3.6** plaats systeemonderdelen gegevensopslag kaarthouder (zoals een database) voor de zone van een intern netwerk, gescheiden van het Perimeternetwerk en andere niet-vertrouwde netwerken.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure maakt gebruik van scheiding van netwerk- en NAT scheiden van klantverkeer van beheer van verkeer. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De architectuur van Contoso Webstore wordt voorkomen dat onbevoegde uitgaand verkeer van de omgeving in het bereik tot het Internet. Dit wordt bereikt door het uitgaande verkeer ACL's voor goedgekeurde poorten en protocollen configureren in Microsoft Azure. Toegang tot het CDE opnemen deze besturingselementen in de SQL Server-database. <br /><br />Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-137"></a>PCI-DSS vereiste 1.3.7

**1.3.7** niet onthullen privé IP-adressen en route-informatie voor niet-geautoriseerde partijen. 

> [!NOTE]
> Methoden voor het IP-adressen worden verborgen mogelijk, maar zijn niet beperkt tot:
> - Network Address Translation (NAT).
> - Servers die gegevens van de kaarthouder achter een proxy-servers/firewall plaatsen.
> - Verwijdering of filteren van de route-advertisements voor particuliere netwerken die gebruikmaken van met geregistreerd adressering.
> - Intern gebruik van de adresruimte RFC1918 in plaats van de geregistreerde adressen.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure maakt gebruik van NAT (Network Address Translation) en netwerk scheiding scheiden van klantverkeer van beheer van verkeer. Azure-apparaten uniek worden aangeduid met hun UUID en worden geverifieerd met Kerberos. Azure beheerd netwerk apparaten door RFC 1918-IP-adressen worden geïdentificeerd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore plaatst alle kaarthouder gegevens achter een proxy-servers/firewall en RFC1918-adresruimte wordt intern gebruikt.|



## <a name="pci-dss-requirement-14"></a>PCI-DSS vereiste 1.4

**1.4** persoonlijke firewall-software of een vergelijkbare functionaliteit installeren op een draagbare apparaten (inclusief bedrijf en/of werknemer) die verbinding maken met Internet wanneer u zich buiten het netwerk (bijvoorbeeld laptops gebruikt door werknemers), en Dit worden ook gebruikt voor toegang tot het CDE. Firewall (of gelijkwaardige) configuraties zijn onder meer:-specifieke configuratie-instellingen zijn gedefinieerd.
- Persoonlijke firewall (of gelijkwaardige functionaliteit) actief wordt uitgevoerd.
- Persoonlijke firewall (of gelijkwaardige functionaliteit) is niet alterable door gebruikers van de draagbare apparaten.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt bescherming van de eindgebruiker geen apparaten. [Microsoft Intune](https://www.microsoft.com/cloud-platform/microsoft-intune) kan worden gebruikt voor het beheren van de mobiele apparaten die uw werknemers gebruikt voor toegang tot bedrijfsgegevens.|



## <a name="pci-dss-requirement-15"></a>PCI-DSS eis 1.5

**1.5** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het beheren van firewalls gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt op het moment van implementatie van de configuraties van de Azure-toepassing firewall zodat alleen opgegeven bereiken van IP-adressen voor toegang tot de site, de bastionomgeving Azure VM's opnemen in hun CDE.|




