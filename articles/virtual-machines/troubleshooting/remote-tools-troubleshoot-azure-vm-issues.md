---
title: Externe hulpprogramma's gebruiken voor het oplossen van problemen met virtuele Azure-machine | Microsoft Docs
description: ''
services: virtual-machines-windows
documentationcenter: ''
author: Deland-Han
manager: cshepard
editor: ''
tags: ''
ms.service: virtual-machines
ms.topic: troubleshooting
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: azurecli
ms.date: 01/11/2018
ms.author: delhan
ms.openlocfilehash: d9bb13b24a16cc38f738d9a654789d4410225c09
ms.sourcegitcommit: 6135cd9a0dae9755c5ec33b8201ba3e0d5f7b5a1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/31/2018
ms.locfileid: "50633910"
---
# <a name="use-remote-tools-to-troubleshoot-azure-vm-issues"></a>Externe hulpprogramma's gebruiken om problemen met virtuele Azure-machine te

Wanneer u problemen op een Azure-machine (VM), kunt u verbinding met de virtuele machine met behulp van de externe hulpprogramma's die worden beschreven in dit artikel in plaats van Remote Desktop Protocol (RDP).

## <a name="serial-console"></a>Seriële Console

Gebruik [seriële Console van virtuele Machine](serial-console-windows.md) opdrachten uitvoeren op de externe Azure-VM.

## <a name="remote-cmd"></a>Externe CMD

Download [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec). Verbinding maken met de virtuele machine met de volgende opdracht:

```cmd
psexec \\<computer>-u user -s cmd
```

>[!Note]
>* De opdracht moet worden uitgevoerd op een computer die zich in hetzelfde VNET.
>* DIP of de hostnaam kan worden gebruikt ter vervanging <computer>.
>* De parameter -s zorgt ervoor dat de opdracht wordt aangeroepen met behulp van systeem-Account (beheerdersrechten).
>* PsExec maakt gebruik van TCP-poort 135 en 445. Daarom moeten de twee poorten op de Firewall zijn geopend.

## <a name="run-commands"></a>Uitvoeropdrachten

Zie [uitvoeren PowerShell-scripts in uw Windows-VM met de opdracht uitvoeren](../windows/run-command.md) voor meer informatie over het gebruik van de functie opdrachten uitvoeren scripts uit te voeren op de virtuele machine.

## <a name="customer-script-extension"></a>Voor Klantenscripts

U kunt de functie voor aangepaste Scriptextensie gebruiken om uit te voeren van een aangepast script op de doel-VM. Deze functie wilt gebruiken, moeten de volgende voorwaarden worden voldaan:

* De VM heeft verbinding.

* Azure-Agent is geïnstalleerd en werkt zoals verwacht op de virtuele machine.

* De extensie is niet eerder hebt geïnstalleerd op de virtuele machine.
 
  De extensie wordt het script invoeren alleen de eerste keer dat deze wordt gebruikt. Als u deze functie later gebruikt, wordt de extensie herkent dat deze al is gebruikt en wordt het nieuwe script niet uploaden.

U moet uw script uploaden naar een opslagaccount en genereren van een eigen container. Voer het volgende script in Azure PowerShell op een computer die verbonden met de virtuele machine is.

### <a name="for-v1-vms"></a>Voor V1-VM 's

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>" 
$storageAccount = "<<STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for the blob in the storage
$vmName = "<<VM NAME>>" 
$vmCloudService = "<<CLOUD SERVICE>>" #Resource group/Cloud Service where the VM is hosted. I.E.: For "demo305.cloudapp.net" the cloud service is going to be demo305

#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module Azure
Add-AzureAccount  #Ensure Login with account associated with subscription ID
Get-AzureSubscription -SubscriptionId $subscriptionID | Select-AzureSubscription

#Setup the access to the storage account and upload the script
$storageKey = (Get-AzureStorageKey -StorageAccountName $storageAccount).Primary
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)<
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
$vm = Get-AzureVM -ServiceName $vmCloudService -Name $vmName
Set-AzureVMCustomScriptExtension "CustomScriptExtension" -VM $vm -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName | Update-AzureVM
```

### <a name="for-v2-vms"></a>Voor de V2 VM 's

```powershell
#Setup the basic variables
$subscriptionID = "<<SUBSCRIPTION ID>>"
$storageAccount = "<<STORAGE ACCOUNT>>"
$storageRG = "<<RESOURCE GROUP OF THE STORAGE ACCOUNT>>" 
$localScript = "<<FULL PATH OF THE PS1 FILE TO EXECUTE ON THE VM>>" 
$blobName = "file.ps1" #Name you want for blob in storage
$vmName = "<<VM NAME>>" 
$vmResourceGroup = "<<RESOURCE GROUP>>"
$vmLocation = "<<DATACENTER>>" 
 
