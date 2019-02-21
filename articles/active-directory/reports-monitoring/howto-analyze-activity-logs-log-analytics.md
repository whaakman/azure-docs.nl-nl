---
title: Analyseren van activiteitenlogboeken van Azure Active Directory met behulp van Azure Monitor-Logboeken (preview) | Microsoft Docs
description: Informatie over het analyseren van activiteitenlogboeken van Azure Active Directory met behulp van Azure Monitor-Logboeken (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 4535ae65-8591-41ba-9a7d-b7f00c574426
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.subservice: report-monitor
ms.date: 11/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: e2e565f5b9bcd9e3e79423c742b2c95c00abd97b
ms.sourcegitcommit: 75fef8147209a1dcdc7573c4a6a90f0151a12e17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/20/2019
ms.locfileid: "56454755"
---
# <a name="analyze-azure-ad-activity-logs-with-azure-monitor-logs-preview"></a>Analyseren van Azure AD-activiteitenlogboeken met Azure Monitor-Logboeken (preview)

Nadat u [integreren van Azure AD-activiteitenlogboeken met Azure Monitor logboeken](howto-integrate-activity-logs-with-log-analytics.md), kunt u de kracht van Azure Monitor-logboeken voor inzicht in uw omgeving. U kunt ook installeren de [analytics weergaven voor Azure AD-activiteit zich logboeken](howto-install-use-log-analytics-views.md) voor toegang tot vooraf gemaakte rapporten met betrekking tot controle en aanmeldingsgebeurtenissen in uw omgeving.

In dit artikel leert u hoe u voor het analyseren van de Azure AD-activiteitenlogboeken in uw Log Analytics-werkruimte. 

## <a name="prerequisites"></a>Vereisten 

Als u wilt volgen, hebt u het volgende nodig:

* Een Log Analytics-werkruimte in uw Azure-abonnement. Meer informatie over het [een Log Analytics-werkruimte maken](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* De stappen voor het eerst voltooid [route de Azure AD-activiteitenlogboeken aan uw Log Analytics-werkruimte](howto-integrate-activity-logs-with-log-analytics.md).

## <a name="navigate-to-the-log-analytics-workspace"></a>Navigeer naar de Log Analytics-werkruimte

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer **Azure Active Directory**, en selecteer vervolgens **logboeken** uit de **bewaking** sectie om uw Log Analytics-werkruimte te openen. De werkruimte wordt geopend met een standaardquery.

    ![Standaardquery](./media/howto-analyze-activity-logs-log-analytics/defaultquery.png)


## <a name="view-the-schema-for-azure-ad-activity-logs"></a>Het schema voor Azure AD-activiteit bekijken Logboeken

De logboeken worden gepusht naar de **AuditLogs** en **SigninLogs** tabellen in de werkruimte. Het schema voor deze tabellen weergeven:

1. Selecteer in de weergave van de query standaard in de vorige sectie **Schema** en vouwt u de werkruimte. 

2. Vouw de **Log Management** sectie en vouw vervolgens ofwel **AuditLogs** of **SignInLogs** om het schema van het logboek weer te geven.
    ![Auditlogboeken](./media/howto-analyze-activity-logs-log-analytics/auditlogschema.png) ![aanmelding-Logboeken](./media/howto-analyze-activity-logs-log-analytics/signinlogschema.png)

## <a name="query-the-azure-ad-activity-logs"></a>Query de Azure AD-activiteitenlogboeken

Nu dat u de logboeken in uw werkruimte hebt, kunt u nu query's uitvoeren met betrekking tot deze. Bijvoorbeeld als u de eerste toepassingen in de afgelopen week gebruikt, vervangt u de standaardquery in met de volgende en selecteert u **uitvoeren**

```
SigninLogs 
| where CreatedDateTime >= ago(7d)
| summarize signInCount = count() by AppDisplayName 
| sort by signInCount desc 
```

Als u de bovenste controlegebeurtenissen in de afgelopen week, gebruikt u de volgende query uit:

```
AuditLogs 
| where TimeGenerated >= ago(7d)
| summarize auditCount = count() by OperationName 
| sort by auditCount desc 
```
## <a name="alert-on-azure-ad-activity-log-data"></a>Waarschuwing op basis van gegevens van Azure AD een activiteitenlogboek

U kunt ook waarschuwingen instellen voor uw query. Bijvoorbeeld, als u wilt configureren een waarschuwing wanneer meer dan 10 zijn toepassingen gebruikt in de afgelopen week:

1. Selecteer in de werkruimte **waarschuwing instellen** openen de **maken regel** pagina.

    ![waarschuwing instellen](./media/howto-analyze-activity-logs-log-analytics/setalert.png)

2. Selecteer de standaard **waarschuwen criteria** gemaakt in de waarschuwing en werk de **drempelwaarde** in de metrische standaardgegevens tot en met 10.

    ![Waarschuwingscriteria](./media/howto-analyze-activity-logs-log-analytics/alertcriteria.png)

3. Voer een naam en beschrijving voor de waarschuwing en kies het ernstniveau. In ons voorbeeld kunt we deze instellen op **ter informatie**.

4. Selecteer de **actiegroep** die wordt gewaarschuwd wanneer het signaal dat zich voordoet. U kunt op de hoogte stellen van uw team via e-mail of SMS-bericht of u de actie met behulp van webhooks, Azure functions of logic apps kan automatiseren. Meer informatie over [maken en beheren van groepen in Azure portal waarschuwen](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

5. Nadat u de waarschuwing hebt geconfigureerd, selecteer **maken waarschuwing** inschakelen. 

## <a name="install-and-use-pre-built-views-for-azure-ad-activity-logs"></a>Installeren en gebruiken van vooraf gemaakte weergaven voor Azure AD-activiteit Logboeken

U kunt ook de weergaven van de vooraf gemaakte log analytics voor Azure AD-activiteit downloaden Logboeken. De weergaven bieden verschillende rapporten met betrekking tot algemene scenario's met betrekking tot controle en aanmeldingsgebeurtenissen. U kunt ook met de waarschuwing bij een van de gegevens die zijn opgegeven in de rapporten met behulp van de stappen die worden beschreven in de vorige sectie.

* **Azure AD-Account ingericht gebeurtenissen**: Deze weergave toont rapporten met betrekking tot het controleren van inrichting activiteit, zoals het aantal nieuwe gebruikers die zijn ingericht en inrichtingsfouten, aantal gebruikers bijgewerkt en bijwerken van fouten en het aantal gebruikers niet ingericht en bijbehorende fouten.    
* **Aanmeldingen gebeurtenissen**: Deze weergave toont de meest relevante rapporten met betrekking tot het controleren van de aanmelding, activiteit, zoals aanmeldingen per toepassing, gebruiker, apparaat, evenals een samenvatting weergegeven voor het bijhouden van het aantal aanmeldingen na verloop van tijd.
* **Gebruikers uitvoeren van toestemming**: Deze weergave toont rapporten met betrekking tot toestemming van de gebruiker, zoals de toestemming verleent door de gebruiker, -aanmeldingen door gebruikers die toestemming verleend, evenals aanmeldingen per toepassing voor alle toepassingen op basis van toestemming. 

Meer informatie over het [installeren en gebruiken van log analytics-weergaven voor Azure AD-activiteit logboeken](howto-install-use-log-analytics-views.md). 


## <a name="next-steps"></a>Volgende stappen

* [Aan de slag met query's in Azure Monitor-Logboeken](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-queries)
* [Waarschuwing groepen in Azure portal maken en beheren](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups)
* [Installeren en de log analytics-weergaven gebruiken voor Azure Active Directory](howto-install-use-log-analytics-views.md)
