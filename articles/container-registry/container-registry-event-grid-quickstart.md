---
title: Snelstartgids - gebeurtenissen naar Event Grid verzenden Azure Container Registry
description: In deze snelstartgids hebt u schakelt Event Grid-gebeurtenissen voor uw containerregister en vervolgens verzenden van container-installatiekopie pushen en gebeurtenissen naar een voorbeeldtoepassing te verwijderen.
services: container-registry
author: dlepow
ms.service: container-registry
ms.topic: article
ms.date: 08/23/2018
ms.author: danlep
ms.custom: seodec18
ms.openlocfilehash: dc3f61760dce6375a64b338fb230ee704863de06
ms.sourcegitcommit: 039263ff6271f318b471c4bf3dbc4b72659658ec
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/06/2019
ms.locfileid: "55755677"
---
# <a name="quickstart-send-events-from-private-container-registry-to-event-grid"></a>Snelstart: Gebeurtenissen van privécontainerregister verzenden naar Event Grid

Azure Event Grid is een volledig beheerde service voor gebeurtenisroutering die uniform gebeurtenisverbruik verbruik van een publish-subscribe-model. In deze Quick Start, kunt u de Azure CLI gebruiken om te abonneren op gebeurtenissen van het register en implementeer een Voorbeeldwebtoepassing voor het ontvangen van de gebeurtenissen, vervolgens een containerregister maken. Ten slotte het activeren van containerinstallatiekopie `push` en `delete` gebeurtenissen en bekijk de nettolading van de gebeurtenis in de voorbeeldtoepassing.

Nadat u de stappen in dit artikel hebt voltooid, worden gebeurtenissen die worden verzonden vanuit uw containerregister naar Event Grid weergegeven in de voorbeeld-web-app:

![Webbrowser weergeven van de voorbeeld-web-App met drie ontvangen gebeurtenissen][sample-app-01]

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account][azure-account] voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

De Azure CLI-opdrachten in dit artikel worden opgemaakt voor de **Bash** shell. Als u een andere shell, zoals PowerShell of Command Prompt gebruikt, moet u mogelijk regel voortzetting tekens of variabele toewijzingsregels overeenkomstig aanpassen. In dit artikel maakt gebruik van variabelen te minimaliseren, het bedrag van de opdracht bewerken vereist.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Een Azure-resourcegroep is een logische container waarin u gegevens kunt implementeren en beheren van uw Azure-resources. De volgende [az-groep maken] [ az-group-create] opdracht maakt u een resourcegroep met de naam *myResourceGroup* in de *eastus* regio. Als u gebruiken een andere naam voor de resourcegroep wilt, stelt u `RESOURCE_GROUP_NAME` op een andere waarde.

```azurecli-interactive
RESOURCE_GROUP_NAME=myResourceGroup

az group create --name $RESOURCE_GROUP_NAME --location eastus
```

## <a name="create-a-container-registry"></a>Een containerregister maken

Vervolgens implementeert u een containerregister in de resourcegroep met de volgende opdrachten. Voordat u de [az acr maken] [ az-acr-create] opdracht, stelt u de `ACR_NAME` naar een naam voor uw register. De naam moet uniek zijn binnen Azure en is beperkt tot 5 tot 50 alfanumerieke tekens bevatten.

```azurecli-interactive
ACR_NAME=<acrName>

az acr create --resource-group $RESOURCE_GROUP_NAME --name $ACR_NAME --sku Basic
```

Zodra het register is gemaakt, uitvoer de Azure CLI die lijkt op het volgende:

```json
{
  "adminUserEnabled": false,
  "creationDate": "2018-08-16T20:02:46.569509+00:00",
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry",
  "location": "eastus",
  "loginServer": "myregistry.azurecr.io",
  "name": "myregistry",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "sku": {
    "name": "Basic",
    "tier": "Basic"
  },
  "status": null,
  "storageAccount": null,
  "tags": {},
  "type": "Microsoft.ContainerRegistry/registries"
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

Zodra de implementatie is voltooid (dit kan enkele minuten duren), open een browser en navigeer naar uw web-app om te controleren of deze wordt uitgevoerd:

`http://<your-site-name>.azurewebsites.net`

Hier ziet u de voorbeeld-app weergegeven met geen gebeurtenisberichten weergegeven:

![Web browser weergeven Voorbeeld web-app met er zijn geen gebeurtenissen weergegeven][sample-app-02]

[!INCLUDE [event-grid-register-provider-cli.md](../../includes/event-grid-register-provider-cli.md)]

