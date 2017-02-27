---
title: Schijf uitsluiten van beveiliging met Azure Site Recovery | Microsoft-documenten
description: In dit artikel wordt beschreven waarom en hoe u een of meer VM-schijven kunt uitsluiten van replicatie voor VMware-naar-Azure- en Hyper-V-naar-Azure-scenario&quot;s.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 1/24/2017
ms.author: nisoneji
translationtype: Human Translation
ms.sourcegitcommit: af0d66d92ca542f415de779fb638db166ba5f26a
ms.openlocfilehash: 5e0527fb0a41d8892c9e22d6d6d2f252972e69d0


---
#<a name="exclude-disk-from-replication"></a>Schijf uitsluiten van replicatie
In dit artikel wordt beschreven hoe u een of meer schijven kunt uitsluiten van replicatie om het verbruik van de replicatiebandbreedte te optimaliseren of om het verbruik van resources aan de doelzijde met behulp van dergelijke schijven te optimaliseren. De functie wordt ondersteund voor VMware-naar-Azure- en Hyper-V-naar-Azure-scenario's.

##<a name="prerequisites"></a>Vereisten

Standaard worden alle schijven op een virtuele machine gerepliceerd. Als u een schijf wilt uitsluiten van replicatie, moet de Mobility-service handmatig worden geïnstalleerd op de computer voordat u replicatie inschakelt als u repliceert van **VMware naar Azure**


## <a name="why-exclude-disks-from-replication"></a>Waarom is het nodig om schijven uit te sluiten van replicatie?
Schijven uitsluiten van replicatie is vaak nodig, omdat:

1. Het gegevensverloop op de uitgesloten schijf niet belangrijk is of niet hoeft te worden gerepliceerd.

2. Opslag- en netwerkresources kunnen worden opgeslagen door dit verloop niet te repliceren.

##<a name="what-are-the-typical-scenarios"></a>Wat zijn de typische scenario's?
Er zijn enkele specifieke voorbeelden van gegevensverloop die gemakkelijk kunnen worden geïdentificeerd en in aanmerking komen voor uitsluiting. Denk aan alle wisselbestanden, tempdb-schrijfbewerkingen in Microsoft SQL Server enzovoort. Afhankelijk van de workload en het opslagsubsysteem kan het wisselbestand een aanzienlijke hoeveelheid verloop registreren. Het repliceren van deze gegevens van de primaire site naar Azure zou echter resource-intensief zijn. Zo kan de replicatie van een virtuele machine met één virtuele schijf met daarop het besturingssysteem en het wisselbestand worden geoptimaliseerd door:

1. De virtuele schijf te splitsen in twee virtuele schijven – één met het besturingssysteem en één met het wisselbestand
2. De wisselbestandschijf uit te sluiten van replicatie

Op dezelfde wijze kunnen instanties van Microsoft SQL Server met tempdb- en systeemgegevensbestanden op dezelfde schijf worden geoptimaliseerd door:

1. De systeemgegevensbestanden en tempdb-bestanden op twee verschillende schijven te bewaren
2. De tempdb-schijf uit te sluiten van replicatie.

##<a name="how-to-exclude-disk-from-replication"></a>Hoe sluit ik een schijf uit van replicatie?

