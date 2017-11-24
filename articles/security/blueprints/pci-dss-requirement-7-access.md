---
title: Azure betaling verwerking blauwdruk - vereisten voor toegang
description: PCI-DSS vereiste 7
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: ac3afee9-0471-465d-a115-67488a1635a6
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 5a3c9eac552fb96309cfa791a2e72a7102662e60
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="access-requirements-for-pci-dss-compliant-environments"></a>Vereisten voor toegang voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-7"></a>PCI-DSS vereiste 7

**Toegang tot gegevens van de kaarthouder beperken door bedrijven dat nodig is**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Om ervoor te zorgen kritieke gegevens alleen toegankelijk zijn voor bevoegd personeel, moeten systemen en processen aanwezig zijn op basis van dat nodig is en volgens functiegroepen toegang te beperken.

'Moet weten' is wanneer rechten zijn verleend aan zo min mogelijk gegevens en de bevoegdheden die nodig zijn om uit te voeren van een taak.

## <a name="pci-dss-requirement-71"></a>PCI-DSS vereiste 7.1

**7.1** beperken van toegang tot de onderdelen van het systeem en gegevens van de kaarthouder om alleen de personen met een functie die toegang vereist.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure worden afgedwongen bestaande ISMS beleid met betrekking tot Azure medewerkers toegang tot Azure-systeemonderdelen, verificatie van de effectiviteit van besturingselement voor toegang, bieden Just-In-Time beheerderstoegang toegangsmachtigingen wanneer niet langer nodig hebt en ervoor zorgen dat personeel intrekken een bedrijf toegang tot de Azure-platform-omgeving hebt nodig. Azure toegang tot klantomgevingen is zeer beperkt en is alleen toegestaan bij de goedkeuring van de klant.<br /><br />Procedures zijn vastgesteld fysieke toegang tot het datacenter aan bevoegde werknemers, leveranciers, aannemers en bezoekers te beperken. Beveiligingsverificatie en controle zijn vereist voor personeel tijdelijk toegang krijgen tot de interior data center-faciliteit vereisen. Fysieke toegangslogboeken worden elk kwartaal beoordeeld door Azure teams. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het beperken van toegang tot de onderdelen van het systeem en gegevens van de kaarthouder om alleen de personen met een functie die toegang vereist. Dit omvat beperken en beperken van toegang tot de Azure-beheerportal en geven accounts of rollen met machtiging voor het maken, wijzigen of verwijderen PaaS-services.|



### <a name="pci-dss-requirement-711"></a>PCI-DSS vereiste 7.1.1

**7.1.1** definiëren access moet voor elke rol, met inbegrip van:
- Onderdelen en gegevens systeembronnen waartoe toegang voor de functie van elke rol moet
- Niveau van bevoegdheden die nodig zijn (bijvoorbeeld gebruiker, administrator, enzovoort) voor toegang tot resources

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het definiëren en documenteren van een goedkeuringsproces gebruikers-ID, minimaal benodigde bevoegdheden definiëren, beperken van toegang tot gegevens van de kaarthouder, met behulp van de unieke id's, biedt scheiding van functies en gebruikerstoegang wanneer dit niet langer nodig intrekken.|



### <a name="pci-dss-requirement-712"></a>PCI-DSS vereiste 7.1.2

**7.1.2** toegang tot bevoegde gebruikers-id's aan de minimaal benodigde bevoegdheden nodig zijn om uit te voeren functiegroepen beperken.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure heeft goedgekeurd van toepassing bedrijven en organisaties beveiligingsbeleid, met inbegrip van een beleid voor beveiliging. De beleidsregels zijn goedgekeurd, gepubliceerd en wordt gecommuniceerd aan Windows Azure. Het beveiligingsbeleid van Microsoft Azure informatie vereist dat toegang tot Microsoft Azure activa verleend op basis van een zakelijke rechtvaardiging met de activumeigenaar toestemming en op basis van 'moet ter informatie' en 'minimale bevoegdheden' principes. Vereisten voor toegang levenscyclusbeheer van toegang tot het inrichten van de autorisatie, verificatie verwijderen van toegangsrechten, waaronder ook een oplossing voor het beleid en periodieke toegang beoordeelt. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Gebruikersrollen zijn beperkt tot rechten op basis van het gedocumenteerde demo-scenario.|



### <a name="pci-dss-requirement-713"></a>PCI-DSS vereiste 7.1.3

**7.1.3** toegang op basis van de taakclassificatie en de functie afzonderlijke personeel toewijzen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Gebruikersrollen zijn beperkt tot rechten op basis van het gedocumenteerde demo-scenario.|



### <a name="pci-dss-requirement-714"></a>PCI-DSS vereiste 7.1.4

**7.1.4** gedocumenteerde goedkeuring door geautoriseerde partijen geven vereiste bevoegdheden vereist.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het beperken van toegang tot de onderdelen van het systeem en gegevens van de kaarthouder om alleen de personen met een functie die toegang vereist. Dit omvat beperken en beperken van toegang tot de Azure-beheerportal en geven accounts of rollen met machtiging voor het maken, wijzigen of verwijderen PaaS-services.|



## <a name="pci-dss-requirement-72"></a>PCI-DSS vereiste 7.2

**7.2** een Toegangscontrolesysteem tot stand brengen voor systemen onderdelen die beperkt de toegang op basis van een gebruiker weten wilt, en is ingesteld op 'Alles weigeren' tenzij expliciet is toegestaan.
Dit Toegangscontrolesysteem moet het volgende omvatten:
- 7.2.1 dekking van alle onderdelen van het systeem.
- 7.2.2 toewijzing van bevoegdheden aan op basis van indeling van de taak en de functie personen.
- 7.2.3 standaardinstelling 'weigeren alle'.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Active Directory toegang te beperken tot alleen de aangewezen gebruikers. Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).|



## <a name="pci-dss-requirement-73"></a>PCI-DSS vereiste 7.3

**7.3** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het beperken van toegang tot gegevens van de kaarthouder gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De documentatie van Contoso Webstore biedt een gebruiksvoorbeeld en een beschrijving met betrekking tot die CHD gebruikt en hoe CHD wordt gebruikt.|




