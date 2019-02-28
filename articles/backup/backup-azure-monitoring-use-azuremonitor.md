---
title: 'Azure Backup: Azure Backup bewaken met Azure Monitor'
description: Azure Backup workloads bewaken en aangepaste waarschuwingen met behulp van Azure Monitor te maken
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure back-up. Waarschuwingen; Diagnostische instellingen. Actiegroepen
ms.service: backup
ms.topic: conceptual
ms.date: 02/26/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: e7b1b3e3fba04276fc284fd71adabedc01185251
ms.sourcegitcommit: 1afd2e835dd507259cf7bb798b1b130adbb21840
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56984812"
---
# <a name="monitoring-at-scale-using-azure-monitor"></a>Bewaking op schaal met Azure Monitor

De [ingebouwde bewaking en waarschuwingen van artikel](backup-azure-monitoring-built-in-monitor.md) vermeld de bewakings- en waarschuwingsfuncties in één Recovery services-kluis en die wordt aangeboden zonder een infrastructuur voor aanvullend beheer. De ingebouwde service is echter beperkt in de volgende scenario's.

- Bewakingsgegevens uit meerdere RS kluizen tussen abonnementen
- Als e-mailadres niet het gewenste meldingskanaal is
- Als gebruikers wilt voor het ophalen van waarschuwingen voor meer scenario 's
- Als u bekijken van gegevens van on-premises-onderdeel, zoals System Center DPM (SC-DPM) in Azure, dat niet wordt weergegeven wilt in [back-uptaken](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) of [waarschuwingen voor back-up](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault) in de portal.

## <a name="using-log-analytics-workspace"></a>Met behulp van Log Analytics-werkruimte

> [!NOTE]
> Gegevens uit Azure-VM back-ups, MAB-Agent, System Center DPM (SC-DPM) is aan de Log Analytics-werkruimte via diagnostische instellingen worden gepompt. Ondersteuning voor SQL-back-ups in Azure VM's, back-ups van Azure File share, Microsoft Azure Backup-Server (MABS) is binnenkort beschikbaar.

We maken gebruik van de mogelijkheden van twee Azure-services - **diagnostische instellingen** (om gegevens te verzenden vanuit meerdere Azure Resource Manager-resources naar een andere resource) en **Log Analytics** (LA - genereren aangepaste waarschuwingen waar u andere meldingskanalen met behulp van actiegroepen kunt definiëren) voor het bewaken van op grote schaal. De volgende secties details over het gebruik van LA voor het bewaken van Azure back-up op schaal.

### <a name="configuring-diagnostic-settings"></a>Diagnostische instellingen configureren

Een Azure Resource Manager-resource, zoals Azure Recovery services-kluis registreert alle mogelijke gegevens over geplande bewerkingen en gebruikersbewerkingen geactiveerd als diagnostische gegevens. Klik op de 'Diagnostische instellingen' in de sectie bewaking en het doel voor diagnostische gegevens van de kluis RS opgeven.

