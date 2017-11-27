---
title: Migreren naar Resource Manager met PowerShell | Microsoft Docs
description: Dit artikel helpt bij de migratie platform ondersteund van IaaS-resources, zoals virtuele machines (VM's), virtuele netwerken (vnet's) en storage-accounts van klassiek naar Azure Resource Manager (ARM) met behulp van Azure PowerShell-opdrachten
services: virtual-machines-windows
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 2b3dff9b-2e99-4556-acc5-d75ef234af9c
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 03/30/2017
ms.author: kasing
ms.openlocfilehash: 01bccb2f8d103faf77b39825a1f9ff663329ed7a
ms.sourcegitcommit: c7215d71e1cdeab731dd923a9b6b6643cee6eb04
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2017
---
# <a name="migrate-iaas-resources-from-classic-to-azure-resource-manager-by-using-azure-powershell"></a>Migreren IaaS-middelen van klassiek naar Azure Resource Manager met behulp van Azure PowerShell
Deze stappen ziet u hoe u Azure PowerShell-opdrachten voor het migreren van infrastructuur als een dienst (IaaS) resources van het klassieke implementatiemodel naar het Azure Resource Manager-implementatiemodel.

Als u wilt, kunt u ook resources migreren met behulp van de [Azure-opdrachtregelinterface (Azure CLI)](../linux/migration-classic-resource-manager-cli.md).

* Zie voor achtergrondinformatie over ondersteunde migratiescenario's [Platform ondersteund migratie van IaaS-middelen van klassiek naar Azure Resource Manager](migration-classic-resource-manager-overview.md).
* Zie voor gedetailleerde instructies en een overzicht van migratie [technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md).
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md)

<br>
Hier volgt een stroomdiagram voor het identificeren van de volgorde waarin stappen moeten worden uitgevoerd tijdens een migratie

![Schermafbeelding van de migratiestappen](media/migration-classic-resource-manager/migration-flow.png)

## <a name="step-1-plan-for-migration"></a>Stap 1: Plannen voor migratie
Hier volgen enkele aanbevolen procedures die wij adviseren kijkt u bij het evalueren van IaaS Resourcemigratie van klassiek naar Resource Manager:

* Lees de [ondersteunde en niet-ondersteunde functies en configuraties](migration-classic-resource-manager-overview.md). Als u virtuele machines die gebruikmaken van niet-ondersteunde configuraties of functies hebt, raden wij u voor ondersteuning van de configuratie of onderdeel worden aangekondigd. Ook als deze aan uw behoeften voldoet, verwijdert u deze functie of dat de configuratie om in te schakelen migratie verlaten.
* Als u scripts die uw infrastructuur en toepassingen vandaag implementeert geautomatiseerde, kunt u een vergelijkbare test-instellingen te maken met behulp van deze scripts voor de migratie. U kunt ook de voorbeeld-omgevingen instellen met behulp van de Azure-portal.

> [!IMPORTANT]
> Toepassingsgateways worden momenteel niet ondersteund voor migratie van klassiek naar Resource Manager. Verwijder de gateway voordat u een voorbereidingsbewerking voor het verplaatsen van het netwerk voor het migreren van een klassiek virtueel netwerk met een Application gateway. Nadat u de migratie hebt voltooid, sluit u de gateway in Azure Resource Manager.
>
>ExpressRoute-gateways die verbinding maken met ExpressRoute-circuits in een ander abonnement kunnen niet automatisch worden gemigreerd. In dergelijke gevallen verwijderen van de ExpressRoute-gateway, migreren van het virtuele netwerk en de gateway opnieuw maken. Zie [migreren ExpressRoute-circuits en bijbehorende virtuele netwerken van het klassieke naar het Resource Manager-implementatiemodel](../../expressroute/expressroute-migration-classic-resource-manager.md) voor meer informatie.
>
>

