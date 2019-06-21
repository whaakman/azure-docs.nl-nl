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
ms.openlocfilehash: da4177fd54c0d8777f15175cea3a74a8b01c0954
ms.sourcegitcommit: 2d3b1d7653c6c585e9423cf41658de0c68d883fa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67305270"
---
1. Zorg ervoor dat u zich hebt aangemeld bij uw Azure-account en met behulp van het abonnement dat u vrijgeven voor deze Preview-versie wilt. Gebruik het volgende voorbeeld om in te schrijven:

    ```azurepowershell-interactive
    Register-AzProviderFeature -FeatureName AllowBastionHost -ProviderNamespace Microsoft.Network
    ```
2.  Uw abonnement opnieuw te registreren met de *Microsoft.Network* naamruimte van de provider.

    ```azurepowershell-interactive
    Register-AzResourceProvider -ProviderNamespace Microsoft.Network
    ````
3. Gebruik de volgende opdracht om te controleren of de *AllowBastionHost* functie is geregistreerd bij uw abonnement:

    ```azurepowershell-interactive
    Get-AzProviderFeature -ProviderNamespace Microsoft.Network
    ````

    Het duurt een paar minuten voor de registratie te voltooien.
