---
title: Verwerken van Azure blob-gegevens met geavanceerde analyses | Microsoft Docs
description: Procesgegevens in Azure Blob-opslag.
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: d8a59078-91d3-4440-b85c-430363c3f4d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: a31b77ca2d315b506c925a83a5050c0e70c54842
ms.sourcegitcommit: 732e5df390dea94c363fc99b9d781e64cb75e220
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2017
---
# <a name="heading"></a>Azure blob-gegevens met geavanceerde analyses verwerken
Dit document bevat informatie over gegevens verkennen en genereren van de functies van de gegevens die zijn opgeslagen in Azure Blob-opslag. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>De gegevens in een frame Pandas-gegevens laden
Om te verkennen en het bewerken van een gegevensset, moet deze worden gedownload van de bron van de blob naar een lokaal bestand die vervolgens kan worden geladen in een Pandas gegevensframe. Hier volgen de stappen voor deze procedure:

1. Download de gegevens van Azure-blob met de volgende voorbeeldcode Python met behulp van blob-service. De variabele in de volgende code vervangen door uw eigen specifieke waarden: 
   
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

## <a name="blob-dataexploration"></a>Gegevensverkenning
Hier volgen enkele voorbeelden van methoden voor het verkennen van gegevens met behulp van Pandas:

1. Het aantal rijen en kolommen controleren 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Inspecteer de eerste of laatste paar rijen in de gegevensset zoals hieronder:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Controleer het gegevenstype dat elke kolom is geïmporteerd als het gebruik van de volgende voorbeeldcode
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. De algemene statistieken voor de kolommen in de gegevensset als volgt controleren
   
        dataframe_blobdata.describe()
5. Het aantal items voor elke waarde in de kolom als volgt bekijken
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Ontbrekende waarden ten opzichte van het werkelijke aantal vermeldingen in elke kolom met de volgende voorbeeldcode tellen
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Als u ontbrekende waarden voor een specifieke kolom in de gegevens hebt, kunt u ze als volgt verwijderen:
   
     dataframe_blobdata_noNA dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape =
   
   Ontbrekende waarden te vervangen op een andere manier is met de modusfunctie:
   
     dataframe_blobdata_mode dataframe_blobdata.fillna = ({< column_name >: .mode()[0]}) dataframe_blobdata ['< column_name >"]        
8. Histogram tekent variabele aantal opslaglocaties gebruiken om te tekenen van de distributie van een variabele maken    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bekijk correlatie tussen variabelen met behulp van een scatterplot of via de ingebouwde correlatiefunctie
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Functie generatie
Functies met behulp van Python als volgt kan worden gegenereerd:

### <a name="blob-countfeature"></a>Indicatorwaarde op basis van functie generatie
Categorische functies kunnen als volgt worden gemaakt:

1. Inspecteer de distributie van de kolom categorische:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Indicatorwaarden voor elk van de kolomwaarden genereren
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Lid worden van de indicatorkolom met de oorspronkelijke gegevensframe 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. De oorspronkelijke variabele zelf verwijderen:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Met binning functie generatie
Voor het genereren van binned functies gaan we als volgt:

1. Een reeks van kolommen die u wilt een numerieke kolom bin toevoegen
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Converteren naar een reeks Boole-variabelen met binning
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Ten slotte de dummy variabelen Join terug naar de oorspronkelijke gegevensframe
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="sql-featuregen"></a>Schrijven van gegevens terug naar Azure blob en gebruiken in Azure Machine Learning
Nadat u de gegevens hebt verkend en de benodigde onderdelen hebt gemaakt, kunt u de gegevens uploaden (actieve of featurized) naar een Azure blob- en deze wordt gebruikt in Azure Machine Learning met behulp van de volgende stappen uit: Houd er rekening mee dat extra functies kunnen worden gemaakt in de Azure-Machine Ook Learning Studio. 

1. Het gegevensframe schrijven naar een lokaal bestand
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Uploaden van de gegevens naar Azure blob als volgt:
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. Nu de gegevens kunnen worden gelezen vanaf de blob met de Azure Machine Learning [importgegevens] [ import-data] module, zoals wordt weergegeven in het scherm hieronder:

![lezer blob][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

