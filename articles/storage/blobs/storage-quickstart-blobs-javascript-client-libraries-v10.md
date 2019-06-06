---
title: 'Snelstart: een blob maken in Azure Storage met behulp van JavaScript en HTML in de browser'
description: Leer hoe u een exemplaar van BlobService gebruikt voor het uploaden, vermelden en verwijderen van blobs met behulp van JavaScript op een HTML-pagina.
services: storage
keywords: opslag, javascript, html
author: KarlErickson
ms.custom: mvc
ms.service: storage
ms.author: karler
ms.reviewer: seguler
ms.date: 05/20/2019
ms.topic: quickstart
ms.subservice: blobs
ms.openlocfilehash: b16bbeee299f4879c14856a8041e6517968efebf
ms.sourcegitcommit: cababb51721f6ab6b61dda6d18345514f074fb2e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2019
ms.locfileid: "66481381"
---
<!-- Customer intent: As a web application developer I want to interface with Azure Blob storage entirely on the client so that I can build a SPA application that is able to upload and delete files on blob storage. -->

# <a name="quickstart-upload-list-and-delete-blobs-using-azure-storage-v10-sdk-for-javascripthtml-in-the-browser"></a>Quickstart: Uploaden, vermelden en verwijderen van blobs met behulp van Azure Storage v10 SDK voor JavaScript/HTML in de browser

