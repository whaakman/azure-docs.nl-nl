U kunt gemakkelijk [automatisch schalen](../articles/monitoring-and-diagnostics/insights-autoscale-best-practices.md) uw [(virtuele machines)](../articles/virtual-machines/windows/overview.md) als u werkt met [virtuele-machineschaalsets](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) en de [functie voor automatisch schalen van Azure Monitor](../articles/monitoring-and-diagnostics/monitoring-overview-autoscale.md). Uw VM's moeten lid zijn van een schaal ingesteld op automatisch worden geschaald. In dit artikel bevat informatie waarmee u beter te begrijpen hoe uw virtuele machines met behulp van automatische en handmatige methoden zowel horizontaal en verticaal te schalen.

## <a name="horizontal-or-vertical-scaling"></a>Horizontale of verticale schalen

De functie voor automatisch schalen van Azure Monitor alleen schaalt horizontaal, die een stijging ('uit') of te verlagen ('in') van het aantal virtuele machines. Horizontaal schalen biedt meer flexibiliteit in een situatie met een cloud omdat deze kunt u mogelijk duizenden virtuele machines om belasting te verwerken uitvoeren. U horizontaal schalen door automatisch of handmatig wijzigen van de capaciteit (of het aantal exemplaren) van de de schaalaanpassingsset. 

Verticale schaling blijft hetzelfde aantal virtuele machines, maar de virtuele machines maakt ('actief') meer of minder ('omlaag') krachtige. Power wordt gemeten in kenmerken, zoals geheugen, CPU-snelheid of schijfruimte. Verticale schaling is afhankelijk van de beschikbaarheid van grotere hardware, waarbij snel een bovenlimiet treffers en kan verschillen per regio. Verticale schaling, moet ook meestal een virtuele machine stoppen en opnieuw starten. U schalen verticaal door het instellen van een nieuwe grootte in de configuratie van de virtuele machines in de schaalset.

Met behulp van runbooks in [Azure Automation](../articles/automation/automation-intro.md), kunt u gemakkelijk [schalen van virtuele machines in een set scale](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-vertical-scale-reprovision.md) omhoog of omlaag.

## <a name="create-a-virtual-machine-scale-set"></a>Maken van een virtuele-machineschaalset

-Schaalsets kunnen gemakkelijk te implementeren en beheren van identieke virtuele machines als een set. U kunt Linux maken of schaal voor Windows wilt instellen met de [Azure-portal](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md), [Azure PowerShell](../articles/virtual-machines/windows/tutorial-create-vmss.md), of de [Azure CLI](../articles/virtual-machines/linux/tutorial-create-vmss.md). U kunt ook maken en beheren-schaalsets met SDK's, zoals [Python](/develop/python) of [Node.js](/nodejs/azure), of rechtstreeks met de [REST-API's](/rest/api/compute/virtualmachinescalesets). Automatische schaling van virtuele machines wordt gerealiseerd door het toepassen van regels en metrische gegevens aan de schaalaanpassingsset.

## <a name="configure-autoscale-for-a-scale-set"></a>Voor een scale-set voor automatisch schalen configureren

Automatische schaling, biedt het juiste aantal virtuele machines voor het afhandelen van de belasting van uw toepassing. Hiermee kunt u virtuele machines voor het verwerken van de toename in laden en geld besparen door het verwijderen van virtuele machines die niet-actieve zit toevoegen. U opgeven een minimum en maximum aantal virtuele machines worden uitgevoerd op basis van een reeks regels. Met een minimale zorgt ervoor dat uw toepassing wordt altijd uitgevoerd zelfs geen load. Met een maximale waarde beperkt mogelijk van uw totale per uur kosten.

