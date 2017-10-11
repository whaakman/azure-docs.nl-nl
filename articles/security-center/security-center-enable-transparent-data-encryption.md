---
title: Transparante gegevensversleuteling in Azure Security Center inschakelen | Microsoft Docs
description: Dit document ziet u hoe de aanbeveling Azure Security Center implementeren ** inschakelen Transparent Data Encryption **.
services: security-center
documentationcenter: na
author: TerryLanfear
manager: MBaldwin
editor: 
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/02/2017
ms.author: terrylan
ms.openlocfilehash: 2a2963affdbff3710ad08f86c6ed4e6304335559
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparante gegevensversleuteling in Azure Security Center inschakelen
Azure Security Center beveelt aan dat u Transparent Data Encryption (TDE) voor SQL-databases inschakelen als TDE nog niet is ingeschakeld. TDE beschermt u uw gegevens en kunt u voldoen aan nalevingsvereisten door uw database, gekoppelde back-ups en transactielogboekbestanden in rust, zonder dat wijzigingen in uw toepassing te versleutelen. Voor meer informatie over [Transparent Data Encryption met Azure SQL Database](https://msdn.microsoft.com/library/dn948096).

Deze aanbeveling is van toepassing op de Azure SQL-service. bevat geen SQL uitgevoerd op uw virtuele machines.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. In de **aanbevelingen** blade Selecteer **Transparent Data Encryption inschakelen**.
   ![Transparante gegevensversleuteling inschakelen][1]
2. Hiermee opent u de **Transparent Data Encryption voor SQL-databases inschakelen** blade. Selecteer een SQL-database in te schakelen TDE voor.
   ![Selecteer SQL DB TDE inschakelen op][2]
3. Op de **transparante gegevensversleuteling** blade Selecteer **ON** onder gegevensversleuteling en selecteer **opslaan** in het bovenste lint van de blade.
   ![TDE inschakelen][3]

   Zodra TDE is ingeschakeld voor de geselecteerde SQL-database, de **coderingsstatus** wordt gewijzigd in **versleutelde**.    

   ![Coderingsstatus][4]

## <a name="see-also"></a>Zie ook
In dit artikel hebt u geleerd hoe u de aanbeveling Security Center implementeert 'Transparent Data Encryption inschakelen'. Voor meer informatie over SQL TDE, Zie de volgende:

* [Transparante gegevensversleuteling met Azure SQL Database](https://msdn.microsoft.com/library/dn948096)
* [Aan de slag met Transparent Data Encryption (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](security-center-policies.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in Azure Security Center beheren](security-center-recommendations.md) --Leer hoe aanbevelingen u uw Azure-resources te beveiligen.
* [Beveiligingsstatus bewaken in Azure Security Center](security-center-monitoring.md) --informatie over het bewaken van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-Beveiligingsblog](http://blogs.msdn.com/b/azuresecurity/) --Lees het laatste nieuws van de Azure-beveiliging en de informatie.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
