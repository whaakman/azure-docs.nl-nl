---
title: Doorsturen van Azure Automation State Configuration rapportagegegevens met Log Analytics
description: In dit artikel ziet u hoe u voor het verzenden van Desired State Configuration (DSC) rapportagegegevens van Azure Automation State Configuration naar Log Analytics om meer inzicht en beheer te bieden.
services: automation
ms.service: automation
ms.component: dsc
author: bobbytreed
ms.author: robreed
ms.date: 11/06/2018
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: aa543ad119716b25b7f3ab00d49efb9fe6fb6c3f
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244233"
---
# <a name="forward-azure-automation-state-configuration-reporting-data-to-log-analytics"></a>Doorsturen van Azure Automation State Configuration rapportagegegevens met Log Analytics

Statusconfiguratie van Azure Automation kan Desired State Configuration (DSC) knooppunt status van gegevens verzenden naar uw Log Analytics-werkruimte. Status van naleving is zichtbaar in de Azure portal of met PowerShell, voor knooppunten en voor afzonderlijke DSC-resources in knooppuntconfiguraties. Met Log Analytics kunt u het volgende doen:

- Informatie over de naleving voor beheerde knooppunten en afzonderlijke resources ophalen
- Activeer een e-mailadres of de waarschuwing op basis van status van naleving
- Geavanceerde query's schrijven over uw beheerde knooppunten
- Combineer status van naleving van Automation-accounts
- Visualiseer uw geschiedenis van naleving van knooppunt na verloop van tijd

## <a name="prerequisites"></a>Vereisten

Als u wilt beginnen met het verzenden van uw configuratie van de status van Automation-rapporten naar Log Analytics, hebt u het volgende nodig:

- De November 2016 of hoger van de release [Azure PowerShell](/powershell/azure/overview) (v2.3.0).
- Een Azure Automation-account. Zie voor meer informatie, [aan de slag met Azure Automation](automation-offering-get-started.md)
- Een Log Analytics-werkruimte met een **Automation en besturing** serviceaanbieding. Zie voor meer informatie, [aan de slag met Log Analytics](../log-analytics/log-analytics-get-started.md).
- Ten minste één knooppunt van de configuratie van Azure Automation-status. Zie voor meer informatie, [Onboarding van machines voor beheer met Azure Automation State Configuration](automation-dsc-onboarding.md)

## <a name="set-up-integration-with-log-analytics"></a>Integratie met Log Analytics instellen

Als u wilt importeren van gegevens uit Azure Automation DSC in Log Analytics, voert u de volgende stappen uit:

