---
title: Azure Migrate-architectuur | Microsoft Docs
description: Biedt een overzicht van de service Azure Migrate.
author: rayne-wiselman
ms.service: azure-migrate
ms.topic: conceptual
ms.date: 07/01/2019
ms.author: raynew
ms.openlocfilehash: 16fbf8d3523ac2ab36447aa51a93a0bb5a9fad54
ms.sourcegitcommit: 47ce9ac1eb1561810b8e4242c45127f7b4a4aa1a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67811425"
---
# <a name="azure-migrate-architecture-and-processes"></a>Azure Migrate-architectuur en -processen

[Azure Migrate](migrate-overview.md) biedt een hub van hulpprogramma's die u helpen te detecteren, beoordelen en apps, infrastructuur en workloads migreren naar Microsoft Azure. De hub bevat Azure Migrate-hulpprogramma's en aanbiedingen van derden independent software vendor (ISV). 

 In dit artikel bevat een overzicht van de evaluatie en migratiearchitectuur en processen voor Azure Migrate-Server-evaluatie en migratie van Azure-Server migreren.

## <a name="assessment-with-azure-migrate-server-assessment"></a>Evaluatie met Azure Migrate Server-evaluatie

Azure Migrate-Server-evaluatie kunt VMware-VM's en Hyper-V-machines evalueren voor migratie naar Azure. Beoordeling werkt als volgt:

1. **Voorbereiden voor de evaluatie van**: On-premises, instellen van een lichtgewicht Azure Migrate-apparaat on-premises als een VMware-VM of Hyper-V virtuele machine en Registreer het apparaat met Azure Migrate.
2. **Virtuele machines detecteren**: Het apparaat Azure Migrate wordt uitgevoerd voor het detecteren van on-premises VM's. 
    - Er niets hoeft te worden geïnstalleerd op de gedetecteerde virtuele machines.
    - De metagegevens van de virtuele machine bevat informatie over de kernen, geheugen, schijven, schijfgrootte en netwerkadapters.
    - Prestatiegegevens bevat informatie over de CPU en geheugen gebruik, schijf-IOPS, schijfdoorvoer (MBps) en netwerkuitvoer (MBps)
- **Verzamelen en beoordelen van machines**: Wanneer de detectie is voltooid, in de Azure portal die u verzamelt gedetecteerd VM's in groepen die doorgaans bestaan uit virtuele machines die u wilt migreren samen. U kunt een evaluatie uitvoeren op een groep.


## <a name="vmware-assessment"></a>VMware-evaluatie 

Het diagram bevat een overzicht van hoe werkt de evaluatie van VMware-VM met behulp van Azure Migrate-Server-evaluatie.

![Beoordelingsarchitectuur voor VMware](./media/migrate-architecture/sas-architecture-vmware.png)

Het proces is als volgt:

1. **Implementeren van het apparaat Azure Migrate**:

    a. Download de sjabloon (OVA) vanuit de Azure-portal.

    b. Importeren met de vCenter-Server-machine te maken van de virtuele machine.

    c. Verbinding maken met deze virtuele machine, basisinstellingen voor het configureren en registreren bij Azure Migrate.

2. **Detectie van initiëren**:

    a. Verbinding maken met de Collector-app op het apparaat starten detectie uitgevoerd.

    b. Het apparaat verzendt voortdurend metagegevens-en prestatiegegevens van de virtuele machines naar Azure.

    - Het apparaat is altijd verbonden met de service Azure Migrate.
    - Omgevingswijzigingen tijdens de detectie van continu worden vastgelegd. Bijvoorbeeld VM's in het detectiebereik toe te voegen of VM-schijven of NIC's toe te voegen.

3. **Evaluatie van computers**:

    a. Wanneer de detectie is voltooid, in de Azure portal die u verzamelt gedetecteerd VM's in groepen.  Een groep bestaat gewoonlijk uit virtuele machines die u wilt tegelijk migreren.

    b. Voer een evaluatie voor elke groep uit.

