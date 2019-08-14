---
title: Transparent Data Encryption in Azure Security Center inschakelen | Microsoft Docs
description: Dit document bevat informatie over het implementeren van de Azure Security Center aanbeveling **transparent Data Encryption in te scha kelen**.
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: e4be8a0e-2118-4ee9-a266-69e52d9f7f8e
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: d5ddec40a1b20e377ec18ce871018f674557e7b4
ms.sourcegitcommit: 0f54f1b067f588d50f787fbfac50854a3a64fff7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/12/2019
ms.locfileid: "60703996"
---
# <a name="enable-transparent-data-encryption-in-azure-security-center"></a>Transparent Data Encryption in Azure Security Center inschakelen
Azure Security Center wordt aangeraden Transparent Data Encryption (TDE) in te scha kelen voor SQL-data bases als TDE nog niet is ingeschakeld. TDE beschermt uw gegevens en helpt u te voldoen aan de nalevings vereisten door uw data base, gekoppelde back-ups en transactie logboek bestanden in rust te versleutelen, zonder dat u wijzigingen hoeft aan te brengen in uw toepassing. Zie [transparent Data Encryption met Azure SQL database](https://msdn.microsoft.com/library/dn948096)voor meer informatie.

Deze aanbeveling is alleen van toepassing op de Azure SQL-service. bevat geen SQL die wordt uitgevoerd op uw virtuele machines.

> [!NOTE]
> In dit document wordt de service geïntroduceerd aan de hand van een voorbeeldimplementatie.  Dit is geen stapsgewijze handleiding.
>
>

## <a name="implement-the-recommendation"></a>De aanbeveling implementeren
1. Selecteer op de Blade **aanbevelingen** de optie **transparent Data Encryption inschakelen**.
   ![Transparante gegevensversleuteling inschakelen][1]
2. Hiermee opent u de Blade **transparent Data Encryption inschakelen op SQL-data bases** . Selecteer een SQL database om TDE in te scha kelen.
   ![Selecteer SQL-Data Base om TDE in te scha kelen][2]
3. Op de Blade **transparante gegevens versleuteling** selecteert u onder gegevens versleuteling en selecteert u **Opslaan** in het bovenste lint van de Blade.
   ![TDE inschakelen][3]

   Zodra TDE is ingeschakeld op de geselecteerde SQL database, wordt de versleutelings **status** gewijzigd in versleuteld.    

   ![Versleutelingsstatus][4]

## <a name="see-also"></a>Zie ook
In dit artikel wordt uitgelegd hoe u de Security Center aanbeveling kunt implementeren Transparent Data Encryption. Zie het volgende voor meer informatie over SQL TDE:

* [Transparent Data Encryption met Azure SQL Database](https://msdn.microsoft.com/library/dn948096)
* [Aan de slag met Transparent Data Encryption (TDE)](../sql-data-warehouse/sql-data-warehouse-encryption-tde.md)

Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Setting security policies in Azure Security Center](tutorial-security-policy.md) (Beveiligingsbeleid instellen in Azure Security Center): leer hoe u beveiligingsbeleid voor uw Azure-abonnementen en -resourcegroepen configureert.
* [Aanbevelingen voor beveiliging in azure Security Center](security-center-recommendations.md) : Ontdek hoe aanbevelingen u helpen uw Azure-resources te beveiligen.
* [Beveiligings status controleren in azure Security Center](security-center-monitoring.md) --meer informatie over het controleren van de status van uw Azure-resources.
* [Managing and responding to security alerts in Azure Security Center](security-center-managing-and-responding-alerts.md) (Beveiligingswaarschuwingen beheren en erop reageren in Azure Security Center): ontdek hoe u beveiligingswaarschuwingen kunt beheren en erop kunt reageren.
* [Partneroplossingen bewaken met Azure Security Center](security-center-partner-solutions.md): leer hoe u de integriteitsstatus van uw partneroplossingen kunt bewaken.
* [Azure Security Center FAQ](security-center-faq.md) (Veelgestelde vragen over Azure Security Center): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Azure-beveiligings blog](https://blogs.msdn.com/b/azuresecurity/) : krijg het meest recente Azure-beveiligings nieuws en-informatie.

<!--Image references-->
[1]: ./media/security-center-enable-tde-on-sql-databases/enable-tde.png
[2]:./media/security-center-enable-tde-on-sql-databases/transparent-data-encryption-blade.png
[3]: ./media/security-center-enable-tde-on-sql-databases/turn-on-tde.png
[4]: ./media/security-center-enable-tde-on-sql-databases/encrypted.png
