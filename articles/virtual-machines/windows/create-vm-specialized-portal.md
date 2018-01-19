---
title: Een Windows-virtuele machine maken vanaf een speciale VHD in de Azure portal | Microsoft Docs
description: Maak een nieuwe Windows VM vanaf een VHD in de Azure portal.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: cynthn
ms.openlocfilehash: 33e94777047ea8a116ae6f393439521356a53509
ms.sourcegitcommit: 828cd4b47fbd7d7d620fbb93a592559256f9d234
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="create-a-vm-from-a-vhd-using-the-azure-portal"></a>Een virtuele machine maken vanaf een VHD met de Azure portal


Er zijn verschillende manieren om te maken van virtuele machines in Azure. Als u al een VHD te gebruiken of wilt kopiëren van de VHD van en de bestaande VM te gebruiken, kunt u een nieuwe virtuele machine maken door het koppelen van de VHD als de besturingssysteemschijf. Dit proces *koppelt* de VHD naar een nieuwe virtuele machine als de besturingssysteemschijf.

U kunt ook een nieuwe virtuele machine maken van de VHD van een virtuele machine die is verwijderd. Bijvoorbeeld, als u een Azure-virtuele machine die niet goed werkt hebt, kunt u verwijderen van de virtuele machine en de VHD gebruiken voor het maken van een nieuwe virtuele machine. U kunt de dezelfde VHD gebruiken of kopie van de VHD maken door het maken van een momentopname en vervolgens een nieuwe beheerde schijf bij het maken van de momentopname. Dit duurt een paar stappen meer, maar zorgt ervoor dat u de oorspronkelijke VHD kunt houden en kunt u ook een momentopname terugvallen op indien nodig.

U hebt een lokale virtuele machine die u gebruiken wilt voor het maken van een virtuele machine in Azure. U kunt de VHD te uploaden en koppel deze aan een nieuwe virtuele machine. Als u wilt uploaden een VHD, moet u PowerShell of een ander hulpprogramma gebruiken om te uploaden naar een opslagaccount en maak vervolgens een beheerde schijf van de VHD. Zie voor meer informatie [een gespecialiseerde VHD uploaden](create-vm-specialized.md#option-2-upload-a-specialized-vhd)

Als u gebruiken van een virtuele machine of de VHD wilt te maken van meerdere virtuele machines, kunt u deze methode niet gebruiken. Voor grotere implementaties, moet u [een installatiekopie maken](capture-image-resource.md) en vervolgens [die installatiekopie gebruiken voor het maken van meerdere virtuele machines](create-vm-generalized-managed.md).


## <a name="copy-a-disk"></a>Een schijf kopiëren

Een momentopname maken en vervolgens maakt u een schijf van de momentopname. Hiermee kunt u de oorspronkelijke VHD bewaren als een vallen terug.

1. Klik in het menu links op **alle resources**.
2. In de **alle typen** vervolgkeuzelijst, schakelt u **Alles selecteren** en schuif naar beneden en selecteer **schijven** beschikbare schijven vinden.
3. Klik op de schijf die u wilt gebruiken. De **overzicht** pagina voor de schijf wordt geopend.
4. Klik op de pagina overzicht in het menu aan de bovenkant op **+ maken momentopname**. 
5. Typ een naam voor de momentopname.
6. Kies een **resourcegroep** voor de momentopname. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken.
7. Kies of u (HDD) standard of Premium (SDD) opslag te gebruiken.
8. Wanneer u klaar bent, klikt u op **maken** om de momentopname te maken.
9. Nadat de momentopname is gemaakt, klik op **+ maken van een resource** in het menu links.
10. Typ in de zoekbalk **beheerde schijven** en selecteer **schijven beheerd** uit de lijst.
11. Op de **schijven beheerd** pagina, klikt u op **maken**.
12. Typ een naam voor de schijf.
13. Kies een **resourcegroep** voor de schijf. U kunt een bestaande resourcegroep gebruiken of een nieuwe maken. Hiermee worden ook de resourcegroep waarin u de virtuele machine maken van de schijf.
14. Kies of u (HDD) standard of Premium (SDD) opslag te gebruiken.
15. In **gegevensbrontype**, zorg ervoor dat **momentopname** is geselecteerd.
16. In de **bron momentopname** vervolgkeuzelijst, selecteert u de momentopname die u wilt gebruiken.
17. Breng eventuele wijzigingen aan en klik vervolgens op **maken** om de schijf te maken.

## <a name="create-a-vm-from-a-disk"></a>Een virtuele machine maken van een schijf

Zodra u de beheerde schijf VHD die u wilt gebruiken hebt, kunt u de virtuele machine maken in de portal.

1. Klik in het menu links op **alle resources**.
2. In de **alle typen** vervolgkeuzelijst, schakelt u **Alles selecteren** en schuif naar beneden en selecteer **schijven** beschikbare schijven vinden.
3. Klik op de schijf die u wilt gebruiken. De **overzicht** pagina voor de schijf wordt geopend.
Controleer op de pagina overzicht **status schijf** wordt vermeld als **ontkoppelde**. Dit niet het geval is, moet u mogelijk ofwel Ontkoppel de schijf van de virtuele machine of verwijderen van de virtuele machine te maken van de schijf vrij.
4. Klik in het menu aan de bovenkant van het deelvenster op **+ maken VM**.
5. Op de **basisbeginselen** pagina voor de nieuwe virtuele machine, typ een naam en selecteer een bestaande resourcegroep of maak een nieuwe.
6. Op de **grootte** pagina, selecteert u een VM-grootte pagina en klik vervolgens op **Selecteer**.
7. Op de **instellingen** pagina kunt u ofwel gebruiken om de portal voor alle nieuwe resources maken of kunt u een bestaande **virtueel netwerk** en **netwerkbeveiligingsgroep**. De portal maken altijd een nieuwe NIC en het openbare IP-adres voor de nieuwe virtuele machine. 
8. Breng wijzigingen in de controle-opties en voeg eventuele uitbreidingen indien nodig.
9. Klik als u klaar bent op **OK**. 
10. Als de VM-configuratie is geslaagd, klikt u op **OK** implementatie te starten.

## <a name="next-steps"></a>Volgende stappen

U kunt ook PowerShell om te gebruiken [een VHD uploaden naar Azure en een speciale virtuele machine maken](create-vm-specialized.md).


