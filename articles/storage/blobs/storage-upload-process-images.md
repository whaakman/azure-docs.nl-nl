---
title: Afbeeldingsgegevens uploaden in de cloud met Azure Storage | Microsoft Docs
description: Azure Blob Storage gebruiken me een web-app om toepassingsgegevens op te slaan
services: storage
documentationcenter: 
author: tamram
manager: jeconnoc
ms.service: storage
ms.workload: web
ms.devlang: csharp
ms.topic: tutorial
ms.date: 02/20/2018
ms.author: tamram
ms.custom: mvc
ms.openlocfilehash: e3c40d0f3db1a33a405a341a714a7ce199908ca4
ms.sourcegitcommit: d1f35f71e6b1cbeee79b06bfc3a7d0914ac57275
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/22/2018
---
# <a name="upload-image-data-in-the-cloud-with-azure-storage"></a>Afbeeldingsgegevens uploaden in de cloud met Azure Storage

Deze zelfstudie is deel één van een serie. In deze zelfstudie leert u hoe u een webtoepassing implementeert die gebruikmaakt van de Azure Storage-clientbibliotheek voor het uploaden van afbeeldingen in een opslagaccount. Als u klaar bent, hebt u een web-app die afbeeldingen vanuit Azure Storage kan opslaan en weergeven.

![Containerweergave van afbeeldingen](media/storage-upload-process-images/figure2.png)

In deel 1 van de reeks leert u het volgende:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Een container maken en machtigingen instellen
> * Een toegangssleutel ophalen
> * Toepassingsinstellingen configureren
> * Een web-app implementeren in Azure
> * Interactie aangaan met de webtoepassing

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie Azure CLI 2.0.4 of nieuwer uitvoeren. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

## <a name="create-a-resource-group"></a>Een resourcegroep maken 

