---
title: Hoge beschikbaarheid van SAP HANA op Azure virtuele Machines (VM's) | Microsoft Docs
description: Hoge beschikbaarheid van SAP HANA op Azure virtuele Machines (VM's) maken.
services: virtual-machines-linux
documentationcenter: 
author: MSSedusch
manager: timlt
editor: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/25/2017
ms.author: sedusch
ms.openlocfilehash: 951150e621d21037b0adde7287b9f985290d8d11
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/03/2017
---
# <a name="high-availability-of-sap-hana-on-azure-virtual-machines-vms"></a>Hoge beschikbaarheid van SAP HANA op Azure virtuele Machines (VM's)

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

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

On-premises kunt u kunt gebruiken beide HANA System Replication of gedeelde opslag gebruiken om hoge beschikbaarheid voor SAP HANA stand te brengen.
Wordt alleen ondersteund HANA System Replication instellen in Azure. SAP HANA replicatie bestaat uit één hoofdknooppunt en ten minste één slave knooppunt. Wijzigingen in de gegevens op het hoofdknooppunt worden gerepliceerd naar de knooppunten slave synchroon of asynchroon.

In dit artikel wordt beschreven hoe de virtuele machines te implementeren, configureren van de virtuele machines, cluster-framework hebt geïnstalleerd, installeren en configureren van SAP HANA System Replication.
Installatie opdrachten enzovoort exemplaarnummer 03 en HANA systeem-ID HDB wordt gebruikt in de voorbeeldconfiguraties.

Lees eerst de volgende opmerkingen bij de SAP en documenten

* SAP-notitie [1928533], die is:
  * Lijst met Azure VM-grootten die worden ondersteund voor de implementatie van SAP-software
  * Informatie over belangrijke capaciteit voor Azure VM-grootten
  * Ondersteunde SAP-software en besturingssysteem (OS) en combinaties van de database
  * Vereiste SAP-kernel-versie voor Windows en Linux op Microsoft Azure
* SAP-notitie [2015553] bevat vereisten voor software-implementaties SAP SAP ondersteund in Azure.
* SAP-notitie [2205917] heeft aanbevolen OS-instellingen voor SUSE Linux Enterprise Server voor SAP-toepassingen
* SAP-notitie [1944799] heeft SAP HANA richtlijnen voor SUSE Linux Enterprise Server voor SAP-toepassingen
* SAP-notitie [2178632] bevat gedetailleerde informatie over alle bewaking metrische gegevens die zijn gerapporteerd voor SAP in Azure.
* SAP-notitie [2191498] heeft de vereiste Hostagent SAP-versie voor Linux in Azure.
* SAP-notitie [2243692] bevat informatie over SAP licentieverlening op Linux in Azure.
* SAP-notitie [1984787] heeft algemene informatie over SUSE Linux Enterprise Server 12.
* SAP-notitie [1999351] bevat aanvullende informatie over probleemoplossing voor de Azure verbeterde extensie Monitoring voor SAP.
* [SAP Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) heeft alle SAP-opmerkingen voor Linux vereist.
* [Azure virtuele Machines, planning en implementatie voor SAP op Linux][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP op Linux (in dit artikel)][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP op Linux][dbms-guide]
* [SAP HANA SR prestaties geoptimaliseerd Scenario] [ suse-hana-ha-guide] de handleiding bevat alle benodigde informatie voor het instellen van SAP HANA System Replication on-premises. Deze handleiding gebruiken als basislijn.

## <a name="deploying-linux"></a>Linux implementeren

De resource-agent voor SAP HANA is opgenomen in SUSE Linux Enterprise Server voor SAP-toepassingen.
Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor SAP-toepassingen 12 met BYOS (uw eigen abonnement Bring) die u gebruiken kunt om nieuwe virtuele machines te implementeren.

### <a name="manual-deployment"></a>Handmatige implementatie

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Twee Storage-Accounts maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Maak een Load Balancer (intern)  
   Selecteer VNET van de bovenstaande stap
1. Virtuele Machine 1 maken  
   https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1  
   SLES voor SAP toepassingen 12 SP1 (BYOS)  
   Selecteer de Storage-Account 1  
   Beschikbaarheidsset selecteren  
1. Virtuele Machine 2 maken  
   https://Portal.Azure.com/#Create/SUSE-byos.SLES-for-SAP-byos12-SP1  
   SLES voor SAP toepassingen 12 SP1 (BYOS)  
   Selecteer Opslagaccount 2   
   Beschikbaarheidsset selecteren  
1. Gegevensschijven toevoegen
1. De load balancer configureren
    1. Een frontend-IP-adresgroep maken
        1. Openen van de load balancer, selecteer frontend-IP-adresgroep en klik op toevoegen
        1. Voer de naam van de nieuwe frontend IP-adresgroep (bijvoorbeeld hana-frontend)
       1. Klik op OK
        1. Nadat de nieuwe frontend-IP-adresgroep is gemaakt, noteert u het IP-adres
    1. Een back endpool maken
        1. Openen van de load balancer, back-endpools selecteren en klik op toevoegen
        1. Voer de naam van de nieuwe back-endpool (bijvoorbeeld hana-back-end)
        1. Klik op een virtuele machine toevoegen
        1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt
        1. Selecteer de virtuele machines van het SAP HANA-cluster
        1. Klik op OK
    1. Een health test maken
       1. Openen van de load balancer, statuscontroles selecteren en klik op toevoegen
        1. Voer de naam van de nieuwe health test (bijvoorbeeld hana-hp)
        1. Selecteer TCP als protocol, poort 625**03**, houd Interval 5 en de drempelwaarde voor onjuiste status 2
        1. Klik op OK
    1. Taakverdelingsregels maken
        1. Open de load balancer en taakverdelingsregels Selecteer klikt u op toevoegen
        1. Voer de naam van de load balancer-regel (bijvoorbeeld hana-lb-3**03**15)
        1. Selecteer de frontend-IP-adres, back-endpool en health test u eerder hebt gemaakt (bijvoorbeeld hana-frontend)
        1. Protocol TCP houden, voert u poort 3**03**15
        1. Verhoog de time-out voor inactiviteit tot 30 minuten
       1. **Zorg ervoor dat u kunt zwevend IP inschakelen**
        1. Klik op OK
        1. Herhaal de stappen hierboven voor poort 3**03**17

### <a name="deploy-with-template"></a>Implementeren met sjabloon
U kunt een van de snel starten-sjablonen op github gebruiken voor het implementeren van alle vereiste bronnen. De sjabloon wordt geïmplementeerd voor de virtuele machines, de load balancer, beschikbaarheidsset enzovoort. Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [databasesjabloon] [ template-multisid-db] of de [geconvergeerde sjabloon] [ template-converged] in de Azure Portal maakt de databasesjabloon alleen de regels voor taakverdeling voor een database terwijl de geconvergeerde sjabloon ook de regels voor taakverdeling voor een ASC's / SCS en Ebruikers (alleen voor Linux)-exemplaar maakt. Als u van plan bent een SAP NetWeaver gebaseerd systeem installeren en u ook wilt voor het exemplaar ASC's / SCS installeren op de dezelfde machines, gebruikt u de [geconvergeerde sjabloon][template-converged].
1. Voer de volgende parameters
    1. SAP-systeem-Id  
       Voer het SAP-systeem Id van het SAP-systeem die u wilt installeren. De Id zal worden gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
    1. Stack-Type (alleen van toepassing als u de sjabloon geconvergeerde gebruikt)  
       Selecteer het type van de stack SAP NetWeaver
    1. Type besturingssysteem  
       Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12 BYOS
    1. DB-Type  
       Selecteer HANA
    1. Grootte van het SAP  
       De hoeveelheid SAP's u het nieuwe systeem krijgt. Als u niet zeker weet hoeveel SAP's u het systeem moet, vraag uw SAP-technologie Partner of System Integrator
    1. Beschikbaarheid van het systeem  
       Selecteer HA
    1. Gebruikersnaam van de beheerder en het wachtwoord van beheerder  
       Een nieuwe gebruiker wordt gemaakt die kunnen worden gebruikt voor aanmelding bij de computer.
    1. Nieuwe of bestaande Subnet  
       Bepaalt of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u de bestaande.
    1. Subnet-Id  
    De ID van het subnet waarmee de virtuele machines moet worden verbonden. Selecteer het subnet van het VPN- of Express Route virtuele netwerk verbinding maken met de virtuele machine van uw on-premises netwerk. De ID meestal ziet eruit als /subscriptions/`<subscription id`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

## <a name="setting-up-linux-ha"></a>Instellen van Linux HA

De volgende items worden voorafgegaan door [A] - van toepassing op alle knooppunten [1] - alleen van toepassing op knooppunt 1 of [2] - alleen van toepassing op knooppunt 2.

1. [A] SLES voor SAP BYOS alleen - SLES om het gebruik van de opslagplaatsen te kunnen registreren
1. [A] SLES voor SAP BYOS alleen - toevoegen openbare cloud module
1. [A] SLES bijwerken
    ```bash
    sudo zypper update

    ```

1. [1] ssh toegang inschakelen
    ```bash
    sudo ssh-keygen -tdsa
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key
    sudo cat /root/.ssh/id_dsa.pub
    ```

2. [2] ssh toegang inschakelen
    ```bash
    sudo ssh-keygen -tdsa

    # insert the public key you copied in the last step into the authorized keys file on the second server
    sudo vi /root/.ssh/authorized_keys
    
    # Enter file in which to save the key (/root/.ssh/id_dsa): -> ENTER
    # Enter passphrase (empty for no passphrase): -> ENTER
    # Enter same passphrase again: -> ENTER
    
    # copy the public key    
    sudo cat /root/.ssh/id_dsa.pub
    ```

1. [1] ssh toegang inschakelen
    ```bash
    # insert the public key you copied in the last step into the authorized keys file on the first server
    sudo vi /root/.ssh/authorized_keys
    
    ```

1. [A] HA-uitbreiding installeren
    ```bash
    sudo zypper install sle-ha-release fence-agents
    
    ```

1. [A] indeling van setup-schijf
    1. LVM  
    In het algemeen wordt aangeraden met LVM voor volumes die bij het opslaan van gegevens en logboekbestanden. Het volgende voorbeeld wordt ervan uitgegaan dat de virtuele machines vier gegevensschijven gekoppeld die moeten worden gebruikt voor het maken van twee volumes hebben.
        * Maak fysieke volumes voor alle schijven die u wilt gebruiken.
    <pre><code>
    sudo pvcreate /dev/sdc
    sudo pvcreate /dev/sdd
    sudo pvcreate /dev/sde
    sudo pvcreate /dev/sdf
    </code></pre>
        * Maak een groep volume voor de gegevensbestanden, één volume groep voor de logboekbestanden en één voor de gedeelde map van SAP HANA
    <pre><code>
    sudo vgcreate vg_hana_data /dev/sdc /dev/sdd
    sudo vgcreate vg_hana_log /dev/sde
    sudo vgcreate vg_hana_shared /dev/sdf
    </code></pre>
        * De logische volumes maken
    <pre><code>
    sudo lvcreate -l 100%FREE -n hana_data vg_hana_data
    sudo lvcreate -l 100%FREE -n hana_log vg_hana_log
    sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared
    sudo mkfs.xfs /dev/vg_hana_data/hana_data
    sudo mkfs.xfs /dev/vg_hana_log/hana_log
    sudo mkfs.xfs /dev/vg_hana_shared/hana_shared
    </code></pre>
        * Maak de mappen koppelen en kopieer de UUID van alle logische volumes
    <pre><code>
    sudo mkdir -p /hana/data
    sudo mkdir -p /hana/log
    sudo mkdir -p /hana/shared
    # write down the id of /dev/vg_hana_data/hana_data, /dev/vg_hana_log/hana_log and /dev/vg_hana_shared/hana_shared
    sudo blkid
    </code></pre>
        * Fstab-vermeldingen voor de drie logische volumes maken
    <pre><code>
    sudo vi /etc/fstab
    </code></pre>
    Deze regel moet worden /etc/fstab invoegen
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_data/hana_data&gt;</b> /hana/data xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_log/hana_log&gt;</b> /hana/log xfs  defaults,nofail  0  2
    /dev/disk/by-uuid/<b>&lt;UUID of /dev/vg_hana_shared/hana_shared&gt;</b> /hana/shared xfs  defaults,nofail  0  2
    </code></pre>
        * Koppel de nieuwe volumes
    <pre><code>
    sudo mount -a
    </code></pre>
    1. Gewone schijven  
       Voor kleine of demo-systemen, kunt u uw bestanden HANA gegevens en logboekbestanden op één schijf plaatsen. De volgende opdrachten een partitie maken op /dev/sdc en formatteert u het met xfs.
    ```bash
    sudo fdisk /dev/sdc
    sudo mkfs.xfs /dev/sdc1
    
    # <a name="write-down-the-id-of-devsdc1"></a>Noteer de id van /dev/sdc1
    sudo/sbin/blkid sudo vi/etc/fstab-fouten
    ```

    Insert this line to /etc/fstab
    <pre><code>
    /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
    </code></pre>

    Create the target directory and mount the disk.

    ```bash
    sudo mkdir /hana
    sudo mount -a
    ```

1. [A] setup hostnaamomzetting voor alle hosts  
    U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen
    ```bash
    sudo vi /etc/hosts
    ```
    Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving    
    
    <pre><code>
    <b>&lt;IP address of host 1&gt; &lt;hostname of host 1&gt;</b>
    <b>&lt;IP address of host 2&gt; &lt;hostname of host 2&gt;</b>
    </code></pre>

1. [1] Cluster installeren
    ```bash
    sudo ha-cluster-init
    
    # Do you want to continue anyway? [y/N] -> y
    # Network address to bind to (e.g.: 192.168.1.0) [10.79.227.0] -> ENTER
    # Multicast address (e.g.: 239.x.x.x) [239.174.218.125] -> ENTER
    # Multicast port [5405] -> ENTER
    # Do you wish to use SBD? [y/N] -> N
    # Do you wish to configure an administration IP? [y/N] -> N
    ```
        
1. [2] knooppunt toevoegen aan cluster
    ```bash
    sudo ha-cluster-join
        
    # WARNING: NTP is not configured to start at system boot.
    # WARNING: No watchdog device found. If SBD is used, the cluster will be unable to start without a watchdog.
    # Do you want to continue anyway? [y/N] -> y
    # IP address or hostname of existing node (e.g.: 192.168.1.1) [] -> IP address of node 1 e.g. 10.0.0.5
    # /root/.ssh/id_dsa already exists - overwrite? [y/N] N
    ```

1. [A] wachtwoord van de wijzigen hacluster naar hetzelfde wachtwoord
    ```bash
    sudo passwd hacluster
    
    ```

1. [A] corosync voor het gebruik van andere transport en voeg nodelist configureren. Cluster werkt niet anders.
    ```bash
    sudo vi /etc/corosync/corosync.conf    
    
    ```

    De volgende vet inhoud toevoegen aan het bestand.
    
    <pre><code> 
    [...]
      interface { 
          [...] 
      }
      <b>transport:      udpu</b>
    } 
    <b>nodelist {
      node {
        ring0_addr:     < ip address of node 1 >
      }
      node {
        ring0_addr:     < ip address of node 2 > 
      } 
    }</b>
    logging {
      [...]
    </code></pre>

    Start de service corosync opnieuw

    ```bash
    sudo service corosync restart
    
    ```

1. [A] HANA HA-pakketten installeren  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

## <a name="installing-sap-hana"></a>SAP HANA installeren

Ga als volgt hoofdstuk 4 van de [SAP HANA SR prestaties geoptimaliseerd Scenario handleiding] [ suse-hana-ha-guide] om SAP HANA System replicatie te installeren.

1. [A] hdblcm uitvoeren vanaf de DVD HANA
    * Installatie te kiezen,-1 >
    * Selecteer extra onderdelen voor installatie-1 >
    * Voer installatiepad [/ hana/gedeelde]: ENTER ->
    * Voer de naam van de lokale Host [.]: -> ENTER
    * Wilt u extra hosts toevoegen aan het systeem? (j/n) [n]: ENTER ->
    * Voer SAP HANA systeem-ID:<SID of HANA e.g. HDB>
    * Voer exemplaarnummer [00]:   
  HANA exemplaarnummer. 03 gebruiken als u de Azure-sjabloon gebruikt of in het bovenstaande voorbeeld gevolgd
    * Selecteer Database modus / Index [1] op Enter: -> ENTER
    * Selecteer systeemgebruik / Voer Index [4]:  
  Selecteer de informatie over het gebruik van het systeem
    * Geef de locatie van gegevensvolumes [/ data/hana/HDB]: ENTER ->
    * Geef de locatie van Logboekvolumes [/ log/hana/HDB]: ENTER ->
    * Maximale geheugentoewijzing beperken? [n]: ENTER ->
    * Geef '...' Host-naam van het certificaat voor Host [...]: ENTER ->
    * Voer SAP Host Agent gebruiker (sapadm) wachtwoord:
    * SAP Host Agent gebruiker (sapadm) wachtwoord bevestigen:
    * Voer de systeembeheerder (hdbadm) wachtwoord:
    * Systeembeheerder (hdbadm) wachtwoord bevestigen:
    * Voer systeembeheerder basismap [/ usr/sap/HDB/home]: ENTER ->
    * Voer systeembeheerder aanmeldingsshell [/ bin/servicel]: ENTER ->
    * Voer systeembeheerder gebruikers-ID [1001]: -> ENTER
    * Voer-ID van gebruikersgroep (sapsys) [79]: ENTER ->
    * Geef het wachtwoord voor Database gebruiker (systeem):
    * Bevestig Database gebruikerswachtwoord (systeem):
    * Systeem opnieuw opstarten nadat de computer opnieuw is opgestart? [n]: ENTER ->
    * Wilt u doorgaan? (j/n):  
  Valideren van de samenvatting en voer j om door te gaan
1. [A] SAP Hostagent bijwerken  
  Download de meest recente Hostagent SAP-archief van de [SAP Softwarecenter] [ sap-swcenter] en voer de volgende opdracht om de agent bijwerken. Het pad naar het archief verwijzen naar de door u gedownloade bestand vervangen.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

1. [1] HANA replicatie maken (als root)  
    Voer de volgende opdracht. Zorg ervoor dat vet tekenreeksen (HANA systeem-ID HDB en exemplaar getal 03) vervangen door de waarden van de SAP HANA-installatie.
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. [A] keystore-vermelding maken (als root)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>
1. [1] back-updatabase (als root)
    <pre><code>
    PATH="$PATH:/usr/sap/<b>HDB</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')" 
    </code></pre>
1. [1] overschakelen naar de gebruiker sapsid (bijvoorbeeld hdbadm) en de primaire site maken.
    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>
1. [2] overschakelen naar de gebruiker sapsid (bijvoorbeeld hdbadm) en de secundaire site maken.
    <pre><code>
    su - <b>hdb</b>adm
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>saphanavm1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-cluster-framework"></a>Cluster-Framework configureren

De standaardinstellingen wijzigen

<pre>
sudo vi crm-defaults.txt
# enter the following to crm-defaults.txt
<code>
property $id="cib-bootstrap-options" \
  no-quorum-policy="ignore" \
  stonith-enabled="true" \
  stonith-action="reboot" \
  stonith-timeout="150s"
rsc_defaults $id="rsc-options" \
  resource-stickiness="1000" \
  migration-threshold="5000"
op_defaults $id="op-options" \
  timeout="600"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Nu we het bestand aan het cluster niet laden
sudo crm load update crm-defaults.txt configureren
</pre>

### <a name="create-stonith-device"></a>STONITH apparaat maken

Het apparaat STONITH maakt gebruik van een Service-Principal worden geautoriseerd op basis van Microsoft Azure. Volg deze stappen voor het maken van een Service-Principal.

1. Ga naar <https://portal.azure.com>
1. Open de blade van Azure Active Directory  
   Ga naar eigenschappen en noteer de id van de Directory. Dit is de **tenant-id**.
1. Klik op App-registraties
1. Klik op Add.
1. Voer een naam, selecteer toepassingstype 'Web-app /-API, een aanmeldings-URL (bijvoorbeeld http://localhost) en klik op maken
1. De aanmeldings-URL kan niet wordt gebruikt en geldige URL
1. Selecteer de nieuwe App en sleutels op in het tabblad instellingen
1. Voer een beschrijving voor een nieuwe sleutel, selecteer 'Verloopt nooit' en klik op Opslaan
1. Noteer de waarde in. Deze wordt gebruikt als de **wachtwoord** voor de Service-Principal
1. Noteer de toepassings-id. Deze wordt gebruikt als de gebruikersnaam (**aanmeldings-id** in de onderstaande stappen) van de Service-Principal

De Service-Principal heeft geen machtigingen voor toegang tot uw Azure-resources standaard. U moet de Service-Principal machtigingen geven om te starten en stoppen (ongedaan gemaakt) alle virtuele machines van het cluster.

1. Ga naar https://portal.azure.com
1. Open de blade met alle bronnen
1. Selecteer de virtuele machine
1. Klik op toegangsbeheer (IAM)
1. Klik op Add.
1. Selecteer de rol van eigenaar
1. Voer de naam van de toepassing die u hierboven hebt gemaakt
1. Klik op OK

Nadat u de machtigingen voor de virtuele machines hebt bewerkt, kunt u de apparaten STONITH configureren in het cluster.

<pre>
sudo vi crm-fencing.txt
# enter the following to crm-fencing.txt
# replace the bold string with your subscription id, resource group, tenant id, service principal id and password
<code>
primitive rsc_st_azure_1 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

primitive rsc_st_azure_2 stonith:fence_azure_arm \
    params subscriptionId="<b>subscription id</b>" resourceGroup="<b>resource group</b>" tenantId="<b>tenant id</b>" login="<b>login id</b>" passwd="<b>password</b>"

colocation col_st_azure -2000: rsc_st_azure_1:Started rsc_st_azure_2:Started
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Nu we het bestand aan het cluster niet laden
sudo crm load update crm-fencing.txt configureren
</pre>

### <a name="create-sap-hana-resources"></a>SAP HANA-resources maken

<pre>
sudo vi crm-saphanatop.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number and HANA system id
<code>
primitive rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
    operations $id="rsc_sap2_<b>HDB</b>_HDB<b>03</b>-operations" \
    op monitor interval="10" timeout="600" \
    op start interval="0" timeout="600" \
    op stop interval="0" timeout="300" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>"

clone cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Nu we het bestand aan het cluster niet laden
sudo crm load update crm-saphanatop.txt configureren
</pre>

<pre>
sudo vi crm-saphana.txt
# enter the following to crm-saphana.txt
# replace the bold string with your instance number, HANA system id and the frontend IP address of the Azure load balancer. 
<code>
primitive rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> ocf:suse:SAPHana \
    operations $id="rsc_sap_<b>HDB</b>_HDB<b>03</b>-operations" \
    op start interval="0" timeout="3600" \
    op stop interval="0" timeout="3600" \
    op promote interval="0" timeout="3600" \
    op monitor interval="60" role="Master" timeout="700" \
    op monitor interval="61" role="Slave" timeout="700" \
    params SID="<b>HDB</b>" InstanceNumber="<b>03</b>" PREFER_SITE_TAKEOVER="true" \
    DUPLICATE_PRIMARY_TIMEOUT="7200" AUTOMATED_REGISTER="false"

ms msl_SAPHana_<b>HDB</b>_HDB<b>03</b> rsc_SAPHana_<b>HDB</b>_HDB<b>03</b> \
    meta is-managed="true" notify="true" clone-max="2" clone-node-max="1" \
    target-role="Started" interleave="true"

primitive rsc_ip_<b>HDB</b>_HDB<b>03</b> ocf:heartbeat:IPaddr2 \ 
    meta target-role="Started" is-managed="true" \ 
    operations $id="rsc_ip_<b>HDB</b>_HDB<b>03</b>-operations" \ 
    op monitor interval="10s" timeout="20s" \ 
    params ip="<b>10.0.0.21</b>" 
primitive rsc_nc_<b>HDB</b>_HDB<b>03</b> anything \ 
    params binfile="/usr/bin/nc" cmdline_options="-l -k 625<b>03</b>" \ 
    op monitor timeout=20s interval=10 depth=0 
group g_ip_<b>HDB</b>_HDB<b>03</b> rsc_ip_<b>HDB</b>_HDB<b>03</b> rsc_nc_<b>HDB</b>_HDB<b>03</b>
 
colocation col_saphana_ip_<b>HDB</b>_HDB<b>03</b> 2000: g_ip_<b>HDB</b>_HDB<b>03</b>:Started \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>:Master  
order ord_SAPHana_<b>HDB</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HDB</b>_HDB<b>03</b> \ 
    msl_SAPHana_<b>HDB</b>_HDB<b>03</b>
</code>

# <a name="now-we-load-the-file-to-the-cluster"></a>Nu we het bestand aan het cluster niet laden
sudo crm load update crm-saphana.txt configureren
</pre>

### <a name="test-cluster-setup"></a>Test-cluster instellen
Het volgende hoofdstuk wordt beschreven hoe u uw instellingen kunt testen. Elke test wordt ervan uitgegaan dat u hoofdmap en de SAP HANA-master wordt uitgevoerd op de virtuele machine saphanavm1.

#### <a name="fencing-test"></a>Hekwerk Test

U kunt de installatie van de agent hekwerk testen door de netwerkinterface op knooppunt saphanavm1 uit te schakelen.

<pre><code>
sudo ifdown eth0
</code></pre>

De virtuele machine moet nu opnieuw gestart of gestopt, afhankelijk van uw clusterconfiguratie.
Als u de stonith-actie die moet worden uitgeschakeld instelt, wordt de virtuele machine wordt gestopt en de bronnen worden gemigreerd naar de actieve virtuele machine.

Zodra u de virtuele machine opnieuw start, zijn de SAP HANA-bron niet worden gestart als secundaire als u AUTOMATED_REGISTER instelt = "false". In dit geval moet u het exemplaar HANA als secundaire configureren door het uitvoeren van de volgende opdracht:

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Een handmatige failover testen

U kunt een handmatige failover testen door het stoppen van de service pacemaker heeft op het knooppunt saphanavm1.
<pre><code>
service pacemaker stop
</code></pre>

U kunt de service opnieuw starten na de failover. De SAP HANA-resource op saphanavm1 niet worden gestart als secundaire als u AUTOMATED_REGISTER instelt = "false". In dit geval moet u het exemplaar HANA als secundaire configureren door het uitvoeren van de volgende opdracht:

<pre><code>
service pacemaker start
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

#### <a name="testing-a-migration"></a>Een migratie testen

U kunt het hoofdknooppunt SAP HANA migreren door de volgende opdracht wordt uitgevoerd
<pre><code>
crm resource migrate msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
crm resource migrate g_ip_<b>HDB</b>_HDB<b>03</b> <b>saphanavm2</b>
</code></pre>

Dit moet de SAP HANA-hoofdknooppunt en de groep waartoe het virtuele IP-adres saphanavm2 migreren.
De SAP HANA-resource op saphanavm1 niet worden gestart als secundaire als u AUTOMATED_REGISTER instelt = "false". In dit geval moet u het exemplaar HANA als secundaire configureren door het uitvoeren van de volgende opdracht:

<pre><code>
su - <b>hdb</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>saphanavm2</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

De migratie maakt locatie beperkingen moeten opnieuw worden verwijderd.

<pre><code>
crm configure edited

# delete location contraints that are named like the following contraint. You should have two contraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HDB</b>_HDB<b>03</b> g_ip_<b>HDB</b>_HDB<b>03</b> role=Started inf: <b>saphanavm2</b>
</code></pre>

U moet ook opruimen van de status van de bron van het secundaire knooppunt

<pre><code>
# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HDB</b>_HDB<b>03</b> <b>saphanavm1</b>
</code></pre>

## <a name="next-steps"></a>Volgende stappen
* [Azure virtuele Machines, planning en implementatie voor SAP][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md). 
