---
title: Visualisatie van afhankelijkheden in Azure Migrate | Microsoft Docs
description: Biedt een overzicht van evaluatie van berekeningen in de Azure Migrate-service.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 11/28/2018
ms.author: raynew
ms.openlocfilehash: 9cbb1f080b9a5a98984ad48854bccc4fb2044c16
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52998341"
---
# <a name="dependency-visualization"></a>Visualisatie van afhankelijkheden

De [Azure Migrate](migrate-overview.md) beoordeelt groepen van on-premises computers voor migratie naar Azure. U kunt de functie voor visualisatie van afhankelijkheden in Azure Migrate gebruiken om groepen te maken. In dit artikel bevat informatie over deze functie.

## <a name="overview"></a>Overzicht

Visualisatie van afhankelijkheden in Azure Migrate kunt u het maken van groepen van hoge betrouwbaarheid voor beoordelingen van de migratie. Met behulp van de visualisatie van afhankelijkheden kunt u weergeven van netwerkafhankelijkheden van machines en gerelateerde machines identificeren die moeten samen worden gemigreerd naar Azure. Deze functionaliteit is handig in scenario's waarbij u bent niet volledig op de hoogte van de machines die deel uitmaken van uw toepassing en moeten samen worden gemigreerd naar Azure.

## <a name="how-does-it-work"></a>Hoe werkt het?

Azure Migrate gebruikt de [Serviceoverzicht](../operations-management-suite/operations-management-suite-service-map.md) oplossing in [Log Analytics](../log-analytics/log-analytics-overview.md) voor visualisatie van afhankelijkheden.
- Als u wilt gebruikmaken van de visualisatie van afhankelijkheden, moet u een Log Analytics-werkruimte, nieuwe of bestaande koppelen met een Azure Migrate-project.
- U kunt alleen maken of koppelen van een werkruimte in hetzelfde abonnement waar het migratieproject is gemaakt.
- Als u wilt een Log Analytics-werkruimte koppelen aan een project, gaat u naar **Essentials** sectie van het project **overzicht** pagina en klik op **configuratie is vereist**

    ![Log Analytics-werkruimte koppelen](./media/concepts-dependency-visualization/associate-workspace.png)

