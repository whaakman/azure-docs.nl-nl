---
title: Azure Linux VM's en Azure Storage | Microsoft Docs
description: Beschrijving van Azure Standard en Premium-opslag en zowel beheerde als onbeheerde schijven met Linux virtuele machines.
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: d364c69e-0bd1-4f80-9838-bbc0a95af48c
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 2/7/2017
ms.author: rasquill
ms.openlocfilehash: 5e8a62bb180de7288531139594cb61440cab04c5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/29/2017
---
# <a name="azure-and-linux-vm-storage"></a>Azure- en Linux-VM-opslag
Azure Storage is de oplossing voor opslag in de cloud voor moderne toepassingen die afhankelijk zijn van duurzaamheid, beschikbaarheid en schaalbaarheid om te voldoen aan de behoeften van klanten.  Naast de mogelijkheid voor ontwikkelaars om grootschalige toepassingen ter ondersteuning van nieuwe scenario's te bouwen, biedt Azure Storage ook de opslagbasis voor Azure Virtual Machines.

## <a name="managed-disks"></a>Beheerde schijven

Virtuele machines in Azure zijn nu beschikbaar via [Azure beheerd schijven](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), waardoor u uw virtuele machines maken zonder maken of beheren van een [Azure Storage-accounts](../../storage/common/storage-introduction.md) zelf. U opgeven of u wilt dat Premium- of Standard-opslag- en hoe groot de schijf moet en Azure de VM-schijven voor u maakt. Virtuele machines met beheerd schijven hebben veel belangrijke functies, waaronder:

- Ondersteuning voor automatische schaalbaarheid. Azure maakt van de schijven en de onderliggende opslag ter ondersteuning van maximaal 10.000 schijven per abonnement beheert.
- Hogere betrouwbaarheid met Beschikbaarheidssets. Azure zorgt ervoor dat VM-schijven van elkaar geïsoleerd in Beschikbaarheidssets automatisch worden.
- Verbeterde toegangsbeheer. Beheerde schijven beschikbaar stellen tal van bewerkingen die worden beheerd door [gebaseerd toegangsbeheer (RBAC)](../../active-directory/role-based-access-control-what-is.md).

