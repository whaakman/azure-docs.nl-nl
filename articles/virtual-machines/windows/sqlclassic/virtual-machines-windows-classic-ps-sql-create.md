---
title: Een SQL Server-Machine maken in Azure PowerShell (klassiek) | Microsoft Docs
description: Bevat de stappen en PowerShell-scripts voor het maken van een Azure-VM met SQL Server-installatiekopieën voor virtuele machines galerie. In dit onderwerp wordt gebruikgemaakt van de klassieke implementatiemodus.
services: virtual-machines-windows
documentationcenter: na
author: MashaMSFT
manager: craigg
tags: azure-service-management
ms.assetid: b73be387-9323-4e08-be53-6e5928e3786e
ms.service: virtual-machines-sql
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows-sql-server
ms.workload: iaas-sql-server
ms.date: 08/07/2017
ms.author: mathoma
ms.reviewer: jroth
ms.openlocfilehash: ad8b59a9290c533a3687b5ff8956d8682fb6d9e9
ms.sourcegitcommit: dede0c5cbb2bd975349b6286c48456cfd270d6e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/16/2019
ms.locfileid: "54332282"
---
# <a name="provision-a-sql-server-virtual-machine-using-azure-powershell-classic"></a>Een SQL Server-machine inrichten met Azure PowerShell (klassiek)

Dit artikel bevat stappen voor het maken van een SQL Server-machine in Azure met behulp van de PowerShell-cmdlets.

> [!IMPORTANT] 
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../azure-resource-manager/resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken.

Zie voor de Resource Manager-versie van dit onderwerp, [een SQL Server-machine inrichten met Azure PowerShell Resource Manager](../sql/virtual-machines-windows-ps-sql-create.md).

