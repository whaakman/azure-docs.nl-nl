---
title: Azure betaling verwerking blauwdruk - beveiligde systeemvereisten
description: PCI-DSS vereiste 6
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 79889fdb-52d2-42db-9117-6e2f33d501e0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 24c8d90d3fec27258165472e99ba3d36ffcba733
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="secure-system-requirements-for-pci-dss-compliant-environments"></a>Systeemvereisten voor PCI DSS-compatibele omgevingen beveiligen 
## <a name="pci-dss-requirement-6"></a>PCI-DSS vereiste 6

**Ontwikkelen en te onderhouden van veilige systemen en toepassingen**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Hier geen personen gebruik beveiligingsproblemen voor bevoorrechte toegang tot systemen. Veel van deze beveiligingsproblemen worden opgelost door de leverancier geleverde beveiligingspatches, die moeten worden geïnstalleerd door de entiteiten die de systemen beheren. Alle systemen moeten alle betreffende softwarepatches te beschermen tegen misbruik en inbreuk op gegevens van de kaarthouder door kwaadwillende personen en schadelijke software hebben.

> [!NOTE]
> Juiste softwarepatches zijn patches die zijn geëvalueerd en voldoende getest om te bepalen dat de patches niet in strijd met bestaande beveiligingsconfiguraties. Voor intern ontwikkelde toepassingen zijn verschillende beveiligingsproblemen met behulp van standaardbesturingssysteem ontwikkelingsprocessen vermeden en beveiligd technieken codering.

## <a name="pci-dss-requirement-61"></a>PCI-DSS vereiste 6.1

**6.1** een proces voor het identificeren van beveiligingsproblemen, met behulp van betrouwbare buiten bronnen voor informatie over de kwetsbaarheid van beveiliging en een risico plaatsen (bijvoorbeeld als 'hoog', 'Gemiddeld,' of 'laag') en nieuwe gedetecteerde security toewijzen beveiligingsproblemen.

