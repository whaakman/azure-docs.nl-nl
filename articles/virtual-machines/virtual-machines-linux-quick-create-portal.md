---
title: Een virtuele Linux-machine maken met behulp van Azure Portal | Microsoft Docs
description: Een virtuele Linux-machine maken met behulp van Azure Portal.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 1/17/2016
ms.author: iainfou
translationtype: Human Translation
ms.sourcegitcommit: beff4fb41ed46b016088734054e7a7897fed1a30
ms.openlocfilehash: 7287b87b1e50e28de06a5363a1f35bd7ac34d51c


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Een virtuele Linux-machine in Azure maken met behulp van de portal
In dit artikel wordt beschreven hoe u met [Azure Portal](https://portal.azure.com/) snel een virtuele Linux-machine maakt.

De vereisten zijn:

* [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)
* [bestanden voor openbare en persoonlijke SSH-sleutels](virtual-machines-linux-mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

## <a name="sign-in"></a>Aanmelden
Meld u bij Azure Portal aan met uw Azure-account-id. Klik op **+ Nieuw** in de linkerbovenhoek:

![Een Azure-resource maken](./media/virtual-machines-linux-quick-create-portal/create_new_resource.png)

## <a name="choose-vm"></a>Kies VM
Klik in de **Marketplace** op **Compute** en selecteer **Ubuntu Server 16.04 LTS** in de installatiekopielijst **Aanbevolen apps**.  Controleer onderin of het implementatiemodel `Resource Manager` is en klik vervolgens op **Maken**.

![Selecteer de installatiekopie van een virtuele machine uit Azure Marketplace](./media/virtual-machines-linux-quick-create-portal/create_new_vm.png)

## <a name="enter-vm-options"></a>Voer VM-opties in
Voer op de pagina **Grondbeginselen** het volgende in:

* de gewenste naam voor de virtuele machine
* het type VM-schijf (standaard SSD, of HDD)
* de gebruikersnaam van de beheerder
* stel het **verificatietype** in op **Openbare SSH-sleutel**
* uw openbare SSH-sleutel als tekenreeks (uit uw `~/.ssh/`-map)
* een resourcegroepnaam, of selecteer een bestaande resourcegroep

en klik op **OK** om door te gaan. De blade moet er ongeveer uitzien als in de volgende schermafbeelding:

![De basisopties invoeren voor de virtuele Azure-machine](./media/virtual-machines-linux-quick-create-portal/enter_basic_vm_details.png)

## <a name="choose-vm-size"></a>VM-grootte kiezen
Kies een VM-grootte. In de volgende voorbeelden is **DS1_V2 Standard** gekozen, waarmee Ubuntu op een Premium SSD wordt geïnstalleerd. De **S** in de VM-grootte geeft aan dat er sprake is van SSD-ondersteuning. Klik op **Selecteren** om instellingen te configureren.

![Een Azure VM-grootte kiezen](./media/virtual-machines-linux-quick-create-portal/select_vm_size.png)

## <a name="storage-and-network"></a>Opslag en netwerk
Op de blade **Instellingen** kunt u ervoor kiezen om Azure Managed Disks te gebruiken voor uw virtuele machine. De huidige standaardinstelling is om niet-beheerde schijven te gebruiken. Azure Managed Disks worden verwerkt door het Azure-platform. Er is geen voorbereiding of locatie nodig om ze op te slaan. Zie [Overzicht van Azure Managed Disks](../storage/storage-managed-disks-overview.md) voor meer informatie over Azure Managed Disks. Voor niet-beheerde schijven moet u een opslagaccount maken of selecteren voor uw virtuele harde schijven:

![Opslagaccount selecteren voor niet-beheerde schijven](./media/virtual-machines-linux-quick-create-portal/configure_non_managed_disks.png)

Als u ervoor kiest om Azure Managed Disks te gebruiken, zijn er geen aanvullende opslagopties om te configureren, zoals blijkt uit het volgende voorbeeld:

![De optie Azure Managed Disks kiezen in de portal](./media/virtual-machines-linux-quick-create-portal/select_managed_disks.png)

Behoud voor de rest van de netwerkinstellingen de standaardwaarden.

## <a name="confirm-vm-settings-and-launch"></a>VM-instellingen bevestigen en starten
Bevestig de instellingen voor uw nieuwe virtuele Ubuntu-machine en klik op **OK**.

![De Azure VM-instellingen controleren en de virtuele machine maken](./media/virtual-machines-linux-quick-create-portal/review_final_vm_settings.png)

## <a name="select-the-vm-resource"></a>De VM-resource selecteren
Open de startpagina van de portal en selecteer **Resourcegroepen** in het menu in de linkerbovenhoek. Indien nodig klikt u op de drie balken boven aan het menu om de lijst als volgt uit te vouwen:

![Een lijst met resourcegroepen openen](./media/virtual-machines-linux-quick-create-portal/select_resource_group.png)

Selecteer de resourcegroep en klik vervolgens op de nieuwe virtuele machine:

![De Azure VM NIC-instellingen zoeken](./media/virtual-machines-linux-quick-create-portal/select_vm_resource.png)

## <a name="find-the-public-ip"></a>Het openbare IP-Adres zoeken
Bekijk het **openbare IP-adres** dat is toegewezen aan de virtuele machine:

![Het openbare IP-adres van de virtuele machine van Azure ophalen](./media/virtual-machines-linux-quick-create-portal/view_public_ip_address.png)

## <a name="ssh-to-the-vm"></a>SSH naar virtuele machine
Voeg SSH toe aan het openbare IP-adres met behulp van uw openbare SSH-sleutel.  Vanaf een Mac- of Linux-werkstation kunt u SSH direct vanaf de terminal uitvoeren. Als u op een Windows-workstation bent, moet u PuTTY, MobaXTerm of Cygwin voor SSH naar Linux gebruiken.  Als u dat nog niet hebt gedaan, is dit een document dat uw Windows-workstation gereed maakt voor SSH naar Linux.

[SSH-sleutels gebruiken met Windows op Azure](virtual-machines-linux-ssh-from-windows.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

```
ssh -i ~/.ssh/azure_id_rsa ops@40.112.255.214
```

## <a name="next-steps"></a>Volgende stappen
U hebt nu snel een virtuele Linux-machine gemaakt die u kunt gebruiken voor test- of demonstratiedoeleinden. Als u een virtuele Linux-machine wilt maken die is afgestemd op uw infrastructuur, volgt u de instructies in een van de volgende artikelen.

* [Een virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-cli-deploy-templates.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een met SSH beveiligde virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Een virtuele Linux-machine maken met behulp van de Azure CLI](virtual-machines-linux-create-cli-complete.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)




<!--HONumber=Feb17_HO3-->


