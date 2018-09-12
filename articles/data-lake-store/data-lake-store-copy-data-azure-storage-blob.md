---
title: Gegevens kopiëren van Azure Storage-Blobs in Azure Data Lake Storage Gen1 | Microsoft Docs
description: AdlCopy gebruiken om gegevens te kopiëren van Azure Storage BLOB naar Azure Data Lake Storage Gen1
services: data-lake-store
documentationcenter: ''
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: dc273ef8-96ef-47a6-b831-98e8a777a5c1
ms.service: data-lake-store
ms.devlang: na
ms.topic: conceptual
ms.date: 05/29/2018
ms.author: nitinme
ms.openlocfilehash: 0cf5ace29dabd3a55524fe38403a07e3916ea7d6
ms.sourcegitcommit: 794bfae2ae34263772d1f214a5a62ac29dcec3d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/11/2018
ms.locfileid: "44390889"
---
# <a name="copy-data-from-azure-storage-blobs-to-azure-data-lake-storage-gen1"></a>Gegevens kopiëren van Azure Storage-Blobs naar Azure Data Lake Storage Gen1
> [!div class="op_single_selector"]
> * [DistCp gebruiken](data-lake-store-copy-data-wasb-distcp.md)
> * [AdlCopy gebruiken](data-lake-store-copy-data-azure-storage-blob.md)
>
>

