Dit artikel geeft een overzicht van een verzameling bewezen procedures voor het uitvoeren van een virtuele Linux-machine in Azure, waarbij aandacht wordt besteed aan schaalbaarheid, beschikbaarheid, beheerbaarheid en beveiliging. Azure ondersteunt verschillende populaire Linux-distributies, met inbegrip van CentOS, Debian, Red Hat Enterprise, Ubuntu en FreeBSD. Zie [Azure en Linux][azure-linux] voor meer informatie.

> [!NOTE]
> Azure biedt twee implementatiemodellen: [Resource Manager][resource-manager-overview] en het klassieke model. In dit artikel wordt gebruikgemaakt Resource Manager, dat Microsoft aanbeveelt voor nieuwe implementaties.
> 
> 

Het is geen goed idee om een enkele virtuele machine te gebruiken voor bedrijfskritieke taken, omdat er dan maar één storingspunt is. Implementeer voor hogere beschikbaarheid meerdere virtuele machines in een [beschikbaarheidsset][availability-set]. Zie [Meerdere virtuele machines uitvoeren in Azure][multi-vm] voor meer informatie. 

## <a name="architecture-diagram"></a>Architectuurdiagram
Het inrichten van een virtuele machine in Azure omvat meer bewegende onderdelen dan alleen de virtuele machine zelf. U moet ook rekening houden met computer-, netwerk- en opslagelementen.

> Een Visio-document met dit architectuurdiagram is beschikbaar in het [Microsoft Downloadcentrum][visio-download]. Dit diagram bevindt zich op de pagina 'Compute - single VM'.
> 
> 

![[0]][0]

* **Resourcegroep.** Een [*resourcegroep*][resource-manager-overview] is een container die verwante resources bevat. Maak een resourcegroep die de resources voor deze virtuele machine bevat.
* **Virtuele machine**. U kunt een virtuele machine inrichten vanuit een lijst met gepubliceerde installatiekopieën of via een VHD-bestand (virtuele harde schijf) dat u naar Azure Blob Storage uploadt.
* **Besturingssysteemschijf.** De besturingssysteemschijf is een VHD die is opgeslagen in [Azure Storage][azure-storage]. Dit betekent dat deze actief blijft zelfs als de hostmachine uitvalt. De besturingssysteemschijf is `/dev/sda1`.
* **Tijdelijke schijf.** De virtuele machine wordt gemaakt met een tijdelijke schijf. Deze schijf wordt opgeslagen op een fysiek station op de hostcomputer. De schijf wordt *niet* opgeslagen in Azure Storage en wordt mogelijk verwijderd tijdens opnieuw opstarten en andere gebeurtenissen in de levensduur van de virtuele machine. Gebruik deze schijf alleen voor tijdelijke gegevens, zoals pagina- of wisselbestanden. De tijdelijke schijf is `/dev/sdb1` en wordt geplaatst in `/mnt/resource` of `/mnt`.
* **Gegevensschijven.** Een [gegevensschijf] [ data-disk] is een permanente VHD die wordt gebruikt voor toepassingsgegevens. Gegevensschijven worden opgeslagen in Azure Storage, net als de besturingssysteemschijf.
* **Virtueel netwerk (VNet) en subnet.** Elke virtuele machine in Azure wordt geïmplementeerd in een VNet dat verder wordt onderverdeeld in subnetten.
* **Openbaar IP-adres.** Een openbaar IP-adres is nodig om te communiceren met de virtuele machine &mdash; bijvoorbeeld via SSH.
* **Netwerkinterface (NIC)**. Via de NIC kan de virtuele machine communiceren met het virtuele netwerk.
* **Netwerkbeveiligingsgroep (NSG)**. De [NSG] [ nsg] wordt gebruikt voor het toestaan/weigeren van netwerkverkeer naar het subnet. U kunt een NSG koppelen aan een afzonderlijke NIC of aan een subnet. Als u een NSG aan een subnet koppelt, gelden de NSG-regels voor alle virtuele machines in dat subnet.
* **Diagnostiek.** Diagnostische logboekregistratie is essentieel voor het beheren van en oplossen van problemen met de virtuele machine.

