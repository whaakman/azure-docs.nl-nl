---
title: Prestatieproblemen analyseren met behulp van Azure Application Insights | Microsoft Docs
description: Zelfstudie voor het vinden en diagnosticeren van prestatieproblemen in uw toepassing met behulp van Azure Application Insights.
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/18/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 437c45891d1d20f5fadca8a58954185a3aef56ac
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="find-and-diagnose-performance-issues-with-azure-application-insights"></a>Prestatieproblemen vinden en diagnosticeren met behulp van Azure Application Insights

Azure Application Insights verzamelt telemetrie van uw toepassing om de werking en prestaties te analyseren.  U kunt deze informatie gebruiken om problemen te identificeren die zich kunnen voordoen of om te identificeren welke verbeteringen in de toepassing het meeste effect zouden hebben voor gebruikers.  Deze zelfstudie leidt u door het proces van het analyseren van de prestaties van zowel de servercomponenten van uw applicatie als het perspectief van de client.  Procedures voor:

> [!div class="checklist"]
> * Het identificeren van de prestaties van bewerkingen aan de serverzijde
> * Het analyseren van serverbewerkingen om de hoofdoorzaak van trage prestaties vast te stellen
> * Het identificeren van de traagste bewerkingen aan de clientzijde
> * Details van paginaweergaven analyseren met een querytaal


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
- Implementeer een .NET-toepassing in Azure en [schakel de Application Insights-SDK](app-insights-asp-net.md)in.
- [Schakel de Application Insights-profiler in](app-insights-profiler.md#installation) voor uw toepassing.

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="identify-slow-server-operations"></a>Trage serverbewerkingen identificeren
Application Insights verzamelt prestatiedetails over de verschillende bewerkingen in uw toepassing.  Door de bewerkingen met de langste duur te identificeren, kunt u potentiële problemen diagnosticeren of de beste ontwikkelingsrichting bepalen voor het verbeteren van de algehele prestaties van de toepassing.

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  
1. U kunt het deelvenster **Prestaties** openen door **Prestaties** te selecteren in het menu **Onderzoeken** of door op de grafiek **Serverreactietijd** te klikken.

    ![Prestaties](media/app-insights-tutorial-performance/performance.png)

2. In het deelvenster **Prestaties** deelvenster ziet u het aantal en de gemiddelde duur van elke bewerking voor de toepassing.  U kunt deze informatie gebruiken om de bewerkingen te identificeren die de meeste impact hebben voor gebruikers. In dit voorbeeld zijn **GET Customers/Details** en **GET Home/Index** goede kandidaten om te onderzoeken, vanwege hun relatief lange duur en hoge aantal aanroepen.  Andere bewerkingen hebben wellicht een langere duur, maar werden zelden aangeroepen, zodat het effect van een verbetering ervan minimaal zou zijn.  

    ![Deelvenster Prestaties](media/app-insights-tutorial-performance/performance-blade.png)

3. De grafiek toont momenteel de gemiddelde duur van alle bewerkingen gedurende een bepaalde periode.  Voeg de bewerkingen waarin u geïnteresseerd bent toe door ze vast te maken aan de grafiek.  Dit laat zien dat er enkele pieken zijn die het onderzoeken waard zijn.  Isoleer dit verder door het tijdvenster van de grafiek te verkleinen.

    ![Bewerkingen vastmaken](media/app-insights-tutorial-performance/pin-operations.png)

4.  Klik op een bewerking om het prestatiedeelvenster ervan aan de rechterkant weer te geven. Dit toont de distributie van de duur voor verschillende aanvragen.  Gebruikers merken langzame prestaties meestal op bij ongeveer een halve seconde. Reduceer het venster dus tot aanvragen van meer dan 500 milliseconden.  

    ![Duurdistributie](media/app-insights-tutorial-performance/duration-distribution.png)

5.  In dit voorbeeld ziet u dat er een groot aantal aanvragen is waarvan het verwerken langer duurt dan een seconde. U kunt de details van deze bewerking bekijken door op **Bewerkingsdetails** te klikken.

    ![Bewerkingsdetails](media/app-insights-tutorial-performance/operation-details.png)

    > [!NOTE]
    Schakel de [preview-ervaring](app-insights-previews.md) "Unified details: E2E Transaction Diagnostics" (Gecombineerde details: E2E-transactiediagnose) in om alle gerelateerde telemetrie aan de serverzijde in één weergave op het volledige scherm te zien, zoals aanvragen, afhankelijkheden, traceringen, gebeurtenissen, enzovoort. 

    Met de ingeschakelde preview kunt u zien hoeveel tijd wordt doorgebracht in afhankelijkheidsaanroepen, alsmede eventuele fouten of uitzonderingen, in één gecombineerde ervaring. Voor transacties tussen onderdelen kunnen het Gantt-diagram en het detailvenster u helpen snel het onderdeel, de afhankelijkheid of de uitzondering te diagnosticeren waar de hoofdoorzaak zich bevindt. U kunt de onderste sectie uitvouwen om de tijdreeks te zien van traceringen of gebeurtenissen die voor de geselecteerde onderdeelbewerking zijn verzameld. [Meer informatie over de nieuwe ervaring](app-insights-transaction-diagnostics.md)  

    ![Diagnostische gegevens voor transacties](media/app-insights-tutorial-performance/e2e-transaction-preview.png)


6.  De informatie die u tot nu toe hebt verzameld, bevestigt alleen dat er trage prestaties zijn, maar helpt niet erg bij het achterhalen van de hoofdoorzaak.  De **Profiler** helpt hiermee door de werkelijke code te laten zien die voor de bewerking is uitgevoerd, en de voor elke stap benodigde tijd. Voor bepaalde bewerkingen is er mogelijk geen tracering, omdat de profiler periodiek wordt uitgevoerd.  Na verloop van tijd moeten meer bewerkingen traceringen hebben.  Klik op **Profilertraceringen** om de profiler voor de bewerking te starten.
5.  De tracering toont de afzonderlijke gebeurtenissen voor elke bewerking, zodat u de hoofdoorzaak voor de duur van de algehele bewerking kunt onderzoeken.  Klik op een van de bovenste voorbeelden, die de langste duur hebben.
6.  Klik op **Snelpad weergeven** om het specifieke pad van gebeurtenissen die het meest bijdragen aan de totale duur van de bewerking, te markeren.  In dit voorbeeld ziet u dat de traagste aanroep afkomstig is van de methode *FabrikamFiberAzureStorage.GetStorageTableData*. Het onderdeel dat de meeste tijd in beslag neemt is de methode *CloudTable.CreateIfNotExist*. Als deze regel code wordt uitgevoerd telkens wanneer de functie wordt aangeroepen, worden er onnodige netwerkaanroepen en CPU-bronnen verbruikt. De beste manier om uw code te verbeteren is door deze regel in een opstartmethode te plaatsen die slechts één keer wordt uitgevoerd. 

    ![Profiler-details](media/app-insights-tutorial-performance/profiler-details.png)

7.  De **Prestatietip** bovenaan het scherm ondersteunt de gedachte dat de lange duur te wijten is aan wachten.  Klik op de koppeling **wachten** voor documentatie over het interpreteren van de verschillende typen gebeurtenissen.

    ![Prestatietip](media/app-insights-tutorial-performance/performance-tip.png)

8.  Voor verdere analyse kunt u op **Download .etl trace** (.etl-tracering downloaden) klikken om de tracering naar Visual Studio te downloaden.

## <a name="use-analytics-data-for-server"></a>Analytische gegevens voor server gebruiken
Application Insights Analytics biedt een uitgebreide querytaal voor het analyseren van alle met Application Insights verzamelde gegevens.  U kunt deze gebruiken om diepgaande analysen uit te voeren op aanvraag- en prestatiegegevens.

1. Ga terug naar het detailvenster van de bewerking en klik op de knop Analytics.

    ![Knop Analytics](media/app-insights-tutorial-performance/server-analytics-button.png)

2. Application Insights Analytics wordt geopend met een query voor elk van de weergaven in het deelvenster.  U kunt deze query’s zó uitvoeren, of ze aanpassen aan uw behoeften.  De eerste query laat de duur van deze bewerking gedurende een bepaalde periode zien.

    ![Analytische gegevens](media/app-insights-tutorial-performance/server-analytics.png)


## <a name="identify-slow-client-operations"></a>Trage clientbewerkingen identificeren
Naast het identificeren van serverprocessen om te optimaliseren, kan Application Insights ook het perspectief van clientbrowsers analyseren.  Dit kan u helpen potentiële verbeteringen aan clientcomponenten te identificeren en zelfs problemen met verschillende browsers of verschillende locaties te identificeren.

1. Selecteer **Browser** onder **Onderzoeken** om het browseroverzicht te openen.  Dit geeft een visueel overzicht van verschillende telemetrieën van uw toepassing vanuit het perspectief van de browser.

    ![Browseroverzicht](media/app-insights-tutorial-performance/browser-summary.png)

2.  Schuif omlaag naar **Wat zijn mijn traagste pagina’s?**.  Hier wordt een lijst weergegeven met de pagina’s in uw toepassing waarvan het laden in de clients de meeste tijd heeft gekost.  U kunt deze informatie gebruiken om prioriteit te geven aan de pagina's die de meeste impact op de gebruiker hebben.
3.  Klik op een van de pagina’s om het deelvenster **Paginaweergave** te openen.  In het voorbeeld laat de pagina **/FabrikamProd** een overmatige gemiddelde duur zien.  Het deelvenster **Paginaweergave** bevat gedetailleerde informatie over deze pagina, waaronder een uitsplitsing van verschillende duurbereiken.

    ![Paginaweergave](media/app-insights-tutorial-performance/page-view.png)

4.  Klik op de langste duur om de details van deze aanvragen te inspecteren.  Klik vervolgens op de afzonderlijke aanvraag om details te bekijken van de client die de pagina aanvraagt, inclusief het type browser en de locatie.  Deze informatie kan u helpen bepalen of er prestatieproblemen zijn die te maken hebben met specifieke typen clients.

    ![Gedetailleerde aanvraaggegevens](media/app-insights-tutorial-performance/request-details.png)

## <a name="use-analytics-data-for-client"></a>Analytische gegevens voor client gebruiken
Net als de gegevens die worden verzameld voor serverprestaties, maakt Application Insights alle clientgegevens beschikbaar voor diepgaande analyse met behulp van Analytics.

1. Ga terug naar het browseroverzicht en klik op het pictogram Analytics.

    ![Pictogram Analytics](media/app-insights-tutorial-performance/client-analytics-icon.png)

2. Application Insights Analytics wordt geopend met een query voor elk van de weergaven in het deelvenster. De eerste query laat de duur zien van verschillende paginaweergaven gedurende een bepaalde periode.

    ![Analytische gegevens](media/app-insights-tutorial-performance/client-analytics.png)

3.  Smart Diagnostics is een functie van Application Insights Analytics die unieke patronen in de gegevens identificeert.  Wanneer u op de Smart Diagnostics-stip in het lijndiagram klikt, wordt dezelfde query uitgevoerd zonder de records die de anomalie hebben veroorzaakt.  Details van deze records worden weergegeven in de commentaarsectie van de query, zodat u de eigenschappen van die paginaweergaven kunt identificeren die de overmatige duur veroorzaken.

    ![Smart Diagnostics](media/app-insights-tutorial-performance/client-smart-diagnostics.png)


## <a name="next-steps"></a>Volgende stappen
Nu u hebt geleerd runtime-uitzonderingen te identificeren, kunt u doorgaan naar de volgende zelfstudie om te leren hoe u waarschuwingen kunt maken in reactie op fouten.

> [!div class="nextstepaction"]
> [Waarschuwing bij toepassingsstatus](app-insights-tutorial-alert.md)
