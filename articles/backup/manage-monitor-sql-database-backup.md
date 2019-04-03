---
title: Beheren en controleren van SQL Server-databases op een Azure-VM die wordt ondersteund door Azure Backup | Microsoft Docs
description: Dit artikel wordt beschreven hoe u kunt beheren en controleren van SQL Server-databases die worden uitgevoerd op een Azure-VM.
services: backup
author: rayne-wiselman
manager: carmonm
ms.service: backup
ms.topic: conceptual
ms.date: 03/14/2018
ms.author: raynew
ms.openlocfilehash: ea5495867d5f453db014e000e01d533d049dc628
ms.sourcegitcommit: 04716e13cc2ab69da57d61819da6cd5508f8c422
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/02/2019
ms.locfileid: "58849591"
---
# <a name="manage-and-monitor-backed-up-sql-server-databases"></a>Beheren en bewaken back-ups van SQL Server-databases

In dit artikel worden algemene taken beschreven voor het beheren en controleren van SQL Server-databases die worden uitgevoerd op een Azure-machine (VM) en die zijn back-ups op een Azure Backup Recovery Services-kluis door het [Azure Backup](backup-overview.md) service. U leert hoe u taken en waarschuwingen bewaken, stoppen en hervatten van databasebeveiliging, back-uptaken uitvoeren en een virtuele machine uit de back-ups van de registratie ongedaan maken.

Als u dit nog niet hebt nog back-ups is geconfigureerd voor uw SQL Server-databases, [maakt u een Back-up van SQL Server-databases op Azure Virtual machines](backup-azure-sql-database.md)

## <a name="monitor-manual-backup-jobs-in-the-portal"></a>Handmatige back-uptaken in de portal bewaken

Azure Backup worden alle handmatig geactiveerde taken in de **back-uptaken** portal. De taken u weergeven in deze portal opnemen database detecteren en registreren en back-up en herstelbewerkingen.

![De portal voor back-up-taken](./media/backup-azure-sql-database/jobs-list.png)

> [!NOTE]
> De **back-uptaken** portal geplande back-uptaken wordt niet weergegeven. Geplande back-uptaken kunt u bewaken met SQL Server Management Studio, zoals wordt beschreven in het volgende gedeelte.
>

Voor meer informatie over scenario's voor bewaking, gaat u naar [bewaken in Azure Portal](backup-azure-monitoring-built-in-monitor.md) en [bewaken met behulp van Azure Monitor](backup-azure-monitoring-use-azuremonitor.md).  


## <a name="view-backup-alerts"></a>Waarschuwingen voor back-ups weergeven

Omdat logboekback-ups worden uitgevoerd om de 15 minuten, kan back-uptaken bewaking worden omslachtig. Azure Backup vereenvoudigt de bewaking in door te verzenden van e-mailwaarschuwingen. E-mailwaarschuwingen zijn:

- Alle mislukte back-ups wordt geactiveerd.
- Geconsolideerd op het databaseniveau van de-foutcode.
- Alleen voor een database de eerste back-upfouten verzonden.

Voor het bewaken van de back-upwaarschuwingen database:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).

2. Selecteer op het kluisdashboard **waarschuwingen en gebeurtenissen**.

   ![Waarschuwingen en gebeurtenissen selecteren](./media/backup-azure-sql-database/vault-menu-alerts-events.png)

3. In **waarschuwingen en gebeurtenissen**, selecteer **waarschuwingen voor back-up**.

   ![Waarschuwingen voor back-ups selecteren](./media/backup-azure-sql-database/backup-alerts-dashboard.png)

## <a name="stop-protection-for-a-sql-server-database"></a>Beveiliging van een SQL Server-database stoppen

U kunt stoppen back-ups van een SQL Server-database in een aantal manieren:

* Alle toekomstige back-uptaken stoppen en verwijderen van alle herstelpunten.
* Alle toekomstige back-uptaken stoppen en de herstelpunten te behouden.

Als u ervoor kiest om te laten herstelpunten, houd rekening met deze details:

