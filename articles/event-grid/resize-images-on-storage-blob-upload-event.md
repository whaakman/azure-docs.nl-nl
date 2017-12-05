---
title: "Gebruik Azure gebeurtenis raster te automatiseren vergroten of verkleinen geüpload installatiekopieën | Microsoft Docs"
description: "Azure Event raster kunt activeren voor uploaden van blob in Azure Storage. U kunt deze gebruiken voor het verzenden van afbeeldingsbestanden naar Azure Storage is geüpload naar andere verbeteringen en andere services, zoals Azure Functions voor het vergroten of verkleinen."
services: event-grid
author: ggailey777
manager: cfowler
editor: 
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 22eafca56eb5677c63a833d298799b725c50f768
ms.sourcegitcommit: 7136d06474dd20bb8ef6a821c8d7e31edf3a2820
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Automatiseren geüploade afbeeldingen met gebeurtenis raster vergroten of verkleinen

[Azure Event raster](overview.md) is een gebeurtenisservice voor de cloud. Gebeurtenis raster kunt u abonnementen op gebeurtenissen die worden gegenereerd door de Azure-services of derden resources maken.  

Deze zelfstudie maakt deel uit van een reeks zelfstudies voor opslag. Hiermee wordt uitgebreid de [vorige opslag zelfstudie] [ previous-tutorial] om toe te voegen zonder server automatisch miniaturen genereren met behulp van Azure Event raster en Azure Functions. Gebeurtenis raster kunt [Azure Functions](..\azure-functions\functions-overview.md) om te reageren op [Azure Blob storage](..\storage\blobs\storage-blobs-introduction.md) gebeurtenissen en genereren van miniaturen van geüploade afbeeldingen. Een gebeurtenisabonnement wordt gemaakt op basis van de Blob storage gebeurtenis maken. Wanneer een blob is toegevoegd aan een specifieke Blob storage-container, wordt een functie-eindpunt aangeroepen. Gegevens die zijn doorgegeven aan de functiebinding van de gebeurtenis raster wordt gebruikt voor toegang tot de blob en het genereren van de miniatuur. 

Met de Azure CLI en de Azure portal kunt u het formaat functionaliteit toevoegen aan een bestaande installatiekopie uploaden app.

