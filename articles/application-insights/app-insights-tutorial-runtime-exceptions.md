---
title: Runtime-uitzonderingen analyseren met behulp van Azure Application Insights | Microsoft Docs
description: Zelfstudie voor het vinden en diagnosticeren van runtime-uitzonderingen in uw toepassing met behulp van Azure Application Insights.
services: application-insights
keywords: 
author: mrbullwinkle
ms.author: mbullwin
ms.date: 09/19/2017
ms.service: application-insights
ms.custom: mvc
ms.topic: tutorial
manager: carmonm
ms.openlocfilehash: 115611c5d4eeffb0f0600dd0a792ee9f80247e36
ms.sourcegitcommit: 5ac112c0950d406251551d5fd66806dc22a63b01
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2018
---
# <a name="find-and-diagnose-run-time-exceptions-with-azure-application-insights"></a>Runtime-uitzonderingen vinden en diagnosticeren met behulp van Azure Application Insights

Azure Application Insights verzamelt telemetrie van uw toepassing om runtime-uitzonderingen te identificeren en diagnosticeren.  In deze zelfstudie leert u dit proces met uw toepassing.  Procedures voor:

> [!div class="checklist"]
> * Het wijzigen van uw project om het bijhouden van uitzonderingen in te schakelen
> * Het identificeren van uitzonderingen voor verschillende onderdelen van uw toepassing
> * Het weergeven van de details van een uitzondering
> * Het downloaden van een momentopname van de uitzondering naar Visual Studio voor het opsporen van fouten
> * Het analyseren van details van mislukte aanvragen met behulp van een querytaal
> * Het maken van een nieuw werkitem om de foute code te corrigeren


## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

