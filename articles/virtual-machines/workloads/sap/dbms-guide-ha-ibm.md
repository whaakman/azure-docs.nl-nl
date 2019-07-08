---
title: IBM Db2 HADR instellen op Azure virtual machines (VM's) | Microsoft Docs
description: Hoge beschikbaarheid van IBM Db2 LUW op Azure virtual machines (VM's) maken.
services: virtual-machines-linux
documentationcenter: ''
author: msjuergent
manager: patfilot
editor: ''
tags: azure-resource-manager
keywords: SAP
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/10/2019
ms.author: juergent
ms.openlocfilehash: 7464ea481d4c95856b78a83a875f2cd24c00705b
ms.sourcegitcommit: 837dfd2c84a810c75b009d5813ecb67237aaf6b8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/02/2019
ms.locfileid: "67503330"
---
[1928533]: https://launchpad.support.sap.com/#/notes/1928533
[2015553]: https://launchpad.support.sap.com/#/notes/2015553
[2178632]: https://launchpad.support.sap.com/#/notes/2178632
[2191498]: https://launchpad.support.sap.com/#/notes/2191498
[2243692]: https://launchpad.support.sap.com/#/notes/2243692
[1984787]: https://launchpad.support.sap.com/#/notes/1984787
[1999351]: https://launchpad.support.sap.com/#/notes/1999351
[2233094]: https://launchpad.support.sap.com/#/notes/2233094
[1612105]: https://launchpad.support.sap.com/#/notes/1612105

[sles-for-sap-bp]:https://www.suse.com/documentation/sles-for-sap-12/
[db2-hadr-11.1]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_11.1.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[db2-hadr-10.5]:https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html
[dbms-db2]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/dbms_guide_ibm
[sles-pacemaker]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker
[sap-instfind]:https://help.sap.com/viewer/9e41ead9f54e44c1ae1a1094b0f80712/ALL/en-US/576f5c1808de4d1abecbd6e503c9ba42.html
[nfs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs
[sles-ha-guide]:https://www.suse.com/releasenotes/x86_64/SLE-HA/12-SP3/
[ascs-ha]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse

[dbms-guide]:dbms-guide.md
[deployment-guide]:deployment-guide.md
[planning-guide]:planning-guide.md
[azr-sap-plancheck]:https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-deployment-checklist



# <a name="high-availability-of-ibm-db2-luw-on-azure-vms-on-suse-linux-enterprise-server-with-pacemaker"></a>Hoge beschikbaarheid van IBM Db2 LUW op Azure VM's in SUSE Linux Enterprise Server met Pacemaker

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
| [1984787] | SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
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
| [SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP3 best practice-richtlijnen][sles-for-sap-bp] |
| [SUSE Linux Enterprise hoge beschikbaarheid extensie 12 SP3][sles-ha-guide] |
| [IBM Db2 Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP][dbms-db2] |
| [IBM Db2 HADR 11.1][db2-hadr-11.1] |
| [IBM Db2 HADR R 10.5][db2-hadr-10.5] |

## <a name="overview"></a>Overzicht
Voor het bereiken van hoge beschikbaarheid, IBM Db2 LUW met HADR is geïnstalleerd op ten minste twee virtuele machines van Azure, die zijn geïmplementeerd in een [Azure-beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) of over meerdere [Azure Availability Zones](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). 

De volgende afbeeldingen weer een installatie van twee virtuele machines van Azure-databaseserver. Zowel Azure-VM's van de database-server hebben hun eigen opslag die is gekoppeld en actief zijn. In HADR heeft één database-instantie in een van de Azure VM's de rol van het primaire exemplaar. Alle clients zijn verbonden met dit primaire exemplaar. Alle wijzigingen in de databasetransacties worden lokaal opgeslagen in het transactielogboek Db2. Als de transactie logboekrecords lokaal opgeslagen worden, worden de records overgebracht via TCP/IP op de database-instantie op de tweede database-server, de stand-by-server of de stand-by-exemplaar. De standby-instantie werkt de lokale database door te draaien naar voren de overgebrachte transactie records in logboek registreren. Op deze manier wordt de stand-by-server bewaard gesynchroniseerd met de primaire server.

HADR is alleen een replicatiefunctionaliteit. Er is geen foutdetectie en geen automatische failover voor een overname of faciliteiten. Een overname of overdracht naar de stand-by-server moet handmatig worden gestart door een databasebeheerder. Als u wilt een automatische overnemen en foutdetectie bereiken, kunt u de clusteringfunctie Linux Pacemaker. Pacemaker bewaakt de twee database server-exemplaren. Wanneer de primaire database server-exemplaar vastloopt, Pacemaker initieert een *automatische* HADR overname door de stand-by-server. Pacemaker zorgt er ook voor dat het virtuele IP-adres is toegewezen aan de nieuwe primaire server.

![Overzicht van IBM Db2 hoge beschikbaarheid](./media/dbms-guide-ha-ibm/ha-db2-hadr-lb.png)

Om SAP-toepassingsservers verbinding maken met de primaire database, moet u de naam van een virtuele host en een virtueel IP-adres. De SAP-toepassingsservers wordt in het geval van een failover verbinding maken met de nieuwe primaire database-instantie. In een Azure-omgeving, een [Azure load balancer](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) is vereist voor het gebruik van een virtueel IP-adres in de manier die voor HADR van IBM Db2 zijn vereist. 

De volgende afbeelding geeft, kunt u beter begrijpen hoe een maximaal beschikbare installatie van de SAP-systeem IBM Db2 LUW met HADR en Pacemaker past een overzicht van een maximaal beschikbare installatie van een SAP-systeem op basis van IBM Db2-database. Dit artikel behandelt alleen IBM Db2, maar deze bevat verwijzingen naar andere artikelen over het instellen van andere onderdelen van een SAP-systeem.

![Overzicht van de volledige omgeving IBM DB2 hoge beschikbaarheid](.//media/dbms-guide-ha-ibm/end-2-end-ha.png)


### <a name="high-level-overview-of-the-required-steps"></a>Overzicht van de vereiste stappen op hoog niveau
Voor het implementeren van een IBM Db2-configuratie, moet u als volgt te werk:

  + Uw omgeving plannen.
  + Implementeer de virtuele machines.
  + SUSE Linux bijwerken en bestandssystemen configureren.
  + Installeer en configureer Pacemaker.
  + Installeer [maximaal beschikbare NFS][nfs-ha].
  + Installeer [ASCS/gebruikers op een ander cluster][ascs-ha].
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
| De eerste optie Azure | De eerste optie Azure of SBD de eerste optie (ten zeerste aanbevolen). Methode om te voorkomen dat splitsen brein situaties wordt voorkomen. |
| SBD VM | Grootte van de virtuele machine SBD, opslag, netwerk. |
| Azure Load Balancer | Verbruik van Basic of Standard (aanbevolen), testpoort voor Db2-database (onze aanbeveling 62500) **testpoort**. |
| Naamomzetting| Hoe de naamomzetting werkt in de omgeving. DNS-service wordt sterk aanbevolen. Lokale hosts-bestand kan worden gebruikt. |
    
Zie voor meer informatie over Linux Pacemaker in Azure, [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker).

## <a name="deployment-on-suse-linux"></a>Implementatie in SUSE Linux

De resource-agent voor IBM Db2 LUW is opgenomen in SUSE Linux Enterprise Server voor SAP-toepassingen. Voor de instelling die wordt beschreven in dit document, moet u SUSE Linux-Server gebruiken voor SAP-toepassingen. De Azure Marketplace bevat een afbeelding voor SUSE Enterprise Server voor 12 van de SAP-toepassingen die u gebruiken kunt voor het implementeren van nieuwe virtuele machines van Azure. Houd rekening met de verschillende support of service-modellen die worden aangeboden door SUSE via Azure Marketplace wanneer u een VM-installatiekopie bij de Azure VM Marketplace selecteert. 

### <a name="hosts-dns-updates"></a>Hosts: DNS-updates
Maak een lijst van alle hostnamen, waaronder de namen van de virtuele host, en werk uw DNS-servers, zodat de juiste IP-adres op de host-naamomzetting. Als een DNS-server bestaat niet of u kunt geen bijwerken en DNS-vermeldingen te maken, moet u de lokale host-bestanden van de afzonderlijke virtuele machines die deelnemen aan in dit scenario gebruiken. Als u bestanden hostvermeldingen, zorg ervoor dat de vermeldingen worden toegepast op alle virtuele machines in de systeemomgeving SAP. We raden u echter aan dat u uw DNS waarmee, in het ideale geval wordt uitgebreid naar Azure


### <a name="manual-deployment"></a>Handmatige implementatie

Zorg ervoor dat het geselecteerde besturingssysteem wordt ondersteund door IBM/SAP voor IBM Db2 LUW. De lijst met ondersteunde versies van het besturingssysteem voor virtuele Azure-machines en Db2 releases is beschikbaar in de SAP-notitie [1928533]. De lijst met OS releases door afzonderlijke Db2 versie is beschikbaar in de Matrix beschikbaarheid van SAP-Product. We raden een minimum van SLES 12 SP3 door met betrekking tot Azure-prestatieverbeteringen in deze of hoger SUSE Linux-versies.

1. Maak of Selecteer een resourcegroep.
1. Maak of Selecteer een virtueel netwerk en subnet.
1. Een Azure-beschikbaarheidsset maken of implementeren van een beschikbaarheidszone.
    + Stel het maximum aantal updatedomeinen op 2 voor de beschikbaarheidsset.
1. Virtuele Machine 1 maken.
    + Gebruik SLES voor SAP-installatiekopie in de Azure Marketplace.
    + Selecteer het Azure-beschikbaarheidsset dat u in stap 3 hebt gemaakt, of binnen een Beschikbaarheidszone.
1.  Maak virtuele Machine 2.
    + Gebruik SLES voor SAP-installatiekopie in de Azure Marketplace.
    + Selecteer het Azure-beschikbaarheidsset u in stap 3 hebt gemaakt, of selecteer binnen een Beschikbaarheidszone (niet de dezelfde zone zoals in stap 3).
1. Gegevensschijven toevoegen aan de virtuele machines en controleer vervolgens de aanbeveling van de installatie van een bestand system in het artikel [IBM Db2 Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP][dbms-db2].

## <a name="create-the-pacemaker-cluster"></a>Maken van het cluster Pacemaker
    
Zie voor het maken van een eenvoudige Pacemaker-cluster voor deze server IBM Db2, [Pacemaker op SUSE Linux Enterprise Server in Azure instellen][sles-pacemaker]. 

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

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Tips voor het instellen van IBM Db2 LUW met HADR installatie

De primaire LUW van IBM Db2-database-instantie instellen:

- Gebruik de hoge beschikbaarheid of gedistribueerde optie.
- Installeer de SAP ASCS/INGEN en Database-exemplaar.
- Maak een back-up van de geïnstalleerde database.


> [!IMPORTANT] 
> Noteer de 'Database communicatiepoort' die ingesteld tijdens de installatie. Dit moet hetzelfde poortnummer voor beide exemplaren van de database

Als u de stand-by-database-server instelt met behulp van de procedure SAP homogene system kopiëren, voert u deze stappen:

1. Selecteer de **System kopiëren** optie > **systemen als doel** > **gedistribueerde** > **Database-instantie**.
1. Als een methode voor kopiëren, selecteert u **homogene System** zodat u back-up gebruiken kunt om te herstellen van een back-up in de stand-by-server-exemplaar.
1. Wanneer u bij de stap afsluiten om de database voor het kopiëren van homogene systeem te herstellen, sluit u het installatieprogramma. De database herstellen vanuit een back-up van de primaire host. Alle fasen van de volgende installatie zijn al uitgevoerd op de primaire database-server.
1. HADR voor IBM Db2 instellen.

   > [!NOTE]
   > Voor de installatie en configuratie die specifiek is voor Azure en Pacemaker: Tijdens de installatieprocedure via SAP Software inrichting Manager is een expliciete vraag over hoge beschikbaarheid voor IBM Db2 LUW:
   >+ Schakel niet **IBM Db2 pureScale**.
   >+ Schakel niet **IBM Tivoli System Automation installeren voor Multiplatforms**.
   >+ Schakel niet **genereren van clusterconfiguraties**.

   Wanneer u een apparaat SBD voor Linux Pacemaker gebruikt, moet u de volgende Db2 HADR parameters instellen:
   + Duur van het venster HADR peer (seconden) (HADR_PEER_WINDOW) = 300  
   + HADR time-outwaarde (HADR_TIMEOUT) = 60

   Wanneer u een agent van de eerste optie Azure Pacemaker gebruikt, moet u de volgende parameters instellen:
   + Duur van het venster HADR peer (seconden) (HADR_PEER_WINDOW) = 900  
   + HADR time-outwaarde (HADR_TIMEOUT) = 60

U wordt aangeraden de bovenstaande parameters op basis van het eerste testen van failover/overnemen. Het is verplicht om te testen voor een correcte werking van failover en overname met deze parameterinstellingen. Omdat de configuraties kunnen verschillen, kunnen de parameters moeten worden aangepast. 

> [!IMPORTANT]
> Die specifiek zijn voor IBM Db2 met HADR configuratie met normaal opstarten: De secundaire of stand-by-database-exemplaar moet actief en werkend voordat u kunt de primaire database-instantie.

Voor demonstratiedoeleinden te gebruiken en de procedures in dit artikel, de database SID is **PTR**.

#### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR selectievakje
Nadat u HADR hebt geconfigureerd en de status van de peer-netwerk en verbonden op de primaire en stand-by-knooppunten is, voert u de volgende controle:

<pre><code>
Execute command as db2&lt;sid&gt; db2pd -hadr -db &lt;SID&gt;

#Primary output:
# Database Member 0 -- Database PTR -- Active -- Up 1 days 01:51:38 -- Date 2019-02-06-15.35.28.505451
# 
#                             <b>HADR_ROLE = PRIMARY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 1
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.170561 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6137
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 13
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000025
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223713
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 374400
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15571, 27902548040
#        STANDBY_RECV_REPLAY_GAP(bytes) = 0
#                      PRIMARY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#                      STANDBY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:34:39.000000 (1549467279)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:40:25.000000 (1549467625)
#              READS_ON_STANDBY_ENABLED = N

#Secondary output:
# Database Member 0 -- Database PTR -- Standby -- Up 1 days 01:46:43 -- Date 2019-02-06-15.38.25.644168
# 
#                             <b>HADR_ROLE = STANDBY
#                           REPLAY_TYPE = PHYSICAL
#                         HADR_SYNCMODE = NEARSYNC
#                            STANDBY_ID = 0
#                         LOG_STREAM_ID = 0
#                            HADR_STATE = PEER
#                            HADR_FLAGS = TCP_PROTOCOL
#                   PRIMARY_MEMBER_HOST = azibmdb02
#                      PRIMARY_INSTANCE = db2ptr
#                        PRIMARY_MEMBER = 0
#                   STANDBY_MEMBER_HOST = azibmdb01
#                      STANDBY_INSTANCE = db2ptr
#                        STANDBY_MEMBER = 0
#                   HADR_CONNECT_STATUS = CONNECTED</b>
#              HADR_CONNECT_STATUS_TIME = 02/05/2019 13:51:47.205067 (1549374707)
#           HEARTBEAT_INTERVAL(seconds) = 15
#                      HEARTBEAT_MISSED = 0
#                    HEARTBEAT_EXPECTED = 6186
#                 HADR_TIMEOUT(seconds) = 60
#         TIME_SINCE_LAST_RECV(seconds) = 5
#              PEER_WAIT_LIMIT(seconds) = 0
#            LOG_HADR_WAIT_CUR(seconds) = 0.000
#     LOG_HADR_WAIT_RECENT_AVG(seconds) = 0.000023
#    LOG_HADR_WAIT_ACCUMULATED(seconds) = 434.595
#                   LOG_HADR_WAIT_COUNT = 223725
# SOCK_SEND_BUF_REQUESTED,ACTUAL(bytes) = 0, 46080
# SOCK_RECV_BUF_REQUESTED,ACTUAL(bytes) = 0, 372480
#             PRIMARY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#             STANDBY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#                   HADR_LOG_GAP(bytes) = 0
#      STANDBY_REPLAY_LOG_FILE,PAGE,POS = S0000280.LOG, 15574, 27902562173
#        STANDBY_RECV_REPLAY_GAP(bytes) = 155
#                      PRIMARY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#                      STANDBY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#               STANDBY_REPLAY_LOG_TIME = 02/06/2019 15:37:34.000000 (1549467454)
#          STANDBY_RECV_BUF_SIZE(pages) = 2048
#              STANDBY_RECV_BUF_PERCENT = 0
#            STANDBY_SPOOL_LIMIT(pages) = 0
#                 STANDBY_SPOOL_PERCENT = NULL
#                    STANDBY_ERROR_TIME = NULL
#                  PEER_WINDOW(seconds) = 300
#                       PEER_WINDOW_END = 02/06/2019 15:43:19.000000 (1549467799)
#              READS_ON_STANDBY_ENABLED = N
</code></pre>



## <a name="db2-pacemaker-configuration"></a>Pacemaker Db2-configuratie

Wanneer u Pacemaker voor automatische failover in het geval van storing op een knooppunt gebruikt, moet u de Db2-exemplaren en Pacemaker dienovereenkomstig configureren. Deze sectie wordt beschreven in dit type configuratie.

De volgende items worden voorafgegaan door een:

- **[A]** : Van toepassing op alle knooppunten
- **[1]** : Alleen van toepassing op knooppunt 1 
- **[2]** : Alleen van toepassing op knooppunt 2

**[A]**  Vereisten voor de configuratie van Pacemaker:
1. Sluit beide database-servers met gebruiker db2\<sid > met db2stop.
1. Wijzigen van de shell-omgeving voor db2\<sid > gebruiker */bin/ksh*. U wordt aangeraden dat u het hulpprogramma Yast gebruiken. 


### <a name="pacemaker-configuration"></a>Pacemaker configuratie

**[1]** IBM Db2 HADR-specific Pacemaker configuration:
<pre><code># Put Pacemaker into maintenance mode
sudo crm configure property maintenance-mode=true
</code></pre>

**[1]**  Maken IBM Db2-resources:
<pre><code># Replace **bold strings** with your instance name db2sid, database SID, and virtual IP address/Azure Load Balancer.

sudo crm configure primitive rsc_Db2_db2ptr_<b>PTR</b> db2 \
        params instance="<b>db2ptr</b>" dblist="<b>PTR</b>" \
        op start interval="0" timeout="130" \
        op stop interval="0" timeout="120" \
        op promote interval="0" timeout="120" \
        op demote interval="0" timeout="120" \
        op monitor interval="30" timeout="60" \
        op monitor interval="31" role="Master" timeout="60"

# Configure virtual IP - same as Azure Load Balancer IP
sudo crm configure primitive rsc_ip_db2ptr_<b>PTR</b> IPaddr2 \
        op monitor interval="10s" timeout="20s" \
        params ip="<b>10.100.0.10</b>"

# Configure probe port for Azure load Balancer
sudo crm configure primitive rsc_nc_db2ptr_<b>PTR</b> anything \
        params binfile="/usr/bin/nc" cmdline_options="-l -k <b>62500</b>" \
        op monitor timeout="20s" interval="10" depth="0"

sudo crm configure group g_ip_db2ptr_<b>PTR</b> rsc_ip_db2ptr_<b>PTR</b> rsc_nc_db2ptr_<b>PTR</b>

sudo crm configure ms msl_Db2_db2ptr_<b>PTR</b> rsc_Db2_db2ptr_<b>PTR</b> \
        meta target-role="Started" notify="true"

sudo crm configure colocation col_db2_db2ptr_<b>PTR</b> inf: g_ip_db2ptr_<b>PTR</b>:Started msl_Db2_db2ptr_<b>PTR</b>:Master

sudo crm configure order ord_db2_ip_db2ptr_<b>PTR</b> inf: msl_Db2_db2ptr_<b>PTR</b>:promote g_ip_db2ptr_<b>PTR</b>:start

sudo crm configure rsc_defaults resource-stickiness=1000
sudo crm configure rsc_defaults migration-threshold=5000
</code></pre>

**[1]** Start IBM Db2 resources:
* Pacemaker uit de onderhoudsmodus plaatsen.
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo crm configure property maintenance-mode=false</pre></code>

**[1]**  Zorg dat de clusterstatus OK is en dat alle resources die worden gestart. Het is niet belangrijk welke bronnen worden uitgevoerd op knooppunt.
<pre><code>sudo crm status</code>

# <a name="2-nodes-configured"></a>2 knooppunten dat is geconfigureerd
# <a name="5-resources-configured"></a>5 resources die zijn geconfigureerd

# <a name="online--azibmdb01-azibmdb02-"></a>Online: [azibmdb01 azibmdb02]

# <a name="full-list-of-resources"></a>Volledige lijst met resources:

#  <a name="stonith-sbd----stonithexternalsbd-started-azibmdb02"></a>stonith instellen-sbd (stonith:external / sbd): Gestarte azibmdb02
#  <a name="resource-group-gipdb2ptrptr"></a>Resourcegroep: g_ip_db2ptr_PTR
#      <a name="rscipdb2ptrptr--ocfheartbeatipaddr2-------started-azibmdb02"></a>rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Gestarte azibmdb02
#      <a name="rscncdb2ptrptr--ocfheartbeatanything------started-azibmdb02"></a>rsc_nc_db2ptr_PTR (ocf::heartbeat: alles):      Gestarte azibmdb02
#  <a name="masterslave-set-msldb2db2ptrptr-rscdb2db2ptrptr"></a>Master/Slave-Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
#      <a name="masters--azibmdb02-"></a>Masters: [azibmdb02]
#      <a name="slaves--azibmdb01-"></a>Slaves: [ azibmdb01 ]
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
 
1. Aanmelden bij de primaire toepassingsserver van het exemplaar J2EE en uitvoeren:   `sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh`
1. Kies in het linkerdeelvenster **security store**.
1. Kies in het rechterdeelvenster de belangrijkstejdbc/groep/\<SAPSID > / url.
1. Wijzig de hostnaam in de JDBC-URL op de virtuele host-naam.
     `jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0`
1. Selecteer **Toevoegen**.
1. Om uw wijzigingen hebt opgeslagen, selecteert u het pictogram van de schijf in de linkerbovenhoek.
1. Sluit het configuratieprogramma.
1. De Java-exemplaar opnieuw opstarten.

## <a name="configure-log-archiving-for-hadr-setup"></a>Logboek voor de installatie van HADR archivering configureren
Als u wilt de Db2-logboek archiveren voor HADR setup configureert, is het raadzaam dat u configureert zowel de primaire als de stand-by-database hebben mogelijkheid van automatisch logboekgegevens voor het ophalen van alle logboek archiveren locaties. De primaire en stand-by-database moet mogelijk zijn om op te halen van logboekbestanden van het archief van alle de logboek archiveren locaties op die een van de database exemplaren mogelijk Archiveer de logboekbestanden. 

Het logboek archiveren wordt alleen door de primaire database uitgevoerd. Als u de rollen HADR van de database-servers wijzigen of als er een fout optreedt, is de nieuwe primaire database is verantwoordelijk voor het archiveren van logboekbestanden. Als u meerdere logboeklocaties archief hebt ingesteld, kunnen uw logboeken twee keer worden gearchiveerd. In het geval van een lokaal of extern bijwerken hebt u mogelijk ook handmatig kopiëren van de gearchiveerde logboeken van de oude primaire server naar de locatie van het actieve logboek van de nieuwe primaire server.

Het is raadzaam om een algemene NFS-share waar logboeken worden geschreven vanaf beide knooppunten te configureren. De NFS-share is maximaal beschikbaar. 

U kunt bestaande maximaal beschikbare NFS-shares voor transporten of een map met het gebruikersprofiel. Zie voor meer informatie:

- [Hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server][nfs-ha] 
- [Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server met Azure NetApp-bestanden voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
- [Azure Files met NetApp](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) (om het NFS-shares te maken)


## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

Deze sectie wordt beschreven hoe u uw instellingen Db2 HADR kunt testen. *Elke test wordt ervan uitgegaan dat u bent aangemeld als gebruiker root* en de primaire IBM Db2 wordt uitgevoerd op de *azibmdb01* virtuele machine.

De initiële status van alle testcases wordt hier uitgelegd: (crm_mon - r of crm status)

- **CRM-status** is een momentopname van de status Pacemaker tijdens de uitvoering 
- **crm_mon - r** is continue uitvoer van Pacemaker status

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   Promoting azibmdb01
     Slaves: [ azibmdb02 ]
</code></pre>

De status van de oorspronkelijke in een SAP-systeem wordt beschreven in de transactie DBACOCKPIT > Configuratie > overzicht, zoals wordt weergegeven in de volgende afbeelding:

![DBACockpit - Pre-migratie](./media/dbms-guide-ha-ibm/hadr-sap-mgr-org.png)




### <a name="test-takeover-of-ibm-db2"></a>Test overname van IBM Db2


> [!IMPORTANT] 
> Voordat u de test start, controleert u of:
> * Pacemaker beschikt niet over een mislukte acties (crm-status).
> * Er zijn geen Locatiebeperkingen (restanten van migratie-test)
> * De synchronisatie van IBM Db2 HADR werkt. Neem contact op met de gebruiker db2\<sid > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Migreren van het knooppunt waarop de primaire Db2-database wordt uitgevoerd door het uitvoeren van de volgende opdracht:
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb02</code></pre>

Nadat de migratie is voltooid, wordt de uitvoer van de status van crm lijkt:
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]
</code></pre>

De status van de oorspronkelijke in een SAP-systeem wordt beschreven in de transactie DBACOCKPIT > Configuratie > overzicht, zoals wordt weergegeven in de volgende afbeelding:

![DBACockpit - nadat de migratie](./media/dbms-guide-ha-ibm/hadr-sap-mgr-post.png)

Resourcemigratie met 'crm resource migreren' maakt Locatiebeperkingen. Locatiebeperkingen moeten worden verwijderd. Als de locatie niet worden verwijderd, de bron kunt geen failback of ongewenste bedrijfsovername kunnen optreden. 

Migreren van de resource naar *azibmdb01* en schakelt u de Locatiebeperkingen
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb01
crm resource clear msl_<b>Db2_db2ptr_PTR</b>
</code></pre>

- **migreren van CRM-resource \<res_name > <host>:** Locatie maakt en kan problemen veroorzaken met overnemen
- **CRM-resource wissen \<res_name >** : Hiermee worden gewist Locatiebeperkingen
- **opruimen van de resource CRM \<res_name >** : Hiermee wist alle fouten van de resource

### <a name="test-the-fencing-agent"></a>Testen van de agent voor de eerste optie

In dit geval testen we SBD de eerste optie, dit is raadzaam dat u uitvoert voor SUSE Linux.

<pre><code>
azibmdb01:~ # ps -ef|grep sbd
root       2374      1  0 Feb05 ?        00:00:17 sbd: inquisitor
root       2378   2374  0 Feb05 ?        00:00:40 sbd: watcher: /dev/disk/by-id/scsi-36001405fbbaab35ee77412dacb77ae36 - slot: 0 - uuid: 27cad13a-0bce-4115-891f-43b22cfabe65
root       2379   2374  0 Feb05 ?        00:01:51 sbd: watcher: Pacemaker
root       2380   2374  0 Feb05 ?        00:00:18 sbd: watcher: Cluster

azibmdb01:~ # kill -9 2374
</code></pre>

Clusterknooppunt *azibmdb01* moet opnieuw worden opgestart. De IBM Db2 primaire HADR rol gaat worden verplaatst naar *azibmdb02*. Wanneer *azibmdb01* is weer online, de Db2 exemplaar wilt verplaatsen in de rol van een secundaire database-exemplaar. 

Als de service Pacemaker niet automatisch wordt gestart op de primaire opnieuw opgestart voormalige, moet u start de service handmatig met:

<code><pre>sudo service pacemaker start</code></pre>

### <a name="test-a-manual-takeover"></a>Een handmatige overname testen

U kunt een handmatige overname testen door het stoppen van de service Pacemaker op *azibmdb01* knooppunt:
<pre><code>service pacemaker stop</code></pre>

status van *azibmdb02*
<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]
</code></pre>

Na de failover, u kunt de service opnieuw starten op *azibmdb01*.
<pre><code>service pacemaker start</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-hadr-primary-database"></a>De Db2-proces op het knooppunt waarop de primaire database HADR beëindigen

<pre><code>#Kill main db2 process - db2sysc
azibmdb01:~ # ps -ef|grep db2s
db2ptr    34598  34596  8 14:21 ?        00:00:07 db2sysc 0

azibmdb01:~ # kill -9 34598
</code></pre>

Het exemplaar Db2 gaat mislukken en Pacemaker rapporteert de status te volgen:

<pre><code>
2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Slaves: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms

</code></pre>

Pacemaker wordt opnieuw opgestart het exemplaar van de primaire database Db2 op hetzelfde knooppunt, of deze een failover uitgevoerd naar het knooppunt waarop de secundaire database-instantie wordt uitgevoerd en wordt een fout gerapporteerd.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=157, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:28:19 2019', queued=40ms, exec=223ms
</code></pre>


### <a name="kill-the-db2-process-on-the-node-that-runs-the-secondary-database-instance"></a>Beëindig de Db2-proces op het knooppunt waarop de secundaire database-exemplaar wordt uitgevoerd

<pre><code>azibmdb02:~ # ps -ef|grep db2s
db2ptr    65250  65248  0 Feb11 ?        00:09:27 db2sysc 0

azibmdb02:~ # kill -9</code></pre>

Het knooppunt opgehaald in kan niet wordt vermeld en fout gemeld
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb02
     Masters: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>

De Db2-exemplaar wordt opnieuw gestart in de secundaire rol die deze had toegewezen voordat.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_monitor_30000 on azibmdb02 'not running' (7): call=144, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:36:59 2019', queued=0ms, exec=0ms</code></pre>



### <a name="stop-db-via-db2stop-force-on-the-node-that-runs-the-hadr-primary-database-instance"></a>Stop DB via db2stop afdwingen op het knooppunt waarop de primaire database HADR exemplaar wordt uitgevoerd

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

Als gebruiker db2\<sid > opdracht db2stop force uitvoeren:
<pre><code>azibmdb01:~ # su - db2ptr
azibmdb01:db2ptr> db2stop force</code></pre>

Fout gedetecteerd
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

 stonith-sbd    (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Stopped
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Stopped
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     rsc_Db2_db2ptr_PTR      (ocf::heartbeat:db2):   FAILED azibmdb01
     Slaves: [ azibmdb02 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_demote_0 on azibmdb01 'unknown error' (1): call=201, status=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:25 2019', queued=1ms, exec=150ms</code></pre>

De Db2 HADR secundaire database-instantie is gepromoveerd tot de primaire rol
<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb01
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ]

Failed Actions:
* rsc_Db2_db2ptr_PTR_start_0 on azibmdb01 'unknown error' (1): call=205, stat
us=complete, exitreason='',
    last-rc-change='Tue Feb 12 14:45:27 2019', queued=0ms, exec=865ms</pre></code>


### <a name="crash-vm-with-restart-on-the-node-that-runs-the-hadr-primary-database-instance"></a>Virtuele machine opnieuw op te starten op het knooppunt waarop het exemplaar van de primaire database HADR vastlopen

<pre><code>#Linux kernel panic - with OS restart
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

Pacemaker wordt de secundaire instantie aan de rol van de primaire instantie promoten. De oude primaire instantie wordt verplaatst naar de secundaire rol nadat de virtuele machine en alle services worden volledig hersteld nadat de virtuele machine opnieuw opstarten:

<pre><code> nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>



### <a name="crash-the-vm-that-runs-the-hadr-primary-database-instance-with-halt"></a>Crash van de virtuele machine waarop het exemplaar van de primaire database HADR met "stilstand"

<pre><code>#Linux kernel panic - halts OS
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

In dat geval detecteert Pacemaker dat het knooppunt waarop de primaire database-exemplaar wordt niet meer reageert.

<pre><code>2 nodes configured
5 resources configured

Node azibmdb01: UNCLEAN (online)
Online: [ azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb01
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb01
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb01 ]
     Slaves: [ azibmdb02 ]</code></pre>

de volgende stap is om te controleren op een *splitsen brein* situatie. Nadat het overgebleven knooppunt heeft vastgesteld dat het knooppunt dat de primaire database-exemplaar het laatst is uitgevoerd niet actief is, wordt een failover van de resources wordt uitgevoerd.
<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb02 ]
OFFLINE: [ azibmdb01 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Stopped: [ azibmdb01 ] </code></pre>


In het geval van een 'beëindiging van de aantasting' van het knooppunt is het knooppunt opnieuw worden gestart via Azure Management-hulpprogramma's (in de Azure-portal, PowerShell of Azure CLI). Nadat het knooppunt weer online is, start deze de Db2-exemplaar in de secundaire rol.

<pre><code>2 nodes configured
5 resources configured

Online: [ azibmdb01 azibmdb02 ]

Full list of resources:

stonith-sbd     (stonith:external/sbd): Started azibmdb02
 Resource Group: g_ip_db2ptr_PTR
     rsc_ip_db2ptr_PTR  (ocf::heartbeat:IPaddr2):       Started azibmdb02
     rsc_nc_db2ptr_PTR  (ocf::heartbeat:anything):      Started azibmdb02
 Master/Slave Set: msl_Db2_db2ptr_PTR [rsc_Db2_db2ptr_PTR]
     Masters: [ azibmdb02 ]
     Slaves: [ azibmdb01 ]</code></pre>

## <a name="next-steps"></a>Volgende stappen
- [Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)

     

