---
title: Gegevens in Azure blob storage met Pandas | Microsoft Docs
description: Hoe gegevens die zijn opgeslagen in Azure blob-container met Pandas verkennen.
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: bradsev
ms.openlocfilehash: a46735dde28740087d201d7490f135349aad76f6
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Met Pandas gegevens verkennen in Azure Blok-opslag
Dit document wordt beschreven hoe gegevens die zijn opgeslagen in het gebruik van Azure blob-container verkennen [Pandas](http://pandas.pydata.org/) Python-pakket.

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u gegevens uit verschillende omgevingen met opslag verkennen met behulp van hulpprogramma's. Deze taak is een stap in de [gegevens wetenschap proces]().

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [een Azure Storage-account maken](../../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Uw gegevens opgeslagen in een Azure blob storage-account. Als u instructies nodig hebt, raadpleegt u [verplaatsen van gegevens naar en van Azure Storage](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>De gegevens in een DataFrame Pandas laden
Om te verkennen en het bewerken van een gegevensset, moet deze eerst worden gedownload van de bron van de blob naar een lokaal bestand, die vervolgens kan worden geladen in een DataFrame Pandas. Hier volgen de stappen voor deze procedure:

1. Download de gegevens van Azure-blob met de volgende Python-codevoorbeeld met behulp van blob-service. De variabele in de volgende code vervangen door uw eigen specifieke waarden: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. De gegevens in een Pandas gegevens tijdskader uit het gedownloade bestand gelezen.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

U bent nu klaar voor de gegevens verkennen en het genereren van functies op deze dataset.

## <a name="blob-dataexploration"></a>Voorbeelden van gegevensverkenning met Pandas
Hier volgen enkele voorbeelden van methoden voor het verkennen van gegevens met behulp van Pandas:

1. Inspecteer de **aantal rijen en kolommen** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Inspecteer** enkele van de eerste of laatste **rijen** in de volgende gegevensset:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Controleer de **gegevenstype** elke kolom is geïmporteerd als het gebruik van de volgende voorbeeldcode
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Controleer de **elementaire statistieken** voor de kolommen in de gegevens als volgt instellen
   
        dataframe_blobdata.describe()
5. Het aantal items voor elke waarde in de kolom als volgt bekijken
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Ontbrekende waarden tellen** ten opzichte van het werkelijke aantal vermeldingen in elke kolom met de volgende voorbeeldcode
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Als u hebt **ontbrekende waarden** voor een specifieke kolom in de gegevens verwijderen ze als volgt:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Ontbrekende waarden te vervangen op een andere manier is met de modusfunctie:
   
     dataframe_blobdata_mode dataframe_blobdata.fillna = ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]        
8. Maak een **histogram** getekend met een variabele aantal opslaglocaties uitzetten van de distributie van een variabele    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bekijk **correlaties** tussen variabelen met behulp van een scatterplot of via de ingebouwde correlatiefunctie
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

