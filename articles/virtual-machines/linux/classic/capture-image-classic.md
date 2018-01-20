---
title: Een installatiekopie van een Linux-VM | Microsoft Docs
description: Ontdek hoe u een installatiekopie van een Linux-gebaseerde Azure virtuele machine (VM) gemaakt met het klassieke implementatiemodel.
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ROBOTS: NOINDEX
ms.assetid: 17d7ffee-a58e-4290-9de1-64c3cf1ddc05
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/14/2017
ms.author: iainfou
ms.openlocfilehash: f63a2cdeaec512bed4fd0f86ef7c9d5a9b77d7f6
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Een klassieke virtuele Linux-machine vastleggen als installatiekopie
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Dit artikel ziet u het vastleggen van een klassieke Azure virtuele machine (VM) met Linux als een installatiekopie van een andere virtuele machines maken. Deze installatiekopie bevat de besturingssysteemschijf en gegevensschijven gekoppeld aan de virtuele machine. Het bevat geen netwerkconfiguratie, dus u hoeft te configureren die bij het maken van de andere virtuele machine uit de afbeelding.

Azure slaat de installatiekopie onder **installatiekopieën**, samen met alle installatiekopieën die u hebt geüpload. Zie voor meer informatie over installatiekopieën [over installatiekopieën van virtuele machines in Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Voordat u begint
Deze stappen wordt ervan uitgegaan dat u hebt al een virtuele machine in Azure met behulp van het klassieke implementatiemodel gemaakt en geconfigureerd, het besturingssysteem, met inbegrip van eventuele gegevensschijven koppelen. Als u een virtuele machine maken moet, lezen [het maken van een virtuele Linux-Machine][How to Create a Linux Virtual Machine].

## <a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen
1. [Verbinding maken met de virtuele machine](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) met behulp van een SSH-client van uw keuze.
2. Typ de volgende opdracht in het venster SSH. De uitvoer van `waagent` mogelijk enigszins verschillen afhankelijk van de versie van dit hulpprogramma:

    ```bash
    sudo waagent -deprovision+user
    ```

    De voorgaande opdracht probeert te reinigen van het systeem, waardoor het geschikt is voor reprovisioning. Deze bewerking wordt uitgevoerd de volgende taken:

   * Hiermee verwijdert u SSH-sleutels voor host (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand is)
   * Hiermee wist u de configuratie van de naamserver in /etc/resolv.conf
   * Hiermee verwijdert u de `root` gebruikerswachtwoord uit/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand)
   * Verwijdering van DHCP-clientlease uit de cache
   * Herstel van hostnaam naar localhost.localdomain
   * Hiermee verwijdert u de laatste ingericht gebruikersaccount (verkregen uit /var/lib/waagent) **en bijbehorende gegevens**.

     > [!NOTE]
     > Opheffen van inrichting verwijdert alle bestanden en gegevens naar 'generaliseer' van de installatiekopie. Deze opdracht alleen uitvoeren op een virtuele machine die u van plan bent om vast te leggen als een nieuwe sjabloon voor de installatiekopie. Er is geen garantie dat de installatiekopie wordt gewist van alle gevoelige informatie of geschikt is voor de herdistributie van derden.

3. Type **y** om door te gaan. U kunt toevoegen de `-force` parameter om te voorkomen dat deze bevestigingsstap.
4. Type **afsluiten** om de SSH-client te sluiten.

   > [!NOTE]
   > De resterende stappen wordt ervan uitgegaan dat u al hebt [geïnstalleerd van de Azure CLI](../../../cli-install-nodejs.md) op de clientcomputer. De volgende stappen kunnen ook worden uitgevoerd in de [Azure-portal](http://portal.azure.com).

5. Open Azure CLI en meld u aan bij uw Azure-abonnement op de clientcomputer. Voor meer informatie lezen [verbinding maken met een Azure-abonnement met de Azure CLI](/cli/azure/authenticate-azure-cli).

   > [!NOTE]
   > In de Azure portal, moet u zich aanmelden bij de portal.

6. Zorg ervoor dat u in Service Management-modus:

    ```azurecli
    azure config mode asm
    ```

7. Sluit de virtuele machine die is al gemaakt. Het volgende voorbeeld wordt de virtuele machine met de naam afgesloten `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Indien nodig, kunt u een lijst van alle VM's in uw abonnement is gemaakt met behulp van weergeven`azure vm list`

   > [!NOTE]
   > Als u de Azure-portal, selecteert u de virtuele machine en klik op **stoppen** afsluiten van de virtuele machine.

8. Wanneer de virtuele machine is gestopt, moet u de installatiekopie vastlegt. Het volgende voorbeeld wordt de virtuele machine met de naam vastgelegd `myVM` en maakt u een algemene installatiekopie met de naam `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    De `-t` subopdracht wordt verwijderd van de oorspronkelijke virtuele machine.

    > [!NOTE]
    > In de Azure portal kunt u een installatiekopie vastleggen selecteren **installatiekopie** in het hubmenu. U moet de volgende informatie voor de installatiekopie opgeven: naam, resourcegroep, locatie, het besturingssysteemtype en pad van opslag-blob.

9. De nieuwe installatiekopie is nu beschikbaar in de lijst met afbeeldingen die kunnen worden gebruikt voor het configureren van een nieuwe virtuele machine. Met de opdracht kunt u deze bekijken:

   ```azurecli
   azure vm image list
   ```

   Op de [Azure-portal](http://portal.azure.com), de nieuwe installatiekopie wordt weergegeven in de **VM-installatiekopieën (klassiek)** die hoort bij de **Compute** services. U hebt toegang tot **VM-installatiekopieën (klassiek)** door te klikken op _meer services_ lijst aan de onderkant van de Azure-service en bekijkt u de **Compute** services.   

   ![De installatiekopie is geslaagd](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Volgende stappen
De installatiekopie is gereed om te worden gebruikt voor het maken van virtuele machines. U kunt de Azure CLI-opdracht `azure vm create` en geef de naam van de installatiekopie die u hebt gemaakt. Zie voor meer informatie [met de Azure CLI met het klassieke implementatiemodel](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

In plaats daarvan gebruiken de [Azure-portal](http://portal.azure.com) voor het maken van een aangepaste virtuele machine met behulp van de **installatiekopie** methode en de installatiekopie die u hebt gemaakt te selecteren. Zie voor meer informatie [het maken van een aangepaste VM][How to Create a Custom Virtual Machine].

**Zie ook:** [gebruikershandleiding voor Azure Linux-Agent](../agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom-classic.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk-classic.md
[How to Create a Linux Virtual Machine]:create-custom-classic.md
