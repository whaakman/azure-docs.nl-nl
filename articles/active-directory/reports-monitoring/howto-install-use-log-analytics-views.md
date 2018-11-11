---
title: Installeren en de Log Analytics-weergaven gebruiken voor Azure Active Directory (preview) | Microsoft Docs
description: Meer informatie over het installeren en de Log Analytics-weergaven gebruiken voor Azure Active Directory (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 2290de3c-2858-4da0-b4ca-a00107702e26
ms.service: active-directory
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: report-monitor
ms.date: 10/01/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: 11cc6a69b8072fca0639da5e517a39c22645710e
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300503"
---
# <a name="install-and-use-the-log-analytics-views-for-azure-active-directory"></a>Installeren en de Log Analytics-weergaven gebruiken voor Azure Active Directory

De weergaven van de Azure Active Directory Log Analytics helpt u bij het analyseren en zoek de Azure AD-activiteitenlogboeken in uw Azure AD-tenant. Azure AD-activiteit logboeken bevatten:

* Auditlogboeken: de [activiteit audittrailrapporten](concept-audit-logs.md) krijgt u toegang tot de geschiedenis van elke taak die wordt uitgevoerd in uw tenant.
* Logboeken voor aanmelden: met de [rapport van aanmeldingsactiviteiten](concept-sign-ins.md), kunt u bepalen wie de taken die worden gerapporteerd in de auditlogboeken uitgevoerd.

## <a name="prerequisites"></a>Vereisten

Voor de Log Analytics-weergaven, hebt u het volgende nodig:

* Een Log Analytics-werkruimte in uw Azure-abonnement. Meer informatie over het [een Log Analytics-werkruimte maken](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).
* De stappen voor het eerst voltooid [route de Azure AD-activiteitenlogboeken aan uw Log Analytics-werkruimte](howto-integrate-activity-logs-with-log-analytics.md).
* Downloaden van de weergaven van de [GitHub-opslagplaats](https://aka.ms/AADLogAnalyticsviews) naar uw lokale computer.

## <a name="install-the-log-analytics-views"></a>Installeren van de Log Analytics-weergaven

1. Navigeer naar uw Log Analytics-werkruimte. Om dit te doen, en Ga eerst naar de [Azure-portal](https://portal.azure.com) en selecteer **alle services**. Type **Log Analytics** in het tekstvak in en selecteer **Log Analytics**. Selecteer de werkruimte die u de activiteitenlogboeken, doorgestuurd als onderdeel van de vereisten.
2. Selecteer **Weergaveontwerper**, selecteer **importeren** en selecteer vervolgens **bestand kiezen** voor het importeren van de weergaven van uw lokale computer.
3. Selecteer de weergaven die u hebt gedownload van de vereisten en selecteer **opslaan** om op te slaan van het importeren. Doe dit voor de **Azure AD-Account inrichten gebeurtenissen** weergeven en de **aanmeldingen gebeurtenissen** weergeven.

## <a name="use-the-views"></a>Gebruik de weergaven

1. Navigeer naar uw Log Analytics-werkruimte. Om dit te doen, en Ga eerst naar de [Azure-portal](https://portal.azure.com) en selecteer **alle services**. Type **Log Analytics** in het tekstvak in en selecteer **Log Analytics**. Selecteer de werkruimte die u de activiteitenlogboeken, doorgestuurd als onderdeel van de vereisten.

2. Bent u in de werkruimte, selecteert u **werkruimte overzicht**. Hier ziet u de volgende drie weergaven:

    * **Azure AD-Account wordt ingericht gebeurtenissen**: deze weergave toont rapporten met betrekking tot het controleren van inrichting activiteit, zoals het aantal nieuwe gebruikers die zijn ingericht en inrichtingsfouten, aantal gebruikers bijgewerkt en fouten en het aantal gebruikers bijwerken verlopen en bijbehorende fouten.    
    * **Aanmeldingen gebeurtenissen**: in deze weergave ziet u de meest relevante rapporten met betrekking tot het controleren van de aanmelding, activiteit, zoals aanmeldingen per toepassing, gebruiker, apparaat, evenals een samenvatting weergegeven voor het bijhouden van het aantal aanmeldingen na verloop van tijd.

3. Selecteer een van deze weergaven om door te gaan met de afzonderlijke rapporten. U kunt ook waarschuwingen instellen op een van de rapportparameters. Bijvoorbeeld, stel een waarschuwing voor telkens wanneer er een fout aanmelden is. Om dit te doen, selecteert u eerst de **aanmeldingen gebeurtenissen** weergave, selecteer **aanmelden fouten na verloop van tijd** rapport en selecteer vervolgens **Analytics** openen van de pagina met details met de werkelijke query achter het rapport. 

    ![Details](./media/howto-install-use-log-analytics-views/details.png)


4. Selecteer **waarschuwing instellen**, en selecteer vervolgens **wanneer het aangepaste zoeken in Logboeken is &lt;logica niet gedefinieerd&gt;**  onder de **waarschuwen criteria** sectie. Omdat we een waarschuwing wanneer er sprake is van een aanmeldingsfout wilt, stelt de **drempelwaarde** van de standaard-waarschuwingslogica naar **1** en selecteer vervolgens **gedaan**. 

    ![Signaallogica configureren](./media/howto-install-use-log-analytics-views/configure-signal-logic.png)

5. Voer een naam en beschrijving voor de waarschuwing en de ernst ingesteld op **waarschuwing**.

    ![Regel maken](./media/howto-install-use-log-analytics-views/create-rule.png)

6. Selecteer de actie bij waarschuwing. In het algemeen is dit een van beide een team dat u wilt een melding via e-mail of SMS-bericht kan zijn of mag een geautomatiseerde taak met behulp van webhooks, runbooks, functions, logic apps of externe ITSM-oplossingen. Meer informatie over het [maken en beheren van actiegroepen in Azure portal](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-action-groups).

7. Selecteer **waarschuwingsregel maken** om de waarschuwing te maken. Nu wordt u gewaarschuwd wanneer er een fout aanmelden is.

## <a name="next-steps"></a>Volgende stappen

* [Over het analyseren van activiteit-Logboeken in Log Analytics](howto-analyze-activity-logs-log-analytics.md)
* [Aan de slag met Log Analytics in Azure portal](https://docs.microsoft.com/azure/log-analytics/query-language/get-started-analytics-portal)
