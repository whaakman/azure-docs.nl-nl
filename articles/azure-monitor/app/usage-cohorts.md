---
title: Azure Application Insights gebruik cohorten | Microsoft Docs
description: Verschillende groepen of gebruikers, sessies, gebeurtenissen of bewerkingen die iets hebben analyseren
services: application-insights
documentationcenter: ''
author: NumberByColors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 04/10/2018
ms.reviewer: mbullwin
ms.pm_owner: daviste;NumberByColors
ms.author: daviste
ms.openlocfilehash: a575161be0a339973f9c59cc13c2320b38939d04
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58108522"
---
# <a name="application-insights-cohorts"></a>Application Insights cohorten

Een cohort is een set van gebruikers, sessies, gebeurtenissen of bewerkingen die iets met elkaar gemeen hebben. In Azure Application Insights worden cohorten gedefinieerd door een analytics-query. In gevallen waar u hebt voor het analyseren van een specifieke set van gebruikers of gebeurtenissen optreden, cohorten kunnen biedt u meer flexibiliteit om exact de set die u geïnteresseerd bent in.

![Deelvenster cohorten](./media/usage-cohorts/001.png)

## <a name="cohorts-versus-basic-filters"></a>Cohorten versus basisfilters

Cohorten worden gebruikt op een manier die vergelijkbaar is met filters. Maar de cohorten definities zijn opgebouwd uit aangepaste analytics-query's, zodat ze complexe en nog veel meer aanpasbaar. In tegenstelling tot filters, kunt u cohorten opslaan zodat andere leden van uw team opnieuw kunnen worden gebruikt.

U kunt een cohort van gebruikers die u hebt al geprobeerd een nieuwe functie in uw app definiëren. U kunt deze cohort opslaan in uw Application Insights-resource. Het is eenvoudig voor het analyseren van deze groep van specifieke gebruikers is opgeslagen in de toekomst.

> [!NOTE]
> Nadat ze zijn gemaakt, zijn cohorten beschikbaar in de hulpprogramma's voor gebruikers, sessies, gebeurtenissen en Gebruikersstromen.

## <a name="example-engaged-users"></a>Voorbeeld: Betrokken gebruikers

Uw team definieert een betrokken gebruiker als iedereen die gebruikmaakt van uw app in vijf of meer keer in een maand. In deze sectie maakt definiëren u een cohort van deze betrokken gebruikers.

1. Open het hulpprogramma voor cohorten.

2. Selecteer de **sjabloongalerie** tabblad. Ziet u een verzameling van sjablonen voor verschillende cohorten.

3. Selecteer **betrokken gebruikers--op dagen gebruikt**.

    Er zijn drie parameters voor deze cohort:
    * **Activiteiten**, waarin u kiezen welke gebeurtenissen en paginaweergaven tellen als "gebruik."
    * **Periode**, de definitie van een maand.
    * **UsedAtLeastCustom**, het aantal keren gebruikers moeten iets gebruiken binnen een periode moeten worden geteld als betrokken.

4. Wijziging **UsedAtLeastCustom** naar **5 + dagen**, en laat **periode** op de standaardwaarde van 28 dagen.

    ![Betrokken gebruikers](./media/usage-cohorts/003.png)

    Nu deze cohort alle gebruikers-id's verzonden met een aangepaste gebeurtenis of pagina weergave op 5 vertegenwoordigt scheiden dagen in de afgelopen 28.

5. Selecteer **Opslaan**.

   > [!TIP]
   > Geef uw cohort een naam, zoals ' Engaged gebruikers (5 + dagen). " Sla het op 'Mijn rapporten' of 'gedeelde rapporten,", afhankelijk van of u wilt dat anderen die toegang hebben tot deze Application Insights-resource om te zien deze cohort.

6. Selecteer **terug naar de galerie**.

### <a name="what-can-you-do-by-using-this-cohort"></a>Wat kunt u doen met behulp van deze cohort?

Open het hulpprogramma voor gebruikers. In de **weergeven** vervolgkeuzelijst kiest u het cohort dat u hebt gemaakt onder **gebruikers die deel uitmaken van**.

Het hulpprogramma gebruikers wordt nu gefilterd op deze cohort van gebruikers:

![Gebruikers deelvenster gefilterd op een bepaalde cohort](./media/usage-cohorts/004.png)

Enkele belangrijke dingen die opvallen:

* U kunt geen deze set maken via normale filters. De datum-logica wordt meer geavanceerde.
* U kunt deze cohort verder filteren met behulp van de normale filters in het hulpprogramma gebruikers. Hoewel het cohort is gedefinieerd in 28 dagen windows, kunt u dus nog steeds het tijdsbereik in het hulpprogramma gebruikers worden 30 en 60 en 90 dagen aanpassen.

