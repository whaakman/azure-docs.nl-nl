---
title: Schijven uitsluiten van beveiliging met Azure Site Recovery | Microsoft Docs
description: In dit artikel wordt beschreven waarom en hoe u een of meer VM-schijven kunt uitsluiten van replicatie voor VMware-naar-Azure- en Hyper-V-naar-Azure-scenario's.
services: site-recovery
documentationcenter: 
author: nsoneji
manager: garavd
editor: 
ms.assetid: 
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 06/05/2017
ms.author: nisoneji
ms.openlocfilehash: fccbe88e3c0c2b2f3e9958f5f2f27adc017e4d03
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="exclude-disks-from-replication"></a>Schijven uitsluiten van replicatie
In dit artikel wordt beschreven hoe u schijven uitsluit van replicatie. Door schijven uit te sluiten, kunt u de verbruikte replicatiebandbreedte optimaliseren of de resources aan de doelzijde waarvan deze schijven gebruikmaken, optimaliseren. De functie wordt ondersteund voor VMware-naar-Azure- en Hyper-V-naar-Azure-scenario's.

## <a name="prerequisites"></a>Vereisten

Standaard worden alle schijven op een machine gerepliceerd. Als u een schijf wilt uitsluiten van replicatie, moet u de Mobility-service handmatig installeren op de machine voordat u replicatie inschakelt als u repliceert van VMware naar Azure.


## <a name="why-exclude-disks-from-replication"></a>Waarom is het nodig om schijven uit te sluiten van replicatie?
Schijven uitsluiten van replicatie is vaak nodig, omdat:

- Het gegevensverloop op de uitgesloten schijf niet belangrijk is of niet hoeft te worden gerepliceerd.

- Opslag- en netwerkresources kunnen worden uitgespaard door dit verloop niet te repliceren.

## <a name="what-are-the-typical-scenarios"></a>Wat zijn de typische scenario's?
Er zijn specifieke voorbeelden van gegevensverloop die in aanmerking komen voor uitsluiting. Mogelijke voorbeelden zijn schrijfbewerkingen naar een wisselbestand (pagefile.sys) en schrijfbewerkingen naar het tempdb-bestand van Microsoft SQL Server. Afhankelijk van de workload en het opslagsubsysteem kan het wisselbestand een aanzienlijke hoeveelheid verloop registreren. Het repliceren van deze gegevens van de primaire site naar Azure zou echter resource-intensief zijn. U kunt de volgende stappen volgen om de replicatie te optimaliseren van een virtuele machine met één virtuele schijf waarop zowel het besturingssysteem als het wisselbestand staat:

1. Splits de virtuele schijf in twee virtuele schijven. De ene virtuele schijf bevat het besturingssysteem en de andere het wisselbestand.
2. Sluit de wisselbestandschijf uit van replicatie.

Op dezelfde wijze kunt u met de volgende stappen een schijf optimaliseren waarop zowel het tempdb-bestand van Microsoft SQL Server als het systeemdatabasebestand staat:

1. Zet het systeemdatabasebestand en het tempdb-bestand op twee verschillende schijven.
2. Sluit de tempdb-schijf uit van replicatie.

## <a name="how-to-exclude-disks-from-replication"></a>Hoe sluit ik schijven uit van replicatie?

### <a name="vmware-to-azure"></a>VMware naar Azure
Volg de werkstroom [Replicatie inschakelen](site-recovery-vmware-to-azure.md) om een virtuele machine te beveiligen vanuit de Azure Site Recovery-portal. Gebruik in de vierde stap van de werkstroom de kolom **TE REPLICEREN SCHIJF** om schijven uit te sluiten van replicatie. Standaard worden alle schijven geselecteerd voor replicatie. Schakel de selectievakjes uit bij schijven die u wilt uitsluiten van replicatie en volg daarna de stappen voor het inschakelen van de replicatie.

![Schijven uitsluiten van replicatie en replicatie inschakelen voor VMware-naar-Azure-failback](./media/site-recovery-exclude-disk/v2a-enable-replication-exclude-disk1.png)