###<a name="vmware-to-azure"></a>VMware naar Azure
Volg de werkstroom [Replicatie inschakelen](site-recovery-vmware-to-azure.md#enable-replication) in de Azure Site Recovery-portal om een virtuele machine te beveiligen. In stap 4 van de werkstroom 'Replicatie inschakelen' ziet u de kolom **TE REPLICEREN SCHIJF**. Hiermee kunt u een schijf uitsluiten van replicatie. Standaard worden alle schijven geselecteerd. Deselecteer de schijf die u wilt uitsluiten van replicatie en volg de stappen voor het inschakelen van de replicatie. 

![Replicatie inschakelen](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)
    
    
>[!NOTE]
> 
> * U kunt alleen schijven uitsluiten waarop de Mobility-service al is geïnstalleerd. U moet de Mobility-service handmatig installeren. De Mobility-service wordt namelijk alleen geïnstalleerd met behulp van het push-mechanisme, nadat replicatie is ingeschakeld.
> * Alleen standaardschijven kunnen worden uitgesloten van replicatie. U kunt besturingssystemen of dynamische schijven niet uitsluiten.
> * Nadat de replicatie is ingeschakeld, kunt u geen schijven meer toevoegen of verwijderen voor replicatie. Als u een schijf wilt toevoegen of uitsluiten, moet u de beveiliging voor de machine uitschakelen en vervolgens weer inschakelen.
> * Als u een schijf uitsluit die nodig is voor de werking van een toepassing, moet u deze na een failover naar Azure handmatig maken in Azure, zodat de gerepliceerde toepassing kan worden uitgevoerd. U kunt Azure Automation ook integreren in een herstelplan om de schijf tijdens failover van de machine te maken.
> * Virtuele Windows-machines: Voor schijven die u handmatig hebt gemaakt in Azure, wordt geen failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en er twee rechtstreeks in de virtuele Azure-machine maakt, wordt alleen voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd. U kunt geen schijven toevoegen die handmatig zijn gemaakt in de failback of bij het opnieuw beveiligen van on-premises naar Azure.
> * Virtuele Linux-machines: Voor schijven die u handmatig maakt in Azure, wordt een failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en er twee rechtstreeks in Azure maakt, wordt voor alle vijf een failback uitgevoerd. U kunt geen schijven die handmatig zijn gemaakt, uitsluiten van failback.
> 

###<a name="hyper-v-to-azure"></a>Hyper-V naar Azure
Volg de werkstroom [Replicatie inschakelen](site-recovery-hyper-v-site-to-azure.md#step-6-enable-replication) in de Azure Site Recovery-portal om een virtuele machine te beveiligen. In stap 4 van de werkstroom 'Replicatie inschakelen' ziet u de kolom **TE REPLICEREN SCHIJF**. Hiermee kunt u een schijf uitsluiten van replicatie. Standaard worden alle schijven geselecteerd voor replicatie. Deselecteer de schijf die u wilt uitsluiten van replicatie en volg de stappen voor het inschakelen van de replicatie. 

![Replicatie inschakelen](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)
    
>[!NOTE]
> 
> * Alleen standaardschijven kunnen worden uitgesloten van replicatie. De besturingssysteemschijf kan niet worden uitgesloten. Het wordt bovendien niet aanbevolen om dynamische schijven uit te sluiten. ASR kan niet bepalen welke schijf VHD-schijf standaard of dynamisch is binnen de virtuele gastmachine.  Als niet alle afhankelijke volumeschijven worden uitgesloten, verandert een beveiligde dynamische schijf op een failover-VM in een niet-werkende schijf en zijn de gegevens erop niet toegankelijk.    
> * Nadat de replicatie is ingeschakeld, kunt u geen schijven meer toevoegen of verwijderen voor replicatie. Als u een schijf wilt toevoegen of uitsluiten, moet u de beveiliging voor de virtuele machine uitschakelen en vervolgens weer inschakelen.
> * Als u een schijf uitsluit die nodig is voor de werking van een toepassing, moet u deze na een failover naar Azure handmatig maken in Azure, zodat de gerepliceerde toepassing kan worden uitgevoerd. U kunt Azure Automation ook integreren in een herstelplan om de schijf tijdens failover van de machine te maken.
> * Voor schijven die u handmatig hebt gemaakt in Azure, wordt geen failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en er twee rechtstreeks op de virtuele Azure-machine maakt, wordt alleen voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd van Azure naar Hyper-V. U kunt schijven die handmatig zijn gemaakt in de failback of in de omgekeerde replicatie van Hyper-V naar Azure, niet opnemen.
 


##<a name="end-to-end-scenarios-of-exclude-disks"></a>Volledige scenario's voor het uitsluiten van schijven
We leggen het uitsluiten van schijven uit aan de hand van twee scenario's.

1. tempdb-schijf in SQL Server
2. Wisselbestandschijf

###<a name="excluding-the-sql-server-tempdb-disk"></a>De tempdb-schijf in SQL Server uitsluiten
We gaan uit van een virtuele SQL Server-machine met een tempdb die kan worden uitgesloten.

Naam van de virtuele machine: SalesDB Schijven op de virtuele bronmachine:


**Schijfnaam** | **Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1| Disk1 | D:\ | SQL-systeemdatabase en gebruikersdatabase&1;
DB-Disk2 (de schijf is uitgesloten van de beveiliging) | Disk2 | E:\ | Tijdelijke bestanden
DB-Disk3 (de schijf is uitgesloten van de beveiliging) | Disk3 | F:\ | SQL-tempdb-database (mappad (F:\MSSQL\Data\) --> Noteer het mappad voordat de failover wordt uitgevoerd)
DB-Disk4 | Disk4 |G:\ |Gebruikersdatabase&2;

Omdat het gegevensverloop op twee schijven van de virtuele machine van tijdelijke aard is, moet u bij het beveiligen van de virtuele machine SalesDB Disk2 en Disk3 uitsluiten van de replicatie. Deze schijven worden niet gerepliceerd door Azure Site Recovery en komen niet beschikbaar op de failover-VM in Azure.

Schijven op de virtuele Azure-machine na de failover:

**Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | ---
DISK0 |    C:\ | Besturingssysteemschijf
Disk1 |    E:\ | Tijdelijke opslag [Azure voegt deze schijf toe en wijst de eerste beschikbare stationsletter toe]
Disk2 | D:\ | SQL-systeemdatabase en gebruikersdatabase&1;
Disk3 | G:\ | Gebruikersdatabase&2;

Omdat Disk2 en Disk3 zijn uitgesloten van de virtuele machine SalesDB, is E: de eerste stationsletter in de lijst. Azure wijst E: toe aan tijdelijk opslagvolume. De stationsletter is voor alle gerepliceerde schijven hetzelfde.

Disk3, voorheen de SQL-schijf tempdb (tempdb-mappad F:\MSSQL\Data\) en uitgesloten van replicatie, is niet beschikbaar op de failover-VM. Als gevolg hiervan stopt de SQL-service en is het pad F:\MSSQL\Data nodig.

Er zijn twee manieren waarop u dit pad kunt maken.

1. Voeg een nieuwe schijf toe en wijs een tempdb-mappad toe of
2. Gebruik de bestaande tijdelijke opslagschijf voor tempdb-mappad

####<a name="add-a-new-disk"></a>Een nieuwe schijf toevoegen:

1. Noteer het pad van SQL-tempdb.mdf en tempdb.ldf voordat de failover wordt uitgevoerd.
2. Voeg via Azure Portal een nieuwe schijf toe aan de failover-VM met hetzelfde of een groter formaat dan de SQL-tempdb-bronschijf (Disk3).
3. Meld u aan op de virtuele Azure-machine. Initialiseer en formatteer de nieuw toegevoegde schijf via de schijfbeheerconsole (diskmgmt.msc). 
4. Wijs dezelfde stationsletter toe die is gebruikt door de SQL-tempdb-schijf (F:).
5. Maak een tempdb-map op de F:-schijf (F:\MSSQL\Data).
6. Start de SQL-service via de serviceconsole.

####<a name="use-existing-temporary-storage-disk-for-sql-tempdb-folder-path"></a>Gebruik de bestaande tijdelijke opslagschijf voor het tempdb-mappad:

1. Open een opdrachtregelconsole
2. Voer SQL Server in de herstelmodus uit via de opdrachtregelconsole

        Net start MSSQLSERVER /f / T3608

3. Voer de volgende sqlcmd uit om het tempdb-pad in een nieuw pad te wijzigen

        sqlcmd -A -S SalesDB        **Use your SQL DBname**
        USE master;        
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = tempdev, FILENAME = 'E:\MSSQL\tempdata\tempdb.mdf');
        GO        
        ALTER DATABASE tempdb        
        MODIFY FILE (NAME = templog, FILENAME = 'E:\MSSQL\tempdata\templog.ldf');        
        GO


4. Stop de Microsoft SQL Server-service.

        Net stop MSSQLSERVER
5. Start de Microsoft SQL Server-service.

        Net start MSSQLSERVER

Raadpleeg de volgende Azure-richtlijn voor een tijdelijke opslagschijf

* Het gebruik van SSD's in virtuele Azure-machines om SQL Server TempDB en buffergroepuitbreidingen op te slaan
* Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure

###<a name="failback-from-azure-to-on-premises"></a>Failback (van Azure naar on-premises)
Laten we nu kijken welke schijven er worden gerepliceerd als u een failover van Azure naar uw on-premises VMware- of Hyper-V-host uitvoert. Schijven die u handmatig hebt gemaakt in Azure, worden niet gerepliceerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en twee rechtstreeks op de virtuele Azure-machine maakt, wordt alleen voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd. U kunt geen schijven toevoegen die handmatig zijn gemaakt in de failback of bij het opnieuw beveiligen van on-premises naar Azure. Er worden ook geen tijdelijke opslagschijven naar on-premises gerepliceerd.

####<a name="failback-to-original-location-recovery-olr"></a>Failback naar de oorspronkelijke locatie herstellen

Schijfconfiguratie van de virtuele Azure-machine in bovenstaand voorbeeld:

**Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf** 
--- | --- | --- 
DISK0 | C:\ | Besturingssysteemschijf
Disk1 |    E:\ | Tijdelijke opslag [Azure voegt deze schijf toe en wijst de eerste beschikbare stationsletter toe]
Disk2 |    D:\ | SQL-systeemdatabase en gebruikersdatabase&1;
Disk3 |    G:\ | Gebruikersdatabase&2;


####<a name="vmware-to-azure"></a>VMware naar Azure
Wanneer failback naar de oorspronkelijke locatie plaatsvindt, bevat de failback-VM-schijfconfiguratie geen uitgesloten schijven. Dat betekent dat de schijven die zijn uitgesloten van VMware naar Azure, niet beschikbaar zullen zijn op de failback-VM. 

Schijven op de virtuele VMWare-machine (oorspronkelijke locatie) na de geplande failover van Azure naar on-premises:

**Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf** 
--- | --- | --- 
DISK0 | C:\ | Besturingssysteemschijf
Disk1 |    D:\ | SQL-systeemdatabase en gebruikersdatabase&1;
Disk2 |    G:\ | Gebruikersdatabase&2;

####<a name="hyper-v-to-azure"></a>Hyper-V naar Azure
Wanneer failback naar de oorspronkelijke locatie wordt uitgevoerd, blijft de schijfconfiguratie voor de failback-VM hetzelfde als de oorspronkelijke schijfconfiguratie voor Hyper-V. Dat betekent dat de schijven die zijn uitgesloten van de Hyper-V-site naar Azure, beschikbaar zullen zijn op de failback-VM.

Schijven op de virtuele Hyper-V-machine (oorspronkelijke locatie) na de geplande failover van Azure naar on-premises:

**Schijfnaam** | **Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |    C:\ | Besturingssysteemschijf
DB-Disk1 | Disk1 | D:\ | SQL-systeemdatabase en gebruikersdatabase&1;
DB-Disk2 (uitgesloten schijf) | Disk2 | E:\ | Tijdelijke bestanden
DB-Disk3 (uitgesloten schijf) | Disk3 | F:\ | SQL-tempdb-database (mappad (F:\MSSQL\Data\)
DB-Disk4 | Disk4 | G:\ | Gebruikersdatabase&2;


####<a name="exclude-paging-file-disk"></a>Wisselbestandschijf uitsluiten

We gaan uit van een virtuele machine met een wisselbestandschijf die kan worden uitgesloten.
Er zijn twee mogelijke situaties:

####<a name="case-1-pagefile-is-configured-on-the-d-drive"></a>Voorbeeld 1: het wisselbestand is geconfigureerd op de D:-schijf
Schijfconfiguratie:


**Schijfnaam** | **Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1 (de schijf is uitgesloten van de beveiliging) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Wisselbestandinstellingen op de bron-VM:

![Replicatie inschakelen](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)
    

Schijven op de virtuele Azure-machine na een failover van VMware naar Azure of Hyper-V naar Azure:
**Schijfnaam** | **Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1 | Disk1 | D:\ | Tijdelijke opslag – > pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Omdat Disk1 (D:) is uitgesloten, is D: de eerste letter in de lijst. Azure wijst de letter D: toe aan tijdelijk opslagvolume.  Omdat D: beschikbaar is op de virtuele Azure-machine, blijft de wisselbestandinstelling van de virtuele machine ongewijzigd.

Wisselbestandinstellingen op de virtuele Azure-machine:

![Replicatie inschakelen](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

####<a name="case-2-pagefile-file-is-configured-on-any-other-driveother-than-d-drive"></a>Voorbeeld 2: Wisselbestand is geconfigureerd op een ander station (met uitzondering van de D:-schijf)

Schijfconfiguratie van de bron-VM:

**Schijfnaam** | **Gastbesturingssysteem schijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1 (de schijf is uitgesloten van de beveiliging) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Wisselbestandinstellingen op de on-premise VM:

![Replicatie inschakelen](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Schijven op de virtuele Azure-machine na een failover van VMware/Hyper-V naar Azure:

**Schijfnaam**| **Gastbesturingssysteem schijf#**| **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Besturingssysteemschijf
DB-Disk1 | Disk1 | D:\ | Tijdelijke opslag – > pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Aangezien D: de eerste letter in de lijst is, wijst Azure de letter D: toe aan tijdelijk opslagvolume. De stationsletter is voor alle gerepliceerde schijven hetzelfde. Omdat de G:-schijf niet beschikbaar is, zal het systeem de C:-schijf gebruiken voor het wisselbestand.

Wisselbestandinstellingen op de virtuele Azure-machine:

![Replicatie inschakelen](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Volgende stappen
Wanneer uw implementatie actief is, kunt u [hier](site-recovery-failover.md) meer lezen over de verschillende soorten failovers.



<!--HONumber=Feb17_HO3-->


