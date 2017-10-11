---
title: Een schijf koppelen aan een klassieke Azure-VM | Microsoft Docs
description: Een gegevensschijf koppelen aan een virtuele Windows-machine gemaakt met het klassieke implementatiemodel en het initialiseren.
services: virtual-machines-windows, storage
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: be4e3e74-05bc-4527-969f-84f10a1d66a7
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 02/21/2017
ms.author: cynthn
ms.openlocfilehash: 087d5cda354f6e1780bddd3725859444177abd16
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="attach-a-data-disk-to-a-windows-virtual-machine-created-with-the-classic-deployment-model"></a>Een gegevensschijf koppelen aan een virtuele Windows-machine die is gemaakt volgens het klassieke implementatiemodel
<!--
Refernce article:
    If you want to use the new portal, see [How to attach a data disk to a Windows VM in the Azure portal](../../virtual-machines-windows-attach-disk-portal.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
-->

In dit artikel leest u hoe nieuwe en bestaande schijven die zijn gemaakt met het klassieke implementatiemodel om een virtuele Windows-computer met de Azure portal te koppelen.

U kunt ook [een gegevensschijf koppelen aan een Linux-VM in de Azure portal](../../linux/attach-disk-portal.md).

Voordat u een schijf koppelen, controleert u de volgende tips:

* De omvang van de virtuele machine bepaalt hoeveel gegevensschijven die u kunt koppelen. Zie voor meer informatie [grootten voor virtuele machines](../../virtual-machines-windows-sizes.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Premium-opslag gebruiken, moet u een virtuele machine DS-serie- of GS-serie. Met deze virtuele machines kunt u schijven uit zowel Premium en Standard-opslagaccounts. Premium-opslag is beschikbaar in bepaalde regio's. Zie voor meer informatie [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../../../storage/common/storage-premium-storage.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

* Voor een nieuwe schijf hoeft u niet eerst maken omdat Azure gemaakt wanneer u dit aansluit.

U kunt ook [een gegevensschijf met behulp van Powershell koppelen](../../virtual-machines-windows-attach-disk-ps.md).

> [!IMPORTANT]
> Azure heeft twee verschillende implementatiemodellen voor het maken en werken met resources: [Resource Manager en Classic](../../../resource-manager-deployment-model.md).

## <a name="find-the-virtual-machine"></a>De virtuele machine gevonden
1. Meld u aan bij [Azure Portal](https://portal.azure.com/).
2. Selecteer de virtuele machine van de resource die worden vermeld op het dashboard.
3. Klik in het linkerdeelvenster onder **instellingen**, klikt u op **schijven**.

    ![Instellingen voor de schijf openen](./media/attach-disk/virtualmachinedisks.png)

Instructies voor het koppelen van een volgende om door te gaan een [nieuwe schijf](#option-1-attach-a-new-disk) of een [bestaande schijf](#option-2-attach-an-existing-disk).

## <a name="option-1-attach-and-initialize-a-new-disk"></a>Optie 1: Koppelen en een nieuwe schijf initialiseren

1. Op de **schijven** blade, klikt u op **Attach nieuwe**.
2. Controleer de standaardinstellingen, indien nodig bijwerken en klik vervolgens op **OK**.

   ![Controleer de Schijfinstellingen](./media/attach-disk/attach-new.png)

3. Nadat Azure de schijf wordt gemaakt en gekoppeld aan de virtuele machine, de nieuwe schijf wordt weergegeven in de instellingen voor de schijf van de virtuele machine onder **gegevensschijven**.

### <a name="initialize-a-new-data-disk"></a>Initialiseer de gegevensschijf van een nieuwe

1. Verbinding maken met de virtuele machine. Zie voor instructies [verbinding maken met en meld u aan een virtuele machine van Azure waarop Windows wordt uitgevoerd bij](../../virtual-machines-windows-connect-logon.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).
2. Nadat u zich aanmeldt bij de virtuele machine, opent u **Serverbeheer**. Selecteer in het linkerdeelvenster **File and Storage Services**.

    ![Open Serverbeheer](../media/attach-disk-portal/fileandstorageservices.png)

3. Selecteer **schijven**.
4. De **schijven** sectie vindt u de schijven. De meeste gevallen heeft een virtuele machine schijf 0, schijf van 1 en 2 schijf. Schijf 0 is de schijf van het besturingssysteem, schijf van 1 wordt de tijdelijke schijf en schijf 2 is de gegevensschijf die zojuist is gekoppeld aan de virtuele machine. De gegevensschijf geeft een lijst van de partitie als **onbekende**.

 Met de rechtermuisknop op de schijf en selecteer **initialiseren**.

5. U wordt gewaarschuwd dat alle gegevens worden gewist wanneer de schijf wordt geïnitialiseerd. Klik op **Ja** te bevestigen van de waarschuwing en initialiseer de schijf. Hierna kunt u de partitie wordt weergegeven als **GPT**. Opnieuw met de rechtermuisknop op de schijf en selecteer **NieuwVolume**.

6. Voltooi de wizard met de standaardwaarden. Wanneer de wizard is voltooid, de **Volumes** sectie vindt u het nieuwe volume. De schijf is nu online en gereed voor het opslaan van gegevens.

    ![Volume is geïnitialiseerd](./media/attach-disk/newdiskafterinitialization.png)

## <a name="option-2-attach-an-existing-disk"></a>Optie 2: Een bestaande schijf koppelen
1. Op de **schijven** blade, klikt u op **Attach bestaande**.
2. Onder **bestaande schijf koppelen**, klikt u op **locatie**.

   ![Bestaande schijf koppelen](./media/attach-disk/attachexistingdisksettings.png)
3. Onder **opslagaccounts**, selecteert u het account en de container waarin het VHD-bestand.

   ![Locatie van de VHD vinden](./media/attach-disk/existdiskstorageaccountandcontainer.png)

4. Selecteer het VHD-bestand.
5. Onder **bestaande schijf koppelen**, het bestand dat u zojuist hebt geselecteerd wordt vermeld onder **VHD-bestand**. Klik op **OK**.
6. Nadat Azure de schijf is gekoppeld aan de virtuele machine, wordt deze weergegeven in de instellingen voor de schijf van de virtuele machine onder **gegevensschijven**.

## <a name="use-trim-with-standard-storage"></a>Gebruik TRIM met standard-opslag

Als u een standard-opslag (HDD) gebruikt, moet u TRIM inschakelen. TRIM worden niet-gebruikte blokken op de schijf verwijderd, zodat u wordt alleen gefactureerd voor opslag die u daadwerkelijk gebruikt. Met ' trim ', kun kosten, met inbegrip van niet-gebruikte blokken die het gevolg zijn van grote bestanden worden verwijderd.

U kunt deze opdracht om te controleren van de beperkende instelling uitvoeren. Open een opdrachtprompt op de virtuele machine van Windows en typ:

```
fsutil behavior query DisableDeleteNotify
```

Als de opdracht 0 retourneert, wordt ' trim ' correct ingeschakeld. Als deze 1 retourneert, voert u de volgende opdracht voor het vrijmaken van opslagruimte inschakelen:
```
fsutil behavior set DisableDeleteNotify 0
```

## <a name="next-steps"></a>Volgende stappen
Als uw toepassing gebruikt de D: station voor het opslaan van gegevens, kunt u [wijzigen van de stationsletter van de tijdelijke schijf Windows](../../virtual-machines-windows-change-drive-letter.md).

## <a name="additional-resources"></a>Aanvullende bronnen
[Over schijven en virtuele harde schijven voor virtuele machines](../../virtual-machines-linux-about-disks-vhds.md)
