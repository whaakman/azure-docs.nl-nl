---
title: Reageren op gebeurtenissen met Azure Log Analytics-waarschuwingen | Microsoft Docs
description: Deze zelfstudie biedt u meer inzicht in waarschuwingen van Log Analytics om belangrijke informatie in uw werkruimte te herkennen en u proactief te informeren over problemen of acties aan te roepen om te proberen deze problemen te verhelpen.
services: log-analytics
documentationcenter: log-analytics
author: mgoedtel
manager: carmonm
editor: ''
ms.assetid: abb07f6c-b356-4f15-85f5-60e4415d0ba2
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 07/30/2018
ms.author: magoedte
ms.custom: mvc
ms.component: na
ms.openlocfilehash: d81a41a0012d4e0be4e812d48074e7af1e92213a
ms.sourcegitcommit: e3d5de6d784eb6a8268bd6d51f10b265e0619e47
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2018
ms.locfileid: "39391143"
---
# <a name="respond-to-events-with-azure-monitor-alerts"></a>Reageren op gebeurtenissen met Azure Monitor-waarschuwingen
Er worden door Azure Alerts regels gemaakt voor het zoeken in logboeken om met regelmatige intervallen automatisch opgegeven logboekzoekopdrachten uit te voeren.  Als de resultaten van de logboekzoekopdracht aan bepaalde criteria voldoen, wordt een waarschuwingsrecord gemaakt. De regel kan vervolgens automatisch een of meer acties uitvoeren met behulp van [actiegroepen](../monitoring-and-diagnostics/monitoring-action-groups.md).  Deze zelfstudie is een vervolg van de zelfstudie [Dashboards van Log Analytics-gegevens maken en delen](log-analytics-tutorial-dashboards.md).   

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een waarschuwingsregel maken
> * Een actiegroep configureren voor het verzenden van een e-mailmelding

Om het voorbeeld in deze zelfstudie uit te voeren, moet u een bestaande virtuele machine [hebben verbonden met de Log Analytics-werkruimte](log-analytics-quick-collect-azurevm.md).

