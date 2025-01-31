---
title: Uw klanten begrijpen in Azure Application Insights | Microsoft Docs
description: Zelfstudie over het gebruiken van Application Insights om inzicht te krijgen in de wijze waarop klanten van uw app gebruikmaken.
keywords: ''
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 90357c13003820a25ee52aab5cda83029c9d85ff
ms.sourcegitcommit: 6ea7f0a6e9add35547c77eef26f34d2504796565
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/14/2019
ms.locfileid: "65601551"
---
# <a name="use-azure-application-insights-to-understand-how-customers-are-using-your-application"></a>Gebruik Application Insights om inzicht te krijgen in de wijze waarop klanten van uw app gebruikmaken

Azure Application Insights verzamelt gebruiksgegevens om u meer inzicht te geven in hoe uw gebruikers omgaan met uw toepassing.  Dit zelfstudie leidt u door de verschillende resources die beschikbaar zijn om deze informatie te analyseren.  U leert het volgende:

> [!div class="checklist"]
> * Details van gebruikers die toegang hebben tot uw toepassing analyseren
> * Sessie-informatie gebruiken om te analyseren hoe klanten uw toepassing gebruiken
> * Trechters definiëren waarmee u uw gewenste gebruikersactiviteit kunt vergelijken met hun werkelijke activiteit 
> * Een werkmap maken om visualisaties en query's in één document samen te voegen
> * Vergelijkbare gebruikers groeperen om ze gezamenlijk te analyseren
> * Ontdekken welke gebruikers terugkomen naar uw toepassing
> * Zien hoe gebruikers navigeren in uw toepassing


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2019](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
- Download en installeer de [Visual Studio Snapshot Debugger](https://aka.ms/snapshotdebugger).
- Implementeer een .NET-toepassing in Azure en [schakel de Application Insights-SDK](../../azure-monitor/app/asp-net.md)in. 
- [Telemetrie van uw toepassing versturen](../../azure-monitor/app/usage-overview.md#send-telemetry-from-your-app) voor het toevoegen van aangepaste gebeurtenissen/paginaweergaven
- [Gebruikerscontext](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context) verzenden om bij te houden wat een gebruiker in de loop der tijd doet en volledig gebruikmaken van de gebruiksfuncties.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="get-information-about-your-users"></a>Informatie over uw gebruikers verzamelen
In het deelvenster **Gebruikers** kunt u op verschillende manieren inzicht krijgen in belangrijke informatie over uw gebruikers. U kunt dit deelvenster gebruiken om informatie te begrijpen over waarvandaan uw gebruikers verbinding maken, details van hun client en welke delen van uw toepassing ze bezoeken. 

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.
2. Selecteer **Gebruikers** in het menu.
3. De standaardweergave toont het aantal unieke gebruikers die de afgelopen 24 uur verbinding hebben gemaakt met uw toepassing.  U kunt het tijdvenster wijzigen en verschillende andere criteria voor het filteren van deze informatie instellen.

    ![Opbouwfunctie voor query's](media/tutorial-users/QueryBuilder.png)

6. Klik op de vervolgkeuzelijst **Tijdens** en wijzig het tijdvenster in zeven dagen.  Hierdoor worden meer gegevens opgenomen in de verschillende grafieken in het deelvenster.

    ![Tijdsbereik wijzigen](media/tutorial-users/TimeRange.png)

4. Klik op de vervolgkeuzelijst **Splitsen op basis van** om een uitsplitsing op basis van een gebruikerseigenschap aan de grafiek toe te voegen.  Selecteer **Land of regio**.  De grafiek bevat de dezelfde gegevens maar kunt u om een uitsplitsing van het aantal gebruikers voor elk land/regio weer te geven.

    ![Land of regio-grafiek](media/tutorial-users/CountryorRegion.png)

5. Plaats de cursor over verschillende balken in de grafiek en houd er rekening mee dat het aantal voor elk land/regio alleen het tijdvenster wordt vertegenwoordigd door die voor de balk weergeeft.
6. Bekijk de kolom **Insights** aan de rechterkant voor analyse van uw gebruikersgegevens.  Dit levert informatie op zoals het aantal unieke sessies gedurende de periode en records met gemeenschappelijke eigenschappen die significant zijn voor de gebruikersgegevens 

    ![Insights-kolom](media/tutorial-users/insights.png)


## <a name="analyze-user-sessions"></a>Gebruikerssessies analyseren
Het deelvenster **Sessies** lijkt op het deelvenster **Gebruikers**.  Terwijl **Gebruikers** u helpt meer te weten te komen over de gebruikers van uw toepassing, helpt **Sessies** u te begrijpen hoe die gebruikers uw toepassing hebben gebruikt.  

1. Selecteer **Sessies** in het menu.
2. Bekijk de grafiek en merk op dat u dezelfde opties hebt voor filteren en uitsplitsen van de gegevens als in het deelvenster **Gebruikers**.

    ![Opbouwfunctie voor sessiequery's](media/tutorial-users/SessionsBuilder.png)

3. Het deelvenster **Voorbeeld van deze sessies** deelvenster aan de rechterkant bevat een lijst met sessies die een groot aantal gebeurtenissen bevatten.  Dit zijn interessante sessies om te analyseren.

    ![Voorbeeld van deze sessies](media/tutorial-users/SessionsSample.png)

4. Klik op een van de sessies om de **Sessietijdlijn** te bekijken, die elke actie in de sessies laat zien.  Hiermee kunt u informatie identificeren zoals sessies met een groot aantal uitzonderingen.

    ![Sessietijdlijn](media/tutorial-users/SessionsTimeline.png)

## <a name="group-together-similar-users"></a>Vergelijkbare gebruikers groeperen
Een **Cohort** is een set gebruikers die worden gegroepeerd met vergelijkbare kenmerken.  U kunt cohorten gebruiken om gegevens in andere deelvensters te filteren zodat u bepaalde groepen gebruikers kunt analyseren.  U wilt bijvoorbeeld alleen gebruikers analyseren die een aankoop hebben voltooid.

1.  Selecteer **Cohorten** in het menu.
2.  Klik op **Nieuw** om een nieuw cohort te maken.
3.  Selecteer de vervolgkeuzelijst **Die hebben gebruikt** en selecteer een actie.  Alleen gebruikers die deze actie binnen het tijdvenster van het rapport hebben uitgevoerd, worden opgenomen.

    ![Cohort dat opgegeven acties heeft uitgevoerd](media/tutorial-users/CohortsDropdown.png)

4.  Selecteer **Gebruikers** in het menu.
5.  Selecteer in de vervolgkeuzelijst **Weergeven** het cohort dat u zojuist hebt gemaakt.  De gegevens voor de grafiek worden beperkt tot die gebruikers.

    ![Cohort in hulpprogramma gebruikers](media/tutorial-users/UsersCohort.png)


## <a name="compare-desired-activity-to-reality"></a>Gewenste activiteit vergelijken met de werkelijkheid
Terwijl de vorige deelvensters gericht zijn op wat gebruikers van uw toepassing hebben gedaan, richten **Trechters** zich op wat u gebruikers wilt laten doen.  Een trechter vertegenwoordigt een reeks stappen in uw toepassing en het percentage gebruikers dat zich tussen fasen verplaatst.  U kunt bijvoorbeeld een trechter maken die het percentage meet van de gebruikers die verbinding maken met uw toepassing die een product zoeken.  Vervolgens kunt u het percentage gebruikers zien dat dat product aan een winkelwagen toevoegt, en vervolgens het percentage van degenen die een aankoop voltooien.

1. Selecteer **Trechters** in het menu en klik op **Nieuw**. 

    ![](media/tutorial-users/funnelsnew.png)

2. Typ een **Trechternaam**.
3. Maak een trechter met ten minste twee stappen door voor elke stap een actie te selecteren.  De lijst met acties is opgebouwd op basis van door Application Insights verzamelde gebruiksgegevens.

    ![](media/tutorial-users/funnelsedit.png)

4. Klik op **Opslaan** om de trechter op te slaan en de resultaten te bekijken.  Het venster rechts van de trechter toont de meest voorkomende gebeurtenissen vóór de eerste activiteit en na de laatste activiteit om u te helpen inzicht te krijgen in de tendensen van gebruikers rond de specifieke reeks.

    ![](media/tutorial-users/funnelsright.png)


## <a name="learn-which-customers-return"></a>Ontdek welke klanten terugkeren
**Retentie** helpt u te begrijpen welke gebruikers terugkomen naar uw toepassing.  

1. Selecteer **Retentie** in het menu.
2. Standaard bevat de geanalyseerde informatie gebruikers die een actie hebben uitgevoerd en vervolgens zijn teruggekeerd om een ​​actie uit te voeren.  U kunt dit filter wijzigen om bijvoorbeeld alleen die gebruikers op te nemen die zijn teruggekeerd na het voltooien van een aankoop.

    ![](media/tutorial-users/retentionquery.png)

3. De terugkerende gebruikers die aan de criteria voldoen, worden in grafische en tabelvorm weergegeven voor verschillende tijdsduren.  Het gebruikelijke patroon is een geleidelijke afname van terugkerende gebruikers in de loop van de tijd.  Een plotselinge daling van de ene periode naar de andere kan reden tot zorg zijn. 

    ![](media/tutorial-users/retentiongraph.png)

## <a name="analyze-user-navigation"></a>Gebruikersnavigatie analyseren
Een **Gebruikersstroom** visualiseert hoe gebruikers navigeren tussen de pagina's en functies van uw toepassing.  Dit helpt u vragen te beantwoorden zoals waar gebruikers doorgaans naartoe gaan vanaf een bepaalde pagina, hoe ze uw toepassing doorgaans verlaten en of er acties zijn die regelmatig worden herhaald.

1.  Selecteer **Gebruikersstromen** in het menu.
2.  Klik op **Nieuw** om een ​​nieuwe gebruikersstroom te maken en klik vervolgens op **Bewerken** om de details te bewerken.
3.  Verhoog het **Tijdsbereik**  tot 7 dagen en selecteer vervolgens een eerste gebeurtenis.  De stroom volgt gebruikerssessies die beginnen met die gebeurtenis.

    ![](media/tutorial-users/flowsedit.png)

4.  De gebruikersstroom wordt weergegeven en u kunt de verschillende gebruikerspaden en hun sessieaantallen bekijken.  Blauwe lijnen geven een actie aan die de gebruiker na de huidige actie heeft uitgevoerd.  Een rode lijn geeft het einde van de gebruikerssessie aan.

    ![](media/tutorial-users/flows.png)

5.  Als u een gebeurtenis uit de stroom wilt verwijderen, klikt u op **x** in de hoek van de actie en vervolgens op **Grafiek maken**.  De grafiek wordt opnieuw getekend met alle exemplaren van die gebeurtenis verwijderd.  Klik op **Bewerken** om te zien dat de gebeurtenis nu is toegevoegd aan de **Uitgesloten gebeurtenissen**.

    ![](media/tutorial-users/flowsexclude.png)

## <a name="consolidate-usage-data"></a>Gebruiksgegevens consolideren
**Werkmappen** combineren gegevensvisualisaties, Analytics-query's en tekst in interactieve documenten.  U kunt werkmappen gebruiken om algemene gebruiksinformatie te groeperen, informatie uit een bepaald incident te consolideren of aan uw team terug te rapporteren over het gebruik van uw toepassing.

1.  Selecteer **Werkmappen** in het menu.
2.  Klik op **Nieuw** om een nieuwe werkmap te maken.
3.  De werkmap bevat al een query waarin alle gebruiksgegevens op de laatste dag worden weergegeven als een staafdiagram.  U kunt deze query gebruiken, deze handmatig bewerken of op **Voorbeeldquery's** klikken om uit andere nuttige query's te selecteren.

    ![](media/tutorial-users/samplequeries.png)

4.  Klik op **Klaar met bewerken**.
5.  Klik op **Bewerken** in het bovenste deelvenster om de tekst bovenaan de werkmap te bewerken.  Deze wordt opgemaakt met markdown.

    ![](media/tutorial-users/markdown.png)

6.  Klik op **Gebruikers toevoegen** om een grafiek met gebruikersgegevens toe te voegen.  Bewerk desgewenst de details van de grafiek en klik vervolgens op **Klaar met bewerken** om deze op te slaan.


## <a name="next-steps"></a>Volgende stappen
Nu u hebt geleerd hoe u uw gebruikers kunt analyseren, gaat u door naar de volgende zelfstudie om te leren hoe u aangepaste dashboards kunt maken die deze informatie combineren met andere nuttige gegevens over uw toepassing.

> [!div class="nextstepaction"]
> [Aangepaste dashboards maken](../../azure-monitor/learn/tutorial-app-dashboards.md)
