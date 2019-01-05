---
title: Aanbevolen procedures voor Azure Functions | Microsoft Docs
description: Informatie over aanbevolen procedures en patronen voor Azure Functions.
services: functions
documentationcenter: na
author: wesmc7777
manager: jeconnoc
keywords: Azure functions, patronen, best practices, functies, gebeurtenisverwerking, webhooks, dynamisch berekenen, architectuur zonder server
ms.assetid: 9058fb2f-8a93-4036-a921-97a0772f503c
ms.service: azure-functions
ms.devlang: multiple
ms.topic: conceptual
ms.date: 10/16/2017
ms.author: glenga
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: c8581510f16896df74f814e501648374d984945e
ms.sourcegitcommit: 8330a262abaddaafd4acb04016b68486fba5835b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54039330"
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>Optimaliseer de prestaties en betrouwbaarheid van Azure Functions

Dit artikel bevat richtlijnen voor het verbeteren van de prestaties en betrouwbaarheid van uw [serverloze](https://azure.microsoft.com/solutions/serverless/) functie-apps. 

## <a name="general-best-practices"></a>Algemene aanbevolen procedures

Hieronder vindt u aanbevolen procedures voor het samenstellen en ontwerpen van uw serverloze oplossingen met behulp van Azure Functions.

### <a name="avoid-long-running-functions"></a>Voorkomen dat de uitvoering lang duurt functies

Grote en langdurige functies kunnen leiden tot onverwachte time-outproblemen. Een functie kan worden grote vanwege veel Node.js-afhankelijkheden. Importeren van afhankelijkheden kan ook leiden tot betere laadtijden die leiden tot onverwachte time-outs. Afhankelijkheden worden beide expliciet en impliciet geladen. Een enkele module geladen door uw code kan een eigen aanvullende modules worden geladen.  

Indien mogelijk wordt herstructureren grote functies in kleinere functie ingesteld dat werk samen en snel antwoorden retourneren. Bijvoorbeeld, een webhook of HTTP-triggerfunctie mogelijk een reactie bevestiging binnen een bepaalde tijdslimiet; het is gebruikelijk voor webhooks voor een onmiddellijke actie nodig is. U kunt de nettolading van de HTTP-trigger in een wachtrij om te worden verwerkt door een activeringsfunctie doorgeven. Deze aanpak kunt u het echte werk uitstellen en een onmiddellijke reactie retourneren.


### <a name="cross-function-communication"></a>Cross-functie-communicatie

[Duurzame functies](durable/durable-functions-concepts.md) en [Azure Logic Apps](../logic-apps/logic-apps-overview.md) zijn gemaakt voor het beheren van statusovergangen en de communicatie tussen meerdere functies.

Als geen duurzame functies of Logic Apps om te integreren met meerdere functies, maar het is doorgaans een aanbevolen procedure voor het gebruik van storage-wachtrijen voor cross-communicatie van de functie.  De belangrijkste reden is storage-wachtrijen zijn goedkoper en eenvoudiger om in te richten. 

Afzonderlijke berichten in een storage-wachtrij worden in grootte beperkt tot 64 KB. Als u nodig hebt om door te geven van grotere berichten tussen functies, een Azure Service Bus-wachtrij kan worden gebruikt ter ondersteuning van bericht in de laag standaard 256 KB-grootten en maximaal 1 MB in de Premium-laag.

Service Bus-onderwerpen zijn handig als u berichten effectief te filteren voorafgaand aan de verwerking is vereist.

Eventhubs zijn handig om ondersteuning voor communicatie met hoog volume.


### <a name="write-functions-to-be-stateless"></a>Functies staatloos schrijven 

Functies moet stateless en indien mogelijk idempotent zijn. Alle informatie over de vereiste status koppelen aan uw gegevens. Bijvoorbeeld, een bestelling wordt verwerkt waarschijnlijk moet een bijbehorende `state` lid. Een functie kan een volgorde op basis van die staat terwijl de functie zelf staatloze blijven verwerken. 

Idempotent-functies zijn met name aanbevolen met timer-triggers. Bijvoorbeeld, als er iets dat absoluut moet één keer per dag worden uitgevoerd, schrijven, zodat het kan elk gewenst moment gedurende de dag worden uitgevoerd met de dezelfde resultaten. De functie kunt afsluiten wanneer er geen werk voor een specifieke dag. Als een eerdere waarvoor niet is voltooid uitvoeren, moet ook de volgende keer wordt uitgevoerd wanneer deze was gebleven kiezen.


### <a name="write-defensive-functions"></a>Defensieve functies schrijven

Wordt ervan uitgegaan dat de functie kan er een uitzondering op elk gewenst moment. Ontwerp uw functies met de mogelijkheid om door te gaan van een eerder punt van mislukken tijdens het volgende uitvoeren. U hebt een scenario waarbij de volgende acties:

1. Query voor 10.000 rijen in een database.
2. Een wachtrijbericht te maken voor elk van deze rijen verdere verwerking van de regel omlaag.
 
Afhankelijk van hoe complex uw systeem is, moet u wellicht: downstream betrokken services naar behoren werkt, netwerken storingen of quotum limieten bereikt, enzovoort. Al deze waarden kunnen invloed hebben op uw functie op elk gewenst moment. U moet uw functies worden voorbereid voor het ontwerpen.

Hoe de code reageren als er een fout optreedt nadat 5000 van de items in een wachtrij voor verwerking invoegen? Bijhouden van items in een verzameling die u hebt voltooid. Anders kan u deze volgende keer opnieuw. Dit kan een ernstige gevolgen hebben voor uw werkstroom. 

Als u een wachtrij-item al is verwerkt, kunt u uw functie moet een niet-op.

Profiteer van beschermingsmaatregelen reeds wordt geboden voor onderdelen die u in het platform, Azure Functions gebruiken. Zie bijvoorbeeld **Onverwerkbare berichten afhandelen** in de documentatie voor [Azure Storage-wachtrij triggers en bindingen](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="scalability-best-practices"></a>Aanbevolen procedures voor schaalbaarheid

Er zijn een aantal factoren die van invloed zijn op hoe de exemplaren van uw functie-app schalen. De details zijn opgegeven in de documentatie voor [functie schalen](functions-scale.md).  Hier volgen enkele aanbevolen procedures om ervoor te zorgen optimale schaalbaarheid van een functie-app.

### <a name="share-and-manage-connections"></a>Delen en beheren van verbindingen

Verbindingen met externe resources waar mogelijk opnieuw gebruiken.  Zie [over het beheren van verbindingen in Azure Functions](./manage-connections.md).

### <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Combineer geen testen en productie-code in dezelfde functie-app

Functies in een functie-app resources delen. Bijvoorbeeld, wordt het geheugen gedeeld. Als u een functie-app in productie, geen test-functies en resources toevoegen aan. Tijdens het uitvoeren van productie-code kan dit leiden tot onverwachte overhead.

Zorg ervoor dat u in uw productie-functie-apps laden. Geheugen is een gemiddelde van elke functie in de app.

Als u een gedeelde assembly waarnaar wordt verwezen in meerdere .net-functies hebt, kunt u deze in een algemene gedeelde map geplaatst. Verwijzen naar de assembly met een instructie die vergelijkbaar is met het volgende voorbeeld als met behulp van C#-Scripts (.csx): 

    #r "..\Shared\MyAssembly.dll". 

Anders is het eenvoudig te implementeren per ongeluk meerdere testversies van dezelfde binaire die zich anders tussen functies gedragen.

Gebruik geen uitgebreide logboekregistratie in productiecode. Er is een negatief prestatieresultaat.

### <a name="use-async-code-but-avoid-blocking-calls"></a>Async-code gebruiken, maar geen aanroepen blokkeren

Asynchrone programmering is een aanbevolen procedure. Echter altijd voorkomen die verwijst naar de `Result` eigenschap of aanroepen `Wait` methode op een `Task` exemplaar. Deze aanpak kan leiden tot uitputting van de thread.

[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

### <a name="receive-messages-in-batch-whenever-possible"></a>Berichten ontvangen in batch indien mogelijk

Sommige triggers, zoals Event Hub inschakelen ontvangen van een batch van berichten op een enkele aanroep.  Batchverwerking berichten heeft veel betere prestaties.  U kunt de maximale batchgrootte in configureren de `host.json` zoals beschreven in het bestand de [naslaginformatie voor host.json](functions-host-json.md)

U kunt het type wijzigen naar een matrix sterk getypeerde voor C#-functies.  Bijvoorbeeld, in plaats van `EventData sensorEvent` handtekening van de methode kan worden `EventData[] sensorEvent`.  Voor andere talen moet u expliciet in te stellen de eigenschap cardinality in uw `function.json` naar `many` om in te schakelen batchverwerking [zoals hieronder](https://github.com/Azure/azure-webjobs-sdk-templates/blob/df94e19484fea88fc2c68d9f032c9d18d860d5b5/Functions.Templates/Templates/EventHubTrigger-JavaScript/function.json#L10).

### <a name="configure-host-behaviors-to-better-handle-concurrency"></a>Host-gedrag voor het afhandelen van beter gelijktijdigheid configureren

De `host.json` -bestand in de functie-app zijn toegestaan voor de configuratie van de gedrag van de runtime en trigger van de host.  Naast het gedrag van batches, kunt u de waarde voor concurrency voor een aantal triggers beheren.  De waarden in deze opties vaak aanpassen, kunt de schaal van elk exemplaar op de juiste wijze voor de eisen van de aangeroepen functies.

Instellingen in het hosts-bestand toepassen voor alle functies in de app binnen een *één exemplaar* van de functie. Als u een functie-app met 2 HTTP-functies en gelijktijdige aanvragen is ingesteld op 25, wordt door een aanvraag om een HTTP-trigger te tellen naar de gedeelde 25 gelijktijdige aanvragen.  Als deze functie-app geschaald naar 10 exemplaren, de 2 functies effectief 250 gelijktijdige aanvragen toestaan (10 exemplaren * 25 gelijktijdige aanvragen per exemplaar).

**Opties voor HTTP-gelijktijdigheid van host**

[!INCLUDE [functions-host-json-http](../../includes/functions-host-json-http.md)]

Andere configuratieopties host vindt [in het configuratiebestand voor host](functions-host-json.md).

## <a name="next-steps"></a>Volgende stappen

Zie de volgende bronnen voor meer informatie:

* [Over het beheren van verbindingen in Azure Functions](manage-connections.md)
* [Aanbevolen procedures van Azure App Service](../app-service/app-service-best-practices.md)