>[!NOTE]
>
> * U kunt alleen schijven uitsluiten waarop de Mobility-service al is geïnstalleerd. U moet de Mobility-service handmatig installeren. De Mobility-service wordt namelijk alleen geïnstalleerd met behulp van het push-mechanisme, nadat replicatie is ingeschakeld.
> * Alleen standaardschijven kunnen worden uitgesloten van replicatie. Besturingssystemen en dynamische schijven kunt u niet uitsluiten.
> * Nadat u de replicatie hebt ingeschakeld, kunt u geen schijven meer toevoegen of verwijderen voor replicatie. Als u een schijf wilt toevoegen of uitsluiten, moet u de beveiliging voor de machine uitschakelen en vervolgens weer inschakelen.
> * Als u een schijf uitsluit die nodig is voor de werking van een toepassing, moet u deze na een failover naar Azure handmatig maken in Azure, zodat de gerepliceerde toepassing kan worden uitgevoerd. U kunt ook Azure Automation integreren in een herstelplan om de schijf te maken tijdens failover van de machine.
> * Virtuele Windows-machines: voor schijven die u handmatig hebt gemaakt in Azure, wordt geen failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en er twee rechtstreeks in virtuele Azure-machines maakt, wordt alleen voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd. U kunt geen schijven toevoegen die handmatig zijn gemaakt in de failback of bij het opnieuw beveiligen van on-premises naar Azure.
> * Virtuele Linux-machines: voor schijven die u handmatig hebt gemaakt in Azure, wordt een failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en er twee rechtstreeks in virtuele Azure-machines maakt, wordt voor alle vijf een failback uitgevoerd. U kunt geen schijven die handmatig zijn gemaakt, uitsluiten van failback.
>

### <a name="hyper-v-to-azure"></a>Hyper-V naar Azure
Volg de werkstroom [Replicatie inschakelen](site-recovery-hyper-v-site-to-azure.md) om een virtuele machine te beveiligen vanuit de Azure Site Recovery-portal. Gebruik in de vierde stap van de werkstroom de kolom **TE REPLICEREN SCHIJF** om schijven uit te sluiten van replicatie. Standaard worden alle schijven geselecteerd voor replicatie. Schakel de selectievakjes uit bij schijven die u wilt uitsluiten van replicatie en volg daarna de stappen voor het inschakelen van de replicatie.

![Schijven uitsluiten van replicatie en replicatie inschakelen voor Hyper-V-naar-Azure-failback](./media/site-recovery-vmm-to-azure/enable-replication6-with-exclude-disk.png)

>[!NOTE]
>
> * U kunt alleen standaardschijven uitsluiten van replicatie. Besturingssysteemschijven kunt u niet uitsluiten. We raden u aan geen dynamische schijven uit te sluiten. Azure Site Recovery kan niet identificeren welke virtuele harde schijf (VHD) standaard of dynamisch is in de virtuele gastmachine.  Als niet alle afhankelijke, dynamische volumeschijven worden uitgesloten, verandert de beveiligde dynamische schijf op een virtuele failover-machine in een niet-werkende schijf en zijn de gegevens op de schijf niet toegankelijk.
> * Nadat u de replicatie hebt ingeschakeld, kunt u geen schijven meer toevoegen of verwijderen voor replicatie. Als u een schijf wilt toevoegen of uitsluiten, moet u de beveiliging voor de virtuele machine uitschakelen en vervolgens weer inschakelen.
> * Als u een schijf uitsluit die nodig is voor de werking van een toepassing, moet u deze na een failover naar Azure handmatig maken in Azure, zodat de gerepliceerde toepassing kan worden uitgevoerd. U kunt ook Azure Automation integreren in een herstelplan om de schijf te maken tijdens failover van de machine.
> * Voor schijven die u handmatig hebt gemaakt in Azure, wordt geen failback uitgevoerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en u er twee rechtstreeks op virtuele Azure-machines maakt, wordt slechts voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd van Azure naar Hyper-V. U kunt schijven die handmatig zijn gemaakt, niet opnemen in de failback of in de omgekeerde replicatie van Hyper-V naar Azure.



