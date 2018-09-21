---
title: Gegevens verkennen in Azure blob-opslag met pandas | Microsoft Docs
description: Klik hier voor meer informatie over het verkennen van gegevens die zijn opgeslagen in Azure blob-container met behulp van pandas.
services: machine-learning,storage
documentationcenter: ''
author: deguhath
manager: cgronlun
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2017
ms.author: deguhath
ms.openlocfilehash: b80fcecf28eaaf05e7fc199a9c318fd4148b9212
ms.sourcegitcommit: 8b694bf803806b2f237494cd3b69f13751de9926
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/20/2018
ms.locfileid: "46497855"
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Gegevens verkennen in Azure blob-opslag met pandas
Dit document wordt uitgelegd hoe u gegevens die zijn opgeslagen in Azure blob-container met verkennen [pandas](http://pandas.pydata.org/) Python-pakket.

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u hulpprogramma's gebruiken om gegevens te verkennen vanuit verschillende omgevingen voor opslag. Deze taak is een stap in de [Data Science Process](overview.md).

[!INCLUDE [cap-explore-data-selector](../../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Vereisten
In dit artikel wordt ervan uitgegaan dat u hebt:

* Een Azure storage-account gemaakt. Als u instructies nodig hebt, raadpleegt u [maken van een Azure Storage-account](../../storage/common/storage-quickstart-create-account.md)
* Uw gegevens opgeslagen in een Azure blob storage-account. Als u instructies nodig hebt, raadpleegt u [om gegevens te verplaatsen naar en van Azure Storage](../../storage/common/storage-moving-data.md)

## <a name="load-the-data-into-a-pandas-dataframe"></a>De gegevens in een pandas DataFrame laden
Om te verkennen en bewerken van een gegevensset, moet deze eerst worden gedownload van de bron-blob naar een lokaal bestand, die vervolgens kan worden geladen in een pandas DataFrame. Hier volgen de stappen voor deze procedure:

1. Download de gegevens van Azure-blob met de volgende Python-codevoorbeeld dat met behulp van blob-service. De variabele in de volgende code vervangen door uw eigen specifieke waarden: 
   
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
2. De gegevens in een pandas DataFrame uit het gedownloade bestand gelezen.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

U bent nu klaar voor de gegevens verkennen en het genereren van functies voor deze gegevensset.

## <a name="blob-dataexploration"></a>Voorbeelden van gegevens verkennen met behulp van pandas
Hier volgen enkele voorbeelden van manieren om met pandas gegevens te verkennen:

1. Inspecteer de **aantal rijen en kolommen** 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Inspecteer** de eerste of laatste paar **rijen** in de volgende gegevensset:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Controleer de **gegevenstype** elke kolom is geïmporteerd als het gebruik van de volgende voorbeeldcode
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Controleer de **elementaire statistieken** voor de kolommen in de gegevens als volgt instellen
   
        dataframe_blobdata.describe()
5. Het aantal vermeldingen voor elke waarde in de kolom als volgt bekijken
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Ontbrekende waarden tellen** versus het werkelijke aantal vermeldingen in elke kolom met behulp van de volgende voorbeeldcode
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Als u hebt **ontbrekende waarden** voor een bepaalde kolom in de gegevens, u kunt neerzetten ze als volgt:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Een andere manier om de ontbrekende waarden vervangen is met de modusfunctie:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Maak een **histogram** tekenen met behulp van het nummer van de variabele van de opslaglocaties voor het tekenen van de distributie van een variabele    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bekijk **correlaties** tussen variabelen met behulp van een teststappen of de ingebouwde correlatiefunctie
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

