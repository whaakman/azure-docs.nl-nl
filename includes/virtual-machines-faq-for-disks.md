---
title: bestand opnemen
description: bestand opnemen
services: virtual-machines
author: roygara
ms.service: virtual-machines
ms.topic: include
ms.date: 03/28/2018
ms.author: rogarana
ms.custom: include file
ms.openlocfilehash: 61f65340c3b683674be195f1d30788494b6855a7
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2019
ms.locfileid: "58671889"
---
# <a name="frequently-asked-questions-about-azure-iaas-vm-disks-and-managed-and-unmanaged-premium-disks"></a>Veelgestelde vragen over Azure IaaS VM-schijven en beheerde en onbeheerde premium-schijven

In dit artikel vindt u antwoorden op enkele veelgestelde vragen over Azure Managed Disks en Azure Premium-SSD-schijven.

## <a name="managed-disks"></a>Beheerde schijven

**Wat is Azure Managed Disks?**

Beheerde schijven is een functie die vereenvoudigt u Schijfbeheer voor Azure IaaS VM's door het verwerken van opslagaccountbeheer voor u. Zie voor meer informatie de [overzicht Managed Disks](../articles/virtual-machines/windows/managed-disks-overview.md).

**Als ik een standard-beheerde schijven van een bestaande VHD die 80 GB is maken, hoeveel dat kost mij?**

Een standard-beheerde schijven gemaakt op basis van een VHD 80 GB wordt beschouwd als de volgende beschikbare standaardschijf grootte, die een S10-schijf. U kosten in rekening gebracht volgens de prijzen van S10 schijf. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Zijn er transactiekosten voor standaard beheerde schijven?**

Ja. U betaalt voor elke transactie. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Voor een standaard beheerde schijf, moet ik betalen voor de werkelijke grootte van de gegevens op de schijf of voor de ingerichte capaciteit van de schijf?**

U betaalt op basis van de ingerichte capaciteit van de schijf. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Hoe verschilt van niet-beheerde schijven prijzen van premium-beheerde schijven?**

De prijs van premium-beheerde schijven, is hetzelfde als niet-beheerde premium-schijven.

**Kan ik het opslagaccounttype (Standard of Premium) van de beheerde schijven wijzigen?**

Ja. U kunt het opslagaccounttype van uw beheerde schijven wijzigen met behulp van de Azure-portal, PowerShell of Azure CLI.

**Kan ik een VHD-bestand in een Azure storage-account gebruiken om te maken van een beheerde schijf met een ander abonnement?**

Ja.

**Kan ik een VHD-bestand in een Azure storage-account gebruiken om te maken van een beheerde schijf in een andere regio?**

Nee.

**Zijn er beperkingen voor klanten die gebruikmaken van beheerde schijven schaal?**

Beheerde schijven wordt voorkomen dat de limieten die zijn gekoppeld aan de storage-accounts. De maximumlimiet is echter 50.000 beheerde schijven per regio en per schijftype voor een abonnement.

**Kan ik een momentopname van een incrementele van een beheerde schijf maken?**

Nee. De huidige nemen van snapshots maakt een volledige kopie van een beheerde schijf.

**Kunnen de virtuele machines in een beschikbaarheidsset bestaan uit een combinatie van beheerde en onbeheerde schijven?**

Nee. De virtuele machines in een beschikbaarheidsset moeten alle beheerde schijven of alle niet-beheerde schijven gebruiken. Wanneer u een beschikbaarheidsset maakt, kunt u kiezen welk type schijven dat u wilt gebruiken.

**De standaardoptie in de Azure-portal voor Managed Disks is?**

Ja.

**Kan ik een lege beheerde schijf maken?**

Ja. U kunt een lege schijf maken. Een beheerde schijf kan worden gemaakt onafhankelijk van een VM bijvoorbeeld zonder deze te koppelen aan een virtuele machine.