## <a name="recommendations"></a>Aanbevelingen

De volgende aanbevelingen gelden voor de meeste scenario's. Volg deze aanbevelingen tenzij er een specifieke vereiste is die iets anders voorschrijft. 

### <a name="vm-recommendations"></a>Aanbevelingen voor virtuele machines

Azure biedt virtuele machines van veel verschillende grootten, maar wij raden de DS- en GS-serie aan, omdat machines van deze grootte [Premium Storage][premium-storage] ondersteunen. Selecteer een machine met een van deze grootten tenzij u een speciale workload hebt, zoals high-performance verwerking. Zie [Grootten voor virtuele machines][virtual-machine-sizes] voor meer informatie.

Als u een bestaande workload naar Azure verplaatst, begint u met de VM-grootte die het meest overeenkomt met uw on-premises servers. Meet vervolgens de prestaties van uw huidige workload met betrekking tot CPU, geheugen en schijfinvoer-/uitvoerbewerkingen per seconde (IOPS), en pas de grootte indien nodig aan. Als u voor uw virtuele machine meerdere NIC's nodig hebt, moet u erop letten dat het maximumaantal NIC's een functie is van de [VM-grootte][vm-size-tables].

Als u de virtuele machine en andere resources inricht, moet u een regio opgeven. Over het algemeen kiest u de regio die het dichtst bij uw interne gebruikers of klanten ligt. Niet alle VM-grootten zijn echter beschikbaar in alle regio's. Zie [Services per regio][services-by-region] voor meer informatie. Voer de volgende Azure CLI-opdracht (opdrachtregelinterface) uit om de VM-grootten weer te geven die in een bepaalde regio beschikbaar zijn:

```
azure vm sizes --location <location>
```

Zie [Linux VM-installatiekopieën selecteren met de Azure CLI ][select-vm-image]voor meer informatie over het kiezen van een installatiekopie van een gepubliceerde virtuele machine.

### <a name="disk-and-storage-recommendations"></a>Aanbevelingen voor schijven en opslag

Voor de beste-I/O-prestaties van de schijf raden we [Premium Storage][premium-storage] aan, waarmee gegevens op SSD's (solid-state drives) worden opgeslagen. De kosten zijn gebaseerd op de grootte van de ingerichte schijf. IOPS en doorvoer (dat wil zeggen de snelheid van de gegevensoverdracht) zijn ook afhankelijk van de schijfgrootte, dus als u een schijf inricht, moet u rekening houden met alle drie factoren (capaciteit, IOPS en doorvoer). 

