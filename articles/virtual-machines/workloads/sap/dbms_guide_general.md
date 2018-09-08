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
ms.date: 09/06/2018
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: e46503f8dc97f58db1cd5acfd2122e2895fb15b0
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162305"
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

Deze handleiding is onderdeel van de documentatie over het implementeren en de SAP-software op Microsoft Azure implementeren. Lees voordat u deze handleiding leest, de [Planning and Implementation Guide][planning-guide]. In dit document bevat informatie over de algemene implementatie-aspecten van met betrekking tot SAP DBMS-systemen op Microsoft Azure Virtual Machines (VM's) met behulp van de Azure-infrastructuur als een Service (IaaS)-mogelijkheden.

Het document is een aanvulling op de documentatie voor installatie van SAP en SAP-opmerkingen, die staan voor de primaire bronnen voor installaties en -implementaties van SAP-software op de opgegeven platforms.

In dit document, zijn overwegingen met betrekking tot SAP DBMS-systemen uitvoeren in de Azure-VM's geïntroduceerd. Er zijn enkele verwijzingen naar specifieke DBMS-systemen in dit hoofdstuk. In plaats daarvan worden de specifieke DBMS-systemen afgehandeld in dit artikel, na dit document.

## <a name="definitions-upfront"></a>Definities kosten vooraf
In het hele document, worden de volgende termen gebruikt:

* IaaS: Infrastructure as a Service.
* PaaS: Platform as a Service.
* SaaS: Software as a Service.
* SAP-onderdeel: Een afzonderlijke SAP-toepassing, zoals ECC, BW, Manager van de oplossing of EP  SAP-onderdelen kunnen worden gebaseerd op de traditionele ABAP- of Java-technologieën of een toepassing NetWeaver op basis van zoals zakelijke objecten.
* SAP-omgeving: een of meer onderdelen van SAP logisch zijn gegroepeerd om uit te voeren van een zakelijke-functie, zoals ontwikkeling, QAS, Training, herstel na Noodgevallen of productie.
* SAP-landschap: Deze term heeft betrekking op de gehele SAP-elementen in een klant IT landschap. De SAP-landschap bevat alle productie- en niet-productieomgevingen.
* SAP-systeem: De combinatie van DBMS-laag en niveau van de toepassing van bijvoorbeeld een ontwikkelsysteem SAP ERP, SAP BW-testsysteem, SAP CRM productiesysteem, enzovoort. In de Azure-implementaties, is het niet ondersteund voor het delen van deze twee lagen tussen on-premises en Azure. Dit betekent dat die een SAP-systeem een is on-premises geïmplementeerd of deze is geïmplementeerd in Azure. U kunt echter de verschillende systemen van SAP-landschap dat in Azure of on-premises implementeren. U kunt bijvoorbeeld implementeert de SAP CRM-ontwikkeling en testen van systemen in Azure, maar de SAP CRM productie system on-premises.
* Cross-Premises: Beschrijving van een scenario waarbij virtuele machines worden geïmplementeerd naar een Azure-abonnement dat site-naar-site, meerdere locaties of ExpressRoute-connectiviteit tussen de on-premises clientresources en Azure. In algemene Azure-documentatie en dit soort implementaties worden ook beschreven als Cross-Premises scenario's. De reden voor de verbinding is om de on-premises domeinen, on-premises Active Directory en DNS-on-premises uitbreiden naar Azure. Het on-premises-landschap is uitgebreid naar de Azure-assets van het abonnement. Met deze extensie, kunnen de virtuele machines deel uitmaken van de on-premises domein. Gebruikers van een domein van de on-premises domein toegang tot de servers en services kunnen worden uitgevoerd op deze VM's (zoals DBMS-services). On-premises communicatie en naamomzetting tussen VM's geïmplementeerd en virtuele machines die worden geïmplementeerd in Azure is mogelijk. In dit scenario is het meest voorkomende scenario voor het implementeren van SAP-assets op Azure. Zie voor meer informatie, [Planning en ontwerp voor VPN-gateway](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design).

> [!NOTE]
> Cross-Premises implementaties van SAP-systemen waarbij Azure Virtual Machines met SAP-systemen lid van een on-premises domein zijn worden ondersteund voor productie SAP-systemen. Cross-Premises configuraties worden ondersteund voor het implementeren van onderdelen of uitvoeren van SAP-landschappen in Azure. De volledige SAP-landschap dat zelfs uitvoeren in Azure vereist dat deze virtuele machines die deel uit van de on-premises domein en AD/LDAP. In vorige versies van de documentatie, hybride IT-scenario's zijn vermeld, waarin de term *hybride* verankerd ligt in het feit dat er een cross-premises-connectiviteit tussen on-premises en Azure. In dit geval *hybride* betekent ook dat de virtuele machines in Azure deel uit van de on-premises Active Directory maken.
> 
> 

Sommige Microsoft-documentatie worden Cross-Premises scenario's iets anders, met name voor DBMS HA configuraties beschreven. In het geval van de SAP-gerelateerde documenten, het Cross-Premises scenario komt meestal neer op een site-naar-site of particuliere [ExpressRoute](https://azure.microsoft.com/services/expressroute/) connectiviteit en met het feit dat de SAP-landschap dat wordt gedistribueerd tussen on-premises en Azure.

## <a name="resources"></a>Resources
De zijn uitgebracht voor verschillende artikelen op de werkbelasting van SAP op Azure.  De aanbeveling is om te beginnen [SAP-workloads op Azure - aan de slag](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) en kies vervolgens het gebied van belang

De volgende SAP-opmerkingen zijn gerelateerd aan SAP op Azure met betrekking tot het gebied in dit document behandeld:

| Houd er rekening mee getal | Titel |
| --- | --- |
| [1928533] |SAP-toepassingen op Azure: ondersteunde producten en Azure-VM-typen |
| [2015553] |SAP op Microsoft Azure: vereisten voor ondersteuning |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2178632] |Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [1409604] |Virtualisatie in Windows: uitgebreide bewaking |
| [2191498] |SAP op Linux met Azure: uitgebreide bewaking |
| [2039619] |SAP-toepassingen op Microsoft Azure met behulp van de Oracle-Database: ondersteunde producten en versies |
| [2233094] |DB6: SAP-toepassingen op Azure met behulp van IBM DB2 voor Linux, UNIX- en Windows - als u meer informatie |
| [2243692] |Linux op Microsoft Azure (IaaS) virtuele machine: problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [2002167] |Red Hat Enterprise Linux 7.x: installatie en Upgrade |
| [2069760] |Oracle Linux 7.x SAP-installatie en Upgrade |
| [1597355] |Wisselruimte aanbeveling voor Linux |
| [2171857] |Oracle Database 12c - ondersteuning van het bestandssysteem in Linux |
| [1114181] |Oracle Database 11g - ondersteuning van het bestandssysteem in Linux |