- Tijdens het koppelen van een werkruimte, ontvangt u de optie voor een nieuwe werkruimte maken of een bestaande werkruimte koppelen:
      - Wanneer u een nieuwe werkruimte maakt, moet u een naam opgeven voor de werkruimte. De werkruimte wordt vervolgens gemaakt in een regio in dezelfde [Azure-Geografie](https://azure.microsoft.com/global-infrastructure/geographies/) als het migratieproject.
      - Wanneer u een bestaande werkruimte koppelen, kunt u kiezen uit alle werkruimten die beschikbaar is in hetzelfde abonnement als de migration-project. Houd er rekening mee dat alleen deze werkruimten worden weergegeven die zijn gemaakt in een regio waar [Serviceoverzicht wordt ondersteund](https://docs.microsoft.com/azure/azure-monitor/insights/service-map-configure#supported-azure-regions). Als u een werkruimte te koppelen, zorg ervoor dat u 'Lezer' toegang tot de werkruimte hebben.

  > [!NOTE]
  > Nadat u hebt een werkruimte gekoppeld aan een project, kunt u deze later niet meer wijzigen.

- De gekoppelde werkruimte met de sleutel is gecodeerd **migratieproject**, en de waarde **projectnaam**, die u kunt gebruiken om te zoeken in Azure portal.
- Ga naar de werkruimte die is gekoppeld aan het project, gaat u naar **Essentials** sectie van het project **overzicht** pagina en toegang tot de werkruimte

    ![Log Analytics-werkruimte navigeren](./media/concepts-dependency-visualization/oms-workspace.png)

Voor het gebruik van visualisatie van afhankelijkheden, die u wilt downloaden en installeren van agents op elke on-premises machine die u wilt analyseren.  

- [Microsoft Monitoring agent(MMA)](https://docs.microsoft.com/azure/log-analytics/log-analytics-agent-windows) moet worden geïnstalleerd op elke computer.
- De [agent voor afhankelijkheden](https://docs.microsoft.com/azure/monitoring/monitoring-service-map-configure) moet worden geïnstalleerd op elke computer.
- Bovendien, als u computers geen verbinding met internet hebt, moet u om te downloaden en Log Analytics-gateway installeren op deze.

U hoeft niet deze agents op computers die u beoordelen wilt, tenzij u visualisatie van afhankelijkheden.

## <a name="do-i-need-to-pay-for-it"></a>Moet ik betalen?

Azure Migrate is zonder extra kosten beschikbaar. Gebruik van de functie voor visualisatie van afhankelijkheden in Azure Migrate Serviceoverzicht vereist en moet u een Log Analytics-werkruimte, nieuwe of bestaande koppelen met de Azure Migrate-project. De functie voor visualisatie van afhankelijkheden in Azure Migrate is gratis voor de eerste 180 dagen in Azure Migrate.

1. Gebruik van eventuele oplossingen die plaats van servicetoewijzing in deze Log Analytics-werkruimte wordt in rekening gebracht [standard Log Analytics](https://azure.microsoft.com/pricing/details/log-analytics/) kosten in rekening gebracht.
2. Ter ondersteuning van scenario's voor migratie zonder extra kosten, de oplossing Serviceoverzicht, worden er kosten in rekening gebracht voor de eerste 180 dagen van de dag van de Log Analytics-werkruimte koppelen aan het Azure Migrate-project. Na 180 dagen geldt de standaardkosten van Log Analytics.

Als u agents aan de werkruimte registreert, gebruiken de ID en de sleutel die is opgegeven door het project op de pagina installatie agent stappen.

Wanneer de Azure Migrate-project wordt verwijderd, wordt de werkruimte niet samen met het verwijderd. Het verwijderen van het project te plaatsen, het gebruik van de Service Map zich niet gratis en elk knooppunt wordt in rekening gebracht volgens de prijscategorie betaald van Log Analytics-werkruimte.

> [!NOTE]
> De functie voor visualisatie van afhankelijkheden maakt gebruik van servicetoewijzing via een Log Analytics-werkruimte. Sinds 28 februari 2018 is met de aankondiging van Azure Migrate algemene beschikbaarheid, de functie nu beschikbaar zonder extra kosten. U moet een nieuw project te maken van de werkruimte gratis gebruik gebruiken. Bestaande werkruimten die vóór de algemene beschikbaarheid worden nog steeds toerekenbare, daarom raden we u om te verplaatsen naar een nieuw project.

Meer informatie over prijzen voor Azure Migrate vindt u [hier](https://azure.microsoft.com/pricing/details/azure-migrate/).

## <a name="how-do-i-manage-the-workspace"></a>Hoe beheer ik de werkruimte?

U kunt de Log Analytics-werkruimte buiten Azure Migrate gebruiken. Het wordt niet verwijderd als het verwijderen van het migratieproject waarin deze is gemaakt. Als u de werkruimte niet meer nodig [verwijderen](../azure-monitor/platform/manage-access.md) handmatig.

Verwijder de werkruimte hebt gemaakt door Azure Migrate, niet, tenzij u het migratieproject verwijderen. Als u dit doet, werkt de functie voor visualisatie van afhankelijkheden niet zoals verwacht.

## <a name="next-steps"></a>Volgende stappen
- [Machines groeperen met behulp van machineafhankelijkheden](how-to-create-group-machine-dependencies.md)
- [Meer informatie](https://docs.microsoft.com/azure/migrate/resources-faq#dependency-visualization) over de veelgestelde vragen over de visualisatie van afhankelijkheden.
