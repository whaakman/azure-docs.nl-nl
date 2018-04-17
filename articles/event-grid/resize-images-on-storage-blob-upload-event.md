---
title: Formaat van geüploade afbeeldingen automatisch wijzigen met Azure Event Grid | Microsoft Docs
description: U kunt instellen dat Azure Event Grid wordt geactiveerd als er blobs worden geüpload in Azure Storage. Dit is handig om afbeeldingsbestanden naar Azure Storage die worden geüpload naar Azure Storage te verzenden naar andere services, zoals Azure Functions, voor het aanpassen van het formaat en andere verbeteringen.
services: event-grid, functions
author: ggailey777
manager: cfowler
editor: ''
ms.service: event-grid
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 10/20/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: 0edf5648ddef58db74273635c84d7473e17e1b30
ms.sourcegitcommit: 5b2ac9e6d8539c11ab0891b686b8afa12441a8f3
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/06/2018
---
# <a name="automate-resizing-uploaded-images-using-event-grid"></a>Formaat van geüploade afbeeldingen automatisch wijzigen met Event Grid

[Azure Event Grid](overview.md) is een gebeurtenisservice voor de cloud. Met Event Grid kunt u abonnementen nemen op gebeurtenissen die worden gegenereerd door Azure-services of resources van derden.  

Deze zelfstudie is deel twee in een reeks zelfstudies over Azure Storage en is een vervolg op de [vorige zelfstudie over Storage][previous-tutorial] met als doel om zonder tussenkomst van een server automatisch miniaturen te genereren met behulp van Azure Event Grid en Azure Functions. Event Grid zorgt ervoor dat [Azure Functions](..\azure-functions\functions-overview.md) kan reageren op gebeurtenissen van [Azure Blob-opslag](..\storage\blobs\storage-blobs-introduction.md) om miniaturen van geüploade afbeeldingen te genereren. Een gebeurtenisabonnement wordt gekoppeld aan gebeurtenis waarmee Blob-opslag wordt gemaakt. Wanneer er een blob wordt toegevoegd aan een specifieke Blob-opslagcontainer, wordt er een functie-eindpunt aangeroepen. Aan de hand van gegevens die vanuit Event Grid worden doorgegeven aan de functiebinding, wordt er toegang verkregen tot de blob en wordt de miniatuurafbeelding gegenereerd. 

Met behulp van de Azure CLI en Azure Portal kunt u de functionaliteit voor formaatwijziging toevoegen aan een bestaande app voor het uploaden van afbeeldingen.

![Gepubliceerde web-app in de Edge-browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png)

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een algemene Azure Storage-account maken
> * Serverloze code implementeren met behulp van Azure Functions
> * Een gebeurtenisabonnement voor Blob-opslag maken in Event Grid

## <a name="prerequisites"></a>Vereisten

Vereisten voor het voltooien van deze zelfstudie:

+ U moet de vorige zelfstudie over Blob-opslag hebben voltooid: [Afbeeldingsgegevens uploaden in de cloud met Azure Storage][previous-tutorial]. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u voor deze zelfstudie gebruikmaken van Azure CLI versie 2.0.14 of hoger. Voer `az --version` uit om de versie te bekijken. Als u Azure CLI 2.0 wilt installeren of upgraden, raadpleegt u [Azure CLI 2.0 installeren]( /cli/azure/install-azure-cli). 

Als u Cloud Shell niet gebruikt, moet u eerst zich aanmelden met `az login`.

## <a name="create-an-azure-storage-account"></a>Een Azure Storage-account maken

