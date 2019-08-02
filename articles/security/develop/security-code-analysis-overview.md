---
title: Overzicht van de documentatie over Microsoft Azure Security code analyse
description: Dit artikel bevat een overzicht van de uitbrei ding voor de analyse van beveiligings code
author: vharindra
manager: sukhans
ms.author: terrylan
ms.date: 07/31/2019
ms.topic: article
ms.service: security
services: azure
ms.assetid: 521180dc-2cc9-43f1-ae87-2701de7ca6b8
ms.devlang: na
ms.tgt_pltfrm: na
ms.workload: na
ms.openlocfilehash: 283d63bafc583f2ac9da3294644aaebd17d7c950
ms.sourcegitcommit: a52f17307cc36640426dac20b92136a163c799d0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68718286"
---
# <a name="about-microsoft-security-code-analysis"></a>Informatie over analyse van micro soft-beveiligings code

Met de **uitbrei ding analyse van beveiligings code van micro soft** kunnen teams de analyse van beveiligings codes naadloos integreren in hun Azure DevOps CI/cd-pijp lijnen, zoals wordt aanbevolen door de [sdl-experts (Secure Development Lifecycle)](https://www.microsoft.com/securityengineering/sdl/practices) bij micro soft. De beveiliging wordt vereenvoudigd door een consistente UX die de complexiteit van het uitvoeren van verschillende hulpprogram ma's opabstractt. Met een op NuGet gebaseerde levering van de hulpprogram ma's hoeven teams de installatie of het bijwerken van het hulp programma niet meer te beheren. Met de opdracht regel-en Basic-taak interfaces kunnen alle gebruikers, van het slimme hulp programma Gurus tot dagelijks ontwikkel aars, zo weinig of zoveel controle over de hulpprogram ma's hebben als ze dat willen. Teams kunnen ook gebruikmaken van krachtige functies voor het verwerken van berichten, zoals publicatie logboeken voor retentie, het genereren van rapporten die gericht zijn op ontwikkel aars & het configureren van build-onderbrekingen voor regressies.

## <a name="why-microsoft-security-code-analysis"></a>Waarom analyse van micro soft-beveiligings code

### <a name="security-simplified"></a>Beveiliging vereenvoudigd

Het toevoegen van hulpprogram ma's voor analyse van beveiligings code aan uw Azure DevOps-pijp lijn is net zo eenvoudig als het toevoegen van nieuwe taken. Pas ze aan of ga met de standaard waarden. De taken worden uitgevoerd als onderdeel van uw DevOps-pijp lijn en genereren logboeken die alle soorten resultaten beschrijven.

### <a name="clean-builds"></a>Schone builds

Nadat u de eerste problemen hebt opgelost die door de hulpprogram ma's worden gerapporteerd, kunt u de uitbrei ding configureren voor het verbreken van de builds van nieuwe problemen. Het instellen van een continue integratie op elke pull-aanvraag is nog nooit zo eenvoudig geweest.

### <a name="set-it-and-forget-it"></a>Instellen en het verg eten

De bouw taken en hulpprogram ma's kunnen zo worden ingesteld dat deze up-to-date blijven (en standaard zijn). Als er een bijgewerkte versie van het hulp programma is, hoeft u dit niet te downloaden en te installeren. deze uitbrei ding zorgt voor u. 

>>>
### <a name="under-the-hood"></a>Onder de motorkap

Met de micro soft security code Analysis extension bouwt u de complexe dingen van:
  - Dynamische analyse hulpprogramma's voor beveiliging uitvoeren en
  - De resultaten van de logboek bestanden verwerken om een samenvattings rapport te maken of de build te verbreken.
>>>

## <a name="security-code-analysis-toolset"></a>Hulp programma voor analyse van beveiligings code

De uitbrei ding voor de analyse van micro soft-beveiligings code maakt de nieuwste versies van belang rijke analyse hulpprogramma's eenvoudig beschikbaar. De uitbrei ding bevat zowel interne als open-source tools van micro soft. De hulpprogram ma's worden automatisch gedownload in de Cloud-gehoste agent nadat u & de pijp lijn hebt geconfigureerd met behulp van de bijbehorende bouw taak. Hieronder ziet u de set hulpprogram ma's die vandaag beschikbaar zijn in de uitbrei ding. Blijf op de hoogte voor meer en stuur ons uw suggesties voor hulp middelen die kunnen worden toegevoegd.

### <a name="anti-malware-scanner"></a>Scanner voor anti-malware

De anti-malware scanner bouwen is nu opgenomen in de uitbrei ding voor de analyse van beveiligings code van micro soft. Dit moet worden uitgevoerd op een build-agent waarop Windows Defender al is geïnstalleerd. Bezoek de website van [Defender](https://aka.ms/defender) voor meer informatie 

### <a name="binskim"></a>BinSkim

BinSkim is een Portable uitvoer bare (PE) licht gewicht scanner waarmee compiler-en koppelings instellingen en andere beveiligings kenmerken worden gevalideerd. De taak bouwen biedt een opdracht regel wrapper rond de toepassing BinSkim. exe. BinSkim is een open source-hulp programma en ga voor meer informatie naar [BinSkim op github](https://github.com/Microsoft/binskim)

### <a name="credential-scanner"></a>Referentie scanner

Wacht woorden en andere geheimen die zijn opgeslagen in bron code, zijn momenteel een aanzienlijk probleem. Referentie scanner is een eigen, statisch analyse hulpprogramma dat referenties, geheimen, certificaten en andere gevoelige inhoud in uw bron code en uw build-uitvoer detecteert.

### <a name="microsoft-security-risk-detection"></a>Micro soft-beveiligings risico detectie

Detectie van beveiligings Risico's is de unieke op de cloud gebaseerde benadering van micro soft voor het opsporen van zwakke plekken in software. Deze service vereist een afzonderlijk voorbereidings proces. Ga voor meer informatie naar [MSRD op docs.Microsoft.com](https://docs.microsoft.com/security-risk-detection/)

### <a name="roslyn-analyzers"></a>Roslyn-analyse functies

Het met compileren geïntegreerde statische analyse hulpprogramma van micro soft voor het analyserenC# van beheerde code (en VB). Ga voor meer informatie naar [Roslyn-analyse functies op docs.Microsoft.com](https://docs.microsoft.com/dotnet/standard/analyzers/)

### <a name="tslint"></a>TSLint

TSLint is een uitbreidbaar, statisch analyse hulpprogramma dat type script code controleert op lees-, onderhouds-en functionaliteits fouten. Het wordt wereld wijd ondersteund in moderne editors en bouwt systemen en kan worden aangepast met uw eigen lint regels, configuraties en formatteringen. TSLint is een open source-hulp programma en ga voor meer informatie naar [TSLint op github](https://github.com/palantir/tslint)

## <a name="analysis-and-post-processing-of-results"></a>Analyse en naverwerking van resultaten

De uitbrei ding voor de analyse van beveiligings code van micro soft heeft ook drie nuttige taken na de verwerking om de resultaten te verwerken en te analyseren die worden gevonden door de taken van de beveiligings hulpprogramma's. Ze worden meestal toegevoegd aan de pijp lijn na alle andere hulp middelen.

### <a name="publish-security-analysis-logs"></a>Logboeken voor beveiligings analyse publiceren
Met de taak logboeken van beveiligings analyse publiceren worden de logboek bestanden van de beveiligings hulpprogramma's die worden uitgevoerd tijdens de build voor onderzoek en follow-up bewaard.

Ze kunnen worden gepubliceerd in de Azure server-artefacten (als een zip-bestand) of vanuit uw Private build-agent kopiëren naar een toegankelijke bestands share.

### <a name="security-report"></a>Beveiligings rapport
Met de taak beveiligings rapport maken worden de logboek bestanden geparseerd die worden gemaakt door de beveiligings hulpprogramma's die tijdens het bouwen worden uitgevoerd. er wordt één samenvattings rapport bestand gemaakt met alle problemen die worden gevonden door de analyse hulpprogramma's.

De taak kan worden geconfigureerd om bevindingen voor specifieke hulpprogram ma's of voor alle hulpprogram ma's te rapporteren, en u kunt ook kiezen welk niveau van problemen (fouten of fouten en waarschuwingen) moet worden gerapporteerd.

### <a name="post-analysis-build-break"></a>Na de analyse (build-einde)
Met de build-taak na de analyse kan de klant een build-einde injecteren en de build mislukken in het geval van een of meer analyse hulpprogramma's die in de code worden gerapporteerd.

De taak kan worden geconfigureerd om de build te verbreken van problemen die zijn gevonden met specifieke hulpprogram ma's of voor alle hulpprogram ma's, en ook op basis van de ernst van problemen die zijn gevonden (fouten of waarschuwingen).

>[!NOTE]
>Afzonderlijke build-taken slagen op basis van het ontwerp, op voor waarde dat het hulp programma is voltooid, ongeacht of er conclusies zijn of niet, zodat de build kan worden uitgevoerd om alle hulpprogram ma's uit te voeren.

## <a name="next-steps"></a>Volgende stappen

Voor instructies voor het onboarden en installeren van de analyse van de beveiligings code raadpleegt u de [hand leiding](security-code-analysis-onboard.md) voor het onboarden en installeren

Zie onze [configuratie handleiding](security-code-analysis-customize.md) voor meer informatie over het configureren van de build-taken

[Raadpleeg onze pagina Veelgestelde vragen](security-code-analysis-faq.md) als u meer vragen hebt over de uitbrei ding en de hulpprogram ma's die worden aangeboden.