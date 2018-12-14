---
title: Inschakelen van controle en detectie van bedreigingen op SQL-databases in Azure Security Center | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **inschakelen van controle en detectie van bedreigingen op SQL-databases**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: 224b6755-2b36-4ecd-9af8-139a198e0df1
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 3de11b88f0bbf10cc0f1716cc0cefd128797cd5f
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341572"
---
# <a name="enable-auditing-and-threat-detection-on-sql-databases-in-azure-security-center"></a>Controle en detectie van bedreigingen op SQL-databases in Azure Security Center inschakelen
Azure Security Center wordt aanbevolen dat u controle en detectie van bedreigingen voor alle SQL-databases inschakelen als controle en detectie van bedreigingen is niet ingeschakeld. Controle en threat detection kunt u de naleving van regelgeving, inzicht in de databaseactiviteiten en inzicht verkrijgen in discrepanties en afwijkingen die kunnen wijzen op problemen voor het bedrijf of vermoedelijke beveiligingsschendingen.

Zodra u auditing hebt ingeschakeld kunt u instellingen voor detectie van bedreigingen en e-mailberichten voor het ontvangen van beveiligingswaarschuwingen configureren. Threat Detection detecteert afwijkende activiteiten die wijzen op mogelijke beveiligingsrisico's met de database. Hiermee kunt u detecteren en erop reageren op mogelijke bedreigingen wanneer deze zich voordoen.

Deze aanbeveling is van toepassing op de Azure SQL-service. Deze bevat geen SQL die worden uitgevoerd op uw virtuele machines.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **inschakelen controle en detectie van bedreigingen op SQL-databases**.  Hiermee opent u de **inschakelen controle en detectie van bedreigingen op SQL-databases** blade.

   ![Controleren voor SQL-databases inschakelen][1]
2. Selecteer een SQL-database om te controleren voor inschakelen. Hiermee opent u de **controle en detectie van bedreigingen** blade.

3. Op de **controle en detectie van bedreigingen** Selecteer **ON** onder **controle**.

   ![Controle en detectie van bedreigingen inschakelen][2]
4. Volg de stappen in [SQL Database Threat Detection in Azure portal](../sql-database/sql-database-threat-detection-portal.md) inschakelen en configureren van detectie van bedreigingen en het configureren van de lijst met e-mailberichten die beveiligingswaarschuwingen tijdens de detectie van afwijkende activiteiten ontvangt.

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u voor het implementeren van de aanbeveling van Security Center "inschakelen controle en detectie van bedreigingen op SQL-databases." Zie de volgende onderwerpen voor meer informatie over het beveiligen van uw SQL-database:

* [Uw SQL-database beveiligen](../sql-database/sql-database-security-overview.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) --de meest recente Azure-beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-auditing-on-sql-databases/enable-auditing-on-sql-databases.png
[2]: ./media/security-center-enable-auditing-on-sql-databases/auditing-threat-detection-blade.png
