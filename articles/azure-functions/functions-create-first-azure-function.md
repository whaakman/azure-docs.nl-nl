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
ms.date: 09/08/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: e446766c0f3d19a8cce585d708e8e7a809593408


---
# <a name="create-your-first-azure-function"></a>Uw eerste Azure-functie maken
## <a name="overview"></a>Overzicht
Azure Functions is een gebeurtenisafhankelijke, compute-on-demand-ervaring die het bestaande Azure-toepassingsplatform uitbreidt met mogelijkheden voor het implementeren van code die wordt geactiveerd door gebeurtenissen in andere Azure-services, SaaS-producten en on-premises systemen. Met Azure Functions schalen uw toepassingen mee met uw behoeften en betaalt u alleen voor de resources die u gebruikt. U kunt Azure Functions gebruiken om geplande of geactiveerde code-eenheden te implementeren in diverse programmeertalen. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

In dit onderwerp wordt beschreven hoe u de snelstartgids voor Azure Functions in de portal kunt gebruiken voor het maken van een eenvoudige ‘hallo wereld’ Node.js-functie die wordt opgeroepen met een HTTP-trigger. U kunt ook een korte video bekijken om te zien hoe deze stappen worden uitgevoerd in de portal.

## <a name="watch-the-video"></a>Video bekijken
In de volgende video laten we zien hoe u de eenvoudige stappen in deze zelfstudie uitvoert. 

> [!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-your-first-Azure-Function-simple/player]
> 
> 

## <a name="create-a-function-from-the-quickstart"></a>Een functie maken vanuit de snelstartgids
Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Volg deze stappen om een nieuwe functie-app en de nieuwe functie te maken. De nieuwe functie-app wordt gemaakt met een standaardconfiguratie. Zie [de andere snelstartzelfstudie voor het maken van Azure Functions](functions-create-first-azure-function-azure-portal.md) voor een expliciet voorbeeld van hoe u de functie-app maakt.

Voordat u uw eerste functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/).

1. Ga naar de [Azure Functions-portal](https://functions.azure.com/signin) en meld u aan met uw Azure-account.
2. Typ een unieke **Naam** voor uw nieuwe functie-app of accepteer de gegenereerde naam, selecteer de gewenste **Regio** en klik op **Create + get started**. 
3. Klik in het tabblad **Snelstartgids** op **WebHook en API** en op **JavaScript** en klik vervolgens op **Een functie maken**. Er wordt een nieuwe, vooraf gedefinieerde Node.js-functie gemaakt. 
   
    ![](./media/functions-create-first-azure-function/function-app-quickstart-node-webhook.png)
4. (Optioneel) Op dit punt in de snelstartgids kunt u ervoor kiezen om een rondleiding te volgen over de Azure Functions-functionaliteit in de portal.    Wanneer u de rondleiding hebt voltooid of overgeslagen, kunt u de nieuwe functie testen met de HTTP-trigger.

## <a name="test-the-function"></a>De functie testen
Omdat de Azure Functions-snelstartgidsen functionele code bevatten, kunt u uw nieuwe functie meteen testen.

1. Ga naar het tabblad **Develop** en bekijk het venster **Code**. U zult zien dat deze Node.js-code een HTTP-aanvraag verwacht met een waarde *name* die wordt doorgegeven in de berichttekst of in een queryreeks. Wanneer de functie wordt uitgevoerd, wordt deze waarde in het antwoordbericht geretourneerd.
   
    ![](./media/functions-create-first-azure-function/function-app-develop-tab-testing.png)
2. Blader omlaag naar het tekstvak **Request body**, wijzig de waarde van de eigenschap *name* in uw naam en klik op **Run**. De uitvoering wordt geactiveerd door een HTTP-testaanvraag, informatie wordt geschreven naar de streaminglogboeken en het antwoord ‘hallo’ wordt weergegeven in de **Output**. 
3. Om de uitvoering van dezelfde functie te activeren vanuit een ander browservenster of tabblad, kopieert u de waarde **Function URL** in het tabblad **Develop** en plakt u die in de adresbalk van een browser. Voeg de queryreekswaarde `&name=yourname` toe en druk op enter. Dezelfde informatie wordt naar de logboeken geschreven en hetzelfde antwoord ‘hallo’ wordt in de browser weergegeven.

## <a name="next-steps"></a>Volgende stappen
Deze snelstartgids demonstreert een zeer eenvoudige uitvoering van een basisfunctie die door HTTP wordt geactiveerd. Zie de volgende onderwerpen voor meer informatie over het gebruik van de mogelijkheden van Azure Functions in uw apps.

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies en het definiëren van triggers en bindingen.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
* [Azure Functions schalen](functions-scale.md)  
  Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het serviceabonnement Dynamic, en helpt u bij het kiezen van het juiste abonnement. 
* [Wat is Azure App Service?](../app-service/app-service-value-prop-what-is.md)  
  Azure Functions maakt gebruik van het Azure App Service-platform voor kernfunctionaliteit zoals implementaties, omgevingsvariabelen en diagnostische procedures. 

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO2-->


