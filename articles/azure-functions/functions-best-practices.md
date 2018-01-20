---
title: Aanbevolen procedures voor Azure Functions | Microsoft Docs
description: Informatie over aanbevolen procedures en patronen voor Azure Functions.
services: functions
documentationcenter: na
author: wesmc7777
manager: cfowler
editor: 
tags: 
keywords: Azure functions, patronen, best practices, functies, gebeurtenisverwerking webhooks, dynamische compute, zonder server-architectuur
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: functions
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: d8088a8a83bcaefce17ac2756360a46119c8eb27
ms.sourcegitcommit: be9a42d7b321304d9a33786ed8e2b9b972a5977e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/19/2018
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>De prestaties en betrouwbaarheid van Azure Functions optimaliseren

Dit artikel bevat richtlijnen voor het verbeteren van de prestaties en betrouwbaarheid van uw [zonder server](https://azure.microsoft.com/overview/serverless-computing/) apps werken. 

## <a name="general-best-practices"></a>Aanbevolen beveiligingsprocedures

Hieronder vindt u aanbevolen procedures bij het opbouwen en bouwen van uw zonder server oplossingen met behulp van Azure Functions.

### <a name="avoid-long-running-functions"></a>Vermijd langlopende functies

Grote, langlopende functies kunnen onverwachte time-out problemen veroorzaken. Een functie kan groot zijn als gevolg van Node.js methode veel afhankelijkheden worden. Afhankelijkheden importeren kan ook leiden tot verhoogde laadtijden die tot onverwachte time-outs leiden. Afhankelijkheden zijn beide expliciet en impliciet geladen. Een enkele module geladen door uw code kan zijn eigen aanvullende modules worden geladen.  

Indien mogelijk wordt Verander grote functies in kleinere functie ingesteld die werken samen en snel antwoorden retourneren. Bijvoorbeeld, een webhook of HTTP-trigger functie mogelijk een bevestiging van de reactie binnen een bepaalde tijd; het is gebruikelijk voor webhooks voor een onmiddellijke actie nodig. U kunt de nettolading van de HTTP-trigger doorgeven in een wachtrij moeten worden verwerkt door een functie van de trigger wachtrij. Deze aanpak kunt u het echte werk uit te stellen en direct een reactie terug te keren.


### <a name="cross-function-communication"></a>Cross-functie communicatie

[Duurzame functies](durable-functions-overview.md) en [Azure Logic Apps](../logic-apps/logic-apps-overview.md) zijn ontwikkeld voor beheer van statusovergangen en de communicatie tussen meerdere functies.

Als geen duurzame functies of Logic Apps om te integreren met meerdere functies, maar het is doorgaans beste gebruikmaken van opslagwachtrijen voor cross-functie communicatie.  De belangrijkste reden is opslagwachtrijen zijn goedkoper en eenvoudiger te richten. 

Afzonderlijke berichten in een opslagwachtrij zijn in grootte beperkt tot 64 KB. Als u doorgeven grotere berichten tussen de functies wilt, een Azure Service Bus-wachtrij kan worden gebruikt ter ondersteuning van bericht tot 256 KB in de prijscategorie Standard groottes en maximaal 1 MB in de laag Premium.

Service Bus-onderwerpen zijn handig als u nodig hebt bericht filteren voordat ze worden verwerkt.

Event hubs zijn handig voor de ondersteuning voor grote volumes communicatie.


### <a name="write-functions-to-be-stateless"></a>Schrijven van functies om te worden staatloze 

Functies moet staatloze en idempotent indien mogelijk. Eventuele vereiste statusgegevens koppelen aan uw gegevens. Bijvoorbeeld, een bestelling wordt verwerkt waarschijnlijk heeft een bijbehorende `state` lid. Een functie kan een volgorde die op basis van die status terwijl de functie zelf staatloze blijft verwerken. 

De Idempotent-functies zijn vooral aanbevolen met timer triggers. Bijvoorbeeld, als er iets dat absoluut moet één keer per dag worden uitgevoerd, schrijven zodat elk gewenst moment tijdens de dag kan worden uitgevoerd met hetzelfde resultaat. De functie kunt afsluiten wanneer er geen werk voor een bepaalde dag. Ook als een eerdere mislukte voltooid uitvoert, de volgende keer uitvoeren moet kunnen worden opgepikt waar deze gebleven was.


### <a name="write-defensive-functions"></a>Defensive functies schrijven

Stel dat uw-functie kan een uitzondering op elk gewenst moment optreden. Ontwerp uw functies met de mogelijkheid om door te gaan van een vorige fouten dat tijdens de volgende uitvoering. Houd rekening met een scenario waarin de volgende acties vereist:

1. De query voor 10.000 rijen in een database.
2. Maken van een wachtrijbericht voor elk van deze rijen verder omlaag in de regel.
 
Afhankelijk van hoe complexe uw systeem is, moet u wellicht: downstream betrokken services naar behoren werkt, netwerken uitval of quotum beperkt bereikt, enzovoort. Al deze waarden kunnen invloed hebben op de functie op elk gewenst moment. U moet uw functies worden voorbereid op het ontwerp.

Hoe uw code reageren als er een fout optreedt nadat 5.000 van deze items in een wachtrij voor verwerking invoegen? Bijhouden van items in een verzameling die u hebt voltooid. Anders wordt u mogelijk plaats ze opnieuw zodra. Dit kan ernstige gevolgen hebben op uw werkstroom. 

Als een wachtrij-item al verwerkt is, kunt u de functie moet geen.

Profiteren van beschermingsmaatregelen reeds wordt geboden voor onderdelen die u in de Azure Functions-platform gebruiken. Zie bijvoorbeeld **verwerken van verontreinigde berichten** in de documentatie voor [Azure Storage, Queue-triggers en bindingen](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="scalability-best-practices"></a>Aanbevolen procedures voor schaalbaarheid

Er zijn een aantal factoren die van invloed op hoe de exemplaren van de functie-app schalen. De details zijn beschikbaar in de documentatie voor [functie schalen](functions-scale.md).  Hier volgen enkele aanbevolen procedures om ervoor te zorgen optimale schaalbaarheid van een functie-app.

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Combineer geen test en productie-code in dezelfde functie-app

Functies binnen een functie-app bronnen delen. Bijvoorbeeld: geheugen wordt gedeeld. Als u een functie-app in productie, niet test-gerelateerde functies en bronnen aan toevoegen. Tijdens de uitvoering van de productie-code kan dit leiden tot onverwachte overhead.

Zorg ervoor dat u in uw productie-functie apps laden. Geheugen wordt gemiddeld over elke functie in de app.

Als u een gedeelde assembly waarnaar wordt verwezen in meerdere .net-functies hebt, kunt u deze in een gemeenschappelijke gedeelde map geplaatst. Verwijzen naar de assembly met een instructie die vergelijkbaar is met het volgende voorbeeld als C#-Scripts (.csx): 

    #r "..\Shared\MyAssembly.dll". 

Anders is het eenvoudig te implementeren per ongeluk meerdere testversies van dezelfde binaire die zich anders tussen functies gedragen.

Gebruik geen uitgebreide logboekregistratie in productiecode. Er is een negatief prestatieresultaat.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Asynchrone code gebruiken, maar geen aanroepen blokkeren

Asynchrone programmering is een aanbevolen procedure. Echter altijd voorkomen die verwijzen naar de `Result` eigenschap of aanroepen `Wait` methode op een `Task` exemplaar. Deze methode kan leiden tot uitputting van de thread.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="receive-messages-in-batch-whenever-possible"></a>Berichten ontvangen in batch indien mogelijk

Sommige triggers zoals Event Hub inschakelen ontvangen van een batch van berichten op een enkele aanroepen.  Batchverwerking berichten heeft veel betere prestaties.  U kunt de maximale batchgrootte in de `functions.json` bestand zoals beschreven in de [host.json naslagdocumentatie](functions-host-json.md)

Voor C#-functies kunt u het type naar een matrix sterk getypeerd.  Bijvoorbeeld, in plaats van `EventData sensorEvent` handtekening van de methode kan worden `EventData[] sensorEvent`.  Voor andere talen moet u de eigenschap cardinality expliciet zijn ingesteld uw `function.json` naar `many` om in te schakelen batchverwerking [zoals hier](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10).

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Host-gedrag voor het afhandelen van beter gelijktijdigheid van taken configureren

De `host.json` bestand in de functie-app kunt configureren op de host-runtime en trigger gedrag.  U kunt naast batches gedrag, gelijktijdigheid van taken voor een aantal triggers beheren.  De waarden in deze opties vaak aan te passen, kunt u elke schaal exemplaar op de juiste wijze voor de vereisten van de geactiveerde functies.

Instellingen in het hosts-bestand toepassen voor alle functies in de app binnen een *één exemplaar* van de functie. Als u had een functie-app met 2 HTTP-functies en gelijktijdige aanvragen is ingesteld op 25, wordt door een aanvraag voor de HTTP-trigger tellen naar de gedeelde 25 gelijktijdige aanvragen.  Als dat functie-app wordt geschaald naar 10 exemplaren, de 2 functies effectief 250 gelijktijdige aanvragen toestaan (10 exemplaren * 25 gelijktijdige aanvragen per exemplaar).

**Opties voor HTTP gelijktijdigheid van host**

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

Andere opties host vindt [in het configuratiebestand voor de host](functions-host-json.md).

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

Omdat Azure Functions maakt gebruik van Azure App Service, dient u zich bewust bent van App Service-richtlijnen.
* [Prestatieoptimalisaties patterns and practice HTTP](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)
