---
title: Een virtuele Windows-machine maken vanaf een gespecialiseerde VHD in Azure portal | Microsoft Docs
description: Maak een nieuwe Windows-VM vanaf een VHD in de Azure-portal.
services: virtual-machines-windows
documentationcenter: ''
author: cynthn
manager: jeconnoc
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 09/20/2018
ms.author: cynthn
ms.openlocfilehash: 905f00842c5ce74f681a6c5c09ff8bf6c7a9e162
ms.sourcegitcommit: 4047b262cf2a1441a7ae82f8ac7a80ec148c40c4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2018
ms.locfileid: "49091246"
---
# <a name="create-a-vm-from-a-vhd-by-using-the-azure-portal"></a>Een virtuele machine van een VHD maken met behulp van Azure portal

Er zijn verschillende manieren om u te maken van een virtuele machine (VM) in Azure: 

- Als u al een virtuele harde schijf (VHD) te gebruiken of als u kopiëren van de VHD van een bestaande virtuele machine wilt te gebruiken, kunt u een nieuwe virtuele machine door *koppelen* de VHD met de nieuwe virtuele machine als een besturingssysteemschijf. 

- U kunt een nieuwe virtuele machine maken van de VHD van een virtuele machine die is verwijderd. Bijvoorbeeld, als u een Azure-VM die niet goed werkt hebt, kunt u verwijderen van de virtuele machine en bijbehorende VHD gebruiken voor het maken van een nieuwe virtuele machine. U kunt dezelfde VHD gebruiken of een kopie van de VHD maken met het maken van een momentopname en vervolgens een nieuwe beheerde schijf maken op basis van de momentopname. Hoewel het maken van een momentopname van een enkele extra stappen nodig, blijven behouden de oorspronkelijke VHD en biedt u een terugval.
 
- U kunt een Azure-VM maken van een on-premises-VHD door de on-premises VHD uploaden en deze te koppelen aan een nieuwe virtuele machine. U PowerShell of een ander hulpprogramma gebruiken voor het uploaden van de VHD naar een opslagaccount en vervolgens maakt u een beheerde schijf van de VHD. Zie voor meer informatie, [een gespecialiseerde VHD uploaden](create-vm-specialized.md#option-2-upload-a-specialized-vhd). 

Gebruik een gespecialiseerde schijf niet als u wilt maken van meerdere virtuele machines. In plaats daarvan voor grotere implementaties [maken van een installatiekopie](capture-image-resource.md) en vervolgens [die installatiekopie gebruiken om te maken van meerdere virtuele machines](create-vm-generalized-managed.md).


## <a name="copy-a-disk"></a>Een diskette kopiëren

Een momentopname maken en maak vervolgens een schijf van de momentopname. Deze strategie kunt u de oorspronkelijke VHD als alternatieve methode houden:

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u in het menu links **alle services**.
2. In de **alle services** het zoekvak, typ **schijven** en selecteer vervolgens **schijven** om de lijst met beschikbare schijven weer te geven.
3. Selecteer de schijf die u wilt gebruiken. De **schijf** pagina voor de schijf wordt weergegeven.
4. Selecteer in het menu aan de bovenkant **maken momentopname**. 
5. Voer een **naam** voor de momentopname.
6. Kies een **resourcegroep** voor de momentopname. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken.
7. Voor **accounttype**, kiest u **standaard (HDD)** of **Premium (SSD)** opslag.
8. Wanneer u klaar bent, selecteert u **maken** om de momentopname te maken.
9. Nadat de momentopname is gemaakt, selecteert u **een resource maken** in het menu links.
10. Voer in het zoekvak **beheerde schijf** en selecteer vervolgens **Managed Disks** in de lijst.
11. Op de **Managed Disks** weergeeft, schakelt **maken**.
12. Voer een **naam** voor de schijf.
13. Kies een **resourcegroep** voor de schijf. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken. Deze selectie wordt ook gebruikt als de resourcegroep waarin u de virtuele machine maken van de schijf.
14. Voor **accounttype**, kiest u **standaard (HDD)** of **Premium (SSD)** opslag.
15. In **gegevensbrontype**, zorg ervoor dat **momentopname** is geselecteerd.
16. In de **bronmomentopname** vervolgkeuzelijst, selecteert u de momentopname die u wilt gebruiken.
17. Breng eventuele wijzigingen aan en selecteer vervolgens **maken** om de schijf te maken.

## <a name="create-a-vm-from-a-disk"></a>Een virtuele machine maken van een schijf

Nadat u de beheerde schijf VHD die u wilt gebruiken hebt, kunt u de virtuele machine maken in de portal:

1. Uit de [Azure-portal](https://portal.azure.com), selecteert u in het menu links **alle services**.
2. In de **alle services** het zoekvak, typ **schijven** en selecteer vervolgens **schijven** om de lijst met beschikbare schijven weer te geven.
3. Selecteer de schijf die u wilt gebruiken. De **schijf** pagina voor de schijf wordt geopend.
4. In de **overzicht** pagina, zorg ervoor dat **SCHIJFSTATUS** wordt vermeld als **gekoppeld**. Als dat niet, moet u mogelijk op de schijf loskoppelen van de virtuele machine of de virtuele machine om vrij te maken van de schijf verwijderen.
4. Selecteer in het menu aan de bovenkant van de pagina **maken VM**.
5. Op de **basisbeginselen** pagina voor de nieuwe virtuele machine, voer een **virtuele-machinenaam** en selecteer een bestaande **resourcegroep** of maak een nieuwe.
6. Voor **grootte**, selecteer **grootte wijzigen** toegang krijgen tot de **grootte** pagina.
7. Selecteer een rij van de grootte van virtuele machine en kies vervolgens **Selecteer**.
8. Op de **netwerken** pagina, u kunt ofwel de portal voor alle nieuwe resources maken of kunt u een bestaande **virtueel netwerk** en **netwerkbeveiligingsgroep**. De portal maakt altijd een nieuwe netwerkinterface en openbare IP-adres voor de nieuwe virtuele machine. 
9. Op de **Management** pagina, wijzigingen aanbrengen in de controle-opties.
10. Op de **Gast config** pagina, alle extensies toevoegen, indien nodig.
11. Wanneer u klaar bent, selecteert u **revisie + maken**. 
12. Als de VM-configuratie is gevalideerd, selecteert u **maken** implementatie te starten.

## <a name="next-steps"></a>Volgende stappen

U kunt ook PowerShell gebruiken om te gebruiken [een VHD uploaden naar Azure en het maken van een gespecialiseerde VM](create-vm-specialized.md).


