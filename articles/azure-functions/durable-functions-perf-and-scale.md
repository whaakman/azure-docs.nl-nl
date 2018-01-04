---
title: Prestaties en schaalbaarheid in duurzame functies - Azure
description: Inleiding tot de extensie duurzame functies voor Azure Functions.
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
ms.openlocfilehash: cc4c643b8d0e8de1b5c38ca7bb1b0193d6b0f05b
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="performance-and-scale-in-durable-functions-azure-functions"></a>Prestaties en schaalbaarheid in duurzame functies (Azure-functies)

Om te optimaliseren prestaties en schaalbaarheid, is het belangrijk te begrijpen van de unieke vergroten/verkleinen kenmerken van [duurzame functies](durable-functions-overview.md).

Voor informatie over het gedrag van de schaal, hoeft u te begrijpen enkele van de details van de onderliggende Azure Storage-provider die wordt gebruikt door duurzame functies.

## <a name="history-table"></a>Geschiedenistabel

De geschiedenistabel is een Azure Storage-tabel die de van geschiedenisgebeurtenissen voor alle exemplaren van de orchestration bevat. Als exemplaren worden uitgevoerd, worden nieuwe rijen aan deze tabel toegevoegd. De partitiesleutel van deze tabel is afgeleid van de exemplaar-ID van de orchestration. Deze waarden zijn in de meeste gevallen willekeurige waardoor optimale distributie van interne partities in Azure Storage.

## <a name="internal-queue-triggers"></a>Interne queue-triggers

Orchestrator-functies en functies van de activiteit worden door interne wachtrijen in de functie-app standaardopslagaccount geactiveerd. Er zijn twee typen van wachtrijen in duurzame functies: de **besturingselement wachtrij** en de **werkitem-wachtrij**.

### <a name="the-work-item-queue"></a>De wachtrij werkitem

Er is een werkitem-wachtrij per taak hub in duurzame functies. Dit is een eenvoudige wachtrij en gedraagt zich op dezelfde manier naar een andere `queueTrigger` wachtrij in Azure Functions. Deze wachtrij wordt gebruikt voor het activeren van staatloze *activiteit functies*. Wanneer een toepassing duurzame functies uitgeschaald naar meerdere virtuele machines, concurreren deze alle virtuele machines te verkrijgen van de werkitem-wachtrij te werken.

### <a name="control-queues"></a>Besturingselement wachtrij(en)

De *besturingselement wachtrij* meer geavanceerde dan de eenvoudiger werkitem-wachtrij is. Wordt gebruikt voor het activeren van de stateful orchestrator-functies. Omdat de exemplaren van de functie orchestrator stateful singletons, is het niet mogelijk een concurrerende consumenten-model gebruiken om u te verdelen over virtuele machines. In plaats daarvan zijn orchestrator berichten taakverdeling binnen meerdere besturingselement wachtrijen. Meer informatie over dit in de volgende secties.

Besturingselement wachtrijen bevatten verschillende typen orchestration lifecycle bericht. Voorbeelden zijn onder meer [orchestrator besturingselement berichten](durable-functions-instance-management.md), activiteit functie *antwoord* berichten en berichten timer.

## <a name="orchestrator-scale-out"></a>Orchestrator scale-out

Functies van de activiteit zijn staatloze en automatisch uitbreiden door virtuele machines toe te voegen. Orchestrator-functies, aan de andere kant zijn *gepartitioneerde* via een of meer wachtrijen voor beheer. Het aantal wachtrijen voor besturingselement is vast en kan niet worden gewijzigd nadat u bent begonnen met het maken van belasting.

