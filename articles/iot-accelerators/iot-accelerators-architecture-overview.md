---
title: IoT-oplossingsversnellers referentiearchitectuur - Azure | Microsoft Docs
description: Meer informatie over de Azure IoT-oplossing accelerators verwijzing intranetarchitectuur. De bestaande oplossingsversnellers gebruikmaken van deze verwijzende architectuur. U kunt ook de referentiearchitectuur gebruiken wanneer u uw eigen aangepaste IoT-oplossingen bouwen.
author: dominicbetts
ms.author: dobett
ms.date: 12/04/2018
ms.topic: conceptual
ms.custom: mvc
ms.service: iot-accelerators
services: iot-accelerators
manager: philmea
ms.openlocfilehash: 5fb615ff042cc5cdd3b6b8e42e4d91b161ebf445
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53602372"
---
# <a name="introduction-to-the-azure-iot-reference-architecture"></a>Inleiding tot de Azure IoT-referentiearchitectuur

Dit artikel bevat de [Azure IoT-referentiearchitectuur](https://aka.ms/iotrefarchitecture) en worden voorbeelden gegeven van hoe de [Azure IoT-oplossingsversnellers](about-iot-accelerators.md) Volg de aanbevelingen.

De open-source [bewaking op afstand](iot-accelerators-remote-monitoring-sample-walkthrough.md) en [verbonden Factory](iot-accelerators-connected-factory-sample-walkthrough.md) oplossingsversnellers Volg veel van de aanbevelingen van referentie-architectuur. U kunt de oplossingsversnellers gebruiken als uitgangspunt voor uw eigen IoT-oplossing of als het leren werken met hulpprogramma's.

## <a name="overview"></a>Overzicht

De referentiearchitectuur beschrijft-elementen van een IoT-oplossing, zoals de technologieprincipes, de samenstelling van Azure IoT-services en de apparaten. De architectuur maakt ook aanbevelingen over de implementatie-technologieën.

U kunt een IoT-oplossing als het wordt opgebouwd uit bekijken op het hoogste niveau:

* Dingen die genereren en verzenden van telemetriegegevens zoals metingen en gebeurtenissen. In de Remote Monitoring solution accelerator zijn apparaten zoals vrachtwagens of liften de dingen die telemetrie wordt verzonden.
* Inzichten die zijn gegenereerd op basis van de telemetrie die is verzonden vanaf de dingen. In de Remote Monitoring solution accelerator, een regel genereert een inzicht. Een regel kan bijvoorbeeld worden aangegeven wanneer de temperatuur in een engine voor een drempel bereikt.
* Acties, op basis van inzichten, die een bedrijfsproces of verbeteren. Een e-mailactie kan een operator over een mogelijk probleem melden met een engine voor in de oplossingsversnellers externe controle.

De [Azure IoT-referentiearchitectuur](https://aka.ms/iotrefarchitecture) is een levend document wordt bijgewerkt als de technologie zich verder ontwikkelt.

## <a name="core-subsystems"></a>Core subsystemen

De referentiearchitectuur identificeert de core subsystemen weer die wordt weergegeven in het volgende diagram:

![Core subsystemen](media/iot-accelerators-architecture-overview/CoreSubsystems.png)

De volgende secties wordt beschreven hoe de onderdelen van de oplossingsverbetering voor externe controle worden toegewezen aan de subsystemen core.

### <a name="iot-devices"></a>IoT-apparaten

Een IoT-oplossing moet veilige, efficiënte en krachtige communicatie tussen vrijwel elk type apparaat en de cloudgateway van een inschakelen. Apparaten zijn zakelijke activa die van eenvoudige Temperatuursensoren tot complexe fabrieksproductielijnen honderden onderdelen en sensoren variëren.

Een veldgateway of edge-apparaat, is een gespecialiseerd apparaat-apparaat of de software voor algemene doeleinden die als fungeren:

* Communicatie factor voor het afhandelen van protocol conversie.
* Lokale apparaat besturingssysteem en het apparaat telemetrieverwerking hub. Een edge-apparaat kan verwerken van telemetrie lokaal op de apparaten worden beheerd zonder te communiceren met de cloud. Een edge-apparaat kunt ook filteren of cumulatieve apparaattelemetrie te verminderen de hoeveelheid telemetrie overgebracht naar de cloud.

In de oplossing voor externe controle apparaten verbinding maken met een IoT-hub en verzenden van telemetrie voor verwerking. De oplossing voor externe controle kunt ook apparaten beheren met behulp van de taken of automatische Apparaatbeheer operators. U kunt de Azure IoT device SDK's gebruiken voor het implementeren van uw apparaten.

De oplossing voor externe controle kunt implementeren en beheren van IoT Edge-apparaten. Verwerking aan de rand helpt bij het verminderen van de hoeveelheid telemetrie verzonden naar de cloud en de snelheid van reacties op apparaatgebeurtenissen.

### <a name="cloud-gateway"></a>Cloudgateway

Een cloudgateway zorgt voor communicatie van en naar apparaten en edge-apparaten. Deze apparaten kunnen zijn op veel externe sites.

Communicatie van apparaat, waaronder het Verbindingsbeheer van de, beveiliging van het communicatiepad, verificatie en autorisatie via de gateway beheerd. De gateway ook worden afgedwongen verbinding en doorvoer quota's en verzamelt telemetrie gebruikt voor facturering, diagnostische gegevens en andere taken controleren.

De oplossing voor externe controle implementeert een IoT-hub voor een veilig eindpunt voor apparaten om telemetrie naar te verzenden. De IoT hub ook:

* Een archief van de apparaat-id voor het beheren van de apparaten die verbinding maken met de oplossing bevat.
* Kan de oplossing opdrachten naar apparaten verzenden. Als u bijvoorbeeld wilt openen, een klep als u wilt vrijgeven druk te verlichten.
* Hiermee bulksgewijs Apparaatbeheer. Als u bijvoorbeeld de firmware bijwerken op een reeks apparaten.

### <a name="stream-processing"></a>Streamverwerking

Als de oplossing telemetrie neemt, is het belangrijk om te begrijpen hoe de stroom van telemetrieverwerking kan verschillen. Afhankelijk van het scenario kunnen door verschillende fasen gegevensrecords stromen:

* Opslag, zoals in het geheugen caches, tijdelijke wachtrijen en permanente archieven.

* Analyse, om uit te voeren telemetrie via een set voorwaarden invoer- en kan gegevensrecords met andere uitvoer produceren. Invoer telemetrie gecodeerd in Avro mogelijk bijvoorbeeld uitvoer telemetrie codering in JSON geretourneerd.

* Oorspronkelijke invoer Telemetrie en -analyse uitvoer records worden meestal opgeslagen en beschikbaar om weer te geven. De telemetrie van invoer en uitvoer records kunnen ook starten en acties zoals e-mailberichten, incident tickets en apparaat-opdrachten.

Routering kan telemetrie naar een of meer eindpunten voor opslag, analyse van processen en acties worden uitgevoerd. Een oplossing mogelijk combineren de fasen in verschillende plekken en ze met gelijktijdige parallelle taken te verwerken.

Maakt gebruik van de oplossing voor externe controle [Azure Stream Analytics](/azure/stream-analytics/) voor de verwerking. De regelengine in de oplossing maakt gebruik van Stream Analytics-query's voor het genereren van waarschuwingen en acties. De oplossing kan bijvoorbeeld een query gebruiken om te bepalen wanneer de gemiddelde temperatuur van een vrachtwagen opslag compartiment meer dan vijf minuten lager dan 36 graden.

### <a name="storage"></a>Storage

IoT-oplossingen kunnen grote hoeveelheden gegevens, wat vaak time series-gegevens genereren. Deze gegevens moet worden bewaard waar deze kan worden gebruikt voor visualisatie en rapportage. Een oplossing moet mogelijk ook toegang tot gegevens later voor analyse of verdere verwerking. Het is gebruikelijk om gegevens te splitsen in warme en koude gegevensarchieven. De warme data store bevat de recente gegevens voor toegang met lage latentie. De opslag van koude gegevens meestal historische gegevens worden opgeslagen.

Maakt gebruik van de oplossing voor externe controle [Azure Time Series Insights](/azure/time-series-insights/) als warme gegevensarchief en Cosmos DB als de opslag van koude gegevens.

### <a name="ui-and-reporting-tools"></a>De gebruikersinterface en hulpprogramma's voor rapportage

De oplossing gebruikersinterface kunt opgeven:

* Toegang tot en visualisatie van gegevens en analyse van de resultaten van apparaat.
* Detectie van apparaten via het register.
* Opdracht en besturing van apparaten.
* Werkstromen voor apparaatinrichting.
* Meldingen en waarschuwingen.
* Integratie met live, interactieve dashboards om gegevens uit een groot aantal apparaten weer te geven.  
* Geo-locatie en geo-compatibele services.

De oplossing voor externe controle bevat een webgebruikersinterface deze functionaliteit leveren. De web UI bevat:

* Een interactieve kaart om de locatie van apparaten weer te geven.
* Toegang tot de Time Series Insights explorer doorzoeken en analyseren van de telemetrie.

### <a name="business-integration"></a>Integratie van Business

De integratie van business laag zorgt voor de integratie van de IoT-oplossing met bedrijfssystemen zoals CRM, ERP en line-of-business-toepassingen. Voorbeelden zijn service facturering, klantenservice en vervangende onderdelen opgeven.

De oplossing voor externe controle gebruikt regels om e-mails te verzenden wanneer een voorwaarde wordt voldaan. De oplossing kan bijvoorbeeld een operator melden wanneer de temperatuur in een vrachtwagen minder is dan 36 graden.

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt geleerd over de Azure IoT-referentiearchitectuur en enkele voorbeelden van hoe de oplossingsverbetering voor externe controle zijn aanbevelingen volgt gezien. De volgende stap is om te lezen de [Microsoft Azure IoT Reference Architecture](https://aka.ms/iotrefarchitecture).