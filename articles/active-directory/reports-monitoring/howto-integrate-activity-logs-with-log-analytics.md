---
title: Stream logboeken van Azure Active Directory naar Azure Monitor-Logboeken (preview) | Microsoft Docs
description: Meer informatie over het Azure Active Directory-logboeken integreren met Azure Monitor-Logboeken (preview)
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
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 056a529101ffc39170ea057832bcd50b283505be
ms.sourcegitcommit: 70550d278cda4355adffe9c66d920919448b0c34
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/26/2019
ms.locfileid: "58436266"
---
# <a name="integrate-azure-ad-logs-with-azure-monitor-logs-preview"></a>Azure AD-logboeken integreren met Azure Monitor-Logboeken (preview)

Logboeken in Azure Monitor kunt u om gegevens te doorzoeken op specifieke gebeurtenissen te zoeken, trends te analyseren en voert u de correlatie over verschillende gegevensbronnen. Met de integratie van Azure AD activiteitenlogboeken in Azure Monitor-Logboeken, kunt u taken, zoals nu uitvoeren:

 * Vergelijk uw Azure AD aanmelden logboeken tegen beveiligingslogboeken gepubliceerd door Azure Security Center

 * Problemen met knelpunten op de aanmeldingspagina van uw toepassing door de toepassing de prestatiegegevens van Azure Application Insights.  

De volgende video van een Ignite-sessie ziet u de voordelen van het gebruik van Azure Monitor-logboeken voor Azure AD-Logboeken in scenario's met praktische gebruiker.

> [!VIDEO https://www.youtube.com/embed/MP5IaCTwkQg?start=1894]

In dit artikel leert u hoe u Azure Active Directory (Azure AD)-logboeken integreren met Azure Monitor.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="supported-reports"></a>Ondersteunde rapporten

U kunt audit-activiteitenlogboeken en aanmeldingsactiviteiten logboeken versturen naar Azure Monitor-logboeken voor verdere analyse. 

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

## <a name="send-logs-to-azure-monitor-logs"></a>Logboeken verzenden naar logboeken van Azure Monitor

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 

2. Selecteer **Azure Active Directory** > **diagnostische instellingen** -> **diagnostische instelling toevoegen**. U kunt ook selecteren **instellingen exporteren** uit de **auditlogboeken** of **aanmeldingen** pagina om te gaan naar de configuratiepagina van de diagnostische instellingen.  
    
3. In de **diagnostische instellingen** in het menu het **verzenden naar Log Analytics-werkruimte** selectievakje en selecteer vervolgens **configureren**.

4. Selecteer de Log Analytics-werkruimte die u wilt de logboeken te verzenden, of een nieuwe werkruimte maken in het dialoogvenster.  

5. Voer een van de volgende bewerkingen uit, of beide:
    * Voor het verzenden van Logboeken voor controle naar de Log Analytics-werkruimte, selecteert u de **AuditLogs** selectievakje. 
    * Als u wilt aanmelden logboeken verzenden naar de Log Analytics-werkruimte, selecteert u de **SignInLogs** selectievakje.

6. Selecteer **Opslaan** om de instelling op te slaan.

    ![Diagnostische instellingen](./media/howto-integrate-activity-logs-with-log-analytics/Configure.png)

7. Controleer na ongeveer 15 minuten, of dat gebeurtenissen worden gestreamd naar uw Log Analytics-werkruimte.

## <a name="next-steps"></a>Volgende stappen

* [Analyseren van Azure AD-activiteitenlogboeken met Azure Monitor-Logboeken](howto-analyze-activity-logs-log-analytics.md)
* [Installeren en de log analytics-weergaven gebruiken voor Azure Active Directory](howto-install-use-log-analytics-views.md)
