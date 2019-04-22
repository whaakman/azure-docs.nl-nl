---
title: 'Zelfstudie: implementatie van een groep met meerdere containers in Azure Container Instances - sjabloon'
description: In deze zelfstudie leert u hoe u een containergroep met meerdere containers in Azure Container Instances implementeren met behulp van een Azure Resource Manager-sjabloon met de Azure CLI.
services: container-instances
author: dlepow
ms.service: container-instances
ms.topic: article
ms.date: 04/03/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: f769beda1654dc9f58ecff733741fb1ab9118031
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59006913"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-resource-manager-template"></a>Zelfstudie: Een groep met meerdere containers met behulp van Resource Manager-sjabloon implementeren

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Resource Manager](container-instances-multi-container-group.md)

Azure Container Instances biedt ondersteuning voor de implementatie van meerdere containers op één host met behulp van een [containergroep](container-instances-container-groups.md). Een containergroep is handig bij het bouwen van een toepassing sidecar voor logboekregistratie, bewaking of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft.

In deze zelfstudie hebt volgen u stappen voor het uitvoeren van een eenvoudige twee-container sidecar-configuratie door het implementeren van een Azure Resource Manager-sjabloon met de Azure CLI. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Configureren van een groep met meerdere containers-sjabloon
> * De containergroep implementeren
> * Bekijk de logboeken van de containers

Resource Manager-sjabloon kan gemakkelijk worden aangepast voor scenario's wanneer u nodig hebt voor het implementeren van aanvullende Azure-service-resources (bijvoorbeeld een Azure-bestandsshare of een virtueel netwerk) met de containergroep. 

> [!NOTE]
> Groepen met meerdere containers zijn momenteel beperkt tot Linux-containers. 

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-template"></a>Configureren van een sjabloon

Begin met het kopiëren van de volgende JSON naar een nieuw bestand met de naam `azuredeploy.json`. In Azure Cloud Shell, kunt u Visual Studio Code om te maken van het bestand in uw werkmap:

```
code azuredeploy.json
```

Deze Resource Manager-sjabloon definieert een containergroep met twee containers, een openbaar IP-adres en twee blootgestelde poorten. De eerste container in de groep wordt uitgevoerd een internetgerichte-webtoepassing. De tweede de sidecar-container maakt een HTTP-aanvraag voor de belangrijkste web-App via een van de groep lokale netwerk.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Als u wilt gebruiken een persoonlijk containerregister-installatiekopie, moet u een object toevoegen aan het JSON-document met de volgende indeling. Zie voor een voorbeeld van de implementatie van deze configuratie de [verwijzing naar ACI Resource Manager-sjabloon] [ template-reference] documentatie.

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Een resourcegroep maken met de opdracht [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementeer de sjabloon met de [az group deployment maken] [ az-group-deployment-create] opdracht.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
```

U ontvangt binnen enkele seconden een eerste reactie van Azure.

## <a name="view-deployment-state"></a>Status van de implementatie weergeven

Als u wilt de status van de implementatie weergeven, gebruikt u de volgende [az container show] [ az-container-show] opdracht:

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Als u de actieve App weergeven wilt, gaat u naar het IP-adres in uw browser. Het IP-adres is bijvoorbeeld `52.168.26.124` in de voorbeelduitvoer:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-container-logs"></a>Containerlogboeken ophalen

Weergave van de logboekuitvoer van een container met de [az container logs] [ az-container-logs] opdracht. De `--container-name` argument geeft u de container waarin om op te halen van Logboeken. In dit voorbeeld wordt de `aci-tutorial-app` container is opgegeven.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Uitvoer:

```bash
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Als u wilt zien van de logboeken voor het sidecar-container, voeren een vergelijkbare opdracht op te geven de `aci-tutorial-sidecar` container.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Uitvoer:

```bash
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Zoals u ziet, is de sidecar regelmatig maken van een HTTP-aanvraag naar de belangrijkste webtoepassing via een van de groep lokale netwerk om ervoor te zorgen dat deze wordt uitgevoerd. In dit voorbeeld sidecar kan worden uitgebreid voor het activeren van een waarschuwing als er een HTTP-responscode dan ontvangen `200 OK`.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie gebruikt u een Azure Resource Manager-sjabloon voor het implementeren van een groep met meerdere containers in Azure Container Instances. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Configureren van een groep met meerdere containers-sjabloon
> * De containergroep implementeren
> * Bekijk de logboeken van de containers

Zie voor aanvullende voorbeeldsjablonen, [Azure Resource Manager-sjablonen voor Azure Container Instances](container-instances-samples-rm.md).

U kunt ook opgeven dat een groep met meerdere containers met een [YAML-bestand](container-instances-multi-container-yaml.md). Implementatie met een YAML-bestand is door meer beknopte aard van de YAML-indeling, een goede keuze wanneer uw implementatie alleen containerinstanties bevat.


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
