---
title: Azure-bestandsshares beheren met Azure Storage Explorer
description: Leer hoe u Azure-bestandsshares beheert met Azure Storage Explorer.
services: storage
documentationcenter: ''
author: wmgries
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 02/27/2018
ms.author: wgries
ms.openlocfilehash: 0c16d3b0ef0b178f99eaafecd74eabb00cca5af9
ms.sourcegitcommit: c3d53d8901622f93efcd13a31863161019325216
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/29/2018
---
# <a name="managing-azure-file-shares-with-azure-storage-explorer"></a>Azure-bestandsshares beheren met Azure Storage Explorer 
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. In deze handleiding worden de basisbeginselen besproken van het werken met Azure-bestandsshares met behulp van [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/). Azure Storage Explorer is een populair clienthulpprogramma dat beschikbaar is voor Windows, macOS en Linux voor het beheren van Azure-bestandsshares en andere opslagresources.

Voor deze snelstartgids moet Azure Storage Explorer zijn geïnstalleerd. Als u dit programma wilt installeren, gaat u naar [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer/) om het te downloaden.

In dit artikel leert u het volgende:

> [!div class="checklist"]
> * Een resourcegroep en een opslagaccount maken
> * Een Azure-bestandsshare maken 
> * Een map maken
> * Bestand uploaden
> * Bestand downloaden
> * Een momentopname van een share maken en gebruiken

Als u nog geen abonnement op Azure hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) maken voordat u begint.

