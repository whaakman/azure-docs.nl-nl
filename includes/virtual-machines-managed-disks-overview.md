# <a name="azure-managed-disks-overview"></a>Overzicht van Azure-beheerde schijven

Azure-schijven die worden beheerd vereenvoudigt Schijfbeheer voor Azure IaaS VM's met het beheren van de [opslagaccounts](../articles/storage/common/storage-introduction.md) die zijn gekoppeld aan de VM-schijven. U hoeft alleen te geven van het type ([Premium](../articles/virtual-machines/windows/premium-storage.md) of [standaard](../articles/virtual-machines/windows/standard-storage.md)) en de grootte van de schijf die u nodig en Azure maken en beheren van de schijf voor u.

## <a name="benefits-of-managed-disks"></a>Voordelen van beheerde schijven

We gaan verdiepen in enkele voordelen u krijgt met behulp van beheerde schijven vanaf deze video Channel 9 [betere Azure VM tolerantie met schijven beheerd](https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency).
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Managed-Disks-for-Azure-Resiliency/player]

### <a name="simple-and-scalable-vm-deployment"></a>Eenvoudige en schaalbare implementaties van virtuele machines

Schijven ingangen opslag achter de schermen voor u beheerd. Voorheen moest u storage-accounts voor het opslaan van de schijven (VHD-bestanden) voor uw Azure VM's maken. Wanneer het omhoog schalen, moest u om ervoor te zorgen dat u extra opslagaccounts hebt gemaakt, zodat u niet langer zijn dan de maximale IOPS voor opslag met een van uw schijven. Met beheerde schijven afhandeling van opslag, u niet langer beperkt door de limieten van het opslagaccount (zoals 20.000 IOPS / -account). U moet ook niet langer aangepaste installatiekopieën (VHD-bestanden) kopiëren naar meerdere accounts voor opslag. U kunt ze beheren op een centrale locatie – één opslagaccount per Azure-regio- en honderden van virtuele machines in een abonnement maken.

Beheerde schijven kunt u maximaal 10.000 virtuele machine maken **schijven** in een abonnement inschakelen die u voor het maken van de duizenden **VMs** in één abonnement. Deze functie ook verder verhoogt de schaalbaarheid van [virtuele Machine Scale Sets (VMSS)](../articles/virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md) doordat u virtuele machines tot duizend in een VMSS met behulp van een Marketplace-installatiekopie maken.

### <a name="better-reliability-for-availability-sets"></a>Betere betrouwbaarheid voor Beschikbaarheidssets

