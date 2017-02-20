## <a name="understand-planned-vs-unplanned-maintenance"></a>Gepland vs. niet-gepland onderhoud begrijpen
Er zijn twee soorten gebeurtenissen voor het Microsoft Azure-platform die invloed kunnen hebben op de beschikbaarheid van uw virtuele machines: gepland onderhoud en niet-gepland onderhoud.

* **Gepland onderhoud** bestaat uit periodieke updates van het onderliggende Azure-platform die door Microsoft worden doorgevoerd ter verbetering van de algemene betrouwbaarheid, prestaties en beveiliging van de platform-infrastructuur waarop uw virtuele machines worden uitgevoerd. Veel van deze updates worden uitgevoerd zonder dat dit van invloed is op uw virtuele machines of cloudservices. Er zijn echter gevallen waarin uw virtuele machines opnieuw moeten worden opgestart om de vereiste updates toe te passen op de platform-infrastructuur.
* **Niet-gepland onderhoud** vindt plaats wanneer de hardware of de fysieke infrastructuur die uw virtuele machine ondersteunt defect raakt. Voorbeelden hiervan zijn lokale netwerkproblemen, lokale schijfdefecten of andere defecten op rack-niveau. Wanneer een dergelijke fout wordt gedetecteerd, migreert het Azure-platform uw virtuele machine automatisch van de fysieke machine met problemen naar een gezonde fysieke machine. Dergelijke gebeurtenissen zijn zeldzaam, maar kunnen er ook toe leiden dat uw virtuele machine opnieuw moet opstarten.

Om de gevolgen van downtime vanwege een of meer van deze gebeurtenissen te beperken, raden we aan voor uw virtuele machines de volgende aanbevolen procedures voor hoge beschikbaarheid te volgen:

* [Configureer meerdere virtuele machines in een beschikbaarheidsset voor redundantie]
* [Configureer elke toepassingslaag in afzonderlijke beschikbaarheidssets]
* [Combineer het gebruik van een load balancer met beschikbaarheidssets]
* [Gebruik meerdere opslagaccounts voor elke beschikbaarheidsset]

## <a name="configure-multiple-virtual-machines-in-an-availability-set-for-redundancy"></a>Configureer meerdere virtuele machines in een beschikbaarheidsset voor redundantie
U wordt aangeraden om twee of meer virtuele machines in een beschikbaarheidsset te groeperen, om uw toepassing van redundantie te voorzien. Deze configuratie zorgt ervoor dat tijdens een geplande of onvoorziene onderhoudsgebeurtenis ten minste één virtuele machine beschikbaar is en wordt voldaan aan de 99,95% Azure SLA. Zie de [SLA voor virtuele machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/) voor meer informatie.

> [!IMPORTANT]
> Voorkom dat een virtuele machine met een enkele instantie als enige deel uitmaakt van een beschikbaarheidsset. Virtuele machines in deze configuratie komen niet in aanmerking voor een SLA-garantie en kunnen te maken krijgen met downtime bij geplande onderhoudswerkzaamheden aan het Azure-platform, tenzij een dergelijke enkele virtuele machine gebruikmaakt van [Azure Premium Storage](../articles/storage/storage-premium-storage.md). De Azure SLA is wel van toepassing op enkele virtuele machines die gebruikmaken van Premium Storage.

Elke virtuele machine in uw beschikbaarheidsset krijgt een **updatedomein** en een **foutdomein** toegewezen door het onderliggende Azure-platform. Voor iedere beschikbaarheidsset worden standaard vijf updatedomeinen toegewezen die niet door gebruiker te bewerken zijn (voor Resource Manager-implementaties kan dit aantal worden opgehoogd tot 20 updatedomeinen), om groepen virtuele machines en onderliggende fysieke hardware aan te duiden die op hetzelfde moment opnieuw kunnen worden opgestart. Wanneer in één beschikbaarheidsset meer dan vijf virtuele machines worden geconfigureerd, wordt de zesde virtuele machine in hetzelfde updatedomein geplaatst als de eerste virtuele machine, de zevende in hetzelfde updatedomein als de tweede virtuele machine, enzovoort. De volgorde waarin updatedomeinen opnieuw worden opgestart, verloopt tijdens gepland onderhoud niet altijd sequentieel, maar er wordt slechts één updatedomein tegelijk opnieuw opgestart.

Foutdomeinen duiden de groep virtuele machines aan die een gemeenschappelijke voeding en switch delen. Standaard worden bij Resource Manager-implementaties de virtuele machines die zijn geconfigureerd in uw beschikbaarheidsset verdeeld over maximaal drie foutdomeinen (twee foutdomeinen bij klassieke implementaties). Hoewel het in een beschikbaarheidsset plaatsen van uw virtuele machines uw toepassing niet beschermt tegen problemen met het besturingssysteem of de toepassing zelf, worden zo wel de gevolgen van mogelijke problemen met de fysieke hardware, netwerkstoringen of stroomonderbrekingen beperkt.

<!--Image reference-->
   ![Concepttekening van een configuratie met een updatedomein en een foutdomein](./media/virtual-machines-common-manage-availability/ud-fd-configuration.png)

### <a name="managed-disk-fault-domains-and-availability-sets"></a>Foutdomeinen en beschikbaarheidssets met Managed Disks
Voor virtuele machines die gebruikmaken van [Azure Managed Disks](../articles/storage/storage-faq-for-disks.md) en deel uitmaken van een beheerde beschikbaarheidsset, worden de virtuele machines afgestemd op Managed Disk-foutdomeinen. Deze afstemming zorgt ervoor dat alle beheerde schijven die zijn gekoppeld aan een virtuele machine, zich binnen hetzelfde Managed Disk-foutdomein bevinden. In een beheerde beschikbaarheidsset kunnen alleen virtuele machines met beheerde schijven worden gemaakt. Het aantal Managed Disk-foutdomeinen verschilt per regio: er zijn twee of drie Managed Disk-foutdomeinen per regio.


