---
title: Opmerkingen bij de release voor Microsoft Azure Backup Server v3
description: In dit artikel bevat de informatie over bekende problemen en tijdelijke oplossingen voor MABS v3.
services: backup
author: JYOTHIRMAISURI
manager: vvithal
ms.service: backup
ms.topic: conceptual
ms.date: 11/22/2018
ms.author: v-jysur
ms.asset: 0c4127f2-d936-48ef-b430-a9198e425d81
ms.openlocfilehash: 1d077fc8a853f145d271fa69d453ea2ea70ab696
ms.sourcegitcommit: a08d1236f737915817815da299984461cc2ab07e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2018
ms.locfileid: "52319475"
---
# <a name="release-notes-for-microsoft-azure-backup-server"></a>Opmerkingen bij de release voor Microsoft Azure Backup Server
Dit artikel vindt de bekende problemen en oplossingen voor Microsoft Azure Backup-Server (MABS) V3.

##  <a name="backup-and-recovery-fails-for-clustered-workloads"></a>Back-up en herstel is mislukt voor geclusterde werkbelastingen

**Beschrijving:** back-up/herstel is mislukt voor geclusterde gegevensbronnen zoals Hyper-V-cluster of SQL-cluster (SQL Always On) of Exchange in de database (groep met Databasebeschikbaarheid) na de upgrade van MABS V2 naar V3 MABS.

**Tijdelijke oplossing:** om dit te voorkomen, opent u SQL Server Management Studio (SSMS)) en voer de volgende SQL-script uit op de DPM-database:


    IF EXISTS (SELECT * FROM dbo.sysobjects
        WHERE id = OBJECT_ID(N'[dbo].[tbl_PRM_DatasourceLastActiveServerMap]')
        AND OBJECTPROPERTY(id, N'IsUserTable') = 1)
        DROP TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap]
        GO

        CREATE TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] (
            [DatasourceId]          [GUID]          NOT NULL,
            [ActiveNode]            [nvarchar](256) NULL,
            [IsGCed]                [bit]           NOT NULL
            ) ON [PRIMARY]
        GO

        ALTER TABLE [dbo].[tbl_PRM_DatasourceLastActiveServerMap] ADD
    CONSTRAINT [pk__tbl_PRM_DatasourceLastActiveServerMap__DatasourceId] PRIMARY KEY NONCLUSTERED
        (
            [DatasourceId]
        )  ON [PRIMARY],

    CONSTRAINT [DF_tbl_PRM_DatasourceLastActiveServerMap_IsGCed] DEFAULT
        (
            0
        ) FOR [IsGCed]
    GO


##  <a name="upgrade-to-mabs-v3-fails-in-russian-locale"></a>Een upgrade uit naar MABS V3 in Rusland landinstelling is mislukt

**Beschrijving:** Upgrade van MABS V2 naar V3 MABS in Rusland landinstelling is mislukt met foutcode **4387**.

**Tijdelijke oplossing:** de volgende stappen uit om te upgraden naar MABS V3 met behulp van Russisch pakket installeren:

1.  [Back-up](https://docs.microsoft.com/sql/relational-databases/backup-restore/create-a-full-database-backup-sql-server?view=sql-server-2017#SSMSProcedure) uw SQL-database en het verwijderen van MABS V2 (Kies de beveiligde gegevens behouden tijdens de installatie ongedaan maken).
2.  Een upgrade uitvoert naar SQL 2017 (Enterprise) en reporting als onderdeel van de upgrade verwijderen.
3. [Installeer](https://docs.microsoft.com/sql/reporting-services/install-windows/install-reporting-services?view=sql-server-2017#install-your-report-server) SQL Server Reporting Services (SSRS).
4.  [Installeer](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-2017#ssms-installation-tips-and-issues-ssms-1791) SQL Server Management Studio (SSMS).
5.  Configureren van rapportage met behulp van de parameters zoals beschreven in [SSRS-configuratie met SQL 2017](https://docs.microsoft.com/azure/backup/backup-azure-microsoft-azure-backup#upgrade-mabs).
6.  [Installeer](backup-azure-microsoft-azure-backup.md) MABS V3.
7. [Herstellen](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms?view=sql-server-2017) SQL met behulp van SSMS en voer DPM-Sync-hulpprogramma, zoals beschreven [hier](https://docs.microsoft.com/it-it/previous-versions/system-center/data-protection-manager-2010/ff634215(v=technet.10)).
8.  Werk de eigenschap 'DataBaseVersion' in dbo.tbl_DLS_GlobalSetting tabel met de volgende opdracht:

        UPDATE dbo.tbl_DLS_GlobalSetting
        set PropertyValue = '13.0.415.0'
        where PropertyName = 'DatabaseVersion'


9.  MSDPM-service te starten.


## <a name="next-steps"></a>Volgende stappen

[Wat is er nieuw in v3 MABS](backup-mabs-whats-new-mabs.md)
