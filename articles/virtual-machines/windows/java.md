---
title: Maken en beheren van een virtuele Machine van Azure met behulp van Java | Microsoft Docs
description: Java- en Azure Resource Manager gebruiken voor het implementeren van een virtuele machine en de ondersteunende bronnen.
services: virtual-machines-windows
documentationcenter: 
author: davidmu1
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 07/17/2017
ms.author: davidmu
ms.openlocfilehash: b970b7a5e1db6d8580ede9699d4776a79e03c98f
ms.sourcegitcommit: b07d06ea51a20e32fdc61980667e801cb5db7333
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2017
---
# <a name="create-and-manage-windows-vms-in-azure-using-java"></a>Maken en beheren van Windows-machines in Azure met behulp van Java

Een [Azure virtuele Machine](overview.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json) (VM) moet meerdere ondersteunende Azure-resources. In dit artikel bevat informatie over het maken, beheren en het verwijderen van de VM-netwerkbronnen met behulp van Java. Procedures voor:

> [!div class="checklist"]
> * Een Maven-project maken
> * Afhankelijkheden toevoegen
> * Referenties maken
> * Resources maken
> * Beheertaken uitvoeren
> * Resources verwijderen
> * De toepassing uitvoeren

Het duurt ongeveer 20 minuten aan deze stappen uit te voeren.

## <a name="create-a-maven-project"></a>Een Maven-project maken

