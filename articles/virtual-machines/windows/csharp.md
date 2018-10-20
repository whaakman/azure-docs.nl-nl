---
title: Maken en beheren van een virtuele Machine van Azure met behulp van C# | Microsoft Docs
description: C# en Azure Resource Manager gebruiken om een virtuele machine en alle ondersteunende resources te implementeren.
services: virtual-machines-windows
documentationcenter: ''
author: zr-msft
manager: jeconnoc
editor: tysonn
tags: azure-resource-manager
ms.assetid: 87524373-5f52-4f4b-94af-50bf7b65c277
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: zarhoads
ms.openlocfilehash: 7281b2bfc8137e4f60c3309c9a57ccabf0fdb4cb
ms.sourcegitcommit: 62759a225d8fe1872b60ab0441d1c7ac809f9102
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/19/2018
ms.locfileid: "49471421"
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a>Windows-machines in Azure met behulp van C# maken en beheren #

Een [virtuele Azure-Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) moet diverse ondersteunende Azure-resources (VM). In dit artikel bevat informatie over het maken, beheren en verwijderen van VM-resources met behulp van C#. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een Visual Studio-project maken
> * Installeer het pakket
> * Referenties maken
> * Resources maken
> * Beheertaken uitvoeren
> * Resources verwijderen
> * De toepassing uitvoeren

Het duurt ongeveer 20 minuten deze stappen uitvoeren.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Als u niet hebt gedaan, installeert u [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Selecteer **.NET-desktopontwikkeling** op de pagina van de werkbelasting, en klik vervolgens op **installeren**. In de samenvatting, kunt u zien dat **.NET Framework 4-4.6 ontwikkelprogramma's** automatisch voor u is geselecteerd. Als u Visual Studio al hebt geïnstalleerd, kunt u de .NET-werkbelasting via Visual Studio starten toevoegen.
2. Klik in Visual Studio op **File** > **New** > **Project**.
3. In **sjablonen** > **Visual C#**, selecteer **Console-App (.NET Framework)**, voer *myDotnetProject* voor de naam van de Project, selecteer de locatie van het project en klik vervolgens op **OK**.

## <a name="install-the-package"></a>Installeer het pakket

NuGet-pakketten zijn de eenvoudigste manier voor het installeren van de bibliotheken die u moet deze stappen voltooien. Als u de bibliotheken die u nodig hebt in Visual Studio, voert u deze stappen uit:

1. Klik op **extra** > **Nuget Package Manager**, en klik vervolgens op **Package Manager Console**.
2. Typ deze opdracht in de console:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a>Referenties maken

Voordat u deze stap begint, zorg ervoor dat u toegang tot hebt een [Active Directory service-principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md). U moet ook de toepassings-ID, de verificatiesleutel en de tenant-ID die u nodig hebt in een latere stap vastleggen.

### <a name="create-the-authorization-file"></a>Het bestand met autorisatieregels maken

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C#-Items*. Noem het bestand *azureauth.properties*, en klik vervolgens op **toevoegen**.
2. Voeg deze eigenschappen autorisatie:

    ```
    subscription=<subscription-id>
    client=<application-id>
    key=<authentication-key>
    tenant=<tenant-id>
    managementURI=https://management.core.windows.net/
    baseURL=https://management.azure.com/
    authURL=https://login.windows.net/
    graphURL=https://graph.windows.net/
    ```

    Vervang **&lt;abonnement-id&gt;** met uw abonnements-id, **&lt;toepassing-id&gt;** met de Active Directory-toepassing id **&lt;verificatiesleutel&gt;** met de sleutel van de toepassing en **&lt;tenant-id&gt;** met de tenant-id.

3. Sla het bestand azureauth.properties. 
4. Een omgevingsvariabele instellen in Windows met de naam AZURE_AUTH_LOCATION met het volledige pad naar bestand met autorisatieregels die u hebt gemaakt. Bijvoorbeeld, kan de volgende PowerShell-opdracht worden gebruikt:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-the-management-client"></a>De management-client maken

1. Open het bestand Program.cs van het project dat u hebt gemaakt en voeg deze using-instructies toe aan de bestaande instructies aan de bovenkant van het bestand:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. Voor het maken van de management-client, voeg deze code toe aan de methode Main om:

    ```
    var credentials = SdkContext.AzureCredentialsFactory
        .FromFile(Environment.GetEnvironmentVariable("AZURE_AUTH_LOCATION"));

    var azure = Azure
        .Configure()
        .WithLogLevel(HttpLoggingDelegatingHandler.Level.Basic)
        .Authenticate(credentials)
        .WithDefaultSubscription();
    ```

## <a name="create-resources"></a>Resources maken

### <a name="create-the-resource-group"></a>De resourcegroep maken

Alle resources moeten worden opgenomen in een [resourcegroep](../../azure-resource-manager/resource-group-overview.md).

Geef waarden op voor de toepassing en maak de resourcegroep, voeg deze code toe aan de methode Main om:

```
var groupName = "myResourceGroup";
var vmName = "myVM";
var location = Region.USWest;
    
Console.WriteLine("Creating resource group...");
var resourceGroup = azure.ResourceGroups.Define(groupName)
    .WithRegion(location)
    .Create();
```

### <a name="create-the-availability-set"></a>De beschikbaarheidsset maken

[Beschikbaarheidssets](tutorial-availability-sets.md) maken het gemakkelijker voor u te houden van de virtuele machines die worden gebruikt door uw toepassing.

Voor het maken van de beschikbaarheidsset, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) is nodig om te communiceren met de virtuele machine.

