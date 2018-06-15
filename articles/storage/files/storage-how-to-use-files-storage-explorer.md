---
title: Azure-bestandsshares beheren met Azure Storage Explorer
description: Leer hoe u Azure-bestandsshares beheert met Azure Storage Explorer.
services: storage
documentationcenter: ''
author: wmgries
manager: aungoo
editor: tamram
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: f628f393f1ff5922d9d2c82b30d4fdbf4b5caf23
ms.sourcegitcommit: c722760331294bc8532f8ddc01ed5aa8b9778dec
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34738170"
---
# <a name="manage-azure-file-shares-with-azure-storage-explorer"></a>Azure-bestandsshares beheren met Azure Storage Explorer 
[Azure Files ](storage-files-introduction.md) is het gebruiksvriendelijke cloudbestandssysteem van Microsoft. In dit artikel doorloopt u basisbeginselen van het werken met Azure-bestandsshares met behulp van [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/). Azure Storage Explorer is een populair clienthulpprogramma dat beschikbaar is voor Windows, macOS en Linux. Met Azure Storage Explorer kunt u Azure-bestandsshares en andere opslagresources beheren.

Voor deze snelstartgids moet Azure Storage Explorer zijn geïnstalleerd. Ga naar [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om het te downloaden en te installeren.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep en een opslagaccount maken
> * Een Azure-bestandsshare maken 
> * Een map maken
> * Bestand uploaden
> * Bestand downloaden
> * Een momentopname van een share maken en gebruiken

Als u nog geen abonnement op Azure hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) maken voordat u begint.