![Diagnostische instelling RS-kluis met LA als doel](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

U kunt een werkruimte LA selecteren vanuit een ander abonnement als het doel. *Als u de dezelfde werkruimte LA voor meerdere RS kluizen selecteert, kunt u kluizen voor abonnementen op één plek bewaken.* Selecteer 'AzureBackupReport' als het logboek aan kanaal alle Azure Backup gerelateerde gegevens in de werkruimte LA.

> [!IMPORTANT]
> Nadat u de configuratie hebt voltooid, moet u wachten gedurende 24 uur voor de initiële gegevens-push te voltooien. Daarna alle gebeurtenissen worden gepusht, zoals vermeld in de [sectie frequentie](#diagnostic-data-update-frequency).

### <a name="deploying-solution-to-log-analytics-workspace"></a>Oplossing implementeren naar Log Analytics-werkruimte

Wanneer de gegevens in de werkruimte LA [een github-sjabloon implementeren](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) naar LA om de gegevens te visualiseren. Zorg ervoor dat u de dezelfde resourcegroep bevinden, Werkruimtenaam en de locatie van de werkruimte wordt herkend en vervolgens installeren met deze sjabloon op het geven.

### <a name="view-azure-backup-data-using-log-analytics-la"></a>Azure Backup-gegevens met behulp van Log Analytics (LA) weergeven

Wanneer de sjabloon is geïmplementeerd, worden de oplossing voor het bewaken van Azure Backup in de werkruimte overzicht regio weergegeven. U kunt passeren via

- Azure Monitor 'Informatie' onder de sectie 'Insights' -> en kies de relevante werkruimte of
- Log Analytics-werkruimten -> Selecteer de relevante werkruimte 'Werkruimte overzicht' onder de sectie Algemeen ->.

![AzureBackupLAMonitoringTile](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Op de tegel klikt, de ontwerpfunctie sjabloon van een reeks grafieken over de basic bewakingsgegevens van Azure Backup, zoals wordt geopend

#### <a name="all-backup-jobs"></a>Alle back-uptaken

![LABackupJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

#### <a name="restore-jobs"></a>Herstellen van taken

![LARestoreJobs](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

#### <a name="inbuilt-azure-backup-alerts-for-azure-resources"></a>Ingebouwde back-up van Azure-waarschuwingen voor Azure-Resources

![LAInbuiltAzureBackupAlertsForAzureResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

#### <a name="inbuilt-azure-backup-alerts-for-on-prem-resources"></a>Ingebouwde back-up van Azure-waarschuwingen voor On-premises bronnen

![LAInbuiltAzureBackupAlertsForOnPremResources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

#### <a name="active-datasources"></a>Actieve gegevensbronnen

![LAActiveBackedUpEntities](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

#### <a name="rs-vault-cloud-storage"></a>Opslag in de Cloud RS-kluis

![LARSVaultCloudStorage](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

De bovenstaande grafieken zijn meegeleverd met de sjabloon en de klant is gratis voor bewerken/toevoegen van meer grafieken.

> [!IMPORTANT]
> Wanneer u de sjabloon implementeert, wordt in feite een alleen-lezenvergrendeling gemaakt en u wilt verwijderen om de sjabloon bewerken en opslaan. Als u wilt verwijderen wordt vergrendeld, zoeken in het deelvenster 'Vergrendelingen' in het gedeelte 'Instellingen' van de Log Analytics-werkruimte.

### <a name="create-alerts-using-log-analytics"></a>Waarschuwingen maken met behulp van Log Analytics

Met Azure Monitor kunt u gebruikers hun eigen om waarschuwingen te maken van LA werkruimte waar u kunt *gebruikmaken van de Azure-actiegroepen om te selecteren van uw voorkeur meldingsmechanisme*. Klik op een of meer grafieken hierboven voor het openen van de sectie 'Logboeken' LA werkruimte ***kunt u de query's bewerken en maken van waarschuwingen boven op deze***.

![LACreateAlerts](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Te klikken op 'Nieuwe waarschuwingsregel' zoals u hierboven ziet, wordt het scherm van Azure Monitor het maken van waarschuwingen geopend.

Zoals u hieronder zien kunt, de resource is al gemarkeerd als de werkruimte LA en integratie met actie is opgegeven.

![LAAzureBackupCreateAlert](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

> [!IMPORTANT]
> Houd er rekening mee dat de relevante invloed op de prijzen voor het maken van deze query wordt geleverd [hier](https://azure.microsoft.com/pricing/details/monitor/).

#### <a name="alert-condition"></a>De voorwaarde voor waarschuwing

Het belangrijkste aspect is de activerende voorwaarde van de waarschuwing. Te klikken op 'Voorwaarde' wordt automatisch geladen de Kusto-query in het scherm 'Logs' zoals hieronder wordt weergegeven en kunt u deze aan de behoeften van uw scenario bewerken. Sommige voorbeeld Kusto-query's vindt u in de [hieronder](#sample-kusto-queries).

![LAAzureBackupAlertCondition](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

De Kusto-query, indien nodig bewerken, selecteert u de juiste drempelwaarde (die bepaalt wanneer de waarschuwing wordt geactiveerd), de juiste periode (tijdvenster waarvoor de query wordt uitgevoerd), en de frequentie. Voor bijvoorbeeld: Voor bijvoorbeeld: Als de drempelwaarde groter dan 0 is, de periode 5 minuten is en de frequentie 5 minuten is, wordt klikt u vervolgens de regel omgezet als 'Voer de query uit om de 5 minuten voor de laatste 5 minuten en als het aantal resultaten groter dan 0 is, stuur me een de geselecteerde actiegroep'

#### <a name="action-group-integration"></a>Integratie met actie

Actiegroepen opgeven de meldingskanalen beschikbaar voor de gebruiker. Sectie te klikken op 'Nieuw' onder "Actiegroepen" wordt de lijst met beschikbare van meldingsmechanismen beschreven.

![LAAzureBackupNewActionGroup](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

Meer informatie over de [waarschuwingen van de werkruimte LA](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) en [actiegroepen](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups) in Azure Monitor-documentatie.

U kunt daarom voldoen aan alle waarschuwingen en bewaking van vereisten uit LA alleen of als een aanvullende techniek voor ingebouwde meldingsmechanismen gebruiken.

### <a name="sample-kusto-queries"></a>Voorbeeld Kusto-query 's

De standaardgrafieken, krijgt u Kusto-query's voor algemene scenario's waarvoor u waarschuwingen kunt bouwen. U kunt ook wijzigen zodat ze de gegevens die u u een melding wilt op. We bieden hier enkele voorbeeld Kusto-query's die u kunnen plakken in het venster 'Logs' en maak vervolgens een waarschuwing op de query.

#### <a name="all-successful-backup-jobs"></a>Alle geslaagde back-uptaken

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Completed"
````

#### <a name="all-failed-backup-jobs"></a>Alle mislukte back-uptaken

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| where OperationName == "Job" and JobOperation_s == "Backup"
| where JobStatus_s == "Failed"
````

#### <a name="all-successful-azure-vm-backup-jobs"></a>Alle succesvolle Azure-VM back-uptaken

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "VM" and BackupManagementType_s == "IaaSVM"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-sql-log-backup-jobs"></a>Alle succesvolle SQL-logboek back-uptaken

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s == "Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "SQLDataBase" and BackupManagementType_s == "AzureWorkload"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

#### <a name="all-successful-mab-agent-backup-jobs"></a>Alle succesvolle MAB-Agent back-uptaken

````Kusto
AzureDiagnostics
| where Category == "AzureBackupReport"
| where SchemaVersion_s == "V2"
| extend JobOperationSubType_s = columnifexists("JobOperationSubType_s", "")
| where OperationName == "Job" and JobOperation_s == "Backup" and JobStatus_s == "Completed" and JobOperationSubType_s != "Log" and JobOperationSubType_s != "Recovery point_Log"
| join kind=inner
(
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where OperationName == "BackupItem"
    | where SchemaVersion_s == "V2"
    | where BackupItemType_s == "FileFolder" and BackupManagementType_s == "MAB"
    | distinct BackupItemUniqueId_s, BackupItemFriendlyName_s
    | project BackupItemUniqueId_s , BackupItemFriendlyName_s
)
on BackupItemUniqueId_s
| extend Vault= Resource
| project-away Resource
````

### <a name="diagnostic-data-update-frequency"></a>De updatefrequentie diagnostische gegevens

De diagnostische gegevens uit de kluis is dat aan de werkruimte LA met enige vertraging. Elke gebeurtenis wordt ontvangen in de werkruimte LA ***met een vertraging van 20-30 minuten nadat deze uit de kluis RS wordt doorgestuurd.***

- De ingebouwde waarschuwingen van de Backup-service (voor alle oplossingen) worden doorgegeven als ze worden gemaakt. Dit betekent dat ze is doorgaans worden weergegeven in de werkruimte LA na een vertraging van 20-30 minuten.
- Ad-hoc back-uptaken en het herstellen van taken (voor alle oplossingen) zo snel als ze worden gepusht ***zijn voltooid***.
- De geplande back-uptaken van alle oplossingen (met uitzondering van de back-up van SQL) zo snel als ze worden gepusht ***zijn voltooid***.
- Voor back-up van SQL, aangezien we elke 15 minuten, voor alle de voltooide geplande back-uptaken, met inbegrip van Logboeken, back-ups van de informatie is verzameld en gepusht om de 6 uur.
- Alle andere gegevens zoals back-upitem, beleid, herstelpunten, opslag enz. over alle oplossingen worden gepusht **ten minste één keer per dag.**
- Een wijziging in de back-upconfiguratie zoals het wijzigen van beleid voor het bewerken van beleid enzovoort wordt geactiveerd voor een push van alle gerelateerde back-upgegevens.

## <a name="using-rs-vaults-activity-logs"></a>Activiteitenlogboeken RS-kluis gebruiken

U kunt ook de activiteitenlogboeken gebruiken om op te halen van de melding voor gebeurtenissen, zoals back-up geslaagd.

> [!CAUTION]
> **Opmerking: dit is alleen van toepassing op virtuele Azure-machines.** U kunt dit niet gebruiken voor andere oplossingen, zoals Azure Backup-Agent, SQL-back-ups in Azure, Azure-bestanden enzovoort.

### <a name="sign-in-into-azure-portal"></a>Meld u aan bij Azure-portal

Meld u aan bij de Azure-portal en gaat u verder met de relevante Azure Recovery Services-kluis en klik op de sectie 'Activity log' in de eigenschappen.

### <a name="identify-appropriate-log-and-create-alert"></a>Juiste logboek te identificeren en waarschuwing maken

De filters weergegeven in de volgende afbeelding om te controleren of u activiteitenlogboeken voor geslaagde back-ups ontvangt van toepassing. De timespan dienovereenkomstig wijzigen om records weer te geven.

![Activiteitenlogboeken voor back-ups van virtuele Azure-machine](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

U kunt klikken op het segment 'JSON' voor meer informatie en bekijken door te kopiëren-plakken deze naar een teksteditor. De details van de kluis moet worden weergegeven en het item dat de activiteit geactiveerd melden dat wil zeggen, de back-upitem.

Klik vervolgens op 'Een waarschuwing voor activiteitenlogboek toevoegen' waarschuwingen voor alle deze logboeken worden gegenereerd.

U kunt klikken op 'Add waarschuwing voor activiteitenlogboek' hierboven wordt weergegeven en wordt het scherm voor het maken van waarschuwingen die vergelijkbaar is met het maken van waarschuwingen scherm geopend [, zoals hierboven beschreven](#create-alerts-using-log-analytics).

De resource als volgt de kluis RS zelf en daarom moet u dezelfde actie voor alle kluizen waarin u een melding via activiteitenlogboeken wilt herhalen. De voorwaarde wordt een drempelwaarde, de periode, de frequentie niet hebben, omdat dit een waarschuwing op basis van gebeurtenissen is. Zodra het relevante activiteitenlogboek wordt gegenereerd, wordt de waarschuwing wordt geactiveerd.

## <a name="recommendation"></a>Aanbeveling

***Alle waarschuwingen die zijn gemaakt van activiteitenlogboeken en LA werkruimten in Azure Monitor kunnen worden weergegeven in het deelvenster 'Waarschuwingen' aan de linkerkant.***

Tijdens de melding via activiteitenlogboeken kan worden gebruikt, ***Azure Backup-service wordt ten zeerste aangeraden naar [LA gebruiken voor bewaking op schaal](#monitoring-at-scale) en niet activiteitenlogboeken voor de volgende redenen***.

- **Beperkte scenario's:** Alleen van toepassing op virtuele Azure-machines en voor elke kluis RS moet worden herhaald.
- **Definitie van past:** De geplande back-activiteit niet voldoen aan de meest recente definitie van activiteitenlogboeken en conform de [diagnostische logboeken](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-are-azure-monitor-diagnostic-logs). Deze leiden tot onverwachte gevolgen wanneer de gegevens die via een activiteit log kanaal pompen wordt gewijzigd zoals hieronder.
- **Problemen met activiteit log kanaal:** We hebben verplaatst naar een nieuw model moet activiteitenlogboeken van Azure Backup op de Recovery Services-kluizen. De verplaatsing is helaas generatie van activiteitenlogboeken in Azure onafhankelijke Clouds beïnvloed. Als gebruikers van Azure onafhankelijke Cloud gemaakt/geconfigureerd geen waarschuwingen van activiteitenlogboeken via Azure Monitor, zouden ze niet worden geactiveerd. Ook wordt in alle Azure openbare regio's, als een gebruiker Recovery Services-activiteitenlogboeken in een Log Analytics-werkruimte verzamelen is zoals vermeld [hier](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), deze logboeken ook niet weergegeven.

Dus het is raadzaam gebruik van Log Analytics-werkruimte voor bewaking en waarschuwingen op schaal voor uw Azure Backup beveiligde werkbelastingen.

## <a name="next-steps"></a>Volgende stappen

- Raadpleeg [Log analytics-gegevensmodel](backup-azure-log-analytics-data-model.md) te maken van aangepaste query's.