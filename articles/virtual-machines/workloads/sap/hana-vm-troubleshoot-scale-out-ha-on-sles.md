---
title: Probleemoplossing voor SAP HANA 2.0 scale-out HSR Pacemaker setup met SLES 12 SP3 op Azure virtual machines | Microsoft Docs
description: Handleiding voor het controleren en oplossen van een complexe hoge beschikbaarheid configureren voor SAP HANA scale-out op basis van SAP HANA System Replication (HSR) en Pacemaker op SLES 12 SP3 die worden uitgevoerd op virtuele machines van Azure
services: virtual-machines-linux
documentationcenter: ''
author: hermannd
manager: jeconnoc
editor: ''
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 09/24/2018
ms.author: hermannd
ms.openlocfilehash: f86107c5fcd4c0175d59689718dca15736aa3b17
ms.sourcegitcommit: 345b96d564256bcd3115910e93220c4e4cf827b3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/28/2018
ms.locfileid: "52497362"
---
# <a name="verify-and-troubleshoot-sap-hana-scale-out-high-availability-setup-on-sles-12-sp3"></a>Controleren en problemen oplossen van SAP HANA scale-out hoge beschikbaarheid instellen op SLES 12 SP3 

[sles-pacemaker-ha-guide]:high-availability-guide-suse-pacemaker.md
[sles-hana-scale-out-ha-paper]:https://www.suse.com/documentation/suse-best-practices/singlehtml/SLES4SAP-hana-scaleOut-PerfOpt-12/SLES4SAP-hana-scaleOut-PerfOpt-12.html
[sap-hana-iaas-list]:https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html
[suse-pacemaker-support-log-files]:https://www.suse.com/support/kb/doc/?id=7022702
[azure-linux-multiple-nics]:https://docs.microsoft.com/azure/virtual-machines/linux/multiple-nics
[suse-cloud-netconfig]:https://www.suse.com/c/multi-nic-cloud-netconfig-ec2-azure/
[sap-list-port-numbers]:https://help.sap.com/viewer/ports
[sles-12-ha-paper]:https://www.suse.com/documentation/sle-ha-12/pdfdoc/book_sleha/book_sleha.pdf
[sles-zero-downtime-paper]:https://www.suse.com/media/presentation/TUT90846_towards_zero_downtime%20_how_to_maintain_sap_hana_system_replication_clusters.pdf
[sap-nw-ha-guide-sles]:high-availability-guide-suse.md
[sles-12-for-sap]:https://www.suse.com/media/white-paper/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf


