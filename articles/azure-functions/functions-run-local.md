---
title: Werken met Azure Functions kernonderdelen | Microsoft Docs
description: Ontdek hoe u de code en testen van Azure functions vanaf de opdrachtprompt of terminal op uw lokale computer voordat u deze op Azure Functions uitvoeren.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: ''
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 06/26/2018
ms.author: glenga
ms.openlocfilehash: 5c582b080ec6f2cff801758fc4bff4f7d07fd7df
ms.sourcegitcommit: d1eefa436e434a541e02d938d9cb9fcef4e62604
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2018
ms.locfileid: "37083066"
---
# <a name="work-with-azure-functions-core-tools"></a>Werken met Azure Functions kernonderdelen

Azure Functions Core-hulpprogramma's kunt u ontwikkelen en testen van uw functies op uw lokale computer vanuit de opdrachtprompt of terminal. Uw lokale functies kunnen verbinding maken met live Azure-services en u kunt fouten opsporen in uw functies op uw lokale computer met behulp van de volledige functies runtime. U kunt ook een functie-app implementeren op uw Azure-abonnement.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## <a name="core-tools-versions"></a>Core-versies van de hulpprogramma 's

Er zijn twee versies van Azure Functions Core Tools. De versie die u gebruikt, is afhankelijk van uw lokale ontwikkelingsomgeving, de keuze van taal en het niveau van ondersteuning vereist:

