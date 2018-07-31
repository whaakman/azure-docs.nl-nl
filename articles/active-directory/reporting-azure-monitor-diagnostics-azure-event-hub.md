---
title: 'Zelfstudie: Azure Active Directory-logboeken streamen naar een Azure Event Hub (preview) | Microsoft Docs'
description: Informatie over het instellen van Azure Diagnostics voor het pushen van Azure Active Directory-logboeken naar een Event Hub (preview)
services: active-directory
documentationcenter: ''
author: priyamohanram
manager: mtillman
editor: ''
ms.assetid: 045f94b3-6f12-407a-8e9c-ed13ae7b43a3
ms.service: active-directory
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 07/13/2018
ms.author: priyamo
ms.reviewer: dhanyahk
ms.openlocfilehash: c4f5bf1b49d7f59b2bb938a7ddc53b96c1d354ef
ms.sourcegitcommit: 194789f8a678be2ddca5397137005c53b666e51e
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/25/2018
ms.locfileid: "39240173"
---
# <a name="tutorial-stream-azure-active-directory-logs-to-an-azure-event-hub-preview"></a>Zelfstudie: Azure Active Directory-logboeken streamen naar een Azure Event Hub (preview)

In deze zelfstudie leert u diagnostische instellingen van Azure Monitor instellen voor het streamen van Azure Active Directory-logboeken naar een Azure Event Hub. Gebruik deze methode voor het integreren van uw logboeken met externe SIEM-hulpprogramma's als Splunk en QRadar.

## <a name="prerequisites"></a>Vereisten 

U hebt de volgende zaken nodig:

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, kunt u zich registreren voor een [gratis proefversie](https://azure.microsoft.com/free/).
* Een Azure Active Directory-tenant
* Een gebruiker die een globale beheerder of beveiligingsbeheerder voor die tenant is
* Een naamruimte van Event Hubs en een Event Hub in uw Azure-abonnement. Lees hoe u er [hier een kunt maken](https://docs.microsoft.com/azure/event-hubs/event-hubs-create.md).

## <a name="archive-logs-to-event-hub"></a>Logboeken in Event Hub archiveren

1. Meld u aan bij [Azure Portal](https://portal.azure.com). 
2. Klik op **Azure Active Directory** -> **Activiteit** -> **Auditlogboeken**. 
3. Klik op **Exportinstellingen** om de blade Diagnostische instellingen te openen. Klik op **Instelling bewerken** als u bestaande instellingen wilt bewerken, of klik op **Diagnostische instelling toevoegen** als u een nieuwe wilt toevoegen. Er zijn maximaal drie instellingen. 
    ![Exportinstellingen](./media/reporting-azure-monitor-diagnostics-azure-event-hub/ExportSettings.png "Exportinstellingen")

4. Schakel het selectievakje **Streamen naar een Event Hub** in en klik op **Event Hub/Configureren**.
5. Selecteer een Azure-abonnement en de Event Hubs-naamruimte waarheen u de logboeken wilt routeren. Het abonnement en de Event Hubs-naamruimte moeten beide worden gekoppeld aan de Active Directory-tenant van waaruit de logboeken worden gestreamd. Ook kunt u een Event Hub opgeven in de Event Hubs-naamruimte waarnaar logboeken moeten worden verzonden. Als geen Event Hub wordt opgegeven, wordt er een in de naamruimte gemaakt. Deze krijgt de standaardnaam **insights-logs-audit**.
6. Klik op **OK** om de configuratie van de Event Hub af te sluiten.
7. Schakel het selectievakje **AuditLogs** in om auditlogboeken naar het opslagaccount te verzenden. 
8. Schakel het selectievakje **SignInLogs** in om aanmeldingsboeken naar het opslagaccount te verzenden.
9. Klik op **Opslaan** om de instelling op te slaan.
    ![Diagnostische instellingen](./media/reporting-azure-monitor-diagnostics-azure-event-hub/DiagnosticSettings.png "Diagnostische instellingen")

10. Controleer na circa vijftien minuten of er gebeurtenissen in uw Event Hub worden weergegeven. Ga hiervoor vanuit de portal naar de Event Hub en controleer of het aantal **inkomende berichten** groter is dan nul. 
    ![Controlelogboeken](./media/reporting-azure-monitor-diagnostics-azure-event-hub/InsightsLogsAudit.png "Controlelogboeken")


## <a name="access-data-from-event-hubs"></a>Gegevens openen vanuit Event Hubs

Zodra er gegevens in de Event Hub worden weergegeven, kunt u ze op twee manieren openen.

* **Een ondersteund SIEM-hulpprogramma configureren om de gegevens te lezen**: voor de meeste hulpprogramma's is vereist dat de Event Hub-verbindingsreeks en bepaalde machtigingen voor uw Azure-abonnement gegevens vanuit de Event Hub moeten kunnen lezen. Hier ziet u een onvolledige lijst met Azure Monitor-integratie:
    1. **Splunk**: zie [Logboeken van Azure Active Directory integreren met Splunk met behulp van Azure Monitor](reporting-azure-monitor-diagnostics-splunk-integration.md) voor meer informatie over het integreren van Azure AD-logboeken met Splunk.
    
    2. **IBM QRadar**: De DSM (hardwarespecifieke module) van Microsoft Azure DSM en Microsoft Azure Event Hub Protocol kunnen worden gedownload op de [ondersteuningswebsite van IBM](http://www.ibm.com/support). Informatie over [de integratie mat Azure vindt u hier](https://www.ibm.com/support/knowledgecenter/SS42VS_DSM/c_dsm_guide_microsoft_azure_overview.html?cp=SS42VS_7.3.0).
    
    3. **SumoLogic**: Volg [deze instructies](https://help.sumologic.com/Send-Data/Applications-and-Other-Data-Sources/Azure-Audit/02Collect-Logs-for-Azure-Audit-from-Event-Hub) voor het instellen van SumoLogic om gegevens vanuit een Event Hub te gebruiken. 

* **Aangepaste tooling instellen voor het lezen van de gegevens**: als uw huidige SIEM nog niet in Azure Monitor Diagnostics wordt ondersteund, kunt u aangepaste tooling instellen met behulp van Event Hub-API's. Zie de [Event Hub-API's](https://docs.microsoft.com/en-us/azure/event-hubs/event-hubs-dotnet-standard-getstarted-receive-eph) voor meer informatie.


## <a name="next-steps"></a>Volgende stappen

* [Logboeken van Azure Active Directory integreren met Splunk met behulp van Azure Monitor Diagnostics](reporting-azure-monitor-diagnostics-splunk-integration.md)
* [Auditlogboekenschema interpreteren in Azure Monitor Diagnostics](reporting-azure-monitor-diagnostics-audit-log-schema.md)
* [Aanmeldingslogboekenschema interpreteren in Azure Monitor Diagnostics](reporting-azure-monitor-diagnostics-sign-in-log-schema.md)