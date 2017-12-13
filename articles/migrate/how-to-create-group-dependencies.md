---
title: Een evaluatie-groep met apparaatgroeptoewijzing van afhankelijkheid in Azure migreren verfijnen | Microsoft Docs
description: Beschrijft hoe een beoordeling in de service Azure migreren met behulp van afhankelijkheid apparaatgroeptoewijzing verfijnen.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 12/12/2017
ms.author: raynew
ms.openlocfilehash: c30d6546e7c2d471d4b262a8af1ce593b2c1c3fb
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
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
