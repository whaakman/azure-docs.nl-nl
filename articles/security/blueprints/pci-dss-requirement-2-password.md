---
title: Azure betaling verwerking blauwdruk - wachtwoordvereisten
description: PCI-DSS vereiste 2
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 0df24870-6156-4415-a608-dd385b6ae807
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 4ae9fc7d5b53d33f9feb98c450970e0560afa2af
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="password-requirements-for-pci-dss-compliant-environments"></a>Vereisten voor wachtwoorden voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-2"></a>PCI-DSS vereiste 2

**Gebruik geen leverancier standaardwaarden voor Systeemwachtwoorden en andere beveiligingsparameters**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Kwaadwillende personen (externe en interne aan een entiteit) vaak gebruikt leverancier standaard wachtwoorden en andere standaardinstellingen van de leverancier van systemen. Deze wachtwoorden en instellingen zijn bekende door de community's van hackers en eenvoudig via openbare gegevens worden bepaald.

## <a name="pci-dss-requirement-21"></a>PCI-DSS vereiste 2.1
 
**2.1** altijd leverancier standaardinstellingen wijzigen en verwijderen of uitschakelen van onnodige standaardaccounts **voordat** installatie van een systeem in het netwerk.
Dit geldt voor alle standaard wachtwoorden, met inbegrip van doch niet beperkt tot die worden gebruikt door de besturingssystemen, software waarmee beveiligingsservices, toepassings- en -accounts, verkooppunten (POS)-aansluitingen, Simple Network Management Protocol (SNMP)-community tekenreeksen, enz.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Vereisten voor Microsoft Azure Active Directory wachtwoordbeleid worden afgedwongen voor de nieuwe wachtwoorden die door klanten in de portal AADUX geleverd. Selfservice voor wachtwoordherstel klant geïnitieerde wijzigingen vereist validatie van eerdere wachtwoord. Beheerder opnieuw instellen van wachtwoorden moeten worden gewijzigd bij volgende aanmelding. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore moeten gebruikers set sterke wachtwoorden gebruiken voor alle gebruikers. Er zijn geen voorbeeld of Gast-accounts zijn ingeschakeld in de demo.<br /><br />SNMP- en draadloze netwerken worden niet geïmplementeerd in de oplossing.|



### <a name="pci-dss-requirement-211"></a>PCI-DSS vereiste 2.1.1

**2.1.1** draadloze omgevingen is verbonden met de kaarthouder gegevensomgeving of uitzenden kaarthouder van gegevens, alle draadloze leverancier standaardinstellingen bij de installatie, inclusief maar niet beperkt tot standaard draadloze versleutelingssleutels, wachtwoorden, wijzigen en SNMP-community-tekenreeksen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | SNMP- en draadloze netwerken worden niet geïmplementeerd in de oplossing.|



## <a name="pci-dss-requirement-22"></a>PCI-DSS vereiste 2.2

