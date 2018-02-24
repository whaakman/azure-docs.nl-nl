---
title: De visualisatie afhankelijkheid in Azure migreren | Microsoft Docs
description: Biedt een overzicht van assessment berekeningen in de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 2/21/2018
ms.author: raynew
ms.openlocfilehash: 886977764517f1fec89eee77fc3263d30ff9ab31
ms.sourcegitcommit: 12fa5f8018d4f34077d5bab323ce7c919e51ce47
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/23/2018
---
# <a name="dependency-visualization"></a>Visualisatie van afhankelijkheden

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

Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/). 

## <a name="how-do-i-manage-the-workspace"></a>Hoe kan ik de werkruimte beheren?

U kunt de werkruimte voor logboekanalyse buiten Azure migreren. Dit wordt niet verwijderd als u verwijdert het migratieproject waarin deze is gemaakt. Als u niet langer de werkruimte [verwijderen](../log-analytics/log-analytics-manage-access.md) handmatig.

Verwijder niet de werkruimte gemaakt door Azure migreren, tenzij u het migratieproject verwijderen. Als u dit doet, werkt afhankelijkheden niet zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

[Groep machines met behulp van de machine-afhankelijkheden](how-to-create-group-machine-dependencies.md)