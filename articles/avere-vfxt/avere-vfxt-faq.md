---
title: Veelgestelde vragen - Avere vFXT voor Azure
description: Veelgestelde vragen over Avere vFXT voor Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 83229cdca0ccff68dcdd543495a7a19b6e6c4c5b
ms.sourcegitcommit: f7f4b83996640d6fa35aea889dbf9073ba4422f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "56990048"
---
# <a name="avere-vfxt-for-azure-faq"></a>Veelgestelde vragen over Avere vFXT for Azure

In dit artikel vindt u antwoorden op vragen die u bepalen kunnen of Avere vFXT voor Azure bij uw behoeften past. Het biedt algemene informatie over Avere vFXT en wordt uitgelegd hoe het werkt met andere Azure-onderdelen en producten van externe leveranciers. 

## <a name="general"></a>Algemeen 

### <a name="what-is-avere-vfxt-for-azure"></a>Wat is Avere vFXT for Azure?

Avere vFXT voor Azure is een krachtige bestandssysteem dat actieve gegevens in Azure-rekenen voor efficiënte verwerking van essentiële workloads in de cache opslaat.

### <a name="is-avere-vfxt-a-storage-solution"></a>Avere vFXT een opslagoplossing is?

Nee. Avere vFXT is een bestandssysteem *cache* die is gekoppeld aan de opslagomgevingen, zoals uw EMC of NetApp NAS of een Azure blob-container. Avere vFXT stroomlijnt gegevensaanvragen van clients en het slaat de gegevens die het resultaat wordt gebruikt om prestaties op schaal en na verloop van tijd te verbeteren. Avere vFXT zelf slaat geen gegevens. Dit heeft geen informatie over de hoeveelheid gegevens die erachter zijn opgeslagen.

### <a name="is-avere-vfxt-a-tiering-solution"></a>Is Avere vFXT een trapsgewijs oplossing?

Avere vFXT wordt niet automatisch gegevens tussen warme en koude lagen van de laag.  

### <a name="how-do-i-know-if-an-environment-is-right-for-avere-vfxt"></a>Hoe weet ik of een omgeving geschikt voor Avere vFXT is?

De beste manier om na te denken over deze vraag is om te vragen, 'Is de werkbelasting gecachet kan worden?' Dat wil zeggen, is de werkbelasting er een maximale ratio van lezen / schrijven? Een voorbeeld is 80/20 of 70/30 lees-/ schrijfbewerkingen.

Houd rekening met Avere vFXT voor Azure als u een bestand op basis van analytische pijplijn hebt die wordt uitgevoerd op een groot aantal virtuele machines van Azure en deze voldoet aan een of meer van de volgende voorwaarden:

* Algemene is traag of inconsistent vanwege een lange toegangstijden (tientallen milliseconden of seconden, afhankelijk van vereisten). Deze latentie is niet toegestaan aan de klant.

* Gegevens die nodig zijn voor de verwerking van bevindt zich aan het einde van een WAN-omgeving, en niet praktisch is om die gegevens definitief te verplaatsen. De gegevens zijn mogelijk in een andere Azure-regio of in een datacenter van de klant.

* Een groot aantal clients vragen de gegevens - bijvoorbeeld in een high performance computing (HPC)-cluster. Het grote aantal gelijktijdige aanvragen kunt hogere latentie.

* De klant wil uitvoeren van hun huidige pijplijn 'as is' in Azure virtual machines en gedeelde opslag (of opslaan in cache) moet een op POSIX gebaseerde oplossing voor schaalbaarheid. Door Avere vFXT voor Azure, moet u niet opnieuw ontwerpen van de pijplijn werk systeemeigen aanroepen naar Azure Blob-opslag.

* Uw HPC-toepassing is gebaseerd op NFSv3 clients. (In sommige gevallen kan het SMB 2.1-clients gebruiken, maar prestaties is beperkt.)

