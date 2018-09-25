---
title: bestand opnemen
description: bestand opnemen
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 03/21/2018
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: e3961878663a26d721478effc01d968e21e34c18
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020792"
---
Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm. Zie voor meer informatie over het aanmelden, [aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli).

```azurecli
az login
```

Als u meer dan één Azure-abonnement hebt, worden alle abonnementen voor het account weergegeven.

```azurecli
az account list --all
```

Geef het abonnement op dat u wilt gebruiken.

```azurecli
az account set --subscription <replace_with_your_subscription_id>
```
