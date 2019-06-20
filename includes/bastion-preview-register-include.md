---
title: bestand opnemen
description: bestand opnemen
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: include
ms.date: 05/28/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: d05d425cc9bfb206207801f15a25e17d60dc0aaf
ms.sourcegitcommit: 156b313eec59ad1b5a820fabb4d0f16b602737fc
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67191744"
---
1. Zorg ervoor dat u zich hebt aangemeld bij uw Azure-account en met behulp van het abonnement dat u vrijgeven voor deze Preview-versie wilt. Gebruik het volgende voorbeeld om in te schrijven:

    ```azurepowershell-interactive
    Register-AzureRmProviderFeature -FeatureName AllowBastionHost -ProviderNamespace Microsoft.Network
    ```
2.  Uw abonnement opnieuw te registreren met de *Microsoft.Network* naamruimte van de provider.

    ```azurepowershell-interactive
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ````
3. Gebruik de volgende opdracht om te controleren of de *AllowBastionHost* functie is geregistreerd bij uw abonnement:

    ```azurepowershell-interactive
    Get-AzureRmProviderFeature -ProviderNamespace Microsoft.Network
    ````

    Het duurt een paar minuten voor de registratie te voltooien.