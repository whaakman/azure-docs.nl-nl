---
title: Werken met Azure Functions Core Tools | Microsoft Docs
description: Informatie over het programmeren en testen van Azure functions vanaf de opdrachtprompt of terminal op uw lokale computer voordat u ze op Azure Functions uitvoeren.
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
ms.openlocfilehash: 44485d04dad3ff9dfc6067a3737989c5d273541f
ms.sourcegitcommit: 7827d434ae8e904af9b573fb7c4f4799137f9d9b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/18/2018
ms.locfileid: "39116177"
---
# <a name="work-with-azure-functions-core-tools"></a>Werken met Azure Functions Core Tools

Azure Functions Core Tools kunt u ontwikkelen en testen van uw functies op uw lokale computer vanuit de opdrachtprompt of terminal. Uw lokale functies kunnen verbinding maken met live Azure-services, en u kunt fouten opsporen in uw functies op uw lokale computer met behulp van de volledige Functions-runtime. U kunt ook een functie-app implementeren naar uw Azure-abonnement.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

## <a name="core-tools-versions"></a>Core-versies van de hulpprogramma 's

Er zijn twee versies van Azure Functions Core Tools. De versie die u gebruikt, is afhankelijk van uw lokale ontwikkelomgeving, de keuze van taal en niveau van ondersteuning vereist:

