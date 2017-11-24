---
title: Azure betaling verwerking blauwdruk - Identiteitsvereisten?
description: PCI-DSS vereiste 8
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 1a398601-8c48-4f8e-b3d4-eba94edad61c
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: f77cc3c9926b5316913c70e5f4412383e55c5193
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="identity-requirements-for-pci-dss-compliant-environments"></a>Identiteitsvereisten voor PCI DSS-compatibele omgevingen 
## <a name="pci-dss-requirement-8"></a>PCI-DSS vereiste 8

**Identificeren en verifiëren van toegang tot de onderdelen van het systeem**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Een unieke identificatie (ID) toewijzen aan elke persoon met toegang zorgt ervoor dat elke gebruiker een unieke verantwoordelijk zijn voor hun acties. Wanneer deze accountability aanwezig is, wordt in kritieke gegevens en systemen uitgevoerde acties worden uitgevoerd door en kunnen worden herleid naar bekende en gemachtigde gebruikers en processen.
De effectiviteit van een wachtwoord wordt grotendeels bepaald door het ontwerp en de implementatie van het verificatiesysteem: in het bijzonder hoe vaak wachtwoordpogingen kan worden uitgevoerd door een aanvaller en de beveiligingsmethoden voor wachtwoorden van gebruikers op het moment van item, beveiligen tijdens de verzending en in de opslag.

> [!NOTE]
> Deze vereisten gelden voor alle accounts, met inbegrip van verkooppunten accounts met beheerdersbevoegdheden en alle accounts die worden gebruikt om te bekijken of toegang tot de kaarthouder gegevens of naar systemen met gegevens van de kaarthouder toegang. Dit geldt ook voor accounts die worden gebruikt door de leveranciers en andere partijen (bijvoorbeeld voor ondersteuning of onderhoud). Deze vereisten zijn niet van toepassing op accounts die worden gebruikt door de consument (bijv, kaarthouders). Vereisten 8.1.1, 8.2, 8.5, 8.2.3 via 8.2.5 en 8.1.6 via 8.1.8 zijn echter niet bedoeld om toe te passen aan gebruikersaccounts in een toepassing verkooppunten betaling die alleen toegang tot een creditcardnummer tegelijk hebben om te faciliteren van een enkele transactie ( zoals kassamedewerker accounts).
 
## <a name="pci-dss-requirement-81"></a>PCI-DSS vereiste 8.1

**8.1** definiëren en implementeer beleidsregels en procedures om ervoor te zorgen beheer door de juiste gebruikers-id voor niet-consumer-gebruikers en beheerders van alle onderdelen van het systeem als volgt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore biedt een gebruiksvoorbeeld en een beschrijving voor het juiste gebruik van beheerders voor de voorbeeldimplementatie.|



### <a name="pci-dss-requirement-811"></a>PCI-DSS vereiste 8.1.1

**8.1.1** een unieke ID van alle gebruikers toewijzen voordat ze toegang systeemonderdelen of kaarthouder gegevens.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore implementeert Azure Active Directory en Azure Active Directory Role-Based toegangsbeheer (RBAC) om te controleren of alle gebruikers een unieke id hebben Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-812"></a>PCI-DSS vereiste 8.1.2

**8.1.2** toevoegen, verwijderen en wijzigen van de gebruikers-id's, referenties en andere id-objecten te beheren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore implementeert Azure Active Directory en Azure Active Directory Role-Based toegangsbeheer (RBAC) om te controleren of alle gebruikers een unieke id hebben Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-813"></a>PCI-DSS vereiste 8.1.3

**8.1.3** onmiddellijk toegang intrekken voor alle gebruikers beëindigd.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Active Directory voor gebruikersbeheer. Intrekking van gebruikers kan worden uitgevoerd in Active Directory.|



### <a name="pci-dss-requirement-814"></a>PCI-DSS vereiste punt 8.1.4 bedoelde

**Punt 8.1.4 bedoelde** verwijderen of uitschakelen van inactieve gebruikersaccounts binnen 90 dagen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Active Directory voor gebruikersbeheer. De `-enableADDomainPasswordPolicy` optie kan worden ingesteld om ervoor te zorgen wachtwoorden verlopen na 90 dagen.|



### <a name="pci-dss-requirement-815"></a>PCI-DSS vereiste 8.1.5

