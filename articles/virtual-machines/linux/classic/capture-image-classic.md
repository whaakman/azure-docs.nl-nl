---
title: Een installatiekopie maken van een Linux-VM | Microsoft Docs
description: Leer hoe u een installatiekopie maken van een op basis van Linux Azure-machine (VM) die zijn gemaakt met het klassieke implementatiemodel.
services: virtual-machines-linux
documentationcenter: ''
author: cynthn
manager: jeconnoc
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
ms.author: cynthn
ms.openlocfilehash: ae87af45ffc442c0de6c7f703694a994e536cdb8
ms.sourcegitcommit: aa988666476c05787afc84db94cfa50bc6852520
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2018
ms.locfileid: "37929209"
---
# <a name="how-to-capture-a-classic-linux-virtual-machine-as-an-image"></a>Een klassieke virtuele Linux-machine vastleggen als installatiekopie
> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken van en werken met resources: [Resource Manager en klassieke](../../../resource-manager-deployment-model.md). In dit artikel bevat informatie over met behulp van het klassieke implementatiemodel. U doet er verstandig aan voor de meeste nieuwe implementaties het Resource Manager-model te gebruiken. Lees [meer informatie over het uitvoeren van deze stappen met het Resource Manager-model](../capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> [!INCLUDE [virtual-machines-common-classic-createportal](../../../../includes/virtual-machines-classic-portal.md)]

Dit artikel leest u hoe om vast te leggen van een klassieke Azure-machine (VM) waarop Linux wordt uitgevoerd als een installatiekopie van een andere virtuele machines maken. Deze installatiekopie bevat de besturingssysteemschijf en gegevensschijven die zijn gekoppeld aan de virtuele machine. Deze bevat geen configuratie van netwerken, dus u hoeft te configureren dat wanneer u de andere virtuele machine van de installatiekopie maken.

Azure slaat de installatiekopie onder **installatiekopieën**, samen met alle installatiekopieën die u hebt geüpload. Zie voor meer informatie over installatiekopieën [About VM Images in Azure][About Virtual Machine Images in Azure].

## <a name="before-you-begin"></a>Voordat u begint
Deze stappen wordt ervan uitgegaan dat u hebt al een Azure-VM met behulp van het klassieke implementatiemodel gemaakt en geconfigureerd, het besturingssysteem, met inbegrip van eventuele gegevensschijven koppelen. Als u een virtuele machine maken wilt, lezen [over het maken van een virtuele Linux-Machine][How to Create a Linux Virtual Machine].

## <a name="capture-the-virtual-machine"></a>De virtuele machine vastleggen
1. [Verbinding maken met de virtuele machine](../mac-create-ssh-keys.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) met behulp van een SSH-client van uw keuze.
2. Typ de volgende opdracht in het venster SSH. De uitvoer van `waagent` enigszins kunnen variëren afhankelijk van de versie van dit hulpprogramma:

    ```bash
    sudo waagent -deprovision+user
    ```

    De voorgaande opdracht wordt geprobeerd te reinigen van het systeem en geef deze geschikt is voor beëindiging. Met deze bewerking worden de volgende taken uitgevoerd:

   * Hiermee verwijdert u SSH-hostsleutels (als Provisioning.RegenerateSshHostKeyPair 'y' in het configuratiebestand is)
   * Hiermee wist de configuratie van de naamserver in /etc/resolv.conf
   * Hiermee verwijdert u de `root` gebruikerswachtwoord uit/etc/shadow (als Provisioning.DeleteRootPassword 'y' in het configuratiebestand)
   * Verwijdering van DHCP-clientlease uit de cache
   * Herstel van hostnaam naar localhost.localdomain
   * Verwijdert de laatste ingerichte gebruikersaccount (verkregen van /var/lib/waagent) **en bijbehorende gegevens**.

     > [!NOTE]
     > Opheffen van inrichting verwijdert alle bestanden en gegevens naar het 'generaliseer' van de installatiekopie. Alleen deze opdracht uitvoeren op een virtuele machine die u van plan bent om vast te leggen als een nieuwe installatiekopie-sjabloon. Er is geen garantie dat de installatiekopie van alle gevoelige informatie is uitgeschakeld of geschikt voor het opnieuw distribueren aan derden is.

3. Type **y** om door te gaan. U kunt toevoegen de `-force` parameter om te voorkomen dat deze bevestigingsstap.
4. Type **afsluiten** te sluiten van de SSH-client.

   > [!NOTE]
   > De resterende stappen wordt ervan uitgegaan dat u al hebt [de Azure CLI geïnstalleerd](../../../cli-install-nodejs.md) op uw clientcomputer. De volgende stappen kunnen ook worden uitgevoerd in de [Azure-portal](http://portal.azure.com).

5. Open Azure CLI en meld u aan bij uw Azure-abonnement vanaf uw clientcomputer. Lees voor meer informatie, [verbinding met een Azure-abonnement via de Azure CLI](/cli/azure/authenticate-azure-cli).

   > [!NOTE]
   > In de Azure-portal, moet u zich aanmelden bij de portal.

6. Zorg ervoor dat u zich in Service Management-modus:

    ```azurecli
    azure config mode asm
    ```

7. Sluit de virtuele machine die is al de inrichting ongedaan gemaakt. Het volgende voorbeeld wordt de virtuele machine met de naam afgesloten `myVM`:

    ```azurecli
    azure vm shutdown myVM
    ```
   Indien nodig, kunt u een lijst met alle virtuele machines in uw abonnement gemaakt met behulp van weergeven `azure vm list`

   > [!NOTE]
   > Als u de Azure-portal, selecteert u de virtuele machine en klikt u op **stoppen** om de virtuele machine af te sluiten.

8. Wanneer de virtuele machine wordt gestopt, moet u de installatiekopie vastleggen. Het volgende voorbeeld bevat de virtuele machine met de naam `myVM` en maakt een gegeneraliseerde installatiekopie met de naam `myNewVM`:

    ```azurecli
    azure vm capture -t myVM myNewVM
    ```

    De `-t` subopdracht Hiermee verwijdert u de oorspronkelijke virtuele machine.

    > [!NOTE]
    > In de Azure-portal, kunt u een installatiekopie vastleggen door te selecteren **installatiekopie** in het hubmenu. U moet de volgende informatie voor de installatiekopie opgeven: naam, resourcegroep, locatie, besturingssysteemtype en het pad naar opslag.

9. De nieuwe installatiekopie is nu beschikbaar in de lijst met installatiekopieën die kunnen worden gebruikt voor het configureren van een nieuwe virtuele machine. U kunt deze weergeven met de opdracht:

   ```azurecli
   azure vm image list
   ```

   Op de [Azure-portal](http://portal.azure.com), de nieuwe installatiekopie wordt weergegeven in de **VM-installatiekopieën (klassiek)** die behoort tot de **Compute** services. U hebt toegang tot **VM-installatiekopieën (klassiek)** door te klikken op **alle services** lijst aan de bovenkant van de Azure-service en bekijkt u de **Compute** services.   

   ![Vastleggen van een installatiekopie geslaagd](./media/capture-image/VMCapturedImageAvailable.png)

## <a name="next-steps"></a>Volgende stappen
De installatiekopie is gereed om te worden gebruikt voor het maken van virtuele machines. U kunt de Azure CLI-opdracht `azure vm create` en geef de naam van de installatiekopie die u hebt gemaakt. Zie voor meer informatie, [met de Azure CLI met het klassieke implementatiemodel](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

Of gebruik de [Azure-portal](http://portal.azure.com) een aangepaste VM maken met behulp van de **installatiekopie** methode en selecteren van de installatiekopie die u hebt gemaakt. Zie voor meer informatie, [over het maken van een aangepaste virtuele machine][How to Create a Custom Virtual Machine].

**Zie ook:** [gebruikershandleiding voor Azure Linux Agent](../../extensions/agent-linux.md)

[About Virtual Machine Images in Azure]:../../virtual-machines-linux-classic-about-images.md
[How to Create a Custom Virtual Machine]:create-custom-classic.md
[How to Attach a Data Disk to a Virtual Machine]:attach-disk-classic.md
[How to Create a Linux Virtual Machine]:create-custom-classic.md
