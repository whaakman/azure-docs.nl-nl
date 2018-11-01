---
title: bestand opnemen
description: bestand opnemen
services: app-service
author: msangapu
ms.service: app-service
ms.topic: include
ms.date: 09/18/2018
ms.author: msangapu
ms.custom: include file
ms.openlocfilehash: 458782011623721bb44771d42caa32130e23bbc9
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50035103"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Maak een resourcegroep in Cloud Shell met de opdracht [`az group create`](/cli/azure/group?view=azure-cli-latest#az-group-create). In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *US - zuid-centraal*. Als u alle ondersteunde locaties voor App Service in de **Gratis** laag wilt zien, voert u de opdracht [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest#az-appservice-list-locations) uit.

```azurecli-interactive
az group create --name myResourceGroup --location "South Central US"
```

In het algemeen maakt u een resourcegroep en resources in een regio bij u in de buurt. 

Wanneer de opdracht is voltooid, laat een JSON-uitvoer u de eigenschappen van de resource-groep zien.