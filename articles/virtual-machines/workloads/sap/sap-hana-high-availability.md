---
title: Hoge beschikbaarheid van SAP HANA op Azure VM's in SUSE Linux Enterprise Server | Microsoft Docs
description: Hoge beschikbaarheid van SAP HANA op Azure VM's in SUSE Linux Enterprise Server
services: virtual-machines-linux
documentationcenter: ''
author: MSSedusch
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 08/16/2018
ms.author: sedusch
ms.openlocfilehash: aca5b1613a6500b3aeca1a7074cabdce50023510
ms.sourcegitcommit: 295babdcfe86b7a3074fd5b65350c8c11a49f2f1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/27/2018
ms.locfileid: "53789497"
---
# <a name="high-availability-of-sap-hana-on-azure-vms-on-suse-linux-enterprise-server"></a>Hoge beschikbaarheid van SAP HANA op Azure VM's in SUSE Linux Enterprise Server

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md

[2205917]:https://launchpad.support.sap.com/#/notes/2205917
[1944799]:https://launchpad.support.sap.com/#/notes/1944799
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2388694]:https://launchpad.support.sap.com/#/notes/2388694
[401162]:https://launchpad.support.sap.com/#/notes/401162

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d
[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

U kunt voor de ontwikkeling van on-premises, hetzij HANA System Replication gebruiken of gedeelde opslag gebruiken om hoge beschikbaarheid voor SAP HANA stand te brengen.
Op Azure virtual machines (VM's) is HANA System Replication in Azure momenteel dat de enige ondersteunde functie voor hoge beschikbaarheid. SAP HANA replicatie bestaat uit één primair knooppunt en ten minste één secundaire knooppunt. Wijzigingen in de gegevens op het primaire knooppunt worden gerepliceerd naar het secundaire knooppunt synchroon of asynchroon.

Dit artikel wordt beschreven hoe u kunt implementeren en configureren van de virtuele machines, installeren van de cluster-framework, en installeert en configureert SAP HANA-Systeemreplicatie.
In de voorbeeldconfiguraties,-opdrachten voor installatie, exemplaarnummer **03**, en HANA-systeem-ID **HN1** worden gebruikt.

Lees eerst de volgende SAP-opmerkingen en documenten:

* SAP-notitie [1928533], heeft:
  * De lijst met Azure VM-grootten die worden ondersteund voor de implementatie van SAP-software.
  * Informatie over belangrijke capaciteit voor Azure VM-grootten.
  * De ondersteunde SAP-software en besturingssysteem (OS) en combinaties van de database.
  * De vereiste SAP kernelversie voor Windows en Linux op Microsoft Azure.
* SAP-notitie [2015553] worden de vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2205917] heeft aanbevolen instellingen voor het besturingssysteem voor SUSE Linux Enterprise Server voor SAP-toepassingen.
* SAP-notitie [1944799] SAP HANA-richtlijnen voor SUSE Linux Enterprise Server heeft voor SAP-toepassingen.
* SAP-notitie [2178632] vindt u meer informatie over alle van de bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1984787] bevat algemene informatie over het SUSE Linux Enterprise Server 12.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* SAP-notitie [401162] bevat informatie over hoe u om te voorkomen dat 'het adres is al in gebruik' bij het instellen van HANA System Replication.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) heeft alle van de vereiste SAP-opmerkingen voor Linux.
* [SAP HANA-gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure)
* [Azure virtuele Machines, planning en implementatie van SAP op Linux] [ planning-guide] handleiding.
* [Azure Virtual Machines-implementatie voor SAP op Linux] [ deployment-guide] (in dit artikel).
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux] [ dbms-guide] handleiding.
* [SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP3 best practice-richtlijnen][sles-for-sap-bp]
  * Instellen van een SAP HANA SR prestaties geoptimaliseerd-infrastructuur (SLES voor SAP-toepassingen 12 SP1). De handleiding bevat alle benodigde informatie kunt u SAP HANA-Systeemreplicatie instellen voor on-premises ontwikkeling. Deze handleiding gebruiken als een basislijn.
  * Instellen van een SAP HANA SR kosten geoptimaliseerd-infrastructuur (SLES voor SAP-toepassingen 12 SP1)

## <a name="overview"></a>Overzicht

SAP HANA is voor het bereiken van hoge beschikbaarheid, geïnstalleerd op de twee virtuele machines. De gegevens worden gerepliceerd via HANA System Replication.

![Overzicht van hoge beschikbaarheid van SAP HANA](./media/sap-hana-high-availability/ha-suse-hana.png)

SAP HANA-Systeemreplicatie setup maakt gebruik van een speciale virtuele hostnaam en het virtuele IP-adressen. In Azure, wordt een load balancer overstappen naar een virtueel IP-adres. De volgende lijst ziet u de configuratie van de load balancer:

* Front-end-configuratie: IP-adres 10.0.0.13 voor hn1-db
* Back-end-configuratie: Verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van HANA-Systeemreplicatie uitmaken
* Test-poort: Poort 62503
* Taakverdelingsregels: 30313 TCP, 30315 TCP, 30317 TCP

## <a name="deploy-for-linux"></a>Voor Linux implementeren

De resource-agent voor SAP HANA is opgenomen in SUSE Linux Enterprise Server voor SAP-toepassingen.
De Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor 12 van de SAP-toepassingen die u gebruiken kunt om nieuwe virtuele machines te implementeren.

### <a name="deploy-with-a-template"></a>Implementeren met een sjabloon

