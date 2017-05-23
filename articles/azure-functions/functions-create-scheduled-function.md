---
title: Een functie maken in Azure die volgens een schema wordt uitgevoerd | Microsoft Docs
description: Ontdek hoe u in Azure een functie maakt die wordt uitgevoerd op basis van een schema dat u definieert.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: ba50ee47-58e0-4972-b67b-828f2dc48701
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: glenga
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: 1478a2eedad496d3113fef28920d10859d11b1ce
ms.contentlocale: nl-nl
ms.lasthandoff: 05/12/2017


---
# <a name="create-a-function-in-azure-that-is-triggered-by-a-timer"></a>Maak een functie in Azure die wordt geactiveerd door een timer

Ontdek hoe u Azure Functions gebruikt om een functie te maken die wordt uitgevoerd op basis van een schema dat u definieert.

![Functie-app maken in Azure Portal](./media/functions-create-scheduled-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Vereisten

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![De functie-app is gemaakt.](./media/functions-create-first-azure-function/function-app-create-success.png)

Vervolgens maakt u een functie in de nieuwe functie-app.

<a name="create-function"></a>

## <a name="create-a-timer-triggered-function"></a>Een door een timer geactiveerde functie maken

1. Vouw uw functie-app uit, klik op de knop **+** naast **Functies** en klik op de sjabloon **TimerTrigger** voor de gewenste taal. Gebruik vervolgens de instellingen zoals die in de tabel zijn opgegeven en klik op **Maken**:

| Instelling | Voorgestelde waarde | Beschrijving |
|---|---|---|
| **Een naam voor de functie opgeven** | TimerTriggerCSharp1 | Bepaalt de naam van de door de timer geactiveerde functie. |
| **[Planning](http://en.wikipedia.org/wiki/Cron#CRON_expression)** | 0 \*/1 \* \* \* \* | Een [CRON-expressie](http://en.wikipedia.org/wiki/Cron#CRON_expression) met zes velden aan de hand waarvan uw functie elke minuut wordt uitgevoerd. |

Er wordt een functie gemaakt in uw gekozen taal en deze wordt elke minuut uitgevoerd.

1. Controleer of dit correct wordt uitgevoerd door de traceringsinformatie die naar logboeken wordt geschreven te bekijken.

![De viewer voor functielogboeken in Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-view-logs2.png)

U kunt het schema voor de functie wijzigen zodat deze minder vaak wordt uitgevoerd (bijvoorbeeld één keer per uur). 

## <a name="update-the-timer-schedule"></a>Het timerschema bijwerken

1. Vouw de functie uit en klik op **Integreren**. Dit is waar u de invoer- en uitvoerbindingen voor de functie definieert en het schema instelt. 

2. Voer voor de nieuwe **Planning** een waarde van `0 0 */1 * * *` in en klik vervolgens op **Opslaan**.  

![Het timerschema voor het bijwerken van functies in Azure Portal.](./media/functions-create-scheduled-function/functions-timer-trigger-change-schedule.png)

U hebt nu een functie die één keer per uur wordt uitgevoerd. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie gemaakt die wordt uitgevoerd op basis van een schema.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie voor meer informatie over timeractiveringen [Schedule code execution with Azure Functions](functions-bindings-timer.md) (Code-uitvoering plannen met Azure Functions).