![Gepubliceerde web-app in Edge-browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een algemeen Azure Storage-account maken
> * Als u kiest met behulp van Azure Functions-code implementeren
> * Maken van een Blob-opslag voor een gebeurtenisabonnement in gebeurtenis raster

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

+ U moet voltooid van de vorige Blob storage-zelfstudie: [image-gegevens in de cloud met Azure Storage uploadt][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u wilt installeren en gebruiken van de CLI lokaal, in dit onderwerp is vereist dat u de Azure CLI versie 2.0.14 worden uitgevoerd of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u Cloud Shell niet gebruikt, moet u eerst zich aanmelden met `az login`.

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Azure Functions is een algemeen opslagaccount vereist. Een afzonderlijke algemeen opslagaccount in de resourcegroep maken met behulp van de [az storage-account maken](/cli/azure/storage/account#create) opdracht.

Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. 

In de volgende opdracht te vervangen door uw eigen globaal unieke naam voor de algemeen opslagaccount waarin u zien hoe de `<general_storage_account>` tijdelijke aanduiding. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Een functie-app maken  

U moet een functie-app voor het hosten van de uitvoering van de functie hebben. De functie-app biedt een omgeving waarin uw functiecode zonder server kan worden uitgevoerd. Een functie-app maken met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#create). 

Vervang de naam van uw eigen unieke functie-app waarin u ziet in de volgende opdracht de `<function_app>` tijdelijke aanduiding. De `<function_app>` wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. In dit geval `<general_storage_account>` is de naam van het algemeen opslagaccount dat u hebt gemaakt.  

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Nu moet u de functie-app verbinding maken met blobopslag configureren. 

## <a name="configure-the-function-app"></a>De functie-app configureren

De functie moet de verbindingsreeks verbinding maken met de blob storage-account. In dit geval `<blob_storage_account>` is de naam van de Blob storage-account die u in de vorige zelfstudie hebt gemaakt. Ophalen van de verbindingsreeks met het [az storage-account weergeven verbindingsreeks](/cli/azure/storage/account#show-connection-string) opdracht. De containernaam miniatuurafbeelding moet ook worden ingesteld op `thumbs`. Deze toepassingsinstellingen toevoegen in de functie-app met de [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set) opdracht.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbs
```

U kunt nu een functie CodeProject om deze functie-app te implementeren.

## <a name="deploy-the-function-code"></a>De functiecode implementeren 

De C#-functie waarmee de grootte van afbeeldingen aanpassen is beschikbaar in deze [voorbeeld GitHub-opslagplaats](https://github.com/Azure-Samples/function-image-upload-resize). Deze functies CodeProject naar de functie-app implementeren met behulp van de [az functionapp implementatieconfiguratie bron](/cli/azure/functionapp/deployment/source#config) opdracht. 

In de volgende opdracht `<function_app>` wordt dezelfde functie-app die u hebt gemaakt in het vorige script.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

De functie van de installatiekopie van formaat wordt geactiveerd door een gebeurtenisabonnement op een gebeurtenis Blob gemaakt. De gegevens die zijn doorgegeven aan de trigger bevat de URL van de blob die op zijn beurt wordt doorgegeven naar de invoer binding voor de installatiekopie van het geüploade van Blob-opslag. De functie genereert een miniatuur en de resulterende stroom schrijft naar een afzonderlijke container in Blob-opslag. Zie voor meer informatie over deze functie, de [Leesmij-bestand in de opslagplaats voorbeeld](https://github.com/Azure-Samples/function-image-upload-resize/blob/master/README.md).
 
De functie projectcode wordt rechtstreeks vanuit de opslagplaats openbaar voorbeeld geïmplementeerd. Zie voor meer informatie over de implementatieopties voor Azure Functions, [continue implementatie voor Azure Functions](../azure-functions/functions-continuous-deployment.md).

## <a name="create-your-event-subscription"></a>Uw gebeurtenisabonnement maken

Een gebeurtenisabonnement geeft aan welke gebeurtenissen provider worden gegenereerd op de gewenste verzonden naar een specifieke eindpunt. In dit geval wordt wordt het eindpunt blootgelegd door de functie. Gebruik de volgende stappen uit voor een gebeurtenisabonnement van de functie in de Azure-portal: 

1. In de [Azure-portal](https://portal.azure.com), klikt u op de pijl naar linksonder om uit te breiden alle services, typt u `functions` in de **Filter** veld en kies vervolgens **functie Apps**. 

    ![Blader naar de functie Apps in Azure portal](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Vouw de functie-app, kiest u de **imageresizerfunc** functioneren en selecteer vervolgens **toevoegen gebeurtenis raster abonnement**.

    ![Blader naar de functie Apps in Azure portal](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Gebruik de gebeurtenis abonnementsinstellingen zoals opgegeven in de tabel.

    ![Voor een gebeurtenisabonnement van de functie in de Azure portal maken](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam** | imageresizersub | De naam die uw abonnement op nieuwe gebeurtenissen identificeert. | 
    | **Onderwerptype** |  Opslagaccounts | Kies de opslagprovider voor de account-gebeurtenis. | 
    | **Abonnement** | Uw abonnement | Standaard moet uw huidige abonnement worden geselecteerd.   |
    | **Resourcegroep** | myResourceGroup | Selecteer **gebruik bestaande** en kiest u de resourcegroep die u hebt gebruikt in dit onderwerp.  |
    | **Exemplaar** |  `<blob_storage_account>` |  Kies de Blob storage-account die u hebt gemaakt. |
    | **Typen gebeurtenissen** | BLOB gemaakt | Schakel alle typen dan **Blob gemaakt**. Gebeurtenistypen van `Microsoft.Storage.BlobCreated` zijn doorgegeven aan de functie.| 
    | **Abonnee eindpunt** | automatisch gegenereerde | Gebruik de eindpunt-URL die wordt gegenereerd voor u. | 
    | **Voorvoegsel filter** | / blobServices/standaard/containers/installatiekopieën/blobs / | Gebeurtenissen van de opslag tot alleen degenen gefilterd op de **installatiekopieën** container.| 

4. Klik op **maken** het gebeurtenisabonnement wilt toevoegen. Hiermee maakt u een abonnement op gebeurtenissen die activeert **imageresizerfunc** wanneer een blob wordt toegevoegd aan de **installatiekopieën** container. Installatiekopieën van het formaat is gewijzigd, worden toegevoegd aan de **duim** container.

Nu dat de back-end-services zijn geconfigureerd, kunt u de installatiekopie formaat functionaliteit testen in de voorbeeld-web-app. 

## <a name="test-the-sample-app"></a>De voorbeeld-app testen

Als u wilt testen in de web-app grootte van afbeeldingen, blader naar de URL van uw gepubliceerde app. De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`.

Klik op de **foto's uploaden** regio selecteren en een bestand te uploaden. U kunt ook een foto slepen naar deze regio. 

Nadat de installatiekopie van het geüploade verdwijnt, een kopie van de installatiekopie van het geüploade wordt weergegeven de **gegenereerd miniaturen** carrousel. Deze installatiekopie is gewijzigd door de functie, toegevoegd aan de container duim en door de webclient gedownload. 

![Gepubliceerde web-app in Edge-browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een algemeen Azure Storage-account maken
> * Als u kiest met behulp van Azure Functions-code implementeren
> * Maken van een Blob-opslag voor een gebeurtenisabonnement in gebeurtenis raster

Ga naar deel 2 van de opslag van de zelfstudie reeks voor informatie over het beveiligen van toegang tot het opslagaccount.

> [!div class="nextstepaction"]
> [Veilige toegang tot gegevens in een toepassingen in de cloud](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)


+ Zie voor meer informatie over de gebeurtenis raster, [een inleiding tot Azure gebeurtenis raster](overview.md). 
+ U probeert een ander zelfstudie met functionaliteit voor Azure Functions, Zie [maken van een functie die kan worden geïntegreerd met Azure Logic Apps](..\azure-functions\functions-twitter-email.md). 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md