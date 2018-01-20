---
title: Problemen met implementatie Windows virtuele machine in Azure | Microsoft Docs
description: Problemen met implementatie Windows virtuele machine in Azurethe Resource Manager-implementatiemodel.
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/03/2017
ms.author: genli
ms.openlocfilehash: adbe54dec363b454d64340bf3a862cdead2e5435
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="troubleshoot-deploying-windows-virtual-machine-issues-in-azure"></a>Problemen met implementatie Windows virtuele machine in Azure

Voor het oplossen van problemen bij de implementatie in Azure virtuele machine (VM), Controleer de [top problemen](#top-issues) voor algemene fouten en oplossingen.

Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/). U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.

## <a name="top-issues"></a>Meest voorkomende problemen
[!INCLUDE [virtual-machines-windows-troubleshoot-deploy-vm-top](../../../includes/virtual-machines-windows-troubleshoot-deploy-vm-top.md)]

## <a name="the-cluster-cannot-support-the-requested-vm-size"></a>Het cluster kan het aangevraagde VM-grootte niet ondersteunen.
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- Probeer de aanvraag met een kleinere VM.
- Als de grootte van de aangevraagde virtuele machine kan niet worden gewijzigd:
    - Stop de virtuele machines in de beschikbaarheidsset. Klik op **resourcegroepen** > uw resourcegroep > **Resources** > uw beschikbaarheidsset > **virtuele Machines** > uw virtuele machine >  **Stop**.
    - Nadat de virtuele machines stoppen, moet u de virtuele machine maken in de gewenste grootte.
    - De nieuwe virtuele machine eerst te starten en selecteert u elk van de gestopte virtuele machines en klik op Start.


## <a name="the-cluster-does-not-have-free-resources"></a>Het cluster heeft geen gratis resources
<properties
supportTopicIds="123456789"
resourceTags="windows"
productPesIds="1234, 5678"
/>
- De aanvraag later opnieuw.
- Als de nieuwe virtuele machine deel van een andere beschikbaarheidsset uitmaken kan
    - Een virtuele machine maken in een andere beschikbaarheidsset (in dezelfde regio).
    - De nieuwe virtuele machine toevoegen aan hetzelfde virtuele netwerk.

## <a name="how-can-i-use-and-deploy-a-windows-client-image-into-azure"></a>Hoe kan ik gebruiken en de installatiekopie van een windows-client implementeren in Azure?

U kunt Windows 7, Windows 8 of Windows 10 in Azure gebruiken voor ontwikkel-/ Testscenario's als u een juiste Visual Studio (voorheen MSDN)-abonnement hebt. Dit [artikel](client-images.md) geeft een overzicht van de vereisten in aanmerking komt voor actieve Windows-client in Azure en maakt gebruik van de installatiekopieën van het Azure-galerie.

## <a name="how-can-i-deploy-a-virtual-machine-using-the-hybrid-use-benefit-hub"></a>Hoe kan ik een virtuele machine met de hybride gebruik voordeel (HUB) implementeren?

Er zijn een aantal verschillende manieren voor het implementeren van virtuele machines en de schaalvoordelen van Azure hybride gebruik van Windows.

Voor een Enterprise Agreement-abonnement:

• VM's van specifieke Marketplace-installatiekopieën die vooraf geconfigureerd met Azure hybride gebruik voordeel zijn implementeren.

Voor Enterprise-overeenkomst:

• Een aangepaste VM uploaden en implementeren met behulp van een Resource Manager-sjabloon of Azure PowerShell.

