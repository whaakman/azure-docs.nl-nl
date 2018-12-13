---
title: Azure Log Analytics processen automatiseren met Microsoft Flow
description: Lees hoe u Microsoft Flow snel herhaalbare om processen te automatiseren met behulp van de Azure Log Analytics-connector kunt gebruiken.
services: log-analytics
documentationcenter: ''
author: mgoedtel
manager: carmonm
ms.service: log-analytics
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.topic: conceptual
ms.date: 09/29/2017
ms.author: bwren
ms.openlocfilehash: 441064d6cbb5b3b2fae77caef3125c8db3467553
ms.sourcegitcommit: 5b869779fb99d51c1c288bc7122429a3d22a0363
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2018
ms.locfileid: "53187446"
---
# <a name="automate-log-analytics-processes-with-the-connector-for-microsoft-flow"></a>Log Analytics processen automatiseren met de connector voor Microsoft Flow
[Microsoft Flow](https://ms.flow.microsoft.com) kunt u geautomatiseerde werkstromen met behulp van honderden acties voor verschillende services maken. Uitvoer van een actie kan worden gebruikt als invoer voor andere zodat u kunt maken van de integratie tussen verschillende services.  De Azure Log Analytics-connector voor Microsoft Flow kunt u werkstromen kunnen opzetten waarmee gegevens opgehaald door bits-zoekopdrachten in Logboeken in Log Analytics bevatten.

Bijvoorbeeld, kunt u Microsoft Flow-Log Analytics-gegevens in een e-mailmelding van Office 365 gebruiken, een bug in Azure DevOps maken of een Slack-bericht.  Door een eenvoudige planning of vanuit een actie in een gekoppelde service, zoals wanneer een e-mailbericht of een tweet wordt ontvangen, kunt u een werkstroom activeren.  

De in dit artikel leert u hoe u een stroom maken die de resultaten van een logboekzoekopdracht Log Analytics automatisch via e-mail, slechts één voorbeeld verzonden van hoe u Log Analytics in Microsoft Flow kunt gebruiken. 


## <a name="step-1-create-a-flow"></a>Stap 1: Stroom maken
1. Aanmelden bij [Microsoft Flow](https://flow.microsoft.com), en selecteer **mijn stromen**.
2. Klik op **+ maken met een lege App**.

## <a name="step-2-create-a-trigger-for-your-flow"></a>Stap 2: Een trigger voor uw stroom maken
1. Klik op **honderden connectors en triggers zoeken**.
2. Type **planning** in het zoekvak in.
3. Selecteer **planning**, en selecteer vervolgens **planning - terugkeerpatroon**.
4. In de **frequentie** vak Selecteer **dag** en in de **Interval** Voer **1**.<br><br>![In het dialoogvenster van Microsoft Flow-trigger](media/flow-tutorial/flow01.png)


## <a name="step-3-add-a-log-analytics-action"></a>Stap 3: Een Log Analytics-actie toevoegen
1. Klik op **+ nieuwe stap**, en klik vervolgens op **een actie toevoegen**.
2. Zoeken naar **Log Analytics**.
3. Klik op **Azure Log Analytics-query uitvoert en resultaten te visualiseren**.<br><br>![Log Analytics uitvoeren van query-venster](media/flow-tutorial/flow02.png)

## <a name="step-4-configure-the-log-analytics-action"></a>Stap 4: De actie Log Analytics configureren

1. Geef de details voor uw werkruimte, waaronder de abonnement-ID, resourcegroep, en de naam van de werkruimte.
2. Voeg de volgende Log Analytics-query uit om de **Query** venster.  Dit is slechts een voorbeeld-query en u kunt vervangen door een andere waarmee gegevens worden geretourneerd.
```
    Event
    | where EventLevelName == "Error" 
    | where TimeGenerated > ago(1day)
    | summarize count() by Computer
    | sort by Computer
```

2. Selecteer **HTML-tabel** voor de **grafiektype**.<br><br>![Log Analytics-actie](media/flow-tutorial/flow03.png)

## <a name="step-5-configure-the-flow-to-send-email"></a>Stap 5: Configureren van de stroom om e-mail te verzenden

1. Klik op **nieuwe stap**, en klik vervolgens op **+ een actie toevoegen**.
2. Zoeken naar **Office 365 Outlook**.
3. Klik op **Office 365 Outlook: een e-mail verzenden**.<br><br>![Selectievenster voor Office 365 Outlook](media/flow-tutorial/flow04.png)

4. Geef het e-mailadres van een ontvanger in het **naar** -venster en een onderwerp in voor de e-mailadres in **onderwerp**.
5. Klik ergens in de **hoofdtekst** vak.  Een **dynamische inhoud** venster wordt geopend met de waarden van vorige acties.  
6. Selecteer **hoofdtekst**.  Dit is de resultaten van de query in de actie Log Analytics.
6. Klik op **geavanceerde opties weergeven**.
7. In de **Is HTML** Schakel **Ja**.<br><br>![Venster voor Office 365 e-configuratie](media/flow-tutorial/flow05.png)

## <a name="step-6-save-and-test-your-flow"></a>Stap 6: Opslaan en uw stroom testen
1. In de **Stroomnaam** vak, een naam voor uw stroom toevoegen en klik vervolgens op **stroom maken**.<br><br>![Stroom opslaan](media/flow-tutorial/flow06.png)
2. De stroom is nu gemaakt en wordt uitgevoerd na een dag dat de planning die u hebt opgegeven. 
3. Als u wilt testen onmiddellijk de stroom, klikt u op **nu uitvoeren** en vervolgens **stroom uitvoeren**.<br><br>![Stroom uitvoeren](media/flow-tutorial/flow07.png)
3. Wanneer de stroom is voltooid, controleert u het e-mailbericht van de ontvanger die u hebt opgegeven.  U moet een e-mailbericht met een instantie die vergelijkbaar is met het volgende hebben ontvangen:<br><br>![Voorbeeldbericht](media/flow-tutorial/flow08.png)


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [zoekopdrachten in Logboeken in Log Analytics](../../azure-monitor/log-query/log-query-overview.md).
- Meer informatie over [Microsoft Flow](https://ms.flow.microsoft.com).



