---
title: Functies verbinden met Azure Storage met behulp van Visual Studio Code
description: Informatie over het toevoegen van een Uitvoerbinding uw functies verbinden met een Azure Storage-wachtrij met behulp van Visual Studio Code.
author: ggailey777
ms.author: glenga
ms.date: 06/25/2019
ms.topic: quickstart
ms.service: azure-functions
ms.custom: mvc
manager: jeconnoc
ms.openlocfilehash: b207064f691391af2c180c7a6ab03e42ed79fcb6
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67450994"
---
# <a name="connect-functions-to-azure-storage-using-visual-studio-code"></a>Functies verbinden met Azure Storage met behulp van Visual Studio Code

Azure Functions kunt u verbinding maken met functies tot Azure-services en andere bronnen zonder te hoeven schrijven van uw eigen integratiecode. Deze *bindingen*, die staan voor zowel invoer en uitvoer, zijn gedefinieerd binnen de functiedefinitie. Gegevens van bindingen is opgegeven voor de functie als parameters. Een trigger is een speciaal type Invoerbinding. Als een functie slechts één trigger heeft, kan deze hebben verschillende invoer en uitvoerbindingen. Zie voor meer informatie, [Azure Functions-triggers en bindingen concepten](functions-triggers-bindings.md).

Dit artikel ziet u hoe u met Visual Studio Code verbinding maken met de functie die u hebt gemaakt in de [vorige artikel](functions-create-first-function-vs-code.md) naar Azure Storage. De Uitvoerbinding die u aan deze functie toevoegt schrijft gegevens uit de HTTP-aanvraag naar een bericht in een Azure Queue storage-wachtrij. 

De meeste bindingen vereisen een opgeslagen verbindingsreeks die functies gebruikt voor toegang tot de afhankelijke service. Te vereenvoudigen, kunt u het opslagaccount dat u hebt gemaakt met de functie-app gebruiken. De verbinding met dit account is al opgeslagen in een app-instelling met de naam `AzureWebJobsStorage`.  

## <a name="prerequisites"></a>Vereisten

Voordat u dit artikel, moet u voldoen aan de volgende vereisten:

