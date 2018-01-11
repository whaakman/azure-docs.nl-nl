---
title: Een SQL Server-VM voor Windows maken met Azure PowerShell | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een virtuele SQL Server 2017-machine voor Windows kunt maken met Azure PowerShell.
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-resource-manager
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: infrastructure-services
ms.date: 12/12/2017
ms.author: jroth
ms.openlocfilehash: 9af08fd46314ff102eff95e0832f7ce96bc161d6
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="create-a-sql-server-windows-virtual-machine-with-azure-powershell"></a>Een virtuele SQL Server-machine voor Windows maken met Azure PowerShell

In deze snelstartgids gaat u een virtuele SQL Server-machine maken met Azure PowerShell.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

Voor deze snelstartgids is moduleversie 3.6 of later van Azure PowerShell vereist. Voer `Get-Module -ListAvailable AzureRM` uit om de versie te bekijken. Als u PowerShell wilt installeren of upgraden, raadpleegt u [De Azure PowerShell-module installeren](/powershell/azure/install-azurerm-ps).

## <a name="configure-powershell"></a>PowerShell configureren

1. Open PowerShell en zorg dat u toegang hebt tot uw Azure-account door de opdracht **Add-AzureRmAccount** uit te voeren.

   ```PowerShell
   Add-AzureRmAccount
   ```

1. Als het goed is, ziet u nu een aanmeldingsscherm waar u uw referenties kunt invoeren. Gebruik hetzelfde e-mailadres en wachtwoord waarmee u zich aanmeldt bij Azure Portal.

## <a name="create-a-resource-group"></a>Een resourcegroep maken

1. Definieer een variabele met een unieke naam voor de resourcegroep. In de andere opdrachten wordt deze naam gebruikt voor alle overige resourcenamen om de rest van de snelstartgids te vereenvoudigen.

   ```PowerShell
   $ResourceGroupName = "sqlvm1"
   ```

1. Definieer de locatie van een Azure-doelregio voor alle VM-resources.

   ```PowerShell
   $Location = "East US"
   ```

1. Maak de resourcegroep.

   ```PowerShell
   New-AzureRmResourceGroup -Name $ResourceGroupName -Location $Location
   ```

## <a name="configure-network-settings"></a>Netwerkinstellingen configureren

1. Maak een virtueel netwerk, subnet en een openbaar IP-adres. Deze resources worden gebruikt voor netwerkconnectiviteit met de virtuele machine en om verbinding met internet te maken.

   ``` PowerShell
   $SubnetName = $ResourceGroupName + "subnet"
   $VnetName = $ResourceGroupName + "vnet"
   $PipName = $ResourceGroupName + $(Get-Random)

   # Create a subnet configuration
   $SubnetConfig = New-AzureRmVirtualNetworkSubnetConfig -Name $SubnetName -AddressPrefix 192.168.1.0/24

   # Create a virtual network
   $Vnet = New-AzureRmVirtualNetwork -ResourceGroupName $ResourceGroupName -Location $Location `
      -Name VnetName -AddressPrefix 192.168.0.0/16 -Subnet $SubnetConfig

   # Create a public IP address and specify a DNS name
   $Pip = New-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName -Location $Location `
      -AllocationMethod Static -IdleTimeoutInMinutes 4 -Name $PipName
   ```

1. Maak een netwerkbeveiligingsgroep. Configureer regels om Extern bureaublad- (RDP) en SQL Server-verbindingen toe te staan.

   ```PowerShell
   # Rule to allow remote desktop (RDP)
   $NsgRuleRDP = New-AzureRmNetworkSecurityRuleConfig -Name "RDPRule" -Protocol Tcp `
      -Direction Inbound -Priority 1000 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 3389 -Access Allow

   #Rule to allow SQL Server connections on port 1433
   $NsgRuleSQL = New-AzureRmNetworkSecurityRuleConfig -Name "MSSQLRule"  -Protocol Tcp `
      -Direction Inbound -Priority 1001 -SourceAddressPrefix * -SourcePortRange * `
      -DestinationAddressPrefix * -DestinationPortRange 1433 -Access Allow

   # Create the network security group
   $NsgName = $ResourceGroupName + "nsg"
   $Nsg = New-AzureRmNetworkSecurityGroup -ResourceGroupName $ResourceGroupName `
      -Location $Location -Name $NsgName `
      -SecurityRules $NsgRuleRDP,$NsgRuleSQL
   ```

1. Maak de netwerkinterface.

   ```PowerShell
   $InterfaceName = $ResourceGroupName + "int"
   $Interface = New-AzureRmNetworkInterface -Name $InterfaceName `
      -ResourceGroupName $ResourceGroupName -Location $Location `
      -SubnetId $VNet.Subnets[0].Id -PublicIpAddressId $Pip.Id `
      -NetworkSecurityGroupId $Nsg.Id
   ```

