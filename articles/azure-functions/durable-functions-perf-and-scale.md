---
title: Prestaties en schaalbaarheid in duurzame functies - Azure
description: Inleiding tot de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: cfowler
editor: ''
tags: ''
keywords: ''
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 110f393e723c7e784a4bd7e79559dd9d55147140
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34599429"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Prestaties en schaalbaarheid in duurzame functies (Azure-functies)

Om te optimaliseren prestaties en schaalbaarheid, is het belangrijk te begrijpen van de unieke vergroten/verkleinen kenmerken van [duurzame functies](durable-functions-overview.md).

Voor informatie over het gedrag van de schaal, hoeft u te begrijpen enkele van de details van de onderliggende provider voor Azure Storage.

## <a name="history-table"></a>Geschiedenistabel

De **geschiedenis** tabel is een Azure Storage-tabel die de van geschiedenisgebeurtenissen voor alle exemplaren van de orchestration binnen een taak hub bevat. De naam van deze tabel is in het formulier *TaskHubName*geschiedenis. Als exemplaren worden uitgevoerd, worden nieuwe rijen aan deze tabel toegevoegd. De partitiesleutel van deze tabel is afgeleid van de exemplaar-ID van de orchestration. Een exemplaar-ID wordt een willekeurige in de meeste gevallen waardoor optimale distributie van interne partities in Azure Storage.

