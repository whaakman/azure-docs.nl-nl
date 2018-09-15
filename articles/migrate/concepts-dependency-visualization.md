---
title: Visualisatie van afhankelijkheden in Azure Migrate | Microsoft Docs
description: Biedt een overzicht van evaluatie van berekeningen in de Azure Migrate-service.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 09/14/2018
ms.author: raynew
ms.openlocfilehash: 5880c98b0f77b75dfb10969311408307b0c4afbd
ms.sourcegitcommit: 616e63d6258f036a2863acd96b73770e35ff54f8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/14/2018
ms.locfileid: "45603359"
---
# <a name="dependency-visualization"></a>Visualisatie van afhankelijkheden

De [Azure Migrate](migrate-overview.md) beoordeelt groepen van on-premises computers voor migratie naar Azure. Machines groeperen, kunt u afhankelijkheidsvisualisatie gebruiken. In dit artikel bevat informatie over deze functie.


## <a name="overview"></a>Overzicht

Visualisatie van afhankelijkheden in Azure Migrate kunt u groepen maken voor migratie evalueren met meer vertrouwen. U kunt de netwerkafhankelijkheden van specifieke computers, of voor een groep machines met visualisatie van afhankelijkheden weergeven. Dit is handig om ervoor te zorgen dat er is geen functionaliteit of verloren (of machines vergeten) in het migratieproces, wanneer toepassingen en workloads worden uitgevoerd op meerdere machines.  

## <a name="how-does-it-work"></a>Hoe werkt het?

Azure Migrate gebruikt de [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md) oplossing in [Log Analytics](../log-analytics/log-analytics-overview.md) voor visualisatie van afhankelijkheden.
- Wanneer u een Azure-migratieproject maakt, wordt een Log Analytics-werkruimte wordt gemaakt in uw abonnement.
- Naam van de werkruimte is de naam die u voor het migratieproject opgeeft, voorafgegaan door **migreren-**, en eventueel voorafgegaan door een getal.
- Navigeer naar de Log Analytics-werkruimte van de **Essentials** sectie van het project **overzicht** pagina.
- De werkruimte gemaakt met de sleutel is gecodeerd **migratieproject**, en de waarde **projectnaam**. U kunt deze gebruiken om te zoeken in Azure portal.  

    ![Log Analytics-werkruimte](./media/concepts-dependency-visualization/oms-workspace.png)

Voor het gebruik van visualisatie van afhankelijkheden, die u wilt downloaden en installeren van agents op elke on-premises machine die u wilt analyseren.  

## <a name="do-i-need-to-pay-for-it"></a>Moet ik betalen?

Azure Migrate is zonder extra kosten beschikbaar. Gebruik van de functies van de visualisatie afhankelijkheid in Azure Migrate, hebt servicetoewijzing nodig. Bij het maken van een Azure Migrate-project maakt Azure Migrate automatisch een nieuwe Log Analytics-werkruimte uit uw naam.

> [!NOTE]
> De functie voor visualisatie van afhankelijkheden maakt gebruik van servicetoewijzing via een Log Analytics-werkruimte. Sinds 28 februari 2018 is met de aankondiging van Azure Migrate algemene beschikbaarheid, de functie nu beschikbaar zonder extra kosten. U moet een nieuw project te maken van de werkruimte gratis gebruik gebruiken. Bestaande werkruimten die vóór de algemene availaibility zijn nog steeds chargable, daarom raden we u om te verplaatsen naar een nieuw project.

1. Gebruik van eventuele oplossingen die plaats van servicetoewijzing in deze werkruimte van Log Analytics rekening standard Log Analytics-kosten in gebracht.
2. Ter ondersteuning van scenario's voor migratie zonder extra kosten, de oplossing Serviceoverzicht, worden er kosten in rekening gebracht voor de eerste 180 dagen van het maken van het project Azure Migrate, waarna de standaardkosten zijn van.
3. Alleen de werkruimte gemaakt als onderdeel van het maken van het project, rekening gebracht voor gebruik.

Als u agents aan de werkruimte registreert, gebruiken de ID en de sleutel die is opgegeven door het project op de pagina installatie agent stappen. U kunt geen gebruik van een bestaande werkruimte en deze koppelen aan het Azure Migrate-project.

Wanneer de Azure Migrate-project wordt verwijderd, wordt de werkruimte niet samen met het verwijderd. Het verwijderen van het project posten, het gebruik van de Service Map zich niet gratis en elk knooppunt wordt in rekening gebracht volgens de prijscategorie betaald van Log Analytics-werkruimte.

Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="how-do-i-manage-the-workspace"></a>Hoe beheer ik de werkruimte?

U kunt de Log Analytics-werkruimte buiten Azure Migrate gebruiken. Het wordt niet verwijderd als het verwijderen van het migratieproject waarin deze is gemaakt. Als u de werkruimte niet meer nodig [verwijderen](../log-analytics/log-analytics-manage-access.md) handmatig.

Verwijder de werkruimte hebt gemaakt door Azure Migrate, niet, tenzij u het migratieproject verwijderen. Als u dit doet, worden de afhankelijkheden niet werkt zoals verwacht.

## <a name="next-steps"></a>Volgende stappen

[Machines groeperen met behulp van machineafhankelijkheden](how-to-create-group-machine-dependencies.md)
