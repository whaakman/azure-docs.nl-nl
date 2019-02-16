---
title: 'Quickstart: Handmatige installatie van één exemplaar SAP HANA op Azure Virtual Machines | Microsoft Docs'
description: Snelstartgids voor handmatige installatie van één exemplaar SAP HANA op Azure Virtual Machines
services: virtual-machines-linux
documentationcenter: ''
author: hermanndms
manager: jeconnoc
editor: ''
tags: azure-resource-manager
keywords: ''
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/06/2018
ms.author: hermannd
ms.openlocfilehash: 561eff75ef4268acd3f737f7aaa92ccaacfda7f3
ms.sourcegitcommit: d2329d88f5ecabbe3e6da8a820faba9b26cb8a02
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/16/2019
ms.locfileid: "56328710"
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Quickstart: Handmatige installatie van één exemplaar SAP HANA op Azure Virtual machines
## <a name="introduction"></a>Inleiding
Deze handleiding helpt u bij het instellen van een single instance SAP HANA op Azure virtual machines (VM's) wanneer u 7.5 van SAP NetWeaver en SAP HANA 1.0 SP12 handmatig installeren. De focus van deze handleiding is over het implementeren van SAP HANA op Azure. Er is geen vervanging voor SAP-documentatie. 

>[!Note]
>Deze handleiding beschrijft de implementatie van SAP HANA in virtuele Azure-machines. Zie voor meer informatie over het implementeren van SAP HANA in HANA grote instanties [SAP op Azure virtual machines (VM's) met behulp van](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Vereisten
Deze handleiding wordt ervan uitgegaan dat u bekend met dergelijke infrastructuur als de basisbeginselen van een service (IaaS) als bent:
 * Klik hier voor meer informatie over het implementeren van virtuele machines of virtuele netwerken via de Azure portal of PowerShell.
 * De Azure platformoverschrijdende opdrachtregelinterface (CLI), met inbegrip van de optie voor het gebruik van sjablonen voor JavaScript Object Notation (JSON).

Deze handleiding wordt ook van uitgegaan dat u bekend met bent:
* SAP HANA en SAP NetWeaver en hoe u on-premises installeert.
* Installeren en operationele SAP HANA en instanties van SAP-toepassingen op Azure.
* De volgende concepten en procedures:
   * Planning voor SAP-implementatie op Azure, waaronder Azure Virtual Network planning en het gebruik van Azure Storage. Zie [SAP NetWeaver op Azure Virtual Machines (VM's) - plannings- en implementatie voor](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * Principes van de implementatie en manieren voor het implementeren van virtuele machines in Azure. Zie [virtuele Machines van Azure-implementatie voor SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Hoge beschikbaarheid voor SAP NetWeaver ASCS (ABAP SAP Central Services), SCS (SAP Central Services) en INGEN (Server in de wachtrij plaatsen replicatie) op Azure. Zie [hoge beschikbaarheid voor SAP NetWeaver op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Als u meer informatie over het verbeteren van de efficiëntie in gebruik te maken van een multi-SID-installatie van ASCS/SCS op Azure. Zie [maken van een multi-SID-configuratie van SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Principes van het uitvoeren van SAP NetWeaver gebaseerd op Linux gebaseerde virtuele machines in Azure. Zie [met SAP NetWeaver op Microsoft Azure SUSE Linux VM's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Deze handleiding bevat specifieke instellingen voor Linux in Azure VM's en meer informatie over het correct Azure storage-schijven koppelen aan virtuele Linux-machines.

De typen Azure VM's die kunnen worden gebruikt voor productiescenario's worden vermeld in de [SAP-documentatie voor IAAS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor niet-productie scenario's is een groter aantal systeemeigen Azure-VM-typen beschikbaar.
Raadpleeg het document voor meer informatie over de virtuele machine configuratie en bewerkingen [configuraties van SAP HANA-infrastructuur en bewerkingen op Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/hana-vm-operations).
Zie voor hoge beschikbaarheid van SAP HANA, [SAP HANA met hoge beschikbaarheid voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview).

Als u op zoek zijn naar een exemplaar van SAP HANA of S/4HANA of BW/4HANA system geïmplementeerd in de zeer snel tijd ophalen, kunt u overwegen het gebruik van [SAP Cloud Appliance Library](http://cal.sap.com). Documentatie over het implementeren, bijvoorbeeld van een S/4HANA-systeem via SAP CAL op Azure in [in deze handleiding](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Alles wat u nodig hebt is een Azure-abonnement en een SAP-gebruiker die kan worden geregistreerd met SAP Cloud Appliance Library.

## <a name="additional-resources"></a>Aanvullende resources
### <a name="sap-hana-backup"></a>Back-up van SAP HANA
Zie voor informatie over back-ups van SAP HANA op Azure Virtual machines:
* [Back-uphandleiding voor SAP HANA op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [SAP HANA Azure back-up op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [SAP HANA-back-up op basis van opslagmomentopnamen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>SAP Cloud Appliance Library
Zie voor meer informatie over het gebruik van SAP Cloud Appliance Library voor het implementeren van S/4HANA, BW/4HANA [implementeren SAP S/4HANA of BW/4HANA op Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA ondersteunde besturingssystemen.
Zie voor informatie over SAP HANA ondersteunde besturingssystemen, [SAP Support Opmerking #2235581 - SAP HANA: Ondersteunde besturingssystemen](https://launchpad.support.sap.com/#/notes/2235581/E). Azure-VM's ondersteunen slechts een subset van deze besturingssystemen. De volgende besturingssystemen worden ondersteund voor het implementeren van SAP HANA op Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Zie voor aanvullende SAP-documentatie over SAP HANA en andere Linux-besturingssystemen:

* [SAP-ondersteuning Opmerking #171356 - SAP-Software op Linux:  Algemene informatie](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP-ondersteuning Opmerking #1944799 - SAP HANA-richtlijnen voor de installatie van de SLES-besturingssysteem](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [SAP Support Opmerking #2205917 - SAP HANA DB aanbevolen besturingssysteeminstellingen voor het voor SLES 12 voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP Support Note #1984787 - SUSE Linux Enterprise Server 12:  Opmerkingen bij de installatie](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP Support Opmerking #1391070 - UUID van de Linux-oplossingen](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP-ondersteuning Opmerking #2009879 - SAP HANA-richtlijnen voor het Red Hat Enterprise Linux (RHEL)-besturingssysteem](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA DB: Aanbevolen besturingssysteeminstellingen voor RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP bewaking in Azure
Zie voor informatie over SAP bewaking in Azure:

* [SAP-notitie 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Deze opmerking bespreekt SAP "uitgebreide bewaking" met virtuele Linux-machines op Azure. 
* [SAP-notitie 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Deze opmerking vindt u informatie over SAPOSCOL op Linux. 
* [SAP-notitie 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Deze opmerking bespreekt bewaking metrische sleutelgegevens voor SAP op Microsoft Azure.

### <a name="azure-vm-types"></a>Azure VM-typen
Azure VM-typen en gebruikt in combinatie met SAP HANA-ondersteund SAP workloadscenario's worden beschreven in [SAP gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

Azure VM-typen die zijn gecertificeerd door SAP voor SAP NetWeaver of de toepassingslaag S/4HANA zijn gedocumenteerd in [SAP-notitie 1928533 - SAP-toepassingen op Azure: Ondersteunde producten en typen Azure VM's](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>SAP-Linux-Azure-integratie wordt alleen ondersteund op Azure Resource Manager en niet het klassieke implementatiemodel. 

## <a name="manual-installation-of-sap-hana"></a>Handmatige installatie van SAP HANA

> [!IMPORTANT]
> Zorg ervoor dat het besturingssysteem die u selecteert SAP gecertificeerd voor SAP HANA op de specifieke VM-typen die u gebruikt. De lijst met SAP HANA-gecertificeerde VM-typen en OS-versies voor die kunnen worden opgezocht [SAP HANA gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Zorg ervoor dat u klikt u op de details van het VM-type weergegeven als u de volledige lijst van SAP HANA ondersteunde OS releases voor het specifieke VM-type. Houd er rekening mee dat in het voorbeeld in dit document hebben we een SLES-OS-versie die niet wordt ondersteund door SAP voor SAP HANA op virtuele machines uit de M-serie gebruikt.
>

Deze handleiding wordt beschreven hoe u handmatig installeren van SAP HANA op Azure Virtual machines in twee verschillende manieren:

* Met behulp van SAP Software inrichting Manager (SWPM) als onderdeel van een gedistribueerde installatie van de NetWeaver in de stap "database-exemplaar installeren"
* Database lifecycle manager hulpprogramma, HDBLCM, en vervolgens NetWeaver te installeren met behulp van de SAP HANA

U kunt ook SWPM gebruiken voor het installeren van alle onderdelen (SAP HANA, SAP-toepassingsserver en de ASCS-instantie) in één enkele virtuele machine, zoals beschreven in dit [SAP HANA-blogbericht](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Deze optie wordt niet beschreven in deze snelstartgids, maar de problemen die u in aanmerking moet nemen zijn hetzelfde.

Voordat u een installatie start, raden we aan dat u leest de ' voorbereiden Azure VM's voor handmatige installatie van SAP HANA ' verderop in deze handleiding. In dat geval, kunt u voorkomen dat op verschillende algemene fouten die optreden mogelijk wanneer u alleen een standaard Azure-VM-configuratie gebruikt.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Belangrijke stappen voor de installatie van SAP HANA wanneer u SAP SWPM
Deze sectie vindt u de belangrijkste stappen voor de installatie van een handmatige, één exemplaar SAP HANA wanneer u SAP SWPM gebruikt om een gedistribueerd SAP NetWeaver 7.5-installatie te voltooien. De afzonderlijke stappen worden in meer detail in schermopnamen verderop in deze handleiding beschreven.

1. Maak een Azure-netwerk met twee test-VM's.
2. De twee Azure-VM's met besturingssystemen (in ons voorbeeld, SUSE Linux Enterprise Server (SLES) en SLES voor SAP-toepassingen 12 SP1), op basis van het Azure Resource Manager-model implementeren.
3. Twee Azure standard of premium-opslagschijven (bijvoorbeeld: 75 GB of 500 GB schijven) koppelen aan de VM-toepassingsserver.
4. Premium-opslagschijven koppelen aan de HANA DB-server virtuele machine. Zie de sectie 'Schijf instellen' verderop in deze handleiding voor meer informatie.
5. Meerdere schijven, afhankelijk van de vereisten voor de grootte of doorvoer, en vervolgens striped volumes maken met behulp van volumebeheer van logische of een beheerprogramma voor meerdere apparaten (MDADM) op de OS-niveau binnen de virtuele machine.
6. XFS bestandssystemen op de gekoppelde schijven of logische volumes maken.
7. Koppel de nieuwe XFS-bestandssystemen op het niveau van het besturingssysteem. Gebruik een bestandssysteem voor alle SAP-software. Het bestandssysteem voor de /sapmnt directory en de back-ups, bijvoorbeeld gebruiken. Koppel de XFS-bestandssystemen op de premium-opslagschijven als /hana en /usr/sap op de SAP HANA DB-server. Dit proces is nodig om te voorkomen dat de basis-bestandssysteem niet groot op Linux Azure Virtual machines, wordt ingenomen.
8. Voer in het lokale IP-adressen van de test-VM's in het bestand/etc/hosts.
9. Voer de **nofail** parameter in het bestand/etc/fstab.
10. Linux-kernel op basis van de Linux OS-versie die u instellen. Zie voor meer informatie, de juiste SAP-opmerkingen over HANA en de sectie 'Kernel parameters' in deze handleiding.
11. Voeg wisselruimte toe.
12. (Optioneel) een grafische desktop installeren op de test-VM's. Gebruik anders een installatie op afstand SAPinst.
13. Download de SAP-software van de SAP Service Marketplace.
14. Installeer de SAP ASCS-exemplaar op de virtuele machine van de appserver.
15. Deel de map /sapmnt tussen de test-VM's met behulp van NFS. De VM-toepassingsserver is de NFS-server.
16. De database-instantie, met inbegrip van HANA, met behulp van SWPM op de server DB VM installeren.
17. Installeer de primaire toepassingsserver (Pa's) op de VM-toepassingsserver.
18. Start de SAP-beheerconsole (SAP MC). Verbinden met SAP-GUI of HANA Studio, bijvoorbeeld.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Belangrijke stappen voor de installatie van SAP HANA wanneer u HDBLCM
Deze sectie vindt u de belangrijkste stappen voor de installatie van een handmatige, één exemplaar SAP HANA wanneer u SAP HDBLCM gebruikt om een gedistribueerd SAP NetWeaver 7.5-installatie te voltooien. De afzonderlijke stappen worden beschreven in de schermafbeeldingen in deze handleiding in meer detail.

1. Maak een Azure-netwerk met twee test-VM's.
2. Implementeer twee Azure-VM's met besturingssystemen (in ons voorbeeld, SLES- en SLES voor SAP-toepassingen 12 SP1) op basis van het Azure Resource Manager-model.
3. Twee Azure standard of premium-opslagschijven (bijvoorbeeld: 75 GB of 500 GB schijven) koppelen aan de virtuele machine van de app-server.
4. Premium-opslagschijven koppelen aan de HANA DB-server virtuele machine. Zie de sectie 'Schijf instellen' verderop in deze handleiding voor meer informatie.
5. Afhankelijk van de vereisten voor de grootte of doorvoer, meerdere schijven en striped volumes maken met behulp van volumebeheer van logische of een beheerprogramma voor meerdere apparaten (MDADM) op de OS-niveau binnen de virtuele machine.
6. XFS bestandssystemen op de gekoppelde schijven of logische volumes maken.
7. Koppel de nieuwe XFS-bestandssystemen op het niveau van het besturingssysteem. Gebruik van een bestandssysteem voor alle SAP-software en de andere is voor de /sapmnt map en de back-ups, bijvoorbeeld gebruiken. Koppel de XFS-bestandssystemen op de premium-opslagschijven als /hana en /usr/sap op de SAP HANA DB-server. Dit proces is nodig om te voorkomen dat de basis-bestandssysteem niet groot op Linux Azure Virtual machines, wordt ingenomen.
8. Voer in het lokale IP-adressen van de test-VM's in het bestand/etc/hosts.
9. Voer de **nofail** parameter in het bestand/etc/fstab.
10. Kernel instellen op basis van de release van de Linux-besturingssysteem die u gebruikt. Zie voor meer informatie, de juiste SAP-opmerkingen over HANA en de sectie 'Kernel parameters' in deze handleiding.
11. Voeg wisselruimte toe.
12. (Optioneel) een grafische desktop installeren op de test-VM's. Gebruik anders een installatie op afstand SAPinst.
13. Download de SAP-software van de SAP Service Marketplace.
14. Maak een groep, sapsys, groep-ID 1001, op de virtuele machine van de HANA DB-server.
15. SAP HANA op de server DB VM installeren met behulp van HANA Database Lifecycle Manager (HDBLCM).
16. Installeer de SAP ASCS-exemplaar op de virtuele machine van de appserver.
17. Deel de map /sapmnt tussen de test-VM's met behulp van NFS. De VM-toepassingsserver is de NFS-server.
18. De database-instantie, met inbegrip van HANA, met behulp van SWPM op de server HANA DB VM installeren.
19. Installeer de primaire toepassingsserver (Pa's) op de VM-toepassingsserver.
20. SAP MC starten. Verbinding maken via de SAP-GUI of HANA Studio.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Virtuele Azure-machines voorbereiden voor een handmatige installatie van SAP HANA
In deze sectie worden de volgende onderwerpen:

* Updates van het besturingssysteem
* Installatie van de schijf
* Kernel-parameters
* Bestandssystemen
* Het bestand/etc/hosts
* Het bestand/etc/fstab

### <a name="os-updates"></a>Updates van het besturingssysteem
Controleren op updates van het Linux-besturingssysteem en oplossingen voordat u extra software installeert. Als u een patch installeert, is het mogelijk om te voorkomen dat een aanroep naar de helpdesk.

Zorg ervoor dat u gebruikmaakt van:
* SUSE Linux Enterprise Server voor SAP-toepassingen.
* Red Hat Enterprise Linux voor SAP-toepassingen of Red Hat Enterprise Linux voor SAP HANA. 

Als u niet hebt gedaan, moet u de implementatie van het besturingssysteem registreren bij uw Linux-abonnement van de Linux-leverancier. Houd er rekening mee dat SUSE heeft installatiekopieën van het besturingssysteem voor SAP-toepassingen die al services bevatten en die automatisch worden geregistreerd.

Hier volgt een voorbeeld van het controleren op beschikbare patches van SUSE Linux met behulp van de **zypper** opdracht:

 `sudo zypper list-patches`

Afhankelijk van het soort probleem, patches ingedeeld op categorie en ernst. Meest gebruikte waarden voor de categorie zijn: **security**, **aanbevolen**, **optionele**, **functie**, **document**, of **yast**.
Meest gebruikte waarden voor incidenten met ernst zijn: **kritieke**, **belangrijk**, **gemiddeld**, **lage**, of **nietopgegeven**.

De **zypper** opdracht zoekt alleen naar de updates die de geïnstalleerde pakketten nodig hebt. U kunt bijvoorbeeld deze opdracht gebruiken:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

U kunt de parameter toevoegen `--dry-run` voor het testen van de update zonder daadwerkelijk bijwerken van het systeem.


### <a name="disk-setup"></a>Installatie van de schijf
Het root-bestandssysteem op een Linux-VM in Azure heeft een maximale grootte. Daarom is het nodig extra schijfruimte koppelen aan een Azure-VM voor het uitvoeren van SAP. Voor SAP-toepassingsserver virtuele Azure-machines, kan het gebruik van Azure standard-opslagschijven voldoende zijn. Echter, is het gebruik van Azure Premium Storage-schijven voor productie- en niet-productie-implementaties voor SAP HANA DBMS-systemen Azure VM's, verplicht.

Op basis van de [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), de volgende Azure Premium Storage-configuratie wordt aangeraden: 

| VM-SKU | RAM |  / hana/gegevens en/hana/log <br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

In de configuratie van de voorgestelde schijven, worden de HANA-gegevensvolume en logboekvolume op dezelfde set Azure premium storage-schijven striped met LVM of MDADM geplaatst. Het is niet nodig voor het definiëren van een RAID-niveau voor redundantie omdat Azure Premium Storage drie kopieën van de schijven voor de redundantie van opgeslagen. Om ervoor te zorgen dat u voldoende opslagruimte configureren, raadpleegt u de [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) en [SAP HANA-Server-installatie- en Updatehandleiding](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Ook rekening houden met de volumes van de doorvoer van verschillende virtuele harde schijf (VHD) van de verschillende Azure premium storage-schijven zoals beschreven in [High-performance Premium Storage en beheerde schijven voor virtuele machines](../../windows/disks-types.md). 

U kunt meer premium storage-schijven toevoegen aan de HANA DBMS-VM's voor het opslaan van de database die of transactielogboek logboekback-ups.

Zie de volgende artikelen voor meer informatie over de twee belangrijkste hulpprogramma's gebruikt voor het configureren van striping:

* [Software-RAID op Linux configureren](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [LVM configureren op een virtuele Linux-machine in Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zie voor meer informatie over het koppelen van schijven met virtuele Azure-machines waarop Linux wordt uitgevoerd als een gastbesturingssysteem [een schijf toevoegen aan een Linux-VM](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure premium SSD's kunt u definiëren schijfcachingtype modi. Voor het bedrijf /hana/data en /hana/log striped set moet opslaan in schijfcache worden uitgeschakeld. Voor de andere volumes (schijven), de modus voor opslaan in cache moet worden ingesteld op **ReadOnly**.

Als u voorbeeld-JSON-sjablonen voor het maken van virtuele machines zoekt, gaat u naar [Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates).
De vm-eenvoudig-sles-sjabloon is een eenvoudige sjabloon. Het bevat een sectie voor opslag, met een schijf van 100 GB gegevens. Deze sjabloon kan worden gebruikt als basis. U kunt de sjabloon aanpassen aan uw specifieke configuratie.

>[!Note]
>Het is belangrijk om te koppelen van de Azure-opslag-schijf met behulp van een UUID zoals beschreven in [SAP NetWeaver uitvoeren op Microsoft Azure SUSE Linux VM's](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Twee Azure standard storage-schijven zijn gekoppeld aan de SAP-appserver VM, in de testomgeving, zoals wordt weergegeven in de volgende schermafbeelding. Een schijf opgeslagen alle van de SAP-software (inclusief NetWeaver 7.5, SAP-GUI en SAP HANA) voor de installatie. De tweede schijf ervoor gezorgd dat er voldoende vrije schijfruimte beschikbaar voor aanvullende vereisten (bijvoorbeeld een back-up en test gegevens) en voor de map /sapmnt (dat wil zeggen, SAP-profielen) moeten worden gedeeld met alle virtuele machines die deel uitmaken van de SAP-landschap dat dezelfde zou zijn.

![SAP HANA-app server schijven venster met twee gegevensschijven en de grootte](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Kernel-parameters
SAP HANA vereist specifieke Linux-kernel instellingen, die geen deel uit van de standaard Azure-galerie met installatiekopieën en moeten handmatig worden ingesteld. Afhankelijk van of u SUSE of Red Hat gebruiken, kunnen de parameters afwijken. Gegevens over deze parameters doorgeven aan de eerder vermelde opmerkingen bij de SAP. In de schermafbeeldingen wordt weergegeven, is SUSE Linux 12 SP1 gebruikt. 

SLES voor SAP-toepassingen 12 GA- en SLES voor SAP-toepassingen 12 SP1 hebt u een nieuw hulpprogramma **afgestemd op de adm**, vervangt de oude **sapconf** hulpprogramma. Een speciaal profiel van de SAP HANA is beschikbaar voor **afgestemd op de adm**. Als u wilt het systeem af te stemmen voor SAP HANA, voert u de volgende als een hoofdgebruiker zijn:

   `tuned-adm profile sap-hana`

Voor meer informatie over **afgestemd op de adm**, Zie de [SUSE-documentatie over afgestemd op de adm](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

In de volgende schermafbeelding ziet u hoe **afgestemd op de adm** gewijzigd de `transparent_hugepage` en `numa_balancing` waarden, op basis van de vereiste instellingen voor SAP HANA.

![Het hulpprogramma is afgestemd op adm wijzigt op basis van de vereiste instellingen voor SAP HANA-waarden](./media/hana-get-started/image005.jpg)

Als u de instellingen voor SAP HANA-kernel permanente, gebruikt u **grub2** op SLES 12. Voor meer informatie over **grub2**, gaat u naar de [structuur van configuratiebestand](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) gedeelte van de SUSE-documentatie.

De volgende schermafbeelding ziet u hoe de kernel-instellingen zijn gewijzigd in het configuratiebestand en vervolgens samengesteld met behulp van **grub2 mkconfig**:

![Kernel-instellingen gewijzigd in het configuratiebestand en gecompileerd met behulp van grub2 mkconfig](./media/hana-get-started/image006.jpg)

Een andere optie is om de instellingen wijzigen met behulp van YaST en de **Boot Loader** > **Kernel Parameters** instellingen:

![Het tabblad instellingen van Parameters van de Kernel in YaST Boot Loader](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Bestandssystemen
De volgende schermafbeelding ziet u twee bestandssystemen die zijn gemaakt op de SAP-appserver VM boven op de twee gekoppelde Azure standard storage-schijven. Beide bestandssystemen zijn van het type XFS en /sapdata en /sapsoftware zijn gekoppeld.

Het is niet verplicht om te structureren van uw bestandssystemen op deze manier. Hebt u andere opties voor het structureren van de schijfruimte. De belangrijkste overweging is om te voorkomen dat het bestandssysteem van de hoofdmap van het uitvoeren van vrije ruimte.

![Twee bestandssystemen die zijn gemaakt op de SAP-appserver VM](./media/hana-get-started/image008.jpg)

Met betrekking tot de SAP HANA DB-VM tijdens de installatie van een database, wanneer u SAPinst (SWPM) en de **typische** installatieoptie, alles onder /hana en /usr/sap is geïnstalleerd. De standaardlocatie voor de SAP HANA logboekback-up is onder /usr/sap. Nogmaals, omdat het is belangrijk om te voorkomen dat het bestandssysteem van de hoofdmap van opslagruimte opraakt, zorg ervoor dat er is onvoldoende vrije ruimte onder /hana en /usr/sap voordat u SAP HANA installeren met behulp van SWPM.

Zie voor een beschrijving van de indeling standaard bestandssysteem van SAP HANA, de [SAP HANA-Server-installatie- en Updatehandleiding](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Aanvullende bestandssystemen die zijn gemaakt op de SAP-appserver VM](./media/hana-get-started/image009.jpg)

Wanneer u SAP NetWeaver op een standard SLES/SLES voor SAP-toepassingen 12 Azure galerijafbeelding installeert, wordt er een bericht weergegeven waarin wordt aangegeven dat er is geen wisselruimte, dat zoals wordt weergegeven in de volgende schermafbeelding. Als u wilt dit bericht negeren, kunt u handmatig een wisselbestand toevoegen met behulp van **dd**, **mkswap**, en **swapon**. Voor meer informatie over hoe, zoeken naar "handmatig toevoegen van een wisselbestand' de [met behulp van de YaST Partitioner](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) gedeelte van de SUSE-documentatie.

Er is een andere optie wisselruimte configureren met behulp van de Linux-VM-agent. Zie voor meer informatie de [gebruikershandleiding voor Azure Linux Agent](../../extensions/agent-linux.md).

![Pop-upbericht dat er onvoldoende wisselruimte is te adviseren](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>Het bestand/etc/hosts
Voordat u begint met het installeren van SAP, zorg er dan voor dat u de namen van hosts en IP-adressen van de SAP-VM's opnemen in het bestand/etc/hosts. Alle SAP virtuele machines binnen een virtueel Azure-netwerk te implementeren en gebruik vervolgens de interne IP-adressen, zoals hier wordt weergegeven:

![Host-namen en IP-adressen van de SAP-virtuele machines die worden vermeld in het bestand/etc/hosts](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>Het bestand/etc/fstab

Is het handig om toe te voegen de **nofail** parameter voor het fstab-bestand. Op deze manier als er iets met de schijven misgaat, de virtuele machine nu niet vastloopt tijdens het opstarten. Maar houd er rekening mee dat extra schijfruimte mogelijk niet beschikbaar en processen vol het root-bestandssysteem. Als /hana ontbreekt, wordt start SAP HANA niet.

![De parameter nofail toevoegen aan het fstab-bestand](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Grafische GNOME bureaublad op SLES 12/SLES voor SAP-toepassingen 12
In deze sectie worden de volgende onderwerpen:

* De GNOME desktop en xrdp installeert op SLES 12/SLES voor SAP-toepassingen 12
* Op Java gebaseerde SAP MC uitgevoerd met behulp van Firefox op SLES 12/SLES voor SAP-toepassingen 12

Ook kunt u alternatieven zoals Xterminal of VNC (niet in deze handleiding beschreven).

### <a name="installing-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>De GNOME desktop en xrdp installeert op SLES 12/SLES voor SAP-toepassingen 12
Als u een Windows-ervaring, kunt u een grafische bureaublad rechtstreeks vanuit de SAP Linux virtuele machines kunt eenvoudig uitvoeren van Firefox, SAPinst, SAP-GUI, SAP MC of HANA Studio en verbinding maken met de virtuele machine via het Remote Desktop Protocol (RDP) vanuit een Windows-computer gebruiken. Afhankelijk van uw bedrijfsbeleid over het toevoegen van grafische gebruikersinterfaces aan productie- en niet-productie-Linux-systemen, wilt u mogelijk GNOME installeren op uw server. Het bureaublad GNOME installeren op een Azure-SLES 12/SLES voor SAP-toepassingen 12 VM:

1. Het bureaublad GNOME installeren met de volgende opdracht (bijvoorbeeld in een venster PuTTY):

   `zypper in -t pattern gnome-basic`

2. Xrdp om toe te staan een verbinding met de virtuele machine via RDP installeren:

   `zypper in xrdp`

3. /Etc/sysconfig/windowmanager bewerken en het beheer van standaard ingesteld op GNOME:

   `DEFAULT_WM="gnome"`

4. Voer **chkconfig** om ervoor te zorgen dat xrdp automatisch wordt gestart na opnieuw opstarten:

   `chkconfig -level 3 xrdp on`

5. Als u er een probleem is met de RDP-verbinding, probeer het opnieuw opstarten (uit een PuTTY venster, bijvoorbeeld):

   `/etc/xrdp/xrdp.sh restart`

6. Als een herstart xrdp is vermeld in de vorige stap niet werkt, controleert u voor een bestand .pid:

   `check /var/run` 

   Zoek naar `xrdp.pid`. Als u deze kunt vinden, verwijderen en probeer het opnieuw.

### <a name="starting-sap-mc"></a>SAP MC starten
Nadat u het bureaublad GNOME hebt geïnstalleerd, vanaf de grafische op Java gebaseerde SAP MC Firefox tijdens het uitvoeren in een Azure-SLES 12/SLES voor SAP-toepassingen 12 virtuele machine mogelijk een foutbericht wordt weergegeven vanwege de ontbrekende Java-invoegtoepassing voor de browser.

Is de URL voor het starten van de SAP-MC `<server>:5<instance_number>13`.

Zie voor meer informatie, [vanaf het Web gebaseerde SAP-beheerconsole](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

De volgende schermafbeelding ziet u het foutbericht dat wordt weergegeven wanneer de Java-invoegtoepassing voor de browser ontbreekt:

![Foutbericht dat aangeeft dat de ontbrekende Java-browser-invoegtoepassing](./media/hana-get-started/image013.jpg)

Een manier om het probleem te verhelpen is voor het installeren van de ontbrekende invoegtoepassing met behulp van YaST, zoals wordt weergegeven in de volgende schermafbeelding:

![Met behulp van YaST voor het installeren van de invoegtoepassing ontbreekt](./media/hana-get-started/image014.jpg)

Wanneer u de URL van de SAP-Management-Console opnieuw invoert, wordt er een bericht weergegeven waarin wordt gevraagd u aan het activeren van de invoegtoepassing:

![In het dialoogvenster invoegtoepassing activering aanvragen](./media/hana-get-started/image015.jpg)

Mogelijk ontvangt u ook een foutbericht over een ontbrekend bestand javafx.properties. Dit is gerelateerd aan de vereisten van Oracle Java 1.8 voor SAP GUI 7.4. (Zie [SAP-notitie 2059429](https://launchpad.support.sap.com/#/notes/2059424).) De IBM-Java-versie noch de geleverd met SLES/SLES voor SAP-toepassingen 12 openjdk-pakket bevat de benodigde javafx.properties-bestand. De oplossing is om te downloaden en installeren van Java SE 8 van Oracle.

Zie voor informatie over een soortgelijk probleem met openjdk op openSUSE, de thread discussie [SAPGui 7.4 Java voor openSUSE 42,1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Handmatige installatie van SAP HANA: SWPM
De reeks schermafbeeldingen in deze sectie ziet u de belangrijkste stappen voor het installeren van 7.5 van SAP NetWeaver en SAP HANA SP12 wanneer u SWPM (SAPinst). Als onderdeel van een installatie van de NetWeaver 7.5 installeren SWPM ook de HANA-database als één instantie.

In een testomgeving voorbeeld we slechts één Advanced Business Application Programming (ABAP) app-server geïnstalleerd. Zoals weergegeven in de volgende schermafbeelding, we hebben gebruikt de **gedistribueerd systeem** optie voor het installeren van de ASCS en de primaire toepassing server-exemplaren in een virtuele machine van Azure en SAP HANA als de database-systeem in een andere Azure-VM.

![ASCS en server-exemplaren van een primaire toepassing geïnstalleerd met de optie Distributed System](./media/hana-get-started/image012.jpg)

Nadat de ASCS-exemplaar is geïnstalleerd op de virtuele machine van de appserver en ingesteld op "green" in de beheerconsole voor SAP (in de volgende schermafbeelding weergegeven is), moet de /sapmnt-map (met inbegrip van de SAP-profiel-map) worden gedeeld met de virtuele machine van de SAP HANA DB-server. De installatiestap DB moet toegang hebben tot deze informatie. Er is de beste manier om toegang te bieden met NFS, die kunnen worden geconfigureerd met behulp van YaST.

![SAP-beheerconsole met de ASCS-exemplaar op de virtuele machine van de app-server is geïnstalleerd en ingesteld op "green"](./media/hana-get-started/image016.jpg)

Op de appserver VM, de map /sapmnt mag worden gedeeld via NFS met behulp van de **rw** en **no_root_squash** opties. De standaardwaarden zijn **ro** en **root_squash**, wat kan leiden tot problemen bij de installatie van de database-instantie.

![De map /sapmnt via NFS delen met behulp van de opties voor rw en no_root_squash](./media/hana-get-started/image017b.jpg)

Zoals in de volgende schermafbeelding wordt weergegeven, de share /sapmnt van de virtuele machine van de app-server moet worden geconfigureerd op de virtuele machine van de SAP HANA DB-server met behulp van **NFS-Client** (en YaST).

![De share /sapmnt is geconfigureerd met behulp van de NFS-Client](./media/hana-get-started/image018b.jpg)

Om uit te voeren van een gedistribueerde installatie van de NetWeaver 7.5 (**Database-instantie**), zoals weergegeven in de volgende schermafbeelding, moet u zich aanmelden bij de virtuele machine van de SAP HANA DB-server en start SWPM.

![Installeren van een database-exemplaar met aanmelden bij de SAP HANA DB-server-VM en SWPM starten](./media/hana-get-started/image019.jpg)

Nadat u hebt geselecteerd **typische** installatie en het pad naar de installatiemedia, voer een DB-SID, naam van de host, het exemplaarnummer en het wachtwoord voor de systeembeheerder DB.

![De SAP HANA-database system administrator aanmeldingspagina opgeven](./media/hana-get-started/image035b.jpg)

Voer het wachtwoord voor het schema DBACOCKPIT:

![Het wachtwoord invoeren voor het schema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Voer een vraag voor het schema SAPABAP1 wachtwoord:

![Een vraag voor het schema SAPABAP1 wachtwoord invoeren](./media/hana-get-started/image037b.jpg)

Nadat elke taak is voltooid, wordt een groen vinkje weergegeven naast elke fase van het installatieproces DB. Het bericht 'uitvoering van... Database exemplaar is voltooid' wordt weergegeven.

![Taak is voltooid-venster met het bevestigingsbericht wordt weergegeven](./media/hana-get-started/image023.jpg)

De SAP-beheerconsole moet ook na de installatie is voltooid, de DB-exemplaar als "green" weergeven en de volledige lijst met SAP HANA-processen (hdbindexserver, hdbcompileserver, enzovoort) worden weergegeven.

![SAP-beheerconsole-venster met de lijst met SAP HANA-processen](./media/hana-get-started/image024.jpg)

De volgende schermafbeelding ziet u de onderdelen van de structuur van het bestand in de map /hana/shared die SWPM tijdens de installatie van HANA gemaakt. Omdat er geen optie om op te geven van een ander pad, is het belangrijk dat u extra schijfruimte in de map /hana voordat de installatie van de SAP HANA met behulp van SWPM koppelen. Dit voorkomt dat het root-bestandssysteem met de beschikbare vrije ruimte.

![De directory /hana/shared structuur gemaakt tijdens de installatie van HANA](./media/hana-get-started/image025.jpg)

Deze schermafbeelding ziet u de structuur van het bestand van de map /usr/sap:

![De/usr/sap directory bestandsstructuur](./media/hana-get-started/image026.jpg)

De laatste stap van de gedistribueerde ABAP-installatie is de primaire toepassing server-exemplaar te installeren:

![ABAP installatie tonen primaire toepassing server-exemplaar als de laatste stap](./media/hana-get-started/image027b.jpg)

Nadat de primaire toepassing server-exemplaar en de SAP-GUI zijn geïnstalleerd, gebruikt u de **DBA Cockpit** transactie om te bevestigen dat de installatie van de SAP HANA correct is voltooid:

![Bevestiging van geslaagde installatie DBA Cockpit-venster](./media/hana-get-started/image028b.jpg)

Als laatste stap, kan u wilt voor de eerste installatie HANA Studio in de SAP-appserver VM en maak verbinding met de SAP HANA-exemplaar dat wordt uitgevoerd op de server-DB VM:

![SAP HANA Studio installeren in de SAP-appserver VM](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Handmatige installatie van SAP HANA: HDBLCM
Naast het installeren van SAP HANA als onderdeel van een gedistribueerde installatie met behulp van SWPM, kunt u de zelfstandige HANA eerst installeren met behulp van HDBLCM. Vervolgens kunt u SAP NetWeaver 7.5, bijvoorbeeld installeren. De schermafbeeldingen in deze sectie laten zien hoe dit proces werkt.

Zie voor meer informatie over het hulpprogramma HANA HDBLCM:

* [De juiste SAP HANA-HDBLCM voor uw taak kiezen](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [SAP HANA Lifecycle-beheerhulpprogramma 's](https://www.tutorialspoint.com/sap_hana_administration/sap_hana_administration_lifecycle_management.htm)
* [SAP HANA-Server-installatie- en Updatehandleiding](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

Om te voorkomen dat problemen met een standaard groepsinstelling-ID voor de `\<HANA SID\>adm user` (gemaakt door het hulpprogramma HDBLCM), definieert u een nieuwe groep met de naam `sapsys` met behulp van groeps-ID `1001` voordat u SAP HANA via HDBLCM installeert:

![Nieuwe groep 'sapsys' gedefinieerd met behulp van groep-ID 1001](./media/hana-get-started/image030.jpg)

Wanneer u HDBLCM op de eerste keer start, wordt een eenvoudige startmenu weergegeven. SELECT-item 1, **nieuw systeem installeren**, zoals weergegeven in de volgende schermafbeelding:

![Optie "Nieuw systeem installeren" in het venster van de start HDBLCM](./media/hana-get-started/image031.jpg)

De volgende schermafbeelding geeft de sleutel opties die u eerder hebt geselecteerd.

> [!IMPORTANT]
> Mappen die zijn met de naam voor HANA-logboek en gegevensvolumes, evenals het installatiepad (/ hana/gedeelde in dit voorbeeld) en /usr/sap, mag geen deel uit van het root-bestandssysteem. Deze mappen deel uitmaken van de Azure-gegevensschijven die zijn gekoppeld aan de virtuele machine (beschreven in de sectie 'Schijf instellen'). Deze aanpak helpt voorkomen dat het bestandssysteem van de hoofdmap wordt uitgevoerd geen schijfruimte meer. In de volgende schermafbeelding ziet u dat de systeembeheerder HANA gebruikers-ID heeft `1005` en maakt deel uit van de `sapsys` groep (ID `1001`) die is gedefinieerd voordat de installatie.

![Lijst met alle belangrijke SAP HANA-onderdelen eerder hebt geselecteerd](./media/hana-get-started/image032.jpg)

U kunt controleren de `\<HANA SID\>adm user` (`azdadm` in de volgende schermafbeelding) gegevens in de map/etc/passwd:

![HANA \<HANA SID\>adm-gebruikersgegevens die worden vermeld in de map/etc/passwd](./media/hana-get-started/image033.jpg)

Nadat u SAP HANA met behulp van HDBLCM hebt geïnstalleerd, ziet u de structuur van het bestand in SAP HANA Studio, zoals wordt weergegeven in de volgende schermafbeelding. Het schema SAPABAP1, waaronder alle SAP NetWeaver-tabellen, is nog niet beschikbaar.

![SAP HANA-bestandsstructuur in SAP HANA Studio](./media/hana-get-started/image034.jpg)

Nadat u SAP HANA, kunt u SAP NetWeaver daarboven installeren. Zoals weergegeven in de volgende schermafbeelding, is de installatie als een gedistribueerde installatie uitgevoerd met behulp van SWPM (zoals beschreven in de vorige sectie). Wanneer u de database-instantie installeren met behulp van SWPM, voert u dezelfde gegevens met behulp van HDBLCM (bijvoorbeeld: hostnaam, HANA SID en exemplaarnummer). SWPM vervolgens de bestaande HANA-installatie wordt gebruikt en voegt meer schema's.

![Een gedistribueerde installatie uitgevoerd met behulp van SWPM](./media/hana-get-started/image035b.jpg)

De volgende schermafbeelding ziet de installatie SWPM stap waarin u gegevens over het schema DBACOCKPIT invoeren:

![De stap van de SWPM installatieprocedure waar DBACOCKPIT schemagegevens worden ingevoerd](./media/hana-get-started/image036b.jpg)

Gegevens over het schema SAPABAP1 invoeren:

![Gegevens over het schema SAPABAP1 invoeren](./media/hana-get-started/image037b.jpg)

Nadat de installatie van de SWPM database-exemplaar is voltooid, ziet u het schema SAPABAP1 in SAP HANA Studio:

![Het schema SAPABAP1 in SAP HANA Studio](./media/hana-get-started/image038b.jpg)

Ten slotte, nadat de SAP-appserver en de SAP-GUI-installaties zijn voltooid, kunt u controleren de HANA DB-exemplaar met behulp van de **DBA Cockpit** transactie:

![Het HANA DB-exemplaar dat is geverifieerd met de DBA Cockpit-transactie](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP software downloaden
U kunt software downloaden van de SAP Service Marketplace, zoals wordt weergegeven in de volgende schermafbeeldingen.

Download NetWeaver 7.5 voor Linux/HANA:

 ![SAP-Service installeren en upgraden venster voor het downloaden van de NetWeaver 7.5](./media/hana-get-started/image001.jpg)

HANA SP12 Platform Edition downloaden:

 ![SAP-Service installeren en upgraden venster voor het downloaden van HANA SP12 Platform Edition](./media/hana-get-started/image002.jpg)

