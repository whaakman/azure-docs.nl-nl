---
title: Azure IoT Hub verklarende woordenlijst van termen | Microsoft Docs
description: Handleiding voor ontwikkelaars - een verklarende woordenlijst van algemene termen met betrekking tot Azure IoT Hub.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 16ef29ea-a185-48c3-ba13-329325dc6716
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 23008d3619af4606703bca41f370e14cf020a16a
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="glossary-of-iot-hub-terms"></a>Verklarende woordenlijst van IoT-Hub
In dit artikel vindt u enkele van de algemene termen die in de artikelen IoT Hub.

## <a name="advanced-message-queueing-protocol"></a>Geavanceerde Message Queueing-Protocol
[Geavanceerde Message Queueing-Protocol (AMQP)](https://www.amqp.org/) is een van de messaging-protocollen die [IoT Hub](#iot-hub) ondersteunt om te communiceren met apparaten. Zie voor meer informatie over de messaging-protocollen die IoT Hub ondersteunt [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="azure-cli"></a>Azure-CLI
De [Azure CLI](../cli-install-nodejs.md) is een hulpprogramma cross-platform, open-source, op basis van shell-opdracht voor het maken en beheren van resources in Microsoft Azure. Deze versie van de CLI is geïmplementeerd met behulp van Node.js.

## <a name="azure-cli-20"></a>Azure CLI 2.0
De [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) is een hulpprogramma cross-platform, open-source, op basis van shell-opdracht voor het maken en beheren van resources in Microsoft Azure. Deze preview-versie van de CLI is geïmplementeerd met behulp van Python.


## <a name="azure-iot-device-sdks"></a>Apparaat met Azure IoT SDK 's
Er zijn _apparaat-SKD's_ beschikbaar zijn voor meerdere talen die het mogelijk maken [apparaat-apps](#device-app) die communiceren met een IoT-hub. De zelfstudies IoT Hub laten zien hoe deze apparaat-SDK's gebruiken. U vindt de broncode en meer informatie over het apparaat-SDK's in deze GitHub [opslagplaats](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK 's
Er zijn _service-SDK's_ beschikbaar zijn voor meerdere talen die het mogelijk maken [back-end apps](#back-end-app) die communiceren met een IoT-hub. De zelfstudies IoT Hub laten zien hoe deze service-SDK's gebruiken. U vindt de broncode en meer informatie over de service-SDK's in deze GitHub [opslagplaats](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-portal"></a>Azure Portal
De [Microsoft Azure-portal](https://portal.azure.com) is een centrale plaats waar u kunt inrichten en beheren van uw Azure-resources. Deze organiseert de inhoud met behulp van _blades_.

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) is een verzameling die u gebruiken kunt voor het beheren van Azure met Windows PowerShell-cmdlets. U kunt de cmdlets gebruiken voor het maken, testen, implementeren en beheren van oplossingen en services die worden geleverd via de Azure-platform.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u werken met de resources in uw oplossing als een groep. U kunt implementeren, bijwerken of verwijderen van de resources voor uw oplossing in een enkele, gecoördineerde bewerking.

## <a name="azure-service-bus"></a>Azure Service Bus
[Service Bus](../service-bus/index.md) biedt ingeschakeld voor de cloud communicatie met berichtenverzending voor bedrijven en relayed communicatie waarmee u oplossingen on-premises-verbinding met de cloud. Sommige zelfstudies IoT Hub maken gebruik van Service Bus [wachtrijen](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Azure Storage](../storage/common/storage-introduction.md) is een cloudoplossing voor opslag. Dit omvat de Blob Storage-service die u gebruiken kunt voor het opslaan van ongestructureerde objectgegevens. Sommige zelfstudies IoT Hub blob storage gebruiken.

## <a name="back-end-app"></a>Back-endserver voor apps
In de context van [IoT Hub](#iot-hub), een back-endserver voor apps is een app die is verbonden met een van de service gerichte eindpunten op een IoT-hub. Bijvoorbeeld een back-endserver voor apps mogelijk opgehaald [apparaat-naar-cloud](#device-to-cloud)berichten of beheren van de [identiteitsregister](#identity-registry). Normaal gesproken een back-end-app wordt uitgevoerd in de cloud, maar in veel van de zelfstudies zijn de back-end-apps console apps die worden uitgevoerd op uw lokale ontwikkelcomputer.

## <a name="built-in-endpoints"></a>Ingebouwde eindpunten
Elke iothub bevat een ingebouwde [eindpunt](iot-hub-devguide-endpoints.md) die Event Hub-compatibele. U kunt een mechanisme dat werkt met Event Hubs apparaat-naar-cloud-berichten lezen uit dit eindpunt.

## <a name="cloud-gateway"></a>Cloudgateway
Een cloudgateway maakt connectiviteit voor apparaten die rechtstreeks naar kan geen verbinding [IoT Hub](#iot-hub). Een cloudgateway wordt gehost in de cloud in tegenstelling tot een [veldgateway](#field-gateway) die wordt lokaal uitgevoerd op uw apparaten. Een typische gebruiksscenario's voor een cloudgateway is protocolvertaling voor uw apparaten implementeren.

## <a name="cloud-to-device"></a>Cloud-naar-apparaat
Verwijst naar berichten die naar een verbonden apparaat vanaf een iothub zijn verzonden. Deze berichten zijn vaak opdrachten die instrueren van het apparaat een actie te ondernemen. Zie voor meer informatie [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="connection-string"></a>Verbindingsreeks
U kunt verbindingsreeksen in uw app-code bevatten de benodigde informatie om te verbinden met een eindpunt. Een verbindingsreeks bevat doorgaans het adres van het eindpunt en de beveiligingsinformatie, maar de verbindingsreeks indelingen verschillen voor verschillende services. Er zijn twee typen van de verbindingsreeks die is gekoppeld aan de service IoT Hub:
- *Apparaat-verbindingsreeksen* apparaten verbinding maken met de eindpunten van het apparaat gerichte op een IoT-hub inschakelen.
- *IoT Hub verbindingsreeksen* back-end-apps verbinding maken met de service gerichte-eindpunten op een IoT-hub inschakelen.

## <a name="custom-endpoints"></a>Aangepaste eindpunten
U kunt aangepaste maken [eindpunten](iot-hub-devguide-endpoints.md) op een IoT-hub te leveren van berichten die zijn verzonden door een [routeringsregel](#routing-rules). Aangepaste eindpunten rechtstreeks verbinding maken met een Event hub, een Service Bus-wachtrij of een Service Bus-onderwerp.

## <a name="custom-gateway"></a>Aangepaste gateway
Een gateway maakt connectiviteit voor apparaten die rechtstreeks naar kan geen verbinding [IoT Hub](#iot-hub). U kunt [Azure IoT rand](#azure-iot-edge) voor het bouwen van aangepaste gateways die aangepaste regels voor het afhandelen van berichten, aangepaste protocol conversies en andere verwerking op de rand implementeert.

## <a name="data-point-message"></a>Gegevenspunt bericht
Een gegevenspunt bericht is een [apparaat-naar-cloud](#device-to-cloud) bericht met [telemetrie](#telemetry) gegevens, zoals o snelheid of temperatuur.

## <a name="desired-configuration"></a>Gewenst Configuratiebeheer
In de context van een [apparaat twin](iot-hub-devguide-device-twins.md), desired configuration verwijst naar de volledige set eigenschappen en metagegevens in de apparaat-twin die moet worden gesynchroniseerd met het apparaat.

## <a name="desired-properties"></a>Gewenste eigenschappen
In de context van een [apparaat twin](iot-hub-devguide-device-twins.md), eigenschappen is een subsectie van het apparaat twin dat wordt gebruikt met de gewenste [eigenschappen gerapporteerd](#reported-properties) synchroniseren apparaatconfiguratie of voorwaarde. Gewenste eigenschappen kunnen alleen worden ingesteld door een [back-endserver voor apps](#back-end-app) en zijn waargenomen door het [apparaattoepassing](#device-app).

## <a name="device-to-cloud"></a>Apparaat-naar-cloud
Verwijst naar berichten van een verbonden apparaat [IoT Hub](#iot-hub). Deze berichten mogelijk [gegevenspunt](#data-point-message) of [interactieve](#interactive-message) berichten. Zie voor meer informatie [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="device"></a>Apparaat
In de context van IoT een apparaat is doorgaans een kleinschalige, zelfstandige computers die kunnen worden verzameld of andere apparaten te beheren. Een apparaat zijn mogelijk een milieu bewaking apparaat of een domeincontroller voor het aan- en ventilatie systemen in een vergunning. De [apparaat catalogus](https://catalog.azureiotsuite.com/) geeft een lijst van apparaten die zijn gecertificeerd voor gebruik met [IoT Hub](#iot-hub).

## <a name="device-app"></a>Apparaat-app
Een apparaat-app wordt uitgevoerd op uw [apparaat](#device) en verwerkt de communicatie met uw [IoT-hub](#iot-hub). Normaal gesproken gebruikt u een van de [apparaat Azure IoT SDK's](#azure-iot-device-sdks) bij het implementeren van een apparaat-app. In veel van de IoT-zelfstudies, gebruikt u een [gesimuleerd apparaat](#simulated-device) voor uw gemak.

## <a name="device-condition"></a>Voorwaarde van apparaat
Verwijst naar apparaat staat informatie, zoals de methode connectiviteit die momenteel in gebruik, zoals gemeld door een [apparaattoepassing](#device-app). [Apparaat-apps](#device-app) kunnen ook hun mogelijkheden rapporteren. U kunt een query voor de voorwaarde en de mogelijkheid om gegevens door middel van horende apparaten.

## <a name="device-data"></a>Apparaatgegevens
Apparaatgegevens verwijst naar de gegevens per apparaat is opgeslagen in de IoT-Hub [identiteitsregister](#identity-registry). Het is mogelijk om te importeren en exporteren van deze gegevens.

## <a name="device-explorer"></a>Apparaat explorer
De [apparaat explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) is een hulpprogramma dat wordt uitgevoerd op Windows en kunt u uw apparaten beheren in de [identiteitsregister](#identity-registry). Het hulpprogramma kan ook verzenden en ontvangen van berichten op uw apparaten.

## <a name="device-identities-rest-api"></a>REST-API voor apparaatidentiteiten
De [apparaat identiteiten REST-API](https://docs.microsoft.com/rest/api/iothub/iothubresource) kunt u voor het beheren van uw apparaten geregistreerd in de [identiteitsregister](#identity-registry) met behulp van een REST-API. Normaal gesproken moet u een van de bovenliggende [service-SDK's](#azure-iot-service-sdks) zoals weergegeven in de zelfstudies IoT Hub.

## <a name="device-identity"></a>Apparaat-id
De apparaat-id is de unieke id die is toegewezen aan elk apparaat dat is geregistreerd in de [identiteitsregister](#identity-registry).

## <a name="device-management"></a>Apparaatbeheer
Beheer van apparaten omvat de volledige levenscyclus die zijn gekoppeld aan het beheren van de apparaten in uw IoT-oplossing met inbegrip van de planning, inrichten, configureren, bewaken en buiten gebruik stellen.

## <a name="device-management-patterns"></a>Patronen voor apparaatbeheer
[IoT-hub](#iot-hub) kunnen algemene apparaat management patronen, zoals opnieuw op te starten, uitvoeren van de fabrieksinstellingen herstellen en firmware-updates uitvoeren op uw apparaten.

## <a name="device-messaging-rest-api"></a>REST-API voor apparaatmessaging
U kunt de [apparaat Messaging REST-API](https://docs.microsoft.com/rest/api/iothub/httpruntime) van een apparaat apparaat-naar-cloud-berichten naar een IoT-hub te verzenden en ontvangen [cloud-naar-apparaat](#cloud-to-device) berichten van een IoT-hub. Normaal gesproken moet u een van de bovenliggende [apparaat-SKD's](#azure-iot-device-sdks) zoals weergegeven in de zelfstudies IoT Hub.

## <a name="device-provisioning"></a>Mobiele apparaten inrichten
Mobiele apparaten inrichten is het proces van het toevoegen van de eerste [apparaatgegevens](#device-data) naar de winkels in uw oplossing. Om in te schakelen op een nieuw apparaat verbinding maken met uw hub, moet u een apparaat-ID en sleutels toevoegen aan de IoT-Hub [identiteitsregister](#identity-registry). Als onderdeel van het proces van inrichting moet u mogelijk apparaatspecifieke gegevens in andere archieven oplossing initialiseren.

## <a name="device-twin"></a>Dubbel apparaat
Een [apparaat twin](iot-hub-devguide-device-twins.md) JSON-document dat apparaat statusinformatie zoals metagegevens, configuraties en voorwaarden slaat is. [IoT Hub](#iot-hub) een apparaat twin voor elk apparaat dat u in uw IoT-hub inrichten zich blijft voordoen. Apparaat horende kunnen u synchroniseren [apparaat voorwaarden](#device-condition) en configuraties tussen het apparaat en de oplossing voor back-end. U kunt query horende apparaten naar specifieke apparaten en de status van langlopende bewerkingen niet opvragen.

## <a name="device-twin-queries"></a>Apparaat twin query 's
[Apparaat twin query's](iot-hub-devguide-query-language.md) de SQL-achtige IoT Hub-querytaal gegevens ophalen uit uw horende apparaten gebruiken. U kunt de dezelfde IoT Hub-querytaal informatie ophalen over [taken](#job) uitgevoerd in uw IoT-hub.

## <a name="device-twin-rest-api"></a>Apparaat Twin REST-API
U kunt de [apparaat Twin REST-API](https://docs.microsoft.com/rest/api/iothub/devicetwinapi) van de oplossing voor back-end voor het beheren van uw apparaat horende. De API kunt u ophalen en bijwerken van [apparaat twin](#device-twin) eigenschappen en oproepen [methoden directe](#direct-method). Normaal gesproken moet u een van de bovenliggende [service-SDK's](#azure-iot-service-sdks) zoals weergegeven in de zelfstudies IoT Hub.

## <a name="device-twin-synchronization"></a>Synchronisatie van apparaten twin
Synchronisatie van apparaten twin gebruikt de [gewenst eigenschappen](#desired-properties) in uw apparaat horende apparaten configureren en ophalen van [eigenschappen gerapporteerd](#reported-properties) van uw apparaten op te slaan in de apparaat-twin.

## <a name="direct-method"></a>Directe methode
Een [directe methode](iot-hub-devguide-direct-methods.md) is een manier voor het activeren van een methode uit te voeren op een apparaat door het aanroepen van een API op uw IoT-hub.

## <a name="endpoint"></a>Eindpunt
Een iothub toont meerdere [eindpunten](iot-hub-devguide-endpoints.md) die ervoor zorgen dat uw apps kunnen verbinding maken met de IoT-hub. Er zijn apparaat gerichte eindpunten waarmee apparaten uitvoeren van bewerkingen zoals het zenden van [apparaat-naar-cloud](#device-to-cloud) berichten en ontvangst [cloud-naar-apparaat](#cloud-to-device) berichten. Er zijn service gerichte management eindpunten waarmee [back-end apps](#back-end-app) bewerkingen uitvoeren zoals [apparaat-id](#device-identity) beheer- en Apparaatbeheer twin. Er zijn service gerichte [ingebouwde eindpunten](#built-in-endpoints) voor het lezen van apparaat-naar-cloud-berichten. U kunt maken [aangepaste eindpunten](#custom-endpoints) ontvangen van apparaat-naar-cloud-berichten die zijn verzonden door een [routeringsregel](#routing-rules).

## <a name="event-hubs-service"></a>Event Hubs service
[Event Hubs](../event-hubs/event-hubs-what-is-event-hubs.md) is een uiterst schaalbare gegevens ingress-service die, miljoenen opnemen kan gebeurtenissen per seconde. De service kunt u om te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door verbonden apparaten en toepassingen. Zie voor een vergelijking met de service IoT Hub [vergelijking van Azure IoT Hub en Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Event Hub-compatibel eindpunt
Lezen [apparaat-naar-cloud](#device-to-cloud) berichten verzonden naar uw IoT-hub, kunt u verbinding maken met een eindpunt op uw hub en een Event Hub-compatibele TriggerSet-methode gebruiken om de berichten te lezen. Event Hub-compatibele methoden omvatten het gebruik van de [Event Hubs SDK's](../event-hubs/event-hubs-programming-guide.md) en [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Veldgateway
Een veldgateway maakt connectiviteit voor apparaten die rechtstreeks naar kan geen verbinding [IoT Hub](#iot-hub) en meestal lokaal is geïmplementeerd in uw apparaten. Zie voor meer informatie [wat is Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="free-account"></a>Gratis account
Kunt u een [gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/) voltooid de zelfstudies IoT Hub en Experimenteer met de IoT Hub-service (en andere Azure-services).

## <a name="gateway"></a>Gateway
Een gateway maakt connectiviteit voor apparaten die rechtstreeks naar kan geen verbinding [IoT Hub](#iot-hub). Zie ook [veld Gateway](#field-gateway), [Gateway Cloud](#cloud-gateway), en [aangepaste Gateway](#custom-gateway).

## <a name="identity-registry"></a>ID-register
De [identiteitsregister](iot-hub-devguide-identity-registry.md) is het ingebouwde onderdeel van een IoT-hub die wordt informatie over de afzonderlijke apparaten opgeslagen verbinding mogen maken met een IoT-hub.

## <a name="interactive-message"></a>Interactieve bericht
Een interactieve bericht is een [cloud-naar-apparaat](#cloud-to-device) bericht dat een onmiddellijke actie in de back-end oplossing activeert. Een apparaat kan bijvoorbeeld een waarschuwing over een fout die moet worden automatisch aangemeld bij een CRM-systeem te verzenden.

[!INCLUDE [azure-iot-hub-edge-glossary-includes](../../includes/azure-iot-hub-edge-glossary-includes.md)]

## <a name="iot-hub"></a>IoT Hub
IoT Hub is een volledig beheerde Azure-service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten maakt en een back-end oplossing. Zie voor meer informatie [wat is Azure IoT Hub?](iot-hub-what-is-iot-hub.md) Met behulp van uw [Azure-abonnement](#subscription), kunt u IoT hubs voor het afhandelen van uw IoT messaging-werkbelastingen.

## <a name="iot-hub-metrics"></a>IoT Hub metrische gegevens
[IoT Hub metrische gegevens](iot-hub-metrics.md) bieden u de gegevens over de status van de IoT-hubs in uw [Azure-abonnement](#subscription). IoT Hub metrische gegevens kunt u de algemene status van de service en de apparaten die zijn verbonden met het beoordelen. IoT Hub metrische gegevens kunt u zien wat er gebeurt met uw IoT-hub en hoofdoorzaak problemen onderzoeken zonder contact opnemen met ondersteuning van Azure.

## <a name="iot-hub-query-language"></a>IoT Hub-querytaal
De [IoT Hub-querytaal](iot-hub-devguide-query-language.md) is een SQL-achtige taal waarmee u query uw [taken](#job) en horende apparaten.

## <a name="iot-hub-resource-provider-rest-api"></a>Resourceprovider IoT-Hub REST-API
U kunt de [IoT Hub Resource Provider REST-API](https://docs.microsoft.com/rest/api/iothub/resourceprovider/iot-hub-resource-provider-rest) voor het beheren van de IoT-hubs in uw [Azure-abonnement](#subscription) uitvoeren van bewerkingen zoals het maken, bijwerken en verwijderen van hubs.

## <a name="iot-suite"></a>IoT Suite
Azure IoT Suite-pakketten meerdere Azure-services met vooraf geconfigureerde oplossingen. Deze vooraf geconfigureerde oplossingen kunnen u snel aan de slag met end-to-end-implementaties van algemene IoT-scenario's. Zie voor meer informatie [wat is Azure IoT Suite?](../iot-suite/iot-suite-overview.md)

## <a name="the-iot-extension-for-azure-cli-20"></a>De IoT-extensie voor Azure CLI 2.0
[De IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) is een platformoverschrijdende, opdrachtregel-hulpprogramma. Het hulpprogramma kunt u uw apparaten beheren in de [identiteitsregister](#identity-registry), verzenden en ontvangen van berichten en bestanden van uw apparaten en uw IoT hub-bewerkingen te controleren.

## <a name="job"></a>Job
De back-end van uw oplossing kunt [taken](iot-hub-devguide-jobs.md) plannen en bijhouden van activiteiten op een reeks apparaten die zijn geregistreerd bij uw IoT-hub. Activiteiten omvatten apparaat twin bijwerken [gewenst eigenschappen](#desired-properties), bijwerken apparaat twin [labels](#tags), en het aanroepen van [methoden directe](#direct-method). [IoT Hub](#iot-hub) wordt ook gebruikgemaakt van taken naar [om te importeren en exporteren](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) van de [identiteitsregister](#identity-registry).

## <a name="jobs-rest-api"></a>Taken REST-API
De [taken REST-API](https://docs.microsoft.com/rest/api/iothub/jobapi) kunt u beheren [taken](#job) uitgevoerd in uw IoT-hub.

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) is een van de messaging-protocollen die [IoT Hub](#iot-hub) ondersteunt om te communiceren met apparaten. Zie voor meer informatie over de messaging-protocollen die IoT Hub ondersteunt [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>Controle van bewerkingen
IoT Hub [operations bewaking](iot-hub-operations-monitoring.md) kunt u de status van bewerkingen voor uw IoT-hub in realtime controleren. [IoT Hub](#iot-hub) gebeurtenissen worden bijgehouden in verscheidene categorieën van bewerkingen. U kunt kiezen voor het verzenden van gebeurtenissen naar een IoT Hub-eindpunt voor de verwerking van een of meer categorieën. U kunt de gegevens op fouten controleren of complexe verwerking op basis van gegevenspatronen instellen.

## <a name="physical-device"></a>Fysiek apparaat
Een fysiek apparaat is een echte apparaat zoals een frambozen Pi die verbinding met een IoT-hub maakt. Veel van de IoT Hub-zelfstudies gebruiken voor het gemak [gesimuleerde apparaten](#simulated-device) waarmee u kunt voorbeelden uitvoeren op uw lokale machine.

## <a name="primary-and-secondary-keys"></a>Primaire en secundaire sleutels
Wanneer u verbinding met een apparaat gerichte of gerichte service-eindpunt op een IoT-hub maakt uw [verbindingsreeks](#connection-string) sleutel om u toegang kunt krijgen. Wanneer u een apparaat toevoegt aan de [identiteitsregister](#identity-registry) of Voeg een [gedeeld toegangsbeleid](#shared-access-policy) op uw hub, de service een primaire en secundaire sleutel gegenereerd. Twee sleutels, kunt u overschakelen van de ene sleutel naar een andere wanneer u een sleutel bijwerken zonder verlies van toegang met de iothub.

## <a name="protocol-gateway"></a>Protocolgateway
Een protocolgateway wordt meestal geïmplementeerd in de cloud en protocol biedt services voor apparaten die verbinding maken met de vertaling [IoT Hub](#iot-hub). Zie voor meer informatie [wat is Azure IoT Hub?](iot-hub-what-is-iot-hub.md)

## <a name="quotas-and-throttling"></a>Quota en beperkingen
Er zijn verschillende [quota](iot-hub-devguide-quotas-throttling.md) die van toepassing zijn op uw gebruik van [IoT Hub](#iot-hub), veel van de quota variëren, afhankelijk van de laag van de IoT-hub. [IoT Hub](#iot-hub) ook van toepassing [bandbreedte](iot-hub-devguide-quotas-throttling.md) op uw gebruik van de service tijdens runtime.

## <a name="reported-configuration"></a>Gerapporteerde configuratie
In de context van een [apparaat twin](iot-hub-devguide-device-twins.md), gerapporteerd configuratie verwijst naar de volledige set eigenschappen en metagegevens in de apparaat-twin die moet worden gerapporteerd aan de back-end oplossing.

## <a name="reported-properties"></a>Gerapporteerde eigenschappen
In de context van een [apparaat twin](iot-hub-devguide-device-twins.md), gerapporteerd eigenschappen is een subsectie van het apparaat twin gebruikt met [gewenst eigenschappen](#desired-properties) synchroniseren apparaatconfiguratie of voorwaarde. Gerapporteerde eigenschappen kunnen alleen worden ingesteld door de [apparaattoepassing](#device-app) en kan worden gelezen en een query uitgevoerd door een [back-endserver voor apps](#back-end-app).

## <a name="resource-group"></a>Resourcegroep
[Azure Resource Manager](#azure-resource-manager) maakt gebruik van resourcegroepen gebruiken om groepen verwante resources. Een resourcegroep kunt u bewerkingen uitvoeren op alle resources in de groep tegelijk.

## <a name="retry-policy"></a>Beleid voor opnieuw proberen
Gebruik van een beleid voor opnieuw proberen voor de verwerking [tijdelijke fouten](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) wanneer u verbinding maakt met een cloudservice.

## <a name="routing-rules"></a>Regels voor het doorsturen
U configureert [routeringsregels](iot-hub-devguide-messages-read-custom.md) in uw iothub voor het routeren van apparaat-naar-cloud-berichten naar een [ingebouwd eindpunt](#built-in-endpoints) of [aangepaste eindpunten](#custom-endpoints) voor verwerking door de back-end van uw oplossing.

## <a name="sasl-plain"></a>SASL ZONDER OPMAAK
SASL zonder opmaak is een protocol dat de [AMQP](#advanced-message-queue-protocol) protocol gebruikt voor het overzetten van beveiligingstokens.

## <a name="shared-access-signature"></a>Shared Access Signature
Shared Access Signatures (SAS) zijn een verificatiemethode op basis van beveiligde SHA-256-hashes of URI's. SAS verificatie bestaat uit twee onderdelen: een _beleid voor gedeelde toegang_ en een _Shared Access Signature_ (vaak een token genoemd). SAS wordt gebruikt voor verificatie met een IoT-hub van een apparaat. [Back-end apps](#back-end-app) SAS ook gebruiken voor verificatie met de service gerichte-eindpunten op een IoT-hub. Normaal gesproken het opnemen van de SAS-token in de [verbindingsreeks](#connection-string) dat een app gebruikmaakt van een verbinding maken met een IoT-hub.

## <a name="shared-access-policy"></a>Beleid voor gedeelde toegang
Een gedeeld toegangsbeleid definieert de machtigingen voor iedereen met een geldige [primaire of secundaire sleutel](#primary-and-secondary-keys) die zijn gekoppeld aan dit beleid. U kunt het beleid voor gedeelde toegang en de sleutels beheren voor uw hub in de [portal](#azure-portal).

## <a name="simulated-device"></a>Gesimuleerd apparaat
Voor het gemak veel van de IoT Hub-zelfstudies gesimuleerde apparaten gebruiken waarmee u kunt voorbeelden uitvoeren op uw lokale machine. Daarentegen een [fysiek apparaat](#physical-device) is een echte apparaat zoals een frambozen Pi die verbinding met een IoT-hub maakt.

## <a name="solution"></a>Oplossing
Een _oplossing_ kan verwijzen naar een Visual Studio-oplossing die een of meer projecten bevat. Een _oplossing_ mogelijk ook verwijzen naar een IoT-oplossing met elementen zoals apparaten, [apparaat-apps](#device-app), een IoT-hub, andere Azure-services en [back-end apps](#back-end-app).

## <a name="subscription"></a>Abonnement
Een Azure-abonnement is waar facturering plaatsvindt. Elke Azure-resource die u maakt of Azure service waarmee u is gekoppeld aan één abonnement. Veel quota's zijn ook van toepassing op het niveau van een abonnement.

## <a name="system-properties"></a>Systeemeigenschappen
In de context van een [apparaat twin](iot-hub-devguide-device-twins.md), Systeemeigenschappen zijn alleen-lezen en bevatten informatie over het gebruik van het apparaat zoals laatste status van activiteit tijd en de verbinding.

## <a name="tags"></a>Tags
In de context van een [apparaat twin](iot-hub-devguide-device-twins.md), labels, zijn de Apparaatmetagegevens van het opgeslagen en opgehaald door de back-end oplossing in de vorm van een JSON-document. Labels zijn niet zichtbaar voor apps op een apparaat.

## <a name="telemetry"></a>Telemetrie
Apparaten telemetriegegevens, zoals o snelheid of temperatuur, verzamelen en gebruiken [gegevenspunt berichten](#data-point-messages) de telemetrie verzendt naar een IoT-hub.

## <a name="token-service"></a>Service voor beveiligingstokens
U kunt een token service verificatiemechanisme voor uw apparaten implementeren. Dit maakt gebruik van een IoT-Hub [gedeeld toegangsbeleid](#shared-access-policy) met **DeviceConnect** machtigingen voor het maken *binnen het bereik van apparaat* tokens. Deze tokens inschakelen voor een apparaat verbinding maakt met uw IoT-hub. Een apparaat gebruikmaakt van een aangepaste verificatiemethode voor verificatie met de service voor beveiligingstokens. Als het apparaat is geverifieerd, geeft de Tokenservice die een SAS-token voor het apparaat moet worden gebruikt voor toegang tot uw IoT-hub.

## <a name="x509-client-certificate"></a>X.509-certificaat
Een apparaat kan een X.509-certificaat gebruiken voor verificatie met [IoT Hub](#iot-hub). Met behulp van een X.509-certificaat is een alternatief voor het gebruik van een [SAS-token](#shared-access-signature).
