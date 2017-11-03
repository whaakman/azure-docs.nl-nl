---
title: Inschakelen van controle en detectie van bedreigingen op SQL-databases in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** inschakelen van controle en detectie van bedreigingen op SQL-databases **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 8f4febdaa4497fee0dc690b59cd6eaa415c5e5cf
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>Inschakelen van controle en detectie van bedreigingen op SQL-databases in Azure Security Center
Azure Security Center beveelt aan dat u controle en detectie van bedreigingen voor alle SQL-databases inschakelen als de controle en detectie van dreigingen nog niet is ingeschakeld. Controle en threat detectie kunt u de naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer van inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

Wanneer u eenmaal hebt ingeschakeld controle kunt u instellingen voor de detectie van dreigingen en e-mailberichten voor het ontvangen van beveiligingsberichten kunt configureren. Detectie van dreigingen detecteert afwijkende databaseactiviteiten die wijzen op beveiligingsdreigingen voor de database. Hiermee kunt u om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen.

Deze aanbeveling is van toepassing op de Azure SQL-service. SQL uitgevoerd op uw virtuele machines zijn niet opgenomen.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **controle inschakelen en detectie van bedreigingen op SQL-databases**.  Hiermee opent u de **controle inschakelen en detectie van bedreigingen op SQL-databases** blade.

   ![Controleren voor SQL-databases inschakelen][1]
2. Selecteer een SQL-database inschakelen van controle op. Hiermee opent u de **controle en detectie van dreigingen** blade.

3. Op de **controle en detectie van dreigingen** blade Selecteer **ON** onder **controle**.

   ![Inschakelen van controle en detectie van bedreigingen][2]
4. Volg de stappen in [SQL Database met detectie van dreigingen in de Azure portal](../sql-database/sql-database-threat-detection-portal.md) inschakelen en configureren van detectie van dreigingen en de lijst met e-mailberichten die u waarschuwingen op vreemde activiteiten ontvangt configureert.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u implementeert de aanbeveling Security Center ' Enable controle en detectie van bedreigingen op SQL-databases.' Zie de volgende onderwerpen voor meer informatie over het beveiligen van uw SQL-database:

* [Uw SQL-database beveiligen](../sql-database/sql-database-security-overview.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees het laatste nieuws van de Azure-beveiliging en de informatie.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
