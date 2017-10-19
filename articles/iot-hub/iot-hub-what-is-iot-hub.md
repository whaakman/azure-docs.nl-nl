---
title: Overzicht Azure IoT Hub | Microsoft Docs
description: 'Overzicht van de service Azure IoT Hub: wat is IoT Hub, verbindingsmogelijkheden voor apparaten, communicatiepatronen voor Internet of Things (IoT), gateways en service-ondersteunde communicatiepatronen'
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 24376318-5344-4a81-a1e6-0003ed587d53
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/14/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b5f44d2ae42ffc6f75887a64c9ef988fe6d8fd69
ms.sourcegitcommit: 51ea178c8205726e8772f8c6f53637b0d43259c6
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="overview-of-the-azure-iot-hub-service"></a>Overzicht van de service Azure IoT Hub

Welkom bij Azure IoT Hub. Dit artikel biedt een overzicht van Azure IoT Hub en een beschrijving van waarom u deze service moet gebruiken als u een Internet of Things-oplossing (IoT) gaat implementeren. Azure IoT Hub is een volledig beheerde service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen IoT-apparaten en de back-end van een oplossing mogelijk maakt. Azure IoT Hub:

* Biedt meerdere opties voor communicatie van apparaat naar cloud en van cloud naar apparaat. Deze opties bestaan onder andere uit berichten in één richting, bestandsoverdracht en methoden voor aanvraag/antwoord.
* Biedt ingebouwde declaratieve-berichtroutering naar andere Azure-services.
* Biedt opslag waarop query's kunnen worden uitgevoerd voor apparaatmetagegevens en informatie over de gesynchroniseerde status.
* Maakt veilige communicatie en toegangsbeheer mogelijk met behulp van beveiligingssleutels of X.509-certificaten per apparaat.
* Biedt uitgebreide bewaking voor verbindingsmogelijkheden van apparaten en gebeurtenissen voor het beheren van apparaat-id’s.
* Apparaatbibliotheken voor de meest populaire talen en platforms.

In het artikel [IoT Hub en Event Hubs vergeleken][lnk-compare] worden de voornaamste verschillen tussen de twee services beschreven en worden de voordelen van het gebruik van IoT Hub in uw IoT-oplossing uitgelicht.

Raadpleeg [Internet of Things security from the ground up][lnk-security-ground-up] (Beveiliging van Internet of Things vanaf de basis) voor meer informatie over hoe Azure en IoT Hub helpen uw IoT-oplossing te beveiligen.

![Azure IoT-Hub als cloudgateway in IoT-oplossing][img-architecture]

> [!NOTE]
> Zie [Microsoft Azure IoT Reference Architecture][lnk-refarch] (Referentiearchitectuur voor Microsoft Azure IoT) voor een uitgebreide beschrijving van de IoT-architectuur.

## <a name="iot-device-connectivity-challenges"></a>Uitdagingen met de connectiviteit van IoT-apparaten

Dankzij IoT Hub en de apparaatbibliotheken kunt u nu op een betrouwbare en veilige manier apparaten verbinden met de back-end van een oplossing. IoT-apparaten:

* Zijn vaak ingesloten systemen waarbij geen menselijke operator komt kijken.
* Kunnen zich op afgelegen locaties bevinden, waar fysieke toegang kostbaar is.
* Kunnen mogelijk alleen worden bereikt via de back-end van de oplossing.
* Hebben mogelijk een beperkt vermogen en een beperkt aantal verwerkingsbronnen.
* Hebben mogelijk een onstabiele, trage of dure netwerkverbinding.
* Vereisen mogelijk het gebruik van bedrijfseigen, aangepaste of branchespecifieke toepassingsprotocollen.
* Kunnen worden gemaakt met behulp van een groot aantal populaire hardware- en softwareplatforms.

Naast de bovenstaande vereisten moet een IoT-oplossing schaalbaar, veilig en betrouwbaar zijn. De resulterende reeks connectiviteitsvereisten is moeilijk te implementeren en tijdrovend als u conventionele technologieën gebruikt, zoals webcontainers en berichtenbrokers.

## <a name="why-use-azure-iot-hub"></a>Waarom Azure IoT Hub gebruiken?

Azure IoT Hub biedt een uitgebreide reeks opties voor communicatie van [apparaat naar cloud][lnk-d2c-guidance] en van [cloud naar apparaat][lnk-c2d-guidance]. Bovendien biedt Azure IoT Hub de volgende oplossingen voor problemen die zich kunnen voordoen als u op een stabiele en veilige wijze verbinding probeert te maken met apparaten:

* **Apparaatdubbels**. Met behulp van [apparaatdubbels][lnk-twins] kunt u metagegevens en statusinformatie van een apparaat opslaan en synchroniseren en er query's op uitvoeren. Apparaatdubbels zijn JSON-documenten waarin statusinformatie van een apparaat, zoals metagegevens, configuraties en voorwaarden zijn opgeslagen. IoT Hub onderhoudt een apparaatdubbel voor elk apparaat dat u verbindt met IoT Hub.

