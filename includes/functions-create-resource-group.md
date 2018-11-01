---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/04/2018
ms.author: glenga
ms.openlocfilehash: d44865dc3189a7f9dc05106baf9f4d120e5e8bf6
ms.sourcegitcommit: 9d7391e11d69af521a112ca886488caff5808ad6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50133841"
---
## <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources, zoals functie-apps, databases en opslagaccounts worden geïmplementeerd en beheerd.

In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt.  
Als u Cloud Shell niet gebruikt, moet u zich eerst aanmelden met `az login`.

```azurecli-interactive
az group create --name myResourceGroup --location westeurope
```
In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt. Als u alle ondersteunde locaties voor App Service-abonnementen wilt zien, voert u de opdracht [az appservice list-locations](/cli/azure/appservice#az-appservice-list-locations) uit.