Dit artikel helpt u om te controleren van de clusterconfiguratie Pacemaker voor SAP HANA scale-out die wordt uitgevoerd op Azure virtual machines (VM's). De installatie is uitgevoerd in combinatie met SAP HANA System Replication (HSR) en de SUSE-RPM-pakket SAPHanaSR ScaleOut. Alle tests zijn uitgevoerd op SUSE SLES 12 SP3 alleen. De secties van het artikel betrekking hebben op verschillende gebieden en omvatten Voorbeeldopdrachten en fragmenten van configuratiebestanden. We raden deze voorbeelden aan als een methode om te controleren en controleer de instellingen voor het hele cluster.



## <a name="important-notes"></a>Belangrijke opmerkingen

Alle tests voor SAP HANA scale-out in combinatie met SAP HANA-Systeemreplicatie en Pacemaker is uitgevoerd met SAP HANA 2.0 alleen. De besturingssysteemversie is SUSE Linux Enterprise Server 12 SP3 voor SAP-toepassingen. De meest recente RPM-pakket, SAPHanaSR-ScaleOut van SUSE, is gebruikt voor het instellen van het cluster Pacemaker.
SUSE gepubliceerd een [gedetailleerde beschrijving van deze instellingen voor optimale prestaties][sles-hana-scale-out-ha-paper].

Voor de typen virtuele machines die worden ondersteund voor SAP HANA scale-out, Controleer de [SAP HANA-gecertificeerde IaaS directory][sap-hana-iaas-list].

Er is een technisch probleem met SAP HANA scale-out in combinatie met meerdere subnetten en vnic's en het instellen van HSR. Het is verplicht gebruik van de meest recente patches voor SAP HANA 2.0 waarbij dit probleem is opgelost. De volgende SAP HANA-versies worden ondersteund: 

* rev2.00.024.04 of hoger 
* rev2.00.032 of hoger

Als u ondersteuning van SUSE nodig hebt, volgt u deze [handleiding][suse-pacemaker-support-log-files]. Verzamel alle informatie over de SAP HANA-cluster voor hoge beschikbaarheid (HA) zoals beschreven in het artikel. Deze informatie moet het SUSE-ondersteuningsteam voor verdere analyse.

De clusterinstallatie raakten tijdens interne tests door een normale correct VM afsluiten via Azure portal. Daarom is het raadzaam dat u een clusterfailover via andere methoden testen. Methoden, zoals het forceren van een kernelprobleem gebruiken of de netwerken afgesloten of migreren de **msl** resource. Zie voor meer informatie de volgende secties. De veronderstelling is dat een standard afsluiten gebeurt er met de bedoeling. Het beste voorbeeld van een opzettelijke afsluiten is voor onderhoud. Zie voor meer informatie [gepland onderhoud](#planned-maintenance).

Ook tijdens interne tests, de clusterinstallatie raakten na een handmatige SAP HANA-overname bij het cluster in de onderhoudsmodus bevindt is. Het is raadzaam dat u overschakelt naar deze weer opnieuw handmatig voordat u de onderhoudsmodus van het cluster beëindigen. Er is een andere optie voor het activeren van een failover voordat u het cluster in de onderhoudsmodus plaatst. Zie voor meer informatie, [gepland onderhoud](#planned-maintenance). De documentatie van SUSE wordt beschreven hoe u het cluster op deze manier kunt herstellen met behulp van de **crm** opdracht. Maar de benadering die worden vermeld was eerder robuuste tijdens interne tests en nooit gebleken dat met een onverwachte neveneffecten.

Wanneer u gebruikt de **crm migreren** opdracht, zorg ervoor dat voor het opschonen van de configuratie van het cluster. Locatiebeperkingen die u mogelijk niet op de hoogte van wordt toegevoegd. Deze beperkingen van invloed zijn op het gedrag van het cluster. Zie voor meer informatie [gepland onderhoud](#planned-maintenance).



## <a name="test-system-description"></a>Beschrijving van het systeem testen

 Voor SAP HANA scale-out HA-verificatie en -certificering, is een installatie gebruikt. Deze bestond uit twee systemen met drie SAP HANA knooppunten elke: één master en twee werkrollen. De volgende tabel een lijst met VM-namen en het interne IP-adressen. Alle controle-voorbeelden die zijn uitgevoerd op deze virtuele machines. Met behulp van deze VM-namen en IP-adressen in de voorbeelden van de opdracht, kunt u beter begrijpen met de opdrachten en de uitvoer:


| Soort knooppunt | VM-naam | IP-adres |
| --- | --- | --- |
| Hoofdknooppunt op site 1 | hso-hana-vm-s1-0 | 10.0.0.30 |
| Worker-knooppunt 1 op site 1 | hso-hana-vm-s1-1 | 10.0.0.31 |
| Worker-knooppunt 2 op site 1 | hso-hana-vm-s1-2 | 10.0.0.32 |
| | | |
| Hoofdknooppunt op locatie 2 | hso-hana-vm-s2-0 | 10.0.0.40 |
| Worker-knooppunt 1 op locatie 2 | hso-hana-vm-s2-1 | 10.0.0.41 |
| Worker-knooppunt 2 op locatie 2 | hso-hana-vm-s2-2  | 10.0.0.42 |
| | | |
| Hoofdknooppuntverzamelingscluster maker | hso-hana-dm | 10.0.0.13 |
| SBD apparaat server | hso-hana-sbd | 10.0.0.19 |
| | | |
| NFS-server 1 | hso-nfs-vm-0 | 10.0.0.15 |
| NFS-server 2 | hso-nfs-vm-1 | 10.0.0.14 |



## <a name="multiple-subnets-and-vnics"></a>Meerdere subnetten en vnic 's

De volgende aanbevelingen voor SAP HANA-netwerken zijn drie subnetten gemaakt binnen een virtueel Azure-netwerk. SAP HANA scale-out op Azure moet worden geïnstalleerd in de niet-gedeelde modus. Dit betekent dat elk knooppunt maakt gebruik van lokale schijfvolumes voor **/hana/gegevens** en **/hana/log**. Omdat de knooppunten alleen lokale schijfvolumes gebruiken, is het niet nodig voor het definiëren van een apart subnet voor de opslag:

- 10.0.2.0/24 voor SAP HANA-communicatie
- 10.0.1.0/24 voor SAP HANA System Replication (HSR)
- 10.0.0.0/24 voor alle andere

Zie voor meer informatie over de configuratie van SAP HANA met betrekking tot het gebruik van meerdere netwerken [SAP HANA global.ini](#sap-hana-globalini).

Elke virtuele machine in het cluster heeft drie vnic's die met het aantal subnetten overeenkomen. [Over het maken van een virtuele Linux-machine in Azure met meerdere netwerkinterfacekaarten] [ azure-linux-multiple-nics] hierin een mogelijk probleem met routing worden in Azure bij het implementeren van een Linux-VM. Deze specifieke routering artikel geldt alleen voor het gebruik van meerdere vnic's. Het probleem is opgelost door SUSE per standaard in SLES 12 SP3. Zie voor meer informatie, [Multi-NIC met cloud-netconfig in EC2 en Azure][suse-cloud-netconfig].


Voer de volgende opdrachten uit om te controleren of SAP HANA correct is geconfigureerd voor het gebruik van meerdere netwerken. Controleer eerst op het niveau van het besturingssysteem dat alle drie interne IP-adressen voor alle drie subnetten actief zijn. Als u de subnetten met verschillende IP-adresbereiken gedefinieerd, hebt u aan te passen van de opdrachten:

<pre><code>
ifconfig | grep "inet addr:10\."
</code></pre>

De volgende voorbeelduitvoer is van de tweede worker-knooppunt op locatie 2. Hier ziet u drie verschillende interne IP-adressen van eth0, eth1 en eth2:

<pre><code>
inet addr:10.0.0.42  Bcast:10.0.0.255  Mask:255.255.255.0
inet addr:10.0.1.42  Bcast:10.0.1.255  Mask:255.255.255.0
inet addr:10.0.2.42  Bcast:10.0.2.255  Mask:255.255.255.0
</code></pre>


Controleer vervolgens of de SAP HANA-poorten voor de naamserver en HSR. SAP HANA moet luisteren op de overeenkomstige subnetten. U hebt om aan te passen van de opdrachten, afhankelijk van het aantal SAP HANA-exemplaar. Voor het testsysteem, was het instantienummer van de **00**. Er zijn verschillende manieren om erachter te komen welke poorten worden gebruikt. 

De volgende SQL-instructie retourneert de exemplaar-ID, exemplaarnummer en andere informatie:

<pre><code>
select * from "SYS"."M_SYSTEM_OVERVIEW"
</code></pre>

Als u de juiste poortnummers zoekt, kunt u bekijken, bijvoorbeeld in HANA Studio onder **configuratie** of via een SQL-instructie:

<pre><code>
select * from M_INIFILE_CONTENTS WHERE KEY LIKE 'listen%'
</code></pre>

Als u zoekt elke poort die wordt gebruikt in de SAP-softwarestack met inbegrip van SAP HANA, zoeken [TCP/IP-poorten van alle SAP-producten][sap-list-port-numbers].

Uitgaand van het getal exemplaar **00** in het testsysteem SAP HANA 2.0, is het het poortnummer voor de naamserver **30001**. Het poortnummer voor communicatie van HSR metagegevens **40002**. Een optie is om te melden met een werkrolknooppunt en controleert u de services van het hoofdknooppunt. In dit artikel gecontroleerd we worker-knooppunt 2 op locatie 2 verbinding maken met het hoofdknooppunt op locatie 2.

Controleer de naam van server-poort:

<pre><code>
nc -vz 10.0.0.40 30001
nc -vz 10.0.1.40 30001
nc -vz 10.0.2.40 30001
</code></pre>

Om te bewijzen dat de communicatie tussen knooppunten maakt gebruik van subnet **10.0.2.0/24**, het resultaat ziet er als de volgende voorbeelduitvoer.
Alleen de verbinding via subnet **10.0.2.0/24** moet slagen:

<pre><code>
nc: connect to 10.0.0.40 port 30001 (tcp) failed: Connection refused
nc: connect to 10.0.1.40 port 30001 (tcp) failed: Connection refused
Connection to 10.0.2.40 30001 port [tcp/pago-services1] succeeded!
</code></pre>

Nu controleren voor HSR poort **40002**:

<pre><code>
nc -vz 10.0.0.40 40002
nc -vz 10.0.1.40 40002
nc -vz 10.0.2.40 40002
</code></pre>

Om te bewijzen dat de communicatie HSR maakt gebruik van subnet **10.0.1.0/24**, het resultaat ziet er als de volgende voorbeelduitvoer.
Alleen de verbinding via subnet **10.0.1.0/24** moet slagen:

<pre><code>
nc: connect to 10.0.0.40 port 40002 (tcp) failed: Connection refused
Connection to 10.0.1.40 40002 port [tcp/*] succeeded!
nc: connect to 10.0.2.40 port 40002 (tcp) failed: Connection refused
</code></pre>



## <a name="corosync"></a>Corosync


De **corosync** config-bestand is alleen correct als op elk knooppunt in het cluster met inbegrip van de maker voor hoofdknooppunten. Als het cluster lid worden van een knooppunt niet werkt zoals verwacht, maken of kopiëren **/etc/corosync/corosync.conf** handmatig op alle knooppunten en de service opnieuw starten. 

De inhoud van **corosync.conf** uit het systeem is een voorbeeld.

De eerste sectie is **totem**, zoals beschreven in [clusterinstallatie](https://review.docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#cluster-installation), stap 11. U kunt de waarde voor negeren **mcastaddr**. Houd het bestaande item. De vermeldingen voor **token** en **consensus** moet worden ingesteld volgens [documentatie voor Microsoft Azure-SAP HANA][sles-pacemaker-ha-guide].

<pre><code>
totem {
    version:    2
    secauth:    on
    crypto_hash:    sha1
    crypto_cipher:  aes256
    cluster_name:   hacluster
    clear_node_high_bit: yes

    token:      30000
    token_retransmits_before_loss_const: 10
    join:       60
    consensus:  36000
    max_messages:   20

    interface {
        ringnumber:     0
        bindnetaddr: 10.0.0.0
        mcastaddr:  239.170.19.232
        mcastport:  5405

        ttl:        1
    }
    transport:      udpu

}
</code></pre>

De tweede sectie **logboekregistratie**, niet is gewijzigd van de opgegeven standaardwaarden:

<pre><code>
logging {
    fileline:   off
    to_stderr:  no
    to_logfile:     no
    logfile:    /var/log/cluster/corosync.log
    to_syslog:  yes
    debug:      off
    timestamp:  on
    logger_subsys {
        subsys:     QUORUM
        debug:  off
    }
}
</code></pre>

De derde sectie ziet u de **nodelist**. Alle knooppunten van het cluster hebt weergegeven met hun **nodeid**:

<pre><code>
nodelist {
  node {
   ring0_addr:hso-hana-vm-s1-0
   nodeid: 1
   }
  node {
   ring0_addr:hso-hana-vm-s1-1
   nodeid: 2
   }
  node {
   ring0_addr:hso-hana-vm-s1-2
   nodeid: 3
   }
  node {
   ring0_addr:hso-hana-vm-s2-0
   nodeid: 4
   }
  node {
   ring0_addr:hso-hana-vm-s2-1
   nodeid: 5
   }
  node {
   ring0_addr:hso-hana-vm-s2-2
   nodeid: 6
   }
  node {
   ring0_addr:hso-hana-dm
   nodeid: 7
   }
}
</code></pre>

In de laatste sectie **quorum**, is het belangrijk om in te stellen de waarde voor **expected_votes** correct. Het moet het aantal knooppunten, met inbegrip van de maker voor hoofdknooppunten. En de waarde voor **two_node** moet **0**. Verwijder de vermelding niet volledig. Stelt u de waarde in op **0**.

<pre><code>
quorum {
    # Enable and configure quorum subsystem (default: off)
    # see also corosync.conf.5 and votequorum.5
    provider: corosync_votequorum
    expected_votes: 7
    two_node: 0
}
</code></pre>


Start de service via **systemctl**:

<pre><code>
systemctl restart corosync
</code></pre>




## <a name="sbd-device"></a>SBD apparaat

Over het instellen van een apparaat SBD op een Azure-VM wordt beschreven in [SBD eerste optie](https://review.docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#sbd-fencing).

Controleer eerst op de server SBD VM als er ACL-vermeldingen voor elk knooppunt in het cluster. Voer de volgende opdracht op de server SBD VM:


<pre><code>
targetcli ls
</code></pre>


Op het testsysteem, is de uitvoer van de opdracht ziet eruit als in het volgende voorbeeld. ACL-namen, zoals **iqn.2006-04.hso-db-0.local:hso-db-0** moet worden ingevoerd als de namen van de bijbehorende initiator op de virtuele machines. Elke virtuele machine moet een ander account.

<pre><code>
 | | o- sbddbhso ................................................................... [/sbd/sbddbhso (50.0MiB) write-thru activated]
  | |   o- alua ................................................................................................... [ALUA Groups: 1]
  | |     o- default_tg_pt_gp ....................................................................... [ALUA state: Active/optimized]
  | o- pscsi .................................................................................................. [Storage Objects: 0]
  | o- ramdisk ................................................................................................ [Storage Objects: 0]
  o- iscsi ............................................................................................................ [Targets: 1]
  | o- iqn.2006-04.dbhso.local:dbhso ..................................................................................... [TPGs: 1]
  |   o- tpg1 ............................................................................................... [no-gen-acls, no-auth]
  |     o- acls .......................................................................................................... [ACLs: 7]
  |     | o- iqn.2006-04.hso-db-0.local:hso-db-0 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-1.local:hso-db-1 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-2.local:hso-db-2 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-3.local:hso-db-3 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-4.local:hso-db-4 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-5.local:hso-db-5 .................................................................. [Mapped LUNs: 1]
  |     | | o- mapped_lun0 ............................................................................. [lun0 fileio/sbddbhso (rw)]
  |     | o- iqn.2006-04.hso-db-6.local:hso-db-6 .................................................................. [Mapped LUNs: 1]
</code></pre>

Controleer vervolgens of de namen van de initiator op alle virtuele machines verschillend zijn en komen met de eerder weergegeven vermeldingen overeen. In dit voorbeeld komt uit de worker-knooppunt 1 op site 1:

<pre><code>
cat /etc/iscsi/initiatorname.iscsi
</code></pre>

De uitvoer lijkt op het volgende voorbeeld:

<pre><code>
##
## /etc/iscsi/iscsi.initiatorname
##
## Default iSCSI Initiatorname.
##
## DO NOT EDIT OR REMOVE THIS FILE!
## If you remove this file, the iSCSI daemon will not start.
## If you change the InitiatorName, existing access control lists
## may reject this initiator.  The InitiatorName must be unique
## for each iSCSI initiator.  Do NOT duplicate iSCSI InitiatorNames.
InitiatorName=iqn.2006-04.hso-db-1.local:hso-db-1
</code></pre>

Vervolgens controleren of de **detectie** correct werkt. Voer de volgende opdracht uit op elk clusterknooppunt met behulp van het IP-adres van de server SBD VM:

<pre><code>
iscsiadm -m discovery --type=st --portal=10.0.0.19:3260
</code></pre>

De uitvoer moet eruitzien als in het volgende voorbeeld:

<pre><code>
10.0.0.19:3260,1 iqn.2006-04.dbhso.local:dbhso
</code></pre>

Het volgende bewijs punt is om te verifiëren dat het knooppunt het SDB-apparaat ziet. Bekijk het op elk knooppunt met inbegrip van het hoofdknooppunt maker:

<pre><code>
lsscsi | grep dbhso
</code></pre>

De uitvoer moet eruitzien als in het volgende voorbeeld. Echter, de namen verschillen. Naam van het apparaat kan ook worden gewijzigd nadat de virtuele machine opnieuw wordt opgestart:

<pre><code>
[6:0:0:0]    disk    LIO-ORG  sbddbhso         4.0   /dev/sdm
</code></pre>

Afhankelijk van de status van het systeem helpt deze soms opnieuw opstarten van de iSCSI-services om problemen te verhelpen. Voer vervolgens de volgende opdrachten uit:

<pre><code>
systemctl restart iscsi
systemctl restart iscsid
</code></pre>


Vanaf elk knooppunt, kunt u controleren of alle knooppunten zijn **wissen**. Zorg ervoor dat u met de naam van het juiste apparaat op een specifiek knooppunt:

<pre><code>
sbd -d /dev/sdm list
</code></pre>

De uitvoer moet worden weergegeven **wissen** voor elk knooppunt in het cluster:

<pre><code>
0       hso-hana-vm-s1-0        clear
1       hso-hana-vm-s2-2        clear
2       hso-hana-vm-s2-1        clear
3       hso-hana-dm     clear
4       hso-hana-vm-s1-1        clear
5       hso-hana-vm-s2-0        clear
6       hso-hana-vm-s1-2        clear
</code></pre>


Een andere SBD controleren de **dump** optie van de **sbd** opdracht. In dit voorbeeld van een opdracht en uitvoer van de maker voor hoofdknooppunten, de naam van het apparaat is **sdd**, niet **sdm**:

<pre><code>
sbd -d /dev/sdd dump
</code></pre>

De uitvoer, naast de naam van het apparaat ziet dezelfde op alle knooppunten:

<pre><code>
==Dumping header on disk /dev/sdd
Header version     : 2.1
UUID               : 9fa6cc49-c294-4f1e-9527-c973f4d5a5b0
Number of slots    : 255
Sector size        : 512
Timeout (watchdog) : 60
Timeout (allocate) : 2
Timeout (loop)     : 1
Timeout (msgwait)  : 120
==Header on disk /dev/sdd is dumped
</code></pre>

Controleren op een meer SBD is de mogelijkheid om een bericht verzenden naar een ander knooppunt. Als u wilt een bericht verzenden naar de worker-knooppunt 2 op locatie 2, voer de volgende opdracht op de worker-knooppunt 1 op locatie 2:

<pre><code>
sbd -d /dev/sdm message hso-hana-vm-s2-2 test
</code></pre>

Op de doel-kant van de virtuele machine, **hso-hana-vm-s2-2** in dit voorbeeld vindt u de volgende vermelding in **/var/log/messages**:

<pre><code>
/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68:   notice: servant: Received command test from hso-hana-vm-s2-1 on disk /dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68
</code></pre>

Controleer of de vermeldingen in **/etc/sysconfig/sbd** komen overeen met de beschrijving in [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](https://review.docs.microsoft.com/en-us/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker#sbd-fencing). Controleer de opstartinstelling worden **/etc/iscsi/iscsid.conf** is ingesteld op automatisch.

De volgende vermeldingen zijn belangrijk in **/etc/sysconfig/sbd**. Pas de **id** waarde indien nodig:

<pre><code>
SBD_DEVICE="/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68;"
SBD_PACEMAKER=yes
SBD_STARTMODE=always
SBD_WATCHDOG=yes
</code></pre>


Controleer de opstartinstelling in **/etc/iscsi/iscsid.conf**. De vereiste instelling moet zich hebben voorgedaan met de volgende **iscsiadm** opdracht, beschreven in de documentatie. Controleren en deze handmatig aan te passen **vi** als het afwijkt.

Met deze opdracht wordt de opstartinstellingen ingesteld:

<pre><code>
iscsiadm -m node --op=update --name=node.startup --value=automatic
</code></pre>

Deze vermelding in **/etc/iscsi/iscsid.conf**:

<pre><code>
node.startup = automatic
</code></pre>

Tijdens het testen en verificatie, na het opnieuw opstarten van een virtuele machine, is het apparaat SBD niet meer in sommige gevallen zichtbaar. Er is een verschil tussen de opstartinstelling en wat YaST2 laten zien. Om te controleren of de instellingen, de volgende stappen uitvoeren:

1. Start YaST2.
2. Selecteer **netwerkservices** aan de linkerkant.
3. Schuif omlaag aan de rechterkant op **iSCSI-Initiator** en selecteer deze.
4. In het volgende scherm onder het **Service** tabblad ziet u de unieke initiatornaam voor het knooppunt.
5. Boven de initiatornaam van de, zorg ervoor dat de **Service Start** waarde is ingesteld op **bij het opstarten**.
6. Als dat niet het geval is, vervolgens instellen op **bij het opstarten** in plaats van **handmatig**.
7. Vervolgens het bovenste tabblad om te schakelen **verbonden doelen**.
8. Op de **verbonden doelen** scherm ziet u een vermelding voor het apparaat SBD, zoals in dit voorbeeld: **10.0.0.19:3260 iqn.2006-04.dbhso.local:dbhso**.
9. Controleer of de **opstarten** waarde is ingesteld op **bij het opstarten van**.
10. Als dit niet het geval is, kiest u **bewerken** en deze wijzigen.
11. Sla de wijzigingen en YaST2 af te sluiten.



## <a name="pacemaker"></a>Pacemaker

Nadat u alles juist is ingesteld, kunt u de volgende opdracht uitvoeren op elk knooppunt om de status van de service Pacemaker te controleren:

<pre><code>
systemctl status pacemaker
</code></pre>

De bovenkant van de uitvoer moet eruitzien als in het volgende voorbeeld. Het is belangrijk dat de status na **Active** wordt weergegeven als **geladen** en **actief (actief)**. De status na **geladen** moet worden weergegeven als **ingeschakeld**.

<pre><code>
  pacemaker.service - Pacemaker High Availability Cluster Manager
   Loaded: loaded (/usr/lib/systemd/system/pacemaker.service; enabled; vendor preset: disabled)
   Active: active (running) since Fri 2018-09-07 05:56:27 UTC; 4 days ago
     Docs: man:pacemakerd
           http://clusterlabs.org/doc/en-US/Pacemaker/1.1-pcs/html/Pacemaker_Explained/index.html
 Main PID: 4496 (pacemakerd)
    Tasks: 7 (limit: 4915)
   CGroup: /system.slice/pacemaker.service
           ├─4496 /usr/sbin/pacemakerd -f
           ├─4499 /usr/lib/pacemaker/cib
           ├─4500 /usr/lib/pacemaker/stonithd
           ├─4501 /usr/lib/pacemaker/lrmd
           ├─4502 /usr/lib/pacemaker/attrd
           ├─4503 /usr/lib/pacemaker/pengine
           └─4504 /usr/lib/pacemaker/crmd
</code></pre>

Als de instelling is nog steeds op **uitgeschakeld**, voer de volgende opdracht uit:

<pre><code>
systemctl enable pacemaker
</code></pre>

Als u wilt zien van alle geconfigureerde bronnen in Pacemaker, voer de volgende opdracht:

<pre><code>
crm status
</code></pre>

De uitvoer moet eruitzien als in het volgende voorbeeld. Het is prima die de **cln** en **msl** resources worden weergegeven als op de maker van de meeste VM, gestopt **hso-hana-dm**. Er is geen SAP HANA-installatie op de meeste maker-knooppunt. De **cln** en **msl** resources worden weergegeven als gestopt. Het is belangrijk dat deze ziet u de juiste totale aantal virtuele machines, **7**. Alle virtuele machines die deel van het cluster uitmaken moeten worden weergegeven met de status **Online**. De huidige primaire hoofdknooppunt moet juist worden herkend. In dit voorbeeld heeft **hso-hana-vm-s1-0**:

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Tue Sep 11 15:56:40 2018
Last change: Tue Sep 11 15:56:23 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0
</code></pre>

Een belangrijk onderdeel van Pacemaker is in de onderhoudsmodus. In deze modus kunt u wijzigingen aanbrengen zonder waar een actie direct-cluster. Een voorbeeld is een VM opnieuw wordt opgestart. Een typische gebruiksscenario's zijn gepland onderhoud voor OS- of Azure-infrastructuur. Zie [gepland onderhoud](#planned-maintenance). Gebruik de volgende opdracht uit om te Pacemaker plaatsen in de onderhoudsmodus:

<pre><code>
crm configure property maintenance-mode=true
</code></pre>

Wanneer u contact op met **crm status**, ziet u in de uitvoer die alle resources zijn gemarkeerd als **niet-beheerde**. Niet in deze status kunnen reageren het cluster op wijzigingen, zoals starten of stoppen van SAP HANA.
Het volgende voorbeeld wordt de uitvoer van de **crm status** opdracht terwijl het cluster in de onderhoudsmodus is:

<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Wed Sep 12 07:48:10 2018
Last change: Wed Sep 12 07:46:54 2018 by root via cibadmin on hso-hana-vm-s2-1

7 nodes configured
17 resources configured

              *** Resource management is DISABLED ***
  The cluster will not attempt to start, stop or recover services

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm (unmanaged)
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00] (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-0 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaTop_HSO_HDB00   (ocf::suse:SAPHanaTopology):    Started hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00] (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s1-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-1 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Slave hso-hana-vm-s2-2 (unmanaged)
     rsc_SAPHanaCon_HSO_HDB00   (ocf::suse:SAPHanaController):  Master hso-hana-vm-s2-0 (unmanaged)
     Stopped: [ hso-hana-dm hso-hana-vm-s1-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s2-0 (unmanaged)
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s2-0 (unmanaged)
</code></pre>


In dit voorbeeld opdracht laat zien hoe de onderhoudsmodus van het cluster beëindigen:

<pre><code>
crm configure property maintenance-mode=false
</code></pre>


Een andere **crm** opdracht wordt de volledige clusterconfiguratie in een editor, zodat u het kunt bewerken. Nadat de wijzigingen wordt opgeslagen, begint het cluster de nodige acties:

<pre><code>
crm configure edit
</code></pre>

Als u wilt kijken naar de volledige clusterconfiguratie, gebruikt u de **crm weergeven** optie:

<pre><code>
crm configure show
</code></pre>



Na fouten van clusterbronnen, de **crm status** opdracht geeft een lijst met **acties mislukt**. Zie het volgende voorbeeld van deze uitvoer:


<pre><code>
Stack: corosync
Current DC: hso-hana-dm (version 1.1.16-4.8-77ea74d) - partition with quorum
Last updated: Thu Sep 13 07:30:44 2018
Last change: Thu Sep 13 07:30:20 2018 by root via crm_attribute on hso-hana-vm-s1-0

7 nodes configured
17 resources configured

Online: [ hso-hana-dm hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started hso-hana-dm
 Clone Set: cln_SAPHanaTop_HSO_HDB00 [rsc_SAPHanaTop_HSO_HDB00]
     Started: [ hso-hana-vm-s1-0 hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-0 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm ]
 Master/Slave Set: msl_SAPHanaCon_HSO_HDB00 [rsc_SAPHanaCon_HSO_HDB00]
     Masters: [ hso-hana-vm-s1-0 ]
     Slaves: [ hso-hana-vm-s1-1 hso-hana-vm-s1-2 hso-hana-vm-s2-1 hso-hana-vm-s2-2 ]
     Stopped: [ hso-hana-dm hso-hana-vm-s2-0 ]
 Resource Group: g_ip_HSO_HDB00
     rsc_ip_HSO_HDB00   (ocf::heartbeat:IPaddr2):       Started hso-hana-vm-s1-0
     rsc_nc_HSO_HDB00   (ocf::heartbeat:anything):      Started hso-hana-vm-s1-0

Failed Actions:
* rsc_SAPHanaCon_HSO_HDB00_monitor_60000 on hso-hana-vm-s2-0 'unknown error' (1): call=86, status=complete, exitreason='none',
    last-rc-change='Wed Sep 12 17:01:28 2018', queued=0ms, exec=277663ms
</code></pre>

Dit is nodig om een cluster opschonen na fouten doen. Gebruik de **crm** opdracht opnieuw uit en gebruik de opdrachtoptie **opschonen** mislukt om te verwijderen van deze actie-items. Naam van de bijbehorende clusterbron als volgt:

<pre><code>
crm resource cleanup rsc_SAPHanaCon_HSO_HDB00
</code></pre>

De opdracht moet retourneren van uitvoer, zoals het volgende voorbeeld:

<pre><code>
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-dm
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s1-2
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-0
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-1
Cleaned up rsc_SAPHanaCon_HSO_HDB00:0 on hso-hana-vm-s2-2
Waiting for 7 replies from the CRMd....... OK
</code></pre>



## <a name="failover-or-takeover"></a>Failover of overnemen

Zoals besproken in [belangrijke opmerkingen](#important-notes), moet u een standard correct afsluiten niet gebruiken voor het testen van de failover-cluster of SAP HANA HSR overname. In plaats daarvan wordt aangeraden een kernelprobleem activeren, afdwingen dat de Resourcemigratie van een of afgesloten mogelijk alle netwerken op het niveau van het besturingssysteem van een virtuele machine. Een andere methode is de **crm \<knooppunt\> stand-by** opdracht. Zie de [SUSE document][sles-12-ha-paper]. 

De volgende drie Voorbeeldopdrachten kunnen afdwingen dat een clusterfailover:

<pre><code>
echo c &gt /proc/sysrq-trigger

crm resource migrate msl_SAPHanaCon_HSO_HDB00 hso-hana-vm-s2-0 force

wicked ifdown eth0
wicked ifdown eth1
wicked ifdown eth2
......
wicked ifdown eth&ltn&gt
</code></pre>

Zoals beschreven in [gepland onderhoud](#planned-maintenance), een goede manier voor het bewaken van de activiteiten van het cluster is om uit te voeren **SAPHanaSR showAttr** met de **watch** opdracht:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Het helpt tevens om te kijken naar de status van de SAP HANA-landschap die afkomstig zijn van een SAP-Python-script. Configureren van het cluster is op zoek naar de waarde van deze status. Zal het duidelijk wanneer u over een storing voor een worker-knooppunt nadenkt. Als een worker-knooppunt uitvalt, plaatsingsfout SAP HANA niet onmiddellijk een voor de status van de gehele scale-out-systeem. 

Er zijn enkele nieuwe pogingen om te voorkomen dat onnodige failovers. Het cluster alleen als de status van verandert reageert **Ok**, retourwaarde **4**naar **fout**, retourwaarde **1**. Zodat deze juist is als de uitvoer van **SAPHanaSR showAttr** ziet u een virtuele machine met de status **offline**. Maar er is geen activiteit nog om over te schakelen primaire en secundaire. Er is geen cluster-activiteit wordt geactiveerd als SAP HANA heeft niet een fout geretourneerd.

U kunt de status van SAP HANA Liggend als gebruiker bewaken  **\<HANA SID\>adm** door het SAP-Python-script als volgt aan te roepen. Mogelijk moet het pad aan te passen:

<pre><code>
watch python /hana/shared/HSO/exe/linuxx86_64/HDB_2.00.032.00.1533114046_eeaf4723ec52ed3935ae0dc9769c9411ed73fec5/python_support/landscapeHostConfiguration.py
</code></pre>

De uitvoer van deze opdracht moet eruitzien als in het volgende voorbeeld. De **hoststatus** kolom en de **algemene hoststatus** beide belangrijk zijn. De werkelijke uitvoer is groter, met extra kolommen.
Als u de uitvoertabel beter leesbare in dit document, zijn de meeste kolommen aan de rechterkant verwijderd:

<pre><code>
| Host             | Host   | Host   | Failover | Remove | 
|                  | Active | Status | Status   | Status | 
|                  |        |        |          |        | 
| ---------------- | ------ | ------ | -------- | ------ |    .......
| hso-hana-vm-s2-0 | yes    | ok     |          |        |        
| hso-hana-vm-s2-1 | yes    | ok     |          |        |         
| hso-hana-vm-s2-2 | yes    | ok     |          |        |        

overall host status: ok
</code></pre>


Er is een andere opdracht om te controleren op huidige activiteiten van het cluster. Zie de volgende opdracht en de uitvoer staart nadat het hoofdknooppunt van de primaire site is afgebroken. U ziet de lijst van acties zoals overgang **bevordering van** het voormalige secundaire hoofdknooppunt, **hso-hana-vm-s2-0**, als de nieuwe primaire master. Als alles goed is, en alle activiteiten zijn voltooid, dit **overgang samenvatting** lijst is leeg zijn.

<pre><code>
 crm_simulate -Ls

...........

Transition Summary:
 * Fence hso-hana-vm-s1-0
 * Stop    rsc_SAPHanaTop_HSO_HDB00:1   (hso-hana-vm-s1-0)
 * Demote  rsc_SAPHanaCon_HSO_HDB00:1   (Master -> Stopped hso-hana-vm-s1-0)
 * Promote rsc_SAPHanaCon_HSO_HDB00:5   (Slave -> Master hso-hana-vm-s2-0)
 * Move    rsc_ip_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
 * Move    rsc_nc_HSO_HDB00     (Started hso-hana-vm-s1-0 -> hso-hana-vm-s2-0)
</code></pre>



## <a name="planned-maintenance"></a>Gepland onderhoud 

Er zijn verschillende use cases als het gaat om gepland onderhoud. Een vraag is of het zojuist onderhoud aan de infrastructuur, zoals wijzigingen op het niveau van het en schijfconfiguratie of een upgrade van HANA.
U kunt aanvullende informatie vinden in documenten van SUSE zoals [richting nul Downtime] [ sles-zero-downtime-paper] of [SAP HANA SR prestaties geoptimaliseerd Scenario] [ sles-12-for-sap]. Deze documenten worden ook voorbeelden die laten hoe zien het handmatig migreren van een primaire bevatten.

Intense interne testen is om te controleren of de infrastructuur onderhoud use-case uitgevoerd. We besloten om te voorkomen dat eventuele problemen met betrekking tot het migreren van de primaire, altijd een primaire migreren voordat u een cluster in de onderhoudsmodus. Op deze manier is het niet nodig om te maken van het cluster te maken over de voormalige situatie: welke kant primaire is en welke secundaire.

Er zijn twee verschillende situaties in dit opzicht:

- **Gepland onderhoud op de huidige secundaire**. In dit geval kunt u zojuist hebt het cluster in de onderhoudsmodus plaatsen en het werk op de secundaire zonder gevolgen voor het cluster.

- **Gepland onderhoud op de huidige primaire**. Zodat gebruikers werken tijdens het onderhoud blijven kunnen, moet u een failover afgedwongen. Met deze methode moet u de clusterfailover door Pacemaker en niet alleen op het niveau van de SAP HANA HSR activeren. De installatie Pacemaker wordt automatisch geactiveerd voor de SAP HANA-overname. U moet ook om uit te voeren van de failover voordat u het cluster in de onderhoudsmodus plaatst.

De procedure voor het onderhoud op de huidige secundaire site is als volgt:

1. Het cluster in de onderhoudsmodus plaatsen.
2. Het werk op de secundaire site uitvoeren. 
3. Einde van de onderhoudsmodus van het cluster.

De procedure voor het onderhoud op de huidige primaire site is wat ingewikkelder:

1. Handmatig een failover of overnemen van de SAP HANA via een Resourcemigratie Pacemaker activeren. Bekijk de details die volgen.
2. SAP HANA op de vorige primaire site wordt afgesloten door de clusterinstallatie.
3. Het cluster in de onderhoudsmodus plaatsen.
4. Nadat de onderhoudswerkzaamheden is voltooid, registreert u de vorige primaire als de nieuwe secundaire site.
5. Opschonen van de configuratie van het cluster. Bekijk de details die volgen.
6. Einde van de onderhoudsmodus van het cluster.


Migreren van een resource, voegt een vermelding toe aan de configuratie van het cluster. Een voorbeeld dwingt een failover. U hebt voor het opschonen van deze items voordat u de onderhoudsmodus beëindigen. Zie het volgende voorbeeld.

Eerst een clusterfailover geforceerd door te migreren de **msl** resource die u wilt de huidige secundaire hoofdknooppunt. Met deze opdracht wordt een waarschuwing weergegeven die een **verplaatsen beperking** is gemaakt:

<pre><code>
crm resource migrate msl_SAPHanaCon_HSO_HDB00 force

INFO: Move constraint created for msl_SAPHanaCon_HSO_HDB00
</code></pre>


Controleer het failoverproces via de opdracht **SAPHanaSR showAttr**. Voor het controleren van de status van het cluster, een specifieke shell-venster geopend en start de opdracht met **watch**:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

De handmatige failover moet worden weergegeven in de uitvoer. Hebt u het voormalige secundaire hoofdknooppunt **gepromoveerd**, in dit voorbeeld **hso-hana-vm-s2-0**. De vorige primaire site is gestopt, **lss** waarde **1** voor voormalige primaire hoofdknooppunt **hso-hana-vm-s1-0**: 

<pre><code>
Global cib-time                 prim  sec srHook sync_state
------------------------------------------------------------
global Wed Sep 12 07:40:02 2018 HSOS2 -   SFAIL  SFAIL


Sites lpt        lss mns              srr
------------------------------------------
HSOS1 10         1   hso-hana-vm-s1-0 P
HSOS2 1536738002 4   hso-hana-vm-s2-0 P


Hosts            clone_state node_state roles                        score  site
----------------------------------------------------------------------------------
hso-hana-dm                  online
hso-hana-vm-s1-0 UNDEFINED   online     master1::worker:             150    HSOS1
hso-hana-vm-s1-1 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s1-2 DEMOTED     online     slave::worker:               -10000 HSOS1
hso-hana-vm-s2-0 PROMOTED    online     master1:master:worker:master 150    HSOS2
hso-hana-vm-s2-1 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
hso-hana-vm-s2-2 DEMOTED     online     slave:slave:worker:slave     -10000 HSOS2
</code></pre>

Nadat de failover-cluster en de overname van SAP HANA, plaatst u het cluster in de onderhoudsmodus zoals beschreven in [Pacemaker](#pacemaker).

De opdrachten **SAPHanaSR showAttr** en **crm status** geven over de beperkingen die zijn gemaakt door de Resourcemigratie. Er is een optie voor het zichtbaar maken voor deze beperkingen om weer te geven van de configuratie van het volledige cluster resource met de volgende opdracht:

<pre><code>
crm configure show
</code></pre>

In de configuratie van het cluster, moet u een nieuwe locatiebeperking veroorzaakt door de voormalige handmatige Resourcemigratie vinden. In dit voorbeeld begint met **locatie cli -**:

<pre><code>
location cli-ban-msl_SAPHanaCon_HSO_HDB00-on-hso-hana-vm-s1-0 msl_SAPHanaCon_HSO_HDB00 role=Started -inf: hso-hana-vm-s1-0
</code></pre>

Helaas dergelijke beperkingen kunnen van invloed zijn op het algehele gedrag van de cluster. Het is dus verplicht om ze te verwijderen opnieuw voordat u het hele systeem een back-up zet. Met de **unmigrate** opdracht, is het mogelijk om de Locatiebeperkingen die zijn gemaakt voordat u op te schonen. De naamgeving kan een beetje verwarrend zijn. Het niet wordt geprobeerd om de resource opnieuw migreren naar de oorspronkelijke virtuele machine waarvan deze is gemigreerd. Deze worden alleen de Locatiebeperkingen en bijbehorende gegevens ook worden geretourneerd wanneer u de opdracht uitvoert:


<pre><code>
crm resource unmigrate msl_SAPHanaCon_HSO_HDB00

INFO: Removed migration constraints for msl_SAPHanaCon_HSO_HDB00
</code></pre>

Aan het einde van de onderhoudswerkzaamheden, u de cluster-onderhoudsmodus stoppen zoals wordt weergegeven in [Pacemaker](#pacemaker).



## <a name="hbreport-to-collect-log-files"></a>hb_report om logboekbestanden te verzamelen

Voor het analyseren van problemen met Pacemaker cluster, het is nuttig en ook aangevraagd door SUSE-ondersteuning om uit te voeren de **hb_report** hulpprogramma. Hiermee verzamelt u alle belangrijke logboekbestanden die u nodig hebt voor het analyseren van wat is er gebeurd. Deze voorbeeld-aanroep maakt gebruik van een begin- en -tijd waarop een bepaald incident is opgetreden. Zie ook [belangrijke opmerkingen](#important-notes):

<pre><code>
hb_report -f "2018/09/13 07:36" -t "2018/09/13 08:00" /tmp/hb_report_log
</code></pre>

De opdracht vertelt u waar het de gecomprimeerde logboekbestanden plaatsen:

<pre><code>
The report is saved in /tmp/hb_report_log.tar.bz2
Report timespan: 09/13/18 07:36:00 - 09/13/18 08:00:00
</code></pre>

U kunt vervolgens de afzonderlijke bestanden via de standaard uitpakken **tar** opdracht:

<pre><code>
tar -xvf hb_report_log.tar.bz2
</code></pre>

Wanneer u de uitgepakte bestanden bekijkt, kunt u de logboekbestanden vinden. De meeste van deze zijn geplaatst in afzonderlijke mappen voor elk knooppunt in het cluster:

<pre><code>
-rw-r--r-- 1 root root  13655 Sep 13 09:01 analysis.txt
-rw-r--r-- 1 root root  14422 Sep 13 09:01 description.txt
-rw-r--r-- 1 root root      0 Sep 13 09:01 events.txt
-rw-r--r-- 1 root root 275560 Sep 13 09:00 ha-log.txt
-rw-r--r-- 1 root root     26 Sep 13 09:00 ha-log.txt.info
drwxr-xr-x 4 root root   4096 Sep 13 09:01 hso-hana-dm
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s1-2
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-0
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-1
drwxr-xr-x 3 root root   4096 Sep 13 09:01 hso-hana-vm-s2-2
-rw-r--r-- 1 root root 264726 Sep 13 09:00 journal.log
</code></pre>


Binnen het tijdsbereik dat is opgegeven, het huidige hoofdknooppunt **hso-hana-vm-s1-0** is afgebroken. U vindt vermeldingen die betrekking hebben op deze gebeurtenis in de **journal.log**:

<pre><code>
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: (to hsoadm) root on none
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session opened for user hsoadm by (uid=0)
2018-09-13T07:38:01+0000 hso-hana-vm-s2-1 systemd[1]: Started Session c44290 of user hsoadm.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] A new membership (10.0.0.13:120996) was formed. Members left: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [TOTEM ] Failed to receive the leave message. failed: 1
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Removing all hso-hana-vm-s1-0 attributes for peer loss
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 attrd[28313]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 stonith-ng[28311]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [QUORUM] Members[6]: 7 2 3 4 5 6
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 corosync[28302]:   [MAIN  ] Completed service synchronization, ready to provide service.
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 crmd[28315]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 pacemakerd[28308]:   notice: Node hso-hana-vm-s1-0 state is now lost
2018-09-13T07:38:02+0000 hso-hana-vm-s2-1 cib[28310]:   notice: Purged 1 peer with id=1 and/or uname=hso-hana-vm-s1-0 from the membership cache
2018-09-13T07:38:03+0000 hso-hana-vm-s2-1 su[93494]: pam_unix(su-l:session): session closed for user hsoadm
</code></pre>

Een ander voorbeeld is het logboekbestand Pacemaker op de secundaire master is geworden van de nieuwe primaire master. Dit fragment ziet u dat de status van het primaire hoofdknooppunt afgesloten is ingesteld op **offline**:

<pre><code>
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 3 still member of group stonith-ng (peer=hso-hana-vm-s1-2, counter=5.1)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 4 still member of group stonith-ng (peer=hso-hana-vm-s2-0, counter=5.2)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 5 still member of group stonith-ng (peer=hso-hana-vm-s2-1, counter=5.3)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 6 still member of group stonith-ng (peer=hso-hana-vm-s2-2, counter=5.4)
Sep 13 07:38:02 [4178] hso-hana-vm-s2-0 stonith-ng:     info: pcmk_cpg_membership:      Node 7 still member of group stonith-ng (peer=hso-hana-dm, counter=5.5)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 1 left group crmd (peer=hso-hana-vm-s1-0, counter=5.0)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: crm_update_peer_proc:     pcmk_cpg_membership: Node hso-hana-vm-s1-0[1] - corosync-cpg is now offline
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: peer_update_callback:     Client hso-hana-vm-s1-0/peer now has status [offline] (DC=hso-hana-dm, changed=4000000)
Sep 13 07:38:02 [4184] hso-hana-vm-s2-0       crmd:     info: pcmk_cpg_membership:      Node 2 still member of group crmd (peer=hso-hana-vm-s1-1, counter=5.0)
</code></pre>





## <a name="sap-hana-globalini"></a>SAP HANA global.ini


De volgende fragmenten zijn uit de SAP HANA **global.ini** -bestand op de cluster-site 2. In dit voorbeeld ziet u de hostnaam van de vermeldingen voor naamomzetting voor het gebruik van verschillende netwerken voor SAP HANA-communicatie en HSR:

<pre><code>
[communication]
tcp_keepalive_interval = 20
internal_network = 10.0.2/24
listeninterface = .internal
</code></pre>


<pre><code>
[internal_hostname_resolution]
10.0.2.40 = hso-hana-vm-s2-0
10.0.2.42 = hso-hana-vm-s2-2
10.0.2.41 = hso-hana-vm-s2-1
</code></pre>

<pre><code>
[ha_dr_provider_SAPHanaSR]
provider = SAPHanaSR
path = /hana/shared/myHooks
execution_order = 1
</code></pre>

<pre><code>
[system_replication_communication]
listeninterface = .internal

[system_replication_hostname_resolution]
10.0.1.30 = hso-hana-vm-s1-0
10.0.1.31 = hso-hana-vm-s1-1
10.0.1.32 = hso-hana-vm-s1-2
10.0.1.40 = hso-hana-vm-s2-0
10.0.1.41 = hso-hana-vm-s2-1
10.0.1.42 = hso-hana-vm-s2-2
</code></pre>



## <a name="hawk"></a>Hawk

De clusteroplossing biedt een browserinterface die een GUI voor gebruikers die liever menu's en afbeeldingen met alle opdrachten op het niveau van de shell biedt.
Voor het gebruik van de browserinterface, Vervang **\<knooppunt\>** met een werkelijke SAP HANA-knooppunt in de volgende URL. Voer de referenties van het cluster (gebruiker **cluster**):

<pre><code>
https://&ltnode&gt:7630
</code></pre>

Deze schermafbeelding ziet u het clusterdashboard:


![Clusterdashboard Hawk](media/hana-vm-scale-out-HA-troubleshooting/hawk-1.png)


In dit voorbeeld ziet u de Locatiebeperkingen veroorzaakt door een clustermigratie resource, zoals uitgelegd in [gepland onderhoud](#planned-maintenance):


![Hawk lijst met beperkingen](media/hana-vm-scale-out-HA-troubleshooting/hawk-2.png)


U kunt ook uploaden de **hb_report** uitvoer in Hawk onder **geschiedenis**, als volgt wordt weergegeven. Zie [hb_report om logboekbestanden te verzamelen](#hbreport-to-collect-log-files): 

![Hawk uploaden hb_report uitvoer](media/hana-vm-scale-out-HA-troubleshooting/hawk-3.png)

Met de **geschiedenis Explorer**, gaat u vervolgens via alle van de cluster-overgangen opgenomen in de **hb_report** uitvoer:

![Hawk overgangen in de uitvoer hb_report](media/hana-vm-scale-out-HA-troubleshooting/hawk-4.png)

Deze laatste schermafbeelding ziet u de **Details** gedeelte van een enkele overgang. Het cluster wordt gereageerd op het vastlopen van een primaire hoofdknooppunt knooppunt **hso-hana-vm-s1-0**. Het is nu bevordering van het secundaire knooppunt als het nieuwe model **hso-hana-vm-s2-0**:

![Hawk overgang](media/hana-vm-scale-out-HA-troubleshooting/hawk-5.png)


## <a name="next-steps"></a>Volgende stappen

Deze handleiding voor probleemoplossing beschrijft hoge beschikbaarheid voor SAP HANA in een scale-out-configuratie. Naast de database is een ander belangrijk onderdeel in een SAP-landschap dat de SAP NetWeaver-stack. Meer informatie over [hoge beschikbaarheid voor SAP NetWeaver op Azure virtual machines die gebruikmaken van SUSE Enterprise Linux Server][sap-nw-ha-guide-sles].

