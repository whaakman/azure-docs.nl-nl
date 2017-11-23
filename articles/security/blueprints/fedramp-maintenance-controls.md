---
title: FedRAMP Azure blauwdruk Automation - onderhoud
description: Webtoepassingen voor FedRAMP - onderhoud
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 53acae01-bea9-4570-a9bc-734ff65262ba
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: a0546f6e10b04bbfdb5b02e5c0bbe6d907c76e72
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="maintenance-ma"></a>Onderhoud (MA)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-ma-1"></a>NIST 800 53 besturingselement MA-1

#### <a name="system-maintenance-policy-and-procedures"></a>Onderhoud systeembeleid en Procedures

**MA-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een beleid voor het onderhoud van systeem die zijn gericht op doel, bereik, rollen, verantwoordelijkheden, management streven, coördinatie tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van het systeem onderhoudsbeleid en de bijbehorende onderhoud controles; en controleert en updates van het huidige systeem onderhoudsbeleid [toewijzing: organisatie gedefinieerde frequentie]; en onderhoudsprocedures system [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Onderhoudsbeleid op bedrijfsniveau systeem en de procedures van de klant mogelijk voldoende zijn voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-2a"></a>NIST 800 53 besturingselement MA-2.

#### <a name="controlled-maintenance"></a>Gecontroleerde onderhoud

**MA 2.** de organisatie plant, uitvoert, documenten en records van het onderhoud en reparaties op onderdelen van het systeem in overeenstemming met de fabrikant of leverancier specificaties en/of organisatorische vereisten informatie bekijkt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het gecontroleerde onderhoud. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-2b"></a>NIST 800 53 besturingselement MA-2.b

#### <a name="controlled-maintenance"></a>Gecontroleerde onderhoud

**MA 2.b** de organisatie goedkeurt en hiermee alle onderhoudsactiviteiten wordt gecontroleerd of uitgevoerd op de site of op afstand en of de apparatuur wordt verwerkt op de site of verwijderd op een andere locatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het gecontroleerde onderhoud. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-2c"></a>NIST 800 53 besturingselement MA-2.c

#### <a name="controlled-maintenance"></a>Gecontroleerde onderhoud

**MA 2.c** de organisatie vereist dat [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] expliciet de verwijdering van het systeem voor informatie of onderdelen van het systeem van de organisatie faciliteiten voor externe onderhoud of reparaties goedkeuren.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure vereist die eigenschap activa (bijv, netwerkapparaat of server) expliciete asset eigenaar goedkeuring vereisen overdracht offsite hebben. |


 ## <a name="nist-800-53-control-ma-2d"></a>NIST 800 53 besturingselement MA-2.d

#### <a name="controlled-maintenance"></a>Gecontroleerde onderhoud

**MA 2.d** de organisatie sanitizes apparatuur alle gegevens verwijderen van de bijbehorende media vóór de verwijdering van de organisatie faciliteiten voor externe onderhoud of reparaties.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure Asset beveiliging standaard definieert de verwerking vereist voor de overdracht van activa offsite voorzorgsmaatregelen asset. De standaard Asset-beveiliging vereist dat gegevensassets opslag gewist/opgeschoond in samenhang met de NIST SP 800-88, richtlijnen voor opschoning van de Media, vóór het verlaten van het datacenter. |


 ## <a name="nist-800-53-control-ma-2e"></a>NIST 800 53 besturingselement MA-2.e

#### <a name="controlled-maintenance"></a>Gecontroleerde onderhoud

**MA-2.e** De organisatie controleert mogelijk betrokken beveiligingsmechanismen om te controleren of de besturingselementen nog steeds werken goed van de volgende onderhoud of reparatie van de activiteiten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het gecontroleerde onderhoud. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-2f"></a>NIST 800 53 besturingselement MA-2.f

#### <a name="controlled-maintenance"></a>Gecontroleerde onderhoud

**MA 2.f** bevat van de organisatie [toewijzing: organisatie gedefinieerde onderhoud gerelateerde informatie] in de organisatie-onderhoudsrecords.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het gecontroleerde onderhoud. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-2-2a"></a>NIST 800 53 besturingselement MA 2 (2) een

#### <a name="controlled-maintenance--automated-maintenance-activities"></a>Beheerd onderhoud | Van geautomatiseerde onderhoudsactiviteiten

**MA 2 (2) een** veiligheidsmaatregelen geautomatiseerde mechanismen planning, uitvoering, en document onderhoud en reparaties voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het automatiseren van onderhoudsactiviteiten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-2-2b"></a>NIST 800 53 besturingselement MA 2 (2) .b

#### <a name="controlled-maintenance--automated-maintenance-activities"></a>Beheerd onderhoud | Van geautomatiseerde onderhoudsactiviteiten

**MA 2 (2) .b** de organisatie produceert bijgewerkt, nauwkeurig, en alle gegevens over alle acties voor onderhoud en reparatie aangevraagd, gepland in proces en voltooid.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het automatiseren van onderhoudsactiviteiten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-3"></a>NIST 800 53 besturingselement MA-3

#### <a name="maintenance-tools"></a>Hulpprogramma's voor onderhoud

**MA-3** de organisatie goedkeurt, besturingselementen, en bewaakt informatie system-hulpprogramma's voor onderhoud.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure maakt gebruik van verschillende hulpprogramma's voor onderhoud. Softwareonderhoud hulpprogramma's zijn goedgekeurd, gecontroleerd en beheerd via de Microsoft Azure wijzigen en release proces. <br /> Het team siteservices onderhoudt een inventaris van goedgekeurde onderhoud-hulpprogramma's voor gebruik binnen het datacenter (Zie MA 3). Medewerkers Maintenance worden omgeleid naar de opgegeven onderhoud-hulpprogramma's gebruiken. Datacenter goedkeuring is vereist voor hulpprogramma's die niet is geleverd door het datacenter gebruiken. Fysieke hand's (schroevedraaier, wrenches, enz.) zijn uitgesloten van dit besturingselement. <br /> Elke faciliteit bevat een beperkte fysieke vergrendelen in- of lokaal toegang beheerd voor het opslaan van speciale hulpprogramma's voor onderhoud, zoals fluke ether bereiken, fluke fiber channel testers, Ethernet toners, enz. Het team siteservices routinematige inventariscontroles uitgevoerd om Controleer de status van alle hulpprogramma's. Toegang tot of het onderhoud opslagruimte vergrendelen wordt bijgehouden in de access badge lezer logboeken beschikbaar in het geval van een onderzoek zijn. |


 ### <a name="nist-800-53-control-ma-3-1"></a>NIST 800 53 besturingselement MA-3 (1)

#### <a name="maintenance-tools--inspect-tools"></a>Hulpprogramma's voor onderhoud | Hulpprogramma's controleren

**MA-3 (1)** de organisatie inspecteert de hulpmiddelen voor onderhoud uitgevoerd naar een faciliteit door medewerkers van onderhoud voor onjuist of niet-geautoriseerde wijzigingen.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure siteservices team onderhoudt een inventaris van goedgekeurde onderhoud-hulpprogramma's voor gebruik binnen het datacenter (MA-3 Zie voor meer informatie). Medewerkers Maintenance worden omgeleid naar de opgegeven onderhoud-hulpprogramma's gebruiken. DCM-goedkeuring is vereist voor hulpprogramma's die niet is geleverd door het datacenter gebruiken. |


 ### <a name="nist-800-53-control-ma-3-2"></a>NIST 800 53 besturingselement MA-3 (2)

#### <a name="maintenance-tools--inspect-media"></a>Hulpprogramma's voor onderhoud | Inspecteer de Media

**MA-3 (2)** de organisatie media met diagnostische en test-programma's voor schadelijke code controleert voordat het medium in het systeem gegevens worden gebruikt.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure verbiedt het gebruik van mobiel computergebruik of opslagmedia in de productieomgeving van Microsoft Azure-datacenters zonder goedkeuring datacenter. Gebruik van privé-eigendom media is niet toegestaan in de productieomgeving van datacenters van Microsoft Azure kan worden gebruikt. <br /> Microsoft Azure is een proces voor het inspecteren van laptops voordat het wordt gebruikt in de productieomgeving van datacenters van Microsoft Azure worden geïmplementeerd. Beveiliging managers zijn getraind naar uitdaging personeel met behulp van laptops controleren of de laptops hebben ondergaan en inspectie doorgegeven in de productieomgeving. |


 ### <a name="nist-800-53-control-ma-3-3"></a>NIST 800 53 besturingselement MA-3 (3)

#### <a name="maintenance-tools--prevent-unauthorized-removal"></a>Hulpprogramma's voor onderhoud | Voorkomen dat onbevoegde verwijderen

**MA-3 (3)** de organisatie wordt voorkomen dat het niet-geautoriseerde verwijderen van onderhoud apparatuur organisatiegegevens door te controleren of er is geen organisatie-gegevens op de apparatuur; opschonen of vernietigen met de apparatuur; behoud de apparatuur binnen de faciliteit; of het verkrijgen van een uitzondering van [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] expliciet verwijderen van de apparatuur van de faciliteit autoriseren.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | De veiligheidsmaatregelen voor Microsoft Azure datacenter specifieke hulpprogramma's voor onderhoud die binnen de faciliteit worden bewaard en worden niet verwijderd. Elke faciliteit bevat een beperkte fysieke vergrendelen of de opslag lokaal die onderhoud-hulpprogramma's, zoals fluke ether bereiken, fluke fiber channel testers, Ethernet toners, enzovoort opslaat. Toegang wordt beheerd op de ruimte vergrendelen of de opslag in DCAT om te voorkomen dat onbevoegde toegang tot de hulpprogramma's voor het onderhoud. <br /> Organisatiegegevens wordt tijdens onderhoud beveiligd door de besturingselementen in MA-4. Voor toegang tot bedrijfsgegevens moet hebt de gebruiker bevoegde accounts en -verificaties. |


 ## <a name="nist-800-53-control-ma-4a"></a>NIST 800 53 besturingselement MA-4.

#### <a name="nonlocal-maintenance"></a>Niet-lokale onderhoud

**MA 4.** de organisatie goedkeurt en niet-lokale onderhoud en diagnostische activiteiten controleert.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitvoeren van niet-lokale onderhoud op besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-4b"></a>NIST 800 53 besturingselement MA-4.b

#### <a name="nonlocal-maintenance"></a>Niet-lokale onderhoud

**MA 4.b** de organisatie kan het gebruik van niet-lokale onderhoud en diagnostische hulpprogramma's als consistent zijn met organisatiebeleid en gedocumenteerd in het beveiligingsplan voor het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitvoeren van niet-lokale onderhoud op besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-4c"></a>NIST 800 53 besturingselement MA-4.c

#### <a name="nonlocal-maintenance"></a>Niet-lokale onderhoud

**MA 4.c** sterk verificators in de inrichting van niet-lokale onderhoud en diagnostische sessies maakt gebruik van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitvoeren van niet-lokale onderhoud op besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-4d"></a>NIST 800 53 besturingselement MA-4.d

#### <a name="nonlocal-maintenance"></a>Niet-lokale onderhoud

**MA 4.d** de organisatie onderhoudt records voor niet-lokale onderhoud en diagnostische activiteiten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitvoeren van niet-lokale onderhoud op besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-4e"></a>NIST 800 53 besturingselement MA-4.e

#### <a name="nonlocal-maintenance"></a>Niet-lokale onderhoud

**MA-4.e** De organisatie beëindigt sessie en netwerkverbindingen als niet-lokale onderhoud is voltooid.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitvoeren van niet-lokale onderhoud op besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-4-2"></a>NIST 800 53 besturingselement MA-4 (2)

#### <a name="nonlocal-maintenance--document-nonlocal-maintenance"></a>Niet-lokale onderhoud | Niet-lokale onderhoud document

**MA-4 (2)** organisatie documenten in het beveiligingsplan voor het systeem gegevens het beleid en de procedures voor de inrichting en het gebruik van niet-lokale onderhoud en diagnostische verbindingen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het onderhoud van niet-lokale documenteren in de beveiligingsplan voor besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-4-3"></a>NIST 800 53 besturingselement MA-4 (3)

#### <a name="nonlocal-maintenance--comparable-security--sanitization"></a>Niet-lokale onderhoud | Vergelijkbare beveiliging / opschoning

**MA-4 (3)** de organisatie vereist dat niet-lokale onderhoud en diagnostische services worden uitgevoerd vanaf een informatiesysteem die u implementeert een beveiliging functionaliteit die vergelijkbaar is met de mogelijkheid geïmplementeerd op het systeem in behandeling; of verwijderd het onderdeel dat moet worden verwerkt vanuit het informatiesysteem voorafgaand aan de niet-lokale onderhoud of diagnostische services, sanitizes het onderdeel (met betrekking tot organisatiegegevens) vóór de verwijdering van de organisatie faciliteiten, en nadat de service is uitgevoerd, inspecteert en het onderdeel (met betrekking tot mogelijk schadelijke softwareprogramma's) sanitizes voordat u opnieuw verbinding te maken van het onderdeel op het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het uitvoeren van alle niet-lokale onderhoud van de klant geïmplementeerd besturingssystemen van een systeem met vergelijkbare beveiliging. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-4-6"></a>NIST 800 53 besturingselement MA-4 (6)

#### <a name="nonlocal-maintenance--cryptographic-protection"></a>Niet-lokale onderhoud | Cryptografische bescherming

**MA-4 (6)** cryptografische mechanismen ter bescherming van de integriteit en vertrouwelijkheid van niet-lokale onderhoud en diagnostische communicatie door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het implementeren van cryptografische mechanismen bij het uitvoeren van niet-lokale onderhoud en diagnostische gegevens van besturingssystemen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-5a"></a>NIST 800 53 besturingselement MA-5.

#### <a name="maintenance-personnel"></a>Medewerkers Maintenance

**MA 5.** stelt een proces voor het onderhoud personeel autorisatie en houdt een lijst van toegestane onderhoud organisaties of medewerkers van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau onderhoud personeel autorisatie en begeleiding systeemprocessen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-5b"></a>NIST 800 53 besturingselement MA-5.b

#### <a name="maintenance-personnel"></a>Medewerkers Maintenance

**MA 5.b** de organisatie zorgt ervoor dat niet-escorted personeel onderhoud uitvoeren op het informatiesysteem-verificatie ondersteunt vereist.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau onderhoud personeel autorisatie en begeleiding systeemprocessen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-5c"></a>NIST 800 53 besturingselement MA-5.c

#### <a name="maintenance-personnel"></a>Medewerkers Maintenance

**MA 5.c** organisatie medewerkers met de vereiste toegang autorisaties en technische bevoegdheid voor de controle van de onderhoudsactiviteiten van medewerkers die niet in het bezit zijn van de vereiste toegang autorisaties Hiermee geeft u de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau onderhoud personeel autorisatie en begeleiding systeemprocessen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-5-1a"></a>NIST 800 53 besturingselement MA-5 (1) een

#### <a name="maintenance-personnel--individuals-without-appropriate-access"></a>Medewerkers Maintenance | Personen zonder de juiste toegangsrechten

**MA-5 (1) een** de organisatie worden procedures voor het gebruik van onderhoud medewerkers die niet over de juiste machtigingen of inwoners, waaronder de volgende vereisten onderhoud medewerkers die niet nodig kan hebben, zijn niet geïmplementeerd -verificatie ondersteunt, vrije ruimten of formele toegang goedkeuringen zijn escorted en onder supervisie tijdens het uitvoeren van onderhoud en diagnostische activiteiten op het informatiesysteem door goedgekeurde organisatie medewerkers die volledig zijn uitgeschakeld, hebben passende -verificatie ondersteunt, en worden gekwalificeerd technisch; voorafgaand aan de gang worden gezet onderhoud of diagnostische activiteiten door medewerkers die komen niet nodig hebben-verificatie ondersteunt, vrije ruimten of formele toegang goedkeuringen, alle opslagonderdelen vanuit het informatiesysteem zijn opgeschoonde vluchtige gegevens en alle niet-vluchtige opslagmedia zijn verwijderd of fysiek losgekoppeld van het systeem en beveiligd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau onderhoud personeel autorisatie en begeleiding systeemprocessen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-ma-5-1b"></a>NIST 800 53 besturingselement MA-5 (1) .b

#### <a name="maintenance-personnel--individuals-without-appropriate-access"></a>Medewerkers Maintenance | Personen zonder de juiste toegangsrechten

**MA-5 (1) .b** de organisatie ontwikkelt en implementeert alternatieve beveiligingsmaatregelen in het geval van een onderdeel van het systeem gegevens kan niet worden opgeschoonde, verwijderd of losgekoppeld van het systeem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau onderhoud personeel autorisatie en begeleiding systeemprocessen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-ma-6"></a>NIST 800 53 besturingselement MA-6

#### <a name="timely-maintenance"></a>Tijdige onderhoud

**MA-6** de organisatie verkrijgt onderhoud ondersteuning en/of spare onderdelen voor [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie] in [toewijzing: organisatie gedefinieerde periode] van de fout.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure-datacenters onderhouden residente onderhoudspersoneel ter ondersteuning van kritieke datacenter infrastructuursystemen, evenals een datacenter-bewerkingen. De teams kritieke beveiligings- en technologie systeemonderdelen hebt geïdentificeerd die ze handhaven reserveonderdelen voor uw bedrijfslocatie worden gebruikt. Kritieke systemen zijn ontworpen in N + 1 configuraties en services zijn ontworpen om netwerkfouten. Hierdoor kan het datacenter-management-team om te voldoen aan de hersteldoelen in het geval van een onderbreking van de service of een plan voor onvoorziene gebeurtenissen situatie. Essentiële informatie systeemservices ingericht van meer dan één datacenter om te voorkomen dat een onderbreking in de service vanwege een incident op een van de datacentra. Toepassingen van de klant zijn verantwoordelijk voor het implementeren van meerdere datacenters te voorzien in redundantie en tolerantie. |