Wanneer een orchestration-exemplaar moet worden uitgevoerd, worden de juiste rijen van de geschiedenistabel in het geheugen geladen. Deze *geschiedenisgebeurtenissen* vervolgens opnieuw en nu afgespeeld in de orchestrator-functiecode terug ophalen in de vorige controlepunt staat. Het gebruik van uitvoergeschiedenis opnieuw kunt opbouwen staat op deze manier wordt beïnvloed door de [gebeurtenis bron patroon](https://docs.microsoft.com/en-us/azure/architecture/patterns/event-sourcing).

## <a name="instances-table"></a>Tabel exemplaren

De **exemplaren** tabel is een andere Azure Storage-tabel die de status van alle exemplaren van de orchestration binnen een taak hub bevat. Nadat exemplaren zijn gemaakt, worden nieuwe rijen aan deze tabel toegevoegd. De partitiesleutel van deze tabel is de orchestration-exemplaar-ID en de rijsleutel is een vaste constante. Er is één rij per orchestration-exemplaar.

Deze tabel wordt gebruikt om te voldoen aan de queryaanvragen exemplaar van de [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_System_String_) API, evenals de [status query HTTP API](https://docs.microsoft.com/en-us/azure/azure-functions/durable-functions-http-api#get-instance-status). Het blijft uiteindelijk consistent is met de inhoud van de **geschiedenis** tabel eerder is vermeld. Het gebruik van een afzonderlijke tabel Azure Storage efficiënt voldoen aan de querybewerkingen exemplaar op deze manier wordt beïnvloed door de [opdracht en Query verantwoordelijkheid scheiding (CQRS) patroon](https://docs.microsoft.com/en-us/azure/architecture/patterns/cqrs).

## <a name="internal-queue-triggers"></a>Interne queue-triggers

Orchestrator-functies en functies van de activiteit worden door interne wachtrijen in de functie-app taak hub geactiveerd. Met behulp van wachtrijen op deze manier biedt betrouwbare 'op minste eenmaal'-bericht leveringsgaranties. Er zijn twee typen van wachtrijen in duurzame functies: de **besturingselement wachtrij** en de **werkitem-wachtrij**.

### <a name="the-work-item-queue"></a>De wachtrij werkitem

Er is een werkitem-wachtrij per taak hub in duurzame functies. Het is een eenvoudige wachtrij en gedraagt zich op dezelfde manier naar een andere `queueTrigger` wachtrij in Azure Functions. Deze wachtrij wordt gebruikt voor het activeren van staatloze *activiteit functies* door dequeueing één bericht tegelijk. Elk van deze berichten bevat activiteit functie ingangen en aanvullende metagegevens, zoals welke functie uit te voeren. Wanneer een toepassing duurzame functies uitgeschaald naar meerdere virtuele machines, concurreren deze alle virtuele machines te verkrijgen van de werkitem-wachtrij te werken.

### <a name="control-queues"></a>Besturingselement wachtrij(en)

Er zijn meerdere *wachtrijen bepalen* per taak hub in duurzame functies. Een *besturingselement wachtrij* meer geavanceerde dan de eenvoudiger werkitem-wachtrij is. Besturingselement wachtrijen worden gebruikt voor het activeren van de stateful orchestrator-functies. Omdat de exemplaren van de functie orchestrator stateful singletons, is het niet mogelijk een concurrerende consumenten-model gebruiken om u te verdelen over virtuele machines. In plaats daarvan zijn orchestrator berichten taakverdeling binnen de besturingselement-wachtrijen. In de volgende secties vindt u meer informatie over dit gedrag.

Besturingselement wachtrijen bevatten verschillende typen orchestration lifecycle bericht. Voorbeelden zijn onder meer [orchestrator besturingselement berichten](durable-functions-instance-management.md), activiteit functie *antwoord* berichten en berichten timer. Maximaal 32 berichten wordt uit de wachtrij van een besturingselement in een enkel poll uit de wachtrij worden geplaatst. Deze berichten bevatten de nettolading van gegevens en metagegevens, met inbegrip van welke het is bedoeld voor orchestration-exemplaar. Als meerdere dequeued berichten zijn bedoeld voor hetzelfde exemplaar van de orchestration, zullen ze als batch worden verwerkt.

## <a name="storage-account-selection"></a>Storage-account selecteren

De wachtrijen, tabellen en blobs die worden gebruikt door duurzame functies worden gemaakt door in een geconfigureerde Azure-opslagaccount. De account moet worden gebruikt, kan worden opgegeven met de `durableTask/azureStorageConnectionStringName` instellen in **host.json** bestand.

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
  }
}
```

Als niet wordt opgegeven, de standaard `AzureWebJobsStorage` storage-account wordt gebruikt. Prestatie-intensieve werkbelastingen wordt configureren van een niet-standaard opslagaccount echter aanbevolen. Duurzame functies Azure Storage intensief gebruikt en een speciaal opslagaccount gebruikt isoleert opslaggebruik duurzame functies van de intern gebruik door de Azure Functions-host.

## <a name="orchestrator-scale-out"></a>Orchestrator scale-out

Functies van de activiteit zijn staatloze en schaal uit automatisch door virtuele machines toe te voegen. Orchestrator-functies, aan de andere kant zijn *gepartitioneerde* via een of meer wachtrijen voor beheer. Het aantal wachtrijen voor besturingselement is gedefinieerd in de **host.json** bestand. Het volgende voorbeeld host.json codefragment wordt de `durableTask/partitionCount` eigenschap `3`.

```json
{
  "durableTask": {
    "partitionCount": 3
  }
}
```
Een taak hub kan worden geconfigureerd met tussen 1 en 16 partities. Als niet wordt opgegeven, is het het standaardaantal partitie **4**.

Wanneer uitbreiden naar meerdere exemplaren van de functie host (meestal op een andere VM's), krijgt elk exemplaar een vergrendeling op een van de besturingselement-wachtrijen. Deze vergrendelingen intern worden geïmplementeerd als blob-opslag-leases en zorg ervoor dat een exemplaar van de orchestration alleen wordt uitgevoerd op een voor één hostexemplaar tegelijk. Als een taak hub is geconfigureerd met drie besturingselement wachtrijen, kunnen orchestration exemplaren worden verdeeld over maximaal drie VM's. Extra virtuele machines kunnen worden toegevoegd aan het vergroot de capaciteit voor de activiteit een functie wordt uitgevoerd.

Het volgende diagram illustreert de interactie van de Azure Functions-host met de opslag-entiteiten in een omgeving met geschaalde uit.

![Schaaldiagram](media/durable-functions-perf-and-scale/scale-diagram.png)

Zoals u in het vorige diagram, worden alle virtuele machines voor berichten in de wachtrij werkitem-concurreren. Echter slechts drie virtuele machines berichten uit besturingselement wachtrijen kunnen verkrijgen en elke VM Hiermee vergrendelt u een wachtrij één besturingselement.

De orchestration-exemplaren zijn verdeeld over alle exemplaren van besturingselement wachtrij. De verdeling wordt uitgevoerd door de exemplaar-ID van de orchestration-hashing. Exemplaar-id's standaard zijn willekeurige GUID's, waarbij u ervoor zorgt dat exemplaren gelijkmatig zijn verdeeld over alle beheer-wachtrijen.

Normaal gesproken orchestrator-functies zijn bedoeld als lichtgewicht en geen grote hoeveelheid rekenkracht vereist. Het is daarom niet nodig voor het maken van een groot aantal besturingselement wachtrij partities hoge doorvoersnelheid ophalen. De meeste van de zwaar werk moet worden gedaan in functies van staatloze activiteit oneindig kunnen worden uitgebreid.

## <a name="auto-scale"></a>Automatisch schalen

Met alle uitgevoerd in het plan verbruik Azure Functions ondersteunt duurzame functies automatisch geschaald via de [Azure Functions schalen controller](functions-scale.md#runtime-scaling). De Controller Scale bewaakt de latentie van alle wachtrijen door uitgifte van periodiek _peek_ opdrachten. Op basis van de latenties peeked berichten, bepaalt de Controller Scale of toevoegen of verwijderen van virtuele machines.

Als de Scale-Controller vaststelt dat besturingselement wachtrij bericht latenties te hoog is, wordt het VM-instanties toevoegen totdat de latentie bericht naar een aanvaardbaar niveau afneemt of het aantal partities van besturingselement wachtrij heeft bereikt. Op deze manier wordt de Scale-Controller VM-instanties voortdurend toevoegen als werkitem-wachtrij latenties hoog, ongeacht het aantal partities zijn.

## <a name="thread-usage"></a>Gebruik van de thread

Orchestrator-functies worden uitgevoerd op een enkele thread om ervoor te zorgen dat kan worden uitgevoerd via veel replays deterministische kan worden. Vanwege deze één thread wordt uitgevoerd is het belangrijk dat orchestrator functie threads geen CPU-intensieve taken uitvoeren, i/o's bent of voor een of andere reden blokkeren. Werk waarvoor i/o, blokkeren, of meerdere threads moeten worden verplaatst naar de functies van de activiteit.

Functies van de activiteit hebben hetzelfde gedrag als gewone wachtrij-geactiveerde functies. Ze kunnen veilig doen i/o, uitvoeren van gegevensintensieve bewerkingen van CPU en gebruik van meerdere threads. Omdat activiteit triggers staatloze, uitbreiden ze vrijelijk naar een niet-gebonden aantal virtuele machines.

## <a name="concurrency-throttles"></a>Gelijktijdigheid van bandbreedte

Azure Functions ondersteunt meerdere functies tegelijk binnen een enkele app-exemplaar wordt uitgevoerd. Deze gelijktijdige uitvoering, verhoogt u parallelle uitvoering en wordt het aantal 'koude startprocedures' die gedurende een periode met een normale app krijgen beperkt. Hoge gelijktijdigheid kan echter resulteren in geheugengebruik hoog VM. Afhankelijk van de behoeften van de functie-app, is het mogelijk nodig om te beperken van de concurrency per exemplaar om te voorkomen dat de mogelijkheid van onvoldoende geheugen beschikbaar in situaties met hoge belasting.

Beide activiteit functie en de orchestrator functie gelijktijdigheid limieten kunnen worden geconfigureerd in de **host.json** bestand. De relevante instellingen zijn `durableTask/maxConcurrentActivityFunctions` en `durableTask/maxConcurrentOrchestratorFunctions` respectievelijk.

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
  }
}
```