## <a name="create-a-storage-account"></a>Create a storage account
U kunt Azure Storage Explorer niet gebruiken voor het maken van nieuwe resources. Voor deze demo maakt u het opslagaccount in [Azure Portal](https://portal.azure.com/). 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connect-storage-explorer-to-azure-resources"></a>Azure Storage Explorer verbinden met Azure-resources
Wanneer u Azure Storage Explorer de eerste keer start, wordt het venster **Microsoft Azure Storage Explorer - Verbinding maken** weergegeven. Azure Storage Explorer biedt verschillende manieren om verbinding te maken met opslagaccounts: 

- **Aanmelden met uw Azure-account**: u kunt u aanmelden met behulp van uw gebruikersreferenties voor uw organisatie of uw Microsoft-account. 
- **Verbinding maken met een specifiek opslagaccount met behulp van een verbindingsreeks of SAS-token**: een verbindingsreeks is een speciale tekenreeks die een opslagaccountnaam en -sleutel/SAS-token bevat. Het token geeft Azure Storage Explorer rechtstreeks toegang tot het opslagaccount (in plaats van dat gewoon alle opslagaccounts in een Azure-account worden weergegeven). Zie [Azure Storage-verbindingsreeksen configureren](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie over verbindingsreeksen.
- **Verbinding maken met een specifiek opslagaccount met behulp van de naam en sleutel van het opslagaccount**: gebruik de naam en de sleutel van het opslagaccount om verbinding te maken met Azure-opslag.

Voor deze snelstartgids meldt u zich aan met uw Azure-account. Selecteer **Een Azure-account toevoegen** en selecteer **Aanmelden**. Volg de aanwijzingen om u aan te melden bij uw Azure-account.

![Een schermafbeelding van Microsoft Azure Storage Explorer - venster Verbinding maken](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Een bestandsshare maken
Ga als volgt te werk om uw eerste Azure-bestandsshare te maken in het opslagaccount *storageacct<random number>*:

1. Vouw het opslagaccount uit dat u hebt gemaakt.
2. Klik met de rechtermuisknop op **Bestandsshares** en selecteer **Bestandsshare maken**.  
    ![Een schermopname van de map File Shares en het contextmenu](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Voer voor de bestandsshare *myshare* in en druk op Enter.

> [!IMPORTANT]  
> De namen van shares mogen alleen kleine letters, cijfers en afbreekstreepjes bevatten, maar mogen niet met een afbreekstreepje beginnen. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

Nadat de bestandsshare is gemaakt, wordt er in het rechterdeelvenster een tabblad voor de bestandsshare geopend. 

## <a name="work-with-the-contents-of-an-azure-file-share"></a>Werken met de inhoud van een Azure-bestandsshare
U beschikt nu over een Azure-bestandsshare. De volgende stap is het koppelen van de bestandsshare met SMB op [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) of [macOS](storage-how-to-use-files-mac.md). U kunt ook Azure CLI gebruiken om met uw Azure-bestandsshare te werken. Het voordeel van het gebruik van Azure CLI ten opzichte van het koppelen van de bestandsshare met behulp van SMB is dat alle aanvragen die zijn ingediend via Azure CLI worden gedaan met behulp van de REST API van het bestand. U kunt de REST API van het bestand gebruiken om bestanden en mappen te maken, te wijzigen en te verwijderen op clients die niet over SMB beschikken.

### <a name="create-a-directory"></a>Een map maken
Het toevoegen van een map biedt een hiërarchische structuur voor het beheren van de bestandsshare. U kunt meerdere niveaus maken in uw map. U moet er echter voor zorgen dat de bovenliggende mappen bestaan voordat u submappen maakt. Voor het pad myDirectory/mySubDirectory moet u bijvoorbeeld eerst de map *myDirectory* maken. Daarna kunt u *mySubDirectory* maken. 

1. Ga naar het tabblad voor de bestandsshare en selecteer in het menu bovenaan de knop **Nieuwe map**. Het deelvenster **Nieuwe map maken** wordt geopend.
    ![Een schermopname van de knop Nieuwe map in context](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Voer voor de naam van de map *myDirectory* in en selecteer **OK**. 

De map *myDirectory* wordt vermeld op het tabblad voor de bestandsshare *myshare*.

### <a name="upload-a-file"></a>Bestand uploaden 
U kunt een bestand van uw lokale computer uploaden naar de nieuwe map in de bestandsshare. U kunt een hele map uploaden of één bestand.

1. Selecteer **Uploaden** in het bovenste menu. U kunt nu een map of bestand uploaden.
2. Selecteer **Bestand uploaden** en selecteer het bestand dat u van uw lokale computer wilt uploaden.
3. Voer *myDirectory* in bij **Uploaden naar een map** en selecteer **Uploaden**. 

Wanneer u klaar bent, wordt het bestand weergegeven in de lijst in het deelvenster *myDirectory*.

### <a name="download-a-file"></a>Bestand downloaden
Als u een kopie van een bestand vanuit uw bestandsshare wilt downloaden, klikt u met de rechtermuisknop op het bestand en selecteert u **Downloaden**. Kies waar u het bestand op uw lokale computer wilt opslaan en selecteer **Opslaan**.

De voortgang van de download wordt weergegeven in het deelvenster **Activiteiten** onderaan het venster.

## <a name="create-and-modify-share-snapshots"></a>Momentopnamen van shares maken en wijzigen
Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een bestandsshare zijn vergelijkbaar met andere technologieën die u mogelijk al kent, zoals:
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) voor Windows-bestandssystemen, zoals NTFS en ReFS
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS

Een momentopname van een share maken:

1. Selecteer het tabblad voor de bestandsshare *myshare*.
2. Selecteer in het bovenste menu **Momentopname maken**. (Afhankelijk van de afmetingen van het venster van Azure Storage Explorer moet u misschien eerst **Meer** selecteren om deze optie te zien.)  
    ![Een schermopname van de knop Momentopname maken in context](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Momentopnamen van shares weergeven en er doorheen bladeren
Wanneer de momentopname is gemaakt, selecteert u **Momentopnamen weergeven** om een lijst te zien met de momentopnamen voor de share. (Afhankelijk van de afmetingen van het venster van Azure Storage Explorer moet u misschien eerst **Meer** selecteren om deze optie te zien.) Dubbelklik op de momentopname van een share als u er doorheen wilt bladeren.

![Een schermopname van het venster waarin u door momentopnamen van shares kunt bladeren](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
Om te laten zien hoe u een bestand herstelt vanuit een momentopname van de share, moet u eerst een bestand verwijderen uit de live Azure-bestandsshare. Ga naar de map *myDirectory*, klik met de rechtermuisknop op het bestand dat u hebt geüpload en selecteer **Verwijderen**. Ga als volgt te werk om dat bestand te herstellen vanuit de momentopname van de share:

1. Selecteer **Momentopnamen voor bestandsshare weergeven**. (Afhankelijk van de afmetingen van het venster van Azure Storage Explorer moet u misschien eerst **Meer** selecteren om deze optie te zien.)
2. Dubbelklik in de lijst met momentopnamen op de momentopname van de share.
3. Blader door de momentopname totdat u het bestand hebt gevonden dat u hebt verwijderd. Selecteer de bestandsshare en selecteer **Momentopname terugzetten**. (Afhankelijk van de afmetingen van het venster van Azure Storage Explorer moet u misschien eerst **Meer** selecteren om deze optie te zien.) In een volgend venster wordt de waarschuwing weergegeven dat hierdoor de inhoud van de bestandsshare wordt overschreven en dat dit niet ongedaan kan worden gemaakt. Selecteer **OK**.
4. Als het goed is, staat het bestand nu weer op de oorspronkelijke locatie in de live Azure-bestandsshare.

### <a name="delete-a-share-snapshot"></a>Een share-momentopname verwijderen
Als u een momentopname van een share wilt verwijderen, gaat u naar de [lijst met momentopnamen van de share](#list-and-browse-share-snapshots). Klik met de rechtermuisknop op de momentopname die u wilt verwijderen en selecteer **Verwijderen**.

## <a name="clean-up-resources"></a>Resources opschonen
U kunt Azure Storage Explorer niet gebruiken voor het verwijderen van resources. U kunt [Azure Portal](https://portal.azure.com/) gebruiken als u wilt opschonen vanuit deze snelstartgids. 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Volgende stappen
- [Bestandsshares beheren met Azure Portal](storage-how-to-use-files-portal.md)
- [Bestandsshares beheren met Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Bestandsshares beheren met Azure CLI](storage-how-to-use-files-cli.md)
- [Implementatie van Azure Files plannen](storage-files-planning.md)