U kunt een van de Quick Start-sjablonen die zich op GitHub om de vereiste resources te implementeren. De sjabloon implementeert de virtuele machines, de load balancer, de beschikbaarheidsset, enzovoort.
Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [databasesjabloon] [ template-multisid-db] of de [geconvergeerd sjabloon] [ template-converged] in Azure portal. 
    De databasesjabloon maakt u de taakverdelingsregels voor alleen een database. Het geconvergeerde sjabloon maakt u ook de taakverdelingsregels voor een ASCS/SCS en INGEN (alleen Linux)-exemplaar. Als u van plan bent voor het installeren van een systeem op basis van SAP NetWeaver en u wilt de ASCS/SCS-exemplaar op de dezelfde machines installeren, gebruikt u de [geconvergeerd sjabloon][template-converged].

1. Voer de volgende parameters:
    - **SAP-systeem-ID**: Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
    - **Stack-Type**: (Deze parameter is alleen van toepassing als u de geconvergeerde sjabloon gebruiken.) Selecteer het type SAP NetWeaver-stack.
    - **Type besturingssysteem**: Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld **SLES 12**.
    - **Databasetype**: Selecteer **HANA**.
    - **SAP-systeem grootte**: Voer het nummer van SAP's die voor u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's van het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
    - **Beschikbaarheid van het systeem**: Selecteer **HA**.
    - **Gebruikersnaam voor de beheerder en het wachtwoord van beheerder**: Een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de machine.
    - **Nieuw of bestaand Subnet**: Bepaalt of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet gebruikt. Als u al een virtueel netwerk dat verbonden met uw on-premises netwerk hebt, selecteert u **bestaande**.
    - **Subnet-ID**: Als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID meestal, lijkt **/subscriptions/\<abonnements-ID > /resourceGroups/\<groepsnaam voor accountresources > /providers/Microsoft.Network/virtualNetworks/\<virtuele-netwerknaam > /subnets/ \<subnetnaam >**.

### <a name="manual-deployment"></a>Handmatige implementatie

> [!IMPORTANT]
> Zorg ervoor dat het besturingssysteem die u selecteert SAP gecertificeerd voor SAP HANA op de specifieke VM-typen die u gebruikt. De lijst met SAP HANA-gecertificeerde VM-typen en OS-versies voor die kunnen worden opgezocht [SAP HANA gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Zorg ervoor dat u klikt u op de details van het VM-type weergegeven als u de volledige lijst van SAP HANA ondersteund OS releases voor het specifieke VM-type
>  

1. Maak een resourcegroep.
1. Maak een virtueel netwerk.
1. Maak een beschikbaarheidsset.
   - Stel het maximale updatedomein.
1. Maak een load balancer (intern).
   - Selecteer het virtuele netwerk in stap 2 hebt gemaakt.
1. Virtuele machine 1 maken.
   - Een installatiekopie SLES4SAP gebruiken in de Azure-galerie die wordt ondersteund voor SAP HANA op het geselecteerde VM-type.
   - Selecteer de beschikbaarheidsset in stap 3 hebt gemaakt.
1. Maak de virtuele machine 2.
   - Een installatiekopie SLES4SAP gebruiken in de Azure-galerie die wordt ondersteund voor SAP HANA op het geselecteerde VM-type.
   - Selecteer de beschikbaarheidsset in stap 3 hebt gemaakt. 
1. Gegevensschijven toevoegen.
1. Configureer de load balancer. Maak eerst een front-end-IP-adresgroep:

   1. De load balancer openen, selecteert u **front-end-IP-adresgroep**, en selecteer **toevoegen**.
   1. Voer de naam van de nieuwe front-end-IP-adresgroep (bijvoorbeeld **hana-frontend**).
   1. Stel de **toewijzing** naar **statische** en voer het IP-adres (bijvoorbeeld **10.0.0.13**).
   1. Selecteer **OK**.
   1. Nadat de nieuwe front-end-IP-adresgroep is gemaakt, houd er rekening mee dat het IP-adres van toepassingen.

1. Maak vervolgens een back-end-groep:

   1. De load balancer openen, selecteert u **back-endpools**, en selecteer **toevoegen**.
   1. Voer de naam van de nieuwe back-end-pool (bijvoorbeeld **hana-back-end**).
   1. Selecteer **toevoegen van een virtuele machine**.
   1. Selecteer de beschikbaarheidsset in stap 3 hebt gemaakt.
   1. Selecteer de virtuele machines van de SAP HANA-cluster.
   1. Selecteer **OK**.

1. Maak vervolgens een statustest:

   1. De load balancer openen, selecteert u **statuscontroles**, en selecteer **toevoegen**.
   1. Voer de naam van de nieuwe statustest (bijvoorbeeld **hana-hp**).
   1. Selecteer **TCP** als het protocol en poort 625**03**. Houd de **Interval** waarde is ingesteld op 5, en de **drempelwaarde voor onjuiste status** waarde ingesteld op 2.
   1. Selecteer **OK**.

1. Voor SAP HANA 1.0, maken de regels voor taakverdeling:

   1. De load balancer openen, selecteert u **taakverdelingsregels**, en selecteer **toevoegen**.
   1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld hana-lb-3**03**15).
   1. Selecteer het front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **hana-frontend**).
   1. Houd de **Protocol** ingesteld op **TCP**, en voer de poort 3**03**15.
   1. Verhoog de **time-out voor inactiviteit** tot 30 minuten.
   1. Zorg ervoor dat u **Floating IP inschakelen**.
   1. Selecteer **OK**.
   1. Herhaal deze stappen voor poort 3**03**17.