In het vorige voorbeeld kunt maximaal 10 orchestrator en 10 activiteit functies uitvoeren op een enkele virtuele machine tegelijk. Als u niet opgeeft, wordt het aantal gelijktijdige activiteit en orchestrator functie uitvoeringen beperkt tot 10 X het aantal kernen op de virtuele machine.

> [!NOTE]
> Deze instellingen zijn nuttig voor het beheer van geheugen en CPU-gebruik op een enkele virtuele machine. Echter, wanneer uitgebreid over meerdere virtuele machines, elke virtuele machine hebben een eigen set set limieten. Deze instellingen kunnen niet worden gebruikt voor gelijktijdigheid van taken op algemeen niveau te beheren.

## <a name="orchestrator-function-replay"></a>Orchestrator-functie opnieuw afspelen
Zoals eerder vermeld, orchestrator-functies worden cookies met de inhoud van de **geschiedenis** tabel. Standaard wordt de functiecode orchestrator replay telkens wanneer een batch van berichten uit de wachtrij van een besturingselement uit de wachtrij worden geplaatst.

Dit gedrag agressieve replay kan worden uitgeschakeld door het inschakelen van **uitgebreide sessies**. Wanneer uitgebreide sessies zijn ingeschakeld, worden instanties van orchestrator-functie in het geheugen langer en nieuwe berichten kunnen worden verwerkt zonder een volledige replay bewaard. Uitgebreide sessies moeten worden ingeschakeld door in te stellen `durableTask/extendedSessionsEnabled` naar `true` in de **host.json** bestand. De `durableTask/extendedSessionIdleTimeoutInSeconds` instelling wordt gebruikt om te bepalen hoe lang een niet-actieve sessie wordt gehouden in het geheugen:

