---
title: Prestaties en schaalbaarheid in duurzame functies - Azure
description: Inleiding tot de extensie duurzame functies voor Azure Functions.
services: functions
author: cgillum
manager: jeconnoc
keywords: ''
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 04/25/2018
ms.author: azfuncdf
ms.openlocfilehash: 54a88188a432a23476af6a1670635a23fb72eea7
ms.sourcegitcommit: c8088371d1786d016f785c437a7b4f9c64e57af0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/30/2018
ms.locfileid: "52643143"
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Prestaties en schaalbaarheid in duurzame functies (Azure Functions)

Voor het optimaliseren van prestaties en schaalbaarheid, is het belangrijk te begrijpen van de unieke vergroten/verkleinen kenmerken van [duurzame functies](durable-functions-overview.md).

Voor meer informatie over het gedrag van de schaal, moet u enkele van de details van de onderliggende Azure Storage-provider.

## <a name="history-table"></a>Tabel met de geschiedenis

De **geschiedenis** tabel is een Azure Storage-tabel met de van geschiedenisgebeurtenissen voor alle orchestration-exemplaren in een hub-taak. De naam van deze tabel is in het formulier *TaskHubName*geschiedenis. Als de exemplaren worden uitgevoerd, worden nieuwe rijen zijn toegevoegd aan deze tabel. De partitiesleutel van deze tabel is afgeleid van de exemplaar-ID van de indeling. Exemplaar-ID wordt een willekeurige in de meeste gevallen, waardoor de optimale distributie van interne partities in Azure Storage.