+ [Versie 1.x](#v1): ondersteunt versie 1.x van de runtime, in het algemeen beschikbaar (GA) is. Deze versie van de hulpprogramma's wordt alleen ondersteund op Windows-computers en wordt geïnstalleerd vanuit een [npm pakket](https://docs.npmjs.com/getting-started/what-is-npm). Met deze versie kunt u functies in experimentele talen die niet officieel ondersteund. Zie voor meer informatie [ondersteunde talen in Azure Functions](supported-languages.md)

+ [Versie 2.x](#v2): versie ondersteunt 2.x van de runtime. Deze versie ondersteunt [Windows](#windows-npm), [Mac OS](#brew), en [Linux](#linux). Platform-specifieke pakket managers of npm gebruikt voor de installatie. Als de runtime 2.x is deze versie van de core-hulpprogramma's momenteel preview.

Tenzij anders vermeld, de voorbeelden in dit artikel zijn voor versie 2.x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure Functions kernonderdelen] bevat een versie van de dezelfde runtime die ook door Azure Functions-runtime die kunnen worden uitgevoerd op de lokale computer. Het biedt ook functies maken en implementeren van de functie projecten verbinding maken met Azure-opdrachten.

### <a name="v1"></a>Versie 1.x

De runtime van Functions 1.x maakt gebruik van de oorspronkelijke versie van de hulpprogramma's. Deze versie .NET Framework (4.7.1) wordt gebruikt en wordt alleen ondersteund op Windows-computers. Voordat u de versie 1.x-hulpprogramma's installeert, moet u [installeren NodeJS](https://docs.npmjs.com/getting-started/installing-node), waaronder npm.

Gebruik de volgende opdracht voor het installeren van de versie 1.x-hulpprogramma's:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Versie 2.x

>[!NOTE]
> Azure Functions-runtime 2.0 is een Preview-versie en op dit moment niet alle functies van Azure Functions worden ondersteund. Zie voor meer informatie [Azure Functions-versies](functions-versions.md) 

Versie 2.x van de hulpprogramma's gebruikmaakt van de Azure Functions-runtime 2.x die is gebaseerd op .NET Core. Deze versie wordt ondersteund op alle platforms .NET Core 2.x ondersteunt, met inbegrip van [Windows](#windows-npm), [Mac OS](#brew), en [Linux](#linux).

#### <a name="windows-npm"></a>Windows

De volgende stappen gebruikt npm Core hulpprogramma's installeren op Windows. U kunt ook [Chocolatey](https://chocolatey.org/). Zie voor meer informatie de [kernonderdelen Leesmij](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Installeer [.NET Core 2.0 voor Windows](https://www.microsoft.com/net/download/windows).

2. Installeer [Node.js], waaronder npm. Voor versie 2.x van de hulpprogramma's, alleen Node.js 8.5 en latere versies worden ondersteund.

3. Installeer het pakket Core's:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>Mac OS met Homebrew

De volgende stappen gebruikt Homebrew de Core-hulpprogramma's installeren op Mac OS.

1. Installeer [.NET Core 2.0 voor Mac OS](https://www.microsoft.com/net/download/macos).

2. Installeer [Homebrew](https://brew.sh/), als deze nog niet is geïnstalleerd.

3. Installeer het pakket Core's:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) met APT

De volgende stappen uitvoeren om [APT](https://wiki.debian.org/Apt) Core hulpprogramma's installeren op uw Ubuntu/Debian Linux-distributie. Zie voor andere Linux-distributies de [kernonderdelen Leesmij](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Installeer [.NET Core 2.0 voor Linux](https://www.microsoft.com/net/download/linux).

2. De productcode van Microsoft als vertrouwd registreren:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Controleer of dat uw Ubuntu server wordt uitgevoerd een van de juiste versies van de onderstaande tabel. De apt als bron wilt toevoegen, voert u het:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux-distributie | Versie |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux 18 voorkomt    | `xenial`  |

4. Installeer het pakket Core's:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## <a name="create-a-local-functions-project"></a>Een lokale functies-project maken

Een projectmap functies bevat de bestanden [host.json](functions-host-json.md) en [local.settings.json](#local-settings-file), langs submappen die de code voor afzonderlijke functies bevatten. Deze map is het equivalent van een functie-app in Azure. Zie voor meer informatie over de structuur van de functies, de [handleiding voor ontwikkelaars voor Azure Functions voor](functions-reference.md#folder-structure).

Versie 2.x, moet u een taal voor uw project selecteren wanneer deze is geïnitialiseerd en alle functies toegevoegd de standaardsjablonen taal gebruiken. In versie 1.x, u de taal opgeven telkens wanneer u een functie.

Voer de volgende opdracht om het project en lokale Git-opslagplaats te maken in het terminalvenster of vanaf een opdrachtprompt:

```bash
func init MyFunctionProj
```

In versie 2.x, wanneer u de opdracht uitvoert moet u een runtime die voor uw project. Als u van plan bent voor het ontwikkelen van JavaScript-functies, kiest u **knooppunt**:

```output
Select a worker runtime:
dotnet
node
```

Omhoog/omlaag pijltoetsen een taal kiezen, druk op Enter. De uitvoer lijkt op het volgende voorbeeld voor een JavaScript-project:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

Gebruik voor het maken van het project zonder een lokale Git-opslagplaats de `--no-source-control [-n]` optie.

## <a name="register-extensions"></a>Extensies registreren

In versie 2.x van de Azure Functions-runtime, hebt u expliciet de binding-uitbreidingen (bindingstypen) die u in de functie-app gebruikt te registreren.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Zie voor meer informatie [Azure Functions triggers en bindingen concepten](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Lokale instellingenbestand

Het bestand local.settings.json slaat app-instellingen, verbindingsreeksen en instellingen voor Azure Functions Core hulpprogramma's. Deze heeft de volgende structuur:

```json
{
  "IsEncrypted": false,
  "Values": {
    "AzureWebJobsStorage": "<connection-string>",
    "AzureWebJobsDashboard": "<connection-string>",
    "MyBindingConnection": "<binding-connection-string>"
  },
  "Host": {
    "LocalHttpPort": 7071,
    "CORS": "*"
  },
  "ConnectionStrings": {
    "SQLConnectionString": "Value"
  }
}
```

| Instelling      | Beschrijving                            |
| ------------ | -------------------------------------- |
| **IsEncrypted** | Als de waarde **true**, alle waarden zijn versleuteld met een sleutel van de lokale computer. Gebruikt met `func settings` opdrachten. Standaardwaarde is **false**. |
| **Waarden** | Verzameling instellingen van toepassingen en verbindingsreeksen gebruikt bij lokale uitvoering. Deze waarden overeenkomen met app-instellingen in de functie-app in Azure, zoals **AzureWebJobsStorage** en **AzureWebJobsDashboard**. Veel triggers en bindingen hebben een eigenschap die naar een verbinding tekenreeks app-instelling, zoals verwijst **verbinding** voor de [Blob storage trigger](functions-bindings-storage-blob.md#trigger---configuration). Voor dergelijke eigenschappen, moet u een toepassingsinstelling gedefinieerd in de **waarden** matrix. <br/>**AzureWebJobsStorage** is een vereiste app-instelling voor triggers dan HTTP. Wanneer u hebt de [Azure-opslagemulator](../storage/common/storage-use-emulator.md) lokaal is geïnstalleerd, kunt u instellen **AzureWebJobsStorage** naar `UseDevelopmentStorage=true` en kernonderdelen maakt gebruik van de emulator. Dit is nuttig tijdens de ontwikkeling, maar u moet testen met een werkelijke opslagverbinding vóór de implementatie. |
| **Host** | Instellingen in deze sectie aanpassen het hostproces van de functies bij lokale uitvoering. |
| **LocalHttpPort** | Hiermee stelt u de standaardpoort gebruikt bij het uitvoeren van de lokale host van de functies (`func host start` en `func run`). De `--port` opdrachtregeloptie heeft voorrang op deze waarde. |
| **CORS** | Definieert de oorsprongen toegestaan voor [cross-origin-resource delen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Oorsprongen zijn opgegeven als een door komma's gescheiden lijst zonder spaties. Het jokerteken (\*) wordt ondersteund, waarmee aanvragen van een oorsprong. |
| **ConnectionStrings** | Gebruik voor de verbindingsreeksen die wordt gebruikt door uw functiebindingen geen deze verzameling. Deze verzameling wordt alleen gebruikt door frameworks die u verbindingsreeksen van ophalen moeten de **ConnectionStrings** sectie van een configuratie-bestand, zoals [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Verbindingsreeksen in dit object zijn toegevoegd aan de omgeving van het providertype [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Items in deze verzameling worden niet gepubliceerd naar Azure met andere appinstellingen. U moet deze waarden te expliciet toevoegen de **verbindingsreeksen** sectie van de **toepassingsinstellingen** voor functie-app. |

In uw code kunnen worden gelezen met de functie app-instellingen waarden als omgevingsvariabelen. Zie voor meer informatie het gedeelte van de omgeving variabelen van de volgende onderwerpen taalspecifieke verwijzing:

+ [Vooraf gecompileerd voor C#](functions-dotnet-class-library.md#environment-variables)
+ [C# script (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Instellingen in het bestand local.settings.json worden alleen gebruikt door extra functies bij lokale uitvoering. Standaard worden deze instellingen niet gemigreerd automatisch wanneer het project wordt gepubliceerd naar Azure. Gebruik de `--publish-local-settings` overschakelen [bij het publiceren van](#publish) om ervoor te zorgen dat deze instellingen zijn toegevoegd aan de functie-app in Azure. De waarden in **ConnectionStrings** nooit worden gepubliceerd.

Als er geen geldige opslagverbindingsreeks is ingesteld voor **AzureWebJobsStorage** en de emulator niet wordt gebruikt, wordt het volgende foutbericht weergegeven:  

>Ontbrekende waarde voor AzureWebJobsStorage in local.settings.json. Dit is vereist voor alle triggers dan HTTP. U kunt uitvoeren ' azure functionapp ophalen-app-instellingen van de func <functionAppName>' of een verbindingsreeks in local.settings.json opgeven.

### <a name="get-your-storage-connection-strings"></a>Uw storage-verbindingsreeksen ophalen

Zelfs wanneer u de opslagemulator gebruikt voor ontwikkeling, is het raadzaam om te testen met een werkelijke opslagverbinding. Ervan uitgaande dat u al hebt [gemaakt van een opslagaccount](../storage/common/storage-create-storage-account.md), krijgt u een geldige opslagverbindingsreeks in een van de volgende manieren:

+ Van de [Azure Portal]. Navigeer naar uw opslagaccount, selecteer **toegangssleutels** in **instellingen**, kopieert u een van de **verbindingsreeks** waarden.

  ![Kopieer de verbindingsreeks vanuit Azure-portal](./media/functions-run-local/copy-storage-connection-portal.png)

+ Gebruik [Azure Opslagverkenner](http://storageexplorer.com/) verbinding maken met uw Azure-account. In de **Explorer**, vouw uw abonnement, selecteer uw storage-account en kopieer de primaire of secundaire verbindingsreeks. 

  ![Kopieer de verbindingsreeks vanuit Opslagverkenner](./media/functions-run-local/storage-explorer.png)

+ Core-hulpprogramma's gebruiken voor het downloaden van de verbindingsreeks van Azure met een van de volgende opdrachten:

    + Alle instellingen voor downloaden van een bestaande functie-app:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + De verbindingsreeks ophalen voor een specifieke opslagaccount:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    Als u niet bent al aangemeld naar Azure, wordt u gevraagd om dit te doen.

## <a name="create-func"></a>Een functie maken

Voer de volgende opdracht voor het maken van een functie:

```bash
func new
```

In versie 2.x tijdens het uitvoeren van `func new` u wordt gevraagd een sjabloon te kiezen in de standaardtaal van uw app functie en u wordt ook gevraagd een naam voor de functie kiezen. In versie 1.x, u wordt ook gevraagd de taal te kiezen.

```output
Select a language: Select a template:
Blob trigger
Cosmos DB trigger
Event Grid trigger
HTTP trigger
Queue trigger
SendGrid
Service Bus Queue trigger
Service Bus Topic trigger
Timer trigger
```

De functiecode wordt gegenereerd in een submap met de naam van de opgegeven functie, zoals u in de volgende uitvoer van de wachtrij-trigger ziet:

```output
Select a language: Select a template: Queue trigger
Function name: [QueueTriggerJS] MyQueueTrigger
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\index.js
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\readme.md
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\sample.dat
Writing C:\myfunctions\myMyFunctionProj\MyQueueTrigger\function.json
```

U kunt ook deze opties opgeven in de opdracht met de volgende argumenten:

| Argument     | Beschrijving                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| De sjabloon programmeertaal, zoals C#, F # of JavaScript. Deze optie is vereist in versie 1.x. In versie 2.x geen gebruik deze optie of kies de standaardtaal van uw project. |
| **`--template -t`** | De sjabloonnaam kan een van de waarden zijn:<br/><ul><li>`Blob trigger`</li><li>`Cosmos DB trigger`</li><li>`Event Grid trigger`</li><li>`HTTP trigger`</li><li>`Queue trigger`</li><li>`SendGrid`</li><li>`Service Bus Queue trigger`</li><li>`Service Bus Topic trigger`</li><li>`Timer trigger`</li></ul> |
| **`--name -n`** | De naam van de functie. |

Bijvoorbeeld: als u wilt een JavaScript-HTTP-trigger maken in één opdracht, uitvoeren:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Een wachtrij geactiveerd om functie te maken in één opdracht, uitvoeren:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>Functies lokaal uitvoeren

Uit de functies host een project functies uit te voeren. De host wordt ingeschakeld voor alle functies in het project triggers:

```bash
func host start
```

`func host start` ondersteunt de volgende opties:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | De lokale poort te luisteren op. Standaardwaarde: 7071. |
| **`--debug <type>`** | Start de host met de poort voor foutopsporing openen zodat u kunt koppelen aan de **func.exe** verwerken van [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) of [Visual Studio 2017](functions-dotnet-class-library.md). De *\<type\>* opties zijn `VSCode` en `VS`.  |
| **`--cors`** | Een door komma's gescheiden lijst van CORS-oorsprong, zonder spaties. |
| **`--nodeDebugPort -n`** | De poort voor het foutopsporingsprogramma knooppunt moet worden gebruikt. Standaard: Een waarde van launch.json of 5858. |
| **`--debugLevel -d`** | Het traceerniveau van de console (uit verbose,-info, waarschuwing of fout). Standaard: Info.|
| **`--timeout -t`** | De time-out voor de host functies om te starten, in seconden. Standaardwaarde: 20 seconden.|
| **`--useHttps`** | Binden aan `https://localhost:{port}` in plaats van naar `http://localhost:{port}`. Deze optie maakt standaard een vertrouwd certificaat op uw computer.|
| **`--pause-on-error`** | Onderbreken om extra gegevens voordat het proces wordt afgesloten. Gebruikt bij het starten van de kernonderdelen van Visual Studio of VS-Code.|

Wanneer de host functies wordt gestart, levert dit de URL van de HTTP-geactiveerde functies:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="passing-test-data-to-a-function"></a>De testgegevens doorgeven aan een functie

Voor het testen van uw functies lokaal u [starten van de host functies](#start) en roept u eindpunten op de lokale server met behulp van HTTP-aanvragen. Het eindpunt dat u aanroept, is afhankelijk van het type van de functie.

>[!NOTE]  
> De cURL voorbeelden in dit onderwerp gebruiken voor het HTTP-aanvragen verzenden vanuit de terminal of vanaf een opdrachtprompt. HTTP-aanvragen te verzenden naar de lokale server kunt u een hulpprogramma van uw keuze. Het hulpprogramma cURL is standaard beschikbaar in de Linux-gebaseerde systemen. In Windows, moet u eerst downloaden en installeren de [cURL hulpprogramma](https://curl.haxx.se/).

Zie voor meer algemene informatie over het testen van functies [strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>HTTP- en webhook geactiveerde functies

U kunt de volgende aanroepen eindpunt voor het lokaal uitvoeren HTTP- en webhook functies geactiveerd:

    http://localhost:{port}/api/{function_name}

Controleer of u de naam van dezelfde server en de poort die de functies host luistert op. U ziet in de uitvoer gegenereerd bij het starten van de functie-host. U kunt deze URL met een HTTP-methode ondersteund door de trigger aanroepen. 

De volgende cURL-opdracht triggers de `MyHttpTrigger` Quick Start-functie van een aanvraag voor ophalen met de _naam_ parameter doorgegeven in de queryreeks. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
Het volgende voorbeeld wordt dezelfde functie aangeroepen vanuit een POST-aanvraag doorgeven _naam_ in de aanvraagtekst:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

U kunt aanvragen ophalen vanuit een browser het doorgeven van gegevens in de queryreeks. Voor alle andere HTTP-methoden, moet u cURL, Fiddler, Postman of een vergelijkbaar testen HTTP-hulpprogramma gebruiken.  

#### <a name="non-http-triggered-functions"></a>Niet-HTTP-geactiveerde functies

Voor alle soorten functies dan HTTP-triggers en webhooks, kunt u uw functies testen lokaal door het aanroepen van een beheer-eindpunt. De functie voor het aanroepen van dit eindpunt met een HTTP POST-aanvraag op de lokale server wordt geactiveerd. U kunt eventueel testgegevens doorgeven aan de uitvoering in de hoofdtekst van de POST-aanvraag. Deze functionaliteit is vergelijkbaar met de **Test** tabblad in de Azure-portal.  

U aanroepen het volgende eindpunt van de beheerder om te activeren, niet-HTTP-functies:

    http://localhost:{port}/admin/functions/{function_name}

Testgegevens doorgeven aan het eindpunt van de beheerder van een functie, moet u de gegevens in de hoofdtekst van een aanvraagbericht POST opgeven. De berichttekst is vereist voor de volgende JSON-indeling hebben:

```JSON
{
    "input": "<trigger_input>"
}
````

De `<trigger_input>` waarde bevat gegevens in een indeling die werd verwacht door de functie. De volgende cURL-voorbeeld is een POST opgegeven naar een `QueueTriggerJS` functie. In dit geval wordt is de invoer een tekenreeks die gelijk is aan het bericht dat naar verwachting worden gevonden in de wachtrij.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Met behulp van de `func run` opdracht in versie 1.x

>[!IMPORTANT]  
> De `func run` opdracht wordt niet ondersteund in versie 2.x van de hulpprogramma's. Zie voor meer informatie het onderwerp [hoe gericht op Azure Functions-runtime-versies](set-runtime-version.md).

U kunt ook een functie aanroepen via `func run <FunctionName>` en invoergegevens bieden voor de functie. Deze opdracht is vergelijkbaar met die voor een functie met de **Test** tabblad in de Azure-portal. 

`func run` ondersteunt de volgende opties:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Inline-inhoud. |
| **`--debug -d`** | Een foutopsporingsprogramma koppelen aan het hostproces voordat u de functie uitvoert.|
| **`--timeout -t`** | Tijd (in seconden) wordt gewacht totdat de lokale host van functies gereed is.|
| **`--file -f`** | De bestandsnaam om te gebruiken als de inhoud.|
| **`--no-interactive`** | Wordt niet gevraagd om invoer. Dit is handig voor automatiseringsscenario's.|

Voer bijvoorbeeld de volgende opdracht om een HTTP-geactiveerde functie aanroepen en hoofdinhoud doorgeven:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Lokaal naar logboekbestanden weergeven

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Publiceren naar Azure

Voor het publiceren van een project functies in een functie-app in Azure, gebruiken de `publish` opdracht:

```bash
func azure functionapp publish <FunctionAppName>
```

U kunt de volgende opties gebruiken:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publicatie-instellingen in local.settings.json naar Azure, waarin wordt gevraagd om te overschrijven als de instelling bestaat al. Als u de opslagemulator gebruikt, wijzigt u de app-instelling op een [werkelijke opslagverbinding](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | Moet worden gebruikt met `-i`. Overschrijft AppSettings in Azure met lokale waarde als deze verschilt. De standaardwaarde is vragen.|

Met deze opdracht publiceert naar een bestaande functie-app in Azure. Een fout optreedt wanneer de `<FunctionAppName>` bestaat niet in uw abonnement. Zie voor informatie over het maken van een functie-app vanuit de opdrachtprompt of terminalvenster met de Azure CLI, [maken van een functie-App voor uitvoering zonder server](./scripts/functions-cli-create-serverless.md).

De `publish` opdracht wordt de inhoud van de projectmap functies geüpload. Als u bestanden lokaal, verwijdert de `publish` opdracht ze niet verwijdert uit Azure. U kunt bestanden in Azure verwijderen met behulp van de [Kudu hulpprogramma](functions-how-to-use-azure-function-app-settings.md#kudu) in de [Azure Portal].  

>[!IMPORTANT]  
> Wanneer u een functie-app in Azure maakt, wordt versie 1.x van de functie runtime standaard. Om te maken van de functie app-gebruik versie 2.x van de runtime de toepassingsinstelling toevoegen `FUNCTIONS_EXTENSION_VERSION=beta`.  
Gebruik de volgende code in de Azure CLI deze instelling toevoegen aan uw app in functie:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>Volgende stappen

Azure Functions Core Tools [open source en wordt gehost op GitHub](https://github.com/azure/azure-functions-cli).  
Fout of het onderdeel aanvragen [opent u een probleem met de GitHub](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure Functions kernonderdelen]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure Portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
