---
title: Inzicht in uw klanten in Azure Application Insights | Microsoft Docs
description: Zelfstudie over het gebruik van Azure Application Insights om te begrijpen hoe klanten uw toepassing gebruiken.
keywords: 
services: application-insights
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/20/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: db61c300ad82270e59d315fa3372d9e4390c7a21
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="use-azure-application-insights-to-understand-how-customers-are-using-your-application"></a>Azure Application Insights gebruiken om te begrijpen hoe klanten uw toepassing gebruiken

Azure Application Insights verzamelt gebruiksgegevens om te begrijpen hoe uw gebruikers werken met uw toepassing.  Deze zelfstudie leert u de verschillende bronnen die beschikbaar zijn voor deze gegevens analyseren.  U leert hoe:

> [!div class="checklist"]
> * Meer informatie over gebruikers die toegang tot uw toepassing analyseren
> * Sessie-informatie voor het analyseren van hoe klanten gebruiken voor uw toepassing gebruiken
> * Definieer schoorstenen waarmee u de gewenste gebruikersactiviteit aan de werkelijke activiteit vergelijken 
> * Maak een werkmap om te consolideren van visualisaties en query's in een document
> * Groep vergelijkbare gebruikers ze samen analyseren
> * Meer informatie over welke gebruikers aan uw toepassing wilt retourneren
> * Hoe gebruikers navigeren door uw toepassing controleren


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
- Download en installeer de [Visual Studio momentopname Debugger](http://aka.ms/snapshotdebugger).
- Een .NET-toepassing in Azure implementeert en [inschakelen van de Application Insights-SDK](app-insights-asp-net.md). 
- [Verzend telemetrie vanuit uw toepassing](app-insights-usage-overview.md#send-telemetry-from-your-app) voor het toevoegen van aangepaste gebeurtenissen/paginaweergaven
- Verzenden [gebruikerscontext](https://docs.microsoft.com/azure/application-insights/app-insights-usage-send-user-context) te houden wat een gebruiker gedurende een bepaalde periode doet en volledig gebruikmaken van de functies voor informatie over het gebruik.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="get-information-about-your-users"></a>Informatie ophalen over uw gebruikers
De **gebruikers** deelvenster kunt u belangrijke informatie over uw gebruikers in tal van manieren begrijpen. U kunt dit deelvenster om te begrijpen gegevens zoals waar uw gebruikers verbinding maakt vanaf de details van de client en welke gebieden van uw toepassing ze openen. 

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.
2. Selecteer **gebruikers** in het menu.
3. De standaardweergave toont het aantal unieke gebruikers die zijn verbonden met uw toepassing in de afgelopen 24 uur.  U kunt het tijdvenster wijzigen en verschillende andere criteria voor het filteren van deze informatie instellen.

    ![Opbouwfunctie voor query 's](media\app-insights-tutorial-users\QueryBuilder.png)

6. Klik op de **tijdens** dropdown en wijzigt u de tijdsduur op 7 dagen.  Dit verhoogt de gegevens die zijn opgenomen in de verschillende grafieken in het deelvenster.

    ![Tijdsbereik wijzigen](media\app-insights-tutorial-users\TimeRange.png)

4. Klik op de **gesplitst** dropdown een uitsplitsing door de gebruikerseigenschap van een toevoegen aan de grafiek.  Selecteer **land of regio**.  De grafiek bevat dezelfde gegevens, maar kunt u een overzicht van het aantal gebruikers per land weergeven.

    ![Land of regio grafiek](media\app-insights-tutorial-users\CountryorRegion.png)

5. Plaats de cursor op verschillende balken in het diagram en houd er rekening mee dat het aantal voor elk land alleen de periode dat wordt vertegenwoordigd door die voor de balk weergeeft.
6. Bekijk de **Insights** kolom rechts die analyse uitvoeren op uw gebruikersgegevens.  Hierdoor krijgt u informatie zoals het aantal unieke sessies gedurende de periode en records met algemene eigenschappen voor die gezamenlijk aanzienlijke van de gebruikersgegevens 

    ![Insights kolom](media\app-insights-tutorial-users\insights.png)


## <a name="analyze-user-sessions"></a>Gebruikerssessies analyseren
De **sessies** Configuratiescherm is vergelijkbaar met de **gebruikers** Configuratiescherm.  Waar **gebruikers** biedt meer inzicht in gegevens over de gebruikers die toegang tot uw toepassing **sessies** helpt u begrijpen hoe deze gebruikers uw toepassing gebruikt.  

1. Selecteer **sessies** in het menu.
2. Bekijk de grafiek en houd er rekening mee dat u dezelfde opties om te filteren en opsplitsen van de gegevens als in de **gebruikers** Configuratiescherm.

    ![Sessies opbouwfunctie voor Query 's](media\app-insights-tutorial-users\SessionsBuilder.png)

3. De **voorbeeld van deze sessies** rechterdeelvenster een lijst met sessies met een groot aantal gebeurtenissen.  Dit zijn interessante sessies te analyseren.

    ![Voorbeeld van deze sessies](media\app-insights-tutorial-users\SessionsSample.png)

4. Klik op een van de sessies om weer te geven de **sessie tijdlijn**, waarin elke actie in de sessies.  Hierdoor kunt u informatie zoals de sessies met een groot aantal uitzonderingen identificeren.

    ![Tijdlijn voor sessies](media\app-insights-tutorial-users\SessionsTimeline.png)

## <a name="group-together-similar-users"></a>Groeperen van vergelijkbare gebruikers
Een **Cohort** is een set gebruikers groupd op vergelijkbare kenmerken.  In andere deelvensters zodat u voor het analyseren van bepaalde groepen gebruikers kunt u cohorten om gegevens te filteren.  U wilt analyseren alleen gebruikers die een aankoop voltooid.

1.  Selecteer **cohorten** in het menu.
2.  Klik op **nieuw** voor het maken van een nieuwe cohort.
3.  Selecteer de **wie gebruikt** vervolgkeuzelijst en selecteer een actie.  Alleen gebruikers met deze actie binnen de periode van het rapport is uitgevoerd, worden opgenomen.

    ![Cohort die opgegeven acties uitgevoerd](media\app-insights-tutorial-users\CohortsDropdown.png)

4.  Selecteer **gebruikers** in het menu.
5.  In de **weergeven** vervolgkeuzelijst, selecteer de cohort die u zojuist hebt gemaakt.  De gegevens voor de grafiek is beperkt tot gebruikers.

    ![Cohort in gebruikers-hulpprogramma](media\app-insights-tutorial-users\UsersCohort.png)


## <a name="compare-desired-activity-to-reality"></a>Gewenste activiteit realiteit vergelijken
Tijdens de vorige panelen gericht zijn op wat gebruikers van uw toepassing gedaan **schoorstenen** zich richten op wat u wilt dat gebruikers te doen.  Een trechter vertegenwoordigt een reeks stappen in uw toepassing en het percentage van de gebruikers die de stappen doorlopen.  U kunt bijvoorbeeld een trechter die berekent het percentage van de gebruikers die verbinding maken met uw toepassing die product zoeken maken.  Vervolgens ziet u het percentage van de gebruikers die aan een winkelwagen product toevoegen en het percentage van de personen die een aankoop voltooit.

1. Selecteer **schoorstenen** in het menu en klik vervolgens op **nieuw**. 

    ![](media\app-insights-tutorial-users\funnelsnew.png)

2. Typ in een **trechter naam**.
3. Een trechter met ten minste twee stappen maken door een actie voor elke stap.  De lijst met acties wordt samengesteld uit verzameld door Application Insights.

    ![](media\app-insights-tutorial-users\funnelsedit.png)

4. Klik op **opslaan** opslaan van de trechter en bekijk vervolgens de resultaten.  Het venster rechts van de trechter bevat de meest voorkomende gebeurtenissen voor de eerste activiteit en na de laatste activiteit om te begrijpen gebruiker instinct rond de specifieke volgorde.

    ![](media\app-insights-tutorial-users\funnelsright.png)


## <a name="learn-which-customers-return"></a>Meer informatie over welke klanten retourneren
**Bewaartermijn** biedt meer inzicht in welke gebruikers zijn terugkomen voor uw toepassing.  

1. Selecteer **bewaren** in het menu.
2. Standaard bevat de geanalyseerde gegevens gebruikers die geen actie uitgevoerd en vervolgens wordt geretourneerd als actie wilt uitvoeren.  U kunt dit filter wijzigen in een opnemen, bijvoorbeeld alleen gebruikers die na het voltooien van een aankoop geretourneerd.

    ![](media\app-insights-tutorial-users\retentionquery.png)

3. De bestaande gebruikers die overeenkomen met de criteria worden weergegeven in de grafische en tabel formulier voor verschillende tijdsduren.  Er is een doorsnee patroon voor een geleidelijke afname gebruikers gedurende een periode wordt geretourneerd.  Een plotselinge afname van een periode naar de volgende mogelijk een probleem veroorzaakt. 

    ![](media\app-insights-tutorial-users\retentiongraph.png)

## <a name="analyze-user-navigation"></a>Navigatie door de gebruiker analyseren
Een **gebruikersstroom** visualiseren hoe gebruikers navigeren tussen de pagina's en onderdelen van uw toepassing.  Zo kunt u antwoorden op vragen zoals waar gebruikers doorgaans verplaatsen van een bepaalde pagina, hoe ze doorgaans sluit uw toepassing en of er zijn geen acties die regelmatig worden herhaald.

1.  Selecteer **gebruiker stromen** in het menu.
2.  Klik op **nieuw** maken van een nieuwe gebruikersstroom en klik vervolgens op **bewerken** om de gegevens te bewerken.
3.  Vergroot de **tijdsbereik** in 7 dagen en selecteer vervolgens een eerste gebeurtenis.  De stroom wordt gebruikerssessies die met die gebeurtenis beginnen bijhouden.

    ![](media\app-insights-tutorial-users\flowsedit.png)

4.  De gebruikersstroom wordt weergegeven, ziet u de paden naar andere gebruikers en hun sessie telt.  Blauwe lijnen duiden op een actie die de gebruiker na de huidige actie uitgevoerd.  Een rode regel geeft aan het einde van de gebruikerssessie.

    ![](media\app-insights-tutorial-users\flows.png)

5.  Als een gebeurtenis uit de stroom verwijderen, klikt u op de **x** in de linkerbenedenhoek van de actie en klik vervolgens op **maken grafiek**.  De grafiek wordt opnieuw getekend met alle geopende exemplaren van die gebeurtenis verwijderd.  Klik op **bewerken** om te zien van de gebeurtenis is nu toegevoegd aan **uitgesloten gebeurtenissen**.

    ![](media\app-insights-tutorial-users\flowsexclude.png)

## <a name="consolidate-usage-data"></a>Consolideren van gebruiksgegevens
**Werkmappen** combineren gegevensvisualisaties, Analytics query's en tekst in interactieve documenten.  Werkmappen kunt u algemene informatie over het gebruik van groeperen, samenvoegen van gegevens uit een bepaald incident of rapporteren aan het team van uw toepassing gebruik.

1.  Selecteer **werkmappen** in het menu.
2.  Klik op **nieuw** voor het maken van een nieuwe werkmap.
3.  Een query is al opgegeven waarin alle gebruiksgegevens in de laatste dag als een staafdiagram weergegeven.  U kunt deze query te gebruiken, handmatig bewerken of op **steekproef query's** in andere nuttige query's te selecteren.

    ![](media\app-insights-tutorial-users\samplequeries.png)

4.  Klik op **bewerken voltooid**.
5.  Klik op **bewerken** in het bovenste deelvenster om de tekst aan de bovenkant van de werkmap te bewerken.  Dit is geformatteerd met markdown.

    ![](media\app-insights-tutorial-users\markdown.png)

6.  Klik op **gebruikers toevoegen** toevoegen van een grafiek met gebruikersgegevens.  Als u wilt en klik vervolgens op de details van de grafiek bewerken **bewerken voltooid** op te slaan.


## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd hoe u uw gebruikers te analyseren, Ga naar de volgende zelfstudie voor meer informatie over het maken van aangepaste dashboards die deze informatie met andere nuttige informatie over uw toepassing combineren.

> [!div class="nextstepaction"]
> [Aangepaste dashboards maken](app-insights-tutorial-dashboards.md)
