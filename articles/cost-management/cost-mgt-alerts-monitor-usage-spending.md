---
title: Gebruik en uitgaven met kosten waarschuwingen controleren | Microsoft Docs
description: Dit artikel wordt beschreven hoe kosten help waarschuwingen bewaken van gebruik en uitgaven in Azure Cost Management.
services: cost-management
keywords: ''
author: bandersmsft
ms.author: banders
ms.date: 02/28/2019
ms.topic: conceptual
ms.service: cost-management
manager: alavital
ms.custom: ''
ms.openlocfilehash: d6f7a597ff5737bb5e9a3f07122ece313bd432ff
ms.sourcegitcommit: 8b41b86841456deea26b0941e8ae3fcdb2d5c1e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/05/2019
ms.locfileid: "57337770"
---
# <a name="use-cost-alerts-to-monitor-usage-and-spending"></a>Gebruik waarschuwingen van de kosten voor gebruik en uitgaven bewaken

Dit artikel helpt u begrijpen en gebruiken van Cost Management waarschuwingen voor het bewaken van uw Azure-gebruik en uitgaven. Kosten waarschuwingen worden automatisch gegenereerd op basis van wanneer de Azure-resources worden verbruikt. Waarschuwingen weergeven van alle actieve kostenbeheer en meldingen voor facturering samen op één plek. Wanneer het gebruik van een bepaalde drempelwaarde bereikt, worden waarschuwingen gegenereerd door Cost Management. Er zijn drie soorten kosten waarschuwingen: budget waarschuwingen, tegoed waarschuwingen en afdeling uitgaven quotum waarschuwingen.

## <a name="budget-alerts"></a>Budget waarschuwingen

Budget waarschuwingen een melding wanneer uitgaven, op basis van gebruik of kosten, is bereikt of hoger is dan de hoeveelheid die is gedefinieerd in de [waarschuwen voorwaarde van het budget](tutorial-acm-create-budgets.md). Budgetten voor kosten Management worden gemaakt met behulp van de Azure portal of de [Azure Consumption](https://docs.microsoft.com/rest/api/consumption) API.

In de Azure-portal, worden budgetten gedefinieerd door kosten. Met de Azure-verbruik-API, zijn budgetten gedefinieerd door de kosten of door hun verbruik. Budget waarschuwingen ondersteunt zowel op basis van kosten en gebruik op basis van budgetten. Budget waarschuwingen worden automatisch gegenereerd wanneer de waarschuwing budget-voorwaarden wordt voldaan. U kunt alle kosten waarschuwingen weergeven in Azure portal. Wanneer een waarschuwing wordt gegenereerd, wordt deze weergegeven in de kosten waarschuwingen. Een e-mailwaarschuwing is ook verzonden naar de mensen in de lijst met ontvangers van het budget.

## <a name="credit-alerts"></a>Tegoed waarschuwingen

Tegoed waarschuwingen een melding wanneer uw Azure-tegoed monetaire toezeggingen worden verbruikt. Monetaire toezeggingen zijn bedoeld voor organisaties met Enterprise-overeenkomsten. Tegoed waarschuwingen worden automatisch gegenereerd op 90% en 100% van uw Azure-tegoed. Wanneer een waarschuwing wordt gegenereerd, wordt deze weergegeven in kosten waarschuwingen en het e-mailbericht verzonden naar de eigenaars.

## <a name="department-spending-quota-alerts"></a>Afdeling bestedingslimiet quotum waarschuwingen

Afdeling bestedingslimiet quotum waarschuwingen een melding wanneer een vaste drempelwaarde van de quota voor afdeling bestedingslimiet bereikt. De bestedingslimiet quota worden geconfigureerd in de EA-portal. Wanneer een drempelwaarde wordt voldaan genereert een e-mailbericht voor eigenaars van de afdeling en wordt weergegeven in de kosten voor waarschuwingen. Bijvoorbeeld 50% of 75% van het quotum.

## <a name="supported-alert-features-by-offer-categories"></a>Waarschuwing functies ondersteund door categorieën van aanbieding

Ondersteuning voor waarschuwingstypen, is afhankelijk van het type Azure-account dat u hebt (aanbieding van Microsoft). De volgende tabel ziet u de waarschuwing functies die worden ondersteund door verschillende Microsoft-aanbiedingen. U vindt de volledige lijst met Microsoft-aanbiedingen [gegevens van kostenbeheer begrijpen](understand-cost-mgt-data.md).

| Waarschuwingstype | Enterprise Agreement | Microsoft-klantovereenkomst | Web direct/Pay-As-You-Go |
|---|---|---|---|
| Budget | ✔ | ✔ | ✔ |
| Tegoed | ✔ |✘ | ✘ |
| Bestedingsquotum van afdeling | ✔ | ✘ | ✘ |



## <a name="view-cost-alerts"></a>Waarschuwingen weergeven

Klik in Azure Portal, in de lijst met services, op **Cost Management en facturering**. Klik in de lijst onder **Cost Management**, selecteer **kosten waarschuwingen**.

![Van de voorbeeldafbeelding van waarschuwingen wordt weergegeven in Cost Management](./media/cost-mgt-alerts-monitor-usage-spending/budget-alerts-fullscreen.png)

Het totale aantal actieve en gesloten waarschuwingen wordt weergegeven op de pagina met waarschuwingen van kosten.

Alle waarschuwingen weergeven voor het type waarschuwing. Een budgetwaarschuwing toont de reden waarom het foutrapport is gegenereerd en de naam van het budget dat van toepassing op. Elke waarschuwing bevat de datum waarop het foutrapport is gegenereerd, de status en het bereik (abonnement of management group) die de waarschuwing is van toepassing op.

Kan de status omvat **active** en **gesloten**. Actieve status geeft aan dat de waarschuwing nog steeds relevant is. Gesloten status geeft aan dat iemand de waarschuwing als u deze niet meer relevant is gemarkeerd.

Selecteer een waarschuwing in de lijst om de details ervan weer te geven. Waarschuwingsdetails weergeven voor meer informatie over de waarschuwing. Budget waarschuwingen bevatten een koppeling naar het budget. Als een aanbeveling voor een budgetwaarschuwing beschikbaar is, wordt ook een koppeling naar de aanbeveling weergegeven. Budget, tegoed en afdeling bestedingslimiet quotum waarschuwingen hebt een koppeling om te analyseren in kostenanalyse waar u de kosten voor de scope van de waarschuwing kunt bekijken. Het volgende voorbeeld ziet uitgaven voor een afdeling met Waarschuwingsdetails.

![Voorbeeldafbeelding van de uitgaven voor een afdeling in die met de details van waarschuwing](./media/cost-mgt-alerts-monitor-usage-spending/dept-spending-selected-with-credits.png)

Wanneer u de details van een gesloten waarschuwing bekijkt, kunt u deze activeren als handmatige actie is vereist. In de volgende afbeelding ziet u een voorbeeld.

![Voorbeeld van de afbeelding die laat zien sluiten en opnieuw activeren opties](./media/cost-mgt-alerts-monitor-usage-spending/Dismiss-reactivate-options.png)

## <a name="see-also"></a>Zie ook

- Als u dit nog niet hebt al een budget gemaakt of waarschuwing voorwaarden voor een budget instellen, voert u de [maken en beheren van budgetten](tutorial-acm-create-budgets.md) zelfstudie.
