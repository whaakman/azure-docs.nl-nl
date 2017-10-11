| Resource | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Resources per [resourcegroep](../articles/azure-resource-manager/resource-group-overview.md#resource-groups) (per resourcetype) |800 |Varieert per resourcetype |
| Implementaties per resourcegroep |800 |800 |
| Resources per implementatie |800 |800 |
| De vergrendelingen Management (per unieke bereik) |20 |20 |
| Aantal Tags (per resource of resourcegroep) |15 |15 |
| Sleutellengte van label |512 |512 |
| De lengte van de tag-waarde |256 |256 |


#### <a name="template-limits"></a>Limieten voor sjabloon

| Waarde | Standaardlimiet | Maximumaantal |
| --- | --- | --- |
| Parameters |256 |256 |
| Variabelen |256 |256 |
| Bronnen (zoals aantal kopieën) |800 |800 |
| uitvoer |64 |64 |
| Sjabloonexpressie |24.576 tekens |24.576 tekens |
| Bronnen in de geëxporteerde sjablonen |200 |200 | 
| De grootte van de sjabloon |1 MB |1 MB |
| De parameter-bestandsgrootte |64 kB |64 kB |

U kunt sommige limieten sjabloon met een geneste sjabloon overschrijdt. Zie voor meer informatie [gekoppelde sjablonen gebruiken bij het implementeren van Azure-resources](../articles/azure-resource-manager/resource-group-linked-templates.md). Als u het aantal parameters en variabelen en uitvoer, kunt u verschillende waarden combineren in een object. Zie voor meer informatie [objecten als parameters](../articles/azure-resource-manager/resource-manager-objects-as-parameters.md).