---
title: Probleemoplossing voor SAP HANA 2.0 scale-out HSR Pacemaker setup met SLES 12 SP3 op Azure virtual machines | Microsoft Docs
description: Handleiding voor het controleren en oplossen van een complexe hoge beschikbaarheid voor scale-out-configuratie voor SAP HANA is op basis van SAP HANA System Replication (HSR) en Pacemaker op SLES 12 SP3 die worden uitgevoerd op virtuele machines van Azure
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
ms.openlocfilehash: 6c0d6397246e8b8db1d59c26229e37a722d49f48
ms.sourcegitcommit: 5b8d9dc7c50a26d8f085a10c7281683ea2da9c10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47184863"
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


In dit artikel is geschreven om te controleren van de clusterconfiguratie Pacemaker voor SAP HANA scale-out die worden uitgevoerd op virtuele machines van Azure. De installatie is uitgevoerd in combinatie met SAP HANA System Replication (HSR) en de SUSE-RPM-pakket SAPHanaSR ScaleOut. Alle tests zijn uitgevoerd op SUSE SLES 12 SP3 alleen. Er zijn verschillende secties die betrekking hebben op verschillende gebieden, maar ook Voorbeeldopdrachten en fragmenten van configuratiebestanden. Deze voorbeelden wordt aanbevolen als een methode om te controleren en controleer de instellingen voor het hele cluster.



## <a name="important-notes"></a>Belangrijke opmerkingen

Alle tests voor SAP HANA scale-out in combinatie met SAP HANA-Systeemreplicatie en Pacemaker is uitgevoerd met SAP HANA 2.0 alleen. De besturingssysteemversie is SUSE Linux Enterprise Server 12 SP3 voor SAP-toepassingen. Daarnaast is het meest recente RPM-pakket SAPHanaSR ScaleOut van SUSE gebruikt voor het instellen van het cluster pacemaker.
SUSE gepubliceerd een gedetailleerde beschrijving van deze instelling geoptimaliseerd voor prestaties, die kan worden gevonden [hier][sles-hana-scale-out-ha-paper]

Voor virtuele machine-typen, die worden ondersteund voor SAP HANA scale-out-of de [SAP HANA-gecertificeerde IaaS-directory][sap-hana-iaas-list]

Er is een technisch probleem met SAP HANA scale-out in combinatie met meerdere subnetten en vnic's en het instellen van HSR. Het is verplicht gebruik van de meest recente patches voor SAP HANA 2.0 waarbij dit probleem is opgelost. De volgende SAP HANA-versies worden ondersteund: 

**rev2.00.024.04 of hoger & rev2.00.032 of hoger.**

Volg deze in het geval moet er een situatie, die ondersteuning van SUSE vereist [handleiding][suse-pacemaker-support-log-files]. Alle gegevens verzamelen over de SAP HANA HA-cluster zoals beschreven in het artikel. Deze informatie moet het SUSE-ondersteuningsteam voor verdere analyse.

Tijdens interne tests van, gebeurde het de clusterinstallatie raakten door een normale correct VM afsluiten via Azure portal. Het is daarom raadzaam om te testen van een clusterfailover via andere methoden. Methoden, zoals het forceren van een kernelprobleem gebruiken of de netwerken afgesloten of migreren de **msl** resource (Zie de informatie in de onderstaande gedeelten). De veronderstelling is dat een standard afsluiten gebeurt er met de bedoeling. Het beste voorbeeld voor het afsluiten van een opzettelijke onderhoud is (Zie de informatie in de sectie over gepland onderhoud).

Tijdens interne tests van gebeurde het dat de clusterinstallatie na een handmatige SAP HANA-overname raakten bij het cluster in de onderhoudsmodus bevindt is. Het is raadzaam deze om terug te schakelen handmatig opnieuw, voordat u de onderhoudsmodus van het cluster beëindigen. Een andere optie is een failover wordt geactiveerd voordat u het cluster in de onderhoudsmodus (Zie de sectie over gepland onderhoud voor meer informatie). De documentatie van SUSE wordt beschreven hoe u het cluster in dit verband met de crm-opdracht kunt herstellen. Maar de benadering die worden vermeld voordat leek te krachtige worden tijdens interne tests en nooit gebleken dat met een onverwachte neveneffecten.

Wanneer deze opdracht met de crm migreert mis niet opschonen van de configuratie van het cluster. Locatiebeperkingen, die u misschien niet op de hoogte van wordt toegevoegd. Deze beperkingen een invloed hebben op het gedrag van het cluster (Zie voor meer informatie de sectie over gepland onderhoud).