#Setup the Azure Powershell module and ensure the access to the subscription
Import-Module AzureRM
Login-AzureRmAccount #Ensure Login with account associated with subscription ID
Get-AzureRmSubscription -SubscriptionId $subscriptionID | Select-AzureRmSubscription

#Setup the access to the storage account and upload the script 
$storageKey = (Get-AzureRmStorageAccountKey -ResourceGroupName $storageRG -Name $storageAccount).Value[0]
$context = New-AzureStorageContext -StorageAccountName $storageAccount -StorageAccountKey $storageKey
$container = "cse" + (Get-Date -Format yyyyMMddhhmmss)
New-AzureStorageContainer -Name $container -Permission Off -Context $context
Set-AzureStorageBlobContent -File $localScript -Container $container -Blob $blobName  -Context $context

#Push the script into the VM
Set-AzureRmVMCustomScriptExtension -Name "CustomScriptExtension" -ResourceGroupName $vmResourceGroup -VMName $vmName -Location $vmLocation -StorageAccountName $storageAccount -StorageAccountKey $storagekey -ContainerName $container -FileName $blobName -Run $blobName
```

## <a name="remote-powershell"></a>Externe PowerShell

>[!Note]
>TCP-poort 5986 (HTTPS) moet zijn geopend zodat u deze optie kunt gebruiken.
>
>Voor ARM-VM's, moet u poort 5986 voor de netwerkbeveiligingsgroep (NSG) openen. Zie voor meer informatie-beveiligingsgroepen. 
>
>Voor de RDFE-VM's, hebt u een eindpunt waarvoor een particuliere poort (5986) en een openbare poort. Vervolgens moet u ook openen die openbare gerichte-poort op de NSG.

### <a name="set-up-the-client-computer"></a>Stel de clientcomputer

Voor het gebruik van PowerShell op afstand verbinding maken met de virtuele machine, hebt u eerst het instellen van de clientcomputer waarmee de verbinding. Om dit te doen, moet u de VM toevoegen aan de lijst met vertrouwde hosts PowerShell door het uitvoeren van de volgende opdracht waar nodig.

Een virtuele machine toevoegen aan lijst met vertrouwde hosts:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName>
```

