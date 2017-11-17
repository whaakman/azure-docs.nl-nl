---
title: Azure betaling verwerking blauwdruk - vereisten voor fysieke toegang
description: PCI-DSS vereiste 9
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 91595a69-e9ce-4f9c-8388-10224165d9c0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 89f7b20a130e988bfe4964d50ae97de788ca4623
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="physical-access-requirements-for-pci-dss-compliant-environments"></a>Fysieke toegangsvereisten voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-9"></a>PCI-DSS vereiste 9

**Beperk de fysieke toegang tot gegevens van de kaarthouder**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Alle fysieke toegang tot de gegevens of systemen die kaarthouder house gegevens biedt de mogelijkheid voor personen om systemen of hardcopies te verwijderen voor toegang tot apparaten of gegevens zijn en moet op de juiste wijze worden beperkt. Voor de doeleinden van vereiste 9, wordt "beveiligingspersoneel personeel" verwijst naar fulltime en fulltime werknemers, tijdelijke werknemers, contractanten en consultants die fysiek aanwezig op de locatie van de entiteit zijn. Een 'bezoeker' verwijst naar een leverancier, Gast personeel dat op de locatie, werknemers van de service of iedereen moet invoeren van de faciliteit gedurende een korte periode, meestal niet meer dan een dag. "Medium" verwijst naar alle papier en elektronische media met gegevens van de kaarthouder.

## <a name="pci-dss-requirement-91"></a>PCI-DSS vereiste 9.1

**9.1** juiste faciliteit vermelding besturingselementen voor limiet en monitor fysieke toegang tot systemen in de kaarthouder gegevensomgeving gebruiken.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor het implementeren en bewaken van fysieke beveiliging voor datacenters afdwingen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-911"></a>PCI-DSS vereiste 9.1.1

**9.1.1** gebruik videocamera of access control mechanismen (of beide) voor het bewaken van afzonderlijke fysieke toegang tot gevoelige gebieden. Verzamelde gegevens bekijken en correleren met andere vermeldingen. Opslaan voor ten minste drie maanden, tenzij anders wordt beperkt door de wet.

> [!NOTE]
> "Gevoelige gebieden" verwijst naar een datacenter, serverruimte of een gebied met systemen die zijn opgeslagen, verwerkt of kaarthouder gegevens verzenden. Dit openbare gebieden waarin alleen verkooppunten aansluitingen aanwezig, zoals de kassamedewerker gebieden in de detailhandel zijn worden uitgesloten.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor het implementeren en bewaking CCTV en mechanismen voor datacenters biometrische toegang afdwingen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-912"></a>PCI-DSS vereiste 9.1.2

**9.1.2** fysieke en/of logische controles toegang te beperken tot aansluitingen openbaar toegankelijk netwerk te implementeren. 

Bijvoorbeeld: netwerk soorten netwerkconnectoren zich bevindt in openbare gebieden en gebieden die toegankelijk zijn voor bezoekers kunnen worden uitgeschakeld en alleen ingeschakeld wanneer toegang tot het netwerk uitdrukkelijk is toegestaan. U kunt ook kunnen processen worden geïmplementeerd om ervoor te zorgen dat bezoekers zijn escorted op alle tijden op gebieden met actieve netwerk-aansluitingen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Er zijn geen openbaar toegankelijk netwerk aansluitingen binnen de Microsoft Azure-platform. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-913"></a>PCI-DSS vereiste 9.1.3

**9.1.3** fysieke toegang tot draadloze toegangspunten, gateways, mobiele apparaten, netwerken/communicatie hardware en telecommunicatie regels beperken.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Fysieke toegang tot Microsoft Azure netwerkhardware strak wordt beheerd door een lijst met toegang tot meerdere formulieren van de verificatie, fysieke belemmering voor het item en de vereiste voor bedrijven moeten worden goedgekeurd voor toegang tot apparatuur. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



## <a name="pci-dss-requirement-92"></a>PCI-DSS vereiste 9.2

**9.2** procedures te ontwikkelen om gemakkelijk onderscheiden beveiligingspersoneel personeel en bezoekers, moeten worden opgenomen:
- Identificatie van lokaal personeel en bezoekers (bijvoorbeeld toewijzen badges)
- Wijzigingen in de toegangsvereisten
- Intrekken of wordt beëindigd op de locatie personeel en verlopen bezoeker identificatie (zoals ID badges).

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor het implementeren, afdwingen en beveiligings- en werknemer of contractor identificatie van fysieke toegang controleren als bezoekende datacenters. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



## <a name="pci-dss-requirement-93"></a>PCI-DSS vereiste 9.3