1. Voor SAP HANA 2.0, maken de regels voor taakverdeling voor de database:

   1. De load balancer openen, selecteert u **taakverdelingsregels**, en selecteer **toevoegen**.
   1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld hana-lb-3**03**13).
   1. Selecteer het front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **hana-frontend**).
   1. Houd de **Protocol** ingesteld op **TCP**, en voer de poort 3**03**13.
   1. Verhoog de **time-out voor inactiviteit** tot 30 minuten.
   1. Zorg ervoor dat u **Floating IP inschakelen**.
   1. Selecteer **OK**.
   1. Herhaal deze stappen voor poort 3**03**14.

1. Voor SAP HANA 2.0, moet u eerst de regels voor taakverdeling voor de tenant-database maken:

   1. De load balancer openen, selecteert u **taakverdelingsregels**, en selecteer **toevoegen**.
   1. Voer de naam van de nieuwe load balancer-regel (bijvoorbeeld hana-lb-3**03**40).
   1. Selecteer de front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **hana-frontend**).
   1. Houd de **Protocol** ingesteld op **TCP**, en voer de poort 3**03**40.
   1. Verhoog de **time-out voor inactiviteit** tot 30 minuten.
   1. Zorg ervoor dat u **Floating IP inschakelen**.
   1. Selecteer **OK**.
   1. Herhaal deze stappen voor poorten 3**03**41 en 3**03**42.

Lees voor meer informatie over de vereiste poorten voor SAP HANA, het hoofdstuk [verbindingen met Tenantdatabases](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6/latest/en-US/7a9343c9f2a2436faa3cfdb5ca00c052.html) in de [SAP HANA-Tenantdatabases](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6) handleiding of [SAP-notitie 2388694][2388694].


## <a name="create-a-pacemaker-cluster"></a>Een cluster Pacemaker maken

Volg de stappen in [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](high-availability-guide-suse-pacemaker.md) om een eenvoudige Pacemaker-cluster voor deze HANA-server te maken. U kunt hetzelfde Pacemaker cluster gebruiken voor SAP HANA en SAP NetWeaver (A) SCS.

## <a name="install-sap-hana"></a>SAP HANA installeren

De stappen in deze sectie gebruikt u de volgende voorvoegsels:
- **[A]** : De stap is van toepassing op alle knooppunten.
- **[1]** : De stap is van toepassing op knooppunt 1 alleen.
- **[2]** : De stap is van toepassing op knooppunt 2 van het cluster Pacemaker alleen.

1. **[A]**  Instellen van de schijfindeling: **Logical Volume Manager (LVM)**.

   U wordt aangeraden LVM voor volumes die opslaan van gegevens en logboekbestanden. Het volgende voorbeeld wordt ervan uitgegaan dat de virtuele machines vier gegevensschijven die zijn gekoppeld die worden gebruikt om twee volumes te maken.

   Overzicht van de beschikbare schijven:

   <pre><code>ls /dev/disk/azure/scsi1/lun*
   </code></pre>

   Voorbeelduitvoer:

   <pre><code>
   /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
   </code></pre>

   Maak fysieke volumes voor alle schijven die u wilt gebruiken:

   <pre><code>sudo pvcreate /dev/disk/azure/scsi1/lun0
   sudo pvcreate /dev/disk/azure/scsi1/lun1
   sudo pvcreate /dev/disk/azure/scsi1/lun2
   sudo pvcreate /dev/disk/azure/scsi1/lun3
   </code></pre>

   Maak een volumegroep voor de gegevensbestanden. Gebruik één volumegroep voor de logboekbestanden en één voor de gedeelde map van SAP HANA:

   <pre><code>sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
   sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
   sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
   </code></pre>

   De logische volumes maken. Een lineaire volume wordt gemaakt wanneer u `lvcreate` zonder de `-i` overschakelen. Het is raadzaam dat u een striped volumes voor betere i/o-prestaties maakt, waar de `-i` argument moet het nummer van de onderliggende fysieke volume. In dit document, twee fysieke volumes worden gebruikt voor het gegevensvolume, zodat de `-i` switch argument is ingesteld op **2**. Een fysieke volume wordt gebruikt voor het logboekvolume, zodat er `-i` switch expliciet wordt gebruikt. Gebruik de `-i` overschakelen en stel deze in op het nummer van de onderliggende fysieke volume wanneer u meer dan één fysieke volume voor elke gegevens gebruiken, logboek of de gedeelde volumes.

   <pre><code>sudo lvcreate <b>-i 2</b> -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
   sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
   sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
   sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
   sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
   sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
   </code></pre>

   Maak de mount-mappen en kopieer de UUID van alle logische volumes:

   <pre><code>sudo mkdir -p /hana/data/<b>HN1</b>
   sudo mkdir -p /hana/log/<b>HN1</b>
   sudo mkdir -p /hana/shared/<b>HN1</b>
   # Write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log, and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
   sudo blkid
   </code></pre>

   Maak `fstab` vermeldingen voor de drie logische volumes:       

   <pre><code>sudo vi /etc/fstab
   </code></pre>

   Voeg de volgende regel in de `/etc/fstab` bestand:      

   <pre><code>/dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
   /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
   /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
   </code></pre>

   De nieuwe volumes koppelen:

   <pre><code>sudo mount -a
   </code></pre>

