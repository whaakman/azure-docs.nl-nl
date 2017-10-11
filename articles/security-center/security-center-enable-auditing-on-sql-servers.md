---
title: Inschakelen van controle en detectie van bedreigingen op SQL-servers in Azure Security Center | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** controle inschakelen en detectie van bedreigingen op SQL-servers **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: 660b537aef8d175a478ff93d60b8391d55fc92ad
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a>Inschakelen van controle en detectie van bedreigingen op SQL-servers in Azure Security Center
Azure Security Center beveelt aan dat u controle inschakelt en detectie van dreigingen voor alle databases op uw Azure SQL-servers als controle is niet ingeschakeld. Controle en threat detectie kunt u de naleving van regelgeving onderhouden, de activiteit in een database begrijpen en meer van inzicht krijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

Wanneer u eenmaal hebt ingeschakeld controle kunt u instellingen voor de detectie van dreigingen en e-mailberichten voor het ontvangen van beveiligingsberichten kunt configureren. Detectie van dreigingen detecteert afwijkende databaseactiviteiten die wijzen op beveiligingsdreigingen voor de database. Hiermee kunt u om te detecteren en op mogelijke bedreigingen reageert wanneer deze zich voordoen.

Deze aanbeveling is van toepassing op de Azure SQL-service. SQL Server wordt uitgevoerd op uw virtuele machines in Azure-infrastructuurservices (Azure IaaS) zijn niet opgenomen.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **controle inschakelen en detectie van bedreigingen op SQL-servers**.  Hiermee opent u de **controle inschakelen en detectie van bedreigingen op SQL-servers** blade.

   ![Controleren voor SQL-servers inschakelen][1]
2. Selecteer een SQL-server inschakelen van controle en detectie van bedreigingen op. Hiermee opent u de **controle en detectie van dreigingen** blade.

3. Op de **controle en detectie van dreigingen** blade Selecteer **ON** onder **controle**.

   ![Controle-instellingen inschakelen][2]
4. Volg de stappen in [SQL database auditing in de Azure portal](../sql-database/sql-database-auditing-portal.md) configureren van opslag waar de controlelogboeken worden opgeslagen. Storage-account voor het verzamelen van gegevens van het abonnement is het standaardopslagaccount.
5. Volg de stappen in [aan de slag met SQL Database met detectie van dreigingen](../sql-database/sql-database-threat-detection.md) inschakelen en configureren van detectie van dreigingen en de lijst met e-mailberichten die u waarschuwingen op vreemde activiteiten ontvangt configureert.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u implementeert de Security Center aanbeveling ' Enable controle en detectie van bedreigingen op SQL-servers." Zie de volgende onderwerpen voor meer informatie over het beveiligen van uw SQL-database:

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
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