## <a name="sign-in-to-azure-portal"></a>Meld u aan bij Azure Portal
Meld u aan bij de Azure-portal op [https://portal.azure.com](https://portal.azure.com).

## <a name="create-alerts"></a>Waarschuwingen maken
Waarschuwingen worden gemaakt door regels voor waarschuwingen in Azure Monitor en kunnen automatisch opgeslagen query's uitvoeren of aangepaste zoekopdrachten in logboeken met regelmatige tussenpozen.  U kunt waarschuwingen maken op basis van specifieke prestatiemetrieken of wanneer bepaalde gebeurtenissen worden gemaakt, de afwezigheid van een gebeurtenis of een aantal gebeurtenissen die binnen een bepaalde periode worden gemaakt.  Waarschuwingen kunnen bijvoorbeeld worden gebruikt om u te waarschuwen wanneer het gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt, er een ontbrekende update wordt gedetecteerd of er een gebeurtenis wordt gegenereerd omdat een specifieke Windows-service of Linux-daemon niet wordt uitgevoerd.  Als de resultaten van de logboekzoekopdracht aan bepaalde criteria voldoen, wordt er een waarschuwing gemaakt. De regel kan vervolgens automatisch een of meer acties uitvoeren, bijvoorbeeld om u te informeren over de waarschuwing of om een ander proces aan te roepen.

In het volgende voorbeeld maakt u een waarschuwingsregel voor een metrische meting die is gebaseerd op de query *Azure VMs - Processor Utilization* die u hebt opgeslagen in de [zelfstudie Gegevens visualiseren](log-analytics-tutorial-dashboards.md). Er wordt een waarschuwing gemaakt voor elke virtuele machine die een drempel van 90% overschrijdt.

1. Klik in Azure Portal op **Alle services**. Typ **Monitor** in de lijst met resources. Als u begint te typen, wordt de lijst gefilterd op basis van uw invoer. Selecteer **Monitor**.
2. Selecteer in het linkerdeelvenster **Waarschuwingen** en klik vervolgens bovenaan de pagina op **Nieuwe waarschuwingsregel** om een nieuwe waarschuwing te maken.

    ![Nieuwe waarschuwingsregel maken](./media/log-analytics-tutorial-response/alert-rule-02.png)

3. Ga naar de sectie **Waarschuwing maken** en selecteer uw werkruimte van Log Analytics als de bron, aangezien dit een waarschuwingssignaal op basis van een logboek is.  Filter de resultaten door in de vervolgkeuzelijst **Abonnement** het abonnement te selecteren met de VM en de eerder gemaakte Log Analytics-werkruimte (als u meer dan één abonnement hebt).  Filter op **resourcetype** door **Log Analytics** te selecteren in de vervolgkeuzelijst.  Selecteer als laatste **DefaultLAWorkspace** in de lijst **Resource** en klik vervolgens op **Gereed**.

    ![Waarschuwing maken - stap 1](./media/log-analytics-tutorial-response/alert-rule-03.png)

4. Klik in de sectie **Waarschuwingscriteria** op **Criteria toevoegen** om de query te definiëren en geef vervolgens logica op die de waarschuwingsregel volgt. Selecteer in het deelvenster **Signaallogica configureren** de optie **Aangepast zoeken in logboeken** als signaalnaam en voer uw query in **Zoekquery** in.

    Bijvoorbeeld:
    ```
    Perf
    | where CounterName == "% Processor Time" and ObjectName == "Processor" and InstanceName == "_Total"
    | summarize AggregatedValue=avg(CounterValue) by bin(TimeGenerated, 1m)
    ```

    Het deelvenster wordt bijgewerkt met de configuratie-instellingen voor de waarschuwing.  Bovenaan ziet u de resultaten voor de afgelopen 30 minuten van het geselecteerde signaal.

5. Configureer de waarschuwing met de volgende gegevens:  
   a. Selecteer in de vervolgkeuzelijst *Gebaseerd op* de optie **Meting van metrische gegevens**.  Een meting van metrische gegevens maakt een waarschuwing voor elk object in de query met een waarde die de opgegeven drempelwaarde overschrijdt.  
   b. Selecteer in de lijst **Voorwaarde** de optie **Groter dan** en geef **90** op voor **Drempel**.  
   c. Selecteer in de sectie 'Waarschuwing activeren op basis van' de optie **Achtereenvolgende schendingen**, selecteer **Groter dan** in de vervolgkeuzelijst en voer een waarde in van 3.  
   d. Accepteer de standaardwaarden in de sectie Evaluatie op basis van. De regel wordt om de vijf minuten uitgevoerd en retourneert records die zijn gemaakt binnen dit tijdsbestek.  
6. Klik op **Gereed** om de waarschuwingsregel te voltooien.

    ![Waarschuwingssignaal configureren](./media/log-analytics-tutorial-response/alert-signal-logic-02.png)

7. Geef een naam op voor de waarschuwing in het veld **Naam waarschuwingsregel**, zoals **Percentage CPU groter dan 90 procent**.  Geef bij **Beschrijving** gedetailleerde informatie op over de waarschuwing en selecteer **Kritiek (ernst 0)** in de vervolgkeuzelijst **Ernst**.

    ![Details van waarschuwing configureren](./media/log-analytics-tutorial-response/alert-signal-logic-04.png)

8. Als u de waarschuwingsregel direct na het maken wilt activeren, accepteert u de standaardwaarde voor **Regel inschakelen wanneer deze wordt gemaakt**.  
9. De laatste stap bestaat uit het instellen van een **actiegroep**, die ervoor zorgt dat elke keer dat een waarschuwing wordt geactiveerd, dezelfde acties worden genomen. De groep kan worden gebruikt voor elke regel die u definieert.  Configureer een nieuwe actiegroep met de volgende gegevens:  
   a. Selecteer **Nieuwe actiegroep** om het deelvenster **Actiegroep toevoegen** weer te geven.  
   b. Geef voor **Naam van de actiegroep** een naam op zoals **IT Operations - waarschuwen** en voor **Korte naam** een waarde zoals **itops w**.  
   c. Controleer of de standaardwaarden voor **Abonnement** en **Resourcegroep** juist zijn. Als dat niet het geval is, selecteert u de juiste waarden in de vervolgkeuzelijst.  
   d. Geef in de sectie Acties een naam op voor de actie, zoals **E-mail verzenden**, en selecteer onder **Actietype** de optie **E-mail/Sms/Push/Spraak** in de vervolgkeuzelijst. Aan de rechterkant zie u het eigenschappendeelvenster **E-mail/Sms/Push/Spraak**, waarin u aanvullende gegevens kunt opgeven.  
   e. Schakel in het deelvenster **E-mail/Sms/Push/Spraak** de optie **E-mail** in en geef een geldig SMTP-e-mailadres op voor verzending van het bericht.  
   f. Klik op **OK** om uw wijzigingen op te slaan.  
       ![Nieuwe actiegroep maken](./media/log-analytics-tutorial-response/action-group-properties-01.png)

10. Klik op **OK** om de actiegroep te voltooien.
11. Klik op **Waarschuwingsregel maken** om de waarschuwingsregel te voltooien. Deze wordt onmiddellijk uitgevoerd.

    ![Nieuwe waarschuwingsregel voltooien](./media/log-analytics-tutorial-response/alert-rule-01.png)

## <a name="view-your-alerts-in-azure-portal"></a>Waarschuwingen bekijken in Azure Portal
U hebt nu een waarschuwing gemaakt en dus kunt u waarschuwingen van Azure weergeven in één deelvenster en daar alle waarschuwingsregels voor uw Azure-abonnementen beheren. U ziet een overzicht van alle waarschuwingsregels (ingeschakeld of uitgeschakeld) en u kunt sorteren op doelresources, resourcegroepen, regelnaam of status. Het venster bevat ook een samengesteld overzicht van alle geactiveerde waarschuwingen en het totale aantal geconfigureerde/ingeschakelde waarschuwingsregels.

![Statuspagina voor Azure-waarschuwingen](./media/log-analytics-tutorial-response/azure-alerts-02.png)

Als de waarschuwing wordt geactiveerd, ziet u dit in de tabel, evenals het aantal keren dat de waarschuwing binnen het geselecteerde tijdsbestek is geactiveerd (de standaardwaarde is de afgelopen zes uur).  Er moet ook een e-mailbericht in uw Postvak IN staan dat er ongeveer zo uitziet, met de virtuele machine waar het om gaat en de belangrijkste resultaten die de zoekopdracht in dit geval heeft opgeleverd.

![Voorbeeld van e-mail met waarschuwing](./media/log-analytics-tutorial-response/azure-alert-email-notification-01.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u geleerd hoe waarschuwingsregels een probleem proactief kunnen identificeren en kunnen reageren op een probleem wanneer deze met regelmatige tussenpozen logboekzoekopdrachten uitvoeren en aan bepaalde criteria voldoen.

Volg deze link om voorbeelden te zien van vooraf gemaakte Log Analytics-scripts.

> [!div class="nextstepaction"]
> [Voorbeelden van Log Analytics-scripts](powershell-samples.md)
