---
title: Azure Analysis Services-database maken en terugzetten | Microsoft Docs
description: Beschrijft hoe u back-up en herstel van een Azure Analysis Services-database.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: kfile
editor: 
ms.assetid: 
ms.service: analysis-services
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/01/2017
ms.author: owend
ms.openlocfilehash: f96e72e4decd475e7859eb7f70046a277b718ac8
ms.sourcegitcommit: d41d9049625a7c9fc186ef721b8df4feeb28215f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/02/2017
---
# <a name="backup-and-restore"></a>Back-ups en herstellen

Back-ups van databases in Azure Analysis Services model in tabelvorm is grotendeels hetzelfde als voor on-premises Analysis-Services. Het belangrijkste verschil is waar u uw back-upbestanden opslaat. Back-upbestanden moeten worden opgeslagen in een container in een [Azure storage-account](../storage/common/storage-create-storage-account.md). Kunt u een opslagaccount en container die u al hebt, of ze kunnen worden gemaakt bij het configureren van instellingen voor de opslag voor uw server.

> [!NOTE]
> Maken van een opslagaccount kan resulteren in een nieuwe factureerbare service. Zie voor meer informatie, [prijzen voor Azure Storage](https://azure.microsoft.com/pricing/details/storage/blobs/).
> 
> 

Back-ups worden opgeslagen met de extensie abf. Voor in het geheugen modellen in tabelvorm, worden zowel modelgegevens en metagegevens opgeslagen. Voor de DirectQuery-modellen in tabelvorm, is alleen de metagegevens van het model opgeslagen. Back-ups worden gecomprimeerd en versleuteld, afhankelijk van de opties die u kiest. 



## <a name="configure-storage-settings"></a>Instellingen voor de opslag configureren
Voordat u een back-up, moet u instellingen voor de opslag voor uw server configureren.


### <a name="to-configure-storage-settings"></a>Instellingen voor de opslag configureren
1.  In Azure portal > **instellingen**, klikt u op **back-up**.

    ![Back-ups in instellingen](./media/analysis-services-backup/aas-backup-backups.png)

2.  Klik op **ingeschakeld**, klikt u vervolgens op **Opslaginstellingen**.

    ![Inschakelen](./media/analysis-services-backup/aas-backup-enable.png)

3. Selecteer uw storage-account of maak een nieuwe.

4. Selecteer een container of maak een nieuwe.

    ![Container selecteren](./media/analysis-services-backup/aas-backup-container.png)

5. De back-instellingen opslaan.

    ![Back-instellingen opslaan](./media/analysis-services-backup/aas-backup-save.png)

## <a name="backup"></a>Back-up

### <a name="to-backup-by-using-ssms"></a>Back-up met behulp van SSMS

1. SSMS, met de rechtermuisknop op een database > **Back-Up**.

2. In **Backup Database** > **back-upbestand**, klikt u op **Bladeren**.

3. In de **bestand opslaan als** dialoogvenster, controleert u het mappad en typ een naam voor de back-upbestand. 

4. In de **Backup Database** dialoogvenster, opties selecteren.

    **Toestaan dat bestand overschreven** -Selecteer deze optie om de back-bestanden met dezelfde naam overschrijven. Als deze optie niet is ingeschakeld, kan niet het bestand dat u wilt opslaan dezelfde naam als een bestand dat al op dezelfde locatie bestaat hebben.

    **Compressie toepassen** -Selecteer deze optie voor het comprimeren van het back-upbestand. Gecomprimeerde back-upbestanden besparen op schijfruimte, maar moeten een enigszins hogere CPU-gebruik. 

    **Versleutelen van back-upbestand** -Selecteer deze optie voor het versleutelen van het back-upbestand. Deze optie vereist een door de gebruiker opgegeven wachtwoord voor het beveiligen van het back-upbestand. Het wachtwoord kan niet worden gelezen van de back-upgegevens enigerlei andere wijze dan een herstelbewerking. Als u kiest voor het versleutelen van back-ups, moet u het wachtwoord opslaan op een veilige locatie.

5. Klik op **OK** maken en de back-upbestand opslaan.


### <a name="powershell"></a>PowerShell
Gebruik [back-up ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/backup-asdatabase-cmdlet) cmdlet.

## <a name="restore"></a>Herstellen
Bij het herstellen, moet het back-upbestand in de storage-account dat u hebt geconfigureerd voor uw server. Als u moet een back-upbestand van een on-premises locatie verplaatsen naar uw opslagaccount, gebruik [Microsoft Azure Storage Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer) of de [AzCopy](../storage/common/storage-use-azcopy.md) opdrachtregelprogramma. 



> [!NOTE]
> Als u vanuit een on-premises server terugzetten wilt, moet u alle domeingebruikers uit het model rollen te verwijderen en opnieuw toevoegen aan de rollen als Azure Active Directory-gebruikers.
> 
> 

### <a name="to-restore-by-using-ssms"></a>Te herstellen met behulp van SSMS

1. SSMS, met de rechtermuisknop op een database > **herstellen**.

2. In de **Backup Database** dialoogvenster in **back-upbestand**, klikt u op **Bladeren**.

3. In de **databasebestanden vinden** dialoogvenster, selecteert u het bestand dat u wilt herstellen.

4. In **Restore database**, selecteert u de database.

5. Geef opties op. Beveiligingsopties moeten overeenkomen met de back-upopties die u hebt gebruikt om een back-up.


### <a name="powershell"></a>PowerShell

Gebruik [terugzetten ASDatabase](https://docs.microsoft.com/sql/analysis-services/powershell/restore-asdatabase-cmdlet) cmdlet.


## <a name="related-information"></a>Gerelateerde informatie

[Azure storage-accounts](../storage/common/storage-create-storage-account.md)  
[Hoge beschikbaarheid](analysis-services-bcdr.md)     
[Azure analyseservices beheren](analysis-services-manage.md)
