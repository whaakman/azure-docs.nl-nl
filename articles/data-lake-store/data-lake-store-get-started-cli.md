---
title: De opdrachtregelinterface gebruiken om aan de slag te gaan met Azure Data Lake Store | Microsoft Docs
description: De platformoverschrijdende opdrachtregel van Azure gebruiken voor het maken van een Data Lake Store-account en uitvoeren van basisbewerkingen
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 380788f3-041d-4ae5-b6be-37ca74ca333d
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 03/17/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: 988e7fe2ae9f837b661b0c11cf30a90644085e16
ms.openlocfilehash: f7748dba30c6e0332c166feda25f4aaa93c06efa
ms.lasthandoff: 04/06/2017


---
# <a name="get-started-with-azure-data-lake-store-using-azure-command-line"></a>Aan de slag met Azure Data Lake Store met de Azure-opdrachtregelinterface
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

Informatie over gebruik van de Azure-opdrachtregelinterface voor het maken van een Azure Data Lake Store-account en het uitvoeren van basisbewerkingen, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account, enzovoort. Zie [Overzicht van Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake Store.

De Azure CLI is geïmplementeerd in Node.js en kan worden gebruikt op elk platform dat ondersteuning biedt voor Node.js, zoals Windows, Mac en Linux. De Azure CLI is open-source. De broncode wordt beheerd in GitHub op <a href= "https://github.com/azure/azure-xplat-cli">https://github.com/azure/azure-xplat-cli</a>. Dit artikel behandelt alleen het gebruik van de Azure CLI met Data Lake Store. Raadpleeg voor algemene richtlijnen voor het gebruik van Azure CLI [De Azure CLI gebruiken][azure-command-line-tools].


> [!NOTE]
> Voor het uploaden en downloaden van grote hoeveelheden gegevens (grote bestanden, een groot aantal bestanden of beide), raden wij u aan om de [Python-SDK](data-lake-store-get-started-python.md), de [.NET-SDK](data-lake-store-get-started-net-sdk.md) of [Azure PowerShell](data-lake-store-get-started-powershell.md) te gebruiken. Met deze opties profiteert u van betere prestaties, omdat er meerdere threads worden gebruikt om de gegevensverplaatsing parallel te laten verlopen.
> 
>

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure CLI**: zie [De Azure CLI installeren en configureren](../cli-install-nodejs.md) voor informatie over de installatie en configuratie. Start de computer opnieuw op nadat u de CLI hebt geïnstalleerd.

## <a name="authentication"></a>Authentication

In dit artikel wordt een eenvoudigere verificatiemethode voor Data Lake Store gebruikt waarbij u zich als een eindgebruiker aanmeldt. Het toegangsniveau voor het account en bestandssysteem van Data Lake Store wordt vervolgens bepaald door het toegangsniveau van de aangemelde gebruiker. Er zijn echter ook andere manieren om te verifiëren in Data Lake Store, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Verifiëren met Data Lake Store met behulp van Azure Active Directory](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.

## <a name="login-to-your-azure-subscription"></a>Meld u aan bij uw Azure-abonnement

1. Volg de stappen die zijn beschreven in [Verbinding maken met een Azure-abonnement met de Azure-opdrachtregelinterface (Azure CLI)](../xplat-cli-connect.md) en maak verbinding met uw abonnement met behulp van de `azure login`-methode.

2. Gebruik de opdracht `azure account list` om de abonnementen weer te geven die aan uw account zijn gekoppeld.
   
        info:    Executing command account list
        data:    Name              Id                                    Current
        data:    ----------------  ------------------------------------  -------
        data:    Azure-sub-1       ####################################  true
        data:    Azure-sub-2       ####################################  false
   
    In de bovenstaande uitvoer is **Azure-sub-1** momenteel ingeschakeld, en het andere abonnement is **Azure-sub-2**. 
3. Selecteer het abonnement waarmee u wilt werken. Als u wilt werken met het abonnement Azure-sub-2, gebruikt u de opdracht `azure account set`.
   
        azure account set Azure-sub-2

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken
Open een opdrachtprompt, shell of terminalvenster en voer de volgende opdrachten uit.

1. Schakel over naar modus Azure Resource Manager met de volgende opdracht:
   
        azure config mode arm
2. Maak een nieuwe resourcegroep. Geef in de volgende opdracht de parameterwaarden op die u wilt gebruiken.
   
        azure group create <resourceGroup> <location>
   
    Als de locatienaam spaties bevat, moet deze tussen dubbele aanhalingstekens worden geplaatst. Bijvoorbeeld “VS-oost 2”.
3. Maak de Data Lake Store-account.
   
        azure datalake store account create <dataLakeStoreAccountName> <location> <resourceGroup>

## <a name="create-folders-in-your-data-lake-store"></a>Mappen maken in uw Data Lake Store
U kunt mappen maken onder uw Azure Data Lake Store-account voor het beheren en opslaan van gegevens. Gebruik de volgende opdracht om een map te maken met de naam ‘mynewfolder’ in de hoofdmap van de Data Lake Store.

    azure datalake store filesystem create <dataLakeStoreAccountName> <path> --folder

Bijvoorbeeld:

    azure datalake store filesystem create mynewdatalakestore /mynewfolder --folder

## <a name="upload-data-to-your-data-lake-store"></a>Gegevens uploaden naar uw Data Lake Store
Als u gegevens uploadt naar Data Lake Store, kunt u dat direct naar het hoogste niveau doen of naar een map die u in het account hebt gemaakt. De codefragmenten hieronder laten zien hoe u voorbeeldgegevens uploadt naar de map (**mynewfolder**) die u in de voorgaande sectie hebt gemaakt.

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken. Download het bestand en sla het in een lokale map op uw computer op, bijvoorbeeld C:\sampledata\.

    azure datalake store filesystem import <dataLakeStoreAccountName> "<source path>" "<destination path>"

Bijvoorbeeld:

    azure datalake store filesystem import mynewdatalakestore "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" "/mynewfolder/vehicle1_09142014.csv"


## <a name="list-files-in-data-lake-store"></a>Bestanden in Data Lake Store weergeven
Gebruik de volgende opdracht om de bestanden in een Data Lake Store-account weer te geven.

    azure datalake store filesystem list <dataLakeStoreAccountName> <path>

Bijvoorbeeld:

    azure datalake store filesystem list mynewdatalakestore /mynewfolder

De uitvoer ziet er ongeveer als volgt uit:

    info:    Executing command datalake store filesystem list
    data:    accessTime: 1446245025257
    data:    blockSize: 268435456
    data:    group: NotSupportYet
    data:    length: 1589881
    data:    modificationTime: 1446245105763
    data:    owner: NotSupportYet
    data:    pathSuffix: vehicle1_09142014.csv
    data:    permission: 777
    data:    replication: 0
    data:    type: FILE
    data:    ------------------------------------------------------------------------------------
    info:    datalake store filesystem list command OK

## <a name="rename-download-and-delete-data-from-your-data-lake-store"></a>Gegevens in uw Data Lake Store een nieuwe naam geven, downloaden en verwijderen
* **Als u de naam van een bestand wilt wijzigen**, gebruikt u de volgende opdracht:
  
        azure datalake store filesystem move <dataLakeStoreAccountName> <path/old_file_name> <path/new_file_name>
  
    Bijvoorbeeld:
  
        azure datalake store filesystem move mynewdatalakestore /mynewfolder/vehicle1_09142014.csv /mynewfolder/vehicle1_09142014_copy.csv
* **Als u een bestand wilt downloaden**, gebruikt u de volgende opdracht: Zorg ervoor dat het doelpad dat u opgeeft al bestaat.
  
        azure datalake store filesystem export <dataLakeStoreAccountName> <source_path> <destination_path>
  
    Bijvoorbeeld:
  
        azure datalake store filesystem export mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv "C:\mysampledata\vehicle1_09142014_copy.csv"
* **Als u een bestand wilt verwijderen**, gebruikt u de volgende opdracht:
  
        azure datalake store filesystem delete <dataLakeStoreAccountName> <path>
  
    Bijvoorbeeld:
  
        azure datalake store filesystem delete mynewdatalakestore /mynewfolder/vehicle1_09142014_copy.csv
  
    Wanneer dit wordt gevraagd, typt u **Y** om het item te verwijderen.

## <a name="view-the-access-control-list-for-a-folder-in-data-lake-store"></a>De toegangsbeheerlijst voor een map in Data Lake Store weergeven
Gebruik de volgende opdracht om de ACL's van een Data Lake Store-map weer te geven. In de huidige release kunnen ACL's alleen worden ingesteld voor de hoofdmap van de Data Lake Store. De onderstaande padparameter wordt dus altijd hoofdmap (/).

    azure datalake store permissions show <dataLakeStoreName> <path>

Bijvoorbeeld:

    azure datalake store permissions show mynewdatalakestore /


## <a name="delete-your-data-lake-store-account"></a>Uw Data Lake Store-account verwijderen
Gebruik de volgende opdracht om een Data Lake Store-account te verwijderen.

    azure datalake store account delete <dataLakeStoreAccountName>

Bijvoorbeeld:

    azure datalake store account delete mynewdatalakestore

Wanneer dit wordt gevraagd, typt u **Y** om het account te verwijderen.

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)

[azure-command-line-tools]: ../cli-install-nodejs.md

