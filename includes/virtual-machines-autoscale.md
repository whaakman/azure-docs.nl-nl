U kunt eenvoudig [automatisch schalen](../articles/monitoring-and-diagnostics/insights-autoscale-best-practices.md) uw [virtuele machines (VM's)](../articles/virtual-machines/windows/overview.md) bij het gebruik [virtuele-machineschaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) en de [functie voor automatisch schalen van Azure Monitor](../articles/monitoring-and-diagnostics/monitoring-overview-autoscale.md). Uw virtuele machines moeten lid zijn van een schaalset automatisch worden geschaald. In dit artikel bevat informatie waarmee u beter te begrijpen hoe u uw virtuele machines met behulp van automatische en handmatige methoden verticaal en horizontaal schalen.

## <a name="horizontal-or-vertical-scaling"></a>Horizontaal of verticaal schalen

De functie voor automatisch schalen van Azure Monitor alleen kan worden geschaald horizontaal, dit is een toename ('uit') of te verlagen ('in') van het aantal virtuele machines. Horizontaal schalen biedt meer flexibiliteit in het geval van een cloud omdat hiermee om uit te voeren mogelijk duizenden VM's om belasting te verwerken. U schalen horizontaal door automatisch of handmatig wijzigen van de capaciteit (of het aantal exemplaren) van de schaalset. 

Verticaal schalen blijft hetzelfde aantal virtuele machines, maar is de virtuele machines ("u") meer of minder ('down') krachtige. Power wordt gemeten in kenmerken, zoals geheugen, CPU-snelheid of schijfruimte. Verticaal schalen is afhankelijk van de beschikbaarheid van grotere hardware die snel komt binnen via een bovenlimiet en per regio kan verschillen. Verticaal schalen, moet ook meestal een virtuele machine te stoppen en opnieuw starten. U schaalt verticaal door in te stellen van een nieuwe grootte in de configuratie van de virtuele machines in de schaalset.

Met behulp van runbooks in [Azure Automation](../articles/automation/automation-intro.md), kunt u eenvoudig [schalen van virtuele machines in een schaalset](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision.md) omhoog of omlaag.

## <a name="create-a-virtual-machine-scale-set"></a>Een virtuele-machineschaalset maken

Schaalsets maken het eenvoudig te implementeren en beheren van identieke VM's als een set. U kunt Linux of Windows-schaalsets met de [Azure-portal](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md), [Azure PowerShell](../articles/virtual-machines/windows/tutorial-create-vmss.md), of de [Azure CLI](../articles/virtual-machines/linux/tutorial-create-vmss.md). U kunt ook maken en beheren van schaalsets met SDK's, zoals [Python](/develop/python) of [Node.js](/nodejs/azure), of rechtstreeks met de [REST-API's](/rest/api/compute/virtualmachinescalesets). Automatisch schalen van virtuele machines wordt gerealiseerd met metrische gegevens en regels toepassen op de schaalset.

## <a name="configure-autoscale-for-a-scale-set"></a>Automatisch schalen configureren voor een schaalset

Automatisch schalen biedt het juiste aantal VM's voor het afhandelen van de belasting van uw toepassing. Hiermee kunt u VM's voor het verwerken van de belasting toeneemt en bespaar geld door het verwijderen van virtuele machines die niet-actieve zit toevoegen. U een minimum en maximum aantal virtuele machines worden uitgevoerd op basis van een set regels. Met een minimale maakt ervoor dat uw toepassing altijd wordt uitgevoerd zelfs zonder enige belasting. Met een maximumwaarde die mogelijk van uw totale kosten per uur worden beperkt.

U kunt automatisch schalen inschakelen bij het maken van de schaalset met behulp van [Azure PowerShell](../articles/monitoring-and-diagnostics/insights-powershell-samples.md#create-and-manage-autoscale-settings) of [Azure CLI](https://docs.microsoft.com/cli/azure/monitor/autoscale-settings). U kunt deze ook inschakelen nadat de schaalset is gemaakt. U kunt een schaalset maken, de uitbreiding te installeren en configureren voor automatisch schalen met behulp van een [Azure Resource Manager-sjabloon](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md). Automatisch schalen van Azure Monitor inschakelen in Azure portal, of automatisch schalen van de set schaalinstellingen inschakelen.

![Automatisch schalen inschakelen](./media/virtual-machines-autoscale/virtual-machines-autoscale-enable.png)
 
### <a name="metrics"></a>Metrische gegevens

De functie voor automatisch schalen van Azure Monitor kunt u voor het schalen van het aantal actieve VM's omhoog of omlaag op basis van [metrische gegevens](../articles/monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Standaard voorzien VM's in eenvoudige metrische gegevens voor de host op schijf-, netwerk- en CPU-gebruik. Wanneer u het verzamelen van diagnostische gegevens met de diagnostische extensie configureert, wordt extra Gast OS-prestatiemeteritems voor de schijf, de CPU en geheugen beschikbaar komen.

![Metrische criteria](./media/virtual-machines-autoscale/virtual-machines-autoscale-criteria.png)

Als de toepassing nodig heeft om te schalen op basis van metrische gegevens die niet beschikbaar is via de host, wordt de virtuele machines in de schaalset moeten beschikken over een de [Linux diagnostic-extensie](../articles/virtual-machines/linux/diagnostic-extension.md) of [Windows diagnostics-extensie](../articles/virtual-machines/windows/ps-extensions-diagnostics.md)geïnstalleerd. Als u een schaalset met behulp van Azure portal maakt, moet u ook Azure PowerShell of Azure CLI gebruiken om de extensie installeren met de configuratie van diagnostische gegevens die u nodig hebt.
 
### <a name="rules"></a>Regels

[Regels](../articles/monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) metrische gegevens worden gecombineerd met een actie die moet worden uitgevoerd. Wanneer de regelvoorwaarden wordt voldaan, kan een of meer acties voor automatisch schalen worden geactiveerd. Bijvoorbeeld, mogelijk hebt u een regel gedefinieerd waarmee het aantal virtuele machines met 1 wordt verhoogd wanneer de gemiddelde CPU-gebruik 85 procent opgegeven.

![Acties voor automatisch schalen](./media/virtual-machines-autoscale/virtual-machines-autoscale-actions.png)
 
### <a name="notifications"></a>Meldingen

U kunt [activeerbare](../articles/monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) zodat specifieke web URL's zijn met de naam of e-mailberichten worden verzonden op basis van de regels voor automatisch schalen die u maakt. Webhooks kunt u de Azure-waarschuwingen doorsturen naar andere systemen voor na verwerking of aangepaste meldingen.

## <a name="manually-scale-vms-in-a-scale-set"></a>Handmatig schalen van virtuele machines in een schaalset

### <a name="horizontal"></a>Horizontaal

U kunt toevoegen of verwijderen van virtuele machines door het wijzigen van de capaciteit van de schaalset. In de Azure-portal, kunt u verkleinen of verhoog het aantal virtuele machines (weergegeven als **aantal instanties**) in de schaalset door te schuiven de onderdrukking voorwaarde-balk op het scherm schalen naar links of rechts.

Met Azure PowerShell, moet u de scale set object met behulp van [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss). Vervolgens stelt u de **sku.capacity** eigenschap in op het aantal virtuele machines die u wilt en bijwerken van de schaalset met [Update-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss). Met Azure CLI kunt u de capaciteit met de **--nieuwe capaciteit** parameter voor de [az vmss scale](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) opdracht.

### <a name="vertical"></a>Verticaal

U kunt de grootte van de virtuele machines in Azure portal op het scherm grootte voor de schaalset handmatig wijzigen. U kunt Azure PowerShell gebruiken met Get-AzureRmVmss, instellen van de sku-eigenschap van de installatiekopie-verwijzing en vervolgens met behulp van [Update-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) en [Update-AzureRmVmssInstance](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over schaalsets in [ontwerpoverwegingen voor Schaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md).

