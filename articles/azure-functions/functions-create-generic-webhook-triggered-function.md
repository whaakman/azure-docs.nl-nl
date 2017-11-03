---
title: Maken van een functie in Azure geactiveerd door een algemene webhook | Microsoft Docs
description: Azure Functions gebruik te maken van een zonder server-functie die wordt opgeroepen door een webhook in Azure.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
tags: 
ms.assetid: fafc10c0-84da-4404-b4fa-eea03c7bf2b1
ms.service: functions
ms.devlang: multiple
ms.topic: quickstart
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 08/12/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: f283f8d79c5ae5fb6a72c84c9e9edb7bb8de4a83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-function-triggered-by-a-generic-webhook"></a>Maak een functie die wordt geactiveerd door een algemene webhook

Met Azure Functions kunt u uw code in een serverloze omgeving uitvoeren zonder dat u eerst een virtuele machine moet maken of een webtoepassing moet publiceren. U kunt bijvoorbeeld een functie om te worden geactiveerd door een waarschuwing gegenereerd door Azure Monitor configureren. Dit onderwerp leest u hoe u C#-code wordt uitgevoerd wanneer een resourcegroep wordt toegevoegd aan uw abonnement.   

![Generic webhook geactiveerd functie in de Azure portal](./media/functions-create-generic-webhook-triggered-function/function-completed.png)

## <a name="prerequisites"></a>Vereisten 

Vereisten voor het voltooien van deze zelfstudie:

+ Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [functions-portal-favorite-function-apps](../../includes/functions-portal-favorite-function-apps.md)]

## <a name="create-an-azure-function-app"></a>Een Azure-functie-app maken

[!INCLUDE [Create function app Azure portal](../../includes/functions-create-function-app-portal.md)]

Vervolgens maakt u een functie in de nieuwe functie-app.

## <a name="create-function"></a>Maak een functie generic webhook geactiveerd

1. Vouw de functie-app uit en klik op de knop **+** naast **Functies**. Als u deze functie is het eerste certificaat in uw app in de functie, selecteert u **aangepaste functie**. U ziet nu de volledige set het functiesjablonen.

    ![De Quick Start-pagina van Functions in Azure Portal](./media/functions-create-generic-webhook-triggered-function/add-first-function.png)

2. Selecteer de **Generic WebHook - C#** sjabloon. Typ een naam voor uw C#-functie en selecteer vervolgens **maken**.

     ![Maak een functie generic webhook geactiveerd in de Azure-portal](./media/functions-create-generic-webhook-triggered-function/functions-create-generic-webhook-trigger.png) 

2. Klik in de nieuwe functie op **<> / Get function URL**, kopiëren en opslaan van de waarde. U kunt deze waarde gebruiken voor het configureren van de webhook. 

    ![De functiecode controleren](./media/functions-create-generic-webhook-triggered-function/functions-copy-function-url.png)
         
Vervolgens maakt u een webhook-eindpunt in een waarschuwing voor het logboek van activiteit in de Azure-Monitor. 

## <a name="create-an-activity-log-alert"></a>Maken van een activiteit logboek-waarschuwing

1. Navigeer in de Azure-portal naar de **Monitor** service, selecteer **waarschuwingen**, en klik op **toevoegen activiteit logboek waarschuwing**.   

    ![Bewaken](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert.png)

