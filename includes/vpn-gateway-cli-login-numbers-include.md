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
ms.openlocfilehash: 70a9e2a8f6327c0af92698b49d5b622bebba3661
ms.sourcegitcommit: 638599eb548e41f341c54e14b29480ab02655db1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/21/2018
ms.locfileid: "36313605"
---
1. Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI 2.0](/cli/azure/get-started-with-azure-cli) voor meer informatie over aanmelden.

  ```azurecli
  az login
  ```
2. Als u meer dan één Azure-abonnement hebt, worden alle abonnementen voor het account weergegeven.

  ```azurecli
  az account list --all
  ```
3. Geef het abonnement op dat u wilt gebruiken.

  ```azurecli
  az account set --subscription <replace_with_your_subscription_id>
  ```