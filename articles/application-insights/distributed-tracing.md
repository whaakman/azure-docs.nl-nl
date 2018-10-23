---
title: Gedistribueerde tracering in Azure Application Insights | Microsoft Docs
description: Bevat informatie over de ondersteuning van Microsoft voor gedistribueerde tracering door middel van onze lokale doorstuurserver en samenwerking in het project OpenCensus
services: application-insights
keywords: ''
author: nikmd23
ms.author: nimolnar
ms.reviewer: mbullwin
ms.date: 09/17/2018
ms.service: application-insights
ms.topic: conceptual
manager: carmonm
ms.openlocfilehash: 9594ccd0449cf68a63c770cfdee03f255aa2beae
ms.sourcegitcommit: 17633e545a3d03018d3a218ae6a3e4338a92450d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/22/2018
ms.locfileid: "49638372"
---
# <a name="what-is-distributed-tracing"></a>Wat is gedistribueerd traceren?

De opkomst van moderne cloud- en microservicesarchitecturen heeft geleid tot eenvoudige, onafhankelijk implementeerbare services waarmee u kosten verlagen kunnen terwijl u hogere beschikbaarheid en doorvoer. Maar terwijl deze verplaatsingen van het type afzonderlijke services gemakkelijker te begrijpen als geheel gemaakt hebben, hebben ze algehele systemen moeilijker reden over en fouten opsporen aangebracht.

In monolithische architecturen hebben we verkregen gebruikt om het foutopsporing met call-stacks. Call-stacks zijn schitterende hulpprogramma's voor het weergeven van de stroom van de uitvoering (methode een aangeroepen methode B, C methode genoemd), samen met details en parameters over elk van deze aanroepen. Dit is ideaal voor monolieten of services die worden uitgevoerd op een enkel proces, maar hoe we fouten bij de aanroep is voor de Procesgrens van een, niet alleen een verwijzing op de lokale stack? 

Dat is waar gedistribueerde tracering is beschikbaar in.  

Gedistribueerde tracering is het equivalent van de aanroep-stacks voor moderne cloud- en microservicesarchitecturen, met de toevoeging van een eenvoudig prestaties profiler opgetreden in. We bieden twee ervaringen voor het gebruiken van gedistribueerde traceringsgegevens in Azure Monitor. De eerste is onze [diagnostische gegevens voor transacties](https://docs.microsoft.com/azure/application-insights/app-insights-transaction-diagnostics) weergave, zoals een aanroepstack voor een tijddimensie toegevoegd in. De weergave van de diagnostische gegevens over transactie biedt meer inzicht in één enkele transactie aanvraag en is handig voor het vinden van de hoofdoorzaak van betrouwbaarheidsproblemen met en knelpunten op basis van per aanvraag.

Azure Monitor biedt ook een [toepassingsoverzicht](https://docs.microsoft.com/azure/application-insights/app-insights-app-map) weergave waarmee veel transacties om weer te geven van een topologische weergave van interactie tussen de systemen, en wat de gemiddelde prestaties en fout-tarieven zijn. 

## <a name="how-to-enable-distributed-tracing"></a>Gedistribueerde tracering inschakelen

Gedistribueerde tracering inschakelen via de services in een toepassing is net zo eenvoudig als het toevoegen van het juiste SDK of de tapewisselaar voor elke service op basis van de taal die in de service is geïmplementeerd.

## <a name="enabling-via-application-insights-sdks"></a>Inschakelen via de Application Insights-SDK 's

De Application Insights-SDK's voor .NET, .NET Core, Java, Node.js en JavaScript die alle gedistribueerde tracering systeemeigen ondersteuning. Hieronder vindt u instructies voor het installeren en configureren van elke Application Insights-SDK:

* [.NET](https://docs.microsoft.com/azure/application-insights/quick-monitor-portal)
* [.NET Core](https://docs.microsoft.com/azure/application-insights/app-insights-dotnetcore-quick-start)
* [Java](https://docs.microsoft.com/azure/application-insights/app-insights-java-get-started)
* [Node.js](https://docs.microsoft.com/azure/application-insights/app-insights-nodejs-quick-start)
* [JavaScript](https://docs.microsoft.com/azure/application-insights/app-insights-javascript)

Met de juiste Application Insights-SDK geïnstalleerd en geconfigureerd, worden traceringsgegevens automatisch verzameld voor populaire frameworks, bibliotheken en -technologieën door SDK afhankelijkheid auto-collectors. De volledige lijst met ondersteunde technologieën is beschikbaar in [de documentatie voor het automatisch verzamelen van afhankelijkheid](https://docs.microsoft.com/azure/application-insights/auto-collect-dependencies).

 Daarnaast elke technologie kan worden gevolgd handmatig met een aanroep naar [TrackDependency](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics) op de [TelemetryClient](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics).

## <a name="enable-via-opencensus"></a>Via OpenCensus inschakelen

Naast de Application Insights SDK's, Application Insights biedt ook ondersteuning voor gedistribueerde tracering via [OpenCensus](https://opencensus.io/). OpenCensus is een open-source, leverancieronafhankelijke, één distributie van bibliotheken voor metrische gegevens verzamelen en gedistribueerde tracering voor services. Hiermee wordt ook de open source-community om in te schakelen gedistribueerde tracering met populaire technologieën zoals Redis Memcached of MongoDB. [Microsoft werkt op OpenCensus samen met enkele andere controle- en cloud partners](https://open.microsoft.com/2018/06/13/microsoft-joins-the-opencensus-project/).

Gedistribueerde tracering mogelijkheden eerst toevoegen aan een toepassing OpenCensus, [installeren en configureren van de Application Insights lokale doorstuurserver](./opencensus-local-forwarder.md). Configureren van daaruit OpenCensus om te routeren gedistribueerde traceringsgegevens via de lokale doorstuurserver. Beide [Python](./opencensus-python.md) en [gaat](./opencensus-go.md) worden ondersteund.

De website OpenCensus onderhoudt API-referentiedocumentatie voor [Python](https://opencensus.io/api/python/trace/usage.html) en [gaat](https://godoc.org/go.opencensus.io), en diverse andere handleidingen voor het gebruik van OpenCensus. 

## <a name="next-steps"></a>Volgende stappen

* [OpenCensus Python (Engelstalig)](https://opencensus.io/api/python/trace/usage.html)
* [Overzicht van de toepassing](./app-insights-app-map.md)
* [End-to-end-prestaties controleren](./app-insights-tutorial-performance.md)
