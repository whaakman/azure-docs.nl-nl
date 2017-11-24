---
title: Azure betaling verwerking blauwdruk - vereisten
description: PCI-DSS vereiste 12
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: a79d59d8-20e3-4efe-8686-c8f4ed80e220
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 05e9ed7c886d37a024db1eedbc541705b7d8a9a9
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="policy-requirements-for-pci-dss-compliant-environments"></a>Vereisten voor PCI DSS-compatibele omgevingen  
## <a name="pci-dss-requirement-12"></a>PCI-DSS vereiste 12

**Een beleid die zijn gericht op beveiliging van informatie voor alle medewerkers onderhouden**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Een beleid voor sterke beveiliging zet de toon beveiliging voor de hele entiteit en informeert personeel van hen wordt verwacht. Alle medewerkers moet rekening houden met de vertrouwelijkheid van gegevens en hun verantwoordelijkheden voor het te beveiligen. Voor de doeleinden van vereiste 12 wordt "personeel" verwijst naar fulltime en fulltime werknemers, tijdelijke werknemers, contractanten en consultants die op de site van de entiteit 'residente' zijn of anders hebben toegang tot de gegevens kaarthouder omgeving.

## <a name="pci-dss-requirement-121"></a>PCI-DSS vereiste 12.1

**12.1** maken, publiceren, onderhouden en de verspreiding van een beveiligingsbeleid.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het instellen en beheren van een beleid voor beveiliging.|



### <a name="pci-dss-requirement-1211"></a>PCI-DSS vereiste 12.1.1

**12.1.1** het beveiligingsbeleid voor ten minste jaarlijks controleren en bijwerken van het beleid wanneer de omgeving wordt gewijzigd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het bijwerken van hun informatie-beveiligingsbeleid ten minste eenmaal per jaar, of wanneer er wijzigingen zijn in hun omgeving in kaarthouder gegevens (CDE).|



## <a name="pci-dss-requirement-122"></a>PCI-DSS vereiste 12.2

**12.2** implementeren een risicoanalyse verwerken die:
- Ten minste jaarlijks en belangrijke wijzigingen aangebracht in de omgeving (bijvoorbeeld overname, fusie, verplaatsing, enz.) wordt uitgevoerd
- Identificeert essentiële activa, bedreigingen en beveiligingsproblemen
- Resulteert in een formele, gedocumenteerde analyse van risico's.
- > Voorbeelden van risico-evaluatie methoden zijn, maar zijn niet beperkt tot OCTAAF ISO 27005 en NIST SP 800-30.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het implementeren van een proces voor het beoordeling van risico's die zijn gericht op alle bedreigingen die worden vermeld in de vereiste 12.2.|



## <a name="pci-dss-requirement-123"></a>PCI-DSS vereiste 12.3

**12.3** gebruiksbeleid voor kritieke technologieën ontwikkelen en correct gebruik van deze technologieën te definiëren.

> [!NOTE]
> Voorbeelden van kritieke technologieën omvatten, maar zijn niet beperkt tot externe toegang en draadloze technologieën, laptops, tablets, verwisselbare elektronische media, e-gebruik en Internet-gebruik.
Zorg ervoor dat deze gebruiksbeleid is het volgende vereist.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1231"></a>PCI-DSS vereiste 12.3.1

**12.3.1** expliciete goedkeuring door geautoriseerde partijen

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1232"></a>PCI-DSS vereiste 12.3.2

**12.3.2** verificatie voor gebruik van de technologie

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1233"></a>PCI-DSS vereiste 12.3.3

**12.3.3** een lijst met alle dergelijke apparaten en medewerkers met toegang

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1234"></a>PCI-DSS vereiste 12.3.4

**12.3.4** een methode voor het eenvoudig en nauwkeurig bepalen eigenaar, contactgegevens en doel (bijvoorbeeld labels, coderen en/of inventariseren van apparaten)

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1235"></a>PCI-DSS vereiste 12.3.5

**12.3.5** acceptabel maakt gebruik van de technologie

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1236"></a>PCI-DSS vereiste 12.3.6

**12.3.6** acceptabele netwerklocaties voor de technologieën

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het bepalen van de aanvaardbare netwerklocaties voor virtuele machines in de cloud, opslag en ondersteunende services.|



### <a name="pci-dss-requirement-1237"></a>PCI-DSS vereiste 12.3.7

**12.3.7** lijst van producten bedrijf goedgekeurde

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het bepalen van de aanvaardbare netwerklocaties voor virtuele machines in de cloud, opslag en ondersteunende services.|



### <a name="pci-dss-requirement-1238"></a>PCI-DSS vereiste 12.3.8

