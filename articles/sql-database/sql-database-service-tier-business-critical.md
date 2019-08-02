---
title: Bedrijfskritiek tier-Azure SQL Database-Service | Microsoft Docs
description: Meer informatie over de Azure SQL Database Bedrijfskritiek-laag
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: jovanpop-msft
ms.author: jovanpop
ms.reviewer: sstein
ms.date: 12/04/2018
ms.openlocfilehash: 208224e10c3acfb17bc5fd89d2d66152943811dc
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566748"
---
# <a name="business-critical-tier---azure-sql-database"></a>Bedrijfskritiek laag-Azure SQL Database

> [!NOTE]
> Bedrijfskritiek laag wordt Premium genoemd in het DTU-aankoop model. Zie [Azure SQL database-inkoop modellen en-resources](sql-database-purchase-models.md)voor een vergelijking van het op vCore gebaseerde aankoop model met het DTU-gebaseerde aankoop model.

Azure SQL Database is gebaseerd op SQL Server data base engine-architectuur die is aangepast voor de cloud omgeving om 99,99% Beschik baarheid te garanderen, zelfs in het geval van infrastructuur fouten. Er zijn drie architectuur modellen die worden gebruikt in Azure SQL Database:
- Algemeen/standaard 
- Business Critical/Premium
- Grootschalig

Premium/Bedrijfskritiek service tier model is gebaseerd op een cluster met data base-engine processen. Dit architectuur model is afhankelijk van een feit dat er altijd een quorum van de beschik bare data base-engine knooppunten is en een minimale invloed heeft op de prestaties van uw werk belasting, zelfs tijdens onderhouds activiteiten.

Azure upgradet en patches onderliggend besturings systeem, stuur Programma's en SQL Server data base-engine transparant met de minimale verwerkings tijd voor eind gebruikers. 

Premium-Beschik baarheid is ingeschakeld in Premium-en Bedrijfskritiek-service lagen van Azure SQL Database en is ontworpen voor intensieve werk belastingen die geen invloed kunnen hebben op de prestaties vanwege de continue onderhouds werkzaamheden.

In het Premium-model integreert Azure SQL database Compute en opslag op het ene knoop punt. Hoge Beschik baarheid in dit architectuur model wordt bereikt door de replicatie van Compute (SQL Server data base engine process) en opslag (lokaal gekoppelde SSD) die is geïmplementeerd in een cluster met vier knoop punten en gebruikmaakt van technologie die vergelijkbaar is met SQL Server AlwaysOn- [beschikbaarheids groepen ](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server).

![Cluster van data base-engine knooppunten](media/sql-database-managed-instance/business-critical-service-tier.png)

Zowel het proces van de SQL database-engine als de onderliggende MDF/LDF-bestanden worden geplaatst op hetzelfde knoop punt met lokaal gekoppelde SSD-opslag met een lage latentie voor uw werk belasting. Hoge Beschik baarheid wordt geïmplementeerd met technologie die vergelijkbaar is met SQL Server AlwaysOn- [beschikbaarheids groepen](https://docs.microsoft.com/sql/database-engine/availability-groups/windows/overview-of-always-on-availability-groups-sql-server). Elke Data Base is een cluster met database knooppunten met één primaire data base die toegankelijk is voor de werk belasting van de klant en een drie secundaire processen met kopieën van gegevens. Het primaire knoop punt pusht voortdurend de wijzigingen naar secundaire knoop punten om ervoor te zorgen dat de gegevens beschikbaar zijn op secundaire replica's als het primaire knoop punt om een of andere reden vastloopt. Failover wordt verwerkt door de SQL Server data base-engine – één secundaire replica wordt het primaire knoop punt en er wordt een nieuwe secundaire replica gemaakt om ervoor te zorgen dat er voldoende knoop punten in het cluster zijn. De werk belasting wordt automatisch omgeleid naar het nieuwe primaire knoop punt.

Daarnaast heeft Bedrijfskritiek cluster ingebouwde read-out voor [lezen](sql-database-read-scale-out.md) , waarmee alleen-lezen-alleen-lezen-knoop punten kunnen worden uitgevoerd die alleen worden gebruikt om query's uit te voeren die alleen worden gelezen (bijvoorbeeld rapporten) die geen invloed mogen hebben op de prestaties van uw primaire werklast.

## <a name="when-to-choose-this-service-tier"></a>Wanneer moet deze servicelaag worden gekozen?

Bedrijfskritiek servicelaag is ontworpen voor de toepassingen die antwoorden met een lage latentie vereisen van de onderliggende SSD-opslag (gemiddeld 1-2 MS), snel herstel als de onderliggende infra structuur mislukt of rapporten, analyses en alleen-lezen moet worden geladen query's naar de gratis, lees bare secundaire replica van de primaire data base.

## <a name="next-steps"></a>Volgende stappen

- Zoek bron kenmerken (aantal kernen, i/o, geheugen) van Bedrijfskritiek-laag in [beheerde instantie](sql-database-managed-instance-resource-limits.md#service-tier-characteristics), afzonderlijke data base in [VCore model](sql-database-vcore-resource-limits-single-databases.md#business-critical-service-tier-for-provisioned-compute-tier) of [DTU-model](sql-database-dtu-resource-limits-single-databases.md#premium-service-tier), of elastische pool in [vCore model](sql-database-vcore-resource-limits-elastic-pools.md#business-critical-service-tier-storage-sizes-and-compute-sizes) en [DTU-model](sql-database-dtu-resource-limits-elastic-pools.md#premium-elastic-pool-limits).
- Meer informatie over [Algemeen](sql-database-service-tier-general-purpose.md) -en [grootschalige](sql-database-service-tier-hyperscale.md) -lagen.
- Meer informatie over [service Fabric](../service-fabric/service-fabric-overview.md).
- Zie [bedrijfs continuïteit](sql-database-business-continuity.md)voor meer opties voor hoge Beschik baarheid en herstel na nood gevallen.
