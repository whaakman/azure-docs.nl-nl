---
title: Filteren van netwerkverkeer met Azure netwerk- en beveiligingsgroepen (Preview) | Microsoft Docs
description: Informatie over het netwerk en de toepassing om beveiligingsgroepen te maken (Preview) te beperken van het type van het netwerkverkeer van en naar virtuele machines.
services: virtual-network
documentationcenter: 
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/03/2017
ms.author: jdial
ms.custom: 
ms.openlocfilehash: ac9a1a8c59a26393d32f9c543e630c302b7ced9d
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="filter-network-traffic-with-network-and-application-security-groups-preview"></a>Filteren van netwerkverkeer met netwerk- en beveiligingsgroepen (Preview)

Informatie over het maken van netwerkbeveiligingsgroepen om te filteren van netwerkverkeer naar en van groepen van virtuele machines met beveiligingsgroepen toepassing in deze zelfstudie. Zie voor meer informatie over netwerkbeveiligingsgroepen en beveiligingsgroepen van toepassing, [beveiligingsoverzicht](security-overview.md). 

De volgende secties vindt u stappen die u ondernemen kunt netwerk om beveiligingsgroepen te maken met behulp van de Azure-opdrachtregelinterface ([Azure CLI](#azure-cli)) en [Azure PowerShell](#powershell). Het resultaat is hetzelfde, ongeacht welk hulpmiddel u gebruikt het netwerk om beveiligingsgroepen te maken. Klik op de koppeling van een hulpprogramma naar die sectie van de zelfstudie. 

Dit artikel bevat stappen voor het maken van netwerk beveiligingsgroepen via het Resource Manager-implementatiemodel het implementatiemodel dat wordt u aangeraden is bij het maken van netwerk-beveiligingsgroepen. Als u wilt maken van een netwerkbeveiligingsgroep (klassiek), Zie [maken van een netwerkbeveiligingsgroep (klassiek)](virtual-networks-create-nsg-classic-ps.md). Als u niet bekend met Azure implementatiemodellen bent, Zie [begrijpen Azure-implementatiemodellen](../azure-resource-manager/resource-manager-deployment-model.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

> [!NOTE]
> Deze zelfstudie maakt gebruik van netwerk groep beveiligingsfuncties die zich momenteel in preview-versie. Functies in de preview-versie beschikt niet over de beschikbaarheid en de betrouwbaarheid als functies in het algemeen release. Als u wilt netwerkbeveiliging groepen in het algemeen alleen functies met de release te implementeren, Zie [maken van een netwerkbeveiligingsgroep](virtual-networks-create-nsg-arm-pportal.md). 

## <a name="azure-cli"></a>Azure-CLI

Azure CLI-opdrachten zijn hetzelfde, ongeacht of u de opdrachten vanaf Windows, Linux of Mac OS uitvoeren. Er zijn echter scripting verschillen tussen de houders van het besturingssysteem. De scripts in de volgende stappen uitvoeren in een Bash-shell. 

1. [Installeren en configureren van de Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json).
2. Verzeker u ervan dat u versie 2.0.18 of hoger van de Azure CLI door te voeren de `az --version` opdracht. Als u niet het geval is, installeert u de meest recente versie.
3. Aanmelden bij Azure met de `az login` opdracht.
4. Registreren voor de preview hiertoe de volgende opdrachten:
    
    ```azurecli
    az feature register --name AllowApplicationSecurityGroups --namespace Microsoft.Network
    az provider register --namespace Microsoft.Network
    ``` 

5. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```azurecli
    az feature show --name AllowApplicationSecurityGroups --namespace Microsoft.Network
    ```

    > [!WARNING]
    > Registratie kan een uur duren om te voltooien. Ga niet verder met de resterende stappen totdat *geregistreerde* wordt weergegeven voor **status** in de uitvoer geretourneerd van de vorige opdracht. Als u doorgaat voordat u bent geregistreerd, mislukt de resterende stappen.

6. Voer het volgende bash-script voor het maken van een resourcegroep:

    ```azurecli
    #!/bin/bash
    
    az group create \
      --name myResourceGroup \
      --location westcentralus
    ```

7. Drie toepassing beveiligingsgroepen maken, één voor elk servertype:

    ```azurecli
    az network asg create \
      --resource-group myResourceGroup \
      --name WebServers \
      --location westcentralus  

    az network asg create \
      --resource-group myResourceGroup \
      --name AppServers \
      --location westcentralus

    az network asg create \
      --resource-group myResourceGroup \
      --name DatabaseServers \
      --location westcentralus
    ```

8. Een netwerkbeveiligingsgroep maken:

    ```azurecli
    az network nsg create \
      --resource-group myResourceGroup \
      --name myNsg \
      --location westcentralus
    ```

9. Beveiligingsregels voor verbindingen binnen het NSG, instellen van de toepassing-beveiligingsgroepen als doel maken:
    
    ```azurecli    
    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name WebRule \
      --priority 200 \
      --access "Allow" \
      --direction "inbound" \
      --source-address-prefixes "Internet" \
      --destination-asgs "WebServers" \
      --destination-port-ranges 80 \
      --protocol "TCP"

    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name AppRule \
      --priority 300 \
      --access "Allow" \
      --direction "inbound" \
      --source-asgs "WebServers" \
      --destination-asgs "AppServers" \
      --destination-port-ranges 443 \
      --protocol "TCP"  

    az network nsg rule create \
      --resource-group myResourceGroup \
      --nsg-name myNsg \
      --name DatabaseRule \
      --priority 400 \
      --access "Allow" \
      --direction "inbound" \
      --source-asgs "AppServers" \
      --destination-asgs "DatabaseServers" \
      --destination-port-ranges 1336 \
      --protocol "TCP" 
    ``` 

10. Een virtueel netwerk maken: 
    
    ```azurecli
    az network vnet create \
      --name myVnet \
      --resource-group myResourceGroup \
      --subnet-name mySubnet \
      --address-prefix 10.0.0.0/16 \
      --location westcentralus
    ```

11. De netwerkbeveiligingsgroep voor het subnet in het virtuele netwerk koppelen:

    ```azurecli
    az network vnet subnet update \
      --name mySubnet \
      --resource-group myResourceGroup \
      --vnet-name myVnet \
      --network-security-group myNsg
    ```
    
12. Maak drie netwerkinterfaces, één voor elk servertype: 

    ```azurecli
    az network nic create \
      --resource-group myResourceGroup \
      --name myNic1 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --location westcentralus \
      --application-security-groups "WebServers" "AppServers"

    az network nic create \
      --resource-group myResourceGroup \
      --name myNic2 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --location westcentralus \
      --application-security-groups "AppServers"

    az network nic create \
      --resource-group myResourceGroup \
      --name myNic3 \
      --vnet-name myVnet \
      --subnet mySubnet \
      --location westcentralus \
      --application-security-groups "DatabaseServers"
    ```

    Alleen de bijbehorende beveiligingsregel die u hebt gemaakt in stap 9 wordt toegepast op de netwerkinterface, op basis van de beveiligingsgroep van de toepassing die de netwerkinterface lid van is. Bijvoorbeeld alleen de *AppRule* regel is van kracht voor *myNic2*, omdat de netwerkinterface lid van is de *AppServers* beveiligingsgroep van de toepassing en de regel Hiermee geeft u de *AppServers* toepassing beveiligingsgroep als doel. De *WebRule* en *DatabaseRule* regels worden niet toegepast op *myNic2*, omdat de netwerkinterface geen lid van is de *endwebservers servers*en *DatabaseServers* toepassing beveiligingsgroepen. Zowel de *WebRule* en *AppRule* regels zijn geschikt voor *myNic1* echter omdat de *myNic1* netwerkinterface is lid van zowel de *endwebservers servers* en *AppServers* toepassing beveiligingsgroepen en de regels geeft de *endwebservers servers* en *AppServers* toepassing beveiligingsgroepen als hun doelen. 

13. Een virtuele machine maken voor elk servertype, de bijbehorende netwerkinterface koppelen aan elke virtuele machine. In dit voorbeeld maakt u virtuele machines van Windows, maar u kunt wijzigen *win2016datacenter* naar *UbuntuLTS* Linux virtuele machines in plaats daarvan maken.

    ```azurecli
    # Update for your admin password
    AdminPassword=ChangeYourAdminPassword1

    az vm create \
      --resource-group myResourceGroup \
      --name myWebVm \
      --location westcentralus \
      --nics myNic1 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword \
      --no-wait

    az vm create \
      --resource-group myResourceGroup \
      --name myAppVm \
      --location westcentralus \
      --nics myNic2 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword \
      --no-wait

    az vm create \
      --resource-group myResourceGroup \
      --name myDatabaseVm \
      --location westcentralus \
      --nics myNic3 \
      --image win2016datacenter \
      --admin-username azureuser \
      --admin-password $AdminPassword    
    ```

14. **Optionele**: verwijderen van de resources die u in deze zelfstudie maakt via de stappen in [resources verwijderen](#delete-cli).

## <a name="powershell"></a>PowerShell

1. Installeer en configureer [PowerShell](/powershell/azure/install-azurerm-ps).
2. Zorg ervoor dat u versie 4.4.0 of hoger van de module AzureRm geïnstalleerd. U kunt de momenteel geïnstalleerde versie controleren door te voeren de `Get-Module -ListAvailable AzureRM` opdracht. Als u wilt installeren of upgraden, installeert u de nieuwste versie van de module AzureRM op basis van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureRM).
3. In een PowerShell-sessie, moet u zich aanmelden bij Azure met uw [Azure-account](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#account) met behulp van de `login-azurermaccount` opdracht.
4. Registreren voor de preview hiertoe de volgende opdrachten:
    
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowApplicationSecurityGroups -ProviderNamespace Microsoft.Network
    Register-AzureRmResourceProvider -ProviderNamespace Microsoft.Network
    ``` 

5. Bevestig dat u voor de preview zijn geregistreerd met de volgende opdracht:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowApplicationSecurityGroups -ProviderNamespace Microsoft.Network
    ```

    > [!WARNING]
    > Registratie kan een uur duren om te voltooien. Ga niet verder met de resterende stappen totdat *geregistreerde* wordt weergegeven voor **RegistrationState** in de uitvoer geretourneerd van de vorige opdracht. Als u doorgaat voordat u bent geregistreerd, mislukt de resterende stappen.
        
6. Een resourcegroep maken:

    ```powershell
    New-AzureRmResourceGroup `
      -Name myResourceGroup `
      -Location westcentralus
    ```

7. Drie toepassing beveiligingsgroepen maken, één voor elk servertype:

    ```powershell
    $webAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name WebServers `
      -Location westcentralus
  
    $appAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name AppServers `
      -Location westcentralus

    $databaseAsg = New-AzureRmApplicationSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Name DatabaseServers `
      -Location westcentralus
    ```

8. Beveiligingsregels voor elk servertype maken.
    
    ```powershell
    $webRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "WebRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 200 `
      -SourceAddressPrefix Internet `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $webAsg.id `
      -DestinationPortRange 80
    
    $appRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "AppRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 300 `
      -SourceApplicationSecurityGroupId $webAsg.id `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $appAsg.id `
      -DestinationPortRange 443
      
    $databaseRule = New-AzureRmNetworkSecurityRuleConfig `
      -Name "DatabaseRule" `
      -Access Allow `
      -Protocol Tcp `
      -Direction Inbound `
      -Priority 400 `
      -SourceApplicationSecurityGroupId $appAsg.id `
      -SourcePortRange * `
      -DestinationApplicationSecurityGroupId $databaseAsg.id `
      -DestinationPortRange 1336
    ``` 

9. Een netwerkbeveiligingsgroep maken:

    ```powershell
    $nsg = New-AzureRmNetworkSecurityGroup `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Name myNsg `
      -SecurityRules $WebRule,$AppRule,$DatabaseRule
    ```

10. De configuratie van een subnet maken en koppelen van de netwerkbeveiligingsgroep aan:
    
    ```powershell
    $subnet = New-AzureRmVirtualNetworkSubnetConfig `
      -AddressPrefix 10.0.0.0/24 `
      -Name mySubnet `
      -NetworkSecurityGroup $nsg
    ```

11. Een virtueel netwerk maken: 
    
    ```powershell
    $vNet = New-AzureRmVirtualNetwork `
      -Name myVnet `
      -AddressPrefix '10.0.0.0/16' `
      -Subnet $subnet `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus
    ```

12. Maak drie netwerkinterfaces, één voor elk servertype. 

    ```powershell
    $nic1 = New-AzureRmNetworkInterface `
      -Name myNic1 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -ApplicationSecurityGroup $webAsg,$appAsg

    $nic2 = New-AzureRmNetworkInterface `
      -Name myNic2 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -ApplicationSecurityGroup $appAsg

    $nic3 = New-AzureRmNetworkInterface `
      -Name myNic3 `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -Subnet $vNet.Subnets[0] `
      -ApplicationSecurityGroup $databaseAsg
    ```

    Alleen de bijbehorende beveiligingsregel die u hebt gemaakt in stap 8 wordt toegepast op de netwerkinterface, op basis van de beveiligingsgroep van de toepassing die de netwerkinterface lid van is. Bijvoorbeeld alleen de *AppRule* regel is van kracht voor *myNic2*, omdat de netwerkinterface lid van is de *AppServers* beveiligingsgroep van de toepassing en de regel Hiermee geeft u de *AppServers* toepassing beveiligingsgroep als doel. De *WebRule* en *DatabaseRule* regels worden niet toegepast op *myNic2*, omdat de netwerkinterface geen lid van is de *endwebservers servers*en *DatabaseServers* toepassing beveiligingsgroepen. Zowel de *WebRule* en *AppRule* regels zijn geschikt voor *myNic1* echter omdat de *myNic1* netwerkinterface is lid van zowel de *endwebservers servers* en *AppServers* toepassing beveiligingsgroepen en de regels geeft de *endwebservers servers* en *AppServers* toepassing beveiligingsgroepen als hun doelen. 

13. Een virtuele machine maken voor elk servertype, de bijbehorende netwerkinterface koppelen aan elke virtuele machine. In dit voorbeeld maakt u virtuele machines van Windows, maar voordat het script wordt uitgevoerd, kunt u *-Windows* naar *- Linux*, *legitieme Microsoft Windows Server* naar *Canonieke*, *Windows Server* naar *UbuntuServer* en *2016 Datacenter* naar *14.04.2-LTS*Linux virtuele machines in plaats daarvan maken.

    ```powershell
    # Create user object
    $cred = Get-Credential -Message "Enter a username and password for the virtual machine."
   
    # Create the web server virtual machine configuration and virtual machine.
    $webVmConfig = New-AzureRmVMConfig `
      -VMName myWebVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName myWebVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic1.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $webVmConfig

    # Create the app server virtual machine configuration and virtual machine.
    $appVmConfig = New-AzureRmVMConfig `
      -VMName myAppVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName myAppVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic2.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $appVmConfig

    # Create the database server virtual machine configuration and virtual machine.
    $databaseVmConfig = New-AzureRmVMConfig `
      -VMName myDatabaseVm `
      -VMSize Standard_DS1_V2 | `
    Set-AzureRmVMOperatingSystem -Windows `
      -ComputerName mydatabaseVm `
      -Credential $cred | `
    Set-AzureRmVMSourceImage `
      -PublisherName MicrosoftWindowsServer `
      -Offer WindowsServer `
      -Skus 2016-Datacenter `
      -Version latest | `
    Add-AzureRmVMNetworkInterface `
      -Id $nic3.Id
    New-AzureRmVM `
      -ResourceGroupName myResourceGroup `
      -Location westcentralus `
      -VM $databaseVmConfig
    ```

14. **Optionele**: verwijderen van de resources die u in deze zelfstudie maakt via de stappen in [resources verwijderen](#delete-cli).

## <a name="remove-a-nic-from-an-asg"></a>Een NIC verwijderen uit een ASG
Wanneer u een netwerkinterface uit de beveiligingsgroep van een toepassing verwijdert, worden geen van de regels die specificeren van de groep van toepassingen beveiliging toegepast op de netwerkinterface die u verwijdert.

### <a name="azure-cli"></a>Azure-CLI

Verwijderen van *myNic3* uit alle beveiligingsgroepen van toepassing, voer de volgende opdracht:

```azurecli
az network nic update \
  --name myNic3 \
  --resource-group myResourceGroup \
  --remove ipConfigurations[0].applicationSecurityGroups
```

### <a name="powershell"></a>PowerShell

Verwijderen van *myNic3* uit alle beveiligingsgroepen van toepassing, voer de volgende opdrachten:

```powershell
$nic=Get-AzureRmNetworkInterface `
  -Name myNic3 `
  -ResourceGroupName myResourceGroup

$nic.IpConfigurations[0].ApplicationSecurityGroups = $null
$nic | Set-AzureRmNetworkInterface 
```

## <a name="delete"></a>Resources verwijderen

Als u deze zelfstudie hebt voltooid, is het raadzaam de resources verwijderen die u hebt gemaakt, zodat u geen gebruik kosten. Verwijderen van een resourcegroep, verwijdert tevens alle bronnen die zich in de resourcegroep.

### <a name="delete-portal"></a>Azure-portal

1. Voer in het zoekvak portal **myResourceGroup**. Klik in de zoekresultaten op **myResourceGroup**.
2. Op de **myResourceGroup** blade, klikt u op de **verwijderen** pictogram.
3. Het verwijderen te bevestigen, in de **TYPE de naam van een RESOURCEGROEP** Voer **myResourceGroup**, en klik vervolgens op **verwijderen**.

### <a name="delete-cli"></a>Azure CLI

Voer de volgende opdracht in een sessie CLI:

```azurecli
az group delete --name myResourceGroup --yes
```

### <a name="delete-powershell"></a>PowerShell

Voer de volgende opdracht in een PowerShell-sessie:

```powershell
Remove-AzureRmResourceGroup -Name myResourceGroup -Force
```