1. Meld u aan bij uw Azure-account in PowerShell. Zie [aanmelden met Azure PowerShell](https://docs.microsoft.com/powershell/azure/authenticate-azureps?view=azurermps-4.0.0)
1. Krijgen de _ResourceId_ van uw automation-account door het uitvoeren van de volgende PowerShell-opdracht: (als u meer dan één automation-account hebt, kiest u de _ResourceID_ voor het account dat u wilt configureren).

  ```powershell
  # Find the ResourceId for the Automation Account
  Get-AzureRmResource -ResourceType 'Microsoft.Automation/automationAccounts'
  ```

1. Krijgen de _ResourceId_ van uw Log Analytics-werkruimte door de volgende PowerShell-opdracht uit: (als u meer dan één werkruimte hebt, kiest u de _ResourceID_ voor de werkruimte die u wilt configureren).

  ```powershell
  # Find the ResourceId for the Log Analytics workspace
  Get-AzureRmResource -ResourceType 'Microsoft.OperationalInsights/workspaces'
  ```

1. Voer de volgende PowerShell-opdracht, vervangt `<AutomationResourceId>` en `<WorkspaceResourceId>` met de _ResourceId_ waarden van elk van de vorige stappen:

  ```powershell
  Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $true -Categories 'DscNodeStatus'
  ```

Als u stoppen met het importeren van gegevens uit Azure Automation State Configuration in Log Analytics wilt, voert u de volgende PowerShell-opdracht:

```powershell
Set-AzureRmDiagnosticSetting -ResourceId <AutomationResourceId> -WorkspaceId <WorkspaceResourceId> -Enabled $false -Categories 'DscNodeStatus'
```

## <a name="view-the-state-configuration-logs"></a>Bekijk de logboeken van de configuratie van status

Na het instellen van integratie met Log Analytics voor uw Automation-status configuratiegegevens, een **zoeken in logboeken** knop wordt weergegeven op de **DSC-knooppunten** blade van uw automation-account. Klik op de **zoeken in logboeken** knop om de logboeken voor DSC-knooppuntgegevens weer te geven.

![Zoekknop log](media/automation-dsc-diagnostics/log-search-button.png)

De **zoeken in logboeken** blade wordt geopend en ziet u een **DscNodeStatusData** bewerking voor elk knooppunt de configuratie van de status en een **DscResourceStatusData** bewerking voor elke [ DSC-resource](/powershell/dsc/resources) genoemd in de configuratie van de knooppunten op dat knooppunt toegepast.

De **DscResourceStatusData** bewerking bevat foutinformatie voor eventuele DSC-resources die niet zijn geslaagd.

Klik op elke bewerking in de lijst om te zien van de gegevens voor deze bewerking.

U kunt ook de logboeken bekijken door [te zoeken in Log Analytics. Zie [vinden van gegevens met behulp van zoekopdrachten in logboeken](../log-analytics/log-analytics-log-searches.md).
Typ de volgende query uit om uw logboeken State Configuration zoeken: `Type=AzureDiagnostics ResourceProvider='MICROSOFT.AUTOMATION' Category='DscNodeStatus'`

U kunt de query ook beperken door de naam van de bewerking. Bijvoorbeeld: `Type=AzureDiagnostics ResourceProvider='MICROSOFT.AUTOMATION' Category='DscNodeStatus' OperationName='DscNodeStatusData'`

### <a name="send-an-email-when-a-state-configuration-compliance-check-fails"></a>Een e-mailbericht verzenden wanneer een compatibiliteitscontrole van configuratie van de status mislukt

Een van onze belangrijkste klantaanvragen is de mogelijkheid voor het verzenden van een e-mailadres of een SMS-bericht wanneer er iets met een DSC-configuratie misgaat.

Voor het maken van een waarschuwingsregel, begint u met het maken van een zoeken in Logboeken voor de configuratie van status rapport records die de waarschuwing moet worden aangeroepen. Klik op de **+ nieuwe waarschuwingsregel** knop maken en configureren van de waarschuwingsregel.

1. Klik op de pagina overzicht van Log Analytics **zoeken in logboeken**.
1. Maak een zoekquery logboek voor de waarschuwing door te zoeken in de volgende typen in het queryveld:  `Type=AzureDiagnostics Category='DscNodeStatus' NodeName_s='DSCTEST1' OperationName='DscNodeStatusData' ResultType='Failed'`

   Als u aan uw werkruimte Logboeken uit meer dan één Automation-account of -abonnement hebt ingesteld, kunt u uw waarschuwingen op basis van abonnement en de Automation-account kunt groeperen.  
   Naam van het Automation-account kan worden afgeleid van de Resource-veld in het zoekvak van DscNodeStatusData.  
1. Om te openen de **maken regel** scherm, klikt u op **+ nieuwe waarschuwingsregel** aan de bovenkant van de pagina. Zie voor meer informatie over de opties voor het configureren van de waarschuwing [maken van een waarschuwing rulelert](../monitoring-and-diagnostics/monitor-alerts-unified-usage.md).

### <a name="find-failed-dsc-resources-across-all-nodes"></a>Mislukte DSC-resources zoeken voor alle knooppunten

Eén voordeel van het gebruik van Log Analytics is dat u kunt zoeken naar mislukte controles op knooppunten.
Alle exemplaren van DSC-resources die niet zijn gevonden.

1. Klik op de pagina overzicht van Log Analytics **zoeken in logboeken**.
1. Maak een zoekquery logboek voor de waarschuwing door te zoeken in de volgende typen in het queryveld:  `Type=AzureDiagnostics Category='DscNodeStatus' OperationName='DscResourceStatusData' ResultType='Failed'`

### <a name="view-historical-dsc-node-status"></a>Historische DSC-knooppuntstatus weergeven

Ten slotte kunt u voor het visualiseren van de geschiedenis van de status van de DSC-knooppunt na verloop van tijd.  
U kunt deze query gebruiken om te zoeken naar de status van de status van uw DSC-knooppunt na verloop van tijd.

`Type=AzureDiagnostics ResourceProvider="MICROSOFT.AUTOMATION" Category=DscNodeStatus NOT(ResultType="started") | measure Count() by ResultType interval 1hour`  

Hiermee wordt een grafiek zien van de status van het knooppunt weergegeven na verloop van tijd.

## <a name="log-analytics-records"></a>Log Analytics-records

Diagnostische gegevens van Azure Automation wordt onderverdeeld in twee records gemaakt in Log Analytics.

### <a name="dscnodestatusdata"></a>DscNodeStatusData

| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd waarop de controle op naleving wordt uitgevoerd. |
| OperationName |DscNodeStatusData |
| resultType |Of het knooppunt is compatibel. |
| NodeName_s |De naam van het beheerd knooppunt. |
| NodeComplianceStatus_s |Of het knooppunt is compatibel. |
| DscReportStatus |Of de controle op naleving is uitgevoerd. |
| ConfigurationMode | Hoe de configuratie wordt toegepast op het knooppunt. Mogelijke waarden zijn __"ApplyOnly"__,__"ApplyandMonitior"__, en __"ApplyandAutoCorrect"__. <ul><li>__ApplyOnly__: DSC geldt de configuratie en doet niets meer, tenzij er een nieuwe configuratie wordt doorgestuurd naar het doelknooppunt of wanneer een nieuwe configuratie wordt opgehaald uit een server. Na de eerste toepassing van een nieuwe configuratie DSC controleert niet op afwijking van een eerder geconfigureerde status. DSC wordt geprobeerd om toe te passen van de configuratie totdat hij erin slaagt voordat __ApplyOnly__ wordt van kracht. </li><li> __ApplyAndMonitor__: dit is de standaardwaarde. De LCM geldt voor alle nieuwe configuraties. Als het doelknooppunt drifts van de gewenste status, rapporteert DSC na de eerste toepassing van een nieuwe configuratie, het verschil in Logboeken. DSC wordt geprobeerd om toe te passen van de configuratie totdat hij erin slaagt voordat __ApplyAndMonitor__ wordt van kracht.</li><li>__ApplyAndAutoCorrect__: DSC eventuele nieuwe configuraties van toepassing. Na de eerste toepassing van een nieuwe configuratie als het doelknooppunt drifts van de gewenste status, DSC rapporteert het verschil in Logboeken en vervolgens opnieuw de huidige configuratie.</li></ul> |
| HostName_s | De naam van het beheerd knooppunt. |
| IP-adres | Het IPv4-adres van de beheerde knooppunten. |
| Categorie | DscNodeStatus |
| Resource | De naam van de Azure Automation-account. |
| Tenant_g | De GUID die de tenant voor de oproepende functie identificeert. |
| NodeId_g |GUID waarmee de beheerd knooppunt. |
| DscReportId_g |De GUID die het rapport identificeert. |
| LastSeenTime_t |Datum en tijd wanneer het rapport voor het laatst is bekeken. |
| ReportStartTime_t |Datum en tijd waarop het rapport is gestart. |
| ReportEndTime_t |De datum en tijd waarop het rapport is voltooid. |
| NumberOfResources_d |Het aantal DSC-resources met de naam in de configuratie die is toegepast op het knooppunt. |
| SourceSystem | Hoe worden de gegevens verzameld door Log Analytics. Altijd *Azure* voor Azure diagnostics. |
| ResourceId |Hiermee geeft u de Azure Automation-account. |
| ResultDescription | De beschrijving voor deze bewerking. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | De naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |De GUID die de correlatie-Id van het nalevingsrapport. |

### <a name="dscresourcestatusdata"></a>DscResourceStatusData

| Eigenschap | Beschrijving |
| --- | --- |
| TimeGenerated |Datum en tijd waarop de controle op naleving wordt uitgevoerd. |
| OperationName |DscResourceStatusData|
| resultType |Of de resource is compatibel. |
| NodeName_s |De naam van het beheerd knooppunt. |
| Categorie | DscNodeStatus |
| Resource | De naam van de Azure Automation-account. |
| Tenant_g | De GUID die de tenant voor de oproepende functie identificeert. |
| NodeId_g |GUID waarmee de beheerd knooppunt. |
| DscReportId_g |De GUID die het rapport identificeert. |
| DscResourceId_s |De naam van de DSC-resource-exemplaar. |
| DscResourceName_s |De naam van de DSC-resource. |
| DscResourceStatus_s |Of de DSC-resource is in acht genomen. |
| DscModuleName_s |De naam van de PowerShell-module met de DSC-resource. |
| DscModuleVersion_s |De versie van de PowerShell-module met de DSC-resource. |
| DscConfigurationName_s |De naam van de configuratie die is toegepast op het knooppunt. |
| ErrorCode_s | De foutcode van het object als de resource is mislukt. |
| ErrorMessage_s |Het foutbericht als de resource is mislukt. |
| DscResourceDuration_d |De tijd in seconden, de DSC-resource is uitgevoerd. |
| SourceSystem | Hoe worden de gegevens verzameld door Log Analytics. Altijd *Azure* voor Azure diagnostics. |
| ResourceId |Hiermee geeft u de Azure Automation-account. |
| ResultDescription | De beschrijving voor deze bewerking. |
| SubscriptionId | De Azure-abonnement-Id (GUID) voor het Automation-account. |
| ResourceGroup | De naam van de resourcegroep voor het Automation-account. |
| ResourceProvider | MICROSOFT.AUTOMATION |
| ResourceType | AUTOMATIONACCOUNTS |
| CorrelationId |De GUID die de correlatie-Id van het nalevingsrapport. |

## <a name="summary"></a>Samenvatting

Uw configuratiegegevens van Automation status verzenden naar Log Analytics, krijgt u inzicht in de status van uw configuratie van de status van Automation-knooppunten op basis van:

- Instellen van waarschuwingen om u te waarschuwen wanneer er een probleem
- Met behulp van aangepaste weergaven en zoekopdrachten naar uw runbookresultaten te visualiseren, gerelateerde de status van de runbook-taak en andere KPI's of metrische gegevens.  

Log Analytics biedt tevens grotere operationele zichtbaarheid aan uw Automation-status-configuratiegegevens en incidenten sneller kan helpen.

## <a name="next-steps"></a>Volgende stappen

- Zie voor een overzicht [Statusconfiguratie van Azure Automation](automation-dsc-overview.md)
- Als u wilt beginnen, Zie [aan de slag met Azure Automation State Configuration](automation-dsc-getting-started.md)
- Zie voor meer informatie over het DSC-configuraties compileren zodat u ze aan de doelknooppunten toewijzen kunt, [-configuraties in Azure Automation-staat configuratie compileren](automation-dsc-compile.md)
- Zie voor naslagdocumentatie voor PowerShell-cmdlet, [Statusconfiguratie van Azure Automation-cmdlets](/powershell/module/azurerm.automation/#automation)
- Zie voor informatie over de prijzen [Statusconfiguratie van Azure Automation-prijzen](https://azure.microsoft.com/pricing/details/automation/)
- Zie voor een voorbeeld van het gebruik van Azure Automation State Configuration in een pijplijn voor continue implementatie [continue implementatie met behulp van Azure Automation Statusconfiguratie- en Chocolatey](automation-dsc-cd-chocolatey.md)
- Zie voor meer informatie over het maken van verschillende zoekquery's en bekijk de logboeken van de configuratie van de Automation-status met Log Analytics, [zoekopdrachten in Logboeken in Log Analytics](../log-analytics/log-analytics-log-searches.md)
- Zie voor meer informatie over Log Analytics en gegevensverzamelingsbronnen [verzamelen van Azure storage-gegevens in Log Analytics-overzicht](../log-analytics/log-analytics-azure-storage.md)