Meerdere virtuele machines toevoegen aan lijst met vertrouwde hosts:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value <ComputerName1>,<ComputerName2>
```

Alle computers toevoegen aan de lijst met vertrouwde hosts:

```powershell
Set-Item wsman:\localhost\Client\TrustedHosts -value *
```

### <a name="enable-remoteps-on-the-vm"></a>RemotePS op de virtuele machine inschakelen

Voor klassieke virtuele machines, aangepaste Scriptextensie te gebruiken om uit te voeren in het volgende script:

```powershell
Enable-PSRemoting -Force
New-NetFirewallRule -Name "Allow WinRM HTTPS" -DisplayName "WinRM HTTPS" -Enabled True -Profile Any -Action Allow -Direction Inbound -LocalPort 5986 -Protocol TCP
$thumbprint = (New-SelfSignedCertificate -DnsName $env:COMPUTERNAME -CertStoreLocation Cert:\LocalMachine\My).Thumbprint
$command = "winrm create winrm/config/Listener?Address=*+Transport=HTTPS @{Hostname=""$env:computername""; CertificateThumbprint=""$thumbprint""}"
cmd.exe /C $command
```

Gebruik voor ARM-VM's, opdrachten uitvoeren vanuit de portal de EnableRemotePS-script uit te voeren:

![Run-opdracht](./media/remote-tools-troubleshoot-azure-vm-issues/run-command.png)

### <a name="connect-to-the-vm"></a>Verbinding maken met de virtuele machine

Computerlocatie van de volgende opdracht, afhankelijk van de client de uitvoering:

* Buiten het VNET of de implementatie

    * Voor een klassieke virtuele machine, moet u de volgende opdracht uitvoeren:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName  "<<CLOUDSERVICENAME.cloudapp.net>>" -port "<<PUBLIC PORT NUMBER>>" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

    * Voor een ARM-VM, moet u eerst een DNS-naam toevoegen aan het openbare IP-adres. Zie voor gedetailleerde stappen [een volledig gekwalificeerde domeinnaam voor een Windows-VM maken in Azure portal](../windows/portal-create-fqdn.md). Voer daarna de volgende opdracht uit:

    ```powershell
    $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
    Enter-PSSession -ComputerName "<<DNSname.DataCenter.cloudapp.azure.com>>" -port "5986" -Credential (Get-Credential) -useSSL -SessionOption $Skip
    ```

* Binnen het VNET of de implementatie, moet u de volgende opdracht uitvoeren:
  
  ```powershell
  $Skip = New-PSSessionOption -SkipCACheck -SkipCNCheck
  Enter-PSSession -ComputerName  "<<HOSTNAME>>" -port 5986 -Credential (Get-Credential) -useSSL -SessionOption $Skip
  ```

>[!Note] 
>Als u de vlag SkipCaCheck omzeilt de vereiste voor het importeren van een certificaat met de virtuele machine bij het starten van de sessie.

U kunt ook de cmdlet Invoke-opdracht gebruiken om uit te voeren van een script op de virtuele machine op afstand:

```powershell
Invoke-Command -ComputerName "<<COMPUTERNAME>" -ScriptBlock {"<<SCRIPT BLOCK>>"}
```

## <a name="remote-registry"></a>Remote Registry

>[!Note]
>TCP-poort 135 of 445 moet openen om deze optie gebruikt.
>
>U hebt open poort 5986 op de NSG voor ARM-VM's. Zie voor meer informatie-beveiligingsgroepen. 
>
>Voor de RDFE-VM's, moet u een eindpunt waarvoor een particuliere poort 5986 en een openbare poort hebben. U moet ook openen die openbare poort op de NSG.

1. Open de Register-editor (regedit.exe) in een andere VM in hetzelfde VNET.

2. Selecteer **bestand** >**verbinding maken met de netwerkregister**.

   ![Externe optie](./media/remote-tools-troubleshoot-azure-vm-issues/remote-registry.png) 

3. Ga naar de doel-VM door **hostnaam** of **dynamische IP-Adressen** (voorkeur) door te voeren in het vak 'Geef de naam van het object te selecteren'.

   ![Externe optie](./media/remote-tools-troubleshoot-azure-vm-issues/input-computer-name.png) 
 
4. Voer de referenties voor de doel-VM.

5. Breng de benodigde wijzigingen.

## <a name="remote-services-console"></a>Externe Services-Console

>[!Note]
>TCP-poort 135 of 445 moet openen om deze optie gebruikt.
>
>U hebt open poort 5986 op de NSG voor ARM-VM's. Zie voor meer informatie-beveiligingsgroepen. 
>
>Voor de RDFE-VM's, moet u een eindpunt waarvoor een particuliere poort 5986 en een openbare poort hebben. Vervolgens moet u ook openen die openbare poort op de NSG.

1. Uit een andere virtuele machine in hetzelfde VNET, opent u een exemplaar van **Services.msc**.

2. Met de rechtermuisknop op **Services (lokaal)**.

3. Selecteer **verbinding maken met een andere computer**.

   ![Externe service](./media/remote-tools-troubleshoot-azure-vm-issues/remote-services.png)

4. Voer de dynamische IP-adres van de doel-VM.

   ![Invoer DIP](./media/remote-tools-troubleshoot-azure-vm-issues/input-ip-address.png)

5. De benodigde wijzigingen aanbrengen in de services.

## <a name="next-steps"></a>Volgende stappen

[Enter-PSSession](https://technet.microsoft.com/library/hh849707.aspx)

[Aangepast Script-extensie voor Windows met behulp van het klassieke implementatiemodel](../extensions/custom-script-classic.md)

PsExec maakt deel uit van de [PSTools Suite](https://download.sysinternals.com/files/PSTools.zip).

Zie voor meer informatie over het pakket PSTools [PSTools Suite](https://docs.microsoft.com/sysinternals/downloads/pstools).


