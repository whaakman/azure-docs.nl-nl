---
title: Maken van een SQL Server-Machine in Azure PowerShell (klassiek) | Microsoft Docs
description: "Bevat de stappen en PowerShell-scripts voor het maken van een virtuele machine in Azure met SQL Server-installatiekopieën voor virtuele machine-galerie. In dit onderwerp maakt gebruik van de klassieke implementatiemodus."
services: virtual-machines-windows
documentationcenter: na
author: rothja
manager: jhubbard
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: jroth
ms.openlocfilehash: c3bd4329e8a22ce8503d6593560d29c2a3135e83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Een SQL Server-machine met Azure PowerShell (klassiek) inrichten

Dit artikel bevat stappen voor het maken van een SQL Server-machine in Azure met behulp van de PowerShell-cmdlets.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Zie voor de Resource Manager-versie van dit onderwerp, [een met Azure PowerShell-Resource Manager van SQL Server-machine inrichten](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>PowerShell installeren en configureren:
1. Als u geen Azure-account hebt, gaat u naar [Azure, gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
2. [Download en installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview).
3. Start Windows PowerShell en verbinden met uw Azure-abonnement met de **Add-AzureAccount** opdracht.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Het doel-Azure-regio bepalen

Uw virtuele Machine van SQL Server zal worden gehost in een cloudservice die een specifieke Azure-regio. De volgende stappen helpen u om te bepalen van uw regio, de storage-account en cloudservice die wordt gebruikt voor de rest van de zelfstudie.

1. Bepaal het datacenter die u gebruiken wilt voor het hosten van uw SQL Server-VM. De volgende PowerShell-opdracht geeft een lijst met beschikbare regionamen.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Zodra u de gewenste locatie hebt geïdentificeerd, instellen van een variabele met de naam **$dcLocation** regio. De volgende opdracht stelt bijvoorbeeld de regio aan 'Oost ons':

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Uw abonnement en storage-account instellen

1. Bepaal het Azure-abonnement dat u voor de nieuwe virtuele machine gebruiken wilt.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Het doel-Azure-abonnement toewijzen aan de **$subscr** variabele. Dit wordt ingesteld als uw huidige Azure-abonnement.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Vervolgens kunt u controleren op bestaande opslagaccounts. Het volgende script geeft alle opslagaccounts die zijn opgenomen in uw gekozen regio:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Als u een nieuw opslagaccount vereist, moet u eerst een opslagaccountnaam alle kleine maken met de opdracht New-AzureStorageAccount zoals in het volgende voorbeeld:`New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Toewijzen van de doel-opslagaccountnaam naar de **$staccount**. Gebruik vervolgens **Set-AzureSubscription** instellen van het abonnement en de huidige opslagaccount.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>Selecteer de installatiekopie van een SQL Server-virtuele machine

1. Ontdek de lijst met beschikbare virtuele machines van SQL Server-installatiekopieën uit de galerie. Deze installatiekopieën alle hebben een **ImageFamily** eigenschap die met 'SQL begint'. De volgende query worden de installatiekopie-familie beschikbaar weergegeven waarmee u kunt SQL Server vooraf geïnstalleerd hebt.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Wanneer u de installatiekopie van virtuele machine familie gevonden, kan dit worden veroorzaakt door meerdere gepubliceerde afbeeldingen in deze familie. Het volgende script gebruiken om te vinden van de naam van de meest recente gepubliceerde virtuele machine installatiekopie voor de geselecteerde installatiekopie-familie (zoals **RTM Enterprise van SQL Server 2016 op Windows Server 2012 R2**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

Maak ten slotte de virtuele machine met PowerShell:

1. Maak een cloudservice voor het hosten van de nieuwe virtuele machine. Houd er rekening mee dat het is ook mogelijk gebruik van een bestaande cloudservice in plaats daarvan. Maak een nieuwe variabele **$svcname** met de korte naam van de cloudservice.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Geef de naam van de virtuele machine en een grootte. Zie voor meer informatie over de grootte van virtuele machines, [grootten van virtuele machines voor Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Het lokale administrator-account en wachtwoord opgeven.

   ```powershell
   $cred=Get-Credential -Message "Type the name and password of the local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Voer het volgende script voor het maken van de virtuele machine.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> Zie voor aanvullende uitleg en configuratie-opties, de **bouwen van uw opdrachtset** in sectie [gebruik Azure PowerShell om te maken en vooraf configureren van virtuele Machines op basis van Windows](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Voorbeeld van de PowerShell-script

Het volgende script geeft een voorbeeld van een volledige-script maakt een **RTM Enterprise van SQL Server 2016 op Windows Server 2012 R2** virtuele machine. Als u dit script gebruikt, moet u de initiële variabelen op basis van de vorige stappen in dit onderwerp aanpassen.

```powershell
# Customize these variables based on your settings and requirements:
$dcLocation = "East US"
$subscr="mysubscription"
$staccount="mystorageaccount"
$family="SQL Server 2016 RTM Enterprise on Windows Server 2012 R2"
$svcname = "mycloudservice"
$vmname="myvirtualmachine"
$vmsize="A5"

# Set the current subscription and storage account
# Comment out the New-AzureStorageAccount line if the account already exists
Select-AzureSubscription -SubscriptionName $subscr –Current
New-AzureStorageAccount -StorageAccountName $staccount -Location $dcLocation
Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount

# Select the most recent VM image in this image family:
$image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

# Create the new cloud service; comment out this line if cloud service exists already:
New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation

# Create the VM config:
$vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image

# Set administrator credentials:
$cred=Get-Credential -Message "Type the name and password of the local administrator account."
$vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password

# Create the SQL Server VM:
New-AzureVM –ServiceName $svcname -VMs $vm1
```

## <a name="connect-with-remote-desktop"></a>Verbinding maken met extern bureaublad

1. Maakt de RDP-bestanden in de huidige gebruiker documentmap om deze virtuele machines, voert u de installatie te starten:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Start het RDP-bestand in de map documenten. Verbinding maken met de beheerder-gebruikersnaam en wachtwoord die eerder is verkregen (bijvoorbeeld, als uw gebruikersnaam VMAdmin, '\VMAdmin' opgeven als de gebruiker en geeft u het wachtwoord).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Voltooi de configuratie van de Machine van SQL Server voor externe toegang

Na aanmelding bij de computer met extern bureaublad, configureert u de SQL Server op basis van de instructies in [stappen voor het configureren van SQL-serververbindingen in een Azure VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Volgende stappen

U vindt aanvullende instructies voor het inrichten van virtuele machines met PowerShell in de [documentatie virtual machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

In veel gevallen worden de volgende stap is om te migreren van uw databases naar deze nieuwe virtuele machine een SQL-Server. Zie voor instructies voor het migreren van database [een Database migreren naar SQL Server op een virtuele machine van Azure](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Als u ook geïnteresseerd bent in met behulp van de Azure-portal voor het maken van virtuele Machines van SQL, Zie [inrichten van een virtuele Machine van SQL Server op Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md). Houd er rekening mee dat de zelfstudie die u bij de portal helpt worden gemaakt met virtuele machines met de aanbevolen Resource Manager-model, in plaats van het klassieke model in dit onderwerp PowerShell gebruikt.

Naast deze resources, wordt aangeraden te controleren [andere onderwerpen die betrekking hebben op het SQL Server wordt uitgevoerd in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
