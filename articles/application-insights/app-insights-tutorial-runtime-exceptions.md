---
title: Diagnose runtime-uitzonderingen met behulp van Azure Application Insights | Microsoft Docs
description: Zelfstudie voor het vinden en runtime-uitzonderingen in uw toepassing met behulp van Azure Application Insights onderzoeken.
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 11e0f2f19acc843f1c558b5d0cfe84291035a6a5
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Zoeken en runtime-uitzonderingen met Azure Application Insights onderzoeken

Azure Application Insights verzamelt telemetrie van uw toepassing om te identificeren en runtime-uitzonderingen te diagnosticeren.  Deze zelfstudie leert u dit proces met uw toepassing.  Procedures voor:

> [!div class="checklist"]
> * Wijzigen van uw project om in te schakelen van uitzonderingen bijhouden
> * Uitzonderingen voor de verschillende onderdelen van uw toepassing te identificeren
> * Details weergeven van een uitzondering
> * Een momentopname van de uitzondering voor Visual Studio downloaden voor foutopsporing
> * Details van mislukte aanvragen met querytaal analyseren
> * Een nieuw werkitem om op te lossen de defecte code maken


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
- Download en installeer de [Visual Studio momentopname Debugger](http://aka.ms/snapshotdebugger).
- Schakel [Visual Studio Snapshot-foutopsporingsprogramma](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-snapshot-debugger)
- Een .NET-toepassing in Azure implementeert en [inschakelen van de Application Insights-SDK](app-insights-asp-net.md). 
- De zelfstudie houdt de identificatie van een uitzondering in uw toepassing, zodat uw code in uw omgeving voor ontwikkeling of tests voor het genereren van een uitzondering te wijzigen. 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Aanmelden bij de Azure portal op [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Analyseren van mislukte pogingen
Application Insights verzamelt fouten in uw toepassing en kunt u de frequentie tussen verschillende bewerkingen kunt concentreren op die met de grootste verbetering bekijken.  U kunt vervolgens inzoomen op gegevens van deze fouten te identificeren hoofdoorzaak.   

1. Selecteer **Application Insights** en vervolgens uw abonnement.  
1. Openen van de **fouten** select van het deelvenster **fouten** onder de **onderzoeken** menu of klik op de **mislukte aanvragen** grafiek.

    ![Mislukte aanvragen](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

2. De **mislukte aanvragen** deelvenster ziet u het aantal mislukte aanvragen en het aantal gebruikers dat van invloed op een voor elke bewerking voor de toepassing.  Deze informatie door de gebruiker wordt gesorteerd kunt u de fouten dat invloed heeft op de meeste gebruikers identificeren.  In dit voorbeeld wordt de **krijgen werknemers/maken** en **klanten/Details ophalen** waarschijnlijk kandidaten zijn voor het onderzoeken van vanwege het grote aantal fouten en betrokken gebruikers.  Een bewerking selecteren bevat meer informatie over deze bewerking in het rechterpaneel.

    ![Configuratiescherm voor aanvragen is mislukt](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

3. Verklein de periode om in te zoomen op de periode waarin het Faalpercentage ziet u een piek.

    ![Venster van mislukte aanvragen](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

4. Klik op **Details weergeven** om de details voor de bewerking te bekijken.  Dit omvat lopende ziet u twee mislukte afhankelijkheden die gezamenlijk bijna de helft van een tweede heeft te voltooien.  U vindt meer informatie over prestatieproblemen analyseren door de zelfstudie [vinden en onderzoeken van prestatieproblemen met Azure Application Insights](app-insights-tutorial-performance.md).

    ![Details van aanvragen is mislukt](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

5. De operations-details bevat ook een FormatException die wordt weergegeven aan de fout heeft veroorzaakt.  Klik op de uitzondering of op de **Top-3 uitzondering typen** aantal om de details ervan weer te geven.  U ziet dat het is vanwege een ongeldige postcode.

    ![Details van uitzondering](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)



## <a name="identify-failing-code"></a>Mislukt code identificeren
Het foutopsporingsprogramma momentopname verzamelt momentopnamen van de meest voorkomende uitzonderingen in uw toepassing om u te helpen bij het oplossen van de hoofdoorzaak in productie.  U kunt momentopnamen foutopsporing weergeven in de portal om te zien van de aanroep stapelen en variabelen op elke aanroepstackframe controleren. U kunt vervolgens de broncode fouten opsporen door te downloaden van de momentopname en openen in Visual Studio 2017.

1. Klik in de eigenschappen van de uitzondering op **Open foutopsporing momentopname**.
2. De **fouten opsporen in momentopname** Configuratiescherm wordt geopend met de aanroepstack voor de aanvraag.  Klik op een methode om de waarden van alle lokale variabelen op het moment van de aanvraag weer te geven.  Vanaf de eerste methode in dit voorbeeld, ziet u lokale variabelen die geen waarde.

    ![Fouten opsporen in momentopname](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. De eerste aanroep met geldige waarden is **ValidZipCode**, en we zien dat een zip-code is opgegeven via de letters die niet kunnen worden vertaald naar een geheel getal.  Dit lijkt te zijn van de fout in de code die moet worden gecorrigeerd.

    ![Fouten opsporen in momentopname](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Als u wilt downloaden deze momentopname in Visual Studio waar we de code die moet worden gecorrigeerd kunnen vinden, klikt u op **momentopname downloaden**.
6. De momentopname is geladen in Visual Studio.
7. U kunt nu een foutopsporingssessie uitvoeren in Visual Studio die snel identificeert de coderegel dat de uitzondering heeft veroorzaakt.

    ![Uitzondering in code](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Analytische gegevens gebruiken
Alle gegevens die worden verzameld door Application Insights wordt opgeslagen in Azure Log Analytics, waardoor een uitgebreide querytaal waarmee u de gegevens op verschillende manieren analyseren.  We kunnen deze gegevens gebruiken voor het analyseren van de aanvragen die we je onderzoekt uitzondering gegenereerd. 

8. Klik op de informatie CodeLens boven de code voor het weergeven van telemetrie geleverd door de Application Insights.

    ![Code](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Klik op **impact analyseren** Application Insights Analytics openen.  Het wordt gevuld met meerdere query's die informatie over mislukte verzoeken zoals betrokken gebruikers browsers en regio's bevatten.<br><br>![Analytische gegevens](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Werkitem toevoegen
Als u Application Insights verbinding met een traceringssysteem zoals Visual Studio Team Services of GitHub, maakt u een werkitem rechtstreeks vanuit Application Insights.

1. Ga terug naar de **eigenschappen uitzondering** deelvenster in Application Insights.
2. Klik op **nieuw werkitem**.
3. De **nieuw werkitem** Configuratiescherm wordt geopend met informatie over de uitzondering al ingevuld.  U kunt geen extra informatie voordat u deze opslaat toevoegen.

    ![Nieuw werkitem](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Volgende stappen
Nu dat u hebt geleerd runtime-uitzonderingen te identificeren, Ga naar de volgende zelfstudie voor informatie over het identificeren en onderzoeken van prestatieproblemen.

> [!div class="nextstepaction"]
> [Problemen met prestaties](app-insights-tutorial-performance.md)