In deze snelstartgids hebt u leert hoe u het gebruik van de [V10 van Azure Storage SDK voor JavaScript - Blob](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/storage/storage-blob#readme) bibliotheek voor het beheren van blobs van JavaScript-code die volledig in de browser wordt uitgevoerd. De hier gebruikte aanpak toont hoe u de vereiste beveiligingsmaatregelen dient te gebruiken om verzekerd te zijn van beveiligde toegang tot uw blobopslagaccount.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

De Azure Storage JavaScript-clientbibliotheken werken niet rechtstreeks vanuit het bestandssysteem en moeten worden geleverd vanaf een webserver. In dit onderwerp wordt gebruikgemaakt van [Node.js](https://nodejs.org) een basic-server te starten. Als u liever niet wilt installeren, knooppunt, kunt u een andere manier van het uitvoeren van een lokale webserver kunt gebruiken.

Als u wilt volgen de stappen op het opsporen van fouten, moet u [Visual Studio Code](https://code.visualstudio.com) en ofwel de [foutopsporingsprogramma voor Chrome](vscode:extension/msjsdiag.debugger-for-chrome) of [foutopsporingsprogramma voor Microsoft Edge](vscode:extension/msjsdiag.debugger-for-edge) extensie.

## <a name="setting-up-storage-account-cors-rules"></a>CORS-regels voor het opslagaccount instellen

Voordat uw webtoepassing toegang krijgen een blob-opslag van de client tot kan, moet u uw account om in te schakelen configureren [cross-origin-resource delen](https://docs.microsoft.com/rest/api/storageservices/cross-origin-resource-sharing--cors--support-for-the-azure-storage-services), of CORS.

Ga terug naar Azure Portal en selecteer uw opslagaccount. Als u een nieuwe CORS-regel definieert, gaat u naar de **instellingen** sectie en klik op de **CORS** koppeling. Klik vervolgens op de knop **Toevoegen** om het venster **CORS-regel toevoegen** te openen. Voor deze snelstart maakt u een open CORS-regel:

![CORS-instellingen in Azure Blob Storage-account](media/storage-quickstart-blobs-javascript-client-libraries/azure-blob-storage-cors-settings.png)

In de volgende tabel worden alle CORS-instellingen beschreven en de waarden voor het definiëren van de regel uitgelegd.

|Instelling  |Waarde  | Description |
|---------|---------|---------|
| Toegestane oorsprongen | * | Accepteert een door komma's gescheiden lijst met domeinen die als acceptabele oorsprongen zijn ingesteld. Als de waarde wordt ingesteld op `*`, hebben alle domeinen toegang tot het opslagaccount. |
| Toegestane bewerkingen     | delete, get, head, merge, post, options en put | Vermeldt de HTTP-bewerkingen die zijn toegestaan om voor het opslagaccount te worden uitgevoerd. Voor deze snelstart selecteert u alle beschikbare opties. |
| Toegestane headers | * | Definieert een lijst met aanvraagheaders (inclusief headers met een voorvoegsel) die door het opslagaccount zijn toegestaan. Als de waarde wordt ingesteld op `*`, hebben alle headers toegang. |
| Weergegeven headers | * | Vermeldt de door het account toegestane antwoordheaders. Als de waarde wordt ingesteld op `*`, kan het account elke header verzenden.  |
| Maximumleeftijd (seconden) | 86400 | De maximale periode dat de browser de voorbereidende aanvraag OPTIONS in de cache opslaat. De waarde *86400* betekent dat de cache een hele dag in stand blijft. |

> [!IMPORTANT]
> Zorg ervoor dat alle instellingen die u in productie beschikbaar maken voor de minimale hoeveelheid toegang tot uw opslagaccount om beveiligde toegang gehandhaafd blijft. De hier beschreven CORS-instellingen zijn geschikt voor een snelstart, omdat ze een minder streng beveiligingsbeleid definiëren. Deze instellingen worden echter niet aangeraden in reële situaties.

Vervolgens gebruikt u Azure Cloud Shell om een beveiligingstoken te maken.

[!INCLUDE [Open the Azure cloud shell](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-shared-access-signature"></a>Een shared access signature maken

Shared Access Signature (handtekening voor gedeelde toegang, SAS) wordt gebruikt door de code die wordt uitgevoerd in de browser om aanvragen bij de blob-opslag te verifiëren. Als u een SAS gebruikt, kan de client toegang tot de opslagresources autoriseren zonder de toegangssleutel of verbindingsreeks van het account. Zie [Handtekeningen voor gedeelde toegang gebruiken](../common/storage-dotnet-shared-access-signature-part-1.md) voor meer informatie over SAS.

U kunt een SAS maken met de Azure-CLI via de Azure cloudshell of met de Azure-portal of Azure Storage Explorer. De volgende tabel beschrijft de parameters die u moet waarden opgeven voor een SAS met de CLI.

| Parameter      |Description  | Tijdelijke aanduiding |
|----------------|-------------|-------------|
| *expiry*       | De vervaldatum van het toegangstoken in de notatie JJJJ-MM-DD. Voer voor deze snelstart de datum van morgen in. | *FUTURE_DATE* |
| *account-name* | De naam van het opslagaccount. Gebruik de naam die in een vorige stap is bewaard. | *YOUR_STORAGE_ACCOUNT_NAME* |
| *account-key*  | De opslagaccountsleutel. Gebruik de sleutel die in een vorige stap is bewaard. | *YOUR_STORAGE_ACCOUNT_KEY* |

Gebruik de volgende CLI-opdracht met daadwerkelijke waarden voor de tijdelijke aanduiding voor het genereren van een SAS die u in uw JavaScript-code gebruiken kunt.

```azurecli-interactive
az storage account generate-sas \
  --permissions racwdl \
  --resource-types sco \
  --services b \
  --expiry FUTURE_DATE \
  --account-name YOUR_STORAGE_ACCOUNT_NAME \
  --account-key YOUR_STORAGE_ACCOUNT_KEY
```

Mogelijk vindt u de reeks waarden na elke parameter wat cryptisch. Deze parameterwaarden zijn afkomstig van de eerste letter van hun respectieve machtiging. In de volgende tabel wordt uitgelegd waar de waarden vandaan komen:

| Parameter        | Value   | Description  |
|------------------|---------|---------|
| *machtigingen*    | racwdl  | Deze SAS staat de mogelijkheden *read*, *append*, *create*, *write*, *delete* en *list* toe. |
| *resource-types* | sco     | De resources die waar de SAS invloed op heeft, zijn *service*, *container* en *object*. |
| *services*       | b       | De service waar de SAS invloed op heeft is de *blob*-service. |

Nu dat de SAS is gegenereerd, kopieert u de geretourneerde waarde en sla deze ergens voor gebruik in een volgende stap. Als u uw SAS met behulp van een andere methode dan de Azure CLI hebt gegenereerd, moet u de eerste verwijderen `?` als deze aanwezig is. Dit teken is een URL-scheidingsteken dat al is opgegeven in de sjabloon URL verderop in dit onderwerp waarbij de SAS wordt gebruikt.

> [!IMPORTANT]
> Geef in productie SAS-tokens altijd door met behulp van SSL. SAS-tokens moeten op de server worden gegenereerd en naar de HTML-pagina verzonden om ze opnieuw door te geven aan Azure Blob Storage. U kunt eventueel een serverloze functie gebruiken om SAS-tokens te genereren. Azure Portal bevat functiesjablonen waarmee een SAS kan worden gemaakt met behulp van een JavaScript-functie.

## <a name="implement-the-html-page"></a>HTML-pagina implementeren

In deze sectie maakt u een eenvoudige webpagina maken en configureren van VS Code om te starten en fouten opsporen in de pagina. Voordat u starten kunt, maar moet u u Node.js gebruikt om een lokale webserver starten en de pagina fungeren als uw browser om vraagt. Vervolgens voegt u JavaScript-code voor het aanroepen van verschillende blob storage-API's en de resultaten worden weergegeven op de pagina. U ziet ook de resultaten van deze aanroepen in de [Azure-portal](https://portal.azure.com), [Azure Storage Explorer](https://azure.microsoft.com/features/storage-explorer), en de [Azure Storage-extensie](vscode:extension/ms-azuretools.vscode-azurestorage) voor VS Code.

### <a name="set-up-the-web-application"></a>Webtoepassing instellen

Maak eerst een nieuwe map met de naam *azure-blobs-javascript* te openen in VS Code. Maak een nieuw bestand in VS Code, voeg vervolgens de volgende HTML-code en sla het bestand als *index.html* in de *azure-blobs-javascript* map.

```html
<!DOCTYPE html>
<html>

<body>
    <button id="create-container-button">Create container</button>
    <button id="delete-container-button">Delete container</button>
    <button id="select-button">Select and upload files</button>
    <input type="file" id="file-input" multiple style="display: none;" />
    <button id="list-button">List files</button>
    <button id="delete-button">Delete selected files</button>
    <p><b>Status:</b></p>
    <p id="status" style="height:160px; width: 593px; overflow: scroll;" />
    <p><b>Files:</b></p>
    <select id="file-list" multiple style="height:222px; width: 593px; overflow: scroll;" />
</body>

<!-- You'll add code here later in this quickstart. -->

</html>
```

### <a name="configure-the-debugger"></a>Het foutopsporingsprogramma configureren

Als u de debugger-extensie in VS Code instelt, selecteert u **fouten opsporen > configuratie toevoegen...** en selecteer vervolgens **Chrome** of **Edge**, afhankelijk van welke extensie die u eerder in de sectie vereisten geïnstalleerd. Met deze actie maakt een *launch.json* -bestand en wordt deze geopend in de editor.

Wijzig vervolgens de *launch.json* bestand zodat het `url` waarde bevat `/index.html` zoals wordt weergegeven:

```json
{
    // Use IntelliSense to learn about possible attributes.
    // Hover to view descriptions of existing attributes.
    // For more information, visit: https://go.microsoft.com/fwlink/?linkid=830387
    "version": "0.2.0",
    "configurations": [
        {
            "type": "chrome",
            "request": "launch",
            "name": "Launch Chrome against localhost",
            "url": "http://localhost:8080/index.html",
            "webRoot": "${workspaceFolder}"
        }
    ]
}
```

Deze configuratie doorgegeven VS Code browser om te starten en welke URL moet worden geladen.

### <a name="launch-the-web-server"></a>De webserver starten

Als u wilt starten op de lokale Node.js-webserver, selecteer **weergave > Terminal** om te openen in een consolevenster in VS Code, voert u de volgende opdracht uit.

```console
npx http-server
```

Met deze opdracht installeert de *http-server* verpakt en start de server, zodat de huidige map beschikbaar via de standaard-URL's zoals aangegeven in de vorige stap.

### <a name="start-debugging"></a>Foutopsporing starten

Om te starten *index.html* in de browser met het foutopsporingsprogramma VS Code gekoppelde, selecteer **fouten opsporen > Foutopsporing starten** of druk op F5 in Visual Studio Code.

De gebruikersinterface weergegeven doet nog niets, maar u JavaScript-code voegt in de volgende sectie voor het implementeren van elke functie die wordt weergegeven. U kunt vervolgens onderbrekingspunten instellen en gebruiken met het foutopsporingsprogramma wanneer deze is onderbroken op uw code.

Wanneer u wijzigingen in aanbrengt *index.html*, zorg ervoor dat u het laden van de pagina en ziet u de wijzigingen in de browser. In VS Code, u kunt ook selecteren **fouten opsporen > Foutopsporing starten** of druk op CTRL + SHIFT + F5.

### <a name="add-the-blob-storage-client-library"></a>Toevoegen van de blob storage-clientbibliotheek

Om in te schakelen aanroepen naar de blob-opslag-API, eerst [downloaden van de Azure Storage SDK voor JavaScript - clientbibliotheek voor Blob](https://aka.ms/downloadazurestoragejsblob), pak de inhoud van het ZIP-bestand en plaats de *azure storage.blob.js* bestand in de *azure-blobs-javascript* map.

Plak vervolgens de volgende HTML-code in *index.html* nadat de `</body>` afsluitcode, vervang de tijdelijke aanduiding opmerking.

```html
<script src="azure-storage.blob.js" charset="utf-8"></script>

<script>
// You'll add code here in the following sections.
</script>
```

Deze code wordt een verwijzing naar het scriptbestand toegevoegd en biedt een locatie voor uw eigen JavaScript-code. Voor de doeleinden van deze quickstart maken we gebruik de *azure storage.blob.js* scriptbestand zodat u deze in VS Code openen kunt, lees de inhoud en onderbrekingspunten instellen. In de productieomgeving, moet u de compactere *azure storage.blob.min.js* -bestand dat is ook beschikbaar in het zip-bestand.

U vindt meer informatie over elke functie van de blob-opslag in de [referentiedocumentatie](https://docs.microsoft.com/javascript/api/%40azure/storage-blob/index). Houd er rekening mee dat sommige van de functies in de SDK alleen beschikbaar in Node.js en is alleen beschikbaar in de browser zijn.

De code in *azure storage.blob.js* Hiermee exporteert u een globale variabele met de naam `azblob`, die u gebruikt in uw JavaScript-code voor toegang tot de API's van de blob-opslag.

### <a name="add-the-initial-javascript-code"></a>Voeg de initiële JavaScript-code

Plak vervolgens de volgende code in de `<script>` element wordt weergegeven in het vorige blok van code, vervang de tijdelijke aanduiding opmerking.

```javascript
const createContainerButton = document.getElementById("create-container-button");
const deleteContainerButton = document.getElementById("delete-container-button");
const selectButton = document.getElementById("select-button");
const fileInput = document.getElementById("file-input");
const listButton = document.getElementById("list-button");
const deleteButton = document.getElementById("delete-button");
const status = document.getElementById("status");
const fileList = document.getElementById("file-list");

const reportStatus = message => {
    status.innerHTML += `${message}<br/>`;
    status.scrollTop = status.scrollHeight;
}
```

Deze code maakt velden voor elk HTML-element dat door de volgende code wordt gebruikt, en implementeert een `reportStatus` functie om uitvoer weer te geven.

In de volgende secties moet u elke nieuwe blok van JavaScript-code toevoegen na de vorige blok.

### <a name="add-your-storage-account-info"></a>De accountgegevens van uw opslag toevoegen

Vervolgens voegt u code voor toegang tot uw storage-account, de tijdelijke aanduidingen vervangt door uw accountnaam en de SAS die u hebt gegenereerd in de vorige stap.

```javascript
const accountName = "<Add your storage account name>";
const sasString = "<Add the SAS you generated earlier>";
const containerName = "testcontainer";
const containerURL = new azblob.ContainerURL(
    `https://${accountName}.blob.core.windows.net/${containerName}?${sasString}`,
    azblob.StorageURL.newPipeline(new azblob.AnonymousCredential)));
```

Deze code maakt gebruik van uw accountgegevens en een SAS maken een [ContainerURL](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL) exemplaar, wat nuttig is voor het maken en bewerken van een storage-container.

### <a name="create-and-delete-a-storage-container"></a>Maken en verwijderen van een storage-container

Vervolgens voegt u code voor het maken en verwijderen van de storage-container wanneer u op de bijbehorende knop toe.

```javascript
const createContainer = async () => {
    try {
        reportStatus(`Creating container "${containerName}"...`);
        await containerURL.create(azblob.Aborter.none);
        reportStatus(`Done.`);
    } catch (error) {
        reportStatus(error.body.message);
    }
};

const deleteContainer = async () => {
    try {
        reportStatus(`Deleting container "${containerName}"...`);
        await containerURL.delete(azblob.Aborter.none);
        reportStatus(`Done.`);
    } catch (error) {
        reportStatus(error.body.message);
    }
};

createContainerButton.addEventListener("click", createContainer);
deleteContainerButton.addEventListener("click", deleteContainer);
```

Deze code roept de ContainerURL [maken](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL#create-aborter--icontainercreateoptions-) en [verwijderen](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL#delete-aborter--icontainerdeletemethodoptions-) functies zonder gebruik van een [Aborter](https://docs.microsoft.com/javascript/api/@azure/storage-blob/aborter) exemplaar. Deze code dingen om eenvoudig te houden voor deze Quick Start, wordt ervan uitgegaan dat uw storage-account is gemaakt en is ingeschakeld. Gebruik bij de productiecode een exemplaar Aborter time-out functionaliteit toe te voegen.

### <a name="list-blobs"></a>Blobs vermelden

Voeg vervolgens de code om de inhoud van de storage-container wanneer u op de **bestanden** knop.

```javascript
const listFiles = async () => {
    fileList.size = 0;
    fileList.innerHTML = "";
    try {
        reportStatus("Retrieving file list...");
        let marker = undefined;
        do {
            const listBlobsResponse = await containerURL.listBlobFlatSegment(
                azblob.Aborter.none, marker);
            marker = listBlobsResponse.nextMarker;
            const items = listBlobsResponse.segment.blobItems;
            for (const blob of items) {
                fileList.size += 1;
                fileList.innerHTML += `<option>${blob.name}</option>`;
            }
        } while (marker);
        if (fileList.size > 0) {
            reportStatus("Done.");
        } else {
            reportStatus("The container does not contain any files.");
        }
    } catch (error) {
        reportStatus(error.body.message);
    }
};

listButton.addEventListener("click", listFiles);
```

Deze code roept de [ContainerURL.listBlobFlatSegment](https://docs.microsoft.com/javascript/api/@azure/storage-blob/ContainerURL#listblobflatsegment-aborter--string--icontainerlistblobssegmentoptions-) functie in een lus om ervoor te zorgen dat alle segmenten worden opgehaald. Voor elk segment, deze wordt uitgevoerd via de lijst met blob-items bevat en werkt de **bestanden** lijst.

### <a name="upload-blobs"></a>Blobs uploaden

Voeg vervolgens de code voor het uploaden van bestanden naar de storage-container wanneer u op de **selecteren en uploaden van bestanden** knop.

```javascript
const uploadFiles = async () => {
    try {
        reportStatus("Uploading files...");
        const promises = [];
        for (const file of fileInput.files) {
            const blockBlobURL = azblob.BlockBlobURL.fromContainerURL(containerURL, file.name);
            promises.push(azblob.uploadBrowserDataToBlockBlob(
                azblob.Aborter.none, file, blockBlobURL));
        }
        await Promise.all(promises);
        reportStatus("Done.");
        listFiles();
    } catch (error) {
        reportStatus(error.body.message);
    }
}

selectButton.addEventListener("click", () => fileInput.click());
fileInput.addEventListener("input", uploadFiles);
```

Deze code maakt verbinding de **selecteren en uploaden van bestanden** knop aan de verborgen `file-input` element. Op deze manier kunnen de knop `click` gebeurtenis activeert de bestandsinvoer `click` gebeurtenis en wordt de bestandenkiezer weergegeven. Nadat u bestanden en het dialoogvenster sluit de `input` gebeurtenis zich voordoet en de `uploadFiles` functie wordt aangeroepen. Deze functie roept de browser-alleen [uploadBrowserDataToBlockBlob](https://docs.microsoft.com/javascript/api/@azure/storage-blob/#uploadbrowserdatatoblockblob-aborter--blob---arraybuffer---arraybufferview--blockbloburl--iuploadtoblockbloboptions-) functie voor elk bestand dat u hebt geselecteerd. Elke aanroep retourneert een Promise, zodat ze kunnen alle worden gestopt in één keer, waardoor de bestanden te uploaden parallel aan een lijst wordt toegevoegd.

### <a name="delete-blobs"></a>Blobs verwijderen

Voeg vervolgens de code voor het verwijderen van bestanden uit de storage-container wanneer u op de **verwijderen van geselecteerde bestanden** knop.

```javascript
const deleteFiles = async () => {
    try {
        if (fileList.selectedOptions.length > 0) {
            reportStatus("Deleting files...");
            for (const option of fileList.selectedOptions) {
                const blobURL = azblob.BlobURL.fromContainerURL(containerURL, option.text);
                await blobURL.delete(azblob.Aborter.none);
            }
            reportStatus("Done.");
            listFiles();
        } else {
            reportStatus("No files selected.");
        }
    } catch (error) {
        reportStatus(error.body.message);
    }
};

deleteButton.addEventListener("click", deleteFiles);
```

Deze code roept de [BlobURL.delete](https://docs.microsoft.com/javascript/api/@azure/storage-blob/BlobURL#delete-aborter--iblobdeleteoptions-) functie voor het verwijderen van elk bestand dat is geselecteerd in de lijst. Vervolgens wordt de `listFiles` functie die wordt weergegeven in een eerdere vernieuwen van de inhoud van de **bestanden** lijst.

### <a name="run-and-test-the-web-application"></a>Uitvoeren en testen van de web-App

Op dit moment kunt u starten de pagina en het experiment naar een idee krijgen van hoe u blob-opslag werkt. Als er fouten optreden (bijvoorbeeld, wanneer u weergeven van bestanden wilt voordat u de container hebt gemaakt), de **Status** deelvenster wordt weergegeven het foutbericht ontvangen. U kunt ook onderbrekingspunten instellen in de JavaScript-code voor het onderzoeken van de waarden die zijn geretourneerd door de storage-API's.

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt opschonen van de resources die tijdens deze Quick Start zijn gemaakt, gaat u naar de [Azure-portal](https://portal.azure.com) en verwijder de resourcegroep die u hebt gemaakt in de sectie vereisten.

## <a name="next-steps"></a>Volgende stappen

In deze snelstartgids hebt u een eenvoudige website waarmee toegang krijgt tot browser gebaseerde JavaScript-opslag-blob gemaakt. Als u wilt weten hoe u een website zelf op blob-opslag kunt hosten, gaat u verder met de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Host een statische website op Blob-opslag](https://docs.microsoft.com/azure/storage/blobs/storage-blob-static-website-host)
