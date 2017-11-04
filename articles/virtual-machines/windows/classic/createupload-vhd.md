---
title: Maken en uploaden van een VM-installatiekopie met behulp van Powershell | Microsoft Docs
description: Informatie over het maken en uploaden van een algemene Windows Server-installatiekopie (VHD) met behulp van het klassieke implementatiemodel en Azure Powershell.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8c4a08fe-7714-4bf0-be87-c728a7806d3f
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 05/23/2017
ms.author: cynthn
ms.openlocfilehash: bc75c8cdd98b0ea0fbff6483c0e3c9d4468d3941
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-and-upload-a-windows-server-vhd-to-azure"></a>Een Windows Server VHD maken en uploaden naar Azure
In dit artikel laat zien hoe uw eigen algemene VM-installatiekopie uploaden als een virtuele harde schijf (VHD), zodat u deze gebruiken kunt om virtuele machines te maken. Zie voor meer informatie over schijven en VHD's in Microsoft Azure [over schijven en virtuele harde schijven voor virtuele Machines](../about-disks-and-vhds.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. U kunt ook [uploaden](../upload-generalized-managed.md) een virtuele machine met het Resource Manager-model.

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* **Een Azure-abonnement** -als u niet hebt, kunt u [gratis een Azure-account openen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* **[Microsoft Azure PowerShell](/powershell/azure/overview)**  -u hebt de Microsoft Azure PowerShell-module geïnstalleerd en geconfigureerd voor het gebruik van uw abonnement.
* **A. VHD-bestand** - ondersteunde Windows-besturingssysteem dat is opgeslagen in een .vhd-bestand en gekoppeld aan een virtuele machine. Controleer of de serverfuncties die wordt uitgevoerd op de VHD worden ondersteund door Sysprep. Zie voor meer informatie [Sysprep-ondersteuning voor serverrollen](https://msdn.microsoft.com/windows/hardware/commercialize/manufacture/desktop/sysprep-support-for-server-roles).

    > [!IMPORTANT]
    > De VHDX-indeling wordt niet ondersteund in Microsoft Azure. U kunt de schijf converteren naar VHD-indeling met behulp van Hyper-V-beheer of de [cmdlet Convert-VHD](http://technet.microsoft.com/library/hh848454.aspx). Zie voor meer informatie dit [blogbericht](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

## <a name="step-1-prep-the-vhd"></a>Stap 1: De VHD voorbereiden
Voordat u de VHD naar Azure uploaden, moet deze worden gegeneraliseerd met het hulpprogramma Sysprep. Dit zorgt ervoor dat de VHD moet worden gebruikt als een afbeelding. Zie voor meer informatie over Sysprep [hoe gebruik Sysprep: An Introduction](http://technet.microsoft.com/library/bb457073.aspx). Back-up van de virtuele machine voordat Sysprep wordt uitgevoerd.

Voer de volgende procedure uit de virtuele machine waar het besturingssysteem is geïnstalleerd:

1. Meld u aan het besturingssysteem.
2. Open een opdrachtpromptvenster als administrator. Wijzig de map in **%windir%\system32\sysprep**, en voer vervolgens `sysprep.exe`.

    ![Open een opdrachtpromptvenster](./media/createupload-vhd/sysprep_commandprompt.png)
3. De **hulpprogramma voor systeemvoorbereiding** dialoogvenster wordt weergegeven.

   ![Sysprep starten](./media/createupload-vhd/sysprepgeneral.png)
4. In de **hulpprogramma voor systeemvoorbereiding**, selecteer **Voer System Out of Box Experience (OOBE)** en zorg ervoor dat **Generalize** is ingeschakeld.
5. In **afsluitopties**, selecteer **afsluiten**.
6. Klik op **OK**.

## <a name="step-2-create-a-storage-account-and-a-container"></a>Stap 2: Een opslagaccount en een container maken
U moet een opslagaccount in Azure, zodat u een plaats voor het uploaden van het VHD-bestand hebt. Deze stap wordt beschreven hoe u een account maken of de gegevens die u moet ophalen uit een bestaand account. Vervang de variabelen in &lsaquo; haken &rsaquo; met uw eigen gegevens.

1. Aanmelden

    ```powershell
    Add-AzureAccount
    ```

2. Stel uw Azure-abonnement.

    ```powershell
    Select-AzureSubscription -SubscriptionName <SubscriptionName>
    ```

3. Maak een nieuw opslagaccount. De naam van het opslagaccount moet uniek zijn, 3 tot 24 tekens. De naam mag uit een combinatie van letters en cijfers. U moet ook een locatie zoals 'Oost ons' opgeven

    ```powershell
    New-AzureStorageAccount –StorageAccountName <StorageAccountName> -Location <Location>
    ```

4. Het nieuwe opslagaccount als standaard instellen.

    ```powershell
    Set-AzureSubscription -CurrentStorageAccountName <StorageAccountName> -SubscriptionName <SubscriptionName>
    ```

5. Maak een nieuwe container.

    ```powershell
    New-AzureStorageContainer -Name <ContainerName> -Permission Off
    ```

## <a name="step-3-upload-the-vhd-file"></a>Stap 3: Het VHD-bestand uploaden
Gebruik de [Add-AzureVhd](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevhd) om de VHD te uploaden.

Vanuit de Azure PowerShell-venster u in de vorige stap hebt gebruikt, typt u de volgende opdracht en vervangt de variabelen in &lsaquo; haken &rsaquo; met uw eigen gegevens.

```powershell
Add-AzureVhd -Destination "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -LocalFilePath <LocalPathtoVHDFile>
```

## <a name="step-4-add-the-image-to-your-list-of-custom-images"></a>Stap 4: De installatiekopie toevoegen aan de lijst met aangepaste installatiekopieën
Gebruik de [toevoegen AzureVMImage](https://docs.microsoft.com/en-us/powershell/module/azure/add-azurevmimage) cmdlet om de installatiekopie toevoegen aan de lijst met aangepaste installatiekopieën.

```powershell
Add-AzureVMImage -ImageName <ImageName> -MediaLocation "https://<StorageAccountName>.blob.core.windows.net/<ContainerName>/<vhdName>.vhd" -OS "Windows"
```

## <a name="next-steps"></a>Volgende stappen
U kunt nu [maken van een aangepaste VM](createportal.md) met behulp van de installatiekopie die u hebt geüpload.
