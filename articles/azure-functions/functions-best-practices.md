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
ms.openlocfilehash: d59ef16de433ac9691f6996eab2bf56f056feb88
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/10/2017
---
# <a name="optimize-the-performance-and-reliability-of-azure-functions"></a>De prestaties en betrouwbaarheid van Azure Functions optimaliseren

Dit artikel bevat richtlijnen voor het verbeteren van de prestaties en betrouwbaarheid van uw [zonder server](https://azure.microsoft.com/overview/serverless-computing/) apps werken. 


## <a name="avoid-long-running-functions"></a>Vermijd langlopende functies

Grote, langlopende functies kunnen onverwachte time-out problemen veroorzaken. Een functie kan groot zijn als gevolg van Node.js methode veel afhankelijkheden worden. Afhankelijkheden importeren kan ook leiden tot verhoogde laadtijden die tot onverwachte time-outs leiden. Afhankelijkheden zijn beide expliciet en impliciet geladen. Een enkele module geladen door uw code kan zijn eigen aanvullende modules worden geladen.  

Indien mogelijk wordt Verander grote functies in kleinere functie ingesteld die werken samen en snel antwoorden retourneren. Bijvoorbeeld, een webhook of HTTP-trigger functie mogelijk een bevestiging van de reactie binnen een bepaalde tijd; het is gebruikelijk voor webhooks voor een onmiddellijke actie nodig. U kunt de nettolading van de HTTP-trigger doorgeven in een wachtrij moeten worden verwerkt door een functie van de trigger wachtrij. Deze aanpak kunt u het echte werk uit te stellen en direct een reactie terug te keren.


## <a name="cross-function-communication"></a>Cross-functie communicatie

Bij het integreren van meerdere functies, maar het is doorgaans beste gebruikmaken van opslagwachtrijen voor cross-functie communicatie.  De belangrijkste reden is opslagwachtrijen zijn goedkoper en eenvoudiger te richten. 

Afzonderlijke berichten in een opslagwachtrij zijn in grootte beperkt tot 64 KB. Als u doorgeven grotere berichten tussen de functies wilt, het formaat van een Azure Service Bus-wachtrij kan worden gebruikt ter ondersteuning van bericht tot 256 KB.

Service Bus-onderwerpen zijn handig als u nodig hebt bericht filteren voordat ze worden verwerkt.

Event hubs zijn handig voor de ondersteuning voor grote volumes communicatie.


## <a name="write-functions-to-be-stateless"></a>Schrijven van functies om te worden staatloze 

Functies moet staatloze en idempotent indien mogelijk. Eventuele vereiste statusgegevens koppelen aan uw gegevens. Bijvoorbeeld, een bestelling wordt verwerkt waarschijnlijk heeft een bijbehorende `state` lid. Een functie kan een volgorde die op basis van die status terwijl de functie zelf staatloze blijft verwerken. 

De Idempotent-functies zijn vooral aanbevolen met timer triggers. Bijvoorbeeld, als er iets dat absoluut moet één keer per dag worden uitgevoerd, schrijven zodat elk gewenst moment tijdens de dag kan worden uitgevoerd met hetzelfde resultaat. De functie kunt afsluiten wanneer er geen werk voor een bepaalde dag. Ook als een eerdere mislukte voltooid uitvoert, de volgende keer uitvoeren moet kunnen worden opgepikt waar deze gebleven was.


## <a name="write-defensive-functions"></a>Defensive functies schrijven

Stel dat uw-functie kan een uitzondering op elk gewenst moment optreden. Ontwerp uw functies met de mogelijkheid om door te gaan van een vorige fouten dat tijdens de volgende uitvoering. Houd rekening met een scenario waarin de volgende acties vereist:

1. De query voor 10.000 rijen in een database.
2. Maken van een wachtrijbericht voor elk van deze rijen verder omlaag in de regel.
 
Afhankelijk van hoe complexe uw systeem is, moet u wellicht: downstream betrokken services naar behoren werkt, netwerken uitval of quotum beperkt bereikt, enzovoort. Al deze waarden kunnen invloed hebben op de functie op elk gewenst moment. U moet uw functies worden voorbereid op het ontwerp.

Hoe uw code reageren als er een fout optreedt nadat 5.000 van deze items in een wachtrij voor verwerking invoegen? Bijhouden van items in een verzameling die u hebt voltooid. Anders wordt u mogelijk plaats ze opnieuw zodra. Dit kan ernstige gevolgen hebben op uw werkstroom. 

Als een wachtrij-item al verwerkt is, kunt u de functie moet geen.

Profiteren van beschermingsmaatregelen reeds wordt geboden voor onderdelen die u in de Azure Functions-platform gebruiken. Zie bijvoorbeeld **verwerken van verontreinigde berichten** in de documentatie voor [Azure Storage, Queue-triggers en bindingen](functions-bindings-storage-queue.md#trigger---poison-messages). 

## <a name="dont-mix-test-and-production-code-in-the-same-function-app"></a>Combineer geen test en productie-code in dezelfde functie-app

Functies binnen een functie-app bronnen delen. Bijvoorbeeld: geheugen wordt gedeeld. Als u een functie-app in productie, niet test-gerelateerde functies en bronnen aan toevoegen. Tijdens de uitvoering van de productie-code kan dit leiden tot onverwachte overhead.

Zorg ervoor dat u in uw productie-functie apps laden. Geheugen wordt gemiddeld over elke functie in de app.

Als u een gedeelde assembly waarnaar wordt verwezen in meerdere .net-functies hebt, kunt u deze in een gemeenschappelijke gedeelde map geplaatst. Verwijzen naar de assembly met een instructie die vergelijkbaar is met het volgende voorbeeld: 

    #r "..\Shared\MyAssembly.dll". 

Anders is het eenvoudig te implementeren per ongeluk meerdere testversies van dezelfde binaire die zich anders tussen functies gedragen.

Gebruik geen uitgebreide logboekregistratie in productiecode. Er is een negatief prestatieresultaat.



## <a name="use-async-code-but-avoid-blocking-calls"></a>Asynchrone code gebruiken, maar geen aanroepen blokkeren

Asynchrone programmering is een aanbevolen procedure. Echter altijd voorkomen die verwijzen naar de `Result` eigenschap of aanroepen `Wait` methode op een `Task` exemplaar. Deze methode kan leiden tot uitputting van de thread.


[!INCLUDE [HTTP client best practices](../../includes/functions-http-client-best-practices.md)]

## <a name="next-steps"></a>Volgende stappen
Zie de volgende bronnen voor meer informatie:

Omdat Azure Functions maakt gebruik van Azure App Service, dient u zich bewust bent van App Service-richtlijnen.
* [Prestatieoptimalisaties patterns and practice HTTP](https://docs.microsoft.com/azure/architecture/antipatterns/improper-instantiation/)
