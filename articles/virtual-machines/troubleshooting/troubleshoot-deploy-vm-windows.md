---
title: Problemen met implementatie Windows virtuele machine in Azure | Microsoft Docs
description: Problemen met implementatie Windows virtuele machine in het Azure Resource Manager-implementatiemodel.
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
ms.openlocfilehash: 5752731f08a7dc9ae8661e698aef9655837c6220
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58540700"
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Problemen met implementatie Windows virtuele machine in Azure

Voor het oplossen van problemen bij de implementatie in Azure virtuele machine (VM), Controleer de [top problemen](#top-issues) voor veelvoorkomende fouten en oplossingen.

Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

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

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Hoe kan ik gebruiken en een windows client-installatiekopie implementeren in Azure?

U kunt Windows 7, Windows 8 of Windows 10 in Azure voor dev/test-scenario's als u een juiste Visual Studio (voorheen MSDN)-abonnement hebt. Dit [artikel](../windows/client-images.md) geeft een overzicht van de vereisten voor geschiktheid voor actieve Windows-client in Azure en maakt gebruik van de Azure-galerie met installatiekopieën.

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a>Hoe kan ik een virtuele machine met behulp van Hybrid Use Benefit (HUB) implementeren?

Er zijn een aantal verschillende manieren voor het implementeren van Windows-machines met Azure Hybrid Use Benefit.

Voor een Enterprise Agreement-abonnement:

• VM's implementeren vanaf een specifieke Marketplace-installatiekopieën die vooraf geconfigureerd met Azure Hybrid Use Benefit zijn.

Voor Enterprise agreement:

• Een aangepaste VM uploaden en implementeren met behulp van een Resource Manager-sjabloon of Azure PowerShell.

Zie de volgende bronnen voor meer informatie:

 - [Overzicht van Azure Hybrid Use Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [Veelgestelde vragen over het downloaden](https://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Azure Hybrid Use Benefit voor WindowsServer en Windows-](../windows/hybrid-use-benefit-licensing.md).

 - [Hoe kan ik het Hybrid Use Benefit gebruiken in Azure](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hoe kan ik mijn maandelijkse tegoed voor Visual studio Enterprise (BizSpark) activeren

Activeer uw maandelijkse tegoed, ziet deze [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a>Hoe kan Enterprise Dev/Test toevoegen aan mijn Enterprise Agreement (EA) toegang te krijgen tot venster-clientinstallatiekopieën?

Alleen accounteigenaren die hiervoor zijn gemachtigd door een Enterprise-beheerder, kunnen abonnementen maken op basis van de Enterprise Dev/Test-aanbieding. De accounteigenaar maakt abonnementen via de Azure Account Portal en vervolgens als medebeheerders Visual Studio-abonnees moet toevoegen. Zodat ze kunnen beheren en gebruiken van de benodigde bronnen voor ontwikkelen en testen. Zie voor meer informatie, [Enterprise Dev/Test](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>De stuurprogramma's ontbreken voor mijn VM Windows-N-serie

Stuurprogramma's voor Windows-VM's zich bevinden [hier](../windows/n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Ik kan een GPU-exemplaar niet vinden in mijn VM uit de N-serie

Als u wilt profiteren van de GPU-mogelijkheden van Azure uit de N-serie VM's met Windows Server 2016 of Windows Server 2012 R2, moet u NVIDIA grafische stuurprogramma's installeren op elke virtuele machine na de implementatie. Informatie over de installatie van stuurprogramma is beschikbaar voor [Windows VMs](../windows/n-series-driver-setup.md) en [virtuele Linux-machines](../linux/n-series-driver-setup.md).

## <a name="is-n-series-vms-available-in-my-region"></a>Virtuele machines uit de N-serie is beschikbaar in mijn regio?

U kunt controleren op de beschikbaarheid van de [producten beschikbaar per regio tabel](https://azure.microsoft.com/regions/services), en prijzen [hier](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a>Welke client-afbeeldingen kan ik gebruiken en implementeren in Azure, en hoe aan krijg ik die?

Kunt u Windows 7, Windows 8 of Windows 10 in Azure voor dev/test-scenario's die dat u hebt een geschikt abonnement op Visual Studio (voorheen MSDN). 

- Windows 10-installatiekopieën beschikbaar zijn via Azure Gallery in [in aanmerking komende dev/test-aanbiedingen](../windows/client-images.md#eligible-offers). 
- Visual Studio-abonnees binnen elk type aanbieding kunnen ook [voldoende voorbereiden en maak](../windows/prepare-for-upload-vhd-image.md) een 64-bits Windows 7, Windows 8 of Windows 10-installatiekopie en vervolgens [uploaden naar Azure](../windows/upload-generalized-managed.md). Het gebruik blijft beperkt tot ontwikkelen en testen door actieve Visual Studio-abonnees.

Dit [artikel](../windows/client-images.md) geeft een overzicht van de vereisten voor geschiktheid voor actieve Windows-client in Azure en het gebruik van de Azure-galerie met installatiekopieën.

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>Ik ben niet kunnen zien van familie van VM-grootte die ik nodig heb wanneer het formaat van mijn virtuele machine.

Wanneer een virtuele machine wordt uitgevoerd, wordt deze geïmplementeerd op een fysieke server. De fysieke servers in Azure-regio's worden gegroepeerd in clusters van algemene fysieke hardware. Formaat van een virtuele machine die de virtuele machine worden verplaatst naar andere hardwareclusters verschilt, afhankelijk van welke implementatiemodel is gebruikt voor het implementeren van de virtuele machine.

- Virtuele machines die worden geïmplementeerd in het klassieke implementatiemodel, de cloud service-implementatie moeten worden verwijderd en opnieuw worden geïmplementeerd om te wijzigen van de virtuele machines in een grootte in een andere serie met grootten.

- In het implementatiemodel van Resource Manager geïmplementeerde virtuele machines, moet u alle virtuele machines stoppen in de beschikbaarheidsset voor de grootte van alle virtuele machines in de beschikbaarheidsset te wijzigen.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>De vermelde VM-grootte wordt niet ondersteund tijdens de implementatie in de Beschikbaarheidsset.

Kies een grootte die wordt ondersteund op de beschikbaarheidsset cluster. Het verdient aanbeveling bij het maken van een beschikbaarheidsset ingesteld op de grootste VM-grootte kiezen u denkt dat u nodig hebt, en hebben dat de eerste implementatie voor de beschikbaarheidsset.

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Kan ik een bestaande klassieke virtuele machine toevoegen aan een beschikbaarheidsset?

Ja. U kunt een bestaande klassieke virtuele machine toevoegen aan een nieuwe of bestaande Beschikbaarheidsset. Zie voor meer informatie [een bestaande virtuele machine toevoegen aan een beschikbaarheidsset](/previous-versions/azure/virtual-machines/windows/classic/configure-availability-classic#addmachine).


## <a name="next-steps"></a>Volgende stappen
Als u hulp nodig hebt op elk gewenst moment in dit artikel, u kunt contact opnemen met de Azure-experts op [het Azure MSDN en Stack Overflow-forums](https://azure.microsoft.com/support/forums/).

U kunt ook een Azure-ondersteuning-incident indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
