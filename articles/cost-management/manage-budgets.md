---
title: Budgetten in Azure Cost Management beheren | Microsoft Docs
description: Dit artikel helpt u bij het maken en beheren van budgetten in Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 06/25/2018
ms.topic: conceptual
ms.service: cost-management
manager: dougeby
ms.custom: ''
ms.openlocfilehash: 106e8f082d148ed9a8c58313177be81ee074a2c3
ms.sourcegitcommit: e2ea404126bdd990570b4417794d63367a417856
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45578460"
---
# <a name="manage-budgets"></a>Budgetten beheren

Opzetten van budgetten en budget gebaseerde waarschuwingen helpen bij het verbeteren van uw cloud-governance en aansprakelijkheid. Dit artikel helpt u snel maken van budgetten en beginnen met het ze in Cost Management beheren.

Wanneer u een Enterprise- of MSP-account hebt, kunt u uw hiërarchische kostenstructuur entiteit maandelijkse budget quota toewijzen aan verschillende bedrijfseenheden, afdelingen of een andere kostenentiteit. Wanneer u een Premium-account hebt, kunt u de functionaliteit voor het beheer van budget, dit vervolgens op uw uitgaven volledige in de cloud toegepast wordt. Alle budgetten worden handmatig toegewezen.

Op basis van toegewezen budgetten, kunt u drempelwaarde voor waarschuwingen op basis van uw budget dat wordt gebruikt voor het percentage instellen en definiëren van de ernst van elke drempelwaarde.

De rapporten met het toegewezen budget budget. Gebruikers kunnen bekijken als hun uitgaven boven, onder of ervoor met hun gebruik na verloop van tijd is. Wanneer u selecteert **velden weergeven/verbergen** aan de bovenkant van een budgetrapport, kunt u kosten, budget, totale kosten of totaal budget weergeven.

## <a name="create-budgets"></a>Budgetten maken

Wanneer u een budget maakt, stelt u deze voor het fiscale jaar en van toepassing op een bepaalde entiteit.

Maken van een budget en wijs deze toe aan een entiteit:

1. Navigeer naar **kosten** &gt; **Cost Management** &gt; **Budget**.
2. Op de pagina budgetbeheer onder **entiteiten**, selecteer de entiteit waarin u wilt maken van het budget.
3. Selecteer in het budgetjaar, het jaar waarin u wilt maken van het budget.
4. Stel de Budgetwaarde van een voor elke maand. Wanneer u klaar bent, klikt u op **opslaan**.
In dit voorbeeld worden de maandelijkse budget voor juni 2018 is ingesteld op $135.000. Het totale budget voor het jaar is $1,615,000.00.
![Maak een budget](./media/manage-budgets/set-budget.png)


Voor het importeren van een bestand voor de jaarlijkse budget:

1. Onder **acties**, selecteer **exporteren** voor het downloaden van een lege sjabloon CSV moet worden gebruikt als de basis voor het budget.
2. Vul in het CSV-bestand met uw budget-fragmenten en lokaal opslaan.
3. Onder **acties**, selecteer **importeren**.
4. Selecteer uw opgeslagen bestand en klik vervolgens op **OK**.

Uw voltooide budget onder exporteren als een CSV-bestand, **acties**, selecteer **exporteren** om het bestand te downloaden.

## <a name="view-budget-in-reports"></a>Weergave budget in rapporten

Wanneer dit is voltooid, uw budget wordt weergegeven in de meeste Kostenrapporten onder **kosten** &gt; **Cost Analysis** en in de kosten voor Visual Studio. Rapport van budget gedurende een periode. U kunt ook op basis van budgetdrempels met behulp van rapporten plannen **acties**.

Hier volgt een voorbeeld van het rapport Cost Analysis. Het toont de totale budget en kosten door de typen werkbelasting en het gebruik sinds het begin van het jaar.

![Voorbeeld van een rapport Cost Analysis met budget](./media/manage-budgets/cost-analysis-budget-example.png)

In dit voorbeeld wordt ervan uitgegaan dat de huidige datum is 22 juni. De kosten voor juni 2018 zijn $71,611.28 vergeleken met het maandelijkse budget van $135.000. De kosten is veel lager dan het maandelijkse budget omdat er nog steeds acht dagen aan vóór het einde van de maand.

Er is een andere manier om het rapport weer te geven om te kijken naar vs van de totale kosten uw budget. Om te zien samengevoegde kosten onder **velden weergeven/verbergen**, selecteer **totale kosten** en **totaalbudget**. Hier volgt een voorbeeld van de totale kosten sinds het begin van het jaar.

![Totale budget](./media/manage-budgets/accumulated-budget.png)

Enige tijd in de toekomst uw totale kosten kan groter zijn dan uw budget. Kunt u eenvoudig kunt zien dat als u de diagramweergave voor wijzigt de _regel_ type.

![Budget wordt weergegeven in het lijndiagram weer te geven](./media/manage-budgets/budget-line.png)

## <a name="create-budget-alerts-for-a-filter"></a>Budget waarschuwingen voor een filter maken

In het vorige voorbeeld, kunt u zien dat de totale kosten het budget nadert. U kunt automatische budget waarschuwingen kunt maken, zodat u een bericht weergegeven wanneer de uitgaven van methoden of uw budget overschrijdt. De waarschuwing is in feite, een gepland rapport met een drempelwaarde. Budget waarschuwingsdrempel voor de metrische gegevens zijn onder andere:

- Resterende kosten versus budget – om op te geven van een valuta waarde drempelwaarde
- Kosten percentage versus budget – om op te geven van een drempelwaarde voor percentage waarde

We bekijken een voorbeeld.

In de kosten voor Visual Studio. Op tijd rapport budget, klikt u op **acties** en selecteer vervolgens **rapport plant**. Selecteer een drempelwaarde voor metrische gegevens op het tabblad drempelwaarde. Bijvoorbeeld, **Cost percentage vs budget**. Selecteer een type waarschuwing en voer een percentage van het budget. Als u wilt om slechts één keer te worden geïnformeerd, selecteer **aantal achtereenvolgende waarschuwingen** en typ vervolgens _1_. Klik op **Opslaan**.

![Budgetwaarschuwing](./media/manage-budgets/budget-alert.png)

## <a name="next-steps"></a>Volgende stappen

- Als u dit nog niet hebt al de eerste zelfstudie voor Cost Management voltooid, leest u via [gebruik en kosten bekijken](https://docs.microsoft.com/azure/cost-management/tutorial-review-usage).
- Meer informatie over de [rapporten die beschikbaar zijn in Cost Management](use-reports.md).
