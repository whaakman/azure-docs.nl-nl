---
title: Azure Functions met behulp van Visual Studio Code ontwikkelen | Microsoft Docs
description: Informatie over het ontwikkelen en testen van Azure Functions met behulp van Azure Functions-extensie voor Visual Studio Code.
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: conceptual
ms.date: 04/11/2019
ms.author: glenga
ms.openlocfilehash: 17550e148ea61eb69a20fc6a3215dfb63b65f18e
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67452694"
---
# <a name="develop-azure-functions-using-visual-studio-code"></a>Azure Functions met behulp van Visual Studio Code ontwikkelen

De [Azure Functions extension for Visual Studio Code] kunt u lokaal ontwikkelen en implementeren van functies in Azure. Als deze ervaring uw eerste met Azure Functions is, kunt u meer informatie op [een inleiding tot Azure Functions](functions-overview.md).

De extensie Azure Functions biedt de volgende voordelen: 

* Bewerken, ontwikkelen en uitvoeren van functies op uw lokale ontwikkelcomputer. 
* Publiceer uw Azure Functions-project rechtstreeks naar Azure. 
* Schrijf uw functies in verschillende talen en hebben alle van de voordelen van Visual Studio Code. 

De extensie kan worden gebruikt met de volgende talen ondersteund door de Azure Functions-runtime versie 2.x: 