* Alle herstelpunten altijd intact blijven, alle weghalen wordt beëindigd op stoppen beveiliging met behoud van gegevens.
* U wordt gefactureerd voor het beveiligde exemplaar en de verbruikte opslag. Zie voor meer informatie, [prijzen van Azure Backup](https://azure.microsoft.com/pricing/details/backup/).
* Als u een gegevensbron verwijdert zonder back-ups wordt gestopt, worden nieuwe back-ups mislukken.

Ga als volgt te werk om de beveiliging van een database te stoppen:

1. Selecteer op het kluisdashboard **back-Upitems**.

2. Onder **Type back-upbeheer**, selecteer **SQL in Azure VM**.

    ![SQL in Azure VM selecteren](./media/backup-azure-sql-database/sql-restore-backup-items.png)

3. Selecteer de database waarvoor u wenst te beveiliging stoppen.

    ![De database selecteren waarvoor u de beveiliging wilt stoppen](./media/backup-azure-sql-database/sql-restore-sql-in-vm.png)

4. Selecteer in het databasemenu **back-up stoppen**.

    ![Back-up stoppen selecteren](./media/backup-azure-sql-database/stop-db-button.png)


5. Op de **back-up stoppen** in het menu selecteren of u wilt behouden of verwijderen van gegevens. Als u wilt, Geef een reden en de opmerking.

    ![Behouden of verwijderen van gegevens in het menu back-up stoppen](./media/backup-azure-sql-database/stop-backup-button.png)

6. Selecteer **back-up stoppen**.


## <a name="resume-protection-for-a-sql-database"></a>Beveiliging voor een SQL-database hervatten

Wanneer u de beveiliging voor de SQL-database stoppen als u selecteert de **back-upgegevens behouden** optie, kunt u later beveiliging hervatten. Als u niet de back-upgegevens behouden, kunt u de beveiliging niet hervatten.

Beveiliging voor een SQL-database hervatten:

1. Open het back-upitem en selecteer **back-up hervatten**.

    ![Back-up hervatten selecteren om de databasebeveiliging te hervatten](./media/backup-azure-sql-database/resume-backup-button.png)

2. Selecteer een beleid in het menu **Back-upbeleid** en selecteer vervolgens **Opslaan**.

## <a name="run-an-on-demand-backup"></a>Een on-demand back-up uitvoeren

U kunt verschillende typen back-ups op aanvraag uitvoeren:

* Volledige back-up
* Alleen te kopiëren volledige back-up
* Differentiële back-up
* Logboekback-up

Terwijl u nodig hebt om op te geven van de bewaartermijn voor de kopie-alleen volledige back-, wordt de bewaartermijn voor andere back-uptypen automatisch ingesteld op 30 dagen na de huidige tijd. <br/>
Zie voor meer informatie, [SQL Server-back-uptypen](backup-architecture.md#sql-server-backup-types).

## <a name="unregister-a-sql-server-instance"></a>Registratie van een SQL Server-exemplaar ongedaan maken

Registratie van een exemplaar van SQL Server nadat u de beveiliging uitschakelen, maar voordat u de kluis verwijderen:

1. Op het kluisdashboard onder **beheren**, selecteer **back-upinfrastructuur**.  

   ![Back-upinfrastructuur selecteren](./media/backup-azure-sql-database/backup-infrastructure-button.png)

2. Onder **Beheerservers** selecteert u **Beveiligde servers**.

   ![Beveiligde servers selecteren](./media/backup-azure-sql-database/protected-servers.png)

3. In **beschermde Servers**, de server om de registratie te selecteren. Als u de kluis wilt verwijderen, moet u de registratie van alle servers ongedaan maken.

4. Met de rechtermuisknop op de beveiligde server en selecteer **verwijderen**.

   ![Verwijderen selecteren](./media/backup-azure-sql-database/delete-protected-server.png)

## <a name="re-register-extension-on-the-sql-server-vm"></a>Extensie opnieuw te registreren op de SQL Server-VM

De extensie van de werkbelasting op de virtuele machine kan soms worden beïnvloed voor een reden of de andere. In dergelijke gevallen zullen alle bewerkingen die worden geactiveerd op de virtuele machine mislukken. Mogelijk moet u vervolgens de extensie op de virtuele machine opnieuw te registreren. **Opnieuw registreren** bewerking opnieuw installeren van de werkbelasting van back-upextensie op de virtuele machine voor bewerkingen om door te gaan.  <br>

Is het raadzaam deze optie wilt gebruiken met waarschuwing; wanneer ze worden geactiveerd op een virtuele machine met de extensie al in orde is, hebben deze bewerking zorgt ervoor dat de extensie voor het ophalen opnieuw wordt gestart. Dit kan resulteren in alle lopende taken mislukken. Schakel voor een of meer van de [symptomen](backup-sql-server-azure-troubleshoot.md#symptoms) voordat u activeert de bewerking opnieuw te registreren.

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie, [back-ups op een SQL Server-database oplossen](backup-sql-server-azure-troubleshoot.md).
