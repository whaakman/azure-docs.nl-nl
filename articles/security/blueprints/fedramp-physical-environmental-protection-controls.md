---
title: FedRAMP Azure blauwdruk Automation - beveiliging voor fysieke en omgevingsbeveiliging
description: Webtoepassingen voor FedRAMP - fysieke en omgevingsbeveiliging beveiliging
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 0bf8349b-450d-413c-a535-6f7b80b82781
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 9e9e473285f4e82e1da40b8c3d496d65733fbd45
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="physical-and-environmental-protection-pe"></a>Fysieke en omgevingsbeveiliging beveiliging (PE)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-pe-1"></a>NIST 800 53 besturingselement PE-1

#### <a name="physical-and-environmental-protection-policy-and-procedures"></a>Fysieke en omgevingsbeveiliging beveiligingsbeleid en -Procedures

**PE-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een fysieke en omgevingsbeveiliging protection-beleid die zijn gericht op doel, bereik, rollen, taken, management het streven, samenwerking tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van de fysieke en omgevingsbeveiliging protection-beleid en de bijbehorende fysieke en omgevingsbeveiliging beveiliging besturingselementen; en controleert en updates van het huidige fysieke en omgevingsbeveiliging beveiligingsbeleid [toewijzing: organisatie gedefinieerde frequentie]; en -procedures voor beveiliging van fysieke en omgevingsbeveiliging [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Op bedrijfsniveau fysieke en omgevingsbeveiliging protection-beleid en de procedures van de klant mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-pe-2a"></a>NIST 800 53 besturingselement PE-2.

#### <a name="physical-access-authorizations"></a>Fysieke toegang autorisaties

**PE 2.** de organisatie ontwikkelt, goedkeurt en onderhoudt een lijst met personen met geautoriseerde toegang tot de faciliteit waarin het informatiesysteem zich bevindt.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Fysieke toegang tot een Microsoft-datacentrum moet worden goedgekeurd door het hulpprogramma voor het datacenter toegang Datacenter (GCB)-team. Toewijzingen Access vereist een einddatum, waarna toegang wordt automatisch verwijderd en opnieuw moet worden goedgekeurd. Bovendien wanneer toegang is niet langer vereist, datacenter-afdelingen of management om aan te vragen handmatig de beëindiging van toegang. |


 ## <a name="nist-800-53-control-pe-2b"></a>NIST 800 53 besturingselement PE-2.b

#### <a name="physical-access-authorizations"></a>Fysieke toegang autorisaties

**PE 2.b** autorisatiereferenties voor toegang van de faciliteit problemen met de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Het datacenter toegang hulpprogramma is de gezaghebbende bron weergeven van alle medewerkers met geautoriseerde toegang tot een specifieke datacenter. Het hulpprogramma is gekoppeld aan het datacenter fysieke beveiliging toegang-apparaten worden beheerd en gemachtigd voor toegang op basis van toegangsniveaus die zijn goedgekeurd door het DCM-team. Toegangsniveaus zijn toegewezen in het hulpprogramma aan van een gebruiker Microsoft uitgegeven badge of een tijdelijke toegang badge dat door het besturingselement ruimte Supervisor (CRS) op het datacenter is toegewezen. Toegangsniveaus zijn goedgekeurd door het DCM-team. Naast de referenties die zijn toegewezen aan fysieke badges, moeten bepaalde gebieden van het datacenter ingeschreven biometrische gegevens van de gebruiker (hand geometrie of vingerafdruk). |


 ## <a name="nist-800-53-control-pe-2c"></a>NIST 800 53 besturingselement PE-2.c

#### <a name="physical-access-authorizations"></a>Fysieke toegang autorisaties

**PE 2.c** de organisatie controleert de lijst met gedetailleerde informatie over geautoriseerde faciliteit toegang door personen [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Naast het intrekken van de toegang wordt beschreven in een Microsoft Azure-deel beoordelingen geautoriseerde toegangslijsten voor Azure-datacenters om de 90 dagen om te verwijderen of bij te werken afzonderlijke toegang zo nodig. |


 ## <a name="nist-800-53-control-pe-2d"></a>NIST 800 53 besturingselement PE-2.d

#### <a name="physical-access-authorizations"></a>Fysieke toegang autorisaties

**PE 2.d** de organisatie worden personen verwijderd uit de lijst van de faciliteit toegang wanneer toegang niet langer vereist is.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure wordt toegang automatisch verwijderd wanneer de einddatum van de toegang tot de toewijzing is bereikt. Wanneer de toegang is niet langer vereist, datacenter-afdelingen of management handmatig vraagt om de beëindiging van de toegang in het datacenter toegang hulpprogramma. Microsoft Azure, worden bovendien alle overbodige toegang machtigingen die worden gedetecteerd als gevolg van de toegang lijst revisie beschreven in deel c verwijderd. |


 ## <a name="nist-800-53-control-pe-3a"></a>NIST 800 53 besturingselement PE-3.

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE 3.** autorisaties op fysieke toegang wordt afgedwongen door de organisatie [toewijzing: organisatie gedefinieerd binnenkomt/verlaat verwijst naar de functie waarin het informatiesysteem zich bevindt] door te controleren of afzonderlijke-verificatie ondersteunt voordat u verleent toegang tot de faciliteit; en inkomend en uitgaand voor het gebruik van de faciliteit beheren [selectie (een of meer): [toewijzing: fysieke toegang organisatie gedefinieerd besturingselement systemen/apparaten]; beveiligingen].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure zorgt er fysieke toegang autorisaties voor alle fysieke toegangspunten naar Azure-datacenters met 24 x 7 personeelsbezetting, alarmen videobewaking, meervoudige verificatie en man-trap voor apparaten. |


 ## <a name="nist-800-53-control-pe-3b"></a>NIST 800 53 besturingselement PE-3.b

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE 3.b** fysieke toegang auditlogboeken voor de organisatie worden bijgehouden [toewijzing: organisatie gedefinieerd binnenkomt/verlaat punten].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Toegang tot Azure-datacenter faciliteiten zijn geregistreerd en gecontroleerd. |


 ## <a name="nist-800-53-control-pe-3c"></a>NIST 800 53 besturingselement PE-3.c

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE 3.c** voorziet in de organisatie [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] op de toegang beheren tot gebieden binnen de faciliteit officieel aangewezen als openbaar toegankelijk.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Azure-datacenters bevatten geen gebieden die zijn aangewezen als openbaar toegankelijk. |


 ## <a name="nist-800-53-control-pe-3d"></a>NIST 800 53 besturingselement PE-3.d

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE 3.d** escorts bezoekers en controleert bezoeker activiteit van de organisatie [toewijzing: organisatie gedefinieerd gevallen bezoeker escorts vereisen en bewaking].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Alle bezoekers die zijn goedgekeurd door de toegang tot het datacenter (Zie PE-2) zijn aangewezen als 'Begeleiding Only' op hun badges of via andere visueel (bijv, gekleurde badges) en zijn vereist om te blijven met hun escorts te allen tijde. Escorted bezoekers hoeft niet alle toegangsniveaus is verleend en kunnen alleen op reis bent op de toegang van hun escorts. Escorts bewaken alle activiteiten van hun bezoeker terwijl in het datacenter. |


 ## <a name="nist-800-53-control-pe-3e"></a>NIST 800 53 besturingselement PE-3.e

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE-3.e** De organisatie beveiligt sleutels, combinaties en andere apparaten fysieke toegang.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Fysieke sleutels en tijdelijke toegang badges worden binnen operations Beveiligingscentrum (SOC) beveiligd. Beveiliging managers zijn waar 24 x 7. Sleutels zijn aan specifieke personeel uitgecheckt door die overeenkomt met de persoon toegang badge aan de fysieke sleutel. Sleutel voorraden worden uitgevoerd tijdens elke shift en sleutels zijn niet toegestaan op een externe locatie worden uitgevoerd. |


 ## <a name="nist-800-53-control-pe-3f"></a>NIST 800 53 besturingselement PE-3.f

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE 3.f** de organisatie voorraden [toewijzing: fysieke toegang organisatie gedefinieerd apparaten] elke [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Fysieke toegangsapparaten binnen het Azure-datacenters worden van ten minste een jaarlijks geïnventariseerd. Sleutels en tijdelijke toegang badges worden meerdere keren per dag gedurende elke shift geïnventariseerd. Badge-lezers van toegang en soortgelijke toegangsapparaten zijn gekoppeld aan de fysieke beveiliging system waar status continu wordt vertegenwoordigd. |


 ## <a name="nist-800-53-control-pe-3g"></a>NIST 800 53 besturingselement PE-3.g

#### <a name="physical-access-control"></a>Fysieke toegangsbeheer

**PE 3.g** de organisatie combinaties en sleutels wijzigen [toewijzing: organisatie gedefinieerde frequentie] en/of wanneer de sleutels zijn kwijtgeraakt, combinaties verdacht zijn, of personen zijn overgedragen of beëindigd.  

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Datacenters van Microsoft Azure hebt procedures voor het implementeren in gevallen waarbij een badge toegang of is kwijtgeraakt of een persoon wordt beëindigd of wordt overgedragen. In het geval van een beëindiging of overdracht van de persoon toegang wordt onmiddellijk verwijderd uit het systeem en hun access badge verwijderd. |


 ### <a name="nist-800-53-control-pe-3-1"></a>NIST 800 53 besturingselement PE-3 (1)

#### <a name="physical-access-control--information-system-access"></a>Fysieke toegangsbeheer | Toegang tot het systeem van informatie

**PE-3 (1)** de organisatie worden afgedwongen autorisaties fysieke toegang tot het informatiesysteem naast de fysieke toegang besturingselementen voor de opslagruimte op [toewijzing: fysieke spaties organisatie gedefinieerd met een of meer onderdelen van het informatiesysteem].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure meer beperkt gebieden binnen Microsoft-datacenters die kritieke systemen (bijvoorbeeld colocations, kritieke omgevingen, MDF-ruimten, enz.) via verschillende beveiligingsmechanismen zoals elektronische toegangsbeheer, biometrische apparaten bevatten, en anti-passback besturingselementen. Toegang tot Azure colocations worden verleend als een afzonderlijke, een hoger niveau van DCAT toegang dan andere gebieden van de toegang van het datacenter. Bovendien zijn alle van Azure FTE en leveranciers die toegang tot de Azure Government colocations hebben vereist voor formeel ondergaan van Microsoft Cloud gescreend en VS burgerschap verificatie voordat geautoriseerde toegang tot de omgeving. Zie de sectie van de PS-03 voor meer informatie met betrekking tot de cloud-controle voor de colocations Azure Government. |


 ## <a name="nist-800-53-control-pe-4"></a>NIST 800 53 besturingselement PE-4

#### <a name="access-control-for-transmission-medium"></a>Toegangsbeheer voor Verzendingsmedium

**PE 4** beheert de organisatie fysieke toegang tot [toewijzing: organisatie gedefinieerde informatie system distributie en transmission regels] binnen de organisatie faciliteiten met [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] .

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Toegangsbeheer voor verzendingsmedium via het ontwerp en de opbouw van de ruimten Main distributie Frame (MDF) en colocations informatie system distributie en transmission regels te beschermen tegen onbedoeld schade, door Microsoft Azure is geïmplementeerd wordt onderbroken en fysieke knoeien. Toegang naar MDF ruimten en colocations vereist twee factor-verificatie (toegang badge en biometrie). Dit zorgt ervoor dat de toegang beperkt tot alleen bevoegd personeel (Zie PE-2, PE 3 is). Regels voor transmissie en distributie zijn binnen de MDF beschermd tegen schade wordt onderbroken en fysieke manipulatie door het gebruik van metal conduits, vergrendelde rekken, kooien of kabel laden. |


 ## <a name="nist-800-53-control-pe-5"></a>NIST 800 53 besturingselement PE-5

#### <a name="access-control-for-output-devices"></a>Toegangsbeheer voor uitvoerapparaten

**PE 5** beheert fysieke toegang tot het systeem-uitvoerapparaten informatie om te voorkomen dat onbevoegde personen het verkrijgen van de uitvoer van de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure-datacenters geen (monitors, printers, audio-apparaten, enz.) permanent verbonden uitvoerapparaten Azure activa of Azure gedeelde elementen. Naast het niet met uitvoerapparaten, uitvoeren-afdelingen fysieke scenario's van de faciliteit meerdere keren per shift controleren op items zoals deuren worden vergrendeld en rekken worden beveiligd. Datacenter-toegang is beperkt tot mensen die zijn goedgekeurd door-verificatie ondersteunt. Colocations vereist dat twee factor-verificatie (toegang badge en biometrie) om toegang te krijgen. |


 ## <a name="nist-800-53-control-pe-6a"></a>NIST 800 53 besturingselement PE-6.a

#### <a name="monitoring-physical-access"></a>Fysieke toegang controleren

**PE 6.a** de organisatie bewaakt de fysieke toegang tot de faciliteit waarin het informatiesysteem om te detecteren en reageren op beveiligingsincidenten fysieke zich bevindt.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Fysieke toegang wordt bewaakt door de implementatie van beveiligingsapparaten en processen in de datacentra. Voorbeelden zijn elektronische bewaking van 24 x 7 van toegangsbeheer, waarschuwing en video-systemen, evenals 24 x 7 op veiligheidspatrouilles site van de opslagruimte en de redenen. Een besturingselement ruimte Supervisor bevindt zich in de SOC te allen tijde om te voorzien in bewaking van de fysieke toegang in het datacenter. |


 ## <a name="nist-800-53-control-pe-6b"></a>NIST 800 53 besturingselement PE-6.b

#### <a name="monitoring-physical-access"></a>Fysieke toegang controleren

**PE 6.b** de organisatie beoordeelt de logboeken van de fysieke toegang [toewijzing: organisatie gedefinieerde frequentie] bij de instantie van [toewijzing: organisatie gedefinieerde gebeurtenissen of potentiële aanwijzingen voor gebeurtenissen].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Fysieke toegangslogboeken zijn continu gecontroleerd en onderhouden voor latere onderzoek revisie. |


 ## <a name="nist-800-53-control-pe-6c"></a>NIST 800 53 besturingselement PE-6.c

#### <a name="monitoring-physical-access"></a>Fysieke toegang controleren

**PE 6.c** de organisatie coördineert de resultaten van de beoordelingen en onderzoeken met de mogelijkheid van de organisatie-respons op incidenten. 

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Beveiligingsgebeurtenissen die zich binnen het datacenter worden gedocumenteerd volgens het beveiligingsteam. Het beveiligingsteam maakt rapporten die de details van een beveiligingsgebeurtenis nadat de gebeurtenis vastlegt. <br /> Voor incidenten verplichte government aanmelding, zal het team van Microsoft Azure-beveiliging met de aanbieder van de primaire toepassing (bijvoorbeeld O365) op de hoogte van de overheid agency klant, ons CERT en FedRAMP in VS-CERT richtlijnen (Zie IR 6) wilt coördineren. |


 ### <a name="nist-800-53-control-pe-6-1"></a>NIST 800 53 besturingselement PE-6 (1)

#### <a name="monitoring-physical-access--intrusion-alarms--surveillance-equipment"></a>Fysieke toegang controleren | Inbraakdetectie alarmen / toezicht apparatuur

**PE-6 (1)** de organisatie bewaakt fysieke inbraakdetectie alarmen en toezicht apparatuur.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Naast de beveiliging van het beveiligingspersoneel 24 x 7 datacenters van Microsoft Azure (geleasete en volledig beheerd) ook gebruikmaken van alarm systemen en CCTV bewaken. Alarmen wordt gecontroleerd en beantwoord door de ruimte beheer Supervisor die 24 x 7 in de sofi Tijdens een situatie antwoord gebruikmaakt van de ruimte beheer Supervisor camera's in het gebied van het incident wordt onderzocht zodat de responder real-time informatie. |


 ### <a name="nist-800-53-control-pe-6-4"></a>NIST 800 53 besturingselement PE-6 (4)

#### <a name="monitoring-physical-access--monitoring-physical-access-to-information-systems"></a>Fysieke toegang controleren | Fysieke toegang tot informatiesystemen bewaking

**PE-6 (4)** de organisatie bewaakt de fysieke toegang tot het informatiesysteem naast het controleren van de fysieke toegang van de faciliteit als [toewijzing: fysieke spaties organisatie gedefinieerd met een of meer onderdelen van de gegevens systeem].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure bewaakt de fysieke toegang tot de faciliteiten, evenals de informatiesystemen binnen de datacentra. Alle Microsoft online services-apparatuur wordt geplaatst op locaties in datacenters waar fysieke toegang wordt bewaakt. Alle plaatsing en MDF-ruimten worden beschermd door toegangsbeheer, alarmen en video's. |


 ## <a name="nist-800-53-control-pe-8a"></a>NIST 800 53 besturingselement PE-8.

#### <a name="visitor-access-records"></a>Bezoeker toegangsrecords

**PE 8.** bezoeker toegangsrecords naar de faciliteit waarbij het informatiesysteem zich bevindt voor de organisatie worden bijgehouden [toewijzing: organisatie gedefinieerde periode].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Datacenter-access-records worden bijgehouden in het datacenter toegang hulpprogramma in de vorm van goedgekeurde aanvragen. Zoals beschreven in PE 3, zijn bezoekers vereist om te allen tijde worden escorted. De begeleiding toegang in het datacenter wordt vastgelegd en indien nodig kan worden gecorreleerd met de bezoeker voor toekomstige revisie. |


 ## <a name="nist-800-53-control-pe-8b"></a>NIST 800 53 besturingselement PE-8.b

#### <a name="visitor-access-records"></a>Bezoeker toegangsrecords

**PE 8.b** de organisatie beoordeelt de bezoeker toegangsrecords [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Bezoekers met een goedgekeurde toegangsaanvraag hebben hun toegang record op het moment dat de identificatie wordt vergeleken met een vorm van de overheid ID uitgegeven gecontroleerd of Microsoft badge uitgegeven. Zoals beschreven in PE 3, worden de bezoekers escorted te allen tijde wanneer ze op het datacenter. |


 ### <a name="nist-800-53-control-pe-8-1"></a>NIST 800 53 besturingselement PE-8 (1)

#### <a name="visitor-access-records--automated-records-maintenance--review"></a>Bezoeker toegangsrecords | Geautomatiseerd Records onderhoud / controleren

**PE-8 (1)** geautomatiseerde mechanismen ter bevordering van het onderhoud en beoordeling van de bezoeker toegangsrecords maakt gebruik van de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure onderhoudt datacenter toegangsrecords in DCAT in de vorm van goedgekeurde DCAT aanvragen. DCAT aanvragen kunnen alleen worden goedgekeurd door het DCM-team. Toegangsniveaus in het datacenter zijn toegewezen en vanuit DCAT beheerd. Datacenter toegang wordt elk kwartaal gecontroleerd. Alle toegang tot Azure-datacenters wordt vastgelegd in DCAT en is beschikbaar voor toekomstige mogelijke onderzoeken. Bezoekers zijn vereist om te allen tijde worden escorted. Toegang tot de begeleiding binnen het datacenter wordt geregistreerd binnen het alarm bewaking van systeem en indien nodig kan worden gecorreleerd met de bezoeker voor toekomstige revisie. Bezoekerstoegang wordt altijd geëvalueerd door de toegewezen begeleiding en door het besturingselement ruimte supervisor via CCTV en het alarm bewakingssysteem. Bezoekers worden niet geleverd met toegang en moeten vergezeld gaan van hun escorts te allen tijde. |


 ## <a name="nist-800-53-control-pe-9"></a>NIST 800 53 besturingselement PE-9

#### <a name="power-equipment-and-cabling"></a>Power apparatuur en bekabeling

**PE-9** de organisatie beveiligt power apparatuur en power bekabeling voor het systeem gegevens tegen beschadiging en vernietigd.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure biedt beschermende spaties en de juiste labels voor kabels. Microsoft Azure-infrastructuur apparatuur, bijvoorbeeld, kabels, elektrische regels en genereren van de back-up moet worden geplaatst in omgevingen die speciaal ontwikkeld om te worden beschermd tegen milieu risico's zoals diefstal, fire, explosieven, rook, water, stof trillingen, aardbeving, schadelijke chemicaliën, elektrische storing, stroomuitval elektrische storingen (pieken). Alle draagbare onlineservices activa (bijvoorbeeld rekken, servers, netwerkapparaten) moeten worden vergrendeld of vastgemaakt plaats om te kunnen bieden bescherming tegen diefstal of verplaatsing schade. Power en informatie system-kabels in een Microsoft Azure-omgeving zijn met het label op de juiste wijze en beveiligd tegen onderschepping of beschadiging. Power en informatie system kabels worden gescheiden van elkaar op alle punten in een omgeving om te voorkomen dat de storing. |


 ## <a name="nist-800-53-control-pe-10a"></a>NIST 800 53 besturingselement PE-10.a

#### <a name="emergency-shutoff"></a>Noodgevallen signalen

**PE 10.a** de organisatie, biedt de mogelijkheid van het afsluiten van power in de informatie system of afzonderlijke system onderdelen in noodsituaties.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure is knoppen Emergency Power uitschakelen (EPO) geïnstalleerd op locaties binnen het datacenter op lokale fire-code vereist. In sommige datacenters van Microsoft Azure beheerd moet het ontwerp datacenter niet meer EPO knoppen. |


 ## <a name="nist-800-53-control-pe-10b"></a>NIST 800 53 besturingselement PE-10.b

#### <a name="emergency-shutoff"></a>Noodgevallen signalen

**PE 10.b** de organisatie plaatst noodgevallen signalen switches of apparaten in [toewijzing: locatie organisatie gedefinieerd door het systeem voor informatie of systeemonderdeel] veilig en eenvoudig toegang voor personeel mogelijk te maken.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. EPO knoppen worden strategisch om toe te staan voor activering in noodsituaties geplaatst. EPO knoppen kunnen worden geplaatst in de colocations, bemand faciliteiten bewerking Centers (FOCs) of door de lokale fire-code vereist. |


 ## <a name="nist-800-53-control-pe-10c"></a>NIST 800 53 besturingselement PE-10.c

#### <a name="emergency-shutoff"></a>Noodgevallen signalen

**PE 10.c** de organisatie signalen van noodgevallen energiemogelijkheden beveiligt tegen onbevoegde activering.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Om te voorkomen dat per ongeluk activering, mogelijk EPO knoppen hebben een beschermende behuizing, dual-activering vereist is of een akoestisch alarm gebruiken als een waarschuwing voor activering. Bovendien zijn EPO knoppen onder Videobewaking. |


 ## <a name="nist-800-53-control-pe-11"></a>NIST 800 53 besturingselement PE-11

#### <a name="emergency-power"></a>Noodgevallen Power

**PE-11** de organisatie biedt een korte termijn noodvoeding te vergemakkelijken [selectie (een of meer): een geordende afsluiting van het informatiesysteem; de overgang van het informatiesysteem op lange termijn alternatieve power] in geval van een primaire stroom bron uitvalt.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure heeft geïmplementeerd noodgevallen power datacenter apparatuur beveiligen en circuits met uninterruptible power supply (UPS) systeem waarmee u een korte termijn voeding om processorvermogen totdat genereren kunnen komen online zijn. |


 ### <a name="nist-800-53-control-pe-11-1"></a>NIST 800 53 besturingselement PE-11 (1)

#### <a name="emergency-power--long-term-alternate-power-supply---minimal-operational-capability"></a>Noodgevallen Power | Op lange termijn alternatieve Uninterruptible Power Supply - minimale operationele functionaliteit

**PE-11 (1)** de organisatie een lange termijn alternatieve voeding biedt van het informatiesysteem die geschikt is voor het onderhouden van minimaal operationele functionaliteit in het geval van een uitgebreide verlies van de primaire stroombron vereist.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure is een langdurige alternatieve voeding voor het informatiesysteem die geschikt is voor het onderhouden van dat een minimaal vereiste operationele mogelijkheid wanneer een uitgebreide verlies van de primaire stroombron optreedt geïmplementeerd. Als power uitvalt of onder het niveau van een onaanvaardbaar spanning komt, wordt ononderbroken (UPS) systemen onmiddellijk starten en de power belasting overnemen. Dit biedt voldoende stroom voor het uitvoeren van de servers totdat het genereren kunnen overnemen. Noodgevallen genereren van een back-up processorvermogen voor uitgebreide onderbrekingen en voor gepland onderhoud en het datacenter met ter plaatse brandstof reserves in het geval van een natuurramp kunnen werken. Azure onderhoudt diesel generator op veel van de datacenters. Genereren van de back-up worden gebruikt wanneer dat nodig is om te raster stabiele of in buitengewone herstellen en onderhoud situaties waarvoor ons te laten de datacenters het raster power uit. |


 ## <a name="nist-800-53-control-pe-12"></a>NIST 800 53 besturingselement PE-12

#### <a name="emergency-lighting"></a>Noodgevallen licht

**PE-12** de organisatie de veiligheidsmaatregelen en onderhoudt automatische noodgevallen verlichtings voor het informatiesysteem die wordt geactiveerd in geval van een stroomstoring of wordt onderbroken en waarin noodgevallen wordt afgesloten en verplaatsing routes in de opslagruimte.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Noodgevallen licht implementeren datacenters van Microsoft Azure (geleasete en volledig beheerd) in de vorm van overhead noodgevallen licht op specifieke circuits back-up gemaakt door UPS en generator-systemen (Zie PE-11). Automatische noodgevallen licht is geïmplementeerd langs alle verplaatsing routes, noodgevallen uitgangen en binnen de colocations in overeenstemming met de nationale brand en beveiliging koppeling (NFPA) levensduur veiligheid Code. In het geval dat een stroomstoring wordt verbroken, wordt automatisch het noodgevallen licht overschakelen naar power geleverd door de UPS en generator-systemen. De systemen noodgevallen licht in datacenters van Microsoft Azure ondergaan routineonderhoud om ervoor te zorgen dat ze in goede staat blijven. |


 ## <a name="nist-800-53-control-pe-13"></a>NIST 800 53 besturingselement PE-13

#### <a name="fire-protection"></a>Beveiliging geactiveerd

**PE-13** veiligheidsmaatregelen en onderhoudt fire onderdrukking en detectie apparaten/systemen voor het systeem gegevens die worden ondersteund door een onafhankelijke energiebron van de organisatie.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure is bescherming tegen geïmplementeerd door het installeren van fire detectie en fire onderdrukking systemen op de Microsoft Azure-datacenters. <br /> Microsoft Azure-datacenters implementeren robuuste fire detectiemechanismen. De aanpak van Microsoft Azure fire beveiliging omvat het gebruik van foto-elektrisch rook detectoren geïnstalleerd onder de floor en op het maximum die zijn geïntegreerd met de sprinklerinstallatie voor beveiliging. Bovendien zijn er Xtralis VESDA (zeer vroege Smoke detectie-apparatuur)-systemen in elke plaatsing die op de lucht controleren. VESDA eenheden zijn zeer vertrouwelijke lucht steekproeven systemen geïnstalleerd in de gehele meerdere waardevolle spaties. VESDA eenheden toestaan voor een onderzoek reactie voordat een waarschuwing wordt gegeven werkelijke fire detectie. <br /> Fire alarm vakken 'Pull station' zijn geïnstalleerd in de datacentra voor handmatige fire-waarschuwing. Toestellen zijn zich bevinden in de datacentra en correct geïnspecteerd, onderhouden en jaarlijks gelabeld. Het personeel beveiliging patrouilles per schip alle ruimten meerdere keren elke shift. Datacenter-personeel uitvoeren voor een dagelijkse website procedure zodat alle fire controle vereisten wordt voldaan. <br /> Gebieden met gevoelige elektrische apparatuur (colocations, MDFs, enzovoort) worden beveiligd door een dubbele koppeling vooraf actie (droge pipe) sprinklersystemen. Droge pipe sprinklers vormen een systeem voor het vooraf actie van twee fasen waarvoor zowel een sprinkler head-activering (vanwege hitte) als rook detectie water vrijgeven. De sprinkler head activering versies de luchtdruk in de pipes waarmee de pipes te vullen met water. Water wordt vrijgegeven wanneer een rook- en detectie ook is geactiveerd. <br /> Fire detectie/onderdrukking en noodgevallen licht systemen in het datacenter-UPS en generator systemen leveren voor de bron van een redundant vermogen bedrading. |


 ### <a name="nist-800-53-control-pe-13-1"></a>NIST 800 53 besturingselement PE-13 (1)

#### <a name="fire-protection--detection-devices--systems"></a>Beveiliging geactiveerd | Detectie van apparaten / systemen

**PE-13 (1)** de organisatie gebruik wordt gemaakt activeert detectie apparaten/systemen voor het informatiesysteem die automatisch activeren en melden [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] en [toewijzing: emergency organisatie gedefinieerd responders] in het geval van een brand.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure gebruik wordt gemaakt gestart detectie apparaten/systemen voor het informatiesysteem die automatisch activeren en datacenter-personeel samen met noodgevallen responders in het geval van een brand melden. In het geval dat een van de mechanismen fire-detectie in elke collocatie is geactiveerd, wordt de lokale fire-afdeling automatisch kennisgeving via het fire-alarmsysteem. Bovendien zijn de fire-beveiliging en fire detectie-systemen gekoppeld in het beveiligingssysteem verwittigen lokaal personeel faciliteit en beveiliging. |


 ### <a name="nist-800-53-control-pe-13-2"></a>NIST 800 53 besturingselement PE-13 (2)

#### <a name="fire-protection--suppression-devices--systems"></a>Beveiliging geactiveerd | Onderdrukking van apparaten / systemen

**PE-13 (2)** de organisatie gebruik wordt gemaakt activeert onderdrukking apparaten/systemen voor het informatiesysteem waarmee u automatische meldingen van een activering [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] en [toewijzing: organisatie gedefinieerd noodgevallen responders].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. In het geval dat een van de onderdrukking fire systemen in het datacenter is geactiveerd, wordt de lokale fire-afdeling automatisch kennisgeving via het fire-alarmsysteem. Bovendien zijn de fire-beveiliging en fire detectie-systemen gekoppeld in het beveiligingssysteem verwittigen lokaal personeel faciliteit en beveiliging. |


 ### <a name="nist-800-53-control-pe-13-3"></a>NIST 800 53 besturingselement PE-13 (3)

#### <a name="fire-protection--automatic-fire-suppression"></a>Beveiliging geactiveerd | Automatische Fire onderdrukking

**PE-13 (3)** veiligheidsmaatregelen voor de organisatie de mogelijkheid van een automatische fire onderdrukking voor het informatiesysteem wanneer de faciliteit niet doorlopend personeelsbezetting.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Datacenters van Microsoft Azure zijn waar 24 x 7. Fire onderdrukking systemen benaderen automatisch zonder handmatige interventie wanneer een situatie fire alarm wordt gedetecteerd. |


 ## <a name="nist-800-53-control-pe-14a"></a>NIST 800 53 besturingselement PE-14.a

#### <a name="temperature-and-humidity-controls"></a>Temperatuur en vochtigheid besturingselementen

**PE 14.a** temperatuur en vochtigheid niveaus binnen de faciliteit waarbij het informatiesysteem bevindt zich op de organisatie worden bijgehouden [toewijzing: aanvaardbaar niveau organisatie gedefinieerd].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure houdt de temperatuur en vochtigheid niveaus in overeenstemming met American Society van verwarming, Refrigerating en airconditioning Engineers (ASHRAE) richtlijnen. De temperatuur en vochtigheid niveaus worden continu bewaakt door het datacenter Building Management System (BMS). |


 ## <a name="nist-800-53-control-pe-14b"></a>NIST 800 53 besturingselement PE-14.b

#### <a name="temperature-and-humidity-controls"></a>Temperatuur en vochtigheid besturingselementen

**PE 14.b** de organisatie bewaakt temperatuur en vochtigheid niveaus [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Op Microsoft Azure-datacenters, worden temperatuur en vochtigheid niveaus bewaakt continu door het gebouw-beheer (BMS). Datacenter-personeel bewaken de BMS van faciliteiten Operations Center (FOC), zodat ze de temperatuur en vochtigheid binnen het datacenter beheren kunnen voordat alle punten alarm is overschreden. |


 ### <a name="nist-800-53-control-pe-14-2"></a>NIST 800 53 besturingselement PE-14 (2)

#### <a name="temperature-and-humidity-controls--monitoring-with-alarms--notifications"></a>Temperatuur en vochtigheid besturingselementen | Bewaking met alarmen / meldingen

**PE-14 (2)** de organisatie de veiligheidsmaatregelen temperatuur en vochtigheid bewaking die biedt een waarschuwing of melding van wijzigingen die potentieel schadelijke aan personeel dat of apparatuur.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Op Microsoft Azure-datacenters, worden temperatuur en vochtigheid niveaus bewaakt continu door het gebouw-beheer (BMS). Datacenter-personeel bewaken de BMS van faciliteiten Operations Center (FOC), zodat ze de temperatuur en vochtigheid binnen het datacenter beheren kunnen voordat alle punten alarm is overschreden. |


 ## <a name="nist-800-53-control-pe-15"></a>NIST 800 53 besturingselement PE-15

#### <a name="water-damage-protection"></a>Water schade beveiliging

**PE-15** de organisatie het systeem gegevens beveiligt tegen schade ten gevolge van water lekken doordat master signalen of isolatie kleppen die toegankelijk, goed werkt zijn en bekende sleutel personeel.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure biedt water/geheugenlek detectie op gebieden met een risico op water lekken (bijv, lucht Handlers Units). Fire onderdrukking systemen hebben ook geheugenlek detectie alarmen die worden bewaakt. Het detectiesysteem water/geheugenlek is geïntegreerd met het faciliteit waarschuwingen en meldingen van het systeem. De sprinklersystemen in de datacentra zijn ingedeeld. Datacenter-personeel bent bekend met noodgevallen procedures vereisen het gebruik van de signalen water en hun locatie. De uitbreidingskaarten sprinkler hebben de mogelijkheid om te worden afgesloten afzonderlijk of als een groep via Schuifafsluiters. Alle sprinklers in de ruimte kritiek zijn dubbele koppeling vooraf actietype sprinklers die twee soorten activering vereist voordat de stroom wordt gestart. De druk van de sprinklerinstallatie wordt bewaakt en tegen het lekken van water is normaal. |


 ### <a name="nist-800-53-control-pe-15-1"></a>Besturingselement PE-15 NIST 800 53 (1)

#### <a name="water-damage-protection--automation-support"></a>Beveiliging van de schade water | Ondersteuning voor automatisering

**PE-15 (1)** veiligheidsmaatregelen geautomatiseerde mechanismen voor het detecteren van de aanwezigheid van water in de buurt van het systeem voor informatie en waarschuwingen voor de organisatie [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure maakt gebruik van geautomatiseerde methoden voor het detecteren van water aanwezigheid in de datacentra en waarschuwt datacenter personeel. Azure biedt water/geheugenlek detectie op gebieden met een risico op water lekken (bijv, lucht Handlers Units). Fire onderdrukking systemen hebben ook geheugenlek detectie alarmen die worden bewaakt. Het detectiesysteem water/geheugenlek is geïntegreerd met het faciliteit waarschuwingen en meldingen van het systeem. De druk van de sprinklerinstallatie wordt bewaakt en tegen het lekken van water is normaal. |


 ## <a name="nist-800-53-control-pe-16"></a>NIST 800 53 besturingselement PE-16

#### <a name="delivery-and-removal"></a>Levering en verwijderen

**PE-16** de organisatie autoriseert, bewaakt en beheert [toewijzing: organisatie gedefinieerde typen van onderdelen van het systeem] invoeren en afsluiten van de faciliteit en onderhoudt records van de items.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure implementeert deze vereiste namens klanten. Microsoft Azure implementeert strikte afdwinging van wat is toegestaan in te voeren en te sluiten van het datacenter. Alle system onderdelen/activa worden bijgehouden in de database asset management-hulpprogramma. |


 ## <a name="nist-800-53-control-pe-17a"></a>NIST 800 53 besturingselement PE-17.a

#### <a name="alternate-work-site"></a>Alternatieve werklocatie

**PE 17.a** maakt gebruik van de organisatie [toewijzing: organisatie gedefinieerd beveiligingsmechanismen] op alternatieve locaties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Van de klant op bedrijfsniveau alternatieve werk site bepalingen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-pe-17b"></a>NIST 800 53 besturingselement PE-17.b

#### <a name="alternate-work-site"></a>Alternatieve werklocatie

**PE 17.b** de organisatie als haalbaar evalueert, de effectiviteit van de beveiliging bepaalt op alternatieve locaties.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Van de klant op bedrijfsniveau alternatieve werk site bepalingen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-pe-17c"></a>NIST 800 53 besturingselement PE-17.c

#### <a name="alternate-work-site"></a>Alternatieve werklocatie

**PE 17.c** de organisatie biedt een manier voor medewerkers kunnen communiceren met beveiligingspersoneel informatie in het geval van beveiligingsincidenten of problemen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Van de klant op bedrijfsniveau alternatieve werk site bepalingen mogelijk onvoldoende voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-pe-18"></a>NIST 800 53 besturingselement PE-18

#### <a name="location-of-information-system-components"></a>Locatie van de onderdelen van het systeem

**PE-18** de organisatie posities onderdelen van het systeem binnen de faciliteit om mogelijke beschadiging van gegevens [toewijzing: organisatie gedefinieerd fysieke en omgevingsbeveiliging gevaren] en de kans voor niet-geautoriseerde minimaliseren toegang.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Klanten hoeft geen fysieke toegang tot alle systeemresources in Azure-datacenters. |
| **Provider (Microsoft Azure)** | Azure implementeert ontwerpbenadering strategische datacenter om te voldoen aan de locatie van informatie system onderdelen besturingselement. Alle Microsoft online services-apparatuur wordt geplaatst in de locaties die speciaal ontwikkeld om te worden beschermd tegen milieu risico's zoals diefstal, fire, explosieven, rook, water, stof, trillingen, aardbeving, schadelijke chemicaliën, elektrische storing stroomuitval elektrische storingen (pieken) en straling. De opslagruimte en de infrastructuur hebben geïmplementeerd seismische steunpunten plaatsen voor bescherming tegen milieu risico's. Alle plaatsing en MDF-ruimten worden beschermd door toegangsbeheer, alarmen en video's. De functie wordt ook wachtrondes door beveiliging managers 24 x 7. Alle draagbare Azure activa zijn vergrendeld of vastgemaakt plaats om te kunnen bieden bescherming tegen diefstal of verplaatsing schade. |