**9.3** beheer de fysieke toegang voor beveiligingspersoneel personeel tot gevoelige gebieden als volgt:
- Toegang moet worden geautoriseerd en op basis van afzonderlijke functie.
- De toegang wordt onmiddellijk ingetrokken bij beëindiging en alle fysieke toegangsmethoden, zoals sleutels, badges, enz., worden geretourneerd of uitgeschakeld.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | -Verificatie ondersteunt voor Microsoft-datacenters wordt beheerd met behulp van een lijst met geautoriseerde toegang goedgekeurd door het team van het datacenter op basis van het principe van minimale bevoegdheden. De toegangsbeheerlijst is gecontroleerd, geverifieerd en elk kwartaal bijgewerkt.<br /><br />Microsoft Azure-datacenters gebruikmaken van fysieke toegangsapparaten zoals perimeter-poorten, elektronische toegang badge-lezers biometrische lezers, man-traps/portals en antivirusprogramma pass back-apparaten. Badge-toegangsapparaten worden continu bewaakt. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



## <a name="pci-dss-requirement-94"></a>PCI-DSS vereiste 9.4

**9.4** procedures voor het identificeren en autoriseren van bezoekers implementeren. Procedures moeten het volgende te omvatten.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor het afdwingen vooraf goedgekeurde leveringen worden ontvangen op een veilige laden bay die fysiek gescheiden is van de verwerking van gegevens faciliteiten en worden bewaakt door bevoegd personeel. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-941"></a>PCI-DSS vereiste 9.4.1

**9.4.1** bezoekers worden geautoriseerd voor het invoeren en escorted op altijd binnen gebieden waar de gegevens van de kaarthouder is verwerkt of onderhouden.


**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor het afdwingen vooraf goedgekeurde leveringen worden ontvangen op een veilige laden bay die fysiek gescheiden is van de verwerking van gegevens faciliteiten en worden bewaakt door bevoegd personeel. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-942"></a>PCI-DSS vereiste 9.4.2

**9.4.2** bezoekers zijn geïdentificeerd en gegeven een badge of andere identificatie die is verlopen en die zichtbaar wordt onderscheiden van de bezoekers op de locatie-personeel.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft data center toegang moet vooraf goedgekeurde en geautoriseerde bezoekers zijn vereist voor inchecken met fysieke beveiliging op het moment van aankomst en geef een geldig bewijs van ID voordat vermelding. Badges duidelijk werknemers. Ontvangen tijdelijke badges die moeten worden gedaan bij het verlaten van de faciliteit aannemers en bezoekers. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-943"></a>PCI-DSS vereiste 9.4.3

**9.4.3** bezoekers wordt gevraagd om u te afstand badge of identificatie vóór het verlaten van de faciliteit of op de datum van verlopen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Bezoekers zijn vereist voor de afstand badges bij het verlaten van een Microsoft-faciliteit. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-944"></a>PCI-DSS vereiste 9.4.4

**9.4.4** een logboek bezoeker wordt gebruikt voor het onderhouden van een fysieke audittrail van bezoeker activiteit naar de opslagruimte, evenals computer ruimten en datacenters waar kaarthouder gegevens worden opgeslagen of verzonden.
Documenteer het Bezoekerscentrum naam, de onderneming weergegeven en het fysieke toegangsmachtiging voor het logboek op de locatie-personeel.
Dit logboek voor een minimum van drie maanden bewaren tenzij anders wordt beperkt door de wet.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure is verantwoordelijk voor het onderhouden van een logboek bezoeker als een fysieke audittrail van bezoeker activiteit naar de opslagruimte, evenals computer ruimten en datacenters waar kaarthouder gegevens worden opgeslagen of verzonden. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



## <a name="pci-dss-requirement-95"></a>PCI-DSS vereiste 9.5

**9.5** alle media een fysiek beveiligde locatie.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-951"></a>PCI-DSS vereiste 9.5.1

**9.5.1** media back-ups opslaan in een veilige locatie, bij voorkeur een externe faciliteit, zoals een alternatieve of back-up-site, of een commerciële opslagfaciliteit. Bekijk de beveiliging van de locatie ten minste eenmaal per jaar.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-96"></a>PCI-DSS vereiste 9,6

**9,6** strikte controle over de interne of externe distributie van elk soort media, waaronder de volgende.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-961"></a>PCI-DSS vereiste 9.6.1

**9.6.1** media classificeren, zodat de vertrouwelijkheid van de gegevens kan worden vastgesteld.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-962"></a>PCI-DSS vereiste 9.6.2

**9.6.2** verzend de media door beveiligde courier of andere leveringsmethode die correct kan worden gevolgd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-963"></a>PCI-DSS vereiste 9.6.3

**9.6.3** management Zorg ervoor dat alle media die wordt verplaatst van een beveiligde gebied (inclusief wanneer media worden gedistribueerd naar personen) goedkeurt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-97"></a>PCI-DSS vereiste 9.7

**9.7** strikte controle over de opslag en toegankelijkheid van media.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-971"></a>PCI-DSS vereiste 9.7.1

**9.7.1** goed logboeken van de inventaris van alle media onderhouden en uitvoering van een media voorraden ten minste eenmaal per jaar.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



## <a name="pci-dss-requirement-98"></a>PCI-DSS vereiste 9,8

**9,8** media vernietigen wanneer deze niet langer nodig is voor bedrijven of wettelijke redenen als volgt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-981"></a>PCI-DSS vereiste 9.8.1