4. **Bekijk evaluatie**: 

    a. Nadat de evaluatie is voltooid, kunt u deze in de portal bekijken.

    b. Download de evaluatie in Excel-indeling voor verdere analyse.



### <a name="vmware-ports"></a>VMware-poorten
Azure Migrate maakt gebruik van de volgende verbindingspoorten voor VMware:

**Bron** | **Target** | **Poort** | **Details**
--- | --- | --- | ---
Azure Migrate-apparaat | Azure Migrate-service | Target-TCP 443 | Metagegevens en de prestaties gegevens verzenden naar Azure migreren.
Azure Migrate-apparaat | vCenter Server | Target-TCP 443 | Verbinding maken met vCenter-Server voor metagegevens-en prestatiegegevens. 443 is standaard maar kunnen worden gewijzigd met de vCenter-luistert op een andere poort. 
RDP-client | Azure Migrate-apparaat | Target-TCP3389 | RDP-verbinding voor het detecteren van de trigger van het apparaat.

### <a name="collected-vmware-metadata"></a>Verzamelde metagegevens voor VMware

Het apparaat verzendt metagegevens en gegevens met betrekking tot prestaties van de virtuele machines naar Azure.

**Actie** | **Details**
--- | ---
**Verzamelde metagegevens** | de naam van de vCenter-VM<br/> vCenter-VM-pad (hostcluster/map)<br/> IP- en MAC-adressen<br/> Besturingssysteem<br/> Aantal schijven/kerngeheugens/NIC<br/> Grootte van geheugen en schijfruimte.
**Verzamelde prestatiegegevens** | Gebruik van CPU/geheugen<br/> Per schijfgegevens (lezen/schrijven-doorvoer voor schijf; schijf lees-/ schrijfbewerkingen per seconde)<br/> NIC-gegevens (netwerk in netwerk).<br/><br/> Prestatiegegevens worden verzameld voortdurend nadat het apparaat verbinding met vCenter-Server. Historische gegevens is niet verzameld.

## <a name="hyper-v-assessment"></a>Hyper-V-evaluatie

Het diagram bevat een overzicht van hoe Hyper-V-evaluatie werkt Azure Migrate-Server-evaluatie sing.

![Beoordelingsarchitectuur voor Hyper-V](./media/migrate-architecture/sas-architecture-hyper-v.png)

Het proces is als volgt:

1. **Maken van het apparaat Azure Migrate**:

    a. De virtuele machine in de gecomprimeerde indeling downloaden vanuit de Azure-portal.

    b. Importeren naar een Hyper-V-host.

    c. Verbinding maken met het VM-apparaat. Basisinstellingen voor het configureren en registreren bij Azure Migrate.

2. **Detectie van initiëren**:

    a. Verbinding maken met het apparaat Azure Migrate starten detectie. Er niets hoeft te worden geïnstalleerd op de gedetecteerde virtuele machines.

    b. Het apparaat verzendt voortdurend de gegevens van de metagegevens en prestaties van de virtuele machine naar het Azure Migrate.

    - Het apparaat is altijd verbonden met de service Azure Migrate (met behulp van CIM-sessies).
    - Omgevingswijzigingen tijdens de detectie van continu worden vastgelegd. Bijvoorbeeld VM's in het detectiebereik toe te voegen of VM-schijven of NIC's toe te voegen.


3. **Evaluatie van computers**:

    a. Wanneer de detectie is voltooid, in de Azure portal die u verzamelt gedetecteerd VM's in groepen.  Een groep bestaat gewoonlijk uit virtuele machines die u wilt tegelijk migreren.

    b. Voer een evaluatie voor elke groep uit.

4. **Bekijk evaluatie**:

    a. Nadat de evaluatie is voltooid, kunt u deze in de portal bekijken.

    b. Download de evaluatie in Excel-indeling voor verdere analyse.

