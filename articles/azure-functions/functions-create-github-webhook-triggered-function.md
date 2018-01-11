---
title: Een functie in Azure maken die wordt geactiveerd door een GitHub-webhook | Microsoft Docs
description: Gebruik Azure Functions voor het maken van een functie zonder server die wordt aangeroepen door een GitHub-webhook.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: 36ef34b8-3729-4940-86d2-cb8e176fcc06
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/31/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: cdfb5db7b304a18d6945328abc0ca7ebf2f9ec6a
ms.sourcegitcommit: fa28ca091317eba4e55cef17766e72475bdd4c96
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2017
---
# <a name="create-a-function-triggered-by-a-github-webhook"></a>Een door een GitHub-webhook geactiveerde functie maken

Ontdek hoe u een functie maakt die wordt geactiveerd door een HTTP-webhookaanvraag met een specifieke GitHub-nettolading.

![Een door een GitHub-webhook geactiveerde functie in Azure Portal](./media/functions-create-github-webhook-triggered-function/function-app-in-portal-editor.png)

## <a name="prerequisites"></a>Vereisten

+ Een GitHub-account met ten minste één project.
+ Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

![De functie-app is gemaakt.](./media/functions-create-first-azure-function/function-app-create-success.png)

Vervolgens maakt u een functie in de nieuwe functie-app.

<a name="create-function"></a>

## <a name="create-a-github-webhook-triggered-function"></a>Een door een GitHub-webhook geactiveerde functie maken

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als dit de eerste functie in de functie-app is, selecteert u **Aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](./media/functions-create-github-webhook-triggered-function/add-first-function.png)

2. Typ `github` in het zoekveld en kies vervolgens de gewenste taal voor de trigger-sjabloon voor de GitHub-webhook. 

     ![De trigger-sjabloon voor de GitHub-webhook kiezen](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger.png) 

2. Typ een **Naam** voor de functie en selecteer vervolgens **Maken**. 

     ![Een door een GitHub-webhook geactiveerde functie configureren in Azure Portal](./media/functions-create-github-webhook-triggered-function/functions-create-github-webhook-trigger-2.png) 

3. Klik in de nieuwe functie op **</> Functie-URL ophalen**, kopieer de waarden en sla deze op. Doe hetzelfde voor **</> GitHub-geheim ophalen**. U hebt deze waarden nodig voor het configureren van de webhook in GitHub.

    ![De functiecode controleren](./media/functions-create-github-webhook-triggered-function/functions-copy-function-url-github-secret.png)

Vervolgens maakt u een webhook in uw GitHub-opslagplaats.

## <a name="configure-the-webhook"></a>De webhook configureren

1. Navigeer naar een van uw opslagplaatsen in GitHub. U kunt ook een opslagplaats gebruiken die u hebt gesplitst. Als u een opslagplaats moet splitsen, gebruikt u <https://github.com/Azure-Samples/functions-quickstart>.

1. Klik achtereenvolgens op **Instellingen**, **Webhooks** en **Webhook toevoegen**.

    ![Een GitHub-webhook toevoegen](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-2.png)

1. Gebruik de instellingen zoals die in de tabel zijn opgegeven en klik vervolgens op **Webhook toevoegen**.

    ![Webhook-URL en geheim instellen](./media/functions-create-github-webhook-triggered-function/functions-create-new-github-webhook-3.png)

| Instelling | Voorgestelde waarde | Beschrijving |
|---|---|---|
| **URL van de nettolading** | Gekopieerde waarde | Gebruik de waarde die wordt geretourneerd door **</> Functie-URL ophalen**. |
| **Geheim**   | Gekopieerde waarde | Gebruik de waarde die wordt geretourneerd door **</> GitHub-geheim ophalen**. |
| **Inhoudstype** | application/json | De functie verwacht een JSON-nettolading. |
| Gebeurtenistriggers | Ik wil afzonderlijke gebeurtenissen selecteren | We willen alleen activeren bij gebeurtenissen met een opmerking bij actie van het item.  |
| | Opmerking bij actie item |  |

Nu is de webhook zo geconfigureerd dat de functie wordt geactiveerd wanneer er een nieuwe probleemopmerking wordt toegevoegd.

## <a name="test-the-function"></a>De functie testen

1. Open in uw GitHub-opslagplaats het tabblad **Problemen** in een nieuw browservenster.

1. Klik in het nieuwe venster op **Nieuw probleem**, voer een titel in en klik op **Nieuw probleem verzenden**.

1. Typ een opmerking bij probleem en klik op **Opmerking**.

    ![Voeg een opmerking bij de actie van het item van GitHub toe.](./media/functions-create-github-webhook-triggered-function/functions-github-webhook-add-comment.png)

1. Ga terug naar de portal en bekijk de logboeken. Hier ziet u een traceervermelding met de tekst van de nieuwe opmerking.

     ![Bekijk de tekst van de opmerking in de logboeken.](./media/functions-create-github-webhook-triggered-function/function-app-view-logs.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie gemaakt die wordt uitgevoerd wanneer er een aanvraag wordt ontvangen van een GitHub-webhook.

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md) (Azure Functions-HTTP- en webhookbindingen) voor meer informatie over webhooktriggers.