Azure Data Lake Storage Gen1 voorziet in een opdrachtregel-hulpprogramma, [AdlCopy](http://aka.ms/downloadadlcopy), het kopiëren van gegevens uit de volgende bronnen:

* Uit Azure Storage-Blobs in Data Lake Storage Gen1. U kunt geen AdlCopy gebruiken om te gegevens kopiëren van Data Lake Storage Gen1 naar Azure Storage-blobs.
* Tussen twee Gen1 van Azure Data Lake Storage-accounts.

U kunt ook het hulpprogramma AdlCopy gebruiken in twee verschillende modi:

* **Zelfstandige**, waar het hulpprogramma Data Lake Storage Gen1 resources gebruikt voor het uitvoeren van de taak.
* **Met behulp van een Data Lake Analytics-account**, waar de eenheden toegewezen aan uw Data Lake Analytics-account worden gebruikt om de kopieerbewerking. Mogelijk wilt deze optie gebruiken wanneer u op zoek bent naar de kopie-taken uitvoeren op een voorspelbare wijze.

## <a name="prerequisites"></a>Vereisten
Voordat u dit artikel gaat lezen, moet u beschikken over het volgende:

* **Een Azure-abonnement**. Zie [Gratis proefversie van Azure ophalen](https://azure.microsoft.com/pricing/free-trial/).
* **Azure Storage-Blobs** container met gegevens.
* **Een account met Azure Data Lake Storage Gen1**. Zie voor instructies over het maken van een [aan de slag met Azure Data Lake Storage Gen1](data-lake-store-get-started-portal.md)
* **Azure Data Lake Analytics-account (optioneel)** -Zie [aan de slag met Azure Data Lake Analytics](../data-lake-analytics/data-lake-analytics-get-started-portal.md) voor instructies over het maken van een Data Lake Analytics-account.
* **AdlCopy hulpprogramma**. Installeer het hulpprogramma AdlCopy van [ http://aka.ms/downloadadlcopy ](http://aka.ms/downloadadlcopy).

## <a name="syntax-of-the-adlcopy-tool"></a>Syntaxis van het hulpprogramma AdlCopy
Gebruik de volgende syntaxis om te werken met het hulpprogramma AdlCopy

    AdlCopy /Source <Blob or Data Lake Storage Gen1 source> /Dest <Data Lake Storage Gen1 destination> /SourceKey <Key for Blob account> /Account <Data Lake Analytics account> /Units <Number of Analytics units> /Pattern

De parameters in de syntaxis worden hieronder beschreven:

| Optie | Beschrijving |
| --- | --- |
| Bron |Hiermee geeft u de locatie van de brongegevens in de Azure storage-blob. De bron mag een blob-container, een blob of een ander Gen1 van Data Lake Storage-account. |
| Doel |Hiermee geeft u de Data Lake Storage Gen1 bestemming om naar te kopiëren. |
| SourceKey |Hiermee geeft u de toegangssleutel voor opslag voor de Azure storage blob-bron. Dit is alleen vereist als de bron een blob-container of een blob is. |
| Account |**Optioneel**. Gebruik deze optie als u wilt gebruiken van Azure Data Lake Analytics-account om uit te voeren van de taak voor het kopiëren. Als u de optie /Account in de syntaxis van de gebruiken maar een Data Lake Analytics-account niet opgeeft, AdlCopy maakt gebruik van een standaardaccount voor de taak uit te voeren. Ook als u deze optie gebruikt, moet u toevoegen de bron (Azure Storage-Blob)- en doelserver (Azure Data Lake Storage Gen1) als gegevensbronnen voor uw Data Lake Analytics-account. |
| Eenheden |Hiermee geeft u het aantal Data Lake Analytics-eenheden die worden gebruikt voor de taak voor het kopiëren. Deze optie is verplicht als u de **/Account** optie om op te geven van de Data Lake Analytics-account. |
| Patroon |Hiermee geeft u een regex-patroon waarmee wordt aangegeven welke blobs of de bestanden te kopiëren. AdlCopy maakt gebruik van hoofdlettergevoelige overeenkomende. Het standaardpatroon gebruikt als er geen patroon is opgegeven, is om alle items te kopiëren. Opgeven van meerdere-bestandpatronen wordt niet ondersteund. |

## <a name="use-adlcopy-as-standalone-to-copy-data-from-an-azure-storage-blob"></a>AdlCopy (als standalone) gebruiken om gegevens te kopiëren van een Azure Storage-blob
1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.
2. Voer de volgende opdracht om te kopiëren van een specifieke blob uit de Broncontainer naar een Data Lake Storage Gen1-map:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/WebsiteLogSampleData/SampleLog/909f2b.log /dest swebhdfs://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

    >[!NOTE] 
    >De bovenstaande syntaxis geeft het bestand moeten worden gekopieerd naar een map in de Gen1 van Data Lake Storage-account. AdlCopy hulpprogramma maakt een map als de naam van de opgegeven map bestaat niet.

    U wordt gevraagd de referenties invoert voor de Azure-abonnement waarmee u uw Data Lake Storage Gen1-account hebt. Hier ziet u uitvoer die vergelijkbaar is met het volgende:

        Initializing Copy.
        Copy Started.
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.

1. U kunt ook alle blobs van één container kopiëren naar de Data Lake Storage Gen1 rekening met de volgende opdracht:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/ /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container>        

    Bijvoorbeeld:

        AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest adl://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ==

### <a name="performance-considerations"></a>Prestatieoverwegingen

Als u uit een Azure Blob Storage-account kopiëren wilt, kunt u tijdens het kopiëren van aan de kant van blob storage worden beperkt. De prestaties van uw taak voor het kopiëren afnemen. Zie voor meer informatie over de beperkingen van Azure Blob Storage, Azure Storage-beperkingen aan [Azure-abonnement en Servicelimieten](../azure-subscription-service-limits.md).

## <a name="use-adlcopy-as-standalone-to-copy-data-from-another-data-lake-storage-gen1-account"></a>AdlCopy (als standalone) gebruiken om gegevens te kopiëren uit een andere Gen1 van Data Lake Storage-account
U kunt ook AdlCopy gebruiken om gegevens tussen twee Gen1 van Data Lake Storage-accounts te kopiëren.

1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.
2. Voer de volgende opdracht om te kopiëren van een specifiek bestand van de ene Gen1 van Data Lake Storage-account naar een andere.

        AdlCopy /Source adl://<source_adlsg1_account>.azuredatalakestore.net/<path_to_file> /dest adl://<dest_adlsg1_account>.azuredatalakestore.net/<path>/

    Bijvoorbeeld:

        AdlCopy /Source adl://mydatastorage.azuredatalakestore.net/mynewfolder/909f2b.log /dest adl://mynewdatalakestorage.azuredatalakestore.net/mynewfolder/

   > [!NOTE]
   > De bovenstaande syntaxis geeft het bestand moeten worden gekopieerd naar een map in de doel-Gen1 van Data Lake Storage-account. AdlCopy hulpprogramma maakt een map als de naam van de opgegeven map bestaat niet.
   >
   >

    U wordt gevraagd de referenties invoert voor de Azure-abonnement waarmee u uw Data Lake Storage Gen1-account hebt. Hier ziet u uitvoer die vergelijkbaar is met het volgende:

        Initializing Copy.
        Copy Started.|
        100% data copied.
        Finishing Copy.
        Copy Completed. 1 file copied.
3. De volgende opdracht worden alle bestanden uit een specifieke map in de bron Gen1 van Data Lake Storage-account gekopieerd naar een map in de doel-Gen1 van Data Lake Storage-account.

        AdlCopy /Source adl://mydatastorage.azuredatalakestore.net/mynewfolder/ /dest adl://mynewdatalakestorage.azuredatalakestore.net/mynewfolder/

### <a name="performance-considerations"></a>Prestatieoverwegingen

Wanneer u AdlCopy als een zelfstandig hulpprogramma, het exemplaar wordt uitgevoerd op gedeelde, Azure beheerde resources. De prestaties u in deze omgeving krijgt mogelijk, is afhankelijk van laden van het systeem en beschikbare resources. In deze modus wordt beste voor kleinere overdrachten op basis van de ad-hoc gebruikt. Er zijn geen parameters moeten worden afgestemd op wanneer adlcopy gebruiken als een zelfstandig hulpprogramma.

## <a name="use-adlcopy-with-data-lake-analytics-account-to-copy-data"></a>AdlCopy gebruiken (met Data Lake Analytics-account) om gegevens te kopiëren
U kunt ook uw Data Lake Analytics-account gebruiken om uit te voeren van de taak AdlCopy om gegevens te kopiëren van Azure storage-blobs naar Data Lake Storage Gen1. Wordt doorgaans gebruikt u deze optie wanneer de gegevens worden verplaatst zich binnen het bereik van gigabytes en terabytes en u wilt dat de doorvoer van betere en voorspelbare prestaties.

Als u uw Data Lake Analytics-account met AdlCopy wilt kopiëren uit een Azure Storage-Blob, moet de bron (Azure Storage-Blob) worden toegevoegd als een gegevensbron voor uw Data Lake Analytics-account. Zie voor instructies over het toevoegen van extra gegevensbronnen aan uw Data Lake Analytics-account [Data Lake Analytics beheren account gegevensbronnen](../data-lake-analytics/data-lake-analytics-manage-use-portal.md#manage-data-sources).

> [!NOTE]
> Als u uit een Azure Data Lake Storage Gen1-account als de gegevensbron met behulp van een Data Lake Analytics-account kopiëren wilt, hoeft u niet de Gen1 van Data Lake Storage-account koppelen aan de Data Lake Analytics-account. De vereiste voor de bron-store koppelen aan de Data Lake Analytics-account is alleen wanneer de bron een Azure Storage-account is.
>
>

Voer de volgende opdracht om te kopiëren van een Azure Storage-blob naar een Gen1 van Data Lake Storage-account met behulp van Data Lake Analytics-account:

    AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Account <data_lake_analytics_account> /Units <number_of_data_lake_analytics_units_to_be_used>

Bijvoorbeeld:

    AdlCopy /Source https://mystorage.blob.core.windows.net/mycluster/example/data/gutenberg/ /dest swebhdfs://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Account mydatalakeanalyticaccount /Units 2

Op dezelfde manier, voer de volgende opdracht uit om alle bestanden uit een specifieke map in de bron Gen1 van Data Lake Storage-account kopiëren naar een map in de doel-Gen1 van Data Lake Storage-account met behulp van Data Lake Analytics-account:

    AdlCopy /Source adl://mysourcedatalakestorage.azuredatalakestore.net/mynewfolder/ /dest adl://mydestdatastorage.azuredatalakestore.net/mynewfolder/ /Account mydatalakeanalyticaccount /Units 2

### <a name="performance-considerations"></a>Prestatieoverwegingen

Het kopiëren van gegevens in het bereik van terabytes, biedt adlcopy gebruiken met uw eigen Azure Data Lake Analytics-account betere en meer voorspelbare prestaties. De parameter die moet zijn afgestemd op is het aantal Azure Data Lake Analytics Units moet worden gebruikt voor de taak voor het kopiëren. Het aantal eenheden vergroot, zullen de prestaties van uw taak voor het kopiëren. Elk bestand moet worden gekopieerd, kan maximaal één eenheid gebruiken. Meer eenheden dan het aantal bestanden worden gekopieerd op te geven, nemen de prestaties niet toe.

## <a name="use-adlcopy-to-copy-data-using-pattern-matching"></a>AdlCopy gebruiken om te kopiëren van gegevens met behulp van jokertekens
In deze sectie leert u hoe u AdlCopy gebruiken om gegevens te kopiëren van een bron (in het onderstaande voorbeeld gebruiken we Azure Storage-Blob) naar een Gen1 van Data Lake Storage-bestemmingsaccount via kenmerkvelden. Bijvoorbeeld, kunt u de volgende stappen uit op alle bestanden met extensie van de bron-blob kopiëren naar de bestemming.

1. Open een opdrachtprompt en navigeer naar de map waarin AdlCopy is geïnstalleerd, doorgaans `%HOMEPATH%\Documents\adlcopy`.
2. Voer de volgende opdracht om alle bestanden met extensie *.csv van een specifieke blob uit de Broncontainer in een Data Lake Storage Gen1-map kopiëren:

        AdlCopy /source https://<source_account>.blob.core.windows.net/<source_container>/<blob name> /dest swebhdfs://<dest_adlsg1_account>.azuredatalakestore.net/<dest_folder>/ /sourcekey <storage_account_key_for_storage_container> /Pattern *.csv

    Bijvoorbeeld:

        AdlCopy /source https://mystorage.blob.core.windows.net/mycluster/HdiSamples/HdiSamples/FoodInspectionData/ /dest adl://mydatalakestorage.azuredatalakestore.net/mynewfolder/ /sourcekey uJUfvD6cEvhfLoBae2yyQf8t9/BpbWZ4XoYj4kAS5Jf40pZaMNf0q6a8yqTxktwVgRED4vPHeh/50iS9atS5LQ== /Pattern *.csv

## <a name="billing"></a>Billing
* Als u het hulpprogramma AdlCopy als zelfstandige gebruiken wordt u gefactureerd voor uitgaand verkeer kosten voor het verplaatsen van gegevens, als de bron van Azure Storage-account zich niet in dezelfde regio als de Gen1 van Data Lake Storage-account.
* Als u het hulpprogramma AdlCopy met uw Data Lake Analytics gebruiken-account standaard [Data Lake Analytics factureringstarieven](https://azure.microsoft.com/pricing/details/data-lake-analytics/) wordt toegepast.

## <a name="considerations-for-using-adlcopy"></a>Overwegingen voor het gebruik van AdlCopy
* AdlCopy (voor versie 1.0.5) biedt ondersteuning voor kopiëren van gegevens uit bronnen die gezamenlijk meer dan duizend van bestanden en mappen hebben. Als er problemen zijn met kopiëren van een grote gegevensset is, kunt u de bestanden/mappen verdelen over verschillende submappen en gebruik het pad naar de submappen van de map als de bron in plaats daarvan.

## <a name="performance-considerations-for-using-adlcopy"></a>Prestatie-overwegingen voor adlcopy gebruiken

AdlCopy ondersteunt het kopiëren van gegevens met duizenden bestanden en mappen. Als er problemen zijn met kopiëren van een grote gegevensset is, kunt u de bestanden/mappen verdelen in kleinere submappen. AdlCopy is gebouwd voor ad-hoc kopieën. Als u probeert om gegevens op periodieke basis te kopiëren, moet u overwegen [Azure Data Factory](../data-factory/connector-azure-data-lake-store.md) die zorgt voor volledig beheer over de kopieerbewerkingen.

## <a name="release-notes"></a>Releaseopmerkingen
* 1.0.13 - als u gegevens naar dezelfde Gen1 van Azure Data Lake Storage-account voor meerdere adlcopy opdrachten kopieert, u hoeft niet opnieuw in te voeren van uw referenties voor elke uitvoering niet meer. Adlcopy nu opgeslagen in de cache die gegevens voor verschillende runs.

## <a name="next-steps"></a>Volgende stappen
* [Gegevens beveiligen in Data Lake Storage Gen1](data-lake-store-secure-data.md)
* [Azure Data Lake Analytics gebruiken met Data Lake Storage Gen1](../data-lake-analytics/data-lake-analytics-get-started-portal.md)
* [Azure HDInsight gebruiken met Data Lake Storage Gen1](data-lake-store-hdinsight-hadoop-use-portal.md)