## <a name="test-system-description"></a>Beschrijving van het systeem testen

Voor SAP HANA scale-out HA-verificatie en -certificering die is een instelling gebruikt, die bestaat uit twee systemen met drie knooppunten voor SAP HANA elk - één master en twee werkrollen. Hier volgt de lijst met namen van de virtuele machine en interne IP-adressen. Alle controle-voorbeelden verder zijn omlaag uitgevoerd op deze virtuele machines. Met behulp van deze VM-namen en IP kunnen-adressen in de voorbeeldopdrachten helpen bij het beter begrip van de opdrachten en de uitvoer.


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

De volgende aanbevelingen voor SAP HANA-netwerken zijn drie subnetten gemaakt binnen een virtueel Azure-netwerk. SAP HANA scale-out op Azure moet worden geïnstalleerd in de niet-gedeelde modus, wat betekent dat elk knooppunt maakt gebruik van lokale schijfvolumes voor **/hana/gegevens** en **/hana/log**. Vanwege het gebruik van alleen lokale schijfvolumes is het niet nodig voor het definiëren van een apart subnet voor de opslag:

- 10.0.2.0/24 voor de communicatie tussen knooppunten SAP HANA
- 10.0.1.0/24 voor SAP HANA-Systeemreplicatie HSR
- 10.0.0.0/24 voor alle andere

Voor informatie over SAP HANA-configuratie voor het werken met meerdere netwerken Zie de sectie **global.ini** verder omlaag.

Overeenkomt met het aantal subnetten elke virtuele machine in het cluster heeft drie vnic's. [Dit] [ azure-linux-multiple-nics] artikel beschrijft een mogelijk probleem met routering in Azure bij het implementeren van een Linux-VM. Dit specifieke routering onderwerp geldt alleen voor het gebruik van meerdere vnic's. Het probleem is opgelost door SUSE per standaard in SLES 12 SP3. Het artikel van SUSE over dit onderwerp vindt u [hier][suse-cloud-netconfig].


Als een basiscontrole uit om te controleren of SAP HANA juist is geconfigureerd voor het gebruik van meerdere netwerken, moet u alleen de onderstaande opdrachten uitvoeren. Eerste stap is eenvoudig om te controleren op het niveau van dat alle drie interne IP-adressen voor alle drie subnetten actief zijn. Als u de subnetten met verschillende IP-adresbereiken gedefinieerd die u moet de opdrachten aanpassen:

<pre><code>
ifconfig | grep "inet addr:10\."
</code></pre>

Hier volgt een voorbeeld van uitvoer van de tweede worker-knooppunt op locatie 2. Hier ziet u drie verschillende interne IP-adressen van eth0, eth1 en eth2:

<pre><code>
inet addr:10.0.0.42  Bcast:10.0.0.255  Mask:255.255.255.0
inet addr:10.0.1.42  Bcast:10.0.1.255  Mask:255.255.255.0
inet addr:10.0.2.42  Bcast:10.0.2.255  Mask:255.255.255.0
</code></pre>


Tweede stap is verificatie van SAP HANA-poorten voor de naamserver en HSR. SAP HANA moet luisteren op de overeenkomstige subnetten. U hebt om aan te passen van de opdrachten, afhankelijk van het aantal SAP HANA-exemplaar. Voor het testsysteem, was het instantienummer van de **00**. Er zijn verschillende manieren om te achterhalen welke poorten worden gebruikt. 

Hieronder ziet u een SQL-instructie retourneert de exemplaar-ID en het exemplaarnummer onder andere informatie:

<pre><code>
select * from "SYS"."M_SYSTEM_OVERVIEW"
</code></pre>

Als u de juiste poortnummers zoekt, kunt u bekijken, bijvoorbeeld in HANA Studio onder '**configuratie**' of via een SQL-instructie:

<pre><code>
select * from M_INIFILE_CONTENTS WHERE KEY LIKE 'listen%'
</code></pre>

Zoeken om elke poort die wordt gebruikt in de SAP-softwarestack met inbegrip van SAP HANA, [hier][sap-list-port-numbers].

Uitgaand van het getal exemplaar **00** in het testsysteem SAP HANA 2.0, is het het poortnummer voor de naamserver **30001**. Het poortnummer voor communicatie van HSR meta **40002**. Een optie is om te melden met een werkrolknooppunt en controleert u de services van het hoofdknooppunt. Hier is de controle uitgevoerd op de worker-knooppunt 2 op locatie 2 verbinding maken met het hoofdknooppunt op locatie 2.

