---
title: Azure blauwdruk Automation - nationale Cyberbeveiliging centrum Cloud Security beveiligingsprincipes-overzicht
description: Azure blauwdruk Automation - nationale Cyberbeveiliging centrum Cloud Security beveiligingsprincipes-overzicht
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 49652fd9-b8c6-4a88-bc5e-0b58a0260ed6
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/17/2017
ms.author: jomolesk
ms.openlocfilehash: 0be18e2c2354ea8f766eb48db793c906e565a201
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="national-cyber-security-centre-cloud-security-principles-overview"></a>Nationale Cyberbeveiliging centrum Cloud Security beveiligingsprincipes-overzicht


> [!NOTE]
> Deze beveiligings-principals zijn gedefinieerd door de VK nationale Cyberbeveiliging Security Center (NCSC). Raadpleeg [NCSC documentatie](https://www.ncsc.gov.uk/guidance/implementing-cloud-security-principles) voor informatie over het testen van de procedures en richtlijnen voor elke beveiligingsprincipal.



## <a name="ncsc-cloud-security-principle-1"></a>NCSC Cloud beveiligingsprincipe 1
### <a name="data-in-transit-protection"></a>Gegevens in de doorvoer-beveiliging
Gebruikersgegevens doorvoer netwerken moeten voldoende worden beschermd tegen knoeien en inbreuk bij.

Dit moet worden bereikt via een combinatie van:

- netwerkbeveiliging - de aanvaller de mogelijkheid gegevens te onderscheppen weigeren
- codering - weigeren de aanvaller de mogelijkheid om gegevens te lezen


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | Deze Azure blauwdruk configureert bronnen te laten communiceren met alleen beveiligde protocollen. Het onderdeel WAF van de toepassingsgateway is geconfigureerd voor de schrijver van externe gebruikt via HTTPS/TLS accepteren en communiceren met de back-endpool alleen via HTTPS/TLS. Extern bureaublad-services zijn geconfigureerd voor gebruik van beveiligde verbindingen. VPN wordt gebruikt om veilig webverkeer tussen AppGateway en Azure. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure maakt gebruik van de industriestandaard-Transport Layer Security (TLS) 1.2-protocol met 2048-bits RSA/SHA256 versleutelingssleutels, zoals aanbevolen door CESG/NCSC voor het versleutelen van communicatie tussen de klant en de cloud, zowel intern tussen Azure en datacentres. Wanneer beheerders de Microsoft Azure-Portal gebruikt voor het beheren van de service voor hun organisatie, kunt u voor de gegevens die tussen de portal en de beheerder apparaat verzonden, wordt verzonden via een versleuteld TLS-kanaal. Wanneer een e-gebruiker verbinding met een standaard webbrowser Outlook.com maakt, biedt de HTTPS-verbinding een beveiligd kanaal voor het ontvangen en verzenden van e-mail.<br /> <br /> Azure biedt zijn klanten tal van opties voor het beveiligen van eigen gegevens en het verkeer. De beheerfuncties van certificaat is ingebouwd in Azure biedt beheerders flexibiliteit voor het configureren van certificaten en versleutelingssleutels voor beheersystemen, afzonderlijke services, secure shell (SSH) sessies, virtueel particulier netwerk (VPN)-verbindingen verbindingen met extern bureaublad (RDP), en andere functies. <br /><br /> Ontwikkelaars kunnen de cryptografische serviceproviders (CSP's) dat is ingebouwd in de Microsoft .NET Framework gebruiken voor toegang tot de algoritmen Advanced Encryption Standard (AES), samen met functionaliteit voor het afhandelen van taken zoals het valideren van digitale Secure Hash Algorithm (SHA-2) handtekeningen. Azure Sleutelkluis kunt klanten cryptografische sleutels en geheimen beveiligen door ze te slaan in hardware security modules (HSM's). |


 ## <a name="ncsc-cloud-security-principle-2"></a>NCSC Cloud beveiligingsprincipe 2
### <a name="asset-protection-and-resilience"></a>Asset beveiligings- en herstelmogelijkheden
Gebruikersgegevens en de activa is opgeslagen of verwerkt, moeten worden beschermd tegen fysieke knoeien, verlies, schade of overname.

De aspecten rekening moet houden zijn:

1. Fysieke locatie en rechtsgebied
2. Datacenters beveiliging
3. Data-at-Rest-beveiliging
4. Gegevens Sanitisation
5. Verwijderen van apparatuur
6. Fysieke herstelmogelijkheden en beschikbaarheid


 ## <a name="ncsc-cloud-security-principle-21"></a>NCSC Cloud beveiligingsprincipe 2.1
### <a name="physical-location-and-legal-jurisdiction"></a>Fysieke locatie en rechtsgebied
Om te begrijpen van de juridische omstandigheden waaronder uw gegevens kan worden geopend zonder uw toestemming moet u de locaties waar ze worden opgeslagen, verwerkt en beheerd identificeren.
U gaat ook nodig om te begrijpen hoe afhandeling van gegevens besturingselementen in de service worden afgedwongen, ten opzichte van VK wetgeving. Ongeschikte bescherming van gebruikersgegevens kan leiden tot juridische en wettelijke sancties of reputatie schade.


**Verantwoordelijkheden:**`Customer`

> [!NOTE]
> Azure-services regionaal zijn geïmplementeerd en klanten bepaalde Azure services voor het opslaan van gegevens van de klant slechts in één regio kunnen configureren. Microsoft Azure bevat een lijst van wereldwijd beschikbare datacentres zodat de beschikbaarheid en betrouwbaarheid op wereldwijde schaal. Alle Azure datacentres zijn tegen de ISO/IEC 27001: 2013 gecertificeerd. De Geo VK bestaat uit 2 regio's: VK Zuid en VK West.

|||
|---|---|
| **De klant** | Deze Azure blauwdruk vraagt de beheerder voor welke regio Azure-bronnen te implementeren. De aanbevolen regio's voor implementatie zijn VK Zuid of VK West. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Niet van toepassing |


 ## <a name="ncsc-cloud-security-principle-22"></a>NCSC Cloud beveiligingsprincipe 2.2
### <a name="datacentre-security"></a>Datacenters beveiliging
Locaties die worden gebruikt om uw cloudservices moeten fysieke bescherming tegen ongeoorloofde toegang, knoeien, diefstal of herconfiguratie van systemen. Onvoldoende bescherming kunnen leiden tot het vrijgeven, afwijking of verlies van gegevens.


**Verantwoordelijkheden:**`Microsoft Azure`


|||
|---|---|
| **De klant** | Klanten geen fysieke toegang tot alle systeemresources in Azure datacentres; datacenters beveiliging veiligheidsmaatregelen worden geïmplementeerd en beheerd door Microsoft Azure. Deze methode is overgenomen van Microsoft Azure. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure implementeert dit principe namens klanten. Microsoft Azure wordt uitgevoerd in geografisch verspreide Microsoft faciliteiten ruimte en hulpprogramma's delen met andere Microsoft online services. Elke faciliteit is ontworpen om te 24 x 7 x 365 en verschillende industriestandaard maatregelen kunt u bewerkingen beveiligen tegen stroomstoring en fysieke inbraakdetectie netwerkstoringen zet. Deze datacentres in overeenstemming met de industriestandaarden (zoals ISO 27001) voor de fysieke beveiliging en beschikbaarheid. Ze worden beheerd, bewaakt en beheerd door de medewerkers van Microsoft-bewerkingen. <br /> <br /> Azure-klanten kunnen worden er zeker van te zijn dat fysieke beveiligingsmaatregelen aanwezig zijn op alle Azure datacentres vanwege Azure certificaten op alle datacentres voor de ISO/IEC 27001: 2013 standaard houden. De Geo VK bestaat uit 2 regio's: VK Zuid en VK West. |


 ## <a name="ncsc-cloud-security-principle-23"></a>NCSC Cloud beveiligingsprincipe 2.3
### <a name="data-at-rest-protection"></a>Data-at-Rest-beveiliging
Om te controleren of gegevens zijn niet beschikbaar voor niet-geautoriseerde partijen met fysieke toegang tot infrastructuur, moeten de gebruikersgegevens die zijn opgeslagen in de service worden beveiligd ongeacht het opslagmedium waarop deze wordt bewaard. Zonder de passende maatregelen erin mogelijk gegevens per ongeluk worden vermeld op verwijderde, verloren of gestolen media.


**Verantwoordelijkheden:**`Shared`

> [!NOTE]
> Alle Microsoft Azure zijn klanten eenvoudig biedt versleuteling-oplossingen integreren met Azure Key Vault, kunt u eenvoudig beheer van versleutelingssleutels.

|||
|---|---|
| **De klant** | Vertrouwelijkheid en integriteit van alle geïmplementeerd met deze oplossing blauwdruk Azure blob-opslag is beveiligd door gebruik te maken van Azure Storage Service versleuteling (SSE). SSE beschermt gegevens in rust in Azure storage-accounts met behulp van 256-bits AES-versleuteling. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure biedt een breed scala aan mogelijkheden voor versleuteling, waardoor klanten de flexibiliteit om te kiezen welke oplossing het beste voldoet aan hun behoeften. Azure Sleutelkluis kunt klanten eenvoudig en kosten effectief beheer van sleutels die door cloudtoepassingen en -services worden gebruikt om gegevens te versleutelen. Azure Disk Encryption kunnen klanten voor het versleutelen van virtuele machines. Azure Storage-Service: versleuteling, maakt het mogelijk alle gegevens die zijn opgenomen in de storage-account van de klant te versleutelen. |


 ## <a name="ncsc-cloud-security-principle-24"></a>NCSC Cloud beveiligingsprincipe 2.4
### <a name="data-sanitisation"></a>Gegevens Sanitisation
Het proces van inrichting, migreren en de inrichting bronnen mogen niet leiden tot onbevoegde toegang tot gebruikersgegevens.

Onvoldoende opschoning van gegevens kan leiden tot:

- Gebruikersgegevens te bewaren door de serviceprovider voor onbepaalde tijd
- Gebruikersgegevens toegankelijk voor andere gebruikers van de service wordt als bronnen worden hergebruikt
- Gebruikersgegevens verloren gaat of vermeld op de media verwijderde, verloren of gestolen.


**Verantwoordelijkheden:**`Microsoft Azure`


|||
|---|---|
| **De klant** | Microsoft Azure biedt contractueel garanties met betrekking tot de gegevensverwijdering van de permanente in Azure. Als zodanig wordt dit principe overgenomen van Microsoft Azure. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure volgt het proces van de verwijdering NIST SP800-88r1 met gegevensclassificatie FIPS-199 gemiddeld uitgelijnd. NIST biedt voor beveiligde wissen benadering (via de firmware van de harde schijf) voor schijven die dit ondersteunen. Voor harde schijven die Microsoft kunnen niet worden gewist vernietigt u ze op en geeft u het herstel van gegevens onmogelijk (bijv, uiteenspringen bestaat, versnipperen, pulverize of verbranding). De juiste wijze van verwijdering wordt bepaald door het activatype. Records van de vernietiging worden bewaard. Alle Microsoft Azure-services gebruikmaken van goedgekeurde mediaservices voor opslag en verwijdering. <br />  <br /> Na het verlopen of beëindiging van een serviceabonnement kan de klant contact opnemen met Microsoft en vertel of: <br /><br /> (1) van de klant account uitschakelen en verwijder vervolgens de klantgegevens; of <br /> (2) behouden de gegevens die zijn opgeslagen in de online service in een account met beperkte functionaliteit ten minste 90 dagen na het verlopen of beëindiging van het abonnement van de klant (de ' bewaarperiode'), zodat die klant de gegevens kan extraheren. Microsoft zal na de vervaldatum van de bewaarperiode van de klant account uitschakelen en alle gegevens verwijderen. In de cache opgeslagen of back-ups wordt opgeschoond binnen 30 dagen na het einde van de bewaarperiode. |


 ## <a name="ncsc-cloud-security-principle-25"></a>NCSC Cloud beveiligingsprincipe 2,5
### <a name="equipment-disposal"></a>Verwijderen van apparatuur
Zodra de apparatuur die worden gebruikt voor het leveren van een service het einde van de levensduur bereikt, moet deze op een manier die de beveiliging van de service of de gebruikersgegevens zijn opgeslagen in de service komt niet in gevaar worden verwijderd.


**Verantwoordelijkheden:**`Microsoft Azure`


|||
|---|---|
| **De klant** | Klanten geen fysieke toegang tot alle systeemresources in Azure datacentres; apparatuur verwijdering procedures zijn geïmplementeerd en beheerd door Microsoft Azure. Deze methode is overgenomen van Microsoft Azure. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure implementeert dit principe namens klanten. Microsoft operationele medewerkers Volg strenge procedures en processen voor hardware-verwijdering om te waarborgen dat geen hardware met gegevens van de klant de afhandeling van gegevens wordt bij een systeem einde van de levenscyclus, beschikbaar gesteld voor niet-vertrouwde partijen. Microsoft Azure volgt het proces van de verwijdering NIST SP800-88r1 met gegevensclassificatie FIPS-199 gemiddeld uitgelijnd. NIST biedt voor beveiligde wissen benadering (via de firmware van de harde schijf) voor schijven die dit ondersteunen. Voor harde schijven die Microsoft kunnen niet worden gewist vernietigt u ze op en geeft u het herstel van gegevens onmogelijk (bijv, uiteenspringen bestaat, versnipperen, pulverize of verbranding). De juiste wijze van verwijdering wordt bepaald door het activatype. Records van de vernietiging worden bewaard. Alle Microsoft Azure-services gebruikmaken van goedgekeurde mediaservices voor opslag en verwijdering. |


 ## <a name="ncsc-cloud-security-principle-26"></a>NCSC Cloud beveiligingsprincipe 2.6
### <a name="physical-resilience-and-availability"></a>Fysieke herstelmogelijkheden en beschikbaarheid
Services hebben verschillende niveaus van herstelmogelijkheden, waardoor de mogelijkheid om te werken normaal in het geval van storingen, incidenten of aanvallen. Een service zonder gegarandeerde beschikbaarheid mogelijk niet beschikbaar zijn, potentieel gedurende langere tijd, ongeacht de gevolgen voor uw bedrijf.


**Verantwoordelijkheden:**`Shared`

> [!NOTE]
> Als de klant op de juiste wijze Microsoft Azure configureert te maken met Azure Site Recovery en alternatieve opslag van gegevens op een andere geo grafisch bevindt Datacenter, kan Microsoft Azure de ononderbroken werking van de klant geïmplementeerde resources ondersteunen.

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het tot stand brengen van een site alternatieve opslaggroep. De instructie klant besturingselement implementatie van de klant de mogelijkheid om te werken in het geval van een incident dient te houden. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure heeft VK nationale Cyberbeveiliging Security Center (NCSC) goedgekeurd datacentres in verschillende geografische locaties (VK Zuid en VK West) zodat de herstelmogelijkheden en beschikbaarheid. Dit is de verantwoordelijkheid van de klant te reserveren capaciteit in een andere regio met behulp van Azure Site Recovery-service. Wanneer ze Azure Site Recovery hebt geconfigureerd, wordt Azure services starten en stoppen van de klant in een naadloze overgang naar de site alternatieve verwerking. |


 ## <a name="ncsc-cloud-security-principle-3"></a>NCSC Cloud beveiligingsprincipe 3
### <a name="separation-between-users"></a>Scheiding tussen gebruikers
Een schadelijke of verdachte gebruiker van de service moet niet kunnen invloed hebben op de service of de gegevens van een andere.

Factoren die van de scheiding van de gebruiker zijn onder andere:

- waar de besturingselementen scheiding worden geïmplementeerd: dit wordt sterk beïnvloed door het servicemodel (bijvoorbeeld IaaS, PaaS, SaaS)
- wie u de service met deelt: dit wordt bepaald door het implementatiemodel (bijvoorbeeld public, private of community cloud)
- het niveau van zekerheid die beschikbaar zijn in de implementatie van de scheiding van besturingselementen

> [!NOTE]
> Overweeg scheiding geleverd door de berekenings-, opslag- en netwerkonderdelen in een IaaS-service. SaaS- en PaaS-services die gebaseerd zijn op IaaS kunnen, sommige van de eigenschappen van de scheiding van de onderliggende IaaS-infrastructuur overgenomen.

**Verantwoordelijkheden:**`Microsoft Azure`


|||
|---|---|
| **De klant** | Microsoft Azure zorgt ervoor dat de isolatie voor elke gebruiker om te voorkomen dat een schadelijke of verdachte gebruiker die betrekking hebben op de service of de gegevens van een andere. Als zodanig wordt dit principe overgenomen van Microsoft Azure. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Als gevolg van de klant cloud-servers worden virtuele, het fysieke scheiding paradigma niet langer van toepassing. Microsoft Azure is ontworpen om te identificeren en teller intrinsieke risico's van een multitenant-omgeving. Verwerking en opslag van gegevens is logisch gescheiden onder de gebruikers van Azure met behulp van Active Directory en -functionaliteit die specifiek zijn ontwikkeld voor multitenant services die erop gericht om ervoor te zorgen dat gebruikersgegevens opgeslagen in gedeelde Azure datacentres is niet toegankelijk is door een andere de organisatie. <br /> <br /> Fundamentele naar een gedeelde cloudarchitectuur is de isolatie voor elke gebruiker om te voorkomen dat een schadelijke of verdachte gebruiker die betrekking hebben op de service of de gegevens van een andere opgegeven. <br /> <br /> Voor meer informatie over Microsoft tenant scheiding Zie de volledige beschrijving in de [blauwdruk Azure - Cloud-beveiligingsprincipes NCSC - klant verantwoordelijkheden Matrix](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-4"></a>NCSC Cloud beveiligingsprincipe 4
### <a name="governance-framework"></a>Governance Framework
De serviceprovider moet een beveiliging governance-framework dat coördineert en stuurt het beheer van de service en de informatie in deze hebben. Technische besturingselementen geïmplementeerd buiten dit framework wordt fundamenteel ondermijnd.
Deze procedure die een framework op doeltreffende wijze van bestuur zorgt personeel, fysieke en technische besturingselementen blijven werken via de levensduur van een service. Het moet ook reageren op wijzigingen in de service, technologische ontwikkelingen en het uiterlijk van nieuwe dreigingen.


**Verantwoordelijkheden:**`Microsoft Azure`


|||
|---|---|
| **De klant** | Microsoft Azure onderhoudt een gedocumenteerde toezicht van het beveiligingskader voor Azure-services. Als zodanig wordt dit principe overgenomen van Microsoft Azure. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Het framework van de naleving Microsoft bevat een standaard methodologie voor het definiëren van naleving domeinen, bepalen welke doelstellingen voor een bepaalde team dat of asset en vast te leggen hoe de doelstellingen van domein met voldoende details worden behandeld als ze van toepassing op een bepaalde set van industrienormen, voorschriften of zakelijke vereisten. Het framework toegewezen besturingselementen aan meerdere regelgeving standaarden, waardoor Microsoft ontwerpen en bouwen van services met behulp van een gemeenschappelijke set besturingselementen, waardoor naleving vandaag de dag van een reeks regels stroomlijnen en als ze in de toekomst ontwikkelen. <br /> <br /> Microsoft nalevingsprocedures ook eenvoudiger het voor klanten bereiken van naleving op meerdere services en hun behoeften veranderende efficiënt. Betere technologie en effectieve nalevingsprocedures inschakelen samen Microsoft onderhouden en uitbreiden van een groot aantal certificaten van derden. Deze certificeringen helpen klanten gereedheid voor naleving aan hun klanten, auditors en regelgevende demonstreren. <br /> <br />  Azure voldoet aan een uitgebreide reeks internationale en de naleving van regionale en branchespecifieke standaarden, zoals ISO 27001, FedRAMP, SOC 1 en SOC 2. Naleving van de strikte beveiligingsmechanismen die is opgenomen in deze normen wordt geverifieerd door de strengere van derden audits dat Azure-services werken met en voldoen aan de hoogwaardige industrienormen, certificeringen verklaringen en machtigingen die worden gedemonstreerd. <br /> <br /> Azure is ontworpen met een nalevingsbeleid waarmee klanten adres zakelijke doelstellingen, evenals industrienormen en -voorschriften. Beveiligingskader voor het nalevingsbeleid omvat test en audit fasen, beveiligingsanalyse best practices voor risico's en benchmark beveiligingsanalyse om certificaten en verklaringen te bereiken. <br /> <br /> Voor meer informatie over de naleving van Microsoft naleving frameworks Zie de volledige beschrijving in de [blauwdruk Azure - Cloud-beveiligingsprincipes NCSC - klant verantwoordelijkheden Matrix](https://aka.ms/blueprintuk-gcrm).|


 ## <a name="ncsc-cloud-security-principle-5"></a>NCSC Cloud beveiligingsprincipe 5
### <a name="operational-security"></a>Operationele beveiliging
De service moet worden uitgevoerd en beheerd veilig om te verhinderen, detecteren of aanvallen te voorkomen. Goede bedrijfsbeveiliging vereist geen complexe, bureaucratische, tijdrovend of dure processen.

Er zijn vier elementen te overwegen:

- Configuratie- en wijzigingsbeheer - moet u zorgen dat wijzigingen in het systeem zijn correct getest en toegestaan. Wijzigingen moeten beveiligingseigenschappen onverwacht niet wijzigen
- Beveiligingslek Management - u moet herkennen en verhelpen van beveiligingsproblemen in samenstellende onderdelen
- Beschermende bewaking - plaatst u maatregelen voor het detecteren van aanvallen en niet-geautoriseerde activiteit op de service
- Incidentbeheer - Zorg ervoor dat u kunt reageren op incidenten en herstellen van een veilige, beschikbare service




 ## <a name="ncsc-cloud-security-principle-51"></a>NCSC Cloud beveiligingsprincipe 5.1
### <a name="configuration-and-change-management"></a>Configuratie- en wijzigingsbeheer
U moet een duidelijk beeld van de activa die de service, samen met hun configuraties en afhankelijkheden hebben.
Wijzigingen die van invloed kunnen zijn op de beveiliging van de service moeten worden geïdentificeerd en beheerd. Niet-geautoriseerde wijzigingen moeten worden gedetecteerd.
Wanneer de wijziging niet effectief wordt beheerd, kunnen de beveiligingsproblemen per ongeluk worden ingevoerd naar een service. En zelfs wanneer er bewustzijn van het probleem, deze kan niet worden volledig verholpen.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | De Azure Resource Manager-sjablonen en de bijbehorende bronnen waaruit deze Azure blauwdruk vertegenwoordigen een basislijn '-configuratie als code' voor de geïmplementeerde architectuur. De oplossing is via GitHub, die kan worden gebruikt voor configuratie-besturingselement opgegeven. <br /> <br /> Azure Active Directory account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan rollen bieden strikt bepalen via welke gebruikers kunnen bekijken en controle geïmplementeerd resources. Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan beveiligingsgroepen. Deze beveiligingsgroepen bepaalt de acties die gebruikers met betrekking tot de configuratie van besturingssysteem uitvoeren kunnen. Deze regelingen op basis van rollen kunnen worden uitgebreid door de klant om missie behoeften te voldoen. <br /> <br /> Om te voldoen aan deze principe verdere configuratie vereist door de klant voor gebruik in productie. Deze configuraties als zodanig moet deel uitmaken van het wijzigingsbeheerproces van de klant. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure controleert en configuratie-instellingen en basislijnconfiguraties van hardware, software en netwerk apparaten jaarlijks bijgewerkt. Wijzigingen zijn ontwikkeld, getest en goedgekeurd voordat het invoeren van de productie-omgeving van een omgeving voor ontwikkeling en/of de test. <br /> <br />Microsoft Azure is van toepassing basislijnconfiguraties met behulp van het proces wijzigings- en release voor Microsoft Azure-softwareonderdelen (bijvoorbeeld OS, Fabric, RDFE, XStore, enzovoort) en bootstrap configuratieproces voor hardware en apparaat netwerkonderdelen invoeren Microsoft Azure-productieomgeving zoals hieronder wordt beschreven. <br /> <br /> De basislijnconfiguraties vereist voor op basis van een Azure-services worden gecontroleerd door het team van Azure-beveiliging en naleving en serviceteams als onderdeel van de testen voorafgaand aan de implementatie van de productieservice. |


 ## <a name="ncsc-cloud-security-principle-52"></a>NCSC Cloud beveiligingsprincipe 5.2
### <a name="vulnerability-management"></a>Beveiligingslek Management
Serviceproviders moeten beschikken over een beheerprocessen om te bepalen, sorteren en beveiligingsproblemen te verhelpen. Services die dit niet doet, worden snel kwetsbaar voor aanvallen met openbaar bekende methoden en hulpprogramma's.


**Verantwoordelijkheden:**`Customer`


|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor beveiligingslek scannen op bronnen klant geïmplementeerd (om op te nemen toepassingen, besturingssystemen, databases en software). De instructie van de implementatie klant moet de hulpprogramma's gebruikt voor het uitvoeren van de scans op zwakke plekken adres. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Beveiligingsbeheer update beschermt systemen van bekende beveiligingsproblemen. Azure maakt gebruik van geïntegreerde implementatie systemen voor het beheren van de distributie en het installeren van beveiligingsupdates voor Microsoft-software. Azure kan ook beschikken over de middelen van de Microsoft Security Response centrum (MSRC), die wordt geïdentificeerd, bewaakt reageert op en wordt omgezet beveiligingsincidenten en in de cloud beveiligingslekken rond de klok, elke dag van het jaar. |


 ## <a name="ncsc-cloud-security-principle-53"></a>NCSC Cloud beveiligingsprincipe 5.3
### <a name="protective-monitoring"></a>Beschermende bewaking
Een service die niet effectief bewaakt voor aanvallen en misbruik de storing is waarschijnlijk niet voor het detecteren van aanvallen (geslaagde en mislukte). Als gevolg hiervan zal zijn voor kan niet snel kan reageren op potentiële inbreuk van uw omgevingen en gegevens.


**Verantwoordelijkheden:**`Customer`


|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de klant geïmplementeerde resources (voor toepassingen, besturingssystemen, databases en software zijn). De instructie klant besturingselement implementatie de mechanismen voor het detecteren, bewaken en reageren op aanvallen en misbruik storing dient te houden. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure-beveiliging is vereisten voor het bewaken van actieve gedefinieerd. Service-teams configureren active controleprogramma's in overeenstemming met deze vereisten. Actieve controleprogramma's omvatten de Monitoring Agent (MA) en System Center Operations Manager (SCOM) die zijn geconfigureerd voor het bieden van realtime-waarschuwingen aan medewerkers van Microsoft Azure-beveiliging in situaties die directe actie is vereist. |


 ## <a name="ncsc-cloud-security-principle-54"></a>NCSC Cloud beveiligingsprincipe 5.4
### <a name="incident-management"></a>Incidentbeheer
Tenzij u zorgvuldig vooraf geplande incidentbeheer processen worden uitgevoerd, zijn slechte beslissingen kunnen worden gemaakt wanneer incidenten plaatsvinden, mogelijk exacerbating de algehele impact van gebruikers.
Deze processen needn't ingewikkeld of grote hoeveelheden beschrijving vereisen, maar goed incidentbeheer wordt tot een minimum beperkt de gevolgen voor gebruikers van beveiliging, betrouwbaarheid en omgevingsitems voor een service.


**Verantwoordelijkheden:**`Shared`

> [!NOTE]
> In gevallen waar de klant beveiligingsincidenten kunnen invloed hebben op de beveiligingsstatus van Microsoft Azure, is de klant verantwoordelijk voor het verwittigen van Microsoft Azure.

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het tot stand brengen van een proces incidentbeheer klant geïmplementeerd bronnen (toepassingen, besturingssystemen, databases en software zijn). De instructie van de implementatie klant dient te houden reporting incidenten en waarschuwingen, ondersteunende tijdige incident antwoorden en doorsturen van gegevens naar de PGA en andere organisaties HMG naar gelang van toepassing. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft heeft een beveiligingsproces incidentbeheer vergemakkelijkt een gecoördineerde reactie op incidenten moet een plaatsvinden geïmplementeerd. <br /> <br /> Indien Microsoft onbevoegde toegang tot alle klantgegevens opgeslagen op de apparatuur of in de opslagruimten of niet-geautoriseerde toegang tot dergelijke apparatuur of faciliteiten leidt tot gegevensverlies, vrijgeven of wijziging van klantgegevens realiseert, is Microsoft die vermeld wordt: <br /> <br />   -De klant van de veiligheidsincident; onmiddellijk te melden <br /> -Onmiddellijk het veiligheidsincident onderzoeken en de klant voorzien van gedetailleerde informatie over het veiligheidsincident; en <br /> -Redelijke en vragen stappen nemen om de effecten en een schade ten gevolge van het veiligheidsincident te minimaliseren.  <br />  <br /> Een framework incidentbeheer is ingesteld met gedefinieerd-rollen en verantwoordelijkheden toegewezen. Het team van Windows Azure Security Incident Management (WASIM) is verantwoordelijk voor het beheer van beveiligingsincidenten, inclusief escalatie en de betrokkenheid van gespecialiseerde teams indien nodig. Azure-Managers die zijn verantwoordelijk voor overzien onderzoek en omzetten van beveiliging en privacy incidenten met ondersteuning van andere functies. <br /> <br /> Voor meer informatie over het antwoord van Microsoft Incident processen Zie de volledige beschrijving in de [blauwdruk Azure - Cloud-beveiligingsprincipes NCSC - klant verantwoordelijkheden Matrix](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-6"></a>NCSC Cloud beveiligingsprincipe 6
### <a name="personnel-security"></a>Personeelsbeveiliging
U moet een hoge mate van vertrouwen in hun betrouwbaarheid waarbij het personeel van serviceprovider toegang hebben tot uw gegevens en systemen. Uitgebreide controle wordt ondersteund door voldoende training vermindert de kans op onbedoeld of schadelijke inbreuk door medewerkers van de serviceprovider.
De serviceprovider moet personeel voldoen aan de security-controle en reguliere beveiligingstraining. Personeel in deze rollen moeten weten wat hun verantwoordelijkheden. Providers moeten verduidelijken hoe ze scherm en beheren van werknemers in bevoorrechte rollen.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor bestandscontrole personen en reguliere beveiligingstraining voor personen met toegang tot bronnen klant geïmplementeerd. De instructie van de implementatie klant moet de bestandsgroepen criteria voor rollen en de frequentie van beveiligingstraining adres. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Azure-medewerkers die Azure-services werken en klantenondersteuning (of Microsoft onderaannemers die bij het platform-bewerkingen, probleemoplossing en technische ondersteuning helpen) bieden een Microsoft ondergaan standaard achtergrond (of gelijkwaardige) controleren werknemer onderwijs, arbeid en strafrechtelijke geschiedenis evalueren. De achtergrondcontroles zijn breed in overeenstemming met de vereisten van een van de overheid VK BPSS/BS7858. Ze omvatten specifiek geen een formele identiteit te controleren.  <br /> <br /> Microsoft levert openbaarmaking voorziet in de contracten werknemer en uitbesteding. Alle juiste Microsoft-werknemers en onderaannemers nemen deel aan een Microsoft Azure gesponsorde beveiligingstraining programma dat personeel van hun verantwoordelijkheden voor informatiebeveiliging informeert. <br /> <br /> Medewerkers van Microsoft Azure-services of onderaannemers van het doorvoeren van schendingen van de beveiliging en/of het overtreden van het beveiligingsbeleid informatie verdacht zijn onderworpen aan een onderzoek en de juiste tuchtrechtelijk tot aan en inclusief beëindiging. Als de omstandigheden rechtvaardigen, Microsoft mogelijk wenden voor vervolgen door gerechtelijk onderzoek. <br /> <br /> Als aanvulling op dit systeem van achtergrondcontroles en beveiliging education, implementeert Microsoft combinaties van preventieve defensive en reactieve besturingselementen om te beschermen tegen onbevoegde ontwikkelaars en/of administratieve activiteiten, waaronder de volgende mechanismen: <br />  <br /> -Nauwe toegangsbeheer voor gevoelige gegevens, inclusief een vereiste voor tweeledige verificatie op basis van een smartcard gevoelige bewerkingen uit te voeren. <br /> -Combinaties van besturingselementen die onafhankelijk van de detectie van schadelijke activiteiten te verbeteren. <br /> -Meerdere niveaus van controle, logboekregistratie en rapportage. |


 ## <a name="ncsc-cloud-security-principle-7"></a>NCSC Cloud beveiligingsprincipe 7
### <a name="secure-development"></a>Veilige ontwikkeling
Services moeten worden ontworpen en ontwikkeld om u te identificeren en beperken van bedreigingen voor de beveiliging ervan. Het is mogelijk dat die niet zijn kwetsbaar voor beveiligingsproblemen die kunnen uw gegevens in gevaar brengen, leiden tot verlies van de service of schakel andere schadelijke activiteiten.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows biedt realtime-integriteit bestandsverificatie, beveiliging en herstel van essentiële systeembestanden die zijn geïnstalleerd als onderdeel van Windows of geautoriseerde Windows-systeemupdates via de functie Windows Resource Protection (WRP), waarbij u realtime integriteitscontrole. <br /> <br /> Windows beschikt over bescherming om te voorkomen dat code wordt uitgevoerd in de beperkte geheugenlocaties: Er is geen uitvoeren (NX), adres ruimte indeling willekeurige (ASLR) en preventie van gegevensuitvoering (DEP). <br /> <br /> Deze blauwdruk Azure worden geïmplementeerd op de host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de Microsoft Windows Defender. Windows Defender is geconfigureerd voor automatisch bijwerken van zowel de antimalware-engine en beveiliging de handtekeningen als release beschikbaar. <br /> <br /> Om te voldoen aan deze principe verdere configuratie vereist door de klant voor gebruik in productie. Deze configuraties als zodanig moet deel uitmaken van de beveiligde ontwikkelingsproces van de klant. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft Security Development Lifecycle (SDL) biedt een effectieve threat modellering proces om dreigingen en zwakke plekken in software en services te identificeren. Threat modellering is van een team oefening die omvat de operations manager, programma of een ander project managers, ontwikkelaars en testers, en een sleutelbeveiliging analysis-taak uitgevoerd voor het ontwerp van de oplossing vertegenwoordigt. Het hulpprogramma SDL Threat modellering teamleden gebruiken als model voor alle services en projecten, wanneer ze zijn gebaseerd en wanneer ze zijn bijgewerkt met nieuwe functies en functionaliteit. Threat modellen hebben betrekking op alle code weergegeven op de kwetsbaarheid voor aanvallen en alle code geschreven door of in licentie gegeven door een derde partij, en overweeg van alle grenzen van vertrouwensrelaties. Het systeem STRIDE (Spoofing, Tampering, Repudiation, vrijgeven van informatie, DOS-aanval en bevoegdheden) wordt gebruikt om te identificeren en bedreigingen vroeg in het ontwerpproces op te lossen voordat ze kunnen invloed hebben op klanten. |


 ## <a name="ncsc-cloud-security-principle-8"></a>NCSC Cloud beveiligingsprincipe 8
### <a name="supply-chain-security"></a>Beveiliging van de toeleveringsketen
De serviceprovider moet ervoor zorgen dat de toeleveringsketen naar tevredenheid alle van de beveiligings-principals die de service ondersteunt voor het implementeren van claims.
Cloudservices zijn vaak afhankelijk van producten van derden en services. Als gevolg daarvan kan als deze methode is niet geïmplementeerd, kan levering keten inbreuk ondermijnen van de beveiliging van de service en van invloed zijn op de implementatie van andere beveiligingsprincipes.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het bieden van beveiligde levering keten documentatie voor een derde partij software en besturingssystemen die worden gebruikt in de Azure-abonnement hebt verkregen. De instructie van de implementatie klant dient de uitzondering als u wilt volgen processen die zijn geïdentificeerd door deze voorziening keten documentatie te houden. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | De Microsoft Cloud leveren keten (MCSC) bestaat uit zes unieke teams elke bijdragen aan Azure beveiligen tegen bedreigingen voor de toeleveringsketen.  <br />  <br /> -Inkoop <br /> -Klant bewerkingen <br /> -Quality implementatie <br /> -Leverancier Relationship Management <br /> -Spares <br />  <br /> Zie voor meer informatie over Microsoft MCSC groep de volledige beschrijving in de [blauwdruk Azure - Cloud-beveiligingsprincipes NCSC - klant verantwoordelijkheden Matrix](https://aka.ms/blueprintuk-gcrm). |


 ## <a name="ncsc-cloud-security-principle-9"></a>NCSC Cloud beveiligingsprincipe 9
### <a name="secure-user-management"></a>Gebruikersbeheer beveiligen
Uw provider moet de hulpprogramma's beschikbaar zijn voor u veilig beheer van uw gebruik van hun service maken. Beheerinterfaces en -procedures zijn een essentieel onderdeel van de beveiligingsbarrière zo wordt voorkomen dat onbevoegde toegang en de wijziging van uw resources, toepassingen en gegevens.

De aspecten rekening moet houden zijn:

- Verificatie van gebruikers beheerinterfaces en ondersteuningskanalen
- Scheiding en toegangsbeheer binnen beheerinterfaces



 ## <a name="ncsc-cloud-security-principle-91"></a>NCSC Cloud beveiligingsprincipe 9.1
### <a name="authentication-of-users-to-management-interfaces-and-within-support-channels"></a>Verificatie van gebruikers en binnen ondersteuningskanalen beheerinterfaces
Om toegang te houden van een beveiligde gebruikers nodig hebben om het uitvoeren van beheeractiviteiten, mogen worden geverifieerd-service wijzigingen rapport fouten of aanvraag aan de service.
Deze activiteiten kunnen worden uitgevoerd via een service-web-beheerportal of via andere kanalen, zoals telefoonnummer of e-mailbericht. Ze zijn waarschijnlijk ook functies zoals het inrichten van nieuwe elementen van de service, het beheren van gebruikersaccounts en gebruikersgegevens beheren.
Serviceproviders moeten ervoor zorgen dat alle aanvragen voor beheer die beveiliging gevolgen kunnen hebben via een veilige en geverifieerde kanalen worden uitgevoerd. Als gebruikers niet sterk zijn geverifieerd vervolgens iemand anders mogelijk is bevoorrechte acties uitvoeren, kan het de beveiliging van de service of de gegevens.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | Als beheerders toegang hebben tot de Microsoft Azure-Portal voor het beheren van Azure-resources voor hun organisatie, de gegevens die worden overgedragen tussen de portal en de beheerder apparaat wordt verzonden via een versleuteld Transport Layer Security (TLS)-kanaal met 2048-bits RSA / SHA256-versleutelingssleutels, zoals aanbevolen door CESG/NCSC.  <br /> <br /> Deze blauwdruk Azure maakt gebruik van Windows-verificatie, extern bureaublad en BitLocker. Deze onderdelen kunnen zijn afhankelijk van FIPS 140 gevalideerd cryptografiemodules worden geconfigureerd. <br /> <br /> Deze Azure blauwdruk dwingt autorisaties van logische toegang met behulp van op rollen gebaseerde toegangsbeheer afgedwongen door Azure Active Directory door gebruikers toewijzen aan rollen, Active Directory gebruikers toe te wijzen aan beveiligingsgroepen, en bepaalt Windows OS-niveau. Azure Active Directory-functies die zijn toegewezen aan gebruikers of groepen logische toegang tot bronnen in Azure op het niveau, groep of een abonnement beheren. Active Directory-beveiligingsgroepen beheren logische toegang tot bronnen van de OS-niveau en functies. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Klanten beheren hun Azure-resources via de Azure portal, dat toegang tot alle virtuele machines, databases, cloudservices biedt, en andere resources die zijn geconfigureerd voor het account van de klant. Webtoegang tot de Azure-portal wordt beveiligd door industriestandaard-Transport Layer Security (TLS) 1.2 verbindingen met 2048-bits RSA/SHA256 versleutelingssleutels, als aanbevolen door CESG/NCSC. Toegangsbeheer op basis van rollen zijn bedoeld om klanten voor beperkte toegang tot Azure management-resources voor specifieke gebruikers en groepen in staat. |


 ## <a name="ncsc-cloud-security-principle-92"></a>Beveiligings-principal voor NCSC Cloud 9.2
### <a name="separation-and-access-control-within-management-interfaces"></a>Scheiding en toegangsbeheer binnen beheerinterfaces
Veel cloudservices worden beheerd via webtoepassingen of -API's. Deze interfaces zijn een belangrijk onderdeel van de beveiliging van de service. Als gebruikers niet voldoende binnen beheerinterfaces worden gescheiden, is het mogelijk dat een gebruiker kunnen invloed hebben op de service of de gegevens van een andere wijzigen.
Uw beschermde accounts beheerdersrechten hebben waarschijnlijk toegang tot grote hoeveelheden gegevens. Beperkingen van de machtigingen van individuele gebruikers, zodat deze absoluut noodzakelijk kan helpen om te beperken van de schade als gevolg van kwaadwillende gebruikers, gestolen referenties of aangetaste apparaten.
Toegangsbeheer op basis van rollen biedt een mechanisme om dit te bereiken en is waarschijnlijk een bijzonder belangrijk mogelijkheid voor gebruikers die grotere implementaties beheren.
Blootstellen beheerinterfaces met minder toegankelijk netwerken (bijvoorbeeld community in plaats van openbare netwerken) maakt het moeilijker voor aanvallers om te bereiken en aanvallen, als ze eerst toegang te krijgen tot een van deze netwerken zou nodig. Richtlijnen voor het vaststellen van de risico's van het blootstellen van interfaces voor verschillende typen netwerken onder principe 11 gegeven.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | Deze Azure blauwdruk een Application Gateway wordt geïmplementeerd, de load balancer en netwerkbeveiligingsgroepen om te bepalen met bewerkingen op de externe grenzen en tussen de interne subnetten configureert. Functionaliteit van de gebruiker is gescheiden van de system management-functionaliteit afdwinging van logische toegang besturingselementen en systeemarchitectuur. Interfaces voor de system management-functionaliteit zijn gescheiden van gebruikersinterfaces. Alle beheer-connectiviteit is via een beveiligde bastion host (jumpbox) zich in een subnet management met netwerkbeveiligingsgroepen om te beperken van toegang tot productiebronnen naar gelang van toepassing. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Zoals wordt beschreven in betekent dit dat scheiding tussen scheiding van de gebruiker zich de kern is ingebouwd in Azure. Azure Active Directory (Azure AD of AAD) kunnen worden gebruikt voor elke gebruiker die zich verifieert bij de Azure-portal met toegang tot alleen de resources die ze hebben recht om te zien en beheren. Als gevolg hiervan zijn de accounts van verschillende klanten strikt gescheiden van elkaar wanneer beheerd via de algemene Azure portal. |


 ## <a name="ncsc-cloud-security-principle-10"></a>NCSC Cloud beveiligingsprincipe 10
### <a name="identity-and-authentication"></a>Identiteit en verificatie
Alle toegang tot service interfaces moet worden beperkt tot geverifieerde en gemachtigde personen.
Zwakke verificatie van deze interfaces kan onbevoegde toegang tot uw systemen, wat leidt tot diefstal of wijziging van uw gegevens, wijzigingen in uw service of een denial of service inschakelen.
Houd voor ogen dat moet verificatie plaatsvinden via beveiligde kanalen. E-mailbericht, HTTP of telefoon zijn kwetsbaar voor aanvallen worden onderschept en social hacking.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | Deze blauwdruk Azure Active Directory de veiligheidsmaatregelen voor accountbeheer. Toegang tot resources die zijn geïmplementeerd door deze blauwdruk Azure is beveiligd tegen replay-aanvallen door de ingebouwde Kerberos-functionaliteit van Azure Active Directory, Active Directory en de Windows-besturingssysteem. In Kerberos-verificatie bevat de verificator verzonden door de client bijkomende gegevens, zoals een lijst met versleutelde IP-, client tijdstempels en ticket levensduur. Als een pakket opnieuw en nu afgespeeld, wordt de tijdstempel gecontroleerd. Als de tijdstempel lager dan is of gelijk zijn aan een eerdere verificator, het pakket is afgewezen. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure biedt services voor het bijhouden van identiteit ook integreren met identiteitsopslag die mogelijk al in gebruik. Azure AD is een uitgebreide identiteits- en toegangsbeheer management-service voor de cloud waarmee beveiligde toegang tot gegevens op on-premises en cloudtoepassingen. Azure AD vereenvoudigt het beheer van gebruikers en groepen ook door een combinatie van core directoryservices, geavanceerde identiteit governance, beveiliging en beheer van toegang van toepassing. |


 ## <a name="ncsc-cloud-security-principle-11"></a>NCSC Cloud beveiligingsprincipe 11
### <a name="external-interface-protection"></a>Beveiliging voor de externe gebruikersinterface
Alle externe of minder vertrouwde interfaces van de service moeten worden geïdentificeerd en op de juiste wijze defended.
Als sommige van de interfaces die zijn blootgesteld persoonlijke (zoals beheerinterfaces) kan de gevolgen van inbreuk belangrijker worden.
Verbinding maken met cloudservices die blootstellen van uw enterprise-systemen om verschillende niveaus van risico's kunt u verschillende modellen.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert-resources in een architectuur met een afzonderlijke web subnet, database-subnet, Active Directory-subnet en management subnet. Subnetten logisch worden gescheiden door netwerkbeveiligingsgroepen toegepast op de afzonderlijke subnetten voor verkeer tussen subnetten alleen noodzakelijke systeem en de beheerfunctionaliteit te beperken (bijvoorbeeld externe verkeer geen toegang tot de database, beheer, (of Active Directory-subnetten).  <br /> <br /> Een Application Gateway wordt geïmplementeerd voor het beheren van externe verbindingen met een webtoepassing die door de klant is geïmplementeerd. Externe verbindingen voor toegang tot zijn beperkt tot een jumpbox (bastion host) die is geïmplementeerd in een subnet management met netwerkbeveiligingsregels toegepast op externe verbindingen beperken tot geautoriseerde IP-adressen. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Microsoft maakt gebruik van een methode wordt aangeroepen 'Red-team' ter verbetering van Azure beveiligingsmechanismen en -processen via reguliere binnendringen testen. Het Team rood is een groep fulltime medewerkers van Microsoft die is gericht op het uitvoeren van gericht en permanente aanvallen tegen Microsoft infrastructuur, platforms en toepassingen, maar niet end-klanten de toepassingen of gegevens. <br /> <br /> Voor meer informatie over Microsoft teams van rood, evenals een beschrijving van blauw teams inspanningen Zie de volledige beschrijving in de [blauwdruk Azure - Cloud-beveiligingsprincipes NCSC - klant verantwoordelijkheden Matrix](https://aka.ms/blueprintuk-gcrm).  |


 ## <a name="ncsc-cloud-security-principle-12"></a>NCSC Cloud beveiligingsprincipe 12
### <a name="secure-service-administration"></a>Beveiligde servicebeheer
Systemen die worden gebruikt voor het beheer van een cloudservice wordt maximaal uitgebreide toegang tot deze service. Hun inbreuk heeft aanzienlijke invloed, inclusief de mogelijkheid om beveiligingsmechanismen overslaan en stelen of manipuleren van grote hoeveelheden gegevens.
Het ontwerp, implementatie en beheer van systemen beheer moeten enterprise raadzaam om, volgen inachtneming echter hun hoge waarde voor aanvallen.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het garanderen van een beveiligde werkstation voor beheer van de Azure-abonnement en de klant geïmplementeerde resources (om op te nemen toepassingen, besturingssystemen, databases en software). De instructie van de implementatie klant dient te houden de mechanismen die wordt gebruikt om het risico van misbruik van de klant geïmplementeerde resources te beperken. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Medewerkers van Microsoft Azure-bewerkingen zijn vereist beveiligde beheerwerkstations (zagen; ook bekend als bevoorrechte toegang werkstations of poten). De aanpak ZAG is een uitbreiding van de goed vastgestelde aanbevolen werkwijze met afzonderlijke admin en gebruikersaccounts voor beheerders. Deze procedure maakt gebruik van een afzonderlijk toegewezen Administrator-account dat is volledig gescheiden van de standaard gebruikersaccount van de gebruiker. ZAG bouwt voort op deze account scheiding praktijk door te geven een betrouwbare werkstation voor die gevoelige accounts. |


 ## <a name="ncsc-cloud-security-principle-13"></a>NCSC Cloud beveiligingsprincipe 13
### <a name="audit-information-for-users"></a>Controle-informatie voor gebruikers
U moet zijn opgegeven met de audit records die nodig zijn voor het bewaken van toegang tot uw service en de gegevens in het bezit. Het type van het controle-informatie voor u beschikbaar heeft een directe invloed op de mogelijkheid om te detecteren en reageren op onjuiste of schadelijke activiteiten binnen een redelijk tijdsbestek.


**Verantwoordelijkheden:**`Shared`


|||
|---|---|
| **De klant** | Gebeurtenissen die worden gecontroleerd door deze Azure blauwdruk bevatten die worden gecontroleerd door Azure activiteitenlogboeken voor geïmplementeerde resources, OS-niveau logboeken en Active Directory-Logboeken. Deze gebeurtenis wordt geregistreerd bevatten informatie die voldoende zijn om te bepalen wanneer gebeurtenissen plaatsvinden, de bron van de gebeurtenis, het resultaat van de gebeurtenis en andere gedetailleerde informatie die ondersteuning biedt voor onderzoek beveiligingsincidenten. Klanten kunnen aanvullende gebeurtenissen moeten worden gecontroleerd om te voldoen aan de behoeften van de missie selecteren. Alle Azure-resources hebben controlelogboeken beschikbaar in de Azure-Portal. |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Azure Log Analytics records van de gebeurtenissen die plaatsvinden binnen een organisatie systemen en netwerken als deze zich voordoen, voordat iemand anders met hen knoeien kan verzamelt en kunt verschillende soorten analyse door de gegevens op meerdere computers. Azure kan klanten om beveiliging gebeurtenis genereren en verzamelen van Azure IaaS en PaaS-rollen naar de centrale opslag in hun abonnementen. Deze verzamelde gebeurtenissen kunnen worden geëxporteerd naar de lokale security information en event management (SIEM)-systemen voor continue bewaking. Nadat de gegevens worden overgedragen naar de opslag, zijn er veel opties om de diagnostische gegevens weer te geven. <br /> <br /> Azure ingebouwde diagnostics kan helpen bij foutopsporing. Voor toepassingen die zijn geïmplementeerd in Azure, worden een aantal beveiligingsgebeurtenissen besturingssysteem standaard ingeschakeld. Klanten kunnen toevoegen, verwijderen of wijzigen van gebeurtenissen die moeten worden gecontroleerd door het besturingssysteem controlebeleid aanpassen. <br /> <br /> Het is heel eenvoudig op een hoog niveau en eenvoudige om te beginnen met het verzamelen van logboeken met behulp van Windows Event Forwarding (WEF) of de meer geavanceerde Azure Diagnostics wanneer Windows gebaseerde virtuele machines zijn geïmplementeerd met behulp van IaaS in Azure. Azure Diagnostics kan bovendien worden geconfigureerd voor het verzamelen van Logboeken en gebeurtenissen van PaaS-rolexemplaren. Wanneer u op basis van IaaS VM's gebruikt, wordt een klant gewoon configureert en kan de gewenste beveiligingsgebeurtenissen op dezelfde wijze als ze Windows-Servers die de audits aanmelden met hun lokale datacentrum inschakelen. Voor webtoepassingen is het ook mogelijk om in te schakelen IIS-registratie als de primaire toepassing en de implementatie in Azure. Klanten kunnen altijd beveiligingsgegevens opslaan in opslagaccounts in ondersteunde geografische locaties van hun keuze om te voldoen aan vereisten voor onafhankelijkheid van gegevens. |


 ## <a name="ncsc-cloud-security-principle-14"></a>NCSC Cloud beveiligingsprincipe 14
### <a name="secure-use-of-the-service"></a>Veilige gebruik van de Service
De beveiliging van cloudservices en gegevens die zijn opgeslagen in deze wordt ondermijnd als u de service slecht gebruiken. Als gevolg daarvan, hebt u bepaalde verantwoordelijkheden wanneer u de service om uw gegevens moet correct worden beveiligd.
De omvang van uw verantwoordelijkheid varieert, afhankelijk van het implementatiemodel van de cloudservice en het scenario waarin u van plan bent om de service te gebruiken. Specifieke functies van afzonderlijke services kunnen ook gevolgen hebben. Hoe uw persoonlijke sleutel wordt beveiligd met een content delivery network of hoe betaling cloudprovider frauduleuze transacties detecteert bijvoorbeeld zijn belangrijk beveiligingsoverwegingen naast de algemene aandachtspunten besproken gedekt door de cloud beveiligings-principals.  
Met IaaS en PaaS-aanbiedingen bent u verantwoordelijk voor belangrijke aspecten van de beveiliging van uw gegevens en werkbelastingen. Bijvoorbeeld, als u schaft u een IaaS compute exemplaar, kunt u normaal gesproken die verantwoordelijk is voor een modern besturingssysteem installeren, configureren van dat besturingssysteem veilig, veilig implementeren van toepassingen en ook onderhouden dat exemplaar via toepassen patches of het uitvoeren van onderhoud vereist.


**Verantwoordelijkheden:**`Customer`

> [!NOTE]
> De klant kan het Azure Beveiligingscentrum gebruiken om te detecteren, voorkomen van en reageren op bedreigingen dankzij een verhoogde zichtbaarheid en controle over de beveiliging van de Azure-resources. Het Azure Beveiligingscentrum biedt geïntegreerde beveiligingsbewaking en beleidsbeheer voor Azure-abonnementen, helpt bedreigingen die anders onopgemerkt, en werkt met een uitgebreid ecosysteem van beveiligingsoplossingen te detecteren.

|||
|---|---|
| **De klant** | De Azure Resource Manager-sjablonen en de bijbehorende bronnen waaruit deze blauwdruk Azure Volg een defensie-in-depth-benadering voor de beveiliging. Om te voldoen aan deze principe verdere configuratie vereist door de klant voor gebruik in productie (bijv, database-software voor beheer, web toepassingsimplementatie). |
| **Provider&nbsp;(Microsoft&nbsp;Azure)** | Niet van toepassing |

## <a name="disclaimer"></a>Vrijwaring

 - Dit document is alleen ter informatie. MICROSOFT GEEFT GEEN GARANTIES, SNELLE, IMPLICIETE OF WETTELIJKE GARANTIE VOOR DE INFORMATIE IN DIT DOCUMENT. Dit document wordt geleverd ' as-is. " Informatie en inzichten die in dit document, inclusief URL's en andere websiteverwijzingen, kunnen zonder kennisgeving worden gewijzigd. Dit document lezen klanten draagt het risico voor het gebruik ervan.
 - Dit document biedt geen enkel wettelijk recht op enig intellectueel eigendom van alle Microsoft-product of oplossingen klanten.
 - Klanten kunnen kopiëren en gebruiken van dit document voor interne referentiedoeleinden.
 - Bepaalde aanbevelingen in dit document kunnen leiden tot hogere-, netwerk- of compute-Resourcegebruik in Azure en een klant Azure licentie of abonnement kosten kunnen verhogen.
 - Deze architectuur is bedoeld om te fungeren als basis voor klanten om aan te passen aan hun specifieke vereisten en mag niet worden gebruikt als-is in een productieomgeving.
 - Dit document is ontwikkeld als een verwijzing en mag niet worden gebruikt voor het definiëren van alle middelen waarmee een klant kan voldoen aan specifieke nalevingsvereisten en -voorschriften. Klanten moeten juridische ondersteuning van hun organisatie met goedgekeurde klant implementaties zoeken.