Lees ook de [SCN Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) die bevat alle SAP-opmerkingen voor Linux.

U hebt een praktische kennis over de Microsoft Azure-architectuur en hoe Microsoft Azure Virtual Machines zijn geïmplementeerd en worden beheerd. U vindt meer informatie op [Azure-documentatie](https://docs.microsoft.com/azure/).

Hoewel het bespreken van IaaS, zijn in het algemeen de installatie van Windows, Linux en DBMS-systemen en configuratie in feite hetzelfde als een virtuele machine of bare metal machine zou u on-premises installeert. Er zijn echter enkele architectuur en system management beslissingen voor de implementatie, die verschillen bij gebruik van Azure IaaS. Het doel van dit document is om uit te leggen van de specifieke architectuur en de system management verschillen die u moet worden voorbereid voor bij het gebruik van Azure IaaS.


## <a name="65fa79d6-a85f-47ee-890b-22e794f51a64"></a>Opslagstructuur van een virtuele machine voor RDBMS-implementaties
Als u wilt volgen dit hoofdstuk, is het nodig zijn om te begrijpen wat is opgegeven [dit] [ deployment-guide-3] hoofdstuk van de [Deployment Guide][deployment-guide]. Informatie over de verschillende VM-serie en de verschillen en de verschillen tussen Azure Standard en [Premium Storage](https://docs.microsoft.com/azure/virtual-machines/windows/premium-storage) moet worden begrepen en bekende voordat dit hoofdstuk wordt gelezen.

In termen van Azure Storage voor Azure-VM's moet u bekend bent met de artikelen:

- [Over schijfopslag voor Azure Windows VM 's](https://docs.microsoft.com/azure/virtual-machines/windows/about-disks-and-vhds)
- [Over de opslag van schijven voor virtuele Azure Linux-machines](https://docs.microsoft.com/azure/virtual-machines/linux/about-disks-and-vhds) 

In een eenvoudige configuratie, gewoonlijk het beste een structuur van waar het besturingssysteem, de DBMS-systemen en de uiteindelijke SAP-binaire bestanden staan los van de bestanden van de implementatie. Daarom raden we aan SAP-systemen die zijn uitgevoerd in Azure Virtual Machines hebben de base VHD (of de logische schijf), zoals geïnstalleerd met het besturingssysteem, de database management systeem uitvoerbare bestanden en de SAP-uitvoerbare bestanden. De DBMS-gegevens en logboekbestanden bestanden worden opgeslagen in Azure Storage (Standard of Premium Storage) in de afzonderlijke schijven en als logische schijven die zijn gekoppeld aan de oorspronkelijke Azure besturingssysteemkopie VM. Met name in Linux-implementaties, kunnen er verschillende aanbevelingen beschreven. Met name wat betreft de SAP HANA.  

Bij het plannen van de schijfindeling, moet u de beste balans tussen de volgende items zoeken:

* Het aantal gegevensbestanden.
* Het aantal schijven dat de bestanden bevatten.
* De quota IOPS van één schijf.
* De gegevensdoorvoer per schijf.
* Het aantal extra gegevensschijven mogelijk per VM-grootte.
* Een virtuele machine in de algemene opslagdoorvoer kan bieden.
* De latentie u een andere Azure Storage-typen krijgt.
* VM-Sla 's

Azure dwingt een quotum IOP's per gegevensschijf. Deze quota zijn verschillend voor schijven die worden gehost op Azure Standard-opslag- en Premium-opslag. I/o-latentie is ook verschil is tussen de twee soorten opslag.  Met Premium Storage levert factoren betere i/o-latentie. Elk van de verschillende typen van de virtuele machine heeft een beperkt aantal gegevensschijven dat u kunt koppelen. Een andere beperking is dat alleen bepaalde typen VM's kunnen gebruikmaken van Azure Premium Storage. Als gevolg hiervan kan de oplossing is voor een bepaald type van de virtuele machine niet alleen worden aangestuurd door de CPU en geheugen eisen, maar ook door de IOPS-waarde, latentie en schijf doorvoer vereisten die meestal worden geschaald met het aantal schijven of het type Premium Storage-schijven. Met name met Premium Storage kan de grootte van een schijf ook worden bepaald door het aantal IOPS en doorvoer die moet worden bereikt door elke schijf.

> [!NOTE]
> Het gebruik van Premium-opslag voor gegevens, transactielogboek of bestanden opnieuw wordt sterk aanbevolen voor DBMS-implementaties. Het maakt waardoor niet uit of u wilt implementeren voor productie- of niet-productiesystemen.

> [!NOTE]
> Als u wilt profiteren van Azure met de unieke [één VM SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/) alle schijven die zijn gekoppeld hoeft te worden van het type Azure Premium Storage, met inbegrip van de basis-VHD.
>

De plaatsing van bestanden van de database en logboekbestanden/opnieuw bestanden en het type Azure-opslag gebruikt, moeten worden gedefinieerd door de vereisten voor IOPS, latentie en doorvoer. Om voldoende IOP's, kan u worden afgedwongen gebruikmaken van meerdere schijven of een grotere Premium-opslag-schijf gebruiken. In het geval van meerdere schijven gebruikt, maakt u een streep software op de schijven die de bestanden bevatten of bestanden log/opnieuw. In dergelijke gevallen zijn de IOPS en de schijfdoorvoer Sla's van de onderliggende schijven voor Premium Storage of de maximale IOPS van Azure Standard Storage-schijven haalbare wél cumulatief voor de resulterende stripeset. 

Als de vereiste IOPS is groter dan wat één VHD kan bieden, als al is vermeld moet u het aantal IOP's die nodig zijn voor de databasebestanden naar een aantal VHD's in balans brengen. Eenvoudigste manier om te verdelen van de IOPS voor schijven is het bouwen van een streep software via de verschillende schijven. Vervolgens plaatst u een aantal gegevensbestanden van de SAP DBMS in de LUN's oppervlaktevariaties buiten de software-stripe. het aantal schijven in de streep wordt aangestuurd door IOPs eisen, schijf doorvoer eisen en volume eisen. 


- - -
> ![Windows][Logo_Windows] Windows
> 
> U wordt aangeraden Windows opslagruimten gebruiken om te maken van dergelijke stripesets over meerdere Azure-VHD's. Het verdient aanbeveling gebruik van ten minste Windows Server 2012 R2 of Windows Server 2016.
> 
> ![Linux][Logo_Linux] Linux
> 
> Alleen MDADM en LVM (Logical Volume Manager) worden ondersteund voor het bouwen van een software-RAID op Linux. Lees de volgende artikelen voor meer informatie:
> 
> - [Configureren van Software-RAID op Linux](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid) MDADM gebruiken
> - [LVM configureren op een Linux-VM in Azure](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm) LVM gebruiken
> 
> 

- - -
 
> [!NOTE]
> Omdat Azure Storage is het houden van drie kopieën van de VHD's, is het niet verstandig om te configureren van een redundantie wanneer striping. U hoeft alleen te configureren striping, dus dat de i/o's ophalen verdeeld over de verschillende VHD's.
>

### <a name="managed-or-non-managed-disks"></a>Beheerde of niet-beheerde schijven
Een Azure Storage-Account is niet alleen een met beheerdersrechten om voor te bereiden, maar ook een onderwerp met beperkingen. De beperkingen verschillen tussen Azure Standard Storage Accounst en Azure Premium Storage-Accounts. De exacte mogelijkheden en beperkingen worden vermeld in het artikel [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)

Voor Azure Standard-opslag, is het belangrijk om terug te halen over de IOP's per opslagaccount geldt een limiet (rij waarin **totale snelheid van aanvragen voor** in het artikel [Azure Storage Scalability and Performance Targets](https://docs.microsoft.com/azure/storage/common/storage-scalability-targets)). Er is bovendien een initiële limiet van het aantal Opslagaccounts per Azure-abonnement. Daarom moet u het saldo van VHD's voor grotere SAP-landschap tussen verschillende opslagaccounts om te voorkomen dat de grenzen van deze opslagaccounts te maken. Een tijdrovende werk wanneer u wordt gesproken over enkele honderden virtuele machines met meer dan duizend VHD's. 

Aangezien dit niet aangeraden wordt Azure Standard-opslag gebruiken voor implementaties in combinatie met de werkbelasting SAP DBMS-systemen, verwijzingen en aanbevelingen voor Azure Standard-opslag zijn beperkt tot dit korte [artikel](https://blogs.msdn.com/b/mast/archive/2014/10/14/configuring-azure-virtual-machines-for-optimal-storage-performance.aspx)

Microsoft geïntroduceerd om te voorkomen dat de beheerwerkzaamheden van plannen en implementeren van VHD's in verschillende Azure Storage-accounts, wat heet [Managed Disks](https://azure.microsoft.com/services/managed-disks/) in 2017. Beheerde schijven zijn beschikbaar voor Azure Standard-opslag, evenals Azure Premium Storage. De belangrijkste voordelen van Managed Disks in vergelijking met de lijst met niet-beheerde schijven, zoals:

- Voor Managed Disks, Azure verdeeld over de verschillende VHD's verschillende opslagaccounts automatisch tijdens de implementatie en daardoor vermijdt u de grenzen van een Azure Storage-account in termen van gegevensvolume, i/o-doorvoer en IOPS te maken.
- Met Managed Disks, Azure Storage is naleven van de concepten van Beschikbaarheidssets van Azure en met die wordt geïmplementeerd voor de basis-VHD en gekoppelde schijf van een virtuele machine in verschillende fout- en updatedomeinen als de virtuele machine deel van een Azure-Beschikbaarheidsset uitmaakt.


> [!IMPORTANT]
> De voordelen van Azure Managed Disks worden gegeven, is het raadzaam gebruik van Azure Managed Disks voor uw DBMS-implementaties en de SAP-implementaties in het algemeen.
>

Als u wilt converteren van niet-beheerde naar managed disks, raadpleegt u de artikelen:

- [Een Windows virtuele machine van niet-beheerde schijven converteren naar managed disks](https://docs.microsoft.com/azure/virtual-machines/windows/convert-unmanaged-to-managed-disks)
- [Een virtuele Linux-machine van niet-beheerde schijven converteren naar managed disks](https://docs.microsoft.com/azure/virtual-machines/linux/convert-unmanaged-to-managed-disks)


### <a name="c7abf1f0-c927-4a7c-9c1d-c7b5b3b7212f"></a>In cache opslaan voor virtuele machines en de gegevensschijven
Wanneer u schijven voor virtuele machines koppelt, kunt u kiezen of het i/o-verkeer tussen de virtuele machine en deze schijven zich bevinden in Azure-opslag in cache zijn opgeslagen. Azure Standard en Premium Storage gebruik van twee verschillende technologieën voor dit type cache. 

De onderstaande aanbevelingen zijn voor Standard DBMS ervan uitgaande dat deze i/o-kenmerken:

- Deze wordt voornamelijk werkbelasting voor de gegevensbestanden van een database gelezen. Deze leesbewerkingen zijn de prestaties van cruciaal belang voor het systeem DBMS-systemen
- Schrijven op basis van de gegevensbestanden in pieken op basis van controlepunten of een constante stroom optreedt. Niettemin, gemiddeld genomen over de dag, de schrijfbewerkingen zijn lager is dan de leesbewerkingen. In in leesbewerkingen van gegevensbestanden, deze schrijfbewerkingen zijn asynchroon en zijn niet in het bezit van gebruikerstransacties die.
- Er zijn nauwelijks leesbewerkingen van de transactie logboek of opnieuw bestanden. Uitzonderingen zijn grote i/o's bij het uitvoeren van transactielogboekback-ups. 
- Belangrijkste transactie of opnieuw logboekbestanden wordt belast is geschreven. Afhankelijk van de aard van de werkbelasting, kunt u i/o's hebben zo klein is als 4 KB of in andere gevallen i/o-grootte van 1 MB of meer.
- Alle schrijfbewerkingen moeten worden opgeslagen op schijf op een betrouwbare manier

Voor Azure Standard-opslag zijn de mogelijke cache-typen:

* Geen
* Lezen
* Lezen/schrijven

Om consistente en deterministisch prestaties, moet u instellen de cache op Azure Standard-opslag voor alle schijven met **DBMS-gerelateerde gegevensbestanden, bestanden log/opnieuw en ruimte voor tabel 'NONE'**. Het in cache plaatsen van de base VHD kan blijven met de standaardinstellingen.

Azure Premium Storage bestaat de volgende opties voor opslaan in cache:

* Geen
* Lezen 
* Lezen/schrijven 
* Geen en Write Accelerator (alleen voor Azure VM's uit de M-serie)
* Lezen + Write Accelerator (alleen voor Azure VM's uit de M-serie)

Aanbeveling voor Azure Premium Storage is om gebruik te **Lees-caching voor gegevensbestanden** van de SAP-database en vervolgens **niet in cache opslaan voor de schijven van een of meer logboekbestanden**.

Voor implementaties van de M-serie, is het raadzaam gebruik van Azure Write Accelerator voor uw implementatie DBMS-systemen. Raadpleeg het document voor meer informatie, beperkingen en implementatie van Azure Write Accelerator [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator). 


### <a name="azure-non-persistent-disks"></a>Niet-permanente schijven van Azure
Azure-VM's bieden een niet-permanente schijven na de implementatie van een virtuele machine. In het geval van een VM opnieuw wordt opgestart, wordt alle inhoud op deze schijven worden gewist. Het is dus een gezien het feit dat de gegevensbestanden en log/opnieuw bestanden van databases onder geen enkele omstandigheid zich op deze schijven niet-persistente bevinden moeten. Mogelijk zijn er uitzonderingen voor enkele van de databases, waar deze stations niet-persistente mogelijk geschikt voor tempdb en tijdelijke tabelruimten. Echter Vermijd het gebruik van deze schijven voor virtuele machines uit de A-serie, omdat deze niet-persistente schijven zijn beperkt in doorvoer met deze VM-reeks. Lees voor meer informatie het artikel [inzicht krijgen in het tijdelijke station op Windows Azure Virtual Machines](https://blogs.msdn.microsoft.com/mast/2013/12/06/understanding-the-temporary-drive-on-windows-azure-virtual-machines/)

- - -
> ![Windows][Logo_Windows] Windows
> 
> Station D:\ in een Azure VM is een niet-persistente-station, die wordt ondersteund door sommige lokale schijven op de Azure compute-knooppunt. Omdat het niet-persistente, betekent dit dat alle wijzigingen aan de inhoud op het station D:\ gaat verloren wanneer de virtuele machine opnieuw wordt opgestart. "De wijzigingen', zoals bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> ![Linux][Logo_Linux] Linux
> 
> Linux Azure-VM's automatisch een station koppelen aan /mnt/resource die is een niet-persistente station ondersteund door lokale schijven op de Azure compute-knooppunt. Omdat het niet-persistente, betekent dit dat alle wijzigingen aan de inhoud in /mnt/resource gaan verloren wanneer de virtuele machine opnieuw wordt opgestart. Door wijzigingen, zoals bestanden, mappen die zijn gemaakt, toepassingen die zijn geïnstalleerd, enzovoort.
> 
> 

- - -



### <a name="10b041ef-c177-498a-93ed-44b3441ab152"></a>Microsoft Azure Storage tolerantie
Microsoft Azure Storage slaat de basis-VHD (met OS) en de gekoppelde schijven of de BLOBs op ten minste drie afzonderlijke opslagknooppunten. Dit wordt lokaal redundante opslag (LRS) genoemd. LRS is de standaardwaarde voor alle typen opslag in Azure. 

Er zijn verschillende meer redundantie methoden, die alle beschreven in het artikel worden [Azure Storage-replicatie](https://docs.microsoft.com/azure/storage/common/storage-redundancy?toc=%2fazure%2fstorage%2fqueues%2ftoc.json).

> [!NOTE]
>Vanaf Azure Premium Storage, het aanbevolen type opslag voor DBMS-VM's en schijven die de database en logboekbestanden/opnieuw bestanden worden opgeslagen is, is de enige beschikbare methode LRS. Als gevolg hiervan moet u database-methoden, zoals SQL Server Always On, Oracle Data Guard of HANA System Replication om in te schakelen van databasereplicatie voor gegevens in een andere Azure-regio of een andere Azure-Beschikbaarheidszone configureren.


> [!NOTE]
> Het gebruik van geografisch redundante opslag met Azure Standard-opslag beschikbaar is voor DBMS-implementaties, niet aanbevolen omdat deze is van invloed op de ernstige prestaties en biedt niet voldoen aan de order schrijven in verschillende virtuele harde schijven die zijn gekoppeld aan een virtuele machine. Het feit dat de volgorde schrijven niet naleven via verschillende VHD's draagt een grote kans om te beëindigen in inconsistente databases aan de replicatie als de database en logboekbestanden/opnieuw worden verdeeld over meerdere VHD's (zoals meestal het geval) op de bron-VM aan clientzijde.

 

## <a name="vm-node-resiliency"></a>Knooppunt-VM-tolerantie
Het Azure-Platform biedt diverse verschillende Sla's voor virtuele machines. De exacte details vindt u in de meest recente versie van [SLA voor virtuele Machines](https://azure.microsoft.com/support/legal/sla/virtual-machines/v1_8/). Aangezien de DBMS-laag meestal een belangrijk onderdeel van de beschikbaarheid van een SAP-systeem is, moet u uzelf bekend zijn met de concepten van Beschikbaarheidssets, Beschikbaarheidszones en onderhoud voor maken. De artikelen waarin al deze concepten beschreven is [de beschikbaarheid van Windows virtuele machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/windows/manage-availability) en [de beschikbaarheid van virtuele Linux-machines in Azure beheren](https://docs.microsoft.com/azure/virtual-machines/linux/manage-availability).  

Minimale aanbeveling voor productie DBMS-scenario's met SAP-werkbelasting is:

- Implementeer twee virtuele machines in een afzonderlijke Beschikbaarheidsset in dezelfde Azure-regio.
- Deze twee VM's wordt uitgevoerd in hetzelfde Azure-VNet en zou hebben NIC's gekoppeld naar de dezelfde subnetten.
- Database-methoden gebruiken om een hot stand-by met de tweede virtuele machine. Methoden kunnen bestaan uit SQL Server Always On, Oracle Data Guard of HANA System Replication.

U kunt bovendien implementeren van een derde VM in een andere Azure-regio en de dezelfde database-methoden gebruiken om op te geven van een asynchrone replica in een andere Azure-regio.

De manier voor het instellen van Beschikbaarheidssets van Azure wordt geïllustreerd in deze [zelfstudie](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets).



## <a name="azure-network-considerations"></a>Azure aandachtspunten voor netwerken 
Bij grootschalige implementaties voor SAP, aanbeveling is dat u van de blauwdruk van gebruikmaakt [Azure Virtual Datacenter](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) voor de VNet-configuratie en machtigingen en rollen toewijzingen met verschillende onderdelen van hun organisatie.

Er zijn enkele aanbevolen procedures, heeft geleid uit honderden implementaties van klanten tot:

- De vnet (s) de SAP-toepassing wordt geïmplementeerd in, heeft geen toegang tot Internet.
- De virtuele machines van de database worden uitgevoerd in hetzelfde VNet als de toepassingslaag.
- De virtuele machines binnen het VNet hebben een statische toewijzing van het particuliere IP-adres. Zie het artikel [IP-adres adrestypen en toewijzingsmethoden in Azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm) als verwijzing.
- Routering beperkingen naar en van de DBMS-VM's zijn **niet** instellen met een firewall is geïnstalleerd op de lokale DBMS-VM's. Routering van verkeer in plaats daarvan is gedefinieerd met [Azure Network Security groepen (NSG)](https://docs.microsoft.com/azure/virtual-network/security-overview)
- Ten behoeve van elkaar te scheiden en te isoleren van verkeer voor de DBMS-VM, kunt u verschillende NIC's toewijzen aan de virtuele machine. Waar elke NIC heeft een ander IP-adres en elke NIC is een toegewezen aan een ander VNet-subnet is die verschillende NSG-regels. Houd er rekening mee dat de isolatie- of scheiding van netwerkverkeer zojuist een meting voor routering is en is niet toegestaan voor het instellen van quota voor de netwerkdoorvoer.

> [!NOTE]
> Moet u statische IP-adressen via Azure middelen toewijzen aan afzonderlijke vnic's. U moet statische IP-adressen binnen het gastbesturingssysteem niet toewijzen aan een vNIC. Sommige Azure-services zoals Azure Backup-Service is afhankelijk van het feit dat op minimaal de primaire vNIC is ingesteld op DHCP- en niet op de vaste IP-adressen. Zie ook het document [los problemen met Azure virtuele machine back-up](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Als u meerdere statische IP-adressen toewijzen aan een virtuele machine wilt, moet u meerdere vnic's toewijzen aan een virtuele machine.
>
>

Met behulp van twee virtuele machines voor uw productie-implementatie van de DBMS-systemen binnen een Azure-Beschikbaarheidsset plus een afzonderlijke routering voor de SAP-toepassingslaag en het beheer en bewerkingen verkeer naar de twee DBMS-VM's, de ruwe diagram zou er als volgt uitzien:

![Diagram van twee virtuele machines in twee subnetten](./media/virtual-machines-shared-sap-deployment-guide/general_two_dbms_two_subnets.PNG)


### <a name="azure-load-balancer-for-redirecting-traffic"></a>Azure load balancer voor het omleiden van verkeer
Het gebruik van persoonlijke virtuele IP-adressen die worden gebruikt in functies ingebouwd, zoals SQL Server Always On of HANA System replication is vereist voor de configuratie van een Azure Load Balancer. De Azure Load Balancer kan via poorten van de test om te bepalen van het actieve knooppunt van de DBMS-systemen en het verkeer routeren uitsluitend naar dat knooppunt actieve database. In het geval van een failover van de database-knooppunt is er niet nodig voor de SAP-toepassing te configureren. In plaats daarvan wordt de meest voorkomende architecturen voor SAP-toepassingen opnieuw verbonden op basis van het persoonlijke virtuele IP-adres. In de tussentijd zorgen de Azure load balancer gereageerd op de failover-knooppunt door het verkeer op basis van het persoonlijke virtuele IP-adres wordt omgeleid naar het tweede knooppunt.

Azure biedt twee verschillende [load balancer-SKU's](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Een basis-SKU en een standaard-SKU. Tenzij u implementeren in Azure-Beschikbaarheidszones wilt, wordt de basisversie van load balancer SKU prima doet. 

Het verkeer tussen de DBMS-VM's en de SAP-toepassingslaag altijd doorgestuurd via de Azure load balancer altijd is? Het antwoord is afhankelijk van hoe u de load balancer configureren. Het binnenkomende verkeer naar de DBMS-VM wordt op dit moment, altijd worden gerouteerd via de Azure load balancer. Het uitgaande verkeer route van de DBMS-VM naar het niveau van de toepassing dat VM, is afhankelijk van de configuratie van de Azure load balancer. De load balancer biedt een optie van DirectServerReturn. Als deze optie is geconfigureerd, wordt het verkeer van de DBMS-VM wordt omgeleid naar de SAP-toepassingslaag wordt **niet** worden gerouteerd via de Azure load balancer. Het gaat in plaats daarvan rechtstreeks naar het niveau van de toepassing. Als DirectServerReturn niet is geconfigureerd, wordt het retourverkeer op het niveau van de SAP-toepassing doorgestuurd via de Azure load balancer

Het verdient aanbeveling DirectServerReturn configureren in combinatie met Azure load balancers die tussen de SAP-toepassingslaag en de DBMS-laag en verminder de netwerklatentie tussen de twee lagen zijn geplaatst

Een voorbeeld van het instellen van een dergelijke configuratie wordt gepubliceerd om SQL server altijd op in [in dit artikel](https://docs.microsoft.com/azure/virtual-machines/windows/sqlclassic/virtual-machines-windows-classic-ps-sql-int-listener).

Als u ervoor kiest gepubliceerde github JSON-sjablonen gebruikt als referentie voor de implementatie van uw SAP-infrastructuur in Azure, moet u dit onderzoek [sjabloon voor een SAP-systeem 3-Laagse](https://github.com/Azure/azure-quickstart-templates/tree/4099ad9bee183ed39b88c62cd33f517ae4e25669/sap-3-tier-marketplace-image-converged-md). In deze sjabloon kunt u ook de juiste instellingen van de Azure load balancer bestuderen.

### <a name="azure-accelerated-networking"></a>Azure versnelde netwerken
Om verder te beperken de netwerklatentie tussen virtuele Azure-machines, het is raadzaam de optie kiezen [Azure Accelerated Networking](https://azure.microsoft.com/blog/maximize-your-vm-s-performance-with-accelerated-networking-now-generally-available-for-both-windows-and-linux/) bij het implementeren van virtuele Azure-machines voor de werkbelasting van SAP. Met name voor de SAP-toepassingslaag en de SAP DBMS-laag. 

> [!NOTE]
> Niet alle VM-typen vindt u ondersteunende versnelde netwerken. Het artikel waarnaar wordt verwezen, is de VM-typen die ondersteuning bieden voor versnelde netwerken aanbieding. 
>  

- - -
> ![Windows][Logo_Windows] Windows
> 
> Voor Windows leest u het artikel [maken van een Windows-machine met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell) voor informatie over de concepten en de manier hoe u virtuele machines met versnelde netwerken te implementeren
> 
> ![Linux][Logo_Linux] Linux
> 
> Voor Linux Lees het artikel [maken van een virtuele Linux-machine met versnelde netwerken](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli) om details voor Linux-distributie. 
> 
> 

- - -

> [!NOTE]
> In het geval van SUSE, Red Hat en Oracle Linux wordt versnelde netwerken ondersteund met recente versies. Oudere versies, zoals SLES 12 SP2 of RHEL 7.2 worden niet ondersteund door Azure Accelerated Networking 
>  


## <a name="deployment-of-host-monitoring"></a>Implementatie van de Host controleren
SAP vereist voor het productiegebruik van SAP-toepassingen in Azure Virtual Machines, de mogelijkheid om op te halen van de host bewakingsgegevens van de fysieke hosts waarop de virtuele Machines van Azure wordt uitgevoerd. Er is een specifieke Host-Agent voor SAP-patchniveau vereist waarmee deze mogelijkheid in SAPOSCOL en SAP Host-Agent. De exacte patchniveau wordt beschreven in de SAP-notitie [1409604].

Raadpleeg voor de details met betrekking tot implementatie van onderdelen die hostgegevens aan SAPOSCOL en SAP-Hostagent leveren en het beheer van de levenscyclus van deze onderdelen, de [Implementatiehandleiding][deployment-guide]


## <a name="next-steps"></a>Volgende stappen
Voor documentatie over bepaalde DBMS-systemen, moet u deze artikelen:

- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SQL Server](dbms_guide_sqlserver.md)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor Oracle](dbms_guide_oracle.md)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor IBM DB2](dbms_guide_ibm.md)
- [DBMS-implementatie voor SAP-werkbelasting in virtuele Azure-machines voor SAP ASE](dbms_guide_sapase.md)
- [SAP maxDB, Live Cache en implementatie van de Server voor webinhoud op Azure](dbms_guide_maxdb.md)
- [Bedieningsgids voor SAP HANA op Azure](hana-vm-operations.md)
- [SAP HANA met hoge beschikbaarheid voor Azure virtual machines](sap-hana-availability-overview.md)
- [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](sap-hana-backup-guide.md)