## <a name="subscribe-to-registry-events"></a>Abonneren op gebeurtenissen van register

In Event Grid, u zich abonneert op een *onderwerp* om aan te geven welke gebeurtenissen u wilt bijhouden, en waar u kunt ze verzenden. De volgende [az eventgrid gebeurtenisabonnement maken] [ az-eventgrid-event-subscription-create] opdracht zich abonneert op het containerregister dat u hebt gemaakt en Hiermee geeft u de URL van uw WebApp als het eindpunt waarop deze gebeurtenissen moet verzenden. De omgevingsvariabelen die u in eerdere secties ingevuld zijn hier, opnieuw gebruikt zodat er geen wijzigingen vereist zijn.

```azurecli-interactive
ACR_REGISTRY_ID=$(az acr show --name $ACR_NAME --query id --output tsv)
APP_ENDPOINT=https://$SITE_NAME.azurewebsites.net/api/updates

az eventgrid event-subscription create \
    --name event-sub-acr \
    --resource-id $ACR_REGISTRY_ID \
    --endpoint $APP_ENDPOINT
```

Wanneer het abonnement is voltooid, ziet u uitvoer die vergelijkbaar is met het volgende:

```JSON
{
  "destination": {
    "endpointBaseUrl": "https://eventgridviewer.azurewebsites.net/api/updates",
    "endpointType": "WebHook",
    "endpointUrl": null
  },
  "filter": {
    "includedEventTypes": [
      "All"
    ],
    "isSubjectCaseSensitive": null,
    "subjectBeginsWith": "",
    "subjectEndsWith": ""
  },
  "id": "/subscriptions/<Subscription ID>/resourceGroups/myResourceGroup/providers/Microsoft.ContainerRegistry/registries/myregistry/providers/Microsoft.EventGrid/eventSubscriptions/event-sub-acr",
  "labels": null,
  "name": "event-sub-acr",
  "provisioningState": "Succeeded",
  "resourceGroup": "myResourceGroup",
  "topic": "/subscriptions/<Subscription ID>/resourceGroups/myresourcegroup/providers/microsoft.containerregistry/registries/myregistry",
  "type": "Microsoft.EventGrid/eventSubscriptions"
}
```

## <a name="trigger-registry-events"></a>Trigger register-gebeurtenissen

Nu dat de voorbeeld-app online is en actief is en u bent geabonneerd op uw register met Event Grid, u kunt voor het genereren van bepaalde gebeurtenissen. In deze sectie kunt u taken van de ACR gebruiken om te bouwen en een containerinstallatiekopie naar uw register pushen. Taken van de ACR is een functie van Azure Container Registry waarmee u installatiekopieën in de cloud, compileren zonder de Docker-Engine zijn geïnstalleerd op uw lokale computer.

### <a name="build-and-push-image"></a>Opbouwen en pushen van installatiekopie

Voer de volgende Azure CLI-opdracht voor het bouwen van een containerinstallatiekopie van de inhoud van een GitHub-opslagplaats. Standaard ACR taken nieuwe versies automatisch een correct gemaakte installatiekopie naar het register, genereert de `ImagePushed` gebeurtenis.

```azurecli-interactive
az acr build --registry $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
```

U ziet de uitvoer is vergelijkbaar met het volgende terwijl ACR taken bouwt en vervolgens pushes uw installatiekopie. De volgende voorbeelduitvoer zijn afgekapt als beknopt alternatief.

```console
$ az acr build -r $ACR_NAME --image myimage:v1 -f Dockerfile https://github.com/Azure-Samples/acr-build-helloworld-node.git
Sending build context to ACR...
Queued a build with build ID: aa2
Waiting for build agent...
2018/08/16 22:19:38 Using acb_vol_27a2afa6-27dc-4ae4-9e52-6d6c8b7455b2 as the home volume
2018/08/16 22:19:38 Setting up Docker configuration...
2018/08/16 22:19:39 Successfully set up Docker configuration
2018/08/16 22:19:39 Logging in to registry: myregistry.azurecr.io
2018/08/16 22:19:55 Successfully logged in
Sending build context to Docker daemon  94.72kB
Step 1/5 : FROM node:9-alpine
...
```

Om te bevestigen dat de gemaakte installatiekopie in het register, dan de volgende opdracht om de labels in de opslagplaats "myimage" weer te geven:

```azurecli-interactive
az acr repository show-tags --name $ACR_NAME --repository myimage
```