Maak voor de virtuele harde schijven (VHD's) afzonderlijke Azure-opslagaccounts voor elke virtuele machine om te voorkomen dat u de IOPS-limieten voor opslagaccounts bereikt. 

Voeg een of meer gegevensschijven toe. Wanneer u een VHD maakt, is deze niet geformatteerd. Meld u aan bij de virtuele machine om de schijf te formatteren. In de Linux-shell worden gegevensschijven weergegeven als `/dev/sdc`, `/dev/sdd`, enzovoort. U kunt `lsblk` uitvoeren om de blokapparaten, met inbegrip van de schijven, weer te geven. Als u een gegevensschijf wilt gebruiken, maakt u een partitie en bestandssysteem, en koppelt u de schijf. Bijvoorbeeld:

```bat
# Create a partition.
sudo fdisk /dev/sdc     # Enter 'n' to partition, 'w' to write the change.     

# Create a file system.
sudo mkfs -t ext3 /dev/sdc1

# Mount the drive.
sudo mkdir /data1
sudo mount /dev/sdc1 /data1
```

Als u een groot aantal gegevensschijven hebt, moet u rekening houden met de totale I/O-limieten van het opslagaccount. Zie [Schijflimieten voor virtuele machines][vm-disk-limits] voor meer informatie.

Wanneer u een gegevensschijf toevoegt, wordt een LUN-ID (logische-eenheidnummer) toegewezen aan de schijf. Eventueel kunt u de LUN-ID opgeven &mdash; bijvoorbeeld, wanneer u een schijf vervangt en dezelfde LUN-ID wilt behouden, of als u een toepassing hebt die een specifieke LUN-ID zoekt. Vergeet echter niet dat LUN-ID's uniek moeten zijn voor elke schijf.

Mogelijk is het een goed idee om de I/O-planner zodanig in te stellen dat wordt gestreefd naar optimale prestaties op SSD's, aangezien schijven voor virtuele machines met een Premium opslagaccount SSD's zijn. Het wordt in het algemeen aanbevolen de NOOP-planner te gebruiken voor SSD's, maar u moet een hulpprogramma zoals [iostat] gebruiken om de I/O-prestaties van de schijf te bewaken voor uw specifieke workload.

Maak voor optimale prestaties een afzonderlijk opslagaccount voor diagnostische logboeken. Een standaardaccount voor lokaal redundante opslag (LRS) is voldoende voor diagnostische logboeken.

### <a name="network-recommendations"></a>Aanbevelingen voor netwerken

Het openbare IP-adres kan dynamisch of statisch zijn. De standaardwaarde is dynamisch.

* Reserveer een [statisch IP-adres][static-ip] als u een vast IP-adres nodig hebt dat niet verandert &mdash; bijvoorbeeld als u een A-record moet maken in DNS of als het IP-adres moet worden toegevoegd aan een lijst met veilige adressen.
* U kunt ook een volledig gekwalificeerde domeinnaam (FQDN) voor het IP-adres maken. U kunt vervolgens een [CNAME-record][cname-record] maken in DNS dat naar de FQDN verwijst. Zie [Een Fully Qualified Domain Name maken in Azure Portal][fqdn] voor meer informatie.

Alle NSG's bevatten een set [standaardregels][nsg-default-rules], met inbegrip van een regel waarmee al het inkomende internetverkeer wordt geblokkeerd. De standaardregels kunnen niet worden verwijderd, maar ze kunnen wel worden vervangen door andere regels. Als u internetverkeer wilt inschakelen, maakt u regels waarmee inkomend verkeer op specifieke poorten is toegestaan &mdash; bijvoorbeeld poort 80 voor HTTP.  

Als u SSH wilt inschakelen, voegt u aan de NSG een regel toe waarmee inkomend verkeer op TCP-poort 22 wordt toegestaan.

## <a name="scalability-considerations"></a>Schaalbaarheidsoverwegingen

Als u omhoog of omlaag wilt schalen, [wijzigt u de VM-grootte][vm-resize]. 

Als u horizontaal wilt schalen, plaatst u twee of meer virtuele machines in een beschikbaarheidsset achter een load balancer. Zie [Meerdere virtuele machines uitvoeren in Azure][multi-vm] voor meer informatie.

## <a name="availability-considerations"></a>Beschikbaarheidsoverwegingen

Implementeer voor hogere beschikbaarheid meerdere virtuele machines in een beschikbaarheidsset. Dit biedt ook een hogere [serviceovereenkomst][vm-sla] (SLA). 

De virtuele machine kan worden beïnvloed door [gepland onderhoud][planned-maintenance] of [niet-gepland onderhoud][manage-vm-availability]. U kunt [logboeken over het opnieuw opstarten van virtuele machines] [ reboot-logs] gebruiken om na te gaan of het opnieuw opstarten van een virtuele machine is veroorzaakt door gepland onderhoud.

VHD's worden opgeslagen in [Azure Storage][azure-storage]. Azure Storage wordt gerepliceerd voor duurzaamheid en beschikbaarheid.

Ter bescherming tegen onbedoeld gegevensverlies tijdens normaal systeemgebruik (bijvoorbeeld vanwege een gebruikersfout) moet u ook punt-in-tijd back-ups implementeren met behulp van [blob-momentopnamen][blob-snapshot] of een ander hulpprogramma.

## <a name="manageability-considerations"></a>Beheerbaarheidsoverwegingen

**Resourcegroepen.** Plaats nauw verwante resources die dezelfde levenscyclus delen, in dezelfde [resourcegroep][resource-manager-overview]. Met resourcegroepen kunt u resources groepsgewijs implementeren en bewaken. Ook kunt u de factureringskosten per groep bijhouden. Daarnaast kunt u resources verwijderen als set. Dit is handig voor testimplementaties. Geef resources een betekenisvolle naam. Dat maakt het gemakkelijker om een specifieke resource te vinden en te begrijpen waar deze voor dient. Zie [Aanbevolen naamgevingsregels voor Azure-resources][naming conventions].

**SSH**. Voordat u een virtuele Linux-machine maakt, moet u een 2048 bits RSA openbaar-persoonlijk sleutelpaar genereren. Gebruik het openbare sleutelbestand wanneer u de virtuele machine maakt. Zie [SSH gebruiken met Linux en Mac in Azure][ssh-linux] voor meer informatie.

**Diagnostische gegevens van virtuele machines.** Schakel bewaking en diagnostiek in, inclusief metrische basisgegevens over de status, diagnostische logboeken over de infrastructuur en [diagnostische gegevens over opstarten][boot-diagnostics]. Met diagnostische gegevens over opstarten kunt u opstartfouten achterhalen als de virtuele machine in een niet-opstartbare status komt. Zie [Controle en diagnose inschakelen][enable-monitoring] voor meer informatie.  

Met de volgende CLI-opdracht schakelt u diagnostische gegevens in:

```
azure vm enable-diag <resource-group> <vm-name>
```

**Een virtuele machine stoppen.** Azure maakt onderscheid tussen een 'gestopte' status en een status waarbij de toewijzing is opgeheven. Er worden kosten in rekening gebracht wanneer de status van de virtuele machine is gestopt, maar niet wanneer de toewijzing van de virtuele machine is opgeheven.

Gebruik de volgende CLI-opdracht om de toewijzing van een virtuele machine op te heffen:

```
azure vm deallocate <resource-group> <vm-name>
```

In Azure Portal wordt met de knop **Stoppen** de toewijzing van een virtuele machine opgeheven. Als u echter afsluit via het besturingssysteem terwijl u bent aangemeld, wordt de virtuele machine wel gestopt, maar de toewijzing ervan *niet* opgeheven, zodat u nog steeds kosten in rekening worden gebracht.

**Een virtuele machine verwijderen.** Als u een virtuele machine verwijdert, worden de VHD's niet verwijderd. Dit betekent dat u de virtuele machine veilig zonder gegevensverlies kunt verwijderen. Er worden echter nog steeds kosten in rekening gebracht voor opslag. Als u de VHD wilt verwijderen, verwijdert u het bestand uit [Blob Storage][blob-storage].

Gebruik ter voorkoming van onbedoeld verwijderen een [resourcevergrendeling][resource-lock] om de gehele resourcegroep of afzonderlijke resources, zoals de virtuele machine, te vergrendelen. 

## <a name="security-considerations"></a>Beveiligingsoverwegingen

Automatiseer updates van het besturingssysteem met behulp van de VM-uitbreiding [OSPatching]. Installeer deze uitbreiding als u de virtuele machine inricht. U kunt opgeven hoe vaak patches moeten worden geïnstalleerd en of daarna opnieuw moet worden opgestart.

Gebruik [op rollen gebaseerd toegangsbeheer] [ rbac] (RBAC) om toegang te beheren tot de Azure-resources die u implementeert. Met RBAC kunt u machtigingsrollen toewijzen aan leden van uw DevOps-team. Iemand met de rol Lezer kan bijvoorbeeld wel Azure-resources weergeven, maar deze niet maken, beheren of verwijderen. Sommige rollen zijn specifiek voor bepaalde typen Azure-resources. Iemand met de rol Inzender voor virtuele machines kan bijvoorbeeld een virtuele machine opnieuw opstarten of de toewijzing van een virtuele machine ongedaan maken, het beheerderswachtwoord opnieuw instellen, een virtuele machine maken, enzovoort. Andere [ingebouwde RBAC-rollen][rbac-roles] die nuttig kunnen zijn voor deze referentiearchitectuur, zijn [DevTest Labs-gebruiker][rbac-devtest] en [Inzender voor netwerken][rbac-network]. 

Een gebruiker kan worden toegewezen aan meerdere rollen. Voor nog gedetailleerdere machtigingen kunt u aangepaste rollen maken.

d> [!NOTE]
> RBAC beperkt niet de acties die een bij een virtuele machine aangemelde gebruiker kan uitvoeren. Deze machtigingen worden bepaald door het accounttype op het gastbesturingssysteem.   
> 
> 

Gebruik [controlelogboeken][audit-logs] om inrichtingsacties en andere VM-gebeurtenissen te bekijken.

Overweeg [Azure Disk Encryption][disk-encryption] als u het besturingssysteem en gegevensschijven wilt versleutelen. 

## <a name="solution-deployment"></a>Implementatie van de oplossing
Een implementatie voor deze referentiearchitectuur is beschikbaar op [GitHub][github-folder]. Deze bevat een VNet, NSG en een enkele virtuele machine. Voer de volgende stappen uit om de architectuur te implementeren: 

1. Klik met de rechtermuisknop op de onderstaande knop en selecteer 'Koppeling in een nieuw tabblad openen' of 'Koppeling in nieuw venster openen'.
   [![Implementeren in Azure](../articles/guidance/media/blueprints/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2Fmspnp%2Freference-architectures%2Fmaster%2Fguidance-compute-single-vm%2Fazuredeploy.json)
2. Nadat de koppeling in Azure Portal is geopend, moet u voor bepaalde instellingen waarden opgeven: 
   
   * De naam voor de **Resourcegroep** is al gedefinieerd in het parameterbestand. Selecteer daarom **Nieuwe maken** en voer `ra-single-vm-rg` in het tekstvak in.
   * Selecteer de regio in de vervolgkeuzelijst **Locatie**.
   * Breng geen wijzigingen aan in de tekstvakken **Basis-URI-sjabloon** en **Basis-URI-parameter**.
.   * Selecteer **linux** in de **Type besturingssysteem** vervolgkeuzelijst.
   * Lees de voorwaarden en bepalingen, en klik vervolgens op het selectievakje **Ik ga akkoord met de bovenstaande voorwaarden**.
   * Klik op de knop **Kopen**.
3. Wacht totdat de installatie is voltooid.
4. De parameterbestanden bevatten een in code vastgelegde gebruikersnaam en wachtwoord. U doet er verstandig aan beide onmiddellijk te wijzigen. Klik op de virtuele machine met de naam `ra-single-vm0 ` in Azure Portal. Klik vervolgens op **Wachtwoord opnieuw instellen** in het gedeelte **Ondersteuning en probleemoplossing**. Selecteer **Wachtwoord opnieuw instellen** in de vervolgkeuzelijst **Modus**. Selecteer vervolgens een nieuwe **Gebruikersnaam** en **Wachtwoord**. Klik op de knop **Bijwerken** om de nieuwe gebruikersnaam en wachtwoord vast te leggen.

## <a name="next-steps"></a>Volgende stappen
Implementeer voor hogere beschikbaarheid twee of meer virtuele machines achter een load balancer. Zie [Meerdere virtuele machines uitvoeren in Azure][multi-vm] voor meer informatie.

<!-- links -->

[audit-logs]: https://azure.microsoft.com/en-us/blog/analyze-azure-audit-logs-in-powerbi-more/
[availability-set]:../articles/virtual-machines/windows/create-availability-set.md
[azure-cli]: /cli/azure/get-started-with-az-cli2
[azure-linux]:../articles/virtual-machines/linux/overview.md
[azure-storage]:../articles/storage/common/storage-introduction.md
[blob-snapshot]:../articles/storage/blobs/storage-blob-snapshots.md
[blob-storage]:../articles/storage/common/storage-introduction.md
[boot-diagnostics]: https://azure.microsoft.com/en-us/blog/boot-diagnostics-for-virtual-machines-v2/
[cname-record]: https://en.wikipedia.org/wiki/CNAME_record
[data-disk]:../articles/virtual-machines/linux/about-disks-and-vhds.md
[disk-encryption]: ../articles/security/azure-security-disk-encryption.md
[enable-monitoring]: ../articles/monitoring-and-diagnostics/insights-how-to-use-diagnostics.md
[fqdn]:../articles/virtual-machines/linux/portal-create-fqdn.md
[github-folder]: https://github.com/mspnp/reference-architectures/tree/master/guidance-compute-single-vm/
[iostat]: https://en.wikipedia.org/wiki/Iostat
[manage-vm-availability]:../articles/virtual-machines/linux/manage-availability.md
[multi-vm]: ../articles/guidance/guidance-compute-multi-vm.md
[naming conventions]: ../articles/guidance/guidance-naming-conventions.md
[nsg]: ../articles/virtual-network/virtual-networks-nsg.md
[nsg-default-rules]: ../articles/virtual-network/virtual-networks-nsg.md#default-rules
[OSPatching]: https://github.com/Azure/azure-linux-extensions/tree/master/OSPatching
[planned-maintenance]:../articles/virtual-machines/linux/planned-maintenance.md
[premium-storage]:../articles/virtual-machines/windows/premium-storage.md
[rbac]: ../articles/active-directory/role-based-access-control-what-is.md
[rbac-roles]: ../articles/active-directory/role-based-access-built-in-roles.md
[rbac-devtest]: ../articles/active-directory/role-based-access-built-in-roles.md#devtest-labs-user
[rbac-network]: ../articles/active-directory/role-based-access-built-in-roles.md#network-contributor
[reboot-logs]: https://azure.microsoft.com/en-us/blog/viewing-vm-reboot-logs/
[Resize-VHD]: https://technet.microsoft.com/en-us/library/hh848535.aspx
[Resize virtual machines]: https://azure.microsoft.com/en-us/blog/resize-virtual-machines/
[resource-lock]: ../articles/resource-group-lock-resources.md
[resource-manager-overview]: ../articles/azure-resource-manager/resource-group-overview.md
[select-vm-image]:../articles/virtual-machines/linux/cli-ps-findimage.md
[services-by-region]: https://azure.microsoft.com/en-us/regions/#services
[ssh-linux]:../articles/virtual-machines/linux/mac-create-ssh-keys.md
[static-ip]: ../articles/virtual-network/virtual-networks-reserved-public-ip.md
[storage-account-limits]: ../articles/azure-subscription-service-limits.md#storage-limits
[storage-price]: https://azure.microsoft.com/pricing/details/storage/
[virtual-machine-sizes]:../articles/virtual-machines/linux/sizes.md
[visio-download]: http://download.microsoft.com/download/1/5/6/1569703C-0A82-4A9C-8334-F13D0DF2F472/RAs.vsdx
[vm-disk-limits]: ../articles/azure-subscription-service-limits.md#virtual-machine-disk-limits
[vm-resize]:../articles/virtual-machines/linux/change-vm-size.md
[vm-size-tables]:../articles/virtual-machines/windows/sizes.md#size-tables
[vm-sla]: https://azure.microsoft.com/support/legal/sla/virtual-machines
[readme]: https://github.com/mspnp/reference-architectures/blob/master/guidance-compute-single-vm
[components]: #Solution-components
[blocks]: https://github.com/mspnp/template-building-blocks
[0]: ./media/guidance-blueprints/compute-single-vm.png "Architectuur met één virtuele Linux-machine in Azure"

