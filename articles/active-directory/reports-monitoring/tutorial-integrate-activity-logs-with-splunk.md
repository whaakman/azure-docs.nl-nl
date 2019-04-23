---
title: Azure Active Directory-logboeken naar Splunk met behulp van Azure Monitor Stream | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-logboeken met Splunk met behulp van Azure Monitor
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 04/18/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 70befad3208f34fe62fbb0a59cea4bf6ea01ce16
ms.sourcegitcommit: bf509e05e4b1dc5553b4483dfcc2221055fa80f2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/22/2019
ms.locfileid: "59999515"
---
# <a name="integrate-azure-ad-logs-with-splunk-using-azure-monitor"></a>Azure AD-logboeken integreren met Splunk met behulp van Azure Monitor

In dit artikel leert u hoe u logboeken van Azure Active Directory (Azure AD) met Splunk integreren met behulp van Azure Monitor. U eerst de logboeken versturen naar een Azure event hub, en vervolgens het integreren van de event hub met Splunk.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:
* Een Azure event hub met Azure AD-activiteit zich aanmeldt. Meer informatie over het [uw activiteitenlogboeken streamen naar een event hub streamen](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* De Azure Monitor-invoegtoepassing voor Splunk. [Downloaden en configureren uw exemplaar Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="tutorial"></a>Zelfstudie 

1. Open uw Splunk-exemplaar en selecteer **gegevenssamenvatting**.

    ![De knop 'Data-Samenvatting'](./media/tutorial-integrate-activity-logs-with-splunk/DataSummary.png)

2. Selecteer de **Sourcetypes** tabblad, en selecteer vervolgens **amal: aadal:audit**

    ![Het tabblad gegevens samenvatting Sourcetypes](./media/tutorial-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    De Azure AD-activiteit logboeken worden weergegeven in de volgende afbeelding:

    ![Activiteitenlogboeken](./media/tutorial-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> Als u niet een invoegtoepassing installeren in uw exemplaar Splunk (bijvoorbeeld als u een proxy of die worden uitgevoerd op Splunk Cloud), kunt u deze gebeurtenissen doorsturen naar Splunk HTTP Event Collector. Gebruik deze om dit te doen, [Azure-functie](https://github.com/Microsoft/AzureFunctionforSplunkVS), die wordt geactiveerd door nieuwe berichten in de event hub. 
>

## <a name="next-steps"></a>Volgende stappen

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Auditlogboekenschema interpreteren in Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)
* [Veelgestelde vragen en bekende problemen](concept-activity-logs-azure-monitor.md#frequently-asked-questions)