## <a name="step-2-install-the-latest-version-of-azure-powershell"></a>Stap 2: Installeer de nieuwste versie van Azure PowerShell
Er zijn twee manieren Azure PowerShell te installeren: [PowerShell Gallery](https://www.powershellgallery.com/profiles/azure-sdk/) of [Web Platform Installer (WebPI)](http://aka.ms/webpi-azps). WebPI ontvangt maandelijkse updates. PowerShell Gallery ontvangt updates doorlopend. In dit artikel is gebaseerd op Azure PowerShell versie 2.1.0.

Zie voor installatie-instructies [installeren en configureren van Azure PowerShell](/powershell/azure/overview).

<br>

## <a name="step-3-ensure-that-you-are-an-administrator-for-the-subscription-in-azure-portal"></a>Stap 3: Zorg ervoor dat u een beheerder zijn voor het abonnement in Azure-portal
Om deze te migreren, moet u toegevoegd als medebeheerder voor het abonnement op de [Azure-portal](https://portal.azure.com).

1. Meld u aan bij de [Azure Portal](https://portal.azure.com).
2. Selecteer in het menu Hub **abonnement**. Als u deze niet ziet, selecteert u **meer services**.
3. De abonnementvermelding in het juiste vinden en bekijk de **mijn rol** veld. Voor een medebeheerder de waarde moet _accountbeheerder_.

Als u niet kunt toevoegen van een CO-beheerder bent, klikt u vervolgens contact op met een servicebeheerder of CO-beheerder voor het abonnement om uzelf toegevoegd.   

## <a name="step-4-set-your-subscription-and-sign-up-for-migration"></a>Stap 4: Stel uw abonnement en aanmelden voor migratie
Eerst een PowerShell-prompt. Voor de migratie, moet u uw omgeving voor zowel klassieke instellen en Resource Manager.

Aanmelden bij uw account voor het Resource Manager-model.

```powershell
    Login-AzureRmAccount
```

De beschikbare abonnementen ophalen met behulp van de volgende opdracht:

```powershell
    Get-AzureRMSubscription | Sort Name | Select Name
```

Stel uw Azure-abonnement voor de huidige sessie. In het volgende voorbeeld wordt de standaardnaam voor het abonnement op **mijn Azure-abonnement**. De naam van het abonnement voorbeeld vervangen door uw eigen.

```powershell
    Select-AzureRmSubscription –SubscriptionName "My Azure Subscription"
```

> [!NOTE]
> Registratie is een eenmalige stap, maar u moet dit doen eenmaal voordat u de migratie. Zonder te registreren ziet u het volgende foutbericht weergegeven:
>
> *BadRequest: Abonnement is niet geregistreerd voor migratie.*
>
>

Registreren bij de resourceprovider voor migratie met behulp van de volgende opdracht:

```powershell
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Wacht vijf minuten om de registratie te voltooien. U kunt de status van de goedkeuring controleren met behulp van de volgende opdracht:

```powershell
    Get-AzureRmResourceProvider -ProviderNamespace Microsoft.ClassicInfrastructureMigrate
```

Zorg ervoor dat RegistrationState `Registered` voordat u doorgaat.

Nu aanmelden bij uw account voor het klassieke model.

```powershell
    Add-AzureAccount
```

De beschikbare abonnementen ophalen met behulp van de volgende opdracht:

```powershell
    Get-AzureSubscription | Sort SubscriptionName | Select SubscriptionName
```

Stel uw Azure-abonnement voor de huidige sessie. In dit voorbeeld wordt het standaardabonnement ingesteld op **mijn Azure-abonnement**. De naam van het abonnement voorbeeld vervangen door uw eigen.

```powershell
    Select-AzureSubscription –SubscriptionName "My Azure Subscription"
```

<br>

## <a name="step-5-make-sure-you-have-enough-azure-resource-manager-virtual-machine-vcpus-in-the-azure-region-of-your-current-deployment-or-vnet"></a>Stap 5: Controleer of er voldoende virtuele Machine van Azure Resource Manager vcpu's in de Azure-regio van uw huidige implementatie of VNET
U kunt de volgende PowerShell-opdracht gebruiken om te controleren van het huidige aantal vcpu's die u in Azure Resource Manager hebt. Zie voor meer informatie over quota vCPU, [limieten en de Azure Resource Manager](../../azure-subscription-service-limits.md#limits-and-the-azure-resource-manager).

In dit voorbeeld controleert de beschikbaarheid de **VS-West** regio. De regionaam voorbeeld vervangen door uw eigen.

```powershell
Get-AzureRmVMUsage -Location "West US"
```

## <a name="step-6-run-commands-to-migrate-your-iaas-resources"></a>Stap 6: Voer de opdrachten voor het migreren van uw IaaS-resources
> [!NOTE]
> Alle bewerkingen die hier wordt beschreven, zijn de idempotent. Als er een probleem dan een niet-ondersteunde functie of een configuratiefout, raden wij aan dat u opnieuw de voorbereiden proberen afgebroken of doorgevoerd, bewerking. Het platform worden vervolgens de actie opnieuw probeert.
>
>

## <a name="step-61-option-1---migrate-virtual-machines-in-a-cloud-service-not-in-a-virtual-network"></a>Stap 6.1: Optie 1 - migreren van virtuele machines in een cloudservice (niet in een virtueel netwerk)
Haal de lijst van cloudservices met behulp van de volgende opdracht uit en kies vervolgens de cloudservice die u wilt migreren. Als de virtuele machines in de cloudservice zich in een virtueel netwerk of als ze web-of werkrollen hebben, de opdracht wordt een foutbericht geretourneerd.

```powershell
    Get-AzureService | ft Servicename
```

De implementatienaam van de voor de cloudservice worden opgehaald. In dit voorbeeld wordt de servicenaam is **mijn Service**. De servicenaam voorbeeld vervangen door uw eigen servicenaam.

```powershell
    $serviceName = "My Service"
    $deployment = Get-AzureDeployment -ServiceName $serviceName
    $deploymentName = $deployment.DeploymentName
```

Bereid de virtuele machines in de cloudservice voor migratie. U hebben kunt kiezen uit twee opties.

* **Optie 1. De virtuele machines migreren naar een virtueel netwerk met platform gemaakt**

    Eerst valideren als u de cloudservice met de volgende opdrachten kunt migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    $validate.ValidationMessages
    ```

    De voorgaande opdracht geeft alle waarschuwingen en fouten die migratie blokkeren. Als validatie gelukt is, dan kunt u aan bij de **voorbereiden** stap:

    ```powershell
    Move-AzureService -Prepare -ServiceName $serviceName `
        -DeploymentName $deploymentName -CreateNewVirtualNetwork
    ```
* **Optie 2. Migreren naar een bestaand virtueel netwerk in het Resource Manager-implementatiemodel**

    Dit voorbeeld wordt de naam van de resourcegroep op **myResourceGroup**, de naam van het virtuele netwerk naar **myVirtualNetwork** en de subnetnaam van het wilt **mySubNet**. De namen in het voorbeeld vervangen door de namen van uw eigen resources.

    ```powershell
    $existingVnetRGName = "myResourceGroup"
    $vnetName = "myVirtualNetwork"
    $subnetName = "mySubNet"
    ```

    Eerst valideren als u het virtuele netwerk met de volgende opdracht kunt migreren:

    ```powershell
    $validate = Move-AzureService -Validate -ServiceName $serviceName `
        -DeploymentName $deploymentName -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName -VirtualNetworkName $vnetName -SubnetName $subnetName
    $validate.ValidationMessages
    ```

    De voorgaande opdracht geeft alle waarschuwingen en fouten die migratie blokkeren. Als validatie geslaagd is, kunt klikt u vervolgens u doorgaan met de volgende voorbereidingsstap:

    ```powershell
        Move-AzureService -Prepare -ServiceName $serviceName -DeploymentName $deploymentName `
        -UseExistingVirtualNetwork -VirtualNetworkResourceGroupName $existingVnetRGName `
        -VirtualNetworkName $vnetName -SubnetName $subnetName
    ```

Nadat de Prepare-bewerking is geslaagd met een van de voorgaande opties, query uitvoeren op de migratiestatus van de virtuele machines. Zorg ervoor dat ze in de `Prepared` staat.

Dit voorbeeld wordt de naam van de VM op **myVM**. De naam in dit voorbeeld vervangen door de naam van uw eigen VM.

```powershell
    $vmName = "myVM"
    $vm = Get-AzureVM -ServiceName $serviceName -Name $vmName
    $vm.VM.MigrationState
```

Controleer de configuratie voor de voorbereide resources met behulp van PowerShell of de Azure-portal. Als u niet klaar voor de migratie bent en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht:

```powershell
    Move-AzureService -Abort -ServiceName $serviceName -DeploymentName $deploymentName
```

Als de voorbereide configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht:

```powershell
    Move-AzureService -Commit -ServiceName $serviceName -DeploymentName $deploymentName
```

## <a name="step-61-option-2---migrate-virtual-machines-in-a-virtual-network"></a>Stap 6.1: Optie 2: migreren van virtuele machines in een virtueel netwerk

Als u wilt migreren van virtuele machines in een virtueel netwerk, moet u het virtuele netwerk migreren. De virtuele machines migreren automatisch met het virtuele netwerk. Kies het virtuele netwerk dat u wilt migreren.
> [!NOTE]
> [Migreren van één klassieke virtuele machine](migrate-single-classic-to-resource-manager.md) door het maken van een nieuwe Resource Manager-virtuele machine met beheerd schijven met behulp van de VHD (besturingssysteem en data)-bestanden van de virtuele machine.
<br>

> [!NOTE]
> Naam van het virtuele netwerk kan afwijken van wat wordt weergegeven in de nieuwe Portal. De nieuwe Azure-Portal geeft de naam op als `[vnet-name]` , maar de daadwerkelijke virtuele-netwerknaam is van het type `Group [resource-group-name] [vnet-name]`. Voordat u migreert, zoeken de naam van het werkelijke virtueel netwerk met de opdracht `Get-AzureVnetSite | Select -Property Name` of weergeven in de oude Azure-Portal. 

Dit voorbeeld wordt de naam van het virtuele netwerk op **myVnet**. De naam van het virtuele netwerk voorbeeld vervangen door uw eigen.

```powershell
    $vnetName = "myVnet"
```

> [!NOTE]
> Als het virtuele netwerk of werkrollen of VM's met niet-ondersteunde configuraties bevat, krijgt u een validatiefoutbericht.
>
>

Eerst valideren als u het virtuele netwerk migreren kunt met behulp van de volgende opdracht:

```powershell
    Move-AzureVirtualNetwork -Validate -VirtualNetworkName $vnetName
```

De voorgaande opdracht geeft alle waarschuwingen en fouten die migratie blokkeren. Als validatie geslaagd is, kunt klikt u vervolgens u doorgaan met de volgende voorbereidingsstap:

```powershell
    Move-AzureVirtualNetwork -Prepare -VirtualNetworkName $vnetName
```

Controleer de configuratie voor de voorbereide virtuele machines met behulp van Azure PowerShell of de Azure-portal. Als u niet klaar voor de migratie bent en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht:

```powershell
    Move-AzureVirtualNetwork -Abort -VirtualNetworkName $vnetName
```

Als de voorbereide configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht:

```powershell
    Move-AzureVirtualNetwork -Commit -VirtualNetworkName $vnetName
```

## <a name="step-62-migrate-a-storage-account"></a>Stap 6.2 migreren, een opslagaccount
Zodra u bent met klaar de virtuele machines migreert, wordt aangeraden migreren van de storage-accounts.

Voordat u het opslagaccount migreert, uitvoeren voorafgaand aan de vereiste controles in:

* **Klassieke virtuele machines waarvan schijven zijn opgeslagen in het opslagaccount migreren**

    Voorafgaand aan de opdracht RoleName eigenschappen en geretourneerd DiskName van alle klassieke VM-schijven in de storage-account. RoleName is de naam van de virtuele machine waaraan een schijf is gekoppeld. Als de voorgaande opdracht retourneert schijven vervolgens ervoor te zorgen dat virtuele machines die deze schijven zijn gekoppeld worden gemigreerd vóór de migratie van het opslagaccount.
    ```powershell
     $storageAccountName = 'yourStorageAccountName'
      Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Select-Object -ExpandProperty AttachedTo -Property `
      DiskName | Format-List -Property RoleName, DiskName

    ```
* **Niet-gekoppelde klassieke VM schijven die zijn opgeslagen in het opslagaccount verwijderd**

    Zoeken naar niet-gekoppelde schijven in de klassieke VM in de opslagruimte rekening met volgende opdracht:

    ```powershell
        $storageAccountName = 'yourStorageAccountName'
        Get-AzureDisk | where-Object {$_.MediaLink.Host.Contains($storageAccountName)} | Where-Object -Property AttachedTo -EQ $null | Format-List -Property DiskName  

    ```
    Als de bovenstaande opdracht retourneert schijven Verwijder deze schijven met volgende opdracht:

    ```powershell
       Remove-AzureDisk -DiskName 'yourDiskName'
    ```
* **VM-installatiekopieën verwijderen opgeslagen in de storage-account**

    Opdracht vóór retourneert alle VM-installatiekopieën met besturingssysteemschijf opgeslagen in het opslagaccount.
     ```powershell
        Get-AzureVmImage | Where-Object { $_.OSDiskConfiguration.MediaLink -ne $null -and $_.OSDiskConfiguration.MediaLink.Host.Contains($storageAccountName)`
                                } | Select-Object -Property ImageName, ImageLabel
     ```
     Opdracht vóór retourneert alle VM-installatiekopieën met gegevensschijven opgeslagen in het opslagaccount.
     ```powershell

        Get-AzureVmImage | Where-Object {$_.DataDiskConfigurations -ne $null `
                                         -and ($_.DataDiskConfigurations | Where-Object {$_.MediaLink -ne $null -and $_.MediaLink.Host.Contains($storageAccountName)}).Count -gt 0 `
                                        } | Select-Object -Property ImageName, ImageLabel
     ```
    Verwijder alle VM-installatiekopieën die worden geretourneerd door de bovenstaande opdrachten met behulp van de voorgaande opdracht:
    ```powershell
    Remove-AzureVMImage -ImageName 'yourImageName'
    ```

Elk opslagaccount voor de migratie valideren met behulp van de volgende opdracht. In dit voorbeeld wordt de naam van het opslagaccount is **myStorageAccount**. De naam in dit voorbeeld vervangen door de naam van uw eigen opslagaccount.

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Validate -StorageAccountName $storageAccountName
```

Volgende stap is het opslagaccount voorbereiden voor migratie

```powershell
    $storageAccountName = "myStorageAccount"
    Move-AzureStorageAccount -Prepare -StorageAccountName $storageAccountName
```

Controleer de configuratie voor de voorbereide storage-account met behulp van Azure PowerShell of de Azure-portal. Als u niet klaar voor de migratie bent en u wilt terugkeren naar de oude status, gebruikt u de volgende opdracht:

```powershell
    Move-AzureStorageAccount -Abort -StorageAccountName $storageAccountName
```

Als de voorbereide configuratie er goed uitziet, kunt u verder gaan en het doorvoeren van de resources met behulp van de volgende opdracht:

```powershell
    Move-AzureStorageAccount -Commit -StorageAccountName $storageAccountName
```

## <a name="next-steps"></a>Volgende stappen
* [Overzicht van de migratie van IaaS resources van klassieke in Azure Resource Manager-platform ondersteund](migration-classic-resource-manager-overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Technische deep dive op platform ondersteund migratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-deep-dive.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Planning voor de migratie van IaaS-resources van het klassieke implementatiemodel naar Azure Resource Manager](migration-classic-resource-manager-plan.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [CLI gebruiken voor het migreren van IaaS-middelen van klassiek naar Azure Resource Manager](../linux/migration-classic-resource-manager-cli.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Community-hulpprogramma's voor hulp bij de migratie van IaaS-middelen van klassiek naar Azure Resource Manager](migration-classic-resource-manager-community-tools.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Bekijk de meest voorkomende migratiefouten](migration-classic-resource-manager-errors.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
* [Lees de veelgestelde vragen over IaaS Resourcemigratie van klassiek naar Azure Resource Manager](migration-classic-resource-manager-faq.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)
