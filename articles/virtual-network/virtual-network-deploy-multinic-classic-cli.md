---
title: Een virtuele machine (klassiek) maken met meerdere NIC's - Azure CLI 1.0 | Microsoft Docs
description: Informatie over het maken van een virtuele machine (klassiek) met meerdere NIC's met behulp van de Azure-opdrachtregelinterface (CLI) 1.0.
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: b436e41e-866c-439f-a7c7-7b4b041725ef
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/02/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e5569209d3628003b3f3e169b227e069b920c03f
ms.sourcegitcommit: 234c397676d8d7ba3b5ab9fe4cb6724b60cb7d25
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/20/2017
---
# <a name="create-a-vm-classic-with-multiple-nics-using-the-azure-cli-10"></a>Een virtuele machine (klassiek) maken met meerdere NIC's met behulp van de Azure CLI 1.0

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

U kunt virtuele machines (VM's) in Azure maken en koppelen van meerdere netwerkinterfaces (NIC's) naar elk van uw virtuele machines. Meerdere NIC's inschakelen scheiding van verkeerstypen tussen NIC's. Één NIC kan bijvoorbeeld communiceren met het Internet, terwijl een andere alleen met interne bronnen die niet is verbonden met Internet communiceert. De mogelijkheid voor het scheiden van netwerkverkeer via meerdere NIC's is vereist voor veel virtuele netwerkapparaten, zoals toepassingen en optimalisatie van WAN-oplossingen.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Meer informatie over hoe u deze stappen uitvoert met behulp van de [Resource Manager-implementatiemodel](../virtual-machines/linux/multiple-nics.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

De volgende stappen gebruikt u een resourcegroep met de naam *IaaSStory* voor de webservers en een resourcegroep met de naam *IaaSStory-back-end* voor de database-servers.

## <a name="prerequisites"></a>Vereisten
Voordat u de database-servers maken kunt, moet u maken de *IaaSStory* resourcegroep met alle benodigde resources voor dit scenario. Voltooi de stappen volgen voor het maken van deze resources. Een virtueel netwerk maken met de stappen in de [een virtueel netwerk maken](virtual-networks-create-vnet-classic-cli.md) artikel.

[!INCLUDE [azure-cli-prerequisites-include.md](../../includes/azure-cli-prerequisites-include.md)]

## <a name="deploy-the-back-end-vms"></a>De back-end virtuele machines implementeren
De back-end virtuele machines, is afhankelijk van het maken van de volgende bronnen:

* **Storage-account voor gegevensschijven**. Voor betere prestaties wordt de gegevensschijven op de databaseservers Solid-State station (SSD)-technologie, waarvoor een premium storage-account gebruiken. Zorg ervoor dat de Azure-locatie u implementeren ter ondersteuning van premium-opslag.
* **NIC's**. Elke virtuele machine heeft twee NIC's, één voor toegang tot de database, en één voor beheer.
* **Beschikbaarheidsset**. Alle databaseservers wordt toegevoegd aan een enkele beschikbaarheid instellen, zodat ten minste één van de virtuele machines actief is en wordt uitgevoerd tijdens het onderhoud.

### <a name="step-1---start-your-script"></a>Stap 1: uw script starten
U kunt downloaden via het volledige bash-script waarmee [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-cli.sh). Voer de volgende stappen uit om te wijzigen van het script te laten werken in uw omgeving:

