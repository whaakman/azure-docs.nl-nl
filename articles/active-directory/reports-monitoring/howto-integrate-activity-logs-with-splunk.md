---
title: Logboeken van Azure Active Directory integreren met Splunk met behulp van Azure Monitor | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-logboeken met SumoLogic met behulp van Azure Monitor
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 05/13/2019
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 8f30e19a351f7b25f995a85cfd566bcba091ac27
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "65597819"
---
# <a name="how-to-integrate-azure-active-directory-logs-with-splunk-using-azure-monitor"></a>Procedure: Logboeken van Azure Active Directory integreren met Splunk met behulp van Azure Monitor

In dit artikel leert u hoe u logboeken van Azure Active Directory (Azure AD) met Splunk integreren met behulp van Azure Monitor. U eerst de logboeken versturen naar een Azure event hub, en vervolgens het integreren van de event hub met Splunk.

## <a name="prerequisites"></a>Vereisten

U hebt het volgende nodig om deze functie te gebruiken:
* Een Azure event hub met Azure AD-activiteit zich aanmeldt. Meer informatie over het [uw activiteitenlogboeken streamen naar een event hub streamen](quickstart-azure-monitor-stream-logs-to-event-hub.md). 
* De Azure Monitor-invoegtoepassing voor Splunk. [Downloaden en configureren uw exemplaar Splunk](https://github.com/Microsoft/AzureMonitorAddonForSplunk/blob/master/README.md).

## <a name="integrate-azure-active-directory-logs"></a>Logboeken van Azure Active Directory integreren 

1. Open uw Splunk-exemplaar en selecteer **gegevenssamenvatting**.

    ![De knop 'Data-Samenvatting'](./media/howto-integrate-activity-logs-with-splunk/DataSummary.png)

2. Selecteer de **Sourcetypes** tabblad, en selecteer vervolgens **amal: aadal:audit**

    ![Het tabblad gegevens samenvatting Sourcetypes](./media/howto-integrate-activity-logs-with-splunk/sourcetypeaadal.png)

    De Azure AD-activiteit logboeken worden weergegeven in de volgende afbeelding:

    ![Activiteitenlogboeken](./media/howto-integrate-activity-logs-with-splunk/activitylogs.png)

> [!NOTE]
> Als u niet een invoegtoepassing installeren in uw exemplaar Splunk (bijvoorbeeld als u een proxy of die worden uitgevoerd op Splunk Cloud), kunt u deze gebeurtenissen doorsturen naar Splunk HTTP Event Collector. Gebruik deze om dit te doen, [Azure-functie](https://github.com/Microsoft/AzureFunctionforSplunkVS), die wordt geactiveerd door nieuwe berichten in de event hub. 
>

## <a name="next-steps"></a>Volgende stappen

* [Interpret audit logs schema in Azure Monitor](reference-azure-monitor-audit-log-schema.md) (Auditlogboekenschema interpreteren in Azure Monitor)
* [Interpret sign-in logs schema in Azure Monitor](reference-azure-monitor-sign-ins-log-schema.md) (Aanmeldingslogboekenschema interpreteren in Azure Monitor)
* [Veelgestelde vragen en bekende problemen](concept-activity-logs-azure-monitor.md#frequently-asked-questions)