1. **[A]**  Instellen van de schijfindeling: **Plain schijven**.

   Voor systemen, demo, kunt u uw HANA-gegevens en logboekbestanden bestanden op één schijf plaatsen. Maken van een partitie op /dev/disk/azure/scsi1/lun0 en formatteert u het met xfs:

   <pre><code>sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
   sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
   
   # Write down the ID of /dev/disk/azure/scsi1/lun0-part1
   sudo /sbin/blkid
   sudo vi /etc/fstab
   </code></pre>

   Deze regel invoegen in het bestand/etc/fstab:

   <pre><code>/dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
   </code></pre>

   De doeldirectory maken en koppelen van de schijf:

   <pre><code>sudo mkdir /hana
   sudo mount -a
   </code></pre>

1. **[A]**  Omzetten van de hostnaam voor alle hosts ingesteld.

   U kunt een DNS-server gebruiken of wijzigen van het bestand/etc/hosts op alle knooppunten. In dit voorbeeld ziet u hoe u het bestand/etc/hosts.
   Vervang het IP-adres en de hostnaam in de volgende opdrachten:

   <pre><code>sudo vi /etc/hosts
   </code></pre>

   Voeg de volgende regels in het bestand/etc/hosts. Wijzig het IP-adres en hostnaam zodat deze overeenkomen met uw omgeving:

   <pre><code><b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. **[A]**  De SAP HANA-pakketten voor hoge beschikbaarheid installeren:

   <pre><code>sudo zypper install SAPHanaSR
   </code></pre>

Volg voor het installeren van SAP HANA-Systeemreplicatie hoofdstuk 4 van de [SAP HANA SR prestaties geoptimaliseerd Scenariogids voor](https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/).

1. **[A]**  Uitvoeren de **hdblcm** programma vanaf de DVD met HANA. Voer de volgende waarden bij de opdrachtprompt:
   * -Installatie te kiezen: Voer **1**.
   * Selecteer extra onderdelen voor installatie: Voer **1**.
   * Voer Installation Path [/ hana/gedeelde]: Selecteer invoeren.
   * Voer de naam van de lokale Host [.]: Selecteer invoeren.
   * Wilt u extra hosts toevoegen aan het systeem? (j/n) [n]: Selecteer invoeren.
   * Voer SAP HANA-systeem-ID: Voer bijvoorbeeld de SID van HANA: **HN1**.
   * Voer exemplaarnummer [00]: Voer het nummer van de HANA-instantie. Voer **03** als u de Azure-sjabloon gebruikt of de handmatige implementatie-sectie van dit artikel wordt gevolgd.
   * Selecteer Database modus / Index [1] invoeren: Selecteer invoeren.
   * Selecteer het gebruik van / Index [4] invoeren: Selecteer de waarde van de gebruik system.
   * Geef de locatie van gegevensvolumes [/ hana/data/HN1]: Selecteer invoeren.
   * Geef de locatie van de Logboekvolumes [/ hana/log/HN1]: Selecteer invoeren.
   * Beperk de maximale hoeveelheid geheugen ingesteld? [n]: Selecteer invoeren.
   * Voer de hostnaam certificaat voor Host '...' [...]: Selecteer invoeren.
   * Voer SAP Host Agent-gebruiker (sapadm) wachtwoord: Voer het wachtwoord van de host agent-gebruiker.
   * SAP-Host Agent-gebruiker (sapadm) wachtwoord bevestigen: Voer het wachtwoord van de host-agent gebruiker opnieuw te bevestigen.
   * Geef de systeembeheerder (hdbadm) wachtwoord: Voer het wachtwoord voor de systeembeheerder.
   * Controleer of de systeembeheerder (hdbadm) wachtwoord: Voer het beheerderswachtwoord van systeem opnieuw te bevestigen.
   * Voer System Administrator basismap [/ usr/sap/HN1/home]: Selecteer invoeren.
   * Voer System Administrator aanmeldingsshell [/ bin/sh]: Selecteer invoeren.
   * Voer de gebruikersnaam van systeembeheerder [1001]: Selecteer invoeren.
   * Voer-ID van de gebruikersgroep (sapsys) [79]: Selecteer invoeren.
   * Geef het wachtwoord voor Database-gebruiker (systeem): Voer het wachtwoord van de database-gebruiker.
   * Bevestig het wachtwoord voor Database-gebruiker (systeem): Voer het wachtwoord van de gebruiker database opnieuw te bevestigen.
   * Systeem opnieuw opstarten na opnieuw opstarten van machine? [n]: Selecteer invoeren.
   * Wilt u doorgaan? (j/n): Valideer de samenvatting. Voer **y** om door te gaan.

1. **[A]**  De SAP-Host-Agent bijwerken.

   Download het nieuwste SAP Host Agent-archief van de [SAP Software Center] [ sap-swcenter] en voer de volgende opdracht uit de agent bij te werken. Vervang het pad naar het archief om te verwijzen naar het bestand dat u hebt gedownload:

   <pre><code>sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive &lt;path to SAP Host Agent SAR&gt;
   </code></pre>

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 System Replication configureren

De stappen in deze sectie gebruikt u de volgende voorvoegsels:

* **[A]** : De stap is van toepassing op alle knooppunten.
* **[1]** : De stap is van toepassing op knooppunt 1 alleen.
* **[2]** : De stap is van toepassing op knooppunt 2 van het cluster Pacemaker alleen.