## <a name="end-to-end-scenarios-of-exclude-disks"></a>End-to-end scenario's voor het uitsluiten van schijven
We leggen het uitsluiten van schijven uit aan de hand van twee scenario's:

- tempdb-schijf in SQL Server
- Schijf van het wisselbestand (pagefile.sys)

### <a name="exclude-the-sql-server-tempdb-disk"></a>De tempdb-schijf in SQL Server uitsluiten
We gaan uit van een virtuele SQL Server-machine met een tempdb die kan worden uitgesloten.

De naam van de virtuele schijf is SalesDB.

De schijven op de virtuele bronmachine zijn de volgende:


**Schijfnaam** | **Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1| Disk1 | D:\ | SQL-systeemdatabase en gebruikersdatabase1
DB-Disk2 (de schijf is uitgesloten van beveiliging) | Disk2 | E:\ | Tijdelijke bestanden
DB-Disk3 (de schijf is uitgesloten van beveiliging) | Disk3 | F:\ | SQL-tempdb-database (mappad (F:\MSSQL\Data\) </br /> </br />Noteer het mappad voordat de failover wordt uitgevoerd.
DB-Disk4 | Disk4 |G:\ |Gebruikersdatabase2

Omdat het gegevensverloop op twee schijven van de virtuele machine tijdelijk is, sluit u Disk2 en Disk3 uit van replicatie terwijl u de virtuele SalesDB-machine beveiligt. Azure Site Recovery repliceert deze schijven niet. Bij een failover zijn deze schijven niet aanwezig op de virtuele failover-machine in Azure.

De schijven op de virtuele Azure-machine na de failover zijn de volgende:

**Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | ---
DISK0 | C:\ | Besturingssysteemschijf
Disk1 | E:\ | Tijdelijke opslag</br /> </br />Azure voegt deze schijf toe en wijst de eerste beschikbare stationsletter toe.
Disk2 | D:\ | SQL-systeemdatabase en gebruikersdatabase1
Disk3 | G:\ | Gebruikersdatabase2

Omdat Disk2 en Disk3 zijn uitgesloten van de virtuele SalesDB-machine, is E: de eerste stationsletter in de lijst. Azure wijst E: toe aan het tijdelijke opslagvolume. De stationsletter is voor alle gerepliceerde schijven hetzelfde.

Disk3, de SQL-tempdb-schijf (tempdb-mappad F:\MSSQL\Data\)) is uitgesloten van replicatie. De schijf is niet beschikbaar op de virtuele failover-machine. Als gevolg hiervan stopt de SQL-service en is het pad F:\MSSQL\Data nodig.

Er zijn twee manieren om dit pad maken:

- Voeg een nieuwe schijf toe en wijs een tempdb-mappad toe.
- Gebruik een bestaande tijdelijke opslagschijf voor het tempdb-mappad.

#### <a name="add-a-new-disk"></a>Een nieuwe schijf toevoegen:

1. Noteer de paden van SQL-tempdb.mdf en tempdb.ldf voordat de failover wordt uitgevoerd.
2. Voeg via Azure Portal een nieuwe schijf toe aan de virtuele failover-machine met hetzelfde of een groter formaat dan de SQL-tempdb-bronschijf (Disk3).
3. Meld u aan bij de virtuele Azure-machine. Initialiseer en formatteer de nieuw toegevoegde schijf via de schijfbeheerconsole (diskmgmt.msc). 
4. Wijs dezelfde stationsletter toe die is gebruikt door de SQL-tempdb-schijf (F:).
5. Maak een tempdb-map op de F:-schijf (F:\MSSQL\Data).
6. Start de SQL-service via de serviceconsole.

