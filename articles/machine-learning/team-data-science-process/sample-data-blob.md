---
title: Voorbeeldgegevens in Azure blob storage | Microsoft Docs
description: Voorbeeldgegevens in Azure Blob-opslag
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: e8d9ad2c-86c5-43d6-80b8-d355b5c0dccf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/13/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 00444853cc7b0f8d8ea4dfa1e6cddad4479daec0
ms.sourcegitcommit: 659cc0ace5d3b996e7e8608cfa4991dcac3ea129
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
# <a name="heading"></a>Voorbeeldgegevens in Azure blob-opslag
Dit document bevat informatie over steekproeven opgeslagen gegevens in Azure blob-opslag door programmatisch downloaden en vervolgens wordt met behulp van de procedures die zijn geschreven in Python.

De volgende **menu** koppelingen naar onderwerpen waarin wordt beschreven hoe u voorbeeldgegevens uit verschillende omgevingen voor opslag. 

[!INCLUDE [cap-sample-data-selector](../../../includes/cap-sample-data-selector.md)]

**Waarom een steekproef nemen uit uw gegevens?**
Als de gegevensset die u wilt analyseren groot is, is het meestal een goed idee om de gegevens om deze aan de grootte van een kleinere maar representatief en gemakkelijker down-voorbeeld. Dit vereenvoudigt het begrip van de gegevens, exploratie en functie-engineering. De rol in de Cortana-Analytics-proces is om in te schakelen snel maken van een prototype van de functies voor het verwerken van gegevens en machine learning-modellen.

Deze taak steekproeven is een stap in de [Team gegevens wetenschap proces (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="download-and-down-sample-data"></a>De download- en omlaag voorbeeldgegevens
1. De gegevens uit Azure blob storage met behulp van de blob-service van de volgende voorbeeldcode voor Python downloaden: 
   
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

2. Gegevens lezen in een Pandas gegevens tijdskader uit het bestand gedownload hierboven.
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. Omlaag-voorbeeld de gegevens met behulp van de `numpy`van `random.choice` als volgt:
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

Nu kunt u werken met de bovenstaande gegevensframe met het voorbeeld 1 procent voor verdere verkenning en functie generatie.

## <a name="heading"></a>Gegevens uploaden en lezen in Azure Machine Learning
U kunt de volgende voorbeeldcode down-voorbeeld de gegevens en deze rechtstreeks in Azure Machine Learning gebruiken:

1. Het gegevensframe schrijven naar een lokaal bestand
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. Het lokale bestand uploaden naar een Azure-blob met behulp van de volgende voorbeeldcode:
   
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
            print ("Something went wrong with uploading to the blob:"+ BLOBNAME)

3. De gegevens lezen van de blob van Azure met Azure Machine Learning [importgegevens](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) zoals weergegeven in de onderstaande afbeelding:

![lezer blob](./media/sample-data-blob/reader_blob.png)

