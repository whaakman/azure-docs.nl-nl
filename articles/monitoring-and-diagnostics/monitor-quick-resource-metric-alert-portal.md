---
title: Een melding ontvangen wanneer er een voorwaarde voldoet aan de metrische waarde voor een | Microsoft Docs
description: Een Quick Start-handleiding voor gebruikers die een metriek maken voor een logische App
author: anirudhcavale
manager: orenr
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.service: monitoring-and-diagnostics
ms.topic: quickstart
ms.date: 09/25/2017
ms.author: ancav
ms.custom: mvc
ms.openlocfilehash: 08d63d47a99bdf9480299a74634bc0e9a09e691e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="receive-a-notification-when-a-metric-value-meets-a-condition"></a>Een melding ontvangen wanneer een metrische waarde aan een voorwaarde voldoet.

Azure Monitor worden metrische gegevens beschikbaar gemaakt voor veel Azure-resources. Deze metrische gegevens overbrengen de prestaties en de status van deze bronnen. In veel gevallen metriek kunnen waarden verwijzen naar iets mis met een resource wordt. U kunt metrische waarschuwingen om te controleren voor abnormaal gedrag en de hoogte gesteld als dit gebeurt maken. Deze snelstartgids stappen bij het maken van een logische App, een taak maken en visualiseren van de metrische gegevens voor de logische app. Vervolgens gaat het bij het maken van een waarschuwing en ontvangt een melding voor een waarde voor de logische App-resource.

Zie voor meer informatie over metrische gegevens en waarschuwingen metrische [Azure Monitor metrische gegevens overzicht](./monitoring-overview-metrics.md) en [overzicht van Azure Monitor waarschuwingen](./monitoring-overview-alerts.md). 

Als u nog geen Azure-abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/) voordat u begint.

## <a name="sign-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal

Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-a-logic-app"></a>Een logica-app maken

1. Klik op de knop **Nieuw** in de linkerbovenhoek van Azure Portal.

2. Zoek en selecteer **logische App**. Maak een nieuwe resourcegroep met de naam **myResourceGroup** gebruiken de standaardlocatie. Klik op de knop **Maken**.

3. Geeft informatie over de logic app en controleert u de **vastmaken aan Dashboard** optie. Klik op **Maken** wanneer u klaar bent.

    ![Geef algemene informatie over uw logische app in de portal](./media/monitoring-quick-resource-metric-alert-portal/create-logic-app-portal.png)  


4. De logische app moet worden vastgemaakt aan uw dashboard. Navigeer naar de logische app door erop te klikken.

5. Selecteer in het deelvenster logische App de **Logic App-ontwerper**

     ![Een trigger terugkeerpatroon gemaakt in de ontwerpfunctie voor logic app in de portal Configuratiescherm](./media/monitoring-quick-resource-metric-alert-portal/logic-app-designer.png)  

6. Stel u waarden zoals te zien is in het volgende diagram.

    ![De logica voor app trigger in het deelvenster portal configureren](./media/monitoring-quick-resource-metric-alert-portal/create-logic-app-triggers.png). 

7. Selecteer in de ontwerpfunctie voor de **terugkeerpatroon** trigger.

8. Stel een interval van 20 en een frequentie van de tweede om te controleren of dat uw logische app elke 20 seconden wordt geactiveerd.

9. Klik op de **nieuwe stap** en selecteer **een actie toevoegen**.

10. Kies de **HTTP** optie en selecteer **HTTP-HTTP-**.

11. Stel de **methode** als POST en de **Uri** aan een webadres van uw keuze.

12. Klik op **Opslaan**.

## <a name="view-metrics-for-your-logic-app"></a>Metrische gegevens voor uw logische app weergeven

1. Klik op de **Monitor** optie in het navigatiedeelvenster links.

2. Selecteer de **metrische gegevens** tabblad, vult u de **abonnement**, **resourcegroep**, **brontype** en **Resource** informatie voor uw logische app.

3. Kies in de lijst van metrische gegevens **wordt uitgevoerd gestart**.

4. Wijzig de **tijdsbereik** van de grafiek moet worden gegevens weergegeven voor het afgelopen uur.

5. U ziet nu een grafiek uitzetten van het totale aantal uw logische app is gestart via het afgelopen uur wordt uitgevoerd.

    ![Een grafiek metrische gegevens voor de resource voor de logische app uitzetten](./media/monitoring-quick-resource-metric-alert-portal/logic-app-metric-chart.png)

## <a name="create-a-metric-alert-for-your-logic-app"></a>Een metriek waarschuwing voor uw logische app maken

1.  Klik in het bovenste rechts deel van het paneel metrische gegevens op de **metrische waarschuwing toevoegen** knop.

2. Naam van de metrische waarschuwing 'myLogicAppAlert' en geef een korte beschrijving voor de waarschuwing.

3. Instellen de **voorwaarde** voor de metrische waarschuwing als 'Die groter is dan' zijn, stel de **drempelwaarde** als "10" en stel de **periode** als 'in de afgelopen 5 minuten'.

4. Ten slotte onder **aanvullende beheerder email(s)** uw e-mailadres invoeren. Deze waarschuwing zorgt ervoor dat u een e-mailbericht ontvangen in het geval van uw logische app heeft meer dan 10 mislukte wordt uitgevoerd binnen een periode van 5 minuten.

    ![De logische app waarschuwing in het deelvenster portal configureren](./media/monitoring-quick-resource-metric-alert-portal/logic-app-metrics-alert-portal.png)

## <a name="receive-metric-alert-notifications-for-your-logic-app"></a>Metrische waarschuwingsmeldingen voor uw logische app ontvangen
1. U ontvangt een e-mailbericht van 'Microsoft Azure-waarschuwingen' om aan de waarschuwing is 'geactiveerd' binnen enkele ogenblikken.

2. Ga terug naar uw logische app en wijzig de trigger terugkeerpatroon een interval van 1 en de frequentie van uur.

3. U ontvangt binnen een paar minuten een e-mailbericht van 'Microsoft Azure-waarschuwingen' geïnformeerd wanneer de waarschuwing is 'opgelost'.

## <a name="clean-up-resources"></a>Resources opschonen

Andere snel aan de slag in deze verzameling bouwen voort op deze snelstartgids. Als u van plan bent om door te gaan op om te werken met de volgende snel aan de slag of met de zelfstudies, geen clean up maakt van de resources in deze snelstartgids hebt gemaakt. Als u niet wilt doorgaan, gebruikt u de volgende stappen om alle resources te verwijderen die tijdens deze Quick Start in Azure Portal zijn gemaakt.

1. Klik in het menu links in de Azure portal op **Monitor**.

2. Selecteer de **waarschuwingen** tabblad, vinden de waarschuwing die u in deze snelstartgids hebt gemaakt en klikt u op.

3. Klik in het deelvenster voor metrische waarschuwing **verwijderen**.

4. Zoek in het menu links in de Azure portal, **logische App** en klik vervolgens op **Logic apps**.

5. In het deelvenster op de logische app die u hebt gemaakt in deze snelstartgids in het tekstvak en klik op **verwijderen**.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geleerd hoe een metrische waarschuwing voor uw resources maken. Voor meer informatie over metrische waarschuwingen doorklikken naar onze overzicht van waarschuwingen.

> [!div class="nextstepaction"]
> [Azure-abonnement actie-waarschuwingen](./monitor-quick-audit-notify-action-in-subscription.md )