1. Als u dit nog niet hebt gedaan, installeert u [Java](http://www.oracle.com/technetwork/java/javase/downloads/index.html).
2. Installeer [Maven](http://maven.apache.org/download.cgi).
3. Maak een nieuwe map en het project:
    
    ```
    mkdir java-azure-test
    cd java-azure-test
    
    mvn archetype:generate -DgroupId=com.fabrikam -DartifactId=testAzureApp -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

## <a name="add-dependencies"></a>Afhankelijkheden toevoegen

1. Onder de `testAzureApp` map, open de `pom.xml` bestands- en toevoegen van de buildconfiguratie van &lt;project&gt; het bouwen van uw toepassing inschakelen:

    ```xml
    <build>
      <plugins>
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>exec-maven-plugin</artifactId>
            <configuration>
                <mainClass>com.fabrikam.testAzureApp.App</mainClass>
            </configuration>
        </plugin>
      </plugins>
    </build>
    ```

2. Voeg de afhankelijkheden die nodig zijn voor toegang tot de Azure SDK voor Java.

    ```xml
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-compute</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-resources</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.microsoft.azure</groupId>
      <artifactId>azure-mgmt-network</artifactId>
      <version>1.1.0</version>
    </dependency>
    <dependency>
      <groupId>com.squareup.okio</groupId>
      <artifactId>okio</artifactId>
      <version>1.13.0</version>
    </dependency>
    <dependency> 
      <groupId>com.nimbusds</groupId>
      <artifactId>nimbus-jose-jwt</artifactId>
      <version>3.6</version>
    </dependency>
    <dependency>
      <groupId>net.minidev</groupId>
      <artifactId>json-smart</artifactId>
      <version>1.0.6.3</version>
    </dependency>
    <dependency>
      <groupId>javax.mail</groupId>
      <artifactId>mail</artifactId>
      <version>1.4.5</version>
    </dependency>
    ```

3. Sla het bestand op.

## <a name="create-credentials"></a>Referenties maken

Voordat u deze stap, zorg ervoor dat u toegang tot hebben een [Active Directory-service-principal](../../azure-resource-manager/resource-group-create-service-principal-portal.md). U moet ook de toepassings-ID, de verificatiesleutel en de tenant-ID die u nodig hebt in een later stadium vastleggen.

### <a name="create-the-authorization-file"></a>Maken van het bestand met autorisatieregels

1. Maak een bestand met de naam `azureauth.properties` en voeg deze eigenschappen toe:

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

    Vervang  **&lt;abonnement-id&gt;**  met uw abonnements-id  **&lt;toepassing-id&gt;**  met de toepassings-id van het Active Directory  **&lt;verificatiesleutel&gt;**  voor de Toepassingssleutel, en  **&lt;tenant-id&gt;**  met tenant-id.

2. Sla het bestand op.
3. Omgevingsvariabele AZURE_AUTH_LOCATION in uw shell met het volledige pad naar het verificatiebestand instellen.

### <a name="create-the-management-client"></a>De management-client maken

1. Open de `App.java` bestand onder `src\main\java\com\fabrikam` en zorg ervoor dat deze instructie package boven:

    ```java
    package com.fabrikam.testAzureApp;
    ```

2. Voeg deze onder de instructie package importinstructies:
   
    ```java
    import com.microsoft.azure.management.Azure;
    import com.microsoft.azure.management.compute.AvailabilitySet;
    import com.microsoft.azure.management.compute.AvailabilitySetSkuTypes;
    import com.microsoft.azure.management.compute.CachingTypes;
    import com.microsoft.azure.management.compute.InstanceViewStatus;
    import com.microsoft.azure.management.compute.DiskInstanceView;
    import com.microsoft.azure.management.compute.VirtualMachine;
    import com.microsoft.azure.management.compute.VirtualMachineSizeTypes;
    import com.microsoft.azure.management.network.PublicIPAddress;
    import com.microsoft.azure.management.network.Network;
    import com.microsoft.azure.management.network.NetworkInterface;
    import com.microsoft.azure.management.resources.ResourceGroup;
    import com.microsoft.azure.management.resources.fluentcore.arm.Region;
    import com.microsoft.azure.management.resources.fluentcore.model.Creatable;
    import com.microsoft.rest.LogLevel;
    import java.io.File;
    import java.util.Scanner;
    ```

2. Voor het maken van de Active Directory-referenties die u wilt aanvragen, moet u deze code toevoegen aan de belangrijkste methode van de klasse App:
   
    ```java
    try {    
        final File credFile = new File(System.getenv("AZURE_AUTH_LOCATION"));
        Azure azure = Azure.configure()
            .withLogLevel(LogLevel.BASIC)
            .authenticate(credFile)
            .withDefaultSubscription();
    } catch (Exception e) {
        System.out.println(e.getMessage());
        e.printStackTrace();
    }

    ```

## <a name="create-resources"></a>Resources maken

### <a name="create-the-resource-group"></a>De resourcegroep maken

Alle resources moeten zijn opgenomen in een [resourcegroep](../../azure-resource-manager/resource-group-overview.md).

Geef waarden op voor de toepassing en de resourcegroep maken, voeg deze code aan de try-blok in de belangrijkste methode:

```java
System.out.println("Creating resource group...");
ResourceGroup resourceGroup = azure.resourceGroups()
    .define("myResourceGroup")
    .withRegion(Region.US_EAST)
    .create();
```

### <a name="create-the-availability-set"></a>De beschikbaarheidsset maken

[Beschikbaarheidssets](tutorial-availability-sets.md) het eenvoudiger voor u de virtuele machines die worden gebruikt door uw toepassing te onderhouden.

Voeg deze code naar het try-blok in de belangrijkste methode voor het maken van de beschikbaarheidsset:

```java
System.out.println("Creating availability set...");
AvailabilitySet availabilitySet = azure.availabilitySets()
    .define("myAvailabilitySet")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withSku(AvailabilitySetSkuTypes.MANAGED)
    .create();
```
### <a name="create-the-public-ip-address"></a>Het openbare IP-adres maken

Een [openbaar IP-adres](../../virtual-network/virtual-network-ip-addresses-overview-arm.md) nodig is om te communiceren met de virtuele machine.

Voeg deze code naar het try-blok in de belangrijkste methode voor het maken van het openbare IP-adres voor de virtuele machine:

```java
System.out.println("Creating public IP address...");
PublicIPAddress publicIPAddress = azure.publicIPAddresses()
    .define("myPublicIP")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withDynamicIP()
    .create();
```

### <a name="create-the-virtual-network"></a>Het virtuele netwerk maken

Een virtuele machine moet worden in een subnet van een [virtueel netwerk](../../virtual-network/virtual-networks-overview.md).

Voor het maken van een subnet en een virtueel netwerk, moet u deze code toevoegen aan de try-blok in de belangrijkste methode:

```java
System.out.println("Creating virtual network...");
Network network = azure.networks()
    .define("myVN")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withAddressSpace("10.0.0.0/16")
    .withSubnet("mySubnet","10.0.0.0/24")
    .create();
```

### <a name="create-the-network-interface"></a>De netwerkinterface maken

Een virtuele machine moet een netwerkinterface om te communiceren met het virtuele netwerk.

Voeg deze code naar het try-blok in de belangrijkste methode voor het maken van een netwerkinterface:

```java
System.out.println("Creating network interface...");
NetworkInterface networkInterface = azure.networkInterfaces()
    .define("myNIC")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetwork(network)
    .withSubnet("mySubnet")
    .withPrimaryPrivateIPAddressDynamic()
    .withExistingPrimaryPublicIPAddress(publicIPAddress)
    .create();
```

### <a name="create-the-virtual-machine"></a>De virtuele machine maken

Nu u de ondersteunende resources hebt gemaakt, kunt u een virtuele machine maken.

Voeg deze code naar het try-blok in de belangrijkste methode voor het maken van de virtuele machine:

```java
System.out.println("Creating virtual machine...");
VirtualMachine virtualMachine = azure.virtualMachines()
    .define("myVM")
    .withRegion(Region.US_EAST)
    .withExistingResourceGroup("myResourceGroup")
    .withExistingPrimaryNetworkInterface(networkInterface)
    .withLatestWindowsImage("MicrosoftWindowsServer", "WindowsServer", "2012-R2-Datacenter")
    .withAdminUsername("azureuser")
    .withAdminPassword("Azure12345678")
    .withComputerName("myVM")
    .withExistingAvailabilitySet(availabilitySet)
    .withSize("Standard_DS1")
    .create();
Scanner input = new Scanner(System.in);
System.out.println("Press enter to get information about the VM...");
input.nextLine();
```

> [!NOTE]
> Deze zelfstudie maakt een virtuele machine met een versie van het besturingssysteem Windows Server. Zie voor meer informatie over het selecteren van de andere afbeeldingen, [navigeren door en selecteren van installatiekopieën van virtuele machine van Azure met Windows PowerShell en Azure CLI](../linux/cli-ps-findimage.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
>

Als u gebruiken van een bestaande schijf in plaats van een marketplace-installatiekopie wilt, moet u deze code gebruiken: 

```java
ManagedDisk managedDisk = azure.disks.define("myosdisk") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withWindowsFromVhd("https://mystorage.blob.core.windows.net/vhds/myosdisk.vhd") 
    .withSizeInGB(128) 
    .withSku(DiskSkuTypes.PremiumLRS) 
    .create(); 

azure.virtualMachines.define("myVM") 
    .withRegion(Region.US_EAST) 
    .withExistingResourceGroup("myResourceGroup") 
    .withExistingPrimaryNetworkInterface(networkInterface) 
    .withSpecializedOSDisk(managedDisk, OperatingSystemTypes.Windows) 
    .withExistingAvailabilitySet(availabilitySet) 
    .withSize(VirtualMachineSizeTypes.StandardDS1) 
    .create(); 
``` 

## <a name="perform-management-tasks"></a>Beheertaken uitvoeren

Tijdens de levenscyclus van een virtuele machine, kan u wilt uitvoeren van beheertaken, zoals starten, stoppen of een virtuele machine wordt verwijderd. Bovendien wilt u maken code om herhaalde of complexe taken te automatiseren.

Wanneer u ondernemen met de virtuele machine moet, moet u een exemplaar van deze. Deze code toevoegen aan de try-blok van de belangrijkste methode:

```java
VirtualMachine vm = azure.virtualMachines().getByResourceGroup("myResourceGroup", "myVM");
```

### <a name="get-information-about-the-vm"></a>Informatie ophalen over de virtuele machine

Voor informatie over de virtuele machine, moet u deze code toevoegen aan de try-blok in de belangrijkste methode:

```java
System.out.println("hardwareProfile");
System.out.println("    vmSize: " + vm.size());
System.out.println("storageProfile");
System.out.println("  imageReference");
System.out.println("    publisher: " + vm.storageProfile().imageReference().publisher());
System.out.println("    offer: " + vm.storageProfile().imageReference().offer());
System.out.println("    sku: " + vm.storageProfile().imageReference().sku());
System.out.println("    version: " + vm.storageProfile().imageReference().version());
System.out.println("  osDisk");
System.out.println("    osType: " + vm.storageProfile().osDisk().osType());
System.out.println("    name: " + vm.storageProfile().osDisk().name());
System.out.println("    createOption: " + vm.storageProfile().osDisk().createOption());
System.out.println("    caching: " + vm.storageProfile().osDisk().caching());
System.out.println("osProfile");
System.out.println("    computerName: " + vm.osProfile().computerName());
System.out.println("    adminUserName: " + vm.osProfile().adminUsername());
System.out.println("    provisionVMAgent: " + vm.osProfile().windowsConfiguration().provisionVMAgent());
System.out.println("    enableAutomaticUpdates: " + vm.osProfile().windowsConfiguration().enableAutomaticUpdates());
System.out.println("networkProfile");
System.out.println("    networkInterface: " + vm.primaryNetworkInterfaceId());
System.out.println("vmAgent");
System.out.println("  vmAgentVersion: " + vm.instanceView().vmAgent().vmAgentVersion());
System.out.println("    statuses");
for(InstanceViewStatus status : vm.instanceView().vmAgent().statuses()) {
    System.out.println("    code: " + status.code());
    System.out.println("    displayStatus: " + status.displayStatus());
    System.out.println("    message: " + status.message());
    System.out.println("    time: " + status.time());
}
System.out.println("disks");
for(DiskInstanceView disk : vm.instanceView().disks()) {
    System.out.println("  name: " + disk.name());
    System.out.println("  statuses");
    for(InstanceViewStatus status : disk.statuses()) {
        System.out.println("    code: " + status.code());
        System.out.println("    displayStatus: " + status.displayStatus());
        System.out.println("    time: " + status.time());
    }
}
System.out.println("VM general status");
System.out.println("  provisioningStatus: " + vm.provisioningState());
System.out.println("  id: " + vm.id());
System.out.println("  name: " + vm.name());
System.out.println("  type: " + vm.type());
System.out.println("VM instance status");
for(InstanceViewStatus status : vm.instanceView().statuses()) {
    System.out.println("  code: " + status.code());
    System.out.println("  displayStatus: " + status.displayStatus());
}
System.out.println("Press enter to continue...");
input.nextLine();   
```

### <a name="stop-the-vm"></a>De virtuele machine stoppen

U kunt een virtuele machine stoppen en alle bijbehorende instellingen behouden, maar blijven in rekening gebracht voor of u kunt een virtuele machine stoppen en deze ongedaan gemaakt. Wanneer een virtuele machine in de toewijzing is opgeheven, zijn alle resources die zijn gekoppeld ook toewijzing ongedaan is gemaakt en facturering eindigt voor.

Om te stoppen van de virtuele machine zonder deze toewijzing, moet u deze code toevoegen aan de try-blok in de belangrijkste methode:

```java
System.out.println("Stopping vm...");
vm.powerOff();
System.out.println("Press enter to continue...");
input.nextLine();
```

Als u wilt dat de virtuele machine ongedaan, wijzigt u de aanroep uitgeschakeld in deze code:

```java
vm.deallocate();
```

### <a name="start-the-vm"></a>Start de virtuele machine

Voeg deze code naar het try-blok in de belangrijkste methode voor het starten van de virtuele machine:

```java
System.out.println("Starting vm...");
vm.start();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="resize-the-vm"></a>De virtuele machine vergroten of verkleinen

Veel aspecten van de implementatie moeten worden beschouwd bij het kiezen van een grootte voor de virtuele machine. Zie voor meer informatie [VM-grootten](sizes.md).  

Als de grootte van de virtuele machine wijzigen, moet u deze code toevoegen aan de try-blok in de belangrijkste methode:

```java
System.out.println("Resizing vm...");
vm.update()
    .withSize(VirtualMachineSizeTypes.STANDARD_DS2)
    .apply();
System.out.println("Press enter to continue...");
input.nextLine();
```

### <a name="add-a-data-disk-to-the-vm"></a>Een gegevensschijf toevoegen aan de virtuele machine

Als u wilt een gegevensschijf toevoegen aan de virtuele machine die is 2 GB groot en heeft een LUN van 0 en een cache in type ReadWrite, voeg deze code aan de try-blok in de belangrijkste methode:

```java
System.out.println("Adding data disk...");
vm.update()
    .withNewDataDisk(2, 0, CachingTypes.READ_WRITE)
    .apply();
System.out.println("Press enter to delete resources...");
input.nextLine();
```

## <a name="delete-resources"></a>Resources verwijderen

Omdat u in rekening voor resources die worden gebruikt in Azure gebracht, maar het is altijd raadzaam om resources verwijderen die niet langer nodig zijn. Als u wilt verwijderen van de virtuele machines en de ondersteunende resources, is hoeft u de resourcegroep verwijderen.

1. Voeg deze code naar het try-blok in de belangrijkste methode voor het verwijderen van de resourcegroep:
   
```java
System.out.println("Deleting resources...");
azure.resourceGroups().deleteByName("myResourceGroup");
```

2. Sla het bestand App.java.

## <a name="run-the-application"></a>De toepassing uitvoeren

Het moet over vijf minuten duren voordat deze consoletoepassing volledig uitvoeren vanaf het begin te voltooien.

1. Gebruik deze Maven-opdracht voor het uitvoeren van de toepassing:

    ```
    mvn compile exec:java
    ```

2. Voordat u op **Enter** om te starten als u resources verwijdert, u kan even duren om te controleren of het maken van de resources in de Azure-portal. Klik op de implementatiestatus voor informatie over de implementatie.


## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het gebruik van de [Azure-beheerbibliotheken voor Java](https://docs.microsoft.com/java/azure/java-sdk-azure-overview).

