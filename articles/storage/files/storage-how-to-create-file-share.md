---
title: Een Azure-bestandsshare maken | Microsoft Docs
description: Informatie over het maken van een Azure-bestandsshare in Azure Files met behulp van de Azure Portal, PowerShell en de Azure CLI.
services: storage
author: RenaShahMSFT
ms.service: storage
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: renash
ms.component: files
ms.openlocfilehash: 318ecf03853e151040622ef0863f4c6986a96bd2
ms.sourcegitcommit: c94cf3840db42f099b4dc858cd0c77c4e3e4c436
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53628507"
---
# <a name="create-a-file-share-in-azure-files"></a>Een bestandsshare maken in Azure Files
U kunt Azure-bestandsshares maken met de  [Azure-portal](https://portal.azure.com/), de PowerShell-cmdlets van Azure Storage, de clientbibliotheken van Azure Storage of de REST API van Azure Storage. In deze zelfstudie leert u het volgende:
* [Een Azure-bestandsshare maken met Azure Portal](#create-file-share-through-the-azure-portal)
* [Een Azure-bestandsshare maken met Powershell](#create-file-share-through-powershell)
* [Een Azure-bestandsshare maken met de CLI](#create-file-share-through-command-line-interface-cli)

## <a name="prerequisites"></a>Vereisten
Voor het maken van een Azure-bestandsshare kunt u een opslagaccount gebruiken dat al bestaat of [een nieuw Azure Storage-account maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u een Azure-bestandsshare wilt maken met PowerShell, hebt u de sleutel en de naam van uw opslagaccount nodig. U hebt een Azure Storage-accountsleutel nodig als u van plan bent Powershell of de CLI te gebruiken.

## <a name="create-a-file-share-through-the-azure-portal"></a>Een bestandsshare maken via de Azure-portal
1. **Ga in de Azure-portal naar de blade Opslagaccount**:    
    ![Blade Opslagaccount](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Klik op de knop Bestandsshare toevoegen**:    
    ![Klik op de knop Bestandsshare toevoegen](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Geef de naam en een quotum op. Op dit moment is de maximale waarde van het quotum van 5 TiB**:    
    ![Geef een naam en een gewenst quotum op voor de nieuwe bestandsshare](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Bekijk de nieuwe bestandsshare**:  ![Nieuwe bestandsshare bekijken](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Een bestand uploaden**:  ![Bestand uploaden](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Blader naar de bestandsshare en beheer uw mappen en bestanden**:  ![Bladeren naar bestandsshare](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Een bestandsshare via de PowerShell maken

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

U bereidt het gebruik van PowerShell voor door de Azure PowerShell-cmdlets te downloaden en te installeren. Zie  [Azure PowerShell installeren en configureren](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) voor het installatiepunt en de installatie-instructies.

> [!Note]  
> Het wordt aanbevolen om de meest recente Azure PowerShell-module te downloaden en te installeren, of hiernaar te upgraden.

1. **Een context maken voor uw opslagaccount en sleutel** De context bevat de naam van het opslagaccount en de accountsleutel. Zie  [Storage account access keys](../common/storage-account-manage.md#access-keys) (Toegangssleutels voor Azure Storage-accounts) voor instructies voor het kopiëren van de accountsleutel uit de  [Azure-portal](https://portal.azure.com/).

    ```powershell
    $storageContext = New-AzStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Maak een nieuwe bestandsshare**:    
    
    ```powershell
    $share = New-AzStorageShare logs -Context $storageContext
    ```

> [!Note]  
> De naam van de bestandsshare mag alleen uit kleine letters bestaan. Zie  [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="create-file-share-through-command-line-interface-cli"></a>Een bestandsshare via de opdrachtregelinterface (CLI) maken
1. **Download en installeer de Azure CLI als voorbereiding op het gebruiken van een opdrachtregelinterface (CLI).**  
    Zie  [Azure CLI installeren](https://docs.microsoft.com/cli/azure/install-azure-cli) en [Aan de slag met de Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli).

2. **Maak een verbindingsreeks naar het opslagaccount op de locatie waar u de share wilt maken.**  
    Vervang  ```<storage-account>``` en ```<resource_group>``` in het volgende voorbeeld door de naam en resourcegroep van uw opslagaccount:

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. **De bestandsshare maken**
    ```azurecli
    az storage share create --name files --quota 2048 --connection-string $current_env_conn_string 1 > /dev/null
    ```

## <a name="next-steps"></a>Volgende stappen
* [Bestandsshare verbinden en koppelen - Windows](storage-how-to-use-files-windows.md)
* [Bestandsshare verbinden en koppelen - Linux](../storage-how-to-use-files-linux.md)
* [Bestandsshare verbinden en koppelen - Mac OS](storage-how-to-use-files-mac.md)

Raadpleeg de volgende koppelingen voor meer informatie over Azure Files.

* [Veelgestelde vragen](../storage-files-faq.md)
* [Problemen oplossen in Windows](storage-troubleshoot-windows-file-connection-problems.md)      
* [Problemen oplossen in Linux](storage-troubleshoot-linux-file-connection-problems.md)   
