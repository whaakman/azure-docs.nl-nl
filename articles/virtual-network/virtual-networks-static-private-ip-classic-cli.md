---
title: "Configureren van privé IP-adressen voor virtuele machines (klassiek) - Azure CLI 1.0 | Microsoft Docs"
description: "Informatie over het configureren van privé IP-adressen voor virtuele machines (klassiek) met behulp van de Azure-opdrachtregelinterface (CLI) 1.0."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 17386acf-c708-4103-9b22-ff9bf04b778d
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/15/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ed0fe2fea20671063395b9ff089599853278989d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-cli-10"></a>Privé IP-adressen voor een virtuele machine (klassiek) met behulp van de Azure CLI 1.0 configureren

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dit artikel is van toepassing op het klassieke implementatiemodel. U kunt ook [een statisch privé IP-adres in het Resource Manager-implementatiemodel beheren](virtual-networks-static-private-ip-arm-cli.md).

De Azure CLI Voorbeeldopdrachten onderstaande verwacht een eenvoudige omgeving al gemaakt. Als u wilt de opdrachten uitvoeren zoals ze worden weergegeven in dit document, eerst de testomgeving wordt beschreven in bouwen [een vnet maken](virtual-networks-create-vnet-classic-cli.md).

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Een statisch privé IP-adres opgeven bij het maken van een virtuele machine
Maken van een nieuwe virtuele machine met de naam *DNS01* in een nieuwe cloudservice met de naam *TestService* op basis van het bovenstaande scenario, als volgt te werk:

1. Als u Azure CLI nog nooit hebt gebruikt, raadpleegt u [De Azure CLI installeren en configureren](../cli-install-nodejs.md) en volgt u de instructies tot het punt waar u uw Azure-account en -abonnement moet selecteren.
2. Voer de **azure service maken** opdracht voor het maken van de cloudservice.
   
        azure service create TestService --location uscentral
   
    Verwachte uitvoer:
   
        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name TestService
        info:    service create command OK
3. Voer de **azure virtuele machine maken** opdracht voor het maken van de virtuele machine. Let op de waarde voor een statisch privé IP-adres. De lijst die na de uitvoer wordt weergegeven, beschrijft de gebruikte parameters.
   
        azure vm create -l centralus -n DNS01 -w TestVNet -S "192.168.1.101" TestService bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2 adminuser AdminP@ssw0rd
   
    Verwachte uitvoer:
   
        info:    Executing command vm create
        warn:    --vm-size has not been specified. Defaulting to "Small".
        info:    Looking up image bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2
        info:    Looking up virtual network
        info:    Looking up cloud service
        warn:    --location option will be ignored
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Retrieving storage accounts
        info:    Creating VM
        info:    OK
        info:    vm create command OK
   
   * **-l (of --locatie)**. Azure-regio waar de virtuele machine wordt gemaakt. In ons scenario *centralus*.
   * **-n (of--vm-naam)**. Naam van de virtuele machine moet worden gemaakt.
   * **-w (of--virtuele-netwerknaam)**. De naam van de VNet waar de virtuele machine wordt gemaakt. 
   * **-S (of--statisch ip-)**. Statische privé IP-adres voor de virtuele machine.
   * **TestService**. Naam van de cloudservice waar de virtuele machine wordt gemaakt.
   * **bd507d3a70934695bc2128e3e5a255ba__RightImage-Windows-2012R2-x64-v14.2**. De afbeelding die wordt gebruikt voor het maken van de virtuele machine.
   * **adminuser**. Lokale beheerder voor de virtuele machine van Windows.
   * **AdminP@ssw0rd**. Lokale administrator-wachtwoord voor de virtuele machine van Windows.

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Het statische privé IP-adresgegevens voor een virtuele machine ophalen
Voer de volgende opdracht in de Azure CLI om weer te geven het statische privé IP-adresgegevens voor de virtuele machine met het bovenstaande script gemaakt, en houd rekening met de waarde voor *netwerk StaticIP*:

    azure vm static-ip show DNS01

Verwachte uitvoer:

    info:    Executing command vm static-ip show
    info:    Getting virtual machines
    data:    Network StaticIP "192.168.1.101"
    info:    vm static-ip show command OK

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Een statisch privé IP-adres van een virtuele machine verwijderen
Toegevoegd aan de virtuele machine in het bovenstaande script Voer de volgende opdracht in de Azure CLI het statische privé IP-adres verwijderen:

    azure vm static-ip remove DNS01

Verwachte uitvoer:

    info:    Executing command vm static-ip remove
    info:    Getting virtual machines
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip remove command OK

## <a name="how-to-add-a-static-private-ip-to-an-existing-vm"></a>Een statisch privé IP-adres toevoegen aan een bestaande virtuele machine
Toevoegen van een statisch privé IP-adres aan de virtuele machine gemaakt met behulp van het script hierboven runt de volgende opdracht:

    azure vm static-ip set DNS01 192.168.1.101

Verwachte uitvoer:

    info:    Executing command vm static-ip set
    info:    Getting virtual machines
    info:    Looking up virtual network
    info:    Reading network configuration
    info:    Updating network configuration
    info:    vm static-ip set command OK

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [gereserveerde openbare IP-adres](virtual-networks-reserved-public-ip.md) adressen.
* Meer informatie over [instantieniveau openbare IP (ILPIP)](virtual-networks-instance-level-public-ip.md) adressen.
* Raadpleeg de [gereserveerd IP-REST-API's](https://msdn.microsoft.com/library/azure/dn722420.aspx).

