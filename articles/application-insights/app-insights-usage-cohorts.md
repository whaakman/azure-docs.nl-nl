---
title: Azure Application Insights gebruik cohorten | Microsoft docs
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
ms.openlocfilehash: 68453499cc6477cc079a342906614e6873938fc8
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="application-insights-cohorts"></a>Application Insights cohorten

Een cohort is een set van gebruikers, sessies, gebeurtenissen of bewerkingen die iets gemeen hebben. In Azure Application Insights worden cohorten gedefinieerd door een query Analytics. Als u merkt dat een specifieke set gebruikers analyseren of gebeurtenissen herhaaldelijk, cohorten kunnen bieden u nog meer flexibiliteit om uit te drukken exact de set die u geïnteresseerd bent in.

![Deelvenster cohorten](.\media\app-insights-usage-cohorts\001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorten versus basic filters

Hoewel cohorten worden gebruikt op vergelijkbare wijze als filters, wordt het feit dat een cohort-definitie wordt samengesteld uit aangepaste Analytics-query's kan ze worden veel meer flexibele en complexe. In tegenstelling tot filters, kunt u cohorten opslaan zodat andere leden van uw team opnieuw kunnen gebruiken.

U kunt een cohort van gebruikers waarop alle is geprobeerd een nieuwe functie in uw app definiëren. Met deze cohort opgeslagen in uw Application Insights-resource, wordt het analyseren van deze specifieke groep gebruikers in de toekomst met één klik.

> [!NOTE]
> Zodra gemaakt, zijn cohorten beschikbaar in de hulpprogramma's voor gebruikers, sessies, gebeurtenissen en loopt van de gebruiker.

## <a name="example-engaged-users"></a>Voorbeeld: betrokken gebruikers

Uw team definieert een betrokken gebruiker als iedereen die uw app vijf of meer keer in een bepaalde maand gebruikt. We gaan een cohort van deze betrokken gebruikers definiëren.

1. Open de **cohorten** hulpprogramma.

2. Klik op **sjablonengalerie** tabblad. Hier vindt u een verzameling sjablonen voor verschillende cohorten.

3. Kies **betrokken gebruikers** – dagen gebruikt '.

    Er zijn drie parameters op voor deze cohort:
      * **Activiteiten** die kunt u kiezen welke gebeurtenissen en paginaweergaven moeten worden geteld als 'gebruik'.
      * **Periode** de definitie van een maand.
      * **UsedAtleastCustom** het aantal keren dat ze gebruikmaken van iets binnen een periode moeten te tellen als een betrokken gebruiker.

4. Wijziging **UsedAtleastCustom** '5 + days' en laat de eigenschap **periode** op de standaardwaarde van 28 dagen.

    ![Installatiekopie](.\media\app-insights-usage-cohorts\003.png)

    Deze cohort vertegenwoordigt nu alle gebruikers-id's die zijn verzonden met een aangepaste weergave voor de gebeurtenis of de pagina op vijf verschillende dagen in de afgelopen 28 dagen.

5. Klik op **Opslaan**.

   > [!TIP]
   >  Geef uw cohort een naam, zoals ' gebruikers Engaged (5 + dagen) ' en sla deze op 'Mijn rapporten' of 'Gedeeld rapporten' afhankelijk van als u wilt dat andere personen met toegang tot deze toepassing Insights-resource deze cohort.

6. Klik op **terug naar de galerie**.

### <a name="what-can-you-do-with-this-cohort"></a>Wat kunt u doen met deze cohort?

Open de **gebruikers** hulpprogramma > In de **weergeven** dropdown > Kies de cohort die u hebt gemaakt onder **gebruikers met...**

Het hulpprogramma voor gebruikers is nu gefilterd op deze cohort gebruikers:

![Deelvenster gebruikers gefilterd op een bepaalde cohort](.\media\app-insights-usage-cohorts\004.png)

Een aantal belangrijke zaken opvalt:
   * Dit is een set die u kan niet door normale filters hebt gemaakt. De datum-logica wordt meer geavanceerde.
   * U kunt deze cohort met behulp van de normale filters in het hulpprogramma gebruikers verder filteren. Terwijl de cohort is gedefinieerd op 28 dagen windows, kunt u dus nog steeds het tijdsbereik in het hulpprogramma gebruikers 30 en 60 en 90 dagen aanpassen. 

Hiermee kunt u meer geavanceerde vragen zoals: _voor gebruikers die zijn ingeschakeld in de afgelopen 28 dagen, deze dezelfde personen werking gedurende de afgelopen 60 dagen?_ die anders zou worden onmogelijk om uit te drukken via de opbouwfunctie voor query's.

## <a name="example-events-cohort"></a>Voorbeeld: gebeurtenissen cohort

Ook kunt u cohorten van gebeurtenissen. Laten we een cohort van de gebeurtenissen en paginaweergaven definiëren en vervolgens informatie over het gebruiken vanuit de andere hulpprogramma's. Dit kan handig zijn voor het definiëren van een reeks gebeurtenissen die uw team acht zijn _active gebruik_, of voor het definiëren van een reeks gebeurtenissen met betrekking tot een bepaalde nieuwe functie.

1. Open de **cohorten** hulpprogramma.

2. Klik op de **sjablonengalerie** tabblad. Hier vindt u een verzameling sjablonen voor verschillende cohorten.

3. Kies **gebeurtenissen objectkiezer**.

    ![Schermopname van gebeurtenissen kiezen](.\media\app-insights-usage-cohorts\006.png)

4. In de **activiteiten** vervolgkeuzelijst, selecteer de gebeurtenissen die u wilt de cohort liggen

5. Sla de cohort en een naam geven.

## <a name="example-active-users-where-you-modify-query"></a>Voorbeeld: actieve gebruikers waar u de query wijzigen

De vorige twee cohorten zijn gedefinieerd met behulp van de vervolgkeuzelijsten. Maar we kunnen ook cohorten met Analytics query's voor totale flexibiliteit definiëren. Laten we zien hoe u door het maken van een cohort van gebruikers uit het Verenigd Koninkrijk.

![Animatie lopen door gebruik te maken van cohorten hulpprogramma](.\media\app-insights-usage-cohorts\cohorts0001.gif)

1. Open de **cohorten** hulpprogramma > klikt u op **sjablonengalerie** tabblad > Kies **leeg gebruikers cohort**.

    ![Lege gebruikers Cohort](.\media\app-insights-usage-cohorts\001.png)

    Er zijn drie secties:
       * Een Markdown-tekstsectie waar u de cohort in meer detail voor anderen in uw team beschrijven kunt.

       * Een sectie parameters die u gebruiken kunt voor het maken van uw eigen parameters, zoals de **activiteiten** en andere vervolgkeuzelijsten van de vorige twee voorbeelden.

       * Het gedeelte van een query die u gebruikt voor het definiëren van de cohort door een query Analytics.

    In de querysectie u [Analytics-query schrijven](https://docs.loganalytics.io/index) die de set rijen met een beschrijving van de cohort die u wilt definiëren selecteert. Het hulpprogramma cohorten impliciet worden toegevoegd een "| de component user_Id samenvatten' aan de query. Dit is een voorbeeld hieronder de query in een tabel zodat u ervoor kunt zorgen dat uw query retourneert resultaten.

    > [!NOTE]
    > Als u de query, wijzig het formaat van de sectie om het groter te maken en weergeven van de query niet ziet. De animatie .gif aan het begin van dit gedeelte ziet u het formaat gedrag.

2. Het volgende in de query-editor kopiëren en plakken:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Klik op **Query uitvoert**. U ziet de gebruikers-id's in de tabel worden weergegeven. Als dat niet het geval is, wijzigt u in een land waar de toepassing heeft voor gebruikers.

4. Opslaan en de naam van de cohort.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

_Ik heb een cohort van gebruikers van een bepaalde land gedefinieerd. Wanneer ik deze cohort in het hulpprogramma gebruikers vergelijkt aan het zojuist een filter instellen op land in het hulpprogramma voor gebruikers, zie ik andere resultaten leiden. Waarom is dat?_

Cohorten en filters zijn verschillend. Stel dat u een cohort van gebruikers uit het Verenigd Koninkrijk hebt (gedefinieerd als in het bovenstaande voorbeeld) en u de resultaten voor het instellen van het filter vergelijken ' land of regio = Verenigd Koninkrijk. "

* De cohort-versie wordt alle gebeurtenissen weergegeven van gebruikers die u ten minste één gebeurtenis uit het Verenigd Koninkrijk in de huidige tijdsbereik verzonden hebt. Als u per land of regio splitst, waarschijnlijk ziet u veel landen en regio's.
* De versie van de filters wordt alleen gebeurtenissen uit het Verenigd Koninkrijk weergegeven. Dat als u per land of regio splitst, u alleen het Verenigd Koninkrijk ziet.

## <a name="learn-more"></a>Meer informatie
- [Analytics-querytaal](https://go.microsoft.com/fwlink/?linkid=856587)
- [Gebruikers, sessies, gebeurtenissen](app-insights-usage-segmentation.md)
- [Gebruikersstromen](app-insights-usage-flows.md)
- [Overzicht gebruik](app-insights-usage-overview.md)