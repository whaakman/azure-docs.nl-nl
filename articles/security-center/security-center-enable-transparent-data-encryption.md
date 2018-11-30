---
title: Transparent Data Encryption in Azure Security Center inschakelen | Microsoft Docs
description: Dit document wordt beschreven hoe u de Azure Security Center-aanbeveling kunt implementeren **Transparent Data Encryption inschakelen**.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: 765a3a985a2198951ed17292dc376b25adfccc03
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52314797"
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparent Data Encryption in Azure Security Center inschakelen
Azure Security Center wordt aanbevolen dat u transparante gegevensversleuteling (TDE) voor SQL-databases inschakelen als TDE is nog niet ingeschakeld. TDE beschermt uw gegevens en helpt u bij het voldoen aan nalevingsvereisten door het versleutelen van uw database, gekoppelde back-ups en transactielogboekbestanden in rust, zonder wijzigingen aan uw toepassing. Voor meer informatie over meer Zie [Transparent Data Encryption met Azure SQL Database](https://msdn.microsoft.com/library/dn948096).

Deze aanbeveling is van toepassing op de Azure SQL-service. geen SQL die worden uitgevoerd op uw virtuele machines.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling voor het implementeren
1. In de **aanbevelingen** Selecteer **Transparent Data Encryption inschakelen**.
   ![Transparante gegevensversleuteling inschakelen][1]
2. Hiermee opent u de **Transparent Data Encryption voor SQL-databases inschakelen** blade. Selecteer een SQL-database TDE inschakelen op.
   ![Selecteer SQL DB TDE inschakelen op][2]
3. Op de **transparante gegevensversleuteling** Selecteer **ON** onder gegevensversleuteling en selecteer **opslaan** in het lint boven aan de blade.
   ![TDE inschakelen][3]

   Zodra TDE is ingeschakeld voor de geselecteerde SQL-database, de **versleutelingsstatus** wordt gewijzigd in **versleutelde**.    

   ![Versleutelingsstatus][4]

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u de aanbeveling van Security Center implementeren 'Transparent Data Encryption inschakelen'. Voor meer informatie over SQL TDE, Zie de volgende:

* [Transparante gegevensversleuteling met Azure SQL Database](https://msdn.microsoft.com/library/dn948096)
* [Aan de slag met transparante gegevensversleuteling (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-azure-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen helpen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](https://blogs.msdn.com/b/azuresecurity/) --de meest recente Azure-beveiliging nieuws en informatie.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
