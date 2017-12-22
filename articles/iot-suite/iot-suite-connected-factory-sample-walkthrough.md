---
title: Kennismaken met oplossingen voor verbonden factory's - Azure | Microsoft Docs
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
ms.date: 12/12/2017
ms.author: dobett
ms.openlocfilehash: 88fe50460baf8b7180da113b33a03120f39cf44f
ms.sourcegitcommit: aaba209b9cea87cb983e6f498e7a820616a77471
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Walkthrough voor de vooraf geconfigureerde oplossing Connected Factory

De [vooraf geconfigureerde oplossing][lnk-preconfigured-solutions] Connected Factory van IoT Suite is een complete brancheoplossing waarmee:

* Gesimuleerde industriële apparaten met OPC UA-servers in gesimuleerde fabrieksproductielijnen en echte OPC UA-serverapparaten kunnen worden verbonden. Zie de [veelgestelde vragen over Connected Factory](iot-suite-faq-cf.md) voor meer informatie over OPC UA.
* Operationele KPI's en OEE van die apparaten en productielijnen kunnen worden bekeken.
* U kunt bekijken hoe cloudgebaseerde toepassingen kunnen worden gebruikt om te werken met OPC UA-serversystemen.
* U verbinding kunt maken met uw eigen OPC UA-serverapparaten.
* U kunt bladeren in de OPC UA-servergegevens en waarmee u deze kunt wijzigen.
* U kunt integreren met de Azure Time Series Insights-service (TSI) om aangepaste weergaven te bekijken van de gegevens van uw OPC UA-servers.

U kunt de oplossing gebruiken als uitgangspunt voor uw eigen implementatie en u kunt deze [aanpassen][lnk-customize] aan uw eigen specifieke zakelijke vereisten.

In dit artikel wordt stapsgewijs een aantal belangrijke elementen van de oplossing Connected Factory beschreven waarmee u inzicht krijgt in de werking. In dit artikel wordt ook beschreven hoe gegevens via de oplossing stromen. Deze kennis helpt u bij:

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

TSI voert query's uit voor de knooppuntgegevens op basis van een **SearchSpan** (**Time.From**, **Time.To**) en sorteert ze op basis van **OPC UA ApplicationUri**, **OPC UA NodeId** of **OPC UA DisplayName**.

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

## <a name="telemetry-data-flow"></a>Telemetriegegevensstroom

![Telemetriegegevensstroom](media/iot-suite-connected-factory-walkthrough/telemetry_dataflow.png)

### <a name="flow-steps"></a>Stromingsstappen

1. OPC Publisher leest de vereiste OPC UA X509-certificaten en IoT Hub-beveiligingsreferenties vanuit het lokale certificaatarchief.
    - Zo nodig maakt OPC Publisher eventuele ontbrekende certificaten of referenties en slaat deze op in het certificaatarchief.

2. OPC Publisher registreert zichzelf bij IoT Hub.
    - Gebruikt het geconfigureerde protocol. Kan gebruikmaken van elk door de SDK van de IoT Hub-client ondersteund protocol. De standaardwaarde is MQTT.
    - Protocolcommunicatie wordt beveiligd door TLS.

3. OPC Publisher leest het configuratiebestand.

4. OPC Publisher maakt een OPC-sessie met elke geconfigureerde OPC UA-Server.
    - Maakt gebruik van een TCP-verbinding.
    - OPC Publisher en de OPC UA-server verifiëren elkaar met behulp van X509-certificaten.
    - Alle overige OPC UA-verkeer wordt versleuteld door het geconfigureerde OPC UA-versleutelingsmechanisme.
    - OPC Publisher maakt een OPC-abonnement in de OPC-sessie voor elk geconfigureerd publicatie-interval.
    - Maakt door OPC bewaakte items voor de OPC-eindpunten die in het OPC-abonnement moeten worden gepubliceerd.

5. Als een bewaakte OPC-eindpuntwaarde wordt gewijzigd, verstuurt de OPC UA-server updates naar OPC Publisher.

6. OPC Publisher transcodeert de nieuwe waarde.
    - Verwerkt meerdere wijzigingen in batches, indien batchverwerking is ingeschakeld.
    - Maakt een IoT Hub-bericht.

7. OPC Publisher stuurt een bericht naar IoT Hub.
    - Gebruikt het geconfigureerde protocol.
    - Communicatie wordt beveiligd door TLS.

8. Time Series Insights (TSI) leest het bericht van IoT Hub.
    - Gebruikt AMQP via TCP/TLS.
    - Deze stap is intern voor het datacenter.

9. Data-at-rest in TSI.

10. Verbonden factory-WebApp in Azure AppService vraagt de vereiste gegevens van TSI.
    - Maakt gebruik van met TCP/TLS beveiligde communicatie.
    - Deze stap is intern voor het datacenter.

