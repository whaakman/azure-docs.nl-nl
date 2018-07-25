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
ms.openlocfilehash: d1dd62d06c7e3ed634795604ce9660694ea073ca
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39239879"
---
# <a name="integrate-azure-active-directory-logs-with-splunk-using-azure-monitor-preview"></a>Logboeken van Azure Active Directory integreren met Splunk met behulp van Azure Monitor (preview)

In dit artikel leert u hoe u Azure Active Directory-logboeken integreren met Splunk met behulp van Azure Monitor. U moet eerst de logboeken te routeren naar een Azure event hub, en vervolgens deze integreren met Splunk.

## <a name="prerequisites"></a>Vereisten

1. Azure Event Hub met de Azure AD-activiteit zich aanmeldt. Meer informatie over het [uw activiteitenlogboeken streamen naar Event Hub streamen](reporting-azure-monitor-diagnostics-azure-event-hub.md). 
2. Gebruik de volgende [instructies](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md) de invoegtoepassing Azure monitor voor Splunk downloaden en configureren uw Splunk-exemplaar.

## <a name="tutorial"></a>Zelfstudie 

1. Open uw Splunk-exemplaar en klik op **gegevenssamenvatting**.
    ![Gegevens beknopte](./media/reporting-azure-monitor-diagnostics-splunk-integration/DataSummary.png "samenvatting van gegevens")

2. Navigeer naar de **Sourcetypes** tabblad en selecteer **amal: aadal:audit** ![Sourcetypes tabblad](./media/reporting-azure-monitor-diagnostics-splunk-integration/sourcetypeaadal.png "Sourcetypes tabblad")

3. Ziet u de Azure AD-activiteitenlogboeken zoals wordt weergegeven in de volgende afbeelding.
    ![Activiteitenlogboeken](./media/reporting-azure-monitor-diagnostics-splunk-integration/activitylogs.png "activiteitenlogboeken")

> [!NOTE]
> Als u niet een invoegtoepassing installeren in uw exemplaar Splunk (bijvoorbeeld als u een proxy of die worden uitgevoerd op Splunk Cloud), kunt u deze gebeurtenissen doorsturen naar Splunk HTTP Event Collector met behulp van dit [Azure-functie die wordt geactiveerd door nieuwe berichten in de Event hub](https://github.com/Microsoft/AzureFunctionforSplunkVS). " 
>

## <a name="next-steps"></a>Volgende stappen

* [Audit logs schema in Azure monitor interpreteren](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Schema-in Logboeken in Azure monitor interpreteren](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)
* [Veelgestelde vragen en bekende problemen](reporting-azure-monitor-diagnostics-overview.md#frequently-asked-questions)