Voor Azure Functions is een algemeen opslagaccount vereist. Maak een afzonderlijk algemeen opslagaccount in de resourcegroep met behulp van de opdracht [az storage account create](/cli/azure/storage/account#az_storage_account_create).

Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. 

Vervang in de volgende opdracht de tijdelijke aanduiding `<general_storage_account>` door uw eigen unieke naam voor het algemene opslagaccount. 

```azurecli-interactive
az storage account create --name <general_storage_account> \
--location westcentralus --resource-group myResourceGroup \
--sku Standard_LRS --kind storage
```

## <a name="create-a-function-app"></a>Een functie-app maken  

U moet een functie-app hebben die als host fungeert voor de uitvoering van uw functie. De functie-app biedt een omgeving waarin uw functiecode zonder server kan worden uitgevoerd. Een functie-app maken met behulp van de opdracht [az functionapp create](/cli/azure/functionapp#az_functionapp_create). 

Vervang in de volgende opdracht de tijdelijke aanduiding `<function_app>` door de unieke naam van uw eigen functie-app. De naam van de functie-app wordt gebruikt als het standaard DNS-domein voor de functie-app. Om die reden moet de naam uniek zijn in alle apps in Azure. Verander voor `<general_storage_account>` de naam van het algemene opslagaccount dat u hebt gemaakt.

```azurecli-interactive
az functionapp create --name <function_app> --storage-account  <general_storage_account>  \
--resource-group myResourceGroup --consumption-plan-location westcentralus
```

Nu moet u de functie-app configureren om verbinding te maken met het Blob-opslagaccount dat u in de [vorige zelfstudie][previous-tutorial] hebt gemaakt.

## <a name="configure-the-function-app"></a>De functie-app configureren

De functie heeft de verbindingsreeks nodig om verbinding te maken met het Blob-opslagaccount. De functiecode die u in de volgende stap in Azure implementeert, zoekt naar de verbindingsreeks in de app-instelling myblobstorage_STORAGE en naar de containernaam van de miniatuurweergave in de app-instelling myContainerName. Haal de verbindingsreeks op met de opdracht [az storage account show-connection-string](/cli/azure/storage/account#show-connection-string). Stel toepassingsinstellingen in met de opdracht [az functionapp config appsettings set](/cli/azure/functionapp/config/appsettings#set).

In de volgende CLI-opdrachten is `<blob_storage_account>` de naam van het Blob-opslagaccount dat u hebt gemaakt in de vorige zelfstudie.

```azurecli-interactive
storageConnectionString=$(az storage account show-connection-string \
--resource-group myResourceGroup --name <blob_storage_account> \
--query connectionString --output tsv)

az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings myblobstorage_STORAGE=$storageConnectionString \
myContainerName=thumbnails
```

U kunt nu een codeproject van Functions implementeren naar deze functie-app.

## <a name="deploy-the-function-code"></a>De functiecode implementeren 

De C#-functie waarmee het formaat van afbeeldingen wordt aangepast, is beschikbaar in deze [GitHub-opslagplaats](https://github.com/Azure-Samples/function-image-upload-resize). Implementeer dit codeproject van Functions naar de functie-app met behulp van de opdracht [az functionapp deployment source config](/cli/azure/functionapp/deployment/source#config). 

In de volgende opdracht is `<function_app>` de naam van de functie-app die u eerder hebt gemaakt.

```azurecli-interactive
az functionapp deployment source config --name <function_app> \
--resource-group myResourceGroup --branch master --manual-integration \
--repo-url https://github.com/Azure-Samples/function-image-upload-resize
```

De functie om afbeeldingen in grootte aan te passen wordt geactiveerd door HTTP-aanvragen die vanaf de Event Grid-service worden verzonden. U kunt Event Grid laten weten dat u deze meldingen bij de URL van uw functie wilt ontvangen door een gebeurtenisabonnement te maken. Voor deze zelfstudie abonneert u zich op gebeurtenissen die door een blob zijn gemaakt.

De gegevens die aan de functie worden doorgegeven in de Event Grid-melding bevatten de URL van de blob. Die URL is op zijn beurt doorgegeven aan de invoerbinding om de geüploade installatiekopie uit de Blob-opslag op te halen. De functie genereert een miniatuurafbeelding en de resulterende stroom wordt weggeschreven naar een afzonderlijke container in de Blob-opslag. 

Dit project gebruikt `EventGridTrigger` als het type trigger. Het wordt aangeraden om de trigger van Event Grid te gebruiken in plaats van algemene HTTP-triggers. Functie-triggers van Event Grid worden namelijk automatisch gevalideerd. Bij gebruik van algemene HTTP-triggers moet u een [validatie-antwoord](security-authentication.md#webhook-event-delivery) implementeren.

Als u meer wilt leren over deze functie, bekijk dan de [bestanden function.json en run.csx](https://github.com/Azure-Samples/function-image-upload-resize/tree/master/imageresizerfunc).
 
Het codeproject van Functions wordt rechtstreeks vanuit de openbare opslagplaats met voorbeelden geïmplementeerd. Zie [Doorlopende implementatie voor Azure Functions](../azure-functions/functions-continuous-deployment.md) voor meer informatie over implementatieopties voor Azure Functions.

## <a name="create-an-event-subscription"></a>Een gebeurtenisabonnement maken

Een gebeurtenisabonnement geeft aan welke door de provider gegenereerde gebeurtenissen u naar een bepaald eindpunt wilt versturen. In dit geval wordt het eindpunt bepaald door de functie. Gebruik de volgende stappen om in Azure Portal een gebeurtenisabonnement te maken die meldingen aan uw functie verzendt: 

1. Klik in [Azure Portal](https://portal.azure.com) op de pijl linksonder om alle services uit te vouwen, typ *functies* in het veld **Filter** en kies vervolgens **Functie-apps**. 

    ![Naar Functie-apps bladeren in Azure Portal](./media/resize-images-on-storage-blob-upload-event/portal-find-functions.png)

2. Vouw de functie-app uit, kies de functie **imageresizerfunc** en selecteer vervolgens **Event Grid-abonnement toevoegen**.

    ![Naar Functie-apps bladeren in Azure Portal](./media/resize-images-on-storage-blob-upload-event/add-event-subscription.png)

3. Gebruik de instellingen voor het gebeurtenisabonnement zoals die zijn opgegeven in de tabel onder de afbeelding.
    
    ![Een gebeurtenisabonnement maken voor de functie in Azure Portal](./media/resize-images-on-storage-blob-upload-event/event-subscription-create-flow.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam** | imageresizersub | De naam voor het nieuwe gebeurtenisabonnement. | 
    | **Onderwerptype** |  Opslagaccounts | Kies de provider voor de gebeurtenissen van het opslagaccount. | 
    | **Abonnement** | Uw Azure-abonnement | Uw huidige Azure-abonnement is standaard geselecteerd.   |
    | **Resourcegroep** | myResourceGroup | Selecteer **Bestaande gebruiken** en kies de resourcegroep die u in deze zelfstudie hebt gebruikt.  |
    | **Exemplaar** |  Uw Blob-opslagaccount |  Kies het Blob-opslagaccount dat u hebt gemaakt. |
    | **Gebeurtenistypen** | BlobCreated | Schakel alle typen uit behalve **BlobCreated**. Alleen gebeurtenistypen van `Microsoft.Storage.BlobCreated` worden doorgegeven aan de functie.| 
    | **Abonneetype** |  Webhook |  De keuzes zijn Webhook of Event Hubs. |
    | **Eindpunt abonnee** | automatisch gegenereerd | Gebruik de eindpunt-URL die voor u wordt gegenereerd. | 
    | **Voorvoegselfilter** | /blobServices/default/containers/images/blobs/ | Opslaggebeurtenissen filteren op alleen de gebeurtenissen in de container **images**.| 

4. Klik op **Maken** om het gebeurtenisabonnement toe te voegen. Er wordt een gebeurtenisabonnement gemaakt die `imageresizerfunc` activeert op het moment dat er een blob wordt toegevoegd aan de container *images*. De functie past de afbeelding in grootte aan en voegt deze toe aan de container *thumbnails*.

De services in de back-end zijn nu geconfigureerd. Dit betekent dat u de functionaliteit voor het aanpassen van het formaat van afbeeldingen kunt gaan testen in de voorbeeld-web-app. 

## <a name="test-the-sample-app"></a>De voorbeeld-app testen

U kunt de formaatwijziging door de web-app testen door naar de URL van de gepubliceerde app te gaan. De standaard-URL van de web-app is `https://<web_app>.azurewebsites.net`.

Klik ergens in het gebied **Foto's uploaden** om een bestand te selecteren en te uploaden. U kunt ook een foto naar dit gebied slepen. 

Zodra de geüploade afbeelding is verdwenen, ziet u een kopie van de geüploade afbeelding in de carrousel **Gegenereerde miniaturen**. Het formaat van deze afbeelding werd gewijzigd door de functie, waarna de afbeelding werd toegevoegd aan de container *thumbnails* en gedownload door de webclient.

![Gepubliceerde web-app in de Edge-browser](./media/resize-images-on-storage-blob-upload-event/tutorial-completed.png) 

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een algemene Azure Storage-account maken
> * Serverloze code implementeren met behulp van Azure Functions
> * Een gebeurtenisabonnement voor Blob-opslag maken in Event Grid

Ga naar deel drie van de reeks met zelfstudies over Azure Storage om te leren hoe u de toegang tot het opslagaccount beveiligt.

> [!div class="nextstepaction"]
> [Toegang tot gegevens van een toepassingen in de cloud beveiligen](../storage/blobs/storage-secure-access-application.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json)

+ Zie [Een inleiding tot Azure Event Grid](overview.md) voor meer informatie over Event Grid. 
+ Zie [Een functie maken die kan worden geïntegreerd met Azure Logic Apps](..\azure-functions\functions-twitter-email.md) als u nog een zelfstudie wilt volgen waarin Azure Functions wordt gebruikt. 

[previous-tutorial]: ../storage/blobs/storage-upload-process-images.md
