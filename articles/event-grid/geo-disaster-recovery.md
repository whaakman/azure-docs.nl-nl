---
title: Geo-noodherstel in Azure Event Grid | Microsoft Docs
description: Hierin wordt beschreven hoe Azure Event Grid geo-noodherstel (GeoDR) automatisch ondersteunt.
services: event-grid
author: spelluru
ms.service: event-grid
ms.topic: conceptual
ms.date: 05/24/2019
ms.author: spelluru
ms.openlocfilehash: 5b5c973a8daa8776efb0909092c569ea46902265
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "66307315"
---
# <a name="server-side-geo-disaster-recovery-in-azure-event-grid"></a>Serverzijde geo-noodherstel in Azure Event Grid
Event Grid heeft nu een automatische geo-noodherstel (GeoDR) van de metagegevens niet alleen voor nieuwe, maar alle bestaande domeinen, onderwerpen en gebeurtenisabonnementen. Als een hele Azure-regio uitvalt, heeft Event Grid al alle van de gebeurtenis met betrekking tot infrastructuur metagegevens die zijn gesynchroniseerd met een gekoppelde regio. Uw nieuwe gebeurtenissen begint aan de stroom opnieuw uit met zonder tussenkomst van de door u. 

Herstel na noodgevallen wordt gemeten met twee metrische gegevens:

- [Recovery Point Objective (RPO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_Point_Objective): de minuten of uren van de gegevens die verloren gaan.
- [Recovery Time Objective (RTO)](https://en.wikipedia.org/wiki/Disaster_recovery#Recovery_time_objective): de minuten van de service is mogelijk uur omlaag.

Automatische failover van Event Grid heeft verschillende RPO's en RTO's voor uw metagegevens (gebeurtenisabonnementen enzovoort) en gegevens (gebeurtenissen). Als u verschillende specificatie van de volgende query nodig hebt, kunt u nog steeds implementeren uw eigen [clientzijde failover met behulp van het onderwerp de gezondheid van API's](custom-disaster-recovery.md).

## <a name="recovery-point-objective-rpo"></a>Recovery Point Objective (RPO)
- **Metadata RPO**: nul minuten. Telkens wanneer een resource wordt gemaakt in Event Grid, wordt deze onmiddellijk gerepliceerd tussen regio's. Wanneer er een failover optreedt, is er geen metagegevens verloren gaan.
- **Data RPO**: Als de status van uw systeem goed is en bestaand verkeer op het moment van de regionale failover is opgevangen, is de RPO voor gebeurtenissen circa 5 minuten.

## <a name="recovery-time-objective-rto"></a>Beoogde hersteltijd (RTO)
- **Metadata RTO**: Hoewel in het algemeen het veel sneller, binnen 60 minuten gebeurt begint Event Grid te accepteert de aanroepen maken/bijwerken/verwijderen van onderwerpen en abonnementen.
- **Data RTO**: Metagegevens, zoals deze in het algemeen gebeurt veel sneller, maar binnen 60 minuten Event Grid zullen nieuwe verkeer accepteren na een regionale failover.

> [!NOTE]
> De kosten voor metagegevens GeoDR op Event Grid: $0.


## <a name="next-steps"></a>Volgende stappen
Als u wilt u eigen client-side om failoverlogica te implementeren, Zie [# bouw uw eigen herstel na noodgevallen voor aangepaste in Event Grid-onderwerpen](custom-disaster-recovery.md)