Het volgende diagram vereenvoudigt het antwoord op deze vraag. Hoe dichter de werkstroom is in de rechterbovenhoek, hoe groter de kans is dat de Avere opslaan in cache oplossing geschikt voor uw omgeving is.

![diagram waarin wordt getoond dat leesintensief loads met duizenden clients beter zijn geschikt voor Avere vFXT](media/avere-vfxt-fit-assessment.png)

### <a name="at-what-scale-of-clients-does-the-avere-vfxt-solution-make-the-most-sense"></a>Op welke schaal van clients wordt de Avere vFXT oplossing zinvol zijn het meest?

De Avere vFXT cache-oplossing is gebouwd voor het afhandelen van honderden, duizenden of tienduizenden compute-kerngeheugens. Als u een aantal computers waarop een licht werk hebt, is Avere vFXT niet de juiste oplossing.

Typische Avere vFXT klanten uitvoeren veeleisende workloads vanaf ongeveer 1000 CPU-kernen. Deze omgevingen kunnen zich even groot zijn als 50.000 kernen of meer. Omdat Avere vFXT schaalbaar is, kunt u knooppunten ter ondersteuning van deze werkbelastingen als ze toenemen om te vereisen meer doorvoer of IOPS meer toevoegen.

### <a name="how-much-data-can-an-avere-vfxt-environment-store"></a>Hoeveel gegevens kan opslaan in een Avere vFXT-omgeving?

Avere vFXT is een cache. Gegevens worden niet specifiek opgeslagen. Hierbij wordt een combinatie van RAM-geheugen en SSD's voor het opslaan van gegevens in de cache. De gegevens worden blijvend opgeslagen op een systeem voor back-end-opslag (bijvoorbeeld een NetApp NAS-systeem of een blob-container). Het Avere vFXT systeem heeft geen informatie over de hoeveelheid gegevens die erachter zijn opgeslagen. Avere vFXT slaat alleen de subset met gegevens die door clients wordt aangevraagd.  

### <a name="what-regions-are-supported"></a>Welke regio's worden ondersteund?

Avere vFXT voor Azure wordt ondersteund in alle regio's, met uitzondering van soevereine regio's (China, Duitsland). Zorg ervoor dat het grote aantal rekenkernen en de VM-exemplaren die nodig zijn voor het maken van het Avere vFXT cluster kunt ondersteuning voor de regio die u wilt gebruiken.

### <a name="how-do-i-get-help-with-avere-vfxt"></a>Hoe krijg ik hulp met Avere vFXT?

Een gespecialiseerde ondersteuningsgroep biedt hulp bij Avere vFXT voor Azure. Volg de instructies in [hulp met uw systeem](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) een ondersteuningsticket openen vanuit de Azure-portal. 

### <a name="is-avere-vfxt-highly-available"></a>Avere vFXT maximaal beschikbaar is?

Ja, Avere vFXT wordt uitgevoerd alleen als een HA-oplossing.

### <a name="does-avere-vfxt-for-azure-also-support-other-cloud-services"></a>Ondersteunt Avere vFXT voor Azure ook andere cloudservices?

Ja, klanten meer dan één cloudprovider kunnen gebruiken met de Avere vFXT-cluster. Ondersteunt standaard buckets AWS S3, standaard buckets van Google Cloud Services en Azure blob-containers. 

> [!NOTE] 
> Een software-tarief geldt voor het gebruik van Avere vFXT in AWS en Google Cloud, maar niet met Azure.

## <a name="technical-compute"></a>Technische: Compute

### <a name="can-you-describe-what-an-avere-vfxt-environment-looks-like"></a>Kunt u beschrijven welke een Avere vFXT-omgeving 'ziet eruit als'?

Avere vFXT is een geclusterde apparaat bestaan uit meerdere virtuele machines van Azure. Een Python-bibliotheek wordt gebruikt voor maken van een cluster, verwijderen en wijzigen. Lezen [wat is er Avere vFXT voor Azure?](avere-vfxt-overview.md) voor meer informatie. 