1. **[1]**  Maken van de tenant-database.

   Als u van SAP HANA 2.0 of MDC gebruikmaakt, maakt u een tenantdatabase voor uw SAP NetWeaver-systeem. Vervang **NW1** met de SID van uw SAP-systeem.

   Meld u aan als \<hanasid > adm en voer de volgende opdracht uit:

   <pre><code>hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]**  System-replicatie configureren op het eerste knooppunt:

   Meld u aan als \<hanasid > adm en back-up van de databases:

   <pre><code>hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   Kopieer de bestanden van de PKI systeem naar de secundaire site:

   <pre><code>scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT   <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY  <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   Maken van de primaire site:

   <pre><code>hdbnsutil -sr_enable --name=<b>SITE1</b>
   </code></pre>

1. **[2]**  System-replicatie configureren op het tweede knooppunt:
    
   Registreer het tweede knooppunt voor het starten van de systeemreplicatie. Meld u aan als \<hanasid > adm en voer de volgende opdracht uit:

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 System Replication configureren

De stappen in deze sectie gebruikt u de volgende voorvoegsels:

* **[A]** : De stap is van toepassing op alle knooppunten.
* **[1]** : De stap is van toepassing op knooppunt 1 alleen.
* **[2]** : De stap is van toepassing op knooppunt 2 van het cluster Pacemaker alleen.

1. **[1]**  Maken van de juiste gebruikers.

   Meld u aan als hoofdgebruiker en voer de volgende opdracht uit. Zorg ervoor dat u het vervangen van vet tekenreeksen (HANA System ID **HN1** en het exemplaarnummer **03**) door de waarden van uw SAP HANA-installatie:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"'
   hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync'
   hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME'
   </code></pre>

1. **[A]**  De sleutelopslagvermelding maken.

   Meld u aan als hoofdgebruiker en voer de volgende opdracht om te maken van een nieuw sleutelopslagvermelding:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
   </code></pre>

1. **[1]**  Maakt u een back-up van de database.

   Meld u aan als hoofdgebruiker en back-up van de databases:

   <pre><code>PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Als u een installatie voor meerdere tenants, ook een back-up van de tenantdatabase:

   <pre><code>hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]**  System-replicatie configureren op het eerste knooppunt.

   Meld u aan als \<hanasid > adm en de primaire site te maken:

   <pre><code>su - <b>hdb</b>adm
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  System-replicatie configureren op het tweede knooppunt.

   Meld u aan als \<hanasid > adm en registreert u de secundaire site:

   <pre><code>sapcontrol -nr <b>03</b> -function StopWait 600 10
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
   </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>Resources voor SAP HANA-cluster maken

Maak eerst de HANA-topologie. Voer de volgende opdrachten op een van de clusterknooppunten Pacemaker:

<pre><code>sudo crm configure property maintenance-mode=true

# Replace the bold string with your instance number and HANA system ID

sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
  operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
  op monitor interval="10" timeout="600" \
  op start interval="0" timeout="600" \
  op stop interval="0" timeout="300" \
  params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"

sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
  meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code></pre>

Maak vervolgens de HANA-resources:

<pre><code># Replace the bold string with your instance number, HANA system ID, and the front-end IP address of the Azure load balancer. 

sudo crm configure primitive rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHana \
  operations \$id="rsc_sap_<b>HN1</b>_HDB<b>03</b>-operations" \
  op start interval="0" timeout="3600" \
  op stop interval="0" timeout="3600" \
  op promote interval="0" timeout="3600" \
  op monitor interval="60" role="Master" timeout="700" \
  op monitor interval="61" role="Slave" timeout="700" \
  params SID="<b>HN1</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
  DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

sudo crm configure ms msl_SAPHana_<b>HN1</b>_HDB<b>03</b> rsc_SAPHana_<b>HN1</b>_HDB<b>03</b> \
  meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
  target-role="Started" interleave="true"

sudo crm configure primitive rsc_ip_<b>HN1</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \
  meta target-role="Started" is-managed="true" \
  operations \$id="rsc_ip_<b>HN1</b>_HDB<b>03</b>-operations" \
  op monitor interval="10s" timeout="20s" \
  params ip="<b>10.0.0.13</b>"

sudo crm configure primitive rsc_nc_<b>HN1</b>_HDB<b>03</b> anything \
  params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \
  op monitor timeout=20s interval=10 depth=0

sudo crm configure group g_ip_<b>HN1</b>_HDB<b>03</b> rsc_ip_<b>HN1</b>_HDB<b>03</b> rsc_nc_<b>HN1</b>_HDB<b>03</b>

sudo crm configure colocation col_saphana_ip_<b>HN1</b>_HDB<b>03</b> 4000: g_ip_<b>HN1</b>_HDB<b>03</b>:Started \
  msl_SAPHana_<b>HN1</b>_HDB<b>03</b>:Master  

sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> Optional: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
  msl_SAPHana_<b>HN1</b>_HDB<b>03</b>

# Clean up the HANA resources. The HANA resources might have failed because of a known issue.
sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

sudo crm configure property maintenance-mode=false
sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

Zorg ervoor dat de clusterstatus ok is en dat alle resources die worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

<pre><code>sudo crm_mon -r

# Online: [ hn1-db-0 hn1-db-1 ]
#
# Full list of resources:
#
# stonith-sbd     (stonith:external/sbd): Started hn1-db-0
# rsc_st_azure    (stonith:fence_azure_arm):      Started hn1-db-1
# Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
#     Started: [ hn1-db-0 hn1-db-1 ]
# Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
#     Masters: [ hn1-db-0 ]
#     Slaves: [ hn1-db-1 ]
# Resource Group: g_ip_HN1_HDB03
#     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
#     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
</code></pre>

## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

Deze sectie wordt beschreven hoe u uw instellingen kunt testen. Elke test wordt ervan uitgegaan dat u hoofdmap en de SAP HANA-master wordt uitgevoerd op de **hn1-db-0** virtuele machine.

### <a name="test-the-migration"></a>Testen van de migratie

Voordat u de test, zorg dat Pacemaker heeft geen een mislukte actie (via crm_mon - r), zijn er geen Locatiebeperkingen voor onverwachte (bijvoorbeeld restanten van een migratie-test) en dat HANA synchronisatiestatus, bijvoorbeeld met SAPHanaSR showAttr:

<pre><code>hn1-db-0:~ # SAPHanaSR-showAttr

Global cib-time
--------------------------------
global Mon Aug 13 11:26:04 2018

Hosts    clone_state lpa_hn1_lpt node_state op_mode   remoteHost    roles                            score site  srmode sync_state version                vhost
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------
hn1-db-0 PROMOTED    1534159564  online     logreplay nws-hana-vm-1 4:P:master1:master:worker:master 150   SITE1 sync   PRIM       2.00.030.00.1522209842 nws-hana-vm-0
hn1-db-1 DEMOTED     30          online     logreplay nws-hana-vm-0 4:S:master1:master:worker:master 100   SITE2 sync   SOK        2.00.030.00.1522209842 nws-hana-vm-1
</code></pre>

U kunt het SAP HANA-hoofdknooppunt migreren door het uitvoeren van de volgende opdracht:

<pre><code>crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

Als u `AUTOMATED_REGISTER="false"`, deze reeks opdrachten moet migreren de SAP HANA-hoofdknooppunt en de groep waarin het virtuele IP-adres hn1-db-1.

Zodra de migratie is voltooid, de crm_mon - r-uitvoer er als volgt uitzien

