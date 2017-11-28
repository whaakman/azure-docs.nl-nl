---
title: Een evaluatie-groep met apparaatgroeptoewijzing van afhankelijkheid in Azure migreren verfijnen | Microsoft Docs
description: Beschrijft hoe een beoordeling in de service Azure migreren met behulp van afhankelijkheid apparaatgroeptoewijzing verfijnen.
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 0527e34e-a078-405e-aeb9-c91a5808112a
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/21/2017
ms.author: raynew
ms.openlocfilehash: b4d6861f147fbb6e65a9d529f17f78b54075eb90
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="refine-a-group-using-group-dependency-mapping"></a>Een groep met apparaatgroeptoewijzing afhankelijkheid verfijnen

Dit artikel wordt beschreven voor het instellen van de toewijzing van de groep afhankelijkheid voor [Azure migreren](migrate-overview.md) assessment. Gebruik deze methode doorgaans in als u instellingen voor een bestaande groep verfijnen door groepsafhankelijkheden tussen verschillende controleren wilt voordat u een evaluatie uitgevoerd. Groepen waarvoor u wilt uitvoeren van afhankelijkheid apparaatgroeptoewijzing mag niet meer dan 10 machines bevatten.  

## <a name="modify-a-group"></a>Een groep wijzigen

1. Migreren in de Azure-project, onder **beheren**, klikt u op **groepen**, en selecteer de groep.
2. Klik op de pagina groep **afhankelijkheden weergeven**, om te openen van de groep afhankelijkheid-kaart. 

     ![Groep bekijken](./media/how-to-create-group-dependencies/create-group.png)

3. Als u wilt meer gedetailleerd afhankelijkheden weergeven, klikt u op het tijdsbereik om dit te wijzigen. Het bereik is standaard een uur. U kunt het tijdsbereik wijzigen of geef de begin- en einddatum en duur.
4. Gebruik Ctrl + klikken om machines op de kaart. Toevoegen of verwijderen van computers van de kaart.
    - U kunt alleen machines die zijn gedetecteerd toevoegen.
    - Ongeldig voorbij beoordelingen voor het toevoegen en verwijderen van computers uit een groep maken.
    - U kunt eventueel ook een nieuwe beoordeling maken wanneer u de groep wijzigt.
5. Klik op **OK** om op te slaan van de groep.

    ![Toevoegen en verwijderen](./media/how-to-create-group-dependencies/add-remove.png)

Als u wilt controleren van de afhankelijkheden van een specifieke machine die wordt weergegeven in de groep afhankelijkheidskaart [machine afhankelijkheid toewijzing instellen](how-to-create-group-machine-dependencies.md).


## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe beoordelingen zijn berekend.
