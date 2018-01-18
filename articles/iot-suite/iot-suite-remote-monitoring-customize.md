---
title: De oplossing voor externe controle - Azure aanpassen | Microsoft Docs
description: In dit artikel bevat informatie over hoe u toegang hebt tot de broncode voor de vooraf geconfigureerde oplossing voor externe controle.
services: 
suite: iot-suite
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-suite
ms.date: 01/17/2018
ms.topic: article
ms.devlang: NA
ms.tgt_pltfrm: NA
ms.workload: NA
ms.openlocfilehash: f5d38091b59110859d4376a5cd16a19f24dad65b
ms.sourcegitcommit: 7edfa9fbed0f9e274209cec6456bf4a689a4c1a6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/17/2018
---
# <a name="customize-the-remote-monitoring-preconfigured-solution"></a>De vooraf geconfigureerde oplossing voor externe controle aanpassen

Dit artikel bevat informatie over hoe u toegang tot de broncode en aanpassen van de externe controle vooraf geconfigureerde oplossing. Dit artikel wordt beschreven:

* De GitHub-opslagplaatsen die de broncode en resources voor de microservices die gezamenlijk de vooraf geconfigureerde oplossing bevatten.
* Algemene scenario's voor aanpassingen zoals het toevoegen van een nieuwe apparaattype.

De volgende video biedt een overzicht van de opties voor het aanpassen van de vooraf geconfigureerde oplossing voor externe controle:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/How-to-customize-the-Remote-Monitoring-Preconfigured-Solution-for-Azure-IoT/Player]

## <a name="project-overview"></a>Overzicht van project

### <a name="implementations"></a>Implementaties

De oplossing voor externe controle heeft zowel .NET en Java-implementaties. Beide implementaties bieden vergelijkbare functionaliteit en zijn afhankelijk van de dezelfde onderliggende Azure-services. U kunt de site op het hoogste GitHub-opslagplaatsen hier vinden:

* [.NET-oplossing](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet)
* [Java-oplossing](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java)

### <a name="microservices"></a>Microservices

Als u geïnteresseerd in een specifieke functie van de oplossing bent, kunt u de GitHub-opslagplaatsen voor elke afzonderlijke microservice openen. Elke microservice implementeert een ander deel van de functionaliteit van de oplossing. Zie voor meer informatie over de algehele architectuur, [externe controle vooraf geconfigureerde oplossingsarchitectuur](iot-suite-remote-monitoring-sample-walkthrough.md).

Deze tabel bevat een overzicht van de huidige beschikbaarheid van elke microservice voor elke taal:

<!-- please add links for each of the repos in the table, you can find them here https://github.com/Azure/azure-iot-pcs-team/wiki/Repositories-->

