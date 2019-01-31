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
ms.date: 01/30/2019
ms.openlocfilehash: 032146742c1a49fc8cdbda24d5c732add3cc5ea7
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55465347"
---
# <a name="azure-sql-database-threat-detection-for-standalone-or-pooled-database"></a>Azure SQL Database Threat Detection voor zelfstandige of gegroepeerde-database

Azure SQL [detectie van bedreigingen](sql-database-threat-detection-overview.md) voor zelfstandige en gepoolde databases detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven. Detectie van bedreigingen kunt identificeren **mogelijke SQL-injectie**, **toegang vanaf ongebruikelijke locatie of data center**, **toegang vanaf onbekende principal of mogelijk schadelijke toepassing**, en **Brute force SQL-referenties** -Zie voor meer informatie [Threat Protection-waarschuwingen](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

U kunt meldingen ontvangen over de gedetecteerde bedreigingen via [e-mailmeldingen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) of [Azure-portal](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) maakt deel uit van de [SQL geavanceerde gegevensbeveiliging](sql-advanced-threat-protection.md) (AD) aanbieding waarmee een uniforme-voor geavanceerde mogelijkheden voor de beveiliging van SQL pakket. Detectie van bedreigingen kan worden geopend en worden beheerd via de centrale SQL AD-portal. Het pakket met geavanceerde beveiliging van gegevens wordt in rekening gebracht 15$ / maand per logische Server met de eerste 30 dagen gratis.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Instellen van detectie van bedreigingen voor uw database in Azure portal

1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van de Azure SQL Database-server die u wilt beveiligen. Selecteer in de beveiligingsinstellingen **gegevensbeveiliging geavanceerde**.
3. Op de **gegevensbeveiliging geavanceerde** configuratiepagina:

   - Schakel beveiliging van geavanceerde gegevens op de server.
   - In **instellingen voor Bedreigingsdetectie**, in de **waarschuwingen verzenden naar** tekst geeft u de lijst met e-mailberichten ontvangen beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten.
  
   ![Detectie van bedreigingen instellen](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Detectie van bedreigingen met behulp van PowerShell instellen

Zie voor een voorbeeldscript [controle en detectie van bedreigingen met behulp van PowerShell configureren](scripts/sql-database-auditing-and-threat-detection-powershell.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van bedreigingen](sql-database-threat-detection-overview.md).
- Meer informatie over [detectie van bedreigingen in het beheerde exemplaar](sql-database-managed-instance-threat-detection.md).  
- Meer informatie over [SQL Advanced gegevensbeveiliging](sql-advanced-threat-protection.md).
- Meer informatie over [Azure SQL Database Auditing](sql-database-auditing.md)
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Zie voor meer informatie over prijzen voor de [pagina met prijzen van SQL-Database](https://azure.microsoft.com/pricing/details/sql-database/)  