U kunt automatisch schalen inschakelen bij het maken van de schaal instelt met behulp van [Azure PowerShell](../articles/monitoring-and-diagnostics/insights-powershell-samples.md#create-and-manage-autoscale-settings) of [Azure CLI](https://docs.microsoft.com/cli/azure/monitor/autoscale-settings). U kunt deze ook inschakelen nadat de schaalaanpassingsset is gemaakt. Maken van een schaalset, de uitbreiding te installeren en configureren voor automatisch schalen met behulp van een [Azure Resource Manager-sjabloon](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-windows-autoscale.md). Inschakelen van de Azure-Monitor voor automatisch schalen in de Azure portal of automatisch schalen van de set schaalinstellingen inschakelen.

![Schakel automatisch schalen](./media/virtual-machines-autoscale/virtual-machines-autoscale-enable.png)
 
### <a name="metrics"></a>Metrische gegevens

De functie voor automatisch schalen van Azure Monitor kunt u het aantal virtuele machines met omhoog schalen of omlaag gaat op basis van [metrische gegevens](../articles/monitoring-and-diagnostics/insights-autoscale-common-metrics.md). Standaard voorzien VM's basismetrieken hostniveau schijf-, netwerk- en CPU-gebruik. Wanneer u het verzamelen van diagnostische gegevens met de extensie voor diagnostische configureert, worden extra Gast OS-prestatiemeteritems beschikbaar voor de schijf, CPU en geheugen.

![Metrische criteria](./media/virtual-machines-autoscale/virtual-machines-autoscale-criteria.png)

Als uw toepassing moet schalen op basis van metrische gegevens die niet beschikbaar is via de host, moet de virtuele machines in de schaalset hebben ofwel de [Linux-extensie voor diagnostische](../articles/virtual-machines/linux/diagnostic-extension.md) of [extensie voor diagnostische gegevens van Windows](../articles/virtual-machines/windows/ps-extensions-diagnostics.md)geïnstalleerd. Als u een schaal ingesteld met de Azure-portal maakt, moet u ook Azure PowerShell of Azure CLI gebruiken voor het installeren van de uitbreiding met de configuratie van diagnostische gegevens die u nodig hebt.
 
### <a name="rules"></a>Regels

[Regels](../articles/monitoring-and-diagnostics/monitoring-autoscale-scale-by-custom-metric.md) een metriek worden gecombineerd met een actie moet worden uitgevoerd. Wanneer de regelvoorwaarden wordt voldaan, worden een of meer acties voor automatisch schalen geactiveerd. U wellicht bijvoorbeeld een regel gedefinieerd waarmee het aantal virtuele machines met 1 verhoogd als het gemiddelde CPU-gebruik hoger dan 85 procent is.

![Acties voor automatisch schalen](./media/virtual-machines-autoscale/virtual-machines-autoscale-actions.png)
 
### <a name="notifications"></a>Meldingen

U kunt [activeerbare](../articles/monitoring-and-diagnostics/insights-autoscale-to-webhook-email.md) zodat specifieke web URL's worden genoemd of e-mailberichten worden verzonden op basis van de regels voor automatisch schalen die u maakt. Webhooks kunt u voor het routeren van de Azure waarschuwingsmeldingen met andere systemen voor na verwerking of aangepaste meldingen.

## <a name="manually-scale-vms-in-a-scale-set"></a>Handmatig schalen van virtuele machines in een schaalset

### <a name="horizontal"></a>Horizontaal

U kunt toevoegen of verwijderen van virtuele machines door het wijzigen van de capaciteit van de schaal is ingesteld. In de Azure portal, u kunt vergroten of verkleinen het aantal virtuele machines (weergegeven als **exemplaar aantal**) in de schaal ingesteld door de onderdrukking voorwaarde-balk op het scherm schalen naar links of rechts schuiven.

Met Azure PowerShell, moet u de schaal ingesteld object met [Get-AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/get-azurermvmss). Vervolgens stelt u de **sku.capacity** eigenschap aan het aantal virtuele machines die u wilt en werk de schaalset met [Update AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss). Met Azure CLI, u wijzigt de capaciteit met de **--nieuwe capaciteit** parameter voor de [az vmss scale](https://docs.microsoft.com/cli/azure/vmss#az_vmss_scale) opdracht.

### <a name="vertical"></a>Verticaal

U kunt de grootte van de virtuele machines in de Azure portal op het scherm grootte voor de schaalaanpassingsset handmatig wijzigen. U kunt Azure PowerShell gebruiken met Get-AzureRmVmss, de installatiekopie van het sku verwijzingseigenschap instellen en vervolgens via [Update AzureRmVmss](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmss) en [Update AzureRmVmssInstance](https://docs.microsoft.com/powershell/module/azurerm.compute/update-azurermvmssinstance).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over schaalsets in [ontwerpoverwegingen voor schaal ingesteld](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-design-overview.md).

