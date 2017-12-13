---
title: Groep virtuele machines met VMware vCenter tagging | Microsoft Docs
description: Beschrijft het maken van een groep voordat u een evaluatie met de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: 967c2a1fc0e5144c6c7d5c8c4a81cec3d77ffdb5
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="group-vms-with-vcenter-tagging"></a>Groep virtuele machines met de vCenter-tagging

In dit artikel wordt beschreven hoe u een groep machines voor [Azure migreren](migrate-overview.md) assessment gebruik in VMware vCenter-tagging. U opgeven de tag-categorie die u gebruiken wilt bij het maken van een groep. 

## <a name="set-up-tagging"></a>Labels instellen

Tijdens de implementatie van Azure migreren detecteert een lokale Azure migreren VM machines op ESXi-hosts worden beheerd door een vCenter-server. U moet vCenter tagging voordat het discovery-proces instellen.

1. Ga in de VMware vSphere webclient naar de vCenter-server.
2. Klik op **labels**, om te controleren van de huidige labels.
3. Selecteer om de labels van een virtuele machine, **verwante objecten** > **virtuele Machines**, en selecteer vervolgens de virtuele machine die u labelen wilt.
4. In **samenvatting** > **labels**, klikt u op **toewijzen**. 
5. Klik op **nieuwe Tag**, en geef een naam en beschrijving.
6. Selecteer voor het verpakken van een categorie voor het label **nieuwe categorie** in de vervolgkeuzelijst.
7. Geef een categorienaam en beschrijving en de kardinaliteit. Klik vervolgens op **OK**.

    ![VM-labels](./media/how-to-tag-v-center/vm-tag.png)

## <a name="use-tagging-to-create-groups"></a>Labels gebruiken om groepen te maken

1. Detectie van lokale machines ingesteld zoals beschreven de [VMware assessment zelfstudie](tutorial-assessment-vmware.md#run-the-collector-to-discover-vms).
2. In **Tag categorie voor groepering**, selecteert u de vCenter-tag categorie waarvan de evaluatie-groep moet worden gebaseerd. Een groep voor de geselecteerde categorie wordt automatisch gemaakt door Azure migreren.

    

## <a name="next-steps"></a>Volgende stappen

[Instellen van VMware VM assessment](tutorial-assessment-vmware.md).