* [C#gecompileerd](functions-dotnet-class-library.md) 
* [C# script](functions-reference-csharp.md)<sup>*</sup>
* [JavaScript](functions-reference-node.md)
* [Java](functions-reference-java.md)
* [PowerShell](functions-reference-powershell.md)
* [Python](functions-reference-python.md)

<sup>*</sup>Moet u dat u [ingesteld C# script op als uw project standaardtaal](#c-script-projects).

In dit artikel voorbeelden zijn momenteel alleen beschikbaar voor JavaScript (Node.js) en C# klasse bibliotheekfuncties.  

Dit artikel bevat informatie over het gebruik van de extensie Azure Functions ontwikkelen van functies en deze publiceren naar Azure. Voordat u dit artikel leest, moet u [uw eerste functie maken met Visual Studio Code](functions-create-first-function-vs-code.md).

> [!IMPORTANT]
> Combineer geen lokale ontwikkeling met portal-ontwikkeling in dezelfde functie-app. Wanneer u in een lokale-project met een functie-app publiceert, worden alle functies die u hebt ontwikkeld in de portal in het implementatieproces overschreven.

## <a name="prerequisites"></a>Vereisten

Voordat u installeert en uitvoert de [Azure Functions-extensie][azure functions extension for visual studio code], moet u voldoen aan de volgende vereisten:

* [Visual Studio Code](https://code.visualstudio.com/) geïnstalleerd op een van de [ondersteunde platforms](https://code.visualstudio.com/docs/supporting/requirements#_platforms).

* Een actief Azure-abonnement.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

Andere resources die u nodig hebt, zoals een Azure Storage-account in uw abonnement gemaakt wanneer u [publiceren met behulp van Visual Studio Code](#publish-to-azure).

> [!IMPORTANT]
> U kunt functies lokaal ontwikkelen en publiceren naar Azure zonder te starten en voer ze lokaal uit. Er zijn aanvullende vereisten voor de functionele die lokaal wordt uitgevoerd, met inbegrip van een automatisch downloaden van de Azure Functions Core Tools. Zie voor meer informatie, [aanvullende vereisten voor het lokaal uitvoeren](#additional-requirements-to-run-locally). 

[!INCLUDE [functions-install-vs-code-extension](../../includes/functions-install-vs-code-extension.md)]

## <a name="create-an-azure-functions-project"></a>Een Azure Functions-project maken

De extensie functies kunt u een functie-app-project, samen met uw eerste functie maken. De volgende stappen laten zien hoe een door HTTP geactiveerde functie maken in een nieuw functions-project. [HTTP-trigger](functions-bindings-http-webhook.md) is de eenvoudigste functie trigger-sjabloon om te demonstreren.

1. In **Azure: Functions** kiest u het pictogram Functie maken.

    ![Een functie maken](./media/functions-develop-vs-code/create-function.png)

1. Selecteer de map voor uw functie-app-project, en vervolgens **een taal selecteren voor uw functieproject**. 

1. Selecteer de **HTTP-trigger** functiesjabloon, of u kunt kiezen om te **stap nu overslaan** om een project zonder een functie te maken. U kunt altijd [een functie toevoegen aan uw project](#add-a-function-to-your-project) op een later tijdstip. 

    ![De sjabloon voor de HTTP-trigger kiezen](./media/functions-develop-vs-code/create-function-choose-template.png)

1. Type `HTTPTrigger` voor de naam van de functie en druk op Enter, selecteert u vervolgens **functie** autorisatie. Dit autorisatieniveau, moet u voor een [functietoets](functions-bindings-http-webhook.md#authorization-keys) bij het aanroepen van de functie-eindpunt.

    ![Functie-verificatie kiest](./media/functions-develop-vs-code/create-function-auth.png)

    Een functie wordt gemaakt in de door u gekozen taal met de sjabloon voor een door HTTP getriggerde functie.

    ![Sjabloon voor een door HTTP getriggerde functie in Visual Studio Code](./media/functions-develop-vs-code/new-function-full.png)

De projectsjabloon, maken maakt een project in uw gekozen taal, installeert de vereiste afhankelijkheden. Voor elke taal heeft het nieuwe project de volgende bestanden:

* **host.json**: Kunt u de host van de functies configureren. Deze instellingen gelden zowel bij het uitvoeren van lokaal en in Azure. Zie voor meer informatie, [naslaginformatie over host.json](functions-host-json.md).

* **local.settings.json**: Instellingen die worden gebruikt bij het uitvoeren van functies lokaal onderhoudt. Deze instellingen worden alleen gebruikt wanneer die lokaal wordt uitgevoerd. Zie voor meer informatie, [lokale instellingenbestand](#local-settings-file).

    >[!IMPORTANT]
    >Omdat het bestand local.settings.json kunt geheimen bevat, moet u deze uitgesloten van uw project broncodebeheer.

U kunt op dit moment invoer toevoegen en uitvoerbindingen naar de functie door [wijzigen van het bestand function.json](#javascript-2), of door [toe te voegen een parameter voor een C# klasse bibliotheek functie](#c-class-library-2).

U kunt ook [een nieuwe functie toevoegen aan uw project](#add-a-function-to-your-project).

## <a name="install-binding-extensions"></a>Binding-extensies installeren

Bindingen zijn, met uitzondering van HTTP- en Timer wordt geactiveerd geïmplementeerd in de extensiepakketten. U moet de extensiepakketten voor de triggers en bindingen die ze nodig hebben. De manier waarop u bindinguitbreidingen installeren, is afhankelijk van de projecttaal van uw.

### <a name="javascript"></a>Javascript

[!INCLUDE [functions-extension-bundles](../../includes/functions-extension-bundles.md)]

### <a name="c-class-library"></a>C\# klassebibliotheek

Voer de [dotnet-pakket toevoegen](/dotnet/core/tools/dotnet-add-package) opdracht in het terminalvenster om de extensiepakketten die u nodig hebt in uw project te installeren. Het volgende voorbeeld installeert de extensie Azure Storage, waarmee de bindingen voor Blob, Queue en Table storage wordt geïmplementeerd.

```bash
dotnet add package Microsoft.Azure.WebJobs.Extensions.Storage --version 3.0.4
```

## <a name="add-a-function-to-your-project"></a>Een functie toevoegen aan uw project

U kunt een nieuwe functie toevoegen aan een bestaand project met behulp van een van de vooraf gedefinieerde sjablonen voor Functions-trigger. Druk op F1 drukt het opdrachtenpalet te openen en vervolgens zoeken naar en voer de opdracht uit om toe te voegen een nieuwe functie-trigger, **Azure Functions: Functie maken...** . Volg de aanwijzingen van de trigger kiezen en de vereiste kenmerken van de trigger kunt definiëren. Als de trigger een toegang toegangssleutel of verbindingsreeks verbinding maken met een service vereist, bereid u deze voordat u de functie-trigger maken. 

De resultaten van deze bewerking, is afhankelijk van de projecttaal van uw:

### <a name="javascript"></a>Javascript

Een nieuwe map wordt gemaakt in het project, die een nieuw bestand in de function.json en het nieuwe bestand van de JavaScript-code bevat.

### <a name="c-class-library"></a>C\# klassebibliotheek

Een nieuwe C# class library (.cs)-bestand wordt toegevoegd aan uw project.

## <a name="add-input-and-output-bindings"></a>Invoer toevoegen en uitvoerbindingen

U kunt u de functie uitbreiden door het invoer- en uitvoerbindingen toe te voegen. De manier waarop u dit doet, is afhankelijk van de projecttaal van uw. Zie voor meer informatie over bindingen [Azure Functions-triggers en bindingen concepten](functions-triggers-bindings.md). 

De volgende voorbeelden verbinding maken met een storage-wachtrij met de naam `outqueue`, waarbij de verbindingsreeks voor het opslagaccount dat is ingesteld in de `MyStorageConnection` toepassingsinstelling in local.settings.json. 

### <a name="javascript"></a>Javascript

Visual Studio Code kunt u bindingen toevoegen aan uw bestand function.json door een handige set prompts te volgen. Een binding maken met de rechtermuisknop op (Ctrl + klik op Mac OS) de `function.json` -bestand in de map van de functie en kies **binding toevoegen...** . 

![Een binding aan een bestaande JavaScript-functie toevoegen ](media/functions-develop-vs-code/function-add-binding.png)

Hieronder vindt u voorbeeld aanwijzingen voor het definiëren van een nieuwe storage-Uitvoerbinding:

| Vraag | Value | Description |
| -------- | ----- | ----------- |
| **Richting van binding selecteren** | `out` | De binding is een Uitvoerbinding. |
| **Binding met de richting selecteren...** | `Azure Queue Storage` | De binding is een Azure Storage-wachtrij-binding. |
| **De naam die wordt gebruikt om deze binding in uw code** | `msg` | Naam ter identificatie van de bindingsparameter waarnaar wordt verwezen in uw code. |
| **De wachtrij waarnaar het bericht wordt verzonden** | `outqueue` | De naam van de wachtrij die de binding naar schrijft. Wanneer de *queueName* niet bestaat, de binding wordt deze gemaakt bij het eerste gebruik. |
| **Selecteer de instelling van "local.setting.json"** | `MyStorageConnection` | De naam van een toepassingsinstelling met de verbindingsreeks voor de Storage-account. De `AzureWebJobsStorage` instelling bevat de verbindingsreeks voor het Opslagaccount dat u hebt gemaakt met de functie-app. |

In dit voorbeeld de volgende binding wordt toegevoegd aan de `bindings` -matrix in uw bestand function.json:

```javascript
{
    "type": "queue",
    "direction": "out",
    "name": "msg",
    "queueName": "outqueue",
    "connection": "MyStorageConnection"
}
```

U kunt ook de definitie van de dezelfde binding rechtstreeks naar uw function.json toevoegen.

In uw functiecode aan te geven, de `msg` binding is toegankelijk vanuit de `context`, zoals in het volgende voorbeeld:

```javascript
context.bindings.msg = "Name passed to the function: " req.query.name;
```

Zie voor meer informatie, de [Queue storage-Uitvoerbinding](functions-bindings-storage-queue.md#output---javascript-example) verwijzing.

### <a name="c-class-library"></a>C\# klassebibliotheek

Bijwerken van de functie-methode voor het toevoegen van de volgende parameter voor de `Run` methodedefinitie:

```cs
[Queue("outqueue"),StorageAccount("MyStorageConnection")] ICollector<string> msg
```

Deze code, moet u de volgende `using` instructie:

```cs
using Microsoft.Azure.WebJobs.Extensions.Storage;
```

De `msg` parameter is een `ICollector<T>` type, dat staat voor een verzameling van berichten die worden geschreven naar een Uitvoerbinding wanneer de functie is voltooid. U een of meer berichten toevoegen aan de verzameling, die naar de wachtrij worden verzonden wanneer de functie is voltooid.

Zie voor meer informatie, de [Queue storage-Uitvoerbinding](functions-bindings-storage-queue.md#output---c-example) verwijzing.

[!INCLUDE [Supported triggers and bindings](../../includes/functions-bindings.md)]

## <a name="publish-to-azure"></a>Publiceren naar Azure

In Visual Studio Code kunt u uw functieproject rechtstreeks in Azure publiceren. In dit proces maakt u een functie-app en de bijbehorende resources in uw Azure-abonnement. De functie-app biedt een context waar u uw functies kunt uitvoeren. Het project wordt in uw Azure-abonnement verpakt en geïmplementeerd in de nieuwe functie-app.

Bij het publiceren vanuit Visual Studio Code, zijn een van twee methoden voor het implementeren gebruikt:

* [Implementeren met het uitvoeren van pakket ingeschakeld ZIP](functions-deployment-technologies.md#zip-deploy): gebruikt voor de meeste implementaties van Azure Functions.
* [URL van de externe pakket](functions-deployment-technologies.md#external-package-url): gebruikt voor implementatie naar Linux-apps op een [verbruiksabonnement](functions-scale.md#consumption-plan).

### <a name="quick-function-app-creation"></a>Snelle functie-app maken

Visual Studio-Code genereert standaard automatisch waarden voor de Azure-resources die nodig zijn voor uw functie-app. Deze waarden zijn gebaseerd op de functie-app die u kiest. Zie voor een voorbeeld van het gebruik van standaard ingesteld op uw project publiceren naar een nieuwe functie-app in Azure, de [snelstartartikel voor Visual Studio Code](functions-create-first-function-vs-code.md#publish-the-project-to-azure).

Als u wilt expliciete namen voor de gemaakte resources op te geven, moet u inschakelen met behulp van geavanceerde opties publiceren.

### <a name="enabled-publishing-with-advanced-create-options"></a>Ingeschakelde publiceren met geavanceerde opties maken

Zodat u controle over de instellingen die zijn gekoppeld aan het maken van het Azure Functions apps, bij te werken van de extensie Azure Functions om geavanceerde instellingen te schakelen.

1. Klik op **bestand > Voorkeuren > instellingen**

1. Navigeren door **gebruikersinstellingen > extensies > Azure Functions**

1. Schakel het selectievakje in voor **Azure-functie: Het maken van geavanceerde**

### <a name="publish-to-a-new-function-app-in-azure-with-advanced-creation"></a>Publiceren naar een nieuwe functie-app in Azure met het maken van geavanceerde

De volgende stappen uit uw project kunt publiceren naar een nieuwe functie-app gemaakt met behulp van geavanceerde opties maken.

1. In de **Azure: Functies** gebied, selecteer de implementeren naar functie-App-pictogram.

    ![Functie-toepassingsinstellingen](./media/functions-develop-vs-code/function-app-publish-project.png)

1. Als niet aangemeld, u wordt gevraagd te **aanmelden bij Azure**. U kunt ook **maken van een gratis Azure-account**. Na geslaagde zich heeft aangemeld vanuit de browser, Ga terug naar Visual Studio Code.

1. Als u meerdere abonnementen hebt **Selecteer een abonnement** voor de functie-app, kies **+ nieuwe functie-App maken in Azure**.

1. Geef de volgende informatie de aanwijzingen te volgen:

    | Vraag | Value | Description |
    | ------ | ----- | ----------- |
    | Selecteer de functie-app in Azure | + Nieuwe functie-App maken in Azure | Typ een unieke naam ter identificatie van uw nieuwe functie-app in de volgende prompt en druk op Enter. Geldige tekens voor de naam van en functie-app zijn `a-z`, `0-9` en `-`. |
    | Selecteer een besturingssysteem | Windows | Functie-app wordt uitgevoerd op Windows |
    | Selecteer een hostingplan | Verbruiksabonnement | Serverloze [hosting via een Verbruiksabonnement](functions-scale.md#consumption-plan) wordt gebruikt. |
    | Selecteer een runtime voor uw nieuwe app | De projecttaal van uw | De runtime moet overeenkomen met het project dat u publiceert. |
    | Selecteer een resourcegroep voor nieuwe resources | Nieuwe resourcegroep maken | In de volgende prompt, typ de naam van een resourcegroep, zoals `myResourceGroup`, en druk op enter. U kunt ook een bestaande resourcegroep. |
    | Selecteer een opslagaccount | Nieuw opslagaccount maken | In de volgende prompt, typ een unieke naam van het nieuwe opslagaccount die wordt gebruikt door uw functie-app en druk op Enter. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account. |
    | Selecteer een locatie voor nieuwe resources | regio | Kies een locatie in een [regio](https://azure.microsoft.com/regions/) bij u in de buurt of in de buurt van andere services die door uw functies worden gebruikt. |

    Nadat de functie-app is gemaakt en het implementatiepakket is toegepast, wordt er een melding weergegeven. Selecteer **weergave uitvoer** in deze melding om het maken en de Implementatieresultaten weer te geven, met inbegrip van de Azure-resources die u hebt gemaakt.

## <a name="republish-project-files"></a>Soubory projektu publiceren

Bij het instellen van [continue implementatie](functions-continuous-deployment.md), uw functie-app in Azure wordt telkens bijgewerkt wanneer er bronbestanden worden bijgewerkt in de locatie van de verbonden. Wanneer u wordt aangeraden deze praktijk ontwikkelen, kunt u ook updates van bestanden van uw project in Visual Studio Code opnieuw publiceren. 

> [!IMPORTANT]
> Als u in een bestaande functie-app publiceert, wordt de inhoud van die app in Azure overschreven.

1. Druk op F1 voor het opdrachtenpalet te openen in Visual Studio Code. In het opdrachtenpalet, zoek en selecteer `Azure Functions: Deploy to function app...`.

1. Als niet aangemeld, u wordt gevraagd te **aanmelden bij Azure**. Na geslaagde zich heeft aangemeld vanuit de browser, Ga terug naar Visual Studio Code. Als u meerdere abonnementen hebt **Selecteer een abonnement** die uw functie-app bevat.

1. Kies uw bestaande functie-app in Azure. Wanneer een waarschuwing over het overschrijven van alle bestanden in de functie-app, kies **implementeren** te bevestigen van de waarschuwing en doorgaan. 

Het project is opnieuw opgebouwd, samen met verpakt en geüpload naar Azure. Het bestaande project wordt vervangen door het nieuwe pakket en de functie-app opnieuw wordt opgestart.

## <a name="get-deployed-function-url"></a>Geïmplementeerde functie-URL ophalen

Als u een door HTTP geactiveerde functie aanroepen, moet u de URL van de functie als op uw functie-app is geïmplementeerd. Deze URL bevat alle vereiste [functietoetsen](functions-bindings-http-webhook.md#authorization-keys). De extensie kunt u deze URL's ophalen voor uw geïmplementeerde functies.

1. Druk op F1-sleutel voor het opdrachtenpalet te openen en vervolgens zoekt en voer de opdracht **Azure Functions: Kopieer de URL van de functie**.

1. Volg de aanwijzingen voor het kiezen van uw functie-app in Azure en vervolgens de specifieke HTTP-trigger die u wilt aanroepen. 

De functie URL wordt gekopieerd naar het Klembord, samen met eventuele vereiste sleutels doorgegeven met behulp van de `code` queryparameter. Een HTTP-hulpprogramma gebruiken om in te dienen POST-aanvragen of een browser voor GET-aanvragen voor de RAS-functie.  

## <a name="run-functions-locally"></a>Functies lokaal uitvoeren

De extensie Azure Functions kunt u een functions-project uitvoeren op uw lokale ontwikkelcomputer. De lokale runtime is de dezelfde runtime die als host fungeert voor uw functie-app in Azure. Lokale instellingen worden gelezen uit de [bestand local.settings.json](#local-settings-file).

### <a name="additional-requirements-to-run-locally"></a>Aanvullende vereisten voor het lokaal uitvoeren

Als u wilt uitvoeren van uw Functions-project lokaal, u moet ook voldoen aan deze aanvullende vereisten:

* Installeer versie 2.x van de [Azure Functions Core Tools](functions-run-local.md#v2). De Core Tools-pakket wordt gedownload en geïnstalleerd voor de automatisch wanneer u het project lokaal start. De Core-hulpprogramma's omvatten de gehele Azure Functions-runtime, zodat het downloaden en de installatie kunnen enige tijd duren.

* Installeer de specifieke vereisten voor de taal van uw keuze:

    | Taal | Vereiste |
    | -------- | --------- |
    | **C#** | [C#-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)<br/>[.NET core CLI-hulpprogramma 's](https://docs.microsoft.com/dotnet/core/tools/?tabs=netcore2x)   |
    | **Java** | [Foutopsporing voor Java-extensie](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug)<br/>[Java 8](https://aka.ms/azure-jdks)<br/>[Maven 3+](https://maven.apache.org/) |
    | **JavaScript** | [Node.js](https://nodejs.org/)<sup>*</sup> |  
    | **Python** | [Python-extensie](https://marketplace.visualstudio.com/items?itemName=ms-python.python)<br/>[Python 3.6+](https://www.python.org/downloads/)|

    <sup>*</sup>Actieve LTS en onderhoud LTS versies (8.11.1 en 10.14.1 aanbevolen).

### <a name="configure-the-project-to-run-locally"></a>Configureren van het project lokaal uitvoeren

De Functions-runtime wordt een Azure Storage-account intern gebruikt voor alle triggers van het type dan HTTP en webhooks. Dit betekent dat u moet de **Values.AzureWebJobsStorage** sleutel op een geldige verbindingsreeks voor Azure Storage-account.

In deze sectie wordt de [Azure Storage-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurestorage) met [Microsoft Azure Storage Explorer](https://storageexplorer.com/) verbinding maken met en het ophalen van de verbindingsreeks voor opslag.   

De verbindingsreeks voor opslag instellen:

1. Open in Visual Studio, **Cloud Explorer**, vouw **Opslagaccount** > **uw Storage-Account**en selecteer vervolgens **eigenschappen**en kopieer de **Primary Connection String** waarde.

2. In uw project, open het bestand local.settings.json en stel de waarde van de **AzureWebJobsStorage** sleutel op de verbindingstekenreeks die u hebt gekopieerd.

3. Herhaal de vorige stap om toe te voegen unieke sleutels zijn aan de **waarden** matrix voor alle andere verbindingen die vereist zijn door uw functies.

Zie voor meer informatie, [lokale instellingenbestand](#local-settings-file).

### <a name="debugging-functions-locally"></a>Lokaal functies voor foutopsporing  

Druk op F5 om op te sporen uw functies. Als u dit nog niet hebt gedownload de [Core Tools][Azure Functions Core Tools], wordt u gevraagd om dit te doen. Wanneer Core-hulpprogramma's is geïnstalleerd en uitgevoerd, uitvoer wordt weergegeven in de Terminal. Dit is hetzelfde als het uitvoeren `func host start` Core Tools-opdracht uit vanaf de Terminal, maar met extra taken en een gekoppelde foutopsporingsprogramma bouwen.  

Met het project dat wordt uitgevoerd, kunt u uw functies activeren, net als bij de implementatie op Azure. Bij het uitvoeren in de foutopsporingsmodus, worden de onderbrekingspunten druk in Visual Studio Code, zoals verwacht.

De aanvraag-URL voor HTTP-triggers wordt weergegeven in de uitvoer in de terminal. Functietoetsen voor HTTP-triggers worden niet gebruikt bij het lokaal uitvoeren. Zie voor meer informatie, [strategieën voor het testen van uw code in Azure Functions](functions-test-a-function.md).  

Zie voor meer informatie, [werken met Azure Functions Core Tools][Azure Functions Core Tools].

[!INCLUDE [functions-local-settings-file](../../includes/functions-local-settings-file.md)]

Standaard worden deze instellingen niet gemigreerd automatisch wanneer het project wordt gepubliceerd naar Azure. Nadat het publiceren is voltooid, krijgt u de mogelijkheid om instellingen van local.settings.json naar uw functie-app publiceren in Azure. Zie voor meer informatie, [publicatie-toepassingsinstellingen](#publish-application-settings).

De waarden in **ConnectionStrings** nooit worden gepubliceerd.

De waarden voor de functie toepassingen instellingen kunnen ook worden gelezen in uw code als omgevingsvariabelen. Zie de sectie van de variabelen voor de omgeving van de volgende artikelen taalspecifieke verwijzing voor meer informatie:

* [C# vooraf geschreven](functions-dotnet-class-library.md#environment-variables)
* [C# script (.csx)](functions-reference-csharp.md#environment-variables)
* [Java](functions-reference-java.md#environment-variables)
* [JavaScript](functions-reference-node.md#environment-variables)

## <a name="application-settings-in-azure"></a>Toepassingsinstellingen in Azure

De instellingen in het bestand local.settings.json in uw project moet hetzelfde zijn als de toepassingsinstellingen in de functie-app in Azure. Alle instellingen die u aan de local.settings.json toevoegt moeten ook worden toegevoegd aan de functie-app in Azure. Deze instellingen worden niet automatisch geüpload wanneer u het project publiceren. Op dezelfde manier de instellingen die u in uw functie-app maakt [in de portal](functions-how-to-use-azure-function-app-settings.md#settings) naar uw lokale project moet worden gedownload.

### <a name="publish-application-settings"></a>Publicatie-toepassingsinstellingen

De eenvoudigste manier om de vereiste instellingen publiceren naar uw functie-app in Azure is met de **instellingen uploaden** koppeling die wordt weergegeven nadat u uw project is gepubliceerd.

![Implementatie voltooid toepassingsinstellingen uploaden](./media/functions-develop-vs-code/upload-app-settings.png)

U kunt ook instellingen publiceren met behulp van de `Azure Functions: Upload Local Setting` opdracht in de command palette. Afzonderlijke instellingen worden toegevoegd aan de toepassingsinstellingen in Azure met behulp van de `Azure Functions: Add New Setting...` opdracht. 

> [!TIP]
> Zorg ervoor dat het bestand local.settings.json opslaan voordat u deze publiceert.

Als het lokale bestand is versleuteld, wordt dit ontsleuteld, gepubliceerd en opnieuw versleuteld. Als instellingen met verschillende waarden op beide locaties bestaat, wordt u gevraagd om te kiezen hoe u om door te gaan.

Bestaande app-instellingen in de **Azure: Functies** gebied door het uitbreiden van uw abonnement, uw functie-app en **toepassingsinstellingen**.

![Weergave functie app-instelling in Visual Studio Code](./media/functions-develop-vs-code/view-app-settings.png)

### <a name="download-settings-from-azure"></a>Instellingen voor downloaden van Azure

Als u de toepassingsinstellingen in Azure hebt gemaakt, kunt u ze in het bestand local.settings.json downloaden. met behulp van de `Azure Functions: Download Remote Settings...` opdracht. 

Net als bij uploaden, als het lokale bestand is versleuteld, is het ontsleuteld, bijgewerkt en opnieuw versleuteld. Als instellingen met verschillende waarden op beide locaties bestaat, wordt u gevraagd om te kiezen hoe u om door te gaan.

## <a name="monitoring-functions"></a>Functions controleren

Wanneer u [lokaal uitvoeren](#run-functions-locally), logboekgegevens naar de Terminal-console wordt gestreamd. U kunt ook logboekgegevens ophalen wanneer uw functions-project wordt uitgevoerd in een functie-app in Azure. U een verbinding maken met het streaming-Logboeken in Azure om te zien van near-real-time-logboekgegevens, of u kunt Application Insights inschakelen voor een volledig begrip van hoe het gedrag van uw functie-app.

### <a name="streaming-logs"></a>Streaminglogboeken

Tijdens het ontwikkelen van een toepassing, is het vaak nuttig om te zien van logboekgegevens in near-real-time. Hier vindt u een stream van logboekbestanden wordt gegenereerd door uw functies. De volgende uitvoer is een voorbeeld van het streaming-logboeken voor een aanvraag naar een HTTP-geactiveerde functie:

![Streaminglogboeken uitvoer voor HTTP-trigger](media/functions-develop-vs-code/streaming-logs-vscode-console.png) 

Zie voor meer informatie, [Streaminglogboeken](functions-monitoring.md#streaming-logs). 

[!INCLUDE [functions-enable-log-stream-vs-code](../../includes/functions-enable-log-stream-vs-code.md)]

> [!NOTE]
> Streaminglogboeken ondersteunen slechts één exemplaar van de host functies. Wanneer uw functie wordt geschaald naar meerdere exemplaren, worden gegevens van andere exemplaren niet weergegeven in het logboekstream. De [Live Metrics Stream](../azure-monitor/app/live-stream.md) in Application Insights meerdere exemplaren ondersteund. Terwijl u ook in bijna real-time, Stream analytics zijn ook gebaseerd op [voorbeeldgegevens](functions-monitoring.md#configure-sampling).

### <a name="application-insights"></a>Application Insights

De aanbevolen manier voor het bewaken van de uitvoering van uw functies is door uw functie-app integreren met Azure Application Insights. Wanneer u een functie-app in Azure portal maakt, wordt deze integratie voor u uitgevoerd door standaard. Echter, wanneer u uw functie-app tijdens het publiceren van Visual Studio maakt, is niet de integratie in uw functie-app in Azure uitgevoerd.

[!INCLUDE [functions-connect-new-app-insights.md](../../includes/functions-connect-new-app-insights.md)]

Zie voor meer informatie, [Monitor Azure Functions](functions-monitoring.md).

## <a name="c-script-projects"></a>C\# script projecten

Standaard alle C# projecten worden gemaakt als [ C# gecompileerd class library-projecten](functions-dotnet-class-library.md). Als u liever in plaats daarvan werkt met C# projecten met een script, moet u C# script op als de standaard-taal in de instellingen van de extensie Azure Functions.

1. Klik op **bestand > Voorkeuren > instellingen**.

1. Navigeren door **gebruikersinstellingen > extensies > Azure Functions**.

1. Kies **C #Script** van **Azure-functie: Project taal**.

Op dit moment oproepen naar de onderliggende Core-hulpprogramma's omvatten de `--csx` optie, die wordt gegenereerd en publiceert C# script (.csx) projectbestanden. Met een standaardtaal is opgegeven, alle projecten standaard gemaakt C# script projecten. U wordt niet gevraagd een projecttaal kiezen als een standaard is ingesteld. Andere taal om projecten te maken, moet u deze instelling wijzigen of verwijderen uit het bestand settings.json. Nadat u deze instelling verwijdert, kunt u wordt opnieuw gevraagd uw taal kiezen wanneer u een project maakt.

## <a name="command-palette-reference"></a>Naslaginformatie voor palet

De extensie Azure Functions biedt een handige grafische interface in het gebied van Azure voor interactie met uw functie-apps in Azure. Dezelfde functionaliteit is ook beschikbaar als de opdrachten in het opdrachtenpalet (F1). De volgende Azure Functions-specifieke opdrachten zijn beschikbaar:

|Azure Functions-opdracht  | Description  |
|---------|---------|
|**Nieuwe instellingen toevoegen...**  |  Hiermee maakt u een nieuwe toepassingsinstelling in Azure. Zie voor meer informatie, [publicatie-toepassingsinstellingen](#publish-application-settings). U moet mogelijk ook [downloaden van deze instelling naar uw lokale instellingen](#download-settings-from-azure). |
| **Implementatiebron configureren...** | Uw functie-app verbinden met een lokale Git-opslagplaats. Zie voor meer informatie, [continue implementatie voor Azure Functions](functions-continuous-deployment.md). |
| **Verbinding maken met GitHub-opslagplaats...** | Verbind uw functie-app een GitHub-opslagplaats. |
| **Functie-URL kopiëren** | Haalt de externe URL van een HTTP geactiveerde functie die wordt uitgevoerd in Azure. Zie voor meer informatie over het [de geïmplementeerde functie-URL ophalen](#get-deployed-function-url). |
| **Functie-app in Azure maken...** | Maakt een nieuwe functie-app in uw abonnement in Azure. Zie voor meer informatie over het [publiceren naar een nieuwe functie-app in Azure](#publish-to-azure).        |
| **Instellingen voor ontsleutelen** | Gebruikt voor het decoderen [lokale instellingen](#local-settings-file) die zijn gecodeerd met behulp van `Azure Functions: Encrypt Settings`.  |
| **Functie-App verwijderen...** | Hiermee verwijdert u een bestaande functie-app uit uw abonnement in Azure. Wanneer er geen andere apps in de App Service-plan zijn, krijgt u de optie die te verwijderen. Andere resources, zoals storage-accounts en -resourcegroepen, worden niet verwijderd. Als u wilt verwijderen van alle resources, moet u in plaats daarvan [verwijderen van de resourcegroep](functions-add-output-binding-storage-queue-vs-code.md#clean-up-resources). Uw lokale project wordt niet beïnvloed. |
|**Functie verwijderen...**  | Hiermee verwijdert u een bestaande functie van een functie-app in Azure. Omdat deze verwijdering heeft geen invloed op uw lokale-project, in plaats daarvan kunt u de functie lokaal verwijderen en vervolgens [opnieuw publiceren van uw project](#republish-project-files). |
| **Proxy verwijderen...** | Hiermee verwijdert u een Azure Functions-proxy van de functie-app in Azure. Zie voor meer informatie over proxy's, [werken met Azure Functions-proxy's](functions-proxies.md). |
| **Instelling verwijderen...** | Hiermee verwijdert u de instelling voor een functie-toepassingen in Azure. Heeft geen invloed op de instellingen in het bestand local.settings.json. |
| **Opslagplaats verbreken...**  | Verwijder de [continue implementatie](functions-continuous-deployment.md) verbinding tussen een functie-app in Azure en een opslagplaats voor bronbeheer. |
| **Instellingen voor externe verbindingen downloaden...** | Instellingen van de gekozen functie-app in Azure in het bestand local.settings.json gedownload. Als het lokale bestand is versleuteld, wordt dit ontsleuteld, bijgewerkt en opnieuw versleuteld. Als instellingen met verschillende waarden op beide locaties bestaat, wordt u gevraagd om te kiezen hoe u om door te gaan. Zorg ervoor dat u wijzigingen hebt opgeslagen in het bestand local.settings.json voordat u deze opdracht uitvoert. |
| **Instellingen bewerken...** | Hiermee wijzigt de waarde van een bestaande functie toepassingsinstelling in Azure. Heeft geen invloed op de instellingen in het bestand local.settings.json.  |
| **Instellingen voor versleutelen** | Versleutelt afzonderlijke items in de `Values` matrix in de [lokale instellingen](#local-settings-file). In dit bestand `IsEncrypted` ook is ingesteld op `true`, waarin staat dat de lokale runtime voor het ontsleutelen van instellingen voordat u ze gebruikt. Lokale instellingen zodat u vermindert het risico van het lekken van waardevolle informatie voor het versleutelen. In Azure versleuteld toepassing instellingen altijd worden opgeslagen. |
| **De functie nu uitvoeren** | Start een [door een timer geactiveerde functie](functions-bindings-timer.md) in Azure voor test-doeleinden handmatig. Zie voor meer informatie over het activeren van de niet-HTTP-functies in Azure, [handmatig uitvoeren van een niet HTTP-geactiveerde functie](functions-manually-run-non-http.md). |
| **Project voor gebruik met VS Code initialiseren...** | Voegt de vereiste bestanden voor Visual Studio Code-project toe aan een bestaand Functions-project. Met deze opdracht gebruiken om te werken met een project die hebt gemaakt met behulp van Core Tools. |
| **Installatie van Update Azure Functions Core Tools** | Wordt geïnstalleerd of werkt u de [Azure Functions Core Tools] die worden gebruikt voor het lokaal uitvoeren. |
| **Redeploy**  | Hiermee kunt u projectbestanden vanuit een verbonden Git-opslagplaats naar een specifieke implementatie in Azure implementeren. Om opnieuw te publiceren van lokale updates van Visual Studio Code, [opnieuw publiceren van uw project](#republish-project-files). |
| **Wijzig de naam van instellingen...** | Wijzigt de naam van de sleutel van een bestaande functie toepassingsinstelling in Azure. Heeft geen invloed op de instellingen in het bestand local.settings.json. Na de naam van de instellingen in Azure, moet u [downloaden van deze wijzigingen aan het lokale project](#download-settings-from-azure). |
| **Opnieuw opstarten** | Start de functie-app in Azure. Implementeren van updates ook de functie-app wordt opnieuw opgestart. |
| **Set AzureWebJobStorage...**| Hiermee stelt u de waarde van de `AzureWebJobStorage` toepassingsinstelling. Deze instelling is vereist voor Azure functions en is ingesteld wanneer de functie-app wordt gemaakt in Azure. |
| **Beginnen** | Start een gestopte functie-app in Azure. | 
| **Logboeken streamen** | Start de stream-logboeken voor de functie-app in Azure. Streaminglogboeken gebruiken tijdens het externe probleemoplossing in Azure als u wilt deze informatie zien in near-real-time. Zie voor meer informatie, [Streaminglogboeken](#streaming-logs). |
| **Stoppen** | Afgesloten omlaag een functie-app in Azure. |
| **Streaminglogboeken stoppen** | Stopt de stream-logboeken voor de functie-app in Azure. |
| **In-/ uitschakelen als site-instelling** | Wanneer dit is ingeschakeld, zorgt dat een toepassingsinstelling voor een bepaald implementatiesleuf zich blijft voordoen. |
| **Azure Functions Core Tools verwijderen** | Hiermee verwijdert u de Azure Functions Core Tools, die nodig is voor de extensie. |
| **Lokale instellingen uploaden...** | Instellingen van het bestand local.settings.json naar de gekozen functie-app in Azure uploadt. Als het lokale bestand is versleuteld, wordt dit ontsleuteld, geüpload en worden nogmaals versleuteld. Als instellingen met verschillende waarden op beide locaties bestaat, wordt u gevraagd om te kiezen hoe u om door te gaan. Zorg ervoor dat u wijzigingen hebt opgeslagen in het bestand local.settings.json voordat u deze opdracht uitvoert. |
| **Weergave doorvoeren in GitHub** | Ziet u de meest recente doorvoeringen in een specifieke implementatie wanneer uw functie-app is verbonden met een opslagplaats. |
| **Implementatielogboeken weergeven** | Ziet u de logboeken voor een specifieke implementatie aan de functie-app in Azure. |

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over de Azure Functions Core Tools, [Code en Azure functions lokaal testen](functions-run-local.md).

Zie voor meer informatie over het ontwikkelen van functies als .NET-klassebibliotheken, [Azure Functions C#-naslaginformatie](functions-dotnet-class-library.md). In dit artikel bevat ook koppelingen naar voorbeelden van hoe u kunt gebruikmaken van kenmerken om aan te geven van de verschillende typen bindingen die worden ondersteund door Azure Functions.    

[Azure Functions extension for Visual Studio Code]: https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions (Azure Functions-extensie voor Visual Studio Code)
[Azure Functions Core Tools]: functions-run-local.md