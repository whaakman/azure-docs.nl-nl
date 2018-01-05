---
title: Uploaden van afbeeldingsgegevens in de cloud met Azure Storage | Microsoft Docs
description: Azure blob storage gebruiken met een web-app voor het opslaan van toepassingsgegevens
services: storage
documentationcenter: 
author: georgewallace
manager: timlt
editor: 
ms.service: storage
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: csharp
ms.topic: tutorial
ms.date: 09/19/2017
ms.author: gwallace
ms.custom: mvc
ms.openlocfilehash: 8d187e51cbb391ee1f34fb5934c8ae1868bb6244
ms.sourcegitcommit: 3cdc82a5561abe564c318bd12986df63fc980a5a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Uploaden van afbeeldingsgegevens in de cloud met Azure Storage

Deze zelfstudie maakt deel uit een reeks. Deze zelfstudie laat zien hoe u een webtoepassing die gebruikmaakt van de Azure Storage-clientbibliotheek voor het uploaden van afbeeldingen met een opslagaccount te implementeren. Wanneer u klaar bent, hebt u een web-app opslaan en weergeven van installatiekopieën van Azure-opslag.

![De weergave van afbeeldingen container](media/storage-upload-process-images/figure2.png)

Deel een van de reeks, leert u hoe:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Een container maken en machtigingen instellen
> * Een toegangssleutel ophalen
> * Configureer toepassingsinstellingen
> * Een Web-App implementeren in Azure
> * Interactie met de webtoepassing

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in deze zelfstudie vereist dat u de Azure CLI versie 2.0.4 zijn uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken 

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.
 
In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>Een opslagaccount maken
 
