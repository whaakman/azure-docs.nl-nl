---
title: Externe bewaking overzicht van de oplossingsversneller - Azure | Microsoft Docs
description: Een overzicht van de oplossingsverbetering voor externe controle.
author: dominicbetts
manager: timlt
ms.service: iot-accelerators
services: iot-accelerators
ms.topic: conceptual
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: 097eba4f5bcbb74d4158cc8d4135255d31e03ebd
ms.sourcegitcommit: d211f1d24c669b459a3910761b5cacb4b4f46ac9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/06/2018
ms.locfileid: "44027007"
---
# <a name="remote-monitoring-solution-accelerator-overview"></a>Externe bewaking accelerator overzicht van de oplossing

Externe bewaking [oplossingsverbetering](../iot-accelerators/about-iot-accelerators.md) een end-to-end oplossing voor prestatiecontrole voor meerdere machines geïmplementeerd op verafgelegen locaties bevinden. De oplossing combineert belangrijke Azure-services voor een algemene implementatie van het bedrijfsscenario. U kunt de oplossing gebruiken als uitgangspunt voor uw eigen implementatie en [aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md) te voldoen aan uw eigen specifieke zakelijke vereisten.

Dit artikel begeleidt u bij sommige van de belangrijkste elementen van de oplossing voor externe controle zodat u begrijpt hoe deze werkt. Deze kennis helpt u bij:

* Het oplossen van problemen met de oplossing.
* Het plannen van een aanpassing van de oplossing zodat deze voldoet aan uw eigen specifieke vereisten.
* Het ontwerpen van uw eigen IoT-oplossing die gebruikmaakt van Azure-services.

## <a name="logical-architecture"></a>Logische architectuur

Het volgende diagram geeft een overzicht van de logische onderdelen van de oplossing voor externe controle accelerator overlay bekijken op de [IoT-architectuur](../iot-fundamentals/iot-introduction.md):