**9.8.1** versnipperen, verbranding of gedrukte materialen papierpulp zodat kaarthouder gegevens kan niet worden gerepareerd. Beveilig de storage-containers gebruikt voor materialen die moeten worden vernietigd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore slaat alle gegevens in Azure SQL Database. Een PaaS SQL Database-exemplaar wordt gebruikt voor de database veiligheidsmaatregelen presenteren. Zie voor meer informatie [richtlijnen PCI - Azure SQL Database](payment-processing-blueprint.md#azure-sql-database).|



### <a name="pci-dss-requirement-982"></a>PCI-DSS vereiste 9.8.2

**9.8.2** renderen gegevens van de kaarthouder op elektronische media onherstelbare zodat kaarthouder gegevens kan niet worden gerepareerd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Gegevens vernietiging technieken variëren afhankelijk van het type gegevensobject worden verbroken, ongeacht of deze abonnementen, opslag, virtuele machines of databases. In de Microsoft Azure-multitenant-omgeving, zorgvuldige aandacht wordt genomen om ervoor te zorgen dat één klantgegevens mag niet beide 'gegevenslekken' in een andere klantgegevens, of wanneer een klant Hiermee verwijdert u de gegevens van andere klanten geen (inclusief, in de meeste gevallen de klant wie de gegevens eenmaal eigenaar) kan toegang krijgen tot die gegevens verwijderd.<br /><br />Microsoft Azure volgt NIST 800 88 richtlijnen op opschoning van de Media, waarmee de principal belang om ervoor te zorgen dat de gegevens niet per ongeluk is vrijgegeven. Deze richtlijnen omvatten elektronische en fysieke opschoning. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore kan volledig door het verwijderen van de resourcegroep die wordt gebruikt tijdens de implementatie worden verwijderd.|



## <a name="pci-dss-requirement-99"></a>PCI-DSS vereiste 9,9

**9,9** beveiligen van apparaten die betaling kaartgegevens via directe fysieke interactie met de kaart tegen knoeien en vervanging vastleggen.

> [!NOTE]
> Deze vereisten van toepassing op apparaten van kaart lezen in kaart aanwezig-transacties (dat wil zeggen kaart doorhalen of dip) verkooppunt gebruikt. Deze vereiste is niet bedoeld om te passen op handmatige invoer van sleutel onderdelen, zoals Computertoetsenborden en POS toetsenblokken. 

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van OMS voor logboekregistratie van alle wijzigingen in het systeem.<br /><br />[Operations Management Suite (OMS)](/azure/operations-management-suite/) biedt uitgebreide logboekregistratie van wijzigingen. Wijzigingen worden beoordeeld en gecontroleerd op juistheid. Zie voor meer specifieke instructies [richtlijnen PCI - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-991"></a>PCI-DSS vereiste 9.9.1

**9.9.1** onderhouden van een bijgewerkte lijst van apparaten. De lijst moet het volgende omvatten:
- Merk, model van apparaat
- Locatie van het apparaat (bijvoorbeeld, het adres van de site of de faciliteit waar het apparaat zich bevindt)
- Serienummer van apparaat of een andere methode van de unieke identificatie

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een referentiearchitectuur en een lijst met alle services die worden gebruikt in de documentatie van de implementatie.|



### <a name="pci-dss-requirement-992"></a>PCI-DSS vereiste 9.9.2

**9.9.2** periodiek controleren verwerkingsinformatie om te detecteren geknoeid (bijvoorbeeld de toevoeging van kaart skimmers op apparaten) of vervangen apparaat (bijvoorbeeld door het controleren van het serienummer wordt geverifieerd of een ander apparaat kenmerken om te controleren of deze is niet is gewisseld met een frauduleuze apparaat).

> [!NOTE]
> Voorbeelden van tekens dat een apparaat mogelijk is gemanipuleerd of vervangen door zijn onverwachte bijlagen of kabels aangesloten op het apparaat, ontbreekt of is gewijzigd beveiligingslabels, verbroken of anders gekleurde hoofdlettergebruik of wijzigingen in het serienummer of andere externe markeringen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



### <a name="pci-dss-requirement-993"></a>PCI-DSS vereiste 9.9.3

**9.9.3** training bieden voor personeel om te worden op de hoogte van de poging om geknoei met of vervangen van apparaten. Training moet het volgende omvatten:
- Controleer of de identiteit van een derde partij personen claimen moet herstellen of onderhoud personeel, alvorens deze toegang als u wilt wijzigen of het oplossen van apparaten te verlenen.
- Geen installeren, vervangen, of apparaten zonder verificatie worden geretourneerd.
- Let verdacht gedrag om apparaten (bijvoorbeeld pogingen door onbekende personen ontkoppelen of open apparaten).
- Rapport verdacht gedrag en aanwijzingen van het apparaat is geknoeid of vervanging aan geschikte personen (bijvoorbeeld om een officer manager of beveiliging).

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|



## <a name="pci-dss-requirement-910"></a>PCI-DSS vereiste 9.10

**9.10** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het beperken van fysieke toegang tot gegevens van de kaarthouder gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing.|




