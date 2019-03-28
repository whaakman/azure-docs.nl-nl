---
title: Azure Analysis Services-database maken en herstellen | Microsoft Docs
description: Beschrijft hoe u back-up en herstellen van een Azure Analysis Services-database.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 01/09/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 31e8e65b382a3a6bcad2998a0babdf9605dc4968
ms.sourcegitcommit: cf971fe82e9ee70db9209bb196ddf36614d39d10
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/27/2019
ms.locfileid: "58539102"
---
# <a name="backup-and-restore"></a>Back-ups en herstellen

Back-ups van databases in Azure Analysis Services tabellair model is bijna hetzelfde als voor on-premises Analysis Services. Het belangrijkste verschil is waar u uw back-upbestanden opslaat. Back-upbestanden moeten worden opgeslagen in een container in een [Azure storage-account](../storage/common/storage-create-storage-account.md). Kunt u een opslagaccount en container die u al hebt, of ze kunnen worden gemaakt bij het configureren van instellingen voor de opslag voor uw server.

> [!NOTE]
> Het maken van een opslagaccount kan resulteren in een nieuwe factureerbare service. Zie voor meer informatie, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Back-ups worden opgeslagen met de extensie .abf zijn. Voor in het geheugen modellen in tabelvorm, worden zowel modelgegevens en metagegevens opgeslagen. Voor DirectQuery-modellen in tabelvorm, is alleen de metagegevens van het model opgeslagen. Back-ups kunnen worden gecomprimeerd en versleuteld, afhankelijk van de opties die u kiest.


## <a name="configure-storage-settings"></a>Configureren van opslag
Voordat u back-up, die u wilt configureren van opslag voor uw server.


### <a name="to-configure-storage-settings"></a>Instellingen voor de opslag configureren
1.  In Azure portal > **instellingen**, klikt u op **back-up**.

    ![Back-ups in de instellingen](./media/analysis-services-backup/aas-backup-backups.png)

2.  Klik op **ingeschakeld**, klikt u vervolgens op **Opslaginstellingen**.

    ![Inschakelen](./media/analysis-services-backup/aas-backup-enable.png)

3. Selecteer uw opslagaccount of maak een nieuwe.

4. Selecteer een container of een nieuwe maken.

    ![Container selecteren](./media/analysis-services-backup/aas-backup-container.png)

5. Uw back-upinstellingen opslaan.

    ![Back-upinstellingen opslaan](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Backup

### <a name="to-backup-by-using-ssms"></a>Back-up maken met behulp van SSMS

1. In SSMS, met de rechtermuisknop op een database > **Back-Up**.

2. In **Backup Database** > **back-upbestand**, klikt u op **Bladeren**.

3. In de **bestand opslaan als** dialoogvenster, controleert u het mappad en typ een naam voor de back-upbestand. 

4. In de **Backup Database** dialoogvenster, selecteer opties.

    **Bestand overschrijven** : Selecteer deze optie voor het overschrijven van de back-upbestanden met dezelfde naam. Als deze optie niet is ingeschakeld, kan het bestand dat u wilt opslaan kan niet dezelfde naam hebben als een bestand dat al op dezelfde locatie bestaat.

    **Compressie toepassen** : Selecteer deze optie voor het comprimeren van het back-upbestand. Gecomprimeerde back-upbestanden schijfruimte bespaard, maar moeten een enigszins hogere CPU-gebruik. 

    **Versleutelen van back-upbestand** : Selecteer deze optie voor het versleutelen van het back-upbestand. Deze optie vereist een door de gebruiker opgegeven wachtwoord voor het beveiligen van het back-upbestand. Het wachtwoord wordt voorkomen dat het lezen van de back-upgegevens van een andere manier dan een herstelbewerking uit. Als u kiest voor het versleutelen van back-ups, moet u het wachtwoord opslaan op een veilige locatie.

5. Klik op **OK** maken en opslaan van het back-upbestand.


### <a name="powershell"></a>PowerShell
Gebruik [back-up-ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.

## <a name="restore"></a>Herstellen
Bij het herstellen, worden uw back-upbestand moet zich in het opslagaccount dat u hebt geconfigureerd voor uw server. Als u een back-upbestand van een on-premises locatie verplaatsen naar uw opslagaccount wilt, gebruikt u [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) of de [AzCopy](../storage/common/storage-use-azcopy.md) opdrachtregel-hulpprogramma. 



> [!NOTE]
> Als u vanuit een on-premises server terugzetten wilt, moet u alle domeingebruikers verwijderen uit het model van rollen en deze opnieuw toevoegen aan de rollen als Azure Active Directory-gebruikers.
> 
> 

### <a name="to-restore-by-using-ssms"></a>Om terug te zetten met behulp van SSMS

1. In SSMS, met de rechtermuisknop op een database > **herstellen**.

2. In de **Backup Database** dialoogvenster in **back-upbestand**, klikt u op **Bladeren**.

3. In de **databasebestanden zoeken** dialoogvenster, selecteer het bestand dat u wilt herstellen.

4. In **Restore database**, selecteert u de database.

5. Geef opties op. Opties voor het beveiligingslogboek moeten overeenkomen met de back-upopties die u hebt gebruikt toen de back-up.


### <a name="powershell"></a>PowerShell

Gebruik [terugzetten ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.


## <a name="related-information"></a>Gerelateerde informatie

[Azure storage-accounts](../storage/common/storage-create-storage-account.md)  
[Hoge beschikbaarheid](analysis-services-bcdr.md)     
[Azure analyseservices beheren](analysis-services-manage.md)
