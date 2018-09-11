---
title: Inschakelen van controle en detectie van bedreigingen op SQL-servers in Azure Security Center | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **inschakelen controle en detectie van bedreigingen op SQL-servers**.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: ''
ms.assetid: 042fca4d-7dab-4172-8614-e8c21ccb4960
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: terrylan
ms.openlocfilehash: fade8de73a35b75f6096a25af13335d679caffac
ms.sourcegitcommit: f3bd5c17a3a189f144008faf1acb9fabc5bc9ab7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2018
ms.locfileid: "44300899"
---
# <a name="enable-auditing-and-threat-detection-on-sql-servers-in-azure-security-center"></a>Controle en detectie van bedreigingen op SQL-servers in Azure Security Center inschakelen
Azure Security Center wordt aanbevolen dat u de controle en detectie van bedreigingen voor alle databases op uw Azure SQL-servers als controle is niet ingeschakeld. Controle en threat detection kunt u de naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht verkrijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

Zodra u auditing hebt ingeschakeld kunt u instellingen voor detectie van bedreigingen en e-mailberichten voor het ontvangen van beveiligingswaarschuwingen configureren. Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database. Hiermee kunt u detecteren en erop reageren op mogelijke bedreigingen wanneer deze zich voordoen.

Deze aanbeveling is van toepassing op de Azure SQL-service. Deze bevat geen SQL Server wordt uitgevoerd op uw virtuele machines in Azure Infrastructure Services (Azure IaaS).

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **inschakelen controle en detectie van bedreigingen op SQL-servers**.  Hiermee opent u de **inschakelen controle en detectie van bedreigingen op SQL-servers** blade.

   ![Controleren voor SQL-servers inschakelen][1]
2. Selecteer een SQL-server voor controle en detectie van bedreigingen inschakelen op. Hiermee opent u de **controle en detectie van bedreigingen** blade.

3. Op de **controle en detectie van bedreigingen** Selecteer **ON** onder **controle**.

   ![Controle-instellingen inschakelen][2]
4. Volg de stappen in [SQL database auditing in Azure portal](../sql-database/sql-database-auditing-portal.md) het configureren van opslag waar uw auditlogboeken worden opgeslagen. Storage-account voor het verzamelen van gegevens van het abonnement is het standaardopslagaccount.
5. Volg de stappen in [aan de slag met SQL Database Threat Detection](../sql-database/sql-database-threat-detection.md) inschakelen en configureren van detectie van bedreigingen en het configureren van de lijst met e-mailberichten die beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten ontvangt.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u voor het implementeren van de Security Center aanbeveling "inschakelen controle en detectie van bedreigingen op SQL-servers." Zie de volgende onderwerpen voor meer informatie over het beveiligen van uw SQL-database:

* [Uw SQL-database beveiligen](../sql-database/sql-database-security-overview.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --de meest recente Azure-beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-server/enable-auditing-on-sql-servers.png
[2]: ./media/security-center-enable-auditing-on-sql-server/auditing-settings-blade.png