<pre><code>Online: [ hn1-db-0 hn1-db-1 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started hn1-db-1
 Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
     Started: [ hn1-db-0 hn1-db-1 ]
 Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
     Masters: [ hn1-db-1 ]
     Stopped: [ hn1-db-0 ]
 Resource Group: g_ip_HN1_HDB03
     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1

Failed Actions:
* rsc_SAPHana_HN1_HDB03_start_0 on hn1-db-0 'not running' (7): call=84, status=complete, exitreason='none',
    last-rc-change='Mon Aug 13 11:31:37 2018', queued=0ms, exec=2095ms
</code></pre>

De SAP HANA-resource op hn1-db-0 niet gestart als secundaire cache. In dit geval de HANA-exemplaar als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr <b>03</b> -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>
</code></pre>

De migratie maakt Locatiebeperkingen moeten opnieuw worden verwijderd:

<pre><code># Switch back to root and clean up the failed state
exit
hn1-db-0:~ # crm resource unmigrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
</code></pre>

U moet ook om de status van de resource van het secundaire knooppunt op te schonen:

<pre><code>hn1-db-0:~ # crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

De status van de HANA-resource met behulp van crm_mon - r bewaken. Wanneer HANA is gestart op hn1-db-0, ziet de uitvoer er als volgt

<pre><code>Online: [ hn1-db-0 hn1-db-1 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started hn1-db-1
 Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
     Started: [ hn1-db-0 hn1-db-1 ]
 Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
     Masters: [ hn1-db-1 ]
     Slaves: [ hn1-db-0 ]
 Resource Group: g_ip_HN1_HDB03
     rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
     rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
</code></pre>

### <a name="test-the-azure-fencing-agent-not-sbd"></a>Testen van de agent voor de eerste optie Azure (niet SBD)

U kunt de installatie van de agent voor de eerste optie Azure testen door het uitschakelen van de netwerkinterface op het knooppunt hn1-db-0:

<pre><code>sudo ifdown eth0
</code></pre>

De virtuele machine moet nu opnieuw starten of stoppen, afhankelijk van uw clusterconfiguratie.
Als u de `stonith-action` is ingesteld op uitgeschakeld, de virtuele machine wordt gestopt en de bronnen worden gemigreerd naar de actieve virtuele machine.

Nadat u de virtuele machine opnieuw starten, de SAP HANA-resource niet kan worden gestart als secundaire cache als u `AUTOMATED_REGISTER="false"`. In dit geval de HANA-exemplaar als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# Switch back to root and clean up the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

### <a name="test-sbd-fencing"></a>De eerste optie SBD testen

U kunt de installatie van SBD testen door het proces inquisitor doden.

<pre><code>hn1-db-0:~ # ps aux | grep sbd
root       1912  0.0  0.0  85420 11740 ?        SL   12:25   0:00 sbd: inquisitor
root       1929  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-360014056f268462316e4681b704a9f73 - slot: 0 - uuid: 7b862dba-e7f7-4800-92ed-f76a4e3978c8
root       1930  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-360014059bc9ea4e4bac4b18808299aaf - slot: 0 - uuid: 5813ee04-b75c-482e-805e-3b1e22ba16cd
root       1931  0.0  0.0  85456 11776 ?        SL   12:25   0:00 sbd: watcher: /dev/disk/by-id/scsi-36001405b8dddd44eb3647908def6621c - slot: 0 - uuid: 986ed8f8-947d-4396-8aec-b933b75e904c
root       1932  0.0  0.0  90524 16656 ?        SL   12:25   0:00 sbd: watcher: Pacemaker
root       1933  0.0  0.0 102708 28260 ?        SL   12:25   0:00 sbd: watcher: Cluster
root      13877  0.0  0.0   9292  1572 pts/0    S+   12:27   0:00 grep sbd

hn1-db-0:~ # kill -9 1912
</code></pre>

Cluster-knooppunt hn1-db-0 moet opnieuw worden opgestart. De Pacemaker-service mogelijk niet aan de slag daarna. Zorg ervoor dat u start het opnieuw.

### <a name="test-a-manual-failover"></a>Een handmatige failover testen

U kunt een handmatige failover testen door te stoppen de `pacemaker` service op het knooppunt hn1-db-0:

<pre><code>service pacemaker stop
</code></pre>

U kunt de service opnieuw starten na de failover. Als u `AUTOMATED_REGISTER="false"`, de SAP HANA-resource op het knooppunt hn1-db-0 niet kan worden gestart als secundaire cache. In dit geval de HANA-exemplaar als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 

# Switch back to root and clean up the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

### <a name="suse-tests"></a>SUSE-tests

> [!IMPORTANT]
> Zorg ervoor dat het besturingssysteem die u selecteert SAP gecertificeerd voor SAP HANA op de specifieke VM-typen die u gebruikt. De lijst met SAP HANA-gecertificeerde VM-typen en OS-versies voor die kunnen worden opgezocht [SAP HANA gecertificeerde IaaS-platformen](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure). Zorg ervoor dat u klikt u op de details van het VM-type weergegeven als u de volledige lijst van SAP HANA ondersteund OS releases voor het specifieke VM-type

Voer alle testcases die worden vermeld in de SAP HANA SR prestaties geoptimaliseerd Scenario of SAP HANA SR kosten geoptimaliseerd Scenario guide, afhankelijk van uw situatie. U kunt de handleidingen vinden op de [SLES voor SAP best practices pagina][sles-for-sap-bp].

De volgende tests uit zijn een kopie van de test-beschrijvingen van de SAP HANA SR prestaties geoptimaliseerd Scenario SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP1 handleiding. Lees voor een actuele versie altijd ook de handleiding zelf. Zorg er altijd HANA is gesynchroniseerd voordat u begint met de test en ook voor zorgen dat de configuratie van de Pacemaker juist is.

In de volgende beschrijvingen van de test Aannemende PREFER_SITE_TAKEOVER = "true" en AUTOMATED_REGISTER = "false".
OPMERKING: De volgende tests zijn ontworpen om te worden uitgevoerd in de volgorde en afhankelijk van de status van het afsluiten van de voorgaande tests.

1. TEST 1: STOP PRIMAIRE DATABASE OP KNOOPPUNT 1

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Voer de volgende opdrachten als \<hanasid > adm op knooppunt hn1-db-0:

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Pacemaker moet vaststellen van de gestopte HANA-instantie en failover naar het andere knooppunt. Nadat de failover is voltooid, wordt de HANA-instantie op knooppunt hn1-db-0 is gestopt omdat Pacemaker het knooppunt niet automatisch geregistreerd als secundaire HANA.

   Voer de volgende opdrachten om knooppunt hn1-db-0 als secundaire en opschonen de mislukte resource te registreren.

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

1. TEST 2: STOP PRIMAIRE DATABASE OP KNOOPPUNT 2

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

   Voer de volgende opdrachten als \<hanasid > adm op knooppunt hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Pacemaker moet vaststellen van de gestopte HANA-instantie en failover naar het andere knooppunt. Nadat de failover is voltooid, wordt de HANA-instantie op knooppunt hn1-db-1 is gestopt omdat Pacemaker het knooppunt niet automatisch geregistreerd als secundaire HANA.

   Voer de volgende opdrachten om knooppunt hn1-db-1 als secundaire en opschonen de mislukte resource te registreren.

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 3: CRASH PRIMAIRE DATABASE OP KNOOPPUNT

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Voer de volgende opdrachten als \<hanasid > adm op knooppunt hn1-db-0:

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>
   
   Pacemaker moet vaststellen van de afgesloten HANA-instantie en failover naar het andere knooppunt. Nadat de failover is voltooid, wordt de HANA-instantie op knooppunt hn1-db-0 is gestopt omdat Pacemaker het knooppunt niet automatisch geregistreerd als secundaire HANA.

   Voer de volgende opdrachten om knooppunt hn1-db-0 als secundaire en opschonen de mislukte resource te registreren.

   <pre><code>hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

1. TEST 4: CRASH PRIMAIRE DATABASE OP KNOOPPUNT 2

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

   Voer de volgende opdrachten als \<hanasid > adm op knooppunt hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>

   Pacemaker moet vaststellen van de afgesloten HANA-instantie en failover naar het andere knooppunt. Nadat de failover is voltooid, wordt de HANA-instantie op knooppunt hn1-db-1 is gestopt omdat Pacemaker het knooppunt niet automatisch geregistreerd als secundaire HANA.

   Voer de volgende opdrachten om knooppunt hn1-db-1 als secundaire en opschonen de mislukte resource te registreren.

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 5: CRASH VAN DE PRIMAIRE SITE-KNOOPPUNT (1 KNOOPPUNT)

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Voer de volgende opdrachten als root op knooppunt hn1-db-0:

   <pre><code>hn1-db-0:~ #  echo 'b' > /proc/sysrq-trigger
   </code></pre>

   Pacemaker moet het afgesloten clusterknooppunt worden gedetecteerd en het knooppunt omheining. Wanneer het knooppunt omheinde is, activeert Pacemaker een overname van de HANA-instantie. Wanneer het omheinde knooppunt opnieuw wordt opgestart, worden Pacemaker niet automatisch gestart.

   Voer de volgende opdrachten om te beginnen Pacemaker, meld schoon de berichten SBD voor knooppunt hn1-db-0, u aan knooppunt hn1-db-0 als secundaire en opschonen de mislukte resource.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-0:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-0:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-0 clear
   
   hn1-db-0:~ # systemctl start pacemaker
   
   # run as &lt;hanasid&gt;adm
   hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMode=sync --name=SITE1
   
   # run as root
   hn1-db-0:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-0
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

1. TEST 6: SECUNDAIRE SITE-KNOOPPUNT (KNOOPPUNT 2) LOOPT VAST

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-1 ]
      Slaves: [ hn1-db-0 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-1
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-1
   </code></pre>

   Voer de volgende opdrachten als root op knooppunt hn1-db-1:

   <pre><code>hn1-db-1:~ #  echo 'b' > /proc/sysrq-trigger
   </code></pre>

   Pacemaker moet het afgesloten clusterknooppunt worden gedetecteerd en het knooppunt omheining. Wanneer het knooppunt omheinde is, activeert Pacemaker een overname van de HANA-instantie. Wanneer het omheinde knooppunt opnieuw wordt opgestart, worden Pacemaker niet automatisch gestart.

   Voer de volgende opdrachten om te beginnen Pacemaker, meld schoon de berichten SBD voor knooppunt hn1-db-1, u aan knooppunt hn1-db-1 als secundaire en opschonen de mislukte resource.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-1:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-1:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-1 clear
   
   hn1-db-1:~ # systemctl start pacemaker
   
   # run as &lt;hanasid&gt;adm
   hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-0 --remoteInstance=03 --replicationMode=sync --name=SITE2
   
   # run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 7: STOP DE SECUNDAIRE DATABASE OP KNOOPPUNT 2

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Voer de volgende opdrachten als \<hanasid > adm op knooppunt hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB stop
   </code></pre>

   Pacemaker detecteert de gestopte HANA-instantie en de bron is mislukt op knooppunt hn1-db-1 markeren. Pacemaker moet automatisch de HANA-exemplaar opnieuw opstarten. Voer de volgende opdracht voor het opschonen van de status mislukt.

   <pre><code># run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 8: DE SECUNDAIRE DATABASE OP KNOOPPUNT 2 CRASHES

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Voer de volgende opdrachten als \<hanasid > adm op knooppunt hn1-db-1:

   <pre><code>hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> HDB kill-9
   </code></pre>

   Pacemaker detecteert de afgesloten HANA-instantie en de bron is mislukt op knooppunt hn1-db-1 markeren. Voer de volgende opdracht voor het opschonen van de status mislukt. Pacemaker moet vervolgens automatisch opnieuw opgestart de HANA-instantie.

   <pre><code># run as root
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

1. TEST 9: CRASH SECUNDAIRE HANA-DATABASE VAN SECUNDAIRE SITE-KNOOPPUNT (KNOOPPUNT 2) DIE WORDT UITGEVOERD

   De resourcestatus van de voordat u begint met de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

   Voer de volgende opdrachten als root op knooppunt hn1-db-1:

   <pre><code>hn1-db-1:~ # echo b > /proc/sysrq-trigger
   </code></pre>

   Pacemaker moet het afgesloten clusterknooppunt worden gedetecteerd en het knooppunt omheining. Wanneer het omheinde knooppunt opnieuw wordt opgestart, worden Pacemaker niet automatisch gestart.

   Voer de volgende opdrachten om te starten Pacemaker, opschonen van de berichten SBD voor knooppunt hn1-db-1 en opschonen de mislukte resource.

   <pre><code># run as root
   # list the SBD device(s)
   hn1-db-1:~ # cat /etc/sysconfig/sbd | grep SBD_DEVICE=
   # SBD_DEVICE="/dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116;/dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1;/dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3"
   
   hn1-db-1:~ # sbd -d /dev/disk/by-id/scsi-36001405772fe8401e6240c985857e116 -d /dev/disk/by-id/scsi-36001405034a84428af24ddd8c3a3e9e1 -d /dev/disk/by-id/scsi-36001405cdd5ac8d40e548449318510c3 message hn1-db-1 clear
   
   hn1-db-1:~ # systemctl start pacemaker  
   
   hn1-db-1:~ # crm resource cleanup msl_SAPHana_HN1_HDB03 hn1-db-1
   </code></pre>

   De resourcestatus van de nadat de test:

   <pre><code>Clone Set: cln_SAPHanaTopology_HN1_HDB03 [rsc_SAPHanaTopology_HN1_HDB03]
      Started: [ hn1-db-0 hn1-db-1 ]
   Master/Slave Set: msl_SAPHana_HN1_HDB03 [rsc_SAPHana_HN1_HDB03]
      Masters: [ hn1-db-0 ]
      Slaves: [ hn1-db-1 ]
   Resource Group: g_ip_HN1_HDB03
      rsc_ip_HN1_HDB03   (ocf::heartbeat:IPaddr2):       Started hn1-db-0
      rsc_nc_HN1_HDB03   (ocf::heartbeat:anything):      Started hn1-db-0
   </code></pre>

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md)
