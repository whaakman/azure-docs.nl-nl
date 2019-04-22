---
title: Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP | Microsoft Docs
description: Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 12/04/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 8b19c0fd8af2792a4ffb877e5c6a7fc6b3f94511
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/17/2019
ms.locfileid: "59699250"
---
# <a name="considerations-for-azure-virtual-machines-dbms-deployment-for-sap-workload"></a>Overwegingen voor Azure Virtual Machines DBMS-implementatie voor de werkbelasting van SAP
[1114181]:https://launchpad.support.sap.com/#/notes/1114181
[1409604]:https://launchpad.support.sap.com/#/notes/1409604
[1597355]:https://launchpad.support.sap.com/#/notes/1597355
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2039619]:https://launchpad.support.sap.com/#/notes/2039619
[2069760]:https://launchpad.support.sap.com/#/notes/2069760
[2171857]:https://launchpad.support.sap.com/#/notes/2171857
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[deployment-guide]:deployment-guide.md
[deployment-guide-3]:deployment-guide.md#b3253ee3-d63b-4d74-a49b-185e76c4088e
[planning-guide]:planning-guide.md

[Logo_Linux]:media/virtual-machines-shared-sap-shared/Linux.png
[Logo_Windows]:media/virtual-machines-shared-sap-shared/Windows.png


[!INCLUDE [learn-about-deployment-models](../../../../includes/learn-about-deployment-models-rm-include.md)]