Zie de volgende bronnen voor meer informatie:

 - [Overzicht van Azure hybride gebruik Benefit](https://azure.microsoft.com/pricing/hybrid-use-benefit/)

 - [Downloadbare Veelgestelde vragen](http://download.microsoft.com/download/4/2/1/4211AC94-D607-4A45-B472-4B30EDF437DE/Windows_Server_Azure_Hybrid_Use_FAQ_EN_US.pdf)

 - [Azure hybride gebruik voordeel voor WindowsServer en Windows-Client](hybrid-use-benefit-licensing.md).

 - [Hoe kan ik het voordeel van hybride gebruiken in Azure gebruiken](https://blogs.msdn.microsoft.com/azureedu/2016/04/13/how-can-i-use-the-hybrid-use-benefit-in-azure)

## <a name="how-do-i-activate-my-monthly-credit-for-visual-studio-enterprise-bizspark"></a>Hoe kan ik mijn maandelijkse tegoed voor Visual studio Enterprise (BizSpark) activeren

Als u wilt uw maandelijkse tegoed hebt geactiveerd, ziet u dit [artikel](https://azure.microsoft.com/offers/ms-azr-0064p/).

## <a name="how-to-add-enterprise-devtest-to-my-enterprise-agreement-ea-to-get-access-to-window-client-images"></a>Het Enterprise ontwikkelen en testen voor mijn Enterprise Agreement (EA) voor toegang tot venster clientinstallatiekopieën toevoegen?

De mogelijkheid te maken op basis van de aanbieding Enterprise ontwikkelen en testen abonnementen is beperkt tot de eigenaren van de Account die gemachtigd is om dit te doen door een ondernemingsadministrator. De accounteigenaar abonnementen via de Portal van het Azure-Account maakt en vervolgens actieve Visual Studio-abonnees moet toevoegen als medebeheerders. Zodat ze kunnen beheren en gebruiken van de benodigde resources voor het ontwikkelen en testen. Zie voor meer informatie [Enterprise ontwikkelen en testen](https://azure.microsoft.com/offers/ms-azr-0148p/).

## <a name="my-drivers-are-missing-for-my-windows-n-series-vm"></a>De stuurprogramma's ontbreken voor mijn Windows-VM met N-serie

Stuurprogramma's voor Windows-VM's zich bevinden [hier](n-series-driver-setup.md).

## <a name="i-cant-find-a-gpu-instance-within-my-n-series-vm"></a>Ik kan een GPU-exemplaar niet vinden in mijn VM N-serie

Als u wilt profiteren van de GPU-mogelijkheden van N-reeks virtuele machines in Azure waarop Windows Server 2016 of Windows Server 2012 R2 wordt uitgevoerd, moet u NVIDIA grafische stuurprogramma's installeren op elke virtuele machine na de implementatie. Stuurprogramma-installatie-informatie is beschikbaar voor [VM's van Windows](n-series-driver-setup.md) en [virtuele Linux-machines](../linux/n-series-driver-setup.md).

## <a name="are-client-images-supported-for-n-series"></a>Worden de clientinstallatiekopieën van de voor N-serie ondersteund?

Azure ondersteunt momenteel alleen N-reeks op virtuele machines met Windows Server- en Linux-besturingssystemen.

## <a name="is-n-series-vms-available-in-my-region"></a>Virtuele machines N-reeks is beschikbaar in mijn regio?

U kunt controleren dat de beschikbaarheid van de [producten die per regio tabel](https://azure.microsoft.com/regions/services), en prijzen [hier](https://azure.microsoft.com/pricing/details/virtual-machines/series/#n-series).

## <a name="what-client-images-can-i-use-and-deploy-in-azure-and-how-to-i-get-them"></a>Welke clientimages kan ik gebruiken en te implementeren in Azure, en hoe naar ik ze?

U kunt Windows 7, Windows 8 of Windows 10 in Azure voor ontwikkel-/ Testscenario's geleverde dat een juiste Visual Studio (voorheen MSDN)-abonnement hebt. 

- Installatiekopieën van Windows 10 zijn beschikbaar in de galerie van Azure binnen [in aanmerking komende ontwikkelen en testen biedt](client-images.md#eligible-offers). 
- Visual Studio-abonnees binnen elk type aanbieding kunnen ook [voldoende voorbereiden en maken](prepare-for-upload-vhd-image.md) een 64-bits installatiekopie voor Windows 7, Windows 8 of Windows 10 en vervolgens [uploaden naar Azure](upload-generalized-managed.md). Het gebruik blijft beperkt tot ontwikkelen en testen door actieve Visual Studio-abonnees.

Dit [artikel](client-images.md) geeft een overzicht van de vereisten in aanmerking komt voor actieve Windows-client in Azure en het gebruik van de installatiekopieën van het Azure-galerie.

## <a name="i-am-not-able-to-see-vm-size-family-that-i-want-when-resizing-my-vm"></a>Ik wil niet kunnen zien van VM-grootte-familie die ik wil wanneer het formaat van mijn VM.

Wanneer een virtuele machine wordt uitgevoerd, wordt geïmplementeerd met een fysieke server. De fysieke servers in de Azure-regio's worden gegroepeerd in clusters van algemene fysieke hardware. Formaat van een virtuele machine die u moet de virtuele machine worden verplaatst naar andere hardware clusters verschilt, afhankelijk van welke implementatiemodel is gebruikt voor het implementeren van de virtuele machine.

- Virtuele machines die worden geïmplementeerd in het klassieke implementatiemodel, de cloud service-implementatie moeten worden verwijderd en opnieuw als u wilt wijzigen in een grootte in een ander gezin van de grootte van de virtuele machines zijn geïmplementeerd.

- Virtuele machines in de Resource Manager-implementatiemodel zijn geïmplementeerd, moet u alle virtuele machines stoppen in beschikbaarheidsset voordat u wijzigt de grootte van een virtuele machine in de beschikbaarheidsset.

## <a name="the-listed-vm-size-is-not-supported-while-deploying-in-availability-set"></a>De vermelde VM-grootte wordt niet ondersteund tijdens de implementatie in Beschikbaarheidsset.

Kies een grootte die wordt ondersteund op de beschikbaarheidsset cluster. Het wordt aanbevolen wanneer de grootste VM-grootte u denkt dat u nodig hebt en hebt die uw eerste implementatie voor de beschikbaarheidsset kiezen voor het maken van een beschikbaarheidsset ingesteld.

## <a name="can-i-add-an-existing-classic-vm-to-an-availability-set"></a>Kan ik een bestaande klassieke virtuele machine toevoegen aan een beschikbaarheidsset

Ja. U kunt een bestaande klassieke virtuele machine toevoegen aan een nieuwe of bestaande Beschikbaarheidsset. Zie voor meer informatie [een bestaande virtuele machine toevoegen aan een beschikbaarheidsset](classic/configure-availability-classic.md#addmachine).


## <a name="next-steps"></a>Volgende stappen
Als u meer hulp op elk gewenst moment in dit artikel nodig hebt, kunt u de Azure-experts raadplegen op [MSDN Azure en Stack Overflow-forums](https://azure.microsoft.com/support/forums/).

U kunt ook een incident voor ondersteuning van Azure indienen. Ga naar de [ondersteuning van Azure site](https://azure.microsoft.com/support/options/) en selecteer **ophalen ondersteunen**.
