---
title: Mainframe-opslag verplaatsen naar Azure Storage
description: Zeer schaalbare Azure storage-resources kunt mainframe op basis van organisaties migreren en IBM z14 toepassingen moderniseren.
author: njray
ms.author: larryme
ms.date: 04/02/2019
ms.topic: article
ms.service: storage
ms.openlocfilehash: 4fea787e7fe20d60de91761811e1b69bebf3a010
ms.sourcegitcommit: 0a3efe5dcf56498010f4733a1600c8fe51eb7701
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58896281"
---
# <a name="move-mainframe-storage-to-azure"></a>Mainframe-opslag verplaatsen naar Azure

Mainframe-workloads uitvoeren op Microsoft Azure, moet u weten hoe de mogelijkheden van uw mainframe zich verhouden tot Azure. De hoge mate schaalbaar storage-resources kunt organisaties begint te moderniseren zonder de toepassingen die ze erop dat vertrouwen wordt afgebroken.

Azure biedt mainframe-achtige functies en voor de opslagcapaciteit die is vergelijkbaar met IBM z14-systemen (de meest recente model op dit moment). In dit artikel wordt uitgelegd hoe u de resultaten van de vergelijkbare ophalen in Azure.

## <a name="mainframe-storage-at-a-glance"></a>Mainframe-opslag in een oogopslag

De IBM-mainframes kenmerkt opslag op twee manieren. De eerste is een directe toegang-opslagapparaat (DASD). De tweede is opeenvolgende opslag. Voor het beheren van opslag, biedt de mainframe gegevens faciliteit Storage Management subsysteem (DFSMS). Hiermee worden beheerd met toegang tot gegevens in de verschillende opslagapparaten.