Installatiekopieën van uploadt het voorbeeld naar een blobcontainer in Azure Storage-account. Een opslagaccount biedt een unieke naamruimte voor het opslaan en toegang tot uw Azure storage-gegevensobjecten. Maak een opslagaccount in de resourcegroep die u hebt gemaakt met behulp van de opdracht [az storage account create](/cli/azure/storage/account#az_storage_account_create). 

> [!IMPORTANT] 
> In deel 2 van de zelfstudie gebruikt u abonnementen voor blob-opslag. Gebeurtenisabonnementen worden momenteel alleen ondersteund voor Blob storage-accounts in de West-Centraal VS en VS-West 2. Vanwege deze beperking, moet u een Blob storage-account dat wordt gebruikt door de voorbeeld-app voor het opslaan van afbeeldingen en miniaturen maken.   

In de volgende opdracht te vervangen door uw eigen globaal unieke naam op voor de Blob storage-account waarin u zien hoe de `<blob_storage_account>` tijdelijke aanduiding.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>Blob storage-containers maken
 
De app gebruikmaakt van twee containers in de Blob storage-account. Containers zijn vergelijkbaar met mappen en worden gebruikt voor het opslaan van blobs. De _installatiekopieën_ container is waar de app uploadt installatiekopieën van een hoge resolutie. Op een hoger deel van de reeks een Azure-functie-app geüpload formaat miniatuurweergaven voor de _duim_ container. 

De opslagaccountsleutel ophalen met behulp van de [lijst met opslagaccounts die sleutels az](/cli/azure/storage/account/keys#list) opdracht. Vervolgens gebruikt u deze sleutel voor het maken van twee containers met behulp van de [az storage-container maken](/cli/azure/storage/container#az_storage_container_create) opdracht.  
 
In dit geval `<blob_storage_account>` is de naam van de Blob storage-account die u hebt gemaakt. De _installatiekopieën_ containers openbare toegang is ingesteld op `off`, wordt de _duim_ containers openbare toegang is ingesteld op `container`. De `container` openbare toegangsinstelling kan de miniaturen kan worden bekeken naar mensen die gaat u naar de webpagina.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbs --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Maak een notitie van uw blob-opslag-accountnaam en de sleutel. De voorbeeld-app gebruikt deze instellingen voor verbinding met de storage-account voor het uploaden van afbeeldingen. 

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken 

Een [App Service-plan](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) geeft de locatie, de grootte en de functies van de webserverfarm aan die als host fungeert voor uw app. 

Maak een App Service-plan met de opdracht [az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create). 

In het volgende voorbeeld wordt een App Service-plan gemaakt met de naam `myAppServicePlan` en de prijscategorie **Gratis**: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Een webtoepassing maken 

De web-app biedt een hosting ruimte voor de voorbeeldcode van de app die vanuit de GitHub-opslagplaats voorbeeld is geïmplementeerd. Maak een [web-app](../../app-service/app-service-web-overview.md) in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp#az_webapp_create).  
 
Vervang in de volgende opdracht `<web_app>` met een unieke naam (geldige tekens zijn `a-z`, `0-9`, en `-`). Als `<web_app>` is niet uniek is, dat u het volgende foutbericht: _Website met de gegeven naam `<web_app>` bestaat al._ De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Implementeer de voorbeeldapp vanuit de GitHub-opslagplaats 

App Service biedt ondersteuning voor verschillende manieren om inhoud te distribueren naar een web-app. In deze zelfstudie implementeert u de web-app uit een [openbaar voorbeeld GitHub-opslagplaats](https://github.com/Azure-Samples/storage-blob-upload-from-webapp). Configureren van de GitHub-implementatie naar de web-app met de [az webapp implementatieconfiguratie bron](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config) opdracht. Vervang `<web_app>` met de naam van de web-app die u in de vorige stap hebt gemaakt.

Het voorbeeldproject bevat een [ASP.NET MVC](https://www.asp.net/mvc) app dat accepteert van een afbeelding, opgeslagen in een opslagaccount en afbeeldingen uit een container miniaturen worden weergegeven. De webtoepassing gebruikt het [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet), en de [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) naamruimten uit in de Azure-opslag om te communiceren met Azure storage-clientbibliotheek. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Instellingen voor web-app configureren 

Het voorbeeld web-app gebruikt de [Azure Storage-clientbibliotheek](/dotnet/api/overview/azure/storage?view=azure-dotnet) aan aanvraag toegangstokens, die worden gebruikt voor het uploaden van afbeeldingen. De opslagaccountreferenties die wordt gebruikt door de opslag-SDK worden ingesteld in de toepassingsinstellingen voor de web-app. Toepassingsinstellingen toevoegen aan de geïmplementeerde app met de [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set) opdracht. 

In de volgende opdracht `<blob_storage_account>` is de naam van uw Blob storage-account en `<blob_storage_key>` is van de bijbehorende sleutel. Vervang `<web_app>` met de naam van de web-app die u in de vorige stap hebt gemaakt.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbs \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Nadat de web-app is geïmplementeerd en geconfigureerd, kunt u de installatiekopie uploaden functionaliteit testen in de app.   

## <a name="upload-an-image"></a>Een installatiekopie uploaden 

Als u wilt testen van de web-app, blader naar de URL van uw gepubliceerde app. De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`. Selecteer de **foto's uploaden** regio te selecteren en het uploaden van een bestand of het slepen en neerzetten van een bestand op het gebied. De afbeelding verdwijnt als geüpload.

![ImageResizer app](media/storage-upload-process-images/figure1.png)

In de voorbeeldcode de `UploadFiletoStorage` taak de `Storagehelper.cs` bestand wordt gebruikt om de installatiekopieën uploadt naar de `images` container binnen de storage-account via de [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet) methode. De volgende voorbeeldcode bevat de `UploadFiletoStorage` taak. 

```csharp
public static async Task<bool> UploadFileToStorage(Stream fileStream, string fileName, AzureStorageConfig _storageConfig)
{
    // Create storagecredentials object by reading the values from the configuration (appsettings.json)
    StorageCredentials storageCredentials = new StorageCredentials(_storageConfig.AccountName, _storageConfig.AccountKey);

    // Create cloudstorage account by passing the storagecredentials
    CloudStorageAccount storageAccount = new CloudStorageAccount(storageCredentials, true);

    // Create the blob client.
    CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();

    // Get reference to the blob container by passing the name by reading the value from the configuration (appsettings.json)
    CloudBlobContainer container = blobClient.GetContainerReference(_storageConfig.ImageContainer);

    // Get the reference to the block blob from the container
    CloudBlockBlob blockBlob = container.GetBlockBlobReference(fileName);

    // Upload the file
    await blockBlob.UploadFromStreamAsync(fileStream);

    return await Task.FromResult(true);
}
```

De volgende klassen en methoden worden gebruikt in de vorige bewerking:

|Klasse  |Methode  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Controleer of dat de afbeelding wordt weergegeven in het opslagaccount

Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer in het menu links **opslagaccounts**, selecteert u de naam van uw opslagaccount. Onder **overzicht**, selecteer de **installatiekopieën** container.

Controleer of dat de afbeelding wordt weergegeven in de container.

![De weergave van afbeeldingen container](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Test miniatuur weer te geven

Als u wilt testen miniatuur weer te geven, hebt u een installatiekopie van een uploaden naar de miniatuur container om ervoor te zorgen dat de toepassing de miniatuur container kan lezen.

Meld u aan bij [Azure Portal](https://portal.azure.com). Selecteer in het menu links **opslagaccounts**, selecteert u de naam van uw opslagaccount. Selecteer **Containers** onder **Blob-Service** en selecteer de **duim** container. Selecteer **uploaden** openen de **blob uploaden** deelvenster.

Kies een bestand met de bestandskiezer en selecteer **uploaden**.

Ga terug naar uw app om te controleren die de installatiekopie van het geüpload naar de **duim** container zichtbaar is.

![De weergave van afbeeldingen container](media/storage-upload-process-images/figure2.png)

In de **duim** container in Azure portal, selecteert u de installatiekopie die u geüpload en selecteer **verwijderen** verwijderen van de installatiekopie. U wilt het maken van de miniatuurafbeeldingen in deel twee van de reeks automatiseren zodat deze testinstallatiekopie is niet nodig.

CDN worden ingeschakeld voor het cache-inhoud van uw Azure storage-account. Terwijl niet in deze zelfstudie wordt beschreven, voor meer informatie over het inschakelen van CDN met uw Azure storage-account, kunt u bezoeken: [integreren van een Azure storage-account met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Volgende stappen

In deel 1 van de reeks, hebt u geleerd over het configureren van een web-app interactie met opslag, zoals het:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Een container maken en machtigingen instellen
> * Een toegangssleutel ophalen
> * Configureer toepassingsinstellingen
> * Een Web-App implementeren in Azure
> * Interactie met de webtoepassing

Ga naar deel twee van de reeks voor meer informatie over het gebruik van gebeurtenis raster voor het activeren van een Azure-functie voor het formaat van een afbeelding.

> [!div class="nextstepaction"]
> [Gebeurtenis raster voor het activeren van een Azure-functie om de grootte van de installatiekopie van een geüploade te gebruiken](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
