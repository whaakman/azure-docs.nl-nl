---
title: Detectie van bedreigingen - Azure SQL Database Managed Instance | Microsoft Docs
description: Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database in een beheerd exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
manager: craigg
ms.date: 12/06/2018
ms.openlocfilehash: a456a214143f39ed9504af40129f9199b2535e46
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52997142"
---
# <a name="azure-sql-database-managed-instance-threat-detection-preview"></a>Azure SQL Database Managed Instance detectie van bedreigingen (Preview)

Azure SQL [detectie van bedreigingen](sql-database-threat-detection-overview.md) voor [SQL Database Managed Instance](sql-database-managed-instance-index.yml) detecteert afwijkende activiteiten die ongebruikelijke en potentieel schadelijke pogingen om toegang tot of misbruik te maken van databases waarmee wordt aangegeven. Detectie van bedreigingen kunt identificeren **mogelijke SQL-injectie**, **toegang vanaf ongebruikelijke locatie of data center**, **toegang vanaf onbekende principal of mogelijk schadelijke toepassing**, en **Brute force SQL-referenties** -Zie voor meer informatie [Threat Protection-waarschuwingen](sql-database-threat-detection-overview.md#azure-sql-database-threat-detection-alerts).

U kunt meldingen ontvangen over de gedetecteerde bedreigingen via [e-mailmeldingen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) of [Azure-portal](sql-database-threat-detection-overview.md#explore-threat-detection-alerts-for-your-database-in-the-azure-portal)

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) maakt deel uit van de [SQL Advanced Threat Protection](sql-advanced-threat-protection.md) (ATP)-aanbieding, dit een geïntegreerde-pakket voor geavanceerde mogelijkheden voor de beveiliging van SQL is. Detectie van bedreigingen kan worden geopend en worden beheerd via de centrale SQL ATP-portal. Threat detection-service wordt in rekening gebracht 15$ / maand per Managed Instance met de eerste 30 dagen gratis.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Detectie van bedreigingen instellen voor uw beheerde exemplaar in de Azure-portal
1. Starten van de Azure portal op [ https://portal.azure.com ](https://portal.azure.com).
2. Navigeer naar de configuratiepagina van het beheerde exemplaar u wilt beveiligen. In de **instellingen** weergeeft, schakelt **detectie van bedreigingen**. 
3. In de configuratiepagina van de detectie van bedreigingen 
   - Schakel **ON** detectie van bedreigingen.
   - Configureer de **lijst met e-mailberichten** voor het ontvangen van beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten.
   - Selecteer de **Azure storage-account** waar controlerecords afwijkende bedreigingen zijn opgeslagen. 
4.  Klik op **opslaan** de nieuwe of bijgewerkte threat detection-beleid op te slaan.

   ![Detectie van bedreigingen](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van bedreigingen](sql-database-threat-detection-overview.md). 
- Meer informatie over Managed Instance, Zie [wat is een beheerd exemplaar](sql-database-managed-instance.md). 
- Meer informatie over [detectie van bedreigingen voor één Database](sql-database-threat-detection.md). 
- Meer informatie over [beheerd exemplaar controle](https://go.microsoft.com/fwlink/?linkid=869430). 
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
