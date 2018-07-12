---
title: Een virtuele machine (klassiek) maken met meerdere NIC's - Azure PowerShell | Microsoft Docs
description: Informatie over het maken van een virtuele machine (klassiek) met meerdere NIC's met behulp van PowerShell.
services: virtual-network
documentationcenter: na
author: genlin
manager: cshepard
editor: ''
tags: azure-service-management
ms.assetid: 6e50f39a-2497-4845-a5d4-7332dbc203c5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/22/2018
ms.author: genli
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ca4e9e77d0e0ca62c04fbbfe132a41fb3e01df46
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38477655"
---
# <a name="create-a-vm-classic-with-multiple-nics-using-powershell"></a>Een virtuele machine (klassiek) maken met meerdere NIC's met behulp van PowerShell

[!INCLUDE [virtual-network-deploy-multinic-classic-selectors-include.md](../../includes/virtual-network-deploy-multinic-classic-selectors-include.md)]

U kunt virtuele machines (VM's) in Azure maken en koppelen van meerdere netwerkinterfaces (NIC's) aan elk van uw virtuele machines. Meerdere NIC's bieden scheiding van verkeerstypen voor de NIC's. Één NIC kan bijvoorbeeld communiceren met Internet, terwijl andere alleen met interne bronnen niet is verbonden met Internet communiceert. De mogelijkheid voor het scheiden van netwerkverkeer tussen meerdere NIC's is vereist voor veel virtuele netwerkapparaten, zoals de levering van toepassingen en oplossingen van WAN-optimalisatie.

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en het klassieke model](../resource-manager-deployment-model.md). Dit artikel gaat over het gebruik van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Meer informatie over het uitvoeren van deze stappen met behulp van de [Resource Manager-implementatiemodel](../virtual-machines/windows/multiple-nics.md).

[!INCLUDE [virtual-network-deploy-multinic-scenario-include.md](../../includes/virtual-network-deploy-multinic-scenario-include.md)]

De volgende stappen gebruikgemaakt van een resourcegroep met de naam *IaaSStory* voor de webservers en een resourcegroep met de naam *IaaSStory-back-end* voor de DB-servers.

## <a name="prerequisites"></a>Vereisten

Voordat u de DB-servers maken kunt, moet u maken de *IaaSStory* resourcegroep met alle benodigde bronnen voor dit scenario. Voltooi de volgende stappen voor het maken van deze resources. Een virtueel netwerk maken met de volgende stappen in de [een virtueel netwerk maken](virtual-networks-create-vnet-classic-netcfg-ps.md) artikel.

[!INCLUDE [azure-ps-prerequisites-include.md](../../includes/azure-ps-prerequisites-include.md)]

## <a name="create-the-back-end-vms"></a>De back-end-VM's maken
De back-end-VM's, is afhankelijk van het maken van de volgende bronnen:

* **Back-endsubnet**. De database-servers uitmaken deel van een apart subnet, om te scheiden van verkeer. Het onderstaande script wordt verwacht dat dit subnet bestaat in een vnet met de naam *WTestVnet*.
* **Storage-account voor gegevensschijven**. Voor betere prestaties, de gegevensschijven op de databaseservers Solid-state drive (SSD)-technologie, waarvoor een premium storage-account gebruikt. Zorg ervoor dat de Azure-locatie u implementeren ter ondersteuning van premium-opslag.
* **Beschikbaarheidsset**. Alle databaseservers worden, toegevoegd aan een enkele beschikbaarheid instellen, zodat ten minste één van de virtuele machines actief is en wordt tijdens onderhoud worden uitgevoerd.

### <a name="step-1---start-your-script"></a>Stap 1: uw script starten
U kunt het volledige PowerShell-script gebruikt downloaden [hier](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/IaaS-Story/11-MultiNIC/classic/virtual-network-deploy-multinic-classic-ps.ps1). Volg de stappen hieronder om het script te laten werken in uw omgeving wijzigen.