## <a name="create-a-storage-account"></a>Een opslagaccount maken
In Azure Storage Explorer is het niet mogelijk om nieuwe resources te maken. Om deze snelstartgids toch te kunnen uitvoeren, maakt u het opslagaccount daarom met [Azure Portal](https://portal.azure.com/). 

[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="connecting-azure-storage-explorer-to-azure-resources"></a>Azure Storage Explorer verbinden met Azure-resources
Bij de eerste keer opstarten wordt het venster **Microsoft Azure Storage Explorer - Verbinding maken** weergegeven. Azure Storage Explorer biedt verschillende manieren om verbinding te maken met opslagaccounts: 

- **Aanmelden met uw Azure-account**: kies deze manier om u aan te melden met uw gebruikersreferenties voor uw organisatie of uw Microsoft-account. 
- **Verbinding maken met een specifiek opslagaccount met behulp van een verbindingsreeks of SAS-token**: een verbindingsreeks is een speciale tekenreeks met de naam van het opslagaccount en de opslagaccountsleutel of het SAS-token waarmee Azure Storage Explorer rechtstreeks toegang kan krijgen tot het opslagaccount (in plaats van alle opslagaccounts binnen een Azure-account weer te geven). Zie [Azure Storage-verbindingsreeksen configureren](../common/storage-configure-connection-string.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) voor meer informatie over verbindingsreeksen.
- **Verbinding maken met een specifiek opslagaccount met behulp van de naam en sleutel van het opslagaccount**: gebruik de naam en de sleutel van het opslagaccount om verbinding te maken met Azure-opslag.

Om deze snelstartgids uit te voeren, meldt u zich aan met uw Azure-account. Selecteer **Een Azure-account toevoegen** en klik op **Aanmelden**. Volg de instructies op het scherm om u aan te melden bij uw Azure-account.

![Het venster Microsoft Azure Storage Explorer - Verbinding maken](./media/storage-how-to-use-files-storage-explorer/connect-to-azure-storage-1.png)

### <a name="create-a-file-share"></a>Een bestandsshare maken
Ga als volgt te werk om uw eerste Azure-bestandsshare te maken binnen het opslagaccount *storageacct<random number>*:

1. Vouw het opslagaccount uit dat u hebt gemaakt.
2. Klik met de rechtermuisknop op **Bestandsshares** en selecteer **Bestandsshare maken**.  
    ![Een schermopname van de map File Shares en het contextmenu](media/storage-how-to-use-files-storage-explorer/create-file-share-1.png)

3. Typ *myshare* delen voor de bestandsshare en druk op **Enter**.

> [!Important]  
> De namen van shares moeten bestaan uit kleine letters, cijfers en afbreekstreepjes, maar mogen niet beginnen met een afbreekstreepje. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://docs.microsoft.com/rest/api/storageservices/Naming-and-Referencing-Shares--Directories--Files--and-Metadata) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

Als de bestandsshare is gemaakt, ziet u in het deelvenster aan de rechterkant een tabblad voor de bestandsshare. 

## <a name="manipulating-the-contents-of-the-azure-file-share"></a>Inhoud van Azure-bestandsshare bewerken
U beschikt nu over een Azure-bestandsshare. De volgende stap is het koppelen van de bestandsshare met SMB op [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) of [macOS](storage-how-to-use-files-mac.md). U kunt uw Azure-bestandsshare ook bewerken met Azure Portal. Alle aanvragen via Azure Portal worden gedaan via de REST-API van File, zodat u bestanden en mappen kunt maken, wijzigen en verwijderen in clients zonder toegang tot SMB.

### <a name="create-a-directory"></a>Een map maken
Het toevoegen van een map biedt een hiërarchische structuur voor het beheren van de bestandsshare. U kunt meerdere niveaus maken, maar u dan wel eerst alle bovenliggende mappen maken voordat u een submap maakt. Het pad mijnMap/mijnSubmap kunt u bijvoorbeeld alleen maken door eerst *mijnMap* te maken en vervolgens *mijnSubmap*. 

1. Ga op het tabblad voor de bestandsshare naar het menu bovenaan en klik op de knop **+ Nieuwe map**. De pagina **Nieuwe map maken** wordt geopend.
    ![Een schermopname van de knop Nieuwe map in context](media/storage-how-to-use-files-storage-explorer/create-directory-1.png)

2. Typ *myDirectory* als de naam en klik vervolgens op **OK**. 

De map *myDirectory* wordt vermeld op het tabblad voor de bestandsshare *myshare*.

### <a name="upload-a-file"></a>Bestand uploaden 
Upload een bestand van uw lokale computer naar de nieuwe map in de bestandsshare. U kunt een volledige map uploaden of een bepaald bestand.

1. Selecteer in het menu bovenaan **Uploaden**. U kunt nu een map of bestand uploaden.

2. Selecteer **Bestand uploaden** en kies vervolgens een bestand op uw lokale computer om te uploaden.

3. Typ *myDirectory* bij **Uploaden naar een map** en klik vervolgens op **Uploaden**. 

Als het uploaden is voltooid, ziet u het bestand in de lijst op de pagina **myDirectory**.

### <a name="download-a-file"></a>Bestand downloaden
U kunt een kopie van een bestand in de bestandsshare downloaden door met de rechtermuisknop op het bestand te klikken en **Downloaden** te selecteren. Geef aan waar u het bestand op uw lokale computer wilt opslaan en klik vervolgens op **Opslaan**.

U ziet de voortgang van de download in het deelvenster **Activiteiten** onderaan het venster.

## <a name="create-and-modify-share-snapshots"></a>Momentopnamen van shares maken en wijzigen
Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een bestandsshare zijn vergelijkbaar met andere technologieën die u mogelijk al kent, zoals:
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee923636) voor Windows-bestandssystemen zoals NTFS en ReFS.
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS. 

Een momentopname van een share maken:

1. Open het tabblad voor de bestandsshare *myshare*.
2. Klik in het menu aan de bovenkant van het tabblad op **Momentopname maken** (deze optie kan verborgen zitten achter de knop **... Meer**, afhankelijk van de grootte van het venster van Azure Storage Explorer).  
    ![Een schermopname van de knop Momentopname maken in context](media/storage-how-to-use-files-storage-explorer/create-share-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Momentopnamen van shares weergeven en er doorheen bladeren
Als de momentopname is gemaakt, kunt u op **Momentopnamen weergeven voor bestandsshare** klikken (deze optie kan verborgen zitten achter de knop **... Meer**, afhankelijk van de grootte van het venster van Azure Storage Explorer) om de momentopnamen voor de share weer te geven. Dubbelklik op de momentopname van een share om er doorheen te bladeren.

![Een schermopname van het scherm met momentopnamen van shares](media/storage-how-to-use-files-storage-explorer/list-browse-snapshots-1.png)

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
Om te laten zien hoe u een bestand kunt herstellen vanuit een share-momentopname, moeten we eerst een bestand verwijderen uit de live Azure-bestandsshare. Navigeer naar de map *myDirectory*, klik met de rechtermuisknop op het bestand dat u hebt geüpload en klik vervolgens op **Verwijderen**. Ga nu als volgt te werk om dit bestand te herstellen vanuit de momentopname van de share:

1. Klik op **Momentopnamen weergeven voor bestandsshare** (deze optie kan verborgen zitten achter de knop **... Meer**, afhankelijk van de grootte van het venster van Azure Storage Explorer).
2. Selecteer een share-momentopname in de lijst en dubbelklik om te navigeren in de momentopname.
3. Navigeer door de momentopname totdat u het bestand ziet dat u hebt verwijderd, selecteer het bestand en klik op **Momentopname terugzetten** (deze optie kan verborgen zitten achter de knop **... Meer**, afhankelijk van de grootte van het venster van Azure Storage Explorer). U ziet een waarschuwing dat hierdoor de inhoud van de bestandsshare wordt overschreven en dat dit niet ongedaan kan worden gemaakt. Selecteer **OK**.
4. Het bestand moet nu weer op de oorspronkelijke locatie in de live Azure-bestandsshare staan.

### <a name="delete-a-share-snapshot"></a>Een share-momentopname verwijderen
Als u een momentopname van een share wilt verwijderen, [gaat u naar de lijst met share-momentopnamen](#list-and-browse-share-snapshots). Klik met de rechtermuisknop op de share-momentopname die u wilt verwijderen en selecteer Verwijderen.

## <a name="clean-up-resources"></a>Resources opschonen
In Azure Storage Explorer is het niet mogelijk om resources te verwijderen van. Gebruik hiervoor [Azure Portal](https://portal.azure.com/). 

[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Volgende stappen
- [Bestandsshares beheren met Azure Portal](storage-how-to-use-files-portal.md)
- [Bestandsshares beheren met Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Bestandsshares beheren met Azure CLI](storage-how-to-use-files-cli.md)
- [Implementatie van Azure Files plannen](storage-files-planning.md)