**8.1.5** beheren-id's van derden gebruikt om toegang, te ondersteunen, of als volgt handhaven van de onderdelen van het systeem via externe toegang:
- Ingeschakeld alleen tijdens de periode die nodig zijn en als deze niet in gebruik is uitgeschakeld.
- Bewaakt als deze in gebruik.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure heeft goedgekeurd van toepassing bedrijven en organisaties beveiligingsbeleid, met inbegrip van een beleid voor beveiliging. De beleidsregels zijn goedgekeurd, gepubliceerd, en wordt gecommuniceerd aan Microsoft Azure. Het beveiligingsbeleid informatie vereist dat de toegang tot Microsoft Azure activa om te worden toegekend op basis van een zakelijke rechtvaardiging met de activumeigenaar toestemming en op basis van de principes van de 'nodig ter informatie' en 'minimale bevoegdheden beperkt'. Daarnaast vereisten voor levenscyclusbeheer van toegang met inbegrip van toegang inrichten, verificatie, autorisatie ook een oplossing voor het beleid, verwijderen van toegangsrechten, en periodieke toegang tot beoordelingen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De demo Contoso Webstore is geïmplementeerd met Azure Active Directory en Azure Active Directory Role-Based toegangsbeheer voor het beheren van de gebruikerstoegang tot de installatie. Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-816"></a>PCI-DSS vereiste 8.1.6

**8.1.6** limiet herhaald toegangspogingen door de gebruikers-ID na niet meer dan zes pogingen worden vergrendeld.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore is duidelijke scheiding van functies (BEPLANTEN) voor alle gebruikers van de demo geïmplementeerd. Zie voor meer informatie ' 'Azure Active Directory: Identity Protection' op [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).|



### <a name="pci-dss-requirement-817"></a>PCI-DSS vereiste 8.1.7

**8.1.7** de vergrendelingsduur ingesteld op een minimum van 30 minuten of totdat een beheerder de gebruikers-ID activeert.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken, afdwingen en bewaking van een wachtwoordbeleid die compatibel zijn met PCI DSS-vereisten.|



### <a name="pci-dss-requirement-818"></a>PCI-DSS vereiste 8.1.8

**8.1.8** als een sessie niet actief is geweest gedurende meer dan 15 minuten, moet de gebruiker opnieuw verifiëren voor de terminal of de sessie opnieuw te activeren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het maken, afdwingen en bewaking van een wachtwoordbeleid die compatibel zijn met PCI DSS-vereisten.|



## <a name="pci-dss-requirement-82"></a>PCI-DSS vereiste 8.2

**8.2** naast het toekennen van een unieke ID, zorg ervoor dat beheer van de juiste gebruikersverificatie voor niet-consumer-gebruikers en beheerders van alle onderdelen van het systeem door middel van ten minste één van de volgende methoden om alle gebruikers te verifiëren:
- Iets, zoals een wachtwoord of de wachtwoordzin weten
- Iets, zoals een token apparaat of een smartcard hebt
- Er, zoals een biometrische zijn

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De uitvoering van Contoso Webstore voor multi-factor authentication is uitgeschakeld om eenvoudig te gebruiken voor de demo. Multi-factor authentication-server kan worden geïmplementeerd met behulp van [Azure multi-factor Authentication](https://azure.microsoft.com/services/multi-factor-authentication/).|



### <a name="pci-dss-requirement-821"></a>PCI-DSS vereiste 8.2.1

**8.2.1** met sterke codering onleesbaar alle verificatiereferenties (zoals wachtwoorden/zinnen) tijdens de overdracht en opslag op alle onderdelen van het systeem.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure tot stand heeft gebracht Sleutelbeheer procedures voor het beheren van de cryptografische sleutels gedurende hun levensduur (bijv, generatie, distributie, intrekken). Microsoft Azure maakt gebruik van Microsoft zakelijke PKI-infrastructuur. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore afgedwongen sterke wachtwoorden, gedocumenteerd in de Implementatiehandleiding. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).<br /><br />|



### <a name="pci-dss-requirement-822"></a>PCI-DSS vereiste 8.2.2

**8.2.2** gebruikersidentiteit verifiëren voordat u wijzigt geen verificatiereferenties, bijvoorbeeld het uitvoeren van wachtwoorden, het inrichten van nieuwe tokens of het nieuwe sleutels genereren.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure tot stand heeft gebracht Sleutelbeheer procedures voor het beheren van de cryptografische sleutels gedurende hun levensduur (bijv, generatie, distributie, intrekken). Microsoft Azure maakt gebruik van Microsoft zakelijke PKI-infrastructuur. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore afgedwongen sterke wachtwoorden, gedocumenteerd in de Implementatiehandleiding. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).|



