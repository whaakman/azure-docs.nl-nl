---
title: 'Azure Backup: Azure Backup bewaken met Azure Monitor'
description: Azure Backup workloads bewaken en aangepaste waarschuwingen te maken met behulp van Azure Monitor.
services: backup
author: pvrk
manager: shivamg
keywords: Log Analytics; Azure back-up. Waarschuwingen; Diagnostische instellingen. Actiegroepen
ms.service: backup
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: pullabhk
ms.assetid: 01169af5-7eb0-4cb0-bbdb-c58ac71bf48b
ms.openlocfilehash: e2d4a235737789f2f5852c00218427613db3d558
ms.sourcegitcommit: 1572b615c8f863be4986c23ea2ff7642b02bc605
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/10/2019
ms.locfileid: "67786318"
---
# <a name="monitor-at-scale-by-using-azure-monitor"></a>Controleren op schaal met behulp van Azure Monitor

Azure Backup biedt [ingebouwde controle- en waarschuwingsfuncties](backup-azure-monitoring-built-in-monitor.md) in een Recovery Services-kluis. Deze mogelijkheden zijn beschikbaar zonder eventuele extra beheerinfrastructuur. Maar deze ingebouwde service is beperkt in de volgende scenario's:

- Als u gegevens uit meerdere Recovery Services-kluizen tussen abonnementen controleren
- Als de gewenste meldingskanaal *niet* e-mailadres
- Als wilt dat gebruikers waarschuwingen voor meer scenario 's
- Als u bekijken van gegevens van een on-premises-onderdeel, zoals System Center Data Protection Manager in Azure, die in de portal niet wordt weergegeven wilt in [ **back-uptaken** ](backup-azure-monitoring-built-in-monitor.md#backup-jobs-in-recovery-services-vault) of [  **Back-Upwaarschuwingen**](backup-azure-monitoring-built-in-monitor.md#backup-alerts-in-recovery-services-vault)

## <a name="using-log-analytics-workspace"></a>Met behulp van Log Analytics-werkruimte

> [!NOTE]
> Gegevens van virtuele Azure-machines, de Azure backup-agent, System Center Data Protection Manager, SQL-back-ups in Azure VM's en back-ups van Azure Files-share is dat aan de Log Analytics-werkruimte via diagnostische instellingen. 

Voor het controleren op schaal, moet u de mogelijkheden van twee Azure-services. *Diagnostische instellingen* verzenden van gegevens vanuit meerdere Azure Resource Manager-resources naar een andere resource. *Log Analytics* genereert aangepaste waarschuwingen waar u actiegroepen gebruiken kunt voor het definiëren van andere meldingskanalen. 

De volgende secties bevatten informatie over het gebruik van Log Analytics voor het bewaken van Azure back-up op schaal.

### <a name="configure-diagnostic-settings"></a>Diagnostische instellingen configureren

Azure Resource Manager-resources, zoals de Recovery Services-kluis, vastleggen informatie over geplande bewerkingen en bewerkingen gebruiker heeft geactiveerd als diagnostische gegevens. 

Selecteer in de sectie bewaking **diagnostische instellingen** en geeft u het doel voor diagnostische gegevens van de Recovery Services-kluis.

![Diagnostische instelling van de Recovery Services-kluis, die gericht is op Log Analytics](media/backup-azure-monitoring-laworkspace/rs-vault-diagnostic-setting.png)

U kunt een Log Analytics-werkruimte van een ander abonnement doelgroep. Voor het controleren van kluizen voor abonnementen op één plek, selecteert u de dezelfde werkruimte van Log Analytics voor meerdere Recovery Services-kluizen. Als u wilt channel alle informatie die gerelateerd aan Azure Backup aan de Log Analytics-werkruimte, selecteert u **AzureBackupReport** als het logboek.

> [!IMPORTANT]
> Nadat u de configuratie te voltooien, moet u wachten 24 uur voor de initiële gegevens-push te voltooien. Nadat die voor de eerste gegevens-push, alle gebeurtenissen worden gepusht, zoals verderop in dit artikel wordt beschreven in de [sectie frequentie](#diagnostic-data-update-frequency).

### <a name="deploy-a-solution-to-the-log-analytics-workspace"></a>Een oplossing implementeren in de Log Analytics-werkruimte

Nadat de gegevens zich in de Log Analytics-werkruimte [een GitHub-sjabloon implementeren](https://azure.microsoft.com/resources/templates/101-backup-oms-monitoring/) naar Log Analytics om de gegevens te visualiseren. Voor het juist identificeren van de werkruimte, moet dat u wijs hieraan de dezelfde resourcegroep bevinden, naam van de werkruimte en de locatie. Vervolgens installeert u deze sjabloon in de werkruimte.

> [!NOTE]
> Als u geen waarschuwingen, back-uptaken of hersteltaken in uw Log Analytics-werkruimte, ziet u mogelijk een foutcode 'BadArgumentError' in de portal. Deze fout negeren en doorgaan met behulp van de oplossing. Nadat de relevante gegevens begint doorgestuurd naar de werkruimte, de visualisaties geldt dezelfde en u niet meer de volgende fout weergegeven.

### <a name="view-azure-backup-data-by-using-log-analytics"></a>Azure Backup-gegevens weergeven met behulp van Log Analytics

Nadat de sjabloon is geïmplementeerd, worden de oplossing voor het bewaken van Azure Backup in de werkruimte overzicht regio weergegeven. Ga naar de samenvatting van een van deze paden te volgen:

- **Azure Monitor**: In de **Insights** sectie, selecteer **meer** en kies vervolgens de relevante werkruimte.
- **Log Analytics-werkruimten**: Selecteer de relevante werkruimte, en klik vervolgens onder **algemene**, selecteer **werkruimte overzicht**.

![De tegel van Log Analytics-bewaking](media/backup-azure-monitoring-laworkspace/la-azurebackup-azuremonitor-tile.png)

Wanneer u de bewaking tegel selecteert, wordt in de ontwerpfunctie sjabloon wordt een reeks grafieken over eenvoudige bewakingsgegevens geopend vanuit Azure Backup. Hier volgen enkele van de grafieken u ziet:

* Alle back-uptaken

   ![Log Analytics grafieken voor back-uptaken](media/backup-azure-monitoring-laworkspace/la-azurebackup-allbackupjobs.png)

* Herstellen van taken

   ![Log Analytics-grafiek voor het herstellen van taken](media/backup-azure-monitoring-laworkspace/la-azurebackup-restorejobs.png)

* Ingebouwde back-up van Azure-waarschuwingen voor Azure-resources

   ![Log Analytics-grafiek voor ingebouwde back-up van Azure-waarschuwingen voor Azure-resources](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts.png)

* Ingebouwde back-up van Azure-waarschuwingen voor on-premises bronnen

   ![Log Analytics-grafiek voor ingebouwde back-up van Azure-waarschuwingen voor on-premises bronnen](media/backup-azure-monitoring-laworkspace/la-azurebackup-activealerts-onprem.png)

* Actieve gegevensbronnen

   ![Log Analytics-grafiek voor actieve back-up-entiteiten](media/backup-azure-monitoring-laworkspace/la-azurebackup-activedatasources.png)

* Recovery Services-kluis opslag in de cloud

   ![Log Analytics-grafiek voor cloudopslag voor Recovery Services-kluis](media/backup-azure-monitoring-laworkspace/la-azurebackup-cloudstorage-in-gb.png)

De grafieken worden geleverd met de sjabloon. U kunt de grafieken te bewerken of meer grafieken toevoegen als u wilt.

> [!IMPORTANT]
> Wanneer u de sjabloon implementeert, maakt u in feite een alleen-lezen vergrendeld. Als u wilt bewerken en opslaan van de sjabloon, moet u de vergrendeling te verwijderen. Kunt u een vergrendeling in de **instellingen** sectie van de Log Analytics-werkruimte, op de **Hiermee vergrendelt u** deelvenster.

### <a name="create-alerts-by-using-log-analytics"></a>Waarschuwingen maken met behulp van Log Analytics

U kunt uw eigen waarschuwingen in Log Analytics-werkruimte maken in Azure Monitor. In de werkruimte, gebruikt u *Azure actiegroepen* uw voorkeur meldingsmechanisme selecteren. 

> [!IMPORTANT]
> Zie voor informatie over de kosten voor het maken van deze query, [prijzen van Azure Monitor](https://azure.microsoft.com/pricing/details/monitor/).

Selecteer een of meer grafieken om te openen de **logboeken** sectie van de Log Analytics-werkruimte. In de **logboeken** sectie, de query's bewerken en waarschuwingen maken op deze.

![Een waarschuwing in Log Analytics-werkruimte maken](media/backup-azure-monitoring-laworkspace/la-azurebackup-customalerts.png)

Wanneer u selecteert **nieuwe waarschuwingsregel**, de pagina van Azure Monitor het maken van de waarschuwing wordt geopend, zoals wordt weergegeven in de volgende afbeelding. De resource is hier al gemarkeerd als de Log Analytics-werkruimte en integratie met actie wordt aangeboden.

![De pagina van de Log Analytics-waarschuwing-maken](media/backup-azure-monitoring-laworkspace/inkedla-azurebackup-createalert.jpg)

#### <a name="alert-condition"></a>De voorwaarde voor waarschuwing

De onderscheidende kenmerken van een waarschuwing is de activerende voorwaarde. Selecteer **voorwaarde** automatisch laden van de Kusto-query op de **logboeken** pagina zoals in de volgende afbeelding. Hier kunt u de voorwaarde op basis van uw behoeften. Zie voor meer informatie, [voorbeeld Kusto-query's](#sample-kusto-queries).

![De voorwaarde voor een waarschuwing instellen](media/backup-azure-monitoring-laworkspace/la-azurebackup-alertlogic.png)

Indien nodig, kunt u de Kusto-query bewerken. Kies een drempelwaarde, de periode en de frequentie. De drempelwaarde bepaalt wanneer de waarschuwing wordt gegeven. De periode is de periode waarin de query wordt uitgevoerd. Bijvoorbeeld, als de drempelwaarde groter dan 0 is, de periode 5 minuten is en de frequentie 5 minuten is, klikt u vervolgens de regel wordt uitgevoerd de query elke 5 minuten controleren van de vorige vijf minuten. Als het aantal resultaten groter dan 0 is, krijgt u een melding via de geselecteerde actiegroep.

#### <a name="alert-action-groups"></a>Actie bij waarschuwing groepen

Een actiegroep gebruiken om een meldingskanaal. Om te zien van de beschikbare meldingsmechanismen onder **actiegroepen**, selecteer **nieuw**.

![Beschikbare meldingsmechanismen in het venster 'Actiegroep toevoegen'](media/backup-azure-monitoring-laworkspace/LA-AzureBackup-ActionGroup.png)

U kunt voldoen aan alle waarschuwingen en bewaking van de vereisten van Log Analytics-zelfstandig of u Log Analytics kunt gebruiken om te voorzien van ingebouwde meldingen.

Zie voor meer informatie, [maken, weergeven en beheren van waarschuwingen via Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-log) en [maken en beheren van actiegroepen in Azure portal](https://docs.microsoft.com/azure/azure-monitor/platform/action-groups).

### <a name="sample-kusto-queries"></a>Voorbeeld Kusto-query 's

De standaardgrafieken bieden u Kusto-query's voor algemene scenario's waarvoor u waarschuwingen kunt bouwen. U kunt ook de query om de gegevens die u wilt worden gewaarschuwd over wijzigen. Plak het volgende voorbeeld Kusto-query's in de **logboeken** pagina en maak vervolgens waarschuwingen op de query's:

* Alle geslaagde back-uptaken

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Completed"
    ````
    
* Alle mislukte back-uptaken

    ````Kusto
    AzureDiagnostics
    | where Category == "AzureBackupReport"
    | where SchemaVersion_s == "V2"
    | where OperationName == "Job" and JobOperation_s == "Backup"
    | where JobStatus_s == "Failed"
    ````
    
* Alle succesvolle Azure-VM back-uptaken

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

* Alle succesvolle SQL logboek back-uptaken

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

* Alle succesvolle Azure Backup agent-taken

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

De diagnostische gegevens uit de kluis is dat aan de Log Analytics-werkruimte met enige vertraging. Elke gebeurtenis bij de Log Analytics-werkruimte aankomt *20-30 minuten* nadat wordt deze doorgestuurd vanaf de Recovery Services-kluis. Hier vindt u meer informatie over de vertraging:

- Voor alle oplossingen worden de ingebouwde waarschuwingen van de Backup-service worden gepusht zodra ze worden gemaakt. Zodat ze doorgaans worden weergegeven in de Log Analytics-werkruimte na 20-30 minuten.
- Voor alle oplossingen ad-hoc back-uptaken en het herstellen van taken worden gepusht zo snel als ze *voltooien*.
- Voor alle oplossingen met uitzondering van de SQL-back-up, geplande back-uptaken zo snel als ze worden gepusht *voltooien*.
- Voor back-up van SQL, omdat elke 15 minuten, de logboekback-ups vindt is gegevens voor alle de voltooide geplande back-uptaken, met inbegrip van Logboeken, batch verwerkt en gepusht om de 6 uur.
- Voor alle oplossingen, andere informatie zoals de back-upitem, beleid, herstelpunten, opslag, enzovoort ten minste wordt gepusht *één keer per dag.*
- Een wijziging in de back-upconfiguratie (zoals het wijzigen van beleid of het bewerken van beleid) wordt geactiveerd voor een push van alle gerelateerde back-upgegevens.

## <a name="using-the-recovery-services-vaults-activity-logs"></a>Met behulp van de Recovery Services-kluis-activiteitenlogboeken

> [!CAUTION]
> De volgende stappen gelden alleen voor *back-ups van virtuele Azure-machine.* U kunt deze stappen niet gebruiken voor oplossingen zoals de Azure backup-agent, SQL-back-ups in Azure of Azure Files.

U kunt ook de activiteitenlogboeken gebruiken om op te halen van de melding voor gebeurtenissen, zoals back-up geslaagd. Volg deze stappen om te beginnen:

1. Meld u aan bij de Azure-portal.
1. Open de relevante Recovery Services-kluis. 
1. Open in de eigenschappen van de kluis, de **activiteitenlogboek** sectie.

Om te bepalen het juiste logboek en een waarschuwing maken:

1. Controleer of dat u hebt ontvangen activiteitenlogboeken voor geslaagde back-ups met de filters weergegeven in de volgende afbeelding. Wijzig de **Timespan** waarde indien nodig om records weer te geven.

   ![Filteren activiteitenlogboeken vinden die voor back-ups van virtuele Azure-machine](media/backup-azure-monitoring-laworkspace/activitylogs-azurebackup-vmbackups.png)

1. Selecteer de naam van de bewerking om de relevante details te bekijken.
1. Selecteer **nieuwe waarschuwingsregel** openen de **maken regel** pagina. 
1. Maken van een waarschuwing met de volgende stappen in [maken, weergeven en beheren van waarschuwingen voor activiteitenlogboeken via Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-activity-log).

   ![Nieuwe waarschuwingsregel](media/backup-azure-monitoring-laworkspace/new-alert-rule.png)

De resource is hier de Recovery Services-kluis zelf. U moet dezelfde stappen herhalen voor alle van de kluizen waarin u wilt een melding via activiteitenlogboeken. De voorwaarde geen een drempelwaarde, punten of frequentie omdat deze waarschuwing is gebaseerd op gebeurtenissen. Zodra het relevante activiteitenlogboek wordt gegenereerd, wordt de waarschuwing wordt gegeven.

## <a name="using-log-analytics-to-monitor-at-scale"></a>Met behulp van Log Analytics om te controleren op schaal

U kunt alle waarschuwingen die zijn gemaakt op basis van activiteitenlogboeken en Log Analytics-werkruimten in Azure Monitor weergeven. Open meteen de **waarschuwingen** in het linkerdeelvenster.

Hoewel u meldingen via activiteitenlogboeken ophalen kunt, sterk aangeraden Log Analytics in plaats van activiteitenlogboeken gebruiken voor bewaking op schaal. Dit is de reden:

- **Beperkte scenario's**: Meldingen via activiteitenlogboeken gelden alleen voor back-ups van virtuele Azure-machine. De meldingen moeten worden ingesteld voor elke Recovery Services-kluis.
- **Definitie past**: De geplande back-activiteit voldoen niet aan de meest recente definitie van activiteitenlogboeken. In plaats daarvan worden uitgelijnd met [diagnostische logboeken](https://docs.microsoft.com/azure/azure-monitor/platform/diagnostic-logs-overview#what-you-can-do-with-diagnostic-logs). Deze afstemming zorgt ervoor dat onverwachte effecten wanneer de gegevens die via de activiteit zich kanaalwijzigingen.
- **Problemen met de activiteit log kanaal**: Activiteitenlogboeken die vanuit Azure Backup worden gepompt Volg een nieuw model in Recovery Services-kluizen. Helaas, is deze wijziging geldt voor het genereren van activiteitenlogboeken in Azure Government, Azure Duitsland en Azure China 21Vianet. Als gebruikers van deze cloudservices maken of configureren van waarschuwingen van activiteitenlogboeken in Azure Monitor, worden de waarschuwingen worden niet geactiveerd. Ook wordt in alle Azure openbare regio's, als een gebruiker [Recovery Services-activiteitenlogboeken in een werkruimte voor Log Analytics verzamelt](https://docs.microsoft.com/azure/azure-monitor/platform/collect-activity-logs), deze logboeken worden niet weergegeven.

Gebruik een Log Analytics-werkruimte voor bewaking en waarschuwingen op schaal voor uw workloads die worden beveiligd door Azure Backup.

## <a name="next-steps"></a>Volgende stappen

Zie voor het maken van aangepaste query's, [Log Analytics-gegevensmodel](backup-azure-log-analytics-data-model.md).
