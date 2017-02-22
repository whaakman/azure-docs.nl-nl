---
title: Een webhook of API Azure Function maken | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie zonder server die wordt aangeroepen door een webhook of API-aanroep.
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
ms.date: 02/02/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: a8f6d111a010666bf4aaaf05e061381cc8fffed0
ms.openlocfilehash: 23a65319fe1825e2ba51f2fd5a2d0b65ca499472


---
# <a name="create-a-webhook-or-api-azure-function"></a>Een webhook of API Azure-functie maken
Azure Functions is een gebeurtenisafhankelijke, compute-on-demand service waarmee u geplande of geactiveerde code-eenheden kunt maken voor implementatie in diverse programmeertalen. Zie [Overzicht van Azure Functions](functions-overview.md) voor meer informatie.

In dit onderwerp wordt beschreven hoe u een JavaScript-functie maakt die wordt aangeroepen door een GitHub-webhook. De nieuwe functie is gemaakt op basis van een vooraf gedefinieerde sjabloon in de Azure Functions-portal. U kunt ook een korte video bekijken om te zien hoe deze stappen worden uitgevoerd in de portal.

De algemene stappen in deze zelfstudie kunnen ook worden gebruikt om een functie in C# of F# te maken in plaats van JavaScript. 

## <a name="watch-the-video"></a>Video bekijken
In de volgende video laten we zien hoe u de eenvoudige stappen in deze zelfstudie uitvoert 

>[!VIDEO https://channel9.msdn.com/Series/Windows-Azure-Web-Sites-Tutorials/Create-a-Web-Hook-or-API-Azure-Function/player]
>
>

## <a name="prerequisites"></a>Vereisten

Voor het voltooien van deze zelfstudie hebt u het volgende nodig:

+ Een actief Azure-account. Als u nog geen account hebt, kunt u zich [registreren voor een gratis Azure-account](https://azure.microsoft.com/free/).  
 U kunt ook de ervaring [Functies uitproberen](https://functions.azure.com/try) gebruiken om de zelfstudie te voltooien zonder Azure-account.
+ Een GitHub-account. U kunt zich [registreren voor een gratis GitHub-account](https://github.com/join) als u er nog geen hebt. 

## <a name="create-a-webhook-triggered-function-from-the-template"></a>Op basis van de sjabloon een functie maken die door een webhook wordt geactiveerd
Een functie-app fungeert als host voor de uitvoering van uw functies in Azure. 

1. Ga naar de [Azure Functions-portal](https://functions.azure.com/signin) en meld u aan met uw Azure-account.

2. Als u een bestaande functie-app hebt die u kunt gebruiken, selecteert u deze in **Uw functie-apps** en klikt u op **Openen**. Als u een functie-app wilt maken, typt u een unieke **Naam** voor uw nieuwe functie-app of accepteert u de gegenereerde naam, selecteert u de gewenste **Regio** en klikt u op **Maken + aan de slag**. 

3. Klik in uw functie-app op **+ Nieuwe functie** > **GitHub-webhook - JavaScript** > **Maken**. In deze stap wordt een functie gemaakt met de standaardnaam die is gebaseerd op de opgegeven sjabloon. U kunt ook een functie maken voor C# of F#.
   
    ![Een door een GitHub-webhook geactiveerde functie maken](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook.png) 

4. U ziet dat in **Ontwikkelen** het voorbeeld express.js-functie in het venster **Code** wordt weergegeven. Deze functie ontvangt een GitHub-aanvraag van een probleemopmerking webhook, registreert de probleemtekst en stuurt een antwoord naar de webhook als `New GitHub comment: <Your issue comment text>`.

    ![De functiecode controleren](./media/functions-create-a-web-hook-or-api-function/functions-new-webhook-in-portal.png) 

1. Kopieer de waarden van **Functie-URL** en **GitHub-geheim** en sla ze op. Deze waarden worden in het volgende gedeelte gebruikt om de webhook te configureren in GitHub. 

2. Klik op **Testen**, vermeld de vooraf gedefinieerde JSON-instantie van een probleemopmerking in de **aanvraagtekst** en klik vervolgens op **Uitvoeren**. 

    ![De webhookfunctie testen in de portal](./media/functions-create-a-web-hook-or-api-function/functions-test-webhook-in-portal.png)
   
    > [!NOTE]
    > U kunt een nieuwe sjabloongebaseerde functie altijd meteen testen in het tabblad **Ontwikkelen** door verwachte JSON-datatekst in te voeren en te klikken op **Uitvoeren**. In dit geval heeft de sjabloon een vooraf gedefinieerde tekst van een probleemopmerking. 

Vervolgens maakt u de werkelijke webhook in uw GitHub-opslagplaats.

## <a name="configure-the-webhook"></a>De webhook configureren
1. Navigeer naar een van uw opslagplaatsen in GitHub. U kunt ook opslagplaatsen gebruiken die u hebt gesplitst.
 
2. Klik op **Instellingen** > **Webhooks & services** > **Webhook toevoegen**.
   
    ![Een GitHub-webhook toevoegen](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-2.png)   

3. Plak de URL en het geheim van de functie in **URL voor nettolading** en **Geheim**, en selecteer **Toepassing/JSON** als **inhoudstype**.

4. Klik op **Ik wil afzonderlijke gebeurtenissen selecteren**, selecteer **Opmerking bij actie item** en klik op **Webhook toevoegen**.
   
    ![Webhook-URL en geheim instellen](./media/functions-create-a-web-hook-or-api-function/functions-create-new-github-webhook-3.png) 

Op dit moment is de GitHub-webhook geconfigureerd voor het activeren van de functie wanneer een nieuwe probleemopmerking wordt toegevoegd.  
Het is nu tijd om te testen.

## <a name="test-the-function"></a>De functie testen
1. Open in uw GitHub-repo het tabblad **Problemen** in een nieuw browservenster.

2. Klik in het nieuwe venster op **Nieuw probleem**, voer een titel in en klik op **Nieuw probleem verzenden**. U kunt ook een bestaand probleem openen.

2. Typ een opmerking bij probleem en klik op **Opmerking**. 

3. Klik in het andere GitHub-venster op **Bewerken** naast uw nieuwe webhook. Blader omlaag naar **Recente leveringen** en controleer of er een webhookaanvraag is verzonden en of de hoofdtekst van het antwoord `New GitHub comment: <Your issue comment text>` is.

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




<!--HONumber=Feb17_HO1-->