### <a name="what-kind-of-azure-virtual-machines-does-avere-vfxt-run-on"></a>Welke virtuele machines van Azure Avere vFXT uit te voeren?  

Een vFXT Avere voor Azure-cluster maakt gebruik van Microsoft Azure E32s_v3 virtuele machines. 

<!-- ### Can I mix and match virtual machine types for my cluster?

No, you must choose one virtual machine type or the other.
    
### Can I move between virtual machine types?

Yes, there is a migration path to move from one VM type to the other. [Open a support ticket](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) to learn how.

-->

### <a name="does-the-avere-vfxt-environment-scale"></a>De Avere vFXT omgeving schalen?

Het Avere vFXT cluster kan worden drie knooppunten van de virtuele machine zo klein of groot maken als 24-knooppunten. Neem contact op met technische ondersteuning van Azure voor hulp bij planning als u denkt dat u een cluster met meer dan negen knooppunten nodig hebt. Het grotere aantal knooppunten vereist een grotere implementatiearchitectuur.

### <a name="does-the-avere-vfxt-environment-autoscale"></a>Biedt de Avere vFXT omgeving "voor automatisch schalen"?

Nee. U kunt het formaat van een cluster omhoog of omlaag schalen, maar toevoegen of verwijderen van knooppunten van het cluster is een handmatige stap.

### <a name="can-i-run-the-avere-vfxt-cluster-as-a-virtual-machine-scale-set"></a>Kan ik het Avere vFXT cluster worden uitgevoerd als een virtuele-machineschaalset?

Avere vFXT biedt geen ondersteuning voor implementatie van een virtuele-machineschaalset. Verschillende mechanismen voor beschikbaarheid van ingebouwde ondersteuning zijn ontworpen voor atomic virtuele machines die deel uitmaken van een cluster.  

### <a name="can-i-run-the-avere-vfxt-cluster-on-low-priority-vms"></a>Kan ik het Avere vFXT cluster uitvoeren op virtuele machines met lage prioriteit

Nee, moet het systeem een onderliggende stabiele set van virtuele machines.

### <a name="can-i-run-the-avere-vfxt-cluster-in-containers"></a>Kan ik het Avere vFXT-cluster in containers uitvoeren?

Nee, Avere vFXT moet worden geïmplementeerd als een onafhankelijke toepassing.

### <a name="do-the-avere-vfxt-vms-count-against-my-compute-quota"></a>De Avere vFXT aantal virtuele machines op basis van mijn rekenquota doen?

Ja. Zorg ervoor dat u hebt een voldoende quotum in de regio voor de ondersteuning van het cluster.  

### <a name="can-i-run-the-avere-vfxt-cluster-machines-in-different-availability-zones"></a>Kan ik de vFXT Avere cluster machines uitvoeren in verschillende beschikbaarheidszones?

Nee. Het model met hoge beschikbaarheid in Avere vFXT op dit moment biedt geen ondersteuning voor afzonderlijke Avere vFXT leden van het cluster zich in verschillende beschikbaarheidszones.

### <a name="can-i-clone-avere-vfxt-virtual-machines"></a>Kan ik Avere vFXT virtuele machines klonen?

Nee, moet u de ondersteunde Python-script toevoegen of verwijderen van knooppunten in het Avere vFXT-cluster. Lees voor meer informatie, [het Avere vFXT cluster beheren](avere-vfxt-manage-cluster.md).  

### <a name="is-there-a-vm-version-of-the-software-i-can-run-in-my-own-local-environment"></a>Is er een 'VM'-versie van de software die ik in mijn eigen lokale omgeving uitvoeren kan?

Nee, het systeem wordt aangeboden als een geclusterde toestel en getest op specifieke virtuele machine-typen. Deze beperking kan klanten te voorkomen dat het maken van een systeem dat de vereisten voor hoge prestaties van een typische Avere vFXT werkstroom niet kan ondersteunen. 

## <a name="technical-disks"></a>Technische: Disks