Controleer de naamserver-poort:

<pre><code>
nc -vz 10.0.0.40 30001
nc -vz 10.0.1.40 30001
nc -vz 10.0.2.40 30001
</code></pre>

Het resultaat ziet er als het voorbeeld hieronder uitvoeren om te bewijzen dat de communicatie tussen knooppunten met behulp van subnet **10.0.2.0/24**.
Alleen de verbinding maken via een subnet **10.0.2.0/24** moet slagen:

<pre><code>
nc: connect to 10.0.0.40 port 30001 (tcp) failed: Connection refused
nc: connect to 10.0.1.40 port 30001 (tcp) failed: Connection refused
Connection to 10.0.2.40 30001 port [tcp/pago-services1] succeeded!
</code></pre>

Nu controleren voor de poort HSR **40002**:

<pre><code>
nc -vz 10.0.0.40 40002
nc -vz 10.0.1.40 40002
nc -vz 10.0.2.40 40002
</code></pre>

Het resultaat ziet er als het voorbeeld hieronder uitvoeren om te bewijzen dat subnet wordt gebruikt door de communicatie HSR **10.0.1.0/24**.
Alleen de verbinding maken via een subnet **10.0.1.0/24** moet slagen:

<pre><code>
nc: connect to 10.0.0.40 port 40002 (tcp) failed: Connection refused
Connection to 10.0.1.40 40002 port [tcp/*] succeeded!
nc: connect to 10.0.2.40 port 40002 (tcp) failed: Connection refused
</code></pre>



## <a name="corosync"></a>Corosync


Het configuratiebestand corosync is alleen correct als op elk knooppunt in het cluster met inbegrip van de maker voor hoofdknooppunten. Als het cluster lid worden van een knooppunt werkt niet zoals verwacht, maken en/of kopieer **/etc/corosync/corosync.conf** handmatig van/naar alle knooppunten en de service opnieuw starten.

Hier is de inhoud van **corosync.conf** uit het testsysteem als voorbeeld.

Eerste sectie is **totem** zoals beschreven in dit [documentatie] [ sles-pacemaker-ha-guide] (sectie clusterinstallatie stap 11). U kunt de waarde voor negeren **mcastaddr**. Houd het bestaande item. De vermeldingen voor **token** en **consensus** moet worden ingesteld op basis van de documentatie van Microsoft Azure-SAP HANA, die u kunt vinden [hier][sles-pacemaker-ha-guide]

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

De tweede sectie **logboekregistratie** niet is gewijzigd van de opgegeven standaardwaarden:

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

De derde sectie ziet u de **nodelist**. Alle knooppunten van het cluster hebben met hun knooppunt-id wordt weergegeven:

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

De documentatie over het instellen van een apparaat SBD op een Azure-VM wordt beschreven [hier] [ sles-pacemaker-ha-guide] (sectie sbd-de eerste optie).

Allereerst om te controleren is om te zoeken op de server SBD VM als er ACL-vermeldingen voor elk knooppunt in het cluster. Voer de volgende opdracht op de server SBD VM:


<pre><code>
targetcli ls
</code></pre>


Op het testsysteem, is de uitvoer van de opdracht hebt bekeken, zoals het onderstaande voorbeeld. De ACL-namen, zoals **iqn.2006-04.hso-db-0.local:hso-db-0** moeten worden ingevoerd als de initiatornaam van de bijbehorende op de virtuele machines. Elke virtuele machine moet een ander account.

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

Daarna controleren of de namen van de initiator op alle virtuele machines verschillend zijn en komen met de vermeldingen die overeen hierboven wordt weergegeven. Hier volgt een voorbeeld van worker-knooppunt 1 op site 1:

<pre><code>
cat /etc/iscsi/initiatorname.iscsi
</code></pre>

De uitvoer hebt bekeken, zoals het voorbeeld hieronder:

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

Controleer vervolgens of als de **detectie** correct werkt en voer de volgende opdracht op elk clusterknooppunt met behulp van het IP-adres van de server SBD VM:

<pre><code>
iscsiadm -m discovery --type=st --portal=10.0.0.19:3260
</code></pre>

De uitvoer moet eruitzien als het voorbeeld hieronder:

<pre><code>
10.0.0.19:3260,1 iqn.2006-04.dbhso.local:dbhso
</code></pre>

Volgende bewijs punt is om te verifiëren dat het knooppunt het SDB-apparaat ziet. Bekijk het op elk knooppunt met inbegrip van het hoofdknooppunt maker:

<pre><code>
lsscsi | grep dbhso
</code></pre>

De uitvoer moet eruitzien als het voorbeeld hieronder. Houd er rekening mee dat de namen verschillen (apparaatnaam mogelijk ook wijzigen nadat de virtuele machine opnieuw is opgestart):

<pre><code>
[6:0:0:0]    disk    LIO-ORG  sbddbhso         4.0   /dev/sdm
</code></pre>

Afhankelijk van de status van het systeem, kan het soms handig om opnieuw te starten van de iscsi-services voor het oplossen van problemen. Voer vervolgens de volgende opdrachten uit:

<pre><code>
systemctl restart iscsi
systemctl restart iscsid
</code></pre>


Vanaf elk knooppunt, kunt u controleren of alle knooppunten zijn **wissen**. Alleen Kijk uit naam van het juiste apparaat gebruiken op een specifiek knooppunt:

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


Een andere SBD controleren de **dump** optie van de opdracht sbd. Hier volgt een voorbeeld van een opdracht en de uitvoer van de meeste maker knooppunt waar de apparaatnaam niet is **sdm** maar **sdd**:

<pre><code>
sbd -d /dev/sdd dump
</code></pre>

De uitvoer (naast de naam van het apparaat) ziet dezelfde op alle knooppunten:

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

Controleren op een meer SBD is de mogelijkheid om een bericht verzenden naar een ander knooppunt. U uitvoeren de volgende opdracht op de worker-knooppunt 1 op locatie 2 om een bericht verzenden naar de worker-knooppunt 2 op locatie 2:

<pre><code>
sbd -d /dev/sdm message hso-hana-vm-s2-2 test
</code></pre>

Op de doel-kant van de VM - die was **hso-hana-vm-s2-2** in dit voorbeeld - vindt u de volgende vermelding in **/var/log/messages**:

<pre><code>
/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68:   notice: servant: Received command test from hso-hana-vm-s2-1 on disk /dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68
</code></pre>

Controleer als de vermeldingen in **/etc/sysconfig/sbd** komen overeen met de beschrijving in onze [documentatie] [ sles-pacemaker-ha-guide] (sectie sbd-de eerste optie). Controleer de opstartinstelling worden **/etc/iscsi/iscsid.conf** is ingesteld op automatisch.

Belangrijke vermeldingen in **/etc/sysconfig/sbd** (het aanpassen van de id-waarde indien nodig):

<pre><code>
SBD_DEVICE="/dev/disk/by-id/scsi-36001405e614138d4ec64da09e91aea68;"
SBD_PACEMAKER=yes
SBD_STARTMODE=always
SBD_WATCHDOG=yes
</code></pre>


Een ander artikel om te controleren is de opstartinstelling in **/etc/iscsi/iscsid.conf**. De vereiste instelling er is gebeurd door de **iscsiadm** weergegeven opdracht hieronder, die wordt beschreven in de documentatie. Het zinvol om te controleren en misschien aan te passen handmatig met **vi** als deze verschilt.

De opdracht uit om de opstartinstellingen:

<pre><code>
iscsiadm -m node --op=update --name=node.startup --value=automatic
</code></pre>

Vermelding in **/etc/iscsi/iscsid.conf**:

<pre><code>
node.startup = automatic
</code></pre>

Tijdens het testen en verificatie exemplaren is er gebeurd waar na het opnieuw opstarten van een virtuele machine het SBD-apparaat is niet zichtbaar meer. Er is een verschil tussen de opstartinstelling en welke yast2 hebt u geleerd. Als u wilt controleren van de instellingen, Voer onderstaande stappen:

1. Yast2 starten
2. Selecteer **netwerkservices** aan de linkerkant
3. Schuif omlaag aan de rechterkant op **iSCSI-Initiator** en selecteert u deze
4. In het volgende scherm onder het **Service** tabblad ziet u de naam van de unieke initiator voor het knooppunt
5. Boven de initiatornaam van de, zorg ervoor dat de **Service Start** waarde is ingesteld op **bij het opstarten**
6. Als dit niet het geval is, vervolgens instellen op **bij het opstarten** in plaats van **handmatig**
7. Naast het eerste tabblad om te schakelen **doelen verbonden**
8. Op het scherm verbonden doelen ziet u een vermelding voor het apparaat SBD, zoals in dit voorbeeld: **10.0.0.19:3260 iqn.2006-04.dbhso.local:dbhso**
9. Controleer als de waarde van het opstarten is ingesteld op '**onboot**"
10. Als dit niet het geval is, kiest u **bewerken** en wijzigen
11. Sla de wijzigingen en yast2 af te sluiten



## <a name="pacemaker"></a>Pacemaker

Wanneer alles juist is ingesteld, kunt u de volgende opdracht uitvoeren op elk knooppunt om de status van de service pacemaker te controleren:

<pre><code>
systemctl status pacemaker
</code></pre>

De bovenkant van de uitvoer moet eruitzien als het voorbeeld hieronder. Het is belangrijk dat de status na **Active** wordt weergegeven als **geladen** en **actief (actief)**. De status na "Geladen" moet worden weergegeven als **ingeschakeld**.

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

Als u wilt zien van alle geconfigureerde bronnen in pacemaker, voer de volgende opdracht:

<pre><code>
crm status
</code></pre>

De uitvoer moet eruitzien als het voorbeeld hieronder. Het is geen probleem als op de maker van de meeste VM gestopt, worden de resources cln en msl weergegeven (**hso-hana-dm**). Er is geen SAP HANA-installatie op de meeste maker-knooppunt. Daarom de **cln** en **msl** resources worden weergegeven als gestopt. Het is belangrijk dat deze ziet u de juiste totale aantal virtuele machines (**7**). Alle virtuele machines die deel van het cluster uitmaken moeten worden weergegeven met de status **Online**. De huidige primaire hoofdknooppunt moet juist worden herkend (in dit voorbeeld is het **hso-hana-vm-s1-0**).

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

Er is een belangrijk onderdeel van pacemaker moet worden geplaatst in de onderhoudsmodus. In deze modus kunt u wijzigingen aanbrengt (bijvoorbeeld een virtuele machine opnieuw opstarten) zonder waar een actie direct-cluster. Een typische gebruiksscenario's is gepland onderhoud voor OS- of Azure-infrastructuur (Zie ook afzonderlijke sectie over gepland onderhoud). Gebruik de volgende opdracht uit om te pacemaker plaatsen in de onderhoudsmodus:

<pre><code>
crm configure property maintenance-mode=true
</code></pre>

Tijdens het controleren met **crm status**, ziet u in de uitvoer die alle resources zijn gemarkeerd als **niet-beheerde**. In deze status wordt reageert het cluster niet op wijzigingen, zoals starten/stoppen SAP HANA.
Hier volgt een voorbeeld van de uitvoer van de **crm status** opdracht terwijl het cluster in de onderhoudsmodus is:

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


En in de onderstaande opdracht voorbeeld ziet u hoe u de onderhoudsmodus van het cluster beëindigen:

<pre><code>
crm configure property maintenance-mode=false
</code></pre>


Een andere crm-opdracht kunt aan de configuratie van het volledige cluster in een editor met de mogelijkheid om te bewerken. Nadat de wijzigingen zijn opgeslagen, begint het cluster de nodige acties:

<pre><code>
crm configure edit
</code></pre>

Als u wilt zien alleen de volledige clusterconfiguratie, gebruikt u de crm **weergeven** optie:

<pre><code>
crm configure show
</code></pre>



Na fouten van clusterbronnen dat toch gebeurt, die de **crm status** opdracht geeft een lijst met **acties mislukt**. Bekijk een voorbeeld voor deze onderstaande uitvoer:


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

Dit is nodig om een cluster opschonen na fouten doen. Alleen de crm-opdracht opnieuw gebruiken en gebruik de opdrachtoptie **opschonen** allerlei hiervan mislukt actie vermeldingen naamgeving van de bijbehorende clusterbron zoals hieronder wordt weergegeven:

<pre><code>
crm resource cleanup rsc_SAPHanaCon_HSO_HDB00
</code></pre>

De opdracht moet een uitvoer die lijkt op het onderstaande voorbeeld retourneren:

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



## <a name="failover--takeover"></a>Failover / overname

Zoals al is vermeld in de eerste sectie met belangrijke opmerkingen, moet u een standard correct afsluiten niet gebruiken voor het testen van de failover-cluster of SAP HANA HSR overname. In plaats daarvan is het raadzaam activeert, bijvoorbeeld een kernelprobleem of afdwingen dat de Resourcemigratie van een of afgesloten misschien alle netwerken op het niveau van een virtuele machine. Een andere manier is de **crm \<knooppunt\> stand-by** opdracht. Zie ook de SUSE-document, die kan worden gevonden [hier][sles-12-ha-paper]. Hieronder ziet u drie Voorbeeldopdrachten om af te dwingen een clusterfailover:

<pre><code>
echo c &gt /proc/sysrq-trigger

crm resource migrate msl_SAPHanaCon_HSO_HDB00 hso-hana-vm-s2-0 force

wicked ifdown eth0
wicked ifdown eth1
wicked ifdown eth2
......
wicked ifdown eth&ltn&gt
</code></pre>

Zoals ook wordt beschreven in de sectie over gepland onderhoud, een goede manier om te controleren van de activiteiten van het cluster is om uit te voeren **SAPHanaSR showAttr** met de **watch** opdracht:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

Bovendien helpt het om te kijken naar de status van de SAP HANA-landschap die afkomstig zijn van een SAP-python-script. De waarde van deze status is de service, die het configureren van het cluster is op zoek naar. Er zal het duidelijk is wanneer u nadenkt over een storing voor een worker-knooppunt. Als een worker-knooppunt uitgeschakeld wordt, zal SAP HANA niet onmiddellijk een fout op wanneer de status van de gehele scale-out-systeem retourneren. Er zijn enkele nieuwe pogingen om te voorkomen dat onnodige failovers. Alleen als de status van Ok (retourwaarde 4) (geretourneerde waarde 1) fout verandert reageert het cluster. Daarom is het juiste, als de uitvoer van **SAPHanaSR showAttr** ziet u een virtuele machine met de status **offline** , maar er is geen activiteit nog om over te schakelen van primaire en secundaire database. Er is geen cluster-activiteit wordt geactiveerd als SAP HANA heeft niet een fout geretourneerd.

U kunt de status van SAP HANA Liggend als gebruiker bewaken \<HANA SID\>adm door het aanroepen van de SAP-python script de volgende manier (mogelijk moet u het pad aan te passen):

<pre><code>
watch python /hana/shared/HSO/exe/linuxx86_64/HDB_2.00.032.00.1533114046_eeaf4723ec52ed3935ae0dc9769c9411ed73fec5/python_support/landscapeHostConfiguration.py
</code></pre>

De uitvoer van deze opdracht moet lijken op het onderstaande voorbeeld. Belangrijk is de **hoststatus** kolom, evenals de **algemene hoststatus**. De werkelijke uitvoer is in feite breder met extra kolommen.
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


Er is een andere opdracht om te controleren op huidige activiteiten van het cluster. Zie hieronder de opdracht en het einde van de uitvoer nadat het hoofdknooppunt van de primaire site is afgebroken. U ziet de lijst van acties zoals overgang **bevordering van** het voormalige secundaire hoofdknooppunt (**hso-hana-vm-s2-0**) als de nieuwe primaire master. Als alles ok is en alle activiteiten zijn voltooid, wordt deze lijst van **overgang samenvatting** moet leeg zijn.

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

Er zijn verschillende use cases als het gaat om gepland onderhoud. Een vraag is bijvoorbeeld, als dit alleen onderhoud aan de infrastructuur, zoals wijzigingen op besturingssysteemniveau en schijfconfiguratie of een upgrade van HANA.
U kunt aanvullende informatie vinden in documenten van SUSE zoals [hier] [ sles-zero-downtime-paper] of [een andere één hier][sles-12-for-sap]. Deze documenten bevatten ook voorbeelden over het handmatig migreren van een primaire.

Intense interne testen is om te controleren of de infrastructuur onderhoud use-case uitgevoerd. Het besluit is genomen om te voorkomen dat elk soort probleem met betrekking tot het migreren van de primaire, altijd een primaire migreren voordat u een cluster in de onderhoudsmodus. Op deze manier is het niet nodig om te maken van het cluster te maken over de voormalige situatie (welke kant primaire is en welke kant is secundaire) hebben.

Er zijn twee verschillende situaties in dit opzicht:

1. Gepland onderhoud op de huidige secundaire. 
   In dit geval, kunt u alleen het cluster in de onderhoudsmodus plaatsen en het werk op de secundaire zonder gevolgen voor het cluster

2. Gepland onderhoud op de huidige primaire. 
   Als u wilt toestaan dat de gebruikers blijven werken tijdens het onderhoud, is het nodig om een failover afgedwongen. Met deze methode moet u de clusterfailover door pacemaker en niet alleen op het niveau van de SAP HANA HSR activeren. De installatie pacemaker wordt automatisch geactiveerd voor de SAP HANA-overname. Bovendien is het nodig zijn om uit te voeren van de failover voordat u het cluster in de onderhoudsmodus.

De procedure voor het onderhoud op de huidige secundaire site wilt de volgende stappen uit:

1. Het cluster in de onderhoudsmodus plaatsen
2. Uitvoeren van het werk op de secundaire site 
3. De onderhoudsmodus van het cluster beëindigen

De procedure voor het onderhoud op de huidige primaire site is wat ingewikkelder:

1. Handmatig een failover wordt geactiveerd / SAP HANA-overname via een Resourcemigratie Pacemaker (Zie hieronder voor meer informatie)
2. SAP HANA op de vorige primaire site wordt ophalen afgesloten door de clusterinstallatie
3. Het cluster in de onderhoudsmodus plaatsen
4. Nadat de onderhoudswerkzaamheden is voltooid, registreert u de vorige primaire als de nieuwe secundaire site
5. Opschonen van de configuratie van het cluster (Zie hieronder voor meer informatie)
6. De onderhoudsmodus van het cluster beëindigen


Migreren van een bron (bijvoorbeeld om een failover afgedwongen) voegt u een vermelding toe aan de configuratie van het cluster. U hebt voor het opschonen van deze items voordat u de onderhoudsmodus te beëindigen. Hier volgt een voorbeeld:

Eerste stap is het afdwingen van een clusterfailover met de resource msl migreren naar de huidige secundaire hoofdknooppunt. De onderstaande opdracht wordt een waarschuwing weergegeven dat 'verplaatsen beperking' is gemaakt.

<pre><code>
crm resource migrate msl_SAPHanaCon_HSO_HDB00 force

INFO: Move constraint created for msl_SAPHanaCon_HSO_HDB00
</code></pre>


Controleer het failoverproces via de opdracht **SAPHanaSR showAttr**. Wat helpt bij het controleren van de status van het cluster is een speciale shell-venster openen en starten van de opdracht met **watch**:

<pre><code>
watch SAPHanaSR-showAttr
</code></pre>

De uitvoer moet overeenkomen met de handmatige failover. Hebt u het voormalige secundaire hoofdknooppunt **gepromoveerd** (in dit voorbeeld **hso-hana-vm-s2-0**) en de vorige primaire site is gestopt (**lss** waarde **1** voor voormalige primaire hoofdknooppunt **hso-hana-vm-s1-0**): 

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

Nadat de failover-cluster en de overname van SAP HANA, plaatst u het cluster in de onderhoudsmodus zoals beschreven in de sectie pacemaker.

De opdrachten **SAPHanaSR showAttr** of **crm status** geven over de beperkingen die zijn gemaakt door de Resourcemigratie. Er is een optie voor het zichtbaar maken voor deze beperkingen om weer te geven van de configuratie van het volledige cluster resource met de volgende opdracht:

<pre><code>
crm configure show
</code></pre>

In de configuratie van het cluster, moet u een nieuwe locatiebeperking veroorzaakt door de voormalige handmatige Resourcemigratie vinden. Hier volgt een voorbeeld (post beginnen met **locatie cli -**):

<pre><code>
location cli-ban-msl_SAPHanaCon_HSO_HDB00-on-hso-hana-vm-s1-0 msl_SAPHanaCon_HSO_HDB00 role=Started -inf: hso-hana-vm-s1-0
</code></pre>

Dergelijke beperkingen kunnen helaas een invloed hebben op het algehele gedrag van de cluster. Het is daarom verplicht om ze te verwijderen weer aan voordat u het hele systeem dat een back-up. Met de **unmigrate** is het mogelijk voor het opschonen van de Locatiebeperkingen die zijn gemaakt voordat de opdracht. De naamgeving kan een beetje verwarrend zijn. Betekent niet dat deze probeert te migreren van de resource terug naar de oorspronkelijke virtuele machine waarvan deze is gemigreerd. Deze worden alleen de Locatiebeperkingen en bijbehorende gegevens ook wordt bij het uitvoeren van de opdracht:


<pre><code>
crm resource unmigrate msl_SAPHanaCon_HSO_HDB00

INFO: Removed migration constraints for msl_SAPHanaCon_HSO_HDB00
</code></pre>

Aan het einde van de onderhoudswerkzaamheden stoppen u de cluster-onderhoudsmodus, zoals wordt weergegeven in de sectie pacemaker.



## <a name="hbreport-to-collect-log-files"></a>hb_report om logboekbestanden te verzamelen

Voor het analyseren van problemen met pacemaker cluster, het is nuttig en ook aangevraagd door SUSE-ondersteuning om uit te voeren de **hb_report** hulpprogramma. Hiermee verzamelt u alle belangrijke logboekbestanden, waardoor de analyse van wat is er gebeurd. Hier volgt een voorbeeld-aanroep met behulp van een begin- en -tijd waarop een bepaald incident is opgetreden (Zie eerste sectie ook over belangrijke opmerkingen):

<pre><code>
hb_report -f "2018/09/13 07:36" -t "2018/09/13 08:00" /tmp/hb_report_log
</code></pre>

De opdracht morsen af waar het de gecomprimeerde logboekbestanden plaatsen:

<pre><code>
The report is saved in /tmp/hb_report_log.tar.bz2
Report timespan: 09/13/18 07:36:00 - 09/13/18 08:00:00
</code></pre>

U kunt vervolgens de afzonderlijke bestanden via de standaard uitpakken **tar** opdracht:

<pre><code>
tar -xvf hb_report_log.tar.bz2
</code></pre>

De uitgepakte bestanden bekijkt u de logboekbestanden. De meeste van deze zijn geplaatst in afzonderlijke mappen voor elk knooppunt in het cluster:

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


Binnen het tijdsbereik, die is opgegeven het huidige hoofdknooppunt **hso-hana-vm-s1-0** is afgebroken. In de **journal.log** vindt u vermeldingen die betrekking hebben op deze gebeurtenis:

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

Een ander voorbeeld is het logboekbestand pacemaker op de secundaire master is geworden van de nieuwe primaire master. Hier volgt een fragment, die laat zien dat de status van het primaire hoofdknooppunt afgesloten is ingesteld op **offline**.

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


Hieronder ziet u fragmenten van de SAP HANA global.ini-bestand in de cluster-site 2 als voorbeeld om de vermeldingen voor naamomzetting van de hostnaam voor het gebruik van verschillende netwerken voor de communicatie tussen knooppunten SAP HANA en HSR weer te geven:

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



## <a name="hawk"></a>HAWK

De oplossing biedt ook een browserinterface, waardoor een mooie GUI voor mensen die liever menu's en afbeeldingen in vergelijking met de opdrachten op het niveau van de shell biedt.
Voor het gebruik van de browserinterface, worden de URL die hieronder wordt weergegeven en vervang **\<knooppunt\>** door een daadwerkelijke SAP HANA-knooppunt en voer de referenties van het cluster (gebruiker **hacluster**):

<pre><code>
https://&ltnode&gt:7630
</code></pre>

De volgende schermafbeelding ziet u het clusterdashboard:


![Clusterdashboard HAWK](media/hana-vm-scale-out-HA-troubleshooting/hawk-1.png)


Op de tweede schermafbeelding ziet u een voorbeeld van de Locatiebeperkingen veroorzaakt door een clustermigratie resource, zoals wordt beschreven in de sectie gepland onderhoud:


![HAWK lijst met beperkingen](media/hana-vm-scale-out-HA-troubleshooting/hawk-2.png)


Een andere mooie functie is de mogelijkheid om het uploaden van een **hb_report** uitvoer (Zie de sectie over **hb_report**) in **HAWK** onder **geschiedenis** als weergegeven op de volgende schermafbeelding:

![HAWK uploaden hb_report uitvoer](media/hana-vm-scale-out-HA-troubleshooting/hawk-3.png)

De **geschiedenis Explorer** toestaat via alle van de cluster-overgangen opgenomen in de **hb_report** uitvoer:

![Bekijk de overgangen in de uitvoer hb_report HAWK](media/hana-vm-scale-out-HA-troubleshooting/hawk-4.png)

Op de laatste schermafbeelding ziet u het gedeelte details van een enkele overgang, die laat zien dat het cluster moet worden gereageerd op het vastlopen van een primaire hoofdknooppunt (knooppunt **hso-hana-vm-s1-0**) en is nu bevordering van het secundaire knooppunt als het nieuwe model (**hso-hana-vm-s2-0**):

![Bekijk een enkele overgang HAWK](media/hana-vm-scale-out-HA-troubleshooting/hawk-5.png)


## <a name="next-steps"></a>Volgende stappen

Deze problemen oplossen handleiding draait om hoge beschikbaarheid voor SAP HANA in een scale-out-configuratie. Een ander belangrijk onderdeel van een SAP-landschap dat naast de database is de SAP NetWeaver-stack. Volgende lees dan over hoge beschikbaarheid voor SAP NetWeaver op Azure virtual machines met behulp van SUSE Enterprise Linux Server in [dit] [ sap-nw-ha-guide-sles] artikel.

