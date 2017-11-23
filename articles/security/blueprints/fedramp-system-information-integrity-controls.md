---
title: FedRAMP Azure blauwdruk Automation - systeem en de integriteit van de gegevens
description: Webtoepassingen voor FedRAMP - systeem en de integriteit van de gegevens
services: security
documentationcenter: na
author: jomolesk
manager: mbaldwin
editor: tomsh
ms.assetid: 2ff2778b-2c37-41b5-a39c-6594b3e3b10b
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: jomolesk
ms.openlocfilehash: 1dc6805a5a1f610f06ce58bd4bd644346436294e
ms.sourcegitcommit: 8aa014454fc7947f1ed54d380c63423500123b4a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2017
---
# <a name="system-and-information-integrity-si"></a>Systeem- en integriteit van de gegevens (SI)

> [!NOTE]
> Deze besturingselementen zijn gedefinieerd door NIST en de V.S. Ministerie van handel als onderdeel van de NIST Special Publication 800-53 revisie 4. Raadpleeg NIST 800 53 Rev. 4 voor informatie over het testen van de procedures en richtlijnen voor elk besturingselement.

## <a name="nist-800-53-control-si-1"></a>NIST 800 53 besturingselement SI-1

#### <a name="system-and-information-integrity-policy-and-procedures"></a>Systeem en integriteitsbeleid informatie en Procedures