### <a name="what-types-of-disks-are-supported-for-the-azure-vms"></a>Welke typen schijven worden ondersteund voor de Azure VM's?

Avere vFXT voor Azure kunt 1 TB of 4 TB premium SSD-configuraties gebruiken. De premium-SSD-configuratie kan worden geïmplementeerd als meerdere beheerde schijven.

### <a name="does-the-cluster-support-unmanaged-disks"></a>Het cluster biedt ondersteuning voor niet-beheerde schijven?

Nee, het cluster beheerde schijven vereist.

### <a name="does-the-system-support-local-attached-ssds"></a>Het systeem biedt ondersteuning voor lokale (gekoppelde) SSD's?

Avere vFXT voor Azure biedt momenteel geen ondersteuning voor lokale SSD's. Schijven die worden gebruikt voor Avere vFXT moeten mogelijk afgesloten en opnieuw gestart, maar lokaal gekoppelde SSD's in deze configuratie kunnen alleen worden beëindigd.

### <a name="does-the-system-support-ultra-ssds"></a>Ondersteunt het systeem ultra SSD's?

Nee, het systeem ondersteunt alleen configuraties van premium SSD.

### <a name="can-i-detach-my-premium-ssds-and-reattach-them-later-to-preserve-cache-contents-between-use"></a>Kan ik mijn premium SSD's loskoppelen en koppelt u deze later als u wilt behouden van de cache-inhoud tussen gebruik opnieuw?

Bezig met ontkoppelen en opnieuw te koppelen SSD's wordt niet ondersteund. Inhoud van de metagegevens of het bestand op de bron mogelijk tussen wordt gebruikt, wat leiden problemen met de gegevensintegriteit tot kunnen zijn gewijzigd.

### <a name="does-the-system-encrypt-the-cache"></a>Het systeem versleuteling van de cache?