**12.3.8** automatisch verbreken van sessies voor de RAS-technologieën na een bepaalde periode van inactiviteit

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure maakt gebruik van Microsoft zakelijke AD sessie lock-functie worden afgedwongen sessie uitsluitingen na een periode van inactiviteit. Netwerkverbindingen worden beëindigd na 30 minuten van inactiviteit. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1239"></a>PCI-DSS vereiste 12.3.9

**12.3.9** activering van de RAS-technologieën voor leveranciers en zakelijke partners alleen wanneer deze nodig leveranciers en zakelijke partners, met directe deactivering na gebruik

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-12310"></a>PCI-DSS vereiste 12.3.10

**12.3.10** voor toegang tot gegevens via de RAS-technologieën, kaarthouder personeel verbieden het kopiëren, verplaatsen en opslag van gegevens van de kaarthouder op de lokale harde schijven en verwisselbare media van elektronische, tenzij u uitdrukkelijk toestemming voor een gedefinieerde zakelijke behoeften.
Wanneer er een geautoriseerde zakelijke behoeften, moeten het gebruiksbeleid vereisen de gegevens worden beveiligd in overeenstemming met alle toepasselijke PCI DSS-eisen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het garanderen dat toegang tot gegevens via de RAS-technologieën kaarthouder personeel zijn uitgesloten van het kopiëren, verplaatsen en opslaan van gegevens van de kaarthouder op de lokale harde schijven en verwisselbare media van elektronische, tenzij u uitdrukkelijk toestemming voor een bepaalde zakelijke behoeften.|



## <a name="pci-dss-requirement-124"></a>PCI-DSS vereiste 12,4

**12,4** ervoor te zorgen dat de beveiligingsbeleid en procedures duidelijk te informatie beveiligingstaken voor alle medewerkers definiëren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1241"></a>PCI-DSS vereiste 12.4.1

**12.4.1** aanvullende vereisten voor alleen-providers: uitvoerend management stelt verantwoordelijkheid voor de beveiliging van gegevens van de kaarthouder en een programma PCI DSS naleving om op te nemen:
- Algemene verantwoordelijkheid voor het onderhouden van PCI DSS naleving
- Een freelance voor een PCI DSS naleving programma's en communicatie definiëren naar uitvoerend management 

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten die serviceproviders zijn verantwoordelijk voor hun programma PCI-naleving documenteren.|



## <a name="pci-dss-requirement-125"></a>PCI-DSS vereiste 12,5

**12,5** toewijzen aan een persoon of het team van de volgende informatie management beveiligingstaken.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het definiëren en informatie beveiligingstaken naar hun werknemers toewijzen.|



### <a name="pci-dss-requirement-1251"></a>PCI-DSS vereiste 12.5.1

**12.5.1** maken, documenteer en distribueren van beveiligingsbeleid en -procedures.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het definiëren en informatie beveiligingstaken naar hun werknemers toewijzen.|



### <a name="pci-dss-requirement-1252"></a>PCI-DSS vereiste 12.5.2

**12.5.2** monitor en analyseren van beveiligingswaarschuwingen en distribueren aan geschikte personen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het definiëren en informatie beveiligingstaken naar hun werknemers toewijzen.|



### <a name="pci-dss-requirement-1253"></a>PCI-DSS vereiste 12.5.3

**12.5.3** maken, documenteer en distribueren van incident antwoord en escalatie beveiligingsprocedures om ervoor te zorgen tijdig en doeltreffend verwerking van alle situaties.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1254"></a>PCI-DSS vereiste 12.5.4

**12.5.4** gebruikersaccounts, met inbegrip van toevoegingen, verwijderingen en wijzigingen beheren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



### <a name="pci-dss-requirement-1255"></a>PCI-DSS vereiste 12.5.5

**12.5.5** bewaking en beheer alle toegang tot gegevens.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en onderhouden van de beleidsregels correct gebruik, implementatie en verificatie voor kritieke technologieën binnen hun CDE dicteren.|



## <a name="pci-dss-requirement-126"></a>PCI-DSS vereiste 12,6

**12,6** implementeren van een formele beveiliging awareness programma zodat alle medewerkers rekening houden met het belang van de kaarthouder gegevens beveiligingsbeleid en -procedures.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken en beheren van beveiliging bewustzijn van medewerkers met toegang tot het CDE omringende-beleid.|



### <a name="pci-dss-requirement-1261"></a>PCI-DSS vereiste 12.6.1

**12.6.1** opleiden personeel van aanstelling en ten minste eenmaal per jaar. 

> [!NOTE]
> Methoden kunnen variëren afhankelijk van de rol van het personeel en het niveau van toegang tot de gegevens van de kaarthouder.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor personeel ontvangen en bevestigt informatiebeveiliging en PCI-DSS-awareness training ten minste eenmaal per jaar.|



### <a name="pci-dss-requirement-1262"></a>PCI-DSS vereiste 12.6.2

