---
title: De quotalimieten voor Azure Data Lake Analytics | Microsoft Docs
description: Informatie over het aanpassen en het verhogen van de quotumlimieten in Azure Data Lake Analytics (ADLA)-accounts.
services: data-lake-analytics
keywords: Azure Data Lake Analytics
documentationcenter: 
author: omidm1
editor: omidm1
ms.assetid: 49416f38-fcc7-476f-a55e-d67f3f9c1d34
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/18/2017
ms.author: omidm
ms.openlocfilehash: 957f306ea0e80b5830ad64e5ef06c6d122d9eccc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="azure-data-lake-analytics-quota-limits"></a>De quotalimieten voor Azure Data Lake Analytics

Informatie over het aanpassen en het verhogen van de quotumlimieten in Azure Data Lake Analytics (ADLA)-accounts. Deze limieten weten, kunt u meer inzicht in uw U-SQL-taak gedrag. Alle quotalimieten zijn zachte, kunt u de maximale limieten verhogen bereiken uit voor ons.

## <a name="azure-subscriptions-limits"></a>Limieten voor Azure-abonnementen

**Maximum aantal ADLA accounts per abonnement:** 5

 Dit is het maximum aantal ADLA accounts die u per abonnement maken kunt. Als u probeert te maken van een zesde ADLA-account, krijgt u een fout "U hebt het maximum aantal Data Lake Analytics-accounts toegestaan (5) in de regio onder de abonnementsnaam van het bereikt". In dit geval Verwijder ongebruikte ADLA accounts of bereiken aan ons door [een ondersteuningsticket openen](#increase-maximum-quota-limits).

## <a name="adla-account-limits"></a>Limieten van ADLA

**Maximum aantal eenheden Analytics (AUs) per account:** 250

Dit is het maximum aantal AUs die tegelijkertijd kunnen worden uitgevoerd in uw account. Als de totale AUs uitvoeren voor alle taken deze limiet overschrijdt, nieuwere taken in de wachtrij automatisch. Bijvoorbeeld:

* Als u slechts één taak hebt uitgevoerd met 250 AUs, wanneer u een tweede indient taak deze in de taakwachtrij wacht totdat de eerste taak is voltooid.
* Als u hebt al vijf taken worden uitgevoerd en elk van 50 gebruikmaakt AUs, wanneer u een zesde taak die 20 moet stuurt deze in de taakwachtrij wacht totdat er 20 AUs AUs beschikbaar.

**Maximum aantal gelijktijdige U-SQL-taken per account:** 20

Dit is het maximum aantal taken die tegelijkertijd kunnen worden uitgevoerd in uw account. Boven deze waarde nieuwere taken in de wachtrij automatisch.

## <a name="adjust-adla-quota-limits-per-account"></a>De quotalimieten ADLA per account aanpassen

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
2. Kies een bestaand ADLA-account.
3. Klik op **Eigenschappen**.
4. Aanpassen **parallelle uitvoering** en **gelijktijdige taken** aan uw behoeften.

    ![Azure Data Lake Analytics-portalblade](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-properties.png)

## <a name="increase-maximum-quota-limits"></a>Maximumquotum limieten verhogen

1. Open een ondersteuningsaanvraag in Azure-Portal.

    ![Azure Data Lake Analytics-portalblade](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-help-support.png)

    ![Azure Data Lake Analytics-portalblade](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request.png)
2. Selecteer het type probleem **quotum**.
3. Selecteer uw **abonnement** (Zorg ervoor dat het is niet een ' ' proefabonnement).
4. Selecteer quotatype **Data Lake Analytics**.

    ![Azure Data Lake Analytics-portalblade](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-basics.png)

5. In de blade probleem uitgelegd uw limiet aangevraagde verhoging met **Details** van waarom u deze extra capaciteit nodig hebt.

    ![Azure Data Lake Analytics-portalblade](./media/data-lake-analytics-quota-limits/data-lake-analytics-quota-support-request-details.png)

6. Uw contactgegevens controleren en maken van de ondersteuningsaanvraag.

Microsoft uw aanvraag beoordeelt en probeert zo snel mogelijk aan de behoeften van uw bedrijf.

## <a name="next-steps"></a>Volgende stappen

* [Overzicht van Microsoft Azure Data Lake Analytics](data-lake-analytics-overview.md)
* [Azure Data Lake Analytics beheren met Azure PowerShell](data-lake-analytics-manage-use-powershell.md)
* [Azure Data Lake Analytics-taken bewaken en problemen oplossen met Azure Portal](data-lake-analytics-monitor-and-troubleshoot-jobs-tutorial.md)
