---
title: Blob storage gegevens verplaatsen met Azure Storage Explorer - Team Data Science Process
description: Gegevens met Azure Storage Explorer verplaatsen van en naar Azure Blob-opslag
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/04/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: a77ad3ec954e77895cfab76a2f5fe2d822e08fb1
ms.sourcegitcommit: 698a3d3c7e0cc48f784a7e8f081928888712f34b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/31/2019
ms.locfileid: "55473167"
---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Gegevens verplaatsen naar en van Azure Blob Storage met behulp van Azure Storage Explorer
Azure Storage Explorer is een gratis hulpprogramma van Microsoft waarmee u werkt met Azure Storage-gegevens op Windows, macOS en Linux. In dit onderwerp wordt beschreven hoe u kunt gebruiken om te uploaden en downloaden van gegevens uit Azure blob-opslag. Het hulpprogramma kan worden gedownload vanaf [Microsoft Azure Storage Explorer](http://storageexplorer.com/).

[!INCLUDE [blob-storage-tool-selector](../../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Als u virtuele machine die is ingesteld met behulp van de scripts die is geleverd door [Data Science Virtual machines in Azure](virtual-machines.md), en vervolgens Azure Storage Explorer al is geïnstalleerd op de virtuele machine.
> 
> [!NOTE]
> Raadpleeg voor een volledige Inleiding tot Azure blob-opslag, [grondbeginselen van Azure Blob](../../storage/blobs/storage-dotnet-how-to-use-blobs.md) en [Azure Blob-Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Vereisten
Dit document wordt ervan uitgegaan dat u een Azure-abonnement, een storage-account en de bijbehorende opslagsleutel voor dat account hebt. Voordat u gegevens uploadt/downloadt, moet u uw Azure-opslag account naam en de accountsleutel weten. 

* Als u een Azure-abonnement instelt, Zie [gratis proefversie van één maand](https://azure.microsoft.com/pricing/free-trial/).
* Zie voor instructies over het maken van een storage-account en voor het ophalen van account en sleutelgegevens [over Azure storage-accounts](../../storage/common/storage-create-storage-account.md). Noteer de toegangssleutel voor uw storage-account, moet u deze sleutel verbinding maken met het account met het hulpprogramma Azure Storage Explorer.
* Het Azure Storage Explorer-hulpprogramma kan worden gedownload vanaf [Microsoft Azure Storage Explorer](http://storageexplorer.com/). Accepteer de standaardwaarden tijdens de installatie.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Azure Storage Explorer gebruiken
De volgende stappen uit het document over het uploaden/downloaden van gegevens met behulp van Azure Storage Explorer. 

1. Start Microsoft Azure Storage Explorer.
2. Om de **aanmelden bij uw account...**  wizard de optie **Azure-Accountinstellingen** pictogram, klikt u vervolgens **een account toevoegen** en geef de referenties. ![Een Azure storage-account toevoegen](./media/move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Om de **verbinding maken met Azure Storage** wizard selecteert u de **verbinding maken met Azure storage** pictogram. ![Klik op "Verbinding maken met Azure storage"](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Voer de toegangssleutel van uw Azure storage-account op de **verbinding maken met Azure Storage** wizard en vervolgens **volgende**. ![Toegangssleutel voor het invoeren van Azure storage-account](./media/move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Voer de naam van opslagaccount in de **accountnaam** vak en selecteer vervolgens **volgende**. ![Externe opslag koppelen](./media/move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Het opslagaccount toegevoegd moet nu worden weergegeven. Voor het maken van een blob-container in een opslagaccount, met de rechtermuisknop op de **Blobcontainers** knooppunt in het account, selecteer **Blob-Container maken**, en voer een naam.
7. Als u wilt gegevens uploaden naar een container, selecteert u de doelcontainer en klik op de **uploaden** knop.![ Storage-accounts](./media/move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klik op de **...**  aan de rechterkant van de **bestanden** Selecteer een of meerdere bestanden voor het uploaden van het bestandssysteem en klikt u op **uploaden** om te beginnen met uploaden van de bestanden.![ Bestanden uploaden](./media/move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Voor het downloaden van gegevens, het selecteren van de blob in de bijbehorende container om te downloaden en klikt u op **downloaden**. ![Bestanden downloaden](./media/move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)