Een resourcegroep maken met de opdracht [az group create](/cli/azure/group#az_group_create). Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd.
 
In het volgende voorbeeld wordt een resourcegroep met de naam `myResourceGroup` gemaakt.
 
```azurecli-interactive 
az group create --name myResourceGroup --location westcentralus 
``` 

## <a name="create-a-storage-account"></a>Een opslagaccount maken
 
Met het voorbeeld worden afbeeldingen geüpload naar een blobcontainer in een Azure Storage-account. Een opslagaccount biedt een unieke naamruimte voor het opslaan en openen van uw Azure Storage-gegevensobjecten. Maak een opslagaccount in de resourcegroep die u hebt gemaakt met behulp van de opdracht [az storage account create](/cli/azure/storage/account#az_storage_account_create). 

> [!IMPORTANT] 
> In deel 2 van de zelfstudie gebruikt u gebeurtenisabonnementen voor blob-opslag. Gebeurtenisabonnementen worden momenteel alleen ondersteund voor Blob Storage-accounts in West-centraal VS en VS - west 2. Vanwege deze beperking dient u een Blob Storage-account te maken dat door de voorbeeld-app wordt gebruikt voor het opslaan van afbeeldingen en miniaturen.   

Vervang in de volgende opdracht het Blob Storage-account in de tijdelijke aanduiding `<blob_storage_account>` door uw eigen unieke naam.  

```azurecli-interactive 
az storage account create --name <blob_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind blobstorage --access-tier hot 
``` 
 
## <a name="create-blob-storage-containers"></a>Blob Storage-containers maken
 
De app gebruikt twee containers in het Blob Storage-account. Containers zijn vergelijkbaar met mappen en worden gebruikt voor het opslaan van blobs. In de container _images_ worden afbeeldingen in volledige resolutie opgeslagen. In een later deel van de serie leert u hoe een Azure-functie-app verkleinde afbeeldingsminiaturen naar de container _thumbnails_ uploadt. 

Haal de opslagaccountsleutel op met behulp van de opdracht [az storage account keys list](/cli/azure/storage/account/keys#az_storage_account_keys_list). Gebruik vervolgens deze sleutel om twee containers te maken met behulp van de opdracht [az storage container create](/cli/azure/storage/container#az_storage_container_create).  
 
In dit geval is `<blob_storage_account>` de naam van het Blob Storage-account dat u hebt gemaakt. De openbare toegang tot de _images_-containers wordt ingesteld op `off`, die tot de _thumbnails_-containers op `container`. Dankzij de instelling voor de openbare toegang tot de `container` zijn de miniaturen zichtbaar voor personen die de webpagina bezoeken.
 
```azurecli-interactive 
blobStorageAccount=<blob_storage_account>

blobStorageAccountKey=$(az storage account keys list -g myResourceGroup \
-n $blobStorageAccount --query [0].value --output tsv) 

az storage container create -n images --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access off 

az storage container create -n thumbnails --account-name $blobStorageAccount \
--account-key $blobStorageAccountKey --public-access container

echo "Make a note of your blob storage account key..." 
echo $blobStorageAccountKey 
``` 

Noteer de naam van het Blob Storage-account en de -sleutel. De voorbeeld-app maakt gebruik van deze instellingen om verbinding te maken met het opslagaccount om afbeeldingen te kunnen uploaden. 

## <a name="create-an-app-service-plan"></a>Een App Service-plan maken 

Een [App Service-plan](../../app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md) geeft de locatie, de grootte en de functies van de webserverfarm aan die als host fungeert voor uw app. 

Maak een App Service-plan met de opdracht [az appservice plan create](/cli/azure/appservice/plan#az_appservice_plan_create). 

In het volgende voorbeeld wordt een App Service-plan gemaakt met de naam `myAppServicePlan` en de prijscategorie **Gratis**: 

```azurecli-interactive 
az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE 
``` 

## <a name="create-a-web-app"></a>Een webtoepassing maken 

De web-app biedt een hostingruimte voor de code van de voorbeeld-app. De ruimte wordt geïmplementeerd vanuit de GitHub-voorbeeld opslagplaats. Maak een [web-app](../../app-service/app-service-web-overview.md) in het App Service-plan `myAppServicePlan` met de opdracht [az webapp create](/cli/azure/webapp#az_webapp_create).  
 
Vervang in de volgende opdracht `<web_app>` door een unieke naam (geldige tekens zijn `a-z`, `0-9` en `-`). Als `<web_app>` niet uniek is, wordt er een foutbericht weergegeven: _Er bestaat al een website met de naam `<web_app>`._ De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`.  

```azurecli-interactive 
az webapp create --name <web_app> --resource-group myResourceGroup --plan myAppServicePlan 
``` 

## <a name="deploy-the-sample-app-from-the-github-repository"></a>Voorbeeld-app implementeren vanuit de GitHub-opslagplaats 

App Service ondersteunt diverse manieren om inhoud in een web-app te implementeren. In deze zelfstudie implementeert u de web-app vanaf een [openbare GitHub-voorbeeldopslagplaats](https://github.com/Azure-Samples/storage-blob-upload-from-webapp) (Engelstalig). Configureer GitHub-implementatie naar de webtoepassing met de opdracht [az webapp deployment source config](/cli/azure/webapp/deployment/source#az_webapp_deployment_source_config). Vervang `<web_app>` door de naam van de web-app die u in de vorige stap hebt gemaakt.

Het voorbeeldproject bevat een [ASP.NET MVC](https://www.asp.net/mvc)-app die een afbeelding accepteert, deze opslaat in een opslagaccount en afbeeldingen vanuit een miniaturencontainer weergeeft. De web-app gebruikt de naamruimten [Microsoft.WindowsAzure.Storage](/dotnet/api/microsoft.windowsazure.storage?view=azure-dotnet), [Microsoft.WindowsAzure.Storage.Blob](/dotnet/api/microsoft.windowsazure.storage.blob?view=azure-dotnet) en [Microsoft.WindowsAzure.Storage.Auth](/dotnet/api/microsoft.windowsazure.storage.auth?view=azure-dotnet) van de Azure Storage-clientbibliotheek voor interactie met Azure Storage. 

```azurecli-interactive 
az webapp deployment source config --name <web_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/storage-blob-upload-from-webapp
``` 

## <a name="configure-web-app-settings"></a>Web-app-instellingen configureren 

De voorbeeldweb-app gebruikt de [Azure Storage-clientbibliotheek](/dotnet/api/overview/azure/storage?view=azure-dotnet) om toegangstokens aan te vragen. Hiermee worden afbeeldingen geüpload. De referenties van het opslagaccount die worden gebruikt door de Storage-SDK, worden ingesteld in de toepassingsinstellingen voor de web-app. Voeg toepassingsinstellingen toe aan de geïmplementeerde app met de opdracht [az webapp config appsettings set](/cli/azure/webapp/config/appsettings#az_webapp_config_appsettings_set). 

In de volgende opdracht is `<blob_storage_account>` de naam van uw Blob Storage-account en `<blob_storage_key>` de bijbehorende sleutel. Vervang `<web_app>` door de naam van de web-app die u in de vorige stap hebt gemaakt.     

```azurecli-interactive 
az webapp config appsettings set --name <web_app> --resource-group myResourceGroup \
--settings AzureStorageConfig__AccountName=<blob_storage_account> \
AzureStorageConfig__ImageContainer=images  \
AzureStorageConfig__ThumbnailContainer=thumbnails \
AzureStorageConfig__AccountKey=<blob_storage_key>  
``` 

Als de web-app is geïmplementeerd en geconfigureerd, kunt u de functionaliteit voor het uploaden van afbeeldingen in de app testen.   

## <a name="upload-an-image"></a>Een installatiekopie uploaden 

U kunt de web-app testen door naar de URL van de gepubliceerde app te gaan. De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`. Selecteer het gebied **Upload photos** om een bestand te selecteren en uploaden of sleep een bestand naar het gebied. Als de afbeelding is geüpload, verdwijnt deze.

![App ImageResizer](media/storage-upload-process-images/figure1.png)

In de voorbeeldcode wordt de taak `UploadFiletoStorage` in bestand `Storagehelper.cs` gebruikt om de afbeeldingen te uploaden naar de `images`-container in het opslagaccount door middel van de methode [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet). Het volgende codevoorbeeld bevat de taak `UploadFiletoStorage`. 

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

In de vorige taak zijn de volgende klassen en methoden gebruikt:

|Klasse  |Methode  |
|---------|---------|
|[StorageCredentials](/dotnet/api/microsoft.windowsazure.storage.auth.storagecredentials?view=azure-dotnet)     |         |
|[CloudStorageAccount](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount?view=azure-dotnet)    |  [CreateCloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.cloudstorageaccount.createcloudblobclient?view=azure-dotnet#Microsoft_WindowsAzure_Storage_CloudStorageAccount_CreateCloudBlobClient)       |
|[CloudBlobClient](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient?view=azure-dotnet)     |[GetContainerReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobclient.getcontainerreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobClient_GetContainerReference_System_String_)         |
|[CloudBlobContainer](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer?view=azure-dotnet)    | [GetBlockBlobReference](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblobcontainer.getblockblobreference?view=azure-dotnet#Microsoft_WindowsAzure_Storage_Blob_CloudBlobContainer_GetBlockBlobReference_System_String_)        |
|[CloudBlockBlob](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob?view=azure-dotnet)     | [UploadFromStreamAsync](/dotnet/api/microsoft.windowsazure.storage.blob.cloudblockblob.uploadfromstreamasync?view=azure-dotnet)        |

## <a name="verify-the-image-is-shown-in-the-storage-account"></a>Controleren of de afbeelding in het opslagaccount wordt weergegeven

Meld u aan bij [Azure Portal](https://portal.azure.com). In het linkermenu selecteert u **opslagaccounts** en vervolgens de naam van uw opslagaccount. Selecteer onder **Overzicht** de **images**-container.

Controleer of de afbeelding in de container wordt weergegeven.

![Containerweergave van afbeeldingen](media/storage-upload-process-images/figure13.png)

## <a name="test-thumbnail-viewing"></a>Weergave van miniaturen testen

Als u de weergave van miniaturen wilt testen, uploadt u een afbeelding naar de miniaturencontainer, zodat u er zeker van bent dat de toepassing de miniaturencontainer kan lezen.

Meld u aan bij [Azure Portal](https://portal.azure.com). In het linkermenu selecteert u **opslagaccounts** en vervolgens de naam van uw opslagaccount. Selecteer **Containers** onder **Blob-service** en selecteer de **thumbnails**-container. Selecteer **Uploaden** om het deelvenster **Blob uploaden** te openen.

Kies een bestand met de bestandenkiezer en selecteer **Uploaden**.

Ga terug naar de app om te controleren of de naar de **thumbnails**-container geüploade afbeelding zichtbaar is.

![Containerweergave van afbeeldingen](media/storage-upload-process-images/figure2.png)

In de **thumbnails**-container in Azure Portal selecteert u de geüploade afbeelding en vervolgens **Verwijderen** om de afbeelding te verwijderen. In deel 2 van de reeks automatiseert u het maken van miniatuurafbeeldingen, zodat u deze testafbeelding niet meer nodig hebt.

CDN kan worden ingeschakeld om inhoud vanuit uw Azure Storage-account in de cache op te slaan. In deze zelfstudie wordt niet beschreven hoe u CDN inschakelt voor uw Azure-opslagaccount. Ga hiervoor naar [Een Azure Storage-account integreren met Azure CDN](../../cdn/cdn-create-a-storage-account-with-cdn.md).

## <a name="next-steps"></a>Volgende stappen

In deel 1 van de reeks hebt u geleerd hoe u een web-app configureert die interactie aangaat met opslag, bijvoorbeeld:

> [!div class="checklist"]
> * Een opslagaccount maken
> * Een container maken en machtigingen instellen
> * Een toegangssleutel ophalen
> * Toepassingsinstellingen configureren
> * Een web-app implementeren in Azure
> * Interactie aangaan met de webtoepassing

Ga door naar deel 2 van de reeks om te leren hoe u Event Grid gebruikt om een Azure-functie te activeren als u het formaat van een afbeelding wilt aanpassen.

> [!div class="nextstepaction"]
> [Event Grid gebruiken om een Azure-functie te activeren bij het aanpassen van geüploade afbeeldingen](../../event-grid/resize-images-on-storage-blob-upload-event.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)
