---
title: Azure SignalR Service gebeurtenissen verzenden naar Event Grid
description: Een handleiding leert u hoe u Event Grid-gebeurtenissen inschakelen voor uw SignalR-Service, klikt u vervolgens clientverbinding verbonden/verbroken gebeurtenissen verzenden naar een voorbeeldtoepassing.
services: azure-signalr
author: chenyl
ms.service: azure-signalr
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: chenyl
ms.openlocfilehash: 2d782306938136ce6d21a331185f591316f58a29
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67789174"
---
# <a name="how-to-send-events-from-azure-signalr-service-to-event-grid"></a>Over het verzenden van gebeurtenissen uit Azure SignalR Service naar Event Grid

Azure Event Grid is een volledig beheerde service voor gebeurtenisroutering die uniform gebeurtenisverbruik gebruik aan de hand van een pub-sub-model biedt. In deze handleiding kunt u de Azure CLI gebruiken om te maken van een Azure SignalR Service, abonneren op verbindingsgebeurtenissen en vervolgens een Voorbeeldwebtoepassing voor het ontvangen van de gebeurtenissen te implementeren. Ten slotte kunt u verbinding maakt en verbinding verbreken en bekijk de nettolading van de gebeurtenis in de voorbeeldtoepassing.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][azure-account] aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

De Azure CLI-opdrachten in dit artikel worden opgemaakt voor de **Bash** shell. Als u een andere shell, zoals PowerShell of Command Prompt gebruikt, moet u mogelijk regel voortzetting tekens of variabele toewijzingsregels overeenkomstig aanpassen. In dit artikel maakt gebruik van variabelen te minimaliseren, het bedrag van de opdracht bewerken vereist.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin u gegevens kunt implementeren en beheren van uw Azure-resources. De volgende [az-groep maken][az-group-create] opdracht maakt u een resourcegroep met de naam *myResourceGroup* in de *eastus* regio. Als u gebruiken een andere naam voor de resourcegroep wilt, stelt u `RESOURCE_GROUP_NAME` op een andere waarde.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-signalr-service"></a>Een SignalR-service maken

Vervolgens implementeert u een Azure Signalr-Service in de resourcegroep met de volgende opdrachten.
```azurecli-interactive
SIGNALR_NAME=SignalRTestSvc

az signalr create --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --sku Free_F1
```

Zodra de SignalR-Service is gemaakt, uitvoer de Azure CLI die lijkt op het volgende:

```json
{
  "externalIp": "13.76.156.152",
  "hostName": "clitest.servicedev.signalr.net",
  "hostNamePrefix": "clitest",
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/clitest1/providers/Microsoft.SignalRService/SignalR/clitest",
  "location": "southeastasia",
  "name": "clitest",
  "provisioningState": "Succeeded",
  "publicPort": 443,
  "resourceGroup": "clitest1",
  "serverPort": 443,
  "sku": {
    "capacity": 1,
    "family": null,
    "name": "Free_F1",
    "size": "F1",
    "tier": "Free"
  },
  "tags": null,
  "type": "Microsoft.SignalRService/SignalR",
  "version": "1.0"
}

```

## <a name="create-an-event-endpoint"></a>Een gebeurtenis-eindpunt maken

In deze sectie maakt u een Resource Manager-sjabloon bevindt zich in een GitHub-opslagplaats gebruiken voor het implementeren van een vooraf gemaakte voorbeeld-web-App in Azure App Service. Later, u abonneren op gebeurtenissen van Event Grid van uw register en geef deze app als het eindpunt waarop de gebeurtenissen worden verzonden.

Voor het implementeren van de voorbeeld-app, stel `SITE_NAME` in een unieke naam voor uw web-app, en voer de volgende opdrachten uit. Naam van de site moet uniek zijn binnen Azure omdat het deel van de volledig gekwalificeerde domeinnaam (FQDN) van de web-app uitmaakt. In een volgende sectie navigeren u naar de FQDN van de app in een webbrowser om gebeurtenissen van uw register weer te geven.

```azurecli-interactive
SITE_NAME=<your-site-name>

az group deployment create \
    --resource-group $RESOURCE_GROUP_NAME \
    --template-uri "https://raw.githubusercontent.com/Azure-Samples/azure-event-grid-viewer/master/azuredeploy.json" \
    --parameters siteName=$SITE_NAME hostingPlanName=$SITE_NAME-plan
```

Zodra de implementatie is gelukt (het kan enkele minuten duren), open een browser en navigeer naar uw web-app om te controleren of deze wordt uitgevoerd:

`http://<your-site-name>.azurewebsites.net`

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Abonneren op gebeurtenissen van register

In Event Grid, u zich abonneert op een *onderwerp* om aan te geven welke gebeurtenissen u wilt bijhouden, en waar u kunt ze verzenden. De volgende [az eventgrid gebeurtenisabonnement maken][az-eventgrid-event-subscription-create] opdracht Hiermee abonneert u zich bij de Azure SignalR Service u hebt gemaakt en Hiermee geeft u de URL van uw WebApp als het eindpunt waarop deze gebeurtenissen moet verzenden. De omgevingsvariabelen die u in eerdere secties ingevuld zijn hier, opnieuw gebruikt zodat er geen wijzigingen vereist zijn.

```azurecli-interactive
SIGNALR_SERVICE_ID=$(az signalr show --resource-group $RESOURCE_GROUP_NAME --name $SIGNALR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-signalr \
    --source-resource-id $SIGNALR_SERVICE_ID \
    --endpoint $APP_ENDPOINT
```

Wanneer het abonnement is voltooid, ziet u uitvoer die vergelijkbaar is met het volgende:

```JSON
{
  "deadLetterDestination": null,
  "destination": {
    "endpointBaseUrl": "https://$SITE_NAME.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "Microsoft.SignalRService.ClientConnectionConnected",
      "Microsoft.SignalRService.ClientConnectionDisconnected"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/Microsoft.SignalRService/SignalR/SignalRTestSvc/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-signalr",
  "labels": null,
  "name": "event-sub-signalr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "retryPolicy": {
    "eventTimeToLiveInMinutes": 1440,
    "maxDeliveryAttempts": 30
  },
  "topic": "/subscriptions/28cf13e2-c598-4aa9-b8c8-098441f0827a/resourceGroups/myResourceGroup/providers/microsoft.signalrservice/signalr/SignalRTestSvc",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Trigger register-gebeurtenissen

Overschakelen naar de servicemodus naar `Serverless Mode` en stelt u een clientverbinding met de SignalR-Service. U kunt ondernemen [Serverloze voorbeeld](https://github.com/aspnet/AzureSignalR-samples/tree/master/samples/Serverless) als uitgangspunt.

```bash
git clone git@github.com:aspnet/AzureSignalR-samples.git

cd samples/Management

# Start the negotiation server
# Negotiation server is responsible for generating access token for clients
cd NegotitationServer
dotnet user-secrets set Azure:SignalR:ConnectionString "<Connection String>"
dotnet run

# Use a seperate command line
# Start a client
cd SignalRClient
dotnet run
```

## <a name="view-registry-events"></a>Register-gebeurtenissen weergeven

U hebt nu een client verbinding met de SignalR-Service. Ga naar de logboeken van de Grid web-app, en u ziet een `ClientConnectionConnected` gebeurtenis. Als u de client wordt beëindigd, ziet u ook een `ClientConnectionDisconnected` gebeurtenis.

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