* **Verificatie per apparaat en beveiligde verbindingen**. U kunt elk apparaat voorzien van een eigen [beveiligingssleutel][lnk-devguide-security], zodat het verbinding kan maken met IoT Hub. Het [id-register van IoT Hub][lnk-devguide-identityregistry] slaat apparaat-id's en sleutels op in een oplossing. De back-end van een oplossing kan afzonderlijke apparaten toevoegen aan lijsten met te weigeren of toegestane apparaten, waardoor de apparaattoegang volledig wordt beheerd.

* **Apparaat-naar-cloud-berichten doorsturen naar Azure-services op basis van declaratieve regels**. Met IoT Hub kunt u berichtroutes definiëren op basis van routeringsregels om te bepalen waar de hub apparaat-naar-cloud-berichten naartoe moet sturen. Voor routeringsregels hoeft u geen code te schrijven. Ze kunnen dienen als vervanging van aangepaste distributie van berichten ná opname.

* **Bewaking van connectiviteitsbewerkingen van apparaten**. U kunt gedetailleerde bewerkingslogboeken over bewerkingen in het beheer van apparaat-id’s en connectiviteitsgebeurtenissen van apparaten ontvangen. Door deze bewakingsmogelijkheid is uw IoT-oplossing in staat om te zien of er verbindingsproblemen zijn. Gebruik deze logboeken om te zien welke apparaten verkeerde referenties geven, te vaak berichten verzenden of alle berichten van de cloud naar apparaten afwijzen.

* **Een uitgebreide reeks apparaatbibliotheken**. [Apparaat-SDK's van Azure IoT][lnk-device-sdks] zijn beschikbaar en worden ondersteund in verscheidene talen en door diverse platforms: C voor veel Linux-distributies, Windows en real-time besturingssystemen. Apparaat-SDK’s van Azure IoT ondersteunen tevens beheerde talen, zoals C#, Java en JavaScript.

* **IoT-protocollen en uitbreidingsmogelijkheden**. Als uw oplossing de apparaatbibliotheken niet kan gebruiken, toont IoT Hub een openbaar protocol waarmee apparaten op systeemeigen wijze de protocollen MQTT v3.1.1, HTTPS 1.1 of AMQP 1.0 kunnen gebruiken. U kunt IoT Hub ook uitbreiden, zodat het aangepaste protocollen ondersteunt. Doe hiervoor het volgende:

  * Maak een veldgateway met [Azure IoT Edge][lnk-iot-edge], die uw aangepaste protocol converteert naar een van de drie protocollen die IoT Hub kent.
  * Pas de [protocolgateway van Azure IoT][protocol-gateway] aan. Dit is een open source-component die wordt uitgevoerd in de cloud.

* **Schalen**. Azure IoT Hub schaalt naar miljoenen gelijktijdig verbonden apparaten en miljoenen gebeurtenissen per seconde.

## <a name="gateways"></a>Gateways

Een gateway in een IoT-oplossing is doorgaans een [protocolgateway][lnk-iotedge] die is geïmplementeerd in de cloud of een [veldgateway][lnk-field-gateway] die lokaal is geïmplementeerd in uw apparaten. Een protocolgateway vertaalt protocollen, bijvoorbeeld MQTT naar AMQP. Een veldgateway kan perifere analyses uitvoeren, tijdgebonden beslissingen nemen waardoor de wachttijden worden verminderd, apparaatbeheerservices bieden, beveiligings- en privacybeperkingen opleggen en protocollen vertalen. Beide gatewaytypen fungeren als schakels tussen uw apparaten en uw IoT Hub.

Een veldgateway onderscheidt zich van een eenvoudig verkeersrouteringsapparaat (zoals een apparaat voor netwerkadresomzetting of firewall) doordat deze doorgaans een actieve rol speelt bij het beheren van de toegang en de informatiestroom in uw oplossing.

Een oplossing kan zowel een protocolgateway als een veldgateway bevatten.

## <a name="how-does-iot-hub-work"></a>Hoe werkt IoT Hub?

Azure IoT Hub implementeert het [service-ondersteunde communicatiepatroon][lnk-service-assisted-pattern] dat interacties tussen uw apparaten en de back-end van uw oplossing overbrengt. Het doel van de service-ondersteunde communicatie is het opzetten van betrouwbare tweerichtingscommunicatiepaden tussen een besturingssysteem, zoals IoT Hub, en apparaten voor speciale doeleinden in mogelijk niet betrouwbare fysieke ruimten. Het patroon gaat uit van de volgende principes:

* Beveiliging gaat voor alle andere functies.

* Apparaten accepteren geen ongevraagde netwerkinformatie. Een apparaat brengt alle verbindingen tot stand en routeert alleen uitgaand verkeer. Als een apparaat een opdracht moet kunnen ontvangen van de back-end van de oplossing, moet het apparaat regelmatig opnieuw verbinding maken om te controleren of er nog opdrachten in afwachting zijn van verwerking.

* Apparaten mogen alleen verbinding maken of routes tot stand brengen met bekende, gelijkwaardige services, zoals IoT Hub.

* Het communicatiepad tussen het apparaat en de service of tussen het apparaat en de gateway is beveiligd op de laag van het toepassingsprotocol.

* Autorisatie en verificatie op systeemniveau geschiedt op basis van de id van een apparaat. Hierdoor kunnen toegangsreferenties en machtigingen bijna onmiddellijk worden ingetrokken.

* Tweerichtingscommunicatie tussen apparaten die slechts sporadisch verbinding maken vanwege problemen met de stroom of de connectiviteit, wordt mogelijk gemaakt door opdrachten en meldingen vast te houden totdat het apparaat weer verbinding maakt en de meldingen en opdrachten ontvangt. IoT Hub heeft apparaatspecifieke wachtrijen voor de opdrachten die worden verzonden.

* Gegevens over de nettolading van toepassing worden afzonderlijk beveiligd, zodat ze veilig via gateways worden verzonden naar een bepaalde service.

Leveranciers van mobiele oplossingen hebben het serviceondersteunde communicatiepatroon gebruikt voor het implementeren van pushmeldingservices, zoals [Windows Push Notification Services][lnk-wns], [Google Cloud Messaging][lnk-google-messaging] en [Apple Push Notification Service][lnk-apple-push].

IoT Hub wordt via het openbare-peeringpad van ExpressRoute ondersteund.

## <a name="next-steps"></a>Volgende stappen

Zie [Berichten verzenden en ontvangen met IoT Hub][lnk-send-messages] om te leren hoe u berichten verzendt vanaf een apparaat en ontvangt via IoT Hub. U leert er ook om berichtroutes te configureren.

Zie [Overzicht van apparaatbeheer met IoT Hub][lnk-device-management] voor meer informatie over hoe IoT Hub op standaarden gebaseerd IoT-apparaatbeheer voor u mogelijk maakt, om zo uw apparaten op afstand te beheren, te configureren en bij te werken.

U kunt de Azure IoT Device SDK's gebruiken voor het implementeren van clienttoepassingen op een groot aantal hardwareplatforms en besturingssystemen. De apparaat-SDK's bevatten bibliotheken die het eenvoudiger maken om telemetrie te verzenden naar een IoT Hub en om cloud-naar-apparaatberichten te ontvangen. Wanneer u de apparaat-SDK's gebruikt, kunt u kiezen uit verschillende netwerkprotocollen om te communiceren met IoT Hub. Raadpleeg ook de [informatie over apparaat-SDK's][lnk-device-sdks].