1. Wijzig de waarden van de variabelen op basis van uw bestaande resourcegroep geïmplementeerd boven in [vereisten](#Prerequisites).

    ```azurecli
    location="useast2"
    vnetName="WTestVNet"
    backendSubnetName="BackEnd"
    ```
2. Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor uw back-end-implementatie.

    ```azurecli
    backendCSName="IaaSStory-Backend"
    prmStorageAccountName="iaasstoryprmstorage"
    image="0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1"
    avSetName="ASDB"
    vmSize="Standard_DS3"
    diskSize=127
    vmNamePrefix="DB"
    osDiskName="osdiskdb"
    dataDiskPrefix="db"
    dataDiskName="datadisk"
    ipAddressPrefix="192.168.2."
    username='adminuser'
    password='adminP@ssw0rd'
    numberOfVMs=2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Stap 2: de benodigde resources voor uw virtuele machines maken
1. Maak een nieuwe cloudservice voor alle back-end-VM's. Let op het gebruik van de `$backendCSName` variabele voor naam van de resourcegroep, en `$location` voor de Azure-regio.

    ```azurecli
    azure service create --serviceName $backendCSName \
        --location $location
    ```

2. Een premium storage-account voor het besturingssysteem en gegevensschijven kunnen worden gebruikt door uw virtuele machines maken.

    ```azurecli
    azure storage account create $prmStorageAccountName \
        --location $location \
        --type PLRS
    ```

### <a name="step-3---create-vms-with-multiple-nics"></a>Stap 3: virtuele machines maken met meerdere NIC 's
1. Start een lus voor het maken van meerdere virtuele machines, op basis van de `numberOfVMs` variabelen.

    ```azurecli
    for ((suffixNumber=1;suffixNumber<=numberOfVMs;suffixNumber++));
    do
    ```

2. Geef de naam en IP-adres van elk van de twee NIC's voor elke VM.

    ```azurecli
    nic1Name=$vmNamePrefix$suffixNumber-DA
    x=$((suffixNumber+3))
    ipAddress1=$ipAddressPrefix$x

    nic2Name=$vmNamePrefix$suffixNumber-RA
    x=$((suffixNumber+53))
    ipAddress2=$ipAddressPrefix$x
    ```

3. De virtuele machine maken. Let op het gebruik van de `--nic-config` parameter, met een lijst met alle NIC's met de naam, subnet en IP-adres.

    ```azurecli
    azure vm create $backendCSName $image $username $password \
        --connect $backendCSName \
        --vm-name $vmNamePrefix$suffixNumber \
        --vm-size $vmSize \
        --availability-set $avSetName \
        --blob-url $prmStorageAccountName.blob.core.windows.net/vhds/$osDiskName$suffixNumber.vhd \
        --virtual-network-name $vnetName \
        --subnet-names $backendSubnetName \
        --nic-config $nic1Name:$backendSubnetName:$ipAddress1::,$nic2Name:$backendSubnetName:$ipAddress2::
    ```

4. Maak twee gegevensschijven voor elke VM.

    ```azurecli
    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-1.vhd

    azure vm disk attach-new $vmNamePrefix$suffixNumber \
        $diskSize \
        vhds/$dataDiskPrefix$suffixNumber$dataDiskName-2.vhd
    done
    ```

### <a name="step-4---run-the-script"></a>Stap 4: Voer het script
Nu dat u hebt gedownload en gewijzigd van het script op basis van uw behoeften, voer het script voor het maken van de back-end database VM's met meerdere NIC's.

1. Sla uw script en voer dit uit uw **Bash** terminal. U ziet de uitvoer van de eerste zoals hieronder wordt weergegeven.

        info:    Executing command service create
        info:    Creating cloud service
        data:    Cloud service name IaaSStory-Backend
        info:    service create command OK
        info:    Executing command storage account create
        info:    Creating storage account
        info:    storage account create command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM

2. De uitvoering wordt beëindigd na een paar minuten en ziet u de rest van de uitvoer zoals hieronder wordt weergegeven.

        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm create
        info:    Looking up image 0b11de9248dd4d87b18621318e037d37__RightImage-Ubuntu-14.04-x64-v14.2.1
        info:    Looking up virtual network
        info:    Looking up cloud service
        info:    Getting cloud service properties
        info:    Looking up deployment
        info:    Creating VM
        info:    OK
        info:    vm create command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK
        info:    Executing command vm disk attach-new
        info:    Getting virtual machines
        info:    Adding Data-Disk
        info:    vm disk attach-new command OK

### <a name="step-5---configure-routing-within-the-vms-operating-system"></a>Stap 5: Configureer routering in het besturingssysteem van de VM

Azure DHCP toegewezen standaardgateway aan de eerste (primaire) netwerkinterface gekoppeld aan de virtuele machine. Azure wijst geen standaardgateway toe aan extra (secundaire) netwerkinterfaces die zijn gekoppeld aan een virtuele machine. Daarom kunt u standaard niet communiceren met resources buiten het subnet waarin een secundaire netwerkinterface zich bevindt. Secundaire netwerkinterfaces kunnen echter communiceren met bronnen buiten hun subnet. Zie configureren van routering voor secundaire netwerkinterfaces [routering binnen een besturingssysteem van de virtuele machine met meerdere netwerkinterfaces](virtual-network-network-interface-vm.md).
