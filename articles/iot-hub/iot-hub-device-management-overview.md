<properties
 pageTitle="Overzicht van apparaatbeheer | Microsoft Azure"
 description="Overzicht van apparaatbeheer via Azure IoT Hub: dubbele apparaten, apparaatquery‘s, apparaattaken"
 services="iot-hub"
 documentationCenter=""
 authors="juanjperez"
 manager="timlt"
 editor=""/>

<tags
 ms.service="iot-hub"
 ms.devlang="na"
 ms.topic="get-started-article"
 ms.tgt_pltfrm="na"
 ms.workload="na"
 ms.date="04/29/2016"
 ms.author="juanpere"/>

# Overzicht van apparaatbeheer via Azure IoT Hub (preview-versie)

Met apparaatbeheer via Azure IoT Hub kunt u gebruikmaken van standaardapparaatbeheer via loT, waarmee u apparaten extern kunt beheren, configureren en bijwerken.

Er zijn drie belangrijke concepten voor apparaatbeheer in Azure IoT:

1.  **Dubbel apparaat:** de weergave van een fysiek apparaat in IoT Hub.

2.  **Apparaatquery's**: stelt u in staat om dubbele apparaten te vinden en meer te weten te komen over meerdere dubbele apparaten. U kunt bijvoorbeeld een query uitvoeren om alle dubbele apparaten te vinden met een firmwareversie 1.0.

3.  **Apparaattaken**: een actie die kan worden uitgevoerd op een of meer fysieke apparaten, zoals firmware bijwerken, opnieuw opstarten en fabrieksinstellingen terugzetten.

## Dubbel apparaat

Het dubbele apparaat is een weergave van een fysiek apparaat in IoT Hub. Het object **Microsoft.Azure.Devices.Device** wordt gebruikt om het dubbele apparaat weer te geven.

![][img-twin]

Het dubbele apparaat heeft de volgende onderdelen:

1.  **Apparaatvelden:** apparaatvelden zijn vooraf gedefinieerde eigenschappen die worden gebruikt voor zowel IoT Hub-berichten als apparaatbeheer. Hiermee kan IoT Hub fysieke apparaten identificeren en er verbinding mee maken. Apparaatvelden zijn niet gesynchroniseerd op het apparaat en zijn exclusief opgeslagen op het dubbele apparaat. Apparaatvelden bevatten de apparaat-id en verificatiegegevens.

2.  **Apparaateigenschappen:** apparaateigenschappen zijn een vooraf gedefinieerde woordenlijst met eigenschappen die een fysiek apparaat beschrijft. Een fysiek apparaat is het hoofdapparaat voor elke eigenschap en is de gezaghebbende opslag voor elke bijbehorende waarde. Een uiteindelijk consistente weergave van deze eigenschappen wordt opgeslagen op het dubbele apparaat in de cloud. De coherentie en vernieuwing zijn onderhevig aan de synchronisatie-instellingen zoals beschreven in [Tutorial: how to use the device twin][Ink-tutorial-twin] (Zelfstudie: het dubbele apparaat gebruiken). Enkele voorbeelden van apparaateigenschappen zijn de firmwareversie, het accuniveau en de naam van de fabrikant.

3.  **Service-eigenschappen:** service-eigenschappen zijn **&lt;sleutel-waardeparen&gt;** die door de ontwikkelaar worden toegevoegd aan de woordenlijst met service-eigenschappen. Deze eigenschappen breiden het gegevensmodel voor het dubbele apparaat uit, waardoor u het apparaat beter kunt typeren. Service-eigenschappen worden niet gesynchroniseerd naar het apparaat en worden alleen opgeslagen op het dubbele apparaat in de cloud. Een voorbeeld van een service-eigenschap is **&lt;NextServiceDate, 11/12/2017&gt;**. Deze eigenschap kan worden gebruikt om apparaten te vinden via hun volgende servicedatum.

