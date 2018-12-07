---
title: Migratie met minimale downtime met Azure Database for MySQL
description: In dit artikel wordt beschreven hoe u een migratie met minimale downtime van een MySQL-database met Azure Database voor MySQL met behulp van de Azure Database Migration Service uitvoeren.
services: mysql
author: ajlam
ms.author: andrela
manager: kfile
editor: jasonwhowell
ms.service: mysql
ms.topic: article
ms.date: 12/07/2018
ms.openlocfilehash: 267212d8f832b96bf66145b97c3464471e28593d
ms.sourcegitcommit: 698ba3e88adc357b8bd6178a7b2b1121cb8da797
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/07/2018
ms.locfileid: "53014188"
---
# <a name="minimal-downtime-migration-to-azure-database-for-mysql"></a>Migratie met minimale downtime met Azure Database for MySQL
U kunt MySQL-migratie naar Azure Database for MySQL met minimale downtime uitvoeren met behulp van de onlangs geïntroduceerde **doorlopende synchronisatie mogelijkheid** voor de [Azure Database Migration Service](https://aka.ms/get-dms) (DMS). Deze functionaliteit beperkt de hoeveelheid uitvaltijd beperkt die worden gemaakt door de toepassing.

## <a name="overview"></a>Overzicht
Azure DMS wordt uitgevoerd van een aanvankelijke belasting van uw on-premises naar Azure Database voor MySQL en vervolgens continu worden gesynchroniseerd met een nieuwe transacties naar Azure terwijl de toepassing actief blijft. Nadat de gegevens de resultaten op de doel-Azure aan clientzijde, u stop de toepassing voor een korte moment (minimale downtime) en wacht tot de laatste batch van gegevens (vanaf het moment dat u de toepassing stoppen voordat de toepassing effectief niet beschikbaar is voor alle nieuwe verkeer) om af te vangen omhoog in de doel- en werk vervolgens de verbindingsreeks om te verwijzen naar Azure. Wanneer u klaar bent, wordt uw toepassing worden live in Azure!

![Doorlopende synchronisatie met de Azure Database Migration Service](./media/howto-migrate-online/ContinuousSync.png)

## <a name="next-steps"></a>Volgende stappen
- Bekijk de video [eenvoudig migreren MySQL/PostgreSQL apps naar Azure managed service](https://medius.studios.ms/Embed/Video/THR2201?sid=THR2201), waarin een demo van het MySQL-apps migreren naar Azure Database voor MySQL.
- Zie de zelfstudie [MySQL migreren met Azure Database voor MySQL online met behulp van DMS](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online).
