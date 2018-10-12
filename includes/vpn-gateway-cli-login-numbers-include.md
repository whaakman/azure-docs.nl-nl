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
ms.openlocfilehash: 82de8eab089e5f666e1a2ce4eab09bfd2895185b
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "47020078"
---
1. Meld u aan bij uw Azure-abonnement met de opdracht [az login](/cli/azure/#login) en volg de instructies op het scherm. Zie [Aan de slag met Azure CLI](/cli/azure/get-started-with-azure-cli) voor meer informatie over aanmelden.

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
