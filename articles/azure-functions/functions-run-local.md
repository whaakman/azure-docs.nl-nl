---
title: Ontwikkelen en lokaal uitvoeren van Azure functions | Microsoft Docs
description: Ontdek hoe u de code en testen van Azure functions op uw lokale computer voordat u deze op Azure Functions uitvoeren.
services: functions
documentationcenter: na
author: ggailey777
manager: cfowler
editor: 
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: functions
ms.workload: na
ms.tgt_pltfrm: multiple
ms.devlang: multiple
ms.topic: article
ms.date: 10/12/2017
ms.author: glenga
ms.openlocfilehash: 3fd392a3f5b48d6b8d19af530c949d91cd461099
ms.sourcegitcommit: 9890483687a2b28860ec179f5fd0a292cdf11d22
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/24/2018
---
# <a name="code-and-test-azure-functions-locally"></a>Code en Azure Functions lokaal testen

Terwijl de [Azure-portal] biedt een volledige set hulpprogramma's voor het ontwikkelen en testen Azure Functions, veel ontwikkelaars de voorkeur aan een lokale ontwikkeling biedt. Azure Functions kunt eenvoudig uw favoriete code-editor en lokale ontwikkelingsprogramma's gebruiken om te ontwikkelen en testen van uw functies op uw lokale computer. Uw functies kunnen activeren van gebeurtenissen in Azure en u kunt fouten opsporen in uw C#- en JavaScript-functies op uw lokale computer. 

Als u een Azure-functies van Visual Studio C# ontwikkelaar, ook bent [kan worden geïntegreerd met Visual Studio 2017](functions-develop-vs.md).

>[!IMPORTANT]  
> Niet door elkaar op lokale ontwikkeling met portal-ontwikkeling in dezelfde functie-app. Wanneer u maken en publiceren van de functies van een lokaal project, moet u niet proberen te onderhouden of te wijzigen projectcode in de portal.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure Functions kernonderdelen] is een lokale versie van de Azure Functions-runtime die kunnen worden uitgevoerd op de lokale computer. Het is niet een emulator of simulator. Het is de dezelfde runtime bevoegdheden werkt in Azure. Er zijn twee versies van Azure Functions Core-hulpprogramma's voor versie 1.x van de runtime en één voor versie 2.x. Beide versies zijn beschikbaar als een [npm pakket](https://docs.npmjs.com/getting-started/what-is-npm).