### <a name="pci-dss-requirement-823"></a>PCI-DSS vereiste 8.2.3

**8.2.3** wachtwoorden/wachtwoordzinnen moeten voldoen aan het volgende:
- Vereist een minimale lengte van ten minste 7 tekens.
- Numerieke en alfabetische tekens bevatten.
U kunt ook hebt de wachtwoorden/wachtwoordzinnen complexiteit en sterkte ten minste gelijk is aan de parameters die hierboven is opgegeven.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore afgedwongen sterke wachtwoorden, gedocumenteerd in de Implementatiehandleiding.|



### <a name="pci-dss-requirement-824"></a>PCI-DSS vereiste 8.2.4

**8.2.4** gebruiker wachtwoorden/wachtwoordzinnen ten minste één keer om de 90 dagen wijzigen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt gebruik van Azure Active Directory voor gebruikersbeheer. De `-enableADDomainPasswordPolicy` optie kan worden ingesteld om ervoor te zorgen wachtwoorden verlopen ten minste één keer om de 90 dagen.|



### <a name="pci-dss-requirement-825"></a>PCI-DSS vereiste 8.2.5

**8.2.5** niet toestaan dat een persoon kan het verzenden van een nieuw wachtwoord/wachtwoordzin die hetzelfde is als een van de laatste vier wachtwoorden/wachtwoordzinnen hij of zij heeft gebruikt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore afgedwongen sterke wachtwoorden, gedocumenteerd in de Implementatiehandleiding. Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).<br /><br />|



### <a name="pci-dss-requirement-826"></a>PCI-DSS vereiste 8.2.6

