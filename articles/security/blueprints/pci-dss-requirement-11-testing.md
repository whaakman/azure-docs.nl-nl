---
title: Azure betaling verwerking blauwdruk - vereisten testen
description: PCI-DSS vereiste 11
services: security
documentationcenter: na
author: simorjay
manager: mbaldwin
editor: tomsh
ms.assetid: 9753853b-ad80-4be4-9416-2583b8fd2029
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/15/2017
ms.author: frasim
ms.openlocfilehash: 57429741afd2ffd16c09a0f1485cb1cfbdda5571
ms.sourcegitcommit: 7d107bb9768b7f32ec5d93ae6ede40899cbaa894
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="testing-requirements-for-pci-dss-compliant-environments"></a>Vereisten voor PCI DSS-compatibele omgevingen testen 
## <a name="pci-dss-requirement-11"></a>PCI-DSS vereiste 11

**Regelmatig testen beveiligingssystemen en processen**

> [!NOTE]
> Deze vereisten zijn gedefinieerd door de [Payment Card Industry (PCI) Security Standards Council](https://www.pcisecuritystandards.org/pci_security/) als onderdeel van de [PCI Data Security Standard (DSS), versie 3.2](https://www.pcisecuritystandards.org/document_library?category=pcidss&document=pci_dss). Raadpleeg de PCI DSS voor informatie over het testen van de procedures en richtlijnen voor elke vereiste.

Beveiligingsproblemen worden voortdurend door kwaadwillende personen en onderzoekers en die zijn geïntroduceerd door nieuwe software wordt gedetecteerd. Onderdelen van het systeem, processen en aangepaste software moeten regelmatig worden getest om ervoor te zorgen beveiligingsmechanismen blijven in overeenstemming met een veranderende omgeving.

## <a name="pci-dss-requirement-111"></a>PCI-DSS vereiste 11.1

**11.1** implementeren te testen op de aanwezigheid van draadloze toegangspunten (802.11) en opsporen en identificeren van alle processen geautoriseerd en niet-geautoriseerde draadloze toegangspunten, per kwartaal.

> [!NOTE]
> Methoden die kunnen worden gebruikt in het proces omvatten, maar niet beperkt tot het draadloze netwerk scans, fysieke/logische controles van de onderdelen van het systeem en infrastructuur, netwerk-toegangsbeheer (NAC) of draadloze-id's / IP-Adressen zijn.
Welke methoden worden gebruikt, moeten ze voldoende zijn voor het opsporen en identificeren van geautoriseerde en niet-geautoriseerde apparaten zijn.


**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure niet toestaan of draadloze verbindingen in de Azure netwerkomgeving toestaan. Interne beveiliging teams scant regelmatig op draadloze signalen rogue per kwartaal en rogue signalen worden onderzocht en verwijderd. Klanten zijn niet toegestaan voor het implementeren van draadloze technologie in de Azure-omgeving. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | SNMP- en draadloze netwerken worden niet geïmplementeerd in de oplossing.|



### <a name="pci-dss-requirement-1111"></a>PCI-DSS vereiste 11.1.1

**11.1.1** een inventaris van geautoriseerde draadloze toegangspunten, met inbegrip van een gedocumenteerde zakelijke reden onderhouden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 11.1](#pci-dss-requirement-11-1). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | SNMP- en draadloze netwerken worden niet geïmplementeerd in de oplossing.|



### <a name="pci-dss-requirement-1112"></a>PCI-DSS vereiste 11.1.2

**11.1.2** respons op incidenten procedures implementeren in het geval van niet-geautoriseerde draadloze toegangspunten worden gedetecteerd.


**Verantwoordelijkheden:&nbsp;&nbsp;`Microsoft Azure Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 11.1](#pci-dss-requirement-11-1). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | SNMP- en draadloze netwerken worden niet geïmplementeerd in de oplossing.|



## <a name="pci-dss-requirement-112"></a>PCI-DSS vereiste 11.2

**11.2** uitvoeren interne en externe netwerk beveiligingslek scant ten minste per kwartaal en na elke belangrijke wijziging in het netwerk (zoals nieuwe system component installaties, wijzigingen in de netwerktopologie, firewall-regel wijzigingen, product upgrades). 

> [!NOTE]
> Meerdere scan rapporten kunnen worden gecombineerd voor het kwartaal scanproces om weer te geven dat alle systemen zijn gescand en alle problemen in de van toepassing zijn gericht. Aanvullende documentatie kan moet controleren of niet-hersteld zwakke plekken zijn bezig te worden toegepast.
> Voor de eerste PCI DSS-compatibiliteit is niet vereist dat vier kwartalen van het doorgeven van scans worden uitgevoerd als de beoordelaar wordt gecontroleerd door 1) de meest recente resultaten van de scan is een scan doorgeven, 2) de entiteit is beschreven in de beleidsregels en procedures vereisen van elk kwartaal scannen en 3) zoals in een re-scan(s) zijn beveiligingsproblemen vermeld in de scanresultaten opgelost. Voor de volgende jaren nadat de eerste PCI DSS bekijken, moet vier kwartalen scans doorgeven van zijn opgetreden.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure voert scans op zwakke plekken elk kwartaal interne en externe. Scans zijn uitgevoerd door gekwalificeerde personeel. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is pen getest en beveiligingslek in een poging 'omdat' gescand. De testresultaten pen kunnen worden gedupliceerd met algemene hulpprogramma's zoals nmap of pentest tools.com. De resultaten van de test Pen bieden onduidelijk kwetsbaarheid voor aanvallen, geen misbruik objecten. Bovendien [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieden informatie over het beveiligingslek en het doorvoeren.|



### <a name="pci-dss-requirement-1121"></a>PCI-DSS vereiste 11.2.1

**11.2.1** elk kwartaal interne beveiligingslek-scans uitvoeren. Beveiligingslekken en voer scant opnieuw om te controleren of dat alle 'hoog risico' zwakke plekken zijn opgelost in overeenstemming met de entiteit beveiligingslek positie (per vereiste 6.1 of hoger). Scans moeten worden uitgevoerd door gekwalificeerde personeel.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure voert scans voor beveiligingsproblemen op de onderliggende infrastructuur-scope. Microsoft Azure implementeert beveiligingslek scannen op server-besturingssystemen, databases en netwerkapparaten met de juiste beveiligingslek scanprogramma's. Azure-webtoepassingen worden gescand met de juiste industrie oplossingen scannen. Scans op zwakke plekken worden uitgevoerd op elk kwartaal.<br /><br />Scant opnieuw worden uitgevoerd naar behoefte tegen alle systemen tot alle 'met een hoog risico' zwakke plekken (zoals beschreven in vereiste 6.1) opgelost zijn. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is pen getest en beveiligingslek in een poging 'omdat' gescand. De testresultaten pen kunnen worden gedupliceerd met algemene hulpprogramma's zoals nmap of pentest tools.com. De resultaten van de test Pen bieden onduidelijk kwetsbaarheid voor aanvallen, geen misbruik objecten. Bovendien [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieden informatie over het beveiligingslek en het doorvoeren.|



### <a name="pci-dss-requirement-1122"></a>PCI-DSS vereiste 11.2.2

**11.2.2** elk kwartaal externe beveiligingslek scans, via een goedgekeurd scannen leverancier (ASV) goedgekeurd door betaling kaart bedrijfstak Security Standards Council (PCI SSC) uitvoeren. Voer scant opnieuw, indien nodig, totdat doorgeven scans worden verkregen. 

> [!NOTE]
> Elk kwartaal externe beveiligingslek scans moeten worden uitgevoerd door een goedgekeurd scannen leverancier (ASV), goedgekeurd door betaling kaart bedrijfstak Security Standards Council (PCI SSC).
> Raadpleeg de gids ASV gepubliceerd op de website PCI SSC voor scan verantwoordelijkheden van de klant, scan voorbereiding, enzovoort.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure voert externe scans voor beveiligingsproblemen op onderliggende infrastructuur-scope die extern toegankelijk is. Door een goedgekeurd scannen leverancier (ASV)-scans zijn uitgevoerd.<br /><br />Microsoft Azure MSRC/OSSC maandelijkse patch meldingen op is geabonneerd en scans op beveiligingsproblemen minimaal eenmaal per kwartaal. Aangegeven beveiligingslekken zijn geëvalueerd en per vastgestelde tijdlijn op basis van het risiconiveau dat is hersteld.<br /><br />Elk kwartaal gericht uitgebreide beveiligingsprobleem gescand ten opzichte van de prioriteit van de onderdelen van de Microsoft Azure-omgeving wordt uitgevoerd om beveiligingsproblemen identificeren. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Wanneer het implementeren van de Contoso Webstore, klanten van de demo zijn verantwoordelijk voor het uitvoeren van elk kwartaal externe beveiligingslek gescand en scant opnieuw naar behoefte tegen alle exemplaren van de PaaS in hun omgeving in kaarthouder gegevens (CDE) met behulp van een goedgekeurd scannen leverancier (ASV) door betaling Card Industry Security Standards Council goedgekeurd.<br /><br />|



### <a name="pci-dss-requirement-1123"></a>PCI-DSS vereiste 11.2.3

**11.2.3** interne en externe scans en scant opnieuw, indien nodig, na elke belangrijke wijziging uitvoeren. Scans moeten worden uitgevoerd door gekwalificeerde personeel.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Resultaten worden gerapporteerd aan belanghebbenden en herstel wordt gevolgd door het Azure Security-Team via sluiting. Azure testresultaten mag worden gedeeld met klanten onder geheimhoudingsverklaring. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Klanten zijn verantwoordelijk voor het uitvoeren van elk kwartaal interne en externe beveiligingslek scans en scant opnieuw naar behoefte tegen alle PaaS-instanties in hun CDE. Scans moeten nadat belangrijke wijzigingen in de omgeving in het bereik worden uitgevoerd.<br /><br />Scans moeten worden uitgevoerd door een ASV of medewerkers met organisatie-onafhankelijkheid.|



## <a name="pci-dss-requirement-113"></a>PCI-DSS vereiste 11, lid 3

**11.3** implementeren van een methodologie voor het testen van binnendringen die het volgende omvat:
- Is gebaseerd op een algemeen geaccepteerde binnendringen benaderingen (bijvoorbeeld NIST SP800-115) testen
- Bevat dekking is voor de hele CDE perimeter- en kritieke systemen
- Testen van zowel binnen als buiten het netwerk bevat
- Testen om te valideren elke segmentering en scope-vermindering besturingselementen bevat
- Definieert de toepassingslaag binnendringen tests voor het toevoegen van ten minste de kwetsbaarheden die worden vermeld in de vereiste 6.5
- Definieert de netwerklaag binnendringen tests uit om het opnemen van onderdelen die netwerkfuncties als besturingssystemen ondersteunen
- Bevat leest en afweging van bedreigingen en zwakke plekken die zijn opgetreden in de afgelopen 12 maanden
- Hiermee geeft u de retentie van binnendringen resultaten en herstel activiteiten resultaten testen

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure services met behulp van derden binnendringen valideert testen op basis van de OWASP (Open Web Application Security-Project), top 10 met behulp van testers CREST gecertificeerd. De resultaten van de testen worden bijgehouden via een register risico, die wordt gecontroleerd en gecontroleerd op regelmatige basis zodat u voldoet aan de procedures voor beveiliging. <br /><br />Microsoft gebruikt ook rood teams op basis van door Microsoft beheerde infrastructuur, services en toepassingen. Er is geen einde-klantgegevens is opzettelijk gericht tijdens rood teams en live site binnendringen testen. De tests zijn tegen Microsoft Azure-infrastructuur en platforms, evenals van het Microsoft-toepassingen en gegevens. Klant-tenants, toepassingen en gegevens die worden gehost in Azure is nooit gericht.<br /><br />Microsoft Azure is een onafhankelijke beoordelaar een systeem assessment plan ontwikkelen en uitvoeren van een beoordeling besturingselementen gebruikt. Besturingselementen beoordelingen jaarlijks worden uitgevoerd en de resultaten worden gerapporteerd aan de relevante partijen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is pen getest en beveiligingslek in een poging 'omdat' gescand. De testresultaten pen kunnen worden gedupliceerd met algemene hulpprogramma's zoals nmap of pentest tools.com. De resultaten van de test Pen bieden onduidelijk kwetsbaarheid voor aanvallen, geen misbruik objecten. Bovendien [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieden informatie over het beveiligingslek en het doorvoeren.|



### <a name="pci-dss-requirement-1131"></a>PCI-DSS vereiste 11.3.1

**11.3.1** uitvoeren *externe* binnendringen testen ten minste jaarlijks en na een aanzienlijke infrastructuur of een upgrade van de toepassing of een wijziging (zoals de upgrade van een besturingssysteem, een submap netwerk toegevoegd aan de omgeving of een webserver die is toegevoegd aan de omgeving).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 11.3](#pci-dss-requirement-11-3). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is pen getest en beveiligingslek in een poging 'omdat' gescand. De testresultaten pen kunnen worden gedupliceerd met algemene hulpprogramma's zoals nmap of pentest tools.com. De resultaten van de test Pen bieden onduidelijk kwetsbaarheid voor aanvallen, geen misbruik objecten. Bovendien [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieden informatie over het beveiligingslek en het doorvoeren.|



### <a name="pci-dss-requirement-1132"></a>PCI-DSS vereiste 11.3.2

**11.3.2** uitvoeren *interne* binnendringen testen ten minste jaarlijks en na een aanzienlijke infrastructuur of een upgrade van de toepassing of een wijziging (zoals de upgrade van een besturingssysteem, een submap netwerk toegevoegd aan de omgeving of een webserver die is toegevoegd aan de omgeving).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure-opdrachten met onafhankelijke beoordelaars binnendringen testen van de Microsoft Azure-grens. Rood Team oefeningen ook regelmatig worden uitgevoerd en resultaten gebruikt om de verbeterde beveiliging. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is pen getest en beveiligingslek in een poging 'omdat' gescand. De testresultaten pen kunnen worden gedupliceerd met algemene hulpprogramma's zoals nmap of pentest tools.com. De resultaten van de test Pen bieden onduidelijk kwetsbaarheid voor aanvallen, geen misbruik objecten. Bovendien [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieden informatie over het beveiligingslek en het doorvoeren.|



### <a name="pci-dss-requirement-1133"></a>PCI-DSS vereiste 11.3.3

**11.3.3** misbruik zwakke plekken die zijn gevonden tijdens het testen van binnendringen zijn gecorrigeerd en testen om te controleren of de correcties wordt herhaald.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Procedures zijn voor het bewaken van de Microsoft Azure-platform-onderdelen voor bekende beveiligingsproblemen gemaakt. <br /><br /><br /><br />Elk kwartaal gericht uitgebreide beveiligingsprobleem gescand ten opzichte van de prioriteit van de onderdelen van de Azure productieomgeving wordt uitgevoerd om beveiligingsproblemen identificeren. Resultaten worden gerapporteerd aan belanghebbenden en herstel wordt gevolgd door het team via sluiting. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations), die bieden informatie over het beveiligingslek en het doorvoeren, zijn gebruikt om ervoor te zorgen dat alle openstaande problemen voor de demo Contoso Webstore CDE zijn hersteld.|



### <a name="pci-dss-requirement-1134"></a>PCI-DSS vereiste 11.3.4

**11.3.4** als segmentering wordt gebruikt voor het isoleren van het CDE naar andere netwerken, binnendringen tests uitvoeren ten minste jaarlijks en na de eventuele wijzigingen in segmentering besturingselementen/methoden om te controleren of de methoden segmentering operationele en effectieve, en alle systemen van buiten het bereik van systemen in het CDE isoleren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Procedures zijn voor het bewaken van de Microsoft Azure-platform-onderdelen voor bekende beveiligingsproblemen gemaakt. <br /><br /><br /><br />Elk kwartaal gericht uitgebreide beveiligingsprobleem gescand ten opzichte van de prioriteit van de onderdelen van de Azure productieomgeving wordt uitgevoerd om beveiligingsproblemen identificeren. Resultaten worden gerapporteerd aan belanghebbenden en herstel wordt gevolgd door het team via sluiting. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations), die bieden informatie over het beveiligingslek en het doorvoeren, zijn gebruikt om ervoor te zorgen dat alle openstaande problemen voor de demo Contoso Webstore CDE zijn hersteld.|



### <a name="pci-dss-requirement-11341"></a>PCI-DSS vereiste 11.3.4.1

**11.3.4.1** *aanvullende vereisten voor alleen-providers:* als segmentering wordt gebruikt, controleert u het bereik PCI DSS door het uitvoeren van binnendringen testen op segmentering bepaalt ten minste elke 6 maanden en na wijzigingen segmentering besturingselementen/methoden.

> [!NOTE]
> Deze vereiste is een aanbevolen procedure tot en met 31 januari 2018, waarna een vereiste wordt.


**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Zie de sectie 'Microsoft Azure' voor [vereiste 11.3.4](#pci-dss-requirement-11-3-4). |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations), die bieden informatie over het beveiligingslek en het doorvoeren, zijn gebruikt om ervoor te zorgen dat alle openstaande problemen voor de demo Contoso Webstore CDE zijn hersteld.|



## <a name="pci-dss-requirement-114"></a>PCI-DSS vereiste 11.4

**11.4** inbraakdetectie en/of inbraakdetectie preventie technieken gebruiken om te detecteren en/of te voorkomen dat beveiligingsrisico's in het netwerk. Alle verkeer in het perimeternetwerk van de kaarthouder gegevensomgeving, evenals op kritieke punten in de omgeving van de gegevens kaarthouder bewaken en waarschuwen van medewerkers vermoedelijke inbreuk.
Inbraakdetectie en preventie engines, basislijnen en handtekeningen up-to-date te houden.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure voert realtime analyses van gebeurtenissen in de operationele omgeving en systemen van id's genereren bijna realtime-waarschuwingen over gebeurtenissen die mogelijk inbreuk is gemaakt van het systeem. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore is een PaaS-service en netwerk inbraakdetectie en preventie Raadpleeg verantwoordelijkheid van Azure. [Azure Security Center](https://azure.microsoft.com/services/security-center/) en [Azure Advisor](/azure/advisor/advisor-security-recommendations) bieden inbraakdetectie waarschuwingen en het doorvoeren.|



## <a name="pci-dss-requirement-115"></a>PCI-DSS vereiste 11.5

**11.5** een mechanisme voor detecteren van gewijzigde's (bijvoorbeeld bestand integriteit controlehulpprogramma's) op de waarschuwing medewerkers onrechtmatige wijzigingen (met inbegrip van wijzigingen en toevoegingen, verwijderingen) van essentiële systeembestanden, configuratiebestanden of inhoud implementeren bestanden. en de software vergelijken van kritieke bestand minstens eenmaal per week configureren. 

> [!NOTE]
> Voor detectie van wijziging doeleinden zijn essentiële bestanden meestal degenen die niet regelmatig te wijzigen, maar de wijziging van dit kan duiden op een inbreuk op het systeem of het risico op inbreuk. Wijziging detectiemechanismen zoals-integriteit bewaking producten zijn meestal vooraf geconfigureerd met kritieke bestanden voor het bijbehorende besturingssysteem. Andere essentiële bestanden, zoals die voor aangepaste toepassingen moeten worden geëvalueerd en worden gedefinieerd door de entiteit (dat wil zeggen, de zakelijke of service provider).

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Microsoft Azure onderhoudt en waarschuwt klanten van mogelijke wijzigingen en gebeurtenissen die mogelijk van invloed op beveiliging en beschikbaarheid van de services via een online Service-Dashboard. Wijzigingen in de beveiligingsverplichtingen en verplichtingen van de beveiliging van Microsoft Azure-klanten worden bijgewerkt op de website van Microsoft Azure tijdig.<br /><br />Installatie of wijzigingen in de software in Microsoft Azure productie-omgeving beperkt tot is geautoriseerd beheer personeel en volgt management procedures wijzigen. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | Contoso Webstore demo is een PaaS-service en opsporing is geïmplementeerd met behulp van OMS. Zie voor meer informatie [richtlijnen PCI - vooraf geïnstalleerde OMS oplossingen](payment-processing-blueprint.md#oms-solutions).<br /><br />|



### <a name="pci-dss-requirement-1151"></a>PCI-DSS vereiste 11.5.1

**11.5.1** een proces om te reageren op waarschuwingen die worden gegenereerd door de oplossing voor het detecteren van gewijzigde's implementeren.

**Verantwoordelijkheden:&nbsp;&nbsp;`Shared`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Azure event bewakingsregels bieden een hogere niveau bewaking voor bewerkingen met een hoog risico en activa. Azure beheerde netwerkapparaten worden gecontroleerd op naleving van tot stand gebrachte beveiligingsstandaarden. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore waarschuwingen voor wijzigingen worden geleverd door de OMS-implementatie. Zie voor meer informatie [richtlijnen PCI - vooraf geïnstalleerde OMS oplossingen](payment-processing-blueprint.md#oms-solutions).<br /><br /><br /><br />|



## <a name="pci-dss-requirement-116"></a>PCI-DSS vereiste 11,6

**11,6** Zorg ervoor dat beveiligingsbeleid en operationele procedures voor beveiliging controleren en testen wordt gedocumenteerd, dat in gebruik is, en bekend is dat alle betrokken partijen.

**Verantwoordelijkheden:&nbsp;&nbsp;`Customer Only`**

|||
|---|---|
| **Provider<br />(Microsoft&nbsp;Azure)** | Niet van toepassing. |
| **Klant<br />(PCI &#8209; DSS&nbsp;blauwdruk)** | De Contoso Webstore waarschuwingen voor wijzigingen worden geleverd door de OMS-implementatie. Zie voor meer informatie [richtlijnen PCI - vooraf geïnstalleerde OMS oplossingen](payment-processing-blueprint.md#oms-solutions).<br /><br /><br /><br />|