1. Wijzig de waarden van de variabelen op basis van uw bestaande resourcegroep die hierboven zijn geïmplementeerd [vereisten](#Prerequisites).

    ```powershell
    $location              = "West US"
    $vnetName              = "WTestVNet"
    $backendSubnetName     = "BackEnd"
    ```

2. Wijzig de waarden van de variabelen op basis van de waarden die u wilt gebruiken voor uw back-end-implementatie.

    ```powershell
    $backendCSName         = "IaaSStory-Backend"
    $prmStorageAccountName = "iaasstoryprmstorage"
    $avSetName             = "ASDB"
    $vmSize                = "Standard_DS3"
    $diskSize              = 127
    $vmNamePrefix          = "DB"
    $dataDiskSuffix        = "datadisk"
    $ipAddressPrefix       = "192.168.2."
    $numberOfVMs           = 2
    ```

### <a name="step-2---create-necessary-resources-for-your-vms"></a>Stap 2: resources die nodig zijn voor uw VM's maken
U moet een nieuwe cloudservice en een opslagaccount voor de gegevensschijven maken voor alle virtuele machines. U moet ook een afbeelding, en een lokale administrator-account opgeven voor de virtuele machines. Voor het maken van deze resources, voert u de volgende stappen uit:

1. Maak een nieuwe cloudservice.

    ```powershell
    New-AzureService -ServiceName $backendCSName -Location $location
    ```

2. Maak een nieuwe premium storage-account.

    ```powershell
    New-AzureStorageAccount -StorageAccountName $prmStorageAccountName `
    -Location $location -Type Premium_LRS
    ```
3. Het opslagaccount dat hierboven is gemaakt als het huidige opslagaccount voor uw abonnement instellen.

    ```powershell
    $subscription = Get-AzureSubscription | where {$_.IsCurrent -eq $true}  
    Set-AzureSubscription -SubscriptionName $subscription.SubscriptionName `
    -CurrentStorageAccountName $prmStorageAccountName
    ```

4. Selecteer een installatiekopie voor de virtuele machine.

    ```powershell
    $image = Get-AzureVMImage `
    | where{$_.ImageFamily -eq "SQL Server 2014 RTM Web on Windows Server 2012 R2"} `
    | sort PublishedDate -Descending `
    | select -ExpandProperty ImageName -First 1
    ```

5. De lokale beheerder accountreferenties ingesteld.

    ```powershell
    $cred = Get-Credential -Message "Enter username and password for local admin account"
    ```

### <a name="step-3---create-vms"></a>Stap 3: virtuele machines maken
U moet een lus gebruiken zo veel virtuele machines als u wilt gebruiken en maken van de benodigde NIC's en VM's binnen de lus maken. Voor het maken van de NIC's en VM's, de volgende stappen worden uitgevoerd.

1. Start een `for` herhalen als u wilt herhalen van de opdrachten voor het maken van een virtuele machine en twee NIC's zo vaak als nodig, op basis van de waarde van de `$numberOfVMs` variabele.

    ```powershell
    for ($suffixNumber = 1; $suffixNumber -le $numberOfVMs; $suffixNumber++){
    ```

2. Maak een `VMConfig` object op te geven van de afbeelding, de grootte en de beschikbaarheidsset voor de virtuele machine.

    ```powershell
    $vmName = $vmNamePrefix + $suffixNumber
    $vmConfig = New-AzureVMConfig -Name $vmName `
        -ImageName $image `
        -InstanceSize $vmSize `
        -AvailabilitySetName $avSetName
    ```

3. De virtuele machine inrichten als een Windows VM.

    ```powershell
    Add-AzureProvisioningConfig -VM $vmConfig -Windows `
        -AdminUsername $cred.UserName `
        -Password $cred.GetNetworkCredential().Password
    ```

4. De standaardwaarde NIC instellen en deze een statisch IP-adres toewijzen.

    ```powershell
    Set-AzureSubnet         -SubnetNames $backendSubnetName -VM $vmConfig
    Set-AzureStaticVNetIP   -IPAddress ($ipAddressPrefix+$suffixNumber+3) -VM $vmConfig
    ```

5. Voeg een tweede NIC toe voor elke virtuele machine.

    ```powershell
    Add-AzureNetworkInterfaceConfig -Name ("RemoteAccessNIC"+$suffixNumber) `
    -SubnetName $backendSubnetName `
    -StaticVNetIPAddress ($ipAddressPrefix+(53+$suffixNumber)) `
    -VM $vmConfig
    ```
    
6. Voor elke virtuele machine maken op gegevensschijven.

    ```powershell
    $dataDisk1Name = $vmName + "-" + $dataDiskSuffix + "-1"    
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk1Name `
    -LUN 0

    $dataDisk2Name = $vmName + "-" + $dataDiskSuffix + "-2"   
    Add-AzureDataDisk -CreateNew -VM $vmConfig `
    -DiskSizeInGB $diskSize `
    -DiskLabel $dataDisk2Name `
    -LUN 1
    ```

7. Elke virtuele machine maakt en eindigen van de lus.

    ```powershell
    New-AzureVM -VM $vmConfig `
    -ServiceName $backendCSName `
    -Location $location `
    -VNetName $vnetName
    }
    ```

### <a name="step-4---run-the-script"></a>Stap 4: het script uitvoeren
Nu dat u hebt gedownload en gewijzigd het script op basis van uw behoeften, runt het script voor het maken van de database van de back-end VM's met meerdere NIC's.

1. Het script opslaan en uitvoeren vanuit de **PowerShell** opdrachtprompt of **PowerShell ISE**. U ziet de uitvoer van de eerste zoals hieronder wordt weergegeven.

        OperationDescription    OperationId                          OperationStatus

        New-AzureService        xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureStorageAccount xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        
        WARNING: No deployment found in service: 'IaaSStory-Backend'.
2. Vul de informatie die nodig is in de referentieprompt en klik op **OK**. De volgende uitvoer wordt geretourneerd.

        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded
        New-AzureVM             xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx Succeeded

### <a name="step-5---configure-routing-within-the-vms-operating-system"></a>Stap 5: Configureer routering in het besturingssysteem van de virtuele machine

Azure DHCP wijst geen standaardgateway toe aan de eerste (primaire) netwerkinterface die is gekoppeld aan de virtuele machine. Azure wijst geen standaardgateway toe aan extra (secundaire) netwerkinterfaces die zijn gekoppeld aan een virtuele machine. Daarom kunt u standaard niet communiceren met resources buiten het subnet waarin een secundaire netwerkinterface zich bevindt. Secundaire netwerkinterfaces kunnen echter wel communiceren met resources buiten hun subnet. Voor het configureren van routering voor secundaire netwerkinterfaces, Zie de volgende artikelen:

- [Een Windows-VM configureren voor meerdere NIC 's](../virtual-machines/windows/multiple-nics.md#configure-guest-os-for-multiple-nics
)

- [Een Linux-VM voor meerdere NIC's configureren](../virtual-machines/linux/multiple-nics.md#configure-guest-os-for-multiple-nics
)
