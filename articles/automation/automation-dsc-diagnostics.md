---
title: Doorsturen van Azure Automation DSC rapportagegegevens met OMS Log Analytics | Microsoft Docs
description: In dit artikel laat zien hoe verzenden Desired State Configuration (DSC) gegevens rapporteren aan Microsoft Operations Management Suite-logboekanalyse meer inzicht leveren en beheren.
services: automation
documentationcenter: 
author: eslesar
manager: carmonm
editor: tysonn
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: eslesar
ms.openlocfilehash: 316031c5297a0201c8db4a9e177298c78962c673
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2017
---
# <a name="forward-azure-automation-dsc-reporting-data-to-oms-log-analytics"></a>Doorsturen van Azure Automation DSC rapportagegegevens met OMS Log Analytics

Automation kan DSC knooppunt statusgegevens verzenden naar de werkruimte voor logboekanalyse voor Microsoft Operations Management Suite (OMS).  
Status van naleving is zichtbaar in de Azure portal of PowerShell voor knooppunten en voor afzonderlijke DSC-resources in het knooppuntconfiguraties. Met Log Analytics kunt u het volgende doen:

* Informatie over de compatibiliteit voor beheerde knooppunten en afzonderlijke bronnen ophalen
* Activeren van een e-mailadres of de waarschuwing op basis van status van naleving
* Geavanceerde query's op uw beheerde knooppunten schrijven
* Status van naleving correleren via Automation-accounts
* De geschiedenis van naleving van knooppunt na verloop van tijd visualiseren

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen met het verzenden van uw Automation DSC-rapporten met Log Analytics, hebt u het volgende nodig:

