---
title: Uw eerste Azure-functie maken | Microsoft Docs
description: Bouw uw eerste Azure-functie, een toepassing zonder server, in minder dan twee minuten.
services: functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 4a1669e7-233e-4ea2-9b83-b8624f2dbe59
ms.service: functions
ms.devlang: multiple
ms.topic: hero-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 03/14/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: afe143848fae473d08dd33a3df4ab4ed92b731fa
ms.openlocfilehash: a797910e286cd2aacf5a8aa6c509e2b0f5f60276
ms.lasthandoff: 03/17/2017


---
# <a name="create-your-first-azure-function"></a>Uw eerste Azure-functie maken

In dit onderwerp wordt beschreven hoe u de snelstartgids voor Azure Functions in de portal kunt gebruiken voor het maken van een eenvoudige 'hallo wereld'-functie die wordt opgeroepen met een HTTP-aanvraag. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

U dient een Azure-account te hebben voordat u begint. Er zijn [gratis accounts](https://azure.microsoft.com/free/) beschikbaar. U kunt ook [Azure Functions proberen](https://azure.microsoft.com/try/app-service/functions/) zonder te registreren bij Azure.

## <a name="create-a-function-from-the-portal-quickstart"></a>Een functie maken vanuit de snelstartgids voor de portal

1. Ga naar de [Azure Functions-portal](https://functions.azure.com/signin) en meld u aan met uw Azure-account. 
 
2. Typ een unieke **Naam** voor uw nieuwe functie-app of accepteer de automatisch gegenereerde naam, selecteer de gewenste **Regio** en klik op **Maken en aan de slag**. Een geldige naam bevat alleen kleine letters, cijfers en afbreekstreepjes. Het onderstrepingsteken (**_**) is niet toegestaan.

3. Klik in het tabblad **Snelstartgids** op **WebHook en API** en kies een taal voor uw functie. Klik vervolgens op **Een functie maken**. Er wordt een nieuwe, vooraf gedefinieerde functie gemaakt in uw gekozen taal. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)

4. (Optioneel) Op dit punt in de snelstartgids kunt u ervoor kiezen om een rondleiding te volgen over de Azure Functions-functionaliteit in de portal. Nadat u de rondleiding hebt voltooid of overgeslagen, kunt u de nieuwe functie testen door een HTTP-aanvraag te verzenden.

## <a name="test-the-function"></a>De functie testen
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="watch-the-video"></a>Video bekijken
In de volgende video laten we zien hoe u de eenvoudige stappen in deze zelfstudie uitvoert. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 


## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