2. Gebruik de instellingen zoals opgegeven in de tabel:

    ![Maken van een activiteit logboek-waarschuwing](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Waarschuwing voor activiteit logboeknaam** | resource-group-maken-waarschuwing | De naam van de activiteit logboek-waarschuwing. |
    | **Abonnement** | Uw abonnement | Het abonnement dat u voor deze zelfstudie gebruikt. | 
    |  **Resourcegroep** | myResourceGroup | De resourcegroep die de waarschuwing bronnen om te worden geïmplementeerd. Met behulp van dezelfde resourcegroep als uw app functie gemakkelijker om op te schonen nadat u de zelfstudie hebt voltooid. |
    | **Gebeurteniscategorie** | Beheerdersrechten | Deze categorie omvat wijzigingen aangebracht in Azure-resources.  |
    | **Brontype** | Resourcegroepen | Filtert waarschuwingen resource om activiteiten te groeperen. |
    | **Resourcegroep**<br/>en **Resource** | Alle | Alle resources bewaken. |
    | **De naam van bewerking** | Een resourcegroep maken | Waarschuwingen voor het maken van de operations-filters. |
    | **Niveau** | Informatief | Informatieve waarschuwing bevatten. | 
    | **Status** | Geslaagd | Filtert waarschuwingen naar acties die zijn met succes voltooid. |
    | **Actiegroep** | Nieuw | Maak een nieuwe actiegroep waarin de actie wordt een waarschuwing wordt gegenereerd. |
    | **De naam van groep** | functie-webhook | Een unieke naam voor de groep in te grijpen.  | 
    | **Korte naam** | funcwebhook | Een korte naam voor de groep in te grijpen. |  

3. In **acties**, een actie met de instellingen die zijn opgegeven in de tabel toevoegen: 

    ![De actiegroep van een toevoegen](./media/functions-create-generic-webhook-triggered-function/functions-monitor-add-alert-settings-2.png)

    | Instelling      |  Voorgestelde waarde   | Beschrijving                              |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam** | CallFunctionWebhook | Een naam voor de actie. |
    | **Actietype** | Webhook | Het antwoord op de waarschuwing is dat een Webhook-URL wordt aangeroepen. |
    | **Details** | URL van de functie | Plak in de webhook-URL van de functie die u eerder hebt gekopieerd. |v

4. Klik op **OK** om de waarschuwing en de actie-groep te maken.  

De webhook is nu aangeroepen wanneer een resourcegroep in uw abonnement wordt gemaakt. Werk vervolgens de code in de functie voor het afhandelen van de JSON-logboekgegevens in de hoofdtekst van de aanvraag.   

## <a name="update-the-function-code"></a>De functiecode bijwerken

1. Ga terug naar de functie-app in de portal uit en vouw de functie. 

2. De C#-scriptcode in de functie in de portal vervangen door de volgende code:

    ```csharp
    #r "Newtonsoft.Json"
    
    using System;
    using System.Net;
    using Newtonsoft.Json;
    using Newtonsoft.Json.Linq;
    
    public static async Task<object> Run(HttpRequestMessage req, TraceWriter log)
    {
        log.Info($"Webhook was triggered!");
    
        // Get the activityLog object from the JSON in the message body.
        string jsonContent = await req.Content.ReadAsStringAsync();
        JToken activityLog = JObject.Parse(jsonContent.ToString())
            .SelectToken("data.context.activityLog");
    
        // Return an error if the resource in the activity log isn't a resource group. 
        if (activityLog == null || !string.Equals((string)activityLog["resourceType"], 
            "Microsoft.Resources/subscriptions/resourcegroups"))
        {
            log.Error("An error occured");
            return req.CreateResponse(HttpStatusCode.BadRequest, new
            {
                error = "Unexpected message payload or wrong alert received."
            });
        }
    
        // Write information about the created resource group to the streaming log.
        log.Info(string.Format("Resource group '{0}' was {1} on {2}.",
            (string)activityLog["resourceGroupName"],
            ((string)activityLog["subStatus"]).ToLower(), 
            (DateTime)activityLog["submissionTimestamp"]));
    
        return req.CreateResponse(HttpStatusCode.OK);    
    }
    ```

U kunt nu de functie testen door een nieuwe resourcegroep maken in uw abonnement.

## <a name="test-the-function"></a>De functie testen

1. Klik op het pictogram van de groep resources aan de linkerkant van de Azure portal, selecteer **+ toevoegen**, typ een **Resourcegroepnaam**, en selecteer **maken** om een lege resourcegroep te maken.
    
    ![Maak een resourcegroep.](./media/functions-create-generic-webhook-triggered-function/functions-create-resource-group.png)

2. Ga terug naar de functie en vouw de **logboeken** venster. Nadat de resourcegroep is gemaakt, wordt de activiteit logboek waarschuwing activeert de webhook en de functie wordt uitgevoerd. Ziet u de naam van de nieuwe resourcegroep naar de logboeken geschreven.  

    ![Een instelling van de toepassing test toevoegen.](./media/functions-create-generic-webhook-triggered-function/function-view-logs.png)

3. (Optioneel) Ga terug en verwijderen van de resourcegroep die u hebt gemaakt. Houd er rekening mee dat deze activiteit de functie niet activeren. Dit komt doordat delete-bewerkingen worden gefilterd door de waarschuwing. 

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Next steps note](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U kunt een functie die wordt uitgevoerd wanneer een aanvraag wordt ontvangen van een algemene webhook hebt gemaakt. 

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions HTTP and webhook bindings](functions-bindings-http-webhook.md) (Azure Functions-HTTP- en webhookbindingen) voor meer informatie over webhooktriggers. Zie voor meer informatie over het ontwikkelen van functies in C#, [Azure Functions C# script referentie voor ontwikkelaars](functions-reference-csharp.md).

