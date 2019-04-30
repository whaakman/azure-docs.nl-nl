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
ms.openlocfilehash: 3c1d0e252b5c658ab6da2b3932918f05ba651d52
ms.sourcegitcommit: 3102f886aa962842303c8753fe8fa5324a52834a
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/23/2019
ms.locfileid: "60835946"
---
[1928533]:https://launchpad.support.sap.com/#/notes/1928533
[2015553]:https://launchpad.support.sap.com/#/notes/2015553
[2178632]:https://launchpad.support.sap.com/#/notes/2178632
[2191498]:https://launchpad.support.sap.com/#/notes/2191498
[2243692]:https://launchpad.support.sap.com/#/notes/2243692
[1984787]:https://launchpad.support.sap.com/#/notes/1984787
[1999351]:https://launchpad.support.sap.com/#/notes/1999351
[2233094]:https://launchpad.support.sap.com/#/notes/2233094
[1612105]:https://launchpad.support.sap.com/#/notes/1612105

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

IBM Db2 LUW (Linux-, Unix- en Windows) in [HADR configuratie](https://www.ibm.com/support/knowledgecenter/en/SSEPGG_10.5.0/com.ibm.db2.luw.admin.ha.doc/doc/c0011267.html) bestaan uit één knooppunt waarop een exemplaar van de primaire database en ten minste één knooppunt dat een secundaire database-exemplaar wordt uitgevoerd. Wijzigingen naar de primaire database-instantie ophalen gerepliceerd naar de secundaire database-instantie synchroon of asynchroon, afhankelijk van uw configuratie. 

Dit artikel wordt beschreven hoe u kunt implementeren en configureren van de virtuele machines, installeren van de cluster-framework, en installeert en configureert IBM Db2 LUW in HADR configuratie. Het artikel wordt niet uitgelegd hoe het installeren en configureren van IBM Db2 LUW in HADR of SAP software-installatie. Verwijzingen naar handleidingen van SAP en IBM-installatie worden aan het bereiken van deze taken verstrekt. Er is gericht op onderdelen die specifiek voor Azure-omgeving zijn. 

Ondersteunde IBM Db2-versies zijn 10,5 en hoger zoals beschreven in SAP-notitie #[1928533].

Lees de volgende SAP-opmerkingen en documentatie eerst voordat u een installatie bijna bereikt:

| SAP-notitie | Description |
| --- | --- |
| [1928533] | SAP-toepassingen op Azure: Ondersteunde producten en typen Azure VM's |
| [2015553] | SAP op Microsoft Azure: Vereisten voor ondersteuning |
| [2178632] | Sleutel metrische gegevens controleren voor SAP op Microsoft Azure |
| [2191498] | SAP op Linux met Azure: Uitgebreide bewaking |
| [2243692] | Linux op Microsoft Azure (IaaS) virtuele machine: Problemen met SAP-licentie |
| [1984787] |SUSE LINUX Enterprise Server 12: Opmerkingen bij de installatie |
| [1999351] |Het oplossen van uitgebreide Azure-bewaking voor SAP |
| [2233094] |DB6: SAP-toepassingen op Azure met behulp van IBM Db2 voor Linux, UNIX- en Windows - als u meer informatie |
| [1612105] |DB6: Veelgestelde vragen over herstel na noodgevallen (HADR) voor hoge beschikbaarheid Db2 |


| Documentatie | 
| --- |
| [SAP-Community WIKI](https://wiki.scn.sap.com/wiki/display/HOME/SAPonLinuxNotes) heeft alle van de vereiste SAP-opmerkingen voor Linux |
| [Azure virtuele Machines, planning en implementatie van SAP op Linux] [ planning-guide] handleiding |
| [Azure Virtual Machines-implementatie voor SAP op Linux] [ deployment-guide] (in dit artikel) |
| [Azure virtuele Machines DBMS-implementatie voor SAP op Linux] [ dbms-guide] handleiding |
| [SAP-werkbelasting op Azure controlelijst voor planning en implementatie][azr-sap-plancheck] |
| [SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP3 best practice-richtlijnen][sles-for-sap-bp] |
| [SUSE Linux Enterprise hoge beschikbaarheid extensie 12 SP3][sles-ha-guide] |
| [IBM Db2 Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP][dbms-db2] |
| [IBM Db2 hoge beschikbaarheid-noodherstel 11.1][db2-hadr-11.1] |
| [IBM Db2 hoge beschikbaarheid-noodherstel R 10,5][db2-hadr-10.5] |

## <a name="overview"></a>Overzicht
Voor het bereiken van hoge beschikbaarheid, IBM Db2 LUW met HADR is geïnstalleerd op ten minste twee virtuele machines van Azure, die zijn geïmplementeerd in een [Azure-beschikbaarheidsset](https://docs.microsoft.com/azure/virtual-machines/windows/tutorial-availability-sets) of over meerdere [Azure Availability Zones](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-ha-availability-zones). De onderstaande afbeeldingen ziet u een installatie van twee database-server Azure VM's. Zowel Azure-VM's van de database-server hebben hun eigen opslag die is gekoppeld en actief zijn. In HADR heeft één database-instantie in een van de Azure VM's de rol van het primaire exemplaar. Alle clients zijn verbonden met dit primaire exemplaar. Alle wijzigingen in de databasetransacties worden lokaal opgeslagen in het transactielogboek Db2. Als de transactie logboekrecords lokaal opgeslagen worden, worden de records overgebracht via TCP/IP op de database-instantie op de tweede database-server, de stand-by-server of de stand-by-exemplaar. De standby-instantie werkt de lokale database door te draaien naar voren de overgebrachte transactie records in logboek registreren. De stand-by-server wordt dus bewaard gesynchroniseerd met de primaire server.

HADR is alleen een replicatiefunctionaliteit. Er is geen foutdetectie en geen automatische failover voor een overname of faciliteiten. Een overname of overdracht naar de stand-by moet handmatig worden gestart door een databasebeheerder. Als u wilt een automatische overnemen en foutdetectie bereiken, kunt u Linux Pacemaker voorziening clustering. Pacemaker bewaakt de twee database-servers/exemplaren. Wanneer de primaire database-server/exemplaar vastloopt, Pacemaker initieert een **automatische** HADR overname door de stand-by-server en ook zorgt ervoor dat het virtuele IP-adres is toegewezen aan de nieuwe primaire server.

![Overzicht van IBM Db2 hoge beschikbaarheid](./media/dbms-guide-ha-ibm/ha-db2-hadr-lb.png)

Voor SAP moeten-toepassingsservers verbinding maakt met primaire database u een virtuele-hostnaam en een virtueel IP-adres. In geval van een failover, wordt de SAP-toepassingsservers verbinding maken met de nieuwe primaire database-instantie. In een Azure-omgeving, een [Azure Load Balancer](https://microsoft.sharepoint.com/teams/WAG/AzureNetworking/Wiki/Load%20Balancing.aspx) is vereist voor het gebruik van een virtueel IP-adres in de manier die vereist zijn voor HADR van IBM Db2. 

De volgende afbeelding geeft om volledig te begrijpen, hoe een maximaal beschikbare systeeminstellingen voor SAP, IBM Db2 LUW met HADR en Pacemaker past een overzicht van een maximaal beschikbare installatie van een SAP-systeem op basis van IBM Db2-database. In dit artikel behandelt alleen IBM Db2 en verwijzingen naar andere artikelen over het instellen van andere onderdelen van SAP-systeem.

![IBM DB2 HA volledig overzicht van de omgeving](.//media/dbms-guide-ha-ibm/end-2-end-ha.png)


### <a name="high-level-overview-of-steps-needed"></a>Overzicht op hoog niveau van de stappen die nodig zijn
Als u wilt implementeren in een IBM Db2-configuratie, moeten deze stappen worden behandeld:

  + Omgeving plannen
  + Virtuele machines implementeren
  + Bestandssystemen, configureren en bijwerken van SUSE Linux
  + Installeren en configureren van Pacemaker
  + Installeer [maximaal beschikbare NFS][nfs-ha]
  + Installeer [ASCS/INGEN op afzonderlijke cluster][ascs-ha] 
  + IBM Db2-database met de optie voor gedistribueerde/hoge beschikbaarheid (SWPM) installeren
  + Secundaire database-knooppunt en instantie installeren/maken en configureren van HADR
  + Controleer of de HADR werkt
  + Pacemaker configuratie toepassen om te bepalen, IBM Db2
  + Azure Load Balancer configureren 
  + Primaire + dialoogvenster installeren toepassingsservers
  + Configuratie voor SAP-toepassingsservers selectievakje/aanpassen
  + Failover uitvoeren / overname test



## <a name="planning-azure-infrastructure-for-hosting-ibm-db2-luw-with-hadr"></a>Azure-infrastructuur voor het hosten van IBM Db2 LUW met HADR plannen

Ga via de plannen voordat u de implementatie uitvoert. Dit is het bouwen van de basis voor het implementeren van een configuratie van Db2 met HADR in Azure. Belangrijkste elementen die deel van de planning voor IMB Db2 LUW (database onderdeel van de SAP-omgeving).

| Onderwerp | Korte beschrijving |
| --- | --- |
| Azure-resourcegroepen bevinden definiëren | Resourcegroepen bevinden waarin u de virtuele machine, VNet, Azure Load Balancer en andere resources implementeren. Kan worden bestaande of nieuwe |
| Virtueel netwerk / subnetdefinitie | Waar zijn virtuele machines voor IBM Db2 en Azure Load Balancer ophalen om te worden geïmplementeerd. Bestaande of nieuwe |
| Virtuele machines die als host fungeert voor IBM Db2 LUW | VM-grootte, opslag, netwerken, IP-adres |
| Virtuele hostnaam & virtueel IP-adres voor IBM Db2-database| Virtuele IP/host-naam die wordt gebruikt voor verbinding met SAP-toepassingsservers. **db-virt-hostname**, **db-virt-ip** |
| De eerste optie Azure | De eerste optie Azure of SBD de eerste optie (ten zeerste aanbevolen). Methode om te voorkomen dat splitsen brein situaties wordt voorkomen |
| SBD VM | De grootte van SBD virtuele machine, opslag, netwerk |
| Azure Load Balancer | Verbruik van Basic of Standard (aanbevolen), testpoort voor Db2-database (onze aanbeveling 62500) **test-poort** |
| Naamomzetting| Hoe de naamomzetting werkt in de omgeving. DNS-service wordt sterk aanbevolen. Lokale hosts-bestand kan worden gebruikt |
    
Meer informatie over het gebruik van Linux Pacemaker in Azure vindt u in deze artikelen:

- [Pacemaker op SUSE Linux Enterprise Server in Azure instellen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)



## <a name="deployment-on-suse-linux"></a>Implementatie in SUSE Linux

De resource-agent voor IBM Db2 LUW is opgenomen in SUSE Linux Enterprise Server voor SAP-toepassingen. Voor de instellingen in dit document beschreven, is het verplichte gebruik van de SUSE Linux-Server voor SAP-toepassingen. De Azure Marketplace bevat een afbeelding voor SUSE Enterprise Server voor 12 van de SAP-toepassingen die u gebruiken kunt voor het implementeren van nieuwe virtuele machines van Azure. Houd rekening met de verschillende support/service-modellen door SUSE aangeboden via de Azure galerieën bij het kiezen van de VM-installatiekopie in de galerie met virtuele Azure-machine. 

### <a name="hosts---dns-updates"></a>Hosts - DNS-updates
Maak een lijst van alle hostnamen, met inbegrip van virtuele hostnamen en uw DNS-servers, zodat de juiste IP-adres hostnaamomzetting bijwerken. In het geval is, een DNS-server bestaat niet of u bent niet kan bijwerken en DNS-vermeldingen te maken, moet u gebruikmaken van het lokale hosts-bestanden van de afzonderlijke virtuele machines die deelnemen aan in dit scenario. In het geval van de host-vermeldingen voor bestanden gebruikt, moet u om ervoor te zorgen dat de vermeldingen worden toegepast op alle virtuele machines in de systeemomgeving SAP. Echter, aanbeveling is het gebruik van uw DNS die in het ideale geval wordt uitgebreid naar Azure


### <a name="manual-deployment"></a>Handmatige implementatie

Zorg ervoor dat het geselecteerde besturingssysteem wordt ondersteund door IBM/SAP voor IBM Db2 LUW. De lijst met ondersteunde versies van het besturingssysteem voor virtuele Azure-machines en Db2 releases is beschikbaar in de SAP-notitie [1928533]. De lijst met OS releases door afzonderlijke Db2 releases is beschikbaar in de Matrix beschikbaarheid van SAP-Product. We raden u aan een minimum van SLES 12 SP3 vanwege Azure gerelateerde prestatieverbeteringen in deze of hoger SUSE Linux-versies.

1. Een resourcegroep maken/selecteren
2. Een virtueel netwerk en subnet maken/selecteren
3. Azure-beschikbaarheidsset maken of implementeren in een Beschikbaarheidszone
    + Beschikbaarheid instellen - max updatedomeinen ingesteld op twee
4. Virtuele Machine 1 maken.
    + SLES voor SAP-installatiekopie in Azure-galerie gebruiken
    + Selecteer Azure availability set in gemaakt in stap 3 of schakel binnen een Beschikbaarheidszone
5.  Maak virtuele Machine 2.
    + SLES voor SAP-installatiekopie in Azure-galerie gebruiken
    + Selecteer Azure beschikbaarheidsset in worden gemaakt in stap 3. of selecteer binnen een Beschikbaarheidszone - niet dezelfde Zone zoals in stap 3.
6. Gegevensschijven toevoegen aan de virtuele machines: Controleer de aanbeveling van de bestandssysteem-instellingen in het artikel [IBM Db2 Azure virtuele Machines DBMS-implementatie voor de werkbelasting van SAP][dbms-db2]

## <a name="create-the-pacemaker-cluster"></a>Maken van het cluster Pacemaker
    
Volg de stappen in [Pacemaker op SUSE Linux Enterprise Server in Azure instellen][sles-pacemaker] om een eenvoudige Pacemaker-cluster voor deze IBM Db2-server te maken. 

## <a name="install-ibm-db2-luw-and-sap-environment"></a>IBM Db2 LUW en SAP-omgeving installeren

Voordat u de installatie van een SAP-omgeving op basis van IBM Db2 LUW begint, controleert u (koppelingen aan het begin van het artikel):

+ Documentatie voor Azure
+ SAP-documentatie
+ IBM-documentatie

Controleer de installatie-handleidingen van SAP op NetWeaver-toepassingen installeren op IBM Db2 LUW.

U vindt de handleidingen SAP Help portal met behulp van de [SAP-installatie handleiding Finder][sap-instfind]

De zoekopdracht zo dat aantal handleidingen die beschikbaar zijn bij het instellen van de filters reduceren, kunt u filteren:

+ Ik wil: "Een nieuw systeem installeren"
+ Mijn Database: "IBM Db2 voor Linux, Unix- en Windows"
+ Meer filters voor SAP Netweaver-versies, stack-configuratie of besturingssysteem.

### <a name="installation-hints-for-setting-up-ibm-db2-luw-with-hadr"></a>Tips voor het instellen van IBM Db2 LUW met HADR installatie

De primaire LUW van IBM Db2-database-instantie instellen:

- Hoge beschikbaarheid of gedistribueerde optie gebruiken
- SAP ASCS/INGEN en Database-exemplaar installeren
- Maak een back-up van de geïnstalleerde database


> [!IMPORTANT] 
> Noteer de 'Database communicatiepoort' ingesteld tijdens de installatie. Dit moet hetzelfde poortnummer voor beide exemplaren van de database

Voor het instellen van de stand-by-database-server met behulp van SAP homogene system kopieerprocedure, voert u deze stappen uit:

  - Gebruik de optie kopiëren System - systemen - gedistribueerd - Database-exemplaar als doel.
  - Als kopieermethode, kies homogene System kopiëren zodat u back-up gebruiken kunt om te herstellen van een back-up op stand-by-server /-exemplaar
  - Wanneer u bij de stap afsluiten om de database voor het kopiëren van homogene systeem te herstellen, sluit u het installatieprogramma. De database herstellen vanuit een back-up van de primaire host. Alle fasen van de volgende installatie zijn al uitgevoerd op de primaire database-server
- HADR instellen voor IBM Db2

> [!NOTE]
> Installatie/configuratie is specifiek voor Azure en Pacemaker. Tijdens de installatieprocedure via SAP Software inrichting Manager, is er nog een expliciete kwestie op hoge beschikbaarheid voor IBM Db2 LUW:
>+ IBM Db2 pureScale niet selecteren
>+ Schakel niet "installeren IBM Tivoli System Automation voor Multiplatforms
>+ Schakel niet 'Genereren cluster configuratie files'

> [!NOTE]
>Wanneer u een apparaat SBD voor Linux Pacemaker, de parameters Db2 HADR instellen
>+ Duur van het venster HADR peer (seconden) (HADR_PEER_WINDOW) = 300  
>+ HADR time-outwaarde (HADR_TIMEOUT) = 60

> [!NOTE]
>Met behulp van Azure Pacemaker de eerste optie agent:
>+ Duur van het venster HADR peer (seconden) (HADR_PEER_WINDOW) = 900  
>+ HADR time-outwaarde (HADR_TIMEOUT) = 60

Parameters worden aanbevolen op basis van het eerste testen van failover/overnemen. Het is verplicht om juiste functionaliteit overnemen en failover met deze parameterinstellingen te testen. Aangezien afzonderlijke configuraties verschillen mogelijk, kunnen deze parameters moeten worden aangepast. 

> [!IMPORTANT]
> Specifieke voor IBM Db2 in de configuratie HADR met normaal - exemplaar van de secundaire/stand-by-database moet actief en werkend voordat u zich kunt om primaire database-exemplaar te starten.

Voor demonstratiedoeleinden te gebruiken en de procedures in dit document worden beschreven, de database SID is **PTR**.

##### <a name="ibm-db2-hadr-check"></a>IBM Db2 HADR selectievakje
Wanneer u dit HADR is geconfigureerd en de status is peer-netwerk en verbonden op de primaire en stand-by-knooppunten.

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

Wanneer u Pacemaker voor automatische failover in het geval van storing op een knooppunt gebruikt, moet u de Db2-exemplaren en Pacemaker dienovereenkomstig configureren. In deze sectie wordt beschreven in dit type configuratie.

De volgende items worden voorafgegaan door een:

- **[A]**  : van toepassing op alle knooppunten
- **[1]**  - alleen van toepassing op knooppunt 1 
- **[2]**  - alleen van toepassing op knooppunt 2.

<!-- Yast is a fixed term in Linux and not a spelling error -->

**[A]**  Vereisten voor de configuratie van Pacemaker:
1. Beide databaseserver met gebruiker db2 afsluiten\<sid > met db2stop
2. Wijziging shell-omgeving voor db2\<sid > gebruiker '/ bin/ksh' - aanbevolen Yast-hulpprogramma te gebruiken 


### <a name="pacemaker-configuration"></a>Pacemaker configuratie:

**[1]**  IBM Db2 HADR specifieke Pacemaker configuratie
<pre><code># Put Pacemaker into maintenance mode
sudo crm configure property maintenance-mode=true
</code></pre>

**[1]**  Maken IBM Db2-resources
<pre><code># Replace **bold strings** with your instance name db2sid, database SID and virtual IP address/Azure Load Balancer

sudo crm configure primitive rsc_Db2_db2ptr_<b>PTR</b> db2 \
        params instance="<b>db2ptr</b>" dblist="<b>PTR</b>" \
        op start interval="0" timeout="130" \
        op stop interval="0" timeout="120" \
        op promote interval="0" timeout="120" \
        op demote interval="0" timeout="120" \
        op monitor interval="30" timeout="60" \
        op monitor interval="31" role="Master" timeout="60"

# Configure virutal IP - same as Azure Load Balancer IP
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

**[1]**  Start IBM Db2-resources - Pacemaker uit de onderhoudsmodus plaatsen
<pre><code># Put Pacemaker out of maintenance-mode - that start IBM Db2
sudo crm configure property maintenance-mode=false</pre></code>

**[1]**  Zorg dat de clusterstatus ok is en dat alle resources die worden gestart. Het is niet belangrijk op welk knooppunt de resources die worden uitgevoerd.
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
> Pacemaker geclusterd exemplaar van de Db2 moet worden uitgevoerd via hulpprogramma's voor Pacemaker beheert. Met behulp van de db2-opdrachten (zoals db2stop) zal worden gedetecteerd door Pacemaker als fout van de resource. In het geval van onderhoud, kunt u de knooppunten of resources naar de onderhoudsmodus plaatsen en Pacemaker schorten bewakingsresources en normale db2 beheeropdrachten kunnen worden gebruikt.


### <a name="configure-azure-load-balancer"></a>Azure Load Balancer configureren
Het verdient aanbeveling gebruik van de [Azure Standard Load Balancer-SKU](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview).

1. Configureer de Azure Load Balancer (via Azure portal). Maak eerst een front-end-IP-adresgroep:

   1. In Azure portal, de Azure Load Balancer openen, selecteert u **front-end-IP-adresgroep**, en selecteer **toevoegen**.
   2. Voer de naam van de nieuwe front-end-IP-adresgroep (bijvoorbeeld **Db2-connection**).
   3. Stel de **toewijzing** te **statische** en voer het IP-adres **virtuele IP-** gedefinieerd aan het begin.
   4. Selecteer **OK**.
   5. Nadat de nieuwe front-end-IP-adresgroep is gemaakt, houd er rekening mee dat het IP-adres van toepassingen.

2. Volgende stap is om een back-end-pool te maken:

   1. In Azure portal, de Azure Load Balancer openen, selecteert u **back-endpools**, en selecteer **toevoegen**.
   2. Voer de naam van de nieuwe back-end-pool (bijvoorbeeld **Db2-back-end**).
   3. Selecteer **toevoegen van een virtuele machine**.
   4. Selecteer de beschikbaarheid van een set/virtuele machines die als host fungeert voor IBM Db2-database gemaakt in stap 3.
   5. Selecteer de virtuele machines van de IBM Db2-cluster.
   6. Selecteer **OK**.

3. Derde stap is het maken van een statustest:

   1. In Azure portal, de Azure Load Balancer openen, selecteert u **statuscontroles**, en selecteer **toevoegen**.
   2. Voer de naam van de nieuwe statustest (bijvoorbeeld **Db2-hp**).
   3. Selecteer **TCP** als het protocol en poort **62500**. Houd de **Interval** waarde is ingesteld op 5, en de **drempelwaarde voor onjuiste status** waarde ingesteld op 2.
   4. Selecteer **OK**.

4. De regels voor taakverdeling maken:

   1. In Azure portal, de Azure Load Balancer openen, selecteert u **taakverdelingsregels**, en selecteer **toevoegen**.
   2. Voer de naam van de nieuwe Load Balancer-regel (bijvoorbeeld **Db2-SID**).
   3. Selecteer het front-end-IP-adres, de back-endpool en de statustest die u eerder hebt gemaakt (bijvoorbeeld **Db2-frontend**).
   4. Houd de **Protocol** ingesteld op **TCP**, en voer de poort *Database communicatiepoort*.
   5. Verhoog de **time-out voor inactiviteit** tot 30 minuten.
   6. Zorg ervoor dat u **Floating IP inschakelen**.
   7. Selecteer **OK**.


### <a name="make-changes-to-sap-profiles-to-use-virtual-ip-for-connection"></a>Wijzigingen aanbrengen in SAP-profielen virtueel IP-adres gebruiken voor verbinding
De SAP-toepassingslaag moet het virtuele IP-adres dat u hebt gedefinieerd en geconfigureerd voor de Azure Load Balancer verbinding maken met het primaire exemplaar van de configuratie van de HADR gebruiken. De volgende wijzigingen zijn vereist.

/sapmnt/\<SID>/profile/DEFAULT.PFL
<pre><code>SAPDBHOST = db-virt-hostname
j2ee/dbhost = db-virt-hostname
</code></pre>

/sapmnt/\<SID>/global/db6/db2cli.ini
<pre><code>Hostname=db-virt-hostname
</code></pre>



## <a name="install-primary-and-dialog-application-servers"></a>Installatie van primaire en dialoogvenster toepassing Servers

Wanneer installatie primaire en het dialoogvenster toepassingsservers op basis van de configuratie van een Db2-HADR, moet u virtuele hostnaam die u hebt geselecteerd voor de configuratie. 

Als u de installatie voordat u het maken van de configuratie van de Db2 HADR hebt uitgevoerd, moet u de wijzigingen beschreven in de vorige alinea en als volgt voor SAP-Java-stacks.

### <a name="abapjava-or-java-stack-systems-jdbc-url-check"></a>ABAP + Java of Java-stack-systemen JDBC URL controleren

De J2EE-Config-hulpprogramma gebruiken om te controleren of de URL JDBC bijwerken. De het hulpprogramma J2EE Config grafisch hulpprogramma is, als gevolg hiervan moet u **X server** geïnstalleerd:
 
1. Meld u aan de primaire toepassingsserver van J2EE-exemplaar en uitvoeren:
    <pre><code>sudo /usr/sap/*SID*/*Instance*/j2ee/configtool/configtool.sh</code></pre>
2. Kies in het linkerdeelvenster security-store.
2. Kies in het rechterdeelvenster de belangrijkstejdbc/groep/ <SAPSID> /URL.
2. Wijzig de hostnaam in de JDBC-URL op de virtuele host-naam <pre><code>jdbc:db2://db-virt-hostname:5912/TSP:deferPrepares=0</code></pre>
5. Kies toevoegen.
5. Om uw wijzigingen hebt opgeslagen, klikt u op het pictogram van de schijf in de linkerbovenhoek.
5. Sluit het configuratieprogramma.
5. De Java-exemplaar opnieuw opstarten.

## <a name="configuration-of-log-archiving-for-hadr-setup"></a>Configuratie van het logboek voor de installatie van HADR archiveren
Als u wilt de Db2-logboek archiveren voor HADR setup configureert, is het raadzaam dat u configureert zowel de primaire als de stand-by-database hebben mogelijkheid van automatisch logboekgegevens voor het ophalen van alle logboek archiveren locaties. Zowel de primaire als de stand-by-database moeten kunnen worden opgehaald logboekbestanden van het archief van alle de logboek archiveren locaties op die een van de database exemplaren mogelijk Archiveer de logboekbestanden. 

Het logboek archiveren wordt alleen uitgevoerd door de primaire database. Als u de rollen HADR van de database-servers wijzigen of als er een fout optreedt, is de nieuwe primaire database is verantwoordelijk voor het archiveren van logboekbestanden. Als u een ander logboek archiveren locaties hebt ingesteld, kunnen uw logboeken twee keer worden gearchiveerd en in het geval van een lokaal of extern bijwerken, moet u mogelijk handmatig kopiëren van de gearchiveerde logboeken van de oude primaire server naar de locatie van het actieve logboek van de nieuwe primaire server.

Het is raadzaam om de algemene NFS-share waar logboeken worden geschreven vanaf beide knooppunten te configureren. NFS is maximaal beschikbaar. 

U kunt bestaande maximaal beschikbare NFS gebruikt voor gegevensoverdrachten, profiel-directory gebruiken. Lezen:

- [Hoge beschikbaarheid voor NFS op Azure VM's in SUSE Linux Enterprise Server][nfs-ha] 
- [Hoge beschikbaarheid voor SAP NetWeaver op Azure VM's in SUSE Linux Enterprise Server met Azure NetApp-bestanden voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files) over het gebruik [Azure NetApp Files](https://docs.microsoft.com/azure/azure-netapp-files/azure-netapp-files-introduction) NFS-shares maken


## <a name="test-the-cluster-setup"></a>De cluster-instellingen testen

Deze sectie wordt beschreven hoe u uw instellingen Db2 HADR kunt testen. **Elke test wordt ervan uitgegaan dat u bent aangemeld als gebruiker root** en IBM Db2 primaire wordt uitgevoerd op **azibmdb01** virtuele machine.

De initiële status van alle testcases wordt hier uitgelegd: (crm_mon - r of crm status)

- **CRM-status** is de status van een momentopname Pacemaker tijdens de uitvoering 
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

De status van de oorspronkelijke in een SAP-systeem wordt beschreven in de transactie DBACOCKPIT apparaatconfiguratie-->--> overzicht, zoals:

![DBACockpit - Pre-migratie](./media/dbms-guide-ha-ibm/hadr-sap-mgr-org.png)




### <a name="test-takeover-of-ibm-db2"></a>Test de overname van IBM Db2.


> [!IMPORTANT] 
> Voordat u de test, zorg ervoor dat Pacemaker heeft geen mislukte acties (crm-status), en er geen Locatiebeperkingen (restanten van migratie-test zijn) en de synchronisatie van IBM Db2 HADR werkt. Neem contact op met de gebruiker db2\<sid > <pre><code>db2pd -hadr -db \<DBSID></code></pre>


Migreren van het knooppunt waarop de primaire Db2-database wordt uitgevoerd door het uitvoeren van de volgende opdracht:
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb02</code></pre>

Zodra de migratie is voltooid, wordt de uitvoer van de status van crm lijkt:
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

De status van de oorspronkelijke in een SAP-systeem wordt beschreven in de transactie DBACOCKPIT apparaatconfiguratie-->--> overzicht, zoals: ![DBACockpit - nadat de migratie](./media/dbms-guide-ha-ibm/hadr-sap-mgr-post.png)

Resourcemigratie met 'crm resource migreren' maakt Locatiebeperkingen. Locatiebeperkingen moeten worden verwijderd. Als de locatie niet worden verwijderd, de bron kunt geen failback of ongewenste bedrijfsovername kunnen optreden. 

Migreren naar **azibmdb01** en schakelt u Locatiebeperkingen
<pre><code>crm resource migrate msl_<b>Db2_db2ptr_PTR</b> azibmdb01
crm resource clear msl_<b>Db2_db2ptr_PTR</b>
</code></pre>


- CRM-resource < res_name > migreren <host> - locatie wordt gemaakt en kan problemen veroorzaken met overnemen
- CRM-resource wissen < res_name > - locatie wordt gewist
- opruimen van de resource CRM < res_name - > alle fouten van de resource wordt gewist


### <a name="test-the-fencing-agent"></a>Testen van de agent voor de eerste optie

In dit geval testen we SBD de eerste optie, die wordt aanbevolen voor gebruik met SUSE Linux.

<pre><code>
azibmdb01:~ # ps -ef|grep sbd
root       2374      1  0 Feb05 ?        00:00:17 sbd: inquisitor
root       2378   2374  0 Feb05 ?        00:00:40 sbd: watcher: /dev/disk/by-id/scsi-36001405fbbaab35ee77412dacb77ae36 - slot: 0 - uuid: 27cad13a-0bce-4115-891f-43b22cfabe65
root       2379   2374  0 Feb05 ?        00:01:51 sbd: watcher: Pacemaker
root       2380   2374  0 Feb05 ?        00:00:18 sbd: watcher: Cluster

azibmdb01:~ # kill -9 2374
</code></pre>

Clusterknooppunt **azibmdb01** moet opnieuw worden opgestart. IBM Db2 primaire rol als HADR zullen worden verplaatst naar **azibmdb02**. Wanneer **azibmdb01** is weer online, de Db2 exemplaar wilt verplaatsen in de rol van een secundaire database-exemplaar. 

Voor het geval is dat de service Pacemaker niet automatisch wordt gestart op de primaire opnieuw opgestart voormalige, zorg ervoor dat u start de service handmatig met:

<code><pre>sudo service pacemaker start</code></pre>

### <a name="test-a-manual-takeover"></a>Een handmatige overname testen

U kunt een handmatige overname testen door het stoppen van de service Pacemaker op **azibmdb01** knooppunt:
<pre><code>service pacemaker stop</code></pre>

status van **azibmdb02**
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

Na de failover, u kunt de service opnieuw starten op **azibmdb01**.
<pre><code>service pacemaker start</code></pre>


### <a name="kill-db2-process-on-the-node-running-the-hadr-primary-database"></a>Db2-proces op het knooppunt waarop de primaire database HADR beëindigen

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

Pacemaker gaat het exemplaar van de primaire database Db2 op hetzelfde knooppunt of de failover naar het knooppunt waarop de secundaire database gestart opnieuw starten en er wordt een fout gerapporteerd.

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


### <a name="kill-db2-process-on-node-that-runs-the-secondary-database-instance"></a>Kill Db2-proces op knooppunt waarop de secundaire database-exemplaar wordt uitgevoerd

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

De Db2-exemplaar ophalen opnieuw opgestart in de secundaire rol die deze had toegewezen voordat

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



### <a name="stop-db-via-db2stop-force-on-node-running-the-hadr-primary-database-instance"></a>Stop DB via db2stop op knooppunt met het exemplaar van de primaire database HADR afdwingen

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


### <a name="crash-vm-with-restart-on-node-running-the-hadr-primary-database-instance"></a>Virtuele machine opnieuw op te starten op knooppunt met het exemplaar van de primaire database HADR vastlopen

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



### <a name="crash-the-vm-running-the-hadr-primary-database-instance-with-halt"></a>Crash van de virtuele machine met het exemplaar van de primaire database HADR met "stilstand"

<pre><code>#Linux kernel panic - halts OS
azibmdb01:~ # echo b > /proc/sysrq-trigger</code></pre>

In dat geval detecteert Pacemaker dat het knooppunt waarop de primaire database-instantie niet reageert.

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

de volgende stap is om te controleren op een **splitsen brein** situatie. Zodra het overgebleven knooppunt ervoor is dat het knooppunt waarop de primaire database-instantie laatst hebt uitgevoerd, is niet actief is, zal een failover van de resources worden uitgevoerd
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


In het geval van het knooppunt 'stoppen', is het knooppunt opnieuw worden gestart via hulpprogramma's voor Azure-beheer (Portal PowerShel, Azure CLI,...). Het knooppunt gaat de Db2-exemplaar in de secundaire rol starten zodra deze weer online is.

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
Deze documentatie:

- [Architectuur voor hoge beschikbaarheid en scenario's voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-architecture-scenarios)
- [Het instellen van Pacemaker op SUSE Linux invoeren
- pri
- ()-Server in Azure SE]https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)

     

