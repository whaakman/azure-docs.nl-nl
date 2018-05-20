---
title: Azure Application Insights gebruik cohorten | Microsoft Docs
description: Verschillende groepen of gebruikers, sessies, gebeurtenissen of bewerkingen die iets hebben analyseren
services: application-insights
documentationcenter: ''
author: mrbullwinkle
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 04/10/2018
ms.author: mbullwin ; daviste
ms.openlocfilehash: f8d566f552c86f749b914ffed70512209ad76ab7
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="application-insights-cohorts"></a>Application Insights cohorten

Een cohort is een set van gebruikers, sessies, gebeurtenissen of bewerkingen die iets gemeen hebben. In Azure Application Insights, worden cohorten gedefinieerd door een query analytics. In gevallen waar u hebt voor het analyseren van een specifieke groep gebruikers of gebeurtenissen herhaaldelijk, cohorten kunnen biedt u meer flexibiliteit om te drukken exact de set die u geïnteresseerd bent in.

![Deelvenster cohorten](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorten versus basic filters

Cohorten worden gebruikt in manieren vergelijkbaar met filters. Maar cohorten definities zijn gebouwd op basis van aangepaste analytics query's, zodat ze complexe en nog veel meer aanpasbaar. In tegenstelling tot filters, kunt u cohorten opslaan zodat andere leden van uw team opnieuw kunnen gebruiken.

U kunt een cohort van gebruikers die hebben geprobeerd alle een nieuwe functie in uw app definiëren. U kunt deze cohort opslaan in uw Application Insights-resource. Het is gemakkelijk in de toekomst deze opgeslagen reeks specifieke gebruikers analyseren.

> [!NOTE]
> Nadat ze zijn gemaakt, zijn cohorten beschikbaar in de hulpprogramma's voor gebruikers, sessies, gebeurtenissen en loopt van de gebruiker.

## <a name="example-engaged-users"></a>Voorbeeld: Betrokken gebruikers

Uw team definieert een betrokken gebruiker als iedereen die uw app vijf of meer keer in een bepaalde maand gebruikt. In deze sectie maakt definiëren u een cohort van deze betrokken gebruikers.

1. Open het hulpprogramma cohorten.

2. Selecteer de **sjablonengalerie** tabblad. Voor verschillende cohorten ziet u een verzameling van sjablonen.

3. Selecteer **betrokken gebruikers--dagen gebruikt**.

    Er zijn drie parameters op voor deze cohort:
    * **Activiteiten**, waarin u kiezen welke gebeurtenissen en paginaweergaven tellen als 'gebruik'.
    * **Periode**, de definitie van een maand.
    * **UsedAtleastCustom**, het aantal vaak gebruikers moeten gebruikmaken van iets binnen een periode te tellen bij die betrokken zijn.

4. Wijziging **UsedAtleastCustom** naar **5 + dagen**, en laat **periode** op de standaardwaarde van 28 dagen.

    ![Betrokken gebruikers](.\media\app-insights-usage-cohorts\003.png)

    Nu deze cohort alle gebruikers-id's verzonden met een aangepaste weergave voor de gebeurtenis of de pagina op 5 vertegenwoordigt dagen in het verleden scheiden 28.

5. Selecteer **Opslaan**.

   > [!TIP]
   >  Geef uw cohort een naam, zoals ' gebruikers Engaged (5 + dagen). ' Sla het op 'Mijn rapporten' of 'Gedeeld rapporten,', afhankelijk van of u wilt dat anderen die toegang tot deze Application Insights-resource hebben voor een overzicht van deze cohort.

6. Selecteer **terug naar de galerie**.

### <a name="what-can-you-do-by-using-this-cohort"></a>Wat kunt u doen met behulp van deze cohort?

Open het hulpprogramma voor gebruikers. In de **weergeven** vervolgkeuzelijst Kies de cohort die u hebt gemaakt onder **gebruikers met**.

Het hulpprogramma voor gebruikers is nu gefilterd op deze cohort gebruikers:

![Deelvenster gebruikers gefilterd op een bepaalde cohort](.\media\app-insights-usage-cohorts\004.png)

Een aantal belangrijke zaken opvalt:
* U kunt geen deze set maken door normale filters. De datum-logica wordt meer geavanceerde.
* U kunt deze cohort verder filteren met behulp van de normale filters in het hulpprogramma voor gebruikers. Hoewel de cohort is gedefinieerd op 28 dagen windows, kunt u dus nog steeds het tijdsbereik in het hulpprogramma gebruikers 30 en 60 en 90 dagen aanpassen.

Deze filters ondersteuning voor meer geavanceerde vragen die zijn onmogelijk om uit te drukken via de opbouwfunctie voor query's. Een voorbeeld is _mensen die zijn ingeschakeld in de afgelopen 28 dagen. Het gedrag van deze dezelfde personen gedurende de afgelopen 60 dagen?_

## <a name="example-events-cohort"></a>Voorbeeld: Gebeurtenissen cohort

Ook kunt u cohorten van gebeurtenissen. In deze sectie maakt definiëren u een cohort van de gebeurtenissen en paginaweergaven. Vervolgens ziet u hoe u deze uit de andere hulpprogramma's gebruikt. Deze cohort kunt definiëren een reeks gebeurtenissen die uw team acht _active gebruik_ of een set die betrekking hebben op een bepaalde nieuwe functie.

1. Open het hulpprogramma cohorten.

2. Selecteer de **sjablonengalerie** tabblad. Voor verschillende cohorten ziet u een verzameling van sjablonen.

3. Selecteer **gebeurtenissen objectkiezer**.

    ![Schermopname van gebeurtenissen kiezen](.\media\app-insights-usage-cohorts\006.png)

4. In de **activiteiten** vervolgkeuzelijst Selecteer de gebeurtenissen die u wilt de cohort liggen.

5. Sla de cohort en een naam geven.

## <a name="example-active-users-where-you-modify-a-query"></a>Voorbeeld: Actieve gebruikers waarin u een query wijzigen

De vorige twee cohorten zijn gedefinieerd met behulp van de vervolgkeuzelijsten. Maar u kunt ook cohorten definiëren met behulp van analysequery's voor totale flexibiliteit. Om te zien hoe een cohort van gebruikers te maken van het Verenigd Koninkrijk.

![Animatie lopen door gebruik te maken van cohorten hulpprogramma](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Open het hulpprogramma cohorten, selecteer de **sjablonengalerie** tabblad en selecteer **leeg gebruikers cohort**.

    ![Lege gebruikers cohort](.\media\app-insights-usage-cohorts\001.png)

    Er zijn drie secties:
    * Een Markdown tekstsectie, waar u de cohort in meer detail voor anderen in uw team beschrijven.

    * Een sectie parameters, waar u uw eigen parameters, zoals aanbrengen **activiteiten** en andere vervolgkeuzelijsten van de vorige twee voorbeelden.

    * Een querysectie waarin u de cohort definiëren met behulp van een query analytics.

    In de querysectie u [analytics-query schrijven](https://docs.loganalytics.io/index). De query selecteert de set rijen met een beschrijving van de cohort die u wilt definiëren. Het hulpprogramma cohorten impliciet worden toegevoegd een "| de component user_Id samenvatten' aan de query. Deze gegevens is een voorbeeld hieronder de query in een tabel zodat u ervoor kunt zorgen dat uw query retourneert resultaten.

    > [!NOTE]
    > Als u de query, wijzig het formaat van de sectie om het groter te maken en weergeven van de query niet ziet. De animatie .gif aan het begin van dit gedeelte ziet u het formaat gedrag.

2. Kopieer en plak de volgende tekst in de query-editor:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Selecteer **Query uitvoert**. Als er geen gebruikers-id's in de tabel worden weergegeven, gewijzigd in een land waar de toepassing heeft voor gebruikers.

4. Opslaan en de naam van de cohort.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

_Ik heb een cohort van gebruikers van een bepaalde land gedefinieerd. Wanneer ik deze cohort in het hulpprogramma gebruikers vergelijkt aan het zojuist een filter instellen op land, zie ik andere resultaten leiden. Waarom is dat?_

Cohorten en filters zijn verschillend. Stel dat u een cohort van gebruikers uit het Verenigd Koninkrijk hebt (gedefinieerd als in het vorige voorbeeld) en u de resultaten voor het instellen van het filter vergelijken ' land of regio = Verenigd Koninkrijk. "

* De cohort-versie worden alle gebeurtenissen weergegeven van gebruikers die een of meer gebeurtenissen uit het Verenigd Koninkrijk in de huidige tijdsbereik verzonden. Als u per land of regio splitst, ziet u waarschijnlijk veel landen en regio's.
* De versie van de filters worden alleen gebeurtenissen uit het Verenigd Koninkrijk weergegeven. Maar als u per land of regio splitst, ziet u alleen het Verenigd Koninkrijk.

## <a name="learn-more"></a>Meer informatie
- [Analytics-querytaal](https://go.microsoft.com/fwlink/?linkid=856587)
- [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
- [Gebruiker stromen](app-insights-usage-flows.md)
- [Overzicht gebruik](app-insights-usage-overview.md)