4.  **Labels:** labels zijn een subset met service-eigenschappen die willekeurige tekenreeksen zijn in plaats van woordenlijsteigenschappen. Ze kunnen worden gebruikt om aantekeningen te maken bij dubbele apparaten of om apparaten te organiseren in groepen. Labels worden niet gesynchroniseerd naar het apparaat en worden alleen opgeslagen op het dubbele apparaat. Als een dubbel apparaat bijvoorbeeld een fysieke truck weergeeft, kunt u bijvoorbeeld een label toevoegen voor elk type lading: **appels**, **sinaasappels** of **bananen**.

## Apparaatquery's

In de vorige sectie hebt u geleerd over de verschillende onderdelen van het dubbele apparaat. Nu wordt uitgelegd hoe u dubbele apparaten kunt vinden in het IoT Hub-apaaraatregister op basis van de apparaateigenschappen, service-eigenschappen of labels. U kunt een query gebruiken om bijvoorbeeld apparaten te vinden die moeten worden bijgewerkt. U kunt een query uitvoeren voor alle apparaten met een opgegeven firmwareversie en het resultaat in een specifieke actie invoeren. (In IoT Hub bekend als een apparaattaak. Dit wordt uitgelegd in de volgende sectie).

U kunt een query uitvoeren met behulp van labels en eigenschappen:

-   Als u met behulp van labels een query wilt uitvoeren voor dubbele apparaten, geeft u een matrix van tekenreeksen op. De query retourneert vervolgens een set apparaten die zijn gelabeld met al deze tekenreeksen.

-   Als u een query naar dubbele apparaten wilt uitvoeren met behulp van service-eigenschappen of apparaateigenschappen, gebruikt u een JSON-query-expressie. In het volgende voorbeeld ziet u hoe u een query kunt uitvoeren voor alle apparaten met de apparaateigenschap met de sleutel **FirmwareVersion** en waarde **1.0**. U kunt zien dat het **type** van de eigenschap **apparaat** is. Dit geeft aan dat er een query wordt uitgevoerd op basis van apparaateigenschappen en niet op basis van service-eigenschappen:

  ```
  {                           
      "filter": {                  
        "property": {                
          "name": "FirmwareVersion",   
          "type": "device"             
        },                           
        "value": "1.0",              
        "comparisonOperator": "eq",  
        "type": "comparison"         
      },                           
      "project": null,             
      "aggregate": null,           
      "sort": null                 
  }
  ```

## Apparaattaken

Het volgende concept in apparaatbeheer is apparaattaken. Dit zorgt voor de coördinatie van orchestrations met meerdere stappen op meerdere apparaten.

Er worden momenteel zes typen apparaattaken geboden met apparaatbeheer via Azure IoT Hub (wanneer klanten daar behoefte aan hebben, zullen er meer taken worden toegevoegd):

- **Firmware-update**: hiermee wordt de firmware (of OS-installatiekopie) bijgewerkt op het fysieke apparaat.
- **Opnieuw opstarten**: hiermee wordt het fysieke apparaat opnieuw opgestart.
- **Fabrieksinstellingen**: hiermee wordt de firmware (of OS-installatiekopie) van het fysieke apparaat teruggezet naar de fabrieksinstellingen, mits de installatiekopie is opgeslagen op het apparaat.
- **Configuratie-update**: hiermee wordt de IoT Hub-clientagent geconfigureerd die wordt uitgevoerd op het fysieke apparaat.
- **Apparaateigenschap Lezen**: hiermee wordt de meest recente waarde van een apparaateigenschap opgehaald op het fysieke apparaat.
- **Apparaateigenschap Schrijven:** hiermee wordt een apparaateigenschap op het fysieke apparaat gewijzigd.

