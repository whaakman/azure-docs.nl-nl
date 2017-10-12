---
title: Een Azure-bestandsshare maken | Microsoft Docs
description: Informatie over het maken van een Azure-bestandsshare in Azure Files met behulp van de Azure Portal, PowerShell en de Azure CLI.
services: storage
documentationcenter: 
author: RenaShahMSFT
manager: aungoo
editor: tysonn
ms.assetid: 
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 09/19/2017
ms.author: renash
ms.openlocfilehash: bc01e5427f32e9532e39694f6de9f0b1146eda35
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="create-a-file-share-in-azure-files"></a>Een bestandsshare maken in Azure Files
U kunt Azure-bestandsshares maken met [Azure Portal](https://portal.azure.com/), de PowerShell-cmdlets van Azure Storage, de clientbibliotheken van Azure Storage of de REST-API van Azure Storage. In deze zelfstudie leert u het volgende:
* [Een Azure-bestandsshare maken met de Azure Portal](#Create file share through the Portal)
* [Een Azure-bestandsshare maken met Powershell](#Create file share using PowerShell)
* [Een Azure-bestandsshare maken met de CLI](#create-file-share-using-command-line-interface-cli)

## <a name="prerequisites"></a>Vereisten
Voor het maken van een Azure-bestandsshare kunt u een opslagaccount gebruiken dat al bestaat of [een nieuw Azure Storage-account maken](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json). Als u een Azure-bestandsshare wilt maken met PowerShell, hebt u de sleutel en de naam van uw opslagaccount nodig. U hebt de sleutel van het opslagaccount nodig als u van plan bent Powershell of de CLI te gebruiken.

## <a name="create-file-share-through-the-azure-portal"></a>Een bestandsshare via de Azure Portal maken
1. **Ga naar de blade Opslagaccount in de Azure Portal**:    
    ![Blade Opslagaccount](./media/storage-how-to-create-file-share/create-file-share-portal1.png)

2. **Klik op de knop Bestandsshare toevoegen**:    
    ![Klik op de knop Bestandsshare toevoegen](./media/storage-how-to-create-file-share/create-file-share-portal2.png)

3. **Geef de naam en een quotum op. Het quotum heeft momenteel een maximum van 5 TiB**:    
    ![Geef een naam en een gewenst quotum op voor de nieuwe bestandsshare](./media/storage-how-to-create-file-share/create-file-share-portal3.png)

4. **Bekijk uw nieuwe bestandsshare**: ![Bekijk uw nieuwe bestandsshare](./media/storage-how-to-create-file-share/create-file-share-portal4.png)

5. **Upload een bestand**: ![Upload een bestand](./media/storage-how-to-create-file-share/create-file-share-portal5.png)

6. **Blader naar de bestandsshare en beheer uw mappen en bestanden**: ![Blader naar de bestandsshare](./media/storage-how-to-create-file-share/create-file-share-portal6.png)


## <a name="create-file-share-through-powershell"></a>Een bestandsshare via de PowerShell maken
U bereidt het gebruik van PowerShell voor door de Azure PowerShell-cmdlets te downloaden en te installeren. Zie [How to install and configure Azure PowerShell](https://azure.microsoft.com/documentation/articles/powershell-install-configure/) (Azure PowerShell installeren en configureren) voor het installatiepunt en de installatie-instructies.

> [!Note]  
> Het wordt aanbevolen om de meest recente Azure PowerShell-module te downloaden en te installeren, of hiernaar te upgraden.

1. **Een context maken voor uw opslagaccount en sleutel** De context bevat de naam van het opslagaccount en de accountsleutel. Voor instructies voor het kopiëren van de accountsleutel uit [Azure Portal](https://portal.azure.com/) raadpleegt u [Opslagtoegangssleutels bekijken en kopiëren](../common/storage-create-storage-account.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json#view-and-copy-storage-access-keys).

    ```powershell
    $storageContext = New-AzureStorageContext <storage-account-name> <storage-account-key>
    ```
    
2. **Maak een nieuwe bestandsshare**:    
    
    ```powershell
    $share = New-AzureStorageShare logs -Context $storageContext
    ```

> [!Note]  
> De naam van de bestandsshare mag alleen uit kleine letters bestaan. Zie [Naming and Referencing Shares, Directories, Files, and Metadata](https://msdn.microsoft.com/library/azure/dn167011.aspx) (Shares, mappen, bestanden en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over de naamgeving van bestandsshares en bestanden.

## <a name="create-file-share-through-command-line-interface-cli"></a>Een bestandsshare via de opdrachtregelinterface (CLI) maken
1. **Download en installeer de Azure CLI als voorbereiding op het gebruiken van een opdrachtregelinterface (CLI).**  
    Zie [Install Azure CLI 2.0](/cli/azure/install-az-cli2.md) (Azure CLI 2.0 installeren) en [Get started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md) (Aan de slag met Azure CLI 2.0).

2. **Maak een verbindingsreeks naar het opslagaccount op de locatie waar u de share wilt maken.**  
    Vervang ```<storage-account>``` en ```<resource_group>``` in het volgende voorbeeld door de naam en resourcegroep van uw opslagaccount:

   ```azurecli
    current_env_conn_string = $(az storage account show-connection-string -n <storage-account> -g <resource-group> --query 'connectionString' -o tsv)

    if [[ $current_env_conn_string == "" ]]; then  
        echo "Couldn't retrieve the connection string."
    fi
    ```

3. **Bestandsshare maken**
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