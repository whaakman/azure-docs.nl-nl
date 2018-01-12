---
title: "Gegevens kopiëren van Azure Storage-Blobs in Data Lake Store | Microsoft Docs"
description: "AdlCopy hulpprogramma gebruiken om gegevens te kopiëren van Azure Storage-Blobs naar Data Lake Store"
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/09/2018
ms.author: nitinme
ms.openlocfilehash: 33bfd200f72cb56b391cab52bf90f19b568893a9
ms.sourcegitcommit: 9292e15fc80cc9df3e62731bafdcb0bb98c256e1
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/10/2018
---
# <a name="copy-data-from-azure-storage-blobs-to-data-lake-store"></a>Gegevens kopiëren van Azure Storage Blob naar Data Lake Store
> [!div class="op_single_selector"]
> * [DistCp gebruiken](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy gebruiken](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Store biedt een opdrachtregelprogramma [AdlCopy](http://aka.ms/downloadadlcopy), om gegevens te kopiëren van de volgende bronnen:

* Uit Azure Storage-Blobs in Data Lake Store. U kunt AdlCopy niet gebruiken om gegevens van Data Lake Store te kopiëren naar Azure Storage-blobs.
* Tussen twee Azure Data Lake Store-accounts.

Bovendien kunt u het hulpprogramma AdlCopy in twee verschillende modi:

* **Zelfstandige**, waarbij het hulpprogramma Data Lake Store-bronnen gebruikt voor het uitvoeren van de taak.
* **Gebruik van een Data Lake Analytics-account**, waarbij de eenheden die zijn toegewezen aan uw Data Lake Analytics-account worden gebruikt om de kopieerbewerking. Mogelijk wilt deze optie gebruiken als u op zoek bent naar de kopie-taken uitvoeren op een voorspelbare wijze.

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Storage-Blobs** container met enkele gegevens.
* **Een Azure Data Lake Store-account**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Store](data-lake-store-get-started-portal.md)
* **Azure Data Lake Analytics-account (optioneel)** -Zie [aan de slag met Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) voor instructies over het maken van een Data Lake Store-account.
* **AdlCopy hulpprogramma**. Installeer het hulpprogramma AdlCopy van [http://aka.ms/downloadadlcopy](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Syntaxis van het hulpprogramma AdlCopy
Gebruik de volgende syntaxis om te werken met het hulpprogramma AdlCopy

    AdlCopy /Source <Blob or Data Lake Store source> /Dest <Data Lake Store destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Unit <Number of Analytics units> /Pattern

De parameters in de syntaxis worden hieronder beschreven:

| Optie | Beschrijving |
| --- | --- |
| Bron |Hiermee geeft u de locatie van de brongegevens in de Azure-opslag-blob. De bron kan een blobcontainer een blob of een andere Data Lake Store-account zijn. |
| Doel |Hiermee geeft u de bestemming Data Lake Store om naar te kopiëren. |
| SourceKey |Hiermee geeft u de toegangssleutel voor opslag voor de bron van Azure storage-blob. Dit is alleen vereist als de bron een blob-container of een blob is. |
| Account |**Optioneel**. Gebruik deze optie als u wilt gebruiken van Azure Data Lake Analytics-account om uit te voeren van de kopieertaak. Als u de optie /Account in de syntaxis gebruiken, maar geen een Data Lake Analytics-account opgeeft, wordt in AdlCopy standaardaccount gebruikt voor het uitvoeren van de taak. Ook als u deze optie gebruikt, moet u toevoegen (Azure Storage-Blob) bron en bestemming (Azure Data Lake Store) als gegevensbronnen voor uw Data Lake Analytics-account. |
| Eenheden |Hiermee geeft u het nummer van Data Lake Analytics-eenheden die wordt gebruikt voor de kopieertaak. Deze optie is verplicht als u de **/Account** optie voor het opgeven van het Data Lake Analytics-account. |
| Patroon |Hiermee geeft u een regex-patroon waarmee wordt aangegeven welke blobs of de bestanden te kopiëren. AdlCopy maakt gebruik van hoofdlettergevoelige overeenkomen. Het volgende standaardpatroon gebruikt wanneer er geen patroon is opgegeven is om alle items te kopiëren. Meerdere bestand patronen opgeven wordt niet ondersteund. |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>AdlCopy (als zelfstandig) gebruiken om gegevens van een Azure Storage-blob kopiëren
1. Open een opdrachtprompt en navigeer naar de map waar AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.
2. Voer de volgende opdracht om een specifieke blob kopiëren van de Broncontainer naar een Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[AZURE.NOTE]De bovenstaande syntaxis Hiermee geeft u het bestand moet worden gekopieerd naar een map in het Data Lake Store-account. AdlCopy hulpprogramma maakt een map als de naam van de opgegeven map niet bestaat.

    U wordt gevraagd de referenties voor de Azure-abonnement waaronder die u een Data Lake Store-account hebt invoeren. Hier ziet u uitvoer ziet er als volgt:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. U kunt ook alle blobs uit een container kopiëren naar het Data Lake Store-account met de volgende opdracht:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Bijvoorbeeld:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Prestatieoverwegingen

Als u uit een Azure Blob Storage-account kopiëren wilt, wordt u mogelijk beperkt tijdens het kopiëren van de blob-opslag-zijde. De prestaties van uw project kopiëren afnemen. Zie voor meer informatie over de beperkingen van Azure Blob Storage, Azure Storage-limieten op [Azure-abonnement en Servicelimieten](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-store-account"></a>AdlCopy (als zelfstandig) gebruiken om gegevens te kopiëren uit een andere Data Lake Store-account
U kunt ook AdlCopy gebruiken om gegevens tussen twee Data Lake Store-accounts te kopiëren.

1. Open een opdrachtprompt en navigeer naar de map waar AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.
2. Voer de volgende opdracht om een specifiek bestand van de ene Data Lake Store-account naar de andere kopiëren.

        AdlCopy /Source adl://<source_adls_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adls_account>.azuredatalakestore.net/<path>/

    Bijvoorbeeld:

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > De bovenstaande syntaxis Hiermee geeft u het bestand moet worden gekopieerd naar een map in de doel-Data Lake Store-account. AdlCopy hulpprogramma maakt een map als de naam van de opgegeven map niet bestaat.
   >
   >

    U wordt gevraagd de referenties voor de Azure-abonnement waaronder die u een Data Lake Store-account hebt invoeren. Hier ziet u uitvoer ziet er als volgt:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. De volgende opdracht worden alle bestanden gekopieerd vanuit een specifieke map in Data Lake Store-account van de bron naar een map in de doel-Data Lake Store-account.

        AdlCopy /Source adl://mydatastore.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestore.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Prestatieoverwegingen

Wanneer u AdlCopy als een zelfstandig hulpprogramma, het exemplaar wordt uitgevoerd op gedeelde, Azure beheerde bronnen. De prestaties mogelijk dat u in deze omgeving, is afhankelijk van systeembelasting en beschikbare resources. Deze modus wordt beste voor kleinere overdrachten op basis van ad-hoc gebruikt. Er zijn geen parameters moeten worden afgestemd bij gebruik van AdlCopy als een zelfstandige tool.

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>AdlCopy (met Data Lake Analytics-account) gebruiken om gegevens te kopiëren
U kunt ook uw Data Lake Analytics-account gebruiken om uit te voeren van de taak AdlCopy om gegevens te kopiëren van Azure storage-blobs naar Data Lake Store. U zou deze optie doorgaans gebruikt wanneer de gegevens worden verplaatst in het bereik van gigabytes en terabyte en gewenste betere en voorspelbare prestaties doorvoer.

Voor het gebruik van uw Data Lake Analytics-account met AdlCopy om van een Azure Storage-Blob te kopiëren, moet de bron (Azure Storage-Blob) worden toegevoegd als een gegevensbron voor uw Data Lake Analytics-account. Zie voor instructies over het toevoegen van extra gegevensbronnen aan uw Data Lake Analytics-account [beheren Data Lake Analytics-account gegevensbronnen](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> Als u uit een Azure Data Lake Store-account als de bron met behulp van een Data Lake Analytics-account kopiëren wilt, hoeft u niet het Data Lake Store-account koppelen aan de Data Lake Analytics-account. De vereiste voor de bron-store koppelen aan het Data Lake Analytics-account is alleen wanneer de bron een Azure Storage-account is.
>
>

Voer de volgende opdracht uit een Azure Storage-blob kopiëren naar een Data Lake Store-account met behulp van Data Lake Analytics-account:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Unit <number_of_data_lake_analytics_units_to_be_used>

Bijvoorbeeld:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Voer op dezelfde manier uit de volgende opdracht uit om alle bestanden kopiëren vanuit een specifieke map in de bron van Data Lake Store-account in een map op de bestemming Data Lake Store-account met behulp van Data Lake Analytics-account:

    AdlCopy /Source adl://mysourcedatalakestore.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastore.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Prestatieoverwegingen

Bij het kopiëren van gegevens in het bereik van terabytes biedt AdlCopy gebruik met uw eigen Azure Data Lake Analytics-account betere en meer voorspelbare prestaties. De parameter die moet worden afgestemd is het aantal Azure Data Lake Analytics Units moet worden gebruikt voor de kopieertaak. Het aantal eenheden vergroot, worden de prestaties van uw project kopiëren. Elk bestand moet worden gekopieerd kan maximaal één eenheid kunt gebruiken. Het opgeven van meer eenheden dan het aantal bestanden worden gekopieerd verhoogd prestaties niet.

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>AdlCopy gebruiken om te kopiëren van gegevens met jokertekens
In dit gedeelte vindt u informatie over het gebruik van AdlCopy om gegevens te kopiëren van een bron (in het onderstaande voorbeeld gebruiken we Azure Storage-Blob) een bestemming Data Lake Store-account met jokertekens. Gebruik de volgende stappen kunt u bijvoorbeeld alle bestanden met de extensie van de bron-blob kopiëren naar de bestemming.

1. Open een opdrachtprompt en navigeer naar de map waar AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.
2. Voer de volgende opdracht uit om alle bestanden met de extensie *.csv kopiëren van een specifieke blob uit de Broncontainer voor een Data Lake Store:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adls_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestore.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Facturering
* Als u het hulpprogramma AdlCopy als zelfstandige wordt u gefactureerd voor uitgaande kosten voor het verplaatsen van gegevens, als de bron van Azure Storage-account zich niet in dezelfde regio bevinden als de Data Lake Store.
* Als u het hulpprogramma AdlCopy met uw Data Lake Analytics gebruiken-account standaard [Data Lake Analytics facturering tarieven](https://azure.microsoft.com/pricing/details/data-lake-analytics/) wordt toegepast.

## <a name="considerations-for-using-adlcopy"></a>Overwegingen voor het gebruik van AdlCopy
* AdlCopy (voor versie 1.0.5) ondersteunt kopiëren van gegevens uit bronnen die gezamenlijk meer dan duizenden bestanden en mappen hebben. Echter als u een grote gegevensset kopiëren problemen ondervindt, kunt u de bestanden/mappen verdelen over verschillende submappen en het pad naar de submappen als de bron in plaats daarvan.

## <a name="performance-considerations-for-using-adlcopy"></a>Prestatie-overwegingen voor het gebruik van AdlCopy

AdlCopy ondersteunt kopiëren van gegevens met duizenden bestanden en mappen. Als u een grote gegevensset kopiëren problemen ondervindt, kunt u de bestanden/mappen verdelen naar kleinere submappen. AdlCopy is gebouwd voor ad-hoc exemplaren. Als u probeert om gegevens op periodieke basis te kopiëren, moet u overwegen [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) die zorgt voor volledig beheer om de kopieerbewerkingen.

## <a name="release-notes"></a>Releaseopmerkingen
* 1.0.13 - als u gegevens naar hetzelfde Azure Data Lake Store-account via meerdere adlcopy opdrachten kopieert niet hoeft u uw referenties voor elke run opnieuw meer. Adlcopy nu opgeslagen in de cache die informatie over meerdere wordt uitgevoerd.

## <a name="next-steps"></a>Volgende stappen
* [Gegevens in Data Lake Store beveiligen](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Store](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md)
