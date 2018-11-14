---
title: Bewaken van Azure Media Services-gebeurtenissen met Event Grid met CLI | Microsoft Docs
description: Dit artikel leest hoe u zich kunt abonneren op Event Grid om te controleren van Azure Media Services-gebeurtenissen.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: ''
ms.topic: article
ms.date: 11/09/2018
ms.author: juliako
ms.openlocfilehash: 16f964c6f881777e0217979a329610902b29a87b
ms.sourcegitcommit: b62f138cc477d2bd7e658488aff8e9a5dd24d577
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2018
ms.locfileid: "51612614"
---
# <a name="create-and-monitor-media-services-events-with-event-grid-using-the-azure-cli"></a>Maken en bewaken van Media Services-gebeurtenissen met Event Grid met de Azure CLI

Azure Event Grid is een gebeurtenisservice voor de cloud. In dit artikel, kunt u de Azure CLI gebruiken om u te abonneren op gebeurtenissen voor Azure Media Services-account. Vervolgens kunt activeren u gebeurtenissen om het resultaat weer te. Normaal gesproken verzendt u gebeurtenissen naar een eindpunt dat de gebeurtenisgegevens verwerkt en vervolgens in actie komt. In dit artikel, kunt u de gebeurtenissen verzenden naar een WebApp die worden verzameld en worden de berichten weergegeven.

## <a name="prerequisites"></a>Vereisten

- Een actief Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) aan voordat u begint.
- Installeren en de CLI lokaal gebruikt, in dit artikel gebruikmaken van Azure CLI versie 2.0 of hoger. Voer `az --version` uit om te zien welke versie u hebt. Als u uw CLI wilt installeren of upgraden, raadpleegt u [De Azure CLI installeren](/cli/azure/install-azure-cli). 

    Op dit moment niet alle [Media Services v3 CLI](https://aka.ms/ams-v3-cli-ref) opdrachten werken in de Azure Cloud Shell. Het verdient de CLI lokaal gebruikt.

- [Een Media Services-account maken](create-account-cli-how-to.md).

    Zorg ervoor dat u vergeet niet de waarden die u voor de naam van de resourcegroep en de naam van de Media Services-account gebruikt.

## <a name="create-a-message-endpoint"></a>Het eindpunt van een bericht maken

Voordat u zich abonneert op de gebeurtenissen voor het Media Services-account, moet u gaan we het eindpunt voor het gebeurtenisbericht maken. Het eindpunt onderneemt normaal gesproken actie op basis van de gebeurtenisgegevens. In dit artikel, implementeert u een [vooraf gemaakte web-app](https://github.com/Azure-Samples/azure-event-grid-viewer) waarmee de gebeurtenisberichten worden weergegeven. De geïmplementeerde oplossing omvat een App Service-plan, een App Service-web-app en broncode van GitHub.

1. Selecteer **Implementeren in Azure** om de oplossing voor uw abonnement te implementeren. Geef in Azure Portal waarden op voor de parameters.

   <a href="https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure-Samples%2Fazure-event-grid-viewer%2Fmaster%2Fazuredeploy.json" target="_blank"><img src="http://azuredeploy.net/deploybutton.png"/></a>

1. De implementatie kan enkele minuten duren. Controleer of uw web-app wordt uitgevoerd nadat de implementatie is voltooid. Navigeer in een webbrowser naar: `https://<your-site-name>.azurewebsites.net`

Als u overschakelt naar de site "Azure Event Grid-Viewer", ziet u dat nog geen gebeurtenissen heeft.
   
[!INCLUDE [event-grid-register-provider-portal.md](../../../includes/event-grid-register-provider-portal.md)]

## <a name="set-the-azure-subscription"></a>Het Azure-abonnement instellen

Geef in de volgende opdracht de id van het Azure-abonnement op dat u wilt gebruiken voor het Media Services-account. U kunt een lijst met abonnementen bekijken waartoe u toegang hebt door naar [Abonnementen](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) te gaan.

```azurecli
az account set --subscription mySubscriptionId
```

## <a name="subscribe-to-media-services-events"></a>Abonneren op gebeurtenissen voor Media Services

U zich abonneert op een artikel Event Grid zien welke gebeurtenissen u wilt bijhouden. Het volgende voorbeeld is geabonneerd op de Media Services-account u gemaakt en hoe de URL van de website die u hebt gemaakt als het eindpunt voor de gebeurtenismelding. 

Vervang `<event_subscription_name>` met een unieke naam voor het gebeurtenisabonnement. Voor `<resource_group_name>` en `<ams_account_name>`, gebruikt u de waarden die u hebt gebruikt bij het maken van het Media Services-account. Voor de `<endpoint_URL>`, geef de URL van uw web-app en voeg `api/updates` naar de URL van startpagina. Het eindpunt opgeeft wanneer u zich abonneert, verwerkt Event Grid de routering van gebeurtenissen naar dit eindpunt. 

1. De resource-id ophalen

    ```azurecli
    amsResourceId=$(az ams account show --name <ams_account_name> --resource-group <resource_group_name> --query id --output tsv)
    ```

    Bijvoorbeeld:

    ```
    amsResourceId=$(az ams account show --name amsaccount --resource-group amsResourceGroup --query id --output tsv)
    ```

2. Abonneren op gebeurtenissen

    ```azurecli
    az eventgrid event-subscription create \
    --resource-id $amsResourceId \
    --name <event_subscription_name> \
    --endpoint <endpoint_URL>
    ```

    Bijvoorbeeld:

    ```
    az eventgrid event-subscription create --resource-id $amsResourceId --name amsTestEventSubscription --endpoint https://amstesteventgrid.azurewebsites.net/api/updates/
    ```    

    > [!TIP]
    > U kunt de handshake validatiewaarschuwing krijgen. Wijs hieraan een paar minuten en de handshake te valideren.

Nu gaan we trigger gebeurtenissen om te zien hoe het bericht aan uw eindpunt wordt gedistribueerd in Event Grid.

## <a name="send-an-event-to-your-endpoint"></a>Een gebeurtenis verzenden naar het eindpunt

U kunt gebeurtenissen voor het Media Services-account door het uitvoeren van een coderingstaak activeren. U kunt volgen [in deze snelstartgids](stream-files-dotnet-quickstart.md) voor het coderen van een bestand en beginnen met het verzenden van gebeurtenissen. 

Bekijk opnieuw uw web-app en u zult zien dat er een validatiegebeurtenis voor een abonnement naartoe is verzonden. Via Event Grid wordt de validatiegebeurtenis verzonden zodat het eindpunt kan controleren of de gebeurtenisgegevens in aanmerking komen om ontvangen te worden. Het eindpunt is om in te stellen `validationResponse` naar `validationCode`. Zie voor meer informatie, [Event Grid-beveiliging en verificatie](../../event-grid/security-authentication.md). U kunt de code van de web-app om te zien hoe het abonnement gevalideerd weergeven.

> [!TIP]
> Selecteer het oogpictogram om de gebeurtenisgegevens uit te breiden. Vernieuw de pagina, niet als u wilt weergeven van alle gebeurtenissen.

![Een abonnementgebeurtenis weergeven](./media/monitor-events-portal/view-subscription-event.png)

## <a name="next-steps"></a>Volgende stappen

[Uploaden, coderen en streamen](stream-files-tutorial-with-api.md)

