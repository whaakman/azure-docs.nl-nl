---
title: Azure SQL Database-resourcebeperkingen - beheerd exemplaar | Microsoft Docs
description: In dit artikel biedt een overzicht van de Azure SQL Database-resourcebeperkingen voor beheerde exemplaren.
services: sql-database
ms.service: sql-database
ms.subservice: managed-instance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab, jovanpop
manager: craigg
ms.date: 10/02/2018
ms.openlocfilehash: f2b1a8e18917c1c045c715bd3424d0bbfc0cdc67
ms.sourcegitcommit: 3856c66eb17ef96dcf00880c746143213be3806a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/02/2018
ms.locfileid: "48045372"
---
# <a name="overview-azure-sql-database-managed-instance-resource-limits"></a>Overzicht van Azure SQL Database Managed Instance-resourcebeperkingen

In dit artikel biedt een overzicht van de resourcelimieten voor Azure SQL Database Managed Instance en bevat informatie over het maken van de aanvraag om abonnement standaardlimieten te verhogen. 

> [!NOTE]
> Zie voor andere Managed Instance beperkingen, die niet specifiek zijn voor afzonderlijke abonnement [vCore gebaseerde aankoopmodel](sql-database-managed-instance.md#vcore-based-purchasing-model) en [Managed Instance-Servicelagen](sql-database-managed-instance.md#managed-instance-service-tiers).

## <a name="default-subscription-level-limits-per-region"></a>Abonnementsniveau standaardlimieten per regio

Beheerd exemplaar ondersteunt momenteel implementatie alleen op de volgende typen abonnementen:

- [Enterprise Agreement (EA)](https://azure.microsoft.com/pricing/enterprise-agreement/)
- [Betalen per gebruik](https://azure.microsoft.com/offers/ms-azr-0003p/)
- [Cloud serviceprovider (CSP)](https://docs.microsoft.com/partner-center/csp-documents-and-learning-resources)

> [!NOTE]
> Deze beperking over ondersteuning voor alleen bepaalde abonnementstypen is tijdelijk.

Beheerde exemplaren heeft twee standaard abonnementsniveau limieten per Azure-regio. Andere abonnementstypen hebben verschillende regionale beperkingen. Deze limieten kunnen worden verhoogd door het maken van speciale ondersteuningsaanvraag in de Azure-portal voor het abonnement met probleemtype **quotum**:

- **Subnet limiet**: het maximum aantal subnetten waarop beheerde exemplaren zijn geïmplementeerd
- **Maximumaantal exemplaar**: het maximum aantal exemplaren per regio

> [!IMPORTANT]
> Wanneer u uw implementatie plant, houd rekening met dat een kritieke zakelijke (BC)-exemplaar (als gevolg van toegevoegde redundantie) in het algemeen 4 x meer capaciteit dan een algemeen doel (GP)-exemplaar worden verbruikt. Ja, voor de berekeningen, 1 exemplaar van de GP = 1 exemplaar eenheid en 1 BC instantie = 4 eenheden van het exemplaar. Ter vereenvoudiging van uw verbruik analyse op basis van de standaardlimieten, een overzicht maken van de exemplaar-eenheden via alle subnetten in de regio waar beheerde instanties zijn geïmplementeerd en vergelijk de resultaten met de exemplaar-eenheid-limieten voor uw abonnementstype.

## <a name="default-limits-by-subscription-type"></a>Standaard beperkt door het abonnementstype

|Abonnementstype| Maximumaantal Managed Instance-subnetten | Maximumaantal exemplaren |Maximumaantal GP beheerde exemplaren *|Maximumaantal BC beheerde exemplaren *|
| :---| :--- | :--- |:--- |:--- |
|Betalen naar gebruik|1 *|4 *|4 *|1 *|
|CSP |1 *|4 *|4 *|1 *|
|EA|3 **|12 **|12 **|3 **|

\* U kunt ofwel 1 BC of 4 GP-exemplaren in één subnet implementeren, zodat het totaal aantal eenheden' instantie' die in een regio nooit meer dan 4.

** Maximum aantal exemplaren in een servicelaag van toepassing als er geen exemplaren in een andere servicelaag zijn. Als u van plan bent om GP- en BC-instanties binnen hetzelfde subnet bevinden, maken gebruik van de volgende sectie als uitgangspunt voor toegestane combinaties. Als een eenvoudige regel, het totale aantal subnetten niet langer zijn dan 3 en het totale aantal eenheden van de sessie mag niet meer dan 12.

## <a name="deployment-options-for-gp-and-bc-deployments-within-the-same-subnet"></a>Implementatie-opties voor BC- en GP-implementaties binnen hetzelfde subnet

De volgende voorbeelden betrekking op implementatie gevallen met niet-lege subnetten en GP- en BC gemengde service-lagen.

|Aantal subnetten|subnet 1|Subnet 2|Subnet 3|
|:---|:---|:---|:---|
|1|BC 0 en maximaal 12 GP<br>BC 1 en maximaal 8 GP<br>BC 2 en maximaal 4 GP<br>BC 3|N/A| N/A|
|2|0 BC, tot 4 GP|0 BC, tot maximaal 8 GP<br>1 BC, tot 4 GP<br>BC 2|N/A|
|2|1 BC|0 BC, tot maximaal 8 GP<br>1 BC, tot 4 GP<br>BC 2|N/A|
|2|BC 2|0 BC, tot maximaal 8 GP<br>1 BC, tot 4 GP<br>BC 2|N/A|
|3|BC 1, 0 GP|BC 1, 0 GP|0 BC, tot 4 GP|
|3|1BC, 0 GP|0 BC, tot 4 GP|BC 1, 0 GP|
|3|0 BC, tot 4 GP|BC 1, 0 GP|1BC, 0 GP|

## <a name="obtaining-a-larger-quota-for-sql-managed-instance"></a>Het ophalen van een grotere quotum voor beheerd exemplaar van SQL

Het proces voor het verkrijgen van een grotere quotum initiëren:

1. Open **Help en ondersteuning**, en klikt u op **nieuwe ondersteuningsaanvraag**. 

   ![Help en ondersteuning](media/sql-database-managed-instance-resource-limits/help-and-support.png)
2. Op het tabblad basisbeginselen voor het nieuwe ondersteuningsaanvraag:
   - Voor **type probleem**, selecteer **limieten voor Service en -abonnement (quota)**.
   - Bij **Abonnement** selecteert u uw abonnement.
   - Voor **quotumtype**, selecteer **SQL Database Managed Instance**.
   - Voor **ondersteuningsplan**, selecteert u uw ondersteuningsplan.

     ![Probleem type quotum](media/sql-database-managed-instance-resource-limits/issue-type-quota.png)

3. Klik op **Volgende**.
4. Op het tabblad probleem voor de nieuwe ondersteuningsaanvraag:
   - Voor **ernst**, selecteert u de ernst van het probleem.
   - Voor **Details**, bieden aanvullende informatie over uw probleem, met inbegrip van foutberichten.
   - Voor **uploaden van het bestand**, koppelt u een bestand met meer informatie (maximaal 4 MB).

     ![Probleemdetails](media/sql-database-managed-instance-resource-limits/problem-details.png)

     > [!IMPORTANT]
     > Er moet een geldige aanvraag omvatten:
     > - Regio in welk abonnement u limiet moet worden verhoogd
     > - Vereiste aantal exemplaren per servicelaag in bestaande subnetten na het quotum (als een van de bestaande subnetten moet worden uitgebreid verhogen
     > - Aantal nieuwe subnetten en totaal aantal exemplaren per servicelaag in de nieuwe subnetten vereist (als u implementeren van beheerde exemplaren in de nieuwe subnetten wilt).
5. Klik op **Volgende**.
6. Voer op het tabblad contact opnemen met informatie voor de nieuwe ondersteuningsaanvraag contactwijze (bijvoorbeeld e-mail of telefoon) en de gegevens van de contactpersoon.
7. Klik op **Create**.

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie over Managed Instance [wat is een beheerd exemplaar?](sql-database-managed-instance.md). 
- Zie voor informatie over de prijzen [prijzen van SQL Database Managed Instance](https://azure.microsoft.com/pricing/details/sql-database/managed/).
- Zie voor meer informatie over het maken van uw eerste Managed Instance, [snelstartgids](sql-database-managed-instance-get-started.md).