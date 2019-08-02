---
title: Detectie van bedreigingen-Azure SQL Database | Microsoft Docs
description: Met detectie van bedreigingen worden afwijkende database activiteiten gedetecteerd die potentiële beveiligings dreigingen voor de data base in één data base of elastische pool aangeven.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto, carlrab
ms.date: 02/08/2019
ms.openlocfilehash: 5549d016978e8bf9491c3745e335e3c4c793212c
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68566334"
---
# <a name="azure-sql-database-threat-detection-for-single-or-pooled-databases"></a>Detectie van Azure SQL Database bedreigingen voor één of gegroepeerde Data bases

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) voor enkelvoudige en gegroepeerde Data bases detecteert afwijkende activiteiten die een ongebruikelijke en potentieel schadelijke pogingen om toegang te krijgen tot data bases of deze Detectie van bedreigingen kan leiden tot **mogelijke SQL**-injecties, **toegang vanaf ongebruikelijke locatie of Data Center**, **toegang tot een onbekende principal of mogelijk schadelijke toepassing**en **SQL-referenties** voor de beveiligings aanval: meer details bekijken in [waarschuwingen voor detectie van bedreigingen](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

U kunt meldingen over de gedetecteerde bedreigingen ontvangen via [e-mail meldingen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) of [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal)

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) maakt deel uit van de aanbieding voor [geavanceerde gegevens beveiliging](sql-database-advanced-data-security.md) (ADS), een uniform pakket voor geavanceerde SQL-beveiligings mogelijkheden. Detectie van bedreigingen kan worden geopend en beheerd via de centrale SQL ADS-Portal. Voor het geavanceerde gegevens beveiligings pakket wordt 15 $/maand per logische server gefactureerd, met de eerste 30 dagen gratis.

## <a name="set-up-threat-detection-for-your-database-in-the-azure-portal"></a>Detectie van bedreigingen voor uw data base instellen in de Azure Portal

1. Start de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Ga naar de configuratie pagina van de Azure SQL Database-Server die u wilt beveiligen. Selecteer **geavanceerde gegevens beveiliging**in de beveiligings instellingen.
3. Op de pagina Geavanceerde configuratie van **gegevens beveiliging** :

   - Geavanceerde gegevens beveiliging op de server inschakelen.
   - In de **instellingen voor detectie van bedreigingen**geeft u in het tekstvak **waarschuwingen verzenden naar** de lijst met e-mail berichten op om beveiligings waarschuwingen te ontvangen bij het detecteren van afwijkende database activiteiten.
  
   ![Detectie van bedreigingen instellen](./media/sql-database-threat-detection/set_up_threat_detection.png)

## <a name="set-up-threat-detection-using-powershell"></a>Detectie van bedreigingen instellen met behulp van Power shell

Zie [controle en detectie van bedreigingen configureren met Power shell](scripts/sql-database-auditing-and-threat-detection-powershell.md)voor een voor beeld van een script.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van dreigingen](sql-database-threat-detection-overview.md).
- Meer informatie over [detectie van dreigingen in een beheerd exemplaar](sql-database-managed-instance-threat-detection.md).  
- Meer informatie over [geavanceerde gegevens beveiliging](sql-database-advanced-data-security.md).
- Meer informatie over [auditing](sql-database-auditing.md)
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro)
- Zie de [pagina met prijzen voor SQL database](https://azure.microsoft.com/pricing/details/sql-database/) voor meer informatie over prijzen.  