**Wat is het aantal ondersteunde foutdomeinen voor een beschikbaarheid instellen die gebruikmaakt van Managed Disks**

Afhankelijk van de regio waar de beschikbaarheidsset die gebruikmaakt van Managed Disks zich bevindt, is het aantal ondersteunde foutdomeinen 2 of 3.

**Hoe wordt het standard-opslagaccount voor diagnostische gegevens instellen?**

Instellen van een persoonlijke opslagaccount voor diagnostische gegevens van virtuele machine.

**Wat voor soort Role-Based Access Control-ondersteuning is beschikbaar voor Managed Disks?**

Beheerde schijven ondersteunt drie belangrijke standaardrollen:

* Eigenaar: Kan alles beheren, inclusief toegang
* Inzender: Kan alles beheren behalve toegang
* Lezer: Alles weergeven, maar kan geen wijzigingen aanbrengen

**Is er een manier dat ik kan kopiëren of een beheerde schijf naar een persoonlijke storage-account exporteren?**

U kunt een alleen-lezen shared access signature (SAS) URI voor de beheerde schijf genereren en deze gebruiken om de inhoud kopiëren naar een persoonlijke opslag account of on-premises opslag. U kunt de SAS-URI met behulp van de Azure portal, Azure PowerShell of de Azure CLI, of [AzCopy](../articles/storage/common/storage-use-azcopy.md)

**Kan ik een kopie van de beheerde schijf maken?**

Klanten kunnen een momentopname van de beheerde schijven en vervolgens de momentopname gebruiken om een andere beheerde schijf te maken.

**Worden niet-beheerde schijven nog steeds ondersteund?**

Ja, niet-beheerde en beheerde schijven worden ondersteund. U wordt aangeraden dat u beheerde schijven voor nieuwe workloads gebruiken en uw huidige workloads naar managed disks migreren.

**Kan ik dezelfde niet-beheerde en beheerde schijven op dezelfde VM plaatsen?**

Nee.

**Als ik een schijf van 128 GB maken en vervolgens de grootte tot 130 gibibytes (GiB verhogen), moet ik betalen voor de volgende schijfgrootte (256 GB)?**

Ja.

**Kan ik lokaal redundante opslag, geografisch redundante opslag, maken en zone-redundante opslag met beheerde schijven?**

Azure Managed Disks ondersteunt momenteel alleen lokaal redundante opslag met beheerde schijven.

**Kan ik verkleinen of krimpen mijn beheerde schijven?**

Nee. Deze functie wordt momenteel niet ondersteund.

**Kan ik een lease verbreken op de schijf?**

Nee. Dit wordt momenteel niet ondersteund als een lease aanwezig is om te voorkomen dat per ongeluk verwijderen als de schijf wordt gebruikt.

**Kan ik de naameigenschap van de computer wijzigen wanneer een gespecialiseerde (niet gemaakt met behulp van het hulpprogramma voor systeemvoorbereiding of gegeneraliseerd) besturingssysteemschijf wordt gebruikt voor het inrichten van een virtuele machine?**

Nee. U kunt de naameigenschap van de computer niet bijwerken. De nieuwe virtuele machine overgenomen van de bovenliggende VM, die is gebruikt voor het maken van de besturingssysteemschijf. 

