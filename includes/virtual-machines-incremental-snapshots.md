# <a name="back-up-azure-unmanaged-vm-disks-with-incremental-snapshots"></a>Back-up van Azure niet-beheerde VM-schijven met incrementele momentopnamen
## <a name="overview"></a>Overzicht
Azure-opslag biedt de mogelijkheid aan momentopnamen van blobs. De status van de blob vastleggen momentopnamen op dat moment. In dit artikel beschrijft we een scenario waarin u back-ups van de schijven van de virtuele machine met momentopnamen kunt onderhouden. U kunt deze methode wanneer u niet wilt gebruiken voor Azure Backup en Recovery-Service en voor het maken van een aangepaste back-upstrategie voor uw virtuele machine-schijven.

Virtuele machine van Azure-schijven zijn opgeslagen als pagina-blobs in Azure Storage. Aangezien we beschrijving van een back-upstrategie voor virtuele-machineschijven in dit artikel, verwijzen we naar momentopnamen in de context van pagina-blobs. Raadpleeg voor meer informatie over momentopnamen [maken van een momentopname van een Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob).

## <a name="what-is-a-snapshot"></a>Wat is er een momentopname?
Een blob-momentopname is een alleen-lezen-versie van een blob die is opgenomen op een punt in tijd. Wanneer een momentopname is gemaakt, kan deze worden gelezen, gekopieerd, of verwijderd, maar niet gewijzigd. Momentopnamen bieden een manier om back-up van een blob zoals deze wordt weergegeven op een moment. Pas REST versie 2015-04-05 moest u de mogelijkheid om te kopiëren van de volledige momentopnamen. Met de REST-versie 2015-07-08 en hoger, u kunt u ook incrementele momentopnamen kopiëren.

## <a name="full-snapshot-copy"></a>Volledige momentopname kopiëren
Momentopnamen kunnen worden gekopieerd naar een ander opslagaccount als blob naar de back-ups van de basis blob houden. U kunt ook een momentopname kopiëren via de base blob, is vergelijkbaar met het herstellen van de blob naar een eerdere versie. Wanneer een momentopname van een opslagaccount wordt gekopieerd naar een andere, in beslag neemt de dezelfde naamruimte als de basis-pagina-blob. Daarom wordt het hele momentopnamen van één opslagaccount kopiëren naar de andere traag en verbruikt veel ruimte in de doel-opslagaccount.

> [!NOTE]
> Als u de basis blob naar een andere bestemming kopieert, worden de momentopnamen van de blob niet samen met het gekopieerd. Op dezelfde manier als u een basis blob met een kopie overschrijven, momentopnamen die zijn gekoppeld aan de basis blob worden niet beïnvloed door en onder de blobnaam van de basis intact blijven.
> 
> 

### <a name="back-up-disks-using-snapshots"></a>Back-up van schijven met momentopnamen
Als een back-upstrategie voor uw virtuele machine-schijven, u kunt momentopnamen periodieke van de schijf of pagina-blob en kopieer ze naar een andere opslag rekening met hulpprogramma's als [Blob kopiëren](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) bewerking of [AzCopy](../articles/storage/common/storage-use-azcopy.md). U kunt een momentopname kopiëren naar een doel-pagina-blob met een andere naam. De resulterende bestemmings-pagina-blob is een beschrijfbare pagina-blob en niet een momentopname. Verderop in dit artikel worden stappen beschreven om back-ups van de schijven van de virtuele machine met momentopnamen beschreven.

### <a name="restore-disks-using-snapshots"></a>Schijven met momentopnamen terugzetten
Wanneer het tijd om te zetten van de schijf op een stabiele versie die eerder is vastgelegd in een van de back-upmomentopnamen is, kunt u een momentopname via de basispagina blob kopiëren. Nadat de momentopname wordt gepromoveerd voor de basispagina blob, blijft van de momentopname, maar de bron wordt overschreven met een kopie die kan worden gelezen en geschreven. Verderop in dit artikel worden beschreven stappen voor het herstellen van een eerdere versie van de schijf van de momentopname is gemaakt.

### <a name="implementing-full-snapshot-copy"></a>Volledige snapshotkopieën implementeren
U kunt een kopie van de volledige momentopname implementeren door het volgende te doen

* Eerst een momentopname van de basis blob met behulp van de [momentopname Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) bewerking.
* Kopieer vervolgens de momentopname naar een doel storage account met [Blob kopiëren](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob).
* Herhaal dit proces voor het onderhouden van de back-ups van uw base blob.