Gegevens worden striped verdeeld over de schijven, maar is niet versleuteld. De schijven zelf kunnen echter worden versleuteld. Zie voor meer informatie, [beveiligen en het gebruik van beleidsregels op virtuele machines in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/security-policy#encryption).

## <a name="technical-networking"></a>Technische: Netwerken

### <a name="what-network-is-recommended"></a>Welke netwerk wordt uitvoering aanbevolen?

Als u on-premises opslag met Avere vFXT gebruikt, moet u een 1-Gbps of betere netwerkverbinding hebt. Als u een kleine hoeveelheid gegevens hebt en u bereid bent te kopiëren van gegevens naar de cloud voordat u taken uitvoert, kan VPN-verbindingen voldoende zijn. 

> [!TIP] 
> Hoe langzamer de netwerkkoppeling is, hoe langzamer de initiële koude leesbewerkingen. Trage leesbewerkingen vergroten de latentie van de pijplijn werk. 

### <a name="can-i-run-avere-vfxt-in-a-different-virtual-network-than-my-compute-cluster"></a>Kan ik Avere vFXT uitvoeren in een ander virtueel netwerk dan mijn rekencluster?

Ja, kunt u uw Avere vFXT systeem in een ander virtueel netwerk maken. Lezen [van plan bent uw Avere vFXT systeem](avere-vfxt-deploy-plan.md) voor meer informatie.

### <a name="does-avere-vfxt-require-its-own-subnet"></a>Vereist Avere vFXT een eigen subnet?

Ja. Avere vFXT wordt uitsluitend gebruikt als een cluster met hoge beschikbaarheid (HA) wordt uitgevoerd en vereist meerdere IP-adressen te werken. Als het cluster zich in een eigen subnet, kunt u de kans op conflicten tijdens IP-adres, die problemen voor installatie en de normale werking veroorzaken kan voorkomen. Subnet van het cluster is binnen het bestaande virtuele netwerk, zolang er zijn geen IP-elkaar overlappen adressen.

### <a name="can-i-run-avere-vfxt-on-infiniband"></a>Kan ik Avere vFXT uitvoeren op InfiniBand?

Nee, Avere vFXT alleen wordt gebruikt door Ethernet/IP.

### <a name="how-do-i-access-my-on-premises-nas-environment-from-avere-vfxt"></a>Hoe krijg ik toegang tot mijn on-premises NAS-omgeving van Avere vFXT?

De Avere vFXT-omgeving is net als elke andere Azure-VM's in die hiervoor gerouteerde toegang via een gateway of VPN naar de klant datacenter (en weer). Overweeg het gebruik van Azure ExpressRoute-connectiviteit, als deze beschikbaar zijn in uw omgeving.

### <a name="what-are-the-bandwidth-requirements-for-avere-vfxt"></a>Wat zijn de vereisten voor bandbreedte voor Avere vFXT?

De algemene vereiste netwerkbandbreedte, is afhankelijk van twee factoren: 

* De hoeveelheid gegevens worden opgevraagd bij de bron 
* Het clientsysteem tolerantie voor latentie tijdens de initiële gegevens laden  

Voor latentiegevoelige omgevingen, moet u een fiber-oplossing met een minimale verbindingssnelheid van 1 Gbps. Gebruik ExpressRoute als deze beschikbaar is.  

### <a name="can-i-run-avere-vfxt-with-public-ip-addresses"></a>Kan ik Avere vFXT uitvoeren met openbare IP-adressen?

Nee, Avere vFXT is bedoeld om u te worden uitgevoerd in een netwerkomgeving die is beveiligd met aanbevolen procedures.  

## <a name="technical-back-end-storage-core-filers"></a>Technische: Back-end-opslag (core filter)

### <a name="how-many-core-filers-does-a-single-avere-vfxt-environment-support"></a>Hoeveel core-filter biedt ondersteuning voor één Avere vFXT omgeving?

Een Avere vFXT-cluster biedt ondersteuning voor maximaal 20 core-filter. 

### <a name="how-does-the-avere-vfxt-environment-store-data"></a>Hoe slaat de Avere vFXT omgeving gegevens?

Avere vFXT is geen opslag. Er is een cache die leest en schrijft gegevens uit meerdere prestatiedoelen van storage core filter genoemd. Gegevens die zijn opgeslagen op premium SSD-schijven in Avere vFXT tijdelijk is en uiteindelijk naar de back-end core filer opslag moet worden leeggemaakt.

### <a name="which-core-filers-does-avere-vfxt-support"></a>Welke core-filter biedt ondersteuning voor Avere vFXT?

In het algemeen ondersteunt Avere vFXT voor Azure de volgende systemen als core filter: 

* Dell EMC Isilon (OneFS 7.1, 7.2, 8.0 en 8.1) 
* NetApp ONTAP (modus 9.4, 9.3, 9.2, geclusterde 9.1P1, 8.0 8.3) en (7.* 7-modus, 8.0 8.3) 
* Azure blob-containers (alleen lokaal redundante opslag) 
* AWS S3 buckets 
* Google Cloud buckets

### <a name="why-doesnt-avere-vfxt-support-all-nfs-filers"></a>Waarom niet Avere vFXT ondersteuning voor alle NFS-filter?

Hoewel alle NFS-platforms voldoen aan de dezelfde IETF-normen, in de praktijk heeft elke implementatie een eigen quirks. Deze gegevens van invloed op hoe Avere vFXT communiceert met het opslagsysteem. De ondersteunde systemen zijn de meest gebruikte platforms in de marketplace.

### <a name="does-avere-vfxt-support-private-object-storage-such-as-swiftstack"></a>Ondersteunt Avere vFXT persoonlijke objectopslag (zoals SwiftStack)?

Avere vFXT biedt geen ondersteuning voor persoonlijke objectopslag.

### <a name="how-can-i-get-a-specific-storage-product-under-support"></a>Hoe kan ik een specifieke opslagproduct onder ondersteuning krijgen?

Ondersteuning is gebaseerd op het bedrag van de vraag in het veld. Als er voldoende omzet-aanvragen voor de ondersteuning van een NAS-oplossing, u we achten. Controleer aanvragen via de ondersteuning van Azure.

### <a name="can-i-use-azure-blob-storage-as-a-core-filer"></a>Kan ik Azure Blob-opslag gebruiken als een filer core?

Ja, Avere vFXT voor Azure een blok-blob-container kunt gebruiken als een cloud core filer.  

### <a name="what-are-the-storage-account-requirements-for-a-blob-core-filer"></a>Wat zijn de vereisten voor een opslagaccount voor een blob core filer?

Uw storage-account moet een algemeen gebruik v2 (GPv2)-account en geconfigureerd voor alleen lokaal redundante opslag. Geografisch redundante opslag en zone-redundante opslag worden niet ondersteund.

### <a name="can-i-use-archive-blob-storage"></a>Kan ik archive blob storage gebruiken?

Nee. De service level agreement (SLA) voor archiefopslag is niet compatibel met de realtime map en bestand toegang behoeften van het Avere vFXT-systeem. 

### <a name="can-i-use-cool-blob-storage"></a>Kan ik cool blob-opslag gebruiken?

U kunt de koude laag gebruiken, maar houd er rekening mee dat het aantal bewerkingen veel hoger zijn. 

### <a name="how-do-i-encrypt-the-blob-container"></a>Hoe ik de blob-container versleutelen?

U kunt blob-versleuteling configureren in Azure (bij voorkeur) of op het niveau van de Avere vFXT core filer.  

### <a name="can-i-use-my-own-encryption-key-for-a-blob-core-filer"></a>Kan ik mijn eigen sleutel gebruiken voor een blob core filer?

Gegevens worden standaard versleuteld door Microsoft beheerde sleutels voor Azure-Blob, Table en Queue-opslag, plus Azure Files. U kunt uw eigen sleutel voor versleuteling brengen voor Blob storage en Azure Files. Als u ervoor kiest om Avere vFXT codering te gebruiken, moet u de sleutel Avere gegenereerd en lokaal opslaat. 

## <a name="purchasing"></a>Inkopen

### <a name="how-do-i-get-avere-vfxt-for-azure-licensing"></a>Hoe krijg ik Avere vFXT voor licentieverlening voor Azure?

Ophalen van een vFXT Avere voor Azure-licentie is eenvoudig via de Azure Marketplace. Aanmelden voor een Azure-account en volg de instructies in [implementeren van het cluster van Avere vFXT](avere-vfxt-deploy.md) om een Avere vFXT-cluster te maken. 

### <a name="how-much-does-avere-vfxt-cost"></a>Wat kost Avere vFXT?

Er zijn geen aanvullende licenties kosten voor het gebruik van Avere vFXT clusters in Azure. Klanten zijn verantwoordelijk voor opslag en de kosten van andere Azure-verbruik.

### <a name="can-avere-vfxt-vms-be-run-as-low-priority"></a>Als lage prioriteit Avere vFXT VM's kunnen worden uitgevoerd

Nee, Avere vFXT clusters vereist 'always on' service. De clusters kunnen worden uitgeschakeld wanneer u niet nodig hebt. 

## <a name="next-steps"></a>Volgende stappen

Lees deze artikelen voor meer informatie over het plannen en implementeren van uw eigen systeem om te beginnen met Avere vFXT voor Azure:

* [Plan uw Avere vFXT-systeem](avere-vfxt-deploy-plan.md)
* [Implementatie-overzicht](avere-vfxt-deploy-overview.md)
* [Voorbereidingen voor het maken van een cluster van Avere vFXT](avere-vfxt-prereqs.md)
* [Het Avere vFXT-cluster implementeren](avere-vfxt-deploy.md)

Voor meer informatie over de mogelijkheden en use cases voor Avere vFXT, gaat u naar [Avere vFXT voor Azure](https://azure.microsoft.com/services/storage/avere-vfxt/).