- Installeer [Visual Studio 2017](https://www.visualstudio.com/downloads/) met de volgende workloads:
    - ASP.NET-ontwikkeling en webontwikkeling
    - Azure-ontwikkeling
- Download en installeer de [Visual Studio Snapshot Debugger](http://aka.ms/snapshotdebugger).
- [Visual Studio Snapshot Debugger](https://docs.microsoft.com/azure/application-insights/app-insights-snapshot-debugger) inschakelen
- Implementeer een .NET-toepassing in Azure en [schakel de Application Insights-SDK](app-insights-asp-net.md)in. 
- De zelfstudie volgt de identificatie van een uitzondering in uw toepassing; wijzig dus uw code in uw ontwikkelings- of testomgeving om een uitzondering te genereren. 

## <a name="log-in-to-azure"></a>Meld u aan bij Azure.
Meld u aan bij Azure Portal op [https://portal.azure.com](https://portal.azure.com).


## <a name="analyze-failures"></a>Fouten analyseren
Application Insights verzamelt fouten in uw toepassing en laat u zien hoe vaak ze voorkomen in verschillende bewerkingen, om u te helpen uw inspanningen te richten op het verhelpen van de fouten met de meeste impact.  Vervolgens kunt u inzoomen op de details van deze fouten om de hoofdoorzaak te identificeren.   

1. Selecteer **Application Insights** en selecteer vervolgens uw abonnement.  
2. U kunt het deelvenster **Fouten** openen door **Fouten** te selecteren in het menu **Onderzoeken** menu of door op de grafiek **Mislukte aanvragen** te klikken.

    ![Mislukte aanvragen](media/app-insights-tutorial-runtime-exceptions/failed-requests.png)

3. In het deelvenster **Mislukte aanvragen** deelvenster ziet u het aantal mislukte aanvragen en het aantal getroffen gebruikers voor elke bewerking van de toepassing.  Door deze informatie te sorteren op gebruiker kunt u de fouten identificeren die de meeste impact hebben op gebruikers.  In dit voorbeeld zijn **GET Employees/Create** en **GET Customers/Details** goede kandidaten om te onderzoeken, vanwege het hoge aantal fouten en getroffen gebruikers.  Door een bewerking te selecteren wordt meer informatie over deze bewerking zichtbaar in het rechter deelvenster.

    ![Deelvenster Mislukte aanvragen](media/app-insights-tutorial-runtime-exceptions/failed-requests-blade.png)

4. Verklein het tijdvenster om in te zoomen op de periode waar het aantal fouten een piek vertoont.

    ![Venster Mislukte aanvragen](media/app-insights-tutorial-runtime-exceptions/failed-requests-window.png)

5. Klik op **Details weergeven** om de details van de bewerking te bekijken.  Dit omvat een Gantt-diagram dat twee mislukte afhankelijkheden toont die samen bijna een halve seconde kostten om uit te voeren.  U kunt meer te weten komen over het analyseren van prestatieproblemen door de zelfstudie [Find and diagnose performance issues with Azure Application Insights](app-insights-tutorial-performance.md) te volgen (Prestatieproblemen vinden en diagnosticeren met Azure Application Insights).

    ![Details over mislukte aanvraag](media/app-insights-tutorial-runtime-exceptions/failed-requests-details.png)

6. De bewerkingsdetails laten ook een FormatException zien, die de fout lijkt te hebben veroorzaakt.  Klik op de uitzondering of op het aantal bij **Top 3 uitzonderingstypen** om de details ervan weer te geven.  U ziet dat de uitzondering te wijten is aan een ongeldige postcode.

    ![Details van uitzondering](media/app-insights-tutorial-runtime-exceptions/failed-requests-exception.png)

> [!NOTE]
Schakel de [preview-ervaring](app-insights-previews.md) "Unified details: E2E Transaction Diagnostics" (Gecombineerde details: E2E-transactiediagnose) in om alle gerelateerde telemetrie aan de serverzijde in één weergave op het volledige scherm te zien, zoals aanvragen, afhankelijkheden, traceringen, gebeurtenissen, enzovoort. 

Met de ingeschakelde preview kunt u zien hoeveel tijd wordt doorgebracht in afhankelijkheidsaanroepen, alsmede eventuele fouten of uitzonderingen, in één gecombineerde ervaring. Voor transacties tussen onderdelen kunnen het Gantt-diagram en het detailvenster u helpen snel het onderdeel, de afhankelijkheid of de uitzondering te diagnosticeren waar de hoofdoorzaak zich bevindt. U kunt de onderste sectie uitvouwen om de tijdreeks te zien van traceringen of gebeurtenissen die voor de geselecteerde onderdeelbewerking zijn verzameld. [Meer informatie over de nieuwe ervaring](app-insights-transaction-diagnostics.md)  

![Diagnostische gegevens voor transacties](media/app-insights-tutorial-runtime-exceptions/e2e-transaction-preview.png)

## <a name="identify-failing-code"></a>Mislukt code identificeren
De Snapshot Debugger verzamelt momentopnamen van de meest frequente uitzonderingen in uw toepassing om u te helpen bij de diagnose van de onderliggende oorzaak in de productie.  U kunt foutopsporingsmomentopnamen in het portaal bekijken om de aanroepstack te zien en variabelen inspecteren bij elk aanroepstackframe. Vervolgens kunt u fouten in de broncode opsporen door de snapshot te downloaden en te openen in Visual Studio 2017.

1. Klik in de eigenschappen van de uitzondering op **Momentopname voor foutopsporing openen**.
2. Het deelvenster **Debug Snapshot** (fouten opsporen in momentopname) wordt geopend met de aanroepstack voor de aanvraag.  Klik op een methode om de waarden van alle lokale variabelen op het moment van de aanvraag weer te geven.  Te beginnen bij de bovenste methode in dit voorbeeld, zien we lokale variabelen die geen waarde hebben.

    ![Foutopsporingsmomentopname](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-01.png)

4. De eerste aanroep met geldige waarden is **ValidZipCode**, en we zien dat er een postcode is opgegeven met letters die niet kan worden omgezet in een geheel getal.  Dit lijkt de fout in de code te zijn die moet worden gecorrigeerd.

    ![Foutopsporingsmomentopname](media/app-insights-tutorial-runtime-exceptions/debug-snapshot-02.png)

5. Klik op **Momentopname downloaden** om deze momentopname te downloaden naar Visual Studio, waar we de werkelijke code kunnen vinden die moet worden gecorrigeerd.
6. De momentopname wordt geladen in Visual Studio.
7. U kunt nu een foutopsporingssessie uitvoeren in Visual Studio die snel de coderegel identificeert die de uitzondering heeft veroorzaakt.

    ![Uitzondering in code](media/app-insights-tutorial-runtime-exceptions/exception-code.png)


## <a name="use-analytics-data"></a>Analytics-gegevens gebruiken
Alle gegevens die door Application Insights worden verzameld, worden opgeslagen in Azure Log Analytics, dat een uitgebreide querytaal biedt waarmee u de gegevens op verschillende manieren kunt analyseren.  We kunnen deze gegevens gebruiken voor het analyseren van de aanvragen die de uitzondering hebben gegenereerd die we aan het onderzoeken zijn. 

8. Klik op de CodeLens-informatie boven de code om telemetrie van Application Insights te bekijken.

    ![Code](media/app-insights-tutorial-runtime-exceptions/codelens.png)

9. Klik op **Impact analyseren** om Application Insights Analytics te openen.  Het wordt gevuld met enkele query's die details bieden over mislukte aanvragen, zoals getroffen gebruikers, browsers en regio's.<br><br>![Analytische gegevens](media/app-insights-tutorial-runtime-exceptions/analytics.png)<br>

## <a name="add-work-item"></a>Werkitem toevoegen
Als u Application Insights aansluit op een trackingsysteem zoals Visual Studio Team Services of GitHub, kunt u direct vanuit Application Insights een werkitem maken.

1. Ga terug naar het deelvenster **Uitzonderingseigenschappen** in Application Insights.
2. Klik op **Nieuw werkitem**.
3. Het deelvenster **Nieuw werkitem** wordt geopend, waarin al gegevens over de uitzondering zijn ingevuld.  U kunt aanvullende informatie toevoegen voordat u opslaat.

    ![Nieuw werkitem](media/app-insights-tutorial-runtime-exceptions/new-work-item.png)

## <a name="next-steps"></a>Volgende stappen
Nu u hebt geleerd runtime-uitzonderingen te identificeren, kunt u doorgaan naar de volgende zelfstudie om te leren hoe u prestatieproblemen kunt identificeren en diagnosticeren.

> [!div class="nextstepaction"]
> [Prestatieproblemen identificeren](app-insights-tutorial-performance.md)