Voor het maken van het openbare IP-adres voor de virtuele machine, voeg deze code toe aan de methode Main om:
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

Een virtuele machine moet zich in een subnet van een [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

Voor het maken van een subnet en een virtueel netwerk, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Creating virtual network...");
var network = azure.Networks.Define("myVNet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithAddressSpace("10.0.0.0/16")
    .WithSubnet("mySubnet", "10.0.0.0/24")
    .Create();
```

### <a name="create-the-network-interface"></a>De netwerkinterface maken

Een virtuele machine moet een netwerkinterface om te communiceren met het virtuele netwerk.

Voor het maken van een netwerkinterface, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Creating network interface...");
var networkInterface = azure.NetworkInterfaces.Define("myNIC")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetwork(network)
    .WithSubnet("mySubnet")
    .WithPrimaryPrivateIPAddressDynamic()
    .WithExistingPrimaryPublicIPAddress(publicIPAddress)
    .Create();
 ```

### <a name="create-the-virtual-machine"></a>De virtuele machine maken

Nu dat u de ondersteunende netwerkbronnen gemaakt, kunt u een virtuele machine kunt maken.

Voor het maken van de virtuele machine, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Creating virtual machine...");
azure.VirtualMachines.Define(vmName)
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .WithAdminUsername("azureuser")
    .WithAdminPassword("Azure12345678")
    .WithComputerName(vmName)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

> [!NOTE]
> Deze zelfstudie maakt u een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van andere installatiekopieën, [navigeren door en selecteren van installatiekopieën van virtuele machine met Windows PowerShell en de Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Als u gebruiken van een bestaande schijf in plaats van een marketplace-installatiekopie wilt, gebruikt u deze code:

```
var managedDisk = azure.Disks.Define("myosdisk")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd")
    .WithSizeInGB(128)
    .WithSku(DiskSkuTypes.PremiumLRS)
    .Create();

azure.VirtualMachines.Define("myVM")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithExistingPrimaryNetworkInterface(networkInterface)
    .WithSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows)
    .WithExistingAvailabilitySet(availabilitySet)
    .WithSize(VirtualMachineSizeTypes.StandardDS1)
    .Create();
```

## <a name="perform-management-tasks"></a>Beheertaken uitvoeren

Tijdens de levenscyclus van een virtuele machine wilt u mogelijk beheertaken uitvoeren, zoals het starten, stoppen of verwijderen van een virtuele machine. Bovendien kunt u maken van code om repetitieve of complexe taken te automatiseren.

Wanneer u iets te doen met de virtuele machine, moet u een exemplaar van het opvragen:

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-the-vm"></a>Informatie over de virtuele machine

Voor informatie over de virtuele machine, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Getting information about the virtual machine...");
Console.WriteLine("hardwareProfile");
Console.WriteLine("   vmSize: " + vm.Size);
Console.WriteLine("storageProfile");
Console.WriteLine("  imageReference");
Console.WriteLine("    publisher: " + vm.StorageProfile.ImageReference.Publisher);
Console.WriteLine("    offer: " + vm.StorageProfile.ImageReference.Offer);
Console.WriteLine("    sku: " + vm.StorageProfile.ImageReference.Sku);
Console.WriteLine("    version: " + vm.StorageProfile.ImageReference.Version);
Console.WriteLine("  osDisk");
Console.WriteLine("    osType: " + vm.StorageProfile.OsDisk.OsType);
Console.WriteLine("    name: " + vm.StorageProfile.OsDisk.Name);
Console.WriteLine("    createOption: " + vm.StorageProfile.OsDisk.CreateOption);
Console.WriteLine("    caching: " + vm.StorageProfile.OsDisk.Caching);
Console.WriteLine("osProfile");
Console.WriteLine("  computerName: " + vm.OSProfile.ComputerName);
Console.WriteLine("  adminUsername: " + vm.OSProfile.AdminUsername);
Console.WriteLine("  provisionVMAgent: " + vm.OSProfile.WindowsConfiguration.ProvisionVMAgent.Value);
Console.WriteLine("  enableAutomaticUpdates: " + vm.OSProfile.WindowsConfiguration.EnableAutomaticUpdates.Value);
Console.WriteLine("networkProfile");
foreach (string nicId in vm.NetworkInterfaceIds)
{
    Console.WriteLine("  networkInterface id: " + nicId);
}
Console.WriteLine("vmAgent");
Console.WriteLine("  vmAgentVersion" + vm.InstanceView.VmAgent.VmAgentVersion);
Console.WriteLine("    statuses");
foreach (InstanceViewStatus stat in vm.InstanceView.VmAgent.Statuses)
{
    Console.WriteLine("    code: " + stat.Code);
    Console.WriteLine("    level: " + stat.Level);
    Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
    Console.WriteLine("    message: " + stat.Message);
    Console.WriteLine("    time: " + stat.Time);
}
Console.WriteLine("disks");
foreach (DiskInstanceView disk in vm.InstanceView.Disks)
{
    Console.WriteLine("  name: " + disk.Name);
    Console.WriteLine("  statuses");
    foreach (InstanceViewStatus stat in disk.Statuses)
    {
        Console.WriteLine("    code: " + stat.Code);
        Console.WriteLine("    level: " + stat.Level);
        Console.WriteLine("    displayStatus: " + stat.DisplayStatus);
        Console.WriteLine("    time: " + stat.Time);
    }
}
Console.WriteLine("VM general status");
Console.WriteLine("  provisioningStatus: " + vm.ProvisioningState);
Console.WriteLine("  id: " + vm.Id);
Console.WriteLine("  name: " + vm.Name);
Console.WriteLine("  type: " + vm.Type);
Console.WriteLine("  location: " + vm.Region);
Console.WriteLine("VM instance status");
foreach (InstanceViewStatus stat in vm.InstanceView.Statuses)
{
    Console.WriteLine("  code: " + stat.Code);
    Console.WriteLine("  level: " + stat.Level);
    Console.WriteLine("  displayStatus: " + stat.DisplayStatus);
}
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="stop-the-vm"></a>De virtuele machine stoppen

U kunt een virtuele machine stoppen en alle bijbehorende instellingen behouden, maar nog steeds in rekening gebracht voor het, of u kunt een virtuele machine stoppen en deze toewijzing. Wanneer een virtuele machine ongedaan is gemaakt, worden alle resources die zijn gekoppeld aan deze ook ongedaan gemaakt en facturering-ends voor het.

Als u wilt de virtuele machine stoppen zonder deze toewijzing ongedaan maken..., voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

Als u wilt de toewijzing van de virtuele machine, wijzigt u de aanroep zijn uitgeschakeld in deze code:

```
vm.Deallocate();
```

### <a name="start-the-vm"></a>De virtuele machine starten

Voor het starten van de virtuele machine, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="resize-the-vm"></a>Grootte van de virtuele machine wijzigen

Veel aspecten van de implementatie moeten worden overwogen bij het bepalen van een grootte voor uw virtuele machine. Zie voor meer informatie, [VM-grootten](sizes.md).  

Als de grootte van de virtuele machine wilt wijzigen, voeg deze code toe aan de methode Main om:

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>Een gegevensschijf toevoegen aan de virtuele machine

Een gegevensschijf toevoegen aan de virtuele machine, voeg deze code toe aan de methode Main om toe te voegen een gegevensschijf die is 2 GB groot, han een LUN van 0 en een type opslaan in cache van ReadWrite om:

```
Console.WriteLine("Adding data disk to vm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter to delete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a>Resources verwijderen

Omdat u voor resources die worden gebruikt in Azure betaalt, is het altijd verstandig resources verwijderen die niet langer nodig zijn. Als u wilt verwijderen van de virtuele machines en de ondersteunende resources, u hoeft te doen, is de resourcegroep verwijderen.

Als u wilt verwijderen van de resourcegroep, voeg deze code toe aan de methode Main om:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Het duurt ongeveer vijf minuten voor deze consoletoepassing volledig uitvoeren vanaf het begin te voltooien. 

1. Als u wilt de consoletoepassing uitvoeren, klikt u op **Start**.

2. Voordat u druk op **Enter** om te beginnen met verwijderen van resources, kunt u een paar minuten om te controleren of het maken van de resources in Azure portal kunt nemen. Klik op de status van de implementatie als u wilt weergeven over de implementatie.

## <a name="next-steps"></a>Volgende stappen
* Profiteer van het gebruik van een sjabloon voor een virtuele machine maken met behulp van de informatie in [een Azure-Machine met behulp van C# en Resource Manager-sjabloon implementeren](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Meer informatie over het gebruik van de [Azure-bibliotheken voor .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).

