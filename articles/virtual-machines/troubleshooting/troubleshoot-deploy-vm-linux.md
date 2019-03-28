---
title: Problemen met implementatie Linux virtuele machine in Azure | Microsoft Docs
description: Problemen met implementatie Linux virtuele machine in het Azure Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: troubleshooting
ms.date: 11/01/2018
ms.author: genli
ms.openlocfilehash: 1317a4731d3598c5fba317167ba4a45d95823ca2
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539816"
---
# <a name="troubleshoot-deploying-linux-virtual-machine-issues-in-azure"></a>Problemen met implementatie Linux virtuele machine in Azure

Voor het oplossen van problemen bij de implementatie in Azure virtuele machine (VM), Controleer de [top problemen](#top-issues) voor veelvoorkomende fouten en oplossingen.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [virtual-machines-linux-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-linux-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>Het cluster kan niet de aangevraagde VM-grootte ondersteunt
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- Probeer de aanvraag met een kleinere virtuele machine.
- Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
    - Stop alle virtuele machines in de beschikbaarheidsset. Klik op **resourcegroepen** > uw resourcegroep > **Resources** > uw beschikbaarheidsset > **virtuele Machines** > uw virtuele machine >  **Stop**.
    - Nadat alle virtuele machines stoppen, moet u de virtuele machine maken in de gewenste grootte.
    - Start de nieuwe virtuele machine eerst en selecteert u elk van de gestopte VM's en klik op Start.


## <a name="the-cluster-does-not-have-free-resources"></a>Het cluster heeft geen gratis resources
\<properties supportTopicIds="123456789" resourceTags="windows" productPesIds="1234, 5678" />
- De aanvraag later opnieuw proberen.
- Als de nieuwe virtuele machine deel van een andere beschikbaarheidsset uitmaken kan
    - Een virtuele machine maken in een andere beschikbaarheidsset (in dezelfde regio).
    - De nieuwe virtuele machine toevoegen aan hetzelfde virtuele netwerk.

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hoe kan ik mijn maandelijkse tegoed voor Visual studio Enterprise (BizSpark) activeren

Activeer uw maandelijkse tegoed, ziet deze [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="why-can-i-not-install-the-gpu-driver-for-an-ubuntu-nv-vm"></a>Waarom kan ik het GPU-stuurprogramma niet voor een Ubuntu-VM NV installeren?

Linux-GPU-ondersteuning is momenteel alleen beschikbaar op Azure Netwerkcontroller-VM's waarop Ubuntu Server 16.04 LTS wordt uitgevoerd. Zie voor meer informatie, [GPU-stuurprogramma's instellen voor N-serie VM's waarop Linux wordt uitgevoerd](../linux/n-series-driver-setup.md).

## <a name="my-drivers-are-missing-for-my-linux-n-series-vm"></a>De stuurprogramma's ontbreken voor mijn Linux-VM uit de N-serie

Stuurprogramma's voor op basis van Linux-VM's zich bevinden [hier](../linux/n-series-driver-setup.md). 

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Ik kan een GPU-exemplaar niet vinden in mijn VM uit de N-serie

Als u wilt profiteren van de GPU-mogelijkheden van Azure uit de N-serie VM's met Windows Server 2016 of Windows Server 2012 R2, moet u NVIDIA grafische stuurprogramma's installeren op elke virtuele machine na de implementatie. Informatie over de installatie van stuurprogramma is beschikbaar voor [Windows VMs](../windows/n-series-driver-setup.md) en [virtuele Linux-machines](../linux/n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>Virtuele machines uit de N-serie is beschikbaar in mijn regio?

U kunt controleren op de beschikbaarheid van de [producten beschikbaar per regio tabel](https://azure.microsoft.com/regions/services), en prijzen [hier](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>Ik ben niet kunnen zien van familie van VM-grootte die ik nodig heb wanneer het formaat van mijn virtuele machine.

Wanneer een virtuele machine wordt uitgevoerd, wordt deze geïmplementeerd op een fysieke server. De fysieke servers in Azure-regio's worden gegroepeerd in clusters van algemene fysieke hardware. Formaat van een virtuele machine die de virtuele machine worden verplaatst naar andere hardwareclusters verschilt, afhankelijk van welke implementatiemodel is gebruikt voor het implementeren van de virtuele machine.

- Virtuele machines die worden geïmplementeerd in het klassieke implementatiemodel, de cloud service-implementatie moeten worden verwijderd en opnieuw worden geïmplementeerd om te wijzigen van de virtuele machines in een grootte in een andere serie met grootten.

- In het implementatiemodel van Resource Manager geïmplementeerde virtuele machines, moet u alle virtuele machines stoppen in de beschikbaarheidsset voor de grootte van alle virtuele machines in de beschikbaarheidsset te wijzigen.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>De vermelde VM-grootte wordt niet ondersteund tijdens de implementatie in de Beschikbaarheidsset.

Kies een grootte die wordt ondersteund op de beschikbaarheidsset cluster. Het verdient aanbeveling bij het maken van een beschikbaarheidsset ingesteld op de grootste VM-grootte kiezen u denkt dat u nodig hebt, en hebben dat de eerste implementatie voor de beschikbaarheidsset.

## <a name="what-linux-distributionsversions-are-supported-on-azure"></a>De Linux-distributies/versies worden ondersteund op Azure?

U kunt de lijst met op Linux vinden op [Azure-Endorsed distributies](../linux/endorsed-distros.md).

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Kan ik een bestaande klassieke virtuele machine toevoegen aan een beschikbaarheidsset?

Ja. U kunt een bestaande klassieke virtuele machine toevoegen aan een nieuwe of bestaande Beschikbaarheidsset. Zie voor meer informatie [een bestaande virtuele machine toevoegen aan een beschikbaarheidsset](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine).


## <a name="next-steps"></a>Volgende stappen
Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/).

U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