**12.6.2** medewerkers ten minste jaarlijks bevestigt dat ze hebben gelezen en beveiligingsbeleid en procedures begrepen nodig.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor personeel ontvangen en bevestigt informatiebeveiliging en PCI-DSS-awareness training ten minste eenmaal per jaar.|



## <a name="pci-dss-requirement-127"></a>PCI-DSS vereiste 12,7

**12,7** scherm potentiële personeel vóór inhuren om het risico van aanvallen, van interne bronnen minimaliseren. (Voorbeelden van achtergrondcontroles zijn eerdere werkervaring, strafbaar record, kredietgeschiedenis en verwijzing controleert.) 

> [!NOTE]
> Voor deze mogelijke personeel om te worden aangenomen voor bepaalde functies zoals store kassamedewerkers alleen toegang tot een creditcardnummer tegelijk hebben wanneer een transactie vergemakkelijken, deze vereiste is alleen een aanbeveling.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor medewerkers met toegang op de CDE grondige achtergrondonderzoek ondergaan.|



## <a name="pci-dss-requirement-128"></a>PCI-DSS vereiste 12,8

**12,8** onderhouden en implementeer beleidsregels en procedures voor het beheren van serviceproviders kaarthouder gegevens worden gedeeld met wie of die de beveiliging van gegevens van de kaarthouder, als volgt kan beïnvloeden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor de bewaking van PCI-naleving voor serviceproviders met wie kaarthouder gegevens wordt gedeeld of de beveiliging van het CDE kan beïnvloeden. Klanten moeten ervoor zorgen dat een lijst met alle service biedt binnen hun CDE gebruikt.|



### <a name="pci-dss-requirement-1281"></a>PCI-DSS vereiste 12.8.1

**12.8.1** onderhouden van een lijst met serviceproviders, inclusief een beschrijving van de service die wordt geleverd.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor de bewaking van PCI-naleving voor serviceproviders met wie kaarthouder gegevens wordt gedeeld of de beveiliging van het CDE kan beïnvloeden. Klanten moeten ervoor zorgen dat een lijst met alle service biedt binnen hun CDE gebruikt.|



### <a name="pci-dss-requirement-1282"></a>PCI-DSS vereiste 12.8.2

**12.8.2** onderhouden van een schriftelijke overeenkomst met een bevestiging dat de serviceproviders zelf verantwoordelijk voor de beveiliging van gegevens van de kaarthouder de serviceproviders bezit zijn of anders opslaan, verwerken of verzenden namens de klant, of voor zover ze kunnen invloed hebben op de beveiliging van de klant kaarthouder gegevensomgeving. 

> [!NOTE]
> De exacte naam opgeven van een bevestiging is afhankelijk van de overeenkomst tussen de twee partijen, de details van de service die wordt aangeboden en de verantwoordelijkheden toegewezen aan elke partij. De bevestiging hoeft niet te omvatten de exacte naam opgeven die worden opgegeven in deze vereiste.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het onderhouden van schriftelijke overeenkomsten met serviceproviders die de verantwoordelijkheid voor het onderhouden van de beveiliging van gegevens van de kaarthouder zijn bevestigd.|



### <a name="pci-dss-requirement-1283"></a>PCI-DSS vereiste 12.8.3

**12.8.3** Zorg dat er een tot stand gebrachte proces voor het juiste vanwege toewijding inzetten vóór engagement inclusief serviceproviders bezighouden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor er is een tot stand gebrachte proces voor het juiste vanwege toewijding inzetten vóór engagement inclusief serviceproviders bezighouden.|



### <a name="pci-dss-requirement-1284"></a>PCI-DSS vereiste 12.8.4

**12.8.4** onderhouden van een programma voor het bewaken van de internetproviders PCI DSS compatibiliteitsstatus ten minste eenmaal per jaar.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten is verantwoordelijk voor het onderhouden van een programma voor het bewaken van de internetproviders PCI DSS compatibiliteitsstatus ten minste eenmaal per jaar.|



### <a name="pci-dss-requirement-1285"></a>PCI-DSS vereiste 12.8.5