Zie de [API documentation for C\# and node.js][Ink-apidocs] (API-documentatie voor C en node.JS) voor details over het gebruik van elk van deze taken.

Een taak kan worden uitgevoerd op meerdere apparaten. Wanneer u een taak start, wordt voor elk van deze apparaten een bijbehorende onderliggende taak gemaakt. Een onderliggende taak werkt op een enkel apparaat. Elke onderliggende taak bevat een verwijzing naar de bovenliggende taak. De bovenliggende taak is slechts een container voor de onderliggende taken. Er wordt met deze taak geen logica geïmplementeerd om onderscheid te maken tussen typen apparaten (zoals het bijwerken van een Intel Edison versus het bijwerken van een Raspberry Pi). In het volgende diagram wordt de relatie weergegeven tussen een bovenliggende taak en de bijbehorende onderliggende taken, en de bijbehorende fysieke apparaten.

![][img-jobs]

U kunt een query uitvoeren voor de taakgeschiedenis om de status te begrijpen van de taken die u hebt gestart. Zie [onze querybibliotheek][Ink-query-samples] voor een aantal voorbeelden van query‘s.

## Apparaatimplementatie

Nu de concepten aan de zijde van de server zijn besproken, kunnen we het hebben over het maken van een beheerd fysiek apparaat. Met de DM-clientbibliotheek van Azure IoT Hub kunt u IoT-apparaten beheren via Azure IoT Hub. Met beheren worden acties bedoeld zoals opnieuw opstarten, fabrieksinstellingen terugzetten en het bijwerken van firmware.  Op dit moment bieden we een platformonafhankelijke C-bibliotheek, maar binnenkort wordt ondersteuning voor andere talen toegevoegd.  

De DM-clientbibliotheek heeft twee hoofdverantwoordelijkheden in apparaatbeheer:

- Eigenschappen op het fysieke apparaat synchroniseren met het bijbehorende dubbele apparaat in IoT Hub
- Apparaattaken besturen die via IoT Hub zijn verzonden naar het apparaat

Zie [Introducing the Azure IoT Hub device management client library for C][Ink-library-c] (Introductie tot de clientbibliotheek voor apparaatbeheer via Azure IoT Hub voor C) voor meer informatie over deze verantwoordelijkheden en de implementatie op fysieke apparaten.

## Volgende stappen

U kunt de SDK's van het IoT-apparaat gebruiken voor het implementeren van clienttoepassingen op een groot aantal hardwareplatforms en besturingssystemen. De SDK's van het IoT-apparaat bevatten bibliotheken die het eenvoudiger maken om telemetrie te verzenden naar een IoT hub en cloud-naar-apparaatopdrachten te ontvangen. Wanneer u de SDK's gebruikt, kunt u kiezen uit een aantal netwerkprotocollen om te communiceren met IoT Hub. Raadpleeg ook de [informatie over apparaat-SDK's][lnk-apparaat-SDK‘s].

Zie de zelfstudie [Get started with Azure IoT Hub device management][Ink-get-started] (Aan de slag met apparaatbeheer via Azure IoT Hub) om nog meer te leren over de functies voor apparaatbeheer via Azure IoT Hub.

<!-- Images and links -->
[img-twin]: media/iot-hub-device-management-overview/image1.png
[img-jobs]: media/iot-hub-device-management-overview/image2.png
[img-client]: media/iot-hub-device-management-overview/image3.png

[lnk-lwm2m]: http://technical.openmobilealliance.org/Technical/technical-information/release-program/current-releases/oma-lightweightm2m-v1-0
[lnk-library-c]: iot-hub-device-management-library.md
[lnk-get-started]: iot-hub-device-management-get-started.md
[lnk-tutorial-twin]: iot-hub-device-management-device-twin.md
[lnk-apidocs]: http://azure.github.io/azure-iot-sdks/
[lnk-query-samples]: https://github.com/Azure/azure-iot-sdks/blob/dmpreview/doc/get_started/dm_queries/query-samples.md
[lnk-apparaat-SDK‘s]: https://github.com/Azure/azure-iot-sdks



<!---HONumber=ago16_HO4-->


