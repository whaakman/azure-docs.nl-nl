---
title: Setup-SAP HANA System replicatie op Azure Virtual Machines (VM's) | Microsoft Docs
description: Hoge beschikbaarheid van SAP HANA op Azure virtuele Machines (VM's) maken.
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
ms.date: 03/24/2018
ms.author: sedusch
ms.openlocfilehash: e23c69b020c96dab4d119ebf4f45791e6f63c4af
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34656836"
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
[2388694]:https://launchpad.support.sap.com/#/notes/2388694

[hana-ha-guide-replication]:sap-hana-high-availability.md#14c19f65-b5aa-4856-9594-b81c7e4df73d
[hana-ha-guide-shared-storage]:sap-hana-high-availability.md#498de331-fa04-490b-997c-b078de457c9d

[suse-hana-ha-guide]:https://www.suse.com/docrep/documents/ir8w88iwu7/suse_linux_enterprise_server_for_sap_applications_12_sp1.pdf
[sap-swcenter]:https://launchpad.support.sap.com/#/softwarecenter
[template-multisid-db]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-multi-sid-db-md%2Fazuredeploy.json
[template-converged]:https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2Fsap-3-tier-marketplace-image-converged%2Fazuredeploy.json

On-premises kunt u kunt gebruiken beide HANA System Replication of gedeelde opslag gebruiken om hoge beschikbaarheid voor SAP HANA stand te brengen.
Is dat de enige ondersteunde hoge beschikbaarheid functie tot nu toe op Azure VM's HANA System Replication op Azure. SAP HANA replicatie bestaat uit één primair knooppunt en ten minste één secundaire knooppunt. Wijzigingen in de gegevens op het primaire knooppunt worden gerepliceerd naar het secundaire knooppunt synchroon of asynchroon.

In dit artikel wordt beschreven hoe de virtuele machines te implementeren, configureren van de virtuele machines, cluster-framework hebt geïnstalleerd, installeren en configureren van SAP HANA System Replication.
Installatie opdrachten enzovoort exemplaarnummer 03 en HANA systeem-ID HN1 wordt gebruikt in de voorbeeldconfiguraties.

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

## <a name="overview"></a>Overzicht

SAP HANA is om te zorgen voor hoge beschikbaarheid, geïnstalleerd op twee virtuele machines. De gegevens worden gerepliceerd met behulp van HANA System replicatie.

![Hoge beschikbaarheid voor SAP HANA-overzicht](./media/sap-hana-high-availability/ha-suse-hana.png)

De SAP HANA SR setup maakt gebruik van een speciale virtuele hostnaam en het virtuele IP-adressen. In Azure, is een load balancer vereist voor het gebruik van een virtueel IP-adres. De volgende lijst ziet u de configuratie van de load balancer.

* Frontend configuratie
  * IP-adres 10.0.0.13 voor hn1-db
* Back-endconfiguratie
  * Verbonden met de primaire netwerkinterfaces van alle virtuele machines die deel van HANA System Replication uitmaken moet
* De Testpoort
  * Poort 62503
* Loadbalancing regels
  * 30313 TCP
  * 30315 TCP
  * 30317 TCP

## <a name="deploying-linux"></a>Linux implementeren

De resource-agent voor SAP HANA is opgenomen in SUSE Linux Enterprise Server voor SAP-toepassingen.
Azure Marketplace bevat een afbeelding voor SUSE Linux Enterprise Server voor SAP-toepassingen 12 die u gebruiken kunt om nieuwe virtuele machines te implementeren.

### <a name="deploy-with-template"></a>Implementeren met sjabloon
U kunt een van de Quick Start-sjablonen op github gebruiken voor het implementeren van alle vereiste resources. De sjabloon wordt geïmplementeerd voor de virtuele machines, de load balancer, beschikbaarheidsset enzovoort. Volg deze stappen voor het implementeren van de sjabloon:

1. Open de [databasesjabloon] [ template-multisid-db] of de [geconvergeerde sjabloon] [ template-converged] in de Azure portal. 
   De databasesjabloon maakt alleen de regels voor taakverdeling voor een database terwijl de geconvergeerde sjabloon ook de regels voor taakverdeling voor een ASC's / SCS en Ebruikers (alleen voor Linux)-exemplaar maakt. Als u van plan bent een SAP NetWeaver gebaseerd systeem installeren en u ook wilt voor het exemplaar ASC's / SCS installeren op de dezelfde machines, gebruikt u de [geconvergeerde sjabloon][template-converged].
1. Voer de volgende parameters
    1. SAP-systeem-ID  
       Voer de SAP-systeem-ID van het SAP-systeem die u wilt installeren. De ID zal worden gebruikt als een voorvoegsel voor de resources die zijn geïmplementeerd.
    1. Stack-Type (alleen van toepassing als u de sjabloon geconvergeerde gebruikt)   
       Selecteer het type van de stack SAP NetWeaver
    1. Type besturingssysteem  
       Selecteer een van de Linux-distributies. Selecteer voor dit voorbeeld SLES 12
    1. DB-Type  
       Selecteer HANA
    1. Grootte van het SAP  
       De hoeveelheid SAP's het nieuwe systeem wilt geven. Als u niet zeker weet hoeveel SAP's het systeem vereist is, vraagt u uw SAP-technologie Partner of System Integrator
    1. Beschikbaarheid van het systeem  
       Selecteer HA
    1. Gebruikersnaam van de beheerder en het wachtwoord van beheerder  
       Een nieuwe gebruiker wordt gemaakt die kunnen worden gebruikt voor aanmelding bij de computer.
    1. Nieuwe of bestaande Subnet  
       Bepaalt of een nieuw virtueel netwerk en subnet moeten worden gemaakt of een bestaand subnet moet worden gebruikt. Als u al een virtueel netwerk dat is verbonden met uw on-premises netwerk hebt, selecteert u de bestaande.
    1. Subnet-ID  
    De ID van het subnet waarmee de virtuele machines moet worden verbonden. Selecteer voor de virtuele machine verbinding met uw on-premises netwerk, het subnet van het virtuele netwerk VPN of Express Route. De ID meestal ziet eruit als /subscriptions/`<subscription ID`> /resourceGroups/`<resource group name`> /providers/Microsoft.Network/virtualNetworks/`<virtual network name`> /subnets/`<subnet name`>

### <a name="manual-deployment"></a>Handmatige implementatie

1. Een resourcegroep maken
1. Een virtueel netwerk maken
1. Een Beschikbaarheidsset maken  
   De maximale updatedomein instellen
1. Maak een Load Balancer (intern)  
   Selecteer gemaakt in de tweede VNET
1. Virtuele Machine 1 maken  
   Ten minste SLES4SAP 12 SP1, in dit voorbeeld gebruiken we de installatiekopie van het SLES4SAP 12 SP2 https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM  
   SLES voor SAP-12 SP2 (Premium)  
   Selecteer Beschikbaarheidsset eerder hebt gemaakt  
1. Virtuele Machine 2 maken  
   Ten minste SLES4SAP 12 SP1, in dit voorbeeld gebruiken we de installatiekopie van het SLES4SAP 12 SP1 BYOS https://ms.portal.azure.com/#create/SUSE.SUSELinuxEnterpriseServerforSAPApplications12SP2PremiumImage-ARM  
   SLES voor SAP-12 SP2 (Premium)  
   Selecteer Beschikbaarheidsset eerder hebt gemaakt  
1. Gegevensschijven toevoegen
1. De load balancer configureren
    1. Een frontend-IP-adresgroep maken
        1. Open de load balancer, selecteer frontend-IP-adresgroep en klik op toevoegen
        1. Voer de naam van de nieuwe frontend IP-adresgroep (bijvoorbeeld hana-frontend)
        1. De toewijzing instellen op statisch en voer het IP-adres (bijvoorbeeld **10.0.0.13**)
        1. Klik op OK
        1. Nadat de nieuwe frontend-IP-adresgroep is gemaakt, noteert u het IP-adres
    1. Een back endpool maken
        1. Open de load balancer, back-endpools selecteren en klik op toevoegen
        1. Voer de naam van de nieuwe back-endpool (bijvoorbeeld hana-back-end)
        1. Klik op een virtuele machine toevoegen
        1. Selecteer de Beschikbaarheidsset u eerder hebt gemaakt
        1. Selecteer de virtuele machines van het SAP HANA-cluster
        1. Klik op OK
    1. Een statustest maken
        1. Openen van de load balancer, statuscontroles selecteren en klik op toevoegen
        1. Voer de naam van de nieuwe health test (bijvoorbeeld hana-hp)
        1. Selecteer TCP als protocol, poort 625**03**, houd Interval 5 en de drempelwaarde voor onjuiste status 2
        1. Klik op OK
    1. SAP HANA 1.0: Taakverdelingsregels maken
        1. Open de load balancer en taakverdelingsregels Selecteer klikt u op toevoegen
        1. Voer de naam van de load balancer-regel (bijvoorbeeld hana-lb-3**03**15)
        1. Selecteer de frontend-IP-adres, back-endpool en health test die u gemaakt eerder (voor voorbeeld hana-frontend)
        1. Protocol TCP houden, voert u poort 3**03**15
        1. Verhoog de time-out voor inactiviteit tot 30 minuten
        1. **Zorg ervoor dat u kunt zwevend IP inschakelen**
        1. Klik op OK
        1. Herhaal de stappen hierboven voor poort 3**03**17
    1. SAP HANA 2.0: Taakverdelingsregels voor systeemdatabase maken
        1. Open de load balancer en taakverdelingsregels Selecteer klikt u op toevoegen
        1. Voer de naam van de load balancer-regel (bijvoorbeeld hana-lb-3**03**13)
        1. Selecteer de frontend-IP-adres, back-endpool en health test die u gemaakt eerder (voor voorbeeld hana-frontend)
        1. Protocol TCP houden, voert u poort 3**03**13
        1. Verhoog de time-out voor inactiviteit tot 30 minuten
        1. **Zorg ervoor dat u kunt zwevend IP inschakelen**
        1. Klik op OK
        1. Herhaal de stappen hierboven voor poort 3**03**14
    1. SAP HANA 2.0: De tenant-database niet aanmaken load-balancingregels voor eerste
        1. Open de load balancer en taakverdelingsregels Selecteer klikt u op toevoegen
        1. Voer de naam van de load balancer-regel (bijvoorbeeld hana-lb-3**03**40)
        1. Selecteer de frontend-IP-adres, back-endpool en health test die u gemaakt eerder (voor voorbeeld hana-frontend)
        1. Protocol TCP houden, voert u poort 3**03**40
        1. Verhoog de time-out voor inactiviteit tot 30 minuten
        1. **Zorg ervoor dat u kunt zwevend IP inschakelen**
        1. Klik op OK
        1. Herhaal de stappen hierboven voor poort 3**03**41 en 3**03**42

Lees voor meer informatie over de vereiste poorten voor SAP HANA hoofdstuk [verbindingen met Tenant-Databases](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6/latest/en-US/7a9343c9f2a2436faa3cfdb5ca00c052.html) van de [SAP HANA Tenant Databases](https://help.sap.com/viewer/78209c1d3a9b41cd8624338e42a12bf6) handleiding of [SAP-notitie 2388694] [2388694].


## <a name="create-pacemaker-cluster"></a>Pacemaker heeft cluster maken

Volg de stappen in [pacemaker heeft op SUSE Linux Enterprise Server in Azure instellen](high-availability-guide-suse-pacemaker.md) een basic pacemaker heeft cluster voor deze server HANA te maken. U kunt hetzelfde cluster pacemaker heeft ook gebruiken voor SAP HANA en SAP NetWeaver (A) SCS.

## <a name="installing-sap-hana"></a>SAP HANA installeren

De volgende items worden voorafgegaan door een **[A]** - van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2 van het cluster pacemaker heeft.

1. **[A]**  Indeling van setup-schijf
    1. LVM  
       
       In het algemeen wordt aangeraden met LVM voor volumes die bij het opslaan van gegevens en logboekbestanden. Het volgende voorbeeld wordt ervan uitgegaan dat de virtuele machines vier gegevensschijven gekoppeld die moeten worden gebruikt voor het maken van twee volumes hebben.

       Lijst van alle beschikbare schijven
       
       <pre><code>
       ls /dev/disk/azure/scsi1/lun*
       </code></pre>
       
       Voorbeeld van uitvoer
       
       ```
       /dev/disk/azure/scsi1/lun0  /dev/disk/azure/scsi1/lun1  /dev/disk/azure/scsi1/lun2  /dev/disk/azure/scsi1/lun3
       ```
       
       Maak fysieke volumes voor alle schijven die u wilt gebruiken.    
       <pre><code>
       sudo pvcreate /dev/disk/azure/scsi1/lun0
       sudo pvcreate /dev/disk/azure/scsi1/lun1
       sudo pvcreate /dev/disk/azure/scsi1/lun2
       sudo pvcreate /dev/disk/azure/scsi1/lun3
       </code></pre>

       Maak een groep volume voor de gegevensbestanden, één volume groep voor de logboekbestanden en één voor de gedeelde map van SAP HANA

       <pre><code>
       sudo vgcreate vg_hana_data_<b>HN1</b> /dev/disk/azure/scsi1/lun0 /dev/disk/azure/scsi1/lun1
       sudo vgcreate vg_hana_log_<b>HN1</b> /dev/disk/azure/scsi1/lun2
       sudo vgcreate vg_hana_shared_<b>HN1</b> /dev/disk/azure/scsi1/lun3
       </code></pre>
       
        De logische volumes maken. Lineaire volume wordt gemaakt wanneer u lvcreate zonder -i overschakelen. We raden aan om striped volumes maken voor betere i/o-prestaties, de i - argument moet hetzelfde zijn als het aantal onderliggende fysieke volume. In dit document 2 fysieke volumes worden gebruikt voor gegevensvolume, zodat de -i overschakelen argument 2. 1 fysieke volume wordt gebruikt voor logboekvolume dus geen i - switch expliciet wordt gebruikt. Gebruik -i switch en het nummer op onderliggende fysieke volume aantal vervangen wanneer u meer dan 1 fysieke volume voor elke gegevens, logboek of gedeelde volumes gebruikt.

       <pre><code>
       sudo lvcreate <b>-i 2</b> -l 100%FREE -n hana_data vg_hana_data_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_log vg_hana_log_<b>HN1</b>
       sudo lvcreate -l 100%FREE -n hana_shared vg_hana_shared_<b>HN1</b>
       sudo mkfs.xfs /dev/vg_hana_data_<b>HN1</b>/hana_data
       sudo mkfs.xfs /dev/vg_hana_log_<b>HN1</b>/hana_log
       sudo mkfs.xfs /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       </code></pre>
       
       Maak de mappen koppelen en kopieer de UUID van alle logische volumes
       
       <pre><code>
       sudo mkdir -p /hana/data/<b>HN1</b>
       sudo mkdir -p /hana/log/<b>HN1</b>
       sudo mkdir -p /hana/shared/<b>HN1</b>
       # write down the ID of /dev/vg_hana_data_<b>HN1</b>/hana_data, /dev/vg_hana_log_<b>HN1</b>/hana_log and /dev/vg_hana_shared_<b>HN1</b>/hana_shared
       sudo blkid
       </code></pre>
       
       Fstab-vermeldingen voor de drie logische volumes maken
       
       <pre><code>
       sudo vi /etc/fstab
       </code></pre>
       
       Deze regel moet worden /etc/fstab invoegen
       
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_data_<b>HN1</b>-hana_data&gt;</b> /hana/data/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_log_<b>HN1</b>-hana_log&gt;</b> /hana/log/<b>HN1</b> xfs  defaults,nofail  0  2
       /dev/disk/by-uuid/<b>&lt;UUID of /dev/mapper/vg_hana_shared_<b>HN1</b>-hana_shared&gt;</b> /hana/shared/<b>HN1</b> xfs  defaults,nofail  0  2
       </code></pre>
       
       Koppel de nieuwe volumes
       
       <pre><code>
       sudo mount -a
       </code></pre>
    
    1. Gewone schijven  
       Voor demo-systemen, kunt u uw bestanden HANA gegevens en logboekbestanden op één schijf plaatsen. De volgende opdrachten een partitie maken op /dev/disk/azure/scsi1/lun0 en formatteert u het met xfs.

       <pre><code>
       sudo sh -c 'echo -e "n\n\n\n\n\nw\n" | fdisk /dev/disk/azure/scsi1/lun0'
       sudo mkfs.xfs /dev/disk/azure/scsi1/lun0-part1
       
       # write down the ID of /dev/disk/azure/scsi1/lun0-part1
       sudo /sbin/blkid
       sudo vi /etc/fstab
       </code></pre>

       Deze regel moet worden /etc/fstab invoegen
       <pre><code>
       /dev/disk/by-uuid/<b>&lt;UUID&gt;</b> /hana xfs  defaults,nofail  0  2
       </code></pre>

       Maken van de doelmap en de schijf koppelen.

       <pre><code>
       sudo mkdir /hana
       sudo mount -a
       </code></pre>

1. **[A]**  Hostnamen instellen voor alle hosts  
    U kunt gebruik van een DNS-server of wijzig de/etc/hosts op alle knooppunten. In dit voorbeeld laat zien hoe het bestand/etc/hosts te gebruiken.
   Het IP-adres en de hostnaam in de volgende opdrachten vervangen
    ```bash
    sudo vi /etc/hosts
    ```
    Voeg de volgende regels/etc/hosts. Wijzig de IP-adres en hostnaam overeenkomen met uw omgeving    
    
   <pre><code>
   <b>10.0.0.5 hn1-db-0</b>
   <b>10.0.0.6 hn1-db-1</b>
   </code></pre>

1. **[A]**  Installeren HANA HA pakketten  
    ```bash
    sudo zypper install SAPHanaSR
    
    ```

Volg voor het installeren van SAP HANA System Replication hoofdstuk 4 van de handleiding voor SAP HANA SR prestaties geoptimaliseerd Scenario op https://www.suse.com/products/sles-for-sap/resource-library/sap-best-practices/

1. **[A]**  Hdblcm uitvoeren vanaf de DVD HANA
    * Installatie te kiezen,-1 >
    * Selecteer extra onderdelen voor installatie-1 >
    * Voer installatiepad [/ hana/gedeelde]: ENTER ->
    * Voer de naam van de lokale Host [.]: -> ENTER
    * Wilt u extra hosts toevoegen aan het systeem? (j/n) [n]: ENTER ->
    * Voer SAP HANA systeem-ID: <SID of HANA e.g. HN1>
    * Voer exemplaarnummer [00]:   
  HANA exemplaarnummer. 03 gebruiken als u de Azure-sjabloon gebruikt of de handmatige implementatie gevolgd
    * Selecteer Database modus / Index [1] op Enter: -> ENTER
    * Selecteer systeemgebruik / Voer Index [4]:  
  Selecteer de informatie over het gebruik van het systeem
    * Geef de locatie van gegevensvolumes [/ data/hana/HN1]: ENTER ->
    * Geef de locatie van Logboekvolumes [/ log/hana/HN1]: ENTER ->
    * Maximale geheugentoewijzing beperken? [n]: ENTER ->
    * Geef '...' Host-naam van het certificaat voor Host [...]: ENTER ->
    * Voer SAP Host Agent gebruiker (sapadm) wachtwoord:
    * SAP Host Agent gebruiker (sapadm) wachtwoord bevestigen:
    * Voer de systeembeheerder (hdbadm) wachtwoord:
    * Systeembeheerder (hdbadm) wachtwoord bevestigen:
    * Voer systeembeheerder basismap [/ usr/sap/HN1/home]: ENTER ->
    * Voer systeembeheerder aanmeldingsshell [/ bin/servicel]: ENTER ->
    * Voer systeembeheerder gebruikers-ID [1001]: -> ENTER
    * Voer-ID van gebruikersgroep (sapsys) [79]: ENTER ->
    * Geef het wachtwoord voor Database gebruiker (systeem):
    * Bevestig Database gebruikerswachtwoord (systeem):
    * Systeem opnieuw opstarten nadat de computer opnieuw is opgestart? [n]: ENTER ->
    * Wilt u doorgaan? (j/n):   
  Valideren van de samenvatting en voer j om door te gaan

1. **[A]**  SAP Hostagent bijwerken  
  Download de meest recente Hostagent SAP-archief van de [SAP Softwarecenter] [ sap-swcenter] en voer de volgende opdracht om de agent bijwerken. Het pad naar het archief verwijzen naar de door u gedownloade bestand vervangen.
    ```bash
    sudo /usr/sap/hostctrl/exe/saphostexec -upgrade -archive <path to SAP Host Agent SAR>
    ```

## <a name="configure-sap-hana-20-system-replication"></a>SAP HANA 2.0 System replicatie configureren

De volgende items worden voorafgegaan door een **[A]** - van toepassing op alle knooppunten **[1]** - alleen van toepassing op knooppunt 1 of **[2]** - alleen van toepassing op knooppunt 2 van het cluster pacemaker heeft.

1. **[1]**  Tenant-Database maken

   Als u een SAP HANA 2.0 of MDC gebruikt, moet u een tenant-database voor uw systeem SAP NetWeaver maken. NW1 vervangen door de SID van uw SAP-systeem.

   Meld u aan als `<hanasid`> adm en voer de volgende opdracht uit

   <pre><code>
   hdbsql -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> -d SYSTEMDB 'CREATE DATABASE <b>NW1</b> SYSTEM USER PASSWORD "<b>passwd</b>"'
   </code></pre>

1. **[1]**  System-replicatie configureren op het eerste knooppunt
   
   Meld u aan als `<hanasid`> adm en back-up van de databases

   <pre><code>
   hdbsql -d SYSTEMDB -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupSYS</b>')"
   hdbsql -d <b>HN1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupHN1</b>')"
   hdbsql -d <b>NW1</b> -u SYSTEM -p "<b>passwd</b>" -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackupNW1</b>')"
   </code></pre>

   Kopieer de bestanden van de PKI systeem naar secundair

   <pre><code>
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/SSFS_<b>HN1</b>.DAT <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/data/
   scp /usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/SSFS_<b>HN1</b>.KEY <b>hn1-db-1</b>:/usr/sap/<b>HN1</b>/SYS/global/security/rsecssfs/key/
   </code></pre>

   De primaire site gemaakt.

   <pre><code>
   hdbnsutil -sr_enable –-name=<b>SITE1</b>
   </code></pre>

1. **[2]**  System-replicatie configureren op het tweede knooppunt
    
    Registreer het tweede knooppunt voor het starten van de replicatie van het systeem. Meld u aan als `<hanasid`> adm en voer de volgende opdracht

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="configure-sap-hana-10-system-replication"></a>SAP HANA 1.0 System replicatie configureren

1. **[1]**  De vereiste gebruikers maken

    Meld u aan als hoofdmap en voer de volgende opdracht. Zorg ervoor dat vet tekenreeksen (HANA systeem-ID HN1 en exemplaar getal 03) vervangen door de waarden van de SAP HANA-installatie.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbsql -u system -i <b>03</b> 'CREATE USER <b>hdb</b>hasync PASSWORD "<b>passwd</b>"' 
    hdbsql -u system -i <b>03</b> 'GRANT DATA ADMIN TO <b>hdb</b>hasync' 
    hdbsql -u system -i <b>03</b> 'ALTER USER <b>hdb</b>hasync DISABLE PASSWORD LIFETIME' 
    </code></pre>

1. **[A]**  Keystore-vermelding maken
   
    Meld u aan als hoofdmap en voer de volgende opdracht voor het maken van een nieuwe vermelding van de sleutelopslag.

    <pre><code>
    PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
    hdbuserstore SET <b>hdb</b>haloc localhost:3<b>03</b>15 <b>hdb</b>hasync <b>passwd</b>
    </code></pre>

1. **[1]**  Back-up van database

   Aanmelden als basis- en back-up van de databases

   <pre><code>
   PATH="$PATH:/usr/sap/<b>HN1</b>/HDB<b>03</b>/exe"
   hdbsql -d SYSTEMDB -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

   Als u een multitenant-installatie gebruikt, ook back-up van de tenant-database

   <pre><code>   
   hdbsql -d <b>HN1</b> -u system -i <b>03</b> "BACKUP DATA USING FILE ('<b>initialbackup</b>')"
   </code></pre>

1. **[1]**  System-replicatie configureren op het eerste knooppunt
    
    Meld u aan als `<hanasid`> adm en de primaire site te maken.

    <pre><code>
    su - <b>hdb</b>adm
    hdbnsutil -sr_enable –-name=<b>SITE1</b>
    </code></pre>

1. **[2]**  System-replicatie configureren op het secundaire knooppunt.

    Meld u aan als `<hanasid`> adm en de secundaire site te registreren.

    <pre><code>
    sapcontrol -nr <b>03</b> -function StopWait 600 10
    hdbnsutil -sr_register --remoteHost=<b>hn1-db-0</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE2</b> 
    </code></pre>

## <a name="create-sap-hana-cluster-resources"></a>Resources voor SAP HANA-cluster maken

   Maak eerst de topologie HANA. Voer de volgende opdrachten op een van de clusterknooppunten pacemaker heeft.
   
   <pre><code>
   sudo crm configure property maintenance-mode=true

   # replace the bold string with your instance number and HANA system ID
   
   sudo crm configure primitive rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> ocf:suse:SAPHanaTopology \
     operations \$id="rsc_sap2_<b>HN1</b>_HDB<b>03</b>-operations" \
     op monitor interval="10" timeout="600" \
     op start interval="0" timeout="600" \
     op stop interval="0" timeout="300" \
     params SID="<b>HN1</b>" InstanceNumber="<b>03</b>"
   
   sudo crm configure clone cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> rsc_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     meta is-managed="true" clone-node-max="1" target-role="Started" interleave="true"
   </code></pre>
   
   Maak vervolgens de bronnen HANA.
   
   <pre><code>
   # replace the bold string with your instance number, HANA system ID and the frontend IP address of the Azure load balancer. 
      
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
   
   sudo crm configure order ord_SAPHana_<b>HN1</b>_HDB<b>03</b> 2000: cln_SAPHanaTopology_<b>HN1</b>_HDB<b>03</b> \
     msl_SAPHana_<b>HN1</b>_HDB<b>03</b>
   
   # Cleanup the HANA resources. The HANA resources might have failed because of a known issue.
   sudo crm resource cleanup rsc_SAPHana_<b>HN1</b>_HDB<b>03</b>

   sudo crm configure property maintenance-mode=false
   </code></pre>

   Zorg ervoor dat de clusterstatus ok is en dat alle resources worden gestart. Het is niet belangrijk op welk knooppunt de bronnen worden uitgevoerd.

   <pre><code>
   sudo crm_mon -r
   
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

### <a name="test-cluster-setup"></a>Test-cluster instellen
In dit hoofdstuk beschrijft hoe u uw instellingen kunt testen. Elke test wordt ervan uitgegaan dat u hoofdmap en de SAP HANA-master wordt uitgevoerd op de virtuele machine hn1-db-0.

#### <a name="fencing-test"></a>Hekwerk Test

U kunt de installatie van de agent hekwerk testen door het uitschakelen van de netwerkinterface op knooppunt hn1-db-0.

<pre><code>
sudo ifdown eth0
</code></pre>

De virtuele machine moet nu opnieuw gestart of gestopt, afhankelijk van uw clusterconfiguratie.
Als u de stonith-actie op off instellen, gaat de virtuele machine moet worden gestopt en de bronnen worden gemigreerd naar de actieve virtuele machine.

Zodra u de virtuele machine opnieuw start, de SAP HANA-resource niet kan worden gestart als secundaire als u AUTOMATED_REGISTER instelt = "false". In dit geval wordt het exemplaar HANA als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b>

# switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-manual-failover"></a>Een handmatige failover testen

U kunt een handmatige failover testen door het stoppen van de service pacemaker heeft op het knooppunt hn1-db-0.
<pre><code>
service pacemaker stop
</code></pre>

U kunt de service opnieuw starten na de failover. Als u AUTOMATED_REGISTER instelt = 'false', de SAP HANA-resource op hn1-db-0 niet wordt gestart als secundaire. In dit geval wordt het exemplaar HANA als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>
service pacemaker start
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 


# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

#### <a name="testing-a-migration"></a>Een migratie testen

U kunt het hoofdknooppunt SAP HANA migreren door de volgende opdracht wordt uitgevoerd
<pre><code>
crm resource migrate msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
crm resource migrate g_ip_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-1</b>
</code></pre>

Als u AUTOMATED_REGISTER instelt = "false", deze reeks opdrachten moet migreren de SAP HANA-hoofdknooppunt en de groep waartoe het virtuele IP-adres hn1-db-1.
De SAP HANA-resource op hn1-db-0 niet gestart als secundaire. In dit geval wordt het exemplaar HANA als secundaire configureren door het uitvoeren van deze opdracht:

<pre><code>
su - <b>hn1</b>adm

# Stop the HANA instance just in case it is running
sapcontrol -nr <b>03</b> -function StopWait 600 10
hdbnsutil -sr_register --remoteHost=<b>hn1-db-1</b> --remoteInstance=<b>03</b> --replicationMode=sync --name=<b>SITE1</b> 
</code></pre>

De migratie maakt Locatiebeperkingen moeten opnieuw worden verwijderd.

<pre><code>
crm configure edited

# Delete location constraints that are named like the following contraint. You should have two constraints, one for the SAP HANA resource and one for the IP address group.
location cli-prefer-g_ip_<b>HN1</b>_HDB<b>03</b> g_ip_<b>HN1</b>_HDB<b>03</b> role=Started inf: <b>hn1-db-1</b>
</code></pre>

U moet ook de status van de bron van het secundaire knooppunt opschonen

<pre><code>
# Switch back to root and cleanup the failed state
exit
crm resource cleanup msl_SAPHana_<b>HN1</b>_HDB<b>03</b> <b>hn1-db-0</b>
</code></pre>

## <a name="next-steps"></a>Volgende stappen
* [Azure virtuele Machines, planning en implementatie voor SAP][planning-guide]
* [Azure virtuele Machines-implementatie voor SAP][deployment-guide]
* [Azure virtuele Machines DBMS-implementatie voor SAP][dbms-guide]
* Zie voor meer informatie over hoe u hoge beschikbaarheid en herstel na noodgevallen van SAP HANA plannen in Azure (grote exemplaren), [SAP HANA (grote exemplaren) hoge beschikbaarheid en herstel na noodgevallen op Azure](hana-overview-high-availability-disaster-recovery.md). 