### <a name="install-and-configure-powershell"></a>PowerShell installeren en configureren:
1. Als u geen Azure-account hebt, gaat u naar [Azure, gratis proefversie](https://azure.microsoft.com/pricing/free-trial/).
2. [Download en installeer de nieuwste Azure PowerShell-opdrachten](/powershell/azure/overview).
3. Start Windows PowerShell en verbinden met uw Azure-abonnement met de **Add-AzureAccount** opdracht.

   ```powershell
   Add-AzureAccount
   ```

## <a name="determine-your-target-azure-region"></a>Het doel-Azure-regio bepalen

Uw virtuele SQL Server-Machine wordt gehost in een cloudservice die een specifieke Azure-regio zich bevindt. De volgende stappen helpen u om te bepalen van uw regio, de storage-account en -service die wordt gebruikt voor de rest van de zelfstudie in de cloud.

1. Bepaal het datacenter dat u gebruiken wilt voor het hosten van uw SQL Server-VM. De volgende PowerShell-opdracht geeft een lijst van beschikbare regionamen.

   ```powershell
   (Get-AzureLocation).Name
   ```

2. Nadat u de locatie van uw voorkeur hebt geïdentificeerd, stelt u een variabele met de naam **$dcLocation** voor deze regio. De volgende opdracht stelt bijvoorbeeld het gebied 'Oost ons':

   ```powershell
   $dcLocation = "East US"
   ```

## <a name="set-your-subscription-and-storage-account"></a>Uw abonnement en storage-account in te stellen

1. Bepaal het Azure-abonnement dat u voor de nieuwe virtuele machine wilt gebruiken.

   ```powershell
   (Get-AzureSubscription).SubscriptionName
   ```

2. Het doel-Azure-abonnement toewijzen aan de **$subscr** variabele. Dit wordt ingesteld als uw huidige Azure-abonnement.

   ```powershell
   $subscr="<subscription name>"
   Select-AzureSubscription -SubscriptionName $subscr –Current
   ```

3. Vervolgens controleren op bestaande storage-accounts. Het volgende script ziet u alle opslagaccounts die zijn opgenomen in de regio van uw gekozen:

   ```powershell
   (Get-AzureStorageAccount | where { $_.GeoPrimaryLocation -eq $dcLocation }).StorageAccountName
   ```

   > [!NOTE]
   > Als u een nieuw opslagaccount vereist, moet u eerst de naam van een opslagaccount voor alle hoofdletters maken met de opdracht New-AzureStorageAccount zoals in het volgende voorbeeld: `New-AzureStorageAccount -StorageAccountName "<storage account name>" -Location $dcLocation`

4. Toewijzen van de naam van het doelopslagaccount op de **$staccount**. Gebruik vervolgens **instellen-Azure-abonnement** om in te stellen van het abonnement en de huidige storage-account.

   ```powershell
   $staccount="<storage account name>"
   Set-AzureSubscription -SubscriptionName $subscr -CurrentStorageAccountName $staccount
   ```

## <a name="select-a-sql-server-virtual-machine-image"></a>Selecteer een virtuele machine-installatiekopie van SQL Server

1. Meer informatie de lijst met beschikbare virtuele machines van SQL Server-installatiekopieën uit de galerie. Deze installatiekopieën hebben een **ImageFamily** eigenschap die met 'SQL begint'. De volgende query geeft de installatiekopie-familie beschikbaar waarmee u kunt SQL Server vooraf geïnstalleerd zijn.

   ```powershell
   Get-AzureVMImage | where { $_.ImageFamily -like "SQL*" } | select ImageFamily -Unique | Sort-Object -Property ImageFamily
   ```

2. Wanneer u de familie van de installatiekopie van virtuele machine hebt gevonden, kan dit worden veroorzaakt door meerdere gepubliceerde installatiekopieën in deze groep. Het volgende script gebruiken om te vinden van de naam van de meest recente gepubliceerde virtuele machine installatiekopie voor de geselecteerde installatiekopie-familie (zoals **SQL Server 2016 RTM Enterprise op Windows Server 2012 R2**):

   ```powershell
   $family="<ImageFamily value>"
   $image=Get-AzureVMImage | where { $_.ImageFamily -eq $family } | sort PublishedDate -Descending | select -ExpandProperty ImageName -First 1

   echo "Selected SQL Server image name:"
   echo "   $image"
   ```

## <a name="create-the-virtual-machine"></a>De virtuele machine maken

Maak ten slotte de virtuele machine met PowerShell:

1. Een cloudservice voor het hosten van de nieuwe virtuele machine maken. Houd er rekening mee dat het is ook mogelijk in plaats daarvan gebruik van een bestaande cloudservice. Maak een nieuwe variabele **$svcname** met de korte naam van de cloudservice.

   ```powershell
   $svcname = "<cloud service name>"
   New-AzureService -ServiceName $svcname -Label $svcname -Location $dcLocation
   ```

2. Geef de naam van de virtuele machine en een grootte. Zie voor meer informatie over de grootten van virtuele machines, [Virtual Machine Sizes for Azure](../sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

   ```powershell
   $vmname="<machine name>"
   $vmsize="<Specify one: Large, ExtraLarge, A5, A6, A7, A8, A9, or see the link to the other VM sizes>"
   $vm1=New-AzureVMConfig -Name $vmname -InstanceSize $vmsize -ImageName $image
   ```

3. Geef op de lokale administrator-account en het wachtwoord.

   ```powershell
   $cred=Get-Credential -Message "Type the name and password of the local administrator account."
   $vm1 | Add-AzureProvisioningConfig -Windows -AdminUsername $cred.GetNetworkCredential().Username -Password $cred.GetNetworkCredential().Password
   ```

4. Voer het volgende script voor het maken van de virtuele machine.

   ```powershell
   New-AzureVM –ServiceName $svcname -VMs $vm1
   ```

> [!NOTE]
> Zie voor aanvullende uitleg en configuratie-opties, de **bouwen van de opdrachtenset** in sectie [Azure PowerShell gebruiken om te maken en vooraf configureren van virtuele Windows-Machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

## <a name="example-powershell-script"></a>Voorbeeld PowerShell-script

Het volgende script geeft een voorbeeld van een volledige script waarmee u maakt een **SQL Server 2016 RTM Enterprise op Windows Server 2012 R2** virtuele machine. Als u dit script gebruikt, moet u de eerste variabelen op basis van de vorige stappen in dit onderwerp aanpassen.

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

1. De RDP-bestanden in de map voor het document om te starten van deze virtuele machines naar een volledige installatie van de huidige gebruiker maken:

   ```powershell
   $documentspath = [environment]::getfolderpath("mydocuments")
   Get-AzureRemoteDesktopFile -ServiceName $svcname -Name $vmname -LocalPath "$documentspath\vm1.rdp"
   ```

2. Start het RDP-bestand in de map documenten. Verbinding maken met de gebruikersnaam van beheerder en het wachtwoord die eerder is verkregen (bijvoorbeeld, als de naam van de gebruiker is VMAdmin, "\VMAdmin" opgeven als de gebruiker en geeft u het wachtwoord).

   ```powershell
   cd $documentspath
   .\vm1.rdp
   ```

## <a name="complete-the-configuration-of-the-sql-server-machine-for-remote-access"></a>Voltooi de configuratie van de SQL Server-Machine voor externe toegang

Nadat u bent aangemeld bij de machine met extern bureaublad configureren van SQL Server op basis van de instructies in [stappen voor het configureren van SQL Server-connectiviteit in een Azure-VM](virtual-machines-windows-classic-sql-connect.md#steps-for-configuring-sql-server-connectivity-in-an-azure-vm).

## <a name="next-steps"></a>Volgende stappen

U vindt aanvullende instructies voor het inrichten van virtuele machines met PowerShell in de [documentatie voor virtuele machines](../classic/create-powershell.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).

In veel gevallen is de volgende stap voor het migreren van uw databases naar deze nieuwe virtuele machine voor SQL-Server. Zie voor hulp bij het migreren van de database, [een Database migreren naar SQL Server op een Azure-VM](../sql/virtual-machines-windows-migrate-sql.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fsqlclassic%2ftoc.json).

Als u ook geïnteresseerd bent in met behulp van de Azure-portal voor het maken van virtuele Machines van SQL, Zie [inrichten van een SQL Server-Machine in Azure](../sql/virtual-machines-windows-portal-sql-server-provision.md). Houd er rekening mee dat de zelfstudie maakt u kennis met de portal virtuele machines met behulp van de aanbevolen Resource Manager-model, in plaats van het klassieke model dat wordt gebruikt in dit onderwerp PowerShell maakt.

Behalve deze resources, raden wij u [andere onderwerpen met betrekking tot het uitvoeren van SQL Server in Azure Virtual Machines](../sql/virtual-machines-windows-sql-server-iaas-overview.md).