```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

Het typische effect uitgebreide sessies in te schakelen, is verminderd i/o op basis van de Azure Storage-account en de totale verbeterde doorvoer.

Een mogelijke nadeel van deze functie is echter die exemplaren in het geheugen die langer blijft inactief orchestrator-functie. Er zijn twee effecten moet weten:

1. Totale toename van de functie app-geheugengebruik.
2. De algemene minder doorvoer als er veel gelijktijdige, tijdelijke orchestrator functies wordt weergegeven.

Bijvoorbeeld als `durableTask/extendedSessionIdleTimeoutInSeconds` is ingesteld op 30 seconden, wordt de episode van een tijdelijke orchestrator-functie die wordt uitgevoerd in minder dan 1 seconde nog steeds geheugen gedurende 30 seconden bezet. Dit wordt ook in mindering gebracht op de `durableTask/maxConcurrentOrchestratorFunctions` quotum gezegd, waardoor mogelijk andere orchestrator-functies worden uitgevoerd.

> [!NOTE]
> Deze instellingen moeten alleen worden gebruikt nadat een orchestrator-functie is volledig ontwikkeld en getest. Het standaardgedrag agressieve replay is nuttig voor idempotency fouten opsporen in de orchestrator-functies op moment van ontwikkeling.

## <a name="performance-targets"></a>Prestatiedoelen

Bij het plannen van duurzame functies gebruiken voor een productietoepassing, is het belangrijk dat u kunt de prestatie-eisen vroeg in het planningsproces. Deze sectie worden enkele eenvoudige gebruiksscenario's en de verwachte maximale doorvoer getallen.

* **Sequentiële activiteit uitvoering**: dit scenario beschrijft een orchestrator-functie die wordt een reeks functies van de activiteit een na elkaar uitgevoerd. Het meest overeenkomt met de [functie Chaining](durable-functions-sequence.md) voorbeeld.
* **Parallelle uitvoering van de activiteit**: dit scenario beschrijft een orchestrator-functie die wordt uitgevoerd voor veel functies van de activiteit in met behulp van parallelle de [Fan-out, Fan-in](durable-functions-cloud-backup.md) patroon.
* **Parallelle verwerking van certificaatantwoord**: dit scenario is de tweede helft van de [Fan-out, Fan-in](durable-functions-cloud-backup.md) patroon. Dit artikel gaat over de prestaties van de fan-in. Het is belangrijk te weten dat in tegenstelling tot fan-out, fan-in wordt uitgevoerd door een exemplaar van de functie één orchestrator en daarom kan alleen worden uitgevoerd op een enkele virtuele machine.
* **Verwerking van de externe gebeurtenis**: dit scenario biedt een enkel orchestrator functie-exemplaar dat wordt gewacht op [externe gebeurtenissen](durable-functions-external-events.md), één voor één.

> [!TIP]
> In tegenstelling tot fan-out zijn fan-in operations beperkt tot één VM. Als uw toepassing de fan-out, fan-in patroon gebruikt en u zich zorgen maakt over de prestaties van fan-in, kunt u de functie activiteit fan-out subplan verdelen over meerdere [onderliggende integraties](durable-functions-sub-orchestrations.md).

De volgende tabel bevat de verwachte *maximale* doorvoer cijfers voor de eerder beschreven scenario's. "Exemplaar" verwijst naar één exemplaar van een orchestrator-functie die is uitgevoerd op een enkele kleine ([A1](../virtual-machines/windows/sizes-previous-gen.md#a-series)) virtuele machine in Azure App Service. In alle gevallen wordt ervan uitgegaan dat [uitgebreide sessies](#orchestrator-function-replay) zijn ingeschakeld. Werkelijke resultaten kunnen variëren afhankelijk van de CPU of i/o-werkzaamheden van de programmacode.

| Scenario | Maximale doorvoer |
|-|-|
| Sequentiële activiteit is uitgevoerd | 5 activiteiten per seconde per exemplaar |
| Parallelle activiteit is uitgevoerd (fan-out) | 100 activiteiten per seconde per exemplaar |
| Parallelle verwerking antwoord (fan-in) | 150 antwoorden per seconde per exemplaar |
| Externe gebeurtenisverwerking | 50 gebeurtenissen per seconde per exemplaar |

> [!NOTE]
> Deze getallen zijn huidige vanaf de release van v1.4.0 (GA) van de uitbreiding duurzame functies. Deze getallen kunnen worden gewijzigd na verloop van tijd wanneer de functie meer vormt krijgt en optimalisaties worden aangebracht.

Als u de doorvoer-nummers die u verwacht en de CPU niet ziet en geheugengebruik, in orde wordt weergegeven, controleert u of de oorzaak is gerelateerd aan [de status van uw opslagaccount](../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance). De duurzame functies uitbreiding aanzienlijke plaatsen kunt belasting van een Azure Storage-account en voldoende hoge belasting mogelijk leiden tot beperking van de storage-account.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Installeer de duurzame functies extensie en voorbeelden](durable-functions-install.md)