Deze filters bieden ondersteuning voor meer geavanceerde vragen die onmogelijk is om uit te drukken via de opbouwfunctie voor query's. Een voorbeeld is _mensen die in de afgelopen 28 dagen betrokken zijn. Het gedrag van deze dezelfde personen gedurende de afgelopen 60 dagen?_

## <a name="example-events-cohort"></a>Voorbeeld: Gebeurtenissencohort

Ook kunt u cohorten van gebeurtenissen. In deze sectie definieert u een cohort van de gebeurtenissen en paginaweergaven. Vervolgens ziet u hoe u ze op de andere hulpprogramma's gebruiken. Deze cohort kan een reeks gebeurtenissen die uw team acht definiëren _active gebruik_ of een set met betrekking tot een bepaalde functie.

1. Open het hulpprogramma voor cohorten.

2. Selecteer de **sjabloongalerie** tabblad. Voor verschillende cohorten ziet u een verzameling van sjablonen.

3. Selecteer **gebeurtenissen kiezen**.

    ![Schermafbeelding van gebeurtenissen kiezen](./media/usage-cohorts/006.png)

4. In de **activiteiten** vervolgkeuzelijst vak, selecteert u de gebeurtenissen die u wilt worden in de cohort.

5. Sla het cohort en geef deze een naam.

## <a name="example-active-users-where-you-modify-a-query"></a>Voorbeeld: Actieve gebruikers waar u een query wijzigen

De vorige twee cohorten zijn gedefinieerd met behulp van de vervolgkeuzelijsten. Maar u kunt ook cohorten definiëren met behulp van analytics-query's voor totale flexibiliteit. Om te zien hoe een cohort van gebruikers te maken van het Verenigd Koninkrijk.

![Gebruik van het programma cohorten hek animatie](./media/usage-cohorts/cohorts0001.gif)

1. Open het hulpprogramma cohorten, selecteer de **sjabloongalerie** tabblad, en selecteer **leeg gebruikerscohort**.

    ![Leeg gebruikerscohort](./media/usage-cohorts/001.png)

    Er zijn drie secties:
   * Een Markdown tekstsectie, waar u het cohort in meer detail voor anderen in uw team beschreven.

   * Een gedeelte van de parameters, waar u uw eigen parameters, zoals maken **activiteiten** en andere vervolgkeuzelijsten uit de vorige twee voorbeelden.

   * Een querysectie, waar u het cohort definiëren met behulp van een analytics-query.

     In de querysectie u [een analytics-query schrijven](/azure/kusto/query). De query selecteert het aantal rijen met een beschrijving van het cohort dat u wilt definiëren. Het programma cohorten vervolgens impliciet wordt toegevoegd een "| samenvatten op user_Id'-component in de query. Deze gegevens is een voorbeeld hieronder de query in een tabel, zodat u kunt ervoor zorgen dat uw query resultaten worden geretourneerd.

     > [!NOTE]
     > Als u de query niet ziet, probeert u het formaat van de sectie als u wilt deze groter en weergeven van de query. De GIF-animaties kunnen aan het begin van deze sectie ziet u het formaat gedrag.

2. Kopieer en plak de volgende tekst in de query-editor:

    ```KQL
    union customEvents, pageViews
    | where client_CountryOrRegion == "United Kingdom"
    ```

3. Selecteer **Query uitvoeren**. Als er geen gebruikers-id's worden weergegeven in de tabel, overstappen naar een land/regio waar uw toepassing heeft voor gebruikers.

4. Opslaan en de naam van het cohort.

## <a name="frequently-asked-questions"></a>Veelgestelde vragen

_Ik heb een cohort van gebruikers uit een bepaald land gedefinieerd. Als ik deze cohort in het hulpprogramma gebruikers op het zojuist instellen van een filter op dat land, zie ik verschillende resultaten. Waarom is dat?_

Cohorten en filters zijn verschillend. Stel dat u een cohort van gebruikers uit het Verenigd Koninkrijk hebt (gedefinieerd als in het vorige voorbeeld) en vergelijken van de resultaten met het instellen van het filter "land of regio = Verenigd Koninkrijk."

* De versie van de cohort toont alle gebeurtenissen van gebruikers die een of meer gebeurtenissen uit het Verenigd Koninkrijk in het huidige tijdsbereik verzonden. Als u op land of regio splitsen, ziet u waarschijnlijk veel landen en regio's.
* De versie van de filters worden alleen gebeurtenissen uit het Verenigd Koninkrijk weergegeven. Maar als u op land of regio splitsen, ziet u alleen het Verenigd Koninkrijk.

## <a name="learn-more"></a>Meer informatie

* [Querytaal voor analytische gegevens](https://go.microsoft.com/fwlink/?linkid=856587)
* [Gebruikers, sessies, gebeurtenissen](usage-segmentation.md)
* [Gebruikersstromen](usage-flows.md)
* [Gebruiksoverzicht](usage-overview.md)