**8.2.6** wachtwoorden/wachtwoordzinnen voor eerste gebruik, en bij het opnieuw instellen naar een unieke waarde voor elke gebruiker instellen en wijzigen onmiddellijk na het eerste gebruik.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore afgedwongen sterke wachtwoorden, gedocumenteerd in de Implementatiehandleiding. Zie voor meer informatie [richtlijnen PCI - identiteitsbeheer](payment-processing-blueprint.md#identity-management).<br /><br />|



## <a name="pci-dss-requirement-83"></a>PCI-DSS vereiste 8.3

**8.3** secure alle afzonderlijke niet-administratieve toegang tot de console en alle externe toegang tot de kaarthouder data environment (CDE) met multi-factor authentication.

> [!NOTE]
> Multi-factor authentication-server vereist dat ten minste twee van de drie verificatiemethoden (Zie vereiste 8.2 voor beschrijvingen van verificatiemethoden) worden gebruikt voor verificatie. Gebruik een van de factoren tweemaal (bijvoorbeeld met behulp van twee afzonderlijke wachtwoorden) wordt niet beschouwd als multi-factor authentication-server.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure beheerders zijn vereist voor multi-factor authentication gebruiken voor toegang tot bij het uitvoeren van onderhoud en beheer op Azure systemen en -servers. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Multi-factor authentication-server is niet geïmplementeerd voor de demo.|



### <a name="pci-dss-requirement-831"></a>PCI-DSS vereiste 8.3.1

**8.3.1** opnemen multi-factor authentication voor alle toegang tot de niet-console in het CDE voor medewerkers met beheerdersrechten toegang.

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure beheerders zijn vereist voor multi-factor authentication gebruiken voor toegang tot bij het uitvoeren van onderhoud en beheer op Azure systemen en -servers. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Multi-factor authentication-server is niet geïmplementeerd voor de demo.|



### <a name="pci-dss-requirement-832"></a>PCI-DSS vereiste 8.3.2

**8.3.2** opnemen multi-factor authentication voor alle externe toegang tot het netwerk (gebruiker en beheerder, en met inbegrip van toegang voor ondersteuning of onderhoud van derden) die afkomstig zijn van buiten het netwerk van de entiteit.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure beheerders zijn vereist voor multi-factor authentication gebruiken voor toegang tot bij het uitvoeren van onderhoud en beheer op Azure systemen en -servers. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Multi-factor authentication-server is niet geïmplementeerd voor de demo.|



## <a name="pci-dss-requirement-84"></a>PCI-DSS vereiste 8.4

**8.4** document en communiceren authenticatiebeleid en -procedures voor alle gebruikers, met inbegrip van:
- Instructies voor het selecteren van referenties voor sterke verificatie
- Richtlijnen voor hoe gebruikers hun referenties voor verificatie moeten beveiligen
- Instructies voor het hergebruik van eerder gebruikte wachtwoorden niet
- Instructies voor het wijzigen van wachtwoorden als er vermoeden het wachtwoord kunnen worden geknoeid.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor de volgende richtlijnen en documenteren en procedures voor verificatie en het beleid voor alle gebruikers communiceren.|



## <a name="pci-dss-requirement-85"></a>PCI-DSS vereiste 8.5

**8.5** gebruik geen gedeeld, groep of algemene id's, wachtwoorden of andere verificatiemethoden te gebruiken als volgt:
- Algemene gebruikers-id's worden uitgeschakeld of verwijderd.
- Er zijn geen gedeelde gebruikers-id's voor Systeembeheer en andere essentiële functies.
- Gedeeld en algemene gebruikers-id's worden niet gebruikt voor het beheren van alle onderdelen van het systeem.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Multi-factor authentication-server is niet geïmplementeerd voor de demo.|



### <a name="pci-dss-requirement-851"></a>PCI-DSS vereiste 8.5.1

**8.5.1** **aanvullende vereisten voor alleen-providers:** serviceproviders met externe toegang tot de klant beschikt (bijvoorbeeld voor de ondersteuning van POS-systemen of -servers) moeten een unieke verificatie referentie (gebruiken zoals een wachtwoord/wachtwoordzin) voor elke klant. 

> [!NOTE]
> Deze vereiste is niet bedoeld om te passen op gedeelde hostingproviders toegang tot hun eigen hostomgeving waar meerdere klantomgevingen worden gehost.

**Verantwoordelijkheden:&nbsp;&nbsp;`Not Applicable`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing voor Microsoft Azure-klanten. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Niet van toepassing voor Microsoft Azure-klanten.|



## <a name="pci-dss-requirement-86"></a>PCI-DSS vereiste 8.6

**8.6** wanneer andere verificatiemechanismen (voor bijvoorbeeld fysieke of logische beveiligingstokens, smartcards, certificaten, enzovoort) worden gebruikt, gebruik van deze mechanismen als volgt moet worden toegewezen:
- Verificatiemechanismen moeten worden toegewezen aan een afzonderlijke account en niet gedeeld door meerdere accounts.
- Fysieke en/of logische besturingselementen moeten in plaats om te controleren of dat alleen de beoogde account kunt dit mechanisme gebruiken om toegang te krijgen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore maakt drie accounts tijdens de implementatie: admin, sqladmin, en erna (de standaardgebruiker aangemeld bij de web-app tijdens het uitvoeren van de demo). Multi-factor authentication-server is niet geïmplementeerd voor de demo. Alle gebruikers de toegang wordt beheerd via [Azure Key Vault](https://azure.microsoft.com/services/key-vault/), welke u beveiligt cryptografische sleutels en geheimen beveiligen die door cloudtoepassingen en -services. |



## <a name="pci-dss-requirement-87"></a>PCI-DSS vereiste 8,7

**8,7** alle toegang tot een database met kaarthouder gegevens (met inbegrip van toegang voor toepassingen, beheerders en alle andere gebruikers) wordt beperkt als volgt:
- Alle gebruikerstoegang tot gebruikersquery van en acties van de gebruiker op de databases zijn softwarematig.
- Databasebeheerders hebben de mogelijkheid rechtstreeks openen of databases query.
- Toepassings-id's voor databasetoepassingen kunnen alleen worden gebruikt door de toepassingen (en niet door de afzonderlijke gebruikers of andere processen die niet van toepassing).

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore beschermt alle kaarthouder gegevens met Azure Sleutelkluis en versleuteling van records wordt beschreven in de Implementatiedocumentatie van de. Zie voor meer informatie [richtlijnen PCI - versleuteling](payment-processing-blueprint.md#encryption-and-secrets-management).<br /><br />|



## <a name="pci-dss-requirement-88"></a>PCI-DSS vereiste 8,8

**8,8** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor identificatie en verificatie wordt gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het afdwingen van het beveiligingsbeleid en operationele procedures voor identificatie en verificatie wordt gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.|