**Waar vind ik een voorbeeld van Azure Resource Manager-sjablonen te maken van virtuele machines met beheerde schijven?**
* [Lijst met sjablonen met behulp van Managed Disks](https://github.com/Azure/azure-quickstart-templates/blob/master/managed-disk-support-list.md)
* https://github.com/chagarw/MDPP

**Bij het maken van een schijf van een blob, is er geen voortdurend bestaande relatie met die bron-blob?**

Nee, wanneer de nieuwe schijf wordt gemaakt op dat moment een volledig zelfstandige kopie van die blob is en er is geen verbinding tussen de twee. Als u dat wilt, als u de schijf hebt gemaakt, kan de bron-blob kan worden verwijderd zonder gevolgen voor de zojuist gemaakte schijf op geen enkele manier.

**Kan ik een beheerde of niet-beheerde schijf wijzigen nadat deze is gemaakt?**

Voor beheerde schijven kan niet u deze wijzigen. U mag een niet-beheerde schijf echter wijzigen, zolang het is momenteel niet gekoppeld aan een VHD of de virtuele machine.

**Kan ik de GPT-partities op een Azure-schijf gebruiken?**

GPT partitioneren kan alleen op gegevensschijven, niet de OS-schijven worden gebruikt. Besturingssysteemschijven moeten de partitiestijl MBR gebruiken.

## <a name="standard-ssd-disks"></a>Standard-SSD-schijven

**Wat zijn Azure Standard-SSD-schijven?**
Standard-SSD-schijven worden ondersteund door SSD media, geoptimaliseerd als voordelige opslag voor workloads die consistente prestaties op lagere niveaus van IOPS moet standaardschijven.

<a id="standard-ssds-azure-regions"></a>**Wat zijn de regio's die momenteel worden ondersteund voor Standard-SSD-schijven?**
Alle Azure-regio's bieden nu ondersteuning voor Standard-SSD-schijven.

**Is Azure Backup beschikbaar bij het gebruik van Standard-SSD's?**
Ja, Azure Backup is nu beschikbaar.

**Hoe kan ik de Standard-SSD-schijven maken?**
U kunt Standard-SSD-schijven met behulp van Azure Resource Manager-sjablonen, SDK, PowerShell of CLI maken. Hieronder vindt u de parameters nodig in het Resource Manager-sjabloon voor het maken van de Standard-SSD-schijven:

* *apiVersion* voor Microsoft.Compute moet worden ingesteld als `2018-04-01` (of hoger)
* Geef *managedDisk.storageAccountType* als `StandardSSD_LRS`

Het volgende voorbeeld wordt de *properties.storageProfile.osDisk* sectie voor een virtuele machine die gebruikmaakt van de Standard-SSD-schijven:

```json
"osDisk": {
    "osType": "Windows",
    "name": "myOsDisk",
    "caching": "ReadWrite",
    "createOption": "FromImage",
    "managedDisk": {
        "storageAccountType": "StandardSSD_LRS"
    }
}
```

Zie voor een voorbeeld van de volledige sjabloon van het maken van een Standard-SSD-schijf met een sjabloon, [een virtuele machine maken vanaf een Windows-installatiekopie met de Standard-SSD-gegevensschijven](https://github.com/azure/azure-quickstart-templates/tree/master/101-vm-with-standardssd-disk/).

**Kan ik mijn bestaande schijven converteren naar Standard-SSD?**
Ja, dat is mogelijk. Raadpleeg [converteren Azure managed disks-opslag van standard naar premium, en vice versa](https://docs.microsoft.com/azure/virtual-machines/windows/convert-disk-storage) voor de algemene richtlijnen voor het converteren van Managed Disks. En gebruik de volgende waarde voor het bijwerken van het schijftype naar de Standard-SSD.
-AccountType StandardSSD_LRS

**Wat is het voordeel van het gebruik van Standard-SSD-schijven in plaats van HDD?**
Standard-SSD-schijven bieden betere latentie, consistentie, beschikbaarheid en betrouwbaarheid ten opzichte van HDD-schijven. Werkbelastingen van toepassingen soepeler veel op Standard-SSD vanwege die. Houd er rekening mee, Premium-SSD-schijven zijn de aanbevolen oplossing voor de meeste i/o-intensieve productieworkloads.

**Kan ik standaard SSD's als niet-beheerde schijven gebruiken?**
Nee, Standard-SSD-schijven zijn alleen beschikbaar als beheerde schijven.

**Standard-SSD-schijven bieden ondersteuning voor 'single instance VM SLA'?**
Nee, Standard-SSD's geen enkel exemplaar VM SLA. Premium-SSD-schijven gebruiken voor één exemplaar VM SLA.

## <a name="migrate-to-managed-disks"></a>Migreren naar Managed Disks

**Zijn er gevolgen van de migratie op de prestaties van Managed Disks?**

Migratie omvat het verkeer van de schijf van de ene opslaglocatie naar de andere. Dit is georganiseerd via achtergrond kopiëren van gegevens, die kan enkele uren duren, meestal minder dan 24 uur per dag uur, afhankelijk van de hoeveelheid gegevens in de schijven. Gedurende die tijd kunt uw toepassing ervaringen hoger dan normaal leeslatentie bij het aantal leesbewerkingen kunnen ophalen omgeleid naar de oorspronkelijke locatie en kunnen het langer duren om. Er zijn geen gevolgen voor schrijven latentie tijdens deze periode.  

**Welke wijzigingen vereist zijn in een bestaande Azure Backup-service configuration voorafgaande/na de migratie naar Managed Disks?**

Er zijn geen wijzigingen vereist.

**Mijn virtuele machine back-ups die zijn gemaakt via Azure Backup-service vóór de migratie blijven werken?**

Ja, back-ups werken naadloos.

**Welke wijzigingen vereist zijn in een bestaande Azure-schijven versleuteling configuratie voorafgaande/na de migratie naar Managed Disks?**

Er zijn geen wijzigingen vereist.

**Wordt automatische migratie van een bestaande virtuele-machineschaalset ingesteld van niet-beheerde schijven naar beheerde schijven die worden ondersteund?**

Nee. U kunt een nieuwe schaalset met Managed Disks met behulp van de installatiekopie van uw oude schaalset met niet-beheerde schijven kunt maken.

**Kan ik een beheerde schijf maken van een pagina-blob-momentopname gemaakt voordat u migreert naar Managed Disks?**

Nee. U kunt een pagina-blob-momentopname exporteren als een pagina-blob en maak vervolgens een beheerde schijf van de geëxporteerde pagina-blob.

**Kan ik mijn on-premises computers beveiligd door Azure Site Recovery voor een virtuele machine met Managed Disks failover?**

Ja, u kunt kiezen om failover naar een virtuele machine met Managed Disks.

**Is er gevolgen van de migratie op Azure Virtual machines beveiligd door Azure Site Recovery via replicatie van Azure naar Azure?**

Ja. Azure Site Recovery Azure naar Azure-bescherming voor virtuele machines met Managed Disks is momenteel beschikbaar als een service voor algemene beschikbaarheid.

**Kan ik virtuele machines migreren met niet-beheerde schijven die zich bevinden op storage-accounts die zijn of die eerder naar managed disks zijn versleuteld?**

Ja

## <a name="managed-disks-and-storage-service-encryption"></a>Beheerde schijven en Storage-Serviceversleuteling

**Azure Storage-Serviceversleuteling standaard zijn ingeschakeld wanneer ik een beheerde schijf maken?**

Ja.

**Wie de versleutelingssleutels beheert?**

Microsoft beheert de versleutelingssleutels.

**Kan ik Storage Service Encryption voor mijn beheerde schijven uitschakelen?**

Nee.

**Is Storage Service Encryption alleen beschikbaar in bepaalde regio's?**

Nee. Het is beschikbaar in alle regio's waar Managed Disks beschikbaar zijn. Beheerde schijven is beschikbaar in alle openbare regio's en Duitsland. Het is ook beschikbaar in China, maar alleen voor Microsoft sleutels, niet beheerde de klant beheerde sleutels.

**Hoe vind ik informatie als de beheerde schijf wordt versleuteld?**

U vindt de tijd waarop een beheerde schijf is gemaakt in Azure portal, de Azure CLI en PowerShell. Als de tijd na 9 juni 2017, en vervolgens de schijf worden versleuteld.

**Hoe kan ik mijn bestaande schijven die zijn gemaakt vóór 10 juni 2017 coderen?**

Vanaf 10 juni 2017, nieuwe gegevens geschreven naar bestaande beheerde schijven automatisch versleuteld. We zijn ook van plan om bestaande gegevens te versleutelen en de versleuteling wordt asynchroon uitgevoerd op de achtergrond. Als u nu bestaande gegevens versleutelen moet, maakt u een kopie van de schijf. Nieuwe schijven worden, versleuteld.

* [Beheerde schijven kopiëren met behulp van de Azure CLI](../articles/virtual-machines/scripts/virtual-machines-linux-cli-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)
* [Beheerde schijven kopiëren met behulp van PowerShell](../articles/virtual-machines/scripts/virtual-machines-windows-powershell-sample-copy-managed-disks-to-same-or-different-subscription.md?toc=%2fcli%2fmodule%2ftoc.json)

**Beheerde momentopnamen en installatiekopieën die versleuteld zijn?**

Ja. Alle beheerde momentopnamen en installatiekopieën die zijn gemaakt na 9 juni 2017 worden automatisch versleuteld. 

**Kan ik virtuele machines converteren met niet-beheerde schijven die zich bevinden op storage-accounts die zijn of die eerder naar managed disks zijn versleuteld?**

Ja

**Een geëxporteerde VHD van een beheerde schijf of momentopname van een ook versleuteld?**

Nee. Maar als u een VHD naar een versleuteld opslagaccount van een versleutelde exporteren beheerde schijf of momentopname en vervolgens het is versleuteld. 

## <a name="premium-disks-managed-and-unmanaged"></a>Premium-schijven: Beheerde en onbeheerde

**Als een virtuele machine gebruikmaakt van een reeks grootte die ondersteuning biedt voor Premium-SSD-schijven, zoals een DSv2 kan ik koppelen premium en standard gegevensschijven?** 

Ja.

**Kan ik premium en standard gegevensschijven koppelen aan een reeks grootte die Premium-SSD-schijven, zoals D, Dv2, G of F-serie biedt geen ondersteuning?**

Nee. U kunt alleen standaard gegevensschijven koppelen aan virtuele machines die niet gebruikmaken van een reeks grootte die ondersteuning biedt voor Premium-SSD-schijven.

**Als ik een gegevensschijf premium van een bestaande VHD die 80 GB maken is, hoeveel dat kost?**

Een premium-gegevensschijf die is gemaakt op basis van een VHD 80 GB wordt beschouwd als de volgende beschikbare premium-schijfgrootte, dit een P10-schijf is. U kosten in rekening gebracht volgens de prijzen van P10 schijf.

**Gelden er transactiekosten voor Premium-SSD-schijven gebruiken?**

Er is een vaste kosten voor de grootte van elke schijf, dat wordt geleverd met bepaalde beperkingen worden ingericht op IOPS en doorvoer. De overige kosten zijn uitgaande bandbreedte en snapshot-capaciteit, indien van toepassing. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/storage) voor meer informatie.

**Wat zijn de limieten voor IOPS en doorvoer die ik van de schijfcache verwerken kan?**

De gecombineerde limieten voor cache en lokale SSD van een DS-serie zijn 4000 IOPS per kern en 33 MiB per seconde per core. De GS-serie biedt 5000 IOP's per kern en 50 MiB per seconde per core.

**Wordt de lokale SSD ondersteund voor een VM-schijven beheerd?**

De lokale SSD is tijdelijke opslag die is opgenomen in een VM-schijven beheerd. Er zijn geen extra kosten verbonden voor deze tijdelijke opslag. We raden u aan deze lokale SSD niet te gebruiken voor het opslaan van uw toepassingsgegevens omdat deze niet is doorgevoerd in Azure Blob-opslag.

**Zijn er gevolgen voor het gebruik van TRIM op premium-schijven?**

Er is geen nadeel aan het gebruik van TRIM op Azure-schijven in een premium of standard-schijven.

## <a name="new-disk-sizes-managed-and-unmanaged"></a>Nieuwe schijfgrootten: Beheerde en onbeheerde

**Wat is de grootste beheerde schijfgrootte voor het besturingssysteem en gegevensschijven worden ondersteund?**

Het partitietype die Azure biedt ondersteuning voor een besturingssysteemschijf wordt de master bootrecord (MBR). De MBR-indeling ondersteunt een schijfgrootte maximaal 2 TiB. De maximale grootte die Azure biedt ondersteuning voor een besturingssysteemschijf is 2 TiB. Azure ondersteunt maximaal 32 TiB voor beheerde gegevensschijven in de globale Azure, 4 TiB in Azure soevereine clouds.

**Wat is de maximumgrootte van de niet-beheerde schijf voor het besturingssysteem en gegevensschijven worden ondersteund?**

Het partitietype die Azure biedt ondersteuning voor een besturingssysteemschijf wordt de master bootrecord (MBR). De MBR-indeling ondersteunt een schijfgrootte maximaal 2 TiB. De maximale grootte die Azure biedt ondersteuning voor een niet-beheerde schijf van besturingssysteem is 2 TiB. Azure biedt ondersteuning voor maximaal 4 TiB voor niet-beheerde gegevensschijven.

**Wat is de maximale grootte uit pagina-blob die wordt ondersteund?**

Pagina-blob is de maximumgrootte die ondersteuning biedt voor Azure 8 TiB (8.191 GiB). De maximale blob paginagrootte wanneer gekoppeld aan een virtuele machine als de gegevens of besturingssysteemschijven is 4 TiB (4095 GiB).

**Moet ik een nieuwe versie van Azure-hulpprogramma's gebruiken om te maken, koppelen, vergroten of verkleinen en de schijven die groter zijn dan 1 TiB uploaden?**

U hoeft niet te upgraden van uw bestaande Azure-hulpprogramma's maken, koppelen of de schijven die groter zijn dan 1 TiB vergroten of verkleinen. Als u wilt uw VHD-bestand van on-premises rechtstreeks naar Azure uploaden als pagina-blobs of niet-beheerde schijf, moet u de nieuwste hulpprogramma's die worden vermeld onder gebruiken. We ondersteunen alleen VHD uploads van maximaal 8 TiB.

|Azure-hulpprogramma 's      | Ondersteunde versies                                |
|-----------------|---------------------------------------------------|
|Azure PowerShell | Versienummer 4.1.0: De release van juni 2017 of hoger|
|Azure CLI v1     | Versienummer 0.10.13: De release van mei 2017 of hoger|
|Azure CLI v2     | Versienummer 2.0.12: De release van juli 2017 of hoger|
|AzCopy           | Versienummer 6.1.0: De release van juni 2017 of hoger|

**Worden schijfgrootten P4 en P6 ondersteund voor niet-beheerde schijven of pagina-blobs?**

P4 (32 GiB) en P6 (64 GiB) schijfgrootten worden niet ondersteund als de standaard schijf lagen voor niet-beheerde schijven en pagina-blobs. Moet u expliciet [instellen van de Blob-laag](https://docs.microsoft.com/rest/api/storageservices/set-blob-tier) P4 en P6 de schijf die is toegewezen aan deze lagen hebben. Als u een niet-beheerde schijf of pagina-blob met de grootte van de schijf of lengte van de inhoud minder dan 32 GiB of tussen 32 GiB in 64 GiB zonder in te stellen van de Blob-laag implementeert, blijft u komt terecht op P10 met 500 IOPS en 100 MiB/s en de gekoppelde prijscategorie.

**Als mijn bestaande premium beheerde schijf kleiner is dan 64 GiB is gemaakt voordat de kleine schijf is ingeschakeld (rond 15 juni 2017), hoe wordt deze gefactureerd?**

Bestaande kleine premium-schijven kleiner is dan 64 GiB nog steeds gefactureerd volgens de prijscategorie van P10.

**Hoe kan ik van de schijf-laag van kleine premium-schijven korter zijn dan 64 GiB van P10 P4 of P6 wisselen?**

U kunt een momentopname van de kleine schijven en maak vervolgens een schijf voor het automatisch de prijscategorie overschakelen naar P4 of P6 op basis van de ingerichte grootte.

**Kunt u het formaat van bestaande beheerde schijven van grootte minder dan 4 tebibytes (TiB) naar nieuwe geïntroduceerde schijfgrootten tot 32 TiB?**

Ja.

**Wat zijn de grootste schijfgrootten die worden ondersteund door Azure Backup en Azure Site Recovery-service?**

De grootste schijfgrootte wordt ondersteund door Azure Backup en Azure Site Recovery-service is 4 TiB. Ondersteuning voor de grotere schijven maximaal 32 TiB wordt binnenkort toegevoegd.

**Wat zijn de aanbevolen VM-grootten voor grotere schijfgrootten (> 4 TiB) voor Standard-SSD en standaard harde schijven te bereiken geoptimaliseerd schijf-IOPS en bandbreedte?**

De schijfdoorvoer van Standard-SSD en HDD Standard grote schijfgrootten bereiken (> 4 TiB) meer dan 500 IOPS en 60 MiB/s, raden we u een nieuwe VM implementeren vanaf een van de volgende VM-grootten voor het optimaliseren van prestaties van uw: B-serie, DSv2-serie, Dsv3-serie, ESv3-serie, Fs-serie, Fsv2-serie, M-serie, GS-serie, NCv2-serie, uit de NCv3-serie of Ls-serie VM's. Grote schijven koppelen aan bestaande virtuele machines of virtuele machines die niet met behulp van de aanbevolen groottes die hierboven, kan de lagere prestaties optreden.

**Hoe kan ik mijn schijven upgraden (> 4 TiB) die zijn geïmplementeerd tijdens de grotere schijf grootten preview om op te halen van de hogere IOPS en bandbreedte op algemene beschikbaarheid?**

U kunt de stop en start de virtuele machine die de schijf is gekoppeld aan of loskoppelen en opnieuw koppelen van de schijf. De prestatiedoelen van grotere schijfgrootten zijn verhoogd voor zowel premium SSD's en standaard SSD's bij algemene beschikbaarheid.

**Welke regio's zijn de grootte van de beheerde schijven van 8 TiB, 16 TiB en 32 TiB ondersteund?**

De TiB 8, 16 TiB en 32 TiB schijf-SKU's worden ondersteund in alle regio's onder algemene Azure. Ondersteuning voor Microsoft Azure Government en Azure China 21Vianet is nog niet beschikbaar.

**Ondersteunen we Host opslaan in cache inschakelen op alle schijfgrootten?**

Dankzij Host opslaan in cache van de alleen-lezen en lezen/schrijven schijfgrootten minder dan 4 TiB. Voor schijfgrootten meer dan 4 TiB wordt niet ondersteund als optie dan geen caching. Het is raadzaam om gebruik te maken van in cache opslaan voor kleinere schijfgrootten waar u kunt verwachten boost voor betere prestaties zien met gegevens in de cache opgeslagen met de virtuele machine.

## <a name="what-if-my-question-isnt-answered-here"></a>Wat gebeurt er als mijn vraag hier niet wordt beantwoord?

Als uw vraag hier niet wordt weergegeven, laat het ons weten en wij helpen u een antwoord vinden. U kunt een vraag stellen aan het einde van dit artikel in de opmerkingen. Als u wilt samenwerken met het Azure Storage-team en andere communityleden over dit artikel, gebruikt u de MSDN [forum van Azure Storage](https://social.msdn.microsoft.com/forums/azure/home?forum=windowsazuredata).

Functies aanvragen, verzenden van aanvragen en ideeën naar de [forum met feedback van Azure Storage](https://feedback.azure.com/forums/217298-storage).
