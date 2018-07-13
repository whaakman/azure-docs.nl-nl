---
title: Maak een netwerkbeveiligingsgroep (klassiek) met behulp van de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken en implementeren van een netwerkbeveiligingsgroep (klassiek) met behulp van de Azure CLI 1.0.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: genli
ms.openlocfilehash: 5468801e56849498d712f51e71cfb31bf068398a
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38696615"
---
# <a name="create-a-network-security-group-classic-using-the-azure-cli-10"></a>Maak een netwerkbeveiligingsgroep (klassiek) met behulp van de Azure CLI 1.0
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [nsg's maken in het Resource Manager-implementatiemodel](tutorial-filter-network-traffic-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

Het volgende voorbeeld van Azure CLI-opdrachten kunt verwachten van een eenvoudige omgeving al gemaakt op basis van het scenario. Als u wilt dat de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst samenstellen van de testomgeving door [het maken van een VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Een NSG voor het front-end-subnet maken

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [installeren en configureren van de Azure CLI](../cli-install-nodejs.md).
2. Overschakelen naar de klassieke modus:

    ```azurecli
    azure config mode asm
    ```   

3. Een NSG te maken:
   
    ```azurecli   
     azure network nsg create -l uswest -n NSG-FrontEnd
    ```
   
4. Maak een beveiligingsregel waarmee toegang tot poort 3389 (RDP) van het internet:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   ```

5. Maak een regel die toegang tot poort 80 (HTTP) vanaf het internet is:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
    ```   

6. De NSG aan het front-end-subnet koppelen:
   
    ```azurecli
    azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   ```

## <a name="create-the-nsg-for-the-back-end-subnet"></a>De NSG voor de back-end-subnet maken

1. De nsg's maken:
   
    ```azurecli
    azure network nsg create -l uswest -n NSG-BackEnd
   ```

2. Maak een regel waarmee toegang tot poort 1433 (SQL) van het front-end-subnet:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   ```

3. Maak een regel voor weigeren van toegang tot het internet:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   ```

4. De NSG aan het back-end-subnet koppelen:
   
    ```azurecli
    azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
    ```