Beheerde schijven biedt betere betrouwbaarheid voor Beschikbaarheidssets door ervoor te zorgen dat de schijven van [virtuele machines in een Beschikbaarheidsset](../articles/virtual-machines/windows/manage-availability.md#use-managed-disks-for-vms-in-an-availability-set) voldoende los van elkaar te vermijden individuele storingspunten zijn. Dit gebeurt automatisch als u de schijven in verschillende opslagunits (stempels). Als een stempel als gevolg van hardware-of softwarefout mislukt, wordt alleen de VM-exemplaren met schijven op die stempels mislukken. Bijvoorbeeld, Stel dat u hebt een toepassing die wordt uitgevoerd op vijf virtuele machines en de virtuele machines zich in een Beschikbaarheidsset. De schijven verder voor deze VMs won't alle worden opgeslagen in dezelfde tijdstempel, dus als één stamp is uitvalt, worden de andere exemplaren van de toepassing.

### <a name="highly-durable-and-available"></a>Zeer duurzaam en hoge beschikbaarheid

Azure-schijven zijn ontworpen om 99,999% van de tijd beschikbaar te zijn. Rest-eenvoudiger weten dat u hebt drie replica's van uw gegevens waarmee hoge duurzaamheid. Als er zich problemen voordoen met een of zelfs twee van de replica’s, kunnen de resterende replica’s ervoor zorgen dat uw gegevens bewaard blijven en storingen weinig kwaad kunnen. Dankzij deze architectuur heeft Azure consistente duurzaamheid op ondernemingsniveau kunnen leveren voor IaaS-schijven, met een foutpercentage van NUL% op jaarbasis, het beste in de bedrijfstak. 

### <a name="granular-access-control"></a>Gedetailleerd toegangsbeheer

U kunt [gebaseerd toegangsbeheer (RBAC)](../articles/active-directory/role-based-access-control-what-is.md) specifieke machtigingen voor een beheerde schijf toewijzen aan een of meer gebruikers. Beheerd schijven zichtbaar gemaakt talloze voor bewerkingen, inclusief lezen, schrijven (maken/bijwerken), verwijderen en bij het ophalen van een [shared access signature (SAS) URI](../articles/storage/common/storage-dotnet-shared-access-signature-part-1.md) voor de schijf. U kunt toegang verlenen tot alleen de bewerkingen die een persoon moet de taak uitvoeren. Bijvoorbeeld, als u niet dat een persoon een beheerde schijf kopiëren naar een opslagaccount wilt, kunt u niet om toegang te verlenen aan de export-actie voor die beheerde schijf. Op dezelfde manier als u niet dat een persoon een SAS-URI wilt voor het kopiëren van een beheerde schijf gebruiken, kunt u geen machtiging te verlenen die aan de beheerde schijf.

### <a name="azure-backup-service-support"></a>Ondersteuning voor Azure Backup-service
Gebruik Azure Backup-service met beheerde schijven te maken van een back-uptaak met back-ups op basis van tijd, eenvoudig herstel van de virtuele machine en back-up bewaarbeleid. Beheerde schijven ondersteunen alleen lokaal redundante opslag (LRS) als de replicatieoptie; Dit betekent dat drie kopieën van de gegevens in één regio blijven. Voor de regionale noodherstel, u moet back-up van uw VM-schijven in een andere regio met [Azure Backup-service](../articles/backup/backup-introduction-to-azure-backup.md) en een GRS-opslagaccount als back-upkluis. Azure Backup ondersteunt gegevensschijf groottes momenteel maximaal 1TB voor back-up. Meer informatie over deze [met behulp van Azure Backup-service voor virtuele machines met schijven beheerd](../articles/backup/backup-introduction-to-azure-backup.md#using-managed-disk-vms-with-azure-backup).

## <a name="pricing-and-billing"></a>Prijzen en facturering

Wanneer u schijven beheerd, zijn de volgende factureringsvoorwaarden van toepassing:
* Opslagtype

* Schijfgrootte

* Aantal transacties

* Uitgaande gegevensoverdracht

* Beheerde schijf-momentopnamen (volledige schijf kopiëren)

We gaan deze nader bekijken.

**Opslagtype:** beheerde schijven biedt 2 prestatielagen: [Premium](../articles/virtual-machines/windows/premium-storage.md) (SSD-gebaseerde) en [standaard](../articles/virtual-machines/windows/standard-storage.md) (gebaseerd op harde schijf). De facturering van een beheerde schijf is afhankelijk van welk type opslagruimte die u hebt geselecteerd voor de schijf.


**Schijfgrootte**: facturering voor beheerde schijven, is afhankelijk van de ingerichte grootte van de schijf. De grootte van de ingerichte (naar boven afronden) Azure toegewezen aan de dichtstbijzijnde schijven beheerd optie zoals opgegeven in de onderstaande tabellen. Elke beheerde schijf toegewezen aan een van de ondersteunde ingerichte grootten en dienovereenkomstig wordt gefactureerd. Bijvoorbeeld, als u een standard-beheerde schijven maken en geef een ingerichte grootte van 200 GB, u worden kosten in rekening gebracht volgens de prijzen van het type S20 schijf.

Hier volgen de schijfgrootte beschikbaar voor premium-beheerde schijven:

| **Premium beheerd <br>schijftype** | **P4** | **P6** |**P10** | **P15** | **P20** | **P30** | **P40** | **P50** | 
|------------------|---------|---------|---------|---------|---------|----------------|----------------|----------------|  
| Schijfgrootte        | 32 GiB   | 64 GiB   | 128 GiB  | 256 GiB  | 512 GiB  | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 


Hier volgen de schijfgrootte beschikbaar voor een standard-beheerde schijven:

| **Standaard beheerde <br>schijftype** | **S4** | **S6** | **S10** | **S20** | **S30** | **S40** | **S50** |
|------------------|---------|---------|--------|--------|----------------|----------------|----------------| 
| Schijfgrootte        | 32 GiB   | 64 GiB   | 128 GiB | 512 GiB | 1024 GiB (1 TiB) | 2048 GiB (2 TiB) | 4095 GiB (4 TiB) | 


**Het aantal transacties**: U wordt gefactureerd voor het aantal transacties die u op een standard-beheerde schijven uitvoert. Er is geen kosten voor transacties voor een premium-beheerde schijven.

**Uitgaande gegevensoverdracht**: [uitgaande gegevensoverdracht](https://azure.microsoft.com/pricing/details/data-transfers/) (gegevens uit de Azure-datacenters gaan) worden gefactureerd voor bandbreedtegebruik.

Zie voor gedetailleerde informatie over prijzen voor schijven beheerd [beheerd schijven prijzen](https://azure.microsoft.com/pricing/details/managed-disks).


## <a name="managed-disk-snapshots"></a>Beheerde schijf momentopnamen

Een momentopname van een beheerd is een alleen-lezen kopie van een beheerde schijf die wordt opgeslagen als een standard-beheerde schijven standaard. Met momentopnamen, kunt u back-up uw beheerde schijven op elk punt in tijd. Deze momentopnamen onafhankelijk van de bronschijf bestaan en kunnen worden gebruikt voor het maken van nieuwe schijven die worden beheerd. Ze worden gefactureerd op basis van de gebruikte grootte. Bijvoorbeeld, als u een momentopname van een beheerde schijf met ingerichte capaciteit van 64 GiB en gegevens die u gebruikt de grootte van 10 GiB maakt, wordt momentopname gefactureerd alleen voor de gebruikte gegevensgrootte van 10 GiB.  

[Incrementele momentopnamen](../articles/virtual-machines/windows/incremental-snapshots.md) worden momenteel niet ondersteund voor schijven die worden beheerd, maar in de toekomst worden ondersteund.

Controleer deze bronnen voor meer informatie over het maken van momentopnamen met schijven beheerd:

* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Windows](../articles/virtual-machines/windows/snapshot-copy-managed-disk.md)
* [Een kopie maken van een VHD die is opgeslagen als beheerde schijf met behulp van momentopnamen in Linux](../articles/virtual-machines/linux/snapshot-copy-managed-disk.md)


## <a name="images"></a>Installatiekopieën

Beheerde schijven bieden ook ondersteuning voor het maken van een beheerde aangepaste installatiekopie. U kunt een installatiekopie van het maken van uw aangepaste VHD in een opslagaccount of rechtstreeks van een gegeneraliseerde (sys DomainPrep) VM. Dit wordt vastgelegd in één installatiekopie alle schijven die zijn gekoppeld aan een virtuele machine, met inbegrip van zowel het besturingssysteem en gegevensschijven die worden beheerd. Hierdoor maken honderden virtuele machines met uw aangepaste installatiekopie zonder de noodzaak om te kopiëren of beheren van alle opslagaccounts.

Controleer voor meer informatie over het maken van installatiekopieën uit de volgende artikelen:
* [Het vastleggen van een begeleide afbeelding van een gegeneraliseerde virtuele machine in Azure](../articles/virtual-machines/windows/capture-image-resource.md)
* [Het generaliseren en vastleggen van een virtuele Linux-machine met behulp van de Azure CLI 2.0](../articles/virtual-machines/linux/capture-image.md)

## <a name="images-versus-snapshots"></a>Installatiekopieën versus momentopnamen

Vaak ziet u het woord 'installatiekopie' gebruikt met virtuele machines en u ziet nu 'momentopnamen' ook. Het is belangrijk om het verschil tussen deze. U kunt een installatiekopie van een gegeneraliseerde virtuele machine die is opgeheven nemen met schijven beheerd. Deze installatiekopie bevat alle schijven die zijn gekoppeld aan de virtuele machine. U kunt deze installatiekopie van een nieuwe virtuele machine maken en neemt alle schijven.

Een momentopname is een kopie van een schijf op het punt in tijd die nodig is. Dit is alleen van toepassing op één schijf. Als u een virtuele machine die slechts één schijf (OS heeft) hebt, kunt u een momentopname of een installatiekopie van het en een virtuele machine maken van de momentopname of de installatiekopie.

Wat gebeurt er als een virtuele machine heeft vijf schijven en ze worden striped opgeslagen? U kunt een momentopname van elk van de schijven, maar er is geen awareness vanuit de virtuele machine van de status van de schijven – de momentopnamen alleen weten over één schijf. In dit geval de momentopnamen zou moeten zijn met elkaar en die momenteel niet wordt ondersteund.

## <a name="managed-disks-and-encryption"></a>Beheerde schijven en versleuteling

Er zijn twee soorten versleuteling te bespreken met betrekking tot beheerde schijven. De eerste is opslag Service versleuteling (SSE), die wordt uitgevoerd door de storage-service. Het tweede is Azure Disk Encryption toe die u voor uw virtuele machines op het besturingssysteem en gegevensschijven kunt inschakelen.

### <a name="storage-service-encryption-sse"></a>Versleuteling van opslag-Service (SSE)

[Azure Storage-Service: versleuteling](../articles/storage/common/storage-service-encryption.md) biedt versleuteling in rust en bescherming van uw gegevens om te voldoen aan uw organisatie beveiliging en naleving verplichtingen. SSE is standaard ingeschakeld voor alle beheerde schijven, momentopnamen en afbeeldingen in alle regio's waar beheerde schijven beschikbaar is. Starten van 10 juni 2017 worden alle nieuwe beheerde momentopnamen-schijven-installatiekopieën en nieuwe gegevens geschreven naar de bestaande beheerde schijven automatisch versleuteld in rust met sleutels die worden beheerd door Microsoft zijn.  Ga naar de [pagina met veelgestelde vragen voor schijven beheerd](../articles/virtual-machines/windows/faq-for-disks.md#managed-disks-and-storage-service-encryption) voor meer informatie.


### <a name="azure-disk-encryption-ade"></a>Azure Disk Encryption (ADE)

Azure Disk Encryption kunt u voor het versleutelen van het besturingssysteem en gegevensschijven die wordt gebruikt door een virtuele Machine voor IaaS. Dit omvat beheerde schijven. De stations zijn versleuteld met behulp van standaard-BitLocker-versleuteling technologie voor Windows. De schijven zijn versleuteld met behulp van de technologie DM-Crypt voor Linux. Dit is geïntegreerd met Azure Sleutelkluis kunt u te controleren en beheren van de versleutelingssleutels voor de schijf. Zie voor meer informatie [Azure Disk Encryption for Windows en Linux IaaS VM's](../articles/security/azure-security-disk-encryption.md).

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de volgende artikelen voor meer informatie over schijven beheerd.

### <a name="get-started-with-managed-disks"></a>Aan de slag met beheerde schijven

* [Een virtuele machine maken met behulp van Resource Manager en PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm.md)

* [Een virtuele Linux-machine maken met behulp van de Azure CLI 2.0](../articles/virtual-machines/linux/quick-create-cli.md)

* [Een beheerde gegevensschijf koppelen aan een virtuele machine van Windows met behulp van PowerShell](../articles/virtual-machines/windows/attach-disk-ps.md)

* [Een beheerde schijf toevoegen aan een virtuele Linux-machine](../articles/virtual-machines/linux/add-disk.md)

* [Het PowerShell-voorbeeldscripts schijven beheerd](https://github.com/Azure-Samples/managed-disks-powershell-getting-started)

* [Beheerd schijven gebruiken in Azure Resource Manager-sjablonen](../articles/virtual-machines/windows/using-managed-disks-template-deployments.md)

### <a name="compare-managed-disks-storage-options"></a>Opties voor opslag van schijven beheerd vergelijken

* [Premium-opslag en schijven](../articles/virtual-machines/windows/premium-storage.md)

* [Standard-opslag en schijven](../articles/virtual-machines/windows/standard-storage.md)

### <a name="operational-guidance"></a>Gebruiksaanwijzing

* [Migreren van AWS en andere platforms naar schijven beheerd in Azure](../articles/virtual-machines/windows/on-prem-to-azure.md)

* [Azure Virtual machines converteren naar beheerde schijven in Azure](../articles/virtual-machines/windows/migrate-to-managed-disks.md)
