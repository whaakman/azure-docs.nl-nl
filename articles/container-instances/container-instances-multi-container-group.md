---
title: Meerdere containergroepen in Azure Containerexemplaren implementeren
description: Informatie over het implementeren van een containergroep met meerdere containers in Azure Containerexemplaren.
services: container-instances
author: neilpeterson
manager: timlt
ms.service: container-instances
ms.topic: article
ms.date: 01/10/2018
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 41a47adb1f1da417038757934f0a6cf7e11555da
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="deploy-a-container-group"></a>Een containergroep implementeren

Azure Containerexemplaren ondersteunt de implementatie van meerdere containers naar één host met behulp van een [containergroep](container-instances-container-groups.md). Dit is handig bij het bouwen van een toepassing ter voor logboekregistratie, bewaken of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft.

Dit document begeleidt u bij de configuratie van een eenvoudige meerdere container ter uitgevoerd door het implementeren van een Azure Resource Manager-sjabloon.

> [!NOTE]
> Meerdere container groepen zijn momenteel beperkt tot Linux containers. Terwijl we proberen te zorgen dat alle functies op Windows-containers, vindt u de huidige platform verschillen in [quota en beschikbaarheid in regio's voor exemplaren van Azure-Container](container-instances-quotas.md).

## <a name="configure-the-template"></a>De sjabloon configureren

Maak een bestand met de naam `azuredeploy.json` en kopieer de volgende JSON naar deze.

In dit voorbeeld wordt een containergroep met twee containers is een openbaar IP-adres en twee blootgestelde poorten gedefinieerd. De eerste container in de groep een internetgerichte-toepassing wordt uitgevoerd. De tweede container, de ter maakt een HTTP-aanvraag naar de belangrijkste webtoepassing via een van de groep lokale netwerk.

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {},
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "microsoft/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "microsoft/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "myContainerGroup",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2017-10-01-preview",
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
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', 'myContainerGroup')).ipAddress.ip]"
    }
  }
}
```

Voor het gebruik van een installatiekopie van privé-container register, moet u een object toevoegen aan het JSON-document met de volgende indeling.

```json
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

Implementeren van de sjabloon met de [az implementatie maken] [ az-group-deployment-create] opdracht.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --name myContainerGroup --template-file azuredeploy.json
```

Binnen enkele seconden, moet u een eerste reactie ontvangen van Azure.

## <a name="view-deployment-state"></a>Implementatiestatus weergeven

U kunt de status van de implementatie weergeven, met de [az container weergeven] [ az-container-show] opdracht. Hiermee wordt de ingerichte openbaar IP-adres waarmee de toepassing toegankelijk zijn.

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Uitvoer:

```bash
Name              ResourceGroup    ProvisioningState    Image                                                           IP:ports               CPU/Memory       OsType    Location
----------------  ---------------  -------------------  --------------------------------------------------------------  ---------------------  ---------------  --------  ----------
myContainerGroup  myResourceGroup  Succeeded            microsoft/aci-helloworld:latest,microsoft/aci-tutorial-sidecar  52.168.26.124:80,8080  1.0 core/1.5 gb  Linux     westus
```

## <a name="view-logs"></a>Logboeken bekijken

Weergave van de logboekuitvoer van een container met de [az container logboeken] [ az-container-logs] opdracht. De `--container-name` argument Hiermee geeft u de container waarin voor het ophalen van Logboeken. In dit voorbeeld wordt is de eerste container opgegeven.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Uitvoer:

```bash
listening on port 80
::1 - - [09/Jan/2018:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [09/Jan/2018:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Overzicht van de logboeken voor de container side auto dezelfde opdracht geven de tweede containernaam worden uitgevoerd.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Uitvoer:

```bash
Every 3s: curl -I http://localhost                          2018-01-09 23:25:11

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
Date: Tue, 09 Jan 2018 23:25:11 GMT
Connection: keep-alive
```

Zoals u ziet, is de ter periodiek een HTTP-aanvraag die naar de belangrijkste webtoepassing via een van de groep lokale netwerk om ervoor te zorgen dat deze wordt uitgevoerd. In dit voorbeeld ter kan worden uitgebreid activeren van een waarschuwing als er een HTTP-antwoordcode dan 200 OK ontvangen.

## <a name="next-steps"></a>Volgende stappen

In dit artikel betrekking op de stappen die nodig zijn voor het implementeren van een exemplaar van de container voor meerdere Azure-container. Zie de zelfstudie exemplaren van Azure-Container voor een end-to-end Containerexemplaren Azure-ervaring.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Containerexemplaren][aci-tutorial]

<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az_container_logs
[az-container-show]: /cli/azure/container#az_container_show
[az-group-create]: /cli/azure/group#az_group_create
[az-group-deployment-create]: /cli/azure/group/deployment#az_group_deployment_create
