# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Veelgestelde vragen over Azure IaaS VM-schijven en beheerde en onbeheerde premium-schijven

Dit artikel worden enkele veelgestelde vragen over Azure beheerd schijven en Azure Premium-opslag.

## <a name="managed-disks"></a>Managed Disks

**Wat is Azure beheerd schijven?**

Beheerde schijven is een functie die vereenvoudigt Schijfbeheer voor Azure IaaS VM's met opslagbeheer-account voor u verwerken. Zie voor meer informatie de [schijven beheerd overzicht](../articles/virtual-machines/windows/managed-disks-overview.md).

**Als ik een standard-beheerde schijven van een bestaande VHD 80 GB maken, hoeveel die kost mij?**

Een standard-beheerde schijven gemaakt op basis van een VHD 80 GB wordt beschouwd als de volgende beschikbare standaard schijfgrootte, die een schijf S10. U kosten in rekening gebracht volgens de S10 schijf prijzen. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Zijn er transactiekosten voor standard-beheerde schijven?**

Ja. U kosten in rekening gebracht voor elke transactie. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Voor een standaard beheerde schijf wordt ik in rekening gebracht voor de werkelijke grootte van de gegevens op de schijf of voor de ingerichte capaciteit van de schijf?**

U bent in rekening gebracht op basis van de ingerichte capaciteit van de schijf. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Hoe wordt de prijzen van beheerde premium-schijven verschilt van niet-beheerde schijven?**

De prijzen van beheerde premium-schijven is hetzelfde als niet-beheerde premium-schijven.

**Kan ik het opslagaccounttype (Standard of Premium) van mijn beheerde schijven wijzigen?**

Ja. U kunt het opslagtype voor de account van uw beheerde schijven wijzigen met behulp van de Azure-portal, PowerShell of Azure CLI.

**Is er een manier die ik kan kopiëren of een beheerde schijf exporteren naar een persoonlijke storage-account?**

Ja. U kunt uw beheerde schijven exporteren met behulp van de Azure-portal, PowerShell of Azure CLI.

**Kan ik een VHD-bestand in Azure storage-account gebruiken voor het maken van een beheerde schijf met een ander abonnement?**

Nee.

**Kan ik een VHD-bestand in Azure storage-account gebruiken voor het maken van een beheerde schijf in een andere regio?**

Nee.

**Zijn er beperkingen scale voor klanten die gebruikmaken van beheerde schijven?**

Beheerde schijven elimineert de grenzen die aan opslagaccounts is gekoppeld. Het aantal beheerde schijven per abonnement is echter standaard beperkt tot 2000. U kunt ondersteuning voor dit aantal verhoogt aanroepen.

**Kan ik een incrementele momentopname van een beheerde schijf volgen?**

Nee. De huidige momentopname maakt een volledige kopie van een beheerde schijf. We zijn echter van plan om ondersteuning voor incrementele momentopnamen in de toekomst.

**Kunnen de virtuele machines in een beschikbaarheidsset bestaan uit een combinatie van beheerde en onbeheerde schijven?**

Nee. De virtuele machines in een beschikbaarheidsset moeten gebruiken voor alle beheerde schijven of alle niet-beheerde schijven. Wanneer u een beschikbaarheidsset maakt, kunt u kiezen welk type schijven dat u wilt gebruiken.

**Is beheerd schijven op de standaardoptie in de Azure portal?**

Momenteel niet, maar deze wordt standaard in de toekomst.

**Kan ik een lege beheerde schijf maken?**

Ja. U kunt een lege schijf maken. Een beheerde schijf kan worden gemaakt onafhankelijk van een virtuele machine, bijvoorbeeld, zonder het image koppelt aan een virtuele machine.

**Wat is het aantal ondersteunde foutdomeinen voor een beschikbaarheid instellen die gebruikmaakt van schijven beheerd**

Afhankelijk van de regio waar de beschikbaarheidsset die gebruikmaakt van schijven beheerd zich bevindt, is het aantal ondersteunde foutdomeinen 2 of 3.

**Hoe wordt het standaard opslagaccount voor diagnostische gegevens instellen?**

Instellen van een persoonlijke opslagaccount voor diagnostische gegevens van virtuele machine. We zullen in de toekomst ook overschakelen van diagnostische gegevens naar de schijven worden beheerd.

**Wat voor soort toegangsbeheer op basis van rollen ondersteuning is beschikbaar voor schijven beheerd?**

Schijven ondersteunt drie belangrijkste standaardrollen beheerd:

* Eigenaar: Kunnen alles beheren, inclusief toegang
* Inzender: Kunnen alles beheren behalve toegang
* Lezer: Kunnen alles weergeven, maar geen wijzigingen aanbrengen

**Is er een manier die ik kan kopiëren of een beheerde schijf exporteren naar een persoonlijke storage-account?**

