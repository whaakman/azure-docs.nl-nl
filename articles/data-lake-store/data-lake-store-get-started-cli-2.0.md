---
title: De opdrachtregelinterface van Azure 2.0 gebruiken om aan de slag te gaan met Azure Data Lake Store | Microsoft Docs
description: Een Data Lake Store-account maken en basisbewerkingen uitvoeren met de platformoverschrijdende opdrachtregelinterface van Azure 2.0
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: 4ffa0f4a-1cca-46ac-803d-1fc8538c685b
ms.service: data-lake-store
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 976ddf25a57dd1eff66e121c5a66bc31f664a9dc
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="get-started-with-azure-data-lake-store-using-azure-cli-20"></a>Aan de slag met Azure Data Lake Store met Azure CLI 2.0
> [!div class="op_single_selector"]
> * [Portal](data-lake-store-get-started-portal.md)
> * [PowerShell](data-lake-store-get-started-powershell.md)
> * [Azure CLI 2.0](data-lake-store-get-started-cli-2.0.md)
>
> 

Ontdek hoe u met de Azure CLI 2.0 een Azure Data Lake Store-account maakt en basisbewerkingen uitvoert, zoals het maken van mappen, uploaden en downloaden van gegevensbestanden, verwijderen van uw account enzovoort. Zie [Overzicht van Data Lake Store](data-lake-store-overview.md) voor meer informatie over Data Lake Store.

