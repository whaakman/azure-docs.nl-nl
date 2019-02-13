---
title: Stream van Azure Active Directory-logboeken naar Log Analytics met Azure Monitor (preview) | Microsoft Docs
description: Meer informatie over het integreren van Azure Active Directory-Logboeken in Log Analytics met behulp van Azure Monitor (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: daveba
editor: ''
ms.assetid: 2c3db9a8-50fa-475a-97d8-f31082af6593
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
ms.openlocfilehash: 51db96523a96015822f4507731bad2a398521530
ms.sourcegitcommit: 301128ea7d883d432720c64238b0d28ebe9aed59
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/13/2019
ms.locfileid: "56165838"
---
# <a name="integrate-azure-ad-logs-with-log-analytics-using-azure-monitor-preview"></a>Azure AD-logboeken integreren met Log Analytics met Azure Monitor (preview)

Log Analytics kunt u om gegevens te doorzoeken op specifieke gebeurtenissen te zoeken, trends te analyseren en voert u de correlatie over verschillende gegevensbronnen. Met de integratie van Azure AD activiteitenlogboeken in Log Analytics, kunt u taken, zoals nu uitvoeren:

 * Vergelijk uw Azure AD aanmelden logboeken tegen beveiligingslogboeken gepubliceerd door Azure Security Center

 * Problemen met knelpunten op de aanmeldingspagina van uw toepassing door de toepassing de prestatiegegevens van Azure Application Insights.  

De volgende video van een Ignite-sessie ziet u de voordelen van het gebruik van Log Analytics voor Azure AD-Logboeken in scenario's met praktische gebruiker.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

In dit artikel leert u hoe u Azure Active Directory (Azure AD)-logboeken integreren met Log Analytics met Azure Monitor.

## <a name="supported-reports"></a>Ondersteunde rapporten

U kunt auditlogboeken voor de activiteit en aanmeldingsactiviteiten logboeken versturen naar Log Analytics voor verdere analyse. 

* **Auditlogboeken**: De [activiteit audittrailrapporten](concept-audit-logs.md) krijgt u toegang tot de geschiedenis van elke taak die wordt uitgevoerd in uw tenant.
* **Meld u in logboeken**: Met de [rapport van aanmeldingsactiviteiten](concept-sign-ins.md), kunt u bepalen wie de taken die worden gerapporteerd in de auditlogboeken uitgevoerd.

> [!NOTE]
> Auditlogboeken en aanmeldingslogboeken met betrekking tot B2C worden momenteel niet ondersteund.
>

## <a name="prerequisites"></a>Vereisten 

U hebt het volgende nodig om deze functie te gebruiken:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een Azure AD-tenant.
* Een gebruiker die een *globale beheerder* of *beveiligingsbeheerder* voor de Azure-tenant is.
* Een Log Analytics-werkruimte in uw Azure-abonnement. Meer informatie over het [een Log Analytics-werkruimte maken](https://docs.microsoft.com/azure/log-analytics/log-analytics-quick-create-workspace).

## <a name="send-logs-to-log-analytics"></a>Logboeken naar Log Analytics verzenden

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer **Azure Active Directory** > **diagnostische instellingen** -> **diagnostische instelling toevoegen**. U kunt ook selecteren **instellingen exporteren** uit de **auditlogboeken** of **aanmeldingen** pagina om te gaan naar de configuratiepagina van de diagnostische instellingen.  
    
3. In de **diagnostische instellingen** in het menu het **verzenden naar Log Analytics** selectievakje en selecteer vervolgens **configureren**.

4. Selecteer de Log Analytics-werkruimte die u wilt de logboeken te verzenden, of een nieuwe werkruimte maken in het dialoogvenster.  

5. Voer een van de volgende bewerkingen uit, of beide:
    * Voor het verzenden van Logboeken voor controle naar de Log Analytics-werkruimte, selecteert u de **AuditLogs** selectievakje. 
    * Als u wilt aanmelden logboeken verzenden naar de Log Analytics-werkruimte, selecteert u de **SignInLogs** selectievakje.

6. Selecteer **Opslaan** om de instelling op te slaan.

    ![Diagnostische instellingen](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Controleer na ongeveer 15 minuten, of dat gebeurtenissen worden gestreamd naar uw Log Analytics-werkruimte.

## <a name="next-steps"></a>Volgende stappen

* [Analyseren van Azure AD-activiteitenlogboeken in Log Analytics](howto-analyze-activity-logs-log-analytics.md)
* [Installeren en de Log Analytics-weergaven gebruiken voor Azure Active Directory](howto-install-use-log-analytics-views.md)
