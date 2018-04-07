---
title: Maken en beheren van een virtuele Machine van Azure met C# | Microsoft Docs
description: C# en Azure Resource Manager gebruiken voor het implementeren van een virtuele machine en de ondersteunende bronnen.
services: virtual-machines-windows
documentationcenter: ''
author: davidmu1
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
ms.author: davidmu
ms.openlocfilehash: 43dcc8e5d9756807a3e005d629e84469a2e6eb04
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="create-and-manage-windows-vms-in-azure-using-c"></a>Maken en beheren van Windows-machines in Azure met C# #

Een [Azure virtuele Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) moet meerdere ondersteunende Azure-resources. In dit artikel bevat informatie over het maken, beheren en het verwijderen van de VM-netwerkbronnen met C#. Procedures voor:

> [!div class="checklist"]
> * Een Visual Studio-project maken
> * Installeer het pakket
> * Referenties maken
> * Resources maken
> * Beheertaken uitvoeren
> * Resources verwijderen
> * De toepassing uitvoeren

Het duurt ongeveer 20 minuten aan deze stappen uit te voeren.

## <a name="create-a-visual-studio-project"></a>Een Visual Studio-project maken

1. Als u nog niet gedaan hebt, installeert u [Visual Studio](https://docs.microsoft.com/visualstudio/install/install-visual-studio). Selecteer **.NET-ontwikkeling voor bureaubladen** op de pagina werkbelastingen en klik vervolgens op **installeren**. In de samenvatting, kunt u zien dat **.NET Framework 4 4.6 ontwikkelingsprogramma's** automatisch voor u is geselecteerd. Als u Visual Studio al hebt geïnstalleerd, kunt u de .NET-werkbelasting met de Visual Studio starten toevoegen.
2. Klik in Visual Studio **bestand** > **nieuw** > **Project**.
3. In **sjablonen** > **Visual C#**, selecteer **Console-App (.NET Framework)**, voer *myDotnetProject* voor de naam van het project, selecteer de locatie van het project en klik vervolgens op **OK**.

## <a name="install-the-package"></a>Installeer het pakket

NuGet-pakketten zijn de eenvoudigste manier om de bibliotheken die u moet deze stappen hebt geïnstalleerd. Als u de bibliotheken die u nodig hebt in Visual Studio, voer deze stappen uit:

1. Klik op **extra** > **Nuget Package Manager**, en klik vervolgens op **Package Manager Console**.
2. Typ deze opdracht in de console:

    ```
    Install-Package Microsoft.Azure.Management.Fluent
    ```

## <a name="create-credentials"></a>Referenties maken

Voordat u deze stap, zorg ervoor dat u toegang tot hebben een [Active Directory-service-principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md). U moet ook de toepassings-ID, de verificatiesleutel en de tenant-ID die u nodig hebt in een later stadium vastleggen.

### <a name="create-the-authorization-file"></a>Maken van het bestand met autorisatieregels

1. Klik in Solution Explorer met de rechtermuisknop op *myDotnetProject* > **toevoegen** > **Nieuw Item**, en selecteer vervolgens **tekstbestand** in *Visual C# Items*. Noem het bestand *azureauth.properties*, en klik vervolgens op **toevoegen**.
2. Deze eigenschappen van autorisatie toevoegen:

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

    Vervang **&lt;abonnement-id&gt;** met uw abonnements-id **&lt;toepassing-id&gt;** met de toepassings-id van het Active Directory **&lt;verificatiesleutel&gt;** voor de Toepassingssleutel, en **&lt;tenant-id&gt;** met tenant-id.

3. Sla het bestand azureauth.properties. 
4. Stelt de omgevingsvariabele in Windows met de naam AZURE_AUTH_LOCATION met het volledige pad naar het bestand met autorisatieregels die u hebt gemaakt. De volgende PowerShell-opdracht kan bijvoorbeeld worden gebruikt:

    ```
    [Environment]::SetEnvironmentVariable("AZURE_AUTH_LOCATION", "C:\Visual Studio 2017\Projects\myDotnetProject\myDotnetProject\azureauth.properties", "User")
    ```

### <a name="create-the-management-client"></a>De management-client maken

1. Open het bestand Program.cs voor het project dat u hebt gemaakt en voeg vervolgens deze using-instructies aan de bestaande instructies aan de bovenkant van het bestand:

    ```
    using Microsoft.Azure.Management.Compute.Fluent;
    using Microsoft.Azure.Management.Compute.Fluent.Models;
    using Microsoft.Azure.Management.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent;
    using Microsoft.Azure.Management.ResourceManager.Fluent.Core;
    ```

2. Voeg deze code aan de Main-methode voor het maken van de management-client:

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

Alle resources moeten zijn opgenomen in een [resourcegroep](../../azure-resource-manager/resource-group-overview.md).

Geef waarden op voor de toepassing en de resourcegroep maken, voeg deze code aan de Main-methode:

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

[Beschikbaarheidssets](tutorial-availability-sets.md) het eenvoudiger voor u de virtuele machines die worden gebruikt door uw toepassing te onderhouden.

Voeg deze code aan de Main-methode voor het maken van de beschikbaarheidsset:

```
Console.WriteLine("Creating availability set...");
var availabilitySet = azure.AvailabilitySets.Define("myAVSet")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithSku(AvailabilitySetSkuTypes.Managed)
    .Create();
```

### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) nodig is om te communiceren met de virtuele machine.

Voeg deze code aan de Main-methode voor het maken van het openbare IP-adres voor de virtuele machine:
   
```
Console.WriteLine("Creating public IP address...");
var publicIPAddress = azure.PublicIPAddresses.Define("myPublicIP")
    .WithRegion(location)
    .WithExistingResourceGroup(groupName)
    .WithDynamicIP()
    .Create();
```

### <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

Een virtuele machine moet worden in een subnet van een [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

Voor het maken van een subnet en een virtueel netwerk, moet u deze code toevoegen aan de Main-methode:

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

Voeg deze code aan de Main-methode voor het maken van een netwerkinterface:

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

Nu u de ondersteunende resources hebt gemaakt, kunt u een virtuele machine maken.

Voeg deze code aan de Main-methode voor het maken van de virtuele machine:

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
> Deze zelfstudie maakt een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van de andere afbeeldingen, [navigeren door en selecteren van installatiekopieën van virtuele machine van Azure met Windows PowerShell en Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Als u gebruiken van een bestaande schijf in plaats van een marketplace-installatiekopie wilt, moet u deze code gebruiken:

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

Tijdens de levenscyclus van een virtuele machine wilt u mogelijk beheertaken uitvoeren, zoals het starten, stoppen of verwijderen van een virtuele machine. Bovendien wilt u maken code om herhaalde of complexe taken te automatiseren.

Wanneer u iets te doen met de virtuele machine, moet u een exemplaar van het:

```
var vm = azure.VirtualMachines.GetByResourceGroup(groupName, vmName);
```

### <a name="get-information-about-the-vm"></a>Informatie ophalen over de virtuele machine

Voor informatie over de virtuele machine, moet u deze code toevoegen aan de Main-methode:

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

U kunt een virtuele machine stoppen en alle bijbehorende instellingen behouden, maar blijven in rekening gebracht voor of u kunt een virtuele machine stoppen en deze ongedaan gemaakt. Wanneer een virtuele machine in de toewijzing is opgeheven, zijn alle resources die zijn gekoppeld ook toewijzing ongedaan is gemaakt en facturering eindigt voor.

Om te stoppen van de virtuele machine zonder deze toewijzing, moet u deze code toevoegen aan de Main-methode:

```
Console.WriteLine("Stopping vm...");
vm.PowerOff();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

Als u wilt dat de virtuele machine ongedaan, wijzigt u de aanroep uitgeschakeld in deze code:

```
vm.Deallocate();
```

### <a name="start-the-vm"></a>Start de virtuele machine

Voeg deze code aan de Main-methode voor het starten van de virtuele machine:

```
Console.WriteLine("Starting vm...");
vm.Start();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="resize-the-vm"></a>De virtuele machine vergroten of verkleinen

Veel aspecten van de implementatie moeten worden beschouwd bij het kiezen van een grootte voor de virtuele machine. Zie voor meer informatie [VM-grootten](sizes.md).  

Als de grootte van de virtuele machine wijzigen, moet u deze code toevoegen aan de Main-methode:

```
Console.WriteLine("Resizing vm...");
vm.Update()
    .WithSize(VirtualMachineSizeTypes.StandardDS2) 
    .Apply();
Console.WriteLine("Press enter to continue...");
Console.ReadLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>Een gegevensschijf toevoegen aan de virtuele machine

Als u wilt een gegevensschijf toevoegen aan de virtuele machine, moet u deze code toevoegen aan de Main-methode voor het toevoegen van een gegevensschijf dat is 2 GB groot en han een LUN van 0 en een cache in type ReadWrite:

```
Console.WriteLine("Adding data disk to vm...");
vm.Update()
    .WithNewDataDisk(2, 0, CachingTypes.ReadWrite) 
    .Apply();
Console.WriteLine("Press enter to delete resources...");
Console.ReadLine();
```

## <a name="delete-resources"></a>Resources verwijderen

Omdat u in rekening voor resources die worden gebruikt in Azure gebracht, maar het is altijd raadzaam om resources verwijderen die niet langer nodig zijn. Als u wilt verwijderen van de virtuele machines en de ondersteunende resources, is hoeft u de resourcegroep verwijderen.

Voeg deze code aan de Main-methode voor het verwijderen van de resourcegroep:

```
azure.ResourceGroups.DeleteByName(groupName);
```

## <a name="run-the-application"></a>De toepassing uitvoeren

Het moet over vijf minuten duren voordat deze consoletoepassing volledig uitvoeren vanaf het begin te voltooien. 

1. Als u wilt de consoletoepassing uitvoeren, klikt u op **Start**.

2. Voordat u op **Enter** om te starten als u resources verwijdert, u kan even duren om te controleren of het maken van de resources in de Azure-portal. Klik op de implementatiestatus voor informatie over de implementatie.

## <a name="next-steps"></a>Volgende stappen
* Profiteren van het gebruik van een sjabloon voor het maken van een virtuele machine met behulp van de informatie in [implementeren van een virtuele Machine van Azure met C# en Resource Manager-sjabloon](csharp-template.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
* Meer informatie over het gebruik van de [Azure-bibliotheken voor .NET](https://docs.microsoft.com/dotnet/azure/?view=azure-dotnet).