De tag 'v1' van de installatiekopie die u hebt gemaakt, moet worden weergegeven in de uitvoer is vergelijkbaar met het volgende:

```console
$ az acr repository show-tags --name $ACR_NAME --repository myimage
[
  "v1"
]
```

### <a name="delete-the-image"></a>De installatiekopie verwijderen

Nu genereren een `ImageDeleted` gebeurtenis door het verwijderen van de installatiekopie met de [az acr repository delete] [ az-acr-repository-delete] opdracht:

```azurecli-interactive
az acr repository delete --name $ACR_NAME --image myimage:v1
```

U ziet de uitvoer is vergelijkbaar met de volgende vragen om bevestiging verwijderen van het manifest en de bijbehorende afbeeldingen:

```console
$ az acr repository delete --name $ACR_NAME --image myimage:v1
This operation will delete the manifest 'sha256:f15fa9d0a69081ba93eee308b0e475a54fac9c682196721e294b2bc20ab23a1b' and all the following images: 'myimage:v1'.
Are you sure you want to continue? (y/n): y
```

## <a name="view-registry-events"></a>Register-gebeurtenissen weergeven

U hebt nu een installatiekopie naar het register gepusht en vervolgens verwijderd. Navigeer naar uw web-app voor Event Grid Viewer en ziet u beide `ImageDeleted` en `ImagePushed` gebeurtenissen. U ziet ook een abonnement validatie van de gebeurtenis is gegenereerd door het uitvoeren van de opdracht in de [abonneren op gebeurtenissen van register](#subscribe-to-registry-events) sectie.

De volgende schermafbeelding ziet u de voorbeeld-app met de drie gebeurtenissen, en de `ImageDeleted` gebeurtenis is uitgevouwen om de details ervan weer te geven.

![Webbrowser met de voorbeeld-app met ImagePushed en ImageDeleted gebeurtenissen][sample-app-03]

Gefeliciteerd! Als u ziet de `ImagePushed` en `ImageDeleted` gebeurtenissen, het register is verzenden van gebeurtenissen naar Event Grid en Event Grid is doorsturen van deze gebeurtenissen aan uw web-app-eindpunt.

## <a name="clean-up-resources"></a>Resources opschonen

Als u klaar bent met de resources die u in deze Quick Start hebt gemaakt, kunt u ze verwijderen met de volgende Azure CLI-opdracht. Wanneer u een resourcegroep verwijdert, worden alle resources die deze bevat permanent verwijderd.

**WAARSCHUWING**: Deze bewerking is niet ongedaan worden gemaakt. Zorg ervoor dat u niet meer nodig hebt van de resources in de groep voordat u de opdracht uitvoert.

```azurecli-interactive
az group delete --name $RESOURCE_GROUP_NAME
```

## <a name="event-grid-event-schema"></a>Event Grid-gebeurtenisschema

U kunt de Azure Container Registry gebeurtenis bericht schemaverwijzing vinden in de documentatie voor Event Grid:

[Azure Event Grid-gebeurtenisschema voor Container Registry](../event-grid/event-schema-container-registry.md)

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u geïmplementeerd een containerregister, een afbeelding met ACR taken gebouwd, verwijderd en van uw register gebeurtenissen uit Event Grid met een voorbeeldtoepassing heeft verbruikt. Vervolgens gaan naar de taken van de ACR-zelfstudie voor meer informatie over het bouwen van containerinstallatiekopieën in de cloud, inclusief geautomatiseerde builds op updates van basisinstallatiekopieën:

> [!div class="nextstepaction"]
> [In de cloud met ACR taken installatiekopieën compileren](container-registry-tutorial-quick-task.md)

<!-- IMAGES -->
[sample-app-01]: ./media/container-registry-event-grid-quickstart/sample-app-01.png
[sample-app-02]: ./media/container-registry-event-grid-quickstart/sample-app-02-no-events.png
[sample-app-03]: ./media/container-registry-event-grid-quickstart/sample-app-03-with-events.png

<!-- LINKS - External -->
[azure-account]: https://azure.microsoft.com/free/?WT.mc_id=A261C142F
[sample-app]: https://github.com/dbarkol/azure-event-grid-viewer

<!-- LINKS - Internal -->
[az-acr-create]: /cli/azure/acr/repository
[az-acr-repository-delete]: /cli/azure/acr/repository#az-acr-repository-delete
[az-eventgrid-event-subscription-create]: /cli/azure/eventgrid/event-subscription#az-eventgrid-event-subscription-create
[az-group-create]: /cli/azure/group#az-group-create
