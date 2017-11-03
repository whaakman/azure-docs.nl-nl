---
title: Prestatieproblemen analyseren met behulp van Azure Application Insights | Microsoft Docs
description: Zelfstudie om te zoeken en prestatieproblemen in uw toepassing met behulp van Azure Application Insights.
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 1176e6ac33db5b9428a323c3a6271818807afc72
ms.sourcegitcommit: e462e5cca2424ce36423f9eff3a0cf250ac146ad
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/01/2017
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Zoeken en onderzoeken van prestatieproblemen met Azure Application Insights

Azure Application Insights verzamelt telemetrie van uw toepassing analyseren van de werking en prestaties.  U kunt deze informatie gebruiken voor het identificeren van problemen die zich zou kunnen voordoen of verbeteringen in de toepassing die de meeste impact gebruikers zouden vast te stellen.  Deze zelfstudie leert u het proces voor het analyseren van de prestaties van zowel de server-onderdelen van uw toepassing en het perspectief van de client.  Procedures voor:

> [!div class="checklist"]
> * Identificeren van de prestaties van de operations-serverzijde
> * Bewerkingen van de server om te bepalen van de hoofdoorzaak van trage prestaties analyseren
> * Geeft de traagste bewerkingen aan clientzijde
> * Details van paginaweergaven met querytaal analyseren


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
- Een .NET-toepassing in Azure implementeert en [inschakelen van de Application Insights-SDK](app-insights-asp-net.md).
- [De profiler Application Insights inschakelen](app-insights-profiler.md#enable-the-profiler) voor uw toepassing.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="identify-slow-server-operations"></a>Trage serverbewerkingen identificeren
Application Insights verzamelt details van de prestaties voor de verschillende bewerkingen in uw toepassing.  U kunt met het vaststellen van deze bewerkingen met de langste duur potentiële problemen te onderzoeken of beste gericht op de verdere ontwikkeling ter verbetering van de algehele prestaties van de toepassing.

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  
1. Openen van de **prestaties** select van het deelvenster **prestaties** onder de **onderzoeken** menu of klik op de **serverreactietijd** grafiek .

    ![Prestaties](media/app-insights-tutorial-performance/performance.png)

2. De **prestaties** deelvenster ziet u het aantal en de gemiddelde duur van elke bewerking voor de toepassing.  Deze informatie kunt u deze bewerkingen dat de meeste gebruikers invloed te identificeren. In dit voorbeeld wordt de **klanten/Details ophalen** en **GET Home/Index** waarschijnlijk kandidaten zijn voor het onderzoeken van vanwege hun relatief hoog duur en het aantal aanroepen.  Andere bewerkingen mogelijk een hogere duur maar zelden werden genoemd, zodat het effect van hun verbetering minimaal zou zijn.  

    ![Deelvenster prestaties](media/app-insights-tutorial-performance/performance-blade.png)

3. De grafiek toont momenteel de gemiddelde duur van alle bewerkingen gedurende een bepaalde periode.  De bewerkingen die u bent geïnteresseerd zijn in door ze vastmaken aan de grafiek toevoegen.  Dit betekent dat er een aantal pieken waard zijn.  Dit verdere isoleren doordat het tijdvenster van de grafiek.

    ![Pincode-bewerkingen](media/app-insights-tutorial-performance/pin-operations.png)

4.  Klik op een bewerking om het deelvenster prestaties aan de rechterkant weer te geven. Dit toont de distributie van de duur voor andere aanvragen.  Gebruikers worden meestal merken langzame prestaties op het punt een halve seconde, dus het venster verkleinen tot aanvragen via 500 milliseconden.  

    ![Duur van de distributie](media/app-insights-tutorial-performance/duration-distribution.png)

5.  In dit voorbeeld ziet u dat een groot aantal aanvragen per seconde zijn overname verwerken. U kunt de details van deze bewerking bekijken door te klikken op **Bewerkingsdetails**.

    ![Details van bewerking](media/app-insights-tutorial-performance/operation-details.png)

6.  De informatie die u hebt verzameld, tot nu toe alleen bevestigt dat zich op trage prestaties, maar weinig om de hoofdoorzaak is.  De **Profiler** helpt dit door de werkelijke code die is uitgevoerd voor de bewerking en de tijd die nodig zijn voor elke stap weergegeven. Bepaalde bewerkingen mogelijk geen traceren, omdat de profiler periodiek wordt uitgevoerd.  Na verloop van tijd moeten meer bewerkingen traceringen hebben.  De profiler voor de bewerking starten, klikt u op **Profiler traceringen**.
5.  De tracering toont de afzonderlijke gebeurtenissen voor elke bewerking zo kunt u de hoofdoorzaak voor de duur van de algehele bewerking onderzoeken.  Klik op een van de bovenste voorbeelden die de langste duur hebben.
6.  Klik op **Hot pad weergeven** Markeer het specifieke pad van gebeurtenissen die de meeste aan de totale duur van de bewerking bijdragen.  In dit voorbeeld ziet u dat de traagste aanroep afkomstig van is *FabrikamFiberAzureStorage.GetStorageTableData* methode. Het onderdeel dat de meeste tijd in beslag neemt is de *CloudTable.CreateIfNotExist* methode. Als u deze regel wordt uitgevoerd telkens wanneer de functie wordt aangeroepen, worden onnodige netwerk oproep- en CPU-bronnen gebruikt. De beste manier om uw code vast is voor deze regel op sommige opstartmethode die slechts één keer worden uitgevoerd. 

    ![Profiler details](media/app-insights-tutorial-performance/profiler-details.png)

7.  De **prestaties Tip** biedt ondersteuning voor de beoordeling dat de overmatige duur vanwege wachten is aan de bovenkant van het scherm.  Klik op de **wachten** koppeling voor documentatie over het interpreteren van de verschillende typen gebeurtenissen.

    ![Prestaties tip](media/app-insights-tutorial-performance/performance-tip.png)

8.  Voor verdere analyse, klikt u op **tracering etl downloaden** voor het downloaden van de tracering in naar Visual Studio.

## <a name="use-analytics-data-for-server"></a>Analytische gegevens voor server gebruiken
Application Insights Analytics biedt een uitgebreide querytaal waarmee u om alle gegevens die worden verzameld door Application Insights te analyseren.  U kunt deze gebruiken voor het uitvoeren van de diepgaande analyse op aanvraag-en prestatiegegevens.

1. Terug naar het bewerking detail deelvenster en klik op de knop Analytics.

    ![Knop Analytics](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Application Insights Analytics wordt geopend met een query voor elk van de weergaven in het deelvenster.  Als ze zijn of voor uw vereisten aanpassen, kunt u deze query's uitvoeren.  De eerste query wordt de duur voor deze bewerking gedurende een bepaalde periode.

    ![Analytische gegevens](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Trage clientbewerkingen identificeren
Naast het identificeren van serverprocessen te optimaliseren, kunt Application Insights analyseren van het perspectief van clientbrowsers.  Hiermee kunt u mogelijke verbeteringen clientonderdelen identificeren en zelfs geven aan welke problemen met verschillende browsers of andere locaties.

1. Selecteer **Browser** onder **onderzoeken** de samenvatting browser openen.  Dit biedt een visuele samenvatting van verschillende telemetries van uw toepassing vanuit het perspectief van de browser.

    ![Browser samenvatting](media/app-insights-tutorial-performance/browser-summary.png)

2.  Schuif omlaag naar **wat mijn traagste's zijn?**.  Hiermee wordt een lijst van de pagina's in uw toepassing die u hebt het langst voor clients om te laden.  U kunt deze informatie gebruiken om de prioriteit van de pagina's die de meeste impact op de gebruiker hebben.
3.  Klik op een van de pagina's te openen de **paginaweergave** Configuratiescherm.  In het voorbeeld wordt de **/FabrikamProd** pagina een overmatige gemiddelde duur wordt weergegeven.  De **paginaweergave** Configuratiescherm vindt u gedetailleerde informatie over deze pagina met inbegrip van een verdeling van andere duur bereiken.

    ![Paginaweergave](media/app-insights-tutorial-performance/page-view.png)

4.  Klik op de hoogste duur om te controleren van de details van deze aanvragen.  Klik vervolgens op de afzonderlijke aanvraag details wilt weergeven van de client opvragen van de pagina met inbegrip van het type browser en de locatie.  Deze informatie kan u helpen bij het bepalen of er prestaties problemen betrekking hebben op specifieke typen clients zijn.

    ![Details van aanvraag](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>Analytische gegevens gebruiken voor client
Als de gegevens verzameld voor de prestaties van de server, Application Insights worden alle clientsgegevens beschikbaar gemaakt voor grondige analyse met Analytics.

1. Terug naar de browser samenvatting en klik op het pictogram Analytics.

    ![Analytics-pictogram](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Application Insights Analytics wordt geopend met een query voor elk van de weergaven in het deelvenster. De eerste query geeft de duur voor verschillende paginaweergaven gedurende een bepaalde periode.

    ![Analytische gegevens](media/app-insights-tutorial-performance/client-analytics.png)

3.  Smart diagnostische gegevens is een functie van Application Insights Analytics waarmee unieke patronen in de gegevens.  Wanneer u de Smart Diagnostics punt in het lijndiagram klikt, wordt dezelfde query uitgevoerd zonder de records die de afwijkingsdetectie veroorzaakt.  Details van deze records worden weergegeven in de sectie Opmerking van de query zodat u de eigenschappen van die paginaweergaven die de overmatige duur veroorzaken kunt identificeren.

    ![Smart diagnostische gegevens](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd runtime-uitzonderingen te identificeren, Ga naar de volgende zelfstudie voor meer informatie over het maken van waarschuwingen in reactie op fouten.

> [!div class="nextstepaction"]
> [Waarschuwing bij toepassingsstatus](app-insights-tutorial-alert.md)
