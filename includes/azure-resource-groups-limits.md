| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Resources per [resourcegroep](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (per resourcetype) |800 |Verschilt per resourcetype |
| Implementaties per resourcegroep in de implementatiegeschiedenis |800 |800 |
| Resources per implementatie |800 |800 |
| Beheervergrendelingen (per unieke bereik) |20 |20 |
| Aantal Tags (per resource of resourcegroep) |15 |15 |
| Tag sleutellengte |512 |512 |
| De lengte van de tag-waarde |256 |256 |


#### <a name="template-limits"></a>Limieten voor sjabloon

| Waarde | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Parameters |256 |256 |
| Variabelen |256 |256 |
| Bronnen (zoals aantal kopieën) |800 |800 |
| Uitvoer |64 |64 |
| Sjabloonexpressie |24.576 tekens |24.576 tekens |
| Resources in de geëxporteerde sjablonen |200 |200 | 
| Sjabloongrootte |1 MB |1 MB |
| De grootte van parameter |64 kB |64 kB |

U kunt sommige limieten sjabloon met behulp van een geneste sjabloon overschrijdt. Zie voor meer informatie, [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](../articles/azure-resource-manager/resource-group-linked-templates.md). Als u wilt verkleinen het aantal parameters, variabelen of uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie, [objecten als parameters](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).

Als u de limiet van 800 implementaties per resourcegroep bereikt, verwijdert u implementaties uit de geschiedenis die niet meer nodig zijn. U kunt items verwijderen uit de geschiedenis met [az group deployment verwijderen](/cli/azure/group/deployment#az_group_deployment_delete) voor Azure CLI of [Remove-AzureRmResourceGroupDeployment](/powershell/module/azurerm.resources/remove-azurermresourcegroupdeployment) in PowerShell. Een vermelding verwijderen uit de implementatiegeschiedenis, heeft dit geen invloed op de resources implementeren. 