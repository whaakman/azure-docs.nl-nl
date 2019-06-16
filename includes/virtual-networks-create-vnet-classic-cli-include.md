---
title: bestand opnemen
description: bestand opnemen
services: virtual-network
author: genlin
ms.service: virtual-network
ms.topic: include
ms.date: 04/13/2018
ms.author: genli
ms.custom: include file
ms.openlocfilehash: cd686e1bf62bbd7f37f61ced767e92918edf919c
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66116915"
---
## <a name="how-to-create-a-classic-vnet-using-azure-cli"></a>Over het maken van een klassiek VNet met behulp van Azure CLI
U kunt Azure CLI gebruiken voor het beheer van uw Azure-resources via de opdrachtprompt op elke computer met Windows, Linux of OS X.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../articles/cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer voor het maken van een VNet en een subnet de **azure network vnet maken** opdracht:
   
            azure network vnet create --vnet TestVNet -e 192.168.0.0 -i 16 -n FrontEnd -p 192.168.1.0 -r 24 -l "Central US"
   
    Verwachte uitvoer:
   
            info:    Executing command network vnet create
            + Looking up network configuration
            + Looking up locations
            + Setting network configuration
            info:    network vnet create command OK
   
   * **--vnet**. Naam van de VNet die moet worden gemaakt. Voor het scenario met *TestVNet*
   * **-e (of--adresruimte)** . VNet-adresruimte. Voor het scenario met *192.168.0.0*
   * **-i (of cidr-)** . Het netwerkmasker in CIDR-indeling. Voor het scenario met *16*.
   * **-n (of--subnet-name**). De naam van het eerste subnet. Voor het scenario met *FrontEnd*.
   * **-p (of--subnet-begin-ip)** . IP-adres voor het subnet of subnet een adresruimte wordt gestart. Voor het scenario met *192.168.1.0*.
   * **-r (of--subnet cidr)** . Het netwerkmasker in CIDR-indeling voor het subnet. Voor het scenario met *24*.
   * **-l (of --locatie)** . Azure-regio waar het VNet wordt gemaakt. Voor het scenario met *VS-midden*.
3. Voer voor het maken van een subnet de **azure network vnet subnet maken** opdracht:
   
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
   
   * **-t (of--vnet naam**. Naam van de VNet waar het subnet wordt gemaakt. Voor het scenario met *TestVNet*.
   * **-n (of --naam)** . Naam van het nieuwe subnet. Voor het scenario met *back-end*.
   * **-a (of --adresvoorvoegsel)** . Subnet CIDR-blok. Voor het scenario met *192.168.2.0/24*.
4. Uitvoeren om de eigenschappen van de nieuwe vnet weer, de **azure network vnet show** opdracht:
   
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

