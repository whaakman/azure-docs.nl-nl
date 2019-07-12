---
title: IBM Db2 HADR instellen op Azure virtual machines (VM's) op RHEL | Microsoft Docs
description: Hoge beschikbaarheid van IBM Db2 LUW op Azure virtual machines (VM's) RHEL vast.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/10/2019
ms.author: juergent
ms.openlocfilehash: b26a66eaee3a107c37d64541ec6b832331a3e2c9
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67812127"
---
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[2002167]: https://launchpad.support.sap.com/#/notes/2002167
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[2233094]: https://launchpad.support.sap.com/#/notes/2233094
[1612105]: https://launchpad.support.sap.com/#/notes/1612105
[2694118]: https://launchpad.support.sap.com/#/notes/2694118


[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm

[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[rhel-ha-addon]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_overview/index
[rhel-ha-admin]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_administration/index
[rhel-ha-ref]:https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/high_availability_add-on_reference/index
[rhel-azr-supp]:https://access.redhat.com/articles/3131341
[rhel-azr-inst]:https://access.redhat.com/articles/3252491
[rhel-db2-supp]:https://access.redhat.com/articles/3144221
[ascs-ha-rhel]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel
[glusterfs]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs
[rhel-pcs-azr]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker
[anf-rhel]:(https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-red-hat-enterprise-linux-server"></a>Hoge beschikbaarheid van IBM Db2 LUW op Azure Virtual machines van Red Hat Enterprise Linux Server

IBM Db2 voor Linux, UNIX- en Windows (LUW) in [hoge beschikbaarheid en noodherstel (HADR) herstelconfiguratie](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) bestaat uit één knooppunt waarop een exemplaar van de primaire database en ten minste één knooppunt dat een secundaire database-exemplaar wordt uitgevoerd. Wijzigingen in de primaire database-instantie worden gerepliceerd naar een secundaire database-exemplaar synchroon of asynchroon, afhankelijk van uw configuratie. 

Dit artikel wordt beschreven hoe u kunt implementeren en configureren van de Azure virtual machines (VM's), installeer het framework van het cluster en de IBM Db2 LUW installeren met HADR configuratie. 

Het artikel niet uitgelegd hoe u installeren en configureren van IBM Db2 LUW met HADR of SAP software-installatie. Voor hulp bij het uitvoeren van deze taken, bieden we verwijzingen naar handleidingen van SAP en IBM-installatie. In dit artikel richt zich op de gedeelten die specifiek voor de Azure-omgeving zijn. 

De ondersteunde IBM Db2-versies zijn 10,5 en hoger, zoals beschreven in de SAP-notitie [1928533].

Voordat u een installatie, Zie de volgende SAP-opmerkingen en documentatie:

| SAP-notitie | Description |
| --- | --- |
| [1928533] | SAP-toepassingen op Azure: Ondersteunde producten en typen Azure VM 's |
| [2015553] | SAP op Azure: Vereisten voor ondersteuning |
| [2178632] | Sleutel metrische gegevens controleren voor SAP op Azure |
| [2191498] | SAP op Linux met Azure: Uitgebreide bewaking |
| [2243692] | Linux op Azure (IaaS) virtuele machine: Problemen met SAP-licentie |
| [2002167] | Red Hat Enterprise Linux 7.x: Installatie en Upgrade |
| [2694118] | Red Hat Enterprise Linux HA-invoegtoepassing op Azure |
| [1999351] | Het oplossen van uitgebreide Azure bewaking voor SAP |
| [2233094] | DB6: SAP-toepassingen op Azure met IBM Db2 voor Linux, UNIX- en Windows - als u meer informatie |
| [1612105] | DB6: Veelgestelde vragen over Db2 met HADR |


| Documentatie | 
| --- |
| [SAP-Community Wiki](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes): Beschikt over alle van de vereiste SAP-opmerkingen voor Linux |
| [Azure virtuele Machines, planning en implementatie van SAP op Linux][planning-guide] handleiding |
| [Azure Virtual Machines-implementatie voor SAP op Linux][deployment-guide] (in dit artikel) |
| [Virtuele Machines van Azure-database management system(DBMS)-implementatie voor SAP op Linux][dbms-guide] handleiding |
| [SAP-werkbelasting op Azure controlelijst voor planning en implementatie][azr-sap-plancheck] |
| [Overzicht van de invoegtoepassing hoge beschikbaarheid voor Red Hat Enterprise Linux 7][rhel-ha-addon] |
| [Beheer van de invoegtoepassing voor hoge beschikbaarheid][rhel-ha-admin] |
| [Naslaginformatie over hoge beschikbaarheid-invoegtoepassing][rhel-ha-ref] |
| [Ondersteuningsbeleid voor RHEL Clusters met hoge beschikbaarheid - Microsoft Azure Virtual Machines als leden van het Cluster][rhel-azr-supp]
| [Installeren en configureren van een Red Hat Enterprise Linux 7.4 (en hoger) Cluster met hoge beschikbaarheid voor Microsoft Azure][rhel-azr-inst]
| [IBM Db2 Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP][dbms-db2] |
| [IBM Db2 HADR 11.1][db2-hadr-11.1] |
| [IBM Db2 HADR 10.5][db2-hadr-10.5] |
| [Ondersteuningsbeleid voor Clusters met hoge beschikbaarheid RHEL - beheer van IBM Db2 voor Linux, Unix- en Windows in een Cluster][rhel-db2-supp]



## <a name="overview"></a>Overzicht
Voor het bereiken van hoge beschikbaarheid, IBM Db2 LUW met HADR is geïnstalleerd op ten minste twee virtuele machines van Azure, die zijn geïmplementeerd in een [Azure-beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) of over meerdere [Azure Availability Zones](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). 

De volgende afbeeldingen weer een installatie van twee virtuele machines van Azure-databaseserver. Zowel Azure-VM's van de database-server hebben hun eigen opslag die is gekoppeld en actief zijn. In HADR heeft één database-instantie in een van de Azure VM's de rol van het primaire exemplaar. Alle clients zijn verbonden met het primaire exemplaar. Alle wijzigingen in de databasetransacties worden lokaal opgeslagen in het transactielogboek Db2. Als de transactie logboekrecords lokaal opgeslagen worden, worden de records overgebracht via TCP/IP op de database-instantie op de tweede database-server, de stand-by-server of de stand-by-exemplaar. De standby-instantie werkt de lokale database door te draaien naar voren de overgebrachte transactie records in logboek registreren. Op deze manier wordt de stand-by-server bewaard gesynchroniseerd met de primaire server.

HADR is alleen een replicatiefunctionaliteit. Er is geen foutdetectie en geen automatische failover voor een overname of faciliteiten. Een overname of overdracht naar de stand-by-server moet handmatig worden gestart door een databasebeheerder. Als u wilt een automatische overnemen en foutdetectie bereiken, kunt u de clusteringfunctie Linux Pacemaker. Pacemaker bewaakt de twee database server-exemplaren. Wanneer de primaire database server-exemplaar vastloopt, Pacemaker initieert een *automatische* HADR overname door de stand-by-server. Pacemaker zorgt er ook voor dat het virtuele IP-adres is toegewezen aan de nieuwe primaire server.

![Overzicht van IBM Db2 hoge beschikbaarheid](./media/high-availability-guide-rhel-ibm-db2-luw/ha-db2-hadr-lb-rhel.png)

Om SAP-toepassingsservers verbinding maken met de primaire database, moet u de naam van een virtuele host en een virtueel IP-adres. De SAP-toepassingsservers wordt in het geval van een failover verbinding maken met de nieuwe primaire database-instantie. In een Azure-omgeving, een [Azure load balancer](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) is vereist voor het gebruik van een virtueel IP-adres in de manier die voor HADR van IBM Db2 zijn vereist. 

De volgende afbeelding geeft, kunt u beter begrijpen hoe een maximaal beschikbare installatie van de SAP-systeem IBM Db2 LUW met HADR en Pacemaker past een overzicht van een maximaal beschikbare installatie van een SAP-systeem op basis van IBM Db2-database. Dit artikel behandelt alleen IBM Db2, maar deze bevat verwijzingen naar andere artikelen over het instellen van andere onderdelen van een SAP-systeem.

![Overzicht van de volledige omgeving IBM DB2 hoge beschikbaarheid](./media/high-availability-guide-rhel-ibm-db2-luw/end-2-end-ha-rhel.png)


### <a name="high-level-overview-of-the-required-steps"></a>Overzicht van de vereiste stappen op hoog niveau
Voor het implementeren van een IBM Db2-configuratie, moet u als volgt te werk:

  + Uw omgeving plannen.
  + Implementeer de virtuele machines.
  + Met deze eigenschap bestandssystemen, configureren en bijwerken van RHEL-Linux.
  + Installeer en configureer Pacemaker.
  + Setup [glusterfs cluster][glusterfs] or [Azure NetApp Files][anf-rhel]
  + Installeer [ASCS/gebruikers op een ander cluster][ascs-ha-rhel].
  + IBM Db2-database met de optie voor gedistribueerde/hoge beschikbaarheid (SWPM) installeren.
  + Installeren en een secundaire database-knooppunt en een exemplaar maken en configureren van HADR.
  + Controleer of de HADR werkt.
  + De configuratie Pacemaker toepassen om te bepalen, IBM Db2.
  + Azure Load Balancer configureren.
  + Primaire installeren en het dialoogvenster toepassingsservers.
  + Controleer en aanpassen van de configuratie van SAP-toepassingsservers.
  + Failover en overname tests uitvoeren.



## <a name="plan-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>Azure-infrastructuur voor het hosten van IBM Db2 LUW met HADR plannen

Het planningsproces voltooien voordat u de implementatie uitvoert. Planning bouwt de fundering voor het implementeren van een configuratie van Db2 met HADR in Azure. Belangrijkste elementen die deel uitmaken van de planning voor IMB Db2 LUW (database onderdeel van de SAP-omgeving) moeten worden weergegeven in de volgende tabel:

| Onderwerp | Korte beschrijving |
| --- | --- |
| Azure-resourcegroepen definiëren | De resourcegroepen waar u de virtuele machine, VNet, Azure Load Balancer en andere resources implementeren. Kan worden bestaande of nieuwe. |
| Virtueel netwerk / subnetdefinitie | Waar zijn virtuele machines voor IBM Db2 en Azure Load Balancer wordt geïmplementeerd. Kan worden bestaande of nieuwe. |
| Virtuele machines die als host fungeert voor IBM Db2 LUW | VM-grootte, opslag, netwerken, IP-adres. |
| Naam van virtuele host en virtuele IP-adres voor IBM Db2-database| De virtuele IP-adres of de host naam die wordt gebruikt voor verbinding met SAP-toepassingsservers. **db-virt-hostname**, **db-virt-ip**. |
| De eerste optie Azure | Methode om te voorkomen dat splitsen brein situaties wordt voorkomen. |
| Azure Load Balancer | Verbruik van Basic of Standard (aanbevolen), testpoort voor Db2-database (onze aanbeveling 62500) **testpoort**. |
| Naamomzetting| Hoe de naamomzetting werkt in de omgeving. DNS-service wordt sterk aanbevolen. Lokale hosts-bestand kan worden gebruikt. |
    
Zie voor meer informatie over Linux Pacemaker in Azure, [Pacemaker op Red Hat Enterprise Linux in Azure instellen][rhel-pcs-azr].

## <a name="deployment-on-red-hat-enterprise-linux"></a>Implementatie van Red Hat Enterprise Linux

De resource-agent voor IBM Db2 LUW is opgenomen in Red Hat Enterprise Linux Server HA Addon. Voor de instelling die wordt beschreven in dit document, moet u Red Hat Enterprise Linux voor SAP. De Azure Marketplace bevat een afbeelding voor Red Hat Enterprise Linux 7.4 voor SAP of hoger die u gebruiken kunt voor het implementeren van nieuwe virtuele machines van Azure. Houd rekening met de verschillende support of service-modellen die worden aangeboden door Red Hat via Azure Marketplace wanneer u een VM-installatiekopie bij de Azure VM Marketplace selecteert.

### <a name="hosts-dns-updates"></a>Hosts: DNS-updates
Maak een lijst van alle hostnamen, waaronder de namen van de virtuele host, en werk uw DNS-servers, zodat de juiste IP-adres op de host-naamomzetting. Als een DNS-server bestaat niet of u kunt geen bijwerken en DNS-vermeldingen te maken, moet u de lokale host-bestanden van de afzonderlijke virtuele machines die deelnemen aan in dit scenario gebruiken. Als u bestanden hostvermeldingen, zorg ervoor dat de vermeldingen worden toegepast op alle virtuele machines in de systeemomgeving SAP. We raden u echter aan dat u uw DNS waarmee, in het ideale geval wordt uitgebreid naar Azure


### <a name="manual-deployment"></a>Handmatige implementatie

Zorg ervoor dat het geselecteerde besturingssysteem wordt ondersteund door IBM/SAP voor IBM Db2 LUW. De lijst met ondersteunde versies van het besturingssysteem voor virtuele Azure-machines en Db2 releases is beschikbaar in de SAP-notitie [1928533]. De lijst met OS releases door afzonderlijke Db2 versie is beschikbaar in de Matrix beschikbaarheid van SAP-Product. We raden een minimum van Red Hat Enterprise Linux 7.4 voor SAP door met betrekking tot Azure-prestatieverbeteringen in deze of hoger Red Hat Enterprise Linux-versies.

1. Maak of Selecteer een resourcegroep.
1. Maak of Selecteer een virtueel netwerk en subnet.
1. Een Azure-beschikbaarheidsset maken of implementeren van een beschikbaarheidszone.
    + Stel het maximum aantal updatedomeinen op 2 voor de beschikbaarheidsset.
1. Virtuele Machine 1 maken.
    + Gebruik Red Hat Enterprise Linux voor SAP-installatiekopie in de Azure Marketplace.
    + Selecteer het Azure-beschikbaarheidsset dat u in stap 3 hebt gemaakt, of binnen een Beschikbaarheidszone.
1.  Maak virtuele Machine 2.
    + Gebruik Red Hat Enterprise Linux voor SAP-installatiekopie in de Azure Marketplace.
    + Selecteer het Azure-beschikbaarheidsset u in stap 3 hebt gemaakt, of selecteer binnen een Beschikbaarheidszone (niet de dezelfde zone zoals in stap 3).
1. Gegevensschijven toevoegen aan de virtuele machines en controleer vervolgens de aanbeveling van de installatie van een bestand system in het artikel [IBM Db2 Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP][dbms-db2].

## <a name="create-the-pacemaker-cluster"></a>Maken van het cluster Pacemaker
    
Zie voor het maken van een eenvoudige Pacemaker-cluster voor deze server IBM Db2, [Pacemaker op Red Hat Enterprise Linux in Azure instellen][rhel-pcs-azr]. 

## <a name="install-the-ibm-db2-luw-and-sap-environment"></a>Installeer de LUW van IBM Db2 en SAP-omgeving

Voordat u begint met de installatie van een SAP-omgeving op basis van IBM Db2 LUW, controleert u de volgende documentatie:

+ Documentatie voor Azure
+ SAP-documentatie
+ IBM-documentatie

In de inleidende sectie van dit artikel vindt u koppelingen naar deze documentatie.

Controleer de SAP-installatie handleidingen over het installeren van de NetWeaver-toepassingen op IBM Db2 LUW.
U vindt de handleidingen in de Help van SAP-portal met behulp van de [SAP-installatie handleiding Finder][sap-instfind].

U kunt het aantal gidsen weergegeven in de portal met de volgende filters reduceren:
- Ik wil: "Een nieuw systeem installeren"
- Mijn Database: "IBM Db2 voor Linux, Unix- en Windows"
- Meer filters voor SAP NetWeaver-versies, -stack-configuratie of -besturingssysteem

#### <a name="red-hat-firewall-rules"></a>Red Hat firewall-regels
Red Hat Enterprise Linux heeft firewall standaard ingeschakeld. 

<pre><code>#Allow access to SWPM tool. Rule is not permanent.
sudo firewall-cmd --add-port=4237/tcp</code></pre>

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Tips voor het instellen van IBM Db2 LUW met HADR installatie

De primaire LUW van IBM Db2-database-instantie instellen:

- Gebruik de hoge beschikbaarheid of gedistribueerde optie.
- Installeer de SAP ASCS/INGEN en Database-exemplaar.
- Maak een back-up van de geïnstalleerde database.

> [!IMPORTANT] 
> Noteer de 'Database communicatiepoort' die ingesteld tijdens de installatie. Het moet hetzelfde poortnummer voor beide exemplaren van de database.
>![De definitie van de SAP SWPM poort](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-swpm-db2-port.png)

### <a name="ibm-db2-hadr-settings-for-azure"></a>IBM Db2 HADR-instellingen voor Azure

   Wanneer u een agent van de eerste optie Azure Pacemaker gebruikt, moet u de volgende parameters instellen:

   - Duur van het venster HADR peer (seconden) (HADR_PEER_WINDOW) = 240  
   - HADR time-outwaarde (HADR_TIMEOUT) = 45

U wordt aangeraden de bovenstaande parameters op basis van het eerste testen van failover/overnemen. Het is verplicht om te testen voor een correcte werking van failover en overname met deze parameterinstellingen. Omdat de configuraties kunnen verschillen, kunnen de parameters moeten worden aangepast. 

> [!NOTE]
> Die specifiek zijn voor IBM Db2 met HADR configuratie met normaal opstarten: De secundaire of stand-by-database-exemplaar moet actief en werkend voordat u kunt de primaire database-instantie.

   
> [!NOTE]
> Voor de installatie en configuratie die specifiek is voor Azure en Pacemaker: Tijdens de installatieprocedure via SAP Software inrichting Manager is een expliciete vraag over hoge beschikbaarheid voor IBM Db2 LUW:
>+ Schakel niet **IBM Db2 pureScale**.
>+ Schakel niet **IBM Tivoli System Automation installeren voor Multiplatforms**.
>+ Schakel niet **genereren van clusterconfiguraties**.
>![SAP SWPM - DB2 HA-opties](./media/high-availability-guide-rhel-ibm-db2-luw/swpm-db2ha-opt.png)


Als u de stand-by-database-server instelt met behulp van de procedure SAP homogene system kopiëren, voert u deze stappen:

1. Selecteer de **System kopiëren** optie > **systemen als doel** > **gedistribueerde** > **Database-instantie**.
1. Als een methode voor kopiëren, selecteert u **homogene System** zodat u back-up gebruiken kunt om te herstellen van een back-up in de stand-by-server-exemplaar.
1. Wanneer u bij de stap afsluiten om de database voor het kopiëren van homogene systeem te herstellen, sluit u het installatieprogramma. De database herstellen vanuit een back-up van de primaire host. Alle fasen van de volgende installatie zijn al uitgevoerd op de primaire database-server.

#### <a name="red-hat-firewall-rules-for-db2-hadr"></a>Red Hat-firewallregels voor DB2 HADR
Firewallregels voor verkeer tussen DB2 voor HADR om te werken en DB2 toevoegen:
+ Communicatiepoort van de database. Als partities, voegt u deze poorten te.
+ HADR poort (waarde van parameter DB2 HADR_LOCAL_SVC)
+ Azure testpoort
<pre><code>sudo firewall-cmd --add-port=&lt;port&gt;/tcp --permanent
sudo firewall-cmd --reload</code></pre>

#### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR selectievakje
Voor demonstratiedoeleinden te gebruiken en de procedures in dit artikel, de database SID is **ID2**.

Nadat u HADR hebt geconfigureerd en de status van de peer-netwerk en verbonden op de primaire en stand-by-knooppunten is, voert u de volgende controle:

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
Database Member 0 -- Database ID2 -- Active -- Up 1 days 15:45:23 -- Date 2019-06-25-10.55.25.349375

                            <b>HADR_ROLE = PRIMARY
                          REPLAY_TYPE = PHYSICAL
                        HADR_SYNCMODE = NEARSYNC
                           STANDBY_ID = 1
                        LOG_STREAM_ID = 0
                           HADR_STATE = PEER
                           HADR_FLAGS =
                  PRIMARY_MEMBER_HOST = az-idb01
                     PRIMARY_INSTANCE = db2id2
                       PRIMARY_MEMBER = 0
                  STANDBY_MEMBER_HOST = az-idb02
                     STANDBY_INSTANCE = db2id2
                       STANDBY_MEMBER = 0
                  HADR_CONNECT_STATUS = CONNECTED</b>
             HADR_CONNECT_STATUS_TIME = 06/25/2019 10:55:05.076494 (1561460105)
          HEARTBEAT_INTERVAL(seconds) = 7
                     HEARTBEAT_MISSED = 5
                   HEARTBEAT_EXPECTED = 52
                HADR_TIMEOUT(seconds) = 30
        TIME_SINCE_LAST_RECV(seconds) = 5
             PEER_WAIT_LIMIT(seconds) = 0
           LOG_HADR_WAIT_CUR(seconds) = 0.000
    LOG_HADR_WAIT_RECENT_AVG(seconds) = 598.000027
   LOG_HADR_WAIT_ACCUMULATED(seconds) = 598.000
                  LOG_HADR_WAIT_COUNT = 1
SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 369280
            PRIMARY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
            STANDBY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
                  HADR_LOG_GAP(bytes) = 132242668
     STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
       STANDBY_RECV_REPLAY_GAP(bytes) = 0
                     PRIMARY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
                     STANDBY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
              STANDBY_REPLAY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
         STANDBY_RECV_BUF_SIZE(pages) = 2048
             STANDBY_RECV_BUF_PERCENT = 0
           STANDBY_SPOOL_LIMIT(pages) = 1000
                STANDBY_SPOOL_PERCENT = 0
                   STANDBY_ERROR_TIME = NULL
                 PEER_WINDOW(seconds) = 300
                      PEER_WINDOW_END = 06/25/2019 11:12:03.000000 (1561461123)
             READS_ON_STANDBY_ENABLED = N



#Secondary output:
Database Member 0 -- Database ID2 -- Standby -- Up 1 days 15:45:18 -- Date 2019-06-25-10.56.19.820474

                            <b>HADR_ROLE = STANDBY
                          REPLAY_TYPE = PHYSICAL
                        HADR_SYNCMODE = NEARSYNC
                           STANDBY_ID = 0
                        LOG_STREAM_ID = 0
                           HADR_STATE = PEER
                           HADR_FLAGS =
                  PRIMARY_MEMBER_HOST = az-idb01
                     PRIMARY_INSTANCE = db2id2
                       PRIMARY_MEMBER = 0
                  STANDBY_MEMBER_HOST = az-idb02
                     STANDBY_INSTANCE = db2id2
                       STANDBY_MEMBER = 0
                  HADR_CONNECT_STATUS = CONNECTED</b>
             HADR_CONNECT_STATUS_TIME = 06/25/2019 10:55:05.078116 (1561460105)
          HEARTBEAT_INTERVAL(seconds) = 7
                     HEARTBEAT_MISSED = 0
                   HEARTBEAT_EXPECTED = 10
                HADR_TIMEOUT(seconds) = 30
        TIME_SINCE_LAST_RECV(seconds) = 1
             PEER_WAIT_LIMIT(seconds) = 0
           LOG_HADR_WAIT_CUR(seconds) = 0.000
    LOG_HADR_WAIT_RECENT_AVG(seconds) = 598.000027
   LOG_HADR_WAIT_ACCUMULATED(seconds) = 598.000
                  LOG_HADR_WAIT_COUNT = 1
SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 367360
            PRIMARY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
            STANDBY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
                  HADR_LOG_GAP(bytes) = 0
     STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000012.LOG, 14151, 3685322855
       STANDBY_RECV_REPLAY_GAP(bytes) = 0
                     PRIMARY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
                     STANDBY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
              STANDBY_REPLAY_LOG_TIME = 06/25/2019 10:45:42.000000 (1561459542)
         STANDBY_RECV_BUF_SIZE(pages) = 2048
             STANDBY_RECV_BUF_PERCENT = 0
           STANDBY_SPOOL_LIMIT(pages) = 1000
                STANDBY_SPOOL_PERCENT = 0
                   STANDBY_ERROR_TIME = NULL
                 PEER_WINDOW(seconds) = 1000
                      PEER_WINDOW_END = 06/25/2019 11:12:59.000000 (1561461179)
             READS_ON_STANDBY_ENABLED = N

</code></pre>



## <a name="db2-pacemaker-configuration"></a>Pacemaker Db2-configuratie

Wanneer u Pacemaker voor automatische failover in het geval van storing op een knooppunt gebruikt, moet u de Db2-exemplaren en Pacemaker dienovereenkomstig configureren. Deze sectie wordt beschreven in dit type configuratie.

De volgende items worden voorafgegaan door een:

- **[A]** : Van toepassing op alle knooppunten
- **[1]** : Alleen van toepassing op knooppunt 1 
- **[2]** : Alleen van toepassing op knooppunt 2

**[A]**  Vereisten voor de configuratie van Pacemaker:
1. Sluit beide database-servers met gebruiker db2\<sid > met db2stop.
1. Wijzigen van de shell-omgeving voor db2\<sid > gebruiker */bin/ksh*:
<pre><code># Install korn shell:
sudo yum install ksh
# Change users shell:
sudo usermod -s /bin/ksh db2&lt;sid&gt;</code></pre>
   

### <a name="pacemaker-configuration"></a>Pacemaker configuratie

**[1]** IBM Db2 HADR-specific Pacemaker configuration:
<pre><code># Put Pacemaker into maintenance mode
sudo pcs property set maintenance-mode=true 
</code></pre>

**[1]**  Maken IBM Db2-resources:
<pre><code># Replace <b>bold strings</b> with your instance name db2sid, database SID, and virtual IP address/Azure Load Balancer.
sudo pcs resource create Db2_HADR_<b>ID2</b> db2 instance='<b>db2id2</b>' dblist='<b>ID2</b>' master meta notify=true resource-stickiness=5000

#Configure resource stickiness and correct cluster notifications for master resoruce
sudo pcs resource update Db2_HADR_<b>ID2</b>-master meta notify=true resource-stickiness=5000

# Configure virtual IP - same as Azure Load Balancer IP
sudo pcs resource create vip_<b>db2id2</b>_<b>ID2</b> IPaddr2 ip='<b>10.100.0.40</b>'

# Configure probe port for Azure load Balancer
sudo pcs resource create nc_<b>db2id2</b>_<b>ID2</b> azure-lb port=<b>62500</b>

#Create a group for ip and Azure loadbalancer probe port
sudo pcs resource group add g_ipnc_<b>db2id2</b>_<b>ID2</b> vip_<b>db2id2</b>_<b>ID2</b> nc_<b>db2id2</b>_<b>ID2</b>

#Create colocation constrain - keep Db2 HADR Master and Group on same node
sudo pcs constraint colocation add g_ipnc_<b>db2id2</b>_<b>ID2</b> with master Db2_HADR_<b>ID2</b>-master

#Create start order constrain
sudo pcs constraint order promote Db2_HADR_<b>ID2</b>-master then g_ipnc_<b>db2id2</b>_<b>ID2</b>
</code></pre>

**[1]** Start IBM Db2 resources:
* Pacemaker uit de onderhoudsmodus plaatsen.
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo pcs property set maintenance-mode=false</pre></code>

**[1]**  Zorg dat de clusterstatus OK is en dat alle resources die worden gestart. Het is niet belangrijk welke bronnen worden uitgevoerd op knooppunt.
<pre><code>sudo pcs status</code>
2 nodes configured
5 resources configured

Online: [az-idb01 az-idb02]

Volledige lijst met resources:

 rsc_st_azure (stonith:fence_azure_arm):      Gestarte az-idb01 (Master/slave) instellen: Db2_HADR_ID2-master [Db2_HADR_ID2] Masters: [ az-idb01 ] Slaves: [ az-idb02 ] Resource Group: g_ipnc_db2id2_ID2 vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Aan de slag az-idb01 nc_db2id2_ID2 (ocf::heartbeat:azure-lb):      Gestarte az-idb01

Daemon-Status: corosync: actief/uitgeschakeld pacemaker: actief/uitgeschakeld pcsd: actief/ingeschakeld
</pre>

> [!IMPORTANT]
> U moet de Pacemaker beheert geclusterde Db2-exemplaar met behulp van hulpprogramma's voor Pacemaker. Als u opdrachten, zoals db2stop db2, detecteert Pacemaker de actie als wanneer u een fout van de resource. Als u onderhoud uitvoert, kunt u de knooppunten of resources in de onderhoudsmodus plaatsen. Pacemaker bewakingsresources wordt onderbroken en vervolgens kunt u normale db2 beheeropdrachten gebruiken.


### <a name="configure-azure-load-balancer"></a>Azure Load Balancer configureren
Voor het configureren van Azure Load Balancer, raden wij aan dat u de [Azure Standard Load Balancer-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) en voer het volgende:

1. Maak een front-end IP-adresgroep:

   a. Open de Azure Load Balancer in Azure portal, selecteer **front-end-IP-adresgroep**, en selecteer vervolgens **toevoegen**.

   b. Voer de naam van de nieuwe front-end-IP-adresgroep (bijvoorbeeld **Db2-connection**).

   c. Stel de **toewijzing** te **statische**, en voer het IP-adres **virtuele IP-** gedefinieerd aan het begin.

   d. Selecteer **OK**.

   e. Nadat de nieuwe front-end-IP-adresgroep is gemaakt, houd er rekening mee dat het IP-adres van toepassingen.

1. Maak een back-end-groep:

   a. Open de Azure Load Balancer in Azure portal, selecteer **back-endpools**, en selecteer vervolgens **toevoegen**.

   b. Voer de naam van de nieuwe back-end-pool (bijvoorbeeld **Db2-back-end**).

   c. Selecteer **toevoegen van een virtuele machine**.

   d. Selecteer de beschikbaarheidsset of de virtuele machines die als host fungeert voor IBM Db2-database in de vorige stap hebt gemaakt.

   e. Selecteer de virtuele machines van de IBM Db2-cluster.

   f. Selecteer **OK**.

1. Maak een statustest:

   a. Open de Azure Load Balancer in Azure portal, selecteer **statuscontroles**, en selecteer **toevoegen**.

   b. Voer de naam van de nieuwe statustest (bijvoorbeeld **Db2-hp**).

   c. Selecteer **TCP** als het protocol en poort **62500**. Houd de **Interval** waarde ingesteld op **5**, en houd de **drempelwaarde voor onjuiste status** waarde ingesteld op **2**.

   d. Selecteer **OK**.

1. De regels voor taakverdeling maken:

   a. Open de Azure Load Balancer in Azure portal, selecteer **Taakverdelingsregels**, en selecteer vervolgens **toevoegen**.

   b. Voer de naam van de nieuwe Load Balancer-regel (bijvoorbeeld **Db2-SID**).

   c. Selecteer het front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **Db2-frontend**).

   d. Houd de **Protocol** ingesteld op **TCP**, en voer de poort *Database communicatiepoort*.

   e. Verhoog de **time-out voor inactiviteit** tot 30 minuten.

   f. Zorg ervoor dat u **Floating IP inschakelen**.

   g. Selecteer **OK**.

**[A]**  Firewallregel toevoegen voor testpoort:
<pre><code>sudo firewall-cmd --add-port=<b><probe-port></b>/tcp --permanent
sudo firewall-cmd --reload</code></pre>

### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Wijzigingen aanbrengen in SAP-profielen virtueel IP-adres gebruiken voor verbinding
Als u wilt verbinding maken met het primaire exemplaar van de configuratie HADR de SAP moet toepassingslaag het virtuele IP-adres dat u hebt gedefinieerd en geconfigureerd voor de Azure Load Balancer gebruiken. De volgende wijzigingen zijn vereist:

/sapmnt/\<SID>/profile/DEFAULT.PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Installatie van primaire en de toepassingsservers dialoogvenster

Wanneer u primaire installeert en toepassingsservers op basis van de configuratie van een Db2-HADR dialoogvenster, gebruikt de virtuele host naam die u hebt geselecteerd voor de configuratie. 

Als u de installatie voordat u de configuratie van de Db2 HADR gemaakt hebt uitgevoerd, moet u de wijzigingen, zoals beschreven in de vorige sectie en als volgt voor SAP-Java-stacks.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>ABAP + Java of Java-stack-systemen JDBC URL controleren

De J2EE-Config-hulpprogramma gebruiken om te controleren of de URL JDBC bijwerken. Omdat het J2EE-Config-hulpprogramma een grafisch hulpprogramma is, moet u hebben X server is geïnstalleerd:
 
1. Aanmelden bij de primaire toepassingsserver van het exemplaar J2EE en uitvoeren:
     <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>
1. Kies in het linkerdeelvenster **security store**.
1. Kies in het rechterdeelvenster de belangrijkste jdbc/groep / \ <SAPSID> /URL.
1. Wijzig de hostnaam in de JDBC-URL op de virtuele host-naam.
     <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>
1. Selecteer **toevoegen**.
1. Om uw wijzigingen hebt opgeslagen, selecteert u het pictogram van de schijf in de linkerbovenhoek.
1. Sluit het configuratieprogramma.
1. De Java-exemplaar opnieuw opstarten.

## <a name="configure-log-archiving-for-hadr-setup"></a>Logboek voor de installatie van HADR archivering configureren
Als u wilt de Db2-logboek archiveren voor HADR setup configureert, is het raadzaam dat u configureert zowel de primaire als de stand-by-database hebben mogelijkheid van automatisch logboekgegevens voor het ophalen van alle logboek archiveren locaties. De primaire en stand-by-database moet mogelijk zijn om op te halen van logboekbestanden van het archief van alle de logboek archiveren locaties op die een van de database exemplaren mogelijk Archiveer de logboekbestanden. 

Het logboek archiveren wordt alleen door de primaire database uitgevoerd. Als u de rollen HADR van de database-servers wijzigen of als er een fout optreedt, is de nieuwe primaire database is verantwoordelijk voor het archiveren van logboekbestanden. Als u meerdere logboeklocaties archief hebt ingesteld, kunnen uw logboeken twee keer worden gearchiveerd. In het geval van een lokaal of extern bijwerken hebt u mogelijk ook handmatig kopiëren van de gearchiveerde logboeken van de oude primaire server naar de locatie van het actieve logboek van de nieuwe primaire server.

Het is raadzaam om de configuratie van een algemene NFS-share of GlusterFS, waar de logboeken vanaf beide knooppunten worden geschreven. De NFS-share of GlusterFS is maximaal beschikbaar. 

U kunt bestaande maximaal beschikbare NFS-shares of GlusterFS gebruiken voor het transport of een map met het gebruikersprofiel. Zie voor meer informatie:

- [GlusterFS op Azure VM's op Red Hat Enterprise Linux voor SAP NetWeaver][glusterfs] 
- [Hoge beschikbaarheid voor SAP NetWeaver op virtuele Azure-machines op Red Hat Enterprise Linux met Azure NetApp-bestanden voor SAP-toepassingen][anf-rhel]
- [Azure Files met NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) (om het NFS-shares te maken)

## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

Deze sectie wordt beschreven hoe u uw instellingen Db2 HADR kunt testen. Elke test wordt ervan uitgegaan dat de primaire IBM Db2 wordt uitgevoerd op de *az-idb01* virtuele machine. Gebruiker met sudo-machtigingen of het basiscertificaat (niet aanbevolen) moet worden gebruikt.

De initiële status van alle testcases wordt hier uitgelegd: (crm_mon - r of pc's status)

- **status van de pc's** is een momentopname van de status Pacemaker tijdens de uitvoering 
- **crm_mon - r** is continue uitvoer van Pacemaker status

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled
</code></pre>

De status van de oorspronkelijke in een SAP-systeem wordt beschreven in de transactie DBACOCKPIT > Configuratie > overzicht, zoals wordt weergegeven in de volgende afbeelding:

![DBACockpit - Pre-migratie](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-org-rhel.png)




### <a name="test-takeover-of-ibm-db2"></a>Test overname van IBM Db2


> [!IMPORTANT] 
> Voordat u de test start, controleert u of:
> * Pacemaker beschikt niet over een mislukte acties (status van de pc's).
> * Er zijn geen Locatiebeperkingen (restanten van migratie-test)
> * De synchronisatie van IBM Db2 HADR werkt. Neem contact op met de gebruiker db2\<sid > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Migreren van het knooppunt waarop de primaire Db2-database wordt uitgevoerd door het uitvoeren van de volgende opdracht:
<pre><code>sudo pcs resource move Db2_HADR_<b>ID2</b>-master</code></pre>

Nadat de migratie is voltooid, wordt de uitvoer van de status van crm lijkt:
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

</code></pre>

De status van de oorspronkelijke in een SAP-systeem wordt beschreven in de transactie DBACOCKPIT > Configuratie > overzicht, zoals wordt weergegeven in de volgende afbeelding:

![DBACockpit - nadat de migratie](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-post-rhel.png)

Resourcemigratie met 'pc's verplaatsen van de resource' maakt Locatiebeperkingen. Locatiebeperkingen belemmeren in dit geval actief IBM Db2-exemplaar op az-idb01. Als de locatie niet worden verwijderd, de bron kunt geen failback.

Remove beperken van de locatie en stand-by-knooppunt wordt gestart op az-idb01.
<pre><code>sudo pcs resource clear Db2_HADR_<b>ID2</b>-master</code></pre>
En clusterstatus gewijzigd in:
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb01
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02</code></pre>

![DBACockpit - verwijderde locatie beperken](./media/high-availability-guide-rhel-ibm-db2-luw/hadr-sap-mgr-clear-rhel.png)


Migreren van de resource naar *az-idb01* en schakelt u de Locatiebeperkingen
<pre><code>sudo pcs resource move Db2_HADR_<b>ID2</b>-master az-idb01
sudo pcs resource clear Db2_HADR_<b>ID2</b>-master
</code></pre>

- **verplaatsen van de pc's-resource \<res_name > <host>:** Locatie maakt en kan problemen veroorzaken met overnemen
- **pc's resource wissen \<res_name >** : Hiermee worden gewist Locatiebeperkingen
- **opruimen van de pc's-resource \<res_name >** : Hiermee wist alle fouten van de resource

### <a name="test-a-manual-takeover"></a>Een handmatige overname testen

U kunt een handmatige overname testen door het stoppen van de service Pacemaker op *az-idb01* knooppunt:
<pre><code>systemctl stop pacemaker</code></pre>

status van *az-ibdb02*
<pre><code>2 nodes configured
5 resources configured

Node az-idb01: pending
Online: [ az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Daemon Status:
  corosync: active/disabled
  pacemaker: active/disabled
  pcsd: active/enabled</code></pre>

Na de failover, u kunt de service opnieuw starten op *az-idb01*.
<pre><code>systemctl start  pacemaker</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-hadr-primary-database"></a>De Db2-proces op het knooppunt waarop de primaire database HADR beëindigen

<pre><code>#Kill main db2 process - db2sysc
[sapadmin@az-idb02 ~]$ sudo ps -ef|grep db2sysc
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0
[sapadmin@az-idb02 ~]$ sudo kill -9 34598
</code></pre>

Het exemplaar Db2 gaat mislukken, en Pacemaker wordt verplaatst-hoofdknooppunt en het rapport status te volgen:

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=49, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 09:57:35 2019', queued=0ms, exec=362ms</code></pre>

Pacemaker wordt opnieuw opgestart het exemplaar van de primaire database Db2 op hetzelfde knooppunt, of deze een failover uitgevoerd naar het knooppunt waarop de secundaire database-instantie wordt uitgevoerd en wordt een fout gerapporteerd.

### <a name="kill-the-db2-process-on-the-node-that-runs-the-secondary-database-instance"></a>Beëindig de Db2-proces op het knooppunt waarop de secundaire database-exemplaar wordt uitgevoerd

<pre><code>[sapadmin@az-idb02 ~]$ sudo ps -ef|grep db2sysc
db2id2    23144  23142  2 09:53 ?        00:00:13 db2sysc 0
[sapadmin@az-idb02 ~]$ sudo kill -9 23144
</code></pre>

Het knooppunt opgehaald in kan niet wordt vermeld en fout gemeld
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01

Failed Actions:
* Db2_HADR_ID2_monitor_20000 on az-idb02 'not running' (7): call=144, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 10:02:09 2019', queued=0ms, exec=0ms</code></pre>

De Db2-exemplaar wordt opnieuw gestart in de secundaire rol die deze had toegewezen voordat.

### <a name="stop-db-via-db2stop-force-on-the-node-that-runs-the-hadr-primary-database-instance"></a>Stop DB via db2stop afdwingen op het knooppunt waarop de primaire database HADR exemplaar wordt uitgevoerd

Als gebruiker db2\<sid > opdracht db2stop force uitvoeren:
<pre><code>az-idb01:db2ptr> db2stop force</code></pre>

Fout gevonden:

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Slaves: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Stopped
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Stopped

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=110, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 14:03:12 2019', queued=0ms, exec=355ms</code></pre>

De Db2 HADR secundaire database-instantie is gepromoveerd tot de primaire rol.
<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

 rsc_st_azure   (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02

Failed Actions:
* Db2_HADR_ID2_demote_0 on az-idb01 'unknown error' (1): call=110, status=complete, exitreason='none',
    last-rc-change='Wed Jun 26 14:03:12 2019', queued=0ms, exec=355ms</pre></code>


### <a name="crash-the-vm-that-runs-the-hadr-primary-database-instance-with-halt"></a>Crash van de virtuele machine waarop het exemplaar van de primaire database HADR met "stilstand"

<pre><code>#Linux kernel panic. 
sudo echo b > /proc/sysrq-trigger</code></pre>

In dat geval detecteert Pacemaker dat het knooppunt waarop de primaire database-exemplaar wordt niet meer reageert.

<pre><code>2 nodes configured
5 resources configured

Node az-idb01: UNCLEAN (online)
Online: [ az-idb02 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb01 ]
     Slaves: [ az-idb02 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb01
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb01</code></pre>

de volgende stap is om te controleren op een *splitsen brein* situatie. Nadat het overgebleven knooppunt heeft vastgesteld dat het knooppunt dat de primaire database-exemplaar het laatst is uitgevoerd niet actief is, wordt een failover van de resources wordt uitgevoerd.

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb02 ]
OFFLINE: [ az-idb01 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Stopped: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02 </code></pre>


In het geval van een kernelprobleem, wordt het knooppunt door agent voor de eerste optie worden restared. Nadat het knooppunt weer online is, moet u pacemaker cluster door starten
<pre><code>sudo pcs cluster start</code></pre> de Db2-exemplaar in de secundaire rol starten

<pre><code>2 nodes configured
5 resources configured

Online: [ az-idb01 az-idb02 ]

Full list of resources:

rsc_st_azure    (stonith:fence_azure_arm):      Started az-idb02
 Master/Slave Set: Db2_HADR_ID2-master [Db2_HADR_ID2]
     Masters: [ az-idb02 ]
     Slaves: [ az-idb01 ]
 Resource Group: g_ipnc_db2id2_ID2
     vip_db2id2_ID2     (ocf::heartbeat:IPaddr2):       Started az-idb02
     nc_db2id2_ID2      (ocf::heartbeat:azure-lb):      Started az-idb02</code></pre>

## <a name="next-steps"></a>Volgende stappen
- [Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Pacemaker op Red Hat Enterprise Linux in Azure instellen][rhel-pcs-azr]
