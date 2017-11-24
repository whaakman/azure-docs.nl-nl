---
title: FedRAMP Azure blauwdruk Automation - Media-beveiliging
description: Webtoepassingen voor FedRAMP - Media-beveiliging
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: fe86fd92-ef6b-4d17-a4a2-de6796d251d0
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 9cb33abc7ab88aaa54a77308c7863f8128f2e6c7
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="media-protection-mp"></a>Media-beveiliging (MP)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-mp-1"></a>NIST 800 53 besturingselement MP-1

#### <a name="media-protection-policy-and-procedures"></a>Media-beveiligingsbeleid en -Procedures

**MP-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een beveiligingsbeleid voor media die zijn gericht op doel, bereik, rollen, verantwoordelijkheden, management streven, coördinatie tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van de media protection-beleid en de bijbehorende media beveiliging besturingselementen; en controleert en updates van het huidige media beveiligingsbeleid [toewijzing: organisatie gedefinieerde frequentie]; en media beveiliging procedures [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Het beveiligingsbeleid op bedrijfsniveau media en de procedures van de klant mogelijk voldoende zijn voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-mp-2"></a>NIST 800 53 besturingselement MP-2

#### <a name="media-access"></a>Media-toegang

**MP-2** de organisatie de toegang tot beperkt [toewijzing: organisatie gedefinieerde typen digitale en/of niet-digitale media] naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is geïmplementeerd media toegang via de uitvoering van het beveiligingsbeleid van Microsoft. Logische toegang tot digitaal medium wordt beheerd via Active Directory groepsbeleidsobjecten (AD) en beveiligingsgroepen. Fysieke toegang tot alle media is beperkt door het proces van de toegang tot datacenter. Toegang is beperkt tot personen die een legitieme bedrijfsdoel voor toegang tot de gegevens hebben. Raadpleeg PE-3, fysieke toegangsbeheer, voor meer informatie over het datacenter toegang controlemechanismen zijn geïmplementeerd. De Asset beveiliging standaard definieert de beveiligingen voor vereist voor het beveiligen van de vertrouwelijkheid, integriteit en beschikbaarheid van gegevens binnen datacenters van Microsoft Azure. |


 ## <a name="nist-800-53-control-mp-3a"></a>NIST 800 53 besturingselement MP-3.

#### <a name="media-marking"></a>Markering voor Media

**MP-3.** de organisatie markeert informatie Systeemmedia die de distributie-beperkingen, verwerken aanvullende opmerkingen en toepasselijke security-markeringen (indien aanwezig) van de gegevens aangeeft.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure markeert activa in Microsoft-datacenters met een HBI, MBI of LBI (hoog, Gemiddeld of laag Business Impact) aanwijzing waarvoor verschillende niveaus van beveiliging en voorzorgsmaatregelen verwerking is vereist. Asset eigenaars zijn vereist voor het classificeren van de activa die zijn opgeslagen in een Microsoft-datacentrum. Raadpleeg Asset classificatie standaard- en Asset beveiliging voor meer informatie. |


 ## <a name="nist-800-53-control-mp-3b"></a>NIST 800 53 besturingselement MP-3.b

#### <a name="media-marking"></a>Markering voor Media

**MP-3.b** de organisatie uitsluit [toewijzing: organisatie gedefinieerde typen informatie Systeemmedia] markeren, zolang de media binnen blijven [toewijzing: gebieden organisatie gedefinieerd beheerd].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure vereist asset-eigenaren hun activa met een activaclassificatie toewijzen, en geen activa zijn uitgesloten van deze vereiste. In de Microsoft-datacenteromgeving verwijzen activa op servers en netwerkapparaten magnetische tapes. Andere digitale media zoals USB flash miniatuur stations, externe/verwisselbare harde schijven, of CD/DVD's worden niet gebruikt. Niet-digitaal medium wordt niet gebruikt in het datacenter. |


 ## <a name="nist-800-53-control-mp-4a"></a>NIST 800 53 besturingselement MP-4.

#### <a name="media-storage"></a>Mediaopslag

**MP 4.** fysiek bepaalt en veilig opgeslagen in de organisatie [toewijzing: organisatie gedefinieerde typen digitale en/of niet-digitale media] in [toewijzing: gebieden organisatie gedefinieerd beheerd].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure digitaal medium activa zijn fysiek en veilig opgeslagen in datacenter collocatie ruimten. Microsoft-datacenters beschikken over meerdere lagen van fysieke toegangsbeheer (toegang badge, biometrie; Zie PE 3 voor meer informatie over fysieke toegangscontrole) en videobewaking voor veilige opslag. Digitaal medium voor omvat servers en netwerkapparaten magnetische tapes gebruikt voor back-up. Niet-digitaal medium wordt niet gebruikt in het datacenteromgeving. |


 ## <a name="nist-800-53-control-mp-4b"></a>NIST 800 53 besturingselement MP-4.b

#### <a name="media-storage"></a>Mediaopslag

**MP-4.b** informatie Systeemmedia in de organisatie worden beveiligd totdat de media worden vernietigd of met behulp van opgeschoonde goedgekeurd apparatuur, technieken en procedures.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure digitaal medium activa worden beveiligd in een Microsoft datacenter colocations via fysieke toegangscontrole (PE 3) en logische toegang besturingselementen (IA-2) voor de levensduur van de asset. Microsoft Azure-activa zijn uitgeschakeld, worden opgeschoond of vernietigd met methoden die consistent zijn met NIST SP 800 88 vóór de verwijdering van de activa. Voor asset vernietiging; maakt gebruik van Microsoft Azure beveiligingspersoneel asset vernietiging services. |


 ## <a name="nist-800-53-control-mp-5a"></a>NIST 800 53 besturingselement MP-5.

#### <a name="media-transport"></a>Media-Transport

**MP-5.** beveiligt en bepaalt de organisatie [toewijzing: organisatie gedefinieerde typen informatie Systeemmedia] tijdens het transport buiten gecontroleerde gebieden met [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure digitaal medium op Microsoft-datacenters bestaan van servers, netwerkapparaten, en magnetische back-uptapes en schijven, indien van toepassing. Microsoft-datacenters gebruik geen niet-digitaal medium. Microsoft maakt gebruik van drie methoden, zodat de media die buiten het datacenter vervoer te beveiligen: 1) de beveiligde Transport, 2) versleuteling 3) opschonen, verwijderen of vernietigen. |


 ## <a name="nist-800-53-control-mp-5b"></a>NIST 800 53 besturingselement MP-5.b