#### <a name="use-an-existing-temporary-storage-disk-for-the-sql-tempdb-folder-path"></a>Gebruik de bestaande tijdelijke opslagschijf voor het SQL-tempdb-mappad:

1. Open een opdrachtprompt.
2. Voer SQL Server in de herstelmodus uit via de opdrachtprompt.

        Net start MSSQLSERVER /f / T3608

3. Voer de volgende sqlcmd uit om het tempdb-pad in een nieuw pad te wijzigen.

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

Raadpleeg de volgende Azure-richtlijn voor een tijdelijke opslagschijf:

* [SSD's in Azure-VM's gebruiken om SQL Server TempDB en buffergroepuitbreidingen op te slaan](https://blogs.technet.microsoft.com/dataplatforminsider/2014/09/25/using-ssds-in-azure-vms-to-store-sql-server-tempdb-and-buffer-pool-extensions/)
* [Aanbevolen procedures voor prestaties voor SQL Server op virtuele machines van Azure](https://docs.microsoft.com/azure/virtual-machines/windows/sql/virtual-machines-windows-sql-performance)

### <a name="failback-from-azure-to-an-on-premises-host"></a>Failback (van Azure naar een on-premises host)
Laten we nu kijken welke schijven er worden gerepliceerd als u een failover van Azure naar uw on-premises VMware- of Hyper-V-host uitvoert. Schijven die u handmatig hebt gemaakt in Azure, worden niet gerepliceerd. Als u bijvoorbeeld een failover hebt uitgevoerd voor drie schijven en twee rechtstreeks op virtuele Azure-machines maakt, wordt alleen voor de drie schijven waarvoor een failover is uitgevoerd, een failback uitgevoerd. U kunt geen schijven toevoegen die handmatig zijn gemaakt in de failback of bij het opnieuw beveiligen van on-premises naar Azure. Er worden ook geen tijdelijke opslagschijven naar on-premises hosts gerepliceerd.

#### <a name="failback-to-original-location-recovery"></a>Failback naar de oorspronkelijke locatie herstellen

De configuratie van de Azure-VM-schijven in het vorige voorbeeld:

**Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | ---
DISK0 | C:\ | Besturingssysteemschijf
Disk1 | E:\ | Tijdelijke opslag</br /> </br />Azure voegt deze schijf toe en wijst de eerste beschikbare stationsletter toe.
Disk2 | D:\ | SQL-systeemdatabase en gebruikersdatabase1
Disk3 | G:\ | Gebruikersdatabase2


#### <a name="vmware-to-azure"></a>VMware naar Azure
Wanneer er een failback naar de oorspronkelijke locatie plaatsvindt, bevat de failback-VM-schijfconfiguratie geen uitgesloten schijven. Dat betekent dat de schijven die zijn uitgesloten van VMware naar Azure, niet beschikbaar zullen zijn op de virtuele failback-machine.

Schijven op de virtuele VMWare-machine (oorspronkelijke locatie) na de geplande failover van Azure naar on-premises VMware-schijven:

**Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | ---
DISK0 | C:\ | Besturingssysteemschijf
Disk1 | D:\ | SQL-systeemdatabase en gebruikersdatabase1
Disk2 | G:\ | Gebruikersdatabase2

#### <a name="hyper-v-to-azure"></a>Hyper-V naar Azure
Wanneer er een failback naar de oorspronkelijke locatie wordt uitgevoerd, blijft de schijfconfiguratie voor de virtuele failback-machine hetzelfde als van de oorspronkelijke virtuele machine-schijf voor Hyper-V. Schijven die zijn uitgesloten van Hyper-V-site naar Azure, zijn beschikbaar op de virtuele failback-machine.

Schijven op de virtuele Hyper-V-machine (oorspronkelijke locatie) na de geplande failover van Azure naar on-premises Hyper-V:

**Schijfnaam** | **Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 |   C:\ | Besturingssysteemschijf
DB-Disk1 | Disk1 | D:\ | SQL-systeemdatabase en gebruikersdatabase1
DB-Disk2 (uitgesloten schijf) | Disk2 | E:\ | Tijdelijke bestanden
DB-Disk3 (uitgesloten schijf) | Disk3 | F:\ | SQL-tempdb-database (mappad (F:\MSSQL\Data\)
DB-Disk4 | Disk4 | G:\ | Gebruikersdatabase2


#### <a name="exclude-the-paging-file-pagefilesys-disk"></a>Schijf van het wisselbestand (pagefile.sys) uitsluiten

We gaan uit van een virtuele machine met een wisselbestandschijf die kan worden uitgesloten.
Er zijn twee mogelijke situaties.

#### <a name="case-1-the-paging-file-is-configured-on-the-d-drive"></a>Situatie 1: het wisselbestand is geconfigureerd op de D:-schijf
Dit is de schijfconfiguratie:


**Schijfnaam** | **Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1 (de schijf is uitgesloten van de beveiliging) | Disk1 | D:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Dit zijn de instellingen voor het wisselbestand op de virtuele bronmachine:

![Instellingen voor het wisselbestand op de virtuele bronmachine](./media/site-recovery-exclude-disk/pagefile-on-d-drive-sourceVM.png)


Schijven op de virtuele Azure-machine na een failover van de virtuele machine van VMware naar Azure of Hyper-V naar Azure:

**Schijfnaam** | **Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1 | Disk1 | D:\ | Tijdelijke opslag</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Omdat Disk1 (D:) is uitgesloten, is D: de eerste stationsletter in de lijst. Azure wijst D: toe aan het tijdelijke opslagvolume. Omdat D: beschikbaar is op de virtuele Azure-machine, blijft de instelling voor het wisselbestand van de virtuele machine hetzelfde.

Dit zijn de instellingen voor het wisselbestand op de virtuele Azure-machine:

![Instellingen voor het wisselbestand op de virtuele Azure-machine](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover.png)

#### <a name="case-2-the-paging-file-is-configured-on-another-drive-other-than-d-drive"></a>Situatie 2: het wisselbestand is geconfigureerd op een ander station (met uitzondering van de D:-schijf)

Dit is de schijfconfiguratie voor de virtuele bronmachine:

**Schijfnaam** | **Gastbesturingssysteemschijf#** | **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0 | C:\ | Besturingssysteemschijf
DB-Disk1 (de schijf is uitgesloten van beveiliging) | Disk1 | G:\ | pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Dit zijn de instellingen voor het wisselbestand op de on-premises virtuele machine:

![Instellingen voor het wisselbestand op de on-premises virtuele machine](./media/site-recovery-exclude-disk/pagefile-on-g-drive-sourceVM.png)

Schijven op de virtuele Azure-machine na een failover van de virtuele machine van VMware of Hyper-V naar Azure:

**Schijfnaam**| **Gastbesturingssysteemschijf#**| **Stationsletter** | **Gegevenstype op de schijf**
--- | --- | --- | ---
DB-Disk0-OS | DISK0  |C:\ |Besturingssysteemschijf
DB-Disk1 | Disk1 | D:\ | Tijdelijke opslag</br /> </br />pagefile.sys
DB-Disk2 | Disk2 | E:\ | Gebruikersgegevens 1
DB-Disk3 | Disk3 | F:\ | Gebruikersgegevens 2

Aangezien D: de eerste letter in de lijst is, wijst Azure de letter D: toe aan het tijdelijke opslagvolume. De stationsletter is voor alle gerepliceerde schijven hetzelfde. Omdat de G:-schijf niet beschikbaar is, gebruikt het systeem het C:-station voor het wisselbestand.

Dit zijn de instellingen voor het wisselbestand op de virtuele Azure-machine:

![Instellingen voor het wisselbestand op de virtuele Azure-machine](./media/site-recovery-exclude-disk/pagefile-on-Azure-vm-after-failover-2.png)

## <a name="next-steps"></a>Volgende stappen
Wanneer uw implementatie actief is, kunt u [hier](site-recovery-failover.md) meer lezen over de verschillende soorten failovers.
