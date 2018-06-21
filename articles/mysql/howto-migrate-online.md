---
title: Minimale downtime migratie naar Azure-Database voor MySQL
description: Dit artikel wordt beschreven hoe u een minimale downtime migratie van een MySQL-database naar Azure-Database voor MySQL uitvoert met behulp van de Service Azure Database migratie.
services: mysql
author: HJToland3
ms.author: jtoland
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 06/21/2018
ms.openlocfilehash: ecbd35bd45bd11292bbe4a032329d704858d4c77
ms.sourcegitcommit: 1438b7549c2d9bc2ace6a0a3e460ad4206bad423
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2018
ms.locfileid: "36293918"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Minimale downtime migratie naar Azure-Database voor MySQL
U MySQL migraties naar Azure-Database kunt uitvoeren voor MySQL met minimale downtime met behulp van de nieuwe geïntroduceerd **continue synchronisatie mogelijkheid** voor de [Azure migratie databaseservice](https://aka.ms/get-dms) (DMS). Deze functionaliteit beperkt de hoeveelheid uitvaltijd die wordt veroorzaakt door de toepassing.

## <a name="overview"></a>Overzicht
DMS voert een initiële belasting van uw on-premises naar Azure Database voor MySQL en eventuele nieuwe transacties naar Azure vervolgens continu gesynchroniseerd terwijl de toepassing actief blijft. Nadat de gegevens de resultaten op de doel-Azure aan clientzijde, u stop de toepassing voor een korte ogenblik (minimale downtime), wacht u totdat de laatste batch van gegevens (vanaf het moment dat u de toepassing gestopt tot de toepassing daadwerkelijk niet beschikbaar is voor alle nieuwe verkeer) te vangen omhoog in de doel- en werk vervolgens de verbindingsreeks om te verwijzen naar Azure. Wanneer u klaar bent, is uw toepassing worden live in Azure.

![Continue synchroniseren met de Azure-Service voor het migreren van Database](./media/howto-migrate-online/ContinuousSync.png)

Migratie van MySQL bronnen DMS is momenteel in preview. Als u uitproberen van de service wilt voor het migreren van de werkbelasting van uw MySQL, zich aanmelden via de Azure-DMS [voorbeeldpagina](https://aka.ms/dms-preview) om uit te drukken, uw interesse. Uw feedback is zeer waardevol bij het verder verbeteren van de service.

## <a name="next-steps"></a>Volgende stappen
- Bekijk de video [eenvoudig migreren MySQL/PostgreSQL apps naar Azure service beheerd](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), die een demo waarin wordt getoond hoe MySQL apps migreren naar Azure-Database voor MySQL bevat.
- Aanmelden voor een beperkte preview van minimale downtime migraties van MySQL met Azure-Database voor MySQL via de Azure-DMS [voorbeeldpagina](https://aka.ms/dms-preview).
