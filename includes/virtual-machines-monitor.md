U kunt profiteren van tal van mogelijkheden om uw virtuele machines bewaken met het verzamelen, weergeven en analyseren van diagnostische en gegevens vastleggen. Eenvoudig doen [bewaking](../articles/monitoring-and-diagnostics/monitoring-overview-azure-monitor.md) van uw virtuele machine, kunt u het scherm overzicht voor de virtuele machine in de Azure portal. U kunt [extensies](../articles/virtual-machines/windows/extensions-features.md) diagnostische gegevens configureren op uw virtuele machines voor het verzamelen van aanvullende metrische gegevens. Ook kunt u meer geavanceerde controle-opties, zoals [Application Insights](../articles/application-insights/app-insights-overview.md) en [logboekanalyse](../articles/log-analytics/log-analytics-overview.md).

## <a name="diagnostics-and-metrics"></a>Diagnostische gegevens en metrische gegevens 

U kunt instellen en controleren van de verzameling van [diagnostics-gegevens](https://docs.microsoft.com/cli/azure/vm/diagnostics) met [metrische gegevens](../articles/monitoring-and-diagnostics/monitoring-overview-metrics.md) in de Azure-portal, de Azure CLI, Azure PowerShell en programming toepassingen Programming Interfaces (API's). U kunt bijvoorbeeld:

- **Houd rekening met basismetrieken voor de virtuele machine.** Op het scherm overzicht van de Azure portal zijn de basismetrieken weergegeven CPU-gebruik, netwerkgebruik, totaalaantal bytes van de schijf en schijfbewerkingen per seconde.

- **Schakel het verzamelen van diagnostische gegevens over opstarten en weergeven met behulp van de Azure-portal.** Wanneer u uw eigen installatiekopie brengt naar Azure of een van de platform-installatiekopieën zelfs wordt opgestart, kunnen er diverse redenen waarom een virtuele machine in een niet-opstartbare status opgehaald. U kunt eenvoudig diagnostische gegevens over opstarten inschakelen wanneer u een virtuele machine maken door te klikken op **ingeschakeld** voor diagnostische gegevens over opstarten onder de sectie bewaking van het scherm.

    Als VMs opstart, wordt de diagnostische boot-agent worden vastgelegd opstarten uitvoer en opgeslagen in Azure storage. Deze gegevens kunnen worden gebruikt met virtuele machine opstarten oplossen. Diagnostische gegevens over opstarten zijn niet automatisch ingeschakeld wanneer u een virtuele machine van de opdrachtregelprogramma's maakt. Voordat u diagnostische gegevens over opstarten inschakelt, moet een opslagaccount worden gemaakt voor het opslaan van de logboeken van de opstartinstallatiekopie. Als u diagnostische gegevens over opstarten in de Azure portal inschakelt, wordt automatisch een opslagaccount voor u gemaakt.

    Als u niet diagnostische gegevens over opstarten inschakelen wanneer de virtuele machine is gemaakt, kunt u altijd inschakelen deze later met behulp van [Azure CLI](https://docs.microsoft.com/cli/azure/vm/boot-diagnostics), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmbootdiagnostics), of een [Azure Resource Manager-sjabloon](../articles/virtual-machines/windows/extensions-diagnostics-template.md).

- **Schakel het verzamelen van Gast OS diagnostics-gegevens.** Wanneer u een virtuele machine maakt, hebt u de mogelijkheid in het scherm Gast OS diagnostische gegevens inschakelen. Wanneer u het verzamelen van diagnostische gegevens, schakel de [IaaSDiagnostics-extensie voor Linux](../articles/virtual-machines/linux/diagnostic-extension.md) of de [IaaSDiagnostics-extensie voor Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md) wordt toegevoegd aan de virtuele machine waarmee u voor het verzamelen van aanvullende schijf, CPU en geheugen gegevens.

    Diagnostische gegevens van verzamelde gegevens gebruikt, kunt u automatisch schalen configureren voor uw virtuele machines. U kunt ook de logboeken voor het opslaan van de gegevens en waarschuwingen instellen om te laten u weten wanneer prestaties niet juist configureren.

## <a name="alerts"></a>Waarschuwingen

U kunt maken [waarschuwingen](../articles/monitoring-and-diagnostics/monitoring-overview-alerts.md) op basis van specifieke parameters. Voorbeelden van de problemen die kunnen worden gewaarschuwd over zijn wanneer de gemiddelde CPU-gebruik een bepaalde drempelwaarde overschrijdt, of de beschikbare vrije schijfruimte zakt onder een bepaalde hoeveelheid. Waarschuwingen kunnen worden geconfigureerd in de [Azure-portal](../articles/monitoring-and-diagnostics/insights-alerts-portal.md)met [Azure PowerShell](../articles/monitoring-and-diagnostics/insights-alerts-powershell.md), of de [Azure CLI](../articles/monitoring-and-diagnostics/insights-alerts-command-line-interface.md).

## <a name="azure-service-health"></a>Azure Service Health

[Status van de Azure-Service](../articles/service-health/service-health-overview.md) persoonlijke hulp en ondersteuning biedt, wanneer u invloed hebben op problemen in de Azure-services en helpt bij het voorbereiden voor toekomstige gepland onderhoud. Status van de Azure-Service waarschuwt u en uw teams gerichte en flexibele meldingen gebruiken.

## <a name="azure-resource-health"></a>Azure Resource Health

[Azure resourcestatus](../articles/service-health/resource-health-overview.md) helpt u bij het diagnosticeren en ondersteuning krijgen wanneer een Azure probleem van invloed is op uw resources. Het biedt u informatie over de huidige en eerdere status van uw resources en helpt u problemen verhelpen. Resource Health biedt technische ondersteuning als u hulp nodig heeft bij problemen met Azure-services.

## <a name="logs"></a>Logboeken

De [Azure Activity Log](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md) is een abonnementlogboek die biedt inzicht in het abonnement op gebeurtenissen die hebben plaatsgevonden in Azure. Het logboek bevat een reeks gegevens van operationele gegevens van de Azure Resource Manager-updates op Service Health-gebeurtenissen. U kunt activiteitenlogboek klikken in de Azure portal het logboek voor uw virtuele machine weergeven.

Enkele dingen die u met het activiteitenlogboek doen kunt:

- Maak een [waarschuwen bij een gebeurtenis activiteitenlogboek](../articles/monitoring-and-diagnostics/monitoring-overview-activity-logs.md).
- [Deze stream naar een Event Hub](../articles/monitoring-and-diagnostics/monitoring-stream-activity-logs-event-hubs.md) voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.
- Analyseren in Power BI met behulp van de [Power BI-inhoudspakket](https://powerbi.microsoft.com/documentation/powerbi-content-pack-azure-audit-logs/).
- [Sla het op een opslagaccount](../articles/monitoring-and-diagnostics/monitoring-archive-activity-log.md) voor archivering of handmatig inspectie. U kunt de retentietijd (in dagen) met behulp van het logboek-profiel opgeven.

U kunt ook logboekgegevens activiteit openen met behulp van [Azure PowerShell](https://docs.microsoft.com/powershell/module/azurerm.insights/), wordt de [Azure CLI](https://docs.microsoft.com/cli/azure/monitor), of [Monitor REST-API's](https://docs.microsoft.com/rest/api/monitor/).

[Azure diagnostische logboeken](../articles/monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md) logboeken die door uw virtuele machine waarvan de rijke, regelmatig gegevens over de werking ervan. Diagnostische logboeken verschillen van het activiteitenlogboek door biedt meer informatie over de bewerkingen die zijn uitgevoerd vanuit de virtuele machine.

Enkele dingen die u met Logboeken met diagnostische gegevens doen kunt:

- [Deze opslaan op een opslagaccount](../articles/monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md) voor inspectie controle of handmatig. U kunt de retentietijd (in dagen) met behulp van de diagnostische instellingen Resource opgeven.
- [Deze stream naar Event Hubs](../articles/monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) voor opname door een service van derden of aangepaste analytics-oplossing zoals Power BI.
- Analyseer ze met [OMS Log Analytics](../articles/log-analytics/log-analytics-azure-storage.md).

## <a name="advanced-monitoring"></a>Geavanceerde controle

- [Operations Management Suite (OMS)](https://docs.microsoft.com/azure/operations-management-suite/) biedt mogelijkheden voor bewaking, waarschuwingen en waarschuwing herstel tussen cloud en on-premises activa. U kunt een uitbreiding installeren op een [Linux VM](../articles/virtual-machines/linux/extensions-oms.md) of een [Windows VM](../articles/virtual-machines/windows/extensions-oms.md) die de OMS-agent installeert en registreert u de virtuele machine naar een bestaande OMS-werkruimte.

- [Meld u Analytics](../articles/log-analytics/log-analytics-overview.md) is een service in OMS die controleert uw cloud en on-premises omgevingen voor het onderhouden van de beschikbaarheid en prestaties. De service verzamelt gegevens afkomstig van resources in uw cloud- en on-premises omgevingen en van andere bewakingsprogramma's om analyse over meerdere resources aan te bieden.

    Voor Windows en Linux-machines is de aanbevolen methode voor het verzamelen van Logboeken en metrische gegevens door het installeren van de agent Log Analytics. De eenvoudigste manier om de Log Analytics-agent installeren op een virtuele machine is via de [Log Analytics VM-extensie](../articles/log-analytics/log-analytics-azure-vm-extension.md). Met de extensie, vereenvoudigt u het installatieproces en configureert automatisch de agent voor het verzenden van gegevens naar de werkruimte voor logboekanalyse die u opgeeft. De agent is ook automatisch bijgewerkt, hebt u de nieuwste functies en oplossingen.

- [Netwerk-Watcher](../articles/network-watcher/network-watcher-monitoring-overview.md) kunt u voor het bewaken van uw virtuele machine en de bijbehorende bronnen die betrekking heeft op het netwerk dat ze in. U kunt de uitbreiding van de netwerk-Watcher-Agent installeren op een [Linux VM](../articles/virtual-machines/linux/extensions-nwa.md) of een [Windows VM](../articles/virtual-machines/windows/extensions-nwa.md).

## <a name="next-steps"></a>Volgende stappen
- Doorloop de stappen in [bewaken van een virtuele Windows-Machine met Azure PowerShell](../articles/virtual-machines/windows/tutorial-monitoring.md) of [bewaken van een virtuele Linux-Machine met de Azure CLI](../articles/virtual-machines/linux/tutorial-monitoring.md).
- Meer informatie over de aanbevolen procedures rond [controle en diagnostische gegevens](https://docs.microsoft.com/azure/architecture/best-practices/monitoring).
