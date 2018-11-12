---
title: Met behulp van de klassieke Azure CLI met Azure Storage | Microsoft Docs
description: Informatie over het gebruik van de Azure classic opdrachtregelinterface (CLI) met Azure Storage te maken en beheren van storage-accounts en werken met Azure-blobs en bestanden.
services: storage
author: seguler
ms.service: storage
ms.topic: article
ms.date: 01/30/2017
ms.author: seguler
ms.component: common
ms.openlocfilehash: 28c9e342aec3eb3ba61b46f4f80c7d097b0653b6
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51258711"
---
# <a name="using-the-azure-classic-cli-with-azure-storage"></a>Met behulp van de klassieke Azure CLI met Azure Storage

## <a name="overview"></a>Overzicht

De klassieke Azure-CLI biedt een open-source, platformoverschrijdende opdrachten om te werken met het Azure-Platform. Het biedt veel van dezelfde functionaliteit gevonden in de [Azure-portal](https://portal.azure.com) ook als uitgebreide data access-functionaliteit.

In deze handleiding wordt beschreven hoe u [Azure klassieke CLI](../../cli-install-nodejs.md) om uit te voeren van een verscheidenheid aan ontwikkeling en beheer taken met Azure Storage. U wordt aangeraden dat u downloaden en installeren of naar de meest recente klassieke CLI upgraden voordat u deze handleiding.

Deze handleiding wordt ervan uitgegaan dat u inzicht in de basisbeginselen van Azure Storage. De handleiding bevat een aantal scripts ter illustratie van het gebruik van de klassieke CLI met Azure Storage. Zorg ervoor dat de scriptvariabelen op basis van uw configuratie voor elk script is uitgevoerd.

> [!NOTE]
> De handleiding bevat de Azure klassieke CLI opdrachtregels en scriptregels-voorbeelden voor klassieke opslagaccounts. Zie [met de Azure CLI voor Mac, Linux en Windows met Azure Resource Management](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects) voor Azure classic CLI-opdrachten voor Resource Manager-opslagaccounts.
>
>

[!INCLUDE [storage-cli-versions](../../../includes/storage-cli-versions.md)]

## <a name="get-started-with-azure-storage-and-the-azure-classic-cli-in-5-minutes"></a>Aan de slag met Azure Storage en de klassieke Azure CLI in vijf minuten
Deze handleiding Ubuntu gebruikt voor voorbeelden, maar andere OS-platformen op dezelfde manier moeten uitvoeren.

**Nieuw bij Azure:** ophalen van een Microsoft Azure-abonnement en een Microsoft-account dat aan het abonnement is gekoppeld. Zie voor meer informatie over Azure-Aankoopopties [gratis proefversie](https://azure.microsoft.com/pricing/free-trial/), [kopen opties](https://azure.microsoft.com/pricing/purchase-options/), en [aanbiedingen voor leden](https://azure.microsoft.com/pricing/member-offers/) (voor leden van MSDN, Microsoft Partner Network en BizSpark, en andere Microsoft-programma's).

Zie [beheerdersrollen toewijzen in Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles-azure-portal) voor meer informatie over Azure-abonnementen.

**Na het maken van een Microsoft Azure-abonnement en -account:**

1. Download en installeer de Azure klassieke CLI volgens de instructies die worden beschreven in [de klassieke Azure-CLI installeren](../../cli-install-nodejs.md).
2. Zodra de klassieke CLI is geïnstalleerd, kunt u zich op de azure-opdracht uit via de opdrachtregelinterface (Bash, Terminal-opdrachtprompt) voor toegang tot de klassieke CLI-opdrachten. Type de _azure_ opdracht en u ziet de volgende uitvoer.

    ![Uitvoer van de Azure-opdracht](./media/storage-azure-cli/azure_command.png)   
3. Typ in de opdrachtregelinterface `azure storage` lijst met alle opdrachten in de azure-opslag en het ophalen van een eerste indruk van de functies van de klassieke CLI biedt. U kunt de naam van de opdracht met typen **-h** parameter (bijvoorbeeld `azure storage share create -h`) om de details van de opdrachtsyntaxis te bekijken.
4. Nu geven we u een eenvoudig script waarin basic klassieke CLI-opdrachten voor toegang tot Azure Storage. Het script wordt eerst gevraagd om in te stellen van de twee variabelen die voor uw opslagaccount en de sleutel. Het script wordt vervolgens een nieuwe container maken in deze nieuwe storage-account en een bestaand installatiekopiebestand (blob) uploaden naar deze container. Nadat het script geeft een lijst van alle blobs in die container, wordt deze de installatiekopie-bestand downloaden naar de doelmap die alleen op de lokale computer bestaat.

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

5. Open uw favoriete teksteditor (vim bijvoorbeeld) in uw lokale computer. Typ het bovenstaande script in de teksteditor.
6. Nu moet u de scriptvariabelen op basis van uw configuratie-instellingen bijwerken.

   * **< Naam_opslagaccount >** gebruik van de opgegeven naam in het script of voer een nieuwe naam voor uw storage-account. **Belangrijk:** de naam van het opslagaccount moet uniek zijn in Azure. Er moet een kleine letter, te!
   * **< Storage_account_key >** de toegangssleutel van uw storage-account.
   * **< Container_name >** gebruik van de opgegeven naam in het script of voer een nieuwe naam voor de container.
   * **< Image_to_upload >** Voer een pad naar een afbeelding op uw lokale computer, zoals: ' ~ / images/HelloWorld.png '.
   * **< Destination_folder >** Geef het pad naar een lokale map voor het opslaan van bestanden die worden gedownload uit Azure Storage, zoals: '~/downloadImages'.
7. Nadat u de benodigde variabelen in vim hebt bijgewerkt, drukt u op toetsencombinaties `ESC`, `:`, `wq!` om op te slaan van het script.
8. Als u wilt uitvoeren met dit script, typt u de bestandsnaam van het script in de bash-console. Na dit script wordt uitgevoerd, moet u een lokale map waarin het gedownloade bestand hebben. De volgende schermafbeelding ziet u een voorbeeld van uitvoer:

Nadat het script wordt uitgevoerd, moet u een lokale map waarin het gedownloade bestand hebben.

## <a name="manage-storage-accounts-with-the-azure-classic-cli"></a>Storage-accounts beheren met de klassieke Azure-CLI
### <a name="connect-to-your-azure-subscription"></a>Verbinding maken met uw Azure-abonnement
Hoewel de meeste van de opslagopdrachten zonder een Azure-abonnement werkt, adviseren we u verbinding maken met uw abonnement vanuit de klassieke CLI.

### <a name="create-a-new-storage-account"></a>Een nieuw opslagaccount maken
Voor het gebruik van Azure storage, moet u een opslagaccount. Nadat u uw computer verbinding maken met uw abonnement hebt geconfigureerd, kunt u een nieuw Azure storage-account maken.

```azurecli
azure storage account create <account_name>
```

De naam van uw opslagaccount moet tussen 3 en 24 tekens lang zijn en cijfers en kleine letters bevatten.

### <a name="set-a-default-azure-storage-account-in-environment-variables"></a>Een standaard Azure storage-account in omgevingsvariabelen instellen
U kunt meerdere opslagaccounts hebben in uw abonnement. U kunt Kies een van deze en stel deze in de omgevingsvariabelen voor alle opslagopdrachten in dezelfde sessie. Hiermee kunt u voor het uitvoeren van de CLI-opdrachten voor klassieke opslag zonder het storage-account op te geven en expliciet sleutel.

```azurecli
export AZURE_STORAGE_ACCOUNT=<account_name>
export AZURE_STORAGE_ACCESS_KEY=<key>
```

Verbindingsreeks maakt gebruik van een andere manier om in te stellen van een storage-standaardaccount. Ten eerste de verbindingsreeks ophalen door de opdracht:

```azurecli
azure storage account connectionstring show <account_name>
```

Vervolgens kopieert u de verbindingsreeks van de uitvoer en instellen op omgevingsvariabele:

```azurecli
export AZURE_STORAGE_CONNECTION_STRING=<connection_string>
```

## <a name="create-and-manage-blobs"></a>Maken en blobs beheren
Azure Blob-opslag is een service voor het opslaan van grote hoeveelheden ongestructureerde gegevens, zoals tekst of binaire gegevens, die kunnen worden benaderd vanaf elke locatie ter wereld via HTTP of HTTPS. In deze sectie wordt ervan uitgegaan dat u al bekend met de concepten van Azure Blob storage bent. Zie voor gedetailleerde informatie [aan de slag met Azure Blob storage met .NET](../blobs/storage-dotnet-how-to-use-blobs.md) en [concepten van Blob Service](https://msdn.microsoft.com/library/azure/dd179376.aspx).

### <a name="create-a-container"></a>Een container maken
Elke blob in Azure storage moet zich in een container. U kunt maken met een privé-container met de `azure storage container create` opdracht:

```azurecli
azure storage container create mycontainer
```

> [!NOTE]
> Er zijn drie niveaus van anonieme leestoegang: **uit**, **Blob**, en **Container**. Om te voorkomen dat anonieme toegang tot blobs, stelt u de parameter machtiging in op **uit**. Standaard worden de nieuwe container privé en kan alleen worden geopend door de eigenaar van het. Anonieme toegang toestaan openbare leestoegang tot blob-resources, maar niet aan de containermetagegevens van de of aan de lijst met blobs in de container, stelt u de parameter machtiging op **Blob**. Als u wilt toestaan dat volledige openbare leestoegang tot de blob-resources, de containermetagegevens van de en de lijst met blobs in de container, kunt u de machtiging-parameter instellen op **Container**. Zie [Anonieme leestoegang tot containers en blobs beheren](../blobs/storage-manage-access-to-resources.md) voor meer informatie.
>
>

### <a name="upload-a-blob-into-a-container"></a>Een blob uploaden naar een container
Azure Blob Storage ondersteunt blok-blobs en pagina-blobs. Zie voor meer informatie, [blok-Blobs, toevoeg-Blobs en pagina-Blobs](https://msdn.microsoft.com/library/azure/ee691964.aspx).

Als u wilt uploaden blobs in een container, kunt u de `azure storage blob upload`. Met deze opdracht wordt standaard de lokale bestanden geüpload naar een blok-blob. Als u wilt het type voor de blob opgeven, kunt u de `--blobtype` parameter.

```azurecli
azure storage blob upload '~/images/HelloWorld.png' mycontainer myBlockBlob
```

### <a name="download-blobs-from-a-container"></a>Blobs downloaden uit een container
Het volgende voorbeeld ziet u hoe u blobs downloaden uit een container.

```azurecli
azure storage blob download mycontainer myBlockBlob '~/downloadImages/downloaded.png'
```

### <a name="copy-blobs"></a>Blobs kopiëren
U kunt blobs binnen of tussen opslagaccounts en regio's asynchroon kopiëren.

In het volgende voorbeeld ziet u hoe u blobs van één opslagaccount naar een ander kopieert. In dit voorbeeld maken we een container waarin blobs openbaar zijn, anoniem toegankelijk is.

```azurecli
azure storage container create mycontainer2 -a <accountName2> -k <accountKey2> -p Blob

azure storage blob upload '~/Images/HelloWorld.png' mycontainer2 myBlockBlob2 -a <accountName2> -k <accountKey2>

azure storage blob copy start 'https://<accountname2>.blob.core.windows.net/mycontainer2/myBlockBlob2' mycontainer
```

In dit voorbeeld voert een asynchroon kopiëren. U kunt de status van elke kopieerbewerking controleren door het uitvoeren van de `azure storage blob copy show` bewerking.

Houd er rekening mee dat de bron-URL voor de kopieerbewerking opgegeven moet openbaar toegankelijk is, of een SAS (shared access signature)-token bevatten.

### <a name="delete-a-blob"></a>Een blob verwijderen
Als u wilt verwijderen van een blob, gebruikt u de onderstaande opdracht:

```azurecli
azure storage blob delete mycontainer myBlockBlob2
```

## <a name="create-and-manage-file-shares"></a>Maken en beheren van bestandsshares
Azure Files biedt gedeelde opslag voor toepassingen die gebruikmaken van de SMB-protocol. Microsoft Azure virtuele machines en cloudservices, evenals on-premises toepassingen, kunnen bestandsgegevens via gekoppelde shares delen. U kunt bestandsshares en gegevens uit een bestand via de klassieke CLI beheren. Zie voor meer informatie over Azure Files [Inleiding tot Azure Files](../files/storage-files-introduction.md).

### <a name="create-a-file-share"></a>Een bestandsshare maken
Een Azure-bestandsshare is een SMB-bestandsshare in Azure. Alle mappen en bestanden moeten worden gemaakt in een bestandsshare. Een account kan een onbeperkt aantal shares bevatten en een share kan een onbeperkt aantal bestanden, tot de capaciteitslimiet van het opslagaccount. Het volgende voorbeeld wordt een bestandsshare met de naam **myshare**.

```azurecli
azure storage share create myshare
```

### <a name="create-a-directory"></a>Een map maken
Een directory van azurebiedt een optionele hiërarchische structuur voor een Azure-bestandsshare. Het volgende voorbeeld wordt een map met de naam **mijnmap** in de bestandsshare.

```azurecli
azure storage directory create myshare myDir
```

Houd er rekening mee dat pad kan meerdere niveaus bevatten *bijvoorbeeld*, **een / b**. Echter, moet u ervoor zorgen dat alle bovenliggende mappen bestaan. Bijvoorbeeld, voor het pad **een / b**, moet u de directory maken **een** eerst, map vervolgens maken **b**.

### <a name="upload-a-local-file-to-directory"></a>Een lokaal bestand uploaden naar map
Het volgende voorbeeld wordt een bestand geüpload **~/temp/samplefile.txt** naar de **mijnmap** directory. Bewerk het bestandspad zodat deze naar een geldig bestand op uw lokale computer verwijst:

```azurecli
azure storage file upload '~/temp/samplefile.txt' myshare myDir
```

Houd er rekening mee dat een bestand in de share tot 1 TB in grootte kan worden.

### <a name="list-the-files-in-the-share-root-or-directory"></a>De bestanden in de hoofdmap van de share of map
U kunt een lijst de bestanden en submappen in de hoofdmap van een share of een map met de volgende opdracht:

```azurecli
azure storage file list myshare myDir
```

Houd er rekening mee dat de naam van de map optioneel voor de lijstbewerking is. Als u dit weglaat, wordt de opdracht de inhoud van de hoofdmap van de share.

### <a name="copy-files"></a>Bestanden kopiëren
Vanaf versie 0.9.8 van de klassieke CLI, kunt u een bestand kopiëren naar een ander bestand, een bestand naar een blob of een blob naar een bestand. Hieronder ziet hoe u deze kopieerbewerkingen met behulp van CLI-opdrachten uitvoert. Een bestand kopiëren naar de nieuwe map:

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

Voor het werken met resources voor Storage Hier vindt u Azure klassieke CLI-opdrachten:

* [Azure klassieke CLI-opdrachten in Resource Manager-modus](../../virtual-machines/azure-cli-arm-commands.md#azure-storage-commands-to-manage-your-storage-objects)
* [Azure klassieke CLI-opdrachten in Azure Service Management-modus](../../cli-install-nodejs.md)

U mogelijk ook interessant om te proberen de nieuwste versie van de [Azure CLI](../storage-azure-cli.md), voor gebruik met het Resource Manager-implementatiemodel.