De Azure CLI 2.0 is de nieuwe opdrachtregelervaring van Azure voor het beheer van Azure-resources. Deze kan worden gebruikt in Mac OS, Linux en Windows. Zie [Overview of Azure CLI 2.0](https://docs.microsoft.com/cli/azure/overview) (Overzicht van Azure CLI 2.0) voor meer informatie. U kunt ook zoeken in de [Naslaggegevens van Azure Data Lake Store CLI 2.0](https://docs.microsoft.com/cli/azure/dls), voor een volledige lijst met opdrachten en syntaxis.


## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).

* **Azure CLI 2.0**: zie [Install Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli) (Azure CLI 2.0 installeren) voor instructies.

## <a name="authentication"></a>Verificatie

In dit artikel wordt een eenvoudigere verificatiemethode voor Data Lake Store gebruikt waarbij u zich als een eindgebruiker aanmeldt. Het toegangsniveau voor het account en bestandssysteem van Data Lake Store wordt vervolgens bepaald door het toegangsniveau van de aangemelde gebruiker. Er zijn echter ook andere manieren om te verifiëren in Data Lake Store, zoals **verificatie door eindgebruikers** en **service-naar-serviceverificatie**. Zie [Eindgebruikersverificatie](data-lake-store-end-user-authenticate-using-active-directory.md) of [Service-to-serviceverificatie](data-lake-store-authenticate-using-active-directory.md) voor instructies en meer informatie over verificatie.


## <a name="log-in-to-your-azure-subscription"></a>Aanmelden bij uw Azure-abonnement

1. Meld u aan bij uw Azure-abonnement.

    ```azurecli
    az login
    ```

    U ontvangt een code die u in de volgende stap nodig hebt. Gebruik een webbrowser om de pagina https://aka.ms/devicelogin te openen en voer de code in voor verificatie. U wordt gevraagd om u aan te melden met uw referenties.

2. Wanneer u bent aangemeld, wordt er een venster weergegeven met alle Azure-abonnementen die aan uw account zijn gekoppeld. Gebruik de volgende opdracht als u een specifiek abonnement wilt gebruiken.
   
    ```azurecli
    az account set --subscription <subscription id> 
    ```

## <a name="create-an-azure-data-lake-store-account"></a>Een Azure Data Lake Store-account maken

1. Maak een nieuwe resourcegroep. Geef in de volgende opdracht de parameterwaarden op die u wilt gebruiken. Als de locatienaam spaties bevat, moet deze tussen dubbele aanhalingstekens worden geplaatst. Bijvoorbeeld “VS-oost 2”. 
   
    ```azurecli
    az group create --location "East US 2" --name myresourcegroup
    ```

2. Maak de Data Lake Store-account.
   
    ```azurecli
    az dls account create --account mydatalakestore --resource-group myresourcegroup
    ```

## <a name="create-folders-in-a-data-lake-store-account"></a>Mappen maken in een Data Lake Store-account

U kunt mappen maken onder uw Azure Data Lake Store-account voor het beheren en opslaan van gegevens. Gebruik de volgende opdracht om in de hoofdmap van Data Lake Store een map te maken met de naam **mynewfolder**.

```azurecli
az dls fs create --account mydatalakestore --path /mynewfolder --folder
```

> [!NOTE]
> Met de parameter `--folder` geeft u aan dat u een map wilt maken. Zonder deze parameter maakt de opdracht een leeg bestand met de naam mynewfolder in de hoofdmap van het Data Lake Store-account.
> 
>

## <a name="upload-data-to-a-data-lake-store-account"></a>Gegevens uploaden naar een Data Lake Store-account

U kunt gegevens direct naar het hoogste niveau in Data Lake Store uploaden of naar een map die u in het account hebt gemaakt. De codefragmenten hieronder laten zien hoe u voorbeeldgegevens uploadt naar de map (**mynewfolder**) die u in de voorgaande sectie hebt gemaakt.

Als u nog geen voorbeeldgegevens hebt om te uploaden, kunt u de map **Ambulance Data** uit de [Azure Data Lake Git-opslagplaats](https://github.com/MicrosoftBigData/usql/tree/master/Examples/Samples/Data/AmbulanceData) gebruiken. Download het bestand en sla het in een lokale map op uw computer op, bijvoorbeeld C:\sampledata\.

```azurecli
az dls fs upload --account mydatalakestore --source-path "C:\SampleData\AmbulanceData\vehicle1_09142014.csv" --destination-path "/mynewfolder/vehicle1_09142014.csv"
```

> [!NOTE]
> Als bestemming moet u het volledige pad inclusief de bestandsnaam opgeven.
> 
>


## <a name="list-files-in-a-data-lake-store-account"></a>Bestanden in een Data Lake Store-account weergeven

Gebruik de volgende opdracht om de bestanden in een Data Lake Store-account weer te geven.

```azurecli
az dls fs list --account mydatalakestore --path /mynewfolder
```

De uitvoer ziet er ongeveer als volgt uit:

    [
        {
            "accessTime": 1491323529542,
            "aclBit": false,
            "blockSize": 268435456,
            "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "length": 1589881,
            "modificationTime": 1491323531638,
            "msExpirationTime": 0,
            "name": "mynewfolder/vehicle1_09142014.csv",
            "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
            "pathSuffix": "vehicle1_09142014.csv",
            "permission": "770",
            "replication": 1,
            "type": "FILE"
        }
    ]

## <a name="rename-download-and-delete-data-from-a-data-lake-store-account"></a>Gegevens in een Data Lake Store-account een nieuwe naam geven, downloaden en verwijderen 

* **Als u de naam van een bestand wilt wijzigen**, gebruikt u de volgende opdracht:
  
    ```azurecli
    az dls fs move --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014.csv --destination-path /mynewfolder/vehicle1_09142014_copy.csv
    ```

* **Als u een bestand wilt downloaden**, gebruikt u de volgende opdracht: Zorg ervoor dat het doelpad dat u opgeeft al bestaat.
  
    ```azurecli     
    az dls fs download --account mydatalakestore --source-path /mynewfolder/vehicle1_09142014_copy.csv --destination-path "C:\mysampledata\vehicle1_09142014_copy.csv"
    ```

    > [!NOTE]
    > Met de opdracht wordt de doelmap gemaakt als deze niet bestaat.
    > 
    >

* **Als u een bestand wilt verwijderen**, gebruikt u de volgende opdracht:
  
    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder/vehicle1_09142014_copy.csv
    ```

    Als u de map **mynewfolder** en het bestand **vehicle1_09142014_copy.csv** met één opdracht wilt verwijderen, gebruikt u de parameter --recurse

    ```azurecli
    az dls fs delete --account mydatalakestore --path /mynewfolder --recurse
    ```

## <a name="work-with-permissions-and-acls-for-a-data-lake-store-account"></a>Machtigingen en ACL's gebruiken voor een Data Lake Store-account

In deze sectie vindt u informatie over het beheer van ACL's en machtigingen met de Azure CLI 2.0. Zie [Toegangsbeheer in Azure Data Lake Store](data-lake-store-access-control.md) voor gedetailleerde informatie over de implementatie van ACL's in Azure Data Lake Store.

* Gebruik de volgende opdracht als u **de eigenaar van een bestand/map wilt bijwerken**:

    ```azurecli
    az dls fs access set-owner --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --group 80a3ed5f-959e-4696-ba3c-d3c8b2db6766 --owner 6361e05d-c381-4275-a932-5535806bb323
    ```

* Gebruik de volgende opdracht als u **de machtigingen voor een bestand/map wilt bijwerken**:

    ```azurecli
    az dls fs access set-permission --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv --permission 777
    ```
    
* Gebruik de volgende opdracht als u **de ACL's voor een gegeven pad wilt ophalen**:

    ```azurecli
    az dls fs access show --account mydatalakestore --path /mynewfolder/vehicle1_09142014.csv
    ```

    De uitvoer moet er ongeveer als volgt uitzien:

        {
            "entries": [
            "user::rwx",
            "group::rwx",
            "other::---"
          ],
          "group": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "owner": "1808bd5f-62af-45f4-89d8-03c5e81bac20",
          "permission": "770",
          "stickyBit": false
        }

* Gebruik de volgende opdracht als u **een item voor een ACL wilt instellen**:

    ```azurecli
    az dls fs access set-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323:-w-
    ```

* Gebruik de volgende opdracht als u **een item voor een ACL wilt verwijderen**:

    ```azurecli
    az dls fs access remove-entry --account mydatalakestore --path /mynewfolder --acl-spec user:6360e05d-c381-4275-a932-5535806bb323
    ```

* Gebruik de volgende opdracht als u **een complete standaard-ACL wilt verwijderen**:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder --default-acl
    ```

* Gebruik de volgende opdracht als u **een complete niet-standaard-ACL wilt verwijderen**:

    ```azurecli
    az dls fs access remove-all --account mydatalakestore --path /mynewfolder
    ```
    
## <a name="delete-a-data-lake-store-account"></a>Een Data Lake Store-account verwijderen
Gebruik de volgende opdracht om een Data Lake Store-account te verwijderen.

```azurecli
az dls account delete --account mydatalakestore
```

Wanneer dit wordt gevraagd, typt u **Y** om het account te verwijderen.

## <a name="next-steps"></a>Volgende stappen
* [Azure Data Lake Store gebruiken voor big data-vereisten](data-lake-store-data-scenarios.md) 
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