Wanneer een exemplaar van de indeling moet worden uitgevoerd, worden de juiste rijen van de tabel met de geschiedenis in het geheugen geladen. Deze *geschiedenisgebeurtenissen* vervolgens opnieuw worden afgespeeld in de orchestrator-functiecode voor deze toegang krijgen tot de vorige controlepunt staat. Het gebruik van uitvoeringsgeschiedenis te bouwen staat op deze manier wordt beïnvloed door de [patroon gebeurtenisbronnen](https://docs.microsoft.com/azure/architecture/patterns/event-sourcing).

## <a name="instances-table"></a>Exemplaren tabel

De **exemplaren** tabel is een andere Azure Storage-tabel met de status van alle orchestration-exemplaren in een hub-taak. Als de exemplaren worden gemaakt, worden nieuwe rijen zijn toegevoegd aan deze tabel. De partitiesleutel van deze tabel is de orchestration-exemplaar-ID en de rijsleutel is een vaste constante. Er is één rij per orchestration-exemplaar.

Deze tabel wordt gebruikt om te voldoen aan de queryaanvragen exemplaar van de [GetStatusAsync](https://azure.github.io/azure-functions-durable-extension/api/Microsoft.Azure.WebJobs.DurableOrchestrationClient.html#Microsoft_Azure_WebJobs_DurableOrchestrationClient_GetStatusAsync_System_String_) API, evenals de [status query HTTP API](https://docs.microsoft.com/azure/azure-functions/durable-functions-http-api#get-instance-status). Deze opgeslagen uiteindelijk consistent zijn met de inhoud van de **geschiedenis** tabel eerder is vermeld. Het gebruik van een afzonderlijke Azure Storage-tabel om te voorzien in efficiënt querybewerkingen exemplaar op deze manier wordt beïnvloed door de [patroon Command and Query Responsibility Segregation (CQRS)](https://docs.microsoft.com/azure/architecture/patterns/cqrs).

## <a name="internal-queue-triggers"></a>Interne queue-triggers

Orchestrator-functies en activiteitsfuncties worden door interne wachtrijen in de functie-app taak hub geactiveerd. Met behulp van wachtrijen op deze manier biedt betrouwbare 'op-één keer' message delivery garanties. Er zijn twee typen wachtrijen in duurzame functies: de **besturingselement wachtrij** en de **werkitem wachtrij**.

### <a name="the-work-item-queue"></a>De wachtrij van het werkitem

Er is een werkitem wachtrij per taak hub in duurzame functies. Er is een eenvoudige wachtrij en gedraagt zich op dezelfde manier naar een andere `queueTrigger` wachtrij in Azure Functions. Deze wachtrij wordt gebruikt voor het activeren van staatloze *activiteitsfuncties* door dequeueing een enkel bericht op een tijdstip. Elk van deze berichten bevat activiteit functie-invoer en aanvullende metagegevens, zoals welke functie uit te voeren. Wanneer een duurzame functies toepassing geschaald naar meerdere virtuele machines, concurreren deze alle VM's aan te schaffen werk uit de wachtrij van het werkitem.

### <a name="control-queues"></a>Besturingselement wachtrij(en)

Er zijn meerdere *wachtrijen beheren* per taak hub in duurzame functies. Een *besturingselement wachtrij* is meer geavanceerde dan de eenvoudiger werkitem wachtrij. Besturingselement wachtrijen worden gebruikt voor het activeren van de stateful orchestrator-functies. Omdat de orchestrator-functie-exemplaren stateful singletons zijn, is het niet mogelijk een concurrerende consumenten-model gebruiken om u te verdelen over virtuele machines. In plaats daarvan zijn orchestrator berichten met load balancing in de besturingselement-wachtrijen. Meer informatie over dit probleem kunnen worden gevonden in de volgende secties.

Besturingselement wachtrijen bevatten verschillende typen orchestration lifecycle bericht. Voorbeelden zijn onder meer [orchestrator besturingsberichten](durable-functions-instance-management.md), activiteit functie *antwoord* berichten en berichten van de timer. Maximaal 32 berichten wordt uit de wachtrij worden verwijderd uit de wachtrij van een besturingselement in een enkele poll. Deze berichten worden gegevens over de nettolading en metagegevens zoals welke orchestration-instantie die is bedoeld voor bevatten. Als meerdere dequeued berichten zijn bedoeld voor hetzelfde exemplaar van de orchestration, worden ze als batch worden verwerkt.

## <a name="storage-account-selection"></a>Storage-account selecteren

De wachtrijen, tabellen en blobs gebruikt door duurzame functies worden gemaakt door in een geconfigureerde Azure Storage-account. Het account moet worden gebruikt, kan worden opgegeven met behulp van de `durableTask/azureStorageConnectionStringName` instellen in **host.json** bestand.

```json
{
  "durableTask": {
    "azureStorageConnectionStringName": "MyStorageAccountAppSetting"
  }
}
```

Indien niet opgegeven, de standaard `AzureWebJobsStorage` storage-account wordt gebruikt. Voor prestatie-intensieve workloads, wordt configureren van een niet-standaard-storage-account echter aanbevolen. Duurzame functies intensief gebruikt Azure Storage en opslaggebruik duurzame functies van het interne gebruik door de Azure Functions-host met behulp van een toegewezen storage-account worden geïsoleerd.

## <a name="orchestrator-scale-out"></a>Orchestrator-uitschalen

Activiteitsfuncties zijn staatloze en geschaalde automatisch door virtuele machines toe te voegen. Orchestrator-functies, aan de andere kant zijn *gepartitioneerde* via een of meer wachtrijen voor controle. Het aantal wachtrijen besturingselement is gedefinieerd in de **host.json** bestand. Het volgende voorbeeld host.json codefragment wordt de `durableTask/partitionCount` eigenschap `3`.

```json
{
  "durableTask": {
    "partitionCount": 3
  }
}
```
Een taak hub kan worden geconfigureerd met tussen 1 en 16 partities. Indien niet opgegeven, wordt het standaardaantal-partities is **4**.

Bij het uitschalen naar meerdere exemplaren van de functie host (meestal op verschillende VM's), krijgt elk exemplaar een vergrendeling op een van de controle-wachtrijen. Deze vergrendelingen intern worden geïmplementeerd als blob-opslag-leases en zorg ervoor dat een orchestration-exemplaar alleen wordt uitgevoerd op een afzonderlijke host-exemplaar op een tijdstip. Als een taak hub is geconfigureerd met drie besturingselement wachtrijen, kunnen orchestration-exemplaren worden met load balancing op maximaal drie VM's. Extra virtuele machines kunnen worden toegevoegd om de capaciteit voor de uitvoering van activiteit-functie te vergroten.

Het volgende diagram illustreert hoe de Azure Functions-host communiceert met de storage-entiteiten in een geschaalde omgeving.

![Schaal van diagram](./media/durable-functions-perf-and-scale/scale-diagram.png)

Zoals weergegeven in het vorige diagram, worden alle virtuele machines concurreren om berichten in de wachtrij van het werkitem. Echter, alleen voor de drie VM's berichten uit wachtrijen besturingselement kunnen verkrijgen en elke virtuele machine Hiermee vergrendelt u een wachtrij één besturingselement.

De orchestration-exemplaren worden verdeeld over alle exemplaren van besturingselement wachtrij. De distributie wordt uitgevoerd door de exemplaar-ID van de orchestration-hashing. Exemplaar-id's standaard zijn willekeurige GUID's, waarbij u ervoor zorgt dat exemplaren evenredig worden verdeeld over alle controle-wachtrijen.

In het algemeen orchestrator-functies zijn bedoeld om te worden lichtgewicht en grote hoeveelheden rekenkracht vereist. Het is daarom niet nodig om te maken van een groot aantal besturingselement wachtrij partities om goede doorvoer te krijgen. De meeste van het zware werk moet worden uitgevoerd in activiteitsfuncties van staatloze oneindig kunnen worden uitgebreid.

## <a name="auto-scale"></a>Automatisch schalen

Als met alle Azure-functies die worden uitgevoerd in het abonnement Consumption, duurzame functies biedt ondersteuning voor automatisch schalen via de [Azure Functions schalen controller](../functions-scale.md#runtime-scaling). De Controller schaal bewaakt de latentie van alle wachtrijen door uitgifte van periodiek _peek_ opdrachten. Op basis van de latenties van de peeked berichten, bepaalt de schaal Controller of u wilt toevoegen of verwijderen van virtuele machines.

Als de domeincontroller van de schaal wordt vastgesteld dat besturingselement wachtrij bericht latenties te hoog zijn, wordt het VM-exemplaren toevoegen totdat de bericht-latentie verlaagd naar een aanvaardbaar niveau of wordt het aantal partities van besturingselement wachtrij bereikt. Op deze manier wordt de schaal Controller VM-exemplaren voortdurend toevoegen als werkitem wachtrij latenties hoge, ongeacht het aantal partities zijn.

## <a name="thread-usage"></a>Gebruik de thread

Orchestrator-functies worden uitgevoerd op een enkele thread om ervoor te zorgen dat kan worden uitgevoerd in veel replays deterministische kan zijn. Vanwege deze één thread wordt uitgevoerd is het belangrijk dat threads van orchestrator-functie niet CPU-intensieve taken uitvoeren, i/o doen of voor een bepaalde reden blokkeren. Werk waarvoor i/o, blokkeren, of meerdere threads in de activiteitsfuncties moeten worden verplaatst.

Activiteitsfuncties hebben hetzelfde gedrag als gewone wachtrij-geactiveerde functies. Ze kunnen veilig doen i/o, CPU-intensieve bewerkingen uitvoeren en meerdere threads. Omdat activiteit triggers staatloos zijn, kunnen ze vrij schaal vergroten tot een niet-gebonden aantal virtuele machines.

## <a name="concurrency-throttles"></a>Gelijktijdigheid vertragingen in

Azure Functions ondersteunt meerdere functies tegelijk binnen een enkele app-exemplaar wordt uitgevoerd. Deze gelijktijdige uitvoering van parallelle uitvoering, verhoogt en minimaliseert het aantal 'koude start' die te met een normale app na verloop van tijd maken. Hoge gelijktijdigheid kan echter resulteren in geheugengebruik hoog per VM. Afhankelijk van de behoeften van de functie-app, is het mogelijk dat het nodig om te beperken de gelijktijdigheid van de per-exemplaar om te voorkomen dat de mogelijkheid om geheugentekort in hoge belasting situaties.

Beide activiteit functie en orchestrator gelijktijdigheidsbeperkingen voor de functie kunnen worden geconfigureerd de **host.json** bestand. De relevante instellingen zijn `durableTask/maxConcurrentActivityFunctions` en `durableTask/maxConcurrentOrchestratorFunctions` respectievelijk.

```json
{
  "durableTask": {
    "maxConcurrentActivityFunctions": 10,
    "maxConcurrentOrchestratorFunctions": 10,
  }
}
```

In het vorige voorbeeld, kan maximaal 10 orchestrator en 10 activiteitsfuncties tegelijkertijd op één virtuele machine. Indien niet opgegeven, het aantal gelijktijdige uitvoeringen voor activiteit en de orchestrator-functie wordt beperkt tot 10 X het aantal kernen op de virtuele machine.

> [!NOTE]
> Deze instellingen zijn handig voor het beheer van geheugen en CPU-gebruik op een enkele virtuele machine. Wanneer u met deze methode wordt uitgeschaald over meerdere virtuele machines, wordt elke virtuele machine echter een eigen set beperkingen hebben. Deze instellingen kunnen niet worden gebruikt voor het beheren van gelijktijdigheid op algemeen niveau.

## <a name="orchestrator-function-replay"></a>Orchestrator-functie opnieuw afspelen
Zoals eerder vermeld, orchestrator-functies zijn opnieuw afgespeeld met behulp van de inhoud van de **geschiedenis** tabel. Standaard wordt de orchestrator-functiecode replay telkens wanneer een berichtenbatch uit de wachtrij van een besturingselement uit de wachtrij zijn geplaatst.

Dit gedrag agressief opnieuw afspelen kan worden uitgeschakeld door in te schakelen **uitgebreide sessies**. Wanneer uitgebreide sessies zijn ingeschakeld, wordt orchestrator-functie-exemplaren worden bewaard in het geheugen langere en nieuwe berichten kunnen worden verwerkt zonder een volledige opnieuw afspelen. Uitgebreide sessies moeten worden ingeschakeld door in te stellen `durableTask/extendedSessionsEnabled` naar `true` in de **host.json** bestand. De `durableTask/extendedSessionIdleTimeoutInSeconds` instelling wordt gebruikt om te bepalen hoe lang een niet-actieve sessies wordt gehouden in het geheugen:

```json
{
  "durableTask": {
    "extendedSessionsEnabled": true,
    "extendedSessionIdleTimeoutInSeconds": 30
  }
}
```

Het typische effect van het inschakelen van uitgebreide sessies wordt verminderd i/o op basis van de Azure Storage-account en de algehele verbeterde doorvoer.

Een mogelijke nadeel van deze functie is echter dat niet-actieve orchestrator-functie exemplaren in het geheugen langer blijft. Er zijn twee effecten rekening mee moet houden:

1. Totale toename van de functie-app-geheugengebruik.
2. Totale afname in doorvoer als er veel gelijktijdige, tijdelijke orchestrator-functie-uitvoeringen.

Een voorbeeld: als `durableTask/extendedSessionIdleTimeoutInSeconds` is ingesteld op 30 seconden, de aflevering van een eenvoudige orchestrator-functie die wordt uitgevoerd in minder dan 1 seconde wordt nog steeds geheugen innemen gedurende 30 seconden. Deze ook's tellen mee voor de `durableTask/maxConcurrentOrchestratorFunctions` quotum gezegd, mogelijk te voorkomen dat andere orchestrator-functies die worden uitgevoerd.

> [!NOTE]
> Deze instellingen moeten alleen worden gebruikt wanneer een orchestrator-functie is volledig ontwikkeld en getest. Het standaardgedrag agressief opnieuw afspelen is handig voor het detecteren van idempotentie fouten in de orchestrator-functies tijdens het ontwikkelen.

## <a name="performance-targets"></a>Prestatiedoelen

Bij het plannen van duurzame functies gebruiken voor een productietoepassing, is het belangrijk dat u rekening houden met de prestatievereisten al vroeg in het planningsproces. Deze sectie worden enkele eenvoudige gebruiksscenario's en de verwachte maximale doorvoer getallen.

* **Sequentiële activiteit uitvoering**: in dit scenario beschrijft een orchestrator-functie die wordt uitgevoerd een reeks activiteitsfuncties een na de andere. Het beste past bij de [functie Chaining](durable-functions-sequence.md) voorbeeld.
* **Parallelle uitvoering van activiteit**: in dit scenario beschrijft een orchestrator-functie die wordt uitgevoerd veel activiteitsfuncties in met behulp van parallelle de [fanout, Fan-in](durable-functions-cloud-backup.md) patroon.
* **Parallelle verwerking van certificaatantwoord**: in dit scenario is de tweede helft van de [fanout, Fan-in](durable-functions-cloud-backup.md) patroon. Dit artikel gaat over de prestaties van de fan-in. Het is belangrijk te weten dat in tegenstelling tot fanout, fan-in wordt uitgevoerd door een exemplaar van de functie één orchestrator, en daarom kan alleen worden uitgevoerd op een enkele virtuele machine.
* **Externe gebeurtenisverwerking**: in dit scenario vertegenwoordigt een instantie van één orchestrator-functie die wacht op [externe gebeurtenissen](durable-functions-external-events.md), één voor één.

> [!TIP]
> In tegenstelling tot fanout zijn fan-in operations beperkt tot een enkele virtuele machine. Als uw toepassing het fanout fan-in patroon gebruikt en u zich zorgen maakt over de prestaties van fan-in, kunt u de functie activiteit fanout subplan verdelen over meerdere [onderliggende indelingen](durable-functions-sub-orchestrations.md).

De volgende tabel ziet u de verwachte *maximale* doorvoerwaarden voor de eerder beschreven scenario's. 'Instantie' verwijst naar één exemplaar van een orchestrator-functie die wordt uitgevoerd op een enkele kleine ([A1](../../virtual-machines/windows/sizes-previous-gen.md#a-series)) virtuele machine in Azure App Service. In alle gevallen wordt ervan uitgegaan dat [uitgebreide sessies](#orchestrator-function-replay) zijn ingeschakeld. De werkelijke resultaten kunnen variëren, afhankelijk van de CPU of i/o werkzaamheden van de functiecode aan te geven.

| Scenario | Maximumdoorvoer |
|-|-|
| Sequentiële activiteit kan worden uitgevoerd | 5 activiteiten per seconde per exemplaar |
| Parallelle activiteit worden uitgevoerd (fanout) | 100 activiteiten per seconde per exemplaar |
| Parallelle verwerking van certificaatantwoord (fan-in) | 150 antwoorden per seconde per exemplaar |
| Externe gebeurtenissen verwerken | 50 gebeurtenissen per seconde, per exemplaar |

> [!NOTE]
> Deze getallen zijn actueel op bovenstaande de v1.4.0 (GA) versie van de extensie duurzame functies. Deze getallen kan na verloop van tijd worden gewijzigd als de functie zich verder ontwikkelt en optimalisaties worden aangebracht.

Als er niet de verwachte doorvoerwaarden en de CPU en geheugengebruik, in orde wordt weergegeven, controleert u of de oorzaak is gerelateerd aan [de status van uw opslagaccount](../../storage/common/storage-monitoring-diagnosing-troubleshooting.md#troubleshooting-guidance). De duurzame functies extensie aanzienlijke plaatsen kunt belasting van een Azure Storage-account en voldoende hoge belasting kunnen resulteren in de beperking van de storage-account.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw eerste duurzame functie in makenC#](durable-functions-create-first-csharp.md)