Prijzen voor schijven beheerd is anders dan voor dat niet-beheerde schijven. Zie voor die informatie [prijzen en facturering voor schijven beheerd](../windows/managed-disks-overview.md#pricing-and-billing).

U kunt bestaande virtuele machines die gebruikmaken van niet-beheerde schijven te gebruiken beheerde schijven met converteren [az vm converteren](/cli/azure/vm#convert). Zie voor meer informatie [een Linux-VM van niet-beheerde schijven converteren naar beheerde Azure-schijven](convert-unmanaged-to-managed-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). U kunt een niet-beheerde schijf kan niet converteren naar een beheerde schijf als de niet-beheerde schijf in een opslagaccount die is, of op elk gewenst moment is, versleuteld met [Azure Storage Service versleuteling (SSE)](../../storage/common/storage-service-encryption.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). De volgende stappen wordt beschreven hoe niet-beheerde schijven die zijn of waren, in een versleutelde opslagaccount converteren:

- Kopiëren van de virtuele harde schijf (VHD) met [az storage-blob kopiëren start](/cli/azure/storage/blob/copy#start) naar een opslagaccount die nooit voor Azure Storage-Service: versleuteling is ingeschakeld.
- Maak een VM die gebruikmaakt van beheerde schijven en die VHD-bestand opgeven tijdens het maken van met [az vm maken](/cli/azure/vm#create), of
- Koppel de gekopieerde VHD met [az vm schijf koppelen](/cli/azure/vm/disk#attach) naar een actieve virtuele machine met beheerde-schijven.


## <a name="azure-storage-standard-and-premium"></a>Azure-opslag: Standaard- en Premium
Virtuele machines in Azure--of schijven beheerd of onbeheerd--kunnen gebaseerd zijn op de opslagschijven standard of premium-opslag-schijven. Wanneer u de portal gebruikt om te kiezen uw virtuele machine, moet u een vervolgkeuzelijst schakelen op de **basisbeginselen** standard en premium-schijven weergeven. Wanneer uitgeschakeld naar SSD, alleen premium-opslag zoals ingeschakelde VM's worden weergegeven, wordt alle back-up SSD stations.  Wanneer uitgeschakeld voor harde schijven, virtuele machines die door standard-opslag ingeschakeld op de back-up draaiende schijfstations worden weergegeven, samen met premium storage-VM's back-up SSD.

Bij het maken van een virtuele machine van de `azure-cli` u kunt kiezen tussen standard en premium bij het kiezen van de VM-grootte via de `-z` of `--vm-size` cli vlag.

## <a name="creating-a-vm-with-a-managed-disk"></a>Maken van een virtuele machine met een beheerde schijf

De Azure CLI 2.0, die u kunt is vereist voor het volgende voorbeeld [installeren hier](/cli/azure/install-azure-cli).

Maak eerst een resourcegroep voor het beheren van resources met [az groep maken](/cli/azure/group#create):

```azurecli
az group create --location westus --name myResourceGroup
```

Maak nu de virtuele machine met [az vm maken](/cli/azure/vm#create). Geef een unieke `--public-ip-address-dns-name` argument, als `mypublicdns` waarschijnlijk wordt genomen.

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns
```

Het vorige voorbeeld maakt een virtuele machine met een beheerde schijf in een Standard-opslagaccount. Voor het gebruik van een Premium storage-account toevoegen de `--storage-sku Premium_LRS` argument, zoals in het volgende voorbeeld:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myVM
    --image UbuntuLTS \
    --admin-username azureuser \
    --generate-ssh-keys \
    --public-ip-address-dns-name mypublicdns \
    --storage-sku Premium_LRS
```

## <a name="standard-storage"></a>Standard Storage
Azure Standard-opslag is het standaardtype van opslag.  Standard-opslag is terwijl u nog steeds zodat rendabel.  

## <a name="premium-storage"></a>Premium Storage
Azure Premium-opslag biedt ondersteuning voor hoge prestaties, lage latentie schijven voor virtuele machines met I/O-intensieve werkbelastingen. Virtuele machine (VM)-schijven die gebruikmaken van Premium-opslag opslaan gegevens op Solid-State stations (SSD's). U kunt uw toepassing VM schijven migreren naar Azure Premium-opslag om te profiteren van de snelheid en prestaties van deze schijven.

Premium-opslag-functies:

* Premium-opslag-schijven: Azure Premium Storage ondersteunt VM-schijven die kunnen worden gekoppeld aan Active Directory, DSv2 of GS-serie virtuele Azure-machines.
* Premium-pagina-Blob: Azure pagina-Blobs, die worden gebruikt voor het opslaan van permanente schijven voor Azure Virtual Machines (VM's) biedt ondersteuning voor Premium-opslag.
* Premium lokaal redundante opslag: Een Premium Storage-account alleen lokaal redundante opslag (LRS) als de replicatieoptie ondersteunt en houdt drie kopieën van de gegevens in één regio.
* [Premium-opslag](../../storage/common/storage-premium-storage.md)

## <a name="premium-storage-supported-vms"></a>Premium-opslag ondersteund virtuele machines
Premium-opslag ondersteunt DS-serie, DSv2-serie GS-serie en Fs-serie Azure Virtual Machines (VM's). Standaard- en Premium-opslag-schijven kunt u met Premium-opslag van virtuele machines worden ondersteund. Maar u Premium-opslag schijven niet gebruiken met VM-reeks niet compatibel Premium-opslag zijn.

Hier volgen de Linux-distributies die we gevalideerd met Premium-opslag.

| Distributie | Versie | Ondersteunde Kernel |
| --- | --- | --- |
| Ubuntu |12.04 |3.2.0-75.110+ |
| Ubuntu |14.04 |3.13.0-44.73+ |
| Debian |7.x, 8.x |3.16.7-ckt4-1+ |
| SLES |SLES 12 |3.12.36-38.1+ |
| SLES |SLES 11 SP4 |3.0.101-0.63.1+ |
| CoreOS |584.0.0+ |3.18.4+ |
| Centos |6.5, 6.6, 6.7, 7.0, 7.1 |3.10.0-229.1.2.el7+ |
| RHEL |6.8+, 7.2+ | |

## <a name="azure-file-storage"></a>Azure File storage
Azure File storage biedt bestandsshares in de cloud met behulp van het standaard SMB-protocol. Met Azure-bestanden, kunt u zakelijke toepassingen die afhankelijk van bestandsservers naar Azure zijn migreren. Toepassingen die worden uitgevoerd in Azure kunnen eenvoudig bestandsshares koppelen vanuit Azure virtuele machines met Linux. En met de meest recente versie van File storage kunt u een bestandsshare ook koppelen vanuit een on-premises toepassing die ondersteuning biedt voor SMB 3.0.  Omdat bestandsshares SMB-shares zijn, kunt u ze openen via standaard bestandssysteem API's.

Opslag van bestanden is gebaseerd op dezelfde technologie als Blob, Table en Queue storage, dus File storage biedt de beschikbaarheid, duurzaamheid, schaalbaarheid en geografische redundantie die is ingebouwd in de Azure-opslag-platform. Zie voor meer informatie over prestatiedoelen van File storage en limieten voor Azure Storage Scalability and Performance Targets.

* [Azure File Storage gebruiken met Linux](../../storage/files/storage-how-to-use-files-linux.md)

## <a name="hot-storage"></a>Hot Storage
De Azure hot storage-laag is geoptimaliseerd voor het opslaan van gegevens die regelmatig worden geopend.  Hot storage is het standaardtype voor opslag voor blob-stores.

## <a name="cool-storage"></a>Cool Storage
De Azure cool storage-laag is geoptimaliseerd voor het opslaan van gegevens die niet regelmatig worden geopend en een lange levensduur hebben. Gebruiksvoorbeelden voor cool storage zijn back-ups van media-inhoud, wetenschappelijke gegevens, naleving en archivering van gegevens. Alle gegevens die worden zelden geopend is in het algemeen een perfecte keuze voor de cool storage.

|  | Opslaglaag voor 'hot' blobs | Opslaglaag voor 'cool' blobs |
|:--- |:---:|:---:|
| Beschikbaarheid |99,9% |99% |
| Beschikbaarheid (RA-GRS-leesbewerkingen) |99,99% |99,9% |
| Gebruikskosten |Hogere opslagkosten |Lagere opslagkosten |
| Lagere toegang |Hogere toegang | |
| en transactiekosten |en transactiekosten | |

## <a name="redundancy"></a>Redundantie
De gegevens in uw Microsoft Azure storage-account worden altijd gerepliceerd voor duurzaamheid en hoge beschikbaarheid, voldoen aan de SERVICEOVEREENKOMST van Azure Storage zelfs met betrekking tot tijdelijke hardwarefouten.

Wanneer u een opslagaccount maakt, moet u een van de volgende replicatieopties selecteren:

* Lokaal redundante opslag (LRS)
* Zone-redundante opslag (ZRS)
* Geografisch redundante opslag (GRS)
* Geografisch redundante opslag met leestoegang (RA-GRS)

### <a name="locally-redundant-storage"></a>Lokaal redundante opslag
Lokaal redundante opslag (LRS) repliceert uw gegevens binnen de regio waarin u uw opslagaccount hebt gemaakt. Als u wilt maximaliseren duurzaamheid, wordt elke aanvraag ten opzichte van de gegevens in uw opslagaccount driemaal gerepliceerd. Deze drie replica's elke zich bevinden in domeinen met afzonderlijke fouten en upgradedomeinen.  Een aanvraag retourneert is slechts eenmaal in het geschreven op alle drie replica's.

### <a name="zone-redundant-storage"></a>Zone-redundante opslag
Zone-redundante opslag (ZRS) uw gegevens gerepliceerd tussen twee of drie faciliteiten in één regio of tussen twee regio's, biedt een hogere duurzaamheid dan LRS. Als uw storage-account ZRS is ingeschakeld heeft, zijn uw gegevens is zelfs het geval van storing optreedt in een van de faciliteiten duurzame.

### <a name="geo-redundant-storage"></a>Geografisch redundante opslag
Geografisch redundante opslag (GRS) worden uw gegevens gerepliceerd naar een secundaire regio die honderden mijl weg van de primaire regio. Als uw storage-account GRS ingeschakeld heeft, zijn uw gegevens is duurzame zelfs het geval van een volledige regionale uitval of een ramp optreedt waarbij de primaire regio kan niet worden hersteld.

### <a name="read-access-geo-redundant-storage"></a>Geografisch redundante opslag met leestoegang
Geografisch redundante opslag met leestoegang (RA-GRS) maximaliseert de beschikbaarheid voor uw opslagaccount door middel van alleen-lezen toegang tot de gegevens in de secundaire locatie, naast de replicatie tussen twee regio's die worden geleverd door GRS. In het geval dat gegevens niet beschikbaar is in de primaire regio, kan uw toepassing gegevens lezen van de secundaire regio.

Voor een diepgaand in Azure storage redundantie Zie:

* [Azure Storage-replicatie](../../storage/common/storage-redundancy.md)

## <a name="scalability"></a>Schaalbaarheid
Azure Storage is zeer schaalbaar. U kunt honderden terabytes aan gegevens opslaan en verwerken ter ondersteuning van de big data-scenario's die u nodig hebt voor toepassingen die u gebruikt voor wetenschap, financiële analyse en media. Ook de kleine hoeveelheden gegevens die u nodig hebt voor een kleine zakelijke website kunt u opslaan. Wat u ook nodig hebt, u betaalt alleen voor de gegevens die u opslaat. Momenteel zijn er tientallen biljoenen unieke klantobjecten opgeslagen in Azure Storage. Daarnaast worden er gemiddeld miljoenen aanvragen per seconde verwerkt.

Voor standard-opslag-accounts: een standard-opslagaccount heeft een maximale totale percentage 20.000 IOPS. Het totale aantal IOP's van alle schijven van een virtuele machine in een standaardopslagaccount mag niet groter zijn dan deze limiet.

Voor premium storage-accounts: een premium storage-account heeft de maximale totale doorvoersnelheid van 50 Gbps. De totale doorvoer op alle schijven in uw virtuele machine mag deze limiet niet overschrijden.

## <a name="availability"></a>Beschikbaarheid
We garanderen dat we 99,99% (99,9% voor de koude toegangslaag) van de tijd erin zullen slagen om aan procesverzoeken voor het lezen van gegevens van Read Access-Geo Redundant Storage-accounts (RA-GRS) te voldoen, mits mislukte pogingen tot het lezen van gegevens van de primaire regio opnieuw kunnen worden uitgevoerd op de secundaire regio.

We garanderen dat we ten minste 99,9% (99% voor de koude toegangslaag) van de tijd erin zullen slagen om procesverzoeken te verwerken voor het lezen van gegevens van accounts voor lokaal redundante opslag (LRS, locally redundant storage), voor zoneredundante opslag (ZRS, zone redundant storage) en voor geografisch reduntante opslag (GRS, geo redundant storage).

We garanderen dat we ten minste 99,9% (99% voor de koude toegangslaag) van de tijd erin zullen slagen om procesverzoeken te verwerken voor het schrijven van gegevens naar accounts voor lokaal redundante opslag (LRS, locally redundant storage), voor zoneredundante opslag (ZRS, zone redundant storage), voor geografisch reduntante opslag (GRS, geo redundant storage) en accounts voor Read Access-Geo Redundant Storage (RA-GRS).

* [Azure SLA voor opslag](https://azure.microsoft.com/support/legal/sla/storage/v1_1/)

## <a name="regions"></a>Regio's
Azure is algemeen beschikbaar in 30 regio's overal ter wereld en plannen voor 4 extra gebieden is aangekondigd. Geografische uitbreiding is een prioriteit voor Azure, omdat klanten dan hogere prestatieniveaus kunnen bereiken en we zo aan hun vereisten en voorkeuren voor de locatie van gegevens kunnen voldoen.  De meest recente regio azures starten is in Duitsland.

De Microsoft Cloud-Duitsland biedt een gedifferentieerde optie met de Microsoft Cloud-services al beschikbaar voor Europa, verbeterde mogelijkheden voor innovatie en groei voor maximaal gereglementeerde partners en klanten maken in Duitsland, de Europese Unie (EU) en de Europese vrije handel Association (EFTA).

Klantgegevens in deze nieuwe datacenters, in Magdeburg en Frankfurt, wordt onder het beheer van een beheerder gegevens International T-systemen, een onafhankelijke Duitse bedrijfsgegevens en vestiging van Deutsche Telekom beheerd. Commerciële van Microsoft-cloudservices in deze datacenters voldoen aan de Duitse voorschriften de afhandeling van gegevens en klanten bieden aanvullende opties van hoe en waar gegevens worden verwerkt.

* [Azure-regio's toewijzen](https://azure.microsoft.com/regions/)

## <a name="security"></a>Beveiliging
Azure Storage biedt een uitgebreide reeks beveiligingsmogelijkheden die samen kunnen ontwikkelaars om beveiligde toepassingen te bouwen. Het opslagaccount zelf kan worden beveiligd met op rollen gebaseerde toegangsbeheer en Azure Active Directory. Gegevens kunnen worden beveiligd tijdens de overdracht tussen een toepassing en Azure met behulp van versleuteling van de Client-Side-, HTTPS- of SMB 3.0. Gegevens kunnen worden ingesteld op automatisch worden versleuteld wanneer geschreven naar Azure Storage met behulp van versleuteling voor opslag-Service (SSE). Besturingssysteem en gegevensschijven die worden gebruikt door virtuele machines kunnen worden ingesteld om te worden versleuteld met Azure Disk Encryption. Gedelegeerde toegang tot de gegevensbron-objecten in Azure Storage kan worden verleend met behulp van handtekeningen voor gedeelde toegang.

### <a name="management-plane-security"></a>Beveiliging van de vlak Management
Het management vlak bestaat uit de bronnen die worden gebruikt voor het beheren van uw opslagaccount. In deze sectie bespreken we het implementatiemodel van Azure Resource Manager en het gebruik van op rollen gebaseerde toegangsbeheer (RBAC) toegang tot uw storage-accounts. Er wordt ook contact opnemen over het beheren van uw toegangscodes voor opslag en hoe u ze opnieuw genereren.

### <a name="data-plane-security"></a>Beveiliging van gegevens vlak
In deze sectie zullen we het toestaan van toegang tot de werkelijke gegevensbron objecten in uw opslagaccount, zoals blobs, bestanden, wachtrijen en tabellen, met behulp van handtekeningen voor gedeelde toegang en toegangsbeleid opgeslagen. Zowel serviceniveau-SAS en account niveau SAS wordt uitgelegd. We ziet ook hoe u wilt beperken van toegang tot een specifiek IP-adres (of een bereik van IP-adressen), het beperken van het protocol dat wordt gebruikt voor HTTPS en hoe voor het intrekken van een Shared Access Signature zonder te wachten deze verlopen.

## <a name="encryption-in-transit"></a>Codering in Transit
Deze sectie wordt beschreven hoe gegevens worden beveiligd wanneer u het overbrengen van of naar Azure Storage. Bespreken we het aanbevolen gebruik van HTTPS en de versleuteling van SMB 3.0 voor Azure-bestandsshares. Er wordt ook rekening houden met kijken clientzijde versleuteling, waarmee u de gegevens te versleutelen voordat deze worden overgedragen naar de opslag in een clienttoepassing en de gegevens te decoderen nadat deze zijn overgebracht buiten de opslag.

## <a name="encryption-at-rest"></a>Codering in rust
We zullen hebben over de Storage-Service-versleuteling (SSE) en hoe u kunt inschakelen voor een opslagaccount, wat resulteert in uw blok-blobs, pagina-blobs en toevoeg-blobs worden automatisch versleuteld wanneer geschreven naar Azure Storage. Ook kijken we hoe u Azure Disk Encryption gebruiken en de belangrijkste verschillen en aanvragen van schijfversleuteling versus SSE ten opzichte van de Client-Side versleuteling verkennen. Kort kijken we FIPS-naleving voor VS Computers van de overheid.

* [Azure Storage-beveiligingshandleiding](../../storage/common/storage-security-guide.md)

## <a name="temporary-disk"></a>Tijdelijke schijf
Elke virtuele machine bevat een tijdelijke schijf. De tijdelijke schijf opslag op korte termijn biedt voor toepassingen en processen en voor het opslaan van gegevens, zoals pagina of het swap-bestanden alleen is bedoeld. Gegevens op de tijdelijke schijf zijn mogelijk verloren gegaan tijdens een [onderhoud](manage-availability.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json#understand-vm-reboots---maintenance-vs-downtime) of wanneer u [opnieuw implementeren van een virtuele machine](redeploy-to-new-node.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Tijdens een standaard opnieuw opstarten van de virtuele machine, moet de gegevens op de tijdelijke schijf handhaven.

Op Linux virtuele machines, de schijf is doorgaans **/dev/sdb** en geformatteerd en is gekoppeld aan **mnt** door de Azure Linux Agent. De grootte van de tijdelijke schijf varieert, afhankelijk van de grootte van de virtuele machine. Zie voor meer informatie [grootten voor virtuele Linux-machines](sizes.md).

Zie voor meer informatie over hoe Azure gebruikt voor de tijdelijke schijf [inzicht in de tijdelijke schijf op Microsoft Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

## <a name="cost-savings"></a>Kostenbesparingen
* [Opslagkosten](https://azure.microsoft.com/pricing/details/storage/)
* [Opslag kosten Rekenmachine](https://azure.microsoft.com/pricing/calculator/?service=storage)

## <a name="storage-limits"></a>Opslaglimieten
* [Service opslaglimieten](../../azure-subscription-service-limits.md#storage-limits)