Raadpleeg de zelfstudie [Aan de slag met IoT Hub][lnk-get-started] als u code wilt leren schrijven en een aantal voorbeelden wilt uitvoeren.

[img-architecture]: media/iot-hub-what-is-iot-hub/hubarchitecture.png

[lnk-get-started]: iot-hub-csharp-csharp-getstarted.md
[protocol-gateway]: https://github.com/Azure/azure-iot-protocol-gateway/blob/master/README.md
[lnk-service-assisted-pattern]: http://blogs.msdn.com/b/clemensv/archive/2014/02/10/service-assisted-communication-for-connected-devices.aspx "Serviceondersteunde communicatie, blogpost van Clemens Vasters"
[lnk-compare]: iot-hub-compare-event-hubs.md
[lnk-iotedge]: iot-hub-protocol-gateway.md
[lnk-field-gateway]: iot-hub-devguide-endpoints.md#field-gateways
[lnk-devguide-identityregistry]: iot-hub-devguide-identity-registry.md
[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-wns]: https://msdn.microsoft.com/library/windows/apps/mt187203.aspx
[lnk-google-messaging]: https://developers.google.com/cloud-messaging/
[lnk-apple-push]: https://developer.apple.com/library/ios/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/Chapters/ApplePushService.html#//apple_ref/doc/uid/TP40008194-CH100-SW9
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-refarch]: http://download.microsoft.com/download/A/4/D/A4DAD253-BC21-41D3-B9D9-87D2AE6F0719/Microsoft_Azure_IoT_Reference_Architecture.pdf
[lnk-iot-edge]: https://github.com/Azure/iot-edge
[lnk-send-messages]: iot-hub-devguide-messaging.md
[lnk-device-management]: iot-hub-device-management-overview.md

[lnk-twins]: iot-hub-devguide-device-twins.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[lnk-security-ground-up]: iot-hub-security-ground-up.md
