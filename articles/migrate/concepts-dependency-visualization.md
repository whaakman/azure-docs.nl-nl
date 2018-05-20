---
title: De visualisatie afhankelijkheid in Azure migreren | Microsoft Docs
description: Biedt een overzicht van assessment berekeningen in de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 05/15/2018
ms.author: raynew
ms.openlocfilehash: 2b6b3e78436a215e64894b1102c4e109f72c896d
ms.sourcegitcommit: eb75f177fc59d90b1b667afcfe64ac51936e2638
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/16/2018
---
# <a name="dependency-visualization"></a>Visualisatie van afhankelijkheden

De [Azure migreren](migrate-overview.md) services evalueert groepen van lokale machines voor migratie naar Azure. U kunt machines groeperen afhankelijkheid visualisatie gebruiken. Dit artikel bevat informatie over deze functie.


## <a name="overview"></a>Overzicht

De visualisatie afhankelijkheid in Azure migreren kunt u groepen maken voor migratie met verbeterde betrouwbaarheid beoordelen. U kunt de afhankelijkheid van het netwerk van specifieke machines, of tussen een groep machines met behulp van afhankelijkheid visualisatie weergeven. Dit is handig om ervoor te zorgen dat er is geen functionaliteit of verloren (of machines vergeten) tijdens de migratie, wanneer toepassingen en werkbelastingen over meerdere machines worden uitgevoerd.  

## <a name="how-does-it-work"></a>Hoe werkt het?

Azure maakt gebruik van migreren de [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md) oplossing in [logboekanalyse](../log-analytics/log-analytics-overview.md) voor afhankelijkheid visualisatie.
- Wanneer u een migratie van de Azure-project maakt, wordt een werkruimte voor logboekanalyse gemaakt in uw abonnement.
- De Werkruimtenaam van de is de naam die u voor de migratieproject opgeeft, voorafgegaan door **migreren-**, en desgewenst voorafgegaan door een getal. 
- Navigeer naar de werkruimte voor logboekanalyse uit de **Essentials** sectie van het project **overzicht** pagina.
- De werkruimte gemaakt met de sleutel is gecodeerd **MigrateProject**, en de waarde **projectnaam**. U kunt deze gebruiken om te zoeken in de Azure portal.  

    ![Log Analytics-werkruimte](./media/concepts-dependency-visualization/oms-workspace.png)

Voor het gebruik van afhankelijkheid visualisatie die u wilt downloaden en installeren van agents op elke on-premises machine die u wilt analyseren.  

## <a name="do-i-need-to-pay-for-it"></a>Heb ik nodig voor het betalen?

Azure Migrate is zonder extra kosten beschikbaar. Gebruik van de functies van de visualisatie afhankelijkheid in Azure migreren vereist Serviceoverzicht. Bij het maken van een project voor het migreren van Azure creëert Azure migreren automatisch een nieuwe werkruimte voor logboekanalyse namens jou.

> [!NOTE]
> De afhankelijkheid visualisatie-functie gebruikt Serviceoverzicht via een werkruimte voor logboekanalyse. Sinds 28 februari 2018 is met de aankondiging van Azure migreren algemeen beschikbaar is, de functie nu beschikbaar zijn zonder extra kosten. U moet een nieuw project te maken van de werkruimte gratis gebruik gebruiken. Bestaande werkruimten voordat algemene availaibility zijn nog steeds chargable, daarom wordt aanbevolen om te verplaatsen naar een nieuw project.

1. Gebruik van elke oplossing dan Serviceoverzicht binnen deze werkruimte voor logboekanalyse zal worden standaard logboekanalyse kosten in rekening. 
2. Ter ondersteuning van migratiescenario's zonder extra kosten, zal de oplossing Serviceoverzicht niet worden geen kosten berekend voor de eerste 180 dagen na het maken van het project Azure migreren, waarna u standaard kosten gelden.
3. Alleen de werkruimte gemaakt als onderdeel van het project maken, zijn beschikbaar voor gebruik.

Wanneer u agents naar de werkruimte registreert, gebruiken de ID en de sleutel komt overeen met het project op de pagina installatie agent stappen. U kunt geen gebruik van een bestaande werkruimte of koppelen aan het project Azure migreren.

Wanneer het project voor het migreren van Azure wordt verwijderd, wordt de werkruimte niet samen met het verwijderd. Het verwijderen van het project posten, het gebruik van het Serviceoverzicht niet worden vrijgegeven en elk knooppunt wordt in rekening gebracht volgens de betaalde laag van de werkruimte voor logboekanalyse.

Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/). 

## <a name="how-do-i-manage-the-workspace"></a>Hoe kan ik de werkruimte beheren?

U kunt de werkruimte voor logboekanalyse buiten Azure migreren. Dit wordt niet verwijderd als u verwijdert het migratieproject waarin deze is gemaakt. Als u niet langer de werkruimte [verwijderen](../log-analytics/log-analytics-manage-access.md) handmatig.

Verwijder niet de werkruimte gemaakt door Azure migreren, tenzij u het migratieproject verwijderen. Als u dit doet, werkt afhankelijkheden niet zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

[Groep machines met behulp van de machine-afhankelijkheden](how-to-create-group-machine-dependencies.md)