---
title: Azure-bestandsshares beheren met Azure Portal
description: Meer informatie over het gebruik van Azure Portal om Azure Files te beheren.
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
ms.date: 03/26/2018
ms.author: wgries
ms.openlocfilehash: 24996e80d2e75978a814bbf471176d4edcf22549
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38479169"
---
# <a name="managing-azure-file-shares-with-the-azure-portal"></a>Azure-bestandsshares beheren met Azure Portal 
[Azure Files ](storage-files-introduction.md) is het eenvoudig te gebruiken cloudbestandssysteem van Microsoft. Azure-bestandsshares kunnen worden gekoppeld in Windows, Linux en macOS. In deze handleiding worden de basisbeginselen besproken van het werken met Azure-bestandsshares met behulp van [Azure Portal](https://portal.azure.com/). Leer hoe u het volgende doet:

> [!div class="checklist"]
> * Een resourcegroep en een opslagaccount maken
> * Een Azure-bestandsshare maken 
> * Een map maken
> * Bestand uploaden 
> * Bestand downloaden
> * Een momentopname van een share maken en gebruiken

Als u nog geen abonnement op Azure hebt, kunt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) maken voordat u begint.

## <a name="create-a-storage-account"></a>Create a storage account
[!INCLUDE [storage-files-create-storage-account-portal](../../../includes/storage-files-create-storage-account-portal.md)]

## <a name="create-a-file-share"></a>Een bestandsshare maken
Een bestandsshare maken:

1. Selecteer het opslagaccount in uw dashboard.
2. Ga op de pagina van het opslagaccount naar het gedeelte **Services** en selecteer **Bestanden**.
    ![Een schermopname van het gedeelte Services van het opslagaccount, met de service Bestanden geselecteerd](media/storage-how-to-use-files-portal/create-file-share-1.png)

3. Klik in het menu bovenaan de pagina **Bestandsservice** op **+ Bestandsshare**. De pagina **Nieuwe bestandsshare** wordt weergegeven.
4. Typ *myshare* in het vak **Naam**.
5. Klik op **OK** om de Azure-bestandsshare te maken.

## <a name="work-with-the-contents-of-the-azure-file-share"></a>Werken met de inhoud van de Azure-bestandsshare
U beschikt nu over een Azure-bestandsshare. De volgende stap is het koppelen van de bestandsshare met SMB op [Windows](storage-how-to-use-files-windows.md), [Linux](storage-how-to-use-files-linux.md) of [macOS](storage-how-to-use-files-mac.md). U kunt ook met Azure Portal werken met uw Azure-bestandsshare. Alle aanvragen via Azure Portal worden gedaan via de REST-API van File, zodat u bestanden en mappen kunt maken, wijzigen en verwijderen in clients zonder toegang tot SMB.

### <a name="create-a-directory"></a>Een map maken
Ga als volgt te werk om in de hoofdmap van uw Azure-bestandsshare een nieuwe map te maken met de naam *myDirectory*:

1. Selecteer op de pagina **Bestandsservice** de bestandsshare **myshare**. De pagina voor de bestandsshare wordt geopend.
2. Selecteer **+ Map toevoegen** in het menu bovenaan de pagina. De pagina **Nieuwe map** wordt weergegeven.
3. Typ *myDirectory* en klik op **OK**.

### <a name="upload-a-file"></a>Bestand uploaden 
Als u wilt uitproberen hoe het uploaden van een bestand in zijn werk gaat, moet u eerst een bestand maken of selecteren om te uploaden. De manier waarop u dit doet, kunt u helemaal zelf bepalen. Ga als volgt te werk wanneer u het bestand hebt geselecteerd dat u wilt uploaden:

1. Klik op de map **myDirectory**. Het deelvenster **myDirectory** wordt geopend.
2. Klik in het menu bovenaan op **Uploaden**. Het deelvenster **Bestanden uploaden** wordt geopend.  
    ![Een schermopname van het deelvenster Bestanden uploaden](media/storage-how-to-use-files-portal/upload-file-1.png)

3. Klik op het mappictogram om een venster te openen waarin u door uw lokale bestanden kunt bladeren. 
4. Selecteer een bestand en klik op **Openen**. 
5. Controleer de bestandsnaam op de pagina **Bestanden uploaden** en klik vervolgens op **Uploaden**.
6. Als het uploaden is voltooid, ziet u het bestand in de lijst op de pagina **myDirectory**.

### <a name="download-a-file"></a>Bestand downloaden
U kunt een kopie downloaden van het bestand dat u hebt geüpload door de rechtermuisknop op het bestand te klikken. Als u vervolgens Downloaden hebt geselecteerd, verschilt de rest van de procedure afhankelijk van het besturingssysteem en browser die u gebruikt.