**2.2** ontwikkelen configuratiestandaarden voor alle onderdelen van het systeem. Zorgen dat deze normen adres van alle bekende beveiligingsproblemen en consistent zijn met een algemeen geaccepteerde beperken van het systeem standaarden.
Bronnen van een algemeen geaccepteerde beperken van het systeem standaarden mogelijk, maar zijn niet beperkt tot:
- Center voor de beveiliging van Internet (configuratie-items)
- International Organization for Standardization (ISO)
- SysAdmin Audit Network (SAN's) beveiliging Institute
- National Institute of Standards Technology (NIST)

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Het team OSSC technische beveiligingsservices ontwikkelt voor Microsoft Azure configuration beveiligingsstandaarden voor systemen in de Microsoft Azure-omgeving die consistent zijn met algemeen geaccepteerde standaarden beperken. Deze configuraties worden beschreven in het systeem basislijnen en relevante configuratiewijzigingen worden gecommuniceerd aan de betrokken teams (bijv, IPAK team). Procedures worden geïmplementeerd om te controleren op naleving op basis van de configuratie beveiligingsstandaarden. De security configuration standards voor systemen in de Microsoft Azure-omgeving consistent zijn met een algemeen geaccepteerde hardening standaarden en ten minste jaarlijks worden gecontroleerd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt beveiliging van alle services binnen het bereik van de kaarthouder data environment (CDE). <br /><br />De Contoso Webstore ook implementeert de [Azure Security Center](https://azure.microsoft.com/services/security-center/), waarmee u een centrale weergave van de beveiligingsstatus van alle Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u alle bronnen die aandacht vereisen snel kan identificeren.<br /><br />De Contoso Webstore maakt gebruik van Operations Management Suite voor logboekregistratie van alle wijzigingen in het systeem. [Operations Management Suite (OMS)](/azure/operations-management-suite/) biedt uitgebreide logboekregistratie van wijzigingen. Wijzigingen worden beoordeeld en gecontroleerd op juistheid. Zie voor meer specifieke instructies [richtlijnen PCI - Operations Management Suite](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-221"></a>PCI-DSS eis 2.2.1

**2.2.1** slechts één primaire functie per server om te voorkomen dat functies waarvoor verschillende beveiligingsniveaus vanaf naast elkaar bestaande op dezelfde server implementeren. (Bijvoorbeeld webservers, databaseservers en DNS moet worden geïmplementeerd op afzonderlijke servers.) 

> [!NOTE]
> Wanneer virtualisatietechnologieën in gebruik zijn, implementeert u slechts één primaire functie per virtuele systeemonderdeel.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore services worden geïmplementeerd als PaaS-services. Alle services zijn geïsoleerd, en gebruik van netwerksegmentering gesegmenteerde.<br /><br />De Contoso Webstore gebruikt ook een [as-omgeving (App Service omgeving)](/azure/app-service-web/app-service-app-service-environment-intro) af te dwingen belangrijke procedures. Zie voor meer informatie [richtlijnen PCI - App Service-omgeving](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-222"></a>PCI-DSS eis 2.2.2

**2.2.2** inschakelen alleen de benodigde services, protocollen, daemons, enz., zoals is vereist voor de functie van het systeem.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Software- en hardwareconfiguraties van Microsoft Azure worden minstens per kwartaal herzien om te identificeren en oplossen van eventuele overbodige functies, -poorten, protocollen en services. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore services worden geïmplementeerd als PaaS-services. Alle services zijn geïsoleerd, en gebruik van netwerksegmentering gesegmenteerde.<br /><br />De Contoso Webstore gebruikt ook een [as-omgeving (App Service omgeving)](/azure/app-service-web/app-service-app-service-environment-intro) af te dwingen belangrijke procedures. Zie voor meer informatie [richtlijnen PCI - App Service-omgeving](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-223"></a>PCI-DSS eis 2.2.3

**2.2.3** implementeren aanvullende beveiligingsfuncties voor alle benodigde services, protocollen of daemons die als onveilig worden beschouwd. 

> [!NOTE]
> Wanneer vroege SSL/TLS wordt gebruikt, kan de vereisten in de bijlage A2 moeten worden uitgevoerd.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore services worden geïmplementeerd als PaaS-services. Alle services zijn geïsoleerd, en gebruik van netwerksegmentering gesegmenteerde. De implementatie biedt ook beveiliging van alle services in het bereik van het CDE. <br /><br />De Contoso Webstore ook implementeert de [Azure Security Center](https://azure.microsoft.com/services/security-center/), waarmee u een centrale weergave van de beveiligingsstatus van alle Azure-resources. In een oogopslag kunt u controleren dat de juiste beveiligingscontroles zijn geïnstalleerd en correct geconfigureerd en u alle bronnen die aandacht vereisen snel kan identificeren.<br /><br />De Contoso Webstore gebruikt ook een [as-omgeving (App Service omgeving)](/azure/app-service-web/app-service-app-service-environment-intro) af te dwingen belangrijke procedures. Zie voor meer informatie [richtlijnen PCI - App Service-omgeving](payment-processing-blueprint.md#app-service-environment).|



### <a name="pci-dss-requirement-224"></a>PCI-DSS vereiste 2.2.4

**2.2.4** beveiligingsparameters voor systeem om te voorkomen dat misbruik configureren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure zorgt ervoor dat alleen bevoegd personeel, kunnen Azure-platform beveiligingsmaatregelen configureren met behulp van toegangsbeheer met meerdere factoren en gedocumenteerde zakelijke behoeften. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore AAD gebruikt en AD RBAC voor het beheren van beveiligingsparameters zijn correct is geïmplementeerd. Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).|



### <a name="pci-dss-requirement-225"></a>PCI-DSS vereiste 2.2.5

**2.2.5** alle onnodige functionaliteit, zoals scripts, stuurprogramma's, functies, subsystemen, bestandssystemen en onnodige webservers te verwijderen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore bevat documentatie over hoe grenzen tot stand worden gebracht. Risicomodel van Contoso en gegevensstroom-diagram ziet u alle services die worden gebruikt en besturingselementen die zijn ingeschakeld.|



## <a name="pci-dss-requirement-23"></a>PCI-DSS vereiste 2.3

**2.3** versleutelen van alle niet-administratieve toegang tot de console met sterke cryptografie. 

> [!NOTE]
> Wanneer vroege SSL/TLS wordt gebruikt, kan de vereisten in de bijlage A2 moeten worden uitgevoerd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure zorgt ervoor dat het gebruik van sterke cryptografie wordt afgedwongen bij het openen van de hypervisor-infrastructuur. Microsoft Azure ook zorgt ervoor dat klanten die gebruikmaken van Microsoft Azure Management Portal toegang kunnen krijgen tot hun service/IaaS-consoles met sterke cryptografie. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore ziet u hoe sterke wachtwoorden kunnen worden geïmplementeerd in een oplossing. Bovendien kunnen alle tests worden uitgevoerd om te controleren dat of versleuteling in de oplossing is geïmplementeerd.<br /><br />De Contoso Webstore gebruikt ook een [as-omgeving (App Service omgeving)](/azure/app-service-web/app-service-app-service-environment-intro) af te dwingen belangrijke procedures. Zie voor meer informatie [richtlijnen PCI - App Service-omgeving](payment-processing-blueprint.md#app-service-environment).|



## <a name="pci-dss-requirement-24"></a>PCI-DSS vereiste 2.4

**2.4** een inventaris van onderdelen van het systeem die binnen het bereik van PCI DSS zijn onderhouden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De inventarisatie Contoso Webstore demo PaaS-oplossing kan worden gecontroleerd in de documentatie van de opgegeven. Zie voor meer informatie [richtlijnen PCI - vooraf geïnstalleerde OMS oplossingen](payment-processing-blueprint.md#oms-solutions).|



## <a name="pci-dss-requirement-25"></a>PCI-DSS vereiste 2,5

**2.5** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het beheren van de standaardinstellingen van de leverancier en andere beveiligingsparameters gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt documentatie inzicht in de beveiligingsparameters biedt en documenten service-elementen. |



## <a name="pci-dss-requirement-26"></a>PCI-DSS vereiste 2.6

**2.6** gedeelde hostingproviders moeten elke entiteit gehoste omgeving en kaarthouder gegevens beveiligen. Deze providers moeten voldoen aan specifieke vereisten zoals beschreven in *bijlage A: extra PCI DSS-vereisten voor Shared Hosting Providers.*

**Verantwoordelijkheden:&nbsp;&nbsp;`Not Applicable`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. Microsoft Azure is niet een gedeelde hostingprovider. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing. Microsoft Azure is niet een gedeelde hostingprovider.|




