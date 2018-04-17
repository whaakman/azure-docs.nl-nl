---
title: Maken van een netwerkbeveiligingsgroep (klassiek) met behulp van de Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken en implementeren van een netwerkbeveiligingsgroep (klassiek) met behulp van de Azure CLI 1.0.
services: virtual-network
documentationcenter: na
author: genli
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
ms.openlocfilehash: a9ab1a83366919a2d05da18819ed8167bdadb20a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="create-a-network-security-group-classic-using-the-azure-cli-10"></a>Maken van een netwerkbeveiligingsgroep (klassiek) met behulp van de Azure CLI 1.0
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [nsg's maken in het Resource Manager-implementatiemodel](tutorial-filter-network-traffic-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

De volgende voorbeeldopdrachten van Azure CLI verwacht een eenvoudige omgeving al gemaakt op basis van het scenario. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst de testomgeving door bouwen [maken van een VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="create-an-nsg-for-the-front-end-subnet"></a>Een NSG voor de front-end-subnet maken

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [installeren en configureren van de Azure CLI](../cli-install-nodejs.md).
2. Overschakelen naar de klassieke modus:

    ```azurecli
    azure config mode asm
    ```   

3. Maken van een NSG::
   
    ```azurecli   
     azure network nsg create -l uswest -n NSG-FrontEnd
    ```
   
4. Maak een regel waarmee toegang tot poort 3389 (RDP) van het internet:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   ```

5. Maak een regel die toegang tot poort 80 (HTTP) van het internet is:
   
    ```azurecli
    azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
    ```   

6. Koppel het NSG aan de front-end-subnet:
   
    ```azurecli
    azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   ```

## <a name="create-the-nsg-for-the-back-end-subnet"></a>Het NSG voor de back-end-subnet maken

1. Het NSG maken:
   
    ```azurecli
    azure network nsg create -l uswest -n NSG-BackEnd
   ```

2. Maak een regel waarmee toegang tot poort 1433 (SQL) van de front-end-subnet:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   ```

3. Maak een regel die de toegang met het internet weigert:
   
    ```azurecli
    azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   ```

4. Het NSG aan het subnet voor back-end koppelen:
   
    ```azurecli
    azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
    ```