>[!NOTE]  
> Voordat u eender welke versie installeert, moet u [installeren NodeJS](https://docs.npmjs.com/getting-started/installing-node), waaronder npm. Voor versie 2.x van de hulpprogramma's, alleen Node.js 8.5 en latere versies worden ondersteund. 

### <a name="version-1x-runtime"></a>Versie 1.x runtime

De runtime van Functions 1.x maakt gebruik van de oorspronkelijke versie van de hulpprogramma's. Deze versie van .NET Framework gebruikt en wordt alleen ondersteund op Windows-computers. Gebruik de volgende opdracht voor het installeren van de versie 1.x-hulpprogramma's:

```bash
npm install -g azure-functions-core-tools
```

### <a name="version-2x-runtime"></a>Versie 2.x runtime

Versie 2.x van de hulpprogramma's gebruikmaakt van de Azure Functions-runtime 2.x die is gebaseerd op .NET Core. Deze versie wordt ondersteund op alle platforms .NET Core 2.x ondersteunt. Deze versie gebruiken voor platformoverschrijdende ontwikkeling en wanneer de runtime van Functions 2.x is vereist. 

>[!IMPORTANT]   
> Voordat u Azure Functions Core Tools installeert [Installeer .NET Core 2.0](https://www.microsoft.com/net/core).  
>
> Azure Functions-runtime 2.0 is een Preview-versie en op dit moment niet alle functies van Azure Functions worden ondersteund. Zie voor meer informatie [Azure Functions-runtime 2.0 bekende problemen](https://github.com/Azure/azure-webjobs-sdk-script/wiki/Azure-Functions-runtime-2.0-known-issues) 

 Gebruik de volgende opdracht voor het installeren van de versie 2.0-hulpprogramma's:

```bash
npm install -g azure-functions-core-tools@core
```

Wanneer u installeert op Ubuntu gebruik `sudo`als volgt:

```bash
sudo npm install -g azure-functions-core-tools@core
```

Wanneer u installeert op Mac OS- en Linux, moet u wellicht omvatten de `unsafe-perm` markeren als volgt:

```bash
sudo npm install -g azure-functions-core-tools@core --unsafe-perm true
```

## <a name="run-azure-functions-core-tools"></a>Azure Functions Core hulpprogramma's uitvoeren
 
Azure Functions Core-hulpprogramma's worden de volgende opdracht aliassen toegevoegd:
* **func**
* **azfun**
* **azurefunctions**

Elk van deze aliassen kan worden gebruikt waar `func` wordt weergegeven in de voorbeelden.

```
func init MyFunctionProj
```

## <a name="create-a-local-functions-project"></a>Een lokale functies-project maken

Wanneer lokaal wordt uitgevoerd, een project functies is een map die de bestanden heeft [host.json](functions-host-json.md) en [local.settings.json](#local-settings-file). Deze map is het equivalent van een functie-app in Azure. Zie voor meer informatie over de structuur van de Azure Functions, de [handleiding voor ontwikkelaars voor Azure Functions voor](functions-reference.md#folder-structure).

Voer de volgende opdracht om het project en lokale Git-opslagplaats te maken in het terminalvenster of vanaf een opdrachtprompt:

```
func init MyFunctionProj
```

De uitvoer lijkt op het volgende voorbeeld:

```
Writing .gitignore
Writing host.json
Writing local.settings.json
Created launch.json
Initialized empty Git repository in D:/Code/Playground/MyFunctionProj/.git/
```

Gebruik voor het maken van het project zonder een lokale Git-opslagplaats de `--no-source-control [-n]` optie.

## <a name="local-settings-file"></a>Lokale instellingenbestand

Het bestand local.settings.json slaat app-instellingen, verbindingsreeksen en instellingen voor Azure Functions Core hulpprogramma's. Deze heeft de volgende structuur:

```json
{
  "IsEncrypted": false,   
  "Values": {
    "AzureWebJobsStorage": "<connection string>", 
    "AzureWebJobsDashboard": "<connection string>" 
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
| **Waarden** | Verzameling toepassingsinstellingen gebruikt bij lokale uitvoering. **AzureWebJobsStorage** en **AzureWebJobsDashboard** zijn voorbeelden; Zie voor een volledige lijst [app naslaginformatie](functions-app-settings.md).  |
| **Host** | Instellingen in deze sectie aanpassen het hostproces van de functies bij lokale uitvoering. | 
| **LocalHttpPort** | Hiermee stelt u de standaardpoort gebruikt bij het uitvoeren van de lokale host van de functies (`func host start` en `func run`). De `--port` opdrachtregeloptie heeft voorrang op deze waarde. |
| **CORS** | Definieert de oorsprongen toegestaan voor [cross-origin-resource delen (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing). Oorsprongen zijn opgegeven als een door komma's gescheiden lijst zonder spaties. Het jokerteken (\*) wordt ondersteund, waarmee aanvragen van een oorsprong. |
| **ConnectionStrings** | De tekenreeksen voor databaseverbindingen voor uw functies bevat. Verbindingsreeksen in dit object zijn toegevoegd aan de omgeving van het providertype **System.Data.SqlClient**.  | 

De meeste triggers en bindingen hebben een **verbinding** eigenschap die is toegewezen aan de naam van een omgeving variabele of een app-instelling. Voor elke verbindingseigenschap moet worden gedefinieerd in local.settings.json-bestand van app-instelling. 

Deze instellingen kunnen ook worden gelezen in uw code als omgevingsvariabelen. Gebruik in C#, [System.Environment.GetEnvironmentVariable](https://msdn.microsoft.com/library/system.environment.getenvironmentvariable(v=vs.110).aspx) of [ConfigurationManager.AppSettings](https://msdn.microsoft.com/library/system.configuration.configurationmanager.appsettings%28v=vs.110%29.aspx). Gebruik in JavaScript, `process.env`. Instellingen die zijn opgegeven als een systeemomgevingsvariabele hebben voorrang op de waarden in het bestand local.settings.json. 

Instellingen in het bestand local.settings.json worden alleen gebruikt door extra functies bij lokale uitvoering. Standaard worden deze instellingen niet gemigreerd automatisch wanneer het project wordt gepubliceerd naar Azure. Gebruik de `--publish-local-settings` overschakelen [bij het publiceren van](#publish) om ervoor te zorgen dat deze instellingen zijn toegevoegd aan de functie-app in Azure.

Als er geen geldige opslagverbindingsreeks is ingesteld voor **AzureWebJobsStorage**, wordt het volgende foutbericht weergegeven:  

>Ontbrekende waarde voor AzureWebJobsStorage in local.settings.json. Dit is vereist voor alle triggers dan HTTP. U kunt uitvoeren ' azure functionapp ophalen-app-instellingen van de func <functionAppName>' of een verbindingsreeks in local.settings.json opgeven.
  
[!INCLUDE [Note to not use local storage](../../includes/functions-local-settings-note.md)]

### <a name="configure-app-settings"></a>App-instellingen configureren

Als u wilt een waarde voor verbindingsreeksen instelt, kunt u een van de volgende opties doen:
* Geef de verbindingsreeks van [Azure Opslagverkenner](http://storageexplorer.com/).
* Gebruik een van de volgende opdrachten:

    ```
    func azure functionapp fetch-app-settings <FunctionAppName>
    ```
    ```
    func azure storage fetch-connection-string <StorageAccountName>
    ```
    Beide opdrachten moeten u eerst aanmelden bij Azure.

<a name="create-func"></a>
## <a name="create-a-function"></a>Een functie maken

Voer de volgende opdracht voor het maken van een functie:

```
func new
``` 
`func new`ondersteunt de volgende optionele argumenten:

| Argument     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--language -l`** | De sjabloon programmeertaal, zoals C#, F # of JavaScript. |
| **`--template -t`** | De naam van de sjabloon. |
| **`--name -n`** | De naam van de functie. |

Bijvoorbeeld: voor het maken van een JavaScript-HTTP-trigger uitvoeren:

```
func new --language JavaScript --template HttpTrigger --name MyHttpTrigger
```

Voer voor het maken van een functie wachtrij geactiveerd:

```
func new --language JavaScript --template QueueTrigger --name QueueTriggerJS
```
<a name="start"></a>
## <a name="run-functions-locally"></a>Functies lokaal uitvoeren

Uit de functies host een project functies uit te voeren. De host wordt ingeschakeld voor alle functies in het project triggers:

```
func host start
```

`func host start`ondersteunt de volgende opties:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
|**`--port -p`** | De lokale poort te luisteren op. Standaardwaarde: 7071. |
| **`--debug <type>`** | De opties zijn `VSCode` en `VS`. |
| **`--cors`** | Een door komma's gescheiden lijst van CORS-oorsprong, zonder spaties. |
| **`--nodeDebugPort -n`** | De poort voor het foutopsporingsprogramma knooppunt moet worden gebruikt. Standaard: Een waarde van launch.json of 5858. |
| **`--debugLevel -d`** | Het traceerniveau van de console (uit verbose,-info, waarschuwing of fout). Standaard: Info.|
| **`--timeout -t`** | De time-out voor de host functies om te starten, in seconden. Standaardwaarde: 20 seconden.|
| **`--useHttps`** | Binden aan https://localhost: {poort} in plaats van op http://localhost: {poort}. Deze optie maakt standaard een vertrouwd certificaat op uw computer.|
| **`--pause-on-error`** | Onderbreken om extra gegevens voordat het proces wordt afgesloten. Dit is handig bij het starten van Azure Functions kernonderdelen van een integrated development environment (IDE).|

Wanneer de host functies wordt gestart, levert dit de URL van de HTTP-geactiveerde functies:

```
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

### <a name="debug-in-vs-code-or-visual-studio"></a>Fouten opsporen in VS-Code of Visual Studio

Als u wilt een foutopsporingsprogramma koppelen, geeft de `--debug` argument. Gebruik voor foutopsporing JavaScript-functies, Visual Studio Code. Voor C#, Visual Studio te gebruiken.

Gebruiken om fouten opsporen in C#-functies, `--debug vs`. U kunt ook [Azure Functions Visual Studio 2017 Tools](https://blogs.msdn.microsoft.com/webdev/2017/05/10/azure-function-tools-for-visual-studio-2017/). 

Voor het starten van de host en JavaScript foutopsporing ingesteld, worden uitgevoerd:

```
func host start --debug vscode
```

Klik in Visual Studio Code in de **Debug** weergave, selecteer **koppelen aan Azure Functions**. U kunt onderbrekingspunten koppelen, variabelen inspecteren en analyseer code.

![JavaScript foutopsporing met Visual Studio Code](./media/functions-run-local/vscode-javascript-debugging.png)

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

```
curl --get http://localhost:7071/api/MyHttpTrigger?name=Azure%20Rocks
```
Het volgende voorbeeld wordt dezelfde functie aangeroepen vanuit een POST-aanvraag doorgeven _naam_ in de aanvraagtekst:

```
curl --request POST http://localhost:7071/api/MyHttpTrigger --data '{"name":"Azure Rocks"}'
```

Houd er rekening mee dat kunt u aanvragen ophalen vanuit een browser het doorgeven van gegevens in de queryreeks. Voor alle andere HTTP-methoden, moet u cURL, Fiddler, Postman of een vergelijkbaar testen HTTP-hulpprogramma gebruiken.  

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

```
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Met behulp van de `func run` opdracht in versie 1.x

>[!IMPORTANT]  
> De `func run` opdracht wordt niet ondersteund in versie 2.x van de hulpprogramma's. Zie voor meer informatie het onderwerp [hoe gericht op Azure Functions-runtime-versies](functions-versions.md).

U kunt ook een functie aanroepen via `func run <FunctionName>` en invoergegevens bieden voor de functie. Deze opdracht is vergelijkbaar met die voor een functie met de **Test** tabblad in de Azure-portal. 

`func run`ondersteunt de volgende opties:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--content -c`** | Inline-inhoud. |
| **`--debug -d`** | Een foutopsporingsprogramma koppelen aan het hostproces voordat u de functie uitvoert.|
| **`--timeout -t`** | Tijd (in seconden) wordt gewacht totdat de lokale host van functies gereed is.|
| **`--file -f`** | De bestandsnaam om te gebruiken als de inhoud.|
| **`--no-interactive`** | Wordt niet gevraagd om invoer. Dit is handig voor automatiseringsscenario's.|

Voer bijvoorbeeld de volgende opdracht om een HTTP-geactiveerde functie aanroepen en hoofdinhoud doorgeven:

```
func run MyHttpTrigger -c '{\"name\": \"Azure\"}'
```

## <a name="publish"></a>Publiceren naar Azure

Voor het publiceren van een project functies in een functie-app in Azure, gebruiken de `publish` opdracht:

```
func azure functionapp publish <FunctionAppName>
```

U kunt de volgende opties gebruiken:

| Optie     | Beschrijving                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publicatie-instellingen in local.settings.json naar Azure, waarin wordt gevraagd om te overschrijven als de instelling bestaat al.|
| **`--overwrite-settings -y`** | Moet worden gebruikt met `-i`. Overschrijft AppSettings in Azure met lokale waarde als deze verschilt. De standaardwaarde is vragen.|

Met deze opdracht publiceert naar een bestaande functie-app in Azure. Een fout optreedt wanneer de `<FunctionAppName>` bestaat niet in uw abonnement. Zie voor informatie over het maken van een functie-app vanuit de opdrachtprompt of terminalvenster met de Azure CLI, [maken van een functie-App voor uitvoering zonder server](./scripts/functions-cli-create-serverless.md).

De `publish` opdracht wordt de inhoud van de projectmap functies geüpload. Als u bestanden lokaal, verwijdert de `publish` opdracht ze niet verwijdert uit Azure. U kunt bestanden in Azure verwijderen met behulp van de [Kudu hulpprogramma](functions-how-to-use-azure-function-app-settings.md#kudu) in de [Azure-portal].  

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
[Azure-portal]: https://portal.azure.com 
