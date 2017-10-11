---
title: Het nsg's maken in de klassieke modus met de Azure CLI | Microsoft Docs
description: Meer informatie over het maken en nsg's implementeren in de klassieke modus met de Azure CLI
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-service-management
ms.assetid: 17d98950-5fbb-4653-bef6-d822ab37541e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.openlocfilehash: 115a1937a4c88ba2b986a40c84b1b759ed5e03b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-nsgs-classic-in-the-azure-cli"></a>Het nsg's (klassiek) maken in de Azure CLI
[!INCLUDE [virtual-networks-create-nsg-selectors-classic-include](../../includes/virtual-networks-create-nsg-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-nsg-intro-include](../../includes/virtual-networks-create-nsg-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [nsg's maken in het Resource Manager-implementatiemodel](virtual-networks-create-nsg-arm-cli.md).

[!INCLUDE [virtual-networks-create-nsg-scenario-include](../../includes/virtual-networks-create-nsg-scenario-include.md)]

De Azure CLI Voorbeeldopdrachten onderstaande verwacht een eenvoudige omgeving al gemaakt op basis van het bovenstaande scenario. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst de testomgeving door bouwen [maken van een VNet](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-create-the-nsg-for-the-front-end-subnet"></a>Het maken van het NSG voor de front-end-subnet
Een NSG met de naam te maken met de naam **NSG-FrontEnd** op basis van het bovenstaande scenario, volg de onderstaande stappen.

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer de  **`azure config mode`**  opdracht uit om te schakelen naar de klassieke modus, zoals hieronder weergegeven.
   
        azure config mode asm
   
    Verwachte uitvoer:
   
        info:    New mode is asm
3. Voer de  **`azure network nsg create`**  opdracht voor het maken van een NSG.
   
        azure network nsg create -l uswest -n NSG-FrontEnd
   
    Verwachte uitvoer:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-FrontEnd"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : NSG-FrontEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parameters:
   
   * **-l (of --locatie)**. Azure-regio waar de nieuwe NSG wordt gemaakt. In ons scenario *westus*.
   * **-n (of --naam)**. Naam voor de nieuwe NSG. In ons scenario *NSG-FrontEnd*.
4. Voer de  **`azure network nsg rule create`**  opdracht maakt u een regel waarmee toegang tot poort 3389 (RDP) van het Internet.
   
        azure network nsg rule create -a NSG-FrontEnd -n rdp-rule -c Allow -p Tcp -r Inbound -y 100 -f Internet -o * -e * -u 3389
   
    Verwachte uitvoer:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "rdp-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : rdp-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 3389
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
   
    Parameters:
   
   * **-a (of--nsg-naam)**. Naam van de NSG waarin de regel wordt gemaakt. In ons scenario *NSG-FrontEnd*.
   * **-n (of --naam)**. Naam voor de nieuwe regel. In ons scenario *rdp-regel*.
   * **-c (of--actie)**. Toegangsniveau voor de regel (weigeren of toestaan).
   * **-p (of--protocol)**. Protocol (Tcp, Udp of *) voor de regel.
   * **-r (of--type)**. Richting van de verbinding (inkomend of uitgaand).
   * **-y (of--prioriteit)**. Prioriteit voor de regel.
   * **-f (of--bron adresvoorvoegsel)**. Voorvoegsel voor bronadres in CIDR- of met standaardlabels.
   * **-o (of--bronpoortbereik)**. Bronpoort, of een poortbereik.
   * **-e (of--bestemming adresvoorvoegsel)**. Voorvoegsel voor doeladres in CIDR- of met standaardlabels.
   * **-u (of--doelpoortbereik)**. Doelpoort, of een poortbereik.
5. Voer de  **`azure network nsg rule create`**  opdracht voor het maken van een regel die toegang tot poort 80 (HTTP) van het Internet is.
   
        azure network nsg rule create -a NSG-FrontEnd -n web-rule -c Allow -p Tcp -r Inbound -y 200 -f Internet -o * -e * -u 80
   
    Verwachte putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-FrontEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : INTERNET
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 200
        info:    network nsg rule create command OK
6. Voer de  **`azure network nsg subnet add`**  opdracht het NSG koppelen aan de front-end-subnet.
   
        azure network nsg subnet add -a NSG-FrontEnd --vnet-name TestVNet --subnet-name FrontEnd
   
    Verwachte uitvoer:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-FrontEnd"
        info:    Looking up the subnet "FrontEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-FrontEnd"
        info:    network nsg subnet add command OK

## <a name="how-to-create-the-nsg-for-the-back-end-subnet"></a>Het maken van het NSG voor de back-end-subnet
Een NSG met de naam te maken met de naam *NSG-back-end* op basis van het bovenstaande scenario, volg de onderstaande stappen.

1. Voer de  **`azure network nsg create`**  opdracht voor het maken van een NSG.
   
        azure network nsg create -l uswest -n NSG-BackEnd
   
    Verwachte uitvoer:
   
        info:    Executing command network nsg create
        info:    Creating a network security group "NSG-BackEnd"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : NSG-BackEnd
        data:    Location                        : West US
        data:    Security group rules:
        data:    Name                               Source IP           Source Port  Destination IP   Destination Port  Protocol  Type      Action  Prior
        ity  Default
        data:    ---------------------------------  ------------------  -----------  ---------------  ----------------  --------  --------  ------  -----
        ---  -------
        data:    ALLOW VNET OUTBOUND                VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Outbound  Allow   65000
             true   
        data:    ALLOW VNET INBOUND                 VIRTUAL_NETWORK     *            VIRTUAL_NETWORK  *                 *         Inbound   Allow   65000
             true   
        data:    ALLOW AZURE LOAD BALANCER INBOUND  AZURE_LOADBALANCER  *            *                *                 *         Inbound   Allow   65001
             true   
        data:    ALLOW INTERNET OUTBOUND            *                   *            INTERNET         *                 *         Outbound  Allow   65001
             true   
        data:    DENY ALL OUTBOUND                  *                   *            *                *                 *         Outbound  Deny    65500
             true   
        data:    DENY ALL INBOUND                   *                   *            *                *                 *         Inbound   Deny    65500
             true   
        info:    network nsg create command OK
   
    Parameters:
   
   * **-l (of --locatie)**. Azure-regio waar de nieuwe NSG wordt gemaakt. In ons scenario *westus*.
   * **-n (of --naam)**. Naam voor de nieuwe NSG. In ons scenario *NSG-FrontEnd*.
2. Voer de  **`azure network nsg rule create`**  opdracht maakt u een regel waarmee toegang tot poort 1433 (SQL) van de front-end-subnet.
   
        azure network nsg rule create -a NSG-BackEnd -n sql-rule -c Allow -p Tcp -r Inbound -y 100 -f 192.168.1.0/24 -o * -e * -u 1433
   
    Verwachte uitvoer:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "sql-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : sql-rule
        data:    Source address prefix           : 192.168.1.0/24
        data:    Source Port                     : *
        data:    Destination address prefix      : *
        data:    Destination Port                : 1433
        data:    Protocol                        : TCP
        data:    Type                            : Inbound
        data:    Action                          : Allow
        data:    Priority                        : 100
        info:    network nsg rule create command OK
3. Voer de  **`azure network nsg rule create`**  opdracht voor het maken van een regel die de toegang met Internet weigert.
   
        azure network nsg rule create -a NSG-BackEnd -n web-rule -c Deny -p Tcp -r Outbound -y 200 -f * -o * -e Internet -u 80
   
    Verwachte putput:
   
        info:    Executing command network nsg rule create
        info:    Looking up the network security group "NSG-BackEnd"
        info:    Creating a network security rule "web-rule"
        info:    Looking up the network security group "NSG-BackEnd"
        data:    Name                            : web-rule
        data:    Source address prefix           : *
        data:    Source Port                     : *
        data:    Destination address prefix      : INTERNET
        data:    Destination Port                : 80
        data:    Protocol                        : TCP
        data:    Type                            : Outbound
        data:    Action                          : Deny
        data:    Priority                        : 200
        info:    network nsg rule create command OK
4. Voer de  **`azure network nsg subnet add`**  opdracht het NSG koppelen aan het subnet voor back-end.
   
        azure network nsg subnet add -a NSG-BackEnd --vnet-name TestVNet --subnet-name BackEnd
   
    Verwachte uitvoer:
   
        info:    Executing command network nsg subnet add
        info:    Looking up the network security group "NSG-BackEndX"
        info:    Looking up the subnet "BackEnd"
        info:    Looking up network configuration
        info:    Creating a network security group "NSG-BackEndX"
        info:    network nsg subnet add command OK

