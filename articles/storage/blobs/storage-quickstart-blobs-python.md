---
title: Azure Quick Start - Objecten overdragen naar/van Azure Blob-opslag met Python | Microsoft Docs
description: Snel leren om objecten over te dragen naar/van Azure Blob-opslag met Python
services: storage
author: ruthogunnnaike
manager: jeconnoc
ms.service: storage
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: quickstart
ms.date: 10/12/2017
ms.author: v-ruogun
ms.openlocfilehash: 50f43e6ef9ee60cbf489bb8d0c1c64ca61a393e1
ms.sourcegitcommit: 85012dbead7879f1f6c2965daa61302eb78bd366
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/02/2018
---
#  <a name="transfer-objects-tofrom-azure-blob-storage-using-python"></a>Objecten overdragen naar/van Azure Blob-opslag met Python
In deze zelfstudie leert u hoe u Python kunt gebruiken om blok-bobs te uploaden, te downloaden en weer te geven in een container in Azure Blob-opslag. 

## <a name="prerequisites"></a>Vereisten

Dit zijn de vereisten voor het voltooien van deze Quickstart: 
* [Python](https://www.python.org/downloads/) installeren
* Download en installeer [Azure Storage-SDK voor Python](storage-python-how-to-use-blob-storage.md#download-and-install-azure-storage-sdk-for-python). 

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [storage-quickstart-tutorial-create-account-portal](../../../includes/storage-quickstart-tutorial-create-account-portal.md)]

## <a name="download-the-sample-application"></a>De voorbeeldtoepassing downloaden
De [voorbeeldtoepassing](https://github.com/Azure-Samples/storage-blobs-python-quickstart.git) die in deze Quick Start wordt gebruikt, is een Python-basistoepassing.  

Gebruik [git](https://git-scm.com/) om een kopie van de toepassing naar uw ontwikkelomgeving te downloaden. 

```bash
git clone https://github.com/Azure-Samples/storage-blobs-python-quickstart.git 
```

Met deze opdracht wordt de opslagplaats naar uw lokale git-map gekloond. U kunt het Python-programma openen door te zoeken naar de map storage-blobs-python-quickstart en het bestand example.py.  

## <a name="configure-your-storage-connection-string"></a>De opslagverbindingsreeks configureren
In de toepassing moet u naam en de accountsleutel van uw opslagaccount opgeven om een `BlockBlobService`-object te kunnen maken. Open het bestand `example.py` vanuit de Solution Explorer in uw IDE. Vervang de waarden **accountname** en **accountkey** met uw accountnaam en de -sleutel. 

```python 
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
```

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren
In dit voorbeeld wordt een testbestand in de map Documenten gemaakt. Door het voorbeeldprogramma uit te voeren wordt een testbestand geüpload naar de Blob-opslag, worden de blobs in the container vermeld en wordt het bestand met een nieuwe naam gedownload. 

Voet het voorbeeld uit. De volgende uitvoer is een voorbeeld van de uitvoer die wordt geretourneerd wanneer de toepassing wordt uitgevoerd:
  
```
Temp file = C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Uploading to Blob storage as blobQuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

List blobs in the container
         Blob name: QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078.txt

Downloading blob to C:\Users\azureuser\Documents\QuickStart_9f4ed0f9-22d3-43e1-98d0-8b2c05c01078_DOWNLOADED.txt
```
Wanneer u op een willekeurige toets drukt om door te gaan, worden de opslagcontainer en de bestanden door het voorbeeldprogramma verwijderd. Voordat u doorgaat moet u controleren of de map Documenten de twee bestanden bevat. Als u ze opent, ziet u dat ze identiek zijn.

U kunt ook een hulpprogramma zoals [Azure Storage Explorer](http://storageexplorer.com) gebruiken om de bestanden in de Blob-opslag te bekijken. Azure Storage Explorer is een gratis hulpprogramma voor meerdere platforms waarmee u toegang hebt tot de gegevens van uw opslagaccount. 

Nadat u de bestanden hebt gecontroleerd, drukt u op een willekeurige toets om de demo te voltooien en de testbestanden te verwijderen. Nu u weet wat het voorbeeldprogramma doet, opent u het bestand example.py om de code te bekijken. 

## <a name="understand-the-sample-code"></a>De voorbeeldcode begrijpen

We gaan nu de voorbeeldcode bespreken, zodat u begrijpt hoe deze werkt.

### <a name="get-references-to-the-storage-objects"></a>Verwijzingen naar de opslagobjecten ophalen
Als eerste moeten verwijzingen worden gemaakt naar objecten die worden gebruikt voor het verkrijgen van toegang tot de Blob-opslag en voor het beheren ervan. Deze objecten worden boven op elkaar gebouwd en elk ervan wordt door de volgende in de lijst gebruikt.

* Maak een exemplaar van het object **BlockBlobService** dat naar de Blob-service in uw opslagaccount verwijst. 

* Maak een exemplaar van het object **CloudBlobContainer** dat de container vertegenwoordigt die u wilt openen. Containers worden gebruikt om uw blobs te organiseren op een wijze die lijkt op het gebruik van mappen op uw computer om uw bestanden te organiseren.

Zodra u de Cloud Blob-container hebt, kunt u een exemplaar maken van het object **CloudBlockBlob** dat verwijst naar de specifieke blob waarin u bent geïnteresseerd, en kunt u bewerkingen uitvoeren zoals uploaden, downloaden en kopiëren.

> [!IMPORTANT]
> Containernamen moeten uit kleine letters bestaan. Zie [Naming and referencing containers, blobs, and metadata](https://docs.microsoft.com/rest/api/storageservices/naming-and-referencing-containers--blobs--and-metadata) (Containers, blobs en metagegevens een naam geven en hiernaar verwijzen) voor meer informatie over namen voor blobs en containers.

In deze sectie gaat u exemplaren maken van de objecten, een nieuwe container maken en vervolgens machtigingen instellen voor de container, zodat de blobs openbaar zijn. De container heeft de naam **quickstartblobs**. 

```python 
# Create the BlockBlockService that is used to call the Blob service for the storage account
block_blob_service = BlockBlobService(account_name='accountname', account_key='accountkey') 
 
# Create a container called 'quickstartblobs'.
container_name ='quickstartblobs'
block_blob_service.create_container(container_name) 

# Set the permission so the blobs are public.
block_blob_service.set_container_acl(container_name, public_access=PublicAccess.Container)
```
### <a name="upload-blobs-to-the-container"></a>Blobs uploaden naar de container

Blob-opslag ondersteunt blok-blobs, toevoeg-blobs en pagina-blobs. Omdat blok-blobs het meest worden gebruikt, worden ze in deze Quick Start gebruikt.  

Als u een bestand naar een blob wilt uploaden, hebt u het volledige pad van het bestand nodig dat u vindt door de naam van de map en de bestandsnaam op uw lokale schijf samen te voegen. Vervolgens kunt u het bestand uploaden naar het opgegeven pad met behulp van de methode **create\_blob\_from\_path**. 

Met de voorbeeldcode wordt een lokaal bestand gemaakt voor het uploaden en downloaden, waarmee het bestand dat moet worden geüpload als **file\_path\_to\_file** en de naam van de blob als **local\_file\_name** wordt opgeslagen. In het volgende voorbeeld wordt het bestand geüpload naar de container met de naam **quickstartblobs**.

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

Er zijn meerdere uploadmethoden die u kunt gebruiken met Blob-opslag. Als u bijvoorbeeld een geheugenstroom hebt, kunt u de methode **create\_blob\_from\_stream** gebruiken in plaats van **create\_blob\_from\_path**. 

Blok-blobs kunnen tot 4,7 TB groot zijn en kunnen van alles zijn: van Excel-spreadsheets tot grote videobestanden. Pagina-blobs worden met name gebruikt voor de VHD-bestanden die worden gebruikt ter ondersteuning van IaaS-VM‘s. Toevoeg-blobs worden gebruikt voor logboekregistratie, bijvoorbeeld wanneer u gegevens wilt wegschrijven naar een bestand en vervolgens gegevens wilt blijven toevoegen. De meeste objecten die worden opgeslagen in Blob-opslag, zijn blok-blobs.

### <a name="list-the-blobs-in-a-container"></a>De blobs in een container in een lijst weergeven

Haal een lijst met bestanden op in de container met behulp van de methode **list_blobs**. Deze methode retourneert een generator. Met de volgende code wordt de lijst met blobs opgehaald en doorlopen, waarbij de namen van de blobs worden weergegeven die in een container zijn aangetroffen.  

```python
# List the blobs in the container
print("\nList blobs in the container")
    generator = block_blob_service.list_blobs(container_name)
    for blob in generator:
        print("\t Blob name: " + blob.name)
```

### <a name="download-the-blobs"></a>De blobs downloaden

Download blobs naar uw lokale schijf met de methode **get\_blob\_to\_path**. Met de volgende code wordt een blob gedownload die in een vorige sectie is geüpload. '_DOWNLOADED' wordt toegevoegd als achtervoegsel aan de naam van de blob, zodat u beide bestanden op de lokale schijf kunt zien. 

```python
# Download the blob(s).
# Add '_DOWNLOADED' as prefix to '.txt' so you can see both files in Documents.
full_path_to_file2 = os.path.join(local_path, string.replace(local_file_name ,'.txt', '_DOWNLOADED.txt'))
print("\nDownloading blob to " + full_path_to_file2)
block_blob_service.get_blob_to_path(container_name, local_file_name, full_path_to_file2)
```

### <a name="clean-up-resources"></a>Resources opschonen
Als u de blobs die in deze Quick Start zijn geüpload niet langer nodig hebt, kunt u de volledige container verwijderen met behulp van **delete\_container**. Als de bestanden die zijn gemaakt niet meer nodig zijn, gebruikt u de methode **delete\_blob** om ze te verwijderen.

```python
# Clean up resources. This includes the container and the temp files
block_blob_service.delete_container(container_name)
os.remove(full_path_to_file)
os.remove(full_path_to_file2)
```

## <a name="next-steps"></a>Volgende stappen
 
In deze Quick Start hebt u geleerd hoe u bestanden overbrengt tussen een lokale schijf en Azure Blob-opslag met behulp van Python. Voor meer informatie over het werken met Blob-opslag, gaat u naar de instructies voor Blob-opslag.

> [!div class="nextstepaction"]
> [Instructies voor bewerkingen in Blob-opslag](./storage-python-how-to-use-blob-storage.md)
 

Zie [Azure Blob-opslagresources beheren met Storage Explorer](../../vs-azure-tools-storage-explorer-blobs.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json) voor meer informatie over Storage Explorer en blobs.
