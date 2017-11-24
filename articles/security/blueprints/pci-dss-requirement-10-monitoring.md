---
title: Azure betaling verwerking blauwdruk - controle van vereisten
description: PCI-DSS vereiste 10
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 293a1673-54bc-478c-9400-231074004eee
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 5fa1d17e68ce04b1f67081479518279be6cca099
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="monitoring-requirements-for-pci-dss-compliant-environments"></a>Controle van vereisten voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-10"></a>PCI-DSS vereiste 10

**Traceren en alle toegang tot netwerkbronnen en kaarthouder gegevens controleren**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Logboekregistratie mechanismen en de mogelijkheid om bij te houden van gebruikersactiviteiten zijn essentieel bij het voorkomen, detecteren of minimaliseert de impact van een inbreuk op gegevens. De aanwezigheid van Logboeken in alle omgevingen staat grondige bijhouden, waarschuwingen en analyse wanneer er iets mis gaat. De oorzaak van een inbreuk is erg lastig zijn, of zelfs onmogelijk zonder systeemlogboeken activiteit.

## <a name="pci-dss-requirement-101"></a>PCI-DSS vereiste 10.1

**10.1** audittrails implementeren voor alle toegang tot het koppelen aan elke individuele gebruiker systeemonderdelen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure beperkt toegang tot beheer- en diagnostische hulpprogramma's tot bevoegd personeel verantwoordelijkheid van de relevante taak. Microsoft Azure beperkt bevoorrechte toegang tot de hulpprogramma's in de productieomgeving op basis van de principes van de minste bevoegdheden. Microsoft Azure registreert en onderhoudt een logboek van alle afzonderlijke gebruikerstoegang tot Microsoft Azure-systeemonderdelen in de platform-omgeving.<br /><br />Microsoft Azure-platform-onderdelen (inclusief OS, CloudNet en Fabric) zijn geconfigureerd voor het aanmelden en beveiligingsgebeurtenissen verzamelen. Activiteit van de beheerder in de Microsoft Azure-platform wordt vastgelegd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft uitgebreide logboekregistratie van alle systeem- en gebruikersactiviteit (inclusief CHD logboekregistratie). Zie voor meer informatie [richtlijnen PCI - logboekregistratie](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-102"></a>PCI-DSS vereiste 10.2

**10.2** implementeren geautomatiseerde audittrails voor alle onderdelen van het systeem opnieuw van de volgende gebeurtenissen:
- **10.2.1** alle afzonderlijke gebruikerstoegang tot gegevens van de kaarthouder
- **10.2.2** alle acties die door een persoon met de hoofd- of beheerdersrechten heeft
- **10.2.3** toegang tot alle audittrails
- **10.2.4** ongeldig logische toegangspogingen
- **10.2.5** gebruik van en wijzigingen aan mechanismen voor identificatie en verificatie, inclusief maar niet beperkt tot het maken van nieuwe accounts en verhoging van bevoegdheden, en alle wijzigingen, toevoegingen of verwijderingen aan accounts met basis- of beheerdersrechten bevoegdheden
- **10.2.6** initialisatie stoppen of onderbreken van de controlelogboeken
- **10.2.7** maken en verwijderen van objecten op systeemniveau

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure beperkt toegang tot beheer- en diagnostische hulpprogramma's tot bevoegd personeel verantwoordelijkheid van de relevante taak. Microsoft Azure beperkt bevoorrechte toegang tot de hulpprogramma's in de productieomgeving op basis van de principes van de minste bevoegdheden. Microsoft Azure registreert en onderhoudt een logboek van alle afzonderlijke gebruikerstoegang tot Microsoft Azure-systeemonderdelen in de platform-omgeving.<br /><br />Microsoft Azure-platform-onderdelen (inclusief OS, CloudNet en Fabric) zijn geconfigureerd voor het aanmelden en beveiligingsgebeurtenissen verzamelen. Activiteit van de beheerder in de Microsoft Azure-platform wordt vastgelegd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft uitgebreide logboekregistratie van alle systeem- en activiteiten, waaronder CHD logboekregistratie. Zie voor meer informatie [richtlijnen PCI - logboekregistratie](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-103"></a>PCI-DSS vereiste 10.3

**10.3** vastleggen ten minste de volgende audit audittrailvermeldingen voor alle onderdelen van het systeem voor elke gebeurtenis:
- **10.3.1** gebruikers-id
- **10.3.2** type gebeurtenis
- **10.3.3** datum en tijd
- **10.3.4** slagen of mislukken vermelding
- **10.3.5** oorsprong van gebeurtenis
- **10.3.6** identiteit of de naam van de betrokken gegevens, systeemonderdeel of resource

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure procedures voor het synchroniseren van servers tot stand heeft gebracht en netwerkapparaten in de Microsoft Azure-omgeving met NTP Stratum 1 tijdservers gesynchroniseerd naar globale positionering System (GPS) satellieten. Synchronisatie wordt automatisch uitgevoerd om de vijf minuten. Microsoft Azure is verantwoordelijk voor het garanderen servicehosts goed synchronisatietijd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore registreert gebruikers-id, gebeurtenistype, datum-/ tijdstempel, gebeurtenissen voor geslaagde is mislukt, bron van gebeurtenis en de naam van de resource zoals vereist door het besturingselement 10.3.|



## <a name="pci-dss-requirement-104"></a>PCI-DSS vereiste 10.4

**10.4** met tijdsynchronisatie technologie werkt, synchroniseren van alle kritieke systeemklokken en tijden en zorg ervoor dat de volgende voor het ophalen, distribueren en opslaan van de tijd is geïmplementeerd. 
> [!NOTE]
> Een voorbeeld van tijd synchronisatie technologie is Network Time Protocol (NAP).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure procedures voor het synchroniseren van servers tot stand heeft gebracht en netwerkapparaten in de Microsoft Azure-omgeving met NTP Stratum 1 tijdservers gesynchroniseerd naar globale positionering System (GPS) satellieten. Synchronisatie wordt automatisch uitgevoerd om de vijf minuten. Microsoft Azure is verantwoordelijk voor het garanderen servicehosts goed synchronisatietijd. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Tijdsynchronisatie voor de PaaS-service wordt uitgevoerd door Azure.|



### <a name="pci-dss-requirement-1041"></a>PCI-DSS vereiste 10.4.1

**10.4.1** kritieke systemen correct en consistente tijd hebt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.4](#pci-dss-requirement-10-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Tijdsynchronisatie voor de PaaS-service wordt uitgevoerd door Azure.|



### <a name="pci-dss-requirement-1042"></a>PCI-DSS vereiste 10.4.2

**10.4.2** tijdgegevens is beveiligd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.4](#pci-dss-requirement-10-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Tijdsynchronisatie voor de PaaS-service wordt uitgevoerd door Azure.|



### <a name="pci-dss-requirement-1043"></a>PCI-DSS vereiste 10.4.3

**10.4.3** tijdinstellingen van algemeen geaccepteerde tijdsbronnen worden ontvangen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.4](#pci-dss-requirement-10-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Tijdsynchronisatie voor de PaaS-service wordt uitgevoerd door Azure.|



## <a name="pci-dss-requirement-105"></a>PCI-DSS vereiste 10.5

**10.5** beveiligde audit muissporen zodat deze kunnen niet worden gewijzigd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | FIM en id's worden geïmplementeerd in de Microsoft Azure-omgeving. Microsoft Azure gebruikt EWS ter ondersteuning van realtime analyses van gebeurtenissen in de operationele omgeving. MAs en doelstellingen genereren bijna realtime-waarschuwingen over gebeurtenissen die mogelijk inbreuk is gemaakt van het systeem. <br /><br />Logboekregistratie van gebeurtenissen die service, gebruiker en beveiliging (webserverlogboeken, FTP-server-Logboeken, enzovoort) is ingeschakeld en centraal behouden. Azure beperkt toegang tot de controlelogboeken tot bevoegd personeel op basis van verantwoordelijkheden. Logboeken worden gearchiveerd op de infrastructuur van Azure beveiligde archivering en 180 dagen worden bewaard. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt voor de controle van alle elementen aan OMS. Back-ups maken van een externe bron kan worden uitgevoerd door [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1051"></a>PCI-DSS vereiste 10.5.1

**10.5.1** limiet weergeven van audit muissporen die die een taak betrekking hebben.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.5](#pci-dss-requirement-10-5). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt voor de controle van alle elementen aan OMS. Back-ups maken van een externe bron kan worden uitgevoerd door [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1052"></a>PCI-DSS vereiste 10.5.2

**10.5.2** beveiligen audit audittrail-bestanden van niet-geautoriseerde wijzigingen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.5](#pci-dss-requirement-10-5). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt voor de controle van alle elementen aan OMS. Back-ups maken van een externe bron kan worden uitgevoerd door [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1053"></a>PCI-DSS vereiste 10.5.3

**10.5.3** onmiddellijk back-up audit audittrail-bestanden naar een gecentraliseerde logboek-server of de media die moeilijk te wijzigen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.5](#pci-dss-requirement-10-5). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt voor de controle van alle elementen aan OMS. Back-ups maken van een externe bron kan worden uitgevoerd door [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1054"></a>PCI-DSS vereiste 10.5.4

**10.5.4** schrijven logboeken voor extern gerichte technologieën op een veilige, gecentraliseerde, interne logboek-server of media-apparaat.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.5](#pci-dss-requirement-10-5). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt voor de controle van alle elementen aan OMS. Back-ups maken van een externe bron kan worden uitgevoerd door [Azure Backup](https://azure.microsoft.com/services/backup/).|



### <a name="pci-dss-requirement-1055"></a>PCI-DSS vereiste 10.5.5

**10.5.5** -integriteit bewaking of wijzig-detectie software van Logboeken gebruiken om ervoor te zorgen dat bestaande gegevens aan het logboek kan niet worden gewijzigd zonder dat er waarschuwingen gegenereerd (Hoewel de nieuwe gegevens worden toegevoegd leidt doorgaans niet tot een waarschuwing).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.5](#pci-dss-requirement-10-5). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt voor de controle van alle elementen aan OMS. Back-ups maken van een externe bron kan worden uitgevoerd door [Azure Backup](https://azure.microsoft.com/services/backup/).|



## <a name="pci-dss-requirement-106"></a>PCI-DSS vereiste 10.6

**10.6** Raadpleeg Logboeken en beveiligingsgebeurtenissen voor alle onderdelen van het systeem om afwijkingen of verdachte activiteiten te identificeren.
 
> [!NOTE]
> Verzamelen en waarschuwingen hulpprogramma's om te voldoen aan deze eis kunnen worden gebruikt bij het parseren van het logboek.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | FIM en id's worden geïmplementeerd in de Microsoft Azure-omgeving. Microsoft Azure gebruikt EWS ter ondersteuning van realtime analyses van gebeurtenissen in de operationele omgeving. MAs en doelstellingen genereren bijna realtime-waarschuwingen over gebeurtenissen die mogelijk inbreuk is gemaakt van het systeem. <br /><br />Logboekregistratie van gebeurtenissen die service, gebruiker en beveiliging (webserverlogboeken, FTP-server-Logboeken, enzovoort) is ingeschakeld en centraal behouden. Azure beperkt toegang tot de controlelogboeken tot bevoegd personeel op basis van verantwoordelijkheden. Logboeken worden gearchiveerd op de infrastructuur van Azure beveiligde archivering en 180 dagen worden bewaard. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Maakt gebruik van de Contoso Webstore [Azure Security Center](https://azure.microsoft.com/services/security-center/) bewaakt en rapporteren en afwijkingen voorkomen. [Azure Advisor](/azure/advisor/advisor-security-recommendations) biedt een consistente, geconsolideerde weergave van aanbevelingen voor alle Azure-resources.|



### <a name="pci-dss-requirement-1061"></a>PCI-DSS vereiste 10.6.1

**10.6.1** ten minste dagelijks Controleer het volgende:
- Alle beveiligingsgebeurtenissen
- Logboeken van alle onderdelen van het systeem die opslaan, verwerkt of verzonden CHD en/of SAD
- Logboeken van alle kritieke systeemonderdelen
- Logboeken van alle servers en -onderdelen van het systeem die beveiligingsfuncties (bijvoorbeeld, firewalls, detecteren van indringers inbraakdetectie-systemen-preventie systemen (id's / IP-Adressen), authentication-servers, e-commerce omleiding servers, enzovoort) uitvoeren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.6](#pci-dss-requirement-10-6). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Maakt gebruik van de Contoso Webstore [Azure Security Center](https://azure.microsoft.com/services/security-center/) bewaakt en rapporteren en afwijkingen voorkomen. [Azure Advisor](/azure/advisor/advisor-security-recommendations) biedt een consistente, geconsolideerde weergave van aanbevelingen voor alle Azure-resources.|



### <a name="pci-dss-requirement-1062"></a>PCI-DSS vereiste 10.6.2

**10.6.2** logboeken van alle andere onderdelen van het systeem regelmatig op basis van de organisatie beleidsregels en risico strategie voor het beheer, zoals wordt bepaald door de jaarlijkse risico-evaluatie van de organisatie.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 10.6](#pci-dss-requirement-10-6). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Maakt gebruik van de Contoso Webstore [Azure Security Center](https://azure.microsoft.com/services/security-center/) bewaakt en rapporteren en afwijkingen voorkomen. [Azure Advisor](/azure/advisor/advisor-security-recommendations) biedt een consistente, geconsolideerde weergave van aanbevelingen voor alle Azure-resources.|



### <a name="pci-dss-requirement-1063"></a>PCI-DSS vereiste 10.6.3

**10.6.3** vervolgactie uitzonderingen en afwijkingen die zijn geïdentificeerd tijdens de controle.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie 'Microsoft Azure' sectie [vereiste 10.6](#pci-dss-requirement-10-6). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Maakt gebruik van de Contoso Webstore [Azure Security Center](https://azure.microsoft.com/services/security-center/) bewaakt en rapporteren en afwijkingen voorkomen. [Azure Advisor](/azure/advisor/advisor-security-recommendations) biedt een consistente, geconsolideerde weergave van aanbevelingen voor alle Azure-resources.|



## <a name="pci-dss-requirement-107"></a>PCI-DSS vereiste 10.7

**10.7** behouden controlegeschiedenis audittrail voor ten minste één jaar met een minimum van drie maanden die onmiddellijk beschikbaar zijn voor analyse (bijvoorbeeld online, gearchiveerde of te herstellen vanuit back-up).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure behoudt controlelogboeken voor één jaar met de meest recente 3 maanden onmiddellijk toegankelijk via hun interne portal. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft uitgebreide logboekregistratie van alle systeem- en gebruikersactiviteit (inclusief CHD logboekregistratie). Zie voor meer informatie [richtlijnen PCI - logboekregistratie](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-108"></a>PCI-DSS vereiste 10.8

**10.8** **aanvullende vereisten voor alleen-providers:** Implementeer een proces voor een tijdige detectie en rapportage van fouten van kritieke besturingselement beveiligingssystemen, inclusief maar niet beperkt tot het mislukken van:
- Firewalls
- ID 'S/IP-ADRESSEN
- FIM
- Antivirusprogramma 's
- Fysieke toegangscontrole
- Logische toegangsbeheer
- Logboekregistratie mechanismen controleren
- Segmentering besturingselementen (indien gebruikt) 

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.



**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure gebruikt EWS ter ondersteuning van realtime analyses van gebeurtenissen in de operationele omgeving. MAs en doelstellingen genereren bijna realtime-waarschuwingen over gebeurtenissen die mogelijk inbreuk is gemaakt van het systeem. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft uitgebreide logboekregistratie van alle systeem- en gebruikersactiviteit (inclusief CHD logboekregistratie). Zie voor meer informatie [richtlijnen PCI - logboekregistratie](payment-processing-blueprint.md#logging-and-auditing).|



### <a name="pci-dss-requirement-1081"></a>PCI-DSS vereiste 10.8.1

**10.8.1** **aanvullende vereisten voor alleen-providers:** reageren op fouten van alle kritieke beveiligingsmechanismen tijdig. Processen voor het reageren op fouten in beveiligingsmechanismen moeten omvatten:
- Beveiligingsfuncties herstellen
- Te identificeren en documenteren van de duur (datum en tijd begin tot eind om) van de beveiligingsfout
- Te identificeren en documenteren cause(s) mislukt, met inbegrip van de hoofdoorzaak en herstel die zijn vereist voor het adres hoofdoorzaak documenteren
- Te identificeren en beveiligingsproblemen die ontstaan tijdens de fout adressering
- Uitvoeren van een risicoanalyse uit te voeren om te bepalen of verdere acties als gevolg van de beveiligingsfout vereist
- Implementatie van besturingselementen om te voorkomen dat de oorzaak van de fout opnieuw optreedt - controle van besturingselementen voor de beveiliging wordt hervat 

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure gebruikt EWS ter ondersteuning van realtime analyses van gebeurtenissen in de operationele omgeving. MAs en doelstellingen genereren bijna realtime-waarschuwingen over gebeurtenissen die mogelijk inbreuk is gemaakt van het systeem. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore heeft uitgebreide logboekregistratie van alle systeem- en gebruikersactiviteit (inclusief CHD logboekregistratie). Zie voor meer informatie [richtlijnen PCI - logboekregistratie](payment-processing-blueprint.md#logging-and-auditing).|



## <a name="pci-dss-requirement-109"></a>PCI-DSS vereiste 10.9

**10.9** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor het bewaken van alle toegang tot netwerkbronnen en gegevens van de kaarthouder gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een gebruiksvoorbeeld en een beschrijving van hoe de CHD beheerd en beveiligd.|




