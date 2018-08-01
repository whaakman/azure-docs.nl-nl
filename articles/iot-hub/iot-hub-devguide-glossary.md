---
title: Azure IoT Hub-woordenlijst van termen | Microsoft Docs
description: Handleiding voor ontwikkelaars - een verklarende woordenlijst van termen met betrekking tot Azure IoT Hub.
author: dominicbetts
manager: timlt
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: dobett
ms.openlocfilehash: 1a5df1081edeb8b8e7520064a71534f70a9a9d33
ms.sourcegitcommit: 30fd606162804fe8ceaccbca057a6d3f8c4dd56d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/30/2018
ms.locfileid: "39344297"
---
# <a name="glossary-of-iot-hub-terms"></a>Verklarende woordenlijst voor IoT Hub
In dit artikel worden enkele van de algemene termen gebruikt in de IoT Hub-artikelen.

## <a name="advanced-message-queueing-protocol"></a>Geavanceerde Message Queueing Protocol
[Advanced Message Queueing Protocol (AMQP)](https://www.amqp.org/) is een van de berichten die protocollen [IoT-Hub](#iot-hub) ondersteunt om te communiceren met apparaten. Zie voor meer informatie over de messaging-protocollen die IoT Hub worden ondersteund, [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="automatic-device-management"></a>Automatic Device Management
Automatische Apparaatbeheer in Azure IoT Hub automatiseert veel herhaalde en complexe taken van grote apparaat vloten via het geheel van de levenscycli hiervan te beheren. Met automatisch beheer van apparaten, kunt u een set met apparaten op basis van hun eigenschappen als doel, definieert een gewenste configuratie en IoT Hub-apparaten bijgewerkt wanneer ze bereik binnenkomen, kunnen.  Bestaat uit [automatische apparaatconfiguraties](iot-hub-auto-device-config.md) en [IoT Edge-automatische implementaties](../iot-edge/how-to-deploy-monitor.md).

## <a name="automatic-device-configuration"></a>Configuratie van automatische inrichting
Uw back-end oplossing kunt [automatische apparaatconfiguraties](iot-hub-auto-device-config.md) gewenste eigenschappen toewijzen aan een set [apparaatdubbels](#device-twin) en status rapporteren met behulp van metrische systeemmeetgegevens en aangepaste metrische gegevens. 

## <a name="azure-cli"></a>Azure-CLI
De [Azure CLI](../cli-install-nodejs.md) is een hulpprogramma voor meerdere platformen, open-source, op basis van shell-opdracht voor het maken en beheren van resources in Microsoft Azure. Deze versie van de CLI is geïmplementeerd met behulp van Node.js.

## <a name="azure-cli-20"></a>Azure CLI 2.0
De [Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-az-cli2) is een hulpprogramma voor meerdere platformen, open-source, op basis van shell-opdracht voor het maken en beheren van resources in Microsoft Azure. Deze preview-versie van de CLI is geïmplementeerd met behulp van Python.


## <a name="azure-iot-device-sdks"></a>Azure IoT device SDK 's
Er zijn _apparaat-SDK's_ beschikbaar voor meerdere talen waarmee u kunt maken [apps op apparaten](#device-app) die communiceren met een IoT-hub. De IoT Hub-zelfstudies laten zien hoe u deze apparaat-SDK's gebruiken. U vindt de broncode en meer informatie over de apparaat-SDK's in deze GitHub [opslagplaats](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-service-sdks"></a>Azure IoT service SDK 's
Er zijn _service-SDK's_ beschikbaar voor meerdere talen waarmee u kunt maken [back-end-apps](#back-end-app) die communiceren met een IoT-hub. De IoT Hub-zelfstudies laten zien hoe u deze service-SDK's gebruiken. U vindt de broncode en meer informatie over de service-SDK's in deze GitHub [opslagplaats](https://github.com/Azure/azure-iot-sdks).

## <a name="azure-iot-toolkit"></a>Azure IoT Toolkit
De [Azure IoT Toolkit](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) is een platformoverschrijdende, open-source Visual Studio Code-extensie die helpt u bij het beheren van Azure IoT Hub en apparaten in VS Code. Met Azure IoT Toolkit kunnen IoT-ontwikkelaars ontwikkelen met IoT-project in VS Code eenvoudig.

## <a name="azure-portal"></a>Azure Portal
De [Microsoft Azure portal](https://portal.azure.com) is een centrale plaats waar u kunt inrichten en beheren van uw Azure-resources. De inhoud met behulp van georganiseerd _blades_.

## <a name="azure-powershell"></a>Azure PowerShell
[Azure PowerShell](/powershell/azure/overview) is een verzameling van cmdlets die u gebruiken kunt voor het beheren van Azure met Windows PowerShell. U kunt de cmdlets gebruiken om te maken, testen, implementeren en beheren van oplossingen en services die worden geleverd via het Azure-platform.

## <a name="azure-resource-manager"></a>Azure Resource Manager
[Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) kunt u werken met de resources in uw oplossing als een groep. U kunt implementeren, bijwerken of verwijderen van de resources voor uw oplossing in een enkele, gecoördineerde bewerking.

## <a name="azure-service-bus"></a>Azure Service Bus
[Service Bus](../service-bus/index.md) biedt cloudcommunicatie met berichtenverzending en relayed communicatie waarmee u verbinding maken met on-premises oplossingen met de cloud. Sommige IoT Hub-zelfstudies maken gebruik van Service Bus [wachtrijen](../service-bus-messaging/service-bus-messaging-overview.md).

## <a name="azure-storage"></a>Azure Storage
[Azure Storage](../storage/common/storage-introduction.md) is een oplossing voor cloudopslag. Het bevat de Blob Storage-service die u gebruiken kunt voor het opslaan van ongestructureerde objectgegevens. Sommige IoT Hub-zelfstudies blob-opslag gebruiken.

## <a name="back-end-app"></a>Back-end-app
In de context van [IoT-Hub](#iot-hub), een back-end-app is een app die verbinding met een van de service gerichte eindpunten voor een IoT hub maakt. Bijvoorbeeld een back-end-app mogelijk ophalen [apparaat-naar-cloud](#device-to-cloud)berichten of beheren van de [id-register](#identity-registry). Normaal gesproken een back-end-app wordt uitgevoerd in de cloud, maar in veel van de zelfstudies voor de back-end-apps zijn console-apps die worden uitgevoerd op uw lokale ontwikkelcomputer.

## <a name="built-in-endpoints"></a>Ingebouwde eindpunten
Elke IoT hub bevat een ingebouwde [eindpunt](iot-hub-devguide-endpoints.md) dat wil zeggen Event Hub-compatibele. U kunt een mechanisme dat met Event Hubs werkt te lezen van apparaat-naar-cloud-berichten vanaf dit eindpunt.

## <a name="cloud-gateway"></a>Cloudgateway
Een cloudgateway zorgt voor connectiviteit voor apparaten die niet kan rechtstreeks een verbinding met [IoT-Hub](#iot-hub). Een cloudgateway wordt gehost in de cloud in tegenstelling tot een [veldgateway](#field-gateway) die wordt lokaal uitgevoerd op uw apparaten. Een typische gebruiksscenario's voor de cloudgateway van een is het implementeren van protocollen vertalen voor uw apparaten.

## <a name="cloud-to-device"></a>Cloud-naar-apparaat
Verwijst naar berichten die naar een aangesloten apparaat van een IoT-hub zijn verzonden. Deze berichten zijn vaak opdrachten die vertelt u het apparaat een actie te ondernemen. Zie voor meer informatie, [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="configuration"></a>Configuratie
In de context van [automatische apparaatconfiguratie](iot-hub-auto-device-config.md), een configuratie in de IoT Hub definieert de gewenste configuratie voor een set apparaten dubbels en een set metrische gegevens voor het rapportstatus en voortgang biedt.

## <a name="connection-string"></a>Verbindingsreeks
U verbindingsreeksen in uw app-code gebruiken om in te kapselen van de vereiste informatie op verbinding maken met een eindpunt. Een verbindingsreeks bevat doorgaans het adres van het eindpunt en de informatie over beveiliging, maar de verbindingsreeks indelingen verschillen voor verschillende services. Er zijn twee typen die zijn gekoppeld aan de service voor IoT Hub-verbindingsreeks:
- *Apparaat-verbindingsreeksen* inschakelen dat apparaten verbinding maken met het apparaat gerichte eindpunten voor een IoT hub.
- *Verbindingsreeksen voor IoT Hub* back-end-apps verbinding maken met de service gerichte eindpunten in een IoT-hub inschakelen.

## <a name="custom-endpoints"></a>Aangepaste eindpunten
U kunt aangepaste maken [eindpunten](iot-hub-devguide-endpoints.md) een IoT-hub kunt u berichten worden verzonden door een [routeringsregel](#routing-rules). Aangepaste eindpunten rechtstreeks verbinding maken met een Event hub, een Service Bus-wachtrij of een Service Bus-onderwerp.

## <a name="custom-gateway"></a>Aangepaste gateway
Een gateway zorgt voor connectiviteit voor apparaten die niet kan rechtstreeks een verbinding met [IoT-Hub](#iot-hub). U kunt [Azure IoT Edge](#azure-iot-edge) aan het bouwen van aangepaste gateways die aangepaste logica voor het afhandelen van berichten, aangepaste protocol conversies en andere verwerking op de rand implementeren.

## <a name="data-point-message"></a>Gegevenspunt bericht
Een gegevenspunt bericht is een [apparaat-naar-cloud](#device-to-cloud) -mailbericht met [telemetrie](#telemetry) gegevens, zoals windsnelheid of temperatuur.

## <a name="desired-configuration"></a>Gewenste configuratie
In de context van een [apparaatdubbel](iot-hub-devguide-device-twins.md), de gewenste configuratie verwijst naar de volledige set eigenschappen en metagegevens in de apparaatdubbel die moet worden gesynchroniseerd met het apparaat.

## <a name="desired-properties"></a>Gewenste eigenschappen
In de context van een [apparaatdubbel](iot-hub-devguide-device-twins.md), gewenste eigenschappen is een subsectie van het dubbele apparaat dat wordt gebruikt met [gerapporteerde eigenschappen](#reported-properties) te synchroniseren met apparaatconfiguratie of voorwaarde. Gewenste eigenschappen kunnen alleen worden ingesteld door een [back-end-app](#back-end-app) en jobniveau in acht genomen door de [apparaattoepassing](#device-app).

## <a name="device-to-cloud"></a>Apparaat-naar-cloud
Verwijst naar berichten van een verbonden apparaat [IoT-Hub](#iot-hub). Deze berichten mogelijk [gegevenspunt](#data-point-message) of [interactieve](#interactive-message) berichten. Zie voor meer informatie, [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="device"></a>Apparaat
Een apparaat is in de context van IoT, doorgaans een kleinschalige, zelfstandige computer of fysiek apparaat die kan worden verzameld of andere apparaten beheren. Een apparaat zijn mogelijk een uitwerking controle-apparaat of een domeincontroller voor het water en ventilatie systemen in een vergunning. De [apparaatcatalogus](https://catalog.azureiotsuite.com/) bevat een lijst met apparaten gecertificeerd voor gebruik met [IoT-Hub](#iot-hub).

## <a name="device-app"></a>Apparaat-app
Een apparaat-app wordt uitgevoerd op uw [apparaat](#device) en verzorgt de communicatie met uw [IoT-hub](#iot-hub). Meestal gebruikt u een van de [Azure IoT device SDK's](#azure-iot-device-sdks) bij het implementeren van een apparaat-app. In veel van de IoT-zelfstudies, gebruikt u een [gesimuleerd apparaat](#simulated-device) voor het gemak.

## <a name="device-condition"></a>Apparaat voorwaarde
Verwijst naar informatie over de apparaatstatus, zoals de methode connectiviteit die momenteel in gebruik, zoals gemeld door een [apparaattoepassing](#device-app). [Apps op apparaten](#device-app) kunnen ook hun mogelijkheden rapporteren. U kunt een query voor de voorwaarde en de functionaliteit van gegevens met behulp van dubbele apparaten.

## <a name="device-data"></a>Apparaatgegevens
Apparaatgegevens verwijst naar de per apparaat gegevens die zijn opgeslagen in de IoT-Hub [id-register](#identity-registry). Het is mogelijk om te importeren en exporteren van deze gegevens.

## <a name="device-explorer"></a>Apparatenverkenner
De [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer) is een hulpprogramma dat wordt uitgevoerd op Windows en kunt u voor het beheren van uw apparaten in de [id-register](#identity-registry). Het hulpprogramma kan ook verzenden en ontvangen van berichten op uw apparaten.

## <a name="device-identity"></a>Apparaat-id
De apparaat-id is de unieke id die is toegewezen aan elk apparaat dat is geregistreerd in de [id-register](#identity-registry).

## <a name="module-identity"></a>Module-identiteit
De identiteit van de module is de unieke id die is toegewezen aan elke module die deel uitmaken van een apparaat. Identiteit van de module ook is geregistreerd in de [id-register](#identity-registry).

## <a name="device-management"></a>Apparaatbeheer
Beheer van apparaten omvat de volledige levenscyclus die zijn gekoppeld aan het beheren van de apparaten in uw IoT-oplossing met inbegrip van plannen, inrichten, configureren, bewaken en buiten gebruik stellen.

## <a name="device-management-patterns"></a>Patronen voor apparaatbeheer
[IoT-hub](#iot-hub) kunnen algemene patronen van Apparaatbeheer met inbegrip van opnieuw opstarten, het uitvoeren van de fabrieksinstellingen herstellen en het uitvoeren van firmware-updates op uw apparaten.

## <a name="device-rest-api"></a>REST-API voor apparaat
U kunt de [REST-API voor apparaat](https://docs.microsoft.com/rest/api/iothub/device) vanaf een apparaat aan de apparaat-naar-cloud-berichten naar een IoT-hub te verzenden en ontvangen [cloud-naar-apparaat](#cloud-to-device) berichten van een IoT-hub. Normaal gesproken moet u een van de hogere [apparaat-SDK's](#azure-iot-device-sdks) zoals wordt weergegeven in de IoT Hub-zelfstudies.

## <a name="device-provisioning"></a>Apparaten inrichten
Apparaatinrichting is het proces van het toevoegen van de eerste [apparaatgegevens](#device-data) naar de winkels in uw oplossing. Als u wilt een nieuw apparaat verbinding maken met uw hub inschakelen, moet u een apparaat-ID en sleutels toevoegen aan de IoT-Hub [id-register](#identity-registry). Als onderdeel van het proces van inrichting moet u mogelijk apparaatspecifieke gegevens in andere archieven oplossing initialiseren.

## <a name="device-twin"></a>Dubbel apparaat
Een [apparaatdubbel](iot-hub-devguide-device-twins.md) is JSON-document waarin informatie over de apparaatstatus, zoals metagegevens, configuraties en voorwaarden. [IoT Hub](#iot-hub) een apparaatdubbel voor elk apparaat dat u in uw IoT-hub inrichten. Dubbele apparaten kunnen u om te synchroniseren [apparaat voorwaarden](#device-condition) en configuraties tussen het apparaat en de oplossing back-end. U kunt een query uitvoeren voor apparaatdubbels naar specifieke apparaten en de status van langlopende bewerkingen opvragen.

## <a name="module-twin"></a>Moduledubbel
Net als bij het dubbele apparaat, is een moduledubbel JSON-document waarin statusinformatie van de module, zoals metagegevens, configuraties en voorwaarden. IoT-Hub zich blijft voordoen een moduledubbel voor elke module-identiteit die u onder een apparaat-id in uw IoT-hub inricht. Moduledubbels kunnen u voorwaarden van de module en configuraties tussen de module en de back-end oplossing te synchroniseren. U kunt een query moduledubbels naar specifieke modules en de status van langlopende bewerkingen opvragen.

## <a name="twin-queries"></a>Apparaatdubbel-query 's
[Module- en apparaat-query's voor apparaattwins](iot-hub-devguide-query-language.md) gebruikt u de SQL-achtige IoT Hub-querytaal gegevens ophalen uit uw apparaatdubbels of moduledubbels. U kunt de dezelfde IoT Hub-querytaal gebruiken om op te halen van informatie over [ ](#job) die worden uitgevoerd in uw IoT-hub.

## <a name="twin-synchronization"></a>Dubbele synchronisatie
Twin synchronisatie gebruikt het [gewenste eigenschappen](#desired-properties) in uw apparaatdubbels of moduledubbels voor uw apparaten te configureren of modules ophalen [gerapporteerde eigenschappen](#reported-properties) uit om op te slaan op het dubbele.

## <a name="direct-method"></a>Directe methode
Een [directe methode](iot-hub-devguide-direct-methods.md) is een manier om u voor het activeren van een methode om uit te voeren op een apparaat door een API voor uw IoT-hub aan te roepen.

## <a name="endpoint"></a>Eindpunt
Een IoT-hub toont meerdere [eindpunten](iot-hub-devguide-endpoints.md) waarmee uw apps verbinding maken met de IoT-hub. Er zijn apparaat gerichte eindpunten waarmee apparaten voor het uitvoeren van bewerkingen zoals het verzenden van [apparaat-naar-cloud](#device-to-cloud) berichten en ontvangen [cloud-naar-apparaat](#cloud-to-device) berichten. Er zijn gerichte service management-eindpunten waarmee [back-end-apps](#back-end-app) bewerkingen uit te voeren, zoals [apparaat-id](#device-identity) management en beheer van de dubbele apparaten. Er zijn service gerichte [ingebouwde eindpunten](#built-in-endpoints) voor het lezen van apparaat-naar-cloud-berichten. U kunt maken [aangepaste eindpunten](#custom-endpoints) voor het ontvangen van apparaat-naar-cloud-berichten verzonden door een [routeringsregel](#routing-rules).

## <a name="event-hubs-service"></a>Event Hubs-service
[Eventhubs](../event-hubs/event-hubs-what-is-event-hubs.md) is een zeer schaalbare service voor die van miljoenen opnemen kan gebeurtenissen per seconde. De service kunt u om te verwerken en analyseren van de enorme hoeveelheden gegevens die worden geproduceerd door uw verbonden apparaten en toepassingen. Zie voor een vergelijking met de IoT Hub-service, [vergelijking van Azure IoT Hub en Azure Event Hubs](iot-hub-compare-event-hubs.md).

## <a name="event-hub-compatible-endpoint"></a>Event Hub-compatibel eindpunt
Om te lezen [apparaat-naar-cloud](#device-to-cloud) berichten verzonden naar uw IoT-hub, u kunt verbinding maken met een eindpunt op uw hub en een Event Hub-compatibele-methode gebruiken om deze berichten te lezen. Event Hub-compatibele methoden zijn onder meer met behulp van de [Event Hubs-SDK's](../event-hubs/event-hubs-programming-guide.md) en [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md).

## <a name="field-gateway"></a>Veldgateway
Een veldgateway maakt de connectiviteit voor apparaten die niet kan rechtstreeks een verbinding met [IoT-Hub](#iot-hub) en meestal lokaal is geïmplementeerd in uw apparaten. Zie voor meer informatie, [wat is Azure IoT Hub?](about-iot-hub.md)

## <a name="free-account"></a>Gratis account
U kunt maken een [gratis Azure-account](https://azure.microsoft.com/pricing/free-trial/) voor het voltooien van de IoT Hub-zelfstudies en Experimenteer met de IoT Hub-service (en andere Azure-services).

## <a name="gateway"></a>Gateway
Een gateway zorgt voor connectiviteit voor apparaten die niet kan rechtstreeks een verbinding met [IoT-Hub](#iot-hub). Zie ook [veld Gateway](#field-gateway), [Gateway Cloud](#cloud-gateway), en [aangepaste Gateway](#custom-gateway).

## <a name="identity-registry"></a>ID-register
De [id-register](iot-hub-devguide-identity-registry.md) is het ingebouwde onderdeel van een IoT-hub die informatie over de afzonderlijke apparaten opgeslagen verbinding mogen maken met een IoT-hub.

## <a name="interactive-message"></a>Interactieve bericht
Een interactieve bericht is een [cloud-naar-apparaat](#cloud-to-device) bericht dat een onmiddellijke actie in de back-end van de oplossing wordt geactiveerd. Een apparaat kan bijvoorbeeld een waarschuwing over een fout die moet worden automatisch aangemeld bij een CRM-systeem te verzenden.

[!INCLUDE [azure-iot-hub-edge-glossary-includes](../../includes/azure-iot-hub-edge-glossary-includes.md)]

## <a name="iot-hub"></a>IoT Hub
IoT Hub is een volledig beheerde Azure-service die stabiele en veilige tweerichtingscommunicatie tussen miljoenen apparaten mogelijk maakt en een back-end oplossing. Zie voor meer informatie, [wat is Azure IoT Hub?](about-iot-hub.md) Met behulp van uw [Azure-abonnement](#subscription), kunt u IoT-hubs voor het afhandelen van uw IoT messaging-workloads.

## <a name="iot-hub-metrics"></a>Metrische gegevens van IoT Hub
[Metrische gegevens van IoT-Hub](iot-hub-metrics.md) bieden u de gegevens over de status van de IoT-hubs in uw [Azure-abonnement](#subscription). Metrische gegevens van IoT Hub kunt u de algemene status van de service en de apparaten die zijn verbonden met het beoordelen. Metrische gegevens van IoT Hub kunt u zien wat er gebeurt met uw IoT-hub en hoofdoorzaak problemen onderzoeken zonder contact opnemen met ondersteuning van Azure.

## <a name="iot-hub-query-language"></a>IoT Hub-querytaal
De [IoT Hub-querytaal](iot-hub-devguide-query-language.md) is een SQL-achtige taal waarmee u kunt query uw [ ](#job) en dubbele apparaten.

## <a name="iot-hub-resource-rest-api"></a>IoT-Hub Resource REST-API
U kunt de [REST-API voor IoT Hub Resource](https://docs.microsoft.com/rest/api/iothub/iothubresource) voor het beheren van de IoT-hubs in uw [Azure-abonnement](#subscription) uitvoeren van bewerkingen, zoals het maken, bijwerken en verwijderen van hubs.

## <a name="iot-solution-accelerators"></a>IoT-oplossingsversnellers
Pakket in oplossingen voor Azure IoT-oplossingsversnellers samen meerdere Azure-services. Deze oplossingen kunnen u snel aan de slag met end-to-end-implementaties van algemene IoT-scenario's. Zie voor meer informatie, [wat zijn Azure-IoT-oplossingsversnellers?](../iot-accelerators/about-iot-accelerators.md)

## <a name="the-iot-extension-for-azure-cli-20"></a>De IoT-extensie voor Azure CLI 2.0
[De IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension) is een platformoverschrijdende, opdrachtregel-hulpprogramma. Het hulpprogramma kunt u voor het beheren van uw apparaten in de [id-register](#identity-registry), verzenden en ontvangen van berichten en bestanden van uw apparaten en bewaken van uw IoT hub-bewerkingen.

## <a name="job"></a>Taak
Uw back-end oplossing kunt [taken](iot-hub-devguide-jobs.md) voor het plannen en de activiteiten bijhouden op een set met apparaten die zijn geregistreerd met uw IoT-hub. Activiteiten omvatten bijwerken van de apparaatdubbel [gewenste eigenschappen](#desired-properties), bijwerken apparaatdubbel [tags](#tags), en aan te roepen [directe methoden](#direct-method). [IoT Hub](#iot-hub) gebruikt ook [te importeren en exporteren](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) uit de [id-register](#identity-registry).

## <a name="mqtt"></a>MQTT
[MQTT](http://mqtt.org/) is een van de berichten die protocollen [IoT-Hub](#iot-hub) ondersteunt om te communiceren met apparaten. Zie voor meer informatie over de messaging-protocollen die IoT Hub worden ondersteund, [berichten verzenden en ontvangen met IoT Hub](iot-hub-devguide-messaging.md).

## <a name="operations-monitoring"></a>Controle van bewerkingen
IoT Hub [bewerkingen controleren](iot-hub-operations-monitoring.md) kunt u de status van de bewerkingen op uw IoT-hub in realtime controleren. [IoT Hub](#iot-hub) gebeurtenissen worden bijgehouden in meerdere categorieën met bewerkingen. U kunt kiezen voor het verzenden van gebeurtenissen uit een of meer categorieën naar een IoT Hub-eindpunt voor verwerking. U kunt de gegevens op fouten controleren of instellen van meer complexe verwerking op basis van, gegevenspatronen.

## <a name="physical-device"></a>Fysiek apparaat
Een fysiek apparaat is een echt apparaat zoals een Raspberry Pi die verbinding met een IoT-hub maakt. Veel van de IoT Hub-zelfstudies gebruiken voor het gemak [gesimuleerde apparaten](#simulated-device) waarmee u kunt voorbeelden uitvoeren op uw lokale computer.

## <a name="primary-and-secondary-keys"></a>Primaire en secundaire sleutel
Wanneer u verbinding met een apparaat gerichte of gerichte service-eindpunt voor een IoT-hub maakt, uw [verbindingsreeks](#connection-string) bevat om u toegang kunt krijgen. Wanneer u een apparaat toevoegt aan de [id-register](#identity-registry) of toe te voegen een [gedeeld toegangsbeleid](#shared-access-policy) op uw hub, de service een primaire en secundaire sleutel is gegenereerd. Twee sleutels betekent dat, kunt u vernieuwen uit één sleutel naar een andere wanneer u een sleutel bijwerken zonder te verliezen toegang tot de IoT-hub.

## <a name="protocol-gateway"></a>Protocolgateway
Een protocolgateway wordt gewoonlijk geïmplementeerd in de cloud en biedt protocol vertaalservices voor apparaten die verbinding maken met [IoT-Hub](#iot-hub). Zie voor meer informatie, [wat is Azure IoT Hub?](about-iot-hub.md)

## <a name="quotas-and-throttling"></a>Quota en beperkingen
Er zijn verschillende [quota](iot-hub-devguide-quotas-throttling.md) die van toepassing is op het gebruik van [IoT-Hub](#iot-hub), veel van de quota variëren op basis van de laag van de IoT-hub. [IoT Hub](#iot-hub) geldt ook [beperkt](iot-hub-devguide-quotas-throttling.md) op het gebruik van de service tijdens runtime.

## <a name="reported-configuration"></a>Gerapporteerde configuratie
In de context van een [apparaatdubbel](iot-hub-devguide-device-twins.md), gerapporteerd configuratie verwijst naar de volledige set eigenschappen en metagegevens in de apparaatdubbel die moet worden gerapporteerd aan de back-end van de oplossing.

## <a name="reported-properties"></a>Gerapporteerde eigenschappen
In de context van een [apparaatdubbel](iot-hub-devguide-device-twins.md), gerapporteerde eigenschappen is een subsectie van het dubbele apparaat gebruikt in combinatie met [gewenste eigenschappen](#desired-properties) te synchroniseren met apparaatconfiguratie of voorwaarde. Gerapporteerde eigenschappen kunnen alleen worden ingesteld door de [apparaattoepassing](#device-app) en kan worden gelezen en query's uitgevoerd door een [back-end-app](#back-end-app).

## <a name="resource-group"></a>Resourcegroep
[Azure Resource Manager](#azure-resource-manager) maakt gebruik van resourcegroepen naar gerelateerde resources te groeperen. Bewerkingen uit te voeren op alle resources in de groep tegelijkertijd kunt u een resourcegroep.

## <a name="retry-policy"></a>Beleid voor opnieuw proberen
Gebruik van een beleid voor opnieuw proberen om af te handelen [tijdelijke fouten](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx) wanneer u verbinding maken met een service in de cloud.

## <a name="routing-rules"></a>Regels voor doorsturen
U configureert [routeringsregels](iot-hub-devguide-messages-read-custom.md) in uw IoT-hub voor het routeren van apparaat-naar-cloud-berichten naar een [ingebouwd eindpunt](#built-in-endpoints) of [aangepaste eindpunten](#custom-endpoints) voor verwerking door de back-end .

## <a name="sasl-plain"></a>SASL ZONDER OPMAAK
SASL zonder opmaak is een protocol dat de [AMQP](#advanced-message-queue-protocol) protocol wordt gebruikt om over te dragen van beveiligingstokens.

## <a name="service-rest-api"></a>Service REST API
U kunt de [REST API voor Service](https://docs.microsoft.com/rest/api/iothub/service) van de oplossing back-end om uw apparaten te beheren. De API kunt u voor het ophalen en bijwerken [apparaatdubbel](#device-twin) eigenschappen aangeroepen [directe methoden](#direct-method), en de planning [taken](#job). Normaal gesproken moet u een van de hogere [service-SDK's](#azure-iot-service-sdks) zoals wordt weergegeven in de IoT Hub-zelfstudies.

## <a name="shared-access-signature"></a>Shared Access Signature
Shared Access Signatures (SAS) zijn een verificatiemechanisme voor op basis van beveiligde SHA-256-hashes of URI's. SAS-verificatie bestaat uit twee onderdelen: een _beleid voor gedeelde toegang_ en een _Shared Access Signature_ (ook wel een token). Een apparaat maakt gebruik van SAS te verifiëren met een IoT-hub. [Back-end-apps](#back-end-app) SAS ook gebruiken om te verifiëren met de service gerichte eindpunten voor een IoT hub. Normaal gesproken het opnemen van de SAS-token in de [verbindingsreeks](#connection-string) dat een app wordt gebruikt om een verbinding naar een IoT-hub te maken.

## <a name="shared-access-policy"></a>Beleid voor gedeelde toegang
Een beleid voor gedeelde toegang definieert de machtigingen voor iedereen met een geldige [primaire of secundaire sleutel](#primary-and-secondary-keys) die zijn gekoppeld aan dit beleid. U kunt het beleid voor gedeelde toegang en sleutels beheren voor uw hub in de [portal](#azure-portal).

## <a name="simulated-device"></a>Gesimuleerd apparaat
Veel van de IoT Hub-zelfstudies voor het gemak gesimuleerde apparaten gebruiken waarmee u kunt voorbeelden uitvoeren op uw lokale computer. Daarentegen een [fysiek apparaat](#physical-device) is een echt apparaat zoals een Raspberry Pi die verbinding met een IoT-hub maakt.

## <a name="solution"></a>Oplossing
Een _oplossing_ kunnen verwijzen naar een Visual Studio-oplossing met een of meer projecten. Een _oplossing_ mogelijk ook verwijzen naar een IoT-oplossing met elementen zoals apparaten, [apps op apparaten](#device-app), een IoT-hub, andere Azure-services en [back-end-apps](#back-end-app).

## <a name="subscription"></a>Abonnement
Een Azure-abonnement is waar facturering plaatsvindt. Elke Azure-resource die u maakt of u Azure-service is gekoppeld aan één abonnement. Veel quota zijn ook van toepassing op het niveau van een abonnement.

## <a name="system-properties"></a>Systeemeigenschappen
In de context van een [apparaatdubbel](iot-hub-devguide-device-twins.md), Systeemeigenschappen zijn alleen-lezen en bevatten informatie over het gebruik van het apparaat, zoals de laatste activiteit tijd en de verbinding staat.

## <a name="tags"></a>Tags
In de context van een [apparaatdubbel](iot-hub-devguide-device-twins.md), labels zijn metagegevens van apparaten die zijn opgeslagen en worden opgehaald met de back-end oplossing in de vorm van een JSON-document. Tags zijn niet zichtbaar voor apps op een apparaat.

## <a name="telemetry"></a>Telemetrie
Apparaten telemetriegegevens, zoals windsnelheid of temperatuur, verzamelen en gebruiken [gegevenspunt berichten](#data-point-messages) de telemetrie verzenden naar een IoT-hub.

## <a name="token-service"></a>Token-service
Een token service kunt u een verificatiemechanisme implementeren voor uw apparaten. Het maakt gebruik van een IoT-Hub [gedeeld toegangsbeleid](#shared-access-policy) met **DeviceConnect** machtigingen voor het maken *binnen het bereik van apparaat* tokens. Deze tokens inschakelen voor een apparaat verbinding maakt met uw IoT-hub. Een apparaat maakt gebruik van een aangepaste verificatiemechanisme voor verificatie met de service voor beveiligingstokens. Als het apparaat is geverifieerd, geeft de service voor beveiligingstokens een SAS-token voor het apparaat gebruiken voor toegang tot uw IoT-hub.

## <a name="x509-client-certificate"></a>X.509-clientcertificaat
Een apparaat een X.509-certificaat kunt gebruiken om te verifiëren met [IoT-Hub](#iot-hub). Met behulp van een X.509-certificaat is een alternatief voor het gebruik van een [SAS-token](#shared-access-signature).
