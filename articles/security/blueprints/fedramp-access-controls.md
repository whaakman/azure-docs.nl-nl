---
title: FedRAMP Azure blauwdruk Automation - toegangsbeheer
description: Webtoepassingen voor FedRAMP - toegangsbeheer
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: f7e6cd8f-b2df-4db6-8332-de97d86c5281
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: a7a840b338af3aa6c6a71f2db3cff1e36a8a6794
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="access-control-ac"></a>Toegangsbeheer (AC)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-ac-1"></a>NIST 800 53 besturingselement AC-1

#### <a name="access-control-policy-and-procedures"></a>Beleid voor toegangsbeheer en Procedures

**AC-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een beleid voor toegangsbeheer die zijn gericht op doel, bereik, rollen, verantwoordelijkheden, management streven, coördinatie tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van het beleid voor toegangsbeheer en de bijbehorende toegangsbeheer; en controleert en updates van het huidige beleid voor toegangsbeheer [toewijzing: organisatie gedefinieerde frequentie]; en toegang tot de controleprocedures [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Het beleid voor toegangsbeheer op bedrijfsniveau en de procedures van de klant mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2a"></a>NIST 800 53 besturingselement AC-2.

#### <a name="account-management"></a>Accountbeheer

**AC 2.** de organisatie identificeert en selecteert u de volgende soorten informatie systeemaccounts ter ondersteuning van de organisatie missies/bedrijfsfuncties: [toewijzing: organisatie gedefinieerde informatie system accounttypen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk is afhankelijk van en implementeert u de volgende typen van de systeem-account: Azure Active Directory-gebruikers (gebruikt de oplossing implementeren en beheren van toegang tot Azure-resources), Windows-besturingssysteem gebruikers (beheerd door Active Directory), SQL Server-serviceaccount. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2b"></a>NIST 800 53 besturingselement AC-2.b

#### <a name="account-management"></a>Accountbeheer

**AC 2.b** accountmanagers voor systeemaccounts informatie door de organisatie wordt toegewezen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het toewijzen van managers aan de accounts in AC 02.a geïdentificeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2c"></a>NIST 800 53 besturingselement AC-2.c

#### <a name="account-management"></a>Accountbeheer

**AC 2.c** voorwaarden van de organisatie voor lidmaatschap van groep en de rol.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het tot stand brengen van functies en groepen lidmaatschapscriteria voor typen van de klant beheerde accounts (Zie AC 02.a). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2d"></a>NIST 800 53 besturingselement AC-2.d

#### <a name="account-management"></a>Accountbeheer

**AC 2.d** de organisatie bevat geautoriseerde gebruikers van de informatie, lidmaatschap van groep en de rol, en toegang autorisaties (dat wil zeggen, bevoegdheden) en andere kenmerken (indien vereist) voor elk account.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een autorisatieproces tot stand gebrachte op bedrijfsniveau-account. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2e"></a>NIST 800 53 besturingselement AC-2.e

#### <a name="account-management"></a>Accountbeheer

**AC 2.e** De organisatie vereist goedkeuringen door [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] voor aanvragen voor gegevens systeemaccounts maken.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een autorisatieproces tot stand gebrachte op bedrijfsniveau-account. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2f"></a>NIST 800 53 besturingselement AC-2.f

#### <a name="account-management"></a>Accountbeheer

**AC 2.f** de organisatie maakt, kunt wijzigt, wordt uitgeschakeld en verwijdert u de informatie systeemaccounts in overeenstemming met [toewijzing: organisatie gedefinieerde procedures of voorwaarden].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant kan afhankelijk zijn van een beheerproces tot stand gebrachte op bedrijfsniveau-account. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2g"></a>NIST 800 53 besturingselement AC-2.g

#### <a name="account-management"></a>Accountbeheer

**AC 2.g** de organisatie bewaakt het gebruik van informatie systeemaccounts.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk implementeert OMS beveiligings- en controle van de oplossing identiteits- en toegangsbeheer dashboard. Dit dashboard kan accountmanagers voor het gebruik van informatie systeemaccounts bewaken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2h"></a>NIST 800 53 besturingselement AC-2.h

#### <a name="account-management"></a>Accountbeheer

**AC 2.h** de organisatie accountmanagers waarschuwt wanneer accounts zijn niet langer vereist; wanneer gebruikers worden beëindigd of overgedragen; en het gebruik van informatie over de afzonderlijke of wijzigingen nodig om te weten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures van de controle van de klant op bedrijfsniveau toegang mogelijk tot stand brengen een proces voor de juiste accountmanager melden wanneer een account is niet meer nodig. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2i"></a>NIST 800 53 besturingselement AC-2.i

#### <a name="account-management"></a>Accountbeheer

**AC 2.i** de organisatie gemachtigd voor toegang tot het informatiesysteem op basis van een geldige autorisatie; beoogde systeemgebruik; en andere kenmerken zoals vereist door de organisatie of de bijbehorende missies/bedrijfsfuncties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures voor toegang op bedrijfsniveau controle van de klant kunnen een autorisatieproces toegang vaststellen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2j"></a>NIST 800 53 besturingselement AC-2.j

#### <a name="account-management"></a>Accountbeheer

**AC 2.j** de organisatie beoordeelt accounts om te voldoen aan de vereisten voor het beheer van account [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen door de klant bewaakte accounts met de vereiste frequentie om te bepalen of de accounts die compatibel zijn met alle vereisten van de organisatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-2k"></a>NIST 800 53 besturingselement AC-2.k

#### <a name="account-management"></a>Accountbeheer

**AC 2.k** de organisatie stelt een proces voor het opnieuw uitgeven van gedeelde of groep accountreferenties (indien geïmplementeerd) wanneer personen zijn verwijderd uit de groep.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures voor toegang op bedrijfsniveau controle van de klant kunnen een proces voor het beheren van accountreferenties groep vaststellen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-1"></a>NIST 800 53 besturingselement AC-2 (1)

#### <a name="account-management--automated-system-account-management"></a>Accountbeheer | Automated System accountbeheer

**AC-2 (1)** geautomatiseerde mechanismen ter ondersteuning van het beheer van informatie systeemaccounts maakt gebruik van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk implementeert OMS beveiligings- en controle van de oplossing identiteits- en toegangsbeheer dashboard. Dit dashboard inschakelen accountmanagers voor het gebruik van informatie systeemaccounts bewaken. OMS kunnen worden geconfigureerd voor het verzenden van meldingen wanneer ongewone activiteit wordt vermoed of andere vooraf gedefinieerde gebeurtenissen plaatsvinden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-2"></a>NIST 800 53 besturingselement AC-2 (2)

#### <a name="account-management--removal-of-temporary--emergency-accounts"></a>Accountbeheer | Verwijderen van de tijdelijke / noodgevallen Accounts

**AC-2 (2)** het informatiesysteem automatisch [selectie: Hiermee verwijdert u; uitgeschakeld] tijdelijk en noodgevallen accounts na [toewijzing: organisatie gedefinieerde periode voor elk type account].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert geen tijdelijke of noodgevallen accounts. Als u niet handmatig is uitgeschakeld, wordt alle inactieve accounts automatisch uitgeschakeld met de geïmplementeerde domeincontroller na 35 dagen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-3"></a>NIST 800 53 besturingselement AC-2 (3)

#### <a name="account-management--disable-inactive-accounts"></a>Accountbeheer | Inactieve Accounts uitschakelen

**AC-2 (3)** wordt de informatie automatisch uitgeschakeld inactieve accounts na [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De domeincontroller die is geïmplementeerd door deze blauwdruk Azure is geconfigureerd voor het uitschakelen van alle gebruikersaccounts na 35 dagen inactief. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-4"></a>NIST 800 53 besturingselement AC-2 (4)

#### <a name="account-management--automated-audit-actions"></a>Accountbeheer | Geautomatiseerde Auditacties

**AC-2 (4)** automatisch acties voor het maken, wijzigen, inschakelen, uitschakelen en verwijdering van account controles en ontvangt een melding, het informatiesysteem [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de volgende typen van de systeem-account: Azure Active Directory-gebruikers, gebruikers van Windows-besturingssysteem, SQL Server-serviceaccount. Azure Active Directory-account acties genereren van een gebeurtenis in het Azure activiteitenlogboek; Acties voor OS-niveau account genereren een gebeurtenis in het systeemlogboek. Deze logboeken die door logboekanalyse verzameld en opgeslagen in de OMS-opslagplaats. OMS kunnen worden geconfigureerd voor het verzenden van meldingen wanneer vooraf gedefinieerde gebeurtenissen plaatsvinden.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-5"></a>NIST 800 53 besturingselement AC-2 (5)

#### <a name="account-management--inactivity-logout"></a>Accountbeheer | Inactiviteit afmelden

**AC-2 (5)** de organisatie vereist dat gebruikers zich afmeldt wanneer [toewijzing: organisatie gedefinieerde-periode van inactiviteit verwachte of beschrijving van wanneer u zich afmeldt].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant kan een beleid dat gebruikers zich afmeldt wanneer ze niet actief voor een bepaalde periode (of andere factoren) verwacht te maken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-7a"></a>NIST 800 53 AC-2 (7) een beheren

#### <a name="account-management--role-based-schemes"></a>Accountbeheer | Schema's op basis van rollen

**AC-2 (7) een** wordt tot stand gebracht en worden beheerd door privileged gebruikersaccounts volgens een schema toegangsgroepen op basis van die toegang tot het systeem toegestane informatie en bevoegdheden in rollen ordent van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de volgende typen van de systeem-account: Azure Active Directory-gebruikers, gebruikers van Windows-besturingssysteem, SQL Server-serviceaccount. Azure Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer door gebruikers toewijzen aan rollen; Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan beveiligingsgroepen. Deze regelingen op basis van rollen kunnen worden uitgebreid door de klant om missie behoeften te voldoen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-7b"></a>NIST 800 53 AC-2 (7) .b beheren

#### <a name="account-management--role-based-schemes"></a>Accountbeheer | Schema's op basis van rollen

**AC-2 (7) .b** bewaakt de organisatie de toewijzingen van bevoorrechte rol.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de OMS-beveiliging en de identiteit en toegang Dashboard Audit-oplossing. Dit dashboard kan accountmanagers voor het gebruik van informatie systeemaccounts bewaken. Deze oplossing kan worden opgezocht toewijzingen van bevoorrechte rol rapporteren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-7c"></a>NIST 800 53 AC-2 (7) .c beheren

#### <a name="account-management--role-based-schemes"></a>Accountbeheer | Schema's op basis van rollen

**AC-2 (7) .c** de organisatie nodig heeft [toewijzing: acties organisatie gedefinieerd] wanneer toewijzingen van bevoorrechte rol niet meer geschikt zijn.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de actie ondernemen op accounts klant beheerd wanneer toewijzingen van bevoorrechte rol geschikt zijn. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-9"></a>NIST 800 53 besturingselement AC-2 (9)

#### <a name="account-management--restrictions-on-use-of-shared--group-accounts"></a>Accountbeheer | Beperkingen bij het gebruik van gedeelde / groep Accounts

**AC-2 (9)** de organisatie is alleen toegestaan voor het gebruik van gedeelde of groep accounts die voldoen aan [toewijzing: organisatie gedefinieerde voorwaarden voor het tot stand brengen van gedeelde/groepsaccounts].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen gedeelde of groep accounts zijn ingeschakeld op de resources die zijn geïmplementeerd door deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-10"></a>NIST 800 53 besturingselement AC-2 (10)

#### <a name="account-management--shared--group-account-credential-termination"></a>Accountbeheer | Gedeelde / groep Account referentie beëindiging

**AC-2 (10)** het informatiesysteem wordt gedeeld of groep accountreferenties beëindigd wanneer leden zorgt dat de groep.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen gedeelde of groep accounts zijn ingeschakeld op de resources die zijn geïmplementeerd door deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-11"></a>NIST 800 53 besturingselement AC-2 (11)

#### <a name="account-management--usage-conditions"></a>Accountbeheer | Gebruik van voorwaarden

**AC-2 (11)** het informatiesysteem wordt afgedwongen [toewijzing: omstandigheden organisatie gedefinieerd en/of gebruik voorwaarden] voor [toewijzing: organisatie gedefinieerde informatie systeemaccounts].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Een Groepsbeleid kan worden ingesteld in Active Directory en geconfigureerd voor het implementeren van de dag-beperkingen of andere voorwaarden van de informatie over het gebruik account. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-12a"></a>NIST 800 53 AC-2 (12) een beheren

#### <a name="account-management--account-monitoring--atypical-usage"></a>Accountbeheer | Gebruik controle / ongebruikelijk account

**AC-2 (12) een** de organisatie bewaakt systeemaccounts informatie voor [toewijzing: ongewone gebruik organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk implementeert OMS beveiligings- en controle van de oplossing identiteits- en toegangsbeheer dashboard. Dit dashboard kan accountmanagers toegangspogingen tegen geïmplementeerde resources bewaken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-12b"></a>NIST 800 53 AC-2 (12) .b beheren

#### <a name="account-management--account-monitoring--atypical-usage"></a>Accountbeheer | Gebruik controle / ongebruikelijk account

**AC-2 (12) .b** de organisatie ongebruikelijk gebruik van informatie systeemaccounts naar rapporten [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk implementeert OMS beveiligings- en controle van de oplossing identiteits- en toegangsbeheer dashboard. Dit dashboard inschakelen accountmanagers toegangspogingen tegen geïmplementeerde resources bewaken. Deze oplossing kan worden geconfigureerd voor het verzenden van meldingen wanneer ongewone activiteit wordt vermoed of andere vooraf gedefinieerde gebeurtenissen plaatsvinden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-2-13"></a>NIST 800 53 besturingselement AC-2 (13)

#### <a name="account-management--disable-accounts-for-high-risk-individuals"></a>Accountbeheer | Accounts voor personen met een hoog risico uitschakelen

**AC-2 (13)** de organisatie wordt uitgeschakeld accounts van gebruikers die een aanzienlijke risico vormen binnen [toewijzing: organisatie gedefinieerde periode] van detectie van het risico.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau toegangsbeleid en procedures mogelijk tot stand brengen voorwaarden voor het uitschakelen van accounts voor gebruikers die een aanzienlijke risico vormen voor de organisatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-3"></a>NIST 800 53 besturingselement AC-3

#### <a name="access-enforcement"></a>Toegang afdwingen

**AC-3** goedgekeurde autorisaties voor logische toegang tot informatie en de systeembronnen in overeenstemming met beleid voor toegangsbeheer van toepassing wordt afgedwongen door het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk dwingt autorisaties van logische toegang met behulp van op rollen gebaseerde toegangsbeheer afgedwongen door Azure Active Directory door gebruikers toewijzen aan rollen, Active Directory gebruikers toe te wijzen aan beveiligingsgroepen, en bepaalt Windows OS-niveau. Azure Active Directory-functies die zijn toegewezen aan gebruikers of groepen logische toegang tot bronnen in Azure op het niveau, groep of een abonnement beheren. Active Directory-beveiligingsgroepen beheren logische toegang tot bronnen van de OS-niveau en functies. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-4"></a>NIST 800 53 besturingselement AC-4

#### <a name="information-flow-enforcement"></a>Informatie stroom afdwingen

**AC-4** goedgekeurde autorisaties voor het beheren van de stroom van gegevens in het systeem en tussen onderling verbonden systemen op basis van het informatiesysteem wordt afgedwongen [toewijzing: beleid voor organisatie gedefinieerde informatie stroom].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk informatie stroom beperkingen met behulp van netwerkbeveiligingsgroepen die zijn toegepast op de subnetten waarin resources worden geïmplementeerd, Application Gateway wordt afgedwongen en de load balancer. Netwerkbeveiligingsgroepen Zorg ervoor dat de informatiestroom tussen resources op basis van goedgekeurde regels wordt beheerd. Application Gateway en de load balancer dynamisch verkeer leiden tot specifieke bronnen op basis van goedgekeurde rollen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-4-8"></a>NIST 800 53 besturingselement AC-4 (8)

#### <a name="information-flow-enforcement--security-policy-filters"></a>Afdwingen van de stroom informatie | Beleid beveiligingsfilters

**AC-4 (8)** het informatiesysteem wordt afgedwongen informatie stroom beheren met [toewijzing: organisatie gedefinieerd beleid beveiligingsfilters] als basis voor het toegangsbeheer voor stroom [toewijzing: organisatie gedefinieerd informatiestromen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het afdwingen van informatie datatransportbesturing binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-4-21"></a>NIST 800 53 besturingselement AC-4 (21)

#### <a name="information-flow-enforcement--physical--logical-separation-of-information-flows"></a>Afdwingen van de stroom informatie | Fysieke / logische scheiding van informatiestromen

**AC-4 (21)** het informatiesysteem scheidt informatiestromen logisch of fysiek met [toewijzing: mechanismen organisatie gedefinieerd en/of technieken] om uit te voeren [toewijzing: kleurscheidingen door typen organisatie gedefinieerd vereist informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het scheiden van informatiestromen binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-5a"></a>NIST 800 53 besturingselement AC-5.

#### <a name="separation-of-duties"></a>Scheiding van functies

**AC 5.** gescheiden van de organisatie [toewijzing: organisatie gedefinieerd taken van personen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de scheiding van functies door de klant bewaakte accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-5b"></a>NIST 800 53 besturingselement AC-5.b

#### <a name="separation-of-duties"></a>Scheiding van functies

**AC 5.b** scheiding van functies van personen en documenten desgewenst de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het vastleggen van de scheiding van functies klant beheerde accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-5c"></a>NIST 800 53 besturingselement AC-5.c

#### <a name="separation-of-duties"></a>Scheiding van functies

**AC 5.c** de organisatie definieert informatie system-verificatie ondersteunt voor de ondersteuning van scheiding van functies.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert op rollen gebaseerde toegangsbeheer die kan worden geconfigureerd voor het scheiden van taken volgens de organisatievereisten van de. Azure Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer door gebruikers toewijzen aan rollen; Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan beveiligingsgroepen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-6"></a>NIST 800 53 besturingselement AC-6

#### <a name="least-privilege"></a>Minimale bevoegdheden

**AC-6** het principe van minimale bevoegdheden, zodat alleen geautoriseerde toegang voor gebruikers (of processen fungeert namens gebruikers) die nodig zijn voor toegewezen taken organisatie missies in overeenstemming met de veiligheidsmaatregelen voor de organisatie en bedrijfsfuncties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert toegangsbeheer op basis van rollen om gebruikers te beperken tot alleen de bevoegdheden die expliciet zijn toegewezen. Azure Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer door gebruikers toewijzen aan rollen; Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan beveiligingsgroepen.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-1"></a>NIST 800 53 besturingselement AC-6 (1)

#### <a name="least-privilege--authorize-access-to-security-functions"></a>Minimale bevoegdheden | Toegang verlenen aan beveiligingsfuncties

**AC-6 (1)** de organisatie expliciet gemachtigd voor toegang tot [toewijzing: organisatie gedefinieerd beveiligingsfuncties (geïmplementeerd in de hardware, software en firmware) en beveiliging relevante informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures voor toegang op bedrijfsniveau controle van de klant kunnen een autorisatieproces toegang die toegang geeft tot beveiligingsfuncties vaststellen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-2"></a>NIST 800 53 besturingselement AC-6 (2)

#### <a name="least-privilege--non-privileged-access-for-nonsecurity-functions"></a>Minimale bevoegdheden | Onbevoegde toegang voor de functies niet voor beveiliging

**AC-6 (2)** de organisatie vereist dat gebruikers van informatie systeemaccounts of rollen, met toegang tot [toewijzing: organisatie gedefinieerd beveiligingsfuncties of beveiliging relevante informatie], niet-gemachtigde accounts of -rollen te gebruiken wanneer toegang tot de functies niet voor beveiliging.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant kan gebruikers niet-gemachtigde accounts gebruikt bij het openen van de functies niet voor beveiliging vereisen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-3"></a>NIST 800 53 besturingselement AC-6 (3)

#### <a name="least-privilege--network-access-to-privileged-commands"></a>Minimale bevoegdheden | Toegang tot het netwerk aan bevoorrechte opdrachten

**AC-6 (3)** toegang tot het netwerk worden geautoriseerd door de organisatie [toewijzing: bevoegde opdrachten organisatie gedefinieerd] alleen voor [toewijzing: organisatie gedefinieerd dwingende operationele behoeften] en documenten van de logica die toegang in de beveiligingsplan voor het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant mogelijk bevoorrechte opdrachten die kunnen worden geopend via een netwerk te definiëren. Opmerking: Klanten hebben geen fysieke toegang tot Azure-infrastructuur. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-5"></a>NIST 800 53 besturingselement AC-6 (5)

#### <a name="least-privilege--privileged-accounts"></a>Minimale bevoegdheden | Bevoegde Accounts

**AC-6 (5)** de organisatie beperkt bevoegde accounts op het informatiesysteem [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant mogelijk beperkingen voor het gebruik van bevoorrechte accounts definiëren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-7a"></a>NIST 800 53 AC-6 (7) een beheren

#### <a name="least-privilege--review-of-user-privileges"></a>Minimale bevoegdheden | Beoordeling van de gebruiker heeft bevoegdheden

**AC-6 (7) een** de beoordelingen organisatie [toewijzing: organisatie gedefinieerde frequentie] de bevoegdheden die zijn toegewezen aan [toewijzing: rollen organisatie gedefinieerd of klassen van gebruikers] voor het valideren van de noodzaak van dergelijke bevoegdheden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren van de gebruiker heeft bevoegdheden van de klant beheerde accounts. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-7b"></a>NIST 800 53 AC-6 (7) .b beheren

#### <a name="least-privilege--review-of-user-privileges"></a>Minimale bevoegdheden | Beoordeling van de gebruiker heeft bevoegdheden

**AC-6 (7) .b** de organisatie wijst opnieuw toe of verwijdert u de bevoegdheden, indien nodig, in overeenstemming met correct organisatie missie/zakelijke behoeften.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het opnieuw toewijzen of verwijderen van bevoegdheden voor de klant beheerde accounts, indien van toepassing. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-8"></a>NIST 800 53 besturingselement AC-6 (8)

#### <a name="least-privilege--privilege-levels-for-code-execution"></a>Minimale bevoegdheden | Bevoegdheidsniveaus voor de uitvoering van Code

**AC-6 (8)** voorkomt u dat het informatiesysteem [toewijzing: software organisatie gedefinieerd] wordt uitgevoerd op een hoger niveau van bevoegdheden dan gebruikers die de software wordt uitgevoerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert toegangsbeheer op basis van rollen om gebruikers te beperken tot alleen de bevoegdheden die expliciet zijn toegewezen. Virtuele machine OS-niveau beveiligingen toegestaan software uit te voeren op een hoger machtigingsniveau dan gebruikers uitvoeren van de software niet. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-9"></a>NIST 800 53 besturingselement AC-6 (9)

#### <a name="least-privilege--auditing-use-of-privileged-functions"></a>Minimale bevoegdheden | Gebruik van bevoorrechte functies controleren

**AC-6 (9)** het informatiesysteem tijdens de uitvoering van bevoegde functies controles.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Log Analytics-service in OMS. Geïmplementeerde virtuele machines en Azure storage-accounts voor diagnostische gegevens zijn verbonden bronnen voor logboekanalyse ervoor te zorgen dat de uitvoering van bevoegde functies wordt gecontroleerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-6-10"></a>NIST 800 53 besturingselement AC-6 (10)

#### <a name="least-privilege--prohibit-non-privileged-users-from-executing-privileged-functions"></a>Minimale bevoegdheden | Voorkomen dat onbevoegde gebruikers bevoorrechte functies uitvoeren

**AC-6 (10)** het informatiesysteem wordt voorkomen dat onbevoegde gebruikers bevoorrechte functies om op te nemen is uitgeschakeld, omzeilen, uitvoeren of wijzigen van beveiliging beveiligingen/countermeasures zijn geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert toegangsbeheer op basis van rollen om gebruikers te beperken tot alleen de bevoegdheden die expliciet zijn toegewezen.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-7a"></a>NIST 800 53 besturingselement AC-7.

#### <a name="unsuccessful-logon-attempts"></a>Mislukte aanmeldingspogingen

**AC 7.** het informatiesysteem dwingt een limiet van [toewijzing: nummer organisatie gedefinieerd] opeenvolgende ongeldige aanmeldingspogingen door een gebruiker tijdens een [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De Azure portal limieten opeenvolgende ongeldige aanmeldingspogingen door gebruikers. Groepsbeleid wordt toegepast op het niveau van het besturingssysteem voor alle virtuele machines die zijn geïmplementeerd door deze blauwdruk Azure. Het beleid beperkt opeenvolgende mislukte aanmeldpogingen door gebruikers niet meer dan drie binnen een periode van 15 minuten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-7b"></a>NIST 800 53 besturingselement AC-7.b

#### <a name="unsuccessful-logon-attempts"></a>Mislukte aanmeldingspogingen

**AC 7.b** het informatiesysteem automatisch [selectie: Hiermee vergrendelt u de accountknooppunt voor een [toewijzing: organisatie gedefinieerde periode]; Hiermee vergrendelt u het account/knooppunt tot vrijgegeven door een beheerder; uitstelt volgende aanmeldingsprompt volgens [ Toewijzing: vertraging organisatie gedefinieerd algoritme]] wanneer het maximum aantal mislukte pogingen is overschreden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De Azure-portal vergrendelt accounts nadat opeenvolgende ongeldige door gebruikers aanmeldingspogingen. Groepsbeleid wordt toegepast op het niveau van het besturingssysteem voor alle virtuele machines die zijn geïmplementeerd door deze blauwdruk Azure. Het beleid wordt vergrendeld accounts gedurende drie uur na drie opeenvolgende mislukte aanmeldpogingen door gebruikers. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-7-2"></a>NIST 800 53 besturingselement AC-7 (2)

#### <a name="unsuccessful-logon-attempts--purge--wipe-mobile-device"></a>Mislukte aanmeldingspogingen | Leegmaken / mobiel apparaat wissen

**AC-7 (2)** het informatiesysteem schoont wissen informatie uit [toewijzing: mobiele apparaten organisatie gedefinieerd] op basis van [toewijzing: organisatie gedefinieerd vereisten/technieken verwijderen/wissen] nadat [toewijzing: aantal organisatie gedefinieerd] aanmeldingspogingen opeenvolgende apparaten met mislukte

**Verantwoordelijkheden:**`Not Applicable`

|||
|---|---|
| **De klant** | Mobiele apparaten zich niet binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor mobiele apparaten binnen de grenzen van de Azure. Als zodanig is dit besturingselement niet van toepassing op Microsoft Azure. |


 ## <a name="nist-800-53-control-ac-8a"></a>NIST 800 53 besturingselement AC-8.

#### <a name="system-use-notification"></a>Gebruik Systeemmelding

**AC 8.** het informatiesysteem wordt weergegeven aan gebruikers [toewijzing: organisatie gedefinieerd gebruik Meldingsbericht of banner] voordat het verlenen van toegang tot het systeem dat privacy en beveiliging meldingen consistent zijn met de toepasselijke federal biedt wettelijke Executive Orders, richtlijnen, beleid, standaarden en richtlijnen en statussen dat gebruikers toegang hebben tot een VS Systeem voor Government informatie; het gebruik van informatie kan worden bewaakt, opgenomen, en onderhevig aan audit; onbevoegd gebruik van het informatiesysteem is verboden en onderworpen aan strafrechtelijke en civiele sancties; en gebruik van het informatiesysteem aangegeven toestemming voor bewaking en op te nemen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Een groepsbeleid implementeert een melding voor het gebruik van systeem die wordt weergegeven aan gebruikers voordat de aanmelding. Opmerking: De Azure-blauwdruk implementeert een voorbeeld Systeemmelding voor het gebruik. De klant moet deze tekst om te voldoen aan de organisatie en/of regelgeving hoofdtekst vereisten bewerken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-8b"></a>NIST 800 53 besturingselement AC-8.b

#### <a name="system-use-notification"></a>Gebruik Systeemmelding

**AC 8.b** het informatiesysteem behoudt de Meldingsbericht of de banner op het scherm totdat gebruikers de voorwaarden gebruik erkennen en expliciete maatregelen aan te melden bij of verder toegang tot het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Een groepsbeleid implementeert een melding voor het gebruik van systeem die wordt weergegeven aan gebruikers voordat de aanmelding. De gebruiker moet de melding om u te melden bevestigen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-8c"></a>NIST 800 53 besturingselement AC-8.c

#### <a name="system-use-notification"></a>Gebruik Systeemmelding

**AC 8.c** geeft informatie over het gebruik van het bestandssysteem van de informatiesysteem voor systemen die openbaar toegankelijk [toewijzing: organisatie gedefinieerde voorwaarden], voordat het verlenen van toegang voor verdere; verwijzingen weergegeven dat moet worden bewaakt, opneemt, of controle die consistent zijn met privacy werkplekken voor dergelijke systemen die in het algemeen die activiteiten verbieden; en bevat een beschrijving van de gemachtigde maakt gebruik van het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het weergeven van een Systeemmelding voor gebruik op alle openbaar toegankelijk klant geïmplementeerd bronnen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-10"></a>NIST 800 53 besturingselement AC-10

#### <a name="concurrent-session-control"></a>Beheer van gelijktijdige sessies

**AC-10** het informatiesysteem beperkt het aantal gelijktijdige sessies voor elk [toewijzing: account organisatie gedefinieerd en/of accounttype] naar [toewijzing: nummer organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Een beleid voor het besturingssysteem is geïmplementeerd voor virtuele machines die door deze blauwdruk Azure is geïmplementeerd. Het beleid implementeert gelijktijdige sessies beperkingen (twee sessies). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-11a"></a>NIST 800 53 besturingselement AC-11.a

#### <a name="session-lock"></a>Sessievergrendeling

**AC 11.a** het informatiesysteem voorkomt verdere toegang tot het systeem door het initiëren van een sessievergrendeling na [toewijzing: organisatie gedefinieerde periode] inactief of een aanvraag wordt ontvangen van een gebruiker.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Een groepsbeleid implementeert een vergrendeling inactiviteit voor RDP-sessies. Gebruikers kunnen handmatig starten van de vergrendeling. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-11b"></a>NIST 800 53 besturingselement AC-11.b

#### <a name="session-lock"></a>Sessievergrendeling

**AC 11.b** het informatiesysteem behoudt de sessievergrendeling totdat de gebruiker toegang tot stand gebrachte procedures voor identificatie en verificatie met herstelt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Een groepsbeleid implementeert een vergrendeling inactiviteit voor RDP-sessies. Gebruikers moeten opnieuw als u wilt ontgrendelen van de sessie.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-11-1"></a>NIST 800 53 besturingselement AC-11 (1)

#### <a name="session-lock--pattern-hiding-displays"></a>Sessievergrendeling | Geeft patroon verbergen

**AC-11 (1)** het informatiesysteem verbergt via de sessievergrendeling informatie die eerder zichtbaar is voor de weergave met een installatiekopie van het openbaar zichtbaar.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert u een domeincontroller waarop alle geïmplementeerde virtuele machines zijn gekoppeld. Een groepsbeleid implementeert een vergrendeling inactiviteit voor RDP-sessies. De sessievergrendeling verbergt informatie die eerder zichtbaar is. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-12"></a>NIST 800 53 besturingselement AC-12

#### <a name="session-termination"></a>Sessie-beëindiging

**AC-12** automatisch beëindigd door het informatiesysteem een gebruikerssessie na [toewijzing: organisatie gedefinieerde voorwaarden of trigger gebeurtenissen vereisen sessie verbreken].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De configuratie van extern bureaublad-sessie-host voor de Windows virtuele machines geïmplementeerd door deze blauwdruk Azure kan worden geconfigureerd om te voldoen aan vereisten voor de afsluiting van organisatie-sessie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-12-1a"></a>(((1) een van NIST 800 53 besturingselement AC-12

#### <a name="session-termination--user-initiated-logouts--message-displays"></a>Sessie-beëindiging | Gestarte afmeldingen gebruiker gestart / bericht weergeven

**AC-12 (1) een** het informatiesysteem biedt een mogelijkheid afmelden voor gebruiker gestart communicatie sessies wanneer verificatie wordt gebruikt om toegang te krijgen tot [toewijzing: organisatie gedefinieerd informatiebronnen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De Azure portal en de virtuele machine besturingssystemen geïmplementeerd door deze blauwdruk Azure inschakelen gebruikt voor het initiëren van een afmelding. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-12-1b"></a>(((1) .b van NIST 800 53 besturingselement AC-12

#### <a name="session-termination--user-initiated-logouts--message-displays"></a>Sessie-beëindiging | Gestarte afmeldingen gebruiker gestart / bericht weergeven

**AC-12 (1) .b** informatie krijgt een expliciete afmeldingsbericht aan gebruikers die de betrouwbare beëindiging van het geverifieerde communicatie sessies aangeeft.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De Azure portal en de virtuele machine besturingssystemen geïmplementeerd door deze blauwdruk Azure inschakelen gebruikt voor het initiëren van een afmelding. Het proces afmelding geeft indicatie voor de gebruikers dat de sessie is beëindigd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-14a"></a>NIST 800 53 besturingselement AC-14.a

#### <a name="permitted-actions-without-identification-or-authentication"></a>Toegestane acties zonder identificatie of verificatie

**AC 14.a** identificeert de organisatie [toewijzing: organisatie gedefinieerd gebruikersacties] die kan worden uitgevoerd op het informatiesysteem zonder identificatie of verificatie consistent zijn met organisatie-missies/bedrijfsfuncties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het identificeren van de acties die kunnen worden uitgevoerd op de bronnen klant geïmplementeerd zonder identificatie of verificatie (bijvoorbeeld, zoals een openbaar toegankelijke webpagina weergegeven). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-14b"></a>NIST 800 53 besturingselement AC-14.b

#### <a name="permitted-actions-without-identification-or-authentication"></a>Toegestane acties zonder identificatie of verificatie

**AC 14.b** documenten en bevat ondersteunende logica in de beveiligingsplan voor het informatiesysteem acties van de gebruiker geen vereisen identificatie of verificatie van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het ontwikkelen van documentatie voor acties van de gebruiker geen vereisen identificatie of verificatie op resources klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-17a"></a>NIST 800 53 besturingselement AC-17.a

#### <a name="remote-access"></a>Externe toegang

**AC 17.a** de organisatie vaststelt en documenten gebruiksbeperkingen, vereisten van de configuration-verbinding en implementatie-instructies voor elk type externe toegang is toegestaan.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant mogelijk RAS gebruiksbeperkingen definiëren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-17b"></a>NIST 800 53 besturingselement AC-17.b

#### <a name="remote-access"></a>Externe toegang

**AC 17.b** externe toegang tot het systeem gegevens voordat deze verbindingen worden geautoriseerd door de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures van de controle van de klant op bedrijfsniveau toegang mogelijk tot stand brengen een autorisatieproces externe toegang. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-17-1"></a>NIST 800 53 besturingselement AC-17 (1)

#### <a name="remote-access--automated-monitoring--control"></a>Externe toegang | Geautomatiseerd volgen / beheren

**AC-17 (1)** het informatiesysteem bewaakt en beheert RAS-methoden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure biedt externe toegang tot het informatiesysteem via de Azure-portal via Extern bureaublad-verbinding via een jumpbox en via een webtoepassing die door de klant is geïmplementeerd. Toegang via de Azure-portal en extern bureaublad-sessies worden gecontroleerd en kunnen worden gecontroleerd met OMS. De klant moet de RAS-besturingselementen, indien nodig, naar de webtoepassing implementeren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-17-2"></a>NIST 800 53 besturingselement AC-17 (2)

#### <a name="remote-access--protection-of-confidentiality--integrity-using-encryption"></a>Externe toegang | Beveiliging van de vertrouwelijkheid / integriteit met behulp van versleuteling

**AC-17 (2)** cryptografische mechanismen ter bescherming van de vertrouwelijkheid en integriteit van de RAS-sessies door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Externe toegang tot bronnen die zijn geïmplementeerd door deze Blueprint Azure, met inbegrip van de Azure-portal, de verbinding met extern bureaublad en de web application gateway, zijn beveiligd via TLS. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-17-3"></a>NIST 800 53 besturingselement AC-17 (3)

#### <a name="remote-access--managed-access-control-points"></a>Externe toegang | Controlepunten voor beheerde toegang

**AC-17 (3)** het informatiesysteem van externe toegang via routes [toewijzing: nummer organisatie gedefinieerd] besturingselement netwerktoegangspunten beheerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Externe toegang tot de fictieve webtoepassing geïmplementeerd door deze blauwdruk Azure is via een toepassingsgateway. Externe toegang tot alle andere resources, is via een jumpbox. Er zijn geen andere openbaar toegankelijke eindpunten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-17-4a"></a>NIST 800 53 AC-17 (4) een beheren

#### <a name="remote-access--privileged-commands--access"></a>Externe toegang | Beschermde opdrachten / toegang

**AC-17 (4) een** tijdens de uitvoering van bevoegde opdrachten en toegang tot beveiliging relevante informatie via externe toegang alleen voor de organisatie worden geautoriseerd [toewijzing: organisatie gedefinieerde behoeften].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant mogelijk bevoorrechte opdrachten die extern worden geopend en bevatten een logica gedefinieerd. Opmerking: Klanten hebben geen met directe netwerktoegang tot Azure-infrastructuur. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-17-4b"></a>NIST 800 53 AC-17 (4) .b beheren

#### <a name="remote-access--privileged-commands--access"></a>Externe toegang | Beschermde opdrachten / toegang

**AC-17 (4) .b** de organisatie en de logica die toegang documenten in de beveiligingsplan voor het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant mogelijk bevoorrechte opdrachten die extern worden geopend en bevatten een logica gedefinieerd. Opmerking: Klanten hebben geen met directe netwerktoegang tot Azure-infrastructuur. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-17-9"></a>NIST 800 53 besturingselement AC-17 (9)

#### <a name="remote-access--disconnect--disable-access"></a>Externe toegang | Verbinding verbreken / toegang uitschakelen

**AC-17 (9)** de organisatie, biedt de mogelijkheid snel verbreken of uitschakelen van externe toegang tot het informatiesysteem binnen [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure biedt externe toegang tot het informatiesysteem via de Azure portal, via Extern bureaublad-verbinding via een jumpbox en via een webtoepassing. Als een Azure Active Directory-account is uitgeschakeld of verwijderd, wordt toegang tot de Azure portal onmiddellijk beëindigd. Op dezelfde manier als een virtuele machine OS-level-account is uitgeschakeld of verwijderd, is de toegang tot extern bureaublad via de jumpbox ontkoppeld onmiddellijk. Externe toegang moeten worden geïmplementeerd door klanten verbreken voor de webtoepassing. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-18a"></a>NIST 800 53 besturingselement AC-18.a

#### <a name="wireless-access"></a>Draadloze toegang

**AC 18.a** de organisatie vaststelt gebruiksbeperkingen, vereisten van de configuration-verbinding en implementatie richtlijnen voor draadloze toegang.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen draadloze toegang binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure stelt gebruiksbeperkingen, vereisten van de configuration-verbinding en richtlijnen voor draadloze toegang via het netwerk Security Standard, die expliciet het gebruik van draadloze in de Microsoft Azure-omgeving verbiedt voor uitvoering. |


 ## <a name="nist-800-53-control-ac-18b"></a>NIST 800 53 besturingselement AC-18.b

#### <a name="wireless-access"></a>Draadloze toegang

**AC 18.b** draadloze toegang tot het systeem gegevens voordat deze verbindingen worden geautoriseerd door de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen draadloze toegang binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor draadloze toegang in datacenters van Microsoft Azure. |


 ### <a name="nist-800-53-control-ac-18-1"></a>NIST 800 53 besturingselement AC-18 (1)

#### <a name="wireless-access--authentication-and-encryption"></a>Draadloze toegang | Verificatie en versleuteling

**AC-18 (1)** draadloze toegang tot het systeem met verificatie van het informatiesysteem worden beschermd [selectie (een of meer): gebruikers; apparaten] en versleuteling.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen draadloze toegang binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor draadloze toegang in de Microsoft Azure-omgeving. |


 ### <a name="nist-800-53-control-ac-18-3"></a>NIST 800 53 besturingselement AC-18 (3)

#### <a name="wireless-access--disable-wireless-networking"></a>Draadloze toegang | Draadloze netwerken uitschakelen

**AC-18 (3)** de organisatie wordt uitgeschakeld, wanneer het niet bedoeld voor gebruik, draadloze netwerken mogelijkheden die intern zijn ingesloten in onderdelen van het systeem informatie voorafgaand aan de uitgifte en de implementatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen draadloze toegang binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor draadloze toegang in de Microsoft Azure-omgeving. |


 ### <a name="nist-800-53-control-ac-18-4"></a>NIST 800 53 besturingselement AC-18 (4)

#### <a name="wireless-access--restrict-configurations-by-users"></a>Draadloze toegang | Configuraties door gebruikers beperken

**AC-18 (4)** de organisatie identificeert en expliciet machtigt gebruikers toegestaan draadloze netwerkmogelijkheden apart te configureren.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen draadloze toegang binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor draadloze toegang in de Microsoft Azure-omgeving. |


 ### <a name="nist-800-53-control-ac-18-5"></a>NIST 800 53 besturingselement AC-18 (5)

#### <a name="wireless-access--antennas--transmission-power-levels"></a>Draadloze toegang | Antennes / Transmission Power niveaus

**AC-18 (5)** selecteert keuzerondje antennes en kalibreren transmission power beveiligingsniveaus om te verminderen de kans dat bruikbare signalen buiten de grenzen organisatie beheerd kunnen worden ontvangen door de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen draadloze toegang binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor draadloze toegang in de Microsoft Azure-omgeving. |


 ## <a name="nist-800-53-control-ac-19a"></a>NIST 800 53 besturingselement AC-19.a

#### <a name="access-control-for-mobile-devices"></a>Toegangsbeheer voor mobiele apparaten

**AC 19.a** de organisatie vaststelt gebruiksbeperkingen, configuratievereisten verbinding vereisten en implementatie-richtlijnen voor mobiele apparaten organisatie worden beheerd.

**Verantwoordelijkheden:**`Not Applicable`

|||
|---|---|
| **De klant** | Er zijn geen klant beheerde mobiele apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor mobiele apparaten binnen de grenzen van de Azure. Als zodanig is dit besturingselement niet van toepassing op Microsoft Azure. |


 ## <a name="nist-800-53-control-ac-19b"></a>NIST 800 53 besturingselement AC-19.b

#### <a name="access-control-for-mobile-devices"></a>Toegangsbeheer voor mobiele apparaten

**AC 19.b** de organisatie machtigt de verbinding van mobiele apparaten om organisatiegegevens systemen.

**Verantwoordelijkheden:**`Not Applicable`

|||
|---|---|
| **De klant** | Er zijn geen klant beheerde mobiele apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor mobiele apparaten binnen de grenzen van de Azure. Als zodanig is dit besturingselement niet van toepassing op Microsoft Azure. |


 ### <a name="nist-800-53-control-ac-19-5"></a>NIST 800 53 besturingselement AC-19 (5)

#### <a name="access-control-for-mobile-devices--full-device--container-based--encryption"></a>Toegangsbeheer voor mobiele apparaten | Volledige apparaat / op basis van een Container versleuteling

**AC-19 (5)** maakt gebruik van de organisatie [selectie: volledige apparaatversleuteling; container versleuteling] bij het beveiligen van de vertrouwelijkheid en integriteit van gegevens op [toewijzing: mobiele apparaten organisatie gedefinieerd].

**Verantwoordelijkheden:**`Not Applicable`

|||
|---|---|
| **De klant** | Er zijn geen klant beheerde mobiele apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is niet toegestaan voor mobiele apparaten binnen de grenzen van de Azure. Als zodanig is dit besturingselement niet van toepassing op Microsoft Azure. |


 ## <a name="nist-800-53-control-ac-20a"></a>NIST 800 53 besturingselement AC-20.a

#### <a name="use-of-external-information-systems"></a>Gebruik van externe informatiesystemen

**AC 20.a** de organisatie vaststelt voorwaarden en bepalingen consistent zijn met alle vertrouwensrelaties met andere organisaties die eigenaar is, werkt, en/of onderhouden van gegevens met externe systemen, waardoor gemachtigde personen voor toegang tot het informatiesysteem uit externe gegevens systemen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant kan een bepaling met betrekking tot het gebruik van cloud-serviceaanbiedingen onder FedRAMP bevatten. Azure heeft gekregen een voorlopige autorisatie werken (P-ATO) door de FedRAMP gezamenlijke autorisatie Mededelingenbord (JAB) om overname gebruik van Azure-cloudservices van overheidsinstanties. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-20b"></a>NIST 800 53 besturingselement AC-20.b

#### <a name="use-of-external-information-systems"></a>Gebruik van externe informatiesystemen

**AC 20.b** de organisatie vaststelt voorwaarden en bepalingen consistent zijn met alle vertrouwensrelaties met andere organisaties die eigenaar is, werkt, en/of onderhouden van gegevens met externe systemen, waardoor gemachtigde personen om te verwerken, opslaan of organisatie gecontroleerd informatie met externe systemen worden verzonden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant kan een bepaling met betrekking tot het gebruik van cloud-serviceaanbiedingen onder FedRAMP bevatten. Azure heeft gekregen een voorlopige autorisatie werken (P-ATO) door de FedRAMP gezamenlijke autorisatie Mededelingenbord (JAB) om overname gebruik van Azure-cloudservices van overheidsinstanties. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-20-1"></a>NIST 800 53 besturingselement AC-20 (1)

#### <a name="use-of-external-information-systems--limits-on-authorized-use"></a>Gebruik van externe informatiesystemen | Limieten voor geautoriseerd gebruik

**AC-20 (1)** bevoegde personen met een extern systeem toegang tot het informatiesysteem of verwerken, opslaan of organisatie gecontroleerd informatie verzenden alleen als de organisatie heeft geverifieerd door de organisatie wordt toegestaan de toepassing van de vereiste controles op het externe systeem, zoals opgegeven in het beveiligingsbeleid van informatie van de organisatie en beveiligingsplan; of behoudt goedgekeurde informatie system verbinding of verwerking overeenkomsten aan de organisatie-entiteit die als host fungeert voor het systeem externe gegevens.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Van de klant op bedrijfsniveau IT-groep mag naleving cloud serviceprovider organisatie informatie beveiligingsvereisten en de goedkeuring van de hele onderneming voor het gebruik van de gekoppelde cloud serviceaanbiedingen verlenen. Azure heeft een voorlopige autorisatie werken (P-ATO) door de FedRAMP gezamenlijke autorisatie Mededelingenbord (JAB) gekregen. Azure wordt beoordeeld door een derde partij FedRAMP goedgekeurd assessment organisatie (3PAO) om te controleren of de naleving van beveiligingscontrole FedRAMP en andere vereisten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ac-20-2"></a>NIST 800 53 besturingselement AC-20 (2)

#### <a name="use-of-external-information-systems--portable-storage-devices"></a>Gebruik van externe informatiesystemen | Draagbare opslagapparaten

**AC-20 (2)** de organisatie [selectie: beperkt; verbiedt] het gebruik van de organisatie worden beheerd draagbare opslagapparaten door gemachtigde gebruikers op externe systemen.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft is niet toegestaan door de klant bewaakte draagbare opslagapparaten binnen de Microsoft Azure-omgeving. |


 ## <a name="nist-800-53-control-ac-21a"></a>NIST 800 53 besturingselement AC-21.a

#### <a name="information-sharing"></a>Delen van gegevens

**AC 21.a** de organisatie doordat de geautoriseerde gebruikers om te bepalen of de machtigingen die toegang worden toegewezen aan de delen partner overeenkomen met de toegangsbeperkingen voor de gegevens voor delen van informatie vereenvoudigt [toewijzing: organisatie gedefinieerde informatie delen omstandigheden waarin de gebruiker naar keuze vereist is].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Beleid op bedrijfsniveau voor toegangsbeheer van de klant kan bepalingen met betrekking tot het delen van gegevens bevatten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-21b"></a>NIST 800 53 besturingselement AC-21.b

#### <a name="information-sharing"></a>Delen van gegevens

**AC 21.b** maakt gebruik van de organisatie [toewijzing: organisatie gedefinieerd geautomatiseerde mechanismen of handmatige processen] om gebruikers te helpen beslissingen informatie delen/samenwerking.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Op bedrijfsniveau gegevensuitwisseling besluit ondersteuningsmogelijkheid mogelijk afhankelijk van de klant. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-22a"></a>NIST 800 53 besturingselement AC-22.a

#### <a name="publicly-accessible-content"></a>Openbaar toegankelijk inhoud

**AC 22.a** personen die gemachtigd zijn om gegevens naar een openbaar toegankelijke informatiesysteem te posten Hiermee geeft u de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau toegang controleprocedures kunnen personen die gemachtigd zijn om gegevens openbaar toegankelijk te posten aanwijzen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-22b"></a>NIST 800 53 besturingselement AC-22.b

#### <a name="publicly-accessible-content"></a>Openbaar toegankelijk inhoud

**AC 22.b** de organisatie traint bevoegde personen om ervoor te zorgen dat openbaar toegankelijk informatie geen niet-openbare informatie bevat.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant mogelijk zijn afhankelijk van de training voor personen die gemachtigd zijn om gegevens openbaar toegankelijk te posten op bedrijfsniveau. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-22c"></a>NIST 800 53 besturingselement AC-22.c

#### <a name="publicly-accessible-content"></a>Openbaar toegankelijk inhoud

**AC 22.c** de organisatie beoordeelt de voorgestelde inhoud van de gegevens voordat ze op het systeem openbaar toegankelijke informatie om ervoor te zorgen dat niet-openbare informatie niet opgenomen is.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures voor toegang op bedrijfsniveau controle van de klant kunnen een controleproces voor inhoud moet een openbaar toegankelijke systeem worden geboekt voorgestelde vaststellen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ac-22d"></a>NIST 800 53 besturingselement AC-22.d

#### <a name="publicly-accessible-content"></a>Openbaar toegankelijk inhoud

**AC 22.d** de organisatie controleert de inhoud op het systeem openbaar toegankelijke informatie voor niet-openbare informatie [toewijzing: organisatie gedefinieerde frequentie] en verwijdert u deze informatie gedetecteerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Procedures voor toegang op bedrijfsniveau controle van de klant kunnen een proces voor de periodieke evaluatie van de inhoud die wordt gepost op openbaar toegankelijke systemen opzetten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |

