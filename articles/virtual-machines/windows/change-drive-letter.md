---
title: 'Het station D: van een virtuele machine een gegevensschijf | Microsoft Docs'
description: 'Beschrijft hoe stationsletters voor een virtuele machine van Windows te wijzigen zodat u het station D: als een gegevensstation gebruiken kunt.'
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager,azure-service-management
ms.assetid: 0867a931-0055-4e31-8403-9b38a3eeb904
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/02/2018
ms.author: cynthn
ms.openlocfilehash: a4e29ce7ebb20753c3313e1148e1c91e853f49f5
ms.sourcegitcommit: 2e540e6acb953b1294d364f70aee73deaf047441
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
ms.locfileid: "27577442"
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Het station D: gebruiken als een gegevensstation op een virtuele machine van Windows
Als uw toepassing het D-station gebruiken moet voor het opslaan van gegevens, volgt u deze instructies voor het gebruik van een andere stationsletter voor de tijdelijke schijf. Gebruik nooit de tijdelijke schijf voor het opslaan van gegevens die u wilt bewaren.

Als u de grootte of **stoppen (Deallocate)** een virtuele machine, kan dit de plaatsing van de virtuele machine naar een nieuwe hypervisor activeren. Een gebeurtenis gepland of ongepland onderhoud activeren mogelijk ook deze plaatsing. In dit scenario wordt de tijdelijke schijf worden toegewezen aan de eerste beschikbare stationsletter. Als u een toepassing die specifiek het station D: moet hebt, moet u als volgt te werk om tijdelijk pagefile.sys verplaatsen, een nieuwe gegevensschijf koppelen en de letter D toewijzen en verplaats pagefile.sys terug naar de tijdelijke schijf. Hierna kunt Azure niet terug te nemen de D: als de virtuele machine wordt verplaatst naar een andere hypervisor.

Zie voor meer informatie over hoe Azure gebruikt voor de tijdelijke schijf [inzicht in de tijdelijke schijf op Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-the-data-disk"></a>De gegevensschijf koppelen
U moet eerst de gegevensschijf koppelen aan de virtuele machine. Om dit te doen met behulp van de portal, Zie [het koppelen van een beheerde gegevensschijf in de Azure portal](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Tijdelijk verplaatsen pagefile.sys naar C-schijf
1. Verbinding maken met de virtuele machine. 
2. Met de rechtermuisknop op de **Start** menu en selecteer **System**.
3. Selecteer in het menu links **Geavanceerde systeeminstellingen**.
4. In de **prestaties** sectie **instellingen**.
5. Selecteer de **Geavanceerd** tabblad.
6. In de **virtueel geheugen** sectie **wijziging**.
7. Selecteer de **C** station en klik vervolgens op **systeem beheerde grootte** en klik vervolgens op **ingesteld**.
8. Selecteer de **D** station en klik vervolgens op **geen wisselbestand** en klik vervolgens op **ingesteld**.
9. Klik op toepassen. U ontvangt een waarschuwing dat de computer moet opnieuw worden opgestart om de wijzigingen door te voeren.
10. Start opnieuw op de virtuele machine.

## <a name="change-the-drive-letters"></a>De stationsletters wijzigen
1. Nadat de virtuele machine opnieuw is opgestart, meld u weer aan bij de virtuele machine.
2. Klik op de **Start** menu en typ **diskmgmt.msc** en druk op Enter. Schijfbeheer wordt gestart.
3. Met de rechtermuisknop op **D**, wordt het station voor tijdelijke opslag, en selecteer **wijziging stationsletter en paden**.
4. Selecteer onder de stationsletter, een nieuw station, zoals **T** en klik vervolgens op **OK**. 
5. Met de rechtermuisknop op de gegevensschijf en selecteer **wijziging stationsletter en paden**.
6. Selecteer onder de stationsletter, station **D** en klik vervolgens op **OK**. 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Pagefile.sys teruggaan naar het station voor tijdelijke opslag
1. Met de rechtermuisknop op de **Start** menu en selecteer **systeem**
2. Selecteer in het menu links **Geavanceerde systeeminstellingen**.
3. In de **prestaties** sectie **instellingen**.
4. Selecteer de **Geavanceerd** tabblad.
5. In de **virtueel geheugen** sectie **wijziging**.
6. Selecteer het station OS **C** en klik op **geen wisselbestand** en klik vervolgens op **ingesteld**.
7. Selecteer het station voor tijdelijke opslag **T** en klik vervolgens op **systeem beheerde grootte** en klik vervolgens op **ingesteld**.
8. Klik op **Toepassen**. U ontvangt een waarschuwing dat de computer moet opnieuw worden opgestart om de wijzigingen door te voeren.
9. Start opnieuw op de virtuele machine.

## <a name="next-steps"></a>Volgende stappen
* U kunt de beschikbare opslag verhogen met uw virtuele machine door [koppelen van een extra gegevensschijf](attach-managed-disk-portal.md).

