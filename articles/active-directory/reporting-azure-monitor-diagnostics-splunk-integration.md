---
title: Over het integreren van Azure Active Directory-logboeken met Splunk met behulp van Azure Monitor (preview) | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-logboeken met Splunk met behulp van Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 42dbb8c7e74bd3acb99028477f34f99f1334d577
ms.sourcegitcommit: 9222063a6a44d4414720560a1265ee935c73f49e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2018
ms.locfileid: "39503839"
---
# <a name="integrate-azure-ad-logs-with-splunk-by-using-azure-monitor-preview"></a>Azure AD-logboeken integreren met Splunk met behulp van Azure Monitor (preview)

In dit artikel leert u hoe u logboeken van Azure Active Directory (Azure AD) met Splunk integreren met behulp van Azure Monitor. U eerst de logboeken versturen naar een Azure event hub, en vervolgens het integreren van de event hub met Splunk.

## <a name="prerequisites"></a>Vereiste onderdelen

U hebt het volgende nodig om deze functie te gebruiken:
* Een Azure event hub met Azure AD-activiteit zich aanmeldt. Meer informatie over het [uw activiteitenlogboeken streamen naar een event hub streamen](reporting-azure-monitor-diagnostics-azure-event-hub.md). 
* De Azure Monitor-invoegtoepassing voor Splunk. [Downloaden en configureren uw exemplaar Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="tutorial"></a>Zelfstudie 

1. Open uw Splunk-exemplaar en selecteer **gegevenssamenvatting**.

    ![De knop 'Data-Samenvatting'](./media/reporting-azure-monitor-diagnostics-splunk-integration/DataSummary.png)

2. Selecteer de **Sourcetypes** tabblad, en selecteer vervolgens **amal: aadal:audit**

    ![Het tabblad gegevens samenvatting Sourcetypes](./media/reporting-azure-monitor-diagnostics-splunk-integration/sourcetypeaadal.png)

    De Azure AD-activiteit logboeken worden weergegeven in de volgende afbeelding:

    ![Activiteitenlogboeken](./media/reporting-azure-monitor-diagnostics-splunk-integration/activitylogs.png)

> [!NOTE]
> Als u niet een invoegtoepassing installeren in uw exemplaar Splunk (bijvoorbeeld als u een proxy of die worden uitgevoerd op Splunk Cloud), kunt u deze gebeurtenissen doorsturen naar Splunk HTTP Event Collector. Gebruik deze om dit te doen, [Azure-functie](https://github.com/Microsoft/AzureFunctionforSplunkVS), die wordt geactiveerd door nieuwe berichten in de event hub. 
>

## <a name="next-steps"></a>Volgende stappen

* [Audit logs schema in Azure Monitor interpreteren](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Schema-in Logboeken in Azure Monitor interpreteren](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Veelgestelde vragen en bekende problemen](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)
