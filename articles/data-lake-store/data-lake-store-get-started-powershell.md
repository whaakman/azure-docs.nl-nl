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
ms.date: 03/06/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: c00aa4a2e79522a6817a135965f6c218b08e26f8
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-powershell"></a>Aan de slag met Azure Data Lake Store met Azure PowerShell
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [.NET-SDK](data-lake-store-get-started-net-sdk.md)
> * [Java-SDK](data-lake-store-get-started-java-sdk.md)
> * [REST-API](data-lake-store-get-started-rest-api.md)
> * [Azure CLI](data-lake-store-get-started-cli.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
> * [Node.js](data-lake-store-manage-use-nodejs.md)
> * [Python](data-lake-store-get-started-python.md)
>
>

Lees hoe u met Azure PowerShell een Azure Data Lake Store-account maakt en basisbewerkingen uitvoert, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account enzovoort. Zie [Overzicht van Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake Store.

## <a name="prerequisites"></a>Vereisten
Voordat u met deze zelfstudie begint, moet u het volgende hebben of hebben gedaan:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure PowerShell 1.0 of hoger**. Zie [Azure PowerShell installeren en configureren](/powershell/azureps-cmdlets-docs).

## <a name="authentication"></a>Authentication
In dit artikel wordt een eenvoudigere verificatiemethode met Data Lake Store gebruikt, waarbij u wordt gevraagd de referenties van uw Azure-account in te voeren. Het toegangsniveau voor het account en bestandssysteem van Data Lake Store wordt vervolgens bepaald door het toegangsniveau van de aangemelde gebruiker. Er zijn echter ook andere manieren om te verifiëren in Data Lake Store, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Verifiëren met Data Lake Store met behulp van Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken
1. Open vanaf het bureaublad een nieuw Windows PowerShell-venster en voer het volgende fragment in om u aan te melden bij uw Azure-account, het abonnement in te stellen en de Data Lake Store-provider te registreren. Wanneer u wordt gevraagd om u aan te melden, zorg dan dat u zich aanmeldt als een van de beheerders/eigenaars van het abonnement:

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

    De uitvoer hiervan moet **waar** zijn.

## <a name="create-directory-structures-in-your-azure-data-lake-store"></a>Mapstructuren maken in uw Azure Data Lake Store
U kunt onder uw Azure Data Lake Store-account mappen maken om gegevens te beheren en op te slaan.

1. Geef een hoofdmap op.

        $myrootdir = "/"
2. Maak onder de opgegeven hoofdmap een nieuwe map met de naam **mynewdirectory**.

        New-AzureRmDataLakeStoreItem -Folder -AccountName $dataLakeStoreName -Path $myrootdir/mynewdirectory
3. Controleer of de nieuwe map is gemaakt.

        Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -Path $myrootdir

    De volgende uitvoer moet worden weergegeven:

    ![Map controleren](./media/data-lake-store-get-started-powershell/ADL.PS.Verify.Dir.Creation.png "Map controleren")

## <a name="upload-data-to-your-azure-data-lake-store"></a>Gegevens uploaden naar uw Azure Data Lake Store
Als u gegevens uploadt naar Data Lake Store, kunt u dat direct naar het hoogste niveau doen of naar een map die u in het account hebt gemaakt. In de codefragmenten hieronder ziet u hoe u voorbeeldgegevens uploadt naar de map (**mynewfolder**) die u in de voorgaande sectie hebt gemaakt.

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

## <a name="performance-guidance-while-using-powershell"></a>Prestatierichtlijnen bij het gebruik van PowerShell

Hieronder volgen de belangrijkste instellingen die u kunt aanpassen voor de beste prestaties wanneer u PowerShell gebruikt met Data Lake Store:

| Eigenschap            | Standaard | Beschrijving |
|---------------------|---------|-------------|
| PerFileThreadCount  | 10      | Met deze parameter kunt u het aantal parallelle threads voor het uploaden of downloaden van elk bestand kiezen. Dit getal staat voor het maximumaantal threads dat per bestand kan worden toegevoegd, maar afhankelijk van uw scenario kunnen er minder threads zijn (als u bijvoorbeeld een bestand van 1 KB uploadt, krijgt u één thread, ook als u vraagt om 20 threads).  |
| ConcurrentFileCount | 10      | Deze parameter is specifiek bedoeld voor het uploaden en downloaden van mappen. De parameter bepaalt het aantal bestanden dat tegelijk kan worden geüpload of gedownload. Dit getal staat voor het maximumaantal bestanden dat tegelijk kan worden geüpload of gedownload, maar afhankelijk van uw scenario kunnen er wel minder bestanden tegelijk worden geüpload of gedownload (als u bijvoorbeeld twee bestanden uploadt, krijgt u twee gelijktijdige bestandsuploads, ook als u vraagt om 15). |

**Voorbeeld**

Met deze opdracht worden bestanden vanuit Azure Data Lake Store naar de lokale schijf van de gebruiker gedownload met 20 threads per bestand en 100 bestanden tegelijk.

    Export-AzureRmDataLakeStoreItem -AccountName <Data Lake Store account name> -PerFileThreadCount 20-ConcurrentFileCount 100 -Path /Powershell/100GB/ -Destination C:\Performance\ -Force -Recurse

### <a name="how-do-i-determine-the-value-to-set-for-these-parameters"></a>Hoe bepaal ik welke waarden ik moet instellen voor deze parameters?

Hier volgen een aantal richtlijnen.

* **Stap 1: bepaal het totale threadaantal** - begin door uit te rekenen wat het totale threadaantal is dat u wilt gebruiken. Een algemene richtlijn is 6 threads voor elke fysieke kern.

        Total thread count = total physical cores * 6

    **Voorbeeld**

    We nemen hier aan dat u de PowerShell-opdrachten uitvoert vanaf een D14-VM met 16 kernen

        Total thread count = 16 cores * 6 = 96 threads


* **Stap 2: bereken de PerFileThreadCount** - we berekenen de PerFileThreadCount op basis van de grootte van de bestanden. Voor bestanden die kleiner zijn dan 2,5 GB is het niet nodig om deze parameter te wijzigen, omdat de standaardwaarde van 10 voldoende is. Bij bestanden die groter zijn dan 2,5 GB, gebruikt u de eerste 10 threads als basis voor de eerste 2,5 GB en voegt u 1 thread toe voor elke toename in bestandsgrootte van 256 MB. Als u een map kopieert met bestanden van zeer verschillende groottes, kunt u overwegen ze op vergelijkbare grootte te sorteren. Grote verschillen in bestandsgroottes kunnen tot slechtere prestaties leiden. Als het niet mogelijk is om bestanden op grootte te sorteren, dient u de PerFileThreadCount in te stellen op basis van het grootste bestand.

        PerFileThreadCount = 10 threads for the first 2.5GB + 1 thread for each additional 256MB increase in file size

    **Voorbeeld**

    Als we hier aannemen dat u 100 bestanden hebt met groottes van 1 GB tot 10 GB, gebruiken we het grootste bestand van 10 GB voor de berekening. Deze ziet er als volgt uit.

        PerFileThreadCount = 10 + ((10GB - 2.5GB) / 256MB) = 40 threads

* **Stap 3: bereken de ConcurrentFilecount** - gebruik het totaalaantal threads en de PerFileThreadCount om de ConcurrentFileCount te berekenen met de volgende berekening.

        Total thread count = PerFileThreadCount * ConcurrentFileCount

    **Voorbeeld**

    We gaan uit van de gebruikte voorbeeldwaarden

        96 = 40 * ConcurrentFileCount

    De **ConcurrentFileCount** is **2,4**, wat we kunnen afronden naar **2**.

### <a name="further-tuning"></a>Verder afstemmen

Mogelijk is verder afstemmen nodig omdat u werkt met bestanden van uiteenlopende groottes. De bovenstaande berekening werkt goed als alle of de meeste bestanden ongeveer 10 GB zijn. Als het echter bestanden van zeer uiteenlopende groottes betreft, waarbij veel bestanden kleiner zijn, kunt u de PerFileThreadCount verlagen. Als we de PerFileThreadCount verlagen, kunnen we de ConcurrentFileCount verhogen. Als we er vanuit gaan dat de meeste bestanden kleiner dan 5 GB zijn, kunnen we onze berekening opnieuw doen:

    PerFileThreadCount = 10 + ((5GB - 2.5GB) / 256MB) = 20

De **ConcurrentFileCount** is nu 96/20 = 4,8, wat kan worden afgerond naar **4**.

U kunt deze instellingen blijven aanpassen door de **PerFileThreadCount** te verhogen of te verlagen, afhankelijk van de verdeling van de bestandsgroottes.

### <a name="limitation"></a>Beperking

* **Het aantal bestanden is lager dan de ConcurrentFileCount**: als het aantal bestanden dat u uploadt lager is dan de **ConcurrentFileCount** die u hebt berekend, dient u de **ConcurrentFileCount** te verlagen tot deze gelijk is met het aantal bestanden. U kunt eventuele overblijvende threads gebruiken om de **PerFileThreadCount** te verhogen.

* **Te veel threads**: als u het aantal threads te veel verhoogt zonder dat u de grootte van uw cluster verhoogt, kan dit tot slechtere prestaties leiden. Er treden mogelijk conflicten op wanneer u van context wisselt op de CPU.

* **Onvoldoende gelijktijdigheid**: als de gelijktijdigheid onvoldoende is, is uw cluster mogelijk te klein. U kunt het aantal knooppunten in uw cluster verhogen, zodat u profiteert van meer gelijktijdigheid.

* **Beperkingsfouten**: er treden mogelijk beperkingsfouten op als uw gelijktijdigheid te hoog is. Als er beperkingsfouten optreden, dient u uw gelijktijdigheid te verkleinen of contact met ons op te nemen.

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)


