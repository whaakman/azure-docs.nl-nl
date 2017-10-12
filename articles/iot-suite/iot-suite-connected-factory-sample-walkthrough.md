---
title: Walkthrough voor de Azure IoT-oplossing Connected Factory | Microsoft Docs
description: Een beschrijving van de vooraf geconfigureerde Azure IoT-oplossing Connected Factory en de bijbehorende architectuur.
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 517e908a744734139ed0aeee314a4f3b9eda86cc
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Walkthrough voor de vooraf geconfigureerde oplossing Connected Factory

De [vooraf geconfigureerd oplossing][lnk-preconfigured-solutions] Connected Factory uit IoT Suite is een implementatie van een end-to-endoplossing waarmee:

* Gesimuleerde industriële apparaten met OPC UA-servers in gesimuleerde fabrieksproductielijnen en echte OPC UA-serverapparaten kunnen worden verbonden. Zie de [veelgestelde vragen over Connected Factory](iot-suite-faq-cf.md) voor meer informatie over OPC UA.
* Operationele KPI's en OEE van die apparaten en productielijnen kunnen worden bekeken.
* U kunt bekijken hoe cloudgebaseerde toepassingen kunnen worden gebruikt om te werken met OPC UA-serversystemen.
* U verbinding kunt maken met uw eigen OPC UA-serverapparaten.
* U kunt bladeren in de OPC UA-servergegevens en waarmee u deze kunt wijzigen.
* U kunt integreren met de Azure Time Series Insights-service (TSI) om aangepaste weergaven te bekijken van de gegevens van uw OPC UA-servers.

U kunt de oplossing gebruiken als uitgangspunt voor uw eigen implementatie en u kunt deze [aanpassen][lnk-customize] aan uw eigen specifieke zakelijke vereisten.

In dit artikel wordt stapsgewijs een aantal belangrijke elementen van de oplossing Connected Factory beschreven, zodat u beter begrijpt hoe deze werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten.
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

Zie de [veelgestelde vragen over Connected Factory](iot-suite-faq-cf.md) voor meer informatie.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de vooraf geconfigureerde oplossing:

![Logische architectuur van Connected Factory][connected-factory-logical]

## <a name="communication-patterns"></a>Communicatiepatronen

Voor de oplossing wordt gebruikgemaakt van de [OPC UA Pub/Sub-specificatie](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) voor het verzenden van OPC UA-telemetriegegevens naar IoT Hub in JSON-indeling. Voor de oplossing wordt gebruikgemaakt van de IoT Edge-module [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) voor dit doeleinde.