Deze handleiding is onderdeel van de documentatie over het implementeren en te gebruiken van SAP-software op Microsoft Azure. Lees voordat u deze handleiding leest, de [handleiding voor Planning en implementatie][planning-guide]. In dit document bevat informatie over de algemene implementatie-aspecten met betrekking tot SAP DBMS-systemen op Microsoft Azure-machines (VM's) met behulp van de Azure-infrastructuur als een service (IaaS)-mogelijkheden.

Het document is een aanvulling op de documentatie voor installatie van SAP en SAP-opmerkingen, die staan voor de primaire bronnen voor installaties en -implementaties van SAP-software op de opgegeven platforms.

In dit document, zijn overwegingen met betrekking tot SAP DBMS-systemen uitvoeren in de Azure-VM's geïntroduceerd. Er zijn enkele verwijzingen naar specifieke DBMS-systemen in dit hoofdstuk. In plaats daarvan worden de specifieke DBMS-systemen afgehandeld in dit artikel, na dit document.

## <a name="definitions"></a>Definities
In het hele document, worden deze termen gebruikt:

* **IaaS**: Infrastructuur als een service.
* **PaaS**: Platform als een service.
* **SaaS**: Software als een service.
* **SAP-onderdeel**: Een afzonderlijke SAP-toepassing, zoals ERP centraal onderdeel (ECC), Business Warehouse (BW), Manager van de oplossing of Enterprise-Portal (EP). SAP-onderdelen kunnen worden gebaseerd op traditionele ABAP- of Java-technologieën of op een niet-NetWeaver-toepassing op basis van zoals zakelijke objecten.
* **SAP-omgeving**: Een of meer SAP-onderdelen die logisch zijn gegroepeerd om uit te voeren van een zakelijke-functie, zoals ontwikkeling, kwaliteit te waarborgen, training, herstel na noodgevallen of productie.
* **SAP-landschap**: Deze term heeft betrekking op de gehele SAP-elementen in een klant IT landschap. De SAP-landschap bevat alle productie- en niet-productieve omgevingen.
* **SAP-systeem**: De combinatie van een DBMS-laag en een toepassingslaag van bijvoorbeeld een systeem voor SAP ERP-ontwikkeling testen een SAP Business Warehouse-systeem, of een SAP CRM productie-systeem. In de Azure-implementaties, wordt niet delen van deze twee lagen tussen on-premises en Azure ondersteund. Als gevolg hiervan is een SAP-systeem on-premises geïmplementeerd of de geïmplementeerd in Azure. U kunt de verschillende systemen van SAP-landschap dat in Azure of on-premises implementeren. U kunt bijvoorbeeld implementeert de SAP CRM-ontwikkeling en testen van systemen in Azure maar de SAP CRM productie system on-premises implementeren.
* **Cross-premises**: Beschrijving van een scenario waarbij virtuele machines worden geïmplementeerd naar een Azure-abonnement met site-naar-site, met meerdere sites of Azure ExpressRoute-connectiviteit tussen de gegevens on-premises datacenters en Azure. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als cross-premises scenario's. 

    De reden voor de verbinding is om de on-premises domeinen, on-premises Active Directory en DNS-on-premises uitbreiden naar Azure. Het on-premises-landschap is uitgebreid naar de Azure-assets van het abonnement. Met deze uitbreiding, kunnen de virtuele machines deel uitmaken van de on-premises domein. Gebruikers van een domein van de on-premises domein kunnen toegang tot de servers en services uitvoeren op deze virtuele machines, net als DBMS-services. On-premises communicatie en naamomzetting tussen VM's geïmplementeerd en virtuele machines die worden geïmplementeerd in Azure is mogelijk. In dit scenario is het meest voorkomende scenario gebruikt assets van SAP op Azure implementeren. Zie voor meer informatie, [Planning en ontwerp voor VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Cross-premises implementaties van SAP-systemen zijn waarvoor Azure virtuele machines met SAP-systemen, leden van een on-premises domein en worden ondersteund voor productie SAP-systemen. Cross-premises configuraties worden ondersteund voor het implementeren van onderdelen of uitvoeren van SAP-landschappen in Azure. De complete SAP-landschap dat zelfs uitvoeren in Azure vereist die VM's deel uitmaken van een on-premises domein en Active Directory/LDAP. 
>
> In eerdere versies van de documentatie, zijn hybride IT-scenario's vermeld. De term *hybride* verankerd ligt in het feit dat er een cross-premises-connectiviteit tussen on-premises en Azure. In dit geval betekent hybride ook dat de virtuele machines in Azure deel uit van de on-premises Active Directory maken.
>
>

Sommige Microsoft-documentatie worden cross-premises scenario's iets anders, met name voor DBMS hoge beschikbaarheid configuraties beschreven. In het geval van de SAP-gerelateerde documenten, het cross-premises scenario komt meestal neer op site-naar-site of persoonlijke [ExpressRoute](https://azure.microsoft.com/services/expressroute/) connectiviteit en SAP-landschap dat wordt gedistribueerd tussen on-premises en Azure.

## <a name="resources"></a>Resources
Er zijn andere artikelen beschikbaar op de werkbelasting van SAP op Azure. Beginnen met [SAP-workloads op Azure: Aan de slag](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) en kies vervolgens uw interessegebied.

De volgende SAP-opmerkingen zijn gerelateerd aan SAP op Azure met betrekking tot het gebied in dit document behandeld.

| Houd er rekening mee getal | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen op Azure: Ondersteunde producten en typen Azure VM 's |
| [2015553] |SAP op Microsoft Azure: Vereisten voor ondersteuning |
| [1999351] |Het oplossen van uitgebreide Azure bewaking voor SAP |
| [2178632] |Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [1409604] |Virtualisatie in Windows: Uitgebreide bewaking |
| [2191498] |SAP op Linux met Azure: Uitgebreide bewaking |
| [2039619] |SAP-toepassingen op Microsoft Azure met behulp van de Oracle-database: Ondersteunde producten en versies |
| [2233094] |DB6: SAP-toepassingen op Azure met behulp van IBM DB2 voor Linux, UNIX- en Windows: Aanvullende informatie |
| [2243692] |Linux op Microsoft Azure (IaaS) virtuele machine: Problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [2002167] |Red Hat Enterprise Linux 7.x: Installatie en upgrade |
| [2069760] |Oracle Linux 7.x SAP-installatie en upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |
| [2171857] |Oracle Database 12c: Ondersteuning van het bestandssysteem in Linux |
| [1114181] |Oracle Database 11g: Ondersteuning van het bestandssysteem in Linux |


Zie voor informatie over alle SAP-opmerkingen voor Linux, de [SAP community wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes).

U moet een praktische kennis van Microsoft Azure-architectuur en hoe Microsoft Azure virtuele machines zijn geïmplementeerd en worden beheerd. Zie voor meer informatie, [documentatie voor Azure](https://docs.microsoft.com/azure/).

In het algemeen zijn de installatie van Windows, Linux en DBMS-systemen en configuratie in wezen hetzelfde als een virtuele machine of bare metal machine die u on-premises installeert. Er zijn enkele architectuur en system management-implementatie beslissingen die verschillen wanneer u Azure IaaS. Dit document wordt uitgelegd voor de specifieke architectuur en de system management verschillen worden voorbereid op wanneer u Azure IaaS.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Opslagstructuur van een virtuele machine voor RDBMS-implementaties
Als u wilt volgen dit hoofdstuk, lezen en informatie over de gegevens die zijn gepresenteerd in [in dit hoofdstuk] [ deployment-guide-3] van de [Deployment Guide][deployment-guide]. U moet begrijpen en te weten over de verschillende VM-serie en de verschillen tussen standard en premium storage voordat u dit hoofdstuk leest. 

Voor meer informatie over Azure Storage voor Azure VM's, Zie:

- [Inleiding tot beheerde schijven voor virtuele Azure Windows-machines](../../windows/managed-disks-overview.md).
- [Inleiding tot beheerde schijven voor virtuele Azure Linux-machines](../../linux/managed-disks-overview.md).

In een eenvoudige configuratie, gewoonlijk het beste de structuur van een implementatie waar het besturingssysteem, de DBMS-systemen en de uiteindelijke SAP-binaire bestanden staan los van de databasebestanden. U wordt aangeraden dat de SAP-systemen die worden uitgevoerd in virtuele Azure-machines hebben de base VHD of schijf, geïnstalleerd met het besturingssysteem, de database management systeem uitvoerbare bestanden en de SAP-uitvoerbare bestanden. 

De DBMS-gegevens en logboekbestanden bestanden worden opgeslagen in de standard-opslag of premium-opslagaccount. Ze die zijn opgeslagen in afzonderlijke schijven en als logische schijven die zijn gekoppeld aan de oorspronkelijke Azure besturingssysteemkopie VM. Voor Linux-distributies, worden andere aanbevelingen beschreven, met name voor SAP HANA.

Wanneer u van plan uw schijfindeling bent, vindt u de beste balans tussen deze items:

* Het aantal gegevensbestanden.
* Het aantal schijven dat de bestanden bevatten.
* De quota IOPS van één schijf.
* De gegevensdoorvoer per schijf.
* Het aantal extra gegevensschijven mogelijk per VM-grootte.
* Een virtuele machine in de algemene opslagdoorvoer kan bieden.
* De latentie u een andere Azure Storage-typen krijgt.
* Sla voor VM's.

Azure dwingt een quotum IOP's per gegevensschijf. Deze quota zijn verschillend voor schijven die worden gehost op standard-opslag- en premium-opslag. I/o-latentie is ook verschil is tussen de twee soorten opslag. Premium storage voorziet in betere i/o-latentie. 

Elk van de verschillende typen van de virtuele machine heeft een beperkt aantal gegevensschijven die u kunt koppelen. Een andere beperking is dat alleen bepaalde typen VM premium storage kunnen gebruiken. Doorgaans wilt u een bepaalde VM-type op basis van CPU en geheugen gebruiken. Kunt u ook overwegen de IOPS, latentie en doorvoer schijfvereisten die meestal worden geschaald met het aantal schijven of het type premium-opslagschijven. Het aantal IOPS en de doorvoer kan worden bereikt door elke schijf kan bepalen schijfgrootte, met name met premium storage.

> [!NOTE]
> Voor implementaties van DBMS-systemen, wij raden het gebruik van premium-opslag voor elk soort gegevens, het transactielogboek of bestanden opnieuw. Het maakt niet uit of u wilt implementeren, productie- of niet-productieve systemen.

> [!NOTE]
> Om te profiteren van Azure met de unieke [één VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/), alle schijven die zijn gekoppeld moet het schijftype voor premium storage, waaronder de basis-VHD.

> [!NOTE]
> Hoofddatabase machinebestanden te hosten, zoals gegevens en logboekbestanden, SAP-databases op opslaghardware die zich in dezelfde locatie bevindt als externe datacenters naast Azure-datacenters wordt niet ondersteund. Voor SAP-workloads, alleen opslag die wordt weergegeven als systeemeigen Azure-service wordt ondersteund voor de gegevens- en transactielogboek-logboekbestanden van SAP-databases.

De plaatsing van bestanden van de database en de bestanden logboek- en opnieuw uitvoeren en het type Azure-opslagcapaciteit die u gebruikt, wordt gedefinieerd door de vereisten voor IOPS, latentie en doorvoer. Als u wilt dat voldoende IOP's, kan u worden afgedwongen naar meerdere schijven gebruiken of gebruik een grotere premium-opslagschijf. Als u meerdere schijven, bouw een streep software op de schijven die de bestanden of het logboek bevatten en bestanden opnieuw. In dergelijke gevallen de IOPS-waarde en de schijfdoorvoer Sla's van de onderliggende premiumopslag worden schijven of de maximale haalbare IOPS van de standard-opslagschijven wél cumulatief voor de resulterende stripeset.

Als al is vermeld, saldo als de vereiste IOPS is groter dan wat één VHD kan bieden, het aantal IOP's die nodig zijn voor de databasebestanden naar een aantal VHD's. De eenvoudigste manier om de IOPS verdelen over schijven is het bouwen van een streep software via de verschillende schijven. Vervolgens plaatst u een aantal gegevensbestanden van de SAP DBMS in de LUN's oppervlaktevariaties buiten de software-stripe. het aantal schijven in de streep wordt aangestuurd door IOPs eisen, schijf doorvoer eisen en volume eisen.


- - -
> ![Windows][Logo_Windows] Windows
>
> U wordt aangeraden gebruik te maken van Windows Storage Spaces aan te brengen stripesets op meerdere Azure-VHD's. Ten minste Windows Server 2012 R2 of Windows Server 2016.
>
> ![Linux][Logo_Linux] Linux
>
> Alleen MDADM en logische Volume Manager (LVM) worden ondersteund voor het bouwen van een software-RAID op Linux. Zie voor meer informatie:
>
> - [Software-RAID op Linux configureren](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) MDADM gebruiken
> - [LVM configureren op een Linux-VM in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) LVM gebruiken
>
>

- - -

> [!NOTE]
> Omdat Azure Storage kan drie kopieën van de VHD's voldoen, verstandig niet het om te configureren van een redundantie wanneer u stripe. U hoeft alleen striping configureren zodat de i/o's worden verdeeld over de verschillende VHD's.
>

### <a name="managed-or-nonmanaged-disks"></a>Beheerde of niet-beheerde schijven
Een Azure storage-account is een met beheerdersrechten om voor te bereiden en ook een onderwerp met beperkingen. Beperkingen verschillen tussen standard storage-accounts en premium storage-accounts. Zie voor informatie over de mogelijkheden en beperkingen, [schaalbaarheids- en prestatiedoelen voor Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets).

Voor standard-opslag, houd er rekening mee dat er een limiet op de IOP's per opslagaccount geldt. Zie de rij met **totale snelheid van aanvragen voor** in het artikel [schaalbaarheids- en prestatiedoelen voor Azure Storage](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets). Er is ook een eerste limiet voor het aantal opslagaccounts per Azure-abonnement. Saldo VHD's voor de grotere SAP-landschap via verschillende opslagaccounts om te voorkomen dat de grenzen van deze opslagaccounts te maken. Dit is telkens wanneer u over enkele honderden virtuele machines met meer dan duizend VHD's praten.

Omdat het gebruik van standard-opslag voor implementaties in combinatie met de werkbelasting van een SAP DBMS-systemen wordt niet aanbevolen, verwijzingen en aanbevelingen voor de standard-opslag zijn beperkt tot dit korte [artikel](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)

Om te voorkomen dat de beheerwerkzaamheden van plannen en implementeren van VHD's voor andere Azure storage-accounts, Microsoft geïntroduceerd [Azure Managed Disks](https://azure.microsoft.com/services/managed-disks/) in 2017. Beheerde schijven zijn beschikbaar voor standard-opslag- en premium-opslag. De belangrijkste voordelen van beheerde schijven in vergelijking met niet-beheerde schijven zijn:

- Voor beheerde schijven verdeeld Azure over de verschillende VHD's verschillende opslagaccounts automatisch tijdens de implementatie. Op deze manier storage-account beperkt voor gegevensvolume, i/o-doorvoer en IOPS niet worden bereikt.
- Azure Storage met behulp van beheerde schijven, zich houdt aan de concepten van Azure-beschikbaarheidssets. Als de virtuele machine deel van een Azure-beschikbaarheidsset uitmaakt, worden de basis-VHD en gekoppelde schijf van een virtuele machine geïmplementeerd in verschillende fout- en updatedomeinen.


> [!IMPORTANT]
> De voordelen van Azure Managed Disks worden gegeven, raden wij aan dat u Azure Managed Disks voor uw DBMS-implementaties en de SAP-implementaties in het algemeen.
>

Als u wilt converteren van niet-beheerde naar managed disks, Zie:

- [Een Windows virtuele machine van niet-beheerde schijven converteren naar managed disks](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks).
- [Een virtuele Linux-machine van niet-beheerde schijven converteren naar managed disks](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks).


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>In cache opslaan voor virtuele machines en de gegevensschijven
Wanneer u schijven voor virtuele machines koppelt, kunt u kiezen of het i/o-verkeer tussen de virtuele machine en deze schijven zich bevinden in Azure-opslag in cache is opgeslagen. Standard en premium storage gebruik van twee verschillende technologieën voor dit type cache.

De volgende aanbevelingen wordt ervan uitgegaan dat deze i/o-kenmerken voor standard DBMS:

- Het is voornamelijk een lezen werkbelasting voor de gegevensbestanden van een database. Deze leesbewerkingen zijn de prestaties van cruciaal belang voor het systeem DBMS-systemen.
- Er treedt een schrijven op basis van de gegevensbestanden op pieken op basis van controlepunten of een constante stream. Gemiddelde gedurende een dag, zijn er minder schrijfbewerkingen dan leesbewerkingen. In het lezen van gegevensbestanden, deze schrijfbewerkingen zijn asynchroon en gebruikerstransacties die geen houdt.
- Er zijn nauwelijks leesbewerkingen van de transactie logboek of opnieuw bestanden. Uitzonderingen zijn grote i/o's wanneer u transactielogboekback-ups uitvoeren.
- De belangrijkste belasting met logboekbestanden transactie of opnieuw wordt geschreven. Afhankelijk van de aard van de werkbelasting, kunt u i/o's hebben zo klein is als 4 KB of, in andere gevallen, i/o-grootte van 1 MB of meer.
- Alle schrijfbewerkingen moeten worden opgeslagen op schijf op een betrouwbare manier worden geactiveerd.

Voor standard-opslag zijn de mogelijke cache-typen:

* Geen
* Lezen
* Lezen/schrijven

Voor consistente en deterministisch prestaties, de cache instellen op standard-opslag voor alle schijven die gegevens met betrekking tot de DBMS-bestanden bevatten melden en opnieuw uitvoeren van bestanden en tabelruimte te **NONE**. Het in cache plaatsen van de base VHD kan blijven met de standaardinstellingen.

Voor premiumopslag bestaat de volgende opties voor opslaan in cache:

* Geen
* Lezen
* Lezen/schrijven
* Geen en Write Accelerator, dat alleen voor Azure-M-serie VM's
* Lees- en Write Accelerator, dat alleen voor Azure-M-serie VM 's

Voor premiumopslag, raden wij aan dat u **Lees-caching voor gegevensbestanden** van de SAP-database en kies **niet in cache opslaan voor de schijven van een of meer logboekbestanden**.

Voor implementaties van de M-serie, raden wij aan dat u Azure Write Accelerator voor uw implementatie DBMS-systemen. Zie voor meer informatie, beperkingen en implementatie van Azure Write Accelerator [Write Accelerator inschakelt](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator).


### <a name="azure-nonpersistent-disks"></a>Niet-persistente Azure-schijven
Azure-VM's bieden een niet-persistente schijven na de implementatie van een virtuele machine. Alle inhoud op deze schijven zijn in het geval van een virtuele machine opnieuw opstarten gewist. Het is een gezien het feit dat de gegevensbestanden en logboek- en opnieuw bestanden van databases onder geen enkele omstandigheid zich op de persistente stations bevinden moeten. Mogelijk zijn er uitzonderingen voor enkele databases, waar deze persistente stations mogelijk geschikt voor tempdb en tijdelijke tabelruimten. Vermijd het gebruik van deze schijven voor virtuele machines uit de A-serie, omdat deze persistente schijven zijn beperkt in doorvoer met deze VM-reeks. 

Zie voor meer informatie, [het tijdelijke station op Windows-VM's in Azure begrijpen](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/).

- - -
> ![Windows][Logo_Windows] Windows
>
> Station D in een Azure VM is een persistente station, die wordt ondersteund door sommige lokale schijven op de Azure compute-knooppunt. Omdat het persistente, alle wijzigingen aan de inhoud op station D gaan verloren wanneer de virtuele machine opnieuw wordt opgestart. Wijzigingen zijn bestanden die zijn opgeslagen, mappen die zijn gemaakt en toepassingen die zijn geïnstalleerd.
>
> ![Linux][Logo_Linux] Linux
>
> Linux Azure-VM's automatisch een station koppelen aan /mnt/resource die is een persistente station ondersteund door lokale schijven op de Azure compute-knooppunt. Omdat het persistente, wijzigingen aangebracht aan de inhoud in/mnt/resource gaan verloren wanneer de virtuele machine opnieuw wordt opgestart. Wijzigingen zijn bestanden die zijn opgeslagen, mappen die zijn gemaakt en toepassingen die zijn geïnstalleerd.
>
>

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage tolerantie
Microsoft Azure Storage slaat de basis-VHD met besturingssysteem en de gekoppelde schijven of de blobs, op ten minste drie afzonderlijke opslagknooppunten. Dit type opslag is lokaal redundante opslag (LRS) genoemd. LRS is de standaardwaarde voor alle typen opslag in Azure.

Er zijn andere methoden voor redundantie. Zie voor meer informatie, [Azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Premium-opslag is het aanbevolen type opslag voor de DBMS-VM's en schijven die opslaan van de database en logboekbestanden en bestanden opnieuw. De methode is alleen beschikbaar redundantie voor premium-opslag is LRS. Als gevolg hiervan moet u database-methoden voor het inschakelen van replicatie van de database-gegevens in een andere Azure-regio of beschikbaarheid zone configureren. Database-methoden zijn SQL Server Always On, Oracle Data Guard en HANA System Replication.


> [!NOTE]
> Voor de DBMS-implementaties, wordt niet het gebruik van geografisch redundante opslag (GRS) aanbevolen voor standard-opslag. GRS is van invloed op prestaties ernstige problemen en niet voldoen aan de order schrijven in verschillende virtuele harde schijven die zijn gekoppeld aan een virtuele machine. Niet naleven van de volgorde schrijven over verschillende VHD's mogelijk leidt tot inconsistente databases aan de replicatie. Deze situatie treedt op als de database en logboekbestanden en opnieuw worden verdeeld over meerdere VHD's, zoals meestal het geval is, op de bron-VM aan clientzijde is.



## <a name="vm-node-resiliency"></a>Knooppunt-VM-tolerantie
Azure biedt verschillende verschillende Sla's voor virtuele machines. Zie voor meer informatie, de meest recente versie van [SLA voor virtuele Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). Omdat de DBMS-laag meestal essentieel is voor de beschikbaarheid in een SAP-systeem is, moet u over de beschikbaarheidssets, beschikbaarheidszones en onderhoud. Zie voor meer informatie over deze concepten [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) en [de beschikbaarheid van virtuele Linux-machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).

De minimale aanbeveling voor productie DBMS-scenario's met een SAP-workload is:

- Implementeer twee virtuele machines in een afzonderlijke beschikbaarheidsset in dezelfde Azure-regio.
- Deze twee virtuele machines uitvoeren in dezelfde Azure virtual network en hebben NIC's die naar de dezelfde subnetten zijn gekoppeld.
- Database-methoden gebruiken om een hot stand-by met de tweede virtuele machine. Methoden kunnen bestaan uit SQL Server Always On, Oracle Data Guard of HANA System Replication.

U kunt ook een derde VM in een andere Azure-regio implementeren en de dezelfde database-methoden gebruiken om op te geven van een asynchrone replica in een andere Azure-regio.

Zie voor meer informatie over het instellen van Azure-beschikbaarheidssets [in deze zelfstudie](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).



## <a name="azure-network-considerations"></a>Overwegingen met betrekking tot Azure-netwerk
In grootschalige SAP-implementaties, gebruikt u de blauwdruk van [Azure Virtual Datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter). Deze gebruiken voor uw virtuele netwerk configureren en de machtigingen en de rol toewijzingen aan verschillende onderdelen van uw organisatie.

Deze aanbevolen procedures zijn het resultaat van honderden implementaties van klanten:

- De virtuele netwerken die de SAP-toepassing wordt geïmplementeerd in hebt geen toegang tot het internet.
- De virtuele machines van de database worden uitgevoerd in hetzelfde virtuele netwerk bevinden als de toepassingslaag.
- De virtuele machines binnen het virtuele netwerk hebben een statische toewijzing van het particuliere IP-adres. Zie voor meer informatie, [IP-adres adrestypen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm).
- Routering beperkingen naar en van de DBMS-VM's zijn *niet* instellen met een firewall is geïnstalleerd op de lokale DBMS-VM's. Routering van verkeer in plaats daarvan is gedefinieerd met [netwerkbeveiligingsgroepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/security-overview).
- Toewijzen voor het scheiden en verkeer isoleren voor de VM DBMS, verschillende NIC's aan de virtuele machine. Elke NIC een ander IP-adres opgehaald en elke NIC is toegewezen aan een ander virtueel netwerk-subnet. Elk subnet heeft verschillende NSG-regels. De isolatie- of scheiding van netwerkverkeer is een maateenheid voor de routering. Het wordt niet gebruikt voor het instellen van quota's voor de netwerkdoorvoer.

> [!NOTE]
> Toewijzen van vaste IP-adressen via Azure betekent toe te wijzen aan afzonderlijke virtuele NIC's. Statische IP-adressen binnen het gastbesturingssysteem niet toewijzen aan een virtuele NIC. Sommige Azure-services zoals Azure Backup is afhankelijk van het feit dat op minimaal de primaire virtuele NIC is ingesteld op DHCP- en niet op de vaste IP-adressen. Zie voor meer informatie, [los problemen met Azure virtuele machine back-up](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Toewijzen als u wilt meerdere statische IP-adressen toewijzen aan een virtuele machine, meerdere virtuele NIC's aan een virtuele machine.
>


> [!IMPORTANT]
> Configureren van [virtuele netwerkapparaten](https://azure.microsoft.com/solutions/network-appliances/) in het communicatiepad voor tussen de SAP-toepassing en de DBMS-laag van een SAP-NetWeaver - Hybris- of S/4HANA gebaseerde SAP-systeem wordt niet ondersteund. Deze beperking is omwille van de functionaliteit en prestaties. Het communicatiepad tussen het niveau van de SAP-toepassing en de DBMS-laag moet een direct. De beperking bevat geen [toepassingsbeveiligingsgroep (ASG) en NSG-regels](https://docs.microsoft.com/azure/virtual-network/security-overview) als deze ASG en NSG-regels toestaat een rechtstreekse communicatie-pad dat. 
>
> Er zijn andere scenario's waar virtuele netwerkapparaten worden niet ondersteund in:
>
> * Communicatiepaden tussen Azure-VM's die staan voor Linux Pacemaker cluster knooppunten en SBD apparaten, zoals beschreven in [hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse).
> * Communicatiepaden tussen Azure VM's en Windows Server Scale-Out bestandsserver (SOFS) instellen zoals beschreven in maximaal [Cluster een SAP ASCS/SCS-exemplaar op een Windows-failovercluster met behulp van een bestandsshare in Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). 
>
> Virtuele netwerkapparaten in communicatiepaden kunnen eenvoudig dubbele de netwerklatentie tussen twee communicatie-partners. Ze kunnen ook doorvoer in kritieke paden tussen het niveau van de SAP-toepassing en de DBMS-laag beperken. In sommige gevallen klant virtuele netwerkapparaten kunnen leiden tot Pacemaker Linux-clusters mislukken. Dit zijn gevallen waar de communicatie tussen de knooppunten van het Linux-Pacemaker op hun apparaat SBD via een virtueel netwerkapparaat communiceren.
>

> [!IMPORTANT]
> Een andere die ontwerpen van *niet* de scheiding van het niveau van de SAP-toepassing en de DBMS-laag in verschillende virtuele netwerken die niet wordt ondersteund is [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) met elkaar. Het is raadzaam dat u de SAP-toepassingslaag en DBMS-laag scheiden met behulp van subnetten binnen een Azure-netwerk in plaats van met behulp van verschillende virtuele netwerken. 
>
> Als u besluit niet op de aanbeveling volgen en in plaats daarvan de twee lagen te scheiden in verschillende virtuele netwerken, de twee virtuele netwerken moeten [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview). 
>
> Houd er rekening mee dat netwerkverkeer tussen twee [gekoppeld](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) virtuele netwerken van Azure is onderworpen aan de kosten van gegevensoverdracht. Grote gegevensvolume heeft dat uit vele terabytes bestaat worden uitgewisseld tussen de SAP-toepassingslaag en de DBMS-laag. Als de SAP-toepassingslaag en DBMS-laag zijn gescheiden tussen twee gekoppelde virtuele netwerken in Azure, kunt u aanzienlijke kosten oplopen.

Gebruik twee virtuele machines voor uw productie-implementatie van de DBMS-systemen binnen een Azure-beschikbaarheidsset instellen. Ook gebruik van afzonderlijke routering voor de SAP-toepassingslaag en het beheer en bewerkingen verkeer naar de twee DBMS-VM's. Zie de volgende afbeelding:

![Diagram van twee virtuele machines in twee subnetten](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="use-azure-load-balancer-to-redirect-traffic"></a>Gebruik Azure Load Balancer verkeer omleiden
Het gebruik van persoonlijke virtuele IP-adressen die worden gebruikt in functies ingebouwd, zoals SQL Server Always On of HANA System Replication is vereist voor de configuratie van een Azure load balancer. De load balancer maakt gebruik van poorten van de test om te bepalen van het actieve knooppunt van de DBMS-systemen en het verkeer routeren uitsluitend naar dat knooppunt actieve database. 

Als er een failover van de database-knooppunt, is er niet nodig voor de SAP-toepassing te configureren. In plaats daarvan opnieuw de meest voorkomende architecturen voor de SAP-toepassingen op basis van het persoonlijke virtuele IP-adres. In de tussentijd zorgen, de load balancer reageert op een failover van het knooppunt door het verkeer op basis van het persoonlijke virtuele IP-adres wordt omgeleid naar het tweede knooppunt.

Azure biedt twee verschillende [load balancer-SKU's](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview): een basis-SKU en een standaard-SKU. Tenzij u implementeren in Azure availability zones wilt, wordt de basisversie van load balancer SKU prima doet.

Het verkeer tussen de DBMS-VM's en de SAP-toepassingslaag altijd worden gerouteerd via de load balancer altijd is? Het antwoord is afhankelijk van hoe u de load balancer configureren. 

Op dit moment wordt altijd het inkomende verkeer voor de DBMS-VM doorgestuurd via de load balancer. Het uitgaande verkeer route van de DBMS-VM naar het niveau van de toepassing dat VM, is afhankelijk van de configuratie van de load balancer. 

De load balancer biedt een optie van DirectServerReturn. Als deze optie is geconfigureerd, wordt het verkeer van de DBMS-VM wordt omgeleid naar de SAP-toepassingslaag is *niet* doorgestuurd via de load balancer. Het gaat in plaats daarvan rechtstreeks naar het niveau van de toepassing. Als DirectServerReturn niet is geconfigureerd, wordt het retourverkeer op het niveau van de SAP-toepassing wordt doorgestuurd via de load balancer.

Het is raadzaam dat u DirectServerReturn configureert in combinatie met load balancers die tussen de SAP-toepassingslaag en de DBMS-laag worden geplaatst. Deze configuratie vermindert netwerklatentie tussen de twee lagen.

Zie voor een voorbeeld van hoe u deze configuratie met SQL Server Always On instelt, [een ILB-listener voor AlwaysOn-beschikbaarheidsgroepen configureren in Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Als u gepubliceerde sjablonen van GitHub JSON als uitgangspunt voor uw SAP-infrastructuur-implementaties in Azure gebruikt, Bestudeer dit [sjabloon voor een SAP-systeem 3-Laagse](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). In deze sjabloon kunt u ook de juiste instellingen voor de load balancer bekijken.

### <a name="azure-accelerated-networking"></a>Azure versnelde netwerken
Om de netwerklatentie tussen virtuele Azure-machines te verminderen, wordt aangeraden dat u kiest [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/). Dit gebruiken wanneer u Azure VM's bij een SAP-workload, met name voor de SAP-toepassingslaag en de SAP DBMS-laag implementeren.

> [!NOTE]
> Niet alle VM-typen bieden ondersteuning voor versnelde netwerken. Het vorige artikel geeft een lijst van de VM-typen die ondersteuning bieden voor versnelde netwerken.
>

- - -
> ![Windows][Logo_Windows] Windows
>
> Zie voor meer informatie over het implementeren van virtuele machines met versnelde netwerken voor Windows, [maken van een Windows-machine met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell).
>
> ![Linux][Logo_Linux] Linux
>
> Zie voor meer informatie over Linux-distributie [maken van een virtuele Linux-machine met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli).
>
>

- - -

> [!NOTE]
> In het geval van SUSE, Red Hat en Oracle Linux wordt versnelde netwerken ondersteund met recente versies. Oudere versies, zoals SLES 12 SP2 of RHEL 7.2 bieden geen ondersteuning voor Azure Accelerated Networking.
>


## <a name="deployment-of-host-monitoring"></a>Implementatie van de host controleren
SAP vereist voor gebruik in productieomgevingen van SAP-toepassingen in virtuele machines van Azure, de mogelijkheid om op te halen van de host bewakingsgegevens van de fysieke hosts waarop de virtuele machines van Azure wordt uitgevoerd. Er is een specifieke Host-Agent voor SAP-patchniveau vereist waarmee deze mogelijkheid in SAPOSCOL en SAP Host-Agent. De exacte patchniveau wordt beschreven in de SAP-notitie [1409604].

Zie voor meer informatie over de implementatie van onderdelen die hostgegevens aan SAPOSCOL en SAP-Hostagent leveren en het beheer van de levenscyclus van deze onderdelen de [Implementatiehandleiding][deployment-guide].


## <a name="next-steps"></a>Volgende stappen
Zie voor meer informatie over een bepaald DBMS:

- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SQL Server](dbms_guide_sqlserver.md)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor Oracle](dbms_guide_oracle.md)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor IBM DB2](dbms_guide_ibm.md)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SAP ASE](dbms_guide_sapase.md)
- [SAP maxDB, Cache voor Live en implementatie van de Server voor webinhoud op Azure](dbms_guide_maxdb.md)
- [Bedieningsgids voor SAP HANA op Azure](hana-vm-operations.md)
- [SAP HANA met hoge beschikbaarheid voor Azure virtual machines](sap-hana-availability-overview.md)
- [Back-uphandleiding voor SAP HANA op Azure virtual machines](sap-hana-backup-guide.md)

