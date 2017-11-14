---
title: Azure Quickstart - objecten voor overdracht naar/van Azure Blob-opslag met behulp van Python | Microsoft Docs
description: Snel informatie over het overdragen van objecten uit Azure Blob storage met Python
services: storage
documentationcenter: storage
author: ruthogunnnaike
manager: cwatson
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 4a197af41f5450d84e1c18e15198d1febb02bab1
ms.sourcegitcommit: e38120a5575ed35ebe7dccd4daf8d5673534626c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2017
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Objecten voor overdracht naar/van Azure Blob storage met Python
In deze snelstartgids leert u hoe u Python gebruikt om te uploaden, downloaden en weergeven van blok-blobs in een container in Azure Blob-opslag. 

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart: 
* Installeer [Python](https://www.python.org/downloads/)
* Download en installeer [Azure-opslag-SDK voor Python](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden
De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) gebruikt in deze snelstartgids wordt een eenvoudige Python-toepassing.  

Gebruik [git](https://git-scm.com/) een kopie van de toepassing op uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Met deze opdracht kloont de opslagplaats naar uw lokale git-map. Als u wilt het Python-programma opent, zoekt u naar de storage-blobs-python-Quick Start-map en example.py-bestand.  

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren
In de toepassing, moet u opgeven naam en sleutel van uw opslagaccount maken een `BlockBlobService` object. Open de `example.py` bestand vanuit de Solution Explorer in uw IDE. Vervang de **accountname** en **accountkey** waarden door uw accountnaam en de sleutel. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>Het voorbeeld uitvoert
Dit voorbeeld maakt een testbestand in de Documentenmap. De voorbeeldcode het testbestand uploadt naar blobopslag, geeft een lijst van de blobs in de container en downloadt het bestand met een nieuwe naam. 

Voet het voorbeeld uit. De volgende uitvoer is een voorbeeld van de uitvoer die wordt geretourneerd wanneer de toepassing wordt uitgevoerd:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Wanneer u op een willekeurige toets om door te gaan, worden de storage-container en de bestanden het voorbeeldprogramma wordt verwijderd. Voordat u doorgaat, Controleer u de map 'Documenten' voor de twee bestanden. U kunt deze openen en ze identiek zijn.

U kunt ook een hulpprogramma zoals gebruiken de [Azure Storage Explorer](http://storageexplorer.com) de bestanden in Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang tot de gegevens van uw storage-account. 

Nadat u de bestanden hebt gecontroleerd, druk op een willekeurige toets om de demo voltooien en verwijderen van de testbestanden. Nu dat u wat het voorbeeld doet weet, open het bestand example.py om te kijken naar de code. 

## <a name="understand-the-sample-code"></a>Inzicht in de voorbeeldcode

Daarna bekijken we de voorbeeldcode zodat u kunt begrijpen hoe het werkt.

### <a name="get-references-to-the-storage-objects"></a>Het ophalen van verwijzingen naar de opslagobjecten
Het eerste wat te doen is de verwijzingen naar objecten die worden gebruikt voor toegang tot en beheren van Blob-opslag maken. Deze objecten bouwen op elkaar en elk wordt gebruikt door de volgende link in de lijst.

* Exemplaar maken van de **BlockBlobService** -object, dat naar de Blob-service in uw opslagaccount verwijst. 

* Exemplaar maken van de **CloudBlobContainer** -object, dat staat voor de container die u wilt openen. Containers worden gebruikt om uw blobs te organiseren dat u de mappen op uw computer gebruiken om uw bestanden te organiseren.

Zodra u de Cloud Blob-container hebt, kunt u instantiëren de **CloudBlockBlob** -object dat verwijst naar de specifieke blob waarin u geïnteresseerd bent en uitvoeren van bewerkingen zoals het uploaden, downloaden en kopiëren.

> [!IMPORTANT]
> Containernamen moeten kleine letters. Zie [Naming en verwijzen naar Containers, Blobs en metagegevens](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) voor meer informatie over de container en de blob-namen.

In deze sectie exemplaar maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn. De container wordt aangeroepen **quickstartblobs**. 

```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
### <a name="upload-blobs-to-the-container"></a>BLOB's uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Blok-blobs zijn het meest gebruikt en dat is wat wordt gebruikt in deze snelstartgids.  

Om een bestand naar een blob uploaden, ophalen van het volledige pad van het bestand door de naam van de map en de bestandsnaam op uw lokale schijf koppelen. Vervolgens kunt u het bestand uploaden naar het opgegeven pad met behulp van de **maken\_blob\_van\_pad** methode. 

De voorbeeldcode maakt een lokaal bestand moet worden gebruikt voor het uploaden en downloaden, het opslaan van het bestand moet worden geüpload als **bestand\_pad\_naar\_bestand** en de naam van de blob als **lokale\_bestand\_naam**. Het volgende voorbeeld wordt het bestand geüpload naar uw zogenaamd **quickstartblobs**.

```python
# Create a file in Documents to test the upload and download.
local_path=os.path.expanduser("~\Documents")
local_file_name ="QuickStart_" + str(uuid.uuid4()) + ".txt"
full_path_to_file =os.path.join(local_path, local_file_name)

# Write text to the file.
file = open(full_path_to_file,  'w')
file.write("Hello, World!")
file.close()

print("Temp file = " + full_path_to_file)
print("\nUploading to Blob storage as blob" + local_file_name)

# Upload the created file, use local_file_name for the blob name
block_blob_service.create_blob_from_path(container_name, local_file_name, full_path_to_file)
```

Er zijn verschillende methoden voor het uploaden die u met Blob storage gebruiken kunt. Bijvoorbeeld, als u een geheugenstream hebt, kunt u de **maken\_blob\_van\_stroom** methode in plaats van de **maken\_blob\_van\_pad**. 

Blok-blobs kunnen even groot zijn als 4,7 TB en kunnen van alles van Excel-werkbladen te grote videobestanden zijn. Pagina-blobs zijn voornamelijk gebruikt voor de VHD-bestanden gebruikt voor back-IaaS VM's. Toevoeg-BLOB's worden gebruikt voor logboekregistratie, zoals indien u wenst te schrijven naar een bestand en klik vervolgens houden meer gegevens toe te voegen. De meeste objecten die zijn opgeslagen in Blob storage zijn blok-blobs.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Een lijst met bestanden in de container met behulp van de **list_blobs** methode. Deze methode retourneert een generator. De volgende code haalt de lijst met blobs en doorlopen, met de namen van de blobs in een container.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>De blobs downloaden

Blobs downloaden naar uw lokale schijf met de **ophalen\_blob\_naar\_pad** methode. De volgende code downloadt de blob uploaden van een vorige sectie. '_DOWNLOADED' wordt toegevoegd als achtervoegsel aan de blob-naam, zodat u beide bestanden op lokale schijf kunt zien. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Resources opschonen
Als u de blobs in deze snelstartgids geüpload niet meer nodig hebt, kunt u verwijderen de volledige container via de **verwijderen\_container**. Als de bestanden die zijn gemaakt niet meer nodig zijn, gebruikt u de **verwijderen\_blob** methode om de bestanden te verwijderen.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Volgende stappen
 
In deze snelstartgids hebt u geleerd hoe u bestanden overbrengen tussen een lokale schijf en Azure blob-opslag met behulp van Python. Voor meer informatie over het werken met blob storage, blijven de instructies van de Blob-opslag.

> [!div class="nextstepaction"]
> [Procedures voor de bewerkingen van de BLOB-opslag](./storage-python-how-to-use-blob-storage.md)
 

Zie voor meer informatie over de Storage Explorer en Blobs [beheren Azure Blob storage-resources met Opslagverkenner](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).