Als onderdeel van de oplossing is ook een OPC UA-client geïntegreerd in een webtoepassing, die verbinding kan maken met on-premises OPC UA-servers. De client maakt gebruik van een [omgekeerde proxy](https://wikipedia.org/wiki/Reverse_proxy) en ontvangt hulp van IoT Hub om verbinding te maken zonder dat er open poorten in de on-premises firewall nodig zijn. Dit communicatiepatroon heet [door service ondersteunde communicatie](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/). Voor de oplossing wordt gebruikgemaakt van de IoT Edge-module [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) voor dit doeleinde.


## <a name="simulation"></a>Simulatie

De gesimuleerde stations en de gesimuleerde productie-uitvoeringssystemen (MES) vormen samen een fabrieksproductielijn. De gesimuleerde apparaten en de OPC Publisher Module zijn gebaseerd op de [OPC UA .NET Standard][lnk-OPC-UA-NET-Standard] die is gepubliceerd door de OPC Foundation.

OPC Proxy en OPC Publisher worden als modules geïmplementeerd op basis van [Azure IoT Edge][lnk-Azure-IoT-Gateway]. Elke gesimuleerde productielijn heeft een eigen gekoppelde gateway.

Alle simulatie-onderdelen worden uitgevoerd in Docker-containers die worden gehost in een Azure Linux VM. De simulatie wordt standaard geconfigureerd voor het uitvoeren van acht gesimuleerde productielijnen.

## <a name="simulated-production-line"></a>Gesimuleerde productielijn

In een productielijn worden onderdelen geproduceerd. Deze bestaat uit verschillende stations: een montagestation, een teststation en een verpakkingsstation.

De simulatie wordt uitgevoerd en werkt de gegevens bij die worden vrijgegeven via de OPC UA-knooppunten. Alle stations uit de gesimuleerde productielijn worden via OPC UA beheerd door de MES.

## <a name="simulated-manufacturing-execution-system"></a>Gesimuleerd systeem voor de productie-uitvoering

De MES bewaakt alle stations in de productielijn via OPC UA om wijzigingen in de stationsstatus te detecteren. Er worden OPC UA-methoden aangeroepen om de stations te bedienen en producten worden van station naar station verplaatst tot ze voltooid zijn.

## <a name="gateway-opc-publisher-module"></a>Gateway OPC Publisher Module

OPC Publisher Module wordt verbonden met de OPC UA-servers van de stations en wordt geabonneerd op de OPC-knooppunten die moeten worden gepubliceerd. De module converteert de knooppuntgegevens naar de JSON-indeling, versleutelt deze en verstuurt ze naar IoT Hub als OPC UA Pub/Sub-berichten.

OPC Publisher Module vereist slechts één uitgaande HTTPS-poort (443) en kan gebruikmaken van de bestaande bedrijfsinfrastructuur.

## <a name="gateway-opc-proxy-module"></a>Gateway OPC Proxy Module

Gateway OPC UA Proxy stuurt binaire OPC UA-opdrachten en bedieningsberichten door en vereist slechts één uitgaande HTTPS-poort (443). De module kan gebruikmaken van de bestaande bedrijfsinfrastructuur, zoals Web Proxies.

Er wordt gebruikgemaakt van IoT Hub-apparaatmethoden om TCP/IP-gegevens in pakketten in de toepassingslaag over te dragen. Dit zorgt voor eindpuntvertrouwen, gegevensversleuteling en integriteit dankzij SSL/TLS.

Het binaire OPC UA-protocol dat wordt doorgegeven via de proxy zelf maakt gebruik van UA-verificatie en -versleuteling.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

De Gateway OPC Publisher Module wordt geabonneerd op OPC UA-serverknooppunten, zodat wijzigingen in de gegevenswaarden kunnen worden gedetecteerd. Als er een gegevenswijziging wordt gedetecteerd in een van de knooppunten, stuurt deze module berichten naar Azure IoT Hub.

IoT Hub biedt een gebeurtenisbron aan Azure TSI. TSI slaat gegevens gedurende 30 dagen op, op basis van de tijdstempels die aan de berichten zijn gekoppeld. Deze gegevens omvatten:

* OPC UA ApplicationUri
* OPC UA NodeId
* Waarde van het knooppunt
* Tijdstempel van de bron
* OPC UA DisplayName

Momenteel kunnen klanten met TSI niet aanpassen hoe lang ze de gegevens willen bewaren.

TSI voert query's uit voor de knooppuntgegevens op basis van een SearchSpan (Time.From, Time.To) en sorteert ze op basis van OPC UA ApplicationUri, OPC UA NodeId of OPC UA DisplayName.

Als u gegevens wilt ophalen voor de OEE- en KPI-meters en de tijdreeksdiagrammen, worden gegevens gesorteerd op basis van het aantal gebeurtenissen, Sum, Avg, Min en Max.

De tijdreeksen worden opgebouwd op basis van een ander proces. De OEE en KPI's worden berekend op basis van stationbasisgegevens en opgedeeld op basis van de topologie (productielijnen, fabrieken, bedrijf) in de toepassing.

Daarnaast worden de tijdreeksen voor OEE- en KPI-topologie berekend in de app zodra er een weergegeven periode gereed is. De dagweergave wordt bijvoorbeeld elk volledig uur bijgewerkt.

De tijdreeksweergave van knooppuntgegevens is rechtstreeks uit TSI afkomstig, met een sortering op basis van de periode.

## <a name="iot-hub"></a>IoT Hub
[IoT Hub][lnk-IoT Hub] ontvangt gegevens die vanaf OPC Publisher Module worden verzonden naar de cloud, en maakt ze beschikbaar voor de Azure TSI-service. 

De IoT Hub in de oplossing doet ook het volgende:
- Een identiteitsregister beheren waarin de id's van OPC Publisher Modules en OPC Proxy Modules worden opgeslagen.
- Handelen als transportkanaal voor bidirectionele communicatie van de OPC Proxy Module.

## <a name="azure-storage"></a>Azure Storage
Voor de oplossing wordt gebruikgemaakt van Azure Blob Storage voor schijfopslag voor de VM en om implementatiegegevens op te slaan.

## <a name="web-app"></a>Web-app
De web-app die wordt geïmplementeerd als onderdeel van de vooraf geconfigureerde oplossing, bestaat uit een ingebouwde OPC UA-client, biedt verwerking van waarschuwingen en biedt visualisatie van de telemetrie.

## <a name="next-steps"></a>Volgende stappen

U kunt verder aan de slag gaan met IoT Suite door de volgende artikelen te lezen:

* [Machtigingen op de site azureiotsuite.com][lnk-permissions]
* [Een gateway implementeren in Windows of Linux voor de vooraf geconfigureerde oplossing Connected Factory](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