## <a name="create-and-modify-share-snapshots"></a>Momentopnamen van shares maken en wijzigen
Nog een andere handige taak die u kunt doen met een Azure-bestandsshare is het maken van een momentopname van de share. Een momentopname bevat voor een specifiek moment de actuele inhoud van een Azure-bestandsshare. Momentopnamen van een share zijn vergelijkbaar met technologieën van besturingssystemen die u mogelijk al kent, zoals:
- [Volume Shadow Copy Service (VSS)](https://docs.microsoft.com/en-us/windows/desktop/VSS/volume-shadow-copy-service-portal) voor Windows-bestandssystemen zoals NTFS en ReFS
- Momentopnamen van [Logical Volume Manager (LVM)](https://en.wikipedia.org/wiki/Logical_Volume_Manager_(Linux)#Basic_functionality) voor Linux-systemen.
- Momentopnamen van [Apple File System (APFS)](https://developer.apple.com/library/content/documentation/FileManagement/Conceptual/APFS_Guide/Features/Features.html) voor macOS. 

Een momentopname van een share maken:

1. Open de pagina voor de bestandsshare door het opslagaccount te openen vanuit uw dashboard > **Bestanden** > **myshare**. 
2. Klik op de pagina voor de bestandsshare op de knop **Momentopname** in het menu bovenaan de pagina en selecteer vervolgens **En momentopname maken**.  
    ![Een schermopname met de knop Een momentopname maken](media/storage-how-to-use-files-portal/create-snapshot-1.png)

### <a name="list-and-browse-share-snapshots"></a>Momentopnamen van shares weergeven en er doorheen bladeren
Als de momentopname is gemaakt, klikt u nogmaals op **Momentopname** en selecteert u vervolgens **Momentopnamen weergeven** om een lijst te zien met de momentopnamen voor de share. Er wordt dan een deelvenster geopend met alle momentopnamen voor deze share. Klik op een momentopname van de share om er doorheen te bladeren.

### <a name="restore-from-a-share-snapshot"></a>Bestand herstellen vanuit een share-momentopname
Om te laten zien hoe u een bestand kunt herstellen vanuit een share-momentopname, moeten we eerst een bestand verwijderen uit de live Azure-bestandsshare. Navigeer naar de map *myDirectory*, klik met de rechtermuisknop op het bestand dat u hebt geüpload en klik vervolgens op **Verwijderen**. Ga nu als volgt te werk om dit bestand te herstellen vanuit de momentopname van de share:

1. Klik in het bovenste menu op **Momentopnamen** en selecteer **Momentopnamen weergeven**. 
2. Klik op de momentopname die u eerder hebt gemaakt om de inhoud op een nieuwe pagina weer te geven. 
3. Klik op **myDirectory** in de momentopname en u ziet het bestand dat u hebt verwijderd. 
4. Klik met de rechtermuisknop op het verwijderde bestand en selecteer **Herstellen**.
5. Er wordt een pop-upvenster weergegeven waarin u kunt aangeven of u het bestand als een kopie wilt herstellen of door het oorspronkelijke bestand te overschrijven. Aangezien we het oorspronkelijke bestand hebben verwijderd, kunnen we **Oorspronkelijke bestand overschrijven** selecteren om het bestand te herstellen zoals het was voordat het werd verwijderd. Klik op **OK** om het bestand terug te zetten in de Azure-bestandsshare.  
    ![Een schermopname van het dialoogvenster voor het herstellen van een bestand](media/storage-how-to-use-files-portal/restore-snapshot-1.png)

6. Als het bestand is hersteld, sluit u de pagina voor de momentopname en gaat u terug naar **myshare** > **myDirectory**. Het bestand moet daar weer op de oorspronkelijke locatie worden weergegeven.

### <a name="delete-a-share-snapshot"></a>Een share-momentopname verwijderen
Als u een momentopname van een share wilt verwijderen, [gaat u naar de lijst met share-momentopnamen](#list-and-browse-a-share-snapshot). Schakel het selectievakje naast de naam van de momentopname van de share in en selecteer de knop **Verwijderen**.

![Een schermopname van het verwijderen van een momentopname van een share](media/storage-how-to-use-files-portal/delete-snapshot-1.png)

## <a name="clean-up-resources"></a>Resources opschonen
[!INCLUDE [storage-files-clean-up-portal](../../../includes/storage-files-clean-up-portal.md)]

## <a name="next-steps"></a>Volgende stappen
- [Bestandsshares beheren met Azure PowerShell](storage-how-to-use-files-powershell.md)
- [Bestandsshares beheren met Azure CLI](storage-how-to-use-files-cli.md)
- [Bestandsshares beheren met Azure Storage Explorer](storage-how-to-use-files-storage-explorer.md)
- [Implementatie van Azure Files plannen](storage-files-planning.md)