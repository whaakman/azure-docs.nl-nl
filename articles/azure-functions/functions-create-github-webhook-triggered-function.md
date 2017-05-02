---
title: Een functie in Azure maken die wordt geactiveerd door een GitHub-webhook | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie zonder server die wordt aangeroepen door een GitHub-webhook.
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
ms.date: 04/18/2017
ms.author: glenga
translationtype: Human Translation
ms.sourcegitcommit: 9eafbc2ffc3319cbca9d8933235f87964a98f588
ms.openlocfilehash: d4354546f3342d65353a86a4cec7d02547ab92e7
ms.lasthandoff: 04/22/2017


---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Een door een GitHub-webhook geactiveerde functie maken

Informatie over het maken van een functie die wordt geactiveerd door een GitHub-webhook. 

![Functie-app maken in Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

Voor dit onderwerp zijn de resources vereist die zijn gemaakt in het onderwerp [Create your first function from the Azure portal](functions-create-first-azure-function.md) (Uw eerste functie maken vanuit Azure Portal).

U hebt ook een GitHub-account nodig. U kunt zich [registreren voor een gratis GitHub-account](https://github.com/join) als u er nog geen hebt. 

Het doorlopen van de stappen in dit onderwerp kost u minder dan vijf minuten.

## <a name="find-your-function-app"></a>Uw functie-app zoeken    

1. Meld u aan bij [Azure Portal](https://portal.azure.com/). 

2. Typ de naam van uw functie-app in de zoekbalk boven in de portal en selecteer deze in de lijst.

## <a name="create-function"></a>Een door een GitHub-webhook geactiveerde functie maken

1. Klik in uw functie-app op de knop **+** naast **Functies**, klik op de sjabloon **GitHubWebHook** voor de gewenste taal en klik op **Maken**.
   
    ![Maak een door een GitHub-webhook geactiveerde functie in Azure Portal.](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. Klik op **</> Functie-URL ophalen**, kopieer de waarden en sla deze op. Doe hetzelfde voor **</> GitHub-geheim ophalen**. U hebt deze waarden nodig voor het configureren van de webhook in GitHub. 

    ![De functiecode controleren](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png) 
         
Vervolgens maakt u een webhook in uw GitHub-opslagplaats. 

## <a name="configure-the-webhook"></a>De webhook configureren
1. Navigeer naar een van uw opslagplaatsen in GitHub. U kunt ook een opslagplaats gebruiken die u hebt gesplitst.
 
2. Klik achtereenvolgens op **Instellingen**, **Webhooks** en **Webhook toevoegen**.
   
    ![Een GitHub-webhook toevoegen](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

3. Plak de URL en het geheim van de functie in **URL voor nettolading** en **Geheim**, en selecteer **Toepassing/JSON** als **inhoudstype**.

4. Klik op **Ik wil afzonderlijke gebeurtenissen selecteren**, selecteer **Opmerking bij actie item** en klik op **Webhook toevoegen**.
   
    ![Webhook-URL en geheim instellen](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

Nu is de webhook zo geconfigureerd dat de functie wordt geactiveerd wanneer er een nieuwe probleemopmerking wordt toegevoegd. 

## <a name="test-the-function"></a>De functie testen
1. Open in uw GitHub-opslagplaats het tabblad **Problemen** in een nieuw browservenster.

2. Klik in het nieuwe venster op **Nieuw probleem**, voer een titel in en klik op **Nieuw probleem verzenden**. 

2. Typ een opmerking bij probleem en klik op **Opmerking**. 

3. Klik in het andere GitHub-venster op **Bewerken** naast uw nieuwe webhook. Blader omlaag naar **Recente leveringen** en controleer of er een webhookaanvraag is verwerkt door uw functie. 
 
    ![Webhook-URL en geheim instellen](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-triggered.png)

   Het antwoord van de functie zou `New GitHub comment: <Your issue comment text>` moeten bevatten.

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

[!INCLUDE [Getting Started Note](../../includes/functions-get-help.md)]


