---
title: De visualisatie afhankelijkheid in Azure migreren | Microsoft Docs
description: Biedt een overzicht van assessment berekeningen in de service Azure migreren.
services: migrate
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 78e52157-edfd-4b09-923f-f0df0880e0e0
ms.service: migrate
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 11/22/2017
ms.author: raynew
ms.openlocfilehash: e787cd6746b53c28fc1495b9a3ac62f2aa08f639
ms.sourcegitcommit: 5bced5b36f6172a3c20dbfdf311b1ad38de6176a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/27/2017
---
# <a name="dependency-visualization"></a>Visualisatie van afhankelijkheid

De [Azure migreren](migrate-overview.md) services evalueert groepen van lokale machines voor migratie naar Azure. U kunt machines groeperen afhankelijkheid visualisatie gebruiken. Dit artikel bevat informatie over deze functie.


## <a name="overview"></a>Overzicht

De visualisatie afhankelijkheid in Azure migreren kunt u groepen maken voor migratie met verbeterde betrouwbaarheid beoordelen. U kunt de afhankelijkheid van het netwerk van specifieke machines, of tussen een groep machines met behulp van afhankelijkheid visualisatie weergeven. Dit is handig om ervoor te zorgen dat er is geen functionaliteit of verloren (of machines vergeten) tijdens de migratie, wanneer toepassingen en werkbelastingen over meerdere machines worden uitgevoerd.  

## <a name="how-does-it-work"></a>Hoe werkt het?

Azure maakt gebruik van migreren de [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md) oplossing in [logboekanalyse](../log-analytics/log-analytics-overview.md) voor afhankelijkheid visualisatie.
- Wanneer u een migratie van de Azure-project maakt, wordt een logboekanalyse OMS-werkruimte gemaakt in uw abonnement.
- De Werkruimtenaam van de is de naam die u voor de migratieproject opgeeft, voorafgegaan door **migreren-**, en desgewenst voorafgegaan door een getal. 
- Navigeer naar de werkruimte voor logboekanalyse uit de **Essentials** sectie van het project **overzicht** pagina.
- De werkruimte gemaakt met de sleutel is gecodeerd **MigrateProject**, en de waarde **projectnaam**. U kunt deze gebruiken om te zoeken in de Azure portal.  

    ![Log Analytics-werkruimte](./media/concepts-dependency-visualization/oms-workspace.png)

Voor het gebruik van afhankelijkheid visualisatie die u wilt downloaden en installeren van agents op elke on-premises machine die u wilt analyseren.  

## <a name="do-i-need-to-pay-for-it"></a>Heb ik nodig voor het betalen?

Ja. De werkruimte voor logboekanalyse standaard wordt gemaakt, maar deze wordt niet gebruikt, tenzij u afhankelijkheid visualisatie in Azure migreren gebruiken. Als u afhankelijkheid visualisatie gebruiken (of gebruik de werkruimte buiten Azure migreren), wordt u in rekening gebracht voor gebruik van de werkruimte.  [Meer informatie](https://www.microsoft.com/cloud-platform/operations-management-suite) over Serviceoverzicht oplossing prijzen. 

## <a name="how-do-i-manage-the-workspace"></a>Hoe kan ik de werkruimte beheren?

U kunt de werkruimte voor logboekanalyse buiten Azure migreren. Dit wordt niet verwijderd als u verwijdert het migratieproject waarin deze is gemaakt. Als u niet langer de werkruimte [verwijderen](../log-analytics/log-analytics-manage-access.md) handmatig.

Verwijder niet de werkruimte gemaakt door Azure migreren, tenzij u het migratieproject verwijderen. Als u dit doet, werkt afhankelijkheden niet zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

[Groep machines met behulp van de machine-afhankelijkheden](how-to-create-group-machine-dependencies.md)