### <a name="hyper-v-ports"></a>Hyper-V-poorten

Azure Migrate-service gebruikt de volgende verbindingspoorten voor Hyper-V:

**Bron** | **Target** | **Poort** | **Details**
--- | --- | --- | ---
Azure Migrate-apparaat | Azure Migrate-service | Target-TCP 443 | Metagegevens en de prestaties gegevens verzenden naar Azure migreren.
Azure Migrate-apparaat | Hyper-V-host/cluster | Doel-standaard-WinRM-poorten-HTTP:5985; HTTPS:5986 | Verbinding maken met de host voor de metagegevens-en prestatiegegevens. CIM-sessie die wordt gebruikt voor verbinding
RDP-client | Azure Migrate-apparaat | Target-TCP3389 | RDP-verbinding voor het detecteren van de trigger van het apparaat.

## <a name="collected-hyper-v-vm-metadata"></a>Verzamelde metagegevens van de Hyper-V-VM

Het apparaat verzendt metagegevens en gegevens met betrekking tot prestaties van de virtuele machines naar Azure.


**Actie** | **Details**
--- | ---
**Verzamelde metagegevens** | VM-naam<br/> VM-pad (hostcluster/map)<br/> IP- en MAC-adressen<br/> Besturingssysteem<br/> Aantal schijven/kerngeheugens/NIC<br/> Grootte van geheugen en schijfruimte<br/> Schijf-IOPS, schijfdoorvoer (MBps), netwerk-uitvoer (MBps)
**Verzamelde prestatiegegevens** |  Gebruik van CPU/geheugen<br/> Per schijfgegevens (lezen/schrijven-doorvoer voor schijf; schijf lees-/ schrijfbewerkingen per seconde)<br/> NIC-gegevens (netwerk in netwerk).<br/><br/> Prestatiegegevens worden verzameld voortdurend nadat het apparaat verbinding met de Hyper-V-host. Historische gegevens is niet verzameld.




## <a name="migration-with-azure-migrate-server-migration"></a>Migratie met Azure Migrate-servermigratie

Met behulp van Azure Migrate-servermigratie, kunt u de volgende migreren naar Azure:

- On-premises VMware VM's
- On-premises Hyper-V-machines
- On-premises fysieke servers
- AWS Windows-VM-exemplaren
- GCP Windows VM-exemplaren

Het migratieproces is enigszins, afhankelijk van wat u wilt migreren.


## <a name="migrate-vmware-vms"></a>Virtuele VMware-machines migreren

Azure Migrate servermigratie biedt twee methoden voor on-premises VMware-VM's migreren:

- **Zonder agent replicatie**: Virtuele machines migreren zonder dat u hoeft te installeren op deze.
- **Replicatie op basis van een agent**. Een agent installeren op de virtuele machine voor replicatie.

Hoewel de zonder agent replicatie vanuit het oogpunt van de implementatie eenvoudiger is heeft momenteel een aantal beperkingen. [Meer informatie](server-migrate-overview.md) over deze beperkingen.
 

### <a name="agent-based-migration"></a>Migratie op basis van een agent

Migratie van virtuele VMware-machines op basis van een agent vereist een aantal onderdelen om te worden geïmplementeerd, zoals samengevat in de tabel.

**Onderdeel** | **Vereiste** | **Details**
--- | --- | ---
**Configuratie van server-machine** | Een enkele on-premises VMware-VM die wordt geïmplementeerd vanuit een gedownloade OVF-sjabloon.<br/><br/> De machine wordt uitgevoerd van alle on-premises Site Recovery-onderdelen, waaronder de configuratieserver en processerver. | **Configuratieserver**: Coördineert de communicatie tussen on-premises en Azure, en beheert de gegevensreplicatie.<br/><br/> **Processerver**: standaard geïnstalleerd op de configuratieserver. Deze ontvangt replicatiegegevens; Met caching, compressie en versleuteling, optimaliseert en verzendt dit naar Azure. De processerver ook installeert Azure Site Recovery Mobility Service op VM's, en wordt automatische detectie van on-premises machines uitgevoerd.
**Mobility-service** | De Mobility-Service moet worden geïnstalleerd op elke VMware-VM die u repliceert. | Het is raadzaam dat u de automatische installatie van de processerver toestaan. U kunt ook de service handmatig installeren of gebruiken van een methode geautomatiseerde implementatie, zoals System Center Configuration Manager.