## <a name="incremental-snapshot-copy"></a>Incrementele snapshotkopieën
De nieuwe functie in de [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) API biedt een veel betere manier om back-up van de momentopnamen van uw pagina-blobs of de schijven. De API retourneert de lijst met wijzigingen tussen de base blob en de momentopnamen die de hoeveelheid opslagruimte die wordt gebruikt voor de back-account reduceert. De API biedt ondersteuning voor pagina-blobs in Premium-opslag, evenals de Standard-opslag. Deze API gebruikt, kunt u snellere en efficiëntere back-oplossingen bouwen voor Azure Virtual machines. Deze API is beschikbaar in de REST-versie 2015-07-08 en hoger.

Incrementele kopie van de momentopname kunt u vanuit één opslagaccount naar de andere kopiëren het verschil tussen,

* Base blob en de momentopname of
* De twee momentopnamen van de basis blob

De volgende voorwaarden wordt voldaan, opgegeven

* De blob is gemaakt op Jan-1-2016 of hoger.
* De blob is niet overschreven met [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) of [Blob kopiëren](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) tussen twee momentopnamen.

**Opmerking**: deze functie is beschikbaar voor Premium en Standard Azure-pagina-Blobs.

Wanneer u een aangepaste back-upstrategie met momentopnamen hebt, kopiëren van de momentopnamen van één opslagaccount naar een andere kan traag zijn en hoeveel opslagruimte kan gebruiken. In plaats van de volledige momentopname kopiëren naar een back-storage-account, kunt u het verschil tussen opeenvolgende momentopnamen om een back-pagina-blob te schrijven. Op deze manier wordt de tijd voor het kopiëren en de ruimte voor het opslaan van back-ups aanzienlijk verminderd.

### <a name="implementing-incremental-snapshot-copy"></a>Incrementele snapshotkopieën implementeren
U kunt incrementele snapshotkopieën implementeren door het volgende te doen

* Een momentopname van het gebruik van de basis blob [momentopname Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob).
* Kopiëren van de momentopname met de doel-upopslag account via [Blob kopiëren](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob). Dit is de back-paginablob. Een momentopname van de back-pagina-blobs en sla deze op in het back-account.
* Maak nog een momentopname van de basis blob met momentopname Blob.
* Het verschil tussen de eerste en tweede momentopnamen van het gebruik van de basis blob ophalen [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges). Gebruik de nieuwe parameter **prevsnapshot**, de DateTime-waarde van de momentopname die u wilt ophalen van het verschil met opgeven. Als deze parameter aanwezig is, betekent dit dat het antwoord REST alleen op de pagina's die zijn gewijzigd tussen doel momentopname en eerdere momentopname, met inbegrip van lege pagina's bevat.
* Gebruik [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) deze wijzigingen toepassen op de back-paginablob.
* Ten slotte een momentopname van de back-pagina-blobs en sla het in het back-storage-account.

In de volgende sectie wordt beschreven in meer detail hoe u back-ups van schijven met incrementele kopie van de momentopname kunt onderhouden

## <a name="scenario"></a>Scenario
In deze sectie beschrijven we een scenario waarbij een aangepaste back-upstrategie voor schijven van de virtuele machine met momentopnamen.

U kunt een DS-serie Azure virtuele machine met een premium-opslag P30 schijf die is gekoppeld. De schijf P30 aangeroepen *mypremiumdisk* wordt opgeslagen in een premium storage-account genoemd *mypremiumaccount*. Standard-opslagaccount aangeroepen *mybackupstdaccount* wordt gebruikt voor het opslaan van de back-up van *mypremiumdisk*. Wij willen graag houden van een momentopname van *mypremiumdisk* elke 12 uur.

Raadpleeg voor meer informatie over het maken van de storage-account en schijven te [over Azure storage-accounts](../articles/storage/storage-create-storage-account.md).

Raadpleeg voor meer informatie over het back-ups van virtuele Azure-machines, [back-ups van virtuele machine van Azure plannen](../articles/backup/backup-azure-vms-introduction.md).

## <a name="steps-to-maintain-backups-of-a-disk-using-incremental-snapshots"></a>Stappen voor het beheren van back-ups van een schijf met een incrementele momentopnamen
De volgende stappen wordt beschreven hoe u momentopnamen van *mypremiumdisk* en onderhouden van de back-ups in *mybackupstdaccount*. De back-up is een standaard pagina-blob aangeroepen *mybackupstdpageblob*. De back-paginablob geeft altijd dezelfde toestand als de laatste momentopname van *mypremiumdisk*.

