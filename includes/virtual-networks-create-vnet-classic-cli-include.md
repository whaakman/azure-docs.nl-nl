---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genli
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: 8476a7dadeaff64e703186396b4505cc307e4d3a
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Het maken van een klassiek VNet met Azure CLI
U kunt Azure CLI gebruiken voor het beheer van uw Azure-resources via de opdrachtprompt op elke computer met Windows, Linux of OS X.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../articles/cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voor het maken van een VNet en een subnet, voer de **azure network vnet maken** opdracht:
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Verwachte uitvoer:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Naam van de VNet die moet worden gemaakt. Voor het scenario *TestVNet*
   * **-e (of--adresruimte)**. VNet-adresruimte. Voor het scenario *192.168.0.0*
   * **-i (of de cidr-)**. Het netwerkmasker in CIDR-notatie. Voor het scenario *16*.
   * **-n (of--subnet naam**). Naam van het eerste subnet. Voor het scenario *FrontEnd*.
   * **-p (of--begin-ip-subnet)**. IP-adres voor het subnet of subnetadresruimte wordt gestart. Voor het scenario *192.168.1.0*.
   * **-r (of--subnet cidr)**. Het netwerkmasker in CIDR-indeling voor het subnet. Voor het scenario *24*.
   * **-l (of --locatie)**. Azure-regio waar de VNet wordt gemaakt. Voor het scenario *VS-midden*.
3. Voor het maken van een subnet, voer de **azure network vnet subnet maken** opdracht:
   
            azure network vnet subnet create -t TestVNet -n BackEnd -a 192.168.2.0/24
   
    De verwachte uitvoer voor de vorige opdracht:
   
            info:    Executing command network vnet subnet create
            + Looking up network configuration
            + Creating subnet "BackEnd"
            + Setting network configuration
            + Looking up the subnet "BackEnd"
            + Looking up network configuration
            data:    Name                            : BackEnd
            data:    Address prefix                  : 192.168.2.0/24
            info:    network vnet subnet create command OK
   
   * **-t (of--vnet naam**. Naam van de VNet waar het subnet wordt gemaakt. Voor het scenario *TestVNet*.
   * **-n (of --naam)**. Naam van het nieuwe subnet. Voor het scenario *back-end*.
   * **-a (of --adresvoorvoegsel)**. Subnet CIDR-blok. Voor het scenario *192.168.2.0/24*.
4. Om de eigenschappen van de nieuwe vnet weer, voer de **azure network vnet show** opdracht:
   
            azure network vnet show
   
    Verwachte uitvoer voor de vorige opdracht:
   
            info:    Executing command network vnet show
            Virtual network name: TestVNet
            + Looking up the virtual network sites
            data:    Name                            : TestVNet
            data:    Location                        : Central US
            data:    State                           : Created
            data:    Address space                   : 192.168.0.0/16
            data:    Subnets:
            data:      Name                          : FrontEnd
            data:      Address prefix                : 192.168.1.0/24
            data:
            data:      Name                          : BackEnd
            data:      Address prefix                : 192.168.2.0/24
            data:
            info:    network vnet show command OK