### <a name="agent-based-replication-process"></a>Op basis van een agent replicatieproces

![Replicatieproces](./media/migrate-architecture/v2a-architecture-henry.png)

1. Wanneer u replicatie voor een virtuele machine inschakelt, begint de initiële replicatie naar Azure. 
    - Replicatie is op blokniveau, in de buurt van continue, met behulp van de Mobility-service-agent die wordt uitgevoerd op de virtuele machine.
    - Replicatie-beleidsinstellingen worden toegepast:
        - **RPO-drempelwaarde**. Deze instelling heeft geen invloed op de replicatie. Dit helpt bij de bewaking. Een gebeurtenis geactiveerd en eventueel een e-mailbericht verzonden, als de huidige RPO overschrijdt de drempelwaarde die u opgeeft.
        - **Bewaarperiode voor herstelpunten**. Deze instelling bepaalt hoe ver terug in de tijd die u gaan wilt wanneer er een onderbreking optreedt. Maximum bewaartermijn op premium storage is 24 uur. Op de standard-opslag is dit 72 uur. 
        - **App-consistente momentopnamen**. App-consistente momentopname tilt elke 1 tot 12 uur, afhankelijk van de behoeften van uw app. Momentopnamen zijn standaard Azure blob-momentopnamen. De Mobility-agent die wordt uitgevoerd op een virtuele machine vraagt een VSS-momentopname in overeenstemming met deze instelling en bladwijzers die point-in-time als een toepassing die consistent punt in de stream van replicatie.

2. Verkeer repliceert naar Azure storage-openbare eindpunten via internet.

    - U kunt ook kunt u Azure ExpressRoute met [Microsoft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering).
    - Het repliceren van verkeer via een site-naar-site virtueel particulier netwerk (VPN) van een on-premises site naar Azure wordt niet ondersteund.
3. Nadat de initiële replicatie is voltooid, begint de replicatie van deltaverschillen naar Azure. Bijgehouden wijzigingen voor een virtuele machine worden verzonden naar de processerver.
2. Communicatie gebeurt er als volgt uit:

    - Virtuele machines communiceren met de on-premises configuratieserver op poort 443 voor HTTPS inkomende, voor het replicatiebeheer van.
    - De configuratieserver coördineert de replicatie met Azure via HTTPS-poort 443 uitgaande.
    - Virtuele machines verzenden replicatiegegevens naar de processerver (die wordt uitgevoerd op de server configuration) op HTTPS-poort 9443 inkomende. Deze poort kan worden gewijzigd.
    - De processerver ontvangt replicatiegegevens, optimaliseert en versleutelt deze en verzendt ze naar Azure storage via poort 443 uitgaande.
5. De replicatiegegevens registreert eerste land in een cache-opslagaccount in Azure. Deze logboeken worden verwerkt en de gegevens worden opgeslagen in een door Azure beheerde schijf (de Azure Site Recovery seed-schijf). De herstelpunten worden gemaakt op deze schijf.







## <a name="next-steps"></a>Volgende stappen

- [Bekijk de veelgestelde vragen](resources-faq.md) over Azure Migrate.
- Voor hulp van de community, gaat u naar de [migreren MSDN-forum Azure](https://social.msdn.microsoft.com/Forums/home?forum=AzureMigrate&filter=alltypes&sort=lastpostdesc) of [Stack Overflow](https://stackoverflow.com/search?q=azure+migrate).