**SI-1** de organisatie ontwikkelt, documenten en verspreide naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] een integriteitsbeleid systeem en gegevens die zijn gericht op doel, bereik, rollen, verantwoordelijkheden, management-streven samenwerking tussen de organisatie-entiteiten en naleving; en -procedures om te vereenvoudigen, de implementatie van het systeem en informatie-integriteitsbeleid en bijbehorende informatie integriteit controles; en controleert en updates van het huidige systeem- en integriteitsbeleid [toewijzing: organisatie gedefinieerde frequentie]; en -procedures voor systeem- en integriteit [toewijzing: organisatie gedefinieerde frequentie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De integriteitsbeleid op bedrijfsniveau systeem en informatie en procedures van de klant mogelijk voldoende zijn voor het oplossen van dit besturingselement. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-2a"></a>NIST 800 53 besturingselement SI-2.

#### <a name="flaw-remediation"></a>Fout-herstel

**SI 2.** de organisatie identificeert, rapporten en informatie system fouten gecorrigeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de OMS Automation & Control-oplossing voor het bijhouden van de status van updates voor de Windows virtuele machines in deze architectuur wordt geïmplementeerd. Vanuit het dashboard OMS de tegel updatebeheer beveiligingsfout herstel wordt status weergegeven voor alle geïmplementeerde Windows-servers. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-2b"></a>NIST 800 53 besturingselement SI-2.b

#### <a name="flaw-remediation"></a>Fout-herstel

**SI 2.b** software en -firmware updates met betrekking tot beveiligingsfout herstel voor effectiviteit en potentiële bijwerkingen voordat de installatie door de organisatie wordt getest.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het testen van updates met betrekking tot beveiligingsfout herstel voor effectiviteit en potentiële neveneffecten voorafgaand aan installatie op bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-2c"></a>NIST 800 53 besturingselement SI-2.c

#### <a name="flaw-remediation"></a>Fout-herstel

**SI 2.c** beveiliging relevante software en -firmware-updates binnen de organisatie is geïnstalleerd [toewijzing: organisatie gedefinieerde periode] van de release van de updates.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Windows virtuele machines zijn geïmplementeerd door deze blauwdruk Azure zijn standaard geconfigureerd voor automatische updates ontvangen van Windows Update-Service. Deze oplossing implementeert ook de OMS Automation & Control-oplossing waarmee Update-implementaties kunnen worden gemaakt voor het implementeren van patches op Windows-servers wanneer deze nodig is. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-2d"></a>NIST 800 53 besturingselement SI-2.d

#### <a name="flaw-remediation"></a>Fout-herstel

**SI 2.d** de organisatie opgenomen met het herstel van de fout in het proces voor het beheer van organisatie-configuratie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het opnemen van herstel van de fout in Configuratiebeheer. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-2-1"></a>NIST 800 53 besturingselement SI-2 (1)

#### <a name="flaw-remediation--central-management"></a>Herstel fout | Centraal beheer

**SI-2 (1)** centraal beheert het herstelproces beveiligingsfout de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de OMS Automation & Control-oplossing voor het bijhouden van de status van updates voor de Windows virtuele machines in deze architectuur wordt geïmplementeerd. Vanuit het dashboard OMS de tegel updatebeheer beveiligingsfout herstel wordt status weergegeven voor alle geïmplementeerde Windows-servers. Update-implementaties kunnen worden gemaakt voor het implementeren van patches op Windows-servers wanneer deze nodig is. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-2-2"></a>NIST 800 53 besturingselement SI-2 (2)

#### <a name="flaw-remediation--automated-flaw-remediation-status"></a>Herstel fout | Herstelstatus van geautomatiseerde fout

**SI-2 (2)** de organisatie de veiligheidsmaatregelen voor geautomatiseerde mechanismen [toewijzing: organisatie gedefinieerde frequentie] om te bepalen van de status van onderdelen van het systeem informatie met betrekking tot herstel van de fout.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de OMS Automation & Control-oplossing voor het bijhouden van de status van updates voor de Windows virtuele machines in deze architectuur wordt geïmplementeerd. Voor elke beheerde Windows-computer wordt twee keer per dag een scan uitgevoerd. Om de vijftien minuten wordt de Windows-API aangeroepen voor de laatste updatetijd om op die manier te bepalen of de status is gewijzigd. Als dat het geval is, wordt er een nalevingsscan gestart. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-2-3a"></a>NIST 800 53 SI-2 (3) een beheren

#### <a name="flaw-remediation--time-to-remediate-flaws--benchmarks-for-corrective-actions"></a>Herstel fout | Tijd voor het herstellen van fouten / Benchmarks voor corrigerende maatregelen

**SI-2 (3) een** de organisatie meet de tijd tussen fout-id en het doorvoeren van de fout.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor beveiligingsstatus fouten binnen de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-2-3b"></a>NIST 800 53 SI-2 (3) .b beheren

#### <a name="flaw-remediation--time-to-remediate-flaws--benchmarks-for-corrective-actions"></a>Herstel fout | Tijd voor het herstellen van fouten / Benchmarks voor corrigerende maatregelen

**SI-2 (3) .b** stelt de organisatie [toewijzing: benchmarks organisatie gedefinieerd] om corrigerende maatregelen te nemen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant mogelijk afhankelijk van benchmarks voor beveiligingsfout herstel processen op bedrijfsniveau. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-3a"></a>NIST 800 53 besturingselement SI-3.

#### <a name="malicious-code-protection"></a>Schadelijke Code beveiliging

**SI 3.** mechanismen voor het beveiligen van schadelijke code op informatie system toegang naar en uitgang punten voor het opsporen en schadelijke code roeien maakt gebruik van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd op basis van een host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de extensie van de Microsoft Antimalware-virtuele machine. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-3b"></a>NIST 800 53 besturingselement SI-3.b

#### <a name="malicious-code-protection"></a>Schadelijke Code beveiliging

**SI 3.b** mechanismen voor het beveiligen van schadelijke code door de organisatie wordt bijgewerkt wanneer nieuwe releases beschikbaar volgens de configuratie van de organisatie-management-beleid en procedures zijn.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd op basis van een host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de extensie van de Microsoft Antimalware-virtuele machine. Deze uitbreiding is geconfigureerd voor automatisch bijwerken van zowel de antimalware-engine en beveiliging de handtekeningen als release beschikbaar. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-3c"></a>NIST 800 53 besturingselement SI-3.c

#### <a name="malicious-code-protection"></a>Schadelijke Code beveiliging

**SI 3.c** de organisatie configureert mechanismen voor het beveiligen van schadelijke code om uit te voeren periodieke scans van het informatiesysteem [toewijzing: organisatie gedefinieerde frequentie] en realtime scans van bestanden van externe bronnen op [selectie (één of meer); eindpunt; netwerk binnenkomt/verlaat punten] als de bestanden zijn gedownload, geopend of uitgevoerd op basis van beleid voor beveiliging van de organisatie; en [selectie (een of meer): schadelijke code blokkeren; quarantaine schadelijke code; waarschuwing voor verzenden naar de beheerder. [Toewijzing: organisatie gedefinieerde actie]] Als reactie op detectie voor schadelijke code.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd op basis van een host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de extensie van de Microsoft Antimalware-virtuele machine. Deze uitbreiding is geconfigureerd voor de real-time en periodieke scans (wekelijks) uitvoeren, automatisch bijwerken van zowel de antimalware-engine en beveiliging de handtekeningen en automatische herstelacties uitvoeren. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-3d"></a>NIST 800 53 besturingselement SI-3.d

#### <a name="malicious-code-protection"></a>Schadelijke Code beveiliging

**SI 3.d** de organisatie de ontvangst van de fout-positieven adressen tijdens uitroeiing en schadelijke code detecteren en de resulterende mogelijke gevolgen voor de beschikbaarheid van het informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beveiligen tegen schadelijke code. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-3-1"></a>NIST 800 53 besturingselement SI-3 (1)

#### <a name="malicious-code-protection--central-management"></a>Schadelijke Code Protection | Centraal beheer

**SI-3 (1)** de organisatie beheert centraal mechanismen voor het beveiligen van schadelijke code.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd op basis van een host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de extensie van de Microsoft Antimalware-virtuele machine. Azure OMS biedt een gecentraliseerde mogelijkheid om te controleren van de huidige status van de antimalwareoplossing. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-3-2"></a>NIST 800 53 besturingselement SI-3 (2)

#### <a name="malicious-code-protection--automatic-updates"></a>Schadelijke Code Protection | Automatische Updates

**SI-3 (2)** informatie automatisch bijgewerkt mechanismen voor het beveiligen van schadelijke code.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd op basis van een host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de extensie van de Microsoft Antimalware-virtuele machine. Deze uitbreiding is geconfigureerd voor automatisch bijwerken van zowel de antimalware-engine en beveiliging de handtekeningen als release beschikbaar. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-3-7"></a>NIST 800 53 besturingselement SI-3 (7)

#### <a name="malicious-code-protection--nonsignature-based-detection"></a>Schadelijke Code Protection | Nonsignature gebaseerde detectie

**SI-3 (7)** detectiemechanismen nonsignature gebaseerde schadelijke code door het informatiesysteem wordt geïmplementeerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd op basis van een host antimalware-bescherming voor alle geïmplementeerde virtuele machines van Windows geïmplementeerd met behulp van de extensie van de Microsoft Antimalware-virtuele machine. Deze uitbreiding is geconfigureerd voor het uitvoeren van heuristische detectie. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4a"></a>NIST 800 53 besturingselement SI-4.

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.** de organisatie controleert het informatiesysteem om te detecteren van aanvallen en indicatoren van mogelijke aanvallen in overeenstemming met [toewijzing: organisatie gedefinieerde doelstellingen bewaking]; en niet-geautoriseerde lokale-, netwerk- en RAS verbindingen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd voor de logboekanalyse en de oplossing OMS beveiligings- en controlebeleid. Deze oplossing biedt een uitgebreid overzicht van beveiligingspostuur, aanvallen en indicatoren van mogelijke aanvallen. De beveiligings- en Audit dashboard biedt op hoog niveau inzicht in de beveiligingsstatus van geïmplementeerde resources met gegevens die beschikbaar zijn voor geïmplementeerde OMS-oplossingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4b"></a>NIST 800 53 besturingselement SI-4.b

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.b** de organisatie identificeert onbevoegd gebruik van het informatiesysteem via [toewijzing: organisatie gedefinieerde-technieken en methoden].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure implementeert de OMS-beveiligings- en Audit-oplossing. Het domein identificeren en toegang biedt een dashboard met een overzicht van de informatie identiteit systeemstatus, met inbegrip van aantal mislukte pogingen om aan te melden en het huidige aantal accounts die zijn aangemeld. De informatie in dit dashboard kan helpen bij de identificatie van mogelijke verdachte activiteit. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4c"></a>NIST 800 53 besturingselement SI-4.c

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.c** bewaking apparaten strategisch vanuit het informatiesysteem voor het verzamelen van essentiële informatie organisatie bepaald; en op ad-hoc locaties binnen het systeem voor het bijhouden van bepaalde typen transacties van de organisatie wordt geïmplementeerd belang zijn voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd voor de logboekanalyse en de oplossing OMS beveiligings- en controlebeleid. De beveiligings- en Audit dashboard biedt op hoog niveau inzicht in de beveiligingsstatus van geïmplementeerde resources met gegevens die beschikbaar zijn voor geïmplementeerde OMS-oplossingen, inclusief inzicht in de VM-besturingssysteem bewaken van gegevens. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4d"></a>NIST 800 53 besturingselement SI-4.d

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.d** de organisatie beschermt gegevens die zijn verkregen van inbraakdetectie bewaking extra tegen ongeoorloofde toegang, aanpassen en verwijderen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Logische toegang besturingselementen worden gebruikt om de controlegegevens binnen deze blauwdruk Azure beveiligen tegen onbevoegde toegang, aanpassen en verwijderen. Azure Active Directory wordt afgedwongen goedgekeurde logische toegang met behulp van op rollen gebaseerde groepslidmaatschappen. De mogelijkheid controlegegevens weergeven en gebruiken van controleprogramma's worden beperkt tot gebruikers die deze machtigingen zijn vereist. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4e"></a>NIST 800 53 besturingselement SI-4.e

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.e** De organisatie kunnen het niveau van het informatiesysteem activiteit controleren wanneer er een indicatie van toegenomen risico voor de organisatie-bewerkingen en activa, personen, andere organisaties of het land op basis van wet afdwinging informatie we veel beter intelligence-informatie, of andere geloofwaardige informatiebronnen.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4f"></a>NIST 800 53 besturingselement SI-4.f

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.f** de organisatie verkrijgt juridische advies met betrekking tot informatie system bewakingsactiviteiten in overeenstemming met federale wetgeving, Executive Orders, richtlijnen, beleid of voorschriften.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-4g"></a>NIST 800 53 besturingselement SI-4.g

#### <a name="information-system-monitoring"></a>Informatie systeemcontrole

**SI 4.g** voorziet in de organisatie [toewijzing: organisatie gedefinieerde informatie system controlegegevens] naar [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] [selectie (een of meer): indien nodig; [Toewijzing: organisatie gedefinieerde frequentie]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-1"></a>NIST 800 53 besturingselement SI-4 (1)

#### <a name="information-system-monitoring--system-wide-intrusion-detection-system"></a>Informatie systeemcontrole | Systeeminstellingen inbraakdetectie detectiesysteem

**SI-4 (1)** verbinding maakt en configureert u afzonderlijke inbraakdetectie detectie-hulpprogramma's in een systeem hele systeem inbraakdetectie detectie van de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-2"></a>NIST 800 53 besturingselement SI-4 (2)

#### <a name="information-system-monitoring--automated-tools-for-real-time-analysis"></a>Informatie systeemcontrole | Hulpprogramma's voor analyse van realtime

**(2) SI-4** veiligheidsmaatregelen hulpprogramma's voor de ondersteuning van bijna realtime analyses van gebeurtenissen voor de organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd voor de logboekanalyse en verschillende OMS-oplossingen, met inbegrip van de beveiligings- en Audit-oplossing. Log Analytics biedt bijna realtime analyses van gebeurtenissen op de geïmplementeerde resources. OMS solutions biedt een uitgebreid overzicht van beveiligingspostuur tussen domeinen van de oplossing. OMS verschaft inzicht in de beveiligingsstatus van geïmplementeerde resources met gegevens die beschikbaar zijn voor geïmplementeerde OMS-oplossingen. OMS kunnen worden geconfigureerd voor het genereren van waarschuwingen op basis van de gedefinieerde criteria. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-4"></a>NIST 800 53 besturingselement SI-4 (4)

#### <a name="information-system-monitoring--inbound-and-outbound-communications-traffic"></a>Informatie systeemcontrole | Voor binnenkomend en uitgaand communicatieverkeer

**SI-4 (4)** het informatiesysteem controleert binnenkomende en uitgaande communicatieverkeer [toewijzing: organisatie gedefinieerde frequentie] voor ongebruikelijke of onbevoegde activiteiten of voorwaarden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-5"></a>NIST 800 53 besturingselement SI-4 (5)

#### <a name="information-system-monitoring--system-generated-alerts"></a>Informatie systeemcontrole | Het systeem gegenereerde waarschuwingen

**SI-4 (5)** de signalen informatie [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] wanneer de volgende aanduidingen van inbreuk of mogelijke inbreuk plaatsvinden: [toewijzing: indicatoren van inbreuk organisatie gedefinieerd].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk implementeert verschillende OMS-oplossingen, met inbegrip van de beveiligings- en Audit oplossing. Log Analytics biedt bijna realtime analyses van gebeurtenissen op de geïmplementeerde resources. OMS solutions biedt een uitgebreid overzicht van beveiligingspostuur tussen domeinen van de oplossing. OMS kunnen worden geconfigureerd voor het genereren van waarschuwingen op basis van de gedefinieerde criteria. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-11"></a>NIST 800 53 besturingselement SI-4 (11)

#### <a name="information-system-monitoring--analyze-communications-traffic-anomalies"></a>Informatie systeemcontrole | Communicatie-verkeer afwijkingen analyseren

**SI-4 (11)** de organisatie uitgaande communicatie-verkeer bij de externe grens van het systeem gegevens analyseert en geselecteerd [toewijzing: interior organisatie gedefinieerde gegevenspunten in het systeem (bijvoorbeeld subnetwerken, subsystemen)] naar afwijkingen gedetecteerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het analyseren van communicatie verkeer afwijkingen voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-14"></a>NIST 800 53 besturingselement SI-4 (14)

#### <a name="information-system-monitoring--wireless-intrusion-detection"></a>Informatie systeemcontrole | Draadloze inbraakdetectie

**SI-4 (14)** de organisatie gebruik wordt gemaakt een detectiesysteem draadloze inbraakdetectie om rogue draadloze apparaten te identificeren en voor het detecteren van aanvallen probeert en de mogelijke inbreuk/inbreuk op het informatiesysteem.

**Verantwoordelijkheden:**`Azure Only`

|||
|---|---|
| **De klant** | Er is geen hardware klant beheerde, draadloze apparaten, waaronder is toegestaan in de Azure-datacenters. |
| **Provider (Microsoft Azure)** | Microsoft Azure controleert regelmatig voor draadloze signalen rogue kwartaal zoals beschreven in de AC-18. <br /> Microsoft Azure implementeert dit besturingselement namens PaaS- en IaaS-klanten. |


 ### <a name="nist-800-53-control-si-4-16"></a>NIST 800 53 besturingselement SI-4 (16)

#### <a name="information-system-monitoring--correlate-monitoring-information"></a>Informatie systeemcontrole | Controlegegevens correleren

**SI-4 (16)** de organisatie correleert informatie van de controlehulpprogramma's gebruikt, in het hele informatiesysteem.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze blauwdruk Azure worden geïmplementeerd voor de logboekanalyse en verschillende OMS-oplossingen, met inbegrip van de beveiligings- en Audit-oplossing. OMS verschaft inzicht in de beveiligingsstatus van geïmplementeerde resources met gegevens die beschikbaar zijn voor geïmplementeerde OMS-oplossingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-18"></a>NIST 800 53 besturingselement SI-4 (18)

#### <a name="information-system-monitoring--analyze-traffic--covert-exfiltration"></a>Informatie systeemcontrole | Analyseer verkeer / converteren Exfiltration

**SI-4 (18)** de organisatie uitgaande communicatie-verkeer bij de externe grens van het systeem (dat wil zeggen, systeem omtrek) voor informatie en analyseert [toewijzing: interior organisatie gedefinieerd in het systeem (bijvoorbeeld subsystemen verwijst subnetwerken)] converteren exfiltration van informatie te detecteren.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het analyseren van communicatieverkeer voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-19"></a>NIST 800 53 besturingselement SI-4 (19)

#### <a name="information-system-monitoring--individuals-posing-greater-risk"></a>Informatie systeemcontrole | Personen die groter risico vormen

**SI-4 (19)** de organisatie implementeert [toewijzing: aanvullende controle organisatie gedefinieerd] van de personen die zijn geïdentificeerd door [toewijzing: bronnen organisatie gedefinieerd] een verhoogde risiconiveau inhouden.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van de personen die een groter risico. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-20"></a>NIST 800 53 besturingselement SI-4 (20)

#### <a name="information-system-monitoring--privileged-users"></a>Informatie systeemcontrole | Bevoegde gebruikers

**SI-4 (20)** de organisatie implementeert [toewijzing: aanvullende controle organisatie gedefinieerd] van bevoegde gebruikers.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de bewaking van bevoegde gebruikers. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-22"></a>NIST 800 53 besturingselement SI-4 (22)

#### <a name="information-system-monitoring--unauthorized-network-services"></a>Informatie systeemcontrole | Niet-geautoriseerde netwerkservices

**SI-4 (22)** het informatiesysteem detecteert netwerkservices die niet zijn toegestaan of goedgekeurd door [toewijzing: organisatie gedefinieerd autorisatie of goedkeuring processen] en [selectie (een of meer): audits; waarschuwingen [toewijzing: personeel organisatie gedefinieerd of -rollen]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het detecteren van niet-geautoriseerde netwerkservices. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-23"></a>NIST 800 53 besturingselement SI-4 (23)

#### <a name="information-system-monitoring--host-based-devices"></a>Informatie systeemcontrole | Host-apparaten

**SI-4 (23)** de organisatie implementeert [toewijzing: organisatie gedefinieerd host gebaseerde controle mechanismen] op [toewijzing: onderdelen van het systeem gedefinieerde organisatie informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Deze Azure blauwdruk verzamelt bewakingsgegevens van geïmplementeerde bronnen, met inbegrip van gegevens van de mogelijkheden voor bewaking op basis van een host. Microsoft Monitoring Agent is geïnstalleerd op alle Windows virtuele machines voor het verzamelen van controlegegevens die wordt gebruikt door Log Analytics en andere OMS-oplossingen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-4-24"></a>NIST 800 53 besturingselement SI-4 (24)

#### <a name="information-system-monitoring--indicators-of-compromise"></a>Informatie systeemcontrole | Indicatoren van inbreuk

**SI-4 (24)** het informatiesysteem detecteert, verzamelt, distribueert en maakt gebruik van indicatoren van inbreuk.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het detecteren, verzamelen, distribueren en met behulp van indicatoren van inbreuk bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-5a"></a>NIST 800 53 besturingselement SI-5.

#### <a name="security-alerts-advisories-and-directives"></a>Beveiligingswaarschuwingen, aanbevelingen en richtlijnen

**SI 5.** de organisatie ontvangt informatie system beveiligingswaarschuwingen, aanbevelingen en richtlijnen uit [toewijzing: externe organisaties organisatie gedefinieerd] voortdurend.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van beveiligingswaarschuwingen, aanbevelingen en richtlijnen voor bronnen klant geïmplementeerd (om op te nemen toepassingen, besturingssystemen, databases en software). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-5b"></a>NIST 800 53 besturingselement SI-5.b

#### <a name="security-alerts-advisories-and-directives"></a>Beveiligingswaarschuwingen, aanbevelingen en richtlijnen

**SI 5.b** de organisatie genereert interne beveiligingswaarschuwingen, aanbevelingen en richtlijnen benodigde.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van beveiligingswaarschuwingen, aanbevelingen en richtlijnen voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-5c"></a>NIST 800 53 besturingselement SI-5.c

#### <a name="security-alerts-advisories-and-directives"></a>Beveiligingswaarschuwingen, aanbevelingen en richtlijnen

**SI 5.c** de organisatie verspreide beveiligingswaarschuwingen, aanbevelingen en richtlijnen: [selectie (een of meer): [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen]; [Toewijzing: elementen organisatie gedefinieerd binnen de organisatie]; [Toewijzing: externe organisaties organisatie gedefinieerd]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van beveiligingswaarschuwingen, aanbevelingen en richtlijnen voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-5d"></a>NIST 800 53 besturingselement SI-5.d

#### <a name="security-alerts-advisories-and-directives"></a>Beveiligingswaarschuwingen, aanbevelingen en richtlijnen

**SI 5.d** de organisatie beveiliging richtlijnen overeenkomstig de ingestelde tijd frames implementeert, of de melding de uitgevende organisatie van de mate van niet-compatibele.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van beveiligingswaarschuwingen, aanbevelingen en richtlijnen voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-5-1"></a>NIST 800 53 besturingselement SI-5 (1)

#### <a name="security-alerts-advisories-and-directives--automated-alerts-and-advisories"></a>Beveiligingswaarschuwingen, aanbevelingen en richtlijnen | Geautomatiseerde waarschuwingen en aanbevelingen

**(1) 5-SI** veiligheidsmaatregelen geautomatiseerde mechanismen om u te maken van de beveiliging voor de organisatie waarschuwing en advisory informatie die beschikbaar zijn in de hele organisatie.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beheren van beveiligingswaarschuwingen, aanbevelingen en richtlijnen voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-6a"></a>NIST 800 53 besturingselement SI-6.a

#### <a name="security-function-verification"></a>Functie beveiligingsverificatie

**SI 6.a** het informatiesysteem controleert of de juiste werking van [toewijzing: organisatie gedefinieerd beveiligingsfuncties].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de functie beveiligingsverificatie klant geïmplementeerd bronnen (toepassingen, besturingssystemen, databases en software zijn). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-6b"></a>NIST 800 53 besturingselement SI-6.b

#### <a name="security-function-verification"></a>Functie beveiligingsverificatie

**SI 6.b** het informatiesysteem voert deze verificatie [selectie (een of meer): [toewijzing: status van het systeem gedefinieerde organisatie overgangs]; na de opdracht door gebruiker met de juiste bevoegdheden; [Toewijzing: organisatie gedefinieerde frequentie]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de functie beveiligingsverificatie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-6c"></a>NIST 800 53 besturingselement SI-6.c

#### <a name="security-function-verification"></a>Functie beveiligingsverificatie

**SI 6.c** waarschuwt het informatiesysteem [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] van verificatietests beveiliging is mislukt.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de functie beveiligingsverificatie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-6d"></a>NIST 800 53 besturingselement SI-6.d

#### <a name="security-function-verification"></a>Functie beveiligingsverificatie

**SI 6.d** het informatiesysteem [selectie (een of meer): het informatiesysteem afgesloten; het informatiesysteem; opnieuw opgestart [Toewijzing: organisatie gedefinieerde alternatieve actie (s)]] Wanneer afwijkingen gedetecteerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de functie beveiligingsverificatie voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-7"></a>NIST 800 53 besturingselement SI-7

#### <a name="software-firmware-and-information-integrity"></a>Software, Firmware en integriteit van de gegevens

**SI-7** integriteit verificatie hulpprogramma's voor het detecteren van niet-geautoriseerde wijzigingen aan de veiligheidsmaatregelen voor de organisatie [toewijzing: organisatie gedefinieerde software, firmware en informatie].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows biedt realtime-integriteit bestandsverificatie, beveiliging en herstel van essentiële systeembestanden die zijn geïnstalleerd als onderdeel van Windows of updates van Windows geautoriseerde via de Windows Resource Protection (WRP)-mogelijkheden. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-7-1"></a>NIST 800 53 besturingselement SI-7 (1)

#### <a name="software-firmware-and-information-integrity--integrity-checks"></a>Software, Firmware en integriteit van de gegevens | Integriteitscontroles

**SI-7 (1)** voert een integriteitscontrole van de informatiesysteem [toewijzing: organisatie gedefinieerde software, firmware en informatie] [selectie (een of meer): bij het opstarten; bij [toewijzing: OVERGANGS statussen organisatie gedefinieerd of beveiliging relevante gebeurtenissen]; [Toewijzing: organisatie gedefinieerde frequentie]].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows biedt realtime-integriteit bestandsverificatie, beveiliging en herstel van essentiële systeembestanden die zijn geïnstalleerd als onderdeel van Windows of updates van Windows geautoriseerde via de Windows Resource Protection (WRP)-mogelijkheden. WRP kunt realtime integriteitscontrole. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-7-2"></a>NIST 800 53 besturingselement SI-7 (2)

#### <a name="software-firmware-and-information-integrity--automated-notifications-of-integrity-violations"></a>Software, Firmware en integriteit van de gegevens | Geautomatiseerde meldingen integriteit schendingen van

**SI-7 (2)** geautomatiseerde hulpprogramma's die een melding naar de veiligheidsmaatregelen voor de organisatie [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen] bij detectie van verschillen tijdens de systeemintegriteit.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows biedt realtime-integriteit bestandsverificatie, beveiliging en herstel van essentiële systeembestanden die zijn geïnstalleerd als onderdeel van Windows of updates van Windows geautoriseerde via de Windows Resource Protection (WRP)-mogelijkheden.  |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-7-5"></a>NIST 800 53 besturingselement SI-7 (5)

#### <a name="software-firmware-and-information-integrity--automated-response-to-integrity-violations"></a>Software, Firmware en integriteit van de gegevens | Geautomatiseerd antwoord op integriteit schendingen

**SI-7 (5)** het informatiesysteem automatisch [selectie (een of meer): het informatiesysteem afgesloten; de informatiesysteem wordt opnieuw opgestart; implementeert [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen]] wanneer integriteit schendingen zijn gedetecteerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het automatisch reageren op integriteit schendingen binnen klant geïmplementeerde resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-7-7"></a>NIST 800 53 besturingselement SI-7 (7)

#### <a name="software-firmware-and-information-integrity--integration-of-detection-and-response"></a>Software, Firmware en integriteit van de gegevens | Integratie van detectie en reactie

**SI-7 (7)** opgenomen met de organisatie de detectie van niet-geautoriseerde [toewijzing: beveiliging relevante wijzigingen in het informatiesysteem organisatie gedefinieerd] in de mogelijkheid van de organisatie-respons op incidenten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het beveiligen van software en informatie integriteit voor bronnen klant geïmplementeerd. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-7-14"></a>NIST 800 53 besturingselement SI-7 (14)

#### <a name="software-firmware-and-information-integrity--binary-or-machine-executable-code"></a>Software, Firmware en integriteit van de gegevens | Binair of machines uitvoerbare Code

**SI-7 (14)** de organisatie verbiedt het gebruik van binaire of machine-uitvoerbare code uit bronnen met weinig of geen enkele garantie en zonder het verstrekken van broncode; en uitzonderingen aan de vereiste bron code alleen voor dwingende missie biedt / operationele vereisten en met goedkeuring van de autorisatie officiële.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant op bedrijfsniveau systeem- en integriteit procedures mogelijk tot stand brengen vereisten voor het verkrijgen van de broncode van de binaire of machines uitvoerbare code uit sommige bronnen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-8a"></a>NIST 800 53 besturingselement SI-8.

#### <a name="spam-protection"></a>Beveiliging van spam

**SI 8.** het beveiligingsmechanisme organisatie gebruik wordt gemaakt spam op informatie system toegang naar en uitgang punten om te detecteren en actie ondernemen door ongewenste berichten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen e-servers geïmplementeerd als onderdeel van deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-8b"></a>NIST 800 53 besturingselement SI-8.b

#### <a name="spam-protection"></a>Beveiliging van spam

**SI 8.b** het organisatie updates spam beveiligingsmechanisme wanneer nieuwe versies beschikbaar zijn in overeenstemming met de configuratie van de organisatie-management-beleid en procedures.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen e-servers geïmplementeerd als onderdeel van deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-8-1"></a>NIST 800 53 besturingselement SI-8 (1)

#### <a name="spam-protection--central-management"></a>Beveiliging tegen spam | Centraal beheer

**(1) SI-8** de organisatie beheert centraal mechanismen voor het beveiligen van spam.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen e-servers geïmplementeerd als onderdeel van deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ### <a name="nist-800-53-control-si-8-2"></a>NIST 800 53 besturingselement SI-8 (2)

#### <a name="spam-protection--automatic-updates"></a>Beveiliging tegen spam | Automatische Updates

**(2) SI-8** informatie automatisch bijgewerkt mechanismen voor het beveiligen van spam.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | Er zijn geen e-servers geïmplementeerd als onderdeel van deze blauwdruk Azure. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-10"></a>NIST 800 53 besturingselement SI-10

#### <a name="information-input-validation"></a>Informatie-validatie voor invoer

**SI 10** controleert de geldigheid van de informatiesysteem [toewijzing: organisatie gedefinieerde informatie invoer].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor de validatie voor invoer informatie klant geïmplementeerd bronnen (toepassingen, besturingssystemen, databases en software zijn). |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-11a"></a>NIST 800 53 besturingselement SI-11.a

#### <a name="error-handling"></a>Foutafhandeling

**SI 11.a** foutberichten die informatie die nodig is voor de corrigerende maatregelen bieden zonder weer te geven informatie op die kan worden misbruikt door tegenstanders wordt de informatie gegenereerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De resources die zijn geïmplementeerd door deze blauwdruk Azure gebruikmaken van commerciële besturingssystemen en toepassingen. Deze software maakt gebruik van beste praktijken, zodat gevoelige informatie wordt niet getoond in foutberichten. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-11b"></a>NIST 800 53 besturingselement SI-11.b

#### <a name="error-handling"></a>Foutafhandeling

**SI 11.b** het informatiesysteem blijkt foutberichten alleen [toewijzing: ondersteuningspersoneel organisatie gedefinieerd of rollen].

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De resources die zijn geïmplementeerd door deze blauwdruk Azure gebruikmaken van commerciële besturingssystemen en toepassingen. Deze software wordt beste praktijken gebruikt voor foutberichten die in de context van de toepassingen die het bericht ontvangen. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-12"></a>NIST 800 53 besturingselement SI-12

#### <a name="information-handling-and-retention"></a>De informatieverwerking van de en retentie

**SI-12** verwerkt en gegevens binnen de informatiesysteem en de uitvoer van de informatie uit het systeem in overeenstemming met federale wetgeving, behoudt de organisatie Executive Orders, richtlijnen, beleid, voorschriften, standaarden, en operationele vereisten.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De klant is verantwoordelijk voor het verwerken en bewaren van gegevens binnen de klant geïmplementeerde resources (om op te nemen toepassingen, besturingssystemen, databases en software) en informatie-uitvoer van deze resources. |
| **Provider (Microsoft Azure)** | Niet van toepassing |


 ## <a name="nist-800-53-control-si-16"></a>NIST 800 53 besturingselement SI-16

#### <a name="memory-protection"></a>Geheugenbeveiliging

**SI-16** door het informatiesysteem geïmplementeerd [toewijzing: organisatie gedefinieerd beveiligingsmaatregelen] het geheugen beschermen tegen onbevoegde code kan worden uitgevoerd.

**Verantwoordelijkheden:**`Customer Only`

|||
|---|---|
| **De klant** | De virtuele machines geïmplementeerd door deze Azure blauwdruk Windows-besturingssystemen worden uitgevoerd. Windows beschikt over bescherming om te voorkomen dat code wordt uitgevoerd in de beperkte geheugenlocaties: Er is geen uitvoeren (NX), adres ruimte indeling willekeurige (ASLR) en preventie van gegevensuitvoering (DEP). |
| **Provider (Microsoft Azure)** | Niet van toepassing |
