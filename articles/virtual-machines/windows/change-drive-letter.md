---
title: Controleer de D:-schijf van een virtuele machine een gegevensschijf | Microsoft Docs
description: 'Beschrijft hoe u stationsletters wijzigt voor een Windows-VM, zodat u het station D: als gegevensstation kunt.'
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
ms.openlocfilehash: cfd46d5e9750a81d89ed6d3a79bcc9bffdc3d0dd
ms.sourcegitcommit: 5839af386c5a2ad46aaaeb90a13065ef94e61e74
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/19/2019
ms.locfileid: "58007288"
---
# <a name="use-the-d-drive-as-a-data-drive-on-a-windows-vm"></a>Het station D: als gegevensstation op een Windows-VM gebruiken
Als uw toepassing moet de D-station wilt gebruiken voor het opslaan van gegevens, volgt u deze instructies voor het gebruik van een andere stationsletter voor de tijdelijke schijf. Gebruik nooit de tijdelijke schijf voor het opslaan van gegevens die u nodig hebt om te houden.

Als u het formaat of **stoppen (toewijzing ongedaan maken)** een virtuele machine, dit plaatsing van de virtuele machine naar een nieuwe hypervisor kan activeren. Een geplande of onvoorziene onderhoudsgebeurtenis kan ook deze plaatsing activeren. In dit scenario wordt de tijdelijke schijf wordt toegewezen aan de eerste beschikbare stationsletter. Als u een toepassing waarbij specifiek de D:-schijf hebt, moet u deze stappen voor het tijdelijk pagefile.sys verplaatsen, een nieuwe gegevensschijf koppelen en de letter D toewijzen en vervolgens de pagefile.sys terug verplaatsen naar het tijdelijke station. Als u klaar bent, Azure niet terug te nemen de D: als de virtuele machine wordt verplaatst naar een andere hypervisor.

Zie voor meer informatie over hoe Azure gebruikt voor de tijdelijke schijf [inzicht krijgen in het tijdelijke station op Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="attach-the-data-disk"></a>De gegevensschijf koppelen
U moet eerst de gegevensschijf koppelen aan de virtuele machine. Om dit te doen met behulp van de portal, Zie [hoe u een beheerde gegevensschijf koppelen in de Azure-portal](attach-managed-disk-portal.md).

## <a name="temporarily-move-pagefilesys-to-c-drive"></a>Tijdelijk verplaatsen pagefile.sys naar C-schijf
1. Maak verbinding met de virtuele machine. 
2. Met de rechtermuisknop op de **Start** menu en selecteer **System**.
3. Selecteer in het menu links **Geavanceerde systeeminstellingen**.
4. In de **prestaties** sectie, selecteer **instellingen**.
5. Selecteer de **Geavanceerd** tabblad.
6. In de **virtueel geheugen** sectie, selecteer **wijziging**.
7. Selecteer de **C** station en klik vervolgens op **systeem beheerde grootte** en klik vervolgens op **ingesteld**.
8. Selecteer de **D** station en klik vervolgens op **geen wisselbestand** en klik vervolgens op **ingesteld**.
9. Klik op toepassen. U ontvangt een waarschuwing dat de computer moet opnieuw worden opgestart om de wijzigingen door te voeren.
10. Start de virtuele machine.

## <a name="change-the-drive-letters"></a>De stationsletters wijzigen
1. Zodra de virtuele machine opnieuw is opgestart, meld u terug naar de virtuele machine.
2. Klik op de **Start** menu en voert u **diskmgmt.msc** en druk op Enter. Schijfbeheer wordt gestart.
3. Met de rechtermuisknop op **D**, wordt het station voor tijdelijke opslag, en selecteer **wijziging stationsletter en paden**.
4. Selecteer onder de stationsletter, een nieuw station, zoals **T** en klik vervolgens op **OK**. 
5. Met de rechtermuisknop op de gegevensschijf en selecteer **wijziging stationsletter en paden**.
6. Selecteer onder de stationsletter, station **D** en klik vervolgens op **OK**. 

## <a name="move-pagefilesys-back-to-the-temporary-storage-drive"></a>Pagefile.sys terug verplaatsen naar de tijdelijke opslagschijf
1. Met de rechtermuisknop op de **Start** menu en selecteer **System**
2. Selecteer in het menu links **Geavanceerde systeeminstellingen**.
3. In de **prestaties** sectie, selecteer **instellingen**.
4. Selecteer de **Geavanceerd** tabblad.
5. In de **virtueel geheugen** sectie, selecteer **wijziging**.
6. Selecteer het station OS **C** en klikt u op **geen wisselbestand** en klik vervolgens op **ingesteld**.
7. Selecteer het station voor tijdelijke opslag **T** en klik vervolgens op **systeem beheerde grootte** en klik vervolgens op **ingesteld**.
8. Klik op **Toepassen**. U ontvangt een waarschuwing dat de computer moet opnieuw worden opgestart om de wijzigingen door te voeren.
9. Start de virtuele machine.

## <a name="next-steps"></a>Volgende stappen
* U kunt de beschikbare opslagruimte verhogen met uw virtuele machine door [Bezig met koppelen van een extra-gegevensschijf](attach-managed-disk-portal.md).