![Logische architectuur](./media/iot-accelerators-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Waarom microservices?

Cloudarchitectuur heeft zich ontwikkeld sinds de eerste oplossingsversnellers de release van Microsoft. [Microservices](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) hebben naar voren gekomen als een bewezen praktijken om schaal en flexibiliteit zonder dat dit ten koste gaat van de ontwikkelingssnelheid van de. Verschillende Microsoft-services gebruiken dit architectuurpatroon intern met geweldige betrouwbaarheid en schaalbaarheid resultaten. De bijgewerkte oplossingsversnellers plaats deze geleerde lessen in de praktijk, zodat u ook van deze profiteren kunt.

> [!TIP]
> Zie [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-toepassingsarchitectuur) en [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Microservices: een toepassingsrevolutie aangedreven door de cloud) voor meer informatie over microservicearchitectuur.

## <a name="device-connectivity"></a>Connectiviteit van apparaten

De oplossing omvat de volgende onderdelen in het gedeelte van de connectiviteit apparaat van de logische architectuur:

### <a name="physical-devices"></a>Fysieke apparaten

U kunt fysieke apparaten verbinden met de oplossing. U kunt het gedrag van uw gesimuleerde apparaten met behulp van de Azure IoT device SDK's kunt implementeren.

U kunt fysieke apparaten vanuit het dashboard in de portal van de oplossing inrichten.

### <a name="device-simulation-microservice"></a>Apparaat simulatie microservice

De oplossing omvat de [apparaat simulatie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-simulation) waarmee u kunt een pool van gesimuleerde apparaten beheren vanaf het dashboard van de oplossing voor het testen van de end-to-end-stroom in de oplossing. De gesimuleerde apparaten:

* Apparaat-naar-cloud telemetrie genereren.
* Reageren op cloud-naar-apparaat methodeaanroepen van IoT-Hub.

De microservices biedt een RESTful-eindpunt kunt maken, starten en stoppen van simulaties. Elke simulatie bestaat uit een set virtuele apparaten van verschillende typen, die telemetrie verzenden en reageren op methodeaanroepen.

U kunt gesimuleerde apparaten vanuit het dashboard in de portal van de oplossing inrichten.

### <a name="iot-hub"></a>IoT Hub

De [IoT-hub](../iot-hub/index.yml) neemt telemetrie die van de fysieke en gesimuleerde apparaten worden verzonden naar de cloud. De IoT-hub maakt de telemetrie beschikbaar voor de services in de back-end oplossing voor IoT voor verwerking.

De IoT Hub in de oplossing doet ook het volgende:

* Een identiteitsregister waarin de id's en verificatiesleutels van de apparaten die verbinding mogen maken met de portal.
* Methoden op uw apparaten roept namens de solution accelerator.
* Onderhoudt apparaatdubbels voor alle geregistreerde apparaten. Een apparaatdubbel slaat de eigenschapswaarden op die door een apparaat worden gerapporteerd. Een apparaatdubbel slaat ook gewenste eigenschappen op die in de oplossingsportal zijn ingesteld, zodat het apparaat deze kan ophalen wanneer opnieuw verbinding wordt maakt.
* Plant taken voor het instellen van eigenschappen voor meerdere apparaten of voor het aanroepen van methoden op meerdere apparaten.

## <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

De oplossing omvat de volgende onderdelen in de gegevensverwerking en analytics deel uit van de logische architectuur:

### <a name="iot-hub-manager-microservice"></a>IoT Hub manager microservice

De oplossing omvat de [IoT-Hub manager microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/iothub-manager) voor het afhandelen van interacties met uw IoT-hub, zoals:

* Het maken en beheren van IoT-apparaten.
* Dubbele apparaten beheren.
* Aanroepen van methoden op apparaten.
* Het beheren van IoT-referenties.

Deze service wordt ook uitgevoerd IoT Hub query's om op te halen die behoren tot de gebruiker gedefinieerde groepen apparaten.

De microservices biedt een RESTful-eindpunt voor het beheren van apparaten en apparaatdubbels, aanroepen van methoden en IoT Hub-query's uitvoeren.

### <a name="telemetry-microservice"></a>Telemetrie microservice

De [telemetrie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/device-telemetry) biedt een RESTful-eindpunt voor lezen-toegang tot telemetrie van apparaten, CRUD-bewerkingen op regels en de toegang voor lezen/schrijven voor definities van de waarschuwing uit de opslag.

### <a name="storage-adapter-microservice"></a>Opslag-adapter microservice

De [opslag adapter microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/storage-adapter) beheert van sleutel / waarde-paren, waarbij de semantiek voor storage-service en presenteren van een eenvoudige interface voor het opslaan van gegevens van elke indeling met behulp van Azure Cosmos DB.

Waarden zijn ingedeeld in verzamelingen. U kunt werken op de afzonderlijke waarden of hele verzamelingen ophalen. Complexe gegevensstructuren worden geserialiseerd met de clients en beheerd als eenvoudige tekst-nettolading.

De service biedt een RESTful-eindpunt voor CRUD-bewerkingen op sleutel / waarde-paren. Waarden

### <a name="cosmos-db"></a>Cosmos DB

Maakt gebruik van de standaardimplementatie van de solution accelerator [Azure Cosmos DB](https://docs.microsoft.com/azure/cosmos-db/) als de belangrijkste storage-service.

### <a name="azure-stream-analytics-manager-microservice"></a>Azure Stream Analytics manager microservice

De [Azure Stream Analytics manager microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/asa-manager) Azure Stream Analytics (ASA)-taken, inclusief het instellen van de configuratie hiervan, starten en stoppen en hun status controleren worden beheerd.

De ASA-taak wordt ondersteund door twee sets van verwijzingsgegevens. Definieert u regels voor één gegevensset en een apparaatgroepen definieert. De referentiegegevens regels is gegenereerd op basis van de informatie die wordt beheerd door de telemetrie-microservice. De Azure Stream Analytics manager microservice transformeert telemetrie regels naar logica voor de verwerking van stromen.

De referentiegegevens voor groepen van apparaten wordt gebruikt om te bepalen welke groep van regels toe te passen op een binnenkomende telemetrie-bericht. De groepen apparaten worden beheerd door de configuratie van microservice en gebruik Azure IoT Hub apparaatdubbel-query's.

### <a name="azure-stream-analytics"></a>Azure Stream Analytics

[Azure Stream Analytics](https://docs.microsoft.com/azure/stream-analytics/) is een engine voor gebeurtenisverwerking waarmee u grote volumes aan gegevensstromen van apparaten onderzoeken.

### <a name="configuration-microservice"></a>Configuratie van microservice

De [configuratie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config) biedt een RESTful-eindpunt voor CRUD-bewerkingen op apparaatgroepen, Oplossingsinstellingen en gebruikersinstellingen in de solution accelerator. Het werkt met de opslag-adapter microservice om vast te leggen van de configuratiegegevens.

### <a name="authentication-and-authorization-microservice"></a>Verificatie en autorisatie microservice

De [verificatie en autorisatie microservice](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/auth) beheert de gebruikers die zijn gemachtigd voor toegang tot de solution accelerator. Beheer van gebruikers kan worden gedaan met behulp van een identity-serviceprovider die ondersteuning biedt voor [OpenId Connect](http://openid.net/connect/).

### <a name="azure-active-directory"></a>Azure Active Directory

Maakt gebruik van de standaardimplementatie van de solution accelerator [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) als OpenID Connect-provider. Azure Active Directory gebruikersgegevens worden opgeslagen en vindt u certificaten voor het valideren van JWT-token handtekeningen. 

## <a name="presentation"></a>Presentatie

De oplossing omvat de volgende onderdelen in het gedeelte van de presentatie van de logische architectuur:

De [online gebruikersinterface is een toepassing reageren Javascript](https://github.com/Azure/pcs-remote-monitoring-webui). De toepassing:

* Maakt alleen gebruik Javascript reageren en volledig in de browser wordt uitgevoerd.
* Is opgemaakt met CSS.
* Communiceert met openbare gerichte microservices via AJAX-aanroepen.

De gebruikersinterface geeft alle functionaliteit van de solution accelerator en communiceert met andere microservices, zoals:

* De verificatie en autorisatie microservice om gebruikersgegevens te beschermen.
* De IoT Hub manager microservice weergeven en beheren van de IoT-apparaten.

De configuratie van microservice kunt de gebruikersinterface voor het opslaan en ophalen van configuratie-instellingen.

## <a name="next-steps"></a>Volgende stappen

Als u de documentatie van de bron-code en ontwikkelaars verkennen wilt, begint u met een van de twee GitHub-opslagplaatsen:

* [De oplossingsversneller voor externe controle met Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [De oplossingsversneller voor externe controle met Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).

Gedetailleerde oplossing architectuurdiagrammen:
* [De oplossingsversneller voor externe controle architectuur](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Zie voor meer informatie over de oplossingsverbetering voor externe controle [de oplossingsversnellers aanpassen](../iot-accelerators/iot-accelerators-remote-monitoring-customize.md).
