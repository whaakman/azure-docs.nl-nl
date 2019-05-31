---
title: Migratie met minimale downtime met Azure Database voor PostgreSQL - één Server
description: Dit artikel wordt beschreven hoe u een migratie met minimale downtime van een PostgreSQL-database met Azure Database voor PostgreSQL - één Server met behulp van de Azure Database Migration Service uitvoert.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 93cd390889c023adf1c30a8470e1c2298598439e
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "65067514"
---
# <a name="minimal-downtime-migration-to-azure-database-for-postgresql---single-server"></a>Migratie met minimale downtime met Azure Database voor PostgreSQL - één Server
U kunt migratie naar Azure Database for PostgreSQL voor PostgreSQL met minimale downtime uitvoeren met behulp van de onlangs geïntroduceerde **doorlopende synchronisatie mogelijkheid** voor de [Azure Database Migration Service](https://aka.ms/get-dms) (DMS) . Deze functionaliteit beperkt de hoeveelheid uitvaltijd beperkt die worden gemaakt door de toepassing.

## <a name="overview"></a>Overzicht
Azure DMS wordt uitgevoerd van een aanvankelijke belasting van uw on-premises naar Azure Database voor PostgreSQL, en vervolgens continu worden gesynchroniseerd met een nieuwe transacties naar Azure terwijl de toepassing actief blijft. Nadat de gegevens de resultaten op de doel-Azure aan clientzijde, u stop de toepassing voor een korte moment (minimale downtime) en wacht tot de laatste batch van gegevens (vanaf het moment dat u de toepassing stoppen voordat de toepassing effectief niet beschikbaar is voor alle nieuwe verkeer) om af te vangen omhoog in de doel- en werk vervolgens de verbindingsreeks om te verwijzen naar Azure. Wanneer u klaar bent, wordt uw toepassing worden live in Azure!

![Doorlopende synchronisatie met de Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Volgende stappen
- Bekijk de video [App-modernisering met Microsoft Azure](https://medius.studios.ms/Embed/Video/BRK2102?sid=BRK2102), die een demo van het PostgreSQL apps migreren naar Azure Database for PostgreSQL bevat.
- Zie de zelfstudie [PostgreSQL migreren met Azure Database for PostgreSQL online met behulp van DMS](https://docs.microsoft.com/azure/dms/tutorial-postgresql-azure-postgresql-online).