**12.8.5** bewaren informatie over welke PCI DSS-vereisten zijn beheerd door elke serviceprovider en die worden beheerd door de entiteit.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het bewaren van een kopie van de [verantwoordelijkheid samenvatting Matrix](https://aka.ms/pciblueprintcrm32), dit geeft een overzicht van de PCI DSS-voorschriften die de verantwoordelijkheid van de klant en welke de verantwoordelijkheid van de Microsoft Azure zijn.|



## <a name="pci-dss-requirement-129"></a>PCI-DSS vereiste 12,9

**12,9** aanvullende vereisten voor alleen-providers: serviceproviders erkent in te schrijven naar klanten die ze verantwoordelijk voor de beveiliging van gegevens van de kaarthouder de serviceprovider bezit of anders is opgeslagen zijn, processen, of Hiermee worden verzonden namens de klant, of voor zover dat ze de beveiliging van de klant kaarthouder gegevensomgeving kunnen beïnvloeden. 

> [!NOTE]
> De exacte naam opgeven van een bevestiging is afhankelijk van de overeenkomst tussen de twee partijen, de details van de service die wordt aangeboden en de verantwoordelijkheden toegewezen aan elke partij. De bevestiging hoeft niet te omvatten de exacte naam opgeven die worden opgegeven in deze vereiste.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten die serviceproviders zijn verantwoordelijk voor hun verantwoordelijkheden voor het onderhouden van PCI-naleving zijn bevestigd. |



## <a name="pci-dss-requirement-1210"></a>PCI-DSS vereiste 12.10

**12.10** implementeren van een beveiligingsplan. Zorg ervoor dat u kunt direct reageren op een schending van het systeem.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



### <a name="pci-dss-requirement-12101"></a>PCI-DSS vereiste 12.10.1

**12.10.1** maken van het plan voor respons op incidenten worden geïmplementeerd in het geval van inbreuk op systeem. Zorg ervoor dat de adressen plan ten minste de volgende:
- Rollen en verantwoordelijkheden communicatie en neem contact op met de strategieën in het geval van een inbreuk met inbegrip van meldingen van de merken betaling ten minste
- Procedures voor specifieke respons op incidenten
- Procedures voor het herstel en de continuïteit van bedrijven
- Gegevens back-processen
- Analyse van wettelijke vereisten voor het melden van inbreuk
- Dekking en antwoorden van alle kritieke systeemonderdelen
- Referentie- of insluitingsfilters van respons op incidenten procedures van de betaling merken

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



### <a name="pci-dss-requirement-12102"></a>PCI-DSS vereiste 12.10.2

**12.10.2** controleren en testen van het abonnement, met inbegrip van alle elementen die worden vermeld in vereiste 12.10.1, ten minste eenmaal per jaar.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



### <a name="pci-dss-requirement-12103"></a>PCI-DSS vereiste 12.10.3

**12.10.3** aanwijzen specifiek personeel beschikbaar zijn op basis van 24/7 om te reageren op waarschuwingen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



### <a name="pci-dss-requirement-12104"></a>PCI-DSS vereiste 12.10.4

**12.10.4** voorzien van juiste training aan personeel inbreuk op de beveiliging antwoord verantwoordelijkheden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



### <a name="pci-dss-requirement-12105"></a>PCI-DSS vereiste 12.10.5

**12.10.5** waarschuwingen van de bewaking, inclusief maar niet beperkt tot het detecteren van indringers, inbraakdetectie preventie, firewalls, en de bewaking systemen integriteit van de beveiliging omvatten.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



### <a name="pci-dss-requirement-12106"></a>PCI-DSS vereiste 12.10.6

**12.10.6** ontwikkelen van een proces om te wijzigen en het plan voor respons op incidenten volgens opgedane kennis ontwikkelen en bedrijfstak ontwikkelingen opnemen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het ontwikkelen van IR-plan en tests die overweegt een klant-besturingselementen die met betrekking tot gedeelde touch-punten en alle toepassingen van de klant gebruik van Azure infrastructuur. Het is van de klant verantwoordelijkheid nauwkeurige contactgegevens bieden aan Azure in het geval van een incident moet worden gerapporteerd aan die van invloed kan hun toepassingen of gegevens.|



## <a name="pci-dss-requirement-1211"></a>PCI-DSS vereiste 12.11

**12.11** **aanvullende vereisten voor alleen-providers:** beoordelingen minimaal eenmaal per kwartaal om te bevestigen dat personeel volgt beveiligingsbeleid en operationele procedures uitvoeren.
Beoordelingen moeten betrekking hebben op de volgende processen:
- Beoordelingen van dagelijkse logboek
- Firewall-regelset beoordelingen
- Configuratiestandaarden toepassen op nieuwe systemen
- Reageren op beveiligingswaarschuwingen
- Wijzigen van de processen voor Apparaatbeheer 

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten die serviceproviders zijn verantwoordelijk voor hun beoordelingen van processen voor het bevestigen van PCI-naleving besturingselement prestaties documenteren.|



### <a name="pci-dss-requirement-12111"></a>PCI-DSS vereiste 12.11.1

**12.11.1** aanvullende vereisten voor alleen-providers: onderhouden van documentatie van elk kwartaal controleproces om op te nemen:
- Resultaten van de beoordelingen documenteren
- Bekijken en afmelden van de resultaten door toegewezen medewerkers verantwoordelijkheid voor het programma PCI DSS-compatibiliteit 

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.


**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten die serviceproviders zijn verantwoordelijk voor hun beoordelingen van processen voor het bevestigen van PCI-naleving besturingselement prestaties documenteren.|




