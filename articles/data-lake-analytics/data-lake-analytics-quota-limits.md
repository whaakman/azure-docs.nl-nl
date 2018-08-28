---
title: Quota en limieten in Azure Data Lake Analytics aanpassen
description: Informatie over het aanpassen en het verhogen van quota en limieten in Azure Data Lake Analytics (ADLA)-accounts.
services: data-lake-analytics
ms.service: data-lake-analytics
author: omidm1
ms.author: omidm
ms.reviewer: jasonwhowell
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.topic: conceptual
ms.date: 03/15/2018
ms.openlocfilehash: 4629b52f3b2c9e351ddc2a68a40c5178a9a73950
ms.sourcegitcommit: 161d268ae63c7ace3082fc4fad732af61c55c949
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/27/2018
ms.locfileid: "43048252"
---
# <a name="adjust-quotas-and-limits-in-azure-data-lake-analytics"></a>Quota en limieten in Azure Data Lake Analytics aanpassen

Informatie over het aanpassen en het verhogen van quota en limieten in Azure Data Lake Analytics (ADLA)-accounts. Deze limieten te weten, kunt u meer inzicht in uw U-SQL-taak gedrag. Alle quotalimieten zijn zachte, zodat u de maximale limieten verhogen kunt contact opnemen met de ondersteuning van Azure.

## <a name="azure-subscriptions-limits"></a>Limieten voor Azure-abonnementen

**Maximum aantal ADLA-accounts per abonnement per regio:** 5

Als u probeert een zesde ADLA-account maakt, wordt u er een foutbericht "U hebt het maximum aantal Data Lake Analytics-accounts toegestaan (5) in het gebied onder de naam van abonnement bereikt". 

Als u verder gaan dan deze limiet wilt, kunt u deze opties proberen:
* Kies een andere regio als geschikt
* Neem contact op met ondersteuning van Azure door [een ondersteuningsticket openen](#increase-maximum-quota-limits) om aan te vragen een quotaverhoging.

## <a name="default-adla-account-limits"></a>Standaardlimieten ADLA-account

**Maximum aantal eenheden van de analyse (AU's) per account:** 32

Dit is het maximum aantal AU's mogen worden uitgevoerd in uw account. Als het totale aantal AU's uitgevoerd in alle projecten deze limiet overschrijdt, nieuwe taken in de wachtrij automatisch. Bijvoorbeeld:

* Als u slechts één taak hebt uitgevoerd met 32 AU's, wanneer u een tweede verzendt taak deze in de taakwachtrij wacht totdat de eerste taak is voltooid.
* Als u al vier taken die worden uitgevoerd en wordt elke 8 gebruikt AU's, wanneer u een vijfde taak die 8 moet verzenden AU's dat deze in de taakwachtrij moet wachten totdat er 8 AU's beschikbaar.

**Maximum aantal eenheden van de analyse (AU's) per taak:** 32

Dit is de standaard maximale aantal AU's dat elke afzonderlijke taak kan worden toegewezen in uw account. Taken die meer dan deze limiet zijn toegewezen, worden geweigerd, tenzij de indiener wordt beïnvloed door een compute-beleid (limiet voor het indienen van taak) waarmee ze meer AU's per taak. De bovengrens van deze waarde is de AU-limiet voor het account.

**Maximum aantal gelijktijdige U-SQL-taken per account:** 20

Dit is het maximale aantal taken die gelijktijdig kunnen worden uitgevoerd in uw account. Boven deze waarde hoger taken worden in de wachtrij automatisch.

## <a name="adjust-adla-account-limits"></a>Limieten ADLA aanpassen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Kies een bestaande ADLA-account.
3. Klik op **Eigenschappen**.
4. Wijzig de waarden voor **Maximum AU's**, **maximumaantal actieve taken**, en **limieten voor het verzenden van taken** op basis van uw behoeften.

## <a name="increase-maximum-quota-limits"></a>Maximale quotalimieten verhogen

U vindt meer informatie over Azure-limieten in de [documentatie voor Azure-servicespecifieke-limieten](../azure-subscription-service-limits.md#data-lake-analytics-limits).

1. Open een ondersteuningsaanvraag in Azure portal.

    ![Azure Data Lake Analytics-portal-pagina](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics-portal-pagina](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Selecteer het probleemtype **quotum**.
3. Selecteer uw **abonnement** (Zorg ervoor dat het is niet een "" proefabonnement).
4. Selecteer quotumtype **Data Lake Analytics**.

    ![Azure Data Lake Analytics-portal-pagina](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. In de pagina van het probleem, leggen uit uw limiet voor de aangevraagde verhoging met **Details** van waarom u deze extra capaciteit nodig hebt.

    ![Azure Data Lake Analytics-portal-pagina](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Controleer of uw contactgegevens en de ondersteuningsaanvraag maken.

Microsoft uw aanvraag beoordeelt en probeert zo snel mogelijk aan de behoeften van uw bedrijf.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics beheren met Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Azure Data Lake Analytics-taken bewaken en problemen oplossen met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
