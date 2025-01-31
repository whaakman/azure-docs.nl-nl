---
title: Werken met Azure Functions Core Tools | Microsoft Docs
description: Informatie over het programmeren en testen van Azure functions vanaf de opdrachtprompt of terminal op uw lokale computer voordat u ze op Azure Functions uitvoeren.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: 242736be-ec66-4114-924b-31795fd18884
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/13/2019
ms.author: glenga
ms.custom: 80e4ff38-5174-43
ms.openlocfilehash: dd6259173792585a83effd42c75ff9a7a7d572e4
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67448362"
---
# <a name="work-with-azure-functions-core-tools"></a>Werken met Azure Functions Core Tools

Azure Functions Core Tools kunt u ontwikkelen en testen van uw functies op uw lokale computer vanuit de opdrachtprompt of terminal. Uw lokale functies kunnen verbinding maken met live Azure-services, en u kunt fouten opsporen in uw functies op uw lokale computer met behulp van de volledige Functions-runtime. U kunt ook een functie-app implementeren naar uw Azure-abonnement.

[!INCLUDE [Don't mix development environments](../../includes/functions-mixed-dev-environments.md)]

Ontwikkelen van functies op uw lokale computer en ze publiceren naar Azure met behulp van Core Tools volgt deze eenvoudige stappen:

> [!div class="checklist"]
> * [Installeer de Core-hulpprogramma's en afhankelijkheden.](#v2)
> * [Een functie-app-project met een specifieke taal-sjabloon maken.](#create-a-local-functions-project)
> * [Trigger en binding extensies registreren.](#register-extensions)
> * [Opslag en andere verbindingen definiëren.](#local-settings-file)
> * [Een functie maken vanuit een trigger en taalspecifieke sjabloon.](#create-func)
> * [De functie lokaal uitvoeren](#start)
> * [Het project publiceren naar Azure](#publish)

## <a name="core-tools-versions"></a>Core-versies van de hulpprogramma 's

Er zijn twee versies van Azure Functions Core Tools. De versie die u gebruikt, is afhankelijk van uw lokale ontwikkelomgeving [keuze van taal](supported-languages.md), en het niveau van ondersteuning vereist:

+ Versie 1.x: biedt ondersteuning voor versie 1.x van de runtime. Deze versie van de hulpprogramma's wordt alleen ondersteund op Windows-computers en wordt geïnstalleerd vanuit een [npm-pakket](https://docs.npmjs.com/getting-started/what-is-npm). Met deze versie kunt u functies maken in de experimentele talen die officieel niet worden ondersteund. Zie voor meer informatie, [ondersteunde talen in Azure Functions](supported-languages.md)

+ [Versie 2.x](#v2): biedt ondersteuning voor [versie 2.x van de runtime](functions-versions.md). Deze versie biedt ondersteuning voor [Windows](#windows-npm), [macOS](#brew), en [Linux](#linux). Maakt gebruik van platform-specifieke pakketmanagers of npm voor installatie.

Tenzij anders vermeld, de voorbeelden in dit artikel zijn voor versie 2.x.

## <a name="install-the-azure-functions-core-tools"></a>Azure Functions Core Tools installeren

[Azure Functions Core Tools] omvat een versie van de dezelfde runtime die wordt gebruikt door Azure Functions-runtime die u op uw lokale ontwikkelcomputer uitvoeren kunt. Het biedt ook opdrachten voor het maken van functies, verbinding maken met Azure, en functie projecten kunt implementeren.

### <a name="v2"></a>Versie 2.x

Versie 2.x van de hulpprogramma's maakt gebruik van de Azure Functions-runtime 2.x die is gebaseerd op .NET Core. Deze versie wordt ondersteund op alle platforms .NET Core 2.x ondersteunt, met inbegrip van [Windows](#windows-npm), [macOS](#brew), en [Linux](#linux). 

> [!IMPORTANT]
> U kunt overslaan van de vereiste voor het installeren van de .NET Core SDK 2.x met behulp van [extensie bundels].

#### <a name="windows-npm"></a>Windows

De volgende stappen gebruikt npm Core Tools installeren op Windows. U kunt ook [Chocolatey](https://chocolatey.org/). Zie voor meer informatie de [Core Tools Leesmij](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#windows).

1. Installeer [Node.js], waaronder npm. Voor versie 2.x van de hulpprogramma's, alleen Node.js 8.5 en latere versies worden ondersteund.

1. Installeer het pakket Core-hulpprogramma's:

    ```bash
    npm install -g azure-functions-core-tools
    ```

   Het duurt een paar minuten voor npm downloaden en installeren van het pakket Core Tools.

1. Als u niet van plan bent te gebruiken [extensie bundels], installeert de [.NET Core 2.x SDK voor Windows](https://www.microsoft.com/net/download/windows).

#### <a name="brew"></a>MacOS met Homebrew

De volgende stappen gebruikt Homebrew de essentiële hulpprogramma installeren op macOS.

1. Installeer [Homebrew](https://brew.sh/), als deze nog niet geïnstalleerd.

1. Installeer het pakket Core-hulpprogramma's:

    ```bash
    brew tap azure/functions
    brew install azure-functions-core-tools
    ```

1. Als u niet van plan bent te gebruiken [extensie bundels], installeer [.NET Core SDK 2.x voor macOS](https://www.microsoft.com/net/download/macos).


#### <a name="linux"></a> Linux (Ubuntu/Debian) met APT

De volgende stappen [APT](https://wiki.debian.org/Apt) Core Tools installeren op uw Ubuntu Debian/Linux-distributie. Zie voor andere Linux-distributies, de [Core Tools Leesmij](https://github.com/Azure/azure-functions-core-tools/blob/master/README.md#linux).

1. De sleutel voor de Microsoft-product als vertrouwd registreren:

    ```bash
    curl https://packages.microsoft.com/keys/microsoft.asc | gpg --dearmor > microsoft.gpg
    sudo mv microsoft.gpg /etc/apt/trusted.gpg.d/microsoft.gpg
    ```

1. Controleer of dat de Ubuntu-server een van de juiste versies van de onderstaande tabel wordt uitgevoerd. Als u wilt de apt-bron toevoegen, voert u de volgende uit:

    ```bash
    sudo sh -c 'echo "deb [arch=amd64] https://packages.microsoft.com/repos/microsoft-ubuntu-$(lsb_release -cs)-prod $(lsb_release -cs) main" > /etc/apt/sources.list.d/dotnetdev.list'
    sudo apt-get update
    ```

    | Linux-distributie | Version |
    | --------------- | ----------- |
    | Ubuntu 18.10    | `cosmic`    |
    | Ubuntu 18.04    | `bionic`    |
    | Ubuntu 17.04    | `zesty`     |
    | Ubuntu 16.04/Linux Mint 18    | `xenial`  |

1. Installeer het pakket Core-hulpprogramma's:

    ```bash
    sudo apt-get install azure-functions-core-tools
    ```

1. Als u niet van plan bent te gebruiken [extensie bundels], installeren [.NET Core SDK voor Linux 2.x](https://www.microsoft.com/net/download/linux).

## <a name="create-a-local-functions-project"></a>Een lokaal Functions-project maken

Een map functions-project bevat de bestanden [host.json](functions-host-json.md) en [local.settings.json](#local-settings-file), samen met de submappen die de code voor afzonderlijke functies bevatten. Deze map is het equivalent van een functie-app in Azure. Zie voor meer informatie over de structuur van de functies, de [handleiding voor ontwikkelaars van Azure Functions voor](functions-reference.md#folder-structure).

Versie 2.x, moet u een taal voor uw project selecteren wanneer deze wordt geïnitialiseerd en alle functies gebruiken standaard taalsjablonen toegevoegd. In versie 1.x, u de taal opgeven telkens wanneer u een functie maken.

In het terminalvenster of vanaf een opdrachtprompt, voer de volgende opdracht om het project en lokale Git-opslagplaats te maken:

```bash
func init MyFunctionProj
```

Wanneer u de naam van een project opgeeft, wordt een nieuwe map met die naam gemaakt en geïnitialiseerd. Anders wordt is de huidige map geïnitialiseerd.  
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

`func init` ondersteunt de volgende opties, die versie 2.x alleen-lezen, tenzij anders wordt vermeld:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--csx`** | Initialiseert een C#-script (.csx)-project. U moet opgeven `--csx` in de volgende opdrachten. |
| **`--docker`** | Maken van een docker-bestand voor een container met behulp van een basisinstallatiekopie die is gebaseerd op de gekozen `--worker-runtime`. Gebruik deze optie als u van plan bent om te publiceren naar een aangepaste Linux-container. |
| **`--force`** | Initialiseren van het project, zelfs wanneer er bestaande bestanden in het project zijn. Deze instelling overschrijft bestaande bestanden met dezelfde naam. Andere bestanden in de projectmap, worden niet beïnvloed. |
| **`--no-source-control -n`** | Hiermee voorkomt u dat het standaard-maken van een Git-opslagplaats in versie 1.x. In versie 2.x, de git-opslagplaats die standaard is niet gemaakt. |
| **`--source-control`** | Hiermee bepaalt u of een git-opslagplaats wordt gemaakt. Standaard wordt een opslagplaats is niet gemaakt. Wanneer `true`, een opslagplaats wordt gemaakt. |
| **`--worker-runtime`** | Hiermee stelt u de taal voor het project. Ondersteunde waarden zijn `dotnet`, `node` (JavaScript), `java`, en `python`. Als niet is ingesteld, wordt u gevraagd uw runtime kiezen tijdens de initialisatie. |

> [!IMPORTANT]
> Standaard versie 2.x van de essentiële hulpprogramma maakt de functie app-projecten voor de .NET runtime als [projecten van C#-klasse](functions-dotnet-class-library.md) (.csproj). Deze C# projecten, die kunnen worden gebruikt met Visual Studio of Visual Studio Code, worden gecompileerd tijdens het testen en bij het publiceren naar Azure. Als u in plaats daarvan wilt maken en werken met de dezelfde C#-script (.csx)-bestanden gemaakt in versie 1.x en in de portal, moet u de `--csx` parameter bij het maken en implementeren van functions.

[!INCLUDE [functions-core-tools-install-extension](../../includes/functions-core-tools-install-extension.md)]

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Standaard worden deze instellingen niet gemigreerd automatisch wanneer het project wordt gepubliceerd naar Azure. Gebruik de `--publish-local-settings` overschakelen [bij het publiceren van](#publish) om ervoor te zorgen deze instellingen worden toegevoegd aan de functie-app in Azure. Houd er rekening mee dat de waarden in **ConnectionStrings** nooit worden gepubliceerd.

De waarden voor de functie-app-instellingen kunnen ook worden gelezen in uw code als omgevingsvariabelen. Zie de sectie van de variabelen voor de omgeving van de volgende taalspecifieke referentie-onderwerpen voor meer informatie:

* [C# vooraf geschreven](functions-dotnet-class-library.md#environment-variables)
* [C# script (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

Wanneer er is geen geldige verbindingsreeks is ingesteld voor [ `AzureWebJobsStorage` ] en de emulator niet wordt gebruikt, wordt het volgende foutbericht weergegeven:

> Ontbrekende waarde voor AzureWebJobsStorage in local.settings.json. Dit is vereist voor alle triggers dan HTTP. U kunt uitvoeren ' azure functionapp ophalen-app-instellingen van de func \<functionAppName\>' of een verbindingsreeks in local.settings.json opgeven.

### <a name="get-your-storage-connection-strings"></a>Uw storage-verbindingsreeksen ophalen

Zelfs wanneer u de opslagemulator gebruikt voor ontwikkeling, kunt u om te testen met een werkelijke opslag-verbinding. Ervan uitgaande dat u al hebt [een storage-account gemaakt](../storage/common/storage-create-storage-account.md), krijgt u een geldige verbindingsreeks in een van de volgende manieren:

+ Uit de [Azure-portal]. Navigeer naar uw opslagaccount, selecteer **toegangssleutels** in **instellingen**, en kopieer een van de **Connection string** waarden.

  ![Verbindingsreeks kopiëren vanuit Azure portal](./media/functions-run-local/copy-storage-connection-portal.png)

+ Gebruik [Azure Storage Explorer](https://storageexplorer.com/) verbinding maken met uw Azure-account. In de **Explorer**, vouw uw abonnement, selecteer uw opslagaccount en kopieer de primaire of secundaire verbindingsreeks.

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

Voer de volgende opdracht uit om een functie te maken:

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

| Argument     | Description                            |
| ------------------------------------------ | -------------------------------------- |
| **`--csx`** | (Versie 2.x) Genereert de dezelfde C#-script (.csx) sjablonen die worden gebruikt in versie 1.x en in de portal. |
| **`--language -l`**| De sjabloon programmeertaal, zoals C#, F#, of JavaScript. Deze optie is vereist in versie 1.x. In versie 2.x gebruikt, geen gebruik deze optie of kies een taal die overeenkomt met de worker-runtime. |
| **`--name -n`** | De naam van de functie. |
| **`--template -t`** | Gebruik de `func templates list` opdracht om te zien van de volledige lijst met beschikbare sjablonen voor elke ondersteunde taal.   |

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

De `host` opdracht is alleen vereist in versie 1.x.

`func host start` ondersteunt de volgende opties:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--no-build`** | Er is geen huidige project build voordat u doen. Voor dotnet projecten. Standaard is ingesteld op false. Versie 2.x alleen. |
| **`--cert`** | Het pad naar een pfx-bestand dat een persoonlijke sleutel bevat. Alleen gebruikt met `--useHttps`. Versie 2.x alleen. |
| **`--cors-credentials`** | Cross-origin-geverifieerde aanvragen (dat wil zeggen cookies en header voor de verificatie) toestaan versie 2.x alleen. |
| **`--cors`** | Een door komma's gescheiden lijst met CORS-oorsprong, zonder spaties. |
| **`--language-worker`** | Argumenten voor het configureren van de werknemer taal. Versie 2.x alleen. |
| **`--nodeDebugPort -n`** | De poort voor het foutopsporingsprogramma knooppunt te gebruiken. Standaard: Een waarde van launch.json of 5858. Versie 1.x alleen. |
| **`--password`** | Het wachtwoord of een bestand met het wachtwoord voor een pfx-bestand. Alleen gebruikt met `--cert`. Versie 2.x alleen. |
| **`--port -p`** | De lokale poort voor luisteren. Standaardwaarde: 7071. |
| **`--pause-on-error`** | Aanvullende invoer wacht voordat het proces wordt afgesloten. Alleen tijdens het starten van Core-hulpprogramma's vanaf een integrated development environment (IDE) gebruikt.|
| **`--script-root --prefix`** | Hiermee geeft het pad naar de hoofdmap van de functie-app die moet worden uitgevoerd of geïmplementeerd. Dit wordt gebruikt voor compilatie projecten die projectbestanden te in een submap genereren. Bijvoorbeeld, wanneer u een C#-klassenbibliotheek project, de host.json local.settings.json en function.json bestanden bouwen worden gegenereerd een *hoofdmap* submap met een pad, zoals `MyProject/bin/Debug/netstandard2.0`. In dit geval stelt het voorvoegsel als `--script-root MyProject/bin/Debug/netstandard2.0`. Dit is de hoofdmap van de functie-app wanneer die wordt uitgevoerd in Azure. |
| **`--timeout -t`** | De time-out voor de host functies om te starten, in seconden. Standaard: 20 seconden.|
| **`--useHttps`** | Verbinding maken met `https://localhost:{port}` in plaats van naar `http://localhost:{port}`. Deze optie maakt standaard een vertrouwd certificaat op uw computer.|

Voor een C#-klassebibliotheekproject (.csproj), moet u opnemen de `--build` optie voor het genereren van het DLL-bestand voor de bibliotheek.

Wanneer de Functions-host wordt gestart, levert dit de URL van de HTTP-geactiveerde functies:

```output
Found the following functions:
Host.Functions.MyHttpTrigger

Job host started
Http Function MyHttpTrigger: http://localhost:7071/api/MyHttpTrigger
```

>[!IMPORTANT]
>Wanneer lokaal uitvoert, wordt verificatie wordt niet afgedwongen voor HTTP-eindpunten. Dit betekent dat alle lokale HTTP-aanvragen worden verwerkt als `authLevel = "anonymous"`. Zie voor meer informatie de [HTTP-binding artikel](functions-bindings-http-webhook.md#authorization-keys).

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
```

De `<trigger_input>` waarde bevat de gegevens in een indeling die door de functie wordt verwacht. De volgende cURL-voorbeeld wordt een bericht naar een `QueueTriggerJS` functie. In dit geval is de invoer een tekenreeks die gelijk is aan het bericht dat wordt verwacht in de wachtrij moet worden gezocht.

```bash
curl --request POST -H "Content-Type:application/json" --data '{"input":"sample queue data"}' http://localhost:7071/admin/functions/QueueTriggerJS
```

#### <a name="using-the-func-run-command-in-version-1x"></a>Met behulp van de `func run` opdracht in versie 1.x

>[!IMPORTANT]
> De `func run` opdracht wordt niet ondersteund in versie 2.x van de hulpprogramma's. Zie voor meer informatie het onderwerp [hoe gericht op versies van Azure Functions-runtime](set-runtime-version.md).

U kunt ook een functie aanroepen via `func run <FunctionName>` en geef de invoergegevens voor de functie. Met deze opdracht is vergelijkbaar met het uitvoeren van een functie met de **Test** tabblad in de Azure-portal.

`func run` ondersteunt de volgende opties:

| Optie     | Description                            |
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

## <a name="publish"></a>Publiceren naar Azure

Azure Functions Core Tools ondersteunt twee typen implementatie: functie projectbestanden implementeren rechtstreeks naar uw functie-app via [Zip implementeren](functions-deployment-technologies.md#zip-deploy) en [implementeren van een aangepaste Docker-container](functions-deployment-technologies.md#docker-container). U moet al hebben [een functie-app gemaakt in uw Azure-abonnement](functions-cli-samples.md#create), waarnaar u implementeert uw code. Projecten waarvoor compilatie moeten worden gemaakt, zodat de binaire bestanden kunnen worden geïmplementeerd.

### <a name="project-file-deployment"></a>Implementatie (projectbestanden)

Voor het publiceren van uw lokale code in een functie-app in Azure, gebruikt u de `publish` opdracht:

```bash
func azure functionapp publish <FunctionAppName>
```

Met deze opdracht publiceert naar een bestaande functie-app in Azure. U krijgt een fout op als u probeert te publiceren naar een `<FunctionAppName>` die niet bestaat in uw abonnement. Zie voor meer informatie over het maken van een functie-app vanuit de opdrachtprompt of terminal-venster met de Azure CLI, [maken van een functie-App voor uitvoering zonder server](./scripts/functions-cli-create-serverless.md). Standaard wordt inschakelen voor deze opdracht de app om uit te voeren [uitvoeren van pakket](run-functions-from-deployment-package.md) modus.

>[!IMPORTANT]
> Wanneer u een functie-app in Azure portal maakt, wordt versie 2.x van de runtime van de functies standaard. Om te maken van de functie app-gebruik versie 1.x van de runtime, volg de instructies in [worden uitgevoerd op versie 1.x](functions-versions.md#creating-1x-apps).
> U kunt de runtimeversie voor een functie-app met bestaande functies niet wijzigen.

De volgende opties voor het publiceren van toepassing voor zowel versies, 1.x en 2.x:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--publish-local-settings -i`** |  Publicatie-instellingen in local.settings.json naar Azure, dat u wordt gevraagd om te overschrijven als de instelling bestaat al. Als u de opslagemulator gebruikt, wijzigt u eerst de app-instelling op een [werkelijke opslagverbinding](#get-your-storage-connection-strings). |
| **`--overwrite-settings -y`** | De prompt dat appinstellingen worden overschreven wanneer `--publish-local-settings -i` wordt gebruikt.|

De volgende opties publiceren, worden alleen ondersteund in versie 2.x:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--publish-settings-only -o`** |  Publicatie-instellingen en alleen de inhoud overslaan. De standaardwaarde is vragen. |
|**`--list-ignored-files`** | Geeft een lijst van bestanden die worden genegeerd tijdens het publiceren, die is gebaseerd op het bestand .funcignore. |
| **`--list-included-files`** | Geeft een lijst van bestanden die zijn gepubliceerd, die is gebaseerd op het bestand .funcignore. |
| **`--nozip`** | Hiermee schakelt u de standaard `Run-From-Package` modus uit. |
| **`--build-native-deps`** | Slaat het genereren van .wheels map bij het publiceren van python functie-apps. |
| **`--additional-packages`** | Lijst met pakketten te installeren bij het bouwen van systeemeigen afhankelijkheden. Bijvoorbeeld: `python3-dev libevent-dev`. |
| **`--force`** | Negeren vooraf publishing verificatie in bepaalde scenario's. |
| **`--csx`** | Een C#-script (.csx)-project kunt publiceren. |
| **`--no-build`** | Overslaan dotnet functies bouwen. |
| **`--dotnet-cli-params`** | Wanneer publiceren gecompileerde C# (.csproj) functies, de essentiële hulpprogramma 'dotnet build--output bin/publiceren'-aanroepen. Parameters doorgegeven aan deze zullen worden toegevoegd aan de opdrachtregel. |

### <a name="deployment-custom-container"></a>Implementatie (aangepaste container)

Azure Functions kunt u uw functieproject in implementeren een [aangepaste Docker-container](functions-deployment-technologies.md#docker-container). Zie voor meer informatie, [een functie in Linux maken met een aangepaste installatiekopie](functions-create-function-linux-custom-image.md). Aangepaste containers moeten een docker-bestand hebben. Voor het maken van een app met een docker-bestand, gebruikt u de optie--dockerfile op `func init`.

```bash
func deploy
```

De volgende aangepaste container implementatie-opties zijn beschikbaar:

| Optie     | Description                            |
| ------------ | -------------------------------------- |
| **`--registry`** | De naam van een Docker-register van de huidige gebruiker aangemeld. |
| **`--platform`** | Host-platform voor de functie-app. Geldige opties zijn `kubernetes` |
| **`--name`** | Naam van de functie-app. |
| **`--max`**  | Hiermee stelt u het maximum aantal exemplaren van de functie-app implementeren op (optioneel). |
| **`--min`**  | Hiermee stelt u het minimum aantal exemplaren van de functie app implementeren op (optioneel). |
| **`--config`** | Hiermee stelt u een configuratiebestand voor optionele implementatie. |

## <a name="monitoring-functions"></a>Functions controleren

De aanbevolen manier voor het bewaken van de uitvoering van uw functies is door te integreren met Azure Application Insights. Wanneer u een functie-app in Azure portal maakt, wordt deze integratie voor u uitgevoerd door standaard. Echter, wanneer u uw functie-app maakt met behulp van de Azure CLI, is niet de integratie in uw functie-app in Azure uitgevoerd.

Application Insights inschakelen voor uw functie-app:

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Zie voor meer informatie, [Monitor Azure Functions](functions-monitoring.md).
## <a name="next-steps"></a>Volgende stappen

Azure Functions Core Tools is [open source en gehost op GitHub](https://github.com/azure/azure-functions-cli).  
Naar het bestand een bug of functie-aanvraag [opent u een GitHub-probleem](https://github.com/azure/azure-functions-cli/issues).

<!-- LINKS -->

[Azure Functions Core Tools]: https://www.npmjs.com/package/azure-functions-core-tools
[Azure-portal]: https://portal.azure.com 
[Node.js]: https://docs.npmjs.com/getting-started/installing-node#osx-or-windows
[`FUNCTIONS_WORKER_RUNTIME`]: functions-app-settings.md#functions_worker_runtime
[`AzureWebJobsStorage`]: functions-app-settings.md#azurewebjobsstorage
[extensie bundels]: functions-bindings-register.md#extension-bundles