## <a name="configure-each-application-tier-into-separate-availability-sets"></a>Configureer elke toepassingslaag in afzonderlijke beschikbaarheidssets
Als uw virtuele machines allemaal bijna identiek zijn en hetzelfde doel dienen voor uw toepassing, raden wij aan dat u een beschikbaarheidsset configureert voor elke laag van uw toepassing.  Als u twee verschillende lagen in dezelfde beschikbaarheidsset plaatst, kunnen alle virtuele machines in dezelfde toepassingslaag in één keer opnieuw worden opgestart. Door voor elke laag ten minste twee virtuele machines in een beschikbaarheidsset te configureren, garandeert u dat in elke laag ten minste één virtuele machine beschikbaar is.

U kunt bijvoorbeeld alle virtuele machines in de front-end van uw toepassing waarop IIS, Apache en Nginx worden uitgevoerd in één beschikbaarheidsset plaatsen. Zorg ervoor dat er alleen virtuele machines van de front-end in dezelfde beschikbaarheidsset worden geplaatst. Zorg vervolgens dat virtuele machines uit de gegevenslaag ook in een eigen beschikbaarheidsset worden geplaatst, zoals uw gerepliceerde virtuele machines met SQL Server of uw virtuele machines met MySQL.

<!--Image reference-->
   ![Toepassingslagen](./media/virtual-machines-common-manage-availability/application-tiers.png)

## <a name="combine-a-load-balancer-with-availability-sets"></a>Combineer het gebruik van een load balancer met beschikbaarheidssets
Combineer de [Azure Load Balancer](../articles/load-balancer/load-balancer-overview.md) met een beschikbaarheidsset om uw toepassingen zo stabiel mogelijk te maken. De Azure Load Balancer verdeelt het verkeer tussen meerdere virtuele machines. De Azure Load Balancer is voor virtuele machines uit de prijscategorie Standard bij de prijs inbegrepen. De Azure Load Balancer is niet bij alle prijscategorieën voor virtuele machines inbegrepen. Zie voor meer informatie over het gebruik van load balancers voor uw virtuele machines [Taakverdeling voor virtuele machines](../articles/virtual-machines/virtual-machines-linux-load-balance.md).

Als de load balancer niet is geconfigureerd om het verkeer te verdelen over meerdere virtuele machines, heeft iedere geplande onderhoudsgebeurtenis invloed op de enkele virtuele machine die uw verkeer afhandelt, waardoor uw applicatielaag onbeschikbaar wordt. Door meerdere virtuele machines van dezelfde laag onder te brengen bij dezelfde load balancer en beschikbaarheidsset, zorgt u ervoor dat verkeer altijd door ten minste één instantie kan worden afgehandeld.

## <a name="use-multiple-storage-accounts-for-each-availability-set"></a>Gebruik meerdere opslagaccounts voor elke beschikbaarheidsset
Als u gebruikmaakt van Azure Managed Disks, kunt u de volgende tips overslaan. Azure Managed Disks bieden inherent een hoog beschikbaarheidsniveau en redundantie, omdat de schijven worden ondergebracht in foutdomeinen die zijn afgestemd op uw VM-beschikbaarheidssets. Zie [Azure Managed Disks overview](../articles/storage/storage-managed-disks-overview.md) (Overzicht van Azure Managed Disks) voor meer informatie.

Als u gebruikmaakt van niet-beheerde schijven, raden wij aan de volgende aanbevolen procedures te volgen met betrekking tot de opslagaccounts die worden gebruikt door de virtuele harde schijven (VHD's) binnen een virtuele machine. Elke schijf (VHD) is een pagina-blob in een Azure Storage-account. Het is belangrijk om te zorgen voor redundantie en isolatie tussen de opslagaccounts, om zo te zorgen voor hoge beschikbaarheid voor de virtuele machines binnen de beschikbaarheidsset.

1. **Zorg dat alle schijven (gegevens en besturingssysteem) worden gekoppeld aan een virtuele machine op hetzelfde opslagaccount**
2. **Houd rekening met de [limieten](../articles/storage/storage-scalability-targets.md) van opslagaccounts** wanneer u meer VHD's wilt toevoegen aan een opslagaccount
3. **Gebruik een afzonderlijk opslagaccount voor elke virtuele machine in een beschikbaarheidsset.** Meerdere virtuele machines in dezelfde beschikbaarheidsset dienen geen opslagaccounts te delen. Het is aanvaardbaar als virtuele machines in verschillende beschikbaarheidssets een gegevensaccount delen, zolang de voorafgaande procedures worden gevolgd

<!-- Link references -->
[Configureer meerdere virtuele machines in een beschikbaarheidsset voor redundantie]: #configure-multiple-virtual-machines-in-an-availability-set-for-redundancy
[Configureer elke toepassingslaag in afzonderlijke beschikbaarheidssets]: #configure-each-application-tier-into-separate-availability-sets
[Combineer het gebruik van een load balancer met beschikbaarheidssets]: #combine-a-load-balancer-with-availability-sets
[Avoid single instance virtual machines in availability sets]: #avoid-single-instance-virtual-machines-in-availability-sets
[Gebruik meerdere opslagaccounts voor elke beschikbaarheidsset]: #use-multiple-storage-accounts-for-each-availability-set



<!--HONumber=Feb17_HO2-->


