---
title: De Azure CLI 1.0 gebruiken met Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure-opdrachtregelinterface (Azure CLI) 1.0 met Azure Storage te maken en beheren van storage-accounts en werken met Azure BLOB's en bestanden. De Azure CLI is een hulpprogramma voor meerdere platforms
services: storage
documentationcenter: na
author: seguler
manager: jahogg
editor: tysonn
ms.assetid: b502232a-e8f6-4d6c-befd-3476592e0e35
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.openlocfilehash: 772417012e4c6aa519e83177bd8e93778f6af3b5
ms.sourcegitcommit: 42ee5ea09d9684ed7a71e7974ceb141d525361c9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2017
---
# <a name="using-the-azure-cli-10-with-azure-storage"></a>De Azure CLI 1.0 gebruiken met Azure Storage

## <a name="overview"></a>Overzicht

De Azure CLI biedt een set van open-source platformoverschrijdende opdrachten voor het werken met de Azure-Platform. Het biedt veel van dezelfde functionaliteit gevonden in de [Azure-portal](https://portal.azure.com) ook zo uitgebreid data access-functionaliteit.

In deze handleiding wordt besproken voor het gebruik van [Azure-opdrachtregelinterface (Azure CLI)](../../cli-install-nodejs.md) om uit te voeren een verscheidenheid aan taken voor ontwikkeling en het beheer met Azure Storage. U wordt aangeraden dat u downloaden en installeren of naar de nieuwste Azure CLI upgraden voordat u deze handleiding.

Deze handleiding wordt ervan uitgegaan dat u de basisconcepten van Azure Storage begrijpt. De handleiding bevat een aantal scripts voor het demonstreren van het gebruik van de Azure CLI met Azure Storage. Zorg ervoor dat de scriptvariabelen op basis van uw configuratie voordat elk script wordt uitgevoerd.

> [!NOTE]
> De handleiding bevat de Azure CLI-opdracht en het script voorbeelden voor klassieke opslagaccounts. Zie [met de Azure CLI voor Mac, Linux en Windows met Azure Resource Management](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) voor Azure CLI-opdrachten voor Resource Manager-opslagaccounts.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-cli-in-5-minutes"></a>Aan de slag met Azure Storage en Azure CLI in vijf minuten
Deze handleiding gebruikt Ubuntu voor voorbeelden, maar andere platforms OS moeten op dezelfde manier worden uitgevoerd.

**Nieuwe naar Azure:** ophalen van een Microsoft Azure-abonnement en een Microsoft-account die is gekoppeld aan dat abonnement. Zie voor informatie over Azure-Aankoopopties, [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/), [kopen opties](https://azure.microsoft.com/pricing/purchase-options/), en [lid biedt](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, Microsoft Partner Network BizSpark en andere Microsoft-programma's).

Zie [beheerdersrollen toewijzen in Azure Active Directory (Azure AD)](https://msdn.microsoft.com/library/azure/hh531793.aspx) voor meer informatie over Azure-abonnementen.

**Na het maken van een Microsoft Azure-abonnement en -account:**

1. Download en installeer de Azure CLI de aanwijzingen in [Azure CLI installeren](../../cli-install-nodejs.md).
2. Zodra de Azure CLI is geïnstalleerd, zich u op de azure-opdracht van de opdrachtregelinterface (Bash, Terminal-opdrachtprompt) voor toegang tot de Azure CLI-opdrachten. Typ de _azure_ opdracht en u ziet de volgende uitvoer.

    ![Azure opdrachtuitvoer](./media/storage-azure-cli/azure_command.png)   
3. Typ in de opdrachtregelinterface `azure storage` lijst met alle azure-opslag-opdrachten en ophalen van een eerste indruk van de functies van de Azure CLI biedt. U kunt de naam van de opdracht met typen **-h** parameter (bijvoorbeeld `azure storage share create -h`) om de details van de opdrachtsyntaxis te bekijken.
4. Nu geven we u een eenvoudig script waarin basic Azure CLI-opdrachten voor toegang tot Azure Storage. Het script eerst vragen u twee variabelen voor uw opslagaccount en de sleutel instellen. Het script wordt vervolgens een nieuwe container in deze nieuwe opslagaccount maken en uploaden van een bestaand installatiekopiebestand (blob) op die container. Nadat het script geeft een lijst van alle blobs in de container, zal deze het installatiekopiebestand downloaden naar de doelmap dat op de lokale computer bestaat.

    ```azurecli
    #!/bin/bash
    # A simple Azure storage example

    export AZURE_STORAGE_ACCOUNT=<storage_account_name>
    export AZURE_STORAGE_ACCESS_KEY=<storage_account_key>

    export container_name=<container_name>
    export blob_name=<blob_name>
    export image_to_upload=<image_to_upload>
    export destination_folder=<destination_folder>

    echo "Creating the container..."
    azure storage container create $container_name

    echo "Uploading the image..."
    azure storage blob upload $image_to_upload $container_name $blob_name

    echo "Listing the blobs..."
    azure storage blob list $container_name

    echo "Downloading the image..."
    azure storage blob download $container_name $blob_name $destination_folder

    echo "Done"
    ```

5. Open uw voorkeur teksteditor (vim voor voorbeeld) in uw lokale computer. Typ het bovenstaande script in een teksteditor.
6. Nu moet u de scriptvariabelen op basis van uw configuratie-instellingen bijwerken.

   * **< Storage_account_name >** de voornaam in het script of geef een nieuwe naam voor uw opslagaccount. **Belangrijk:** de naam van het opslagaccount moet uniek zijn in Azure. Er moet een kleine letter, te!
   * **< Storage_account_key >** de toegangssleutel van uw opslagaccount.
   * **< Container_name >** de voornaam in het script of geef een nieuwe naam voor de container.
   * **< Image_to_upload >** Voer een pad naar een afbeelding op uw lokale computer, zoals: ' ~ / images/HelloWorld.png '.
   * **< Destination_folder >** een pad opgeven naar een lokale map voor het opslaan van bestanden die zijn gedownload van Azure Storage, zoals: '~/downloadImages'.
7. Nadat u de benodigde variabelen in vim hebt bijgewerkt, drukt u op toetsencombinaties `ESC`, `:`, `wq!` om op te slaan van het script.
8. Als u wilt dit script uitvoert, typt u de naam van het script in de console bash. Nadat u dit script wordt uitgevoerd, moet u een lokale doelmap met het gedownloade bestand hebben. De volgende schermafbeelding ziet een voorbeeld van uitvoer:

Nadat het script wordt uitgevoerd, moet u een lokale doelmap met het gedownloade bestand hebben.

## <a name="manage-storage-accounts-with-the-azure-cli"></a>Storage-accounts met de Azure CLI beheren
### <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement
Hoewel de meeste van de opdrachten opslag zonder een Azure-abonnement werkt, wordt aangeraden u verbinding maken met uw abonnement met Azure CLI. Volg de stappen in de Azure CLI werkt met uw abonnement configureren [verbinding maken met een Azure-abonnement met de Azure CLI](/cli/azure/authenticate-azure-cli).

### <a name="create-a-new-storage-account"></a>Een nieuw opslagaccount maken
Voor het gebruik van Azure-opslag, moet u een opslagaccount. Nadat u uw computer verbinding maken met uw abonnement hebt geconfigureerd, kunt u een nieuwe Azure storage-account maken.

```azurecli
azure storage account create <account_name>
```

De naam van uw opslagaccount moet tussen 3 en 24 tekens lang zijn en alleen cijfers en kleine letters te gebruiken.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Een Azure storage-standaardaccount in omgevingsvariabelen instellen
U kunt meerdere opslagaccounts hebben in uw abonnement. U kunt kiezen één van deze en stel deze in de omgevingsvariabelen voor alle opslag-opdrachten in dezelfde sessie. Hiermee kunt u voor het uitvoeren van de Azure CLI-opdrachten voor opslag zonder op te geven van het opslagaccount en expliciet sleutel.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Verbindingsreeks maakt gebruik van een andere manier om een storage-standaardaccount. Ten eerste de verbindingsreeks ophalen door de opdracht:

```azurecli
azure storage account connectionstring show <account_name>
```

Vervolgens Kopieer de verbindingsreeks van de uitvoer en stel deze in op de omgevingsvariabele:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Maken en blobs beheren
Azure Blob storage is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die toegankelijk zijn vanuit overal ter wereld via HTTP of HTTPS. Deze sectie wordt ervan uitgegaan dat u al bekend met de Azure Blob storage-concepten bent. Zie voor gedetailleerde informatie [aan de slag met Azure Blob storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md) en [concepten van Blob-Service](http://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Een container maken
Elke blob in Azure-opslag moet zich in een container. U kunt maken met een privé-container met de `azure storage container create` opdracht:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Er zijn drie niveaus van anonieme toegang voor lezen: **uit**, **Blob**, en **Container**. Als u wilt voorkomen dat anonieme toegang tot blobs, stelt u de machtiging-parameter in op **uit**. Standaard worden de nieuwe container privé is en kan alleen worden geopend door de eigenaar van het account. Anonieme toegang toestaan openbare leestoegang tot blob-bronnen, maar niet aan de metagegevens van de container of aan de lijst met blobs in de container, stelt u de parameter machtiging op **Blob**. Stel de parameter machtiging op wilt toestaan volledige openbare leestoegang tot de blob-bronnen, container metagegevens en de lijst met blobs in de container, **Container**. Zie voor meer informatie [anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md).
>
>

### <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Azure Blob Storage ondersteunt blok-blobs en pagina-blobs. Zie voor meer informatie [blok-Blobs, toevoeg-Blobs en pagina-Blobs](http://msdn.microsoft.com/library/azure/ee691964.aspx).

Blobs in als u wilt uploaden een container, kunt u de `azure storage blob upload`. Standaard wordt met deze opdracht de lokale bestanden naar een blok-blob geüpload. Als u wilt opgeven welk type voor de blob, kunt u de `--blobtype` parameter.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Blobs downloaden uit een container
Het volgende voorbeeld laat zien hoe blobs downloaden uit een container.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Blobs kopiëren
U kunt blobs binnen of tussen opslagaccounts en regio's asynchroon kopiëren.

In het volgende voorbeeld ziet u hoe u blobs van één opslagaccount naar een ander kopieert. In dit voorbeeld maken we een container waarin blobs openbaar zijn, anoniem toegankelijk.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

Dit voorbeeld voert een asynchrone kopie. U kunt de status van elke kopieerbewerking bewaken door het uitvoeren van de `azure storage blob copy show` bewerking.

Let op de bron-URL voor de kopieerbewerking opgegeven moet openbaar toegankelijk, of een SAS (shared access signature)-token omvatten.

### <a name="delete-a-blob"></a>Een blob verwijderen
Als u wilt verwijderen van een blob, gebruiken de onderstaande opdracht:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Maken en beheren van bestandsshares
Azure Files biedt gedeelde opslag voor toepassingen die gebruikmaken van het standaard SMB-protocol. Microsoft Azure virtuele machines en cloud-services, evenals de on-premises toepassingen kunnen bestandsgegevens via gekoppelde shares delen. U kunt bestandsshares en bestandsgegevens via de Azure CLI kunt beheren. Zie voor meer informatie over Azure-bestanden [Inleiding tot Azure-bestanden](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Een bestandsshare maken
Een Azure-bestandsshare is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bestandsshare. Een account kan een onbeperkt aantal shares bevatten en een bestandsshare kan een onbeperkt aantal bestanden, tot de capaciteitslimiet van het opslagaccount opslaan. Het volgende voorbeeld wordt een bestandsshare met de naam **mijnshare**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Een map maken
Een directory biedt een optionele hiërarchische structuur voor een Azure-bestandsshare. Het volgende voorbeeld wordt een map met de naam **myDir** in de bestandsshare.

```azurecli
azure storage directory create myshare myDir
```

Opmerking die directorypad kan bestaan uit meerdere niveaus *bijvoorbeeld*, **een / b**. Echter, moet u ervoor zorgen dat alle bovenliggende mappen bestaan. Bijvoorbeeld: voor pad **een / b**, moet u directory **een** vervolgens Maak eerst map **b**.

### <a name="upload-a-local-file-to-directory"></a>Een lokaal bestand uploaden naar de directory
Het volgende voorbeeld wordt een bestand uit geüpload **~/temp/samplefile.txt** naar de **myDir** directory. Bewerk het bestandspad, zodat deze naar een geldig bestand op uw lokale machine verwijst:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Houd er rekening mee dat een bestand in de share mag maximaal 1 TB groot.

### <a name="list-the-files-in-the-share-root-or-directory"></a>Lijst van de bestanden in de hoofdmap van de share of map
U kunt een lijst met de bestanden en submappen in de hoofdmap van een share of map met de volgende opdracht:

```azurecli
azure storage file list myshare myDir
```

Houd er rekening mee dat de mapnaam optioneel voor de bewerking van de aanbieding is. Als u dit weglaat, wordt de opdracht de inhoud van de hoofdmap van de share.

### <a name="copy-files"></a>Bestanden kopiëren
Vanaf versie 0.9.8 van Azure CLI, kunt u een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Hieronder ziet u hoe u deze kopieerbewerkingen uitvoert met behulp van de CLI-opdrachten. Een bestand naar de nieuwe map kopiëren:

```azurecli
azure storage file copy start --source-share srcshare --source-path srcdir/hello.txt --dest-share destshare
    --dest-path destdir/hellocopy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

Een blob kopieert naar een map:

```azurecli
azure storage file copy start --source-container srcctn --source-blob hello2.txt --dest-share hello
    --dest-path hellodir/hello2copy.txt --connection-string $srcConnectionString --dest-connection-string $destConnectionString
```

## <a name="next-steps"></a>Volgende stappen

U vindt de Azure CLI 1.0-opdrachtregelprogramma voor het werken met opslagbronnen hier:

* [Azure CLI-opdrachten in de modus Resource Manager](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Azure CLI-opdrachten in de Azure Service Management-modus](../../cli-install-nodejs.md)

Ook wilt proberen de [Azure CLI 2.0](../storage-azure-cli.md), de volgende generatie CLI geschreven in Python, voor gebruik met het implementatiemodel van Resource Manager.