#### <a name="media-transport"></a>Media-Transport

**MP-5.b** de organisatie onderhoudt accountability voor informatie Systeemmedia tijdens het transport buiten gecontroleerde gebieden.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure onderhoudt accountability voor bedrijfsmiddelen die met het datacenter met behulp van de richtlijnen van NIST SP 800 88 verlaten: consistente opschoon/opschonen, vernietiging asset, versleuteling, nauwkeurige inventariseren, bijhouden en bescherming van bewakingsketen tijdens het transport. |


 ## <a name="nist-800-53-control-mp-5c"></a>NIST 800 53 besturingselement MP-5.c

#### <a name="media-transport"></a>Media-Transport

**MP-5.c** activiteiten die zijn gekoppeld aan het transport van informatie Systeemmedia en documenten desgewenst de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure onderhoudt records van de inventarisatie vóór transport, tracerings- en bescherming van bewakingsketen tijdens het transport, asset reinigen/opschonen, vernietiging asset, ontvangst van activa en voorraad transport-validatie. |


 ## <a name="nist-800-53-control-mp-5d"></a>NIST 800 53 besturingselement MP-5.d

#### <a name="media-transport"></a>Media-Transport

**MP-5.d** de organisatie beperkt de activiteiten die zijn gekoppeld aan het transport van informatie Systeemmedia tot bevoegd personeel.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | De activiteiten van asset transport beperkt Microsoft Azure tot bevoegd personeel via de bescherming van de keten van bewakingsketen. Het gebruik van vergrendelingen, kunnen knoeien bewijs verzegeling en validatie van de asset voorraden vereisen zorgt ervoor dat alleen bevoegd personeel die zijn betrokken bij het transport actief zijn. |


 ### <a name="nist-800-53-control-mp-5-4"></a>NIST 800 53 besturingselement MP-5 (4)

