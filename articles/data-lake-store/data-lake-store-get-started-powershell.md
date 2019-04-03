---
title: PowerShell gebruiken voor het aan de slag met Azure Data Lake Storage Gen1 | Microsoft Docs
description: Azure PowerShell gebruiken voor een Azure Data Lake Storage Gen1-account maken en basisbewerkingen uitvoert
services: data-lake-store
documentationcenter: ''
author: twooley
manager: mtillman
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 06/27/2018
ms.author: twooley
ms.openlocfilehash: 6635b572d11339307083b8aa2aaaba69dc731101
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58885856"
---
# <a name="get-started-with-azure-data-lake-storage-gen1-using-azure-powershell"></a>Aan de slag met Azure Data Lake Storage Gen1 met behulp van Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure-CLI](data-lake-store-get-started-cli-2.0.md)
>
>

[!INCLUDE [data-lake-storage-gen1-rename-note.md](../../includes/data-lake-storage-gen1-rename-note.md)]

Leer hoe u met Azure PowerShell een Azure Data Lake Storage Gen1-account maken en uitvoeren van basisbewerkingen, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account, enzovoort. Zie voor meer informatie over Data Lake Storage Gen1 [overzicht van Data Lake Storage Gen1](data-lake-store-overview.md).

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

## <a name="authentication"></a>Authentication
In dit artikel wordt een eenvoudigere verificatiemethode met Data Lake Storage Gen1 waarbij u wordt gevraagd de referenties van uw Azure-account in te voeren. Het toegangsniveau voor Data Lake Storage Gen1 account en bestandssysteem wordt vervolgens bepaald door het toegangsniveau van de aangemelde gebruiker. Er zijn echter andere manieren ook om te verifiëren met Data Lake Storage Gen1, zoals **verificatie door eindgebruikers** of **service-naar-serviceverificatie**. Zie [Verificatie door eindgebruikers](data-lake-store-end-user-authenticate-using-active-directory.md) of [Service-to-serviceverificatie](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.

## <a name="create-a-data-lake-storage-gen1-account"></a>Een Data Lake Storage Gen1-account maken
1. Open een nieuw Windows PowerShell-venster via het bureaublad. Voer het volgende fragment Meld u aan bij uw Azure-account, het abonnement instellen en registreren van de Data Lake Storage Gen1-provider. Wanneer u hierom wordt gevraagd om aan te melden, zorg er dan voor dat u zich aanmeldt als een van de beheerders/eigenaar van het abonnement:

        # Log in to your Azure account
        Connect-AzAccount

        # List all the subscriptions associated to your account
        Get-AzSubscription

        # Select a subscription
        Set-AzContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Storage Gen1
        Register-AzResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Een Data Lake Storage Gen1-account is gekoppeld aan een Azure-resourcegroep. Maak daarom eerst een Azure-resourcegroep.

        $resourceGroupName = "<your new resource group name>"
        New-AzResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Een Azure-resourcegroep maken](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Een Azure-resourcegroep maken")
3. Maak een Data Lake Storage Gen1-account. De naam die u opgeeft, mag alleen kleine letters en cijfers bevatten.

        $dataLakeStorageGen1Name = "<your new Data Lake Storage Gen1 account name>"
        New-AzDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStorageGen1Name -Location "East US 2"

    ![Maak een account voor Data Lake Storage Gen1](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "een Gen1 van Data Lake Storage-account maken")
4. Controleer of het account is gemaakt.

        Test-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name

    De uitvoer van de cmdlet moet **waar** zijn.

## <a name="create-directory-structures-in-your-data-lake-storage-gen1-account"></a>Mapstructuren maken in uw Data Lake Storage Gen1-account
U kunt mappen maken onder uw Data Lake Storage Gen1-account te beheren en opslaan van gegevens.

1. Geef een hoofdmap op.

        $myrootdir = "/"
2. Maak onder de opgegeven hoofdmap een nieuwe map met de naam **mynewdirectory**.

        New-AzDataLakeStoreItem -Folder -AccountName $dataLakeStorageGen1Name -Path $myrootdir/mynewdirectory
3. Controleer of de nieuwe map is gemaakt.

        Get-AzDataLakeStoreChildItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir

    De uitvoer moet overeenkomen met de volgende schermafbeelding:

    ![Map controleren](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Map controleren")

## <a name="upload-data-to-your-data-lake-storage-gen1-account"></a>Gegevens uploaden naar uw Data Lake Storage Gen1-account
U kunt uw gegevens uploaden naar Data Lake Storage Gen1 rechtstreeks op het hoogste niveau doen of naar een map die u in het account hebt gemaakt. In de codefragmenten in deze sectie ziet u hoe u voorbeeldgegevens uploadt naar de map (**mynewfolder**) die u in de voorgaande sectie hebt gemaakt.

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken. Download het bestand en sla het in een lokale map op uw computer op, bijvoorbeeld C:\sampledata\.

    Import-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-storage-gen1-account"></a>Wijzig de naam, downloaden en gegevens uit uw Data Lake Storage Gen1-account verwijderen
Als u de naam van een bestand wilt wijzigen, gebruikt u de volgende opdracht:

    Move-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Gebruik de volgende opdracht als u een bestand wilt uploaden:

    Export-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Gebruik de volgende opdracht als u een bestand wilt verwijderen:

    Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Wanneer dit wordt gevraagd, typt u **Y** om het item te verwijderen. Als u meer dan één bestand wilt verwijderen, kunt u alle paden, door komma's gescheiden, opgeven.

    Remove-AzDataLakeStoreItem -AccountName $dataLakeStorageGen1Name -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-data-lake-storage-gen1-account"></a>Uw Data Lake Storage Gen1-account verwijderen
Gebruik de volgende opdracht om uw Gen1 van Data Lake Storage-account te verwijderen.

    Remove-AzDataLakeStoreAccount -Name $dataLakeStorageGen1Name

Wanneer dit wordt gevraagd, typt u **Y** om het account te verwijderen.

## <a name="next-steps"></a>Volgende stappen
* [Richtlijnen voor het gebruik van PowerShell met Azure Data Lake Storage Gen1 afstemmen van prestaties](data-lake-store-performance-tuning-powershell.md)
* [Azure Data Lake Storage Gen1 gebruiken voor big data-vereisten](data-lake-store-data-scenarios.md) 
* [Bescherm uw gegevens in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)
