---
title: Gegevens verplaatsen naar en van Blob Storage met Azure Opslagverkenner | Microsoft Docs
description: Gegevens met Azure Storage Explorer verplaatsen van en naar Azure Blob-opslag
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/04/2017
ms.author: bradsev
ms.openlocfilehash: 3db06eed16191ef906f2c565d78fd0063cdb2f7b
ms.sourcegitcommit: 93902ffcb7c8550dcb65a2a5e711919bd1d09df9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2017
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Gegevens verplaatsen en naar Azure Blob Storage met behulp van Azure Storage Explorer
Azure Storage Explorer is een gratis hulpprogramma van Microsoft waarmee u werkt met Azure Storage-gegevens op Windows-, Mac OS- en Linux. In dit onderwerp wordt beschreven hoe u Hiermee uploaden en downloaden van gegevens uit Azure blob storage. Het hulpprogramma kan worden gedownload vanaf [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Als u virtuele machine die is ingesteld met de scripts die is geleverd door [gegevens wetenschappelijke virtuele machines in Azure](virtual-machines.md), en vervolgens Azure Opslagverkenner is al geïnstalleerd op de virtuele machine.
> 
> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [basisbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u een Azure-abonnement, een opslagaccount en de bijbehorende opslagsleutel voor dat account hebt. Voordat u gaat gegevens uploaden/downloaden, moet u weten Azure naam en sleutel van uw opslagaccount. 

* Als u een Azure-abonnement instelt, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Zie voor instructies over het maken van een opslagaccount en voor het ophalen van de account en sleutelgegevens [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Noteer de toegangssleutel voor uw storage-account als u deze sleutel verbinding maken met het account met het hulpprogramma Azure Opslagverkenner nodig.
* Het hulpprogramma Azure Opslagverkenner kan worden gedownload vanaf [Microsoft Azure Storage Explorer](http://storageexplorer.com/). Accepteer de standaardinstellingen tijdens de installatie.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Opslagverkenner gebruiken
De volgende stappen uit het document hoe uploaden/downloaden van gegevens met behulp van Azure Storage Explorer. 

1. Start Microsoft Azure Storage Explorer.
2. Online zetten van de **aanmelden bij uw account...**  wizard selecteert u **Azure Accountinstellingen** pictogram, klikt u vervolgens **account toevoegen** en geef de referenties. ![Een Azure storage-account toevoegen](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Online zetten van de **verbinding maken met Azure Storage** wizard selecteert u de **verbinding maken met Azure storage** pictogram. ![Verbinding maken met Azure-opslag](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Voer de toegangssleutel van uw Azure storage-account op de **verbinding maken met Azure Storage** wizard en vervolgens **volgende**. ![Verbinding maken met Azure-opslag](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Voer opslagaccountnaam in de **accountnaam** vak en selecteer vervolgens **volgende**. ![Externe opslag koppelen](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Het storage-account toegevoegd zou nu moeten worden vermeld. Voor het maken van een blob-container in een opslagaccount, met de rechtermuisknop op de **Blob-Containers** knooppunt in het account, selecteer **Blob-Container maken**, en voer een naam.
7. Als u wilt gegevens uploaden naar een container, selecteer de doelcontainer en klik op de **uploaden** knop.![ Storage-accounts](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klik op de **...**  rechts van de **bestanden** Selecteer een of meerdere bestanden voor het uploaden van het bestandssysteem en klikt u op **uploaden** om te beginnen met de bestanden te uploaden.![ Bestanden uploaden](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Voor het downloaden van gegevens, de blob selecteren in de bijbehorende container om te downloaden en klik op **downloaden**. ![Bestanden downloaden](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