| Microservice      | Beschrijving | Java | .NET |
| ----------------- | ----------- | ---- | ---- |
| Web-UI            | Web-app voor de oplossing voor externe controle. Gebruikersinterface met behulp van React.js framework implementeert. | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) | [N/A(React.js)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) |
| IoT Hub Manager   | Communicatie met de IoT Hub worden verwerkt.        | [Beschikbaar](https://github.com/Azure/iothub-manager-java) | [Beschikbaar](https://github.com/Azure/iothub-manager-dotnet)   |
| Verificatie    |  Azure Active Directory-integratie beheert.  | Nog niet beschikbaar | [Beschikbaar](https://github.com/Azure/pcs-auth-dotnet)   |
| Apparaatsimulatie | Een pool van de gesimuleerde apparaten beheert. | Nog niet beschikbaar | [Beschikbaar](https://github.com/Azure/device-simulation-dotnet)   |
| Telemetrie         | Maakt de apparaattelemetrie beschikbaar voor de gebruikersinterface. | [Beschikbaar](https://github.com/Azure/device-telemetry-java) | [Beschikbaar](https://github.com/Azure/device-telemetry-dotnet)   |
| Telemetrie-Agent   | De telemetriestroom analyseert, slaat de berichten van Azure IoT Hub en genereert waarschuwingen volgens de gedefinieerde regels.  | [Beschikbaar](https://github.com/Azure/telemetry-agent-java) | [Beschikbaar](https://github.com/Azure/telemetry-agent-dotnet)   |
| UI-configuratie         | Beheert de configuratiegegevens van de gebruikersinterface. | [Beschikbaar](https://github.com/azure/pcs-ui-config-java) | [Beschikbaar](https://github.com/azure/pcs-ui-config-dotnet)   |
| Opslagadapter   |  Interactie met storage-service beheert.   | [Beschikbaar](https://github.com/azure/pcs-storage-adapter-java) | [Beschikbaar](https://github.com/azure/pcs-storage-adapter-dotnet)   |
| Omgekeerde proxy     | Beschrijft de persoonlijke bronnen op een beheerde manier via een unieke eindpunt. | Nog niet beschikbaar | [Beschikbaar](https://github.com/Azure/reverse-proxy-dotnet)   |

De Java-oplossing gebruikt momenteel de .NET-authenticatie, simulatie en microservices omgekeerde proxy. Deze microservices wordt vervangen door Java-versies, zodra deze beschikbaar komen.

## <a name="presentation-and-visualization"></a>Presentatie en visualisatie

De volgende secties worden de opties voor het aanpassen van de laag voor presentatie en visualisaties in de oplossing voor externe controle:

### <a name="change-the-logo-in-the-ui"></a>Het logo in de gebruikersinterface wijzigen

De standaardimplementatie gebruikmaakt van de naam van het bedrijf Contoso en het logo in de gebruikersinterface. Wijzigen van deze UI-elementen als uw bedrijfsnaam en het logo wilt weergeven:

1. Gebruik de volgende opdracht voor het klonen van de Web-UI-bibliotheek:

    ```cmd/sh
    git clone https://github.com/Azure/pcs-remote-monitoring-webui.git
    ```

1. Als u wilt de bedrijfsnaam wijzigen, opent u de `src/common/lang.js` bestand in een teksteditor.

1. Zoek de volgende regel in het bestand:

    ```js
    CONTOSO: 'Contoso',
    ```

1. Vervang `Contoso` met de naam van uw bedrijf. Bijvoorbeeld:

    ```js
    CONTOSO: 'YourCo',
    ```

1. Sla het bestand op.

1. Voor het bijwerken van het logo, Voeg een nieuw SVG-bestand naar de `assets/icons` map. Het bestaande logo is de `assets/icons/Contoso.svg` bestand.

1. Open de `src/components/layout/leftNav/leftNav.js` bestand in een teksteditor.

1. Zoek de volgende regel in het bestand:

    ```js
    import ContosoIcon from '../../../assets/icons/Contoso.svg';
    ```

1. Vervang `Contoso.svg` met de naam van uw logobestand. Bijvoorbeeld:

    ```js
    import ContosoIcon from '../../../assets/icons/YourCo.svg';
    ```

1. Zoek de volgende regel in het bestand:

    ```js
    alt="ContosoIcon"
    ```

1. Vervang `ContosoIcon` met uw `alt` tekst. Bijvoorbeeld:

    ```js
    alt="YourCoIcon"
    ```

1. Sla het bestand op.

1. Als u wilt testen of de wijzigingen, kunt u uitvoeren de bijgewerkte `webui` op uw lokale machine. Voor informatie over het bouwen en uitvoeren van de `webui` oplossing lokaal, Zie [samenstellen, uitvoeren en test lokaal](https://github.com/Azure/pcs-remote-monitoring-webui/blob/master/README.md#build-run-and-test-locally) in de `webui` GitHub-opslagplaats Leesmij-bestand.

1. Zie voor het implementeren van de wijzigingen de [verwijzing ontwikkelaarshandleiding](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide).

<!--

### Add a new KPI to the Dashboard page

The following steps describe how to add a new KPI to display on the **Dashboard** page. The new KPI shows information about the number of alarms with specific status values as a pie chart:

1. Step 1

1. Step 2
-->

### <a name="customize-the-map"></a>De kaart aanpassen

Zie de [aanpassen kaart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) pagina in GitHub voor meer informatie over de onderdelen van de kaart in de oplossing.

<!--
### Customize the telemetry chart

See the [Customize telemetry chart](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of the telemetry chart components in the solution.

### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

### Duplicate an existing control

To duplicate an existing UI element such as a chart or alert, see the [Duplicate a control](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub.

-->

### <a name="other-customization-options"></a>Andere aanpassingsopties

Als u wilt de presentatie en visualisaties laag in de oplossing voor externe controle verder wijzigen, kunt u de code bewerken. De relevante GitHub-opslagplaatsen zijn:

* [UIConfig (.NET)](https://github.com/Azure/pcs-ui-config-dotnet/)
* [UIConfig (Java)](https://github.com/Azure/pcs-ui-config-java/)
* [Azure-pc's externe controle WebUI](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="device-connectivity-and-streaming"></a>Connectiviteit van apparaten en streaming

De volgende secties worden de opties voor het aanpassen van de connectiviteit van apparaten en streaming laag in de oplossing voor externe controle. [Apparaatmodellen](https://github.com/Azure/device-simulation-dotnet/wiki/Device-Models) beschrijven de apparaattypen en telemetrie in de oplossing. U apparaatmodellen voor gesimuleerde en fysieke apparaten gebruiken.

Zie voor een voorbeeld van een implementatie van het fysieke apparaat, [Verbind het apparaat met de vooraf geconfigureerde oplossing voor externe controle](iot-suite-connecting-devices-node.md).

Als u een _fysiek apparaat_, moet u de clienttoepassing met een model van het apparaat met de specificatie van metagegevens en telemetrie apparaat opgeven.

De volgende secties worden besproken met behulp van apparaatmodellen met gesimuleerde apparaten:

### <a name="add-a-telemetry-type"></a>Een type telemetrie toevoegen

De typen apparaten in de Contoso-demo-oplossing geeft de telemetrie die elk apparaattype verzendt. Als u de aanvullende telemetrie-typen, kan een apparaat telemetrie definities als metagegevens verzenden naar de oplossing. Als u deze indeling gebruikt, het dashboard uw apparaattelemetrie en de beschikbare methoden dynamisch verbruikt en u hoeft niet te wijzigen van de gebruikersinterface. U kunt ook de typedefinitie van het apparaat in de oplossing wijzigen.

Voor informatie over het toevoegen van aangepaste telemetrie in de _apparaatsimulator_ microservice, Zie [testen van uw oplossing met gesimuleerde apparaten](iot-suite-remote-monitoring-test.md).

### <a name="add-a-device-type"></a>Een apparaattype toevoegen

De Contoso-demo-oplossing definieert sommige apparaattypen voorbeeld. De oplossing kunt u aangepast apparaattypen om te voldoen aan de vereisten van uw specifieke toepassing definiëren. Uw bedrijf kan bijvoorbeeld een industriële gateway gebruiken als het primaire apparaat is verbonden met de oplossing.

Voor het maken van een nauwkeurige weergave van uw apparaat, moet u de toepassing die wordt uitgevoerd op uw apparaat om te voldoen aan de apparaatvereisten wijzigen.

Voor informatie over het toevoegen van een nieuw apparaattype in de _apparaatsimulator_ microservice, Zie [testen van uw oplossing met gesimuleerde apparaten](iot-suite-remote-monitoring-test.md).

### <a name="define-custom-methods-for-simulated-devices"></a>Aangepaste methoden voor het gesimuleerde apparaten definiëren

Zie voor meer informatie over het definiëren van aangepaste methoden voor het gesimuleerde apparaten in de oplossing voor externe controle, [Apparaatmodellen](https://github.com/Azure/device-simulation-dotnet/wiki/%5BAPI-Specifications%5D-Device-Models) in de GitHub-opslagplaats.

<!--
#### Using the simulator service

TODO: add steps for the simulator microservice here
-->

#### <a name="using-a-physical-device"></a>Met behulp van een fysiek apparaat

Als u wilt implementeren methoden en taken op uw fysieke apparaten, Zie de volgende artikelen voor IoT Hub:

* [Begrijpen en direct methoden uit IoT Hub aanroepen](../iot-hub/iot-hub-devguide-direct-methods.md).
* [Plannen van taken op meerdere apparaten](../iot-hub/iot-hub-devguide-jobs.md).

### <a name="other-customization-options"></a>Andere aanpassingsopties

Als u wilt wijzigen verder de connectiviteit van apparaten en streaming laag in de oplossing voor externe controle, kunt u de code bewerken. De relevante GitHub-opslagplaatsen zijn:

* [De Apparaattelemetrie (.NET)](https://github.com/Azure/device-telemetry-dotnet)
* [De Apparaattelemetrie (Java)](https://github.com/Azure/device-telemetry-java)
* [Telemetry Agent (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Telemetry Agent (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="data-processing-and-analytics"></a>Gegevensverwerking en -analyse

<!--
The following sections describe options to customize the data processing and analytics layer in the remote monitoring solution:

### Rules and actions

See the [Customize rules and actions](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to customize the rules and actions in solution.


### Other customization options
-->

Voor het wijzigen van de gegevensverwerking en analytics laag in de oplossing voor externe controle, kunt u de code bewerken. De relevante GitHub-opslagplaatsen zijn:

* [Telemetry Agent (.NET)](https://github.com/Azure/telemetry-agent-dotnet)
* [Telemetry Agent (Java)](https://github.com/Azure/telemetry-agent-java)

## <a name="infrastructure"></a>Infrastructuur

<!--
The following sections describe options for customizing the infrastructure services in the remote monitoring solution:

### Change storage

The default storage service for the remote monitoring solution is Cosmos DB. See the [Customize storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses.

### Change log storage

The default storage service for logs is Cosmos DB. See the [Customize log storage service](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to change the storage service the solution uses for logging.

### Other customization options
-->

U kunt de code bewerken voor het wijzigen van de infrastructuur van de oplossing voor externe controle. De relevante GitHub-opslagplaatsen zijn:

* [IoTHub Manager (.NET)](https://github.com/Azure/iothub-manager-dotnet)
* [IoTHub Manager (Java)](https://github.com/Azure/iothub-manager-java)
* [Opslagadapter (.NET)](https://github.com/Azure/pcs-storage-adapter-dotnet)
* [Opslagadapter (Java)](https://github.com/Azure/pcs-storage-adapter-java)

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u geleerd over de resources beschikbaar om te helpen u de vooraf geconfigureerde oplossing aanpassen.

Zie voor meer informatie over de vooraf geconfigureerde oplossing voor externe controle [voor externe controle van architectuur](iot-suite-remote-monitoring-sample-walkthrough.md)

Zie voor meer informatie over het aanpassen van de oplossing voor externe controle:

* [Snelzoekgids voor ontwikkelaars](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide)
* [Ontwikkelaarsgids voor het oplossen van problemen](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Troubleshooting-Guide)

<!-- Next tutorials in the sequence -->