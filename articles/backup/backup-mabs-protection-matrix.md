---
title: Wat Azure Backup-Server kan een back-up | Microsoft Docs
description: Dit artikel bevat een lijst van alle werkbelastingen, gegevenstypen en -installaties die v2 voor Azure Backup-Server beveiligt ondersteuningsmatrix.
services: backup
documentation center: 
author: markgalioto
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
keywords: 
ms.date: 05/15/2017
ms.topic: article
ms.author: markgal,masaran
manager: carmonm
ms.openlocfilehash: 3608c310f43ff9f97563d252437d50d962162f47
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="azure-backup-server-protection-matrix"></a>Beveiligingsmatrix voor Azure Backup Server

Dit artikel worden de verschillende servers en werkbelastingen die u met Azure Backup-Server beveiligen kunt. De volgende matrix geeft een lijst wat kan worden beveiligd met Azure Backup-Server v1- en v2.

## <a name="protection-support-matrix"></a>De ondersteuningsmatrix voor beveiliging

|Workload|Versie|Azure Backup-server</br> installation|Azure Backup</br> Server v2|Azure Backup</br> Server v1 |Beveiliging en herstel|
|------------|-----------|--------------------|--------------------------------------------|--------------------------------|---------------------------|
|System Center VMM|VMM-2016<br/>VMM 2012 SP1, R2|Fysieke server<br /><br />Virtuele Hyper-V-machine |J|J|Alle implementatiescenario's: Database|
|Clientcomputers (64-bits en 32-bits)|Windows 10|Fysieke server<br /><br />Virtuele Hyper-V-machine <br /><br />Virtuele VMware-machine|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS zijn. FAT en FAT32 worden niet ondersteund.<br /><br />Volumes moeten ten minste 1 GB. DPM gebruikt de Volume Shadow Copy Service (VSS) momentopname van de gegevens en de momentopname werkt alleen als het volume minstens 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows 8.1|Fysieke server<br /><br />Virtuele Hyper-V-machine |J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS zijn. FAT en FAT32 worden niet ondersteund.<br /><br />Volumes moeten ten minste 1 GB. DPM gebruikt de Volume Shadow Copy Service (VSS) momentopname van de gegevens en de momentopname werkt alleen als het volume minstens 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows 8.1|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows 8|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows 8|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows 7|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows 7|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows Vista met SP2|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows Vista met SP1|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows Vista|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Bestanden<br /><br />Beveiligde volumes moeten NTFS en ten minste 1 GB.|
|Clientcomputers (64-bits en 32-bits)|Windows Vista|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Volume, share, map, bestand, systeemstatus/bare metal), ontdubbelde volumes|
|Servers (32-bits en 64-bits)|Windows Server 2016|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)<br /><br />Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)<br /><br />Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J<br /><br />Niet-Nano server|N|Volume, share, map, bestand, systeemstatus/bare metal), ontdubbelde volumes|
|Servers (32-bits en 64-bits)|Windows Server 2012 R2 - Datacenter en Standard|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J |Volume, share, map, bestand<br /><br />DPM moet worden uitgevoerd op ten minste Windows Server 2012 R2 naar Windows Server 2012 beveiligen ontdubbelde volumes.|
|Servers (32-bits en 64-bits)|Windows Server 2012 R2 - Datacenter en Standard|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Volume, share, map, bestand, systeemstatus/bare metal)<br /><br />DPM moet worden uitgevoerd op Windows Server 2012 of 2012 R2 naar Windows Server 2012 ontdubbelde volumes beveiligen.|
|Servers (32-bits en 64-bits)|Windows Server 2012/2012 met SP1 - Datacenter en Standard|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Volume, share, map, bestand, systeemstatus/bare metal<br /><br />DPM moet worden uitgevoerd op ten minste Windows Server 2012 R2 naar Windows Server 2012 beveiligen ontdubbelde volumes.|
|Servers (32-bits en 64-bits)|Windows Server 2012/2012 met SP1 - Datacenter en Standard|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J|Volume, share, map, bestand<br /><br />DPM moet worden uitgevoerd op ten minste Windows Server 2012 R2 naar Windows Server 2012 beveiligen ontdubbelde volumes.|
|Servers (32-bits en 64-bits)|Windows Server 2012/2012 met SP1 - Datacenter en Standard|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Volume, share, map, bestand, systeemstatus/bare metal<br /><br />DPM moet worden uitgevoerd op ten minste Windows Server 2012 R2 naar Windows Server 2012 beveiligen ontdubbelde volumes.|
|Servers (32-bits en 64-bits)|Windows Server 2008 R2 SP1 - Standard en Enterprise|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J<br /><br />U moet SP1 en installeer [Windows Management Frame 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|J|Volume, share, map, bestand, systeemstatus/bare metal|
|Servers (32-bits en 64-bits)|Windows Server 2008 R2 SP1 - Standard en Enterprise|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J<br /><br />U moet SP1 en installeer [Windows Management Frame 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|J |Volume, share, map, bestand|
|Servers (32-bits en 64-bits)|Windows Server 2008 R2 SP1 - Standard en Enterprise|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J<br /><br />U moet SP1 en installeer [Windows Management Frame 4.0](https://www.microsoft.com/en-us/download/details.aspx?id=40855)|J |Volume, share, map, bestand, systeemstatus/bare metal|
|Servers (32-bits en 64-bits)|Windows Server 2008 R2|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Volume, share, map, bestand, systeemstatus/bare metal|
|Servers (32-bits en 64-bits)|Windows Server 2008 R2|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|N|J|Volume, share, map, bestand|
|Servers (32-bits en 64-bits)|Windows Server 2008 R2|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|N|J|Volume, share, map, bestand, systeemstatus/bare metal|
|Servers (32-bits en 64-bits)|Windows Server 2008|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|N|J|Volume, share, map, bestand, systeemstatus/bare metal|
|Servers (32-bits en 64-bits)|Windows Server 2008|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |Volume, share, map, bestand, systeemstatus/bare metal|
|Servers (32-bits en 64-bits)|Windows Storage Server 2008|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Volume, share, map, bestand, systeemstatus/bare metal|
|SQL Server|SQL Server 2016|Fysieke server <br /><br /> On-premises Hyper-V virtuele machine <br /> <br /> virtuele Azure-machine <br /><br /> Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J |N|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2014|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J |Alle implementatiescenario's: database|
|SQL Server|SQL Server 2014|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2012 met SP2|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J |Alle implementatiescenario's: database|
|SQL Server|SQL Server 2012 met SP2|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2012 met SP2|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2012, SQL Server 2012 met SP1|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2012, SQL Server 2012 met SP1|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2012, SQL Server 2012 met SP1|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2008 R2|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2008 R2|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2008 R2|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |Alle implementatiescenario's: database|
|SQL Server|SQL Server 2008|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2008|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J|Alle implementatiescenario's: database|
|SQL Server|SQL Server 2008|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|Alle implementatiescenario's: database|
|Exchange|Exchange 2016|Fysieke server<br/><br/> On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) beveiligen: zelfstandige Exchange server, database onder een beschikbaarheidsgroep database (DAG)<br /><br />(Alle implementatiescenario's) herstellen: Postvak, postvakdatabases onder een DAG|
|Exchange|Exchange 2016|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|(Alle implementatiescenario's) beveiligen: zelfstandige Exchange server, database onder een beschikbaarheidsgroep database (DAG)<br /><br />(Alle implementatiescenario's) herstellen: Postvak, postvakdatabases onder een DAG|
|Exchange|Exchange 2013|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) beveiligen: zelfstandige Exchange server, database onder een beschikbaarheidsgroep database (DAG)<br /><br />(Alle implementatiescenario's) herstellen: Postvak, postvakdatabases onder een DAG|
|Exchange|Exchange 2013|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |(Alle implementatiescenario's) beveiligen: zelfstandige Exchange server, database onder een beschikbaarheidsgroep database (DAG)<br /><br />(Alle implementatiescenario's) herstellen: Postvak, postvakdatabases onder een DAG|
|Exchange|Exchange 2010|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) beveiligen: zelfstandige Exchange server, database onder een beschikbaarheidsgroep database (DAG)<br /><br />(Alle implementatiescenario's) herstellen: Postvak, postvakdatabases onder een DAG|
|Exchange|Exchange 2010|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |(Alle implementatiescenario's) beveiligen: zelfstandige Exchange server, database onder een beschikbaarheidsgroep database (DAG)<br /><br />(Alle implementatiescenario's) herstellen: Postvak, postvakdatabases onder een DAG|
|Exchange|Exchange 2007|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) te beschermen: opslaggroep<br /><br />(Alle implementatiescenario's) herstellen: opslaggroep, database, Postvak|
|Exchange|Exchange 2007|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |(Alle implementatiescenario's) te beschermen: opslaggroep<br /><br />(Alle implementatiescenario's) herstellen: opslaggroep, database, Postvak|
|SharePoint|SharePoint 2016|Fysieke server<br /><br />On-premises Hyper-V virtuele machine<br /><br />Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)<br /><br />Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J |N|(Alle implementatiescenario's) te beschermen: Farm, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver<br /><br />Houd er rekening mee dat het beveiligen van een SharePoint-farm die van de SQL Server 2012 AlwaysOn-functie voor de inhoudsdatabases gebruikmaakt wordt niet ondersteund.|
|SharePoint|SharePoint 2013|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) te beschermen: Farm, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver<br /><br />Houd er rekening mee dat het beveiligen van een SharePoint-farm die van de SQL Server 2012 AlwaysOn-functie voor de inhoudsdatabases gebruikmaakt wordt niet ondersteund.|
|SharePoint|SharePoint 2013|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine) - DPM 2012 R2 updatepakket 3 en hoger|J|J|(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver<br /><br />Houd er rekening mee dat het beveiligen van een SharePoint-farm die van de SQL Server 2012 AlwaysOn-functie voor de inhoudsdatabases gebruikmaakt wordt niet ondersteund.|
|SharePoint|SharePoint 2013|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J |(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver<br /><br />Houd er rekening mee dat het beveiligen van een SharePoint-farm die van de SQL Server 2012 AlwaysOn-functie voor de inhoudsdatabases gebruikmaakt wordt niet ondersteund.|
|SharePoint|SharePoint 2010|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver|
|SharePoint|SharePoint 2010|Azure virtuele machine (indien de werkbelasting wordt uitgevoerd als Azure virtuele machine)|J|J |(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver|
|SharePoint|SharePoint 2010|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver|
|SharePoint|SharePoint 2007|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver|
|SharePoint|SharePoint 2007|Windows virtuele machine in VMWare (beveiligt werkbelasting die wordt uitgevoerd in Windows virtuele machine in VMWare)|J|J|(Alle implementatiescenario's) te beschermen: Farm, SharePoint zoeken, inhoud van frontend-webserver<br /><br />(Alle implementatiescenario's) herstellen: Farm, database, webtoepassing, bestand of lijstitem, SharePoint zoeken, frontend-webserver|
|Hyper-V-host - DPM-beveiligingsagent op de Hyper-V-hostserver, cluster- of virtuele machine|Windows Server 2016|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|N|Beveiligen: Hyper-V-computers, gedeelde clustervolumes (CSV's)<br /><br />Herstellen: virtuele machine, herstel op itemniveau van bestanden en map, volumes, virtuele harde schijven|
|Hyper-V-host - DPM-beveiligingsagent op de Hyper-V-hostserver, cluster- of virtuele machine|Windows Server 2012 R2 - Datacenter en Standard|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Beveiligen: Hyper-V-computers, gedeelde clustervolumes (CSV's)<br /><br />Herstellen: virtuele machine, herstel op itemniveau van bestanden en map, volumes, virtuele harde schijven|
|Hyper-V-host - DPM-beveiligingsagent op de Hyper-V-hostserver, cluster- of virtuele machine|WindowsServer 2012 - Datacenter en Standard|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Beveiligen: Hyper-V-computers, gedeelde clustervolumes (CSV's)<br /><br />Herstellen: virtuele machine, herstel op itemniveau van bestanden en map, volumes, virtuele harde schijven|
|Hyper-V-host - DPM-beveiligingsagent op de Hyper-V-hostserver, cluster- of virtuele machine|Windows Server 2008 R2 SP1 - Enterprise en Standard|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|J|J|Beveiligen: Hyper-V-computers, gedeelde clustervolumes (CSV's)<br /><br />Herstellen: virtuele machine, herstel op itemniveau van bestanden en map, volumes, virtuele harde schijven|
|Hyper-V-host - DPM-beveiligingsagent op de Hyper-V-hostserver, cluster- of virtuele machine|Windows Server 2008|Fysieke server<br /><br />On-premises Hyper-V virtuele machine|N|N|Beveiligen: Hyper-V-computers, gedeelde clustervolumes (CSV's)<br /><br />Herstellen: virtuele machine, herstel op itemniveau van bestanden en map, volumes, virtuele harde schijven|
|Virtuele VMware-machines|VMware server 5.5 of 6.0 of 6.5 |On-premises Hyper-V virtuele machine|J|Y (met UR1)|Virtuele VMware-machines op gedeelde clustervolumes (CSV's), NFS en SAN-opslag<br /> Herstel op itemniveau van bestanden en mappen die alleen beschikbaar voor Windows<br /> VMware vApps niet ondersteund|
|Linux|Linux uitvoeren als Hyper-V- of VMware-Gast|On-premises Hyper-V virtuele machine|J|J|Hyper-V moet worden uitgevoerd op Windows Server 2012 R2 of Windows Server 2016. Beveiligen: Volledige virtuele machine<br /><br />Herstellen: Volledige virtuele machine|

## <a name="cluster-support"></a>Clusterondersteuning
Azure Backup-Server kan gegevens beschermen in de volgende geclusterde toepassingen:

-   Bestandsservers

-   SQL Server

-   Hyper-V - als u een Hyper-V-cluster met uitgebreide DPM-beveiliging beveiligt, kunt u secundaire beveiliging voor de beveiligde Hyper-V-werkbelastingen toevoegen.

    Als u Hyper-V op Windows Server 2008 R2 uitvoert, controleert u of de update wordt beschreven in KB [975354](https://support.microsoft.com/en-us/kb/975354).
    Als u Hyper-V in Windows Server 2008 R2 in een clusterconfiguratie uitvoert, zorg ervoor dat u installeert SP2 en KB [971394](https://support.microsoft.com/en-us/kb/971394).

-   Exchange Server - back-upserver van Azure kunt niet-gedeelde schijfclusters beveiligen voor ondersteunde Exchange Server-versies (cluster-continue replicatie) en kan ook Exchange Server is geconfigureerd voor lokale continue replicatie beveiligen.

-   SQL Server - back-upserver van Azure biedt geen ondersteuning voor back-ups van SQL Server-databases die worden gehost op gedeelde clustervolumes (CSV's).

Azure Backup-Server kan cluster werkbelastingen beveiligen die zich bevinden in hetzelfde domein als de DPM-server en in een onderliggend of vertrouwd domein. Als u wilt om gegevensbronnen in niet-vertrouwde domeinen of werkgroepen te beveiligen, gebruikt u verificatie van NTLM of certificaatverificatie voor één server of verificatie via certificaat alleen voor een cluster.
