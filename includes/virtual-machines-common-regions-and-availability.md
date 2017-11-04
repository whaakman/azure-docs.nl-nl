# <a name="regions-and-availability-for-virtual-machines-in-azure"></a>Regio's en beschikbaarheid voor virtuele machines in Azure
Azure werkt vanuit diverse datacentra, op locaties overal wereld. Deze datacenters worden gegroepeerd in geografische regio's, waardoor u flexibiliteit heeft bij het kiezen waar u uw toepassingen ontwikkelt. Het is belangrijk om inzicht te hebben in hoe en waar uw virtuele machines (VM's) in Azure werken, evenals in wat uw mogelijkheden zijn om de prestaties, beschikbaarheid en redundantie te maximaliseren. Dit artikel biedt een overzicht van de mogelijkheden van Azure op het gebied van beschikbaarheid en redundantie.

## <a name="what-are-azure-regions"></a>Wat zijn Azure-regio's?
U kunt Azure-resources maken in gedefinieerde geografische regio's zoals 'VS-West', 'Noord-Europa' of Zuidoost-Azië. U kunt zelf de [lijst met regio's en bijbehorende locaties](https://azure.microsoft.com/regions/) bekijken. In elke regio bevinden zich meerdere datacenters, om te zorgen voor voldoende redundantie en beschikbaarheid. Deze aanpak biedt u flexibiliteit bij het ontwerpen van toepassingen voor virtuele machines die het dichtst bij uw gebruikers maken en voor het voldoen aan een juridisch, compatibiliteit of BTW-doeleinden.

## <a name="special-azure-regions"></a>Speciale Azure-regio's
Azure heeft bepaalde speciale gebieden die u gebruiken wilt mogelijk bij het bouwen van uw toepassingen voor juridische redenen of compatibiliteit. Voorbeelden van dergelijke speciale regio's zijn:

* **VS (overheid) - Virginia** en **VS (overheid) - Iowa**
  * Een fysiek en logisch van netwerken afgeschermd exemplaar van Azure voor de Amerikaanse overheid en zijn partners, bediend door gecontroleerde Amerikaanse staatsburgers. Dit exemplaar beschikt over aanvullende nalevingscertificeringen, zoals [FedRAMP](https://www.microsoft.com/en-us/TrustCenter/Compliance/FedRAMP) en [DISA](https://www.microsoft.com/en-us/TrustCenter/Compliance/DISA). Meer informatie over [Azure Government](https://azure.microsoft.com/features/gov/).
* **China - noord** en **China - oost**
  * Deze regio's zijn beschikbaar via een unieke samenwerking tussen Microsoft en 21Vianet, waarbij Microsoft niet rechtstreeks de datacentra onderhoudt. Zie meer informatie over [Microsoft Azure in China](http://www.windowsazure.cn/).
* **Duitsland - centraal** en **Duitsland - noordoost**
  * Deze gebieden zijn beschikbaar via een gegevensmodel beheerder waarbij klantgegevens in Duitsland onder het beheer van T-systemen, een bedrijf Deutsche Telekom blijft, fungeert als de beheerder van de Duitse gegevens.

## <a name="region-pairs"></a>Regioparen
Elke Azure-regio is gekoppeld aan een andere regio binnen hetzelfde geografische gebied (zoals VS, Europa of Azië). Hierdoor kunnen resources, zoals VM-opslag, worden gerepliceerd op een andere locatie binnen hetzelfde geografische gebied, zodat de kans wordt beperkt dat beide regio’s tegelijkertijd worden beïnvloed door natuurrampen, onrusten, stroomstoringen of fysieke netwerkuitval. Het gebruik van regioparen biedt meer voordelen:

* In het geval van een bredere Azure-storing krijgt van elk paar één regio prioriteit, om te zorgen dat toepassingen zo snel mogelijk weer beschikbaar zijn. 
* Geplande Azure-updates worden voor gepaarde regio’s één voor één geïmplementeerd, om downtime en het risico op niet-beschikbare toepassingen te beperken.
* Gegevens blijven voor juridische en belastingdoeleinden voor ieder paar binnen hetzelfde geografische gebied (met uitzondering van Brazilië - zuid).

Voorbeelden van regioparen zijn:

| Primair | Secundair |
|:--- |:--- |
| VS - west |VS - oost |
| Noord-Europa |West-Europa |
| Zuidoost-Azië |Oost-Azië |

U vindt de volledige [lijst met regioparen hier](../articles/best-practices-availability-paired-regions.md#what-are-paired-regions).

## <a name="feature-availability"></a>Beschikbaarheid van functies
Sommige services of VM-functies zijn alleen beschikbaar in bepaalde regio's, zoals specifieke VM-grootten of opslagtypen. Er zijn ook een aantal algemene Azure-services waarvoor u geen specifieke regio hoeft te kiezen, zoals [Azure Active Directory](../articles/active-directory/active-directory-whatis.md), [Traffic Manager](../articles/traffic-manager/traffic-manager-overview.md) en [Azure DNS](../articles/dns/dns-overview.md). Als hulp bij het ontwerpen van de omgeving van uw toepassing, kunt u de [beschikbaarheid van Azure-services per regio](https://azure.microsoft.com/regions/#services) raadplegen. U kunt ook [programmatisch query uitvoeren op de ondersteunde VM-grootten en -beperkingen in elke regio](../articles/azure-resource-manager/resource-manager-sku-not-available-errors.md).

## <a name="storage-availability"></a>Opslagbeschikbaarheid
Inzicht in Azure-regio's en geografische locaties wordt belangrijk wanneer u gaat kijken naar de verschillende opties voor opslagreplicatie. U hebt verschillende replicatieopties, afhankelijk van het opslagtype.

**Azure Managed Disks**
* Lokaal redundante opslag (LRS)
  * Uw gegevens worden drie keer gerepliceerd in de regio waar u uw storage-account hebt gemaakt.

**Schijven voor opslag op basis van een account**
* Lokaal redundante opslag (LRS)
  * Uw gegevens worden drie keer gerepliceerd in de regio waar u uw storage-account hebt gemaakt.
* Zone-redundante opslag (ZRS)
  * Uw gegevens worden binnen twee of drie faciliteiten in één regio of tussen twee regio's driemaal gerepliceerd.
* Geografisch redundante opslag (GRS)
  * Uw gegevens worden gerepliceerd naar een secundaire regio, honderden kilometers van de primaire regio.
* Geografisch redundante opslag met leestoegang (RA-GRS)
  * Uw gegevens worden gerepliceerd naar een secundaire regio, net als bij GRS, maar vervolgens biedt de secundaire locatie alleen-lezen toegang tot de gegevens.

De volgende tabel geeft een overzicht van de verschillen tussen de typen opslagreplicatie:

| Replicatiestrategie | LRS | ZRS | GRS | RA-GRS |
|:--- |:--- |:--- |:--- |:--- |
| Gegevens worden gerepliceerd bij meerdere faciliteiten. |Nee |Ja |Ja |Ja |
| Gegevens kunnen worden gelezen vanaf de secundaire locatie en vanaf de primaire locatie. |Nee |Nee |Nee |Ja |
| Het aantal exemplaren van de gegevens op afzonderlijke knooppunten. |3 |3 |6 |6 |

Meer informatie over [Azure Storage-replicatieopties vindt u hier](../articles/storage/common/storage-redundancy.md). Zie voor meer informatie over beheerde schijven [overzicht Azure Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md).

### <a name="storage-costs"></a>Opslagkosten
De kosten zijn afhankelijk van het opslagtype en de beschikbaarheid die u selecteert.

**Azure Managed Disks**
* Premium-beheerde schijven zijn Solid-State stations (SSD's) en beheerd standaardschijven worden ondersteund door reguliere draaiende schijven. Voor zowel Premium als Standard Managed Disks worden kosten in rekening gebracht op basis van de ingerichte capaciteit van de schijf.

**Niet-beheerde schijven**
* Premium-opslag wordt ondersteund door Solid-State stations (SSD's) en wordt in rekening gebracht op basis van de capaciteit van de schijf.
* Standaard opslag wordt ondersteund door traditionele draaiende schijven en wordt in rekening gebracht op basis van de gebruikte capaciteit en de gewenste opslagbeschikbaarheid.
  * Voor RA-GRS wordt een toeslag in rekening gebracht voor de gegevensoverdracht voor geo-replicatie, voor de bandbreedtekosten van het repliceren van gegevens naar een andere Azure-regio.

Zie [prijzen van Azure Storage](https://azure.microsoft.com/pricing/details/storage/) voor informatie over prijzen van de verschillende opslagtypen en beschikbaarheidsopties.

## <a name="availability-sets"></a>Beschikbaarheidssets
Een beschikbaarheidsset is een logische groepering van virtuele machines binnen een datacenter waarmee Azure om te begrijpen hoe uw toepassing is ontworpen om te voorzien in redundantie en beschikbaarheid. Het is raadzaam dat twee of meer virtuele machines worden gemaakt binnen een beschikbaarheidsset te voorzien van een maximaal beschikbare toepassing en om te voldoen aan de [99,95% Azure SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/). Wanneer een enkele virtuele machine gebruikmaakt van [Azure Premium Storage](../articles/virtual-machines/windows/premium-storage.md), is de Azure SLA van toepassing op niet-gepland onderhoud. 

Een beschikbaarheidsset bestaat uit twee extra groeperingen die beveiligen tegen hardwarestoringen en toestaan dat updates veilig worden toegepast: fault-domeinen (FDs) en update-domeinen (UDs). Lees meer over het beheren van de beschikbaarheid van [Linux-VM's](../articles/virtual-machines/linux/manage-availability.md) of [Windows VM's](../articles/virtual-machines/windows/manage-availability.md).

### <a name="fault-domains"></a>Foutdomeinen
Een foutdomein is een logische groep onderliggende hardware met een gemeenschappelijke voeding en netwerkswitch, vergelijkbaar met een rack in een on-premises datacenter. Wanneer u virtuele machines in een beschikbaarheidsset maakt, verdeelt het Azure-platform uw virtuele machines automatisch tussen deze foutdomeinen. Deze aanpak beperkt de gevolgen van mogelijke problemen met de fysieke hardware, netwerkstoringen of stroomonderbrekingen.

### <a name="update-domains"></a>Updatedomeinen
Een updatedomein is een logische groep onderliggende hardware die op hetzelfde moment onderhoud kan ondergaan of opnieuw kan worden opgestart. Wanneer u virtuele machines in een beschikbaarheidsset maakt, verdeelt het Azure-platform uw virtuele machines automatisch tussen deze updatedomeinen. Deze aanpak zorgt ervoor dat altijd ten minste één exemplaar van uw toepassing beschikbaar blijft wanneer er periodiek onderhoud wordt uitgevoerd aan het Azure-platform. De volgorde waarin updatedomeinen opnieuw worden opgestart, verloopt tijdens gepland onderhoud niet altijd sequentieel, maar er wordt slechts één updatedomein tegelijk opnieuw opgestart.

### <a name="managed-disk-fault-domains"></a>Domeinen met fouten schijf beheerd
Voor virtuele machines die gebruikmaken van [Azure Managed Disks](../articles/virtual-machines/windows/faq-for-disks.md) en deel uitmaken van een beheerde beschikbaarheidsset, worden de virtuele machines afgestemd op Managed Disk-foutdomeinen. Deze afstemming zorgt ervoor dat alle beheerde schijven die zijn gekoppeld aan een virtuele machine, zich binnen hetzelfde Managed Disk-foutdomein bevinden. In een beheerde beschikbaarheidsset kunnen alleen virtuele machines met beheerde schijven worden gemaakt. Het aantal Managed Disk-foutdomeinen verschilt per regio: er zijn twee of drie Managed Disk-foutdomeinen per regio. Meer informatie over dit beheerde domeinen met fouten voor schijf [virtuele Linux-machines](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set) of [VM's van Windows](../articles/virtual-machines/linux/manage-availability.md?#use-managed-disks-for-vms-in-an-availability-set).

## <a name="availability-zones"></a>Beschikbaarheid zones

[Beschikbaarheid zones](../articles/availability-zones/az-overview.md) (preview), een alternatief voor beschikbaarheid ingesteld uit, vouw de mate van controle die u hebt om de beschikbaarheid van de toepassingen en gegevens op uw virtuele machines te houden. Een Zone beschikbaarheid is een fysiek afzonderlijke zone binnen een Azure-regio. Er zijn drie beschikbaarheid Zones per ondersteunde Azure-regio. Elke Zone beschikbaarheid heeft een afzonderlijke energie-bron-, netwerk- en koeling en logisch losstaat van de andere beschikbaarheid Zones in de Azure-regio. Door uw oplossingen voor het gebruik van de gerepliceerde virtuele machines in zones worden veranderd, kunt u uw apps en gegevens beschermen tegen het verlies van een datacenter. Als één zone is geknoeid, waarna zijn gerepliceerde apps en gegevens onmiddellijk beschikbaar in een andere zone. 

![Beschikbaarheid zones](./media/virtual-machines-common-regions-and-availability/three-zones-per-region.png)

[!INCLUDE [availability-zones-preview-statement.md](availability-zones-preview-statement.md)]

Meer informatie over het implementeren van een [Windows](../articles/virtual-machines/windows/create-powershell-availability-zone.md) of [Linux](../articles/virtual-machines/linux/create-cli-availability-zone.md) virtuele machine in een Zone beschikbaarheid.

## <a name="next-steps"></a>Volgende stappen
U kunt nu deze functies voor beschikbaarheid en redundantie gaan gebruiken om uw eigen Azure-omgeving te bouwen. Zie voor informatie over aanbevolen procedures de [aanbevolen procedures voor Azure-beschikbaarheid](../articles/best-practices-availability-checklist.md).

