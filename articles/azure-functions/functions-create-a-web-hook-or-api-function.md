---
title: Een webhook of API Azure Function maken | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie die wordt opgeroepen door een WebHook of API-aanroep.
services: azure-functions
documentationcenter: na
author: ggailey777
manager: erikre
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: get-started-article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/30/2016
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 47a89987b65b442c7b489984a4fc139eb1b82758
ms.openlocfilehash: 8efde94c5771212b4549f10882a4e55739231d61


---
# <a name="create-a-webhook-or-api-azure-function"></a>Een webhook of API Azure-functie maken
Azure Functions is een gebeurtenisafhankelijke, compute-on-demand service waarmee u geplande of geactiveerde code-eenheden kunt maken voor implementatie in diverse programmeertalen. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

In dit onderwerp wordt beschreven hoe u een Node.js-functie maakt die wordt opgeroepen door een GitHub-webhook. De nieuwe functie is gemaakt op basis van een vooraf gedefinieerde sjabloon in de Azure Functions-portal. U kunt ook een korte video bekijken om te zien hoe deze stappen worden uitgevoerd in de portal.

## <a name="watch-the-video"></a>Video bekijken
In de volgende video laten we zien hoe u de eenvoudige stappen in deze zelfstudie uitvoert 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Op basis van de sjabloon een functie maken die door een webhook wordt geactiveerd
Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. Voordat u een functie kunt maken, moet u een actief Azure-account hebben. Als u nog geen Azure-account hebt, zijn er [gratis accounts beschikbaar](https://azure.microsoft.com/free/). 

1. Ga naar de [Azure Functions-portal](https://functions.azure.com/signin) en meld u aan met uw Azure-account.

2. Als u een bestaande functie-app hebt die u kunt gebruiken, selecteert u deze in **Uw functie-apps** en klikt u op **Openen**. Als u een functie-app wilt maken, typt u een unieke **Naam** voor uw nieuwe functie-app of accepteert u de gegenereerde naam, selecteert u de gewenste **Regio** en klikt u op **Maken + aan de slag**. 

3. Klik in uw functie-app op **+ nieuwe functie** > **GitHub Webhook - node** > **maken**. In deze stap wordt een functie gemaakt met de standaardnaam die is gebaseerd op de opgegeven sjabloon. 
   
    ![Een door een GitHub-webhook geactiveerde functie maken](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. U ziet dat in **Ontwikkelen** het voorbeeld express.js-functie in het venster **Code** wordt weergegeven. Deze functie ontvangt een GitHub-aanvraag van een probleemopmerking webhook, registreert de probleemtekst en stuurt een antwoord naar de webhook als `New GitHub comment: <Your issue comment text>`.

    ![De functiecode controleren](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. Kopieert de **functie-URL** en **GitHub-geheim** waarden. U hebt deze waarden nodig bij het maken van de webhook in GitHub. 

2. Schuif omlaag naar **Uitvoeren**, houd rekening met de vooraf gedefinieerde JSON-instantie van een probleemopmerking in de aanvraagtekst en klik vervolgens op **Uitvoeren**. 
   
    U kunt een nieuwe sjabloongebaseerde functie altijd meteen testen in het tabblad **Ontwikkelen** door verwachte JSON-datatekst in te voeren en te klikken op **Uitvoeren**. In dit geval heeft de sjabloon een vooraf gedefinieerde tekst van een probleemopmerking. 

Vervolgens maakt u de werkelijke webhook in uw GitHub-opslagplaats.

## <a name="configure-the-webhook"></a>De webhook configureren
1. Navigeer naar een van uw opslagplaatsen in GitHub. U kunt ook opslagplaatsen gebruiken die u hebt gesplitst.
 
2. Klik op **Instellingen** > **Webhooks & services** > **Webhook toevoegen**.
   
    ![Een GitHub-webhook toevoegen](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   
3. Plak uw functie-URL en geheim in **Nettolading URL** en **Geheim**, klik vervolgens op **Ik wil afzonderlijke gebeurtenissen selecteren**, selecteer **Probleemopmerking** en klik op **Webhook toevoegen**.
   
    ![Webhook-URL en geheim instellen](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Op dit moment is de GitHub-webhook geconfigureerd voor het activeren van de functie wanneer een nieuwe probleemopmerking wordt toegevoegd.  
Het is nu tijd om te testen.

## <a name="test-the-function"></a>De functie testen
1. Open in uw GitHub-repo het tabblad **Problemen** in een nieuw browservenster en klik op **Nieuw probleem**, typ een titel en klik vervolgens op **Nieuw probleem registreren**. U kunt ook een bestaand probleem openen.

2. Typ een opmerking bij probleem en klik op **Opmerking**. Op dit moment kunt u teruggaan naar uw nieuwe webhook in GitHub en ziet u onder **Recente leveringen** dat een webhookaanvraag is verzonden en dat de antwoordtekst `New GitHub comment: <Your issue comment text>` is.

3. Blader in de Functions-portal omlaag naar de logs en ga na of de functie werd geactiveerd en de waarde `New GitHub comment: <Your issue comment text>` wordt geschreven naar de streaminglogboeken.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg de volgende onderwerpen voor meer informatie over Azure Functions.

* [Naslaginformatie over Azure Functions voor ontwikkelaars](functions-reference.md)  
  Naslaginformatie voor programmeurs over het coderen van functies.
* [Azure Functions testen](functions-test-a-function.md)  
  Beschrijft de verschillende hulpprogramma’s en technieken voor het testen van uw functies.
* [Azure Functions schalen](functions-scale.md)  
  Beschrijft de serviceabonnementen die beschikbaar zijn voor Azure Functions, zoals het hostingabonnement Consumption, en helpt u bij het kiezen van het juiste abonnement.  

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]




<!--HONumber=Nov16_HO4-->


