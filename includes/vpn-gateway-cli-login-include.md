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
ms.openlocfilehash: bc7367ca2162dde9a266341f0d7b3dd1e8b0da81
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313690"
---
Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) voor meer informatie over aanmelden.

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