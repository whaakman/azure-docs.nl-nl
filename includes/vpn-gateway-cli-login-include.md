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
ms.openlocfilehash: f76dd47081a826e341d15fedc583d29f0373e475
ms.sourcegitcommit: 48ab1b6526ce290316b9da4d18de00c77526a541
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/23/2018
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