---
title: Fan-uitgaand/fan-in scenario's in duurzame functies - Azure
description: Informatie over het implementeren van een scenario fan-uitgaand-ventilator-in in de uitbreiding duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: 
tags: 
keywords: 
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 09/29/2017
ms.author: azfuncdf
ms.openlocfilehash: a5d539172f03246e3c658f2485d29d3ae389ae52
ms.sourcegitcommit: a48e503fce6d51c7915dd23b4de14a91dd0337d8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2017
---
# <a name="fan-outfan-in-scenario-in-durable-functions---cloud-backup-example"></a>Fan-uitgaand/fan-in-scenario in duurzame functies - Cloud back-voorbeeld

*Fan-uitgaand/fan-in* verwijst naar het patroon van meerdere functies tegelijk uitvoeren en vervolgens een aggregatie uit te voeren op de resultaten. Dit artikel wordt uitgelegd voor een voorbeeldtoepassing die gebruikmaakt van [duurzame functies](durable-functions-overview.md) een fan-in/fan-uitgaand-scenario implementeren. Het voorbeeld is een duurzame functie die een back-up alle of enkele van de site-inhoud van een app in Azure Storage.

## <a name="prerequisites"></a>Vereisten

* Volg de instructies in [duurzame functies installeren](durable-functions-install.md) voor het instellen van het voorbeeld.
* In dit artikel wordt ervan uitgegaan dat u al hebt doorlopen de [Hello Sequence](durable-functions-sequence.md) voorbeeld scenario.

## <a name="scenario-overview"></a>Overzicht van scenario's

In dit voorbeeld wordt uploaden de functies alle bestanden onder een opgegeven map recursief naar de blobopslag. Ze tellen ook het totale aantal bytes dat is geüpload.

Het is mogelijk één functie die voor alles zorgt schrijven. Het belangrijkste probleem zou is **schaalbaarheid**. De uitvoering van een enkele functie kan alleen uitgevoerd op een enkele virtuele machine, zodat de doorvoer wordt beperkt door de doorvoer van die één virtuele machine. Een ander probleem is **betrouwbaarheid**. Als er een fout halverwege via, of als het hele proces langer dan vijf minuten duurt, de back-up in een gedeeltelijk voltooide status kan mislukken. Vervolgens moet er opnieuw worden gestart.

Een meer robuuste benadering zou zijn om twee reguliere functies te schrijven: één zou opsommen van de bestanden en voeg de bestandsnamen toe aan een wachtrij en een andere zou lezen uit de wachtrij en de bestanden uploaden naar blob storage. Dit is het beter in termen van de doorvoer en betrouwbaarheid, maar het vereist dat u voor het inrichten en beheren van een wachtrij. Belangrijker aanzienlijke complexiteit wordt geïntroduceerd in termen van **management status** en **coördinatie** als u wilt verder te doen, zoals rapport het totale aantal bytes geüpload.

Een benadering duurzame functies kunt u alle voordelen van de genoemde met weinig overhead.

## <a name="the-functions"></a>De functies

Dit artikel wordt uitgelegd dat de volgende functies in de voorbeeld-app:

* `E2_BackupSiteContent`
* `E2_GetFileList`
* `E2_CopyFileToBlob`

De volgende secties worden de configuratie en code die worden gebruikt voor het ontwikkelen van Azure portal. De code voor het ontwikkelen van Visual Studio wordt weergegeven aan het einde van het artikel.

## <a name="the-cloud-backup-orchestration-visual-studio-code-and-azure-portal-sample-code"></a>De cloud back-orchestration (Visual Studio Code en de Azure portal voorbeeldcode)

De `E2_BackupSiteContent` functie maakt gebruik van de standaard *function.json* voor orchestrator-functies.

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/function.json)]

Dit is de code die de orchestrator-functie implementeert:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_BackupSiteContent/run.csx)]

Deze functie voor orchestrator in wezen doet het volgende:

1. Neemt een `rootDirectory` waarde als invoerparameter.
2. Een functie om een recursieve lijst van bestanden onder aangeroepen `rootDirectory`.
3. Maakt meerdere parallelle functieaanroepen voor het uploaden van elk bestand in Azure Blob Storage.
4. Wacht tot alle uploads te voltooien.
5. Retourneert de som totaal aantal bytes dat is geüpload naar Azure Blob Storage.

U ziet de `await Task.WhenAll(tasks);` regel. Alle aanroepen voor de `E2_CopyFileToBlob` functie zijn *niet* afgewacht. Dit is opzettelijk zodat ze parallel worden uitgevoerd. Wanneer we deze matrix van taken aan doorgeven `Task.WhenAll`, krijgen we weer een taak die won't voltooien *tot alle kopieerbewerkingen hebt voltooid*. Als u ervaring hebt met de taak parallelle bibliotheek (TPL) in .NET, dan dit geen nieuw voor u is. Het verschil is dat deze taken mogelijk gelijktijdig op meerdere virtuele machines worden uitgevoerd en de extensie duurzame functies zorgt ervoor dat de end-to-end-uitvoering tegen recyclen van processen is.

