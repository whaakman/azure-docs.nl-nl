---
title: Over het integreren van Azure Active Directory-logboeken met Splunk met behulp van Azure Monitor (preview) | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-logboeken met Splunk met behulp van Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: c4b605b6-6fc0-40dc-bd49-101d03f34665
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: d12d220b7c648fed8fd2f0229b57a29fc775ff5e
ms.sourcegitcommit: 98645e63f657ffa2cc42f52fea911b1cdcd56453
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/23/2019
ms.locfileid: "54809867"
---
# <a name="integrate-azure-ad-logs-with-splunk-using-azure-monitor-preview"></a>Azure AD-logboeken integreren met Splunk met behulp van Azure Monitor (preview)

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
