---
title: Azure-logboekanalyse processen met Microsoft-Flow automatiseren
description: Meer informatie over hoe u Microsoft Flow snel herhaalbare processen automatiseren met behulp van de Azure-logboekanalyse-connector kunt gebruiken.
services: log-analytics
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: log-analytics
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 09/29/2017
ms.author: bwren
ms.openlocfilehash: 9884b97aad8f022e6bc20b2a630ccdd07ebc64db
ms.sourcegitcommit: 5735491874429ba19607f5f81cd4823e4d8c8206
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/16/2017
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a>Log Analytics-processen met de connector voor Microsoft-Flow automatiseren
[Microsoft-Flow](https://ms.flow.microsoft.com) kunt u geautomatiseerde werkstromen met honderden acties voor diverse services maken. De uitvoer van een actie kan worden gebruikt als invoer voor andere zodat u integratie tussen verschillende services maken.  De Azure-logboekanalyse-connector voor Microsoft-Flow kunt u voor het bouwen van werkstromen die door het logboek van zoekopdrachten in logboekanalyse opgehaalde gegevens bevatten.

U kunt bijvoorbeeld Microsoft Flow Log Analytics-gegevens in een e-mailmelding vanuit Office 365 gebruiken, maakt een bug in Visual Studio Team Services of een toegestane bericht.  Door een eenvoudige planning of van een bepaalde actie in een gekoppelde service, zoals wanneer een e-mailbericht of een tweet wordt ontvangen, kunt u een werkstroom activeren.  

De zelfstudie in dit artikel ziet u het maken van een stroom die de resultaten van een zoekopdracht Log Analytics-logboek automatisch via e-mail, slechts één voorbeeld verzonden van hoe u Log Analytics in Microsoft Flow kunt gebruiken. 


## <a name="step-1-create-a-flow"></a>Stap 1: Een stroom maken
1. Aanmelden bij [Microsoft Flow](http://flow.microsoft.com), en selecteer **mijn loopt**.
2. Klik op **+ maken van leeg**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Stap 2: Maak een trigger voor uw flow
1. Klik op **zoeken honderden connectors en triggers**.
2. Type **planning** in het zoekvak.
3. Selecteer **planning**, en selecteer vervolgens **schema - terugkeerpatroon**.
4. In de **frequentie** vak Selecteer **dag** en in de **Interval** Voer **1**.<br><br>![Dialoogvenster voor Microsoft-Flow-trigger](media/log-analytics-flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Stap 3: Een actie logboekanalyse toevoegen
1. Klik op **+ een nieuwe stap**, en klik vervolgens op **een actie toevoegen**.
2. Zoeken naar **Meld Analytics**.
3. Klik op **Azure Log Analytics-query uitvoeren en visualiseren resultaten**.<br><br>![Log Analytics query-venster uitvoeren](media/log-analytics-flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>Stap 4: Configureer de actie Log Analytics

1. Geef de details voor uw werkruimte met inbegrip van het abonnement-ID, resourcegroep, en de naam van de werkruimte.
2. Voeg de volgende Log Analytics-query voor de **Query** venster.  Dit is alleen een voorbeeldquery en u kunt vervangen door een andere waarmee gegevens worden geretourneerd.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
```

2. Selecteer **HTML-tabel** voor de **grafiektype**.<br><br>![Log Analytics actie](media/log-analytics-flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>Stap 5: De stroom voor het verzenden van e-mail configureren

1. Klik op **nieuwe stap**, en klik vervolgens op **+ een actie toevoegen**.
2. Zoeken naar **OfficeOutlook 365**.
3. Klik op **Outlook in Office 365 – stuurt u een e-mailadres**.<br><br>![Selectievenster voor Office 365 Outlook](media/log-analytics-flow-tutorial/flow04.png)

4. Geef het e-mailadres van een ontvanger in de **naar** venster en een onderwerp in voor de e-mailadres in **onderwerp**.
5. Klik ergens in de **hoofdtekst** vak.  Een **dynamische inhoud** venster wordt geopend met waarden uit de vorige acties.  
6. Selecteer **hoofdtekst**.  Dit is de resultaten van de query in de actie logboekanalyse.
6. Klik op **geavanceerde opties weergeven**.
7. In de **Is HTML** de optie **Ja**.<br><br>![Venster van Office 365 e-configuratie](media/log-analytics-flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Stap 6: Opslaan en testen van uw flow
1. In de **stroom naam** vak en klik vervolgens op toevoegen van een naam voor uw flow **stroom maken**.<br><br>![Stroom opslaan](media/log-analytics-flow-tutorial/flow06.png)
2. De stroom is nu gemaakt en wordt uitgevoerd na een dag van de planning die u hebt opgegeven. 
3. Als u wilt testen onmiddellijk de stroom, klikt u op **nu uitvoeren** en vervolgens **uitvoeren stroom**.<br><br>![Stroom uitvoeren](media/log-analytics-flow-tutorial/flow07.png)
3. Wanneer de stroom is voltooid, controleert u het e-mailbericht van de ontvanger die u hebt opgegeven.  U moet een e-mailbericht met de volgende strekking hoofdtekst hebt ontvangen:<br><br>![Voorbeeldbericht](media/log-analytics-flow-tutorial/flow08.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [zoekopdrachten aanmelden met logboekanalyse](log-analytics-log-search-new.md).
- Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).



