---
title: 'Snelstartgids: Handmatige installatie van de single instance SAP HANA op Azure Virtual Machines | Microsoft Docs'
description: Snelstartgids voor handmatige installatie van de single instance SAP HANA op Azure Virtual Machines
services: virtual-machines-linux
documentationcenter: 
author: hermanndms
manager: timlt
editor: 
tags: azure-resource-manager
keywords: 
ms.assetid: c51a2a06-6e97-429b-a346-b433a785c9f0
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 09/15/2016
ms.author: hermannd
ms.openlocfilehash: 321a86d6ce355273820617e6de9df2b0816c73fa
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="quickstart-manual-installation-of-single-instance-sap-hana-on-azure-vms"></a>Snelstartgids: Handmatige installatie van de single instance SAP HANA op Azure Virtual machines
## <a name="introduction"></a>Inleiding
Deze handleiding helpt u Hiermee stelt u een single instance SAP HANA op Azure virtuele machines (VM's) wanneer u SAP NetWeaver 7.5 en SAP HANA 1.0 SP12 handmatig installeren. Deze handleiding is over het implementeren van SAP HANA op Azure. Er is geen vervanging voor SAP-documentatie. 

>[!Note]
>Deze handleiding beschrijft de implementaties van SAP HANA in Azure Virtual machines. Zie voor meer informatie over het implementeren van SAP HANA in grote exemplaren HANA [SAP op Azure virtuele machines (VM's) met behulp van](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started).
 
## <a name="prerequisites"></a>Vereisten
Deze handleiding wordt ervan uitgegaan dat u bekend met dergelijke infrastructuur als een dienst (IaaS) basisbeginselen als bent:
 * Klik hier voor meer informatie over het implementeren van virtuele machines of virtuele netwerken via de Azure-portal of PowerShell.
 * De Azure platformoverschrijdende opdrachtregelinterface (CLI), inclusief de mogelijkheid om JSON JavaScript Object Notation ()-sjablonen te gebruiken.

Deze handleiding wordt ervan uitgegaan dat u bekend met bent:
* SAP HANA en SAP NetWeaver en het on-premises installeert.
* Installeren en besturingssysteem SAP HANA en SAP exemplaren van een toepassing in Azure.
* De volgende concepten en procedures:
   * Planning voor SAP-implementatie op Azure, met inbegrip van Azure Virtual Network planning en gebruik van Azure Storage. Zie [SAP NetWeaver op Azure Virtual Machines (VM's) - handleiding voor Planning en implementatie](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide).
   * De principes van de implementatie en manieren voor het implementeren van virtuele machines in Azure. Zie [virtuele Machines van Azure-implementatie voor SAP](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/deployment-guide).
   * Hoge beschikbaarheid voor SAP NetWeaver ASC's (ABAP SAP centrale Services), SCS (SAP centrale Services) en Ebruikers (geëvalueerd ontvangst regeling) in Azure. Zie [hoge beschikbaarheid voor SAP NetWeaver op Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide).
   * Meer informatie over het verbeteren van de efficiëntie van het gebruik van een multi-SID-installatie van ASC's / SCS op Azure. Zie [maken van een configuratie van de multi-SID SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-multi-sid). 
   * Principes van actieve SAP NetWeaver gebaseerd op Linux gebaseerde virtuele machines in Azure. Zie [SAP NetWeaver uitgevoerd op Microsoft Azure SUSE Linux VM's](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/suse-quickstart). Deze handleiding bevat de specifieke instellingen voor Linux in Azure VM's en meer informatie over het correct Azure opslagschijven koppelen aan virtuele Linux-machines.

Op dit moment worden Azure Virtual machines gecertificeerd door SAP voor SAP HANA scale-up alleen configuraties. Scale-out configuraties met SAP HANA-werkbelastingen zijn nog niet ondersteund. Zie voor SAP HANA hoge beschikbaarheid in geval van een scale-up-configuraties, [hoge beschikbaarheid van SAP HANA op Azure virtuele machines (VM's)](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-high-availability).

Als u zoekt een SAP HANA-exemplaar of S/4HANA of BW/4HANA system zeer snel tijdig worden geïmplementeerd, moet u het gebruik van [SAP-Cloudbibliotheek toestel](http://cal.sap.com). Vindt u documentatie over het implementeren, bijvoorbeeld een systeem S/4HANA via SAP CAL op Azure in [in deze handleiding](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h). Alle moet u beschikken over een Azure-abonnement en een SAP-gebruiker die kan worden geregistreerd met SAP-Cloudbibliotheek toestel is.

## <a name="additional-resources"></a>Aanvullende bronnen
### <a name="sap-hana-backup"></a>SAP HANA back-up
Zie voor informatie over back-ups SAP HANA-databases op Azure Virtual machines:
* [Back-handleiding voor SAP HANA op Azure Virtual Machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
* [SAP HANA Azure back-up op bestandsniveau](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
* [SAP HANA-back-up op basis van opslag-momentopnamen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)

### <a name="sap-cloud-appliance-library"></a>SAP-Cloudbibliotheek toestel
Zie voor meer informatie over het gebruik van SAP-Cloudbibliotheek toestel implementeren S/4HANA of BW/4HANA [implementeren SAP S/4HANA of BW/4HANA in Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h).

### <a name="sap-hana-supported-operating-systems"></a>SAP HANA-ondersteunde besturingssystemen.
Zie voor informatie over SAP HANA-ondersteunde besturingssystemen, [SAP ondersteuning Opmerking #2235581 - SAP HANA: ondersteunde besturingssystemen](https://launchpad.support.sap.com/#/notes/2235581/E). Virtuele machines in Azure ondersteunen slechts een subset van deze besturingssystemen. De volgende besturingssystemen worden ondersteund voor het implementeren van SAP HANA op Azure: 

* SUSE Linux Enterprise Server 12.x
* Red Hat Enterprise Linux 7.2

Zie voor aanvullende SAP-documentatie over SAP HANA en verschillende Linux-besturingssystemen:

* [SAP ondersteuning Opmerking #171356 - SAP-Software op Linux: algemene informatie](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP-notitie ondersteuning #1944799 - richtlijnen voor SAP HANA SLES voor installatie van besturingssysteem](http://go.sap.com/documents/2016/05/e8705aae-717c-0010-82c7-eda71af511fa.html)
* [SAP ondersteuning Opmerking #2205917 - SAP HANA DB aanbevolen OS-instellingen voor SLES 12 voor SAP-toepassingen](https://launchpad.support.sap.com/#/notes/2205917/E)
* [SAP ondersteuning Opmerking #1984787 - SUSE Linux Enterprise Server 12: Opmerkingen bij de installatie van de](https://launchpad.support.sap.com/#/notes/1984787)
* [SAP ondersteuning Opmerking #1391070 - oplossingen voor Linux-UUID](https://launchpad.support.sap.com/#/notes/1391070)
* [SAP-notitie ondersteuning #2009879 - SAP HANA richtlijnen voor het Red Hat Enterprise Linux (RHEL)-besturingssysteem](https://launchpad.support.sap.com/#/notes/2009879)
* [2292690 - SAP HANA-database: OS aanbevolen instellingen voor RHEL 7](https://launchpad.support.sap.com/#/notes/2292690/E)

### <a name="sap-monitoring-in-azure"></a>SAP bewaken in Azure
Zie voor informatie over SAP bewaken in Azure:

* [SAP-notitie 2191498](https://launchpad.support.sap.com/#/notes/2191498/E). Deze opmerking wordt besproken SAP 'uitgebreide bewaking' met een virtuele Linux-machines in Azure. 
* [SAP-notitie 1102124](https://launchpad.support.sap.com/#/notes/1102124/E). Deze opmerking vindt u informatie over SAPOSCOL op Linux. 
* [SAP-notitie 2178632](https://launchpad.support.sap.com/#/notes/2178632/E). Deze opmerking wordt belangrijkste bewaking metrische gegevens voor SAP beschreven in Microsoft Azure.

### <a name="azure-vm-types"></a>Azure VM-typen
Azure VM-typen en werkbelasting SAP-ondersteunde scenario's gebruikt met SAP HANA worden beschreven in [SAP gecertificeerd IaaS Platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). 

Azure VM-typen die zijn gecertificeerd door SAP voor SAP NetWeaver of de toepassingslaag S/4HANA zijn gedocumenteerd in [SAP-notitie 1928533 - SAP-toepassingen in Azure: typen ondersteunde producten en de virtuele machine van Azure](https://launchpad.support.sap.com/#/notes/1928533/E).

>[!Note]
>SAP-Linux-Azure-integratie wordt alleen ondersteund op Azure Resource Manager en niet met het klassieke implementatiemodel. 

## <a name="manual-installation-of-sap-hana"></a>Handmatige installatie van het SAP HANA
Deze handleiding wordt beschreven hoe SAP HANA handmatig installeren op Azure VM's op twee verschillende manieren:

* Met behulp van SAP Software inrichting Manager (SWPM) als onderdeel van een gedistribueerde installatie NetWeaver in de stap "database-exemplaar installeren"
* Met behulp van de SAP HANA-database lifecycle manager hulpprogramma HDBLCM en vervolgens NetWeaver te installeren

U kunt ook SWPM gebruiken voor het installeren van alle onderdelen (SAP HANA, de SAP-toepassingsserver en het exemplaar ASC's) in één enkele virtuele machine, zoals beschreven in dit [SAP HANA-blog aankondiging](https://blogs.saphana.com/2013/12/31/announcement-sap-hana-and-sap-netweaver-as-abap-deployed-on-one-server-is-generally-available/). Deze optie wordt niet beschreven in deze snelstartgids, maar de problemen die u in aanmerking moet nemen zijn hetzelfde.

Voordat u een installatie start, raden we aan dat u leest de ' voorbereiden Azure VM's voor handmatige installatie van het SAP HANA ' verderop in deze handleiding. In dat geval kan u helpen voorkomen van verschillende algemene fouten die optreden mogelijk wanneer u alleen een standaard Azure VM-configuratie gebruiken.

## <a name="key-steps-for-sap-hana-installation-when-you-use-sap-swpm"></a>Belangrijke stappen voor SAP HANA-installatie wanneer u SAP SWPM
Deze sectie vindt de belangrijkste stappen voor de installatie van een handmatige, single instance SAP HANA wanneer u SAP SWPM voor het uitvoeren van een gedistribueerde SAP NetWeaver 7.5-installatie. De afzonderlijke stappen worden in meer detail in schermopnamen verderop in deze handleiding beschreven.

1. Maak een Azure-netwerk met twee test-virtuele machines.
2. Implementeer de twee Azure VM's met besturingssystemen (in ons voorbeeld SUSE Linux Enterprise Server (SLES) en SLES voor SAP-toepassingen 12 SP1), afhankelijk van het Azure Resource Manager-model.
3. Twee Azure standard of premium-opslag-schijven (bijvoorbeeld 75 GB of 500 GB schijven) koppelen aan de VM-toepassingsserver.
4. Premium-opslag-schijven aan de HANA DB server VM koppelen. Zie de sectie 'Disk setup' verderop in deze handleiding voor meer informatie.
5. Koppelen van meerdere schijven, afhankelijk van de vereisten voor de grootte of doorvoer, en vervolgens striped volumes maken met behulp van logische volumebeheer of een beheerprogramma voor meerdere apparaten (MDADM) op het niveau van het besturingssysteem in de virtuele machine.
6. Bestandssystemen XFS op de gekoppelde schijven of logische volumes maken.
7. Koppel de nieuwe XFS bestandssystemen op het niveau van het besturingssysteem. Een bestandssysteem wordt gebruikt voor alle SAP-software. Gebruik bijvoorbeeld het bestandssysteem voor de /sapmnt directory en de back-ups. Koppel de bestandssystemen XFS op de schijven van de premium-opslag als /hana en /usr/sap op de server SAP HANA-database. Dit proces is nodig om te voorkomen dat de basis-bestandssysteem niet groot op Linux Azure Virtual machines, wordt gevuld.
8. Geef het lokale IP-adressen van de test-virtuele machines in het bestand/etc/hosts.
9. Voer de **nofail** parameter in het bestand/etc/fstab-fouten.
10. Stel Linux kernel parameters volgens de Linux-besturingssysteem versie die u gebruikt. Zie voor meer informatie de juiste SAP-opmerkingen waarin HANA en de sectie 'Kernel parameters' in deze handleiding worden beschreven.
11. Voeg wisselruimte toe.
12. Installeer eventueel een grafische bureaublad op de test-virtuele machines. Anders gebruikt u een installatie op afstand SAPinst.
13. Download de SAP-software in de Marketplace SAP-Service.
14. Het exemplaar SAP ASC's installeren op de appserver VM.
15. Deel de map /sapmnt tussen de test-virtuele machines met behulp van NFS. De VM-toepassingsserver is de NFS-server.
16. Het database-exemplaar, met inbegrip van HANA, met behulp van SWPM op de server voor DB VM installeren.
17. Installeer de primaire toepassingsserver (Pa's) op de VM-toepassingsserver.
18. Start SAP Management Console (SAP MV). Verbinding met SAP-GUI of HANA Studio, bijvoorbeeld.

## <a name="key-steps-for-sap-hana-installation-when-you-use-hdblcm"></a>Belangrijke stappen voor SAP HANA-installatie, wanneer u HDBLCM
Deze sectie vindt de belangrijkste stappen voor de installatie van een handmatige, single instance SAP HANA wanneer u SAP HDBLCM voor het uitvoeren van een gedistribueerde SAP NetWeaver 7.5-installatie. De afzonderlijke stappen worden in meer detail in schermopnamen in deze handleiding beschreven.

1. Maak een Azure-netwerk met twee test-virtuele machines.
2. Twee Azure VM's met besturingssystemen (in ons voorbeeld SLES en SLES voor SAP-toepassingen 12 SP1) volgens het Azure Resource Manager-model implementeren.
3. Twee Azure standard of premium-opslag-schijven (bijvoorbeeld 75 GB of 500 GB schijven) aan de appserver VM koppelen.
4. Premium-opslag-schijven aan de HANA DB server VM koppelen. Zie de sectie 'Disk setup' verderop in deze handleiding voor meer informatie.
5. Afhankelijk van de vereisten voor de grootte of doorvoer, koppel meerdere schijven en striped volumes maken met behulp van logische volumebeheer of een beheerprogramma voor meerdere apparaten (MDADM) op het niveau van het besturingssysteem in de virtuele machine.
6. Bestandssystemen XFS op de gekoppelde schijven of logische volumes maken.
7. Koppel de nieuwe XFS bestandssystemen op het niveau van het besturingssysteem. Gebruik een bestandssysteem voor alle SAP-software en gebruik de andere voor de /sapmnt directory en de back-ups, bijvoorbeeld. Koppel de bestandssystemen XFS op de schijven van de premium-opslag als /hana en /usr/sap op de server SAP HANA-database. Dit proces is nodig om te voorkomen dat de basis-bestandssysteem niet groot op Linux Azure Virtual machines, wordt gevuld.
8. Geef het lokale IP-adressen van de test-virtuele machines in het bestand/etc/hosts.
9. Voer de **nofail** parameter in het bestand/etc/fstab-fouten.
10. Stel kernel parameters volgens de Linux-besturingssysteem versie die u gebruikt. Zie voor meer informatie de juiste SAP-opmerkingen waarin HANA en de sectie 'Kernel parameters' in deze handleiding worden beschreven.
11. Voeg wisselruimte toe.
12. Installeer eventueel een grafische bureaublad op de test-virtuele machines. Anders gebruikt u een installatie op afstand SAPinst.
13. Download de SAP-software in de Marketplace SAP-Service.
14. Maak een groep, sapsys, met groep-ID 1001 op de server HANA DB VM.
15. SAP HANA op de server voor DB VM installeren met behulp van HANA Database Lifecycle Manager (HDBLCM).
16. Het exemplaar SAP ASC's installeren op de appserver VM.
17. Deel de map /sapmnt tussen de test-virtuele machines met behulp van NFS. De VM-toepassingsserver is de NFS-server.
18. Het database-exemplaar, met inbegrip van HANA, met behulp van SWPM op de server HANA DB VM installeren.
19. Installeer de primaire toepassingsserver (Pa's) op de VM-toepassingsserver.
20. SAP MV starten. Verbinding maken via SAP-GUI of HANA Studio.

## <a name="preparing-azure-vms-for-a-manual-installation-of-sap-hana"></a>Azure Virtual machines voorbereiden voor een handmatige installatie van de SAP HANA
Deze sectie bevat de volgende onderwerpen:

* Updates voor het besturingssysteem
* Installatie van de schijf
* Kernel-parameters
* Bestandssystemen
* Het bestand/etc/hosts
* Het bestand/etc/fstab-fouten

### <a name="os-updates"></a>Updates voor het besturingssysteem
Controle voor Linux OS-updates en oplossingen voordat u extra software installeert. Als u een patch installeert, is het mogelijk om te voorkomen dat een aanroep naar de helpdesk.

Zorg ervoor dat u gebruikt:
* SUSE Linux Enterprise Server voor SAP-toepassingen.
* Red Hat Enterprise Linux voor SAP-toepassingen of Red Hat Enterprise Linux voor SAP HANA. 

Als u nog niet gedaan hebt, registreert u de implementatie van een besturingssysteem met uw Linux-abonnement van de leverancier van Linux. Houd er rekening mee dat SUSE heeft installatiekopieën van het besturingssysteem voor SAP-toepassingen die al services bevatten en die automatisch worden geregistreerd.

Hier volgt een voorbeeld van het controleren op beschikbare patches voor SUSE Linux met behulp van de **zypper** opdracht:

 `sudo zypper list-patches`

Patches zijn afhankelijk van het soort probleem geclassificeerd op categorie en ernst. Veelgebruikte waarden voor de categorie zijn: **beveiliging**, **aanbevolen**, **optionele**, **functie**, **document**, of **yast**.
Veelgebruikte waarden voor ernst zijn: **kritieke**, **belangrijke**, **gemiddeld**, **lage**, of **niet nader omschreven**.

De **zypper** opdracht zoekt alleen de updates die moeten worden uw geïnstalleerde pakketten. U kunt bijvoorbeeld deze opdracht gebruiken:

`sudo zypper patch  --category=security,recommended --severity=critical,important`

U kunt de parameter toevoegen `--dry-run` voor het testen van de update zonder daadwerkelijk bijwerken van het systeem.


### <a name="disk-setup"></a>Installatie van de schijf
Het bestandssysteem hoofdmap op een Linux-VM in Azure is een maximale grootte. Daarom is het nodig extra ruimte op schijf koppelen aan een virtuele machine van Azure voor het uitvoeren van SAP. Voor SAP-toepassingsserver virtuele Azure-machines, kan het gebruik van Azure standard opslagschijven voldoende zijn. Echter, is het gebruik van Azure Premium-opslag-schijven voor productie- en niet voor productie-implementaties voor SAP HANA DBMS Azure VM's verplicht.

Op basis van de [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html), de volgende Azure Premium-opslag-configuratie wordt voorgesteld: 

| VM-SKU | RAM |  hana/gegevens en hana/logboekbestanden <br /> striped met LVM of MDADM | / hana/gedeeld | / Root-volume | / usr/sap |
| --- | --- | --- | --- | --- | --- |
| GS5 | 448 GB | 2 x P30 | 1 x P20 | 1 x P10 | 1 x P10 | 

De HANA gegevensvolume en logboekvolume in de configuratie van de voorgestelde schijf worden op dezelfde set Azure premium-schijven voor opslag met LVM of MDADM striped geplaatst. Het is niet nodig voor het definiëren van een RAID-niveau redundantie omdat Azure Premium-opslag drie afbeeldingen van de schijven voor de redundantie houdt. Om ervoor te zorgen dat u voldoende opslagruimte configureren, raadpleegt u de [opslagvereisten voor SAP HANA TDI](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html) en [Update handleiding en SAP HANA-serverinstallatie](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm). Ook rekening houden met de andere virtuele harde schijf (VHD) doorvoer volumes van de verschillende Azure premium storage schijven zoals beschreven in [beheerde schijven voor virtuele machines en hoge prestaties Premium-opslag](https://docs.microsoft.com/azure/storage/storage-premium-storage). 

U kunt meer schijven voor premium-opslag toevoegen aan de HANA DBMS virtuele machines voor het opslaan van de database of het transactielogboek logboekback-ups.

Zie de volgende artikelen voor meer informatie over de twee belangrijkste hulpprogramma's gebruikt voor het configureren van striping:

* [Configureren van software-RAID op Linux](../../linux/configure-raid.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [LVM configureren op een virtuele Linux-machine in Azure](../../linux/configure-lvm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Zie voor meer informatie over het koppelen van schijven aan de virtuele Azure-machines met Linux als een gastbesturingssysteem [een schijf toevoegen aan een Linux-VM](../../linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Azure Premium-opslag kunt u voor het definiëren van de schijfcache modi. Voor de striped set /hana/data en /hana/log, moet schijfcache worden uitgeschakeld. Voor de andere volumes (schijven), de cachemodus moet worden ingesteld op **ReadOnly**.

Zie voor meer informatie [Premium-opslag: krachtige opslag voor Azure Virtual Machine-werkbelasting](../../windows/premium-storage.md).

Als u wilt zoeken voorbeeld JSON-sjablonen voor het maken van virtuele machines, gaat u naar [Azure-Snelstartsjablonen](https://github.com/Azure/azure-quickstart-templates).
De vm-eenvoudige-sles-sjabloon is een eenvoudige sjabloon. Bevat een opslagsectie, met een schijf extra 100 GB aan gegevens. Deze sjabloon kan worden gebruikt als basis. U kunt de sjabloon aanpassen aan uw specifieke configuratie.

>[!Note]
>Het is belangrijk voor het koppelen van de Azure-opslag-schijf met behulp van een UUID zoals beschreven in [SAP NetWeaver uitgevoerd op Microsoft Azure SUSE Linux VM's](suse-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

In de testomgeving zijn twee Azure standard-opslag-schijven gekoppeld aan de appserver SAP VM, zoals wordt weergegeven in de volgende schermafbeelding. Alle SAP-software (inclusief NetWeaver 7.5, SAP-GUI en SAP HANA) één schijf opgeslagen voor de installatie. De tweede schijf ervoor gezorgd dat er voldoende vrije ruimte beschikbaar voor aanvullende vereisten (bijvoorbeeld een back-up en test gegevens) en voor de map /sapmnt (dat wil zeggen, SAP-profielen) wordt gedeeld met alle virtuele machines die deel uitmaken van de dezelfde SAP liggend zou zijn.

![SAP HANA app server schijven venster om van twee gegevensschijven en de grootte weer te geven](./media/hana-get-started/image003.jpg)


### <a name="kernel-parameters"></a>Kernel-parameters
SAP HANA vereist specifieke Linux kernel instellingen, die deel uitmaken van de installatiekopieën van het standaard Azure-galerie en moeten handmatig worden ingesteld. Afhankelijk van of u SUSE of Red Hat gebruikt, kunnen de parameters anders zijn. De eerder vermelde opmerkingen bij de SAP geven informatie over deze parameters. In de schermafbeeldingen wordt weergegeven, werd SUSE Linux 12 SP1 gebruikt. 

SLES voor SAP-toepassingen 12 GA en SLES voor SAP-toepassingen 12 SP1 hebt u een nieuw hulpprogramma **afgestemd adm**, die wordt vervangen door de oude **sapconf** hulpprogramma. Er is een speciaal SAP HANA-profiel beschikbaar voor **afgestemd adm**. Als u wilt het systeem voor SAP HANA afstemmen, voert u het volgende als een hoofdgebruiker:

   `tuned-adm profile sap-hana`

Voor meer informatie over **afgestemd adm**, Zie de [SUSE documentatie over afgestemd adm](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip).

In de volgende schermafbeelding ziet u hoe **afgestemd adm** gewijzigd de `transparent_hugepage` en `numa_balancing` waarden, afhankelijk van de vereiste instellingen voor SAP HANA.

![Het hulpprogramma afgestemd adm waarden volgens vereiste SAP HANA-instellingen gewijzigd](./media/hana-get-started/image005.jpg)

Als u de instellingen van de kernel SAP HANA permanente, gebruikt u **grub2** op SLES 12. Voor meer informatie over **grub2**, gaat u naar de [structuur van configuratiebestand](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) gedeelte van de SUSE-documentatie.

De volgende schermafbeelding ziet u hoe de kernel-instellingen zijn gewijzigd in het configuratiebestand en vervolgens gecompileerd met behulp van **grub2 mkconfig**:

![Kernel-instellingen gewijzigd in het configuratiebestand en gecompileerd met behulp van grub2 mkconfig](./media/hana-get-started/image006.jpg)

Er is een andere optie om de instellingen wijzigen met behulp van YaST en de **opstartlaadprogramma** > **Kernel Parameters** instellingen:

![Het tabblad instellingen van Parameters van de Kernel in YaST opstartlaadprogramma](./media/hana-get-started/image007.jpg)

### <a name="file-systems"></a>Bestandssystemen
De volgende schermafbeelding ziet twee bestandssystemen die zijn gemaakt op de SAP Apps VM boven op de twee gekoppelde Azure standard-opslag-schijven. Beide bestandssystemen zijn van het type XFS en /sapdata en /sapsoftware zijn gekoppeld.

Het is niet verplicht structuur van uw bestandssystemen op deze manier. Hebt u andere opties voor de schijfruimte structureren. De belangrijkste overweging is om te voorkomen dat het root-bestandssysteem geen beschikbare ruimte die wordt uitgevoerd.

![Twee bestandssystemen die zijn gemaakt op de server in SAP app VM](./media/hana-get-started/image008.jpg)

Met betrekking tot SAP HANA-database VM, tijdens de installatie van een database, wanneer u SAPinst (SWPM) en de **typische** installatieoptie, alles onder /hana en /usr/sap is geïnstalleerd. De standaardlocatie voor de back-up van de SAP HANA ligt onder de /usr/sap. Opnieuw, omdat het is belangrijk om te voorkomen dat het bestandssysteem hoofdmap met de beschikbare opslagruimte Zorg ervoor dat er is onvoldoende vrije ruimte onder /hana en /usr/sap voordat u SAP HANA installeren met behulp van SWPM.

Zie voor een beschrijving van de indeling standaard bestandssysteem van SAP HANA de [Update handleiding en SAP HANA-serverinstallatie](http://help.sap.com/saphelp_hanaplatform/helpdata/en/4c/24d332a37b4a3caad3e634f9900a45/frameset.htm).

![Aanvullende bestandssystemen die zijn gemaakt op de server in SAP app VM](./media/hana-get-started/image009.jpg)

Wanneer u SAP NetWeaver op een standaard SLES/SLES voor SAP-toepassingen 12 Azure-galerie installatiekopie installeert, wordt een bericht weergegeven dat er geen wisselruimte, zoals wordt weergegeven in de volgende schermafbeelding. Als u wilt dit bericht negeren, kunt u handmatig een wisselbestand toevoegen met behulp van **dd**, **mkswap**, en **swapon**. Voor meer informatie over hoe, zoeken naar 'Handmatig toevoegen van een wisselbestand' de [met behulp van de YaST Partitioner](https://www.suse.com/documentation/sles-for-sap-12/pdfdoc/sles-for-sap-12-sp1.zip) gedeelte van de SUSE-documentatie.

Er is een andere optie wisselruimte configureren met behulp van de Linux VM-agent. Zie voor meer informatie de [gebruikershandleiding voor Azure Linux Agent](../../linux/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

![Pop-bericht verschijnt dat er onvoldoende wisselruimte is](./media/hana-get-started/image010.jpg)


### <a name="the-etchosts-file"></a>Het bestand/etc/hosts
Voordat u begint met het installeren van SAP, zorg er dan voor dat u de hostnamen en IP-adressen van de SAP-virtuele machines in het bestand/etc/hosts opnemen. Alle SAP VM's binnen een virtuele Azure-netwerk te implementeren en gebruik vervolgens de interne IP-adressen als volgt te werk:

![Hostnamen en IP-adressen van de SAP-virtuele machines die worden vermeld in het bestand/etc/hosts](./media/hana-get-started/image011.jpg)

### <a name="the-etcfstab-file"></a>Het bestand/etc/fstab-fouten

Het is nuttig om toe te voegen de **nofail** parameter naar het bestand fstab-fouten. Op deze manier als er iets mis met de schijven, gaat de virtuele machine nu niet vastloopt tijdens het opstarten. Maar houd er rekening mee dat extra schijfruimte mogelijk niet beschikbaar en processen met het root-bestandssysteem opvullen mogelijk. Als /hana ontbreekt, start SAP HANA niet.

![De parameter nofail toevoegen aan het bestand fstab-fouten](./media/hana-get-started/image000c.jpg)

## <a name="graphical-gnome-desktop-on-sles-12sles-for-sap-applications-12"></a>Grafische GNOME bureaublad op SLES 12/SLES voor SAP-toepassingen 12
Deze sectie bevat de volgende onderwerpen:

* De GNOME bureaublad en xrdp geïnstalleerd op SLES 12/SLES voor SAP-toepassingen 12
* Java gebaseerde SAP MV uitgevoerd met behulp van Firefox op SLES 12/SLES voor SAP-toepassingen 12

U kunt ook alternatieven zoals Xterminal of VNC (wordt niet beschreven in deze handleiding) gebruiken.

### <a name="installing-the-gnome-desktop-and-xrdp-on-sles-12sles-for-sap-applications-12"></a>De GNOME bureaublad en xrdp geïnstalleerd op SLES 12/SLES voor SAP-toepassingen 12
Als u een Windows-achtergrond hebt, kunt u eenvoudig een grafische bureaublad rechtstreeks vanuit de SAP Linux virtuele machines gebruiken Firefox, SAPinst, GUI SAP, SAP MV of HANA Studio uitvoeren en vanaf een Windows-computer verbinding maken met de virtuele machine via de Remote Desktop Protocol (RDP). Afhankelijk van uw bedrijfsbeleid over het toevoegen van grafische gebruikersinterfaces voor productie en niet-productieve Linux-systemen, wilt u mogelijk GNOME installeren op uw server. Het bureaublad GNOME installeren op een Azure-SLES 12/SLES voor SAP-toepassingen 12 VM:

1. Het bureaublad GNOME installeren met de volgende opdracht (bijvoorbeeld in een venster PuTTY):

   `zypper in -t pattern gnome-basic`

2. Xrdp om toe te staan een verbinding met de virtuele machine via RDP installeren:

   `zypper in xrdp`

3. /Etc/sysconfig/windowmanager bewerken en het beheer van standaard ingesteld op GNOME:

   `DEFAULT_WM="gnome"`

4. Voer **chkconfig** om ervoor te zorgen dat xrdp automatisch wordt gestart na opnieuw opstarten:

   `chkconfig -level 3 xrdp on`

5. Als er een probleem met de RDP-verbinding, probeer het opnieuw opstarten (uit een PuTTY venster bijvoorbeeld):

   `/etc/xrdp/xrdp.sh restart`

6. Als een herstart xrdp is vermeld in de vorige stap niet werkt, controleert u voor een bestand .pid:

   `check /var/run` 

   Zoek naar `xrdp.pid`. Als u wordt gevonden, verwijderen en probeer het opnieuw.

### <a name="starting-sap-mc"></a>SAP MV starten
Nadat u het bureaublad GNOME vanaf de grafische Java gebaseerde SAP MV Firefox tijdens het uitvoeren van een Azure-SLES 12/SLES voor SAP-toepassingen 12 VM mogelijk een foutbericht wordt weergegeven vanwege de ontbrekende Java browser-invoegtoepassing.

De URL naar het starten van de SAP-MV is `<server>:5<instance_number>13`.

Zie voor meer informatie [starten van de beheerconsole van Web-Based SAP](https://help.sap.com/saphelp_nwce10/helpdata/en/48/6b7c6178dc4f93e10000000a42189d/frameset.htm).

De volgende schermafbeelding ziet het foutbericht dat wordt weergegeven wanneer de Java browser-invoegtoepassing ontbreekt:

![Foutbericht dat aangeeft dat de ontbrekende Java browser-invoegtoepassing](./media/hana-get-started/image013.jpg)

Een manier om het probleem oplost is het installeren van het ontbrekende invoegtoepassing met behulp van YaST, zoals wordt weergegeven in de volgende schermafbeelding:

![Met behulp van YaST ontbreekt invoegtoepassing installeren](./media/hana-get-started/image014.jpg)

Wanneer u de URL van de SAP-Management-Console opnieuw invoert, wordt er een bericht weergegeven waarin u voor het activeren van de invoegtoepassing:

![In het dialoogvenster invoegtoepassing activering aanvraagt](./media/hana-get-started/image015.jpg)

U kunt ook een foutbericht over een ontbrekend bestand, ontvangen javafx.properties. Dit is gerelateerd aan de vereisten van Oracle Java 1.8 voor SAP GUI 7.4. (Zie [SAP-notitie 2059429](https://launchpad.support.sap.com/#/notes/2059424).) De IBM Java-versie noch het openjdk pakket geleverd met SLES/SLES voor SAP-toepassingen 12 bevat de benodigde javafx.properties-bestand. De oplossing opnieuw te downloaden en installeren van Java SE 8 uit Oracle.

Zie voor informatie over een soortgelijk probleem met openjdk op openSUSE de bespreking van de thread [SAPGui 7.4 Java voor openSUSE 42,1 Leap](https://scn.sap.com/thread/3908306).

## <a name="manual-installation-of-sap-hana-swpm"></a>Handmatige installatie van het SAP HANA: SWPM
De reeks van schermopnamen in dit gedeelte bevat de belangrijkste stappen voor het installeren van SAP NetWeaver 7.5 en SAP HANA SP12 wanneer u SWPM (SAPinst) gebruikt. Als onderdeel van een installatie van de NetWeaver 7.5 kunt SWPM HANA-database ook installeren als één exemplaar.

In een testomgeving voorbeeld er slechts één geavanceerde Business Application Programming (ABAP) appserver geïnstalleerd. Zoals u in de volgende schermafbeelding, hebben we gebruikt de **Distributed System** optie voor het installeren van de ASC's en de primaire toepassing server-exemplaren in een virtuele machine van Azure en SAP HANA als het databasesysteem in een andere virtuele machine in Azure.

![ASC's en exemplaren van de primaire toepassing-server is geïnstalleerd met behulp van de optie Distributed System](./media/hana-get-started/image012.jpg)

Nadat het exemplaar ASC's is geïnstalleerd op de appserver VM en ingesteld op 'groen' in de beheerconsole voor SAP (in de volgende schermafbeelding weergegeven is), moet de /sapmnt-map (met inbegrip van de map SAP-profiel) worden gedeeld met de SAP HANA-database-server VM. De stap van de installatie DB moet toegang tot deze informatie. Er is de beste manier om toegang te bieden met NFS, die kan worden geconfigureerd met behulp van YaST.

![SAP-beheerconsole weergegeven van het exemplaar ASC's geïnstalleerd op de appserver VM en ingesteld op 'groen'](./media/hana-get-started/image016.jpg)

Op de appserver VM, de map /sapmnt mag worden gedeeld via NFS met behulp van de **rw** en **no_root_squash** opties. De standaardwaarden zijn **ro** en **root_squash**, wat kan leiden tot problemen bij het installeren van de database-instantie.

![De map /sapmnt via NFS delen met behulp van de opties voor rw en no_root_squash](./media/hana-get-started/image017b.jpg)

Als de volgende schermafbeelding ziet, de share /sapmnt van de appserver VM op de server voor SAP HANA DB VM moet worden geconfigureerd met behulp **NFS-Client** (en YaST).

![De share /sapmnt is geconfigureerd met behulp van de NFS-Client](./media/hana-get-started/image018b.jpg)

Een gedistribueerde installatie van de NetWeaver 7.5 uitvoeren (**Database-exemplaar**), zoals weergegeven in de volgende schermafbeelding, moet u zich aanmelden bij de server voor SAP HANA DB VM en SWPM starten.

![Een database-exemplaar installeren met het aanmelden bij de server voor SAP HANA DB VM en SWPM starten](./media/hana-get-started/image019.jpg)

Nadat u hebt geselecteerd **typische** installatie en het pad naar de installatiemedia, voer een SID DB, de hostnaam, het exemplaarnummer en het beheerderswachtwoord van het DB-systeem.

![De SAP HANA-database systeembeheerder aanmeldingspagina](./media/hana-get-started/image035b.jpg)

Voer het wachtwoord voor het schema DBACOCKPIT:

![Het wachtwoord invoeren voor het schema DBACOCKPIT](./media/hana-get-started/image036b.jpg)

Geef een vraag om het schema SAPABAP1 wachtwoord:

![Geef een vraag om het schema SAPABAP1 wachtwoord](./media/hana-get-started/image037b.jpg)

Nadat elke taak is voltooid, wordt een groen vinkje weergegeven naast elke fase van het installatieproces van de database. Het bericht 'uitvoering van... Database exemplaar is voltooid' wordt weergegeven.

![Taak is voltooid-venster met bevestigingsbericht](./media/hana-get-started/image023.jpg)

De beheerconsole voor SAP moet na de installatie is voltooid, ook weergeven van de database-instantie als 'groen' en de volledige lijst weergeven met SAP HANA processen (hdbindexserver, hdbcompileserver, enzovoort).

![SAP Management Console-venster met de lijst met SAP HANA-processen](./media/hana-get-started/image024.jpg)

De volgende schermafbeelding ziet de onderdelen van de structuur in de map /hana/shared die SWPM tijdens de installatie HANA gemaakt. Omdat er geen optie om op te geven van een ander pad is, is het belangrijk dat u extra schijfruimte onder de map /hana voordat de SAP HANA-installatie met behulp van SWPM koppelen. Dit voorkomt dat het bestandssysteem hoofdmap met de beschikbare vrije ruimte.

![De /hana/shared directory bestandsstructuur gemaakt tijdens de installatie van de HANA](./media/hana-get-started/image025.jpg)

Deze schermafbeelding ziet u de structuur van de map /usr/sap:

![De/usr/sap directory bestandsstructuur](./media/hana-get-started/image026.jpg)

De laatste stap van de installatie van de gedistribueerde ABAP is voor het installeren van het exemplaar van de primaire server:

![ABAP installatie tonen primaire application server-exemplaar als laatste stap](./media/hana-get-started/image027b.jpg)

Nadat het exemplaar van de primaire server en SAP-GUI zijn geïnstalleerd, gebruikt u de **DBA Cockpit** transactie om te bevestigen dat de installatie van de SAP HANA correct is voltooid:

![DBA Cockpit venster geslaagde installatie bevestigen](./media/hana-get-started/image028b.jpg)

Als een laatste stap mogelijk u eerste installatie HANA Studio in de appserver SAP VM wilt en maak verbinding met de SAP HANA-exemplaar dat wordt uitgevoerd op de server voor DB VM:

![SAP HANA Studio installeren in de appserver SAP VM](./media/hana-get-started/image038b.jpg)

## <a name="manual-installation-of-sap-hana-hdblcm"></a>Handmatige installatie van het SAP HANA: HDBLCM
Naast de SAP HANA installeren als onderdeel van een gedistribueerde installatie met behulp van SWPM, kunt u de zelfstandige HANA eerst installeren met behulp van HDBLCM. U kunt bijvoorbeeld SAP NetWeaver 7.5, installeren. De schermafbeeldingen in deze sectie laten zien hoe dit proces werkt.

Zie voor meer informatie over het hulpprogramma HANA HDBLCM:

* [De juiste SAP HANA HDBLCM voor de taak kiezen](https://help.sap.com/saphelp_hanaplatform/helpdata/en/68/5cff570bb745d48c0ab6d50123ca60/content.htm)
* [SAP HANA Lifecycle beheerhulpprogramma 's](http://saphanatutorial.com/sap-hana-lifecycle-management-tools/)
* [Update-handleiding en SAP HANA-serverinstallatie](http://help.sap.com/hana/SAP_HANA_Server_Installation_Guide_en.pdf)

Om te voorkomen dat problemen met een instelling standaard groep-ID voor de `\<HANA SID\>adm user` (gemaakt door het hulpprogramma HDBLCM), definieert u een nieuwe groep aangeroepen `sapsys` met behulp van de groeps-ID `1001` voordat u een SAP HANA via HDBLCM installeert:

![Nieuwe groep 'sapsys' gedefinieerd met behulp van groep-ID 1001](./media/hana-get-started/image030.jpg)

Wanneer u HDBLCM voor het eerst start, wordt een eenvoudige startmenu weergegeven. SELECT-item 1, **nieuw systeem installeren**, zoals wordt weergegeven in de volgende schermafbeelding:

![Optie 'Nieuw systeem installeren' in het venster van de start HDBLCM](./media/hana-get-started/image031.jpg)

De volgende schermafbeelding wordt weergegeven voor de sleutel opties die u eerder hebt geselecteerd.

> [!IMPORTANT]
> Mappen die zijn met de naam van het logboek HANA en gegevensvolumes, evenals het installatiepad (hana/gedeeld in dit voorbeeld) en /usr/sap, mag geen deel uit van het root-bestandssysteem. Deze mappen behoren tot de Azure data-schijven die zijn gekoppeld aan de virtuele machine (beschreven in de sectie 'Disk setup'). Deze aanpak helpt voorkomen dat het root-bestandssysteem geen ruimte meer is die wordt uitgevoerd. In de volgende schermafbeelding ziet u dat de systeembeheerder HANA gebruikers-ID heeft `1005` en maakt deel uit van de `sapsys` groep (ID `1001`) die is gedefinieerd vóór de installatie.

![Lijst van alle belangrijke SAP HANA-onderdelen eerder hebt geselecteerd](./media/hana-get-started/image032.jpg)

U kunt controleren de `\<HANA SID\>adm user` (`azdadm` in de volgende schermafbeelding) gegevens in de map/etc/passwd:

![HANA \<HANA SID\>adm gebruikersgegevens die worden vermeld in de map/etc/passwd](./media/hana-get-started/image033.jpg)

Nadat u een SAP HANA installeren met behulp van HDBLCM, ziet u de structuur in SAP HANA-Studio, zoals wordt weergegeven in de volgende schermafbeelding. Het schema SAPABAP1, die alle SAP NetWeaver tabellen bevat, is nog niet beschikbaar.

![Bestandsstructuur voor SAP HANA in SAP HANA-Studio](./media/hana-get-started/image034.jpg)

Nadat u SAP HANA installeert, kunt u SAP NetWeaver installeren toe. Zoals u in de volgende schermafbeelding, is de installatie als een gedistribueerde installatie uitgevoerd met behulp van SWPM (zoals beschreven in de vorige sectie). Wanneer u de database-exemplaar installeren met behulp van SWPM, voert u dezelfde gegevens met behulp van HDBLCM (bijvoorbeeld: hostnaam, HANA SID en exemplaarnummer). SWPM vervolgens gebruikt de bestaande installatie van de HANA en voegt meer schema's.

![Een gedistribueerde installatie uitgevoerd met behulp van SWPM](./media/hana-get-started/image035b.jpg)

De volgende schermafbeelding ziet de stap van de installatie SWPM waarin u gegevens over het schema DBACOCKPIT invoeren:

![De SWPM installatiestap waar DBACOCKPIT schemagegevens zijn ingevoerd](./media/hana-get-started/image036b.jpg)

Gegevens over het schema SAPABAP1 invoeren:

![Gegevens over het schema SAPABAP1 invoeren](./media/hana-get-started/image037b.jpg)

Nadat de installatie van de SWPM database-exemplaar is voltooid, ziet u het schema SAPABAP1 in SAP HANA Studio:

![Het schema SAPABAP1 in SAP HANA-Studio](./media/hana-get-started/image038b.jpg)

Ten slotte nadat de SAP-app-server en de SAP-GUI-installaties zijn voltooid, kunt u controleren de HANA-database-exemplaar met behulp van de **DBA Cockpit** transactie:

![Het geverifieerd aan de transactie DBA Cockpit HANA-database-exemplaar](./media/hana-get-started/image039b.jpg)


## <a name="sap-software-downloads"></a>SAP softwaredownloads
U kunt software downloaden van SAP Service Marketplace, zoals wordt weergegeven in de volgende schermafbeeldingen.

Download NetWeaver 7.5 voor Linux/HANA:

 ![SAP-Service-installatie en Upgrade-venster voor het downloaden van de NetWeaver 7.5](./media/hana-get-started/image001.jpg)

Download HANA SP12 Platform editie:

 ![SAP-Service-installatie en Upgrade-venster voor het downloaden van HANA SP12 Platform Edition](./media/hana-get-started/image002.jpg)

