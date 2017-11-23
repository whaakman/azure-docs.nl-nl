---
title: FedRAMP Azure blauwdruk Automation - Configuratiebeheer
description: Webtoepassingen voor FedRAMP - Configuratiebeheer
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 5b953c0d-236f-4b61-b2c5-df2199490c73
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: e93aa430b7150f07210f5d1f37e2027d95334a59
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="configuration-management-cm"></a>Configuratiebeheer (CM)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-cm-1"></a>NIST 800 53 besturingselement CM-1

#### <a name="configuration-management-policy-and-procedures"></a>Beleid voor configuratie en -Procedures

**CM-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een configuration management-beleid die zijn gericht op doel, bereik, rollen, verantwoordelijkheden, management-streven samenwerking tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van de configuration management-beleid en de bijbehorende configuration management-besturingselementen; en controleert en updates van het huidige configuration management-beleid [toewijzing: organisatie gedefinieerde frequentie]; configuratieprocedures en beheer [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Op bedrijfsniveau configuration management-beleid en de procedures van de klant mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-2"></a>NIST 800 53 besturingselement CM-2

#### <a name="baseline-configuration"></a>Basislijn Configuration

**CM-2** de organisatie ontwikkelt, documenten en onderhoudt onder het Configuratiebeheer, een huidige basislijnconfiguratie van het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De Azure Resource Manager-sjablonen en de bijbehorende bronnen waaruit deze Azure blauwdruk vertegenwoordigen een basislijn '-configuratie als code' voor de geïmplementeerde architectuur. De oplossing is via GitHub, die kan worden gebruikt voor configuratie-besturingselement opgegeven. De oplossing omvat een basislijn Desired State Configuration (DSC) voor elke geïmplementeerde virtuele machine. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-2-1a"></a>NIST 800 53 besturingselement CM-2 (1) een

#### <a name="baseline-configuration--reviews-and-updates"></a>Basislijnconfiguratie | Revisies en Updates

**CM-2 (1) een** de organisatie controleert en werkt u de basislijnconfiguratie van het informatiesysteem [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren en bijwerken van de basislijnconfiguratie van de klant geïmplementeerde resources (om op te nemen toepassingen, besturingssystemen, databases en software). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-2-1b"></a>(((1) .b van NIST 800 53 besturingselement CM-2

#### <a name="baseline-configuration--reviews-and-updates"></a>Basislijnconfiguratie | Revisies en Updates

**CM-2 (1) .b** controleert en werkt u de basislijnconfiguratie van het informatiesysteem indien nodig vanwege [toewijzing organisatie gedefinieerd omstandigheden] van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren en bijwerken van de basislijnconfiguratie van de klant geïmplementeerde resources indien nodig. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-2-1c"></a>(((1) .c van NIST 800 53 besturingselement CM-2

#### <a name="baseline-configuration--reviews-and-updates"></a>Basislijnconfiguratie | Revisies en Updates

**CM-2 (1) .c** controleert en werkt u de basislijnconfiguratie van het informatiesysteem als integraal onderdeel van de informatie system component installaties en upgrades van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren en bijwerken van de basislijnconfiguratie van de klant geïmplementeerde resources indien nodig. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-2-2"></a>NIST 800 53 besturingselement CM-2 (2)

#### <a name="baseline-configuration--automation-support-for-accuracy--currency"></a>Basislijnconfiguratie | Ondersteuning voor automatisering op juistheid / valuta

**CM-2 (2)** veiligheidsmaatregelen geautomatiseerde mechanismen voor het onderhouden van een actuele, volledige nauwkeurige en direct beschikbaar basislijnconfiguratie van het informatiesysteem voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De Azure Resource Manager-sjablonen en de bijbehorende bronnen waaruit deze Azure blauwdruk vertegenwoordigen een basislijn '-configuratie als code' voor de geïmplementeerde architectuur. De oplossing is via GitHub, die kan worden gebruikt voor configuratie-besturingselement opgegeven. In de Azure portal een automatiseringsscript is beschikbaar voor alle geïmplementeerde resources en biedt een altijd actuele weergave van deze bronnen.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-2-3"></a>NIST 800 53 besturingselement CM-2 (3)

#### <a name="baseline-configuration--retention-of-previous-configurations"></a>Basislijnconfiguratie | Bewaren van vorige configuraties

**CM-2 (3)** behoudt de organisatie [toewijzing: vorige versies van basislijnconfiguraties van het informatiesysteem organisatie gedefinieerd] voor de hersteloptie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het bewaren van eerdere versies van basislijnconfiguraties voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-2-7a"></a>NIST 800 53 CM-2 (7) een beheren

#### <a name="baseline-configuration--configure-systems-components-or-devices-for-high-risk-areas"></a>Basislijnconfiguratie | Configureren van systemen, onderdelen of apparaten voor met een hoog risico gebieden

**CM-2 (7) een** problemen met de organisatie [toewijzing: organisatie gedefinieerd informatiesystemen, onderdelen van het systeem of apparaten] met [toewijzing: configuraties organisatie gedefinieerd] naar personen onderweg op locaties die de organisatie acht van aanzienlijke risico.

**Verantwoordelijkheden:**`Not Applicable`

|||
|---|---|
| **De klant** | Er zijn geen door de klant bewaakte fysieke apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Microsoft Azure klant inhoud wordt nooit opgeslagen buiten Microsoft Azure, die zich fysiek binnen de Europese Verenigde Staten. Microsoft Azure-personeel reizen niet met apparaten die zijn opgenomen in de inventarisatie van Microsoft Azure. Als zodanig is dit besturingselement niet van toepassing op Microsoft Azure. |


 ### <a name="nist-800-53-control-cm-2-7b"></a>NIST 800 53 CM-2 (7) .b beheren

#### <a name="baseline-configuration--configure-systems-components-or-devices-for-high-risk-areas"></a>Basislijnconfiguratie | Configureren van systemen, onderdelen of apparaten voor met een hoog risico gebieden

**CM-2 (7) .b** de organisatie van toepassing is [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] op de apparaten wanneer personen retourneren.

**Verantwoordelijkheden:**`Not Applicable`

|||
|---|---|
| **De klant** | Er zijn geen door de klant bewaakte fysieke apparaten binnen het bereik van systemen die zijn geïmplementeerd in Azure. |
| **Provider (Microsoft Azure)** | Inhoud van de Microsoft Azure-klant nooit buiten de Microsoft Azure is opgeslagen en medewerkers van Microsoft Azure niet reizen met apparaten die zijn opgenomen in de inventarisatie van Microsoft Azure, dus dit besturingselement is niet van toepassing. |


 ## <a name="nist-800-53-control-cm-3a"></a>NIST 800 53 besturingselement CM-3.

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.** bepaalt welke wijzigingen in het systeem gegevens die gecontroleerd door configuratie worden van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het bepalen welke typen wijzigingen klant geïmplementeerde resources (inclusief toepassingen, besturingssystemen, databases en software) worden gecontroleerd door configuratie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-3b"></a>NIST 800 53 besturingselement CM-3.b

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.b** de organisatie beoordeelt de voorgestelde gecontroleerd door configuratie wijzigingen in het informatiesysteem en keurt deze goed of disapproves dergelijke wijzigingen met expliciete overweging voor beveiliging impact analyses.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren van de voorgestelde gecontroleerd door configuratie wijzigingen aan de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-3c"></a>NIST 800 53 besturingselement CM-3.c

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.c** de configuratie van de organisatie-documenten die zijn gekoppeld aan het informatiesysteem beslissingen wijzigen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het documenteren gecontroleerd door configuratie wijzigingen die zijn gekoppeld aan de klant geïmplementeerde resources (Zie CM 03.b). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-3d"></a>NIST 800 53 besturingselement CM-3.d

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.d** de organisatie implementeert goedgekeurde gecontroleerd door configuratie wijzigingen in het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het implementeren van configuratie beheerd wijzigingen in de CM-03.b goedgekeurd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-3e"></a>NIST 800 53 besturingselement CM-3.e

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.e** De organisatie behoudt records gecontroleerd door configuratie wijzigingen van het systeem voor [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het bewaren van een record van wijzigingen gecontroleerd door configuratie tot bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-3f"></a>NIST 800 53 besturingselement CM-3.f

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.f** de organisatie controles en beoordelingen van activiteiten die zijn gekoppeld aan configuratie beheerd wijzigingen in het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren en beoordelen van wijzigingen in de configuratie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-3g"></a>NIST 800 53 besturingselement CM-3.g

#### <a name="configuration-change-control"></a>Configuratie-wijzigingsbeheer

**CM 3.g** de organisatie worden gecoördineerd en toezicht biedt voor configuratie wijzigen-controleactiviteiten via [toewijzing: organisatie gedefinieerde configuratie wijzigen controle-element (bijvoorbeeld Comité, mededelingenbord)] die bijeen [selectie () een of meer): [toewijzing: organisatie gedefinieerde frequentie]; [Toewijzing: organisatie gedefinieerde configuratie wijzigen-voorwaarden]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het coördineren en toezicht voor configuratie wijzigen-controleactiviteiten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-1a"></a>NIST 800 53 besturingselement CM-3 (1) een

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Beheer van configuratie wijzigen | Automatische Document / melding / verbieden van wijzigingen

**CM-3 (1) een** veiligheidsmaatregelen geautomatiseerde mechanismen om vast te leggen voorgestelde wijzigingen in het informatiesysteem voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van geautomatiseerde mechanismen om vast te leggen voorgestelde wijzigingen (Zie CM 03.b). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-1b"></a>NIST 800 53 besturingselement CM-3 (1) .b

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Beheer van configuratie wijzigen | Automatische Document / melding / verbieden van wijzigingen

**CM-3 (1) .b** veiligheidsmaatregelen geautomatiseerde mechanismen om u te waarschuwen voor de organisatie [toewijzing: ingedeeld gedefinieerde goedkeuringsinstanties] met voorgestelde wijzigingen in de informatie systeem en aanvraag goedkeuring wijzigen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van een geautomatiseerde mechanisme route en vraag goedkeuring voor voorgestelde wijzigingen aan de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-1c"></a>NIST 800 53 besturingselement CM-3 (1) .c

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Beheer van configuratie wijzigen | Automatische Document / melding / verbieden van wijzigingen

**CM-3 (1) .c** geautomatiseerde mechanismen om te markeren voorgestelde wijzigingen in het systeem voor gegevens die niet zijn goedgekeurd of afgekeurde door de veiligheidsmaatregelen voor de organisatie [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van een automatisch mechanisme om te markeren verwerkte wijziging voorstellen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-1d"></a>NIST 800 53 besturingselement CM-3 (1) .d

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Beheer van configuratie wijzigen | Automatische Document / melding / verbieden van wijzigingen

**CM-3 (1) .d** veiligheidsmaatregelen geautomatiseerde mechanismen om te voorkomen dat wijzigingen in het informatiesysteem totdat aangewezen goedkeuringen zijn ontvangen voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van een automatisch mechanisme om te voorkomen dat de implementatie van niet-goedgekeurde wijzigingen aan de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-1e"></a>NIST 800 53 besturingselement CM-3 (1) CF,o.e

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Beheer van configuratie wijzigen | Automatische Document / melding / verbieden van wijzigingen

**CF,o.e CM-3 (1)** De organisatie de veiligheidsmaatregelen voor geautomatiseerde mechanismen om vast te leggen van alle wijzigingen in het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van een automatisch mechanisme om vast te leggen van alle veranderingen aan resources klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-1f"></a>NIST 800 53 besturingselement CM-3 (1) .f

#### <a name="configuration-change-control--automated-document--notification--prohibition-of-changes"></a>Beheer van configuratie wijzigen | Automatische Document / melding / verbieden van wijzigingen

**CM-3 (1) .f** veiligheidsmaatregelen geautomatiseerde mechanismen om u te waarschuwen voor de organisatie [toewijzing: personeel organisatie gedefinieerd] waarop goedgekeurde wijzigingen in het informatiesysteem zijn voltooid.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van een automatisch mechanisme voor meldingen bij de klant geïmplementeerde resources goedgekeurde wijzigingen zijn voltooid. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-2"></a>NIST 800 53 besturingselement CM-3 (2)

#### <a name="configuration-change-control--test--validate--document-changes"></a>Beheer van configuratie wijzigen | Test / valideren / documentwijzigingen

**CM-3 (2)** de organisatie test, valideert en wijzigingen in het informatiesysteem voordat u de wijzigingen op het operationele systeem implementeert en documenten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het testen, valideren en documenteren wijzigingen aan klant geïmplementeerde resources voorafgaand aan de implementatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-4"></a>NIST 800 53 besturingselement CM-3 (4)

#### <a name="configuration-change-control--security-representative"></a>Beheer van configuratie wijzigen | Beveiliging vertegenwoordiger

**CM-3 (4)** de organisatie vereist een medewerker van de beveiliging informatie lid zijn van de [toewijzing: organisatie gedefinieerde configuratie wijzigen-besturingselement element].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het toewijzen van een medewerker van de beveiliging informatie lid zijn van de wijziging besturingselement element gedefinieerd in de CM-03.g. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-3-6"></a>NIST 800 53 besturingselement CM-3 (6)

#### <a name="configuration-change-control--cryptography-management"></a>Beheer van configuratie wijzigen | Cryptografie-Management

**CM-3 (6)** de organisatie zorgt ervoor dat cryptografische mechanismen gebruikt om te geven [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] onder beheer van de configuratie zijn.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het ervoor te zorgen dat cryptografische mechanismen onder beheer van de configuratie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-4"></a>NIST 800 53 besturingselement CM-4

#### <a name="security-impact-analysis"></a>Impactanalyse beveiliging

**CM-4** de organisatie analyseert wijzigingen in het informatiesysteem om te bepalen van de mogelijke gevolgen voor beveiliging voorafgaande aan de implementatie wijzigen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het analyseren van de voorgestelde wijzigingen aan de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-4-1"></a>NIST 800 53 besturingselement CM-4 (1)

#### <a name="security-impact-analysis--separate-test-environments"></a>Beveiliging impactanalyse | Afzonderlijke testomgevingen

**CM-4 (1)** de organisatie analyseert wijzigingen in het informatiesysteem in een afzonderlijke testomgeving voorafgaand aan de implementatie in een operationele omgeving, zoekt u beveiliging effecten vanwege fouten, zwakke punten, incompatibiliteit of opzettelijk malice.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het analyseren van de voorgestelde wijzigingen tot bronnen in een testomgeving vóór implementatie in een operationele omgeving klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-5"></a>NIST 800 53 besturingselement CM-5

#### <a name="access-restrictions-for-change"></a>Toegangsbeperkingen voor wijziging

**CM-5** de organisatie definieert, documenten, goedkeurt en beperkingen van de fysieke en logische toegang die is gekoppeld aan de wijzigingen in het informatiesysteem wordt afgedwongen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure Active Directory account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan rollen bieden strikt bepalen via welke gebruikers kunnen bekijken en controle geïmplementeerd resources. Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan beveiligingsgroepen. Deze beveiligingsgroepen bepaalt de acties die gebruikers met betrekking tot de configuratie van besturingssysteem uitvoeren kunnen. Deze regelingen op basis van rollen kunnen worden uitgebreid door de klant om missie behoeften te voldoen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-5-1"></a>NIST 800 53 besturingselement CM-5 (1)

#### <a name="access-restrictions-for-change--automated-access-enforcement--auditing"></a>Toegang tot de beperkingen voor wijziging | Automatische toegang afdwingen / controle

**CM-5 (1)** het informatiesysteem toegangsbeperkingen wordt afgedwongen en de controle van de afdwingacties ondersteunt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Azure Active Directory account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan rollen bieden strikt bepalen via welke gebruikers kunnen bekijken en controle geïmplementeerd resources. Active Directory-account rechten zijn geïmplementeerd met behulp van op rollen gebaseerde toegangsbeheer gebruikers toe te wijzen aan beveiligingsgroepen. Deze beveiligingsgroepen bepaalt de acties die gebruikers met betrekking tot de configuratie van besturingssysteem uitvoeren kunnen. Alle toegangspogingen en toegangspogingen worden gecontroleerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-5-2"></a>NIST 800 53 besturingselement CM-5 (2)

#### <a name="access-restrictions-for-change--review-system-changes"></a>Toegang tot de beperkingen voor wijziging | Bekijk de wijzigingen in het systeem

**CM-5 (2)** de organisatie beoordeelt wijzigingen in het systeem informatie [toewijzing: organisatie gedefinieerde frequentie] en [toewijzing: omstandigheden organisatie gedefinieerd] om te bepalen of niet-geautoriseerde wijzigingen zijn opgetreden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren van wijzigingen aan de klant geïmplementeerd bronnen om te bepalen of niet-geautoriseerde wijzigingen zijn opgetreden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-5-3"></a>NIST 800 53 besturingselement CM-5 (3)

#### <a name="access-restrictions-for-change--signed-components"></a>Toegang tot de beperkingen voor wijziging | Ondertekende onderdelen

**CM-5 (3)** het informatiesysteem wordt voorkomen dat de installatie van [toewijzing: organisatie gedefinieerde software en -firmware onderdelen] zonder verificatie dat het onderdeel digitaal is ondertekend met een certificaat dat wordt herkend en goedgekeurd door de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Virtuele machines die zijn geïmplementeerd door deze blauwdruk Azure implementeren Windows AppLocker om op te geven welke gebruikers kunnen installeren en/of specifieke toepassingen worden uitgevoerd. Verder, alle Windows-besturingssysteemupdates worden digitale handtekening. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-5-5a"></a>NIST 800 53 CM-5 (5) een beheren

#### <a name="access-restrictions-for-change--limit-production--operational-privileges"></a>Toegang tot de beperkingen voor wijziging | Beperken van productie / operationele bevoegdheden

**CM-5 (5) een** bevoegdheden als onderdelen van het systeem en informatie over het systeem in een productie- of operationele omgeving wilt wijzigen door de organisatie wordt beperkt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beperken van bevoegdheden te wijzigen in productie klant geïmplementeerd of operationele omgevingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-5-5b"></a>NIST 800 53 CM-5 (5) .b beheren

#### <a name="access-restrictions-for-change--limit-production--operational-privileges"></a>Toegang tot de beperkingen voor wijziging | Beperken van productie / operationele bevoegdheden

**CM-5 (5) .b** de organisatie controleert en bevoegdheden reevaluates [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren en reevaluating bevoegdheden die zijn gedefinieerd in een van de CM-05 (05). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-6a"></a>NIST 800 53 besturingselement CM-6.a

#### <a name="configuration-settings"></a>Configuratie-instellingen

**CM 6.a** vaststelt en documenten van configuratie-instellingen voor producten van technologie informatie van het in de informatie systeem met behulp van de organisatie [toewijzing: beveiliging organisatie gedefinieerde configuratie controlelijsten] die overeenstemming de meest beperkende modus consistent zijn met de operationele vereisten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk bevat een basislijn Desired State Configuration (DSC) voor elke geïmplementeerde virtuele machine. Deze declaratieve PowerShell-scripts definiëren en configureer de bronnen waarop ze worden toegepast. De basislijn DSC opgenomen voor resources die zijn geïmplementeerd met deze oplossing kan worden uitgebreid door de klant om missie behoeften te voldoen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-6b"></a>NIST 800 53 besturingselement CM-6.b

#### <a name="configuration-settings"></a>Configuratie-instellingen

**CM 6.b** de organisatie implementeert de configuratie-instellingen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk bevat een basislijn Desired State Configuration (DSC) voor elke geïmplementeerde virtuele machine. De basislijnen wordt automatisch toegepast op virtuele machines tijdens de implementatie van de virtuele machine-extensie voor aangepaste scripts gebruiken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-6c"></a>NIST 800 53 besturingselement CM-6.c

#### <a name="configuration-settings"></a>Configuratie-instellingen

**CM 6.c** de organisatie identificeert, documenten en keurt deze goed eventuele afwijkingen van vastgestelde configuratie-instellingen voor [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie] op basis van [toewijzing: organisatie gedefinieerd operationele vereisten].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het identificeren van documenteren en eventuele afwijkingen van vastgestelde configuratie-instellingen voor de klant geïmplementeerde resources goedkeuren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-6d"></a>NIST 800 53 besturingselement CM-6.d

#### <a name="configuration-settings"></a>Configuratie-instellingen

**CM 6.d** organisatie monitors en besturingselementen wijzigingen in de configuratie-instellingen in overeenstemming met organisatie-beleidsregels en procedures.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de Automation DSC. Automation DSC-machineconfiguraties met een specifieke organisatie gedefinieerde configuratie uitgelijnd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-6-1"></a>NIST 800 53 besturingselement CM-6 (1)

#### <a name="configuration-settings--automated-central-management--application--verification"></a>Configuratie-instellingen | Geautomatiseerd Centraal beheer / toepassing / verificatie

**CM-6 (1)** geautomatiseerde mechanismen centraal beheren, toepassen en controleer of de configuratie-instellingen voor de veiligheidsmaatregelen voor de organisatie [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert Azure Automation DSC. Automation DSC worden uitgelijnd met een specifieke organisatie gedefinieerde configuratie-machineconfiguraties en voortdurend wordt gecontroleerd op wijzigingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-6-2"></a>NIST 800 53 besturingselement CM-6 (2)

#### <a name="configuration-settings--respond-to-unauthorized-changes"></a>Configuratie-instellingen | Reageren op niet-geautoriseerde wijzigingen

**CM-6 (2)** maakt gebruik van de organisatie [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] om te reageren op niet-geautoriseerde wijzigingen aan [toewijzing: organisatie gedefinieerde configuratie-instellingen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert Azure Automation DSC. Onderdeel van Azure van Operations Management Suite (OMS), Automation DSC kan worden geconfigureerd voor het genereren van een waarschuwing of te verhelpen door middel van configuratiefouten wanneer gedetecteerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-7a"></a>NIST 800 53 besturingselement CM-7.

#### <a name="least-functionality"></a>Minimaal functionaliteit

**CM 7.** de organisatie zorgt ervoor dat gegevens alleen essentiële mogelijkheden bieden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De resources die zijn geïmplementeerd door deze blauwdruk Azure zijn geconfigureerd voor de minimale functionaliteit biedt voor het beoogde doel. Een basislijn Desired State Configuration (DSC) is opgenomen voor elke geïmplementeerde virtuele machine. Deze declaratieve PowerShell-scripts definiëren en configureer de bronnen waarop ze worden toegepast. De basislijn DSC opgenomen voor resources die zijn geïmplementeerd met deze oplossing kan worden uitgebreid door de klant functionaliteit missie behoeften verder te beperken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-7b"></a>NIST 800 53 besturingselement CM-7.b

#### <a name="least-functionality"></a>Minimaal functionaliteit

**CM 7.b** de organisatie verbiedt of beperkt het gebruik van de volgende functies, -poorten, protocollen en/of -services: [toewijzing: organisatie gedefinieerde verboden of beperkte functies, -poorten, protocollen en/of services].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert Azure Application Gateway en netwerk-beveiligingsgroepen voor het gebruik van poorten en protocollen beperken tot alleen die nodig zijn. Toepassingsgateway netwerkbeveiligingsgroepen en DSC-basislijnen voor virtuele machines kunnen verder zijn geconfigureerd door de klant het gebruik van functies, -poorten, protocollen en -services om alleen de benodigde functionaliteit bedoeld beperken. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-7-1a"></a>NIST 800 53 besturingselement CM 7 (1) een

#### <a name="least-functionality--periodic-review"></a>Minimaal functionaliteit | Periodieke controle

**CM-7 (1) een** de organisatie controleert het informatiesysteem [toewijzing: organisatie gedefinieerde frequentie] om onnodige en/of niet-beveiligde functies, poorten, protocollen en services te identificeren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren van de klant geïmplementeerde resources (zodanig dat toepassingen, besturingssystemen, databases en software) om onnodige en/of niet-beveiligde functies, -poorten, protocollen en services te identificeren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-7-1b"></a>(((1) .b van NIST 800 53 besturingselement CM 7

#### <a name="least-functionality--periodic-review"></a>Minimaal functionaliteit | Periodieke controle

**CM-7 (1) .b** de organisatie wordt uitgeschakeld [toewijzing: organisatie gedefinieerde functies, -poorten, protocollen en services in het informatiesysteem geacht onnodige en/of niet-beveiligde].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitschakelen van functies, -poorten, protocollen en services die u hebt vastgesteld dat is onnodig of niet-beveiligd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-7-2"></a>NIST 800 53 besturingselement CM-7 (2)

#### <a name="least-functionality--prevent-program-execution"></a>Minimaal functionaliteit | Voorkomen dat programma-uitvoering

**CM-7 (2)** het informatiesysteem wordt voorkomen dat programma-uitvoering in overeenstemming met [selectie (een of meer): [toewijzing: organisatie gedefinieerd beleid met betrekking tot software programma gebruik en beperkingen]; regels voor het machtigen van de voorwaarden en voorwaarden voor softwaregebruik programma].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het programma kan worden uitgevoerd in overeenstemming met de klant gedefinieerde software programma gebruiksbeleid verhinderen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-7-5a"></a>NIST 800 53 CM 7 (5) een beheren

#### <a name="least-functionality--authorized-software--whitelisting"></a>Minimaal functionaliteit | Software geautoriseerd / Whitelisting

**CM-7 (5) een** identificeert de organisatie [toewijzing: organisatie gedefinieerd softwareprogramma's mogen uitvoeren op het informatiesysteem].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het identificeren van toegestane programma's. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-7-5b"></a>NIST 800 53 CM 7 (5) .b beheren

#### <a name="least-functionality--authorized-software--whitelisting"></a>Minimaal functionaliteit | Software geautoriseerd / Whitelisting

**CM-7 (5) .b** veiligheidsmaatregelen een deny all, toestaan door uitzondering beleid waarmee de uitvoering van toegestane programma's op het informatiesysteem voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van een all-weigeren, toestaan door uitzondering beleid waarmee u de uitvoering van toegestane programma's op resources klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-7-5c"></a>NIST 800 53 CM 7 (5) .c beheren

#### <a name="least-functionality--authorized-software--whitelisting"></a>Minimaal functionaliteit | Software geautoriseerd / Whitelisting

**CM-7 (5) .c** controleert en updates van de lijst met toegestane programma's van de organisatie [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het controleren en bijwerken van de lijst met toegestane programma's. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-8a"></a>NIST 800 53 besturingselement CM-8.

#### <a name="information-system-component-inventory"></a>De systeeminventaris onderdeel informatie

**CM 8.** de organisatie ontwikkelt en documenteert u een inventarisatie van onderdelen van het systeem die nauwkeurig weerspiegelt de huidige informatiesysteem; omvat alle onderdelen binnen de grenzen van de autorisatie van het informatiesysteem; is op de mate van detail noodzakelijk voor het bijhouden en rapporteren; en bevat [toewijzing: organisatie gedefinieerde informatie die voor een effectieve system component accountability geacht].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert alle resources in een Azure Resource Manager-resourcegroep. Azure Resource Manager biedt een altijd actuele lijst met geïmplementeerde resources en kan worden aangepast aan het label en groep resources voor voorraadbeheer. Resources die zijn geïmplementeerd met deze oplossing krijgen een specifieke bron-code die gekoppeld aan de systeemgrens worden kan. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-8b"></a>NIST 800 53 besturingselement CM-8.b

#### <a name="information-system-component-inventory"></a>De systeeminventaris onderdeel informatie

**CM 8.b** de organisatie controleert en updates van de informatie van het onderdeel systeeminventaris [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert alle resources in een Azure Resource Manager-resourcegroep. Azure Resource Manager biedt een altijd actuele lijst met geïmplementeerde resources die beschikbaar is voor controle in de Azure portal. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-8-1"></a>NIST 800 53 besturingselement CM-8 (1)

#### <a name="information-system-component-inventory--updates-during-installations--removals"></a>De systeeminventaris onderdeel informatie | Updates tijdens installaties / verwijderingen

**CM-8 (1)** de organisatie de inventaris van onderdelen van het systeem als integraal onderdeel van de geïnstalleerde onderdelen, verwijderingen en systeemupdates informatie-updates.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert alle resources in een Azure Resource Manager-resourcegroep. De blade resources in de Azure portal geeft een lijst met alle geïmplementeerde resources een altijd actuele inventaris verstrekken die resources worden geïmplementeerd en wordt verwijderd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-8-2"></a>NIST 800 53 besturingselement CM-8 (2)

#### <a name="information-system-component-inventory--automated-maintenance"></a>De systeeminventaris onderdeel informatie | Automatisch onderhoud

**CM-8 (2)** geautomatiseerde mechanismen om u te helpen een actuele, volledige nauwkeurige en direct beschikbaar inventaris van onderdelen van het systeem houden de veiligheidsmaatregelen voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert alle resources in een Azure Resource Manager-resourcegroep. De blade resources in de Azure portal geeft een lijst met alle geïmplementeerde resources een altijd actuele inventaris verstrekken die resources worden geïmplementeerd en wordt verwijderd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-8-3a"></a>NIST 800 53 CM-8 (3) een beheren

#### <a name="information-system-component-inventory--automated-unauthorized-component-detection"></a>De systeeminventaris onderdeel informatie | Automatische detectie van niet-geautoriseerde onderdeel

**CM-8 (3) een** de organisatie de veiligheidsmaatregelen voor geautomatiseerde mechanismen [toewijzing: organisatie gedefinieerde frequentie] om de aanwezigheid van niet-geautoriseerde hardware, software en firmware-onderdelen in het informatiesysteem te detecteren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het nemen van geautomatiseerde methoden voor het detecteren van de aanwezigheid van niet-geautoriseerde software binnen de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-8-3b"></a>NIST 800 53 CM-8 (3) .b beheren

#### <a name="information-system-component-inventory--automated-unauthorized-component-detection"></a>De systeeminventaris onderdeel informatie | Automatische detectie van niet-geautoriseerde onderdeel

**CM-8 (3) .b** de organisatie de volgende acties uitgevoerd als niet-geautoriseerde onderdelen worden gedetecteerd: [selectie (een of meer): Hiermee schakelt u toegang tot het netwerk door deze onderdelen; isoleert de onderdelen; waarschuwt [toewijzing: organisatie gedefinieerd medewerkers of -rollen]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de actie te ondernemen wanneer ongeoorloofde software wordt gedetecteerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-8-4"></a>NIST 800 53 besturingselement CM-8 (4)

#### <a name="information-system-component-inventory--accountability-information"></a>De systeeminventaris onderdeel informatie | Accountability informatie

**CM-8 (4)** informatie system component informatie over de inventaris, een manier om te identificeren door de organisatie bevat [selectie (een of meer): name; positie; rol], personen verantwoordelijk/verantwoordelijk voor het beheren van deze onderdelen .

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert alle resources in een Azure Resource Manager-resourcegroep. Azure-resourcetags zijn sleutel / waarde-paren die kunnen worden ingezet om bronnen voor accountability en/of management-toepassing te categoriseren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-8-5"></a>NIST 800 53 besturingselement CM-8 (5)

#### <a name="information-system-component-inventory--no-duplicate-accounting-of-components"></a>De systeeminventaris onderdeel informatie | Er is geen dubbele Accounting van onderdelen

**CM-8 (5)** de organisatie controleert of alle onderdelen binnen de grenzen van de autorisatie van het systeem gegevens worden niet gedupliceerd in andere informatie system component inventarissen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert alle resources in een Azure Resource Manager-resourcegroep. Azure Resource Manager biedt een altijd actuele lijst met geïmplementeerde resources. Resources die zijn geïmplementeerd met deze oplossing krijgen een specifieke bron-code die gekoppeld aan de systeemgrens worden kan. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-9a"></a>NIST 800 53 besturingselement CM-9.a

#### <a name="configuration-management-plan"></a>Plan voor configuratie

**CM 9.a** de organisatie ontwikkelt, documenten en implementeert u een plan voor configuratie voor het systeem voor gegevens die zijn gericht op functies, verantwoordelijkheden en configuratie processen en procedures.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het ontwikkelen, documenteren en implementeren van een plan voor configuratie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-9b"></a>NIST 800 53 besturingselement CM-9.b

#### <a name="configuration-management-plan"></a>Plan voor configuratie

**CM 9.b** de organisatie ontwikkelt, documenten en implementeert u een plan voor configuratie voor het informatiesysteem waarmee een proces voor bladeren identificeerbaar configuratie-items gedurende de levensduur van de ontwikkeling van systeem en voor de de configuratie van de configuratie-items beheren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het ontwikkelen, documenteren en implementeren van een plan voor configuratie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-9c"></a>NIST 800 53 besturingselement CM-9.c

#### <a name="configuration-management-plan"></a>Plan voor configuratie

**CM 9.c** de organisatie ontwikkelt, documenten en implementeert een plan voor configuratie voor het informatiesysteem die u de configuratie definieert voor het informatiesysteem-items en plaatst vervolgens de configuratie-items onder configuratie beheer.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het ontwikkelen, documenteren en implementeren van een plan voor configuratie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-9d"></a>NIST 800 53 besturingselement CM-9.d

#### <a name="configuration-management-plan"></a>Plan voor configuratie

**CM 9.d** de organisatie ontwikkelt, documenten en een plan voor configuratie voor het informatiesysteem die het plan voor configuratie tegen ongeautoriseerde bekendmaking en de wijzigingsopties beschermt implementeert.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het ontwikkelen, documenteren en implementeren van een plan voor configuratie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-10a"></a>NIST 800 53 besturingselement CM-10.a

#### <a name="software-usage-restrictions"></a>Gebruiksbeperkingen software

**CM 10.a** de organisatie worden gebruikt voor software en de bijbehorende documentatie in overeenstemming met het auteursrecht en contract overeenkomsten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Windows en SQL Server-licenties worden vermeld voor de resources die door deze blauwdruk Azure is geïmplementeerd. Dit is een ingebouwde functie van Azure. Organisaties met bestaande softwarelicentieovereenkomsten overwegen alternatieve licentiemodellen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-10b"></a>NIST 800 53 besturingselement CM-10.b

#### <a name="software-usage-restrictions"></a>Gebruiksbeperkingen software

**CM 10.b** de organisatie houdt het gebruik van software en de bijbehorende documentatie die zijn beveiligd door het aantal licenties om kopiëren en distributiepunten te beheren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Windows en SQL Server-licenties worden vermeld voor de resources die door deze blauwdruk Azure is geïmplementeerd. De gebruiker is niet vereist voor het gebruik van de licenties afzonderlijk bijhouden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-10c"></a>NIST 800 53 besturingselement CM-10.c

#### <a name="software-usage-restrictions"></a>Gebruiksbeperkingen software

**CM 10.c** bepaalt en het gebruik van peer-to-peer-technologie om ervoor te zorgen dat deze mogelijkheid niet wordt gebruikt voor de niet-geautoriseerde distributie, weergeven, prestaties of verveelvoudigen auteursrecht voor bestandsdeling en documenten van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er is geen peer-to-peer-bestand delen mogelijkheid geïmplementeerd door deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-10-1"></a>NIST 800 53 besturingselement CM-10 (1)

#### <a name="software-usage-restrictions--open-source-software"></a>Software-gebruiksbeperkingen | Open-sourcesoftware

**CM-10 (1)** de organisatie de volgende beperkingen voor het gebruik van open-sourcesoftware vaststelt: [toewijzing: organisatie gedefinieerde beperkingen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Van de klant op bedrijfsniveau configuration management-beleid kan omgaan met beperkingen voor het gebruik van open-sourcesoftware. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-11a"></a>NIST 800 53 besturingselement CM-11.a

#### <a name="user-installed-software"></a>Gebruiker geïnstalleerde Software

**CM 11.a** stelt de organisatie [toewijzing: organisatie gedefinieerde beleidsregels] voor de installatie van software door gebruikers.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het tot stand brengen van een beleid voor de installatie van software op bronnen klant geïmplementeerd door gebruikers. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-11b"></a>NIST 800 53 besturingselement CM-11.b

#### <a name="user-installed-software"></a>Gebruiker geïnstalleerde Software

**CM 11.b** de organisatie worden afgedwongen beleid voor software-installatie via [toewijzing: organisatie gedefinieerde methoden].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het afdwingen van beleid voor software-installatie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-cm-11c"></a>NIST 800 53 besturingselement CM-11.c

#### <a name="user-installed-software"></a>Gebruiker geïnstalleerde Software

**CM 11.c** de organisatie bewaakt de beleidsnaleving op [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het bewaken van de naleving van de klant geïmplementeerde resources met de beleidsregels die in de CM-11.a geïdentificeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-cm-11-1"></a>NIST 800 53 besturingselement CM-11 (1)

#### <a name="user-installed-software--alerts-for-unauthorized-installations"></a>Gebruiker geïnstalleerde Software | Waarschuwingen voor niet-geautoriseerde installaties

**CM-11 (1)** de signalen informatie [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] als de niet-geautoriseerde installatie van software is gedetecteerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor een waarschuwing geven wanneer de niet-geautoriseerde installatie van software wordt gedetecteerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |

