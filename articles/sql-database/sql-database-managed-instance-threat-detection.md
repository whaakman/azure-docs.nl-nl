---
title: Configureren van detectie van bedreigingen - Azure SQL Database managed instance | Microsoft Docs
description: Threat detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database in een beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: howto
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 02/04/2019
ms.openlocfilehash: d8522967154a69b8473475932f2074bc98b4f24d
ms.sourcegitcommit: 3aa0fbfdde618656d66edf7e469e543c2aa29a57
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/05/2019
ms.locfileid: "55731220"
---
# <a name="configure-threat-detection-preview-in-azure-sql-database-managed-instance"></a>Detectie van bedreigingen (Preview) in beheerd exemplaar voor Azure SQL Database configureren

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) voor een [beheerd exemplaar](sql-database-managed-instance-index.yml) detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven. Detectie van bedreigingen kunt identificeren **mogelijke SQL-injectie**, **toegang vanaf ongebruikelijke locatie of data center**, **toegang vanaf onbekende principal of mogelijk schadelijke toepassing**, en **Brute force SQL-referenties** -Zie voor meer informatie [waarschuwingen voor detectie van bedreigingen](sql-database-threat-detection-overview.md#threat-detection-alerts).

U kunt meldingen ontvangen over de gedetecteerde bedreigingen via [e-mailmeldingen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) of [Azure-portal](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) maakt deel uit van de [geavanceerde gegevensbeveiliging](sql-database-advanced-data-security.md) (AD) aanbieding waarmee een uniforme-voor geavanceerde mogelijkheden voor de beveiliging van SQL pakket. Detectie van bedreigingen kan worden geopend en worden beheerd via de centrale SQL AD-portal. Threat detection-service wordt in rekening gebracht 15$ / maand per beheerd exemplaar met de eerste 30 dagen gratis.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Detectie van bedreigingen voor uw beheerde exemplaar in de Azure-portal instellen

1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van het beheerde exemplaar dat u wilt beveiligen. In de **instellingen** weergeeft, schakelt **detectie van bedreigingen**.
3. In de configuratiepagina van de detectie van bedreigingen
   - Schakel **ON** detectie van bedreigingen.
   - Configureer de **lijst met e-mailberichten** voor het ontvangen van beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten.
   - Selecteer de **Azure storage-account** waar controlerecords afwijkende bedreigingen zijn opgeslagen.
4. Klik op **opslaan** de nieuwe of bijgewerkte threat detection-beleid op te slaan.

   ![Detectie van bedreigingen](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van bedreigingen](sql-database-threat-detection-overview.md).
- Meer informatie over beheerde exemplaren, Zie [wat is er een beheerd exemplaar](sql-database-managed-instance.md).
- Meer informatie over [bedreigingen voor één database](sql-database-threat-detection.md).
- Meer informatie over [beheerd exemplaar controle](https://go.microsoft.com/fwlink/?linkid=869430).
- Meer informatie over [Azure security center](https://docs.microsoft.com/azure/security-center/security-center-intro).
