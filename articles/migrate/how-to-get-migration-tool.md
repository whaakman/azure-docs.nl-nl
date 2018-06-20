---
title: Machines na beoordeling migreren met Azure migreren | Microsoft Docs
description: Hierin wordt beschreven hoe u de aanbevelingen voor het migreren van krijgen machines nadat u een evaluatie uitgevoerd met de service Azure migreren.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: article
ms.date: 06/19/2018
ms.author: raynew
ms.openlocfilehash: 571bd2424d1d38e6c0048a95b263dda000477e44
ms.sourcegitcommit: 16ddc345abd6e10a7a3714f12780958f60d339b6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/19/2018
ms.locfileid: "36221876"
---
# <a name="migrate-machines-after-assessment"></a>Machines na beoordeling migreren


[Azure migreren](migrate-overview.md) lokale machines om te controleren of ze geschikt is voor migratie naar Azure, en biedt sizing en kosten schattingen voor het uitvoeren van de machine in Azure evalueert. Op dit moment beoordeelt Azure migreren alleen machines voor de migratie. De migratie zelf wordt momenteel uitgevoerd voor het met andere Azure-services.

Dit artikel wordt beschreven hoe u suggesties voor een hulpprogramma voor migratie van na het uitvoeren van een migratie uit te voeren.

## <a name="migration-tool-suggestion"></a>Migratie hulpprogramma suggestie

Als u suggesties met betrekking tot de hulpprogramma's voor migratie, moet u doen een grondige detectie van de on-premises omgeving. Grondige detectie wordt uitgevoerd door de installatie van agents op de lokale-machines.  

1. Een Azure migreren-project maken, lokale machines detecteren en maken van een migratie uit te voeren. [Meer informatie](tutorial-assessment-vmware.md).
2. Download en installeer de Azure migreren agents op elke lokale machine waarvoor u wilt laten zien van een van de aanbevolen migratiemethode. [Volg deze procedure](how-to-create-group-machine-dependencies.md#prepare-machines-for-dependency-mapping) om de agents te installeren.
2. Identificeer uw lokale machines die geschikt voor migratie van de lift en shift zijn. Dit zijn de virtuele machines die wijzigingen in apps die worden uitgevoerd op deze niet nodig en kunnen worden gemigreerd zoals is.
3. Lift en shift migratie, is het raadzaam met Azure Site Recovery. [Meer informatie](../site-recovery/tutorial-migrate-on-premises-to-azure.md). U kunt ook de hulpprogramma's van derden die ondersteuning bieden voor migratie naar Azure.
4. Als u lokale computers die niet geschikt is voor een migratie lift en shift hebt, dat wil zeggen, kunt als u wilt migreren van specifieke app in plaats van een volledige virtuele machine, u andere hulpprogramma's voor migratie. Bijvoorbeeld, het is raadzaam de [Migratieservice voor Azure Database](https://azure.microsoft.com/campaigns/database-migration/) als u wilt migreren lokale databases, zoals een SQL Server, MySQL of Oracle naar Azure.


## <a name="review-suggested-migration-methods"></a>Voorgestelde migratiemethoden controleren

1. Voordat u een voorgestelde migratiemethode ophalen kunt, moet u een Azure migreren-project maken, het lokale machines detecteren en het uitvoeren van een migratie uit te voeren. [Meer informatie](tutorial-assessment-vmware.md).
2. Nadat de beoordeling is gemaakt, weergeven in het project > **overzicht** > **Dashboard**. Klik op **Readiness Assessment**

    ![Gereedheid evalueren](./media/tutorial-assessment-vmware/assessment-report.png)  

3. In **hulpprogramma voorgestelde**, lees de suggesties voor hulpprogramma's die u voor migratie gebruiken kunt.

    ![Voorgesteld hulpprogramma](./media/tutorial-assessment-vmware/assessment-suitability.png) 




## <a name="next-steps"></a>Volgende stappen

[Meer informatie](concepts-assessment-calculation.md) over hoe evaluaties worden berekend.
