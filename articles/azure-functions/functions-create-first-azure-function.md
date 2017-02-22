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
ms.date: 02/09/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 7743bfb98552f7fa2334d8daa6a6f6935969f393
ms.openlocfilehash: d76630e0be4a021720d88e6d7e64cf2258f84266


---
# <a name="create-your-first-azure-function"></a>Uw eerste Azure-functie maken
## <a name="overview"></a>Overzicht
Azure Functions is een gebeurtenisafhankelijke, compute-on-demand-ervaring die het bestaande Azure-toepassingsplatform uitbreidt met mogelijkheden voor het implementeren van code die wordt geactiveerd door gebeurtenissen in andere Azure-services, SaaS-producten en on-premises systemen. Met Azure Functions schalen uw toepassingen mee met uw behoeften en betaalt u alleen voor de resources die u gebruikt. Met Azure Functions kunt u geplande of geactiveerde code-eenheden implementeren in diverse programmeertalen. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

In dit onderwerp wordt beschreven hoe u de snelstartgids voor Azure Functions in de portal kunt gebruiken voor het maken van een eenvoudige 'hallo wereld'-JavaScript-functie die wordt aangeroepen met een HTTP-trigger. U kunt ook een korte video bekijken om te zien hoe deze stappen worden uitgevoerd in de portal.

## <a name="watch-the-video"></a>Video bekijken
In de volgende video laten we zien hoe u de eenvoudige stappen in deze zelfstudie uitvoert. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Een functie maken vanuit de snelstartgids
Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Volg deze stappen om een functie-app bij de nieuwe functie te maken. De functie-app wordt gemaakt met een standaardconfiguratie. Zie [de andere snelstartzelfstudie voor het maken van Azure Functions](functions-create-first-azure-function-azure-portal.md) voor een expliciet voorbeeld van hoe u de functie-app maakt.

Voordat u uw eerste functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/).

1. Ga naar de [Azure Functions-portal](https://functions.azure.com/signin) en meld u aan met uw Azure-account.
2. Typ een unieke **Naam** voor uw nieuwe functie-app of accepteer de automatisch gegenereerde naam, selecteer de gewenste **Regio** en klik op **Maken en aan de slag**. U moet een geldige naam invoeren die alleen letters, cijfers en afbreekstreepjes bevat. Het onderstrepingsteken (**_**) is niet toegestaan.
3. Klik in het tabblad **Snelstartgids** op **WebHook en API** en op **JavaScript** en klik vervolgens op **Een functie maken**. Er wordt een nieuwe, vooraf gedefinieerde JavaScript-functie gemaakt. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Optioneel) Op dit punt in de snelstartgids kunt u ervoor kiezen om een rondleiding te volgen over de Azure Functions-functionaliteit in de portal. Nadat u de rondleiding hebt voltooid of overgeslagen, kunt u de nieuwe functie testen met de HTTP-trigger.

## <a name="test-the-function"></a>De functie testen
[!INCLUDE [Functions quickstart test](../../includes/functions-quickstart-test.md)]

## <a name="next-steps"></a>Volgende stappen
[!INCLUDE [Functions quickstart next steps](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Feb17_HO2-->