#### <a name="media-transport--cryptographic-protection"></a>Media Transport | Cryptografische bescherming

**MP-5 (4)** cryptografische mechanismen om de vertrouwelijkheid en integriteit van gegevens op digitaal medium tijdens het transport buiten gecontroleerde gebieden te beveiligen door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure gebruik wordt gemaakt Data Protection Service (DP's) voor het beheren van cryptografische sleutels met behulp van een FIPS 140-2-3-gevalideerde versleuteling op bestandsniveau module (cert #1694) en HSM (cert #1178) om AES 256-bits gecodeerde gegevens op de tapes magnetische te beveiligen. |


 ## <a name="nist-800-53-control-mp-6a"></a>NIST 800 53 besturingselement MP-6.a

#### <a name="media-sanitization"></a>Media-opschoning

**MP-6.a** sanitizes van de organisatie [toewijzing: organisatie gedefinieerde informatie Systeemmedia] vóór de verwijdering, vrijgeven buiten de organisatie-besturingselement of vrij te geven voor het gebruik van hergebruik [toewijzing: opschoning organisatie gedefinieerde-technieken en -procedures] volgens de toepasselijke federale en organisatie-standaarden en beleidsregels.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure vereist digitale media in de Microsoft Azure-datacenter-omgeving worden gereinigd/opgeschoond met behulp van hulpprogramma's van Microsoft Azure is goedgekeurd en in samenhang met de NIST SP 800-88 richtlijnen voor opschoning van de Media, alvorens wordt hergebruikt of verwijderd . Niet-digitaal medium wordt niet gebruikt door Microsoft Azure in het datacenteromgeving. |


 ## <a name="nist-800-53-control-mp-6b"></a>NIST 800 53 besturingselement MP-6.b

#### <a name="media-sanitization"></a>Media-opschoning

**MP-6.b** veiligheidsmaatregelen opschoning mechanismen met de sterkte en de integriteit van overeenstemming met de beveiligingscategorie of classificatie van de gegevens voor de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure gebruikt gegevens verwijdering eenheden en processen voor het opschonen/leegmaken als u gegevens in samenhang met de NIST SP 800 88 en die evenredig is met de Microsoft Azure asset classificatie van de activa. Voor de activa vernietiging vereisen, maakt gebruik van Microsoft Azure beveiligingspersoneel asset vernietiging services. |


 ### <a name="nist-800-53-control-mp-6-1"></a>NIST 800 53 besturingselement MP-6 (1)

#### <a name="media-sanitization--review--approve--track--document--verify"></a>Media-opschoning | Bekijk / goedkeuren / bijhouden / Document / controleren

**MP-6 (1)** de organisatie beoordeelt, goedkeurt, houdt, documenten en controleert of de acties voor opschoning en verwijdering van media.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure is media opschoning procedures overeenkomstig de richtlijnen in NIST SP 800 88 geïmplementeerd voor de Asset classificatie standaard- en Asset beveiliging. Alle magnetische of elektronische media is door de volgende NIST SP 800 88 specificaties in overeenstemming met de Azure activaclassificatie gereinigd/opgeschoond. Azure maakt gebruik van gegevens verwijdering eenheden van Extreme Protocol oplossingen (EPS). EPS software ondersteunt NIST SP 800 88 vereisten voor het reinigen en Beveilig opschonen/verwijdering. |


 ### <a name="nist-800-53-control-mp-6-2"></a>NIST 800 53 besturingselement MP-6 (2)

#### <a name="media-sanitization--equipment-testing"></a>Media-opschoning | Apparatuur testen

**MP-6 (2)** de organisatie test opschoning apparatuur en procedures [toewijzing: organisatie gedefinieerde frequentie] om te controleren dat de beoogde opschoning wordt wordt bereikt.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure gebruikt gegevens verwijdering eenheden en processen voor gegevens in samenhang met de NIST SP 800 88 opschonen/opschonen. Per 180 dagen DC's operations test verwijdering eenheden van de Microsoft Azure-gegevens en het proces voor verwijdering. In de test DC's operations verifieert dat de beoogde opschoning wordt wordt bereikt door middel van een forensische analyse van geteste harde schijven om te bevestigen dat de gegevens heeft is opgeschoonde door de gegevens wissen-eenheden |


 ### <a name="nist-800-53-control-mp-6-3"></a>NIST 800 53 besturingselement MP-6 (3)

#### <a name="media-sanitization--nondestructive-techniques"></a>Media-opschoning | Niet-destructieve technieken

**MP-6 (3)** de organisatie niet-destructieve opschoning technieken geldt voor draagbare opslagapparaten voordat dergelijke apparaten verbinding laten maken met het informatiesysteem in de volgende omstandigheden: [toewijzing: organisatie gedefinieerd omstandigheden vereisen opschoning van draagbare opslagapparaten].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure zorgt ervoor dat Azure-datacenters volgt u de hulpprogramma's en verwisselbare Media Security-Procedure in het adresboek Data Center-Services worden uitgevoerd om te voorkomen dat de infectie van de overheid-omgeving door schadelijke software op een draagbaar opslagapparaat. De procedure wordt opgegeven dat de volgende acties worden genomen met USB-stations voor gebruik in de overheid-omgeving: <br /> (1) de USB-stations wanneer de stations eerst worden gekocht bij de fabrikant of de leverancier voordat u het eerste gebruik, of wanneer opnieuw wordt gebruikt voor een ander hulpprogramma-indeling. <br /> (2) een USB-station moet worden gebruikt in een gebied Government aangewezen voor malware, voordat u het station in het gebied scannen. <br /> (3) nadat u een station binnen een gebied Government aangewezen formatteren van de schijf voordat u het gedeelte verlaat. <br /> De hulpprogramma's en verwisselbare Media beveiliging Procedure is ook vereist dat alle verloren, verloren, gestolen of verkeerd geplaatste miniatuur stations worden nooit opnieuw geïntroduceerd in Azure-datacenters, maar dat ze in plaats daarvan worden gecatalogiseerd en vernietigd. |


 ## <a name="nist-800-53-control-mp-7"></a>NIST 800 53 besturingselement MP-7

#### <a name="media-use"></a>Media gebruiken

**MP-7** de organisatie [selectie: beperkt; verbiedt] het gebruik van [toewijzing: organisatie gedefinieerde typen informatie Systeemmedia] op [toewijzing: organisatie gedefinieerde informatiesystemen of onderdelen van het systeem] met [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure vereist asset-eigenaren hun activa met een activaclassificatie toewijzen en geen activa zijn uitgesloten van deze vereiste. In de Microsoft Azure-datacenter-omgeving Raadpleeg activa servers en netwerkapparaten. Andere digitale media zoals USB-flash/USB-stations worden beheerd door specifieke beleidsregels en procedures betreffende hoe deze apparaten worden beheerd. CD/DVD's worden niet gebruikt. Niet-digitaal medium wordt niet gebruikt in het datacenter. Het gebruik van digitaal medium in Microsoft Azure datacenter omgevingen is bewaakte 24 x 7 via CCTV dekking. PE-06-Zie voor meer informatie. |


 ### <a name="nist-800-53-control-mp-7-1"></a>NIST 800 53 besturingselement MP-7 (1)

#### <a name="media-use--prohibit-use-without-owner"></a>Gebruik van Media | Gebruik zonder eigenaar verbieden

**MP-7 (1)** de organisatie verbiedt het gebruik van draagbare opslagapparaten in organisatiegegevens systemen wanneer deze apparaten geen persoonsgegevens eigenaar hebben.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen door de klant bewaakte medium binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft beperkt het gebruik van de schrijfbaar, verwisselbare media naar medium dat expliciet is goedgekeurd door datacenterbeheer via de DC's hulpprogramma's en verwisselbare Media Procedure. Media die persoonlijk eigendom is of heeft geen persoonsgegevens eigenaar is niet toegestaan in een productiegebied zoals aangegeven in het document Microsoft Datacenter werk regels en voorschriften kunnen zorgen. |