* Installeer de [Azure Storage-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage).
* Installeer [Azure Storage Explorer](https://storageexplorer.com/). Storage Explorer is een hulpprogramma dat u gebruikt om die worden gegenereerd door de Uitvoerbinding Wachtrijberichten te onderzoeken. Storage Explorer wordt ondersteund op macOS, Windows en Linux-besturingssystemen.
* Installeer [.NET Core CLI-hulpprogramma's](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x) (C# alleen projecten).
* Voer de stappen in [deel 1 van de Visual Studio Code-snelstartgids](functions-create-first-function-vs-code.md). 

In dit artikel wordt ervan uitgegaan dat u al bent aangemeld uw Azure-abonnement vanuit Visual Studio Code. U kunt zich aanmelden door te voeren `Azure: Sign In` vanuit het opdrachtenpalet. 

## <a name="download-the-function-app-settings"></a>De instellingen voor de functie-app downloaden

In de [vorige artikel](functions-create-first-function-vs-code.md), u hebt een functie-app in Azure gemaakt, samen met het Opslagaccount dat is vereist. De verbindingsreeks voor dit account veilig opgeslagen in de instellingen van de app in Azure. In dit artikel, kunt u berichten schrijven naar een opslagwachtrij van hetzelfde account. Voor verbinding met uw Storage-account wanneer de functie lokaal wordt uitgevoerd, moet u app-instellingen downloaden naar het bestand local.settings.json. 

1. Druk op F1 drukt het opdrachtenpalet te openen en vervolgens zoeken naar en voer de opdracht `Azure Functions: Download Remote Settings....`. 

1. Kies de functie-app die in de vorige artikel hebt gemaakt. Selecteer **Ja op Alles** overschrijven de bestaande lokale instellingen. 

    > [!IMPORTANT]  
    > Omdat deze geheimen, bevat het bestand local.settings.json nooit wordt gepubliceerd en wordt uitgesloten van broncodebeheer.

1. Kopieer de waarde `AzureWebJobsStorage`, dit is de sleutel voor de Storage account connection string-waarde. U deze verbinding gebruiken om te controleren of de Uitvoerbinding werkt zoals verwacht.

## <a name="register-binding-extensions"></a>Binding-extensies registreren

Omdat u van een Queue storage-Uitvoerbinding gebruikmaakt, hebt u de Storage-bindingen-extensie geïnstalleerd voordat u het project uitvoeren. 

### <a name="javascript"></a>Javascript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>C\# klassebibliotheek

Met uitzondering van de HTTP- en timer wordt geactiveerd, worden de bindingen geïmplementeerd als extensiepakketten. Voer de volgende [dotnet-pakket toevoegen](/dotnet/core/tools/dotnet-add-package) opdracht in de Terminal-venster voor het pakket met de uitbreiding opslag toevoegen aan uw project.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

U kunt nu de storage-Uitvoerbinding aan uw project toevoegen.

## <a name="add-an-output-binding"></a>Een uitvoerbinding toevoegen

In de functies, elk type binding vereist een `direction`, `type`, en een unieke `name` worden gedefinieerd in de function.json-bestand. De manier waarop u deze kenmerken definiëren, is afhankelijk van de taal van uw functie-app.

### <a name="javascript"></a>Javascript

Binding-kenmerken worden rechtstreeks in het bestand function.json gedefinieerd. Afhankelijk van het bindingstype mogelijk extra eigenschappen vereist. De [configuratie van de wachtrij uitvoer](functions-bindings-storage-queue.md#output---configuration) beschrijft de velden die nodig zijn voor een Azure Storage-wachtrij-binding. De extensie kunt eenvoudig bindingen toevoegen aan het bestand function.json. 

Een binding maken met de rechtermuisknop op (Ctrl + klik op Mac OS) de `function.json` in de map HttpTrigger-bestand en kies **binding toevoegen...** . Volg de aanwijzingen voor het definiëren van de volgende bindingeigenschappen voor de nieuwe binding:

| Vraag | Value | Description |
| -------- | ----- | ----------- |
| **Richting van binding selecteren** | `out` | De binding is een Uitvoerbinding. |
| **Binding met de richting selecteren...** | `Azure Queue Storage` | De binding is een Azure Storage-wachtrij-binding. |
| **De naam die wordt gebruikt om deze binding in uw code** | `msg` | Naam ter identificatie van de bindingsparameter waarnaar wordt verwezen in uw code. |
| **De wachtrij waarnaar het bericht wordt verzonden** | `outqueue` | De naam van de wachtrij die de binding naar schrijft. Wanneer de *queueName* niet bestaat, de binding wordt deze gemaakt bij het eerste gebruik. |
| **Selecteer de instelling van "local.setting.json"** | `AzureWebJobsStorage` | De naam van een toepassingsinstelling met de verbindingsreeks voor de Storage-account. De `AzureWebJobsStorage` instelling bevat de verbindingsreeks voor het Opslagaccount dat u hebt gemaakt met de functie-app. |

Een binding wordt toegevoegd aan de `bindings` -matrix in uw function.json-bestand er nu als in het volgende voorbeeld uitzien moet:

```json
{
   ...

  "bindings": [
    {
      "authLevel": "function",
      "type": "httpTrigger",
      "direction": "in",
      "name": "req",
      "methods": [
        "get",
        "post"
      ]
    },
    {
      "type": "http",
      "direction": "out",
      "name": "$return"
    },
    {
      "type": "queue",
      "direction": "out",
      "name": "msg",
      "queueName": "outqueue",
      "connection": "AzureWebJobsStorage"
    }
  ]
}
```

### <a name="c-class-library"></a>C\# klassebibliotheek

In een C# -klassebibliotheekproject, de bindingen zijn gedefinieerd als bindend kenmerken voor de functie-methode. Het bestand function.json wordt vervolgens automatisch gegenereerd op basis van deze kenmerken.

Open het projectbestand HttpTrigger.cs en voeg de volgende `using` instructie:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

Voeg de volgende parameter voor de `Run` methodedefinitie:

```cs
[Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg
```

De `msg` parameter is een `ICollector<T>` type, dat staat voor een verzameling van berichten die worden geschreven naar een Uitvoerbinding wanneer de functie is voltooid. In dit geval wordt de uitvoer is een storage-wachtrij met de naam `outqueue`. De verbindingsreeks voor het Opslagaccount dat is ingesteld door de `StorageAccountAttribute`. Dit kenmerk geeft aan dat de instelling die de verbindingsreeks van de Storage-account bevat en kan worden toegepast op de klasse, methode of het niveau van de parameter. In dit geval zou u `StorageAccountAttribute` omdat u al van het standaardopslagaccount gebruikmaakt.

De definitie van de Run-methode moet er nu als volgt uitzien:  

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
```

## <a name="add-code-that-uses-the-output-binding"></a>Code toevoegen die gebruikmaakt van de uitvoerbinding

Nadat de binding is gedefinieerd, kunt u de `name` van de binding om deze te openen als een kenmerk in de functiehandtekening. Met behulp van een Uitvoerbinding, kunt u geen gebruik van de Azure Storage-SDK-code voor verificatie ophalen van de verwijzing naar een wachtrij of schrijven van gegevens. De Azure Functions-runtime en queue-uitvoer binding die taken voor u doen.

### <a name="javascript"></a>Javascript

Voeg code toe die gebruikmaakt van de `msg` binding uitvoerobject op `context.bindings` om een wachtrijbericht te maken. Voeg deze code toe vóór de instructie `context.res`.

```javascript
// Add a message to the Storage queue.
context.bindings.msg = "Name passed to the function: " + 
(req.query.name || req.body.name);
```

Op dit moment ziet uw functie er als volgt uit:

```javascript
module.exports = async function (context, req) {
    context.log('JavaScript HTTP trigger function processed a request.');

    if (req.query.name || (req.body && req.body.name)) {
        // Add a message to the Storage queue.
        context.bindings.msg = "Name passed to the function: " + 
        (req.query.name || req.body.name);
        context.res = {
            // status: 200, /* Defaults to 200 */
            body: "Hello " + (req.query.name || req.body.name)
        };
    }
    else {
        context.res = {
            status: 400,
            body: "Please pass a name on the query string or in the request body"
        };
    }
};
```

### <a name="c"></a>C\#

Voeg code toe die gebruikmaakt van de `msg` uitvoer binding-object om een wachtrijbericht te maken. Voeg deze code toe voordat de methode retourneert.

```cs
if (!string.IsNullOrEmpty(name))
{
    // Add a message to the output collection.
    msg.Add(string.Format("Name passed to the function: {0}", name));
}
```

Op dit moment ziet uw functie er als volgt uit:

```cs
[FunctionName("HttpTrigger")]
public static async Task<IActionResult> Run(
    [HttpTrigger(AuthorizationLevel.Function, "get", "post", Route = null)] HttpRequest req, 
    [Queue("outqueue"),StorageAccount("AzureWebJobsStorage")] ICollector<string> msg, ILogger log)
{
    log.LogInformation("C# HTTP trigger function processed a request.");

    string name = req.Query["name"];

    string requestBody = await new StreamReader(req.Body).ReadToEndAsync();
    dynamic data = JsonConvert.DeserializeObject(requestBody);
    name = name ?? data?.name;

    if (!string.IsNullOrEmpty(name))
    {
        // Add a message to the output collection.
        msg.Add(string.Format("Name passed to the function: {0}", name));
    }
    return name != null
        ? (ActionResult)new OkObjectResult($"Hello, {name}")
        : new BadRequestObjectResult("Please pass a name on the query string or in the request body");
}
```

[!INCLUDE [functions-run-function-test-local-vs-code](../../includes/functions-run-function-test-local-vs-code.md)]

Wanneer de uitvoerbinding voor het eerst wordt gebruikt, wordt er door de runtime van Functions een nieuwe wachtrij met de naam **outqueue** gemaakt in uw opslagaccount. U gebruikt Storage Explorer om te controleren of de wachtrij is gemaakt, samen met het nieuwe bericht.

### <a name="connect-storage-explorer-to-your-account"></a>Storage Explorer verbinden met uw account

Deze sectie overslaan als u al hebt geïnstalleerd Azure Storage Explorer en hebt verbonden met uw Azure-account.

1. Voer de [Azure-opslagverkenner] hulpprogramma, selecteer het verbindingspictogram aan de linkerkant en selecteer **een account toevoegen**.

    ![Toevoegen van een Azure-account aan Microsoft Azure Storage Explorer](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-add-account.png)

1. In de **Connect** dialoogvenster kiezen **toevoegen van een Azure-account**, kies uw **Azure-omgeving**, en selecteer **aanmelden...** . 

    ![Aanmelden bij uw Azure-account](./media/functions-add-output-binding-storage-queue-vs-code/storage-explorer-connect-azure-account.png)

Nadat u bij uw account aanmelden, ziet u alle van de Azure-abonnementen die zijn gekoppeld aan uw account.

### <a name="examine-the-output-queue"></a>De uitvoerwachtrij controleren

1. Druk op F1 drukt het opdrachtenpalet te openen en vervolgens zoeken naar en voer de opdracht uit in Visual Studio Code, `Azure Storage: Open in Storage Explorer` en kies de naam van uw Opslagaccount. Uw storage-account wordt geopend in Azure Storage Explorer.  

1. Vouw het knooppunt **Wachtrijen** uit en selecteer vervolgens de wachtrij met de naam **outqueue**. 

   De wachtrij bevat het bericht dat met de Queue Storage-uitvoerbinding is gemaakt toen u de met HTTP geactiveerde functie hebt uitgevoerd. Als u de functie hebt aangeroepen met de standaardwaarde voor `name`, namelijk *Azure*, is het wachtrijbericht *Naam is doorgegeven aan de functie: Azure*.

    ![Wachtrijbericht weergegeven in Azure Storage Explorer](./media/functions-add-output-binding-storage-queue-vs-code/function-queue-storage-output-view-queue.png)

1. Voer de functie een andere aanvraag opnieuw verzenden en ziet u een nieuw bericht weergegeven in de wachtrij.  

Nu is het tijd om opnieuw te publiceren van de bijgewerkte functie-app naar Azure.

## <a name="redeploy-and-test-the-updated-app"></a>Opnieuw implementeren en testen van de bijgewerkte app

1. Druk op F1 voor het opdrachtenpalet te openen in Visual Studio Code. In het opdrachtenpalet, zoek en selecteer `Azure Functions: Deploy to function app...`.

1. Kies de functie-app die u hebt gemaakt in het eerste artikel. Omdat u uw project binnen dezelfde app opnieuw wilt implementeren, selecteer **implementeren** voor het verwijderen van de waarschuwing over bestanden worden overschreven.

1. Nadat de implementatie is voltooid, kunt u het opnieuw cURL of een browser gebruiken om het opnieuw geïmplementeerde functie te testen. Als voorheen, voeg de queryreeks `&name=<yourname>` naar de URL, zoals in het volgende voorbeeld:

    ```bash
    curl https://myfunctionapp.azurewebsites.net/api/httptrigger?code=cCr8sAxfBiow548FBDLS1....&name=<yourname>
    ```

1. Opnieuw [bericht weergeven in de storage-wachtrij](#examine-the-output-queue) om te controleren dat de Uitvoerbinding opnieuw worden gegenereerd voor een nieuw bericht in de wachtrij.

## <a name="clean-up-resources"></a>Resources opschonen

*Resources* verwijst in Azure naar functie-apps, functies, opslagaccounts enzovoort. Deze zijn gegroepeerd in *resourcegroepen*. U kunt alle resources in een groep verwijderen door de groep zelf te verwijderen.

U hebt resources gemaakt om deze snelstartgidsen te voltooien. Deze resources kunnen bij u in rekening worden gebracht, afhankelijk van de [accountstatus](https://azure.microsoft.com/account/) en [serviceprijzen](https://azure.microsoft.com/pricing/). Als u de resources niet meer nodig hebt, kunt u ze als volgt verwijderen:

1. Druk op F1 voor het opdrachtenpalet te openen in Visual Studio Code. In het opdrachtenpalet, zoek en selecteer `Azure Functions: Open in portal`.

1. Kies uw functie-app en druk op Enter. De pagina functie-app wordt geopend in de [Azure-portal](https://portal.azure.com).

1. In de **overzicht** tabblad, selecteert u de koppeling met de naam onder **resourcegroep**.

    ![Selecteer op de pagina Functie-app de resourcegroep die u wilt verwijderen.](./media/functions-add-output-binding-storage-queue-vs-code/functions-app-delete-resource-group.png)

1. Bekijk op de pagina **Resourcegroep** de lijst met opgenomen resources en controleer of dit de resources zijn die u wilt verwijderen.
 
1. Selecteer **Resourcegroep verwijderen** en volg de instructies.

   Verwijderen kan enkele minuten duren. Wanneer dit is voltooid, verschijnt een aantal seconden een melding in beeld. U kunt ook het belpictogram boven aan de pagina selecteren om de melding te bekijken.

## <a name="next-steps"></a>Volgende stappen

U kunt uw HTTP-geactiveerde functie voor het schrijven van gegevens in een opslagwachtrij hebt bijgewerkt. Zie voor meer informatie over het ontwikkelen van functies, [Azure Functions ontwikkelen met behulp van Visual Studio Code](functions-develop-vs-code.md).

Vervolgens moet u de bewaking van Application Insights voor uw functie-app inschakelen:

> [!div class="nextstepaction"]
> [Application Insights-integratie inschakelen](functions-monitoring.md#manually-connect-an-app-insights-resource)

[Azure-opslagverkenner]: https://storageexplorer.com/
