---
title: Detectie van bedreigingen configureren-Azure SQL Database beheerd exemplaar | Microsoft Docs
description: Met detectie van bedreigingen worden afwijkende database activiteiten gedetecteerd die potentiële beveiligings dreigingen aan de data base in een beheerd exemplaar aangeven.
services: sql-database
ms.service: sql-database
ms.subservice: security
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: rmatchoro
ms.author: ronmat
ms.reviewer: vanto
ms.date: 02/04/2019
ms.openlocfilehash: 6d1f2a9547f01de91a8e7739d827a91154842d7b
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68567294"
---
# <a name="configure-threat-detection-preview-in-azure-sql-database-managed-instance"></a>Detectie van bedreigingen (preview) configureren in Azure SQL Database Managed instance

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) voor een [beheerd exemplaar](sql-database-managed-instance-index.yml) detecteert afwijkende activiteiten die een ongebruikelijke en potentieel schadelijke pogingen om toegang te krijgen tot data bases of deze te gebruiken. Detectie van bedreigingen kan leiden tot **mogelijke SQL**-injecties, **toegang vanaf ongebruikelijke locatie of Data Center**, **toegang tot een onbekende principal of mogelijk schadelijke toepassing**en **SQL-referenties** voor de beveiligings aanval: meer details bekijken in [waarschuwingen voor detectie van bedreigingen](sql-database-threat-detection-overview.md#advanced-threat-protection-alerts).

U kunt meldingen over de gedetecteerde bedreigingen ontvangen via [e-mail meldingen](sql-database-threat-detection-overview.md#explore-anomalous-database-activities-upon-detection-of-a-suspicious-event) of [Azure Portal](sql-database-threat-detection-overview.md#explore-advanced-threat-protection-alerts-for-your-database-in-the-azure-portal)

[Detectie van bedreigingen](sql-database-threat-detection-overview.md) maakt deel uit van de aanbieding voor [geavanceerde gegevens beveiliging](sql-database-advanced-data-security.md) (ADS), een uniform pakket voor geavanceerde SQL-beveiligings mogelijkheden. Detectie van bedreigingen kan worden geopend en beheerd via de centrale SQL ADS-Portal. Voor de service voor de detectie van bedreigingen wordt 15 $/maand per beheerd exemplaar gefactureerd, met een gratis prijs van 30 dagen.

## <a name="set-up-threat-detection-for-your-managed-instance-in-the-azure-portal"></a>Detectie van bedreigingen instellen voor uw beheerde instantie in de Azure Portal

1. Start de Azure Portal op [https://portal.azure.com](https://portal.azure.com).
2. Ga naar de configuratie pagina van het beheerde exemplaar dat u wilt beveiligen. Selecteer op de pagina **instellingen** de optie **bedreigingen detectie**.
3. Op de pagina detectie van bedreigingen
   - Schakel detectie **van bedreigingen in** .
   - De **lijst met e-mail** berichten configureren voor het ontvangen van beveiligings waarschuwingen bij het detecteren van afwijkende database activiteiten.
   - Selecteer het **Azure-opslag account** waarin records voor afwijkende bedreigings controle worden opgeslagen.
4. Klik op **Opslaan** om het nieuwe of bijgewerkte beleid voor de detectie van bedreigingen op te slaan.

   ![detectie van bedreigingen](./media/sql-database-managed-instance-threat-detection/threat-detection.png)

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [detectie van dreigingen](sql-database-threat-detection-overview.md).
- Zie [Wat is een beheerd exemplaar](sql-database-managed-instance.md)? voor meer informatie over beheerde exemplaren.
- Meer informatie over [detectie van bedreigingen voor één data base](sql-database-threat-detection.md).
- Meer informatie over [controle van beheerde exemplaren](https://go.microsoft.com/fwlink/?linkid=869430).
- Meer informatie over [Azure Security Center](https://docs.microsoft.com/azure/security-center/security-center-intro).
