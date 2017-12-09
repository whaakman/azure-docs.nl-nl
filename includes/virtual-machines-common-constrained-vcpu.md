

Sommige werkbelastingen database zoals SQL Server- of Oracle vereisen veel geheugen, opslag, en i/o-bandbreedte, maar niet op een hoog core-telling. Veel werkbelastingen van de database zijn geen CPU-intensief. Azure biedt bepaalde VM-grootten waar u het aantal VM vCPU's te verlagen de kosten van de software licensing behoud van hetzelfde geheugen-, opslag- en i/o-bandbreedte kunt beperken.

Het aantal vCPU kan worden beperkt tot kwartaal helft of een van de oorspronkelijke VM-grootte. Deze nieuwe VM-grootten een achtervoegsel hebben die het aantal actieve vcpu's geeft zodat ze gemakkelijker kunt identificeren.

Bijvoorbeeld, de huidige VM-grootte Standard_GS5 wordt geleverd met 32 Vcpu, 448 GB RAM-geheugen, 64 schijven (maximaal 256 TB) en 80.000 IOPs of 2 GB/s i/o-bandbreedte. De nieuwe VM-groottes Standard_GS5-16 en Standard_GS5-8 wordt geleverd met 16 en 8 active Vcpu respectievelijk, terwijl de rest van de specificaties van de Standard_GS5 voor geheugen, opslag en i/o-bandbreedte te behouden.

De licentie kosten in rekening gebracht voor SQL Server of Oracle zijn beperkt tot het aantal nieuwe vCPU en andere producten moeten worden in rekening gebracht op basis van de nieuwe vCPU aantal. Dit leidt tot een toename van 50% tot 75% op de verhouding tussen het VM-specificaties voor actieve (factureerbaar) vcpu's. Deze nieuwe VM-grootten die alleen beschikbaar in Azure zijn, zodat de werkbelastingen voor de push-hoger CPU-gebruik op een fractie van de (per core) licentiëring kosten. Op dit moment blijft de compute-kosten, waaronder OS-licentieverlening, de dezelfde versie als de oorspronkelijke grootte. Zie voor meer informatie [Azure VM-grootten voor meer rendabele database werkbelastingen](https://azure.microsoft.com/blog/announcing-new-azure-vm-sizes-for-more-cost-effective-database-workloads/).


| Naam                | vCPU | Specificaties           |
|---------------------|------|-----------------|
| Standard_M64 32ms   | 32   | Zelfde als M64ms   |
| Standard_M64-16 MS   | 16   | Zelfde als M64ms   |
| Standard_M128 64ms  | 64   | Zelfde als M128ms  |
| Standard_M128 32ms  | 32   | Zelfde als M128ms  |
| Standard_E32 16_v3  | 16   | Zelfde als E32s_v3 |
| Standard_E32 8s_v3  | 8    | Zelfde als E32s_v3 |
| Standard_E64 32s_v3 | 32   | Zelfde als E64s_v3 |
| Standard_E64 16s_v3 | 16   | Zelfde als E64s_v3 |
| Standard_GS4 8      | 8    | Zelfde als GS4     |
| Standard_GS4 4      | 4    | Zelfde als GS4     |
| Standard_GS5-16     | 16   | Zelfde als GS5     |
| Standard_GS5 8      | 8    | Zelfde als GS5     |
| Standard_DS13 4_v2  | 4    | Zelfde als DS13_v2 |
| Standard_DS13 2_v2  | 2    | Zelfde als DS13_v2 |
| Standard_DS14 8_v2  | 8    | Zelfde als DS14_v2 |
| Standard_DS14 4_v2  | 4    | Zelfde als DS14_v2 |