U kunt een alleen-lezen shared access signature voor URI ophalen voor de beheerde schijf en de inhoud te kopiëren naar een persoonlijke opslag account of on-premises opslag gebruiken.

**Kan ik een kopie van de beheerde computer maken?**

Klanten kunnen een momentopname van het bijbehorende beheerde schijven en gebruik vervolgens de momentopname maken van een andere beheerde schijf.

**Worden niet-beheerde schijven nog steeds ondersteund?**

Ja. Wij ondersteunen onbeheerde en beheerde schijven. U wordt aangeraden dat u beheerde schijven voor nieuwe workloads en migreren van uw huidige werkbelastingen naar beheerde schijven.


**Als ik een schijf van 128 GB maken en vervolgens de grootte tot 130 GB te verhogen, ik gefactureerd voor de volgende schijfgrootte (512 GB)?**

Ja.

**Kan ik geografisch redundante opslag met lokaal redundante opslag maken en zone-redundante opslag schijven die worden beheerd?**

Azure-beheerde schijven ondersteunt momenteel alleen lokaal redundante opslag beheerd schijven.

**Kan ik verkleinen of mijn beheerde schijven afslanken?**

Nee. Deze functie is momenteel niet ondersteund. 

**Kan ik de naameigenschap van de computer wijzigen wanneer een gespecialiseerde (niet gemaakt met behulp van het hulpprogramma voor systeemvoorbereiding of gegeneraliseerd) schijf van besturingssysteem wordt gebruikt voor het inrichten van een virtuele machine?**

Nee. U kunt de naameigenschap van de computer niet bijwerken. De nieuwe virtuele machine overgenomen van de bovenliggende VM die is gebruikt voor het maken van de besturingssysteemschijf. 