Na het in afwachting van `Task.WhenAll`, we weten dat alle functieaanroepen zijn voltooid en back-waarden aan ons zijn geretourneerd. Elke aanroep van `E2_CopyFileToBlob` retourneert het aantal bytes geüpload, zodat toe te voegen die alle retourwaarden samen het berekenen van het aantal som totaal aantal bytes is.

## <a name="helper-activity-functions"></a>Activiteit hulpfuncties

Activiteit hulpfuncties net als bij andere voorbeelden zijn alleen normale functies die gebruikmaken van de `activityTrigger` binding activeren. Bijvoorbeeld, de *function.json* voor het bestand `E2_GetFileList` vergelijkbaar is met het volgende:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/function.json)]

En dit is de implementatie:

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_GetFileList/run.csx)]

> [!NOTE]
> U wellicht waarom u kan niet alleen plaats deze code rechtstreeks in de orchestrator-functie. U kunt, maar dit strijdig is met een van de fundamentele regels van orchestrator-functies, die is dat ze nooit i/o, inclusief toegang tot lokale bestandssysteem moeten doen.

De *function.json* voor het bestand `E2_CopyFileToBlob` op dezelfde manier eenvoudig:

[!code-json[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/function.json)]

De implementatie is ook heel eenvoudig. Dit gebeurt dat u een aantal geavanceerde functies van Azure Functions-Bindingen (dat wil zeggen, het gebruik van de `Binder` parameter), maar u hoeft niet te hoeven maken over de gegevens voor dit scenario.

[!code-csharp[Main](~/samples-durable-functions/samples/csx/E2_CopyFileToBlob/run.csx)]

De implementatie laadt het bestand van de schijf en asynchroon streams de inhoud naar een blob met dezelfde naam in de container 'back-ups'. De geretourneerde waarde is het aantal bytes dat wordt gekopieerd naar opslag, dat vervolgens wordt gebruikt door de orchestrator-functie het totaal te berekenen.

> [!NOTE]
> Dit is een voorbeeld van het verplaatsen van i/o-bewerkingen in een `activityTrigger` functie. Niet alleen kan het werk verdeeld over veel verschillende VM's, maar worden ook de voordelen van het plaatsen van controlepunten de voortgang. Als het hostproces opgehaald beëindigd om welke reden, weet u welke uploads al hebt voltooid.

## <a name="run-the-sample"></a>Het voorbeeld uitvoert

U kunt de orchestration starten door de volgende HTTP POST-aanvraag te verzenden.

```
POST http://{host}/orchestrators/E2_BackupSiteContent
Content-Type: application/json
Content-Length: 20

"D:\\home\\LogFiles"
```

> [!NOTE]
> De `HttpStart` functie die u aanroept werkt alleen met inhoud JSON-indeling. Daarom moet de `Content-Type: application/json` header is vereist en het pad naar de map is gecodeerd als een JSON-tekenreeks.

Triggers voor deze HTTP-aanvraag de `E2_BackupSiteContent` orchestrator en geeft de tekenreeks `D:\home\LogFiles` als parameter. Het antwoord bevat een koppeling om de status van de back-upbewerking:

```
HTTP/1.1 202 Accepted
Content-Length: 719
Content-Type: application/json; charset=utf-8
Location: http://{host}/admin/extensions/DurableTaskExtension/instances/b4e9bdcc435d460f8dc008115ff0a8a9?taskHub=DurableFunctionsHub&connection=Storage&code={systemKey}

(...trimmed...)
```

Deze bewerking kan enkele minuten duren, afhankelijk van hoeveel logboekbestanden hebt u in de functie-app. U kunt de nieuwste status ophalen door het opvragen van de URL in de `Location` header van het vorige 202 HTTP-antwoord.

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

De functie is nog actief. U zijn kunt zien van de ingang die is opgeslagen in de orchestrator-status en de laatste keer dat bijgewerkte. U kunt blijven gebruiken de `Location` headerwaarden poll-frequentie voor voltooiing. Wanneer de status 'voltooid' is, ziet u een waarde van HTTP-antwoord ziet er als volgt:

```
HTTP/1.1 200 OK
Content-Length: 152
Content-Type: application/json; charset=utf-8

{"runtimeStatus":"Completed","input":"D:\\home\\LogFiles","output":452071,"createdTime":"2017-06-29T18:50:55Z","lastUpdatedTime":"2017-06-29T18:51:26Z"}
```

U kunt nu zien dat de orchestration voltooid is en ongeveer hoeveel tijd het duurt om te voltooien. U ziet ook een waarde op voor de `output` veld waarmee wordt aangegeven dat ongeveer 450 KB van Logboeken zijn geüpload.

## <a name="visual-studio-sample-code"></a>Voorbeeldcode voor Visual Studio

Dit is de orchestration als één C#-bestand in Visual Studio-project:

[!code-csharp[Main](~/samples-durable-functions/samples/precompiled/BackupSiteContent.cs)]

## <a name="next-steps"></a>Volgende stappen

Dit voorbeeld is het implementeren van het fan-uitgaand/fan-in patroon weergegeven. Het volgende voorbeeld laat zien hoe voor het implementeren van de [stateful singleton](durable-functions-singletons.md) patroon een [eeuwige orchestration](durable-functions-eternal-orchestrations.md).

> [!div class="nextstepaction"]
> [De stateful singleton-voorbeeld uitvoeren](durable-functions-counter.md)