[DASD](https://en.wikipedia.org/wiki/Direct-access_storage_device) verwijst naar een afzonderlijk apparaat voor secundaire (niet-opslag in het geheugen) waarmee een uniek adres moet worden gebruikt voor rechtstreekse toegang van gegevens. Oorspronkelijk, de term DASD toegepast op draaiende schijven, magnetische trommels of gegevenscellen met. Nu de term kan ook van toepassing op opslagapparaten van de SSD-(SSD's), storage area network (SAN's) en NAS-opslag (NAS) en optische stations. Voor de doeleinden van dit document wordt DASD verwijst naar draaiende schijven, SAN's en SSD's.

In tegenstelling tot DASD opslag verwijst sequentiële opslag op een mainframe naar apparaten, zoals tapestations waar de gegevens is toegankelijk vanuit een beginpunt en vervolgens gelezen of geschreven in een regel.

Opslagapparaten zijn meestal gekoppeld met behulp van een fiber-verbinding (FICON) of rechtstreeks op met behulp van de mainframe i/o-bus zijn toegankelijk [HiperSockets](https://www.ibm.com/support/knowledgecenter/zosbasics/com.ibm.zos.znetwork/znetwork_85.htm), een technologie voor IBM voor snelle communicatie tussen partities op een server met een hypervisor.

De meeste mainframesystemen afzonderlijke opslag in twee typen:

- *Onlineopslag* (ook bekend als hot storage) is nodig voor dagelijkse werkzaamheden. DASD opslag wordt doorgaans gebruikt voor dit doel. Sequentiële opslag, zoals dagelijkse tapeback-ups (logische of fysieke), kan echter ook worden gebruikt voor dit doel.

- *Archiefopslag* (ook wel bekend als koude opslag) is niet noodzakelijkerwijs op een bepaald moment worden gekoppeld. In plaats daarvan is gekoppeld en geopend indien nodig. Archiefopslag wordt vaak geïmplementeerd met behulp van opeenvolgende tapeback-ups (logische of fysieke) voor opslag.

## <a name="mainframe-versus-io-latency-and-iops"></a>Mainframe ten opzichte van i/o-latentie en IOPS

Mainframes worden vaak gebruikt voor toepassingen die hoge i/o en lage latentie voor i/o-vereisen. Ze kunnen dit doen met behulp van de verbindingen FICON voor i/o-apparaten en HiperSockets. Wanneer HiperSockets worden gebruikt om toepassingen en apparaten rechtstreeks naar een mainframe van i/o-kanaal, kan latentie in de microseconden worden bereikt.

## <a name="azure-storage-at-a-glance"></a>Azure-opslag in een oogopslag

Azure infrastructure-as-a-service ([IaaS](https://azure.microsoft.com/overview/what-is-iaas/)) opties voor opslag bieden vergelijkbare mainframe-capaciteit.

Microsoft biedt petabytes aan opslag voor toepassingen die worden gehost in Azure, en u hebt verschillende opties voor opslag. Deze tussen SSD-opslag voor hoge prestaties en lage kosten blob-opslag voor massaopslag en archieven. Bovendien biedt Azure een optie voor gegevensredundantie voor opslag, iets dat is meer nodig om in te stellen in een mainframe-omgeving.

Azure storage is beschikbaar als [Azure Disks](/azure/virtual-machines/windows/managed-disks-overview), [Azure Files](/azure/storage/files/storage-files-introduction), en [Azure Blobs](/azure/storage/blobs/storage-blobs-overview) zoals in de volgende tabel geeft een overzicht van. Meer informatie over [wanneer het gebruik van elk](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks).

<!-- markdownlint-disable MD033 -->

<table>
<thead>
    <tr><th>Type</th><th>Beschrijving</th><th>Gebruik deze wanneer u het volgende wilt doen:</th></tr>
</thead>
<tbody>
<tr><td>Azure Files
</td>
<td>
Voorziet in een SMB-interface, clientbibliotheken, en een <a href="https://docs.microsoft.com/rest/api/storageservices/file-service-rest-api">REST</a> interface waarmee toegang vanaf elke locatie opgeslagen bestanden.
</td>
<td><ul>
<li>Lift- and -shift van een toepassing in de cloud wanneer de toepassing de systeemeigen bestandssysteem-API's gebruikt voor het delen van gegevens tussen deze en andere toepassingen die worden uitgevoerd in Azure.</li>
<li>Store-ontwikkeling en foutopsporing van hulpprogramma's die moeten worden geopend via een groot aantal virtuele machines.</li>
</ul>
</td>
</tr>
<tr><td>Azure Blobs
</td>
<td>Beschikt over clientbibliotheken en een <a href="https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api">REST</a> interface waarmee ongestructureerde gegevens worden opgeslagen en geopend op een zeer grote schaal in blok-blobs. Biedt ook ondersteuning voor <a href="/azure/storage/blobs/data-lake-storage-introduction">Azure Data Lake Storage Gen2</a> voor enterprise big data analytics-oplossingen.
</td>
<td><ul>
<li>Ondersteuning voor streaming en RAM scenario's in een toepassing.</li>
<li>Hebben toegang tot toepassingsgegevens vanaf elke locatie.</li>
<li>Een enterprise data lake bouwen op Azure en big data-analyses uitvoeren.</li>
</ul></td>
</tr>
<tr><td>Azure-schijven
</td>
<td>Beschikt over clientbibliotheken en een <a href="https://docs.microsoft.com/rest/api/compute/disks">REST</a> interface waarmee gegevens worden permanent opgeslagen en geopend via een gekoppelde virtuele harde schijf.
</td>
<td><ul>
<li>Verbeter en verplaats toepassingen die gebruikmaken van systeemeigen bestandssysteem-API's om te lezen en schrijven van gegevens naar permanente schijven.</li>
<li>Gegevens die is niet vereist voor toegang vanuit buiten de virtuele machine waarop de schijf is aangesloten Store.</li>
</ul></td>
</tr>
</tbody>
</table>
<!-- markdownlint-enable MD033 -->

## <a name="azure-hot-online-and-cold-archive-storage"></a>Azure hot (online) en de opslag koud (archief)

Het type opslag voor een bepaald systeem, is afhankelijk van de vereisten van het systeem, inclusief opslaggrootte, doorvoer en IOPS. Voor DASD-type opslag op een mainframe-toepassingen op Azure doorgaans Azure Disks station opslag gebruiken in plaats daarvan. Blob storage wordt gebruikt voor opslag van mainframe-archief, op Azure.

SSD's bieden de beste opslagprestaties in Azure. De volgende opties zijn beschikbaar (vanaf het schrijven van dit document):

| Type         | Grootte           | IOPS                  |
|--------------|----------------|-----------------------|
| Ultra SSD    | 4 GB tot 64 TB  | 1200-160.000 IOPS |
| Premium SSD  | 32 GB tot 32 TB | 12-15.000 IOPS     |
| Standard - SSD | 32 GB tot 32 TB | 12 tot 2000 IOPS      |

BLOB-opslag biedt de grootste hoeveelheid opslag op Azure. Naast de opslaggrootte biedt Azure zowel beheerde als niet-beheerde opslag. Met beheerde opslag verzorgd door Azure van het beheer van de onderliggende opslagaccounts. Met niet-beheerde opslag neemt de gebruiker de verantwoordelijkheid voor het instellen van Azure storage-accounts van de juiste grootte heeft om te voldoen aan de vereisten voor opslag.

## <a name="next-steps"></a>Volgende stappen

- [Mainframe-migratie](/azure/architecture/cloud-adoption/infrastructure/mainframe-migration/overview)
- [Mainframe opnieuw hosten op Azure Virtual Machines](/azure/virtual-machines/workloads/mainframe-rehosting/overview)
- [Mainframe-compute verplaatsen naar Azure](mainframe-compute-Azure.md)
- [Bepalen wanneer u Azure-Blobs, Azure Files of Azure-schijven gebruiken](https://docs.microsoft.com/azure/storage/common/storage-decide-blobs-files-disks)
- [Standard-SSD Managed Disks voor Azure VM-workloads](https://docs.microsoft.com/azure/virtual-machines/windows/disks-standard-ssd)

### <a name="ibm-resources"></a>IBM-resources

- [Parallelle Sysplex op IBM-Z](https://www.ibm.com/it-infrastructure/z/technologies/parallel-sysplex-resources)
- [IBM CICS en de faciliteit koppeling: Een stapje verder](http://www.redbooks.ibm.com/redbooks/pdfs/sg248420.pdf)
- [Het maken van de vereiste gebruikers voor een Db2-pureScale-functie installeren](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.qb.server.doc/doc/t0055374.html?pos=2)
- [Db2icrt - exemplaar-opdracht maken](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.cmd.doc/doc/r0002057.html)
- [Db2 pureScale geclusterde Database-oplossing](http://www.ibmbigdatahub.com/blog/db2-purescale-clustered-database-solution-part-1)
- [IBM Data Studio](https://www.ibm.com/developerworks/downloads/im/data/index.html/)

### <a name="azure-government"></a>Azure Government

- [Microsoft Azure Government-cloud voor mainframe-toepassingen](https://azure.microsoft.com/resources/microsoft-azure-government-cloud-for-mainframe-applications/)
- [Microsoft en FedRAMP](https://www.microsoft.com/TrustCenter/Compliance/FedRAMP)

### <a name="more-migration-resources"></a>Meer resources voor migratie

- [Platform-modernisering Alliance: IBM Db2 on Azure](https://www.platformmodernization.org/pages/ibmdb2azure.aspx)
- [Azure Virtual Datacenter Lift- and -Shift-handleiding](https://azure.microsoft.com/resources/azure-virtual-datacenter-lift-and-shift-guide/)
- [GlusterFS iSCSI](https://docs.gluster.org/en/latest/Administrator%20Guide/GlusterFS%20iSCSI/)