**Waar vind ik Azure Resource Manager voorbeeldsjablonen virtuele machines maken met beheerde-schijven**
* [Lijst met sjablonen met schijven beheerd](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

## <a name="managed-disks-and-storage-service-encryption"></a>Beheerd schijven en versleuteling van opslag-Service 

**Is Azure Storage-Service: versleuteling standaard ingeschakeld wanneer ik een beheerde schijf maken?**

Ja.

**De versleutelingssleutels die het account?**

Microsoft beheert de versleutelingssleutels.

**Kan ik voor mijn beheerde schijven versleuteling van opslag Service uitschakelen?**

Nee.

**Is versleuteling van opslag Service alleen beschikbaar in specifieke gebieden?**

Nee. Het is beschikbaar in alle regio's waar beheerd schijven beschikbaar is. Beheerde schijven is beschikbaar in alle openbare regio's en Duitsland.

**Hoe vind ik als mijn beheerde schijf worden versleuteld?**

U vindt hier de tijd waarop een beheerde schijf is gemaakt in de Azure-portal, Azure CLI en PowerShell. De schijf wordt versleuteld als de tijd na 9 juni 2017. 

**Hoe kan ik mijn bestaande schijven die zijn gemaakt vóór 10 juni 2017 coderen?**

Vanaf 10 juni 2017 nieuwe gegevens geschreven naar de bestaande beheerde schijven automatisch versleuteld. We ook van plan bent om bestaande gegevens te coderen en de versleuteling gebeurt asynchroon op de achtergrond. Als u bestaande gegevens moet nu worden versleuteld, maakt u een kopie van de schijf. Nieuwe schijf wordt versleuteld.

* [Beheerde schijven kopiëren met behulp van de Azure CLI](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [Beheerde schijven met behulp van PowerShell kopiëren](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

**Zijn beheerde momentopnamen en installatiekopieën versleuteld?**

Ja. Alle beheerde momentopnamen en installatiekopieën die zijn gemaakt na 9 juni 2017 worden automatisch versleuteld. 

**Kan ik virtuele machines met niet-beheerde schijven die zich bevinden op opslagaccounts die zijn of die eerder zijn versleuteld met beheerde schijven converteren**

Ja

**Een geëxporteerde VHD van een beheerde schijf of een momentopname ook worden versleuteld?**

Nee. Maar als u een VHD naar een versleutelde storage-account van een gecodeerd exporteren beheerd schijf of een momentopname en ze zijn versleuteld. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premium-schijven: beheerde en onbeheerde

**Als een virtuele machine gebruikmaakt van een reeks grootte die ondersteuning biedt voor Premium-opslag, zoals een DSv2 kan ik koppelen zowel premium en standard gegevensschijven?** 

Ja.

**Kan ik zowel premium en standard gegevensschijven koppelen aan een reeks grootte die biedt geen ondersteuning voor Premium-opslag, zoals D, Dv2, G of F-serie?**

Nee. U kunt alleen standaard gegevensschijven koppelen aan virtuele machines die geen gebruikmaken van een reeks grootte die ondersteuning biedt voor Premium-opslag.

**Als ik een gegevensschijf premium van een bestaande VHD die 80 GB maken is, hoeveel die kost?**

Een gegevensschijf premium is gemaakt op basis van een VHD 80 GB wordt beschouwd als de grootte van de schijf beschikbaar zijn voor het volgende premium, die een schijf P10. U kosten in rekening gebracht volgens de P10 schijf prijzen.

**Zijn er transactiekosten Premium-opslag gebruiken?**

Er is een vaste kosten voor de grootte van elke schijf, dat wordt meegeleverd met specifieke limieten ingerichte op IOPS en doorvoerlimieten. De andere kosten zijn uitgaande bandbreedte en capaciteit van de momentopname, indien van toepassing. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Wat zijn de limieten voor IOPS en doorvoerlimieten die ik van de schijfcache ophalen kan?**

De gecombineerde limieten voor cache en lokale SSD voor een reeks DS zijn 4000 IOP's per core en 33 MB per seconde per core. De reeks GS biedt 5000 IOP's per core en 50 MB per seconde per core.

**Wordt de lokale SSD ondersteund voor een VM-schijven beheerd?**

De lokale SSD is tijdelijke opslag die is opgenomen in een VM-schijven worden beheerd. Er is geen extra kosten verbonden voor deze tijdelijke opslag. U wordt aangeraden dat u deze lokale SSD niet gebruiken voor het opslaan van uw toepassingsgegevens omdat deze is niet in Azure Blob-opslag permanent.

**Zijn er gevolgen voor het gebruik van TRIM op premium-schijven?**

Er is geen nadeel van het gebruik van TRIM op Azure ofwel premium-schijven of standaardschijven.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Nieuwe schijfgrootten: beheerde en onbeheerde

**Wat is de grootste schijfgrootte voor het besturingssysteem en gegevensschijven ondersteund?**

Het partitietype die Azure biedt ondersteuning voor de schijf van een besturingssysteem is de master bootrecord (MBR). Het formaat van de MBR-indeling ondersteunt een schijf 2 TB. De maximale grootte aan die Azure biedt ondersteuning voor de schijf van een besturingssysteem is 2 TB. Azure ondersteunt maximaal 4 TB voor gegevensschijven. 

**Wat is de grootste blob paginagrootte die wordt ondersteund?**

Pagina is-blob de maximumgrootte die ondersteuning biedt voor Azure 8 TB (8.191 GB). Pagina-blobs die groter zijn dan 4 TB (4095 GB) gekoppeld aan een virtuele machine als gegevens of besturingssysteem schijven worden niet ondersteund.

**Moet ik een nieuwe versie van Azure-hulpprogramma's kunt maken, te koppelen, te vergroten of verkleinen en uploaden schijven groter dan 1 TB?**

U hoeft niet bijwerken van uw bestaande Azure-hulpprogramma's voor het maken, koppelen of vergroten of verkleinen schijven groter dan 1 TB. Als u wilt uw VHD-bestand van on-premises naar Azure rechtstreeks als een pagina-blob of een niet-beheerde schijf uploadt, moet u de nieuwste hulpprogramma's gebruiken:

|Azure-hulpprogramma 's      | Ondersteunde versies                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Versienummer 4.1.0: release van juni 2017 of hoger|
|Azure CLI v1     | Versienummer 0.10.13: mei 2017 release of hoger|
|AzCopy           | Versienummer 6.1.0: release van juni 2017 of hoger|

De ondersteuning voor Azure CLI v2 en Azure Storage Explorer is binnenkort beschikbaar. 

**Worden P4 en P6 schijfgrootten ondersteund voor niet-beheerde schijven of pagina-blobs?**

Nee. P4 (32 GB) en P6 schijfgrootten (64 GB) worden alleen ondersteund voor beheerde schijven. Ondersteuning voor niet-beheerde schijven en pagina-blobs is binnenkort beschikbaar.

**Als mijn bestaande premium beheerd schijf kleiner is dan 64 GB is gemaakt voordat de kleine schijf (rond 15 juni 2017) is ingeschakeld, hoe wordt deze in rekening gebracht?**

Bestaande kleine premium schijven minder dan 64 GB worden in rekening gebracht volgens de prijscategorie P10 blijven. 

**Hoe kan ik de schijf-laag van kleine premium-schijven bevat die kleiner is dan 64 GB van P10 P4 of P6 overschakelen?**

U kunt een momentopname van de kleine schijven en maak vervolgens een schijf voor de prijscategorie automatisch schakelen P4 of op basis van de grootte van de ingerichte P6. 


## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?

Als uw vraag hier niet is vermeld, laat ons weten en wij helpen u bij een antwoord vinden. U kunt een vraag aan het einde van dit artikel plaatsen in de opmerkingen. Als u wilt gaan met het Azure Storage-team en andere communityleden over dit artikel, gebruiken het MSDN [Azure Storage-forum](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).

Voor het aanvragen van functies, de aanvragen en ideeën voor het indienen de [forum met feedback van Azure Storage](https://feedback.azure.com/forums/217298-storage).
