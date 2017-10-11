---
title: Azure Containerexemplaren - meerdere containergroep | Azure Docs
description: Azure Containerexemplaren - groep met meerdere container
services: container-instances
documentationcenter: 
author: neilpeterson
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: container-instances
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 140f58582645ea32f77e901eb13364ed145bbecf
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-a-container-group"></a>Een containergroep implementeren

Azure Containerexemplaren ondersteuning van de implementatie van meerdere containers naar één host met behulp van een *containergroep*. Dit is handig bij het bouwen van een toepassing ter voor logboekregistratie, bewaken of een andere configuratie waarbij een tweede gekoppelde proces in een service nodig heeft. 

Dit document helpt bij het uitvoeren van een eenvoudige meerdere container ter-configuratie met een Azure Resource Manager-sjabloon.

## <a name="configure-the-template"></a>De sjabloon configureren

Maak een bestand met de naam `azuredeploy.json` en kopieer de volgende json naar deze. 

In dit voorbeeld wordt is een containergroep met twee containers en een openbare IP-adres gedefinieerd. De eerste container van de groep wordt een internettoepassing gerichte uitgevoerd. De tweede container, de ter maakt een HTTP-aanvraag naar de belangrijkste webtoepassing via een van de groep lokale netwerk. 

In dit voorbeeld ter kan worden uitgebreid activeren van een waarschuwing als er een HTTP-antwoordcode dan 200 OK ontvangen. 

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
  },
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
        "apiVersion": "2017-08-01-preview",
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

Voor het gebruik van een installatiekopie van privé-container register, moet u een object toevoegen aan het json-document met de volgende indeling.

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

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create).

```azurecli-interactive
az group create --name myResourceGroup --location westus
```

Implementeren van de sjabloon met de [az implementatie maken](/cli/azure/group/deployment#create) opdracht.

```azurecli-interactive
az group deployment create --name myContainerGroup --resource-group myResourceGroup --template-file azuredeploy.json
```

Binnen enkele seconden ontvangt u een eerste reactie van Azure. 

## <a name="view-deployment-state"></a>Implementatiestatus weergeven

U kunt de status van de implementatie weergeven, met de `az container show` opdracht. Hiermee wordt de ingerichte openbaar IP-adres gedurende welke de toepassing toegankelijk zijn.

```azurecli-interactive
az container show --name myContainerGroup --resource-group myResourceGroup -o table
```

Uitvoer:

```azurecli
Name              ResourceGroup    ProvisioningState    Image                                                             IP:ports           CPU/Memory    OsType    Location
----------------  ---------------  -------------------  ----------------------------------------------------------------  -----------------  ------------  --------  ----------
myContainerGroup  myResourceGrou2  Succeeded            microsoft/aci-tutorial-sidecar,microsoft/aci-tutorial-app:v1      40.118.253.154:80  1.0 core/1.5 gb   Linux     westus
```

## <a name="view-logs"></a>Logboeken bekijken   

Weergave van de logboekuitvoer van een container met de `az container logs` opdracht. De `--container-name` argument Hiermee geeft u de container waarin voor het ophalen van Logboeken. In dit voorbeeld wordt is de eerste container opgegeven. 

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-app --resource-group myResourceGroup
```

Uitvoer:

```bash
istening on port 80
::1 - - [27/Jul/2017:17:35:29 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:32 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:35 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [27/Jul/2017:17:35:38 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Overzicht van de logboeken voor de container side auto dezelfde opdracht geven de tweede containernaam worden uitgevoerd.

```azurecli-interactive
az container logs --name myContainerGroup --container-name aci-tutorial-sidecar --resource-group myResourceGroup
```

Uitvoer:

```bash
Every 3.0s: curl -I http://localhost                                                                                                                       Mon Jul 17 11:27:36 2017

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0     0    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
Accept-Ranges: bytes
Content-Length: 1663
Content-Type: text/html; charset=utf-8
Last-Modified: Sun, 16 Jul 2017 02:08:22 GMT
Date: Mon, 17 Jul 2017 18:27:36 GMT
```

Zoals u ziet, is de ter periodiek een HTTP-aanvraag die naar de belangrijkste webtoepassing via een van de groep lokale netwerk om ervoor te zorgen dat deze wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen

Dit document besproken de stappen die nodig zijn voor het implementeren van een exemplaar van de container voor meerdere Azure-container. Zie de zelfstudie exemplaren van Azure-Container voor een complete Containerexemplaren Azure-ervaring.

> [!div class="nextstepaction"]
> [Azure Containerexemplaren zelfstudie]:./container-instances-tutorial-prepare-app.md
