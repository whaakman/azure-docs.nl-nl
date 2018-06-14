---
title: Maak een zoned Windows-VM met de Azure portal | Microsoft Docs
description: Een virtuele Windows-machine maken in een zone beschikbaarheid met de Azure-portal
services: virtual-machines-windows
documentationcenter: virtual-machines
author: dlepow
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: ''
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 03/27/2018
ms.author: danlep
ms.custom: ''
ms.openlocfilehash: 3d3561cf1ad760930821fabeef9839c25d55f2a9
ms.sourcegitcommit: 20d103fb8658b29b48115782fe01f76239b240aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2018
ms.locfileid: "30321985"
---
# <a name="create-a-windows-virtual-machine-in-an-availability-zone-with-the-azure-portal"></a>Maken van een virtuele Windows-machine in een zone beschikbaarheid met de Azure-portal

In dit artikel wordt stapsgewijs via de Azure portal gebruiken voor het maken van een virtuele machine in een zone Azure beschikbaarheid. Een [beschikbaarheidszone](../../availability-zones/az-overview.md) is een fysiek afgescheiden zone in een Azure-regio. Gebruik beschikbaarheidszones om uw apps en gegevens te beschermen tegen het onwaarschijnlijke risico van een storing of het verloren gaan van een heel datacenter.

Voor het gebruik van een zone beschikbaarheid, maakt u uw virtuele machine in een [ondersteunde Azure-regio](../../availability-zones/az-overview.md#regions-that-support-availability-zones).

## <a name="log-in-to-azure"></a>Meld u aan bij Azure. 

Meld u aan bij Azure Portal op https://portal.azure.com.

## <a name="create-virtual-machine"></a>Virtuele machine maken

1. Klik in de linkerbovenhoek van Azure Portal op **Een resource maken**.

2. Selecteer **Compute** en vervolgens **Windows Server 2016 Datacenter**. 

3. Geef de informatie van de virtuele machine op. De gebruikersnaam en het wachtwoord die u hier opgeeft, worden gebruikt voor aanmelding bij de virtuele machine. Het wachtwoord moet minstens 12 tekens lang zijn en moet voldoen aan de [gedefinieerde complexiteitsvereisten](faq.md#what-are-the-password-requirements-when-creating-a-vm). Kies een locatie zoals VS-Oost 2 die ondersteuning biedt voor beschikbaarheid zones. Na het voltooien klikt u op **OK**.

    ![Voer basisinformatie over uw virtuele machine in op de portalblade](./media/create-portal-availability-zone/create-windows-vm-portal-basic-blade.png)

4. Kies een grootte voor de virtuele machine. Selecteer een aanbevolen grootte of filter op basis van functies. Bevestig dat het formaat is beschikbaar in de zone die u wilt gebruiken.

    ![Selecteer een VM-grootte](./media/create-portal-availability-zone/create-windows-vm-portal-sizes.png)  

5. Onder **instellingen** > **hoge beschikbaarheid**, selecteer een van de genummerde zones van de **beschikbaarheid zone** dropdown, blijven de overige standaardinstellingen, en Klik op **OK**.

    ![Selecteer een zone beschikbaarheid](./media/create-portal-availability-zone/create-windows-vm-portal-availability-zone.png)

6. Klik op de pagina overzicht **maken** implementatie van virtuele machine te starten.

7. De VM wordt aan het dashboard van de Azure Portal vastgemaakt. Zodra de implementatie is voltooid, wordt de samenvatting van de VM automatisch geopend.

## <a name="confirm-zone-for-managed-disk-and-ip-address"></a>Zone voor beheerde schijf- en IP-adres bevestigen

Wanneer de virtuele machine is geïmplementeerd in een zone beschikbaarheid, wordt een beheerde schijf voor de virtuele machine gemaakt in dezelfde regio bevindt beschikbaarheid. Een openbaar IP-adres wordt standaard ook gemaakt in de zone.

U kunt bevestigen dat de zone-instellingen voor deze bronnen in de portal.  

1. Klik op **resourcegroepen** en vervolgens de naam van de resource te groeperen voor de virtuele machine, zoals *myResourceGroup*.

2. Klik op de naam van de schijfbron. De **overzicht** pagina bevat gedetailleerde gegevens over de locatie en de beschikbaarheid van de zone van de resource.

    ![Beschikbaarheid van zone voor beheerde schijf](./media/create-portal-availability-zone/create-windows-vm-portal-disk.png)

3. Klik op de naam van de bron van het openbare IP-adres. De **overzicht** pagina bevat gedetailleerde gegevens over de locatie en de beschikbaarheid van de zone van de resource.

    ![Beschikbaarheid van zone voor IP-adres](./media/create-portal-availability-zone/create-windows-vm-portal-ip.png)



## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd hoe u een virtuele machine maken in een zone beschikbaarheid. Meer informatie over [regio's en beschikbaarheid](regions-and-availability.md) voor virtuele Azure-machines.