Wanneer uitbreiden naar meerdere exemplaren van de functie host (meestal op een andere VM's), krijgt elk exemplaar een vergrendeling op een van de besturingselement-wachtrijen. Deze vergrendeling zorgt ervoor dat een exemplaar van de orchestration alleen wordt uitgevoerd op een enkele virtuele machine op een tijdstip. Dit betekent dat als een taak hub is geconfigureerd met drie besturingselement wachtrijen, orchestration-exemplaren verdeeld over maximaal drie VM's worden kunnen. Extra virtuele machines kunnen worden toegevoegd aan het vergroot de capaciteit voor de activiteit een functie wordt uitgevoerd.  Maar de aanvullende bronnen wordt niet gebruikt voor het uitvoeren van de orchestrator-functies.

Het volgende diagram illustreert de interactie van de Azure Functions-host met de opslag-entiteiten in een omgeving met geschaalde uit.

![Schaaldiagram](media/durable-functions-perf-and-scale/scale-diagram.png)

Zoals u ziet, kunnen alle virtuele machines voor berichten in de wachtrij werkitem-concurreren. Echter slechts drie virtuele machines berichten uit besturingselement wachtrijen kunnen verkrijgen en elke VM Hiermee vergrendelt u een wachtrij één besturingselement.

Orchestration-exemplaren zijn verdeeld over besturingselement wachtrij exemplaren door te voeren van een interne hash-functie op de orchestration-serverexemplaar-ID. Exemplaar-id's zijn automatisch gegenereerd en willekeurige standaard dat ervoor zorgt dat exemplaren zijn verdeeld zijn over alle beschikbare besturingselement wachtrijen. De huidige standaardaantal ondersteunde besturingselement wachtrij partities is **4**.

> [!NOTE]
> Het is niet op dit moment mogelijk is het aantal partities voor besturingselement wachtrij configureren in Azure Functions. [Werk ter ondersteuning van deze configuratieoptie wordt bijgehouden](https://github.com/Azure/azure-functions-durable-extension/issues/73).

In het algemeen orchestrator-functies zijn bedoeld als lichtgewicht en veel rekenkracht niet nodig. Daarom is het niet nodig zijn voor het maken van een groot aantal besturingselement wachtrij partities hoge doorvoersnelheid ophalen. In plaats daarvan wordt meeste zware werk gedaan in functies van staatloze activiteit oneindig kunnen worden uitgebreid.

## <a name="auto-scale"></a>Automatisch schalen

Als met alle Azure-functies uitvoeren in het plan verbruik, duurzame functies automatisch geschaald via ondersteunen de [Azure Functions schalen controller](https://docs.microsoft.com/azure/azure-functions/functions-scale#runtime-scaling). De Controller Scale controleert de lengte van het werkitem-wachtrij en elk van de besturingselement-wachtrijen, toevoegen of verwijderen van VM-instanties dienovereenkomstig. Als de lengte van de wachtrij besturingselement gedurende een bepaalde periode stijgt zijn, blijft de controller scale VM-instanties toevoegen totdat het aantal partities van besturingselement wachtrij bereikt. Als lengten van werkitem-wachtrij na verloop van tijd stijgt worden, blijft de controller scale VM-instanties toevoegen totdat deze kan overeenkomen met de belasting, ongeacht het aantal partities van besturingselement wachtrij.

## <a name="thread-usage"></a>Gebruik van de thread

Orchestrator-functies worden uitgevoerd op een enkele thread. Dit is vereist om ervoor te zorgen dat de uitvoering van de orchestrator-functie deterministisch is. In deze context houden is het belangrijk dat nooit behouden de orchestrator-functie thread onnodig bezig is met taken, zoals het i/o (dit is niet toegestaan voor een aantal redenen), blokkeren of draaiende-bewerkingen. Werk waarvoor i/o, blokkeren, of meerdere threads moeten worden verplaatst naar de functies van de activiteit.

Functies van de activiteit hebben hetzelfde gedrag als gewone wachtrij-geactiveerde functies. Dit betekent dat ze kunnen veilig doen i/o, uitvoeren van gegevensintensieve bewerkingen van CPU en gebruik van meerdere threads. Omdat activiteit triggers staatloze, uitbreiden ze vrijelijk naar een niet-gebonden aantal virtuele machines.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Installeer de duurzame functies extensie en voorbeelden](durable-functions-install.md)
