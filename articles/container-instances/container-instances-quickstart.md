---
title: Uw eerste Azure Container Instances-container maken | Azure Docs
description: Azure Container Instances implementeren en ermee aan de slag gaan
services: container-service
documentationcenter: 
author: seanmck
manager: timlt
editor: 
tags: 
keywords: 
ms.assetid: 
ms.service: 
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/26/2017
ms.author: seanmck
ms.custom: 
ms.translationtype: HT
ms.sourcegitcommit: 3b15d6645b988f69f1f05b27aff6f726f34786fc
ms.openlocfilehash: 933299ce5a5d6f5b2262d40ae768019ccaf8796a
ms.contentlocale: nl-nl
ms.lasthandoff: 07/26/2017

---

# <a name="create-your-first-container-in-azure-container-instances"></a>Uw eerste container maken in Azure Container Instances

Met Azure Container Instances kunt u eenvoudig containers maken en beheren in Azure. In deze snelstartgids maakt u een container in Azure en geeft u deze op internet weer met een openbaar IP-adres. Deze bewerking wordt uitgevoerd in één opdracht. Binnen een paar seconden ziet u dit in uw browser:

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

De CLI-opdrachten van Azure Container Instances zijn momenteel alleen beschikbaar in de Azure Cloud Shell.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

Azure Container Instances zijn Azure-resources en moeten worden geplaatst in een Azure-resourcegroep, een logische verzameling waarin Azure resources worden geïmplementeerd en beheerd.

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). 

In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *VS Oost*.

```azurecli-interactive 
az group create --name myResourceGroup --location eastus
```

## <a name="create-a-container"></a>Een container maken

U kunt een container maken door een naam, een Docker-installatiekopie en een Azure-resourcegroep op te geven. U kunt de container desgewenst weergeven op internet met een openbaar IP-adres. We gebruiken in dit geval een container die als host fungeert voor een zeer eenvoudige web-app die is geschreven in [Node.js](http://nodejs.org).

CLI-opdrachten van Azure Container Instances zijn momenteel alleen beschikbaar in de Azure Cloud Shell.

```azurecli-interactive
az container create --name mycontainer --image microsoft/aci-helloworld --resource-group myResourceGroup --ip-address public 
```

Binnen enkele seconden krijgt u een reactie op uw aanvraag. In eerste instantie heeft de container de status **Maken** staat, maar deze moet binnen een paar seconden starten. U kunt de status controleren met de opdracht `show`:

```azurecli-interactive
az container show --name mycontainer --resource-group myResourceGroup
```

Aan de onderkant van de uitvoer ziet u de inrichtingsstatus en het IP-adres van de container:

```json
...
"ipAddress": {
      "ip": "13.88.8.148",
      "ports": [
        {
          "port": 80,
          "protocol": "TCP"
        }
      ]
    },
    "osType": "Linux",
    "provisioningState": "Succeeded"
...
```

Als de container de status **Voltooid** heeft, u kunt deze benaderen in de browser met behulp van het opgegeven IP-adres. 

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-app-browser]

## <a name="pull-the-container-logs"></a>De containerlogboeken ophalen

U kunt de logboeken voor de container die u hebt gemaakt ophalen met de opdracht `logs`:

```azurecli-interactive
az container logs --name mycontainer --resource-group myResourceGroup
```

Uitvoer:

```bash
listening on port 80
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET / HTTP/1.1" 200 1663 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
::ffff:10.240.255.105 - - [21/Jul/2017:00:01:46 +0000] "GET /favicon.ico HTTP/1.1" 404 150 "http://104.210.39.122/" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/59.0.3071.115 Safari/537.36"
```

## <a name="delete-the-container"></a>De container verwijderen

Wanneer u klaar bent met de container, kunt u deze verwijderen met behulp van de opdracht `delete`:

```azurecli-interactive
az container delete --name mycontainer --resource-group myResourceGroup
```

## <a name="next-steps"></a>Volgende stappen

Alle code voor de container die wordt gebruikt in deze snelstartgids is beschikbaar [op GitHub][app-github-repo], samen met het Dockerfile. Als u wilt proberen om deze zelf te bouwen en te implementeren in Azure Container Instances met behulp van het Azure Container Registry, gaat u verder met de zelfstudie van Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudies voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)


<!-- LINKS -->
[app-github-repo]: https://github.com/Azure-Samples/aci-helloworld.git

<!-- IMAGES -->
[aci-app-browser]: ./media/container-instances-quickstart/aci-app-browser.png
