---
title: FedRAMP Azure blauwdruk Automation - identificatie en verificatie
description: Webtoepassingen voor FedRAMP - identificatie en verificatie
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 1033f63f-daf0-4174-a7f6-7b0f569020e2
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 5ceac04f3f5c58ac5810dda6c99b40ebc8ad736f
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="identification-and-authentication-ia"></a>Identificatie en verificatie (IA)

## <a name="nist-800-53-control-ia-1"></a>NIST 800 53 besturingselement IA-1

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

#### <a name="identification-and-authentication-policy-and-procedures"></a>Identificatie en verificatiebeleid en Procedures

**IA-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een beleid voor identificatie en verificatie dat doel, bereik, rollen, taken, management-adressen het streven, samenwerking tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van de identificatie en verificatiebeleid en bijbehorende identificatie en verificatie controles; en controleert en updates van het huidige beleid voor identificatie en verificatie [toewijzing: organisatie gedefinieerde frequentie]; en -procedures voor identificatie en verificatie [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De identificatie en verificatie beleid op bedrijfsniveau en -procedures van de klant mogelijk voldoende zijn voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-2"></a>NIST 800 53 besturingselement IA-2

#### <a name="identification-and-authentication-organizational-users"></a>Identificatie en verificatie (organisatie gebruikers)

**IA-2** uniek wordt aangeduid en organisatie-gebruikers (of processen fungeert namens gebruikers organisatie) verifieert het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Accounts die zijn gemaakt door deze Azure blauwdruk hebben unieke id's. Ingebouwde accounts met niet-unieke id's worden uitgeschakeld of verwijderd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-1"></a>NIST 800 53 besturingselement IA-2 (1)

#### <a name="identification-and-authentication-organizational-users--network-access-to-privileged-accounts"></a>Identificatie en verificatie (organisatie gebruikers) | Netwerktoegang tot beschermde Accounts

**IA-2 (1)** meervoudige authenticatie voor netwerktoegang tot beschermde accounts door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het implementeren van meervoudige authenticatie voor netwerktoegang tot beschermde accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-2"></a>NIST 800 53 besturingselement IA-2 (2)

#### <a name="identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts"></a>Identificatie en verificatie (organisatie gebruikers) | Toegang tot het netwerk naar niet-gemachtigde Accounts

**IA-2 (2)** meervoudige authenticatie voor netwerktoegang tot niet-gemachtigde accounts door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het implementeren van meervoudige authenticatie voor netwerktoegang tot niet-gemachtigde accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-3"></a>NIST 800 53 besturingselement IA-2 (3)

#### <a name="identification-and-authentication-organizational-users--local-access-to-privileged-accounts"></a>Identificatie en verificatie (organisatie gebruikers) | Lokale toegang tot beschermde Accounts

**IA-2 (3)** multifactor-verificatie voor lokale toegang tot beschermde accounts door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klant hoeft geen lokale toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure mag geen lokale toegang, tenzij de fysieke toegang is vereist. Lokale beheerder toegang moet alleen worden gebruikt voor het oplossen van problemen in gevallen waar de lidserver netwerkproblemen ondervindt en domeinverificatie werkt niet. <br /> Azure multi-factor authentication voor lokale toegang via Besturingsmechanismen toegang nodig is voor de fysieke toegang tot de omgeving implementeert. Binnen de Azure-datacenters die alle Azure-infrastructuur systemen binnen de grenzen van het systeem bevatten zijn beperkt tot en met verschillende fysieke beveiligingsmechanismen, met inbegrip van de vereiste voor zakelijke smartcard badging toegang en biometrische apparaten. Beide soorten verificatie zijn vereist voor de fysieke toegang op het punt inkomende gegevens naar Azure-datacenter colocations. |


 ### <a name="nist-800-53-control-ia-2-4"></a>NIST 800 53 besturingselement IA-2 (4)

#### <a name="identification-and-authentication-organizational-users--local-access-to-non-privileged-accounts"></a>Identificatie en verificatie (organisatie gebruikers) | Lokale toegang tot niet-gemachtigde Accounts

**IA-2 (4)** multifactor-verificatie voor lokale toegang tot niet-gemachtigde accounts door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klant hoeft geen lokale toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure beschouwt alle Microsoft Azure Government accounts die worden gebruikt door medewerkers van Microsoft Azure Government als bevoegdheden. Multifactor-verificatie is geïmplementeerd voor alle Microsoft Azure Government personeel accounts met smartcards en pincodes, waaronder lokale toegang. |


 ### <a name="nist-800-53-control-ia-2-5"></a>NIST 800 53 besturingselement IA-2 (5)

#### <a name="identification-and-authentication-organizational-users--group-authentication"></a>Identificatie en verificatie (organisatie gebruikers) | Groepsverificatie

**IA-2 (5)** de organisatie personen kunnen worden geverifieerd met een afzonderlijke verificator wanneer een groep verificator dienst is vereist.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen gedeelde of groep accounts zijn ingeschakeld op de resources die zijn geïmplementeerd door deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-8"></a>NIST 800 53 besturingselement IA-2 (8)

#### <a name="identification-and-authentication-organizational-users--network-access-to-privileged-accounts---replay-resistant"></a>Identificatie en verificatie (organisatie gebruikers) | Netwerktoegang tot beschermde Accounts - bestand zijn tegen opnieuw afspelen

**IA-2 (8)** replay-mailbeveiliging verificatiemechanismen voor netwerktoegang tot beschermde accounts door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Toegang tot resources die zijn geïmplementeerd door deze blauwdruk Azure is beveiligd tegen replay-aanvallen door de ingebouwde Kerberos-functionaliteit van Azure Active Directory, Active Directory en de Windows-besturingssysteem. In Kerberos-verificatie bevat de verificator verzonden door de client bijkomende gegevens, zoals een lijst met versleutelde IP-, client tijdstempels en ticket levensduur. Als een pakket opnieuw en nu afgespeeld, wordt de tijdstempel gecontroleerd. Als de tijdstempel lager dan is of gelijk zijn aan een eerdere verificator, het pakket is afgewezen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-9"></a>NIST 800 53 besturingselement IA-2 (9)

#### <a name="identification-and-authentication-organizational-users--network-access-to-non-privileged-accounts---replay-resistant"></a>Identificatie en verificatie (organisatie gebruikers) | Netwerktoegang tot onbevoegde-Accounts: mailbeveiliging opnieuw afspelen

**IA-2 (9)** replay-mailbeveiliging verificatiemechanismen voor netwerktoegang tot niet-gemachtigde accounts door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Toegang tot resources die zijn geïmplementeerd door deze blauwdruk Azure is beveiligd tegen replay-aanvallen door de ingebouwde Kerberos-functionaliteit van Azure Active Directory, Active Directory en de Windows-besturingssysteem. In Kerberos-verificatie bevat de verificator verzonden door de client bijkomende gegevens, zoals een lijst met versleutelde IP-, client tijdstempels en ticket levensduur. Als een pakket opnieuw en nu afgespeeld, wordt de tijdstempel gecontroleerd. Als de tijdstempel lager dan is of gelijk zijn aan een eerdere verificator, het pakket is afgewezen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-11"></a>NIST 800 53 besturingselement IA-2 (11)

#### <a name="identification-and-authentication-organizational-users--remote-access----separate-device"></a>Identificatie en verificatie (organisatie gebruikers) | Externe toegang - afzonderlijk apparaat

**IA-2 (11)** multifactor-verificatie voor externe toegang tot beschermde door het informatiesysteem wordt geïmplementeerd en niet-bevoegde accounts zodanig zijn dat een van de factoren wordt aangeboden door een apparaat dat los van het systeem toegang en het apparaat voldoet aan [ Toewijzing: organisatie gedefinieerd sterkte van mechanisme vereisten].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het implementeren van meervoudige verificatie op afstand toegang krijgen tot bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-2-12"></a>NIST 800 53 besturingselement IA-2 (12)

#### <a name="identification-and-authentication-organizational-users--acceptance-of-piv-credentials"></a>Identificatie en verificatie (organisatie gebruikers) | Acceptatie van Piv referenties

**IA-2 (12)** het systeem gegevens accepteert en elektronisch Personal Identity Verification (PIV) referenties worden geverifieerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het accepteren en verifiëren van referenties Personal Identity Verification (PIV). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-3"></a>NIST 800 53 besturingselement IA-3

#### <a name="device-identification-and-authentication"></a>Apparaat-identificatie en verificatie

**IA-3** uniek wordt aangeduid en verifieert het informatiesysteem [toewijzing: specifieke organisatie gedefinieerd en/of de typen apparaten] voordat er een [selectie (een of meer): lokale; extern; netwerk] verbinding.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het implementeren van apparaat identificatie en verificatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-4a"></a>NIST 800 53 besturingselement IA-4.

#### <a name="identifier-management"></a>Id-beheer

**IA-4.** de organisatie beheert systeem-id's van gegevens door te ontvangen van de autorisatie van [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] toewijzen van een persoon, groep of rol apparaat-id.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van id's (dat wil zeggen, personen, groepen, functies en -apparaten) voor resources van de klant. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-4b"></a>NIST 800 53 besturingselement IA-4.b

#### <a name="identifier-management"></a>Id-beheer

**IA-4.b** de organisatie beheert informatie systeem-id's door het selecteren van een id die een persoon identificeert groep, rol of apparaat.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk wordt gevraagd tijdens de implementatie voor de klant opgegeven id's voor individuele accounts.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-4c"></a>NIST 800 53 besturingselement IA-4.c

#### <a name="identifier-management"></a>Id-beheer

**IA-4.c** de organisatie beheert informatie systeem-id's door de id toewijzen aan de beoogde persoon, groep, rol of het apparaat.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van id's (dat wil zeggen, personen, groepen, functies en -apparaten) voor resources van de klant. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-4d"></a>NIST 800 53 besturingselement IA-4.d

#### <a name="identifier-management"></a>Id-beheer

**IA-4.d** de organisatie beheert systeem-id's van gegevens door te voorkomen dat hergebruik van id's voor [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Active Directory en lokale accounts voor Windows-besturingssysteem zijn een unieke beveiligings-id (SID) toegewezen. Azure Active Directory-accounts zijn toegewezen een globaal unieke Object-ID. Deze unieke id's zijn niet hergebruikt. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-4e"></a>NIST 800 53 besturingselement IA-4.e

#### <a name="identifier-management"></a>Id-beheer

**IA-4.e** De organisatie beheert informatie systeem-id's door het uitschakelen van de id na [toewijzing: organisatie gedefinieerde periode van inactiviteit].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert een geplande taak voor Active Directory accounts automatisch uitschakelen na 35 dagen inactief. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-4-4"></a>NIST 800 53 besturingselement IA-4 (4)

#### <a name="identifier-management--identify-user-status"></a>ID Management | Identificeren van de gebruikersstatus

**IA-4 (4)** de organisatie beheert individuele id's door elke persoon afzonderlijk als een unieke id [toewijzing: organisatie gedefinieerd kenmerk status van afzonderlijke identificeren].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure Active Directory en Active Directory-ondersteuning die aangeeft aannemers en leveranciers andere gebruikerstypen met behulp van naamgevingsregels toegepast op de id. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5a"></a>NIST 800 53 besturingselement IA-5.

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.** de organisatie beheert informatie system verificators door te controleren als onderdeel van de eerste verificator distributie, de identiteit van de persoon, groep, rol of apparaat ontvangt de verificator.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van verificators. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5b"></a>NIST 800 53 besturingselement IA-5.b

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.b** de organisatie beheert informatie system verificators door het opstellen van de inhoud van de eerste verificator voor verificators gedefinieerd door de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Alle inhoud van de eerste verificator voor accounts die zijn gemaakt door deze blauwdruk Azure voldoen aan de vereisten vermeld in de IA-5 (1) geverifieerd wanneer opgegeven door de klant tijdens de implementatie.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5c"></a>NIST 800 53 besturingselement IA-5.c

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.c** de organisatie beheert informatie system verificators door ervoor te zorgen dat verificators voldoende sterkte van het mechanisme voor het beoogde gebruik hebben.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Verificators gebruikt door deze voldoen aan vereisten voor Azure blauwdruk voor sterkte door FedRAMP vereist. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5d"></a>NIST 800 53 besturingselement IA-5.d

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.d** de organisatie beheert informatie system verificators door tot stand brengen en implementeren van administratieve procedures voor initiële verificator distributie, verificators/geknoeid of is beschadigd en voor het intrekken verificators.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van verificators. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5e"></a>NIST 800 53 besturingselement IA-5.e

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.e** De organisatie beheert informatie system verificators door het wijzigen van de Standaardinhoud van verificators vóór de installatie van het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Alle verificators voor de onderdelen van deze Azure blauwdruk zijn standaardwaarden gewijzigd. Verificators zijn klant opgegeven tijdens de implementatie van deze oplossing. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5f"></a>NIST 800 53 besturingselement IA-5.f

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.f** de organisatie beheert informatie system verificators door het opstellen van beperkingen van de minimale en maximale levensduur en hergebruik voorwaarden voor verificators.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van verificators. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5g"></a>NIST 800 53 besturingselement IA-5.g

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.g** de organisatie beheert informatie system verificators door het wijzigen van/vernieuwen verificators [toewijzing: organisatie gedefinieerde periode door type verificator].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Groepsbeleid is ingesteld en geconfigureerd voor het implementeren van de beperkingen van de levensduur wachtwoord (60 dagen). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5h"></a>NIST 800 53 besturingselement IA-5.h

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.h** de organisatie informatie system verificators door de verificator inhoud beveiligen tegen ongeautoriseerde bekendmaking en de wijzigingsopties beheert.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert Key Vault authenticator om inhoud te beveiligen tegen ongeautoriseerde bekendmaking en wijziging. De volgende verificators worden opgeslagen in de Sleutelkluis: Azure-wachtwoord voor account implementeren, administrator-wachtwoord voor virtuele machine, het wachtwoord van SQL Server-serviceaccount. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5i"></a>NIST 800 53 besturingselement IA-5.i

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.i** de organisatie beheert informatie system verificators doordat personen te nemen en met apparaten implementeren, specifieke veiligheidsmaatregelen ter bescherming van verificators.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert Key Vault authenticator om inhoud te beveiligen tegen ongeautoriseerde bekendmaking en wijziging. De volgende verificators worden opgeslagen in de Sleutelkluis: Azure-wachtwoord voor account implementeren, administrator-wachtwoord voor virtuele machine, het wachtwoord van SQL Server-serviceaccount. Sleutelkluis versleutelt sleutels en geheimen (zoals verificatiesleutels, opslagaccountsleutels, gegevensversleutelingssleutels en wachtwoorden) met behulp van de sleutels die worden beveiligd door hardware security modules (HSM's). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-5j"></a>NIST 800 53 besturingselement IA-5.j

#### <a name="authenticator-management"></a>Verificator Management

**IA-5.j** de organisatie beheert informatie system verificators door verificators voor groep/rol accounts te wijzigen wanneer het lidmaatschap van die accounts wijzigingen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen gedeelde of groep accounts zijn ingeschakeld op de resources die zijn geïmplementeerd door deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-1a"></a>NIST 800 53 besturingselement IA-5 (1) een

#### <a name="authenticator-management--password-based-authentication"></a>Verificator Management | Verificatie op basis van wachtwoorden

**IA-5 (1) een** het informatiesysteem voor verificatie op basis van wachtwoorden worden afgedwongen minimale wachtwoordcomplexiteit van [toewijzing: vereisten voor de hoofdlettergevoeligheid, aantal tekens, organisatie gedefinieerd mix van hoofdletters, kleine letters bestaan letters, cijfers en speciale tekens, met inbegrip van de minimale vereisten voor elk type].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Groepsbeleid is ingesteld en geconfigureerd voor het afdwingen van de vereisten voor wachtwoordcomplexiteit voor lokale accounts van virtuele machine en de AD-accounts.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-1b"></a>NIST 800 53 besturingselement IA-5 (1) .b

#### <a name="authenticator-management--password-based-authentication"></a>Verificator Management | Verificatie op basis van wachtwoorden

**IA-5 (1) .b** het informatiesysteem voor verificatie op basis van wachtwoorden worden afgedwongen ten minste het volgende aantal gewijzigde tekens wanneer nieuwe wachtwoorden worden gemaakt: [toewijzing: nummer organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor die gebruikmaakt van verificatie op basis van wachtwoorden binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-1c"></a>.C van NIST 800 53 besturingselement IA-5 (1)

#### <a name="authenticator-management--password-based-authentication"></a>Verificator Management | Verificatie op basis van wachtwoorden

**IA-5 (1) .c** het informatiesysteem voor verificatie op basis van wachtwoorden worden opgeslagen en worden alleen cryptografisch beveiligde wachtwoorden verzonden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure-map wordt gebruikt om ervoor te zorgen dat alle wachtwoorden cryptografisch zijn beveiligd terwijl opgeslagen en verzonden. Wachtwoorden worden opgeslagen door Active Directory en lokaal in de geïmplementeerde virtuele machines van Windows worden automatisch opgedeeld als onderdeel van de ingebouwde beveiligingsfuncties. Van extern bureaublad-verificatiesessies worden beveiligd via TLS om ervoor te zorgen verificators zijn beveiligd wanneer verzonden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-1d"></a>.D van NIST 800 53 besturingselement IA-5 (1)

#### <a name="authenticator-management--password-based-authentication"></a>Verificator Management | Verificatie op basis van wachtwoorden

**IA-5 (1) .d** wachtwoord minimum- en de beperkingen van de maximale levensduur van het informatiesysteem voor verificatie op basis van een wachtwoord wordt afgedwongen [toewijzing: getallen organisatie gedefinieerd voor de levensduur van minimum, maximum voor levensduur].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Groepsbeleid is gemaakt en geconfigureerd voor het afdwingen van beperkingen voor wachtwoorden die (1 dag) een minimale maximale (60 dagen) levensduur en beperkingen voor lokale accounts en AD-accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-1e"></a>CF,o.e van NIST 800 53 besturingselement IA-5 (1)

#### <a name="authenticator-management--password-based-authentication"></a>Verificator Management | Verificatie op basis van wachtwoorden

**CF,o.e IA-5 (1)** Het informatiesysteem voor verificatie op basis van wachtwoorden verbiedt het wachtwoord opnieuw kunnen worden gebruikt voor [toewijzing: nummer organisatie gedefinieerd] generaties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Groepsbeleid is ingesteld en geconfigureerd voor het afdwingen van beperkingen voor hergebruik voorwaarden (24 wachtwoorden) voor lokale accounts en AD-accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-1f"></a>.F van NIST 800 53 besturingselement IA-5 (1)

#### <a name="authenticator-management--password-based-authentication"></a>Verificator Management | Verificatie op basis van wachtwoorden

**IA-5 (1) .f** het informatiesysteem voor verificatie op basis van wachtwoorden staat het gebruik van een tijdelijk wachtwoord voor aanmelding bij systeem met een onmiddellijke wijziging van een permanente wachtwoord.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure Active Directory wordt gebruikt voor het beheren van toegang tot de gegevens-systeem beheren. Wanneer een account in eerste instantie is gemaakt of een tijdelijk wachtwoord wordt gegenereerd, is Azure Active Directory gebruikt, om te vereisen dat de gebruiker het wachtwoord bij volgende aanmelding wijzigen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-2a"></a>NIST 800 53 besturingselement IA-5 (2) een

#### <a name="authenticator-management--pki-based-authentication"></a>Verificator Management | Verificatie op basis van PKI

**IA-5 (2) een** het informatiesysteem voor verificatie op basis van een PKI valideert certificeringen door te maken en verifiëren van een certificeringsinstantie pad naar een geaccepteerde vertrouwensanker, inclusief het controleren van de certificaatstatus.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor die gebruikmaakt van verificatie op basis van een PKI binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-2b"></a>NIST 800 53 besturingselement IA-5 (2) .b

#### <a name="authenticator-management--pki-based-authentication"></a>Verificator Management | Verificatie op basis van PKI

**IA-5 (2) .b** het informatiesysteem voor verificatie op basis van een PKI wordt afgedwongen gemachtigde gebruikers toegang hebben tot de bijbehorende persoonlijke sleutel.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor die gebruikmaakt van verificatie op basis van een PKI binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-2c"></a>NIST 800 53 besturingselement IA-5 (2) .c

#### <a name="authenticator-management--pki-based-authentication"></a>Verificator Management | Verificatie op basis van PKI

**IA-5 (2) .c** het informatiesysteem voor verificatie op basis van een PKI wordt de identiteit van de geverifieerde toegewezen aan het account van de persoon of groep.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor die gebruikmaakt van verificatie op basis van een PKI binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-2d"></a>NIST 800 53 besturingselement IA-5 (2) .d

#### <a name="authenticator-management--pki-based-authentication"></a>Verificator Management | Verificatie op basis van PKI

**IA-5 (2) .d** het informatiesysteem voor verificatie op basis van een PKI implementeert een lokale cache van intrekkingsgegevens ter ondersteuning van pad detectie en validatie in geval van niet meer toegankelijk intrekkingsgegevens via het netwerk.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor die gebruikmaakt van verificatie op basis van een PKI binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-3"></a>NIST 800 53 besturingselement IA-5 (3)

#### <a name="authenticator-management--in-person-or-trusted-third-party-registration"></a>Verificator Management | in de persoon of vertrouwde derde-registratie

**IA-5 (3)** de organisatie vereist dat de registratie voor het ontvangen van verwerken [toewijzing: typen organisatie gedefinieerd en/of specifieke verificators] uit te voeren [selectie: persoonlijk; door een vertrouwde derde] voordat [toewijzing: organisatie gedefinieerd registratieautoriteit] met toestemming van [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het registreren van verificators. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-4"></a>NIST 800 53 besturingselement IA-5 (4)

#### <a name="authenticator-management--automated-support--for-password-strength-determination"></a>Verificator Management | Automatische ondersteuning voor wachtwoord sterkte bepalen

**IA-5 (4)** geautomatiseerde hulpprogramma's om te bepalen of wachtwoord verificators voldoende sterk om te voldoen aan de veiligheidsmaatregelen voor de organisatie [toewijzing: vereisten organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Gebruikersaccounts die zijn geïmplementeerd met deze blauwdruk Azure opnemen AD en lokale gebruikersaccounts. Deze biedt methoden die naleving tot stand gebrachte wachtwoordvereisten om te kunnen maken van een eerste wachtwoord en tijdens het wijzigen van wachtwoorden forceren. Azure Active Directory is het geautomatiseerde hulpprogramma ingezet om te bepalen of wachtwoord verificators voldoende sterk om te voldoen aan de wachtwoordlengte, complexiteit, draaiing en beperkingen van de levensduur van tot stand gebracht in IA-5 (1). Azure Active Directory zorgt ervoor dat de verificator Wachtwoordsterkte bij het maken van voldoet aan deze normen wordt voldaan. De klant gespecificeerde wachtwoorden gebruikt voor het implementeren van deze oplossing worden gecontroleerd om te voldoen aan vereisten voor Wachtwoordsterkte. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-6"></a>NIST 800 53 besturingselement IA-5 (6)

#### <a name="authenticator-management--protection-of-authenticators"></a>Verificator Management | Beveiliging van verificators

**IA-5 (6)** verificators overeenstemming met de categorie van de beveiliging van de gegevens waarvoor gebruik van de verificator toegang kunnen worden beschermd door de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beveiligen van verificaties. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-7"></a>NIST 800 53 besturingselement IA-5 (7)

#### <a name="authenticator-management--no-embedded-unencrypted-static-authenticators"></a>Verificator Management | Er zijn geen ingesloten niet-versleutelde statische verificators

**IA-5 (7)** de organisatie zorgt ervoor dat de niet-versleutelde statische verificators niet zijn ingesloten in toepassingen of scripts toegang of op de functietoetsen opgeslagen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er is geen gebruik van niet-versleutelde statische verificators ingesloten in toepassingen, scripts openen of functietoetsen geïmplementeerd door deze blauwdruk Azure. Een script of een toepassing die gebruikmaakt van een verificator wordt een aanroep van een Azure Sleutelkluis-container vóór elk gebruik. Toegang tot Azure Key Vault containers wordt gecontroleerd, kunnen de detectie van schending van deze verboden als een service-account wordt gebruikt voor toegang tot een systeem zonder een bijbehorende aanroep naar de Azure Sleutelkluis-container. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-8"></a>NIST 800 53 besturingselement IA-5 (8)

#### <a name="authenticator-management--multiple-information-system-accounts"></a>Verificator Management | Meerdere informatie systeemaccounts

**IA-5 (8)** de organisatie implementeert [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] voor het beheren van het risico op inbreuk als gevolg van mensen met accounts op meerdere systemen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant mogelijk afhankelijk van bedrijfsniveau beveiligingsmaatregelen voor het beheren van risico dat samenhangt met mensen met accounts op meerdere systemen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-11"></a>NIST 800 53 besturingselement IA-5 (11)

#### <a name="authenticator-management--hardware-token-based-authentication"></a>Verificator Management | Hardware op tokens gebaseerde verificatie

**IA-5 (11)** mechanismen die voldoen aan de veiligheidsmaatregelen voor het informatiesysteem voor hardware-authenticatie op basis van tokens, [toewijzing: token kwaliteitsvereisten organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de toepassing mechanismen om te voldoen aan de hardwarevereisten voor verificatie op basis van tokens kwaliteit. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-5-13"></a>NIST 800 53 besturingselement IA-5 (13)

#### <a name="authenticator-management--expiration-of-cached-authenticators"></a>Verificator Management | Vervaldatum van verificaties in cache

**IA-5 (13)** informatie mogen niet worden gebruikt in de cache verificators na [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen resources geïmplementeerd door deze blauwdruk Azure zijn geconfigureerd voor het gebruik van cache verificators toestaan. Verificatie van de geïmplementeerde virtuele machines is vereist dat een verificator wordt ingevoerd op het moment van verificatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-6"></a>NIST 800 53 besturingselement IA-6

#### <a name="authenticator-feedback"></a>Verificator Feedback

**IA-6** het informatiesysteem geven feedback van verificatiegegevens tijdens het verificatieproces de om informatie te beschermen tegen mogelijke misbruik/gebruik door onbevoegde gebruikers.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Toegang tot resources die zijn geïmplementeerd door deze blauwdruk Azure via Extern bureaublad is en is afhankelijk van de Windows-verificatie. Het standaardgedrag van Windows-verificatie-sessies maskeert wachtwoorden wanneer invoer tijdens een verificatiesessie van.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-7"></a>NIST 800 53 besturingselement IA-7

#### <a name="cryptographic-module-authentication"></a>Verificatie van de cryptografische Module

**IA-7** mechanismen voor verificatie van een cryptografische module die voldoen aan de vereisten van federale wetgeving, Executive Orders richtlijnen, beleid, voorschriften, standaarden en richtlijnen voor zoals door het informatiesysteem wordt geïmplementeerd -verificatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Windows-verificatie, extern bureaublad en BitLocker worden gebruikt door deze blauwdruk Azure. Deze onderdelen kunnen zijn afhankelijk van FIPS 140 gevalideerd cryptografiemodules worden geconfigureerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ia-8"></a>NIST 800 53 besturingselement IA-8

#### <a name="identification-and-authentication-non-organizational-users"></a>Identificatie en verificatie (niet-organisatie gebruikers)

**IA-8** het informatiesysteem uniek wordt aangeduid en verifieert niet van de organisatie-gebruikers (of processen fungeert namens gebruikers niet-organisatie).

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het identificeren en verifiëren van niet-organisatie gebruikers toegang krijgen tot bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-8-1"></a>NIST 800 53 besturingselement IA-8 (1)

#### <a name="identification-and-authentication-non-organizational-users--acceptance-of-piv-credentials-from-other-agencies"></a>Identificatie en verificatie (niet-organisatie gebruikers) | Acceptatie van Piv referenties van andere instanties

**IA-8 (1)** het informatiesysteem accepteert en referenties van de Personal Identity Verification (PIV) van andere federale instanties elektronisch worden geverifieerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het accepteren en verifiëren van Personal Identity Verification (PIV)-referenties die zijn uitgegeven door een andere federale instanties. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-8-2"></a>NIST 800 53 besturingselement IA-8 (2)

#### <a name="identification-and-authentication-non-organizational-users--acceptance-of-third-party-credentials"></a>Identificatie en verificatie (niet-organisatie gebruikers) | Acceptatie van de referenties van derden

**IA-8 (2)** het systeem gegevens accepteert alleen FICAM goedgekeurde van derden referenties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het accepteren van alleen derden referenties die zijn goedgekeurd door het initiatief Federal identiteit, referentie en Access Management (FICAM) vertrouwen Framework-oplossingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-8-3"></a>NIST 800 53 besturingselement IA-8 (3)

#### <a name="identification-and-authentication-non-organizational-users--use-of-ficam-approved-products"></a>Identificatie en verificatie (niet-organisatie gebruikers) | Gebruik van producten Ficam goedgekeurd

**IA-8 (3)** alleen informatie FICAM goedgekeurd onderdelen van het systeem in de veiligheidsmaatregelen voor de organisatie [toewijzing: organisatie gedefinieerd informatiesystemen] te accepteren van derden referenties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het alleen Federal identiteit, referentie, maar en Access Management (FICAM) vertrouwen Framework oplossingen initiatief goedgekeurd bronnen voor het accepteren van de referenties van derden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ia-8-4"></a>NIST 800 53 besturingselement IA-8 (4)

#### <a name="identification-and-authentication-non-organizational-users--use-of-ficam-issued-profiles"></a>Identificatie en verificatie (niet-organisatie gebruikers) | Gebruik van Ficam verleende profielen

**IA-8 (4)** het informatiesysteem voldoet aan de FICAM verleende profielen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor die voldoet aan de profielen die zijn uitgegeven door het initiatief Federal identiteit, referentie en Access Management (FICAM) vertrouwen Framework-oplossingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |
