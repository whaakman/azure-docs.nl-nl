---
title: Aan de slag met Azure Data Lake Store met PowerShell | Microsoft Docs
description: Azure Powershell gebruiken om een Data Lake Store-account te maken en basisbewerkingen uit te voeren
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: bf85f369-f9aa-4ca1-9ae7-e03a78eb7290
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 31c98b930ccb8203316e3a3b13c3c0dc7d74dd9c
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Aan de slag met Azure Data Lake Store met Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
>

Lees hoe u met Azure PowerShell een Azure Data Lake Store-account maakt en basisbewerkingen uitvoert, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account enzovoort. Zie [Overzicht van Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake Store.

## <a name="prerequisites"></a>Vereisten

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azure/overview).

## <a name="authentication"></a>Verificatie
In dit artikel wordt een eenvoudigere verificatiemethode met Data Lake Store gebruikt, waarbij u wordt gevraagd de referenties van uw Azure-account in te voeren. Het toegangsniveau voor het account en bestandssysteem van Data Lake Store wordt vervolgens bepaald door het toegangsniveau van de aangemelde gebruiker. Er zijn echter ook andere manieren om te verifiëren in Data Lake Store, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Verificatie door eindgebruikers](data-lake-store-end-user-authenticate-using-active-directory.md) of [Service-to-serviceverificatie](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken
1. Open een nieuw Windows PowerShell-venster via het bureaublad. Voer het volgende codefragment in om u aan te melden bij uw Azure-account, stel het abonnement in en registreer de Data Lake Store-provider. Wanneer u wordt gevraagd om u aan te melden, zorg dan dat u zich aanmeldt als een van de beheerders/eigenaars van het abonnement:

        # Log in to your Azure account
        Login-AzureRmAccount

        # List all the subscriptions associated to your account
        Get-AzureRmSubscription

        # Select a subscription
        Set-AzureRmContext -SubscriptionId <subscription ID>

        # Register for Azure Data Lake Store
        Register-AzureRmResourceProvider -ProviderNamespace "Microsoft.DataLakeStore"
2. Een Azure Data Lake Store-account is gekoppeld aan een Azure-resourcegroep. Maak daarom eerst een Azure-resourcegroep.

        $resourceGroupName = "<your new resource group name>"
        New-AzureRmResourceGroup -Name $resourceGroupName -Location "East US 2"

    ![Een Azure-resourcegroep maken](./media/data-lake-store-get-started-powershell/ADL.PS.CreateResourceGroup.png "Een Azure-resourcegroep maken")
3. Maak een Azure Data Lake Store-account. De naam die u opgeeft, mag alleen kleine letters en cijfers bevatten.

        $dataLakeStoreName = "<your new Data Lake Store name>"
        New-AzureRmDataLakeStoreAccount -ResourceGroupName $resourceGroupName -Name $dataLakeStoreName -Location "East US 2"

    ![Een Azure Data Lake Store-account maken](./media/data-lake-store-get-started-powershell/ADL.PS.CreateADLAcc.png "Een Azure Data Lake Store-account maken")
4. Controleer of het account is gemaakt.

        Test-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

    De uitvoer van de cmdlet moet **waar** zijn.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Mapstructuren maken in uw Azure Data Lake Store
U kunt onder uw Azure Data Lake Store-account mappen maken om gegevens te beheren en op te slaan.

1. Geef een hoofdmap op.

        $myrootdir = "/"
2. Maak onder de opgegeven hoofdmap een nieuwe map met de naam **mynewdirectory**.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Controleer of de nieuwe map is gemaakt.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    De uitvoer moet overeenkomen met de volgende schermafbeelding:

    ![Map controleren](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Map controleren")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Gegevens uploaden naar uw Azure Data Lake Store
Als u gegevens uploadt naar Data Lake Store, kunt u dat direct naar het hoogste niveau doen of naar een map die u in het account hebt gemaakt. In de codefragmenten in deze sectie ziet u hoe u voorbeeldgegevens uploadt naar de map (**mynewfolder**) die u in de voorgaande sectie hebt gemaakt.

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken. Download het bestand en sla het in een lokale map op uw computer op, bijvoorbeeld C:\sampledata\.

    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "C:\sampledata\vehicle1_09142014.csv" -Destination $myrootdir\mynewdirectory\vehicle1_09142014.csv


## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Gegevens in uw Data Lake Store een nieuwe naam geven, downloaden en verwijderen
Als u de naam van een bestand wilt wijzigen, gebruikt u de volgende opdracht:

    Move-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014.csv -Destination $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Gebruik de volgende opdracht als u een bestand wilt uploaden:

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv -Destination "C:\sampledata\vehicle1_09142014_Copy.csv"

Gebruik de volgende opdracht als u een bestand wilt verwijderen:

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014_Copy.csv

Wanneer dit wordt gevraagd, typt u **Y** om het item te verwijderen. Als u meer dan één bestand wilt verwijderen, kunt u alle paden, door komma's gescheiden, opgeven.

    Remove-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Paths $myrootdir\mynewdirectory\vehicle1_09142014.csv, $myrootdir\mynewdirectoryvehicle1_09142014_Copy.csv

## <a name="delete-your-azure-data-lake-store-account"></a>Uw Azure Data Lake Store-account verwijderen
Gebruik de volgende opdracht om uw Data Lake Store-account te verwijderen.

    Remove-AzureRmDataLakeStoreAccount -Name $dataLakeStoreName

Wanneer dit wordt gevraagd, typt u **Y** om het account te verwijderen.

## <a name="next-steps"></a>Volgende stappen
* [Richtlijnen voor het afstemmen van de prestaties voor het gebruik van PowerShell met Azure Data Lake Store](data-lake-store-performance-tuning-powershell.md)
* [Azure Data Lake Store gebruiken voor big data-vereisten](data-lake-store-data-scenarios.md) 
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)