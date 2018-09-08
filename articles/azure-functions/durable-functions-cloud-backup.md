---
title: Fan-uitgaand/fan-in scenario's in duurzame functies - Azure
description: Informatie over het implementeren van een scenario fan-uitgaand-ventilator-in in de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 03/19/2018
ms.author: azfuncdf
ms.openlocfilehash: eec75ad9cf0f568e674b2a4f12d962982f84294f
ms.sourcegitcommit: af60bd400e18fd4cf4965f90094e2411a22e1e77
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44092662"
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Fan-uitgaand/fan-in-scenario in duurzame functies - back-voorbeeld van de Cloud

*Fan-uitgaand/fan-in* verwijst naar het patroon van meerdere functies gelijktijdig uitgevoerd en vervolgens een aggregatie uit te voeren op de resultaten. Dit artikel wordt uitgelegd voor een voorbeeld dat gebruikmaakt van [duurzame functies](durable-functions-overview.md) voor het implementeren van een fan-in/fan-uitgaand scenario. Het voorbeeld is een duurzame functie die back-ups van alle of een deel van de site-inhoud van een app in Azure Storage.

## <a name="prerequisites"></a>Vereisten

* [Duurzame functies installeren](durable-functions-install.md).
* Voltooi de [Hello reeks](durable-functions-sequence.md) scenario.

## <a name="scenario-overview"></a>Overzicht van scenario's

In dit voorbeeld wordt met de functies alle bestanden onder een opgegeven map recursief uploaden naar blob-opslag. Tellen wordt ook het totale aantal bytes dat zijn geüpload.

Het is mogelijk één functie die zorgt voor van alles wat schrijven. Het belangrijkste probleem dat u het volgende uit in is **schaalbaarheid**. Uitvoering van één functie kan alleen uitgevoerd op een enkele virtuele machine, zodat de doorvoer wordt beperkt door de doorvoer van dat één virtuele machine. Een ander probleem is **betrouwbaarheid**. Als er een fout halverwege, of als het hele proces langer dan vijf minuten duurt, de back-up kan niet in een status gedeeltelijk voltooid. Er moet opnieuw worden gestart.

Een meer robuuste benadering zou zijn om twee reguliere functies te schrijven: één zou het inventariseren van de bestanden en voeg de bestandsnamen toe aan een wachtrij en een andere zou lezen uit de wachtrij en de bestanden uploaden naar blob-opslag. Dit is het beter wat betreft de doorvoer en betrouwbaarheid, maar u kunt inrichten en beheren van een wachtrij moet. Nog belangrijker, aanzienlijke complexiteit is geïntroduceerd in termen van **management status** en **coördinatie** als u doen meer wilt, zoals rapport het totale aantal bytes geüpload.

Een duurzame functies aanpak biedt u alle voordelen van de genoemde met weinig overhead.

## <a name="the-functions"></a>De functies

Dit artikel wordt uitgelegd dat de volgende functies in de voorbeeld-app:

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

De volgende secties worden de configuratie en de code die worden gebruikt voor C#-scripts. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.

## <a name="the-cloud-backup-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>De cloud back-indeling (Visual Studio Code en Azure portal voorbeeldcode)

De `E2_BackupSiteContent` functie gebruikmaakt van de standaard *function.json* voor orchestrator-functies.

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/function.json)]

Dit is de code die de orchestrator-functie implementeert:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/run.csx)]

### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_BackupSiteContent/index.js)]

Deze functie orchestrator doet in feite het volgende:

1. Neemt een `rootDirectory` waarde als een invoerparameter.
2. Roept een functie om op te halen van een recursieve lijst met bestanden onder `rootDirectory`.
3. Maakt meerdere parallelle functieaanroepen naar elk bestand uploaden naar Azure Blob Storage.
4. Wacht tot alle uploads om te voltooien.
5. Retourneert de som totaal aantal bytes die zijn geüpload naar Azure Blob Storage.

U ziet dat de `await Task.WhenAll(tasks);` (C#) en `yield context.df.Task.all(tasks);` regel (JS). Alle aanroepen naar de `E2_CopyFileToBlob` functie zijn *niet* gestopt. Dit is opzettelijk zodat ze parallel worden uitgevoerd. Wanneer we deze reeks taken doorgeven `Task.WhenAll`, krijgen we weer een taak die wordt niet voltooid *totdat alle kopieerbewerkingen hebt voltooid*. Als u ervaring hebt met de taak parallelle bibliotheek (TPL) in .NET, dan dit geen nieuw voor u is. Het verschil is dat deze taken mogelijk gelijktijdig op meerdere virtuele machines worden uitgevoerd en de extensie duurzame functies zorgt ervoor dat de end-to-end-uitvoering tegen recyclen van processen is.

Taken zijn vergelijkbaar met de JavaScript-concept van beloften. Echter, `Promise.all` heeft enkele verschillen van `Task.WhenAll`. Het concept van `Task.WhenAll` is overgebracht via als onderdeel van de `durable-functions` JavaScript-module en is exclusief voor deze.

Na in afwachting van `Task.WhenAll` (of uit wordt vrijgegeven `context.df.Task.all`), we weten dat alle aanroepen van de functie hebt voltooid en waarden terug naar ons zijn geretourneerd. Elke aanroep van `E2_CopyFileToBlob` retourneert het aantal bytes geüpload, zodat het berekenen van de som totaal aantal bytes-telling is een kwestie van het toevoegen van alle waarden samen retourneren.

## <a name="helper-activity-functions"></a>Activiteit hulpfuncties

De activiteit hulpfuncties net als bij andere voorbeelden zijn alleen normale functies die gebruikmaken van de `activityTrigger` binding activeren. Bijvoorbeeld, de *function.json* voor het bestand `E2_GetFileList` er als volgt uit:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/function.json)]

En de implementatie als volgt:

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/run.csx)]

### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_GetFileList/index.js)]

De JavaScript-uitvoering van `E2_GetFileList` maakt gebruik van de `readdirp` module recursief moeten lezen de mapstructuur.

> [!NOTE]
> U kunt vraagt zich misschien af waarom u kan niet alleen plaats deze code rechtstreeks in de orchestrator-functie. U kunt, maar deze strijdig is met een van de fundamentele regels van orchestrator-functies, is dat ze nooit i/o, inclusief lokale toegang tot bestandssysteem moeten doen.

De *function.json* voor het bestand `E2_CopyFileToBlob` is op deze manier eenvoudig:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/function.json)]

De C#-implementatie is ook eenvoudig. Dit gebeurt met het gebruik van enkele geavanceerde functies van Azure Functions-Bindingen (dat wil zeggen, het gebruik van de `Binder` parameter), maar u hoeft zich geen zorgen over de gegevens ten behoeve van dit scenario te.

### <a name="c"></a>C#

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/run.csx)]

### <a name="javascript-functions-v2-only"></a>JavaScript (alleen functies v2)

De JavaScript-uitvoering heeft geen toegang tot de `Binder` functie van Azure Functions, zodat de [Azure Storage SDK voor Node](https://github.com/Azure/azure-storage-node) de plaatsvindt. Houd er rekening mee dat de SDK vereist een `AZURE_STORAGE_CONNECTION_STRING` app-instelling.

[!code-javascript[Main](~/samples-durable-functions/samples/javascript/E2_CopyFileToBlob/index.js)]

De uitvoering wordt het bestand wordt geladen vanaf schijf en asynchroon streamt de inhoud naar een blob met dezelfde naam in de container 'back-ups'. De geretourneerde waarde is het aantal bytes dat is gekopieerd naar de opslag, die vervolgens wordt gebruikt door de orchestrator-functie voor het berekenen van het totaal.

> [!NOTE]
> Dit is een goed voorbeeld van het verplaatsen van i/o-bewerkingen in een `activityTrigger` functie. Niet alleen kan worden het werk verdeeld over veel verschillende virtuele machines, maar ook profiteren van de voordelen van het plaatsen van controlepunten de voortgang. Als het hostproces opgehaald beëindigd voor een bepaalde reden, weet u welke uploads al hebt voltooid.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

U kunt de indeling starten door de volgende HTTP POST-aanvraag te verzenden.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> De `HttpStart` -functie die u aanroept werkt alleen met inhoud voor JSON-indeling. Om deze reden de `Content-Type: application/json` -header is vereist en het pad naar de map is gecodeerd als een JSON-tekenreeks.

Triggers voor deze HTTP-aanvraag de `E2_BackupSiteContent` orchestrator en geeft de tekenreeks `D:\home\LogFiles` als parameter. Het antwoord bevat een koppeling om de status van de back-upbewerking ophalen:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Deze bewerking kan enkele minuten duren, afhankelijk van hoeveel logboekbestanden in uw functie-app hebt. U kunt de meest recente status krijgen door het uitvoeren van query's de URL in de `Location` -header van het vorige HTTP 202-antwoord.

```
GET http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}
```

```
HTTP/1.1 202 Accepted
Content-Length: 148
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

{"runtimeStatus":"Running","input":"D:\\home\\LogFiles","output":null,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:16Z"}
```

In dit geval wordt wordt de functie nog steeds uitgevoerd. U zijn kunt zien van de invoer die is opgeslagen in de orchestrator-status en de tijd voor laatst bijgewerkt. U kunt echter ook doorgaan met de `Location` headerwaarden worden gecontroleerd op voltooiing. Als de status 'voltooid' is, ziet u een HTTP-antwoord-waarde die vergelijkbaar is met het volgende:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:26Z"}
```

U kunt nu zien dat de indeling voltooid is en ongeveer hoeveel tijd het duurde om te voltooien. U ziet ook een waarde voor de `output` veld, waarmee wordt aangegeven dat ongeveer 450 KB van Logboeken zijn geüpload.

## <a name="visual-studio-sample-code"></a>Visual Studio-voorbeeldcode

Hier volgt de indeling als één C#-bestand in een Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs)]

## <a name="next-steps"></a>Volgende stappen

In dit voorbeeld heeft laten zien hoe u de fan-uitgaand/fan-in patroon wilt implementeren. Het volgende voorbeeld laat zien hoe voor het implementeren van de monitor patroon via [duurzame timers](durable-functions-timers.md).

> [!div class="nextstepaction"]
> [De monitor-voorbeeld uitvoeren](durable-functions-monitor.md)