> [!NOTE]
> Risico classificaties moeten worden gebaseerd op de beste praktijken, evenals een afweging van potentiële impact. Criteria voor de rangschikking van zwakke plekken omvat mogelijk rekening wordt gehouden met de CVSS base score en/of de classificatie door de leverancier en/of systemen die van invloed op een type. 
> 
> Methoden voor het evalueren van beveiligingsproblemen en risico classificaties toe te wijzen variëren, afhankelijk van de omgeving en de strategie van de risico-evaluatie van een organisatie. Risico classificaties moeten ten minste alle zwakke plekken beschouwd als 'hoog risico' aan de omgeving identificeren. Naast de ranking risico kunnen zwakke plekken die worden overwogen 'kritiek' als ze een onmiddellijke bedreiging vormt voor de omgeving, impact kritieke systemen, en/of zou leiden tot een (mogelijke) schending als dat niet behandeld. Voorbeelden van kritieke systemen kunnen beveiligingssystemen, openbare-apparaten en systemen, databases en andere systemen die, wordt opslaat zijn of kaarthouder gegevens verzenden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Procedures zijn gemaakt en geïmplementeerd om te scannen op beveiligingsproblemen op hypervisorhosts in de grens van het bereik. Beveiligingslek scannen wordt uitgevoerd op server-besturingssystemen, databases en netwerkapparaten met de juiste beveiligingslek scanning-hulpprogramma's. De scans op zwakke plekken worden per kwartaal minimaal uitgevoerd. Microsoft Azure-opdrachten met onafhankelijke beoordelaars binnendringen testen van de Microsoft Azure-grens. Rood Team oefeningen ook regelmatig worden uitgevoerd en resultaten gebruikt om de verbeterde beveiliging. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met WAF en de ruleset OWASP is ingeschakeld. Zie voor meer informatie [richtlijnen PCI - beperking van de risico's van beveiligingsproblemen](payment-processing-blueprint.md#application-gateway).|



## <a name="pci-dss-requirement-62"></a>PCI-DSS vereiste 6.2

**6.2** ervoor dat alle onderdelen van het systeem en software van bekende beveiligingsproblemen worden beveiligd door het installeren van de betreffende leverancier beveiligingspatches. Kritieke beveiligingspatches binnen een maand na de release installeren.

> [!NOTE]
> Kritieke beveiligingspatches moeten worden aangeduid volgens het proces dat is gedefinieerd in de vereiste 6.1 rangschikking risico.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor alle netwerkapparaten en hypervisor OS-software is beveiligd tegen bekende beveiligingsproblemen door het installeren van de betreffende leverancier beveiligingspatches. Tenzij een klant vraagt om de service niet te gebruiken, wordt een beheerproces patch bestaat om ervoor te zorgen dat besturingssysteem niveau beveiligingsproblemen worden voorkomen en tijdig worden hersteld. Productieservers worden gescand voor het valideren van naleving van de patch op maandelijkse basis. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is een oplossing voor PaaS-service. Azure biedt onderhoud van alle service-patches.|



## <a name="pci-dss-requirement-63"></a>PCI-DSS vereiste 6.3

**6.3** ontwikkelen van interne en externe softwaretoepassingen (waaronder web gebaseerde beheerderstoegang tot toepassingen) veilig, als volgt:
- PCI DSS (bijvoorbeeld veilige verificatie en logboekregistratie) in overeenstemming met
- Op basis van industrienormen en/of aanbevolen procedures
- Het opnemen van informatiebeveiliging gedurende de levenscyclus van software-ontwikkeling 

> [!NOTE]
> Dit geldt voor alle software die intern zijn ontwikkeld, evenals op maat gemaakte of aangepaste software die is ontwikkeld door derden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure-toepassingen en eindpunten worden ontwikkeld in overeenstemming met de Microsoft Security Development Lifecycle (SDL)-methode die in overeenstemming met DSS-vereisten. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is ontworpen om aanbevolen procedures voor het beveiligen van CHD bedrijfstak volgt. Richtlijnen voor de implementatie biedt de details van de beveiliging meassures en logboekregistratie is ingeschakeld.|



### <a name="pci-dss-requirement-631"></a>PCI-DSS vereiste 6.3.1

**6.3.1** ontwikkeling, testen en/of aangepaste toepassing accounts, gebruikersnamen en wachtwoorden verwijderen voordat toepassingen actief of worden vrijgegeven aan klanten.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Laatste beveiliging controleren (FSR) wordt uitgevoerd voor belangrijke releases vóór de productie-implementatie met een aangewezen Security Advisor buiten het Azure ontwikkelteam om te controleren of alleen toepassingen gereed voor productie zijn vrijgegeven. Als onderdeel van de laatste evaluatie wordt gewaarborgd dat alle testaccounts en de testgegevens zijn verwijderd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een gefaseerde installatie service die is geregistreerd en geïsoleerd. Elk van de lagen netwerk heeft een speciale netwerkbeveiligingsgroep [NSG]. Zie voor meer informatie [richtlijnen PCI - Netwerkbeveiligingsgroepen](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-632"></a>PCI-DSS vereiste 6.3.2

**6.3.2** revisie aangepaste code vóór de release voor productie of klanten om te kunnen identificeren van eventuele codering, beveiligingsprobleem (met behulp van handmatige of geautomatiseerde processen) ten minste het volgende omvatten:
- Codewijzigingen worden gecontroleerd door personen dan de auteur van het oorspronkelijke code en personen kennis heeft van code-revisie technieken en veilige procedures coderen.
- Code beoordelingen zorg code is ontwikkeld overeenkomstig de richtlijnen voor veilige codering
- Correcties zijn geïmplementeerd vóór de release
- Resultaten van de code beoordeling zijn gecontroleerd en goedgekeurd door vóór de release management 

> [!NOTE]
> Deze vereiste voor revisies code geldt voor alle aangepaste code (interne en openbare), als onderdeel van de levenscyclus van de systeem-ontwikkeling. 
>
> Beoordelingen van code kunnen worden uitgevoerd door geïnformeerde interne personeel of derden. Openbare webtoepassingen gelden ook extra besturingselementen, adres doorlopende bedreigingen en beveiligingsproblemen na implementatie, zoals gedefinieerd op PCI DSS vereiste 6.6.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure-toepassingen en eindpunten worden ontwikkeld in overeenstemming met de Microsoft Security Development Lifecycle (SDL)-methode. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een gefaseerde installatie service die is geregistreerd en geïsoleerd. Elk van de lagen netwerk heeft een speciale netwerkbeveiligingsgroep [NSG]. Zie voor meer informatie [richtlijnen PCI - Netwerkbeveiligingsgroepen](payment-processing-blueprint.md#network-security-groups).|



## <a name="pci-dss-requirement-64"></a>PCI-DSS vereiste, 6.4

**6.4** Volg wijzigen besturingselement processen en procedures voor alle wijzigingen in de onderdelen van het systeem. De processen vergezeld gaan van de volgende (Zie vereisten 6.4.1-6.4.6).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft volgt NIST richtlijnen met betrekking tot beveiligingsoverwegingen in softwareontwikkeling in de beveiliging van deze informatie moeten worden geïntegreerd in de SDLC van systeem begin. Continue integratie van procedures voor beveiliging in de Microsoft SDL kunnen:<ul><li>Vroege identificatie en beperking van beveiligingsrisico's en onvolkomenheden</li><li>Bewustzijn van mogelijke software problemen veroorzaakt door vereist beveiligingsmechanismen coderen</li><li>Identificatie van gedeelde beveiligingsservices en het hergebruik van best practices hulpprogramma's voor beveiliging, wat zorgt voor betere beveiligingspostuur via beproefde methoden en technieken</li><li>Afdwinging van het programma van Microsoft al uitgebreide risico management</li></ul>Microsoft Azure wijziging tot stand heeft gebracht en de vrijgave van beheerprocessen om te bepalen van de implementatie van grote wijzigingen, met inbegrip van:<ul><li>De identificatie en de documentatie van de geplande wijziging</li><li>Identificatie van zakelijke doelstellingen, prioriteiten en scenario's bij het plannen van het product</li><li>Specificatie van het ontwerp van de functie/onderdeel</li><li>Operationele gereedheid controleren op basis van een vooraf gedefinieerde criteria /-controlelijst om algemene risico's / impact vast te stellen</li><li>Testen, autorisatie en wijzigingsbeheer op basis van binnenkomt/verlaat criteria voor ontwikkeling (ontwikkeling), INT (integratie testen), fase (voor preproductie) en PROD (productie) omgevingen naar gelang van toepassing. Klanten zijn verantwoordelijk voor hun eigen toepassingen die worden gehost in Microsoft Azure.</li></ul> |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De demo Contoso Webstore biedt een gefaseerde installatie service die is geregistreerd en geïsoleerd. <br /><br />Elk van de lagen netwerk heeft een speciale netwerkbeveiligingsgroep [NSG]. Zie voor meer informatie [richtlijnen PCI - Netwerkbeveiligingsgroepen](payment-processing-blueprint.md#network-security-groups).<br /><br />Wijzigingen worden vastgelegd met behulp van Operations Management Suite en Runbooks worden gebruikt voor het verzamelen van Logboeken. [Operations Management Suite (OMS)](/azure/operations-management-suite/) biedt uitgebreide logboekregistratie van wijzigingen. Wijzigingen worden beoordeeld en gecontroleerd op juistheid. Zie voor meer specifieke instructies [richtlijnen PCI - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-641"></a>PCI-DSS vereiste 6.4.1

**6.4.1** afzonderlijke omgevingen van ontwikkeling en testen van productieomgevingen en afdwingen van de scheiding tussen met toegangsbeheer.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een gefaseerde installatie service die is geregistreerd en geïsoleerd. Elk van de lagen netwerk heeft een speciale netwerkbeveiligingsgroep [NSG]. Zie voor meer informatie [richtlijnen PCI - Netwerkbeveiligingsgroepen](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-642"></a>PCI-DSS vereiste 6.4.2

**6.4.2** scheiding van functies tussen ontwikkeling en testen en productie-omgevingen

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een gefaseerde installatie service die is geregistreerd en geïsoleerd. Elk van de lagen netwerk heeft een speciale netwerkbeveiligingsgroep [NSG]. Zie voor meer informatie [richtlijnen PCI - Netwerkbeveiligingsgroepen](payment-processing-blueprint.md#network-security-groups).|



### <a name="pci-dss-requirement-643"></a>PCI-DSS vereiste 6.4.3

**6.4.3** productiegegevens (live pannen) worden niet gebruikt voor testdoeleinden of ontwikkeling.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft geen primaire account nummer (PAN) gegevens live.|



### <a name="pci-dss-requirement-644"></a>PCI-DSS vereiste 6.4.4

**6.4.4** verwijderen van testgegevens en accounts van onderdelen van het systeem voordat het systeem geactiveerd wordt of naar de productie overschakelt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft geen testaccounts.|



### <a name="pci-dss-requirement-645"></a>PCI-DSS vereiste 6.4.5

**6.4.5** controleprocedures wijzigen voor de implementatie van beveiligingspatches en software wijzigingen moeten het volgende bevatten:
- **6.5.4.1** documentatie van impact.
- **6.5.4.2** gedocumenteerde goedkeuring door geautoriseerde partijen wijzigen.
- **6.5.4.3** functionaliteit testen om te controleren dat de wijziging niet nadelig de beveiliging van het systeem beïnvloedt.
- **6.5.4.4** procedures voor back-out.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is een oplossing voor PaaS-service. En Azure biedt onderhoud van alle service-patches.|



### <a name="pci-dss-requirement-646"></a>PCI-DSS vereiste 6.4.6

**6.4.6** na voltooiing van een belangrijke wijziging, alle relevante PCI DSS-vereisten moeten worden geïmplementeerd op alle nieuwe of gewijzigde systemen en netwerken en documentatie, indien van toepassing is bijgewerkt.

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is een oplossing voor PaaS-service. En Azure biedt onderhoud van alle service-patches.|



## <a name="pci-dss-requirement-65"></a>PCI-DSS vereiste 6.5

**6.5** adres algemene coderen beveiligingslekken in software ontwikkelingsprocessen als volgt:
- Training van ontwikkelaars ten minste jaarlijks in up-to-date beveiligde codering technieken, waaronder het voorkomen van veelvoorkomende codering beveiligingslekken.
- Ontwikkel toepassingen op basis van beveiligde richtlijnen voor codering.

> [!NOTE]
> De kwetsbaarheden die worden vermeld op 6.5.1 via 6.5.10 zijn de meest recente beste praktijken wanneer deze versie van PCI DSS is gepubliceerd. Echter, zoals bedrijfstak aanbevolen procedures voor het beheer van het beveiligingsprobleem (bijvoorbeeld de Guide OWASP SANS CWE Top 25, CERT Secure coderen, enz.) zijn bijgewerkt, kan de huidige best practices moeten worden gebruikt voor deze vereisten. 
> 
> [!NOTE]
> Vereisten 6.5.1 via 6.5.6, hieronder, voor alle toepassingen gelden (intern of extern). Vereisten 6.5.7 via 6.5.10, hieronder, van toepassing op webtoepassingen en toepassingsinterfaces (intern of extern). 

- **6.5.1** injectie beveiligingsfouten, met name SQL-injectie. U kunt ook OS opdracht injectie, LDAP en XPath injectie fouten, evenals andere injectie beveiligingsfouten.
- **6.5.2** buffer overschrijdt
- **6.5.3** onbeveiligde cryptografische opslag
- **6.5.4** onbeveiligde communicatie
- **6.5.5** onjuiste foutafhandeling
- **6.5.6** alle 'hoog risico' zwakke plekken die worden geïdentificeerd in het proces van de identificatie beveiligingslek (zoals gedefinieerd in PCI DSS vereiste 6.1)
- **6.5.7** Cross-site scripting (XSS)
- **6.5.8** onjuiste toegangsbeheer (zoals onbeveiligde direct objectverwijzingen, niet kan worden beperkt toegangs-URL, verandering van directory en gebruikerstoegang tot functies beperken is mislukt)
- **6.5.9** Cross-site aanvraag kunnen worden vervalst (CSRF)
- **6.5.10** verificatie-en sessie wordt verbroken

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 6.4](#pci-dss-requirement-6-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore demo biedt richtlijnen voor veilige ontwikkeling, een GSD en threat model ter illustratie van de procedures voor veilige ontwikkeling.|



## <a name="pci-dss-requirement-66"></a>PCI-DSS vereiste 6.6

**6.6** voor webtoepassingen openbare adres van de nieuwe dreigingen voor en zwakke plekken voortdurend en zorg ervoor dat deze toepassingen zijn beschermd tegen bekende aanvallen door een van de volgende methoden:

- Openbare-webtoepassingen via handmatige of automatische toepassing beveiligingslek beveiliging analyseprogramma's of methoden, controleren en na wijzigingen ten minste eenmaal per jaar 

   > [!NOTE]
   > Deze evaluatie is niet hetzelfde zijn als de scans op zwakke plekken van vereiste 11.2 uitgevoerd. 

- Installatie van een geautomatiseerde technische oplossing die detecteert en voorkomt aanvallen op het web (bijvoorbeeld een web application firewall) voor openbare-webtoepassingen, voortdurend al het verkeer controleren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Voordat toepassingen op de productie-omgeving zijn geïmplementeerd, Microsoft Azure openbare webtoepassingen getest als onderdeel van de SDL-proces. Bovendien scant Microsoft alle openbare webtoepassingen in productie op gezette tijden voor het detecteren van alle mogelijke beveiligingsproblemen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De referentie-oplossing vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met WAF en de ruleset OWASP is ingeschakeld. Zie voor meer informatie [richtlijnen PCI - beperking van de risico's van beveiligingsproblemen](payment-processing-blueprint.md#application-gateway).|



## <a name="pci-dss-requirement-67"></a>PCI-DSS vereiste 6.7

**6.7** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het ontwikkelen en onderhouden van veilige systemen en toepassingen zijn gedocumenteerd in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De referentie-oplossing vermindert het risico van beveiligingsproblemen met behulp van een toepassingsgateway met WAF en de ruleset OWASP is ingeschakeld. Zie voor meer informatie [richtlijnen PCI - beperking van de risico's van beveiligingsproblemen](payment-processing-blueprint.md#application-gateway).|




