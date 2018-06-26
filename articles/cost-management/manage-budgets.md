---
title: Budgetten in Azure kosten Management beheren | Microsoft Docs
description: In dit artikel helpt u bij het maken en beheren van budgetten in kostenbeheer.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/25/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 9d6bf29909393846ec17a1bcc210fb989efd7f99
ms.sourcegitcommit: 828d8ef0ec47767d251355c2002ade13d1c162af
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/25/2018
ms.locfileid: "36938390"
---
# <a name="manage-budgets"></a>Budgetten beheren

Opzetten van budgetten en op basis van budget waarschuwingen help om uw cloud governance en accountability te verbeteren. In dit artikel helpt u snel budgetten maken en beheren in kostenbeheer te starten.

Wanneer u een Enterprise- of MSP-account hebt, kunt u de structuur van uw hiërarchische kosten entiteit maandelijkse budget quota's toewijzen aan verschillende bedrijfseenheden, afdelingen of een andere entiteit van de kosten. Wanneer u een Premium-account hebt, kunt u de functionaliteit voor het beheer van budget, die vervolgens wordt toegepast op uw uitgaven volledige in de cloud. Alle budgetten handmatig worden toegewezen.

Op basis van toegewezen budgetten, stelt u drempelwaarde voor waarschuwingen op basis van het percentage van uw budget die wordt gebruikt en de ernst van elke drempelwaarde definiëren.

De rapporten met het toegewezen budget budget. Gebruikers kunnen bekijken wanneer hun uitgaven boven, onder of op nominaal met hun verbruik gedurende een bepaalde periode. Wanneer u selecteert **velden weergeven/verbergen** aan de bovenkant van een budgetrapport u kosten, budget, samengevoegde kosten of totale budget kunt weergeven.

## <a name="create-budgets"></a>Budgetten maken

Wanneer u een budget maakt, moet u instellen dat deze voor het fiscale jaar en van toepassing op een specifieke entiteit.

Een budget maken en toewijzen aan een entiteit:

1. Navigeer naar **kosten** &gt; **kostenbeheer** &gt; **Budget**.
2. Op de pagina Budget Management onder **entiteiten**, selecteer de entiteit waar u wilt maken van het budget.
3. Selecteer in het budgetjaar het jaar waarin u wilt maken van het budget.
4. Stel een Budgetwaarde voor elke maand. Wanneer u bent klaar, klikt u op **opslaan**.
In dit voorbeeld is het maandelijkse budget voor juni 2018 ingesteld op $135.000. Het totale budget voor het jaar is $1,615,000.00.
![Een budget maken](./media/manage-budgets/set-budget.png)


Een bestand voor de jaarlijkse budget importeren:

1. Onder **acties**, selecteer **exporteren** voor het downloaden van een lege sjabloon CSV wilt gebruiken als uw basis voor het budget.
2. Vul in het CSV-bestand met uw budget-vermeldingen en lokaal opslaat.
3. Onder **acties**, selecteer **importeren**.
4. Selecteer uw opgeslagen bestand en klik op **OK**.

Het voltooide budget onder exporteren als een CSV-bestand **acties**, selecteer **exporteren** om het bestand te downloaden.

## <a name="view-budget-in-reports"></a>Weergave budget in rapporten

Wanneer voltooid, uw budget wordt weergegeven in de meeste Kostenrapporten onder **kosten** &gt; **kosten Analysis** en in de vs kosten. Rapport budget gedurende een periode. U kunt ook op basis van budgetdrempels met behulp van rapporten plannen **acties**.

Hier volgt een voorbeeld van het rapport analyse van de kosten. U ziet de totale budget en kosten op werkbelasting en gebruik typen sinds het begin van het jaar.

![Voorbeeld van de kosten-analyserapport met budget](./media/manage-budgets/cost-analysis-budget-example.png)

In dit voorbeeld wordt ervan uitgegaan dat de huidige datum valt 22 juni. De kosten voor juni 2018 is $71,611.28 vergeleken met het maandelijkse budget van fl 135.000. De kosten zijn veel lager zijn dan het maandelijkse budget omdat er nog acht dagen van uitgaven vóór het einde van de maand.

Het rapport weergeven op een andere manier is om te kijken naar samengevoegde kosten tegenover uw budget. Om te zien samengevoegde kosten onder **velden weergeven/verbergen**, selecteer **samengevoegde kosten** en **totaalbudget**. Hier volgt een voorbeeld van de samengevoegde kosten sinds het begin van het jaar.

![Samengevoegde budget](./media/manage-budgets/accumulated-budget.png)

Ergens in de toekomst de samengevoegde kosten, overschrijdt mogelijk uw budget. Kunt u eenvoudig kunt zien dat als u de diagramweergave voor wijzigt de _regel_ type.

![Budget wordt weergegeven in een lijndiagram](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Budget waarschuwingen voor een filter maken

In het vorige voorbeeld ziet u dat de samengevoegde kosten het budget nadert. U kunt automatische budget waarschuwingen maken zodat u een melding krijgen bent wanneer benaderingen uitgaven of uw budget overschrijdt. De waarschuwing wordt in feite, een gepland rapport met een drempelwaarde. Budget waarschuwingsdrempel voor de metrische gegevens behoren:

- Resterende kosten versus budget – om op te geven van een valuta waarde drempelwaarde
- Kosten percentage versus budget – een drempelwaarde voor percentage waarde opgeven

Bekijk een voorbeeld.

In de vs kosten. Budget op tijd rapport, klikt u op **acties** en selecteer vervolgens **rapport plant**. Selecteer een drempelwaarde metrische gegevens op het tabblad drempelwaarde. Bijvoorbeeld: **kosten percentage VS. budget**. Selecteer een Waarschuwingstype en voer een percentage van het budget. Als u de hoogte worden gebracht slechts één keer wilt, selecteert u **aantal opeenvolgende waarschuwingen** en typ vervolgens _1_. Klik op **Opslaan**.

![Budget waarschuwing](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Volgende stappen

- Als u nog niet de eerste zelfstudie hebt voltooid voor het beheer van kosten, leest u op het [gebruik en kosten bekijken](https://docs.microsoft.com/en-us/azure/cost-management/tutorial-review-usage).
- Meer informatie over de [rapporten beschikbaar zijn in kostenbeheer](use-reports.md).
