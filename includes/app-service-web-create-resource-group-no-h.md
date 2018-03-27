---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 4624663cc1bcdeea87ab3b79d2ffb8017b539f45
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/16/2018
---
[!INCLUDE [resource group intro text](resource-group.md)]

Maak een resourcegroep in Cloud Shell met de opdracht [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *West-Europa*. Als u alle ondersteunde locaties voor App Service in de **Gratis** laag wilt zien, voert u de opdracht [`az appservice list-locations --sku F1`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) uit.

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt. 

Wanneer de opdracht is voltooid, laat een JSON-uitvoer u de eigenschappen van de resource-groep zien.