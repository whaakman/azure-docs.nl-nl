---
title: Detectie van bedreigingen - Azure SQL Database | Microsoft Docs
description: Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database in een individuele database of elastische pool.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
manager: craigg
ms.date: 01/03/2019
ms.openlocfilehash: 3f4a120e2aaf2925805bec26f721d5cfb4194bf1
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54041540"
---
# <a name="azure-sql-database-threat-detection-for-single-database"></a>Azure SQL Database Threat Detection voor individuele Database

Azure SQL [detectie van bedreigingen](sql-database-threat-detection-overview.md) voor [SQL-Database](sql-database-technical-overview.md) individuele databases detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven. Detectie van bedreigingen kunt identificeren **mogelijke SQL-injectie**, **toegang vanaf ongebruikelijke locatie of data center**, **toegang vanaf onbekende principal of mogelijk schadelijke toepassing**, en **Brute force SQL-referenties** -Zie voor meer informatie [Threat Protection-waarschuwingen](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

U kunt meldingen ontvangen over de gedetecteerde bedreigingen via [e-mailmeldingen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) of [Azure-portal](sql-database-threat-detection.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) maakt deel uit van de [SQL Advanced Threat Protection](sql-advanced-threat-protection.md) (ATP)-aanbieding, dit een geïntegreerde-pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL is. Detectie van bedreigingen kan worden geopend en worden beheerd via de centrale SQL ATP-portal. Threat detection-service wordt in rekening gebracht 15$ / maand per logische Server met de eerste 30 dagen gratis.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Instellen van detectie van bedreigingen voor uw database in Azure portal

1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van de Azure SQL Database-server die u wilt beveiligen. Selecteer in de beveiligingsinstellingen **Advanced Threat Protection**.
3. Op de **Advanced Threat Protection** configuratiepagina:

   - Advanced Threat Protection inschakelen op de server.
   - In **instellingen voor Bedreigingsdetectie**, in de **waarschuwingen verzenden naar** tekst geeft u de lijst met e-mailberichten ontvangen beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten.
  
   ![Detectie van bedreigingen instellen](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Detectie van bedreigingen met behulp van PowerShell instellen

Zie voor een voorbeeldscript [controle en detectie van bedreigingen met behulp van PowerShell configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van bedreigingen](sql-database-threat-detection-overview.md).
- Meer informatie over [detectie van bedreigingen in het beheerde exemplaar](sql-database-managed-instance-threat-detection.md).  
- Meer informatie over [SQL Advanced Threat Protection](sql-advanced-threat-protection.md).
- Meer informatie over [Azure SQL Database Auditing](sql-database-auditing.md)
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Zie voor meer informatie over prijzen voor de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)  
