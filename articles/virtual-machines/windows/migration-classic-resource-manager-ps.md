---
title: Migreren naar Resource Manager met PowerShell | Microsoft Docs
description: Dit artikel helpt bij het platform ondersteunde migratie van IaaS-resources zoals virtuele machines (VM's), virtuele netwerken (VNETs) en storage-accounts van klassiek naar Azure Resource Manager (ARM) met behulp van Azure PowerShell-opdrachten
services: virtual-machines-windows
documentationcenter: ''
author: singhkays
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 02b3f4b0db46d05751569d465abf09310be91e8a
ms.sourcegitcommit: bd15a37170e57b651c54d8b194e5a99b5bcfb58f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/07/2019
ms.locfileid: "57549767"
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migreren IaaS-resources van klassiek naar Azure Resource Manager met behulp van Azure PowerShell
Deze stappen laten zien hoe u Azure PowerShell-opdrachten gebruiken voor het migreren van infrastructuur als een service (IaaS)-resources van het klassieke implementatiemodel naar het Azure Resource Manager-implementatiemodel.

Als u wilt, kunt u ook resources migreren met behulp van de [Azure Command Line Interface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* Zie voor achtergrondinformatie over ondersteunde migratiescenario's, [Platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md).
* Zie voor gedetailleerde richtlijnen en een overzicht van migratie [technische details van platform ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md)

<br>
Hier volgt een stroomdiagram voor het identificeren van de volgorde waarin stappen moeten worden uitgevoerd tijdens een migratie

![Schermafbeelding van de migratiestappen](media/migration-classic-resource-manager/migration-flow.png)

[!INCLUDE [updated-for-az-vm.md](../../../includes/updated-for-az-vm.md)]

## <a name="step-1-plan-for-migration"></a>Stap 1: Plan voor migratie
Hier volgen enkele aanbevolen procedures die wij adviseren kijkt u bij het evalueren van migratie IaaS-resources van klassiek naar Resource Manager:

* Lees de [ondersteund en niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md). Als u virtuele machines die gebruikmaken van niet-ondersteunde configuraties of functies hebt, raden wij u wacht de configuratie-/ functieondersteuning binnenkort aangekondigd. U kunt ook als deze aan uw behoeften voldoet, die functie te verwijderen of verplaatsen buiten die configuratie om in te schakelen van de migratie.
* Als u beschikken over een scripts die vandaag nog uw infrastructuur en toepassingen implementeren geautomatiseerde, proberen te maken van een vergelijkbare installatie van de test met behulp van deze scripts voor migratie. U kunt ook voorbeeldomgevingen instellen met behulp van de Azure-portal.

> [!IMPORTANT]
> Toepassingsgateways zijn momenteel niet ondersteund voor migratie van klassiek naar Resource Manager. Als u wilt migreren van een klassiek virtueel netwerk met een Application gateway, moet u de gateway verwijderen voordat u een voorbereidingsbewerking voor het verplaatsen van het netwerk uitvoert. Nadat u de migratie hebt voltooid, sluit u de gateway in Azure Resource Manager.
>
>ExpressRoute-gateways verbinding maakt met ExpressRoute-circuits in een ander abonnement worden niet automatisch gemigreerd. In dergelijke gevallen de ExpressRoute-gateway verwijderen, te migreren van het virtuele netwerk en maak de gateway opnieuw. Raadpleeg [migreren ExpressRoute-circuits en gekoppelde virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel](../../expressroute/expressroute-migration-classic-resource-manager.md) voor meer informatie.

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Stap 2: Installeer de nieuwste versie van Azure PowerShell
Er zijn twee manieren om Azure PowerShell te installeren: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) of [Web Platform Installer (WebPI)](https://aka.ms/webpi-azps). WebPI ontvangt maandelijkse updates. PowerShell Gallery ontvangt op continue basis-updates. In dit artikel is gebaseerd op Azure PowerShell versie 2.1.0.

Zie voor installatie-instructies [hoe u Azure PowerShell installeren en configureren](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a>Stap 3: Zorg ervoor dat u een administrator is voor het abonnement in Azure portal
Als u wilt deze migratie uitvoert, moet u toegevoegd als co-beheerder voor het abonnement op de [Azure-portal](https://portal.azure.com).

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **abonnement**. Als u dit niet ziet, selecteert u **alle services**.
3. Zoek de vermelding van het juiste abonnement en kijk vervolgens naar de **mijn rol** veld. Voor een CO-beheerder, de waarde moet _accountbeheerder_.

Als u niet kunt toevoegen van een CO-beheerder bent, klikt u vervolgens contact op met een servicebeheerder of CO-beheerder voor het abonnement om op te halen zelf toegevoegd.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>Stap 4: Uw abonnement instellen en zich registreren voor migratie
Start een PowerShell-prompt. Voor migratie, moet u voor het instellen van uw omgeving voor zowel klassieke en Resource Manager.

Aanmelden bij uw account voor het Resource Manager-model.

```powershell
    Connect-AzAccount
```

De beschikbare abonnementen ophalen met behulp van de volgende opdracht uit:

```powershell
    Get-AzSubscription | Sort Name | Select Name
```

Stel uw Azure-abonnement voor de huidige sessie. In het volgende voorbeeld wordt de naam van het standaard-abonnement op **mijn Azure-abonnement**. Vervang de naam van het voorbeeld-abonnement door uw eigen.

```powershell
    Select-AzSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Registratie is een eenmalige stap, maar u moet dit doen zodra voordat u probeert te migreren. Zonder te registreren ziet u de volgende strekking weergegeven:
>
> *BadRequest: Abonnement is niet geregistreerd voor migratie.*

Registreren bij de resourceprovider voor migratie met behulp van de volgende opdracht uit:

```powershell
    Register-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Wacht vijf minuten voor de registratie te voltooien. U kunt de status van de goedkeuring controleren met behulp van de volgende opdracht uit:

```powershell
    Get-AzResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Zorg ervoor dat RegistrationState `Registered` voordat u doorgaat.

Nu aanmelden bij uw account voor het klassieke model.

```powershell
    Add-AzureAccount
```

De beschikbare abonnementen ophalen met behulp van de volgende opdracht uit:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Stel uw Azure-abonnement voor de huidige sessie. In dit voorbeeld wordt het standaardabonnement ingesteld op **mijn Azure-abonnement**. Vervang de naam van het voorbeeld-abonnement door uw eigen.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 5: Zorg ervoor dat er voldoende virtuele Machine van Azure Resource Manager-vcpu's in de Azure-regio van uw huidige implementatie of VNET
U kunt de volgende PowerShell-opdracht gebruiken om te controleren of het huidige aantal vcpu's die u in Azure Resource Manager hebt. Zie voor meer informatie over vCPU-quota, [limieten en de Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-azure-resource-manager).

In dit voorbeeld controleert de beschikbaarheid de **VS-West** regio. Vervang de naam van de regio voorbeeld door uw eigen.

```powershell
Get-AzVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a>Stap 6: Opdrachten uitvoeren voor het migreren van uw IaaS-resources
* [Migreren van virtuele machines in een cloudservice (niet in een virtueel netwerk)](#step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network)
* [Migreren van virtuele machines in een virtueel netwerk](#step-61-option-2---migrate-virtual-machines-in-a-virtual-network)
* [Storage-Account migreren](#step-62-migrate-a-storage-account)

> [!NOTE]
> Alle bewerkingen die hier worden beschreven zijn idempotent. Als er een probleem met uitzondering van een niet-ondersteunde functie of een Configuratiefoutbericht wordt weergegeven, raden wij aan dat u het opnieuw de voorbereiden proberen afgebroken of doorgevoerd, bewerking. Het platform probeert vervolgens de actie opnieuw uit.


### <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Stap 6.1: Optie 1: migratie van virtuele machines in een cloudservice (niet in een virtueel netwerk)
De lijst met cloudservices ophalen met behulp van de volgende opdracht uit en kies vervolgens de service in de cloud die u wilt migreren. Als de virtuele machines in de cloudservice zich in een virtueel netwerk of als ze beschikken over de web-of werkrollen, de opdracht een foutbericht weergegeven wordt.

```powershell
    Get-AzureService | ft Servicename
```

Haal de implementatienaam voor de cloudservice. In dit voorbeeld wordt de servicenaam is **mijn Service**. Vervang de naam van de voorbeeld-service met de servicenaam van uw eigen.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Bereid de virtuele machines in de cloudservice voor migratie. U hebt twee opties om uit te kiezen.

* **Optie 1. De virtuele machines migreren naar een virtueel netwerk voor platform-gemaakt**

    Valideer eerst als u de cloudservice met de volgende opdrachten kunt migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    De volgende opdracht geeft alle waarschuwingen en fouten die migratie blokkeren. Als validatie gelukt is, dan u naar verplaatsen kunt de **voorbereiden** stap:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Optie 2. Migreren naar een bestaand virtueel netwerk in het Resource Manager-implementatiemodel**

    Dit voorbeeld wordt de naam van de resourcegroep op **myResourceGroup**, de naam van het virtuele netwerk naar **myVirtualNetwork** en de naam van het subnet naar **mySubNet**. De namen in het voorbeeld vervangen door de namen van uw eigen resources.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Valideer eerst als u het virtuele netwerk met de volgende opdracht kunt migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    De volgende opdracht geeft alle waarschuwingen en fouten die migratie blokkeren. Als de validatie geslaagd is, kunt klikt u vervolgens u doorgaan met de volgende voorbereidingsstap:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Nadat de bewerking voorbereiden is voltooid met een van de voorgaande opties, query uitvoeren op de migratiestatus van de virtuele machines. Zorg ervoor dat deze zich in de `Prepared` staat.

In dit voorbeeld wordt de naam van de virtuele machine ingesteld op **myVM**. De naam in dit voorbeeld vervangen door de naam van uw eigen virtuele machine.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

Controleer de configuratie voor de voorbereide resources met behulp van PowerShell of Azure portal. Als u nog niet klaar bent voor de migratie en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht uit:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Als de vooraf gedefinieerde configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht uit:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

### <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>Stap 6.1: Optie 2 - migreren van virtuele machines in een virtueel netwerk

Als u wilt migreren van virtuele machines in een virtueel netwerk, moet u het virtuele netwerk migreren. De virtuele machines migreren automatisch met het virtuele netwerk. Kies het virtuele netwerk dat u wilt migreren.
> [!NOTE]
> [Eén klassieke virtuele machine migreren](migrate-single-classic-to-resource-manager.md) door het maken van een nieuwe virtuele machine van Resource Manager met Managed Disks met behulp van de VHD (besturingssysteem en)-bestanden van de virtuele machine.
<br>

> [!NOTE]
> De naam van het virtuele netwerk kan afwijken van wat wordt weergegeven in de nieuwe Portal. De nieuwe Azure Portal geeft de naam op als `[vnet-name]` , maar de werkelijke virtuele-netwerknaam is van het type `Group [resource-group-name] [vnet-name]`. Voordat u migreert, zoeken de naam van de werkelijke virtuele netwerk met de opdracht `Get-AzureVnetSite | Select -Property Name` of weergeven in de oude Azure-Portal. 

In het volgende voorbeeld wordt de naam van het virtuele netwerk op **myVnet**. Vervang het voorbeeld van de virtuele-netwerknaam door uw eigen.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Als het virtuele netwerk web- of werkrollen of VM's met niet-ondersteunde configuraties bevat, krijgt u een validatiefoutbericht.

Valideer eerst als u het virtuele netwerk migreren kunt met behulp van de volgende opdracht uit:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

De volgende opdracht geeft alle waarschuwingen en fouten die migratie blokkeren. Als de validatie geslaagd is, kunt klikt u vervolgens u doorgaan met de volgende voorbereidingsstap:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Controleer de configuratie voor de voorbereide virtuele machines met behulp van Azure PowerShell of Azure portal. Als u nog niet klaar bent voor de migratie en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht uit:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Als de vooraf gedefinieerde configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht uit:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

### <a name="step-62-migrate-a-storage-account"></a>Stap 6.2 migreren een storage-account
Wanneer u klaar bent de virtuele machines migreert, is het beter om u de volgende vereiste controles voordat u migreert van de storage-accounts.

> [!NOTE]
> Als uw storage-account geen gekoppelde schijven of VM-gegevens heeft, gaat u rechtstreeks naar de **Storage-Account valideren en migratie Start** sectie.

* **Controles op vereisten zijn als u virtuele machines hebt gemigreerd, of uw storage-account schijfbronnen heeft**
    * **Migreren van klassieke virtuele machines waarvan schijven zijn opgeslagen in de storage-account**

        De volgende opdracht retourneert RoleName en DiskName eigenschappen van alle klassieke VM-schijven in de storage-account. RoleName is de naam van de virtuele machine waarop een schijf is aangesloten. Als deze opdracht retourneert schijven vervolgens zorgen ervoor dat de virtuele machines waarop deze schijven zijn gekoppeld worden gemigreerd voordat u migreert van het storage-account.
        ```powershell
         $storageAccountName = 'yourStorageAccountName'
          Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
          DiskName | Format-List -Property RoleName, DiskName

        ```
    * **Niet-gekoppelde klassieke VM-schijven die zijn opgeslagen in de storage-account verwijderen**

        Zoeken op niet-gekoppelde schijven in de klassieke virtuele machine in de storage-account met de volgende opdracht:

        ```powershell
            $storageAccountName = 'yourStorageAccountName'
            Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

        ```
        Als de bovenstaande opdracht verwijderen retourneert schijven vervolgens deze schijven van de volgende opdracht:

        ```powershell
           Remove-AzureDisk -DiskName 'yourDiskName'
        ```
    * **VM-installatiekopieën verwijderen die zijn opgeslagen in de storage-account**

        De volgende opdracht retourneert alle VM-installatiekopieën met besturingssysteemschijf die zijn opgeslagen in de storage-account.
         ```powershell
            Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                    } | Select-Object -Property ImageName, ImageLabel
         ```
         De volgende opdracht retourneert alle VM-installatiekopieën met gegevensschijven die zijn opgeslagen in de storage-account.
         ```powershell

            Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                             -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                            } | Select-Object -Property ImageName, ImageLabel
         ```
        Verwijder alle VM-installatiekopieën die worden geretourneerd door de bovenstaande opdrachten met de volgende opdracht:
        ```powershell
        Remove-AzureVMImage -ImageName 'yourImageName'
        ```
* **Valideren van de Storage-Account en migratie**

    Elk opslagaccount voor de migratie valideren met behulp van de volgende opdracht uit. In dit voorbeeld wordt de naam van het opslagaccount is **myStorageAccount**. De naam in dit voorbeeld vervangen door de naam van uw eigen opslagaccount.

    ```powershell
        $storageAccountName = "myStorageAccount"
        Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
    ```

    Volgende stap bestaat uit het opslagaccount voorbereiden voor migratie

    ```powershell
        $storageAccountName = "myStorageAccount"
        Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
    ```

    Controleer de configuratie voor de voorbereide storage-account met behulp van Azure PowerShell of Azure portal. Als u nog niet klaar bent voor de migratie en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht uit:

    ```powershell
        Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
    ```

    Als de vooraf gedefinieerde configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht uit:

    ```powershell
        Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
    ```

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van het platform ondersteunde migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technische details over door platforms ondersteunde migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-resources van klassiek naar Azure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Lees de veelgestelde vragen over migratie IaaS-resources van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