+ [Versie 1.x](#v1): biedt ondersteuning voor versie 1.x van de runtime, die algemeen beschikbaar (GA). Deze versie van de hulpprogramma's wordt alleen ondersteund op Windows-computers en wordt geïnstalleerd vanuit een [npm-pakket](https://docs.npmjs.com/getting-started/what-is-npm). Met deze versie kunt u functies maken in de experimentele talen die officieel niet worden ondersteund. Zie voor meer informatie, [ondersteunde talen in Azure Functions](supported-languages.md)

+ [Versie 2.x](#v2): biedt ondersteuning voor versie 2.x van de runtime. Deze versie biedt ondersteuning voor [Windows](#windows-npm), [macOS](#brew), en [Linux](#linux). Maakt gebruik van platform-specifieke pakketmanagers of npm voor installatie. Als de runtime 2.x is deze versie van de essentiële hulpprogramma momenteel in preview.

Tenzij anders vermeld, de voorbeelden in dit artikel zijn voor versie 2.x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure Functions Core Tools] omvat een versie van de dezelfde runtime die wordt gebruikt door Azure Functions-runtime die u op uw lokale ontwikkelcomputer uitvoeren kunt. Het biedt ook opdrachten voor het maken van functies, verbinding maken met Azure, en functie projecten kunt implementeren.

### <a name="v1"></a>Versie 1.x

De Functions-runtime 1.x maakt gebruik van de oorspronkelijke versie van de hulpprogramma's. Deze versie .NET Framework (4.7.1) wordt gebruikt en wordt alleen ondersteund op Windows-computers. Voordat u de versie 1.x-hulpprogramma's installeert, moet u [NodeJS installeren](https://docs.npmjs.com/getting-started/installing-node), waaronder npm.

Gebruik de volgende opdracht voor het installeren van de versie 1.x-hulpprogramma's:

```bash
npm install -g azure-functions-core-tools
```

### <a name="v2"></a>Versie 2.x

>[!NOTE]
> Azure Functions-runtime 2.0 is beschikbaar als preview en momenteel niet alle functies van Azure Functions worden ondersteund. Zie voor meer informatie, [versies van Azure Functions](functions-versions.md) 

Versie 2.x van de hulpprogramma's maakt gebruik van de Azure Functions-runtime 2.x die is gebaseerd op .NET Core. Deze versie wordt ondersteund op alle platforms .NET Core 2.x ondersteunt, met inbegrip van [Windows](#windows-npm), [macOS](#brew), en [Linux](#linux).

#### <a name="windows-npm"></a>Windows

De volgende stappen gebruikt npm Core Tools installeren op Windows. U kunt ook [Chocolatey](https://chocolatey.org/). Zie voor meer informatie de [Core Tools Leesmij](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Installeer [.NET Core, 2.1 voor Windows](https://www.microsoft.com/net/download/windows).

2. Installeer [Node.js], waaronder npm. Voor versie 2.x van de hulpprogramma's, alleen Node.js 8.5 en latere versies worden ondersteund.

3. Installeer het pakket Core-hulpprogramma's:

    ```bash
    npm install -g azure-functions-core-tools@core
    ```

#### <a name="brew"></a>MacOS met Homebrew

De volgende stappen gebruikt Homebrew de essentiële hulpprogramma installeren op macOS.

1. Installeer [2.1 van de .NET Core voor macOS](https://www.microsoft.com/net/download/macos).

2. Installeer [Homebrew](https://brew.sh/), als deze nog niet geïnstalleerd.

3. Installeer het pakket Core-hulpprogramma's:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools 
    ```

#### <a name="linux"></a> Linux (Ubuntu/Debian) met APT

De volgende stappen [APT](https://wiki.debian.org/Apt) Core Tools installeren op uw Ubuntu Debian/Linux-distributie. Zie voor andere Linux-distributies, de [Core Tools Leesmij](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. Installeer [.NET Core, 2.1 voor Linux](https://www.microsoft.com/net/download/linux).

2. De sleutel voor de Microsoft-product als vertrouwd registreren:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

3. Controleer of dat de Ubuntu-server een van de juiste versies van de onderstaande tabel wordt uitgevoerd. Als u wilt de apt-bron toevoegen, voert u de volgende uit:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux-distributie | Versie |
    | --------------- | ----------- |
    | Ubuntu 17.10    | `artful`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux voorkomt 18    | `xenial`  |

4. Installeer het pakket Core-hulpprogramma's:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

## <a name="create-a-local-functions-project"></a>Een lokale Functions-project maken

Een map functions-project bevat de bestanden [host.json](functions-host-json.md) en [local.settings.json](#local-settings-file), langs de submappen die de code voor afzonderlijke functies bevatten. Deze map is het equivalent van een functie-app in Azure. Zie voor meer informatie over de structuur van de functies, de [handleiding voor ontwikkelaars van Azure Functions voor](functions-reference.md#folder-structure).

Versie 2.x, moet u een taal voor uw project selecteren wanneer deze wordt geïnitialiseerd en alle functies gebruiken standaard taalsjablonen toegevoegd. In versie 1.x, u de taal opgeven telkens wanneer u een functie maken.

In het terminalvenster of vanaf een opdrachtprompt, voer de volgende opdracht om het project en lokale Git-opslagplaats te maken:

```bash
func init MyFunctionProj
```

In versie 2.x, wanneer u de opdracht uitvoert moet u een runtime voor uw project. Als u van plan bent voor het ontwikkelen van JavaScript-functies, kiest u **knooppunt**:

```output
Select a worker runtime:
dotnet
node
```

Omhoog/omlaag pijltoetsen een taal kiezen, druk op Enter. De uitvoer ziet eruit zoals in het volgende voorbeeld voor een JavaScript-project:

```output
Select a worker runtime: node
Writing .gitignore
Writing host.json
Writing local.settings.json
Writing C:\myfunctions\myMyFunctionProj\.vscode\extensions.json
Initialized empty Git repository in C:/myfunctions/myMyFunctionProj/.git/
```

Gebruik voor het maken van het project zonder een lokale Git-opslagplaats het `--no-source-control [-n]` optie.

## <a name="register-extensions"></a>Extensies registreren

In versie 2.x van de Azure Functions-runtime die u moet expliciet de binding-extensies (bindingstypen) die u in uw functie-app gebruikt te registreren.

[!INCLUDE [Register extensions](../../includes/functions-core-tools-install-extension.md)]

Zie voor meer informatie, [Azure Functions-triggers en bindingen concepten](functions-triggers-bindings.md#register-binding-extensions).

## <a name="local-settings-file"></a>Voor lokale instellingenbestand

Het bestand local.settings.json slaat de app-instellingen, verbindingsreeksen en instellingen voor Azure Functions Core Tools. Het heeft de volgende structuur:

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
| **IsEncrypted** | Als de waarde **waar**, alle waarden zijn versleuteld met behulp van de sleutel van een lokale computer. Gebruikt in combinatie met `func settings` opdrachten. Standaardwaarde is **false**. |
| **Waarden** | Verzameling van toepassings- en verbindingsreeksen gebruikt bij het lokaal worden uitgevoerd. Deze waarden overeenkomen met app-instellingen in uw functie-app in Azure, zoals **AzureWebJobsStorage** en **AzureWebJobsDashboard**. Veel triggers en bindingen hebben een eigenschap die naar een appinstelling voor de verbindingsreeks, zoals verwijst **verbinding** voor de [Blob storage-trigger](functions-bindings-storage-blob.md#trigger---configuration). Voor deze eigenschappen, moet u een toepassingsinstelling gedefinieerd in de **waarden** matrix. <br/>**AzureWebJobsStorage** is een vereiste app-instelling voor triggers dan HTTP. Wanneer u hebt de [Azure-opslagemulator](../storage/common/storage-use-emulator.md) lokaal is geïnstalleerd, kunt u instellen **AzureWebJobsStorage** naar `UseDevelopmentStorage=true` en Core Tools maakt gebruik van de emulator. Dit is handig tijdens het ontwikkelen, maar u moet testen met een verbinding van de werkelijke opslag vóór de implementatie. |
| **Host** | Instellingen in deze sectie aanpassen hostproces van de functies bij lokale uitvoering. |
| **LocalHttpPort** | Hiermee stelt u de standaardpoort gebruikt bij het uitvoeren van de lokale host van de functies (`func host start` en `func run`). De `--port` opdrachtregeloptie te gebruiken heeft voorrang op deze waarde. |
| **CORS** | Definieert de oorsprongen toegestaan voor [cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Oorsprongen zijn opgegeven als een door komma's gescheiden lijst zonder spaties. Het jokerteken (\*) wordt ondersteund, waarmee aanvragen van een oorsprong. |
| **ConnectionStrings** | Gebruik deze verzameling niet voor de verbindingsreeksen die worden gebruikt door uw functiebindingen. Deze verzameling wordt alleen gebruikt door frameworks die u moeten ophalen verbindingsreeksen uit de **ConnectionStrings** gedeelte van een configuratie-bestand, zoals [Entity Framework](https://msdn.microsoft.com/library/aa937723(v=vs.113).aspx). Verbindingsreeksen in dit object zijn toegevoegd aan de omgeving van het providertype [System.Data.SqlClient](https://msdn.microsoft.com/library/system.data.sqlclient(v=vs.110).aspx). Items in deze verzameling zijn niet gepubliceerd naar Azure met andere app-instellingen. U moet expliciet toevoegen met deze waarden naar de **verbindingsreeksen** sectie van de **toepassingsinstellingen** voor uw functie-app. |

De waarden voor de functie-app-instellingen kunnen ook worden gelezen in uw code als omgevingsvariabelen. Zie de sectie van de variabelen voor de omgeving van de volgende taalspecifieke referentie-onderwerpen voor meer informatie:

+ [C# vooraf geschreven](functions-dotnet-class-library.md#environment-variables)
+ [C# script (.csx)](functions-reference-csharp.md#environment-variables)
+ [F#](functions-reference-fsharp.md#environment-variables)
+ [Java](functions-reference-java.md#environment-variables) 
+ [JavaScript](functions-reference-node.md#environment-variables)

Instellingen in het bestand local.settings.json worden alleen gebruikt door de Functions-hulpprogramma's bij lokale uitvoering. Standaard worden deze instellingen niet gemigreerd automatisch wanneer het project wordt gepubliceerd naar Azure. Gebruik de `--publish-local-settings` overschakelen [bij het publiceren van](#publish) om ervoor te zorgen deze instellingen worden toegevoegd aan de functie-app in Azure. De waarden in **ConnectionStrings** nooit worden gepubliceerd.

Wanneer er is geen geldige verbindingsreeks is ingesteld voor **AzureWebJobsStorage** en de emulator niet wordt gebruikt, wordt het volgende foutbericht weergegeven:  

>Ontbrekende waarde voor AzureWebJobsStorage in local.settings.json. Dit is vereist voor alle triggers dan HTTP. U kunt uitvoeren ' azure functionapp ophalen-app-instellingen van de func <functionAppName>' of een verbindingsreeks in local.settings.json opgeven.

### <a name="get-your-storage-connection-strings"></a>Uw storage-verbindingsreeksen ophalen

Zelfs wanneer u de opslagemulator gebruikt voor ontwikkeling, kunt u om te testen met een werkelijke opslag-verbinding. Ervan uitgaande dat u al hebt [een storage-account gemaakt](../storage/common/storage-create-storage-account.md), krijgt u een geldige verbindingsreeks in een van de volgende manieren:

+ Uit de [Azure Portal]. Navigeer naar uw opslagaccount, selecteer **toegangssleutels** in **instellingen**, en kopieer een van de **Connection string** waarden.

  ![Verbindingsreeks kopiëren vanuit Azure portal](./media/functions-run-local/copy-storage-connection-portal.png)

+ Gebruik [Azure Storage Explorer](http://storageexplorer.com/) verbinding maken met uw Azure-account. In de **Explorer**, vouw uw abonnement, selecteer uw opslagaccount en kopieer de primaire of secundaire verbindingsreeks. 

  ![Kopieer de verbindingsreeks van Storage Explorer](./media/functions-run-local/storage-explorer.png)

+ Core-hulpprogramma's gebruiken voor het downloaden van de verbindingsreeks van Azure met een van de volgende opdrachten:

    + Alle instellingen voor downloaden van een bestaande functie-app:

    ```bash
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    + De verbindingsreeks ophalen voor een specifiek opslagaccount:

    ```bash
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    
    Wanneer u bent niet al aangemeld naar Azure, wordt u gevraagd om dit te doen.

## <a name="create-func"></a>Een functie maken

Voer de volgende opdracht voor het maken van een functie:

```bash
func new
```

In versie 2.x, tijdens het uitvoeren van `func new` u wordt gevraagd om te kiezen van een sjabloon in de standaardtaal van de functie-app wordt u gevraagd om een naam voor uw functie te kiezen. In versie 1.x, u ook gevraagd om de taal te kiezen.

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

U kunt deze opties ook opgeven in de opdracht met behulp van de volgende argumenten:

| Argument     | Beschrijving                            |
| ------------------------------------------ | -------------------------------------- |
| **`--language -l`**| De sjabloon programmeertaal zoals C#, F # of JavaScript. Deze optie is vereist in versie 1.x. In versie 2.x gebruikt, geen gebruik deze optie en kiest u de standaardtaal van uw project. |
| **`--template -t`** | De sjabloonnaam, dit kan een van de waarden:<br/><ul><li>`Blob trigger`</li><li>`Cosmos DB trigger`</li><li>`Event Grid trigger`</li><li>`HTTP trigger`</li><li>`Queue trigger`</li><li>`SendGrid`</li><li>`Service Bus Queue trigger`</li><li>`Service Bus Topic trigger`</li><li>`Timer trigger`</li></ul> |
| **`--name -n`** | De naam van de functie. |

Voer bijvoorbeeld de volgende opdracht voor het maken van een JavaScript-HTTP-trigger in één opdracht:

```bash
func new --template "Http Trigger" --name MyHttpTrigger
```

Voer het volgende als u voor het maken van een wachtrij geactiveerde functie in één opdracht:

```bash
func new --template "Queue Trigger" --name QueueTriggerJS
```

## <a name="start"></a>Functies lokaal uitvoeren

Om uit te voeren een Functions-project, de host van de functies worden uitgevoerd. De host kunt triggers voor alle functies in het project:

```bash
func host start
```

`func host start` ondersteunt de volgende opties:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | De lokale poort voor luisteren. Standaardwaarde: 7071. |
| **`--debug <type>`** | Start de host met de poort voor foutopsporing openen zodat u kunt koppelen aan de **func.exe** verwerken van [Visual Studio Code](https://code.visualstudio.com/tutorials/functions-extension/getting-started) of [Visual Studio 2017](functions-dotnet-class-library.md). De *\<type\>* opties zijn `VSCode` en `VS`.  |
| **`--cors`** | Een door komma's gescheiden lijst met CORS-oorsprong, zonder spaties. |
| **`--nodeDebugPort -n`** | De poort voor het foutopsporingsprogramma knooppunt te gebruiken. Standaard: Een waarde van launch.json of 5858. |
| **`--debugLevel -d`** | Het traceerniveau console (uit, uitgebreid, info, waarschuwing of fout). Standaard: Info.|
| **`--timeout -t`** | De time-out voor de host functies om te starten, in seconden. Standaard: 20 seconden.|
| **`--useHttps`** | Verbinding maken met `https://localhost:{port}` in plaats van naar `http://localhost:{port}`. Deze optie maakt standaard een vertrouwd certificaat op uw computer.|
| **`--pause-on-error`** | Aanvullende invoer wacht voordat het proces wordt afgesloten. Bij het starten van Core Tools van Visual Studio of Visual Studio Code gebruikt.|

Wanneer de Functions-host wordt gestart, levert dit de URL van de HTTP-geactiveerde functies:

```bash
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="passing-test-data-to-a-function"></a>Testgegevens doorgeven aan een functie

Voor het testen van uw functies lokaal, u [starten van de host functies](#start) en -eindpunten worden aangeroepen op de lokale server met behulp van HTTP-aanvragen. Het eindpunt dat u aanroept, is afhankelijk van het type van de functie.

>[!NOTE]  
> Het hulpprogramma cURL voorbeelden in dit onderwerp gebruiken voor het verzenden van HTTP-aanvragen vanuit de terminal of opdrachtprompt. U kunt een hulpprogramma van uw keuze gebruiken voor het verzenden van HTTP-aanvragen naar de lokale server. Het hulpprogramma cURL is standaard beschikbaar in Linux-systemen. Op Windows, moet u eerst downloaden en installeren van de [hulpprogramma cURL](https://curl.haxx.se/).

Zie voor meer algemene informatie over het testen van functies, [strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md).

#### <a name="http-and-webhook-triggered-functions"></a>HTTP- en webhook geactiveerde functies

U kunt de volgende aanroepen eindpunt om uit te voeren lokaal HTTP- en webhook geactiveerde functies:

    http://localhost:{port}/api/{function_name}

Zorg ervoor dat u de dezelfde servernaam en poort op die de Functions-host luistert. U ziet in de uitvoer gegenereerd bij het starten van de functie-host. U kunt deze URL met behulp van een HTTP-methode ondersteund door de trigger aanroepen. 

De volgende cURL-opdracht triggers de `MyHttpTrigger` Quick Start-functie van een GET-aanvraag met de _naam_ parameter doorgegeven in de querytekenreeks. 

```bash
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
Het volgende voorbeeld wordt dezelfde functie aangeroepen vanuit een POST-aanvraag doorgeven _naam_ in de aanvraagtekst:

```bash
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

U kunt aanvragen ophalen vanuit een browser voor het doorgeven van gegevens in de querytekenreeks. Voor alle andere HTTP-methoden, moet u cURL, Fiddler, Postman of een vergelijkbaar hulpprogramma van de HTTP-tests gebruiken.  

#### <a name="non-http-triggered-functions"></a>Niet-HTTP-geactiveerde functies

Voor alle soorten functies dan HTTP-triggers en webhooks, kunt u uw functies lokaal door het aanroepen van een beheer-eindpunt testen. Het aanroepen van dit eindpunt met een HTTP POST-aanvraag voor de lokale server, wordt de functie geactiveerd. U kunt eventueel testgegevens doorgeven aan de uitvoering in de hoofdtekst van de POST-aanvraag. Deze functionaliteit is vergelijkbaar met de **Test** tabblad in de Azure-portal.  

U aanroepen het volgende administrator-eindpunt voor het activeren van niet-HTTP-functies:

    http://localhost:{port}/admin/functions/{function_name}

Testgegevens doorgeven aan het eindpunt van de beheerder van een functie, dient u de gegevens in de hoofdtekst van een bericht POST-aanvraag. De hoofdtekst van bericht is vereist om de volgende JSON-indeling:

```JSON
{
    "input": "<trigger_input>"
}
````

De `<trigger_input>` waarde bevat de gegevens in een indeling die door de functie wordt verwacht. De volgende cURL-voorbeeld wordt een bericht naar een `QueueTriggerJS` functie. In dit geval is de invoer een tekenreeks die gelijk is aan het bericht dat wordt verwacht in de wachtrij moet worden gezocht.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Met behulp van de `func run` opdracht in versie 1.x

>[!IMPORTANT]  
> De `func run` opdracht wordt niet ondersteund in versie 2.x van de hulpprogramma's. Zie voor meer informatie het onderwerp [hoe gericht op versies van Azure Functions-runtime](set-runtime-version.md).

U kunt ook een functie aanroepen via `func run <FunctionName>` en geef de invoergegevens voor de functie. Met deze opdracht is vergelijkbaar met het uitvoeren van een functie met de **Test** tabblad in de Azure-portal. 

`func run` ondersteunt de volgende opties:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Inline-inhoud. |
| **`--debug -d`** | Voeg een debugger toe aan het hostproces voordat u de functie uitvoert.|
| **`--timeout -t`** | Tijd (in seconden) wachten totdat de lokale functies host gereed is.|
| **`--file -f`** | De bestandsnaam om te gebruiken als de inhoud.|
| **`--no-interactive`** | Wordt niet gevraagd om invoer. Dit is handig voor scenario's voor automation.|

Bijvoorbeeld, als u wilt een HTTP-geactiveerde functie aanroepen en de hoofdtekst van de inhoud doorgeeft, voer de volgende opdracht:

```bash
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

### <a name="viewing-log-files-locally"></a>Logboekbestanden lokaal weergeven

[!INCLUDE [functions-local-logs-location](../../includes/functions-local-logs-location.md)]

## <a name="publish"></a>Publiceren naar Azure

Als u wilt publiceren een Functions-project aan een functie-app in Azure, gebruikt u de `publish` opdracht:

```bash
func azure functionapp publish <FunctionAppName>
```

U kunt de volgende opties gebruiken:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publicatie-instellingen in local.settings.json naar Azure, dat u wordt gevraagd om te overschrijven als de instelling bestaat al. Als u de opslagemulator gebruikt, wijzigt u de app-instelling op een [werkelijke opslagverbinding](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | Moet worden gebruikt met `-i`. Hiermee wordt AppSettings in Azure met lokale waarde overschreven als deze verschilt. De standaardwaarde is vragen.|

Met deze opdracht publiceert naar een bestaande functie-app in Azure. Een fout optreedt wanneer de `<FunctionAppName>` bestaat niet in uw abonnement. Zie voor meer informatie over het maken van een functie-app vanuit de opdrachtprompt of terminal-venster met de Azure CLI, [maken van een functie-App voor uitvoering zonder server](./scripts/functions-cli-create-serverless.md).

De `publish` opdracht wordt de inhoud van de projectmap functies geüpload. Als u bestanden lokaal, verwijdert de `publish` opdracht worden niet verwijderd van Azure. U kunt bestanden in Azure verwijderen met behulp van de [Kudu hulpprogramma](functions-how-to-use-azure-function-app-settings.md#kudu) in de [Azure Portal].  

>[!IMPORTANT]  
> Wanneer u een functie-app in Azure maakt, wordt versie 1.x van de runtime van de functies standaard. Om te maken van de functie app-gebruik versie 2.x van de runtime, de toepassingsinstelling toevoegen `FUNCTIONS_EXTENSION_VERSION=beta`.  
Gebruik de volgende Azure CLI-code voor deze instelling toevoegen aan uw functie-app:

```azurecli-interactive
az functionapp config appsettings set --name <function_app> \
--resource-group myResourceGroup \
--settings FUNCTIONS_EXTENSION_VERSION=beta   
```

## <a name="next-steps"></a>Volgende stappen

Azure Functions Core Tools is [open source en gehost op GitHub](https://github.com/azure/azure-functions-cli).  
Naar het bestand een bug of functie-aanvraag [opent u een GitHub-probleem](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure Portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