## <a name="create-the-sql-vm"></a>De SQL-VM maken

1. Definieer uw referenties om u aan te melden bij de VM. De gebruikersnaam is: azureadmin. Wijzig het wachtwoord voordat u de opdracht uitvoert.

   ``` PowerShell
   # Define a credential object
   $SecurePassword = ConvertTo-SecureString 'Change.This!000' `
      -AsPlainText -Force
   $Cred = New-Object System.Management.Automation.PSCredential ("azureadmin", $securePassword)
   ```

1. Maak een configuratieobject voor de virtuele machine en maak vervolgens de VM. Met de volgende opdracht wordt een SQL Server 2017 Developer Edition-VM gemaakt in Windows Server 2016.

   ```PowerShell
   # Create a virtual machine configuration
   $VMName = $ResourceGroupName + "VM"
   $VMConfig = New-AzureRmVMConfig -VMName $VMName -VMSize Standard_DS13 | `
      Set-AzureRmVMOperatingSystem -Windows -ComputerName $VMName -Credential $Cred -ProvisionVMAgent -EnableAutoUpdate | `
      Set-AzureRmVMSourceImage -PublisherName "MicrosoftSQLServer" -Offer "SQL2017-WS2016" -Skus "SQLDEV" -Version "latest" | `
      Add-AzureRmVMNetworkInterface -Id $Interface.Id
   
   # Create the VM
   New-AzureRmVM -ResourceGroupName $ResourceGroupName -Location $Location -VM $VMConfig
   ```

   > [!TIP]
   > Het duurt enkele minuten om de VM te maken.

## <a name="install-the-sql-iaas-agent"></a>SQL IaaS-agent installeren

Voor integratie met de portal en voor de functies van de SQL-VM, moet u de [extensie voor de SQL Server IaaS-agent](virtual-machines-windows-sql-server-agent-extension.md) installeren. Als u de agent wilt installeren op de nieuwe VM, voert u de volgende opdracht uit nadat deze is gemaakt.

   ```PowerShell
   Set-AzureRmVMSqlServerExtension -ResourceGroupName $ResourceGroupName -VMName $VMName -name "SQLIaasExtension" -version "1.2" -Location $Location
   ```

## <a name="remote-desktop-into-the-vm"></a>Extern bureaublad op de VM

1. Met de volgende opdracht wordt het openbare IP-adres opgehaald voor de nieuwe VM.

   ```PowerShell
   Get-AzureRmPublicIpAddress -ResourceGroupName $ResourceGroupName | Select IpAddress
   ```

1. Geef vervolgens het geretourneerde IP-adres als opdrachtregelparameter door aan **mstsc** om een Extern bureaublad-sessie te starten op de nieuwe VM.

   ```
   mstsc /v:<publicIpAddress>
   ```

1. Wanneer u om referenties wordt gevraagd, voert u de referenties in voor een ander account. Voer de gebruikersnaam voorafgegaan door een backslash, (bijvoorbeeld `\azureadmin`), en het wachtwoord in dat u eerder in deze snelstartgids hebt ingesteld.

## <a name="connect-to-sql-server"></a>Verbinding maken met SQL Server

1. Nadat u bent aangemeld bij de Extern bureaublad-sessie, start u **SQL Server Management Studio 2017** vanuit het startmenu.

1. Laat de standaardwaarden in het dialoogvenster **Verbinding maken met server** staan. De servernaam is de naam van de VM. Verificatie is ingesteld op **Windows-verificatie**. Klik op **Verbinden**.

U hebt nu lokaal verbinding met SQL Server. Als u extern verbinding wilt maken, moet u [connectiviteit handmatig of vanuit de portal configureren](virtual-machines-windows-sql-connect.md).

## <a name="clean-up-resources"></a>Resources opschonen

Als het niet nodig is dat de VM continu wordt uitgevoerd, kunt u onnodige kosten voorkomen door de virtuele machine te stoppen wanneer deze niet in gebruik is. Met de volgende opdracht wordt de VM gestopt, maar blijft deze beschikbaar voor toekomstig gebruik.

```PowerShell
Stop-AzureRmVM -Name $VMName -ResourceGroupName $ResourceGroupName
```

U kunt ook alle resources die aan de virtuele machine zijn gekoppeld, definitief verwijderen met de opdracht **Remove-AzureRmResourceGroup**. Wees voorzichtig met het gebruik van deze opdracht, want hiermee verwijdert u ook de virtuele machine zelf definitief.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een virtuele SQL Server 2017-machine gemaakt met behulp van Azure PowerShell. Raadpleeg het volgende artikel voor meer informatie over hoe u uw gegevens migreert naar de nieuwe SQL-server.

> [!div class="nextstepaction"]
> [Een database migreren naar een SQL-VM](virtual-machines-windows-migrate-sql.md)