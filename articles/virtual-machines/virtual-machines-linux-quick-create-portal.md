---
title: Een virtuele Linux-machine maken met behulp van Azure Portal | Microsoft Docs
description: Een virtuele Linux-machine maken met behulp van Azure Portal.
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: cc5dc395-dc54-4402-8804-2bb15aba8ea2
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: hero-article
ms.date: 10/28/2016
ms.author: v-livech
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: b1446cd8892e14988ff428eaa03233f8e9aefb8a


---
# <a name="create-a-linux-vm-on-azure-using-the-portal"></a>Een virtuele Linux-machine in Azure maken met behulp van de portal
In dit artikel wordt beschreven hoe u met [Azure Portal](https://portal.azure.com/) snel een virtuele Linux-machine maakt.

De vereisten zijn:

* [een Azure-account.](https://azure.microsoft.com/pricing/free-trial/)
* [bestanden voor openbare en persoonlijke SSH-sleutels](virtual-machines-linux-mac-create-ssh-keys.md)

## <a name="sign-in"></a>Aanmelden
Meld u bij Azure Portal aan met uw Azure-account en klik in de linkerbovenhoek op **+ Nieuw**:

![scherm1](../media/virtual-machines-linux-quick-create-portal/screen1.png)

## <a name="choose-vm"></a>Kies VM
Klik in de **Marketplace** op **Virtual Machines** en vervolgens op **Ubuntu Server 14.04 TNS** in de installatiekopielijst **Aanbevolen apps**.  Controleer onderin of het implementatiemodel `Resource Manager` is en klik vervolgens op **Maken**.

![scherm2](../media/virtual-machines-linux-quick-create-portal/screen2.png)

## <a name="enter-vm-options"></a>Voer VM-opties in
Voer op de pagina **Grondbeginselen** het volgende in:

* de gewenste naam voor de virtuele machine
* de gebruikersnaam van de beoogde beheerder
* het verificatietype (dit moet zijn ingesteld op **Openbare SSH-sleutel**)
* uw openbare SSH-sleutel als tekenreeks (uit uw `~/.ssh/`-map)
* een resourcegroep of selecteer een bestaande resourcegroep

Klik vervolgens op **OK** om door te gaan en de grootte van uw virtuele machine te kiezen. Dit ziet er ongeveer als de volgende schermopname uit:

![scherm3](../media/virtual-machines-linux-quick-create-portal/screen3.png)

## <a name="choose-vm-size"></a>VM-grootte kiezen
Kies de **DS1**-grootte, waarmee Ubuntu wordt geïnstalleerd op een Premium-SSD, en klik op **Selecteren** om de instellingen te configureren.

![scherm4](../media/virtual-machines-linux-quick-create-portal/screen4.png)

## <a name="storage-and-network"></a>Opslag en netwerk
Laat bij **Instellingen** de standaardwaarden voor Opslag en Netwerk onveranderd en klik op **OK** om het overzicht weer te geven.  U ziet dat door uw keuze voor DS1 het schijftype is ingesteld op Premium-SSD (de **S** staat voor SSD).

![scherm5](../media/virtual-machines-linux-quick-create-portal/screen5.png)

## <a name="confirm-vm-settings-and-launch"></a>VM-instellingen bevestigen en starten
Bevestig de instellingen voor uw nieuwe virtuele Ubuntu-machine en klik op **OK**.

![scherm6](../media/virtual-machines-linux-quick-create-portal/screen6.png)

## <a name="find-the-vm-nic"></a>De VM NIC zoeken
Open het dashboard van de portal en kies in **Netwerkinterfaces** uw NIC.

![scherm7](../media/virtual-machines-linux-quick-create-portal/screen7.png)

## <a name="find-the-public-ip"></a>Het openbare IP-Adres zoeken
Open onder de NIC-instellingen het menu Openbare IP-adressen.

![scherm8](../media/virtual-machines-linux-quick-create-portal/screen8.png)

## <a name="ssh-to-the-vm"></a>SSH naar virtuele machine
Voeg SSH toe aan het openbare IP-adres met behulp van uw openbare SSH-sleutel.  Vanaf een Mac- of Linux-werkstation kunt u SSH direct vanaf de terminal uitvoeren. Als u op een Windows-workstation bent, moet u PuTTY, MobaXTerm of Cygwin voor SSH naar Linux gebruiken.  Als u dat nog niet hebt gedaan, is dit een document dat uw Windows-workstation gereed maakt voor SSH naar Linux.

[SSH-sleutels gebruiken met Windows op Azure](virtual-machines-linux-ssh-from-windows.md)

```
ssh -i ~/.ssh/azure_id_rsa ubuntu@13.91.99.206
```

## <a name="next-steps"></a>Volgende stappen
U hebt nu snel een virtuele Linux-machine gemaakt die u kunt gebruiken voor test- of demonstratiedoeleinden. Als u een virtuele Linux-machine wilt maken die is afgestemd op uw infrastructuur, volgt u de instructies in een van de volgende artikelen.

* [Een virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-cli-deploy-templates.md)
* [Een met SSH beveiligde virtuele Linux-machine in Azure maken met behulp van sjablonen](virtual-machines-linux-create-ssh-secured-vm-from-template.md)
* [Een virtuele Linux-machine maken met behulp van de Azure CLI](virtual-machines-linux-create-cli-complete.md)




<!--HONumber=Nov16_HO2-->


