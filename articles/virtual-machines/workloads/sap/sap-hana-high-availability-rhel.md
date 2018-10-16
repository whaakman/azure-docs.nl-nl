---
title: SAP HANA-Systeemreplicatie instellen op Azure virtual machines (VM's) | Microsoft Docs
description: Hoge beschikbaarheid van SAP HANA op Azure virtual machines (VM's) maken.
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
ms.openlocfilehash: 77f4eeec1aa87f42c90d4e93f98f460a8b54b9a9
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2018
ms.locfileid: "49167406"
---
# <a name="high-availability-of-sap-hana-on-azure-vms-on-red-hat-enterprise-linux"></a>Hoge beschikbaarheid van SAP HANA op Azure Virtual machines van Red Hat Enterprise Linux

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
[2292690]:https://launchpad.support.sap.com/#/notes/2292690
[2455582]:https://launchpad.support.sap.com/#/notes/2455582
[2002167]:https://launchpad.support.sap.com/#/notes/2002167
[2009879]:https://launchpad.support.sap.com/#/notes/2009879

[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json

U kunt voor de ontwikkeling van on-premises, hetzij HANA System Replication gebruiken of gedeelde opslag gebruiken om hoge beschikbaarheid voor SAP HANA stand te brengen.
Op Azure virtual machines (VM's) is HANA System Replication in Azure momenteel dat de enige ondersteunde functie voor hoge beschikbaarheid.
SAP HANA replicatie bestaat uit één primair knooppunt en ten minste één secundaire knooppunt. Wijzigingen in de gegevens op het primaire knooppunt worden gerepliceerd naar het secundaire knooppunt synchroon of asynchroon.

Dit artikel wordt beschreven hoe u kunt implementeren en configureren van de virtuele machines, installeren van de cluster-framework, en installeert en configureert SAP HANA-Systeemreplicatie.
In de voorbeeldconfiguraties,-opdrachten voor installatie, exemplaarnummer **03**, en HANA-systeem-ID **HN1** worden gebruikt.

Lees eerst de volgende SAP-opmerkingen en documenten:

* SAP-notitie [1928533], heeft:
  * De lijst met Azure VM-grootten die worden ondersteund voor de implementatie van SAP-software.
  * Informatie over belangrijke capaciteit voor Azure VM-grootten.
  * De ondersteunde SAP-software en besturingssysteem (OS) en combinaties van de database.
  * De vereiste SAP kernelversie voor Windows en Linux op Microsoft Azure.
* SAP-notitie [2015553] bevat vereisten voor SAP-ondersteunde SAP software-implementaties in Azure.
* SAP-notitie [2002167] heeft aanbevolen instellingen voor Red Hat Enterprise Linux OS
* SAP-notitie [2009879] bevat richtlijnen voor SAP HANA voor Red Hat Enterprise Linux
* SAP-notitie [2178632] vindt u meer informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste versie van de SAP-Host-Agent voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over de licentieverlening voor SAP op Linux in Azure.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure uitgebreide controle-extensie voor SAP.
* [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) bevat alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie van SAP op Linux][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP op Linux (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [SAP HANA-systeemreplicatie in pacemaker cluster](https://access.redhat.com/articles/3004101)
* Algemene RHEL-documentatie
  * [Overzicht van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index)
  * [Beheer van de invoegtoepassing voor hoge beschikbaarheid](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index)
  * [Naslaginformatie over hoge beschikbaarheid-invoegtoepassing](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index)
* Azure specifieke RHEL-documentatie:
  * [Ondersteuningsbeleid voor RHEL Clusters met hoge beschikbaarheid - Microsoft Azure Virtual Machines als leden van het Cluster](https://access.redhat.com/articles/3131341)
  * [Installeren en configureren van een Red Hat Enterprise Linux 7.4 (en hoger) Cluster met hoge beschikbaarheid voor Microsoft Azure](https://access.redhat.com/articles/3252491)
  * [SAP HANA op Red Hat Enterprise Linux installeren voor gebruik in Microsoft Azure](https://access.redhat.com/solutions/3193782)

## <a name="overview"></a>Overzicht

SAP HANA is voor het bereiken van hoge beschikbaarheid, geïnstalleerd op de twee virtuele machines. De gegevens worden gerepliceerd via HANA System Replication.

![Overzicht van hoge beschikbaarheid van SAP HANA](./media/sap-hana-high-availability-rhel/ha-hana.png)

SAP HANA-Systeemreplicatie setup maakt gebruik van een speciale virtuele hostnaam en het virtuele IP-adressen. In Azure, wordt een load balancer overstappen naar een virtueel IP-adres. De volgende lijst ziet u de configuratie van de load balancer:

* Front-end-configuratie: IP-adres 10.0.0.13 voor hn1-db
* Back-end-configuratie: verbonden met primaire netwerkinterfaces van alle virtuele machines die deel van HANA-Systeemreplicatie uitmaken
* Testpoort: Poort 62503
* Regels voor taakverdeling: 30313 TCP, 30315 TCP, 30317 TCP, 30340 TCP, 30341 TCP, 30342 TCP

## <a name="deploy-for-linux"></a>Voor Linux implementeren

De Azure Marketplace bevat een afbeelding voor Red Hat Enterprise Linux 7.4 voor SAP HANA die u gebruiken kunt om nieuwe virtuele machines te implementeren.

### <a name="deploy-with-a-template"></a>Implementeren met een sjabloon

U kunt een van de Quick Start-sjablonen die zich op GitHub om de vereiste resources te implementeren. De sjabloon implementeert de virtuele machines, de load balancer, de beschikbaarheidsset, enzovoort.
Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [databasesjabloon] [ template-multisid-db] in Azure portal.
1. Voer de volgende parameters:
    * **SAP-systeem-ID**: Voer de SAP-systeem-ID van de SAP-systeem die u wilt installeren. De ID wordt gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
    * **Type besturingssysteem**: Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld **RHEL 7**.
    * **Databasetype**: Selecteer **HANA**.
    * **SAP-systeem grootte**: Voer het aantal SAP's die voor u het nieuwe systeem. Als u niet zeker weet hoeveel SAP's van het systeem is vereist, vraagt u uw SAP-technologie-Partner of systeemintegrator.
    * **Beschikbaarheid van het systeem**: Selecteer **HA**.
    * **Admin Username, Administrator-wachtwoord of SSH-sleutel**: een nieuwe gebruiker wordt gemaakt die kan worden gebruikt voor aanmelding bij de machine.
    * **Subnet-ID**: als u wilt de virtuele machine implementeren in een bestaand VNet waarin u een subnet dat is gedefinieerd hebben de virtuele machine moet worden toegewezen aan de ID van dat specifieke subnet een naam. De ID meestal, lijkt **/subscriptions/\<abonnements-ID > /resourceGroups/\<groepsnaam voor accountresources > /providers/Microsoft.Network/virtualNetworks/\<virtuele-netwerknaam > /subnets/ \<subnetnaam >**. Leeg laat, als u wilt een nieuw virtueel netwerk maken

### <a name="manual-deployment"></a>Handmatige implementatie

1. Maak een resourcegroep.
1. Maak een virtueel netwerk.
1. Maak een beschikbaarheidsset.  
   Stel het maximale updatedomein.
1. Maak een load balancer (intern).
   * Selecteer het virtuele netwerk in stap 2 hebt gemaakt.
1. Virtuele machine 1 maken.  
   Gebruik ten minste Red Hat Enterprise Linux 7.4 voor SAP HANA. In dit voorbeeld wordt de Red Hat Enterprise Linux 7.4 voor SAP HANA-installatiekopie <https://ms.portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74forSAPHANA-ARM> selecteert u de beschikbaarheidsset worden gemaakt in stap 3.
1. Maak de virtuele machine 2.  
   Gebruik ten minste Red Hat Enterprise Linux 7.4 voor SAP HANA. In dit voorbeeld wordt de Red Hat Enterprise Linux 7.4 voor SAP HANA-installatiekopie <https://ms.portal.azure.com/#create/RedHat.RedHatEnterpriseLinux74forSAPHANA-ARM> selecteert u de beschikbaarheidsset worden gemaakt in stap 3.
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

## <a name="install-sap-hana"></a>SAP HANA installeren

De stappen in deze sectie gebruikt u de volgende voorvoegsels:

* **[A]** : De stap is van toepassing op alle knooppunten.
* **[1]** : De stap is van toepassing op knooppunt 1 alleen.
* **[2]** : De stap is van toepassing op knooppunt 2 van het cluster Pacemaker alleen.

1. **[A]**  Instellen van de schijfindeling: **logische Volume Manager (LVM)**.

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

1. **[A]**  RHEL voor HANA-configuratie

   RHEL configureren zoals beschreven in de SAP-notitie [2292690] en [2455582] en <https://access.redhat.com/solutions/2447641>.

1. **[A]**  De SAP HANA installeren

   Volg voor het installeren van SAP HANA-Systeemreplicatie <https://access.redhat.com/articles/3004101>.

   * Voer de **hdblcm** programma vanaf de DVD met HANA. Voer de volgende waarden bij de opdrachtprompt:
   * -Installatie te kiezen: Voer **1**.
   * Selecteer extra onderdelen voor installatie: Voer **1**.
   * Voer Installation Path [/ hana/gedeelde]: Selecteer invoeren.
   * Voer de naam van de lokale Host [.]: Selecteer invoeren.
   * Wilt u extra hosts toevoegen aan het systeem? (j/n) [n]: Selecteer invoeren.
   * Voer SAP HANA-systeem-ID: Voer de SID van HANA, bijvoorbeeld: **HN1**.
   * Voer exemplaarnummer [00]: Voer in HANA-instantie. Voer **03** als u de Azure-sjabloon gebruikt of de handmatige implementatie-sectie van dit artikel wordt gevolgd.
   * Selecteer Database modus / Index [1] Voer: Voer selecteren.
   * Selecteert u het gebruik van / Voer Index [4]: Selecteer de waarde van de gebruik system.
   * Geef de locatie van gegevensvolumes [/ hana/data/HN1]: Selecteer invoeren.
   * Geef de locatie van de Logboekvolumes [/ hana/log/HN1]: Selecteer invoeren.
   * Beperk de maximale hoeveelheid geheugen ingesteld? [n]: Selecteer invoeren.
   * Voer de hostnaam certificaat voor Host '...' [...]: Selecteer invoeren.
   * Voer SAP Host Agent-gebruiker (sapadm) wachtwoord: Voer het wachtwoord van de host agent-gebruiker.
   * SAP-Host Agent-gebruiker (sapadm) wachtwoord bevestigen: Voer het wachtwoord van de host-agent gebruiker opnieuw te bevestigen.
   * Systeembeheerder (hdbadm) wachtwoord invoeren: Voer het wachtwoord voor de systeembeheerder.
   * Controleer of de systeembeheerder (hdbadm) wachtwoord: Voer het beheerderswachtwoord van systeem opnieuw te bevestigen.
   * De systeembeheerder ENTER Home-map [/ usr/sap/HN1/home]: Selecteer invoeren.
   * Voer System Administrator aanmeldingsshell [/ bin/sh]: Selecteer invoeren.
   * Voer de gebruikersnaam van systeembeheerder [1001]: Selecteer invoeren.
   * Voer-ID van de gebruikersgroep (sapsys) [79]: Selecteer invoeren.
   * Geef het wachtwoord van de Database-gebruiker (systeem): Voer het wachtwoord van de database-gebruiker.
   * Bevestig het wachtwoord van de Database-gebruiker (systeem): Geef het wachtwoord van de database-gebruiker opnieuw om te bevestigen.
   * Systeem opnieuw opstarten na opnieuw opstarten van machine? [n]: Selecteer invoeren.
   * Wilt u doorgaan? (j/n): de samenvatting te valideren. Voer **y** om door te gaan.

1. **[A]**  De SAP-Host-Agent bijwerken.

   Download het nieuwste SAP Host Agent-archief van de [SAP Software Center] [ sap-swcenter] en voer de volgende opdracht uit de agent bij te werken. Vervang het pad naar het archief om te verwijzen naar het bestand dat u hebt gedownload:

   <pre><code>sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive &lt;path to SAP Host Agent SAR&gt;
   </code></pre>

1. **[A]**  Firewall configureren

   De firewall-regel voor de testpoort Azure Load Balancer maken.

   <pre><code>sudo firewall-cmd --zone=public --add-port=625<b>03</b>/tcp
   sudo firewall-cmd --zone=public --add-port=625<b>03</b>/tcp --permanent
   </code></pre>

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 System Replication configureren

De stappen in deze sectie gebruikt u de volgende voorvoegsels:

* **[A]** : De stap is van toepassing op alle knooppunten.
* **[1]** : De stap is van toepassing op knooppunt 1 alleen.
* **[2]** : De stap is van toepassing op knooppunt 2 van het cluster Pacemaker alleen.

1. **[A]**  Firewall configureren

   Firewallregels voor HANA-Systeemreplicatie en client-verkeer maken. De vereiste poorten worden vermeld op [TCP/IP-poorten van alle SAP-producten](https://help.sap.com/viewer/ports). De volgende opdrachten zijn slechts een voorbeeld waarmee verkeer voor database SYSTEMDB, HN1 en NW1 HANA-Systeemreplicatie 2.0 en de client.

   <pre><code>sudo firewall-cmd --zone=public --add-port=40302/tcp --permanent
sudo firewall-cmd --zone=public --add-port=40302/tcp
sudo firewall-cmd --zone=public --add-port=40301/tcp --permanent
sudo firewall-cmd --zone=public --add-port=40301/tcp
sudo firewall-cmd --zone=public --add-port=40307/tcp --permanent
sudo firewall-cmd --zone=public --add-port=40307/tcp
sudo firewall-cmd --zone=public --add-port=40303/tcp --permanent
sudo firewall-cmd --zone=public --add-port=40303/tcp
sudo firewall-cmd --zone=public --add-port=40340/tcp --permanent
sudo firewall-cmd --zone=public --add-port=40340/tcp
sudo firewall-cmd --zone=public --add-port=30340/tcp --permanent
sudo firewall-cmd --zone=public --add-port=30340/tcp
sudo firewall-cmd --zone=public --add-port=30341/tcp --permanent
sudo firewall-cmd --zone=public --add-port=30341/tcp
sudo firewall-cmd --zone=public --add-port=30342/tcp --permanent
sudo firewall-cmd --zone=public --add-port=30342/tcp
   </code></pre>

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

1. **[1]**  Replicatiestatus controleren

   De replicatiestatus controleren en wacht tot alle databases gesynchroniseerd zijn. Als de status onbekend is, controleert u de firewall-instellingen.

   <pre><code>sudo su - <b>hn1</b>adm -c "python /usr/sap/<b>HN1</b>/HDB<b>03</b>/exe/python_support/systemReplicationStatus.py"
   # | Database | Host     | Port  | Service Name | Volume ID | Site ID | Site Name | Secondary | Secondary | Secondary | Secondary | Secondary     | Replication | Replication | Replication    |
   # |          |          |       |              |           |         |           | Host      | Port      | Site ID   | Site Name | Active Status | Mode        | Status      | Status Details |
   # | -------- | -------- | ----- | ------------ | --------- | ------- | --------- | --------- | --------- | --------- | --------- | ------------- | ----------- | ----------- | -------------- |
   # | SYSTEMDB | <b>hn1-db-0</b> | 30301 | nameserver   |         1 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30301 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>HN1</b>      | <b>hn1-db-0</b> | 30307 | xsengine     |         2 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30307 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>NW1</b>      | <b>hn1-db-0</b> | 30340 | indexserver  |         2 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30340 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   # | <b>HN1</b>      | <b>hn1-db-0</b> | 30303 | indexserver  |         3 |       1 | <b>SITE1</b>     | <b>hn1-db-1</b>  |     30303 |         2 | <b>SITE2</b>     | YES           | SYNC        | ACTIVE      |                |
   #
   # status system replication site "2": ACTIVE
   # overall system replication status: ACTIVE
   #
   # Local System Replication State
   # ~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
   #
   # mode: PRIMARY
   # site id: 1
   # site name: <b>SITE1</b>
   </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 System Replication configureren

De stappen in deze sectie gebruikt u de volgende voorvoegsels:

* **[A]** : De stap is van toepassing op alle knooppunten.
* **[1]** : De stap is van toepassing op knooppunt 1 alleen.
* **[2]** : De stap is van toepassing op knooppunt 2 van het cluster Pacemaker alleen.

1. **[A]**  Firewall configureren

   Firewallregels voor HANA-Systeemreplicatie en client-verkeer maken. De vereiste poorten worden vermeld op [TCP/IP-poorten van alle SAP-producten](https://help.sap.com/viewer/ports). De volgende opdrachten zijn slechts een voorbeeld om toe te staan van HANA-Systeemreplicatie 2.0. Deze worden aangepast aan de installatie van de SAP HANA 1.0.

   <pre><code>sudo firewall-cmd --zone=public --add-port=40302/tcp --permanent
   sudo firewall-cmd --zone=public --add-port=40302/tcp
   </code></pre>

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

   <pre><code>HDB stop
   hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>
   HDB start
   </code></pre>

## <a name="create-a-pacemaker-cluster"></a>Een cluster Pacemaker maken

Volg de stappen in [Pacemaker op Red Hat Enterprise Linux in Azure instellen](high-availability-guide-rhel-pacemaker.md) om een eenvoudige Pacemaker-cluster voor deze HANA-server te maken.

## <a name="create-sap-hana-cluster-resources"></a>Resources voor SAP HANA-cluster maken

Installeer de SAP HANA-resource-agents op **alle knooppunten**. Zorg ervoor dat u het inschakelen van een opslagplaats waarvan het pakket bevat.

<pre><code># Enable repository that contains SAP HANA resource agents
sudo subscription-manager repos --enable="rhel-sap-hana-for-rhel-7-server-rpms"
   
sudo yum install -y resource-agents-sap-hana
</code></pre>

Maak vervolgens de HANA-topologie. Voer de volgende opdrachten op een van de clusterknooppunten Pacemaker:

<pre><code>sudo pcs property set maintenance-mode=true

# Replace the bold string with your instance number and HANA system ID
sudo pcs resource create SAPHanaTopology_<b>HN1</b>_<b>03</b> SAPHanaTopology SID=<b>HN1</b> InstanceNumber=<b>03</b> --clone clone-max=2 clone-node-max=1 interleave=true
</code></pre>

Maak vervolgens de HANA-resources:

<pre><code># Replace the bold string with your instance number, HANA system ID, and the front-end IP address of the Azure load balancer.

sudo pcs resource create SAPHana_<b>HN1</b>_<b>03</b> SAPHana SID=<b>HN1</b> InstanceNumber=<b>03</b> PREFER_SITE_TAKEOVER=true DUPLICATE_PRIMARY_TIMEOUT=7200 AUTOMATED_REGISTER=false master notify=true clone-max=2 clone-node-max=1 interleave=true

sudo pcs resource create vip_<b>HN1</b>_<b>03</b> IPaddr2 ip="<b>10.0.0.13</b>"

sudo pcs resource create nc_<b>HN1</b>_<b>03</b> azure-lb port=625<b>03</b>

sudo pcs resource group add g_ip_<b>HN1</b>_<b>03</b> nc_<b>HN1</b>_<b>03</b> vip_<b>HN1</b>_<b>03</b>

sudo pcs constraint order SAPHanaTopology_<b>HN1</b>_<b>03</b>-clone then SAPHana_<b>HN1</b>_<b>03</b>-master symmetrical=false

sudo pcs constraint colocation add g_ip_<b>HN1</b>_<b>03</b> with master SAPHana_<b>HN1</b>_<b>03</b>-master 4000

sudo pcs property set maintenance-mode=false
</code></pre>

Zorg ervoor dat de clusterstatus ok is en dat alle resources die worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.

<pre><code>sudo pcs status

# Online: [ hn1-db-0 hn1-db-1 ]
#
# Full list of resources:
#
# azure_fence     (stonith:fence_azure_arm):      Started hn1-db-0
#  Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
#      Started: [ hn1-db-0 hn1-db-1 ]
#  Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
#      Masters: [ hn1-db-0 ]
#      Slaves: [ hn1-db-1 ]
#  Resource Group: g_ip_HN1_03
#      nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
#      vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

Deze sectie wordt beschreven hoe u uw instellingen kunt testen. Voordat u een test, zorg ervoor dat Pacemaker heeft geen een mislukte actie (via de status van de pc's), zijn er geen Locatiebeperkingen voor onverwachte (bijvoorbeeld restanten van een migratie-test) en dat HANA synchronisatiestatus, bijvoorbeeld met systemReplicationStatus is:

<pre><code>[root@hn1-db-0 ~]# sudo su - hn1adm -c "python /usr/sap/HN1/HDB03/exe/python_support/systemReplicationStatus.py"
</code></pre>

### <a name="test-the-migration"></a>Testen van de migratie

De resourcestatus van de voordat u begint met de test:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

U kunt het SAP HANA-hoofdknooppunt migreren door het uitvoeren van de volgende opdracht:

<pre><code>[root@hn1-db-0 ~]# pcs resource move SAPHana_HN1_03-master
</code></pre>

Als u `AUTOMATED_REGISTER="false"`, met deze opdracht moet de SAP HANA-hoofdknooppunt en de groep waarin het virtuele IP-adres 1-db-hn1 migreren.

Zodra de migratie is voltooid, de 'sudo-pc's status'-uitvoer er als volgt uitzien

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Stopped: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

De SAP HANA-resource op hn1-db-0 is gestopt. In dit geval de HANA-exemplaar als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>[root@hn1-db-0 ~]# su - hn1adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr 03 -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=hn1-db-1 --remoteInstance=03 --replicationMod
e=sync --name=SITE1
</code></pre>

De migratie maakt Locatiebeperkingen moeten opnieuw worden verwijderd:

<pre><code># Switch back to root
exit
[root@hn1-db-0 ~]# pcs resource clear SAPHana_HN1_03-master
</code></pre>

Controleer de status van de HANA-resource met behulp van 'status pc's '. Wanneer HANA is gestart op hn1-db-0, ziet de uitvoer er als volgt

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

### <a name="test-the-azure-fencing-agent"></a>De agent voor de eerste optie Azure testen

De resourcestatus van de voordat u begint met de test:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
    Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

U kunt de installatie van de agent voor de eerste optie Azure testen door het uitschakelen van de netwerkinterface op het knooppunt waarop de SAP HANA-master wordt uitgevoerd.
Zie [Red Hat Knowledgebase-artikel 79523](https://access.redhat.com/solutions/79523) voor een descricption over het simuleren van een netwerkfout. In dit voorbeeld gebruiken we het script net_breaker alle toegang tot het netwerk blokkeren.

<pre><code>[root@hn1-db-1 ~]# sh ./net_breaker.sh BreakCommCmd 10.0.0.6
</code></pre>

De virtuele machine moet nu opnieuw starten of stoppen, afhankelijk van uw clusterconfiguratie.
Als u de `stonith-action` is ingesteld op uitgeschakeld, de virtuele machine wordt gestopt en de bronnen worden gemigreerd naar de actieve virtuele machine.

> [!NOTE]
> Duurt tot vijftien minuten totdat de virtuele machines weer online is.

Nadat u de virtuele machine opnieuw starten, de SAP HANA-resource niet kan worden gestart als secundaire cache als u `AUTOMATED_REGISTER="false"`. In dit geval de HANA-exemplaar als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> sapcontrol -nr <b>03</b> -function StopWait 600 10
hn1adm@hn1-db-1:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b>

# Switch back to root and clean up the failed state
exit
[root@hn1-db-1 ~]# pcs resource cleanup SAPHana_HN1_03-master
</code></pre>

De resourcestatus van de nadat de test:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

### <a name="test-a-manual-failover"></a>Een handmatige failover testen

De resourcestatus van de voordat u begint met de test:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-0 ]
    Slaves: [ hn1-db-1 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-0
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-0
</code></pre>

U kunt een handmatige failover testen door te stoppen van het cluster op het knooppunt hn1-db-0:

<pre><code>[root@hn1-db-0 ~]# pcs cluster stop
</code></pre>

U kunt het cluster opnieuw starten na de failover. Als u `AUTOMATED_REGISTER="false"`, de SAP HANA-resource op het knooppunt hn1-db-0 niet kan worden gestart als secundaire cache. In dit geval de HANA-exemplaar als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>[root@hn1-db-0 ~]# pcs cluster start
[root@hn1-db-0 ~]# su - hn1adm

# Stop the HANA instance just in case it is running
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> sapcontrol -nr 03 -function StopWait 600 10
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# Switch back to root and clean up the failed state
hn1adm@hn1-db-0:/usr/sap/HN1/HDB03> exit
[root@hn1-db-1 ~]# pcs resource cleanup SAPHana_HN1_03-master
</code></pre>

De resourcestatus van de nadat de test:

<pre><code>Clone Set: SAPHanaTopology_HN1_03-clone [SAPHanaTopology_HN1_03]
    Started: [ hn1-db-0 hn1-db-1 ]
Master/Slave Set: SAPHana_HN1_03-master [SAPHana_HN1_03]
    Masters: [ hn1-db-1 ]
     Slaves: [ hn1-db-0 ]
Resource Group: g_ip_HN1_03
    nc_HN1_03  (ocf::heartbeat:azure-lb):      Started hn1-db-1
    vip_HN1_03 (ocf::heartbeat:IPaddr2):       Started hn1-db-1
</code></pre>

## <a name="next-steps"></a>Volgende stappen

* [Azure virtuele Machines, planning en implementatie van SAP][planning-guide]
* [Azure Virtual Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over het opzetten van hoge beschikbaarheid en plan voor herstel na noodgevallen van SAP HANA op Azure (grote instanties), [SAP HANA (grote instanties) hoge beschikbaarheid en herstel na noodgeval op Azure](hana-overview-high-availability-disaster-recovery.md)
