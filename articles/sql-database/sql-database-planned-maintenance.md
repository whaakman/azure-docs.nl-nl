---
title: Planning voor de Azure-onderhoud gebeurtenissen - Azure SQL Database | Microsoft Docs
description: Informatie over het voorbereiden van gepland onderhoudsgebeurtenissen naar uw Azure SQL-Database.
services: sql-database
ms.service: sql-database
ms.subservice: operations
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: aamalvea
ms.author: aamalvea
ms.reviewer: carlrab
manager: craigg
ms.date: 01/30/2019
ms.openlocfilehash: 928338a911efae051df7164239dbd19f9317338a
ms.sourcegitcommit: 359b0b75470ca110d27d641433c197398ec1db38
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55824109"
---
# <a name="planning-for-azure-maintenance-events-in-azure-sql-database"></a>Planning voor de Azure-onderhoud-gebeurtenissen in Azure SQL Database

Informatie over het voorbereiden van gepland onderhoudsgebeurtenissen in uw Azure SQL-database.

## <a name="what-is-a-planned-maintenance-event"></a>Wat is een geplande onderhoudsgebeurtenis

Voor elke database houdt Azure SQL DB een quorum van waar een van de replica de primaire is databasereplica. Ten allen tijde een primaire replica moet zich online onderhoud en ten minste één secundaire replica moet zich in orde. Tijdens gepland onderhoud gaat de leden van het quorum van de database offline één bewerking tegelijk met de bedoeling dat er een primaire replica en ten minste één secundaire replica online is om te controleren of er geen uitvaltijd van de client reageert. Als de primaire replica offline worden gezet moet, een nieuwe configuratie en/of failoverproces vindt plaats één secundaire replica waarin de nieuwe primaire worden.  

## <a name="what-to-expect-during-a-planned-maintenance-event"></a>Wat u kunt verwachten tijdens gepland onderhoud

Wanneer/failovers algemeen binnen 30 seconden worden voltooid – het gemiddelde is 8 seconden. Als al verbonden, uw toepassing moet opnieuw verbinding maken met het exemplaar in orde nieuwe primaire replica van de database. Als u een nieuwe verbinding wordt uitgevoerd terwijl de database wordt momenteel uitgevoerd opnieuw configureren voordat de nieuwe primaire replica online is, krijgt u fout 40613 (Database niet beschikbaar): "Database {databasename} op server {servername} is momenteel niet beschikbaar. De verbinding later opnieuw probeer het opnieuw. ". Als uw database een langlopende query heeft, wordt deze query worden onderbroken tijdens het opnieuw configureren en moet opnieuw worden gestart.

## <a name="retry-logic"></a>Logica voor opnieuw proberen

Productie met elke clienttoepassing die verbinding met een cloudservice voor de database maakt moet een robuuste verbinding implementeren [logica voor opnieuw proberen](sql-database-connectivity-issues.md#retry-logic-for-transient-errors). Dit helpt deze situaties te voorkomen en over het algemeen moet de fouten transparant voor de eindgebruiker.

## <a name="frequency"></a>Frequentie

Gemiddeld versie 1.7 gepland onderhoud gebeurtenissen per maand.

## <a name="resource-health"></a>Status van resources

Als uw SQL-database mislukte aanmeldpogingen bij ondervindt, controleert u de [Resource Health](../service-health/resource-health-overview.md#getting-started) venster in de [Azure-portal](https://portal.azure.com) voor de huidige status. De sectie Statusgeschiedenis bevat de reden van de downtime voor elke gebeurtenis (indien beschikbaar).


## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [Resource Health](sql-database-resource-health.md) voor SQL-Database
- Zie voor meer informatie over de logica voor opnieuw proberen, [Pogingslogica voor tijdelijke problemen](sql-database-connectivity-issues.md#retry-logic-for-transient-errors)