1. De back-pagina-blob voor de premium-opslag-schijf maken door het maken van een momentopname van *mypremiumdisk* aangeroepen *mypremiumdisk_ss1*.
2. Deze momentopname als een pagina-blob aangeroepen kopiëren naar mybackupstdaccount *mybackupstdpageblob*.
3. Een momentopname van *mybackupstdpageblob* aangeroepen *mybackupstdpageblob_ss1*met [momentopname Blob](https://docs.microsoft.com/rest/api/storageservices/Snapshot-Blob) en op te slaan in *mybackupstdaccount*.
4. Tijdens het back-upvenster maken nog een momentopname van *mypremiumdisk*, spreek *mypremiumdisk_ss2*, en op te slaan in *mypremiumaccount*.
5. Ophalen van de incrementele wijzigingen tussen de twee momentopnamen *mypremiumdisk_ss2* en *mypremiumdisk_ss1*met [GetPageRanges](https://docs.microsoft.com/rest/api/storageservices/Get-Page-Ranges) op *mypremiumdisk_ ss2* met de **prevsnapshot** parameter ingesteld op de tijdstempel van *mypremiumdisk_ss1*. Deze incrementele wijzigingen geschreven naar de back-paginablob *mybackupstdpageblob* in *mybackupstdaccount*. Als er in de incrementele wijzigingen verwijderde bereiken, moeten ze worden gewist van de back-paginablob. Gebruik [PutPage](https://docs.microsoft.com/rest/api/storageservices/Put-Page) incrementele wijzigingen schrijven naar de back-paginablob.
6. Een momentopname van de back-paginablob *mybackupstdpageblob*die *mybackupstdpageblob_ss2*. Verwijderen van de vorige momentopname *mypremiumdisk_ss1* van premium storage-account.
7. Herhaal stap 4-6 elke back-upvenster. Op deze manier kunt u back-ups van blijven *mypremiumdisk* in een standard-opslagaccount.

![Maak een back-up van schijf met behulp van incrementele momentopnamen](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-1.png)

## <a name="steps-to-restore-a-disk-from-snapshots"></a>Stappen voor het herstellen van een schijf van momentopnamen
De volgende stappen beschrijven het herstellen van de schijf premium *mypremiumdisk* naar een eerdere momentopname van de back-upopslag account *mybackupstdaccount*.

1. Het punt te identificeren in de tijd die u wilt de premium-schijf te herstellen. Stel de machine momentopnamen *mybackupstdpageblob_ss2*, die is opgeslagen in het account van de back-upopslag *mybackupstdaccount*.
2. In mybackupstdaccount, bevordering van de momentopname *mybackupstdpageblob_ss2* als de nieuwe back-up basispagina blob *mybackupstdpageblobrestored*.
3. Een momentopname van deze herstelde back-pagina-blob, aangeroepen *mybackupstdpageblobrestored_ss1*.
4. De herstelde paginablob kopiëren *mybackupstdpageblobrestored* van *mybackupstdaccount* naar *mypremiumaccount* als de nieuwe schijf met premium *mypremiumdiskrestored*.
5. Een momentopname van *mypremiumdiskrestored*die *mypremiumdiskrestored_ss1* voor toekomstige incrementele back-ups maken.
6. De DS-serie VM verwijzen naar de teruggezette schijf *mypremiumdiskrestored* en loskoppelen van de oude *mypremiumdisk* van de virtuele machine.
7. Beginnen met de back-up wordt beschreven in de vorige sectie voor de herstelde schijf *mypremiumdiskrestored*, waarbij de *mybackupstdpageblobrestored* als de back-pagina-blob.

![Schijf terugzetten van momentopnamen](../articles/virtual-machines/windows/media/incremental-snapshots/storage-incremental-snapshots-2.png)

## <a name="next-steps"></a>Volgende stappen
Gebruik de volgende koppelingen voor meer informatie over het maken van momentopnamen van een blob en het plannen van uw back-upinfrastructuur VM.

* [Maken van een momentopname van een Blob](https://docs.microsoft.com/rest/api/storageservices/Creating-a-Snapshot-of-a-Blob)
* [Uw VM-back-infrastructuur plannen](../articles/backup/backup-azure-vms-introduction.md)

