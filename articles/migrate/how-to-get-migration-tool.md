---
title: Machines na beoordeling migreren met Azure Migrate | Microsoft Docs
description: Beschrijft hoe u aanbevelingen ophalen voor het migreren van machines na het uitvoeren van een evaluatie met de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 09/17/2018
ms.author: raynew
ms.openlocfilehash: 0b02ae4b75426b379ad7c124f5ddeb053c142ce6
ms.sourcegitcommit: 1b561b77aa080416b094b6f41fce5b6a4721e7d5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2018
ms.locfileid: "45730291"
---
# <a name="migrate-machines-after-assessment"></a>Machines na beoordeling migreren


[Azure Migrate](migrate-overview.md) beoordeelt on-premises computers om te controleren of ze geschikt voor migratie naar Azure, en biedt formaat- en kostenramingen voor het uitvoeren van de machine in Azure. Op dit moment beoordeelt Azure Migrate alleen computers voor migratie. De migratie zelf wordt momenteel uitgevoerd met behulp van andere Azure-services.

In dit artikel wordt beschreven hoe u suggesties voor een hulpprogramma voor migratie ophalen nadat u een evaluatie van de migratie hebt uitgevoerd.

## <a name="migration-tool-suggestion"></a>Migratie hulpprogramma suggestie

Als u suggesties met betrekking tot de hulpprogramma's voor migratie, moet u doen een grondige detectie van de on-premises omgeving. De grondige detectie wordt uitgevoerd door het installeren van agents op de on-premises computers.  

1. Maak een Azure Migrate-project, on-premises computers detecteren en maken van een migratie-evaluatie. [Meer informatie](tutorial-assessment-vmware.md).
2. Download en installeer de Azure Migrate-agents op elke on-premises computer waarvoor u wilt zien van een aanbevolen methode. [Volg deze procedure](how-to-create-group-machine-dependencies.md#prepare-for-dependency-visualization) om de agents te installeren.
2. Uw on-premises machines identificeren die geschikt voor lift-and-shift-migratie zijn. Dit zijn de virtuele machines die niet vereist geen wijzigingen in apps die daarop worden uitgevoerd en kunnen worden gemigreerd zoals is.
3. Lift-and-shift-migratie, is het raadzaam met Azure Site Recovery. [Meer informatie](../site-recovery/tutorial-migrate-on-premises-to-azure.md). U kunt ook hulpprogramma's van derden die ondersteuning bieden voor migratie naar Azure.
4. Als u on-premises machines die niet geschikt zijn voor een lift-and-shift-migratie hebt, dat wil zeggen, kunt als u wilt migreren van specifieke app in plaats van een hele virtuele machine, u andere hulpprogramma's voor migratie. Bijvoorbeeld, het is raadzaam de [Azure Database Migration service](https://azure.microsoft.com/campaigns/database-migration/) als u wilt migreren van on-premises databases die een SQL Server, MySQL of Oracle naar Azure.


## <a name="review-suggested-migration-methods"></a>Voorgestelde migratiemethoden bekijken

1. Voordat u een voorgestelde migratiemethode krijgt, moet u een Azure Migrate-project maken, on-premises computers detecteren en uitvoeren van een migratie-evaluatie. [Meer informatie](tutorial-assessment-vmware.md).
2. Nadat de evaluatie is gemaakt, weergeven in het project > **overzicht** > **Dashboard**. Klik op **gereedheid evalueren**

    ![Gereedheid evalueren](./media/tutorial-assessment-vmware/assessment-report.png)  

3. In **voorgesteld hulpprogramma**, lees de suggesties voor hulpprogramma's die u voor migratie gebruiken kunt.

    ![Voorgesteld hulpprogramma](./media/tutorial-assessment-vmware/assessment-suitability.png)




## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