* De November 2016 of hoger release van [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
* Een Azure Automation-account. Zie voor meer informatie [aan de slag met Azure Automation](automation-offering-get-started.md)
* Een werkruimte voor logboekanalyse met een **Automation en Control** serviceaanbieding. Zie voor meer informatie [aan de slag met logboekanalyse](../log-analytics/log-analytics-get-started.md).
* Ten minste één Azure Automation DSC-knooppunt. Zie voor meer informatie [machines voorbereiden voor beheer door Azure Automation DSC](automation-dsc-onboarding.md) 

## <a name="set-up-integration-with-log-analytics"></a>Integratie met logboekanalyse instellen

Om te beginnen met het importeren van gegevens uit Azure Automation DSC in Log Analytics, moet u de volgende stappen uitvoeren:

1. Aanmelden bij uw Azure-account in PowerShell. Zie [aanmelden met Azure PowerShell](https://docs.microsoft.com/en-us/powershell/azure/authenticate-azureps?view=azurermps-4.0.0)
1. Ophalen van de _ResourceId_ van uw automation-account met de volgende PowerShell-opdracht: (als u meer dan een automation-account hebt, kiest u de _ResourceID_ voor het account dat u wilt configureren).

  ```powershell
  # Find the ResourceId for the Automation Account
  Find-AzureRmResource -ResourceType "Microsoft.Automation/automationAccounts"
  ```
1. Ophalen van de _ResourceId_ van uw werkruimte voor logboekanalyse met de volgende PowerShell-opdracht: (als er meer dan één werkruimte, kiest u de _ResourceID_ voor de werkruimte die u wilt configureren).

  ```powershell
  # Find the ResourceId for the Log Analytics workspace
  Find-AzureRmResource -ResourceType "Microsoft.OperationalInsights/workspaces"
  ```
1. Voer de volgende PowerShell-opdracht vervangen `<AutomationResourceId>` en `<WorkspaceResourceId>` met de _ResourceId_ waarden van elk van de vorige stappen:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories "DscNodeStatus"
  ```

Als u stoppen met het importeren van gegevens uit Azure Automation DSC in Log Analytics wilt, de volgende PowerShell-opdracht uitvoeren.

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories "DscNodeStatus"
```

## <a name="view-the-dsc-logs"></a>Bekijk de DSC-Logboeken

Na het instellen van integratie met Log Analytics voor uw Automation DSC-gegevens, een **logboek zoeken** knop wordt weergegeven op de **DSC-knooppunten** blade van uw automation-account. Klik op de **logboek zoeken** knop om de logboeken voor DSC-knooppuntgegevens weer te geven.

![Logboek zoekknop](media/automation-dsc-diagnostics/log-search-button.png)

De **logboek zoeken** blade wordt geopend en ziet u een **DscNodeStatusData** bewerking voor elk DSC-knooppunt en een **DscResourceStatusData** bewerking voor elk [DSC resource](https://msdn.microsoft.com/powershell/dsc/resources) aangeroepen in de configuratie van de knooppunten op dat knooppunt toegepast.

De **DscResourceStatusData** bewerking bevat informatie over de fout voor eventuele DSC-resources die niet zijn geslaagd.

Klik op elke bewerking in de lijst om de gegevens voor de bewerking te bekijken.

U kunt ook de logboeken weergeven door [zoeken in logboekanalyse. Zie [vinden van gegevens met behulp van logboek zoekopdrachten](../log-analytics/log-analytics-log-searches.md).
Typ de volgende query voor het vinden van de DSC-Logboeken:`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category = "DscNodeStatus"`

U kunt ook de query beperken door de naam van de bewerking. Bijvoorbeeld: "Type = AzureDiagnostics ResourceProvider ="MICROSOFT Corporation. Categorie AUTOMATION' = 'DscNodeStatus' OperationName = "DscNodeStatusData"

### <a name="send-an-email-when-a-dsc-compliance-check-fails"></a>Een e-mailbericht verzenden als een DSC-controle is mislukt

Een van onze belangrijkste klantaanvragen is voor de mogelijkheid voor het verzenden van een e-mailbericht of een tekst wanneer er iets mis met een DSC-configuratie gaat.   

Als u wilt een waarschuwingsregel maakt, begint u met het maken van een zoekopdracht logboek voor de DSC-rapport-records die de waarschuwing moet worden aangeroepen.  Klik op de **waarschuwing** knop maken en configureren van de waarschuwingsregel.

1. Klik op de pagina overzicht van Log Analytics **logboek zoeken**.
1. Maak een zoekquery logboek voor de waarschuwing door de volgende zoeken in het queryveld typen:`Type=AzureDiagnostics Category=DscNodeStatus NodeName_s=DSCTEST1 OperationName=DscNodeStatusData ResultType=Failed`

  Als u naar de werkruimte logboeken van meer dan een Automation-account of -abonnement hebt ingesteld, kunt u uw waarschuwingen per abonnement en de Automation-account kunt groeperen.  
  Automation-accountnaam kan worden afgeleid van het veld Resource in het doorzoeken van DscNodeStatusData.  
1. Openen van de **waarschuwingsregel toevoegen** scherm, klikt u op **waarschuwing** boven aan de pagina. Zie voor meer informatie over de opties voor het configureren van de waarschuwing [waarschuwingen in logboekanalyse](../log-analytics/log-analytics-alerts.md#alert-rules).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Mislukte DSC-resources te vinden op alle knooppunten

Een voordeel van het gebruik van logboekanalyse is dat u naar mislukte controles op knooppunten zoeken kunt.
Alle exemplaren van DSC-resources die niet vinden.

1. Klik op de pagina overzicht van Log Analytics **logboek zoeken**.
1. Maak een zoekquery logboek voor de waarschuwing door de volgende zoeken in het queryveld typen:`Type=AzureDiagnostics Category=DscNodeStatus OperationName=DscResourceStatusData ResultType=Failed`

### <a name="view-historical-dsc-node-status"></a>Historische status van DSC-knooppunt weergeven

Ten slotte wilt u de geschiedenis van de status van de DSC-knooppunt visualiseren gedurende een bepaalde periode.  
U kunt deze query gebruiken om te zoeken naar de status van de status van uw DSC-knooppunt gedurende een bepaalde periode.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Hiermee wordt een grafiek zien van het knooppunt status weergegeven gedurende een bepaalde periode.

## <a name="log-analytics-records"></a>Log Analytics-records

Diagnostische gegevens van Azure Automation maakt twee categorieën van records in logboekanalyse.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd wanneer de nalevingscontrole is uitgevoerd. |
| OperationName |DscNodeStatusData |
| ResultType |Hiermee wordt aangegeven of het knooppunt is compatibel. |
| NodeName_s |De naam van de beheerde knooppunten. |
| NodeComplianceStatus_s |Hiermee wordt aangegeven of het knooppunt is compatibel. |
| DscReportStatus |Hiermee wordt aangegeven of de nalevingscontrole is uitgevoerd. |
| ConfigurationMode | Hoe de configuratie wordt toegepast op het knooppunt. Mogelijke waarden zijn __'ApplyOnly'__,__'ApplyandMonitior'__, en __'ApplyandAutoCorrect'__. <ul><li>__ApplyOnly__: DSC geldt de configuratie en doet niets meer tenzij u een nieuwe configuratie wordt doorgeschoven, is naar het doelknooppunt of wanneer een nieuwe configuratie is opgehaald van een server. Na de eerste toepassing van een nieuwe configuratie controleert het DSC niet voor afwijking van een eerder geconfigureerde status. DSC probeert de configuratie toepassen, totdat hij erin slaagt voordat __ApplyOnly__ wordt van kracht. </li><li> __ApplyAndMonitor__: dit is de standaardwaarde. De LCM geldt voor alle nieuwe configuraties. Als het doelknooppunt drifts van de gewenste status rapporteert DSC na de eerste toepassing van een nieuwe configuratie, de discrepantie in Logboeken. DSC probeert de configuratie toepassen, totdat hij erin slaagt voordat __ApplyAndMonitor__ wordt van kracht.</li><li>__ApplyAndAutoCorrect__: DSC eventuele nieuwe configuraties van toepassing. Na de eerste toepassing van een nieuwe configuratie als het doelknooppunt drifts van de gewenste status DSC rapporteert het verschil in Logboeken en vervolgens wordt de huidige configuratie opnieuw toegepast.</li></ul> |
| HostName_s | De naam van de beheerde knooppunten. |
| IP-adres | Het IPv4-adres van het beheerde knooppunt. |
| Category | DscNodeStatus |
| Resource | De naam van het Azure Automation-account. |
| Tenant_g | GUID die de tenant voor de aanroeper identificeert. |
| NodeId_g |De GUID die de beheerde knooppunt aangeeft. |
| DscReportId_g |GUID die het rapport identificeert. |
| LastSeenTime_t |Datum en tijd wanneer het rapport voor het laatst is bekeken. |
| ReportStartTime_t |Datum en tijd waarop het rapport is gestart. |
| ReportEndTime_t |Datum en tijd waarop het rapport is voltooid. |
| NumberOfResources_d |Het aantal DSC-resources in de configuratie die wordt toegepast op het knooppunt genoemd. |
| SourceSystem | Log Analytics verzameld hoe de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResourceId |Hiermee geeft u het Azure Automation-account. |
| ResultDescription | De beschrijving voor deze bewerking. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | Naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT CORPORATION. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |GUID die de correlatie-id van het rapport voor naleving. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd wanneer de nalevingscontrole is uitgevoerd. |
| OperationName |DscResourceStatusData|
| ResultType |Hiermee wordt aangegeven of de resource is compatibel. |
| NodeName_s |De naam van de beheerde knooppunten. |
| Category | DscNodeStatus |
| Resource | De naam van het Azure Automation-account. |
| Tenant_g | GUID die de tenant voor de aanroeper identificeert. |
| NodeId_g |De GUID die de beheerde knooppunt aangeeft. |
| DscReportId_g |GUID die het rapport identificeert. |
| DscResourceId_s |De naam van de DSC-resource-instantie. |
| DscResourceName_s |De naam van de DSC-resource. |
| DscResourceStatus_s |Hiermee wordt aangegeven of de DSC-resource is compatibel. |
| DscModuleName_s |De naam van de PowerShell-module met de DSC-resource. |
| DscModuleVersion_s |De versie van de PowerShell-module met de DSC-resource. |
| DscConfigurationName_s |De naam van de configuratie die wordt toegepast op het knooppunt. |
| ErrorCode_s | De foutcode als de bron is mislukt. |
| ErrorMessage_s |Het foutbericht als de bron is mislukt. |
| DscResourceDuration_d |De tijd in seconden, de DSC-resource is uitgevoerd. |
| SourceSystem | Log Analytics verzameld hoe de gegevens. Altijd *Azure* voor Azure diagnostics. |
| ResourceId |Hiermee geeft u het Azure Automation-account. |
| ResultDescription | De beschrijving voor deze bewerking. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | Naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT CORPORATION. AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |GUID die de correlatie-id van het rapport voor naleving. |

## <a name="summary"></a>Samenvatting

Uw Automation DSC-gegevens te verzenden naar Log Analytics, krijgt u inzicht in de status van uw Automation DSC-knooppunten op basis van:

* Instellen van waarschuwingen om u te waarschuwen wanneer er een probleem
* Met behulp van aangepaste weergaven en zoekopdrachten voor het visualiseren van de runbookresultaten, gerelateerde de status van de runbook-taak en andere indicatoren of metrische gegevens.  

Log Analytics biedt groter operationeel inzicht in uw Automation DSC-gegevens en adres incidenten sneller kan helpen.  

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over het maken van verschillende zoekquery's en de Automation DSC-logboeken met Log Analytics, [zoekopdrachten aanmelden met Log Analytics](../log-analytics/log-analytics-log-searches.md)
* Zie voor meer informatie over het gebruik van Azure Automation DSC, [aan de slag met Azure Automation DSC](automation-dsc-getting-started.md)
* Zie [Collecting Azure storage data in Log Analytics overview (Overzicht Azure-opslaggegevens verzamelen in Log Analytics)](../log-analytics/log-analytics-azure-storage.md) voor meer informatie over OMS Log Analytics en gegevensverzamelingsbronnen