11. Webbrowser maakt verbinding met de web-app Connected Factory.
    - Geeft het dashboard van Connected Factory weer.
    - Maakt verbinding via HTTPS.
    - Voor toegang tot de app Connected Factory is verificatie van de gebruiker via Azure Active Directory vereist.
    - WebApi-aanroepen naar de app Connected Factory worden beveiligd door anti-vervalsingstokens.

12. Bij gegevensupdates verstuurt de verbonden factory-WebApp bijgewerkte gegevens naar de webbrowser.
    - Maakt gebruik van het SignalR-protocol.
    - Beveiligd door TCP/TLS.

## <a name="browsing-data-flow"></a>Browsegegevensstroom

![Browsegegevensstroom](media/iot-suite-connected-factory-walkthrough/browsing_dataflow.png)

### <a name="flow-steps"></a>Stromingsstappen

1. OPC-proxy (servercomponent) wordt opgestart.
    - Leest de gedeelde toegangssleutels vanaf een lokale opslag.
    - Slaat zo nodig ontbrekende toegangssleutels in de opslag op.

2. OPC-proxy (servercomponent) registreert zichzelf bij IoT Hub.
    - Leest alle bekende apparaten vanaf IoT Hub.
    - Maakt gebruik van MQTT via TLS via socket of beveiligde WebSocket.

3. Webbrowser maakt verbinding met de web-app Connected Factory en geeft het dashboard van Connected Factory weer.
    - Maakt gebruik van HTTPS.
    - Een gebruiker selecteert een OPC UA-server om verbinding mee te maken.

4. Verbonden factory-WebApp brengt een OPC UA-sessie tot stand met de geselecteerde OPC UA-server.
    - Maakt gebruik van OPC UA-stack.

5. OPC-proxytransport ontvangt een aanvraag van de OPC UA-stack om een TCP-socketverbinding met de OPC UA-server tot stand te brengen.
    - De TCP-nettolading wordt opgehaald en ongewijzigd gebruikt.
    - Deze stap is intern voor de web-app Connected Factory.

6. OPC-proxy (clientcomponent) zoekt het OPC-proxy (servercomponent)-apparaat in het IoT Hub-apparaatregister. Vervolgens wordt een apparaatmethode van het OPC-proxy (servercomponent)-apparaat in IoT Hub aangeroepen.
    - Maakt gebruik van HTTPS via TCP/TLS om OPC-proxy te zoeken.
    - Maakt gebruik van HTTPS via TCP/TLS om een TCP-socketverbinding met de OPC UA-server tot stand te brengen.
    - Deze stap is intern voor het datacenter.

7. IoT Hub roept een apparaatmethode in het OPC-proxy (servercomponent)-apparaat aan.
    - Maakt gebruik van een vastgestelde MQTT via TLS via een socket- of beveiligde WebSocket-verbinding om een TCP-socketverbinding met de OPC UA-server tot stand te brengen.

8. OPC-proxy (servercomponent) stuurt de TCP-nettolading door naar het netwerk van de werkvloer.

9. De OPC UA-server verwerkt de nettolading en stuurt het antwoord terug.

10. Het antwoord wordt ontvangen door de socket van de OPC-proxy (servercomponent).
    - OPC-proxy stuurt de gegevens als retourwaarde van de apparaatmethode naar IoT Hub en de OPC-proxy (clientcomponent).
    - Deze gegevens worden afgeleverd aan de OPC UA-stack in de app Connected Factory.

11. De verbonden factory-WebApp retourneert OPC-browser-UX verrijkt met de OPC UA-specifieke gegevens (ontvangen van de OPC UA-server) naar de webbrowser om ze weer te geven.
    - Tijdens het browsen door de OPC-adresruimte en het toepassen van functies op knooppunten in de OPC-adresruimte, maakt het OPC-browser-UX-clientonderdeel gebruik van AJAX-aanroepen via HTTPS (beveiligd met anti-vervalsingstokens) om gegevens op te halen uit de verbonden factor-WebApp.
    - Zo nodig maakt de client gebruik van de communicatie (zoals uitgelegd in stap 4 t/m 10) om gegevens uit te wisselen met de OPC UA-server.

> [!NOTE]
> De OPC-proxy (servercomponent) en OPC-proxy (clientcomponent) voltooien stap 4 t/m 10 voor alle TCP-verkeer dat gerelateerd is met OPC UA-communicatie.

> [!NOTE]
> Voor de OPC UA-server en de OPC UA-stack in de web-app Connected Factory is de OPC-proxycommunicatie transparant en zijn alle OPC UA-beveiligingsfuncties voor verificatie en versleuteling van toepassing.

## <a name="next-steps"></a>Volgende stappen

U kunt verder aan de slag gaan met IoT Suite door de volgende artikelen te lezen:

* [Machtigingen op de site azureiotsuite.com][lnk-permissions]
* [Een gateway implementeren in Windows of Linux voor de vooraf geconfigureerde oplossing Connected Factory](iot-suite-connected-factory-gateway-deployment.md)
* [OPC Publisher reference implementation](iot-suite-connected-factory-publisher.md) (Implementatie ter referentie van OPC Publisher).

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-v1-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-v1-permissions.md
