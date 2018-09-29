---
title: Meer informatie over apparaatdubbels Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - apparaatdubbels gebruiken om de status en configuratie van gegevens tussen IoT-Hub en uw apparaten te synchroniseren
author: fsautomata
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/29/2018
ms.author: elioda
ms.openlocfilehash: 9997bf49ee986d6ceaa566e14edea0b9469fa14e
ms.sourcegitcommit: f31bfb398430ed7d66a85c7ca1f1cc9943656678
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/28/2018
ms.locfileid: "47452374"
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Begrijpen en gebruiken van apparaatdubbels in IoT Hub

*Apparaatdubbels* JSON-documenten waarin statusinformatie van apparaat met inbegrip van metagegevens, configuraties en voorwaarden zijn. Azure IoT Hub onderhoudt een apparaatdubbel voor elk apparaat dat u verbinding met IoT Hub maakt. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Dit artikel wordt beschreven:

* De structuur van de apparaatdubbel: *tags*, *gewenste* en *gerapporteerde eigenschappen*.
* De bewerkingen die apps op apparaten en back-ends op dubbele apparaten kunnen uitvoeren.

Apparaatdubbels om te gebruiken:

* Store apparaatspecifieke metagegevens in de cloud. Bijvoorbeeld, de implementatielocatie van een Verkoopautomaat.

* Rapport van informatie over de huidige status, zoals de beschikbare mogelijkheden en voorwaarden van uw apparaat-app. Bijvoorbeeld, een apparaat is verbonden met uw IoT-hub via mobiel of Wi-Fi.

* De status van langlopende werkstromen tussen apparaat-app en de back-end-app worden gesynchroniseerd. Wanneer de oplossing een back-end Hiermee geeft u de nieuwe firmwareversie te installeren en de verschillende stadia van het updateproces van de apparaat-app-rapporten.

* Query uitvoeren op de metagegevens van apparaten, de configuratie of de status.

Raadpleeg [richtlijnen voor communicatie van apparaat-naar-cloud](iot-hub-devguide-d2c-guidance.md) voor informatie over het gebruik van de gerapporteerde eigenschappen, apparaat-naar-cloud-berichten of bestand uploaden.

Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen](iot-hub-devguide-c2d-guidance.md) voor informatie over het gebruik van de gewenste eigenschappen, directe methoden of cloud-naar-apparaat-berichten.

## <a name="device-twins"></a>Apparaatdubbels

Apparaatdubbels apparaatgerelateerde gegevens opslaan die:

* Apparaat- en back-ends kunnen gebruiken om te synchroniseren apparaat voorwaarden en de configuratie.

* De back-end oplossing kunt gebruiken voor query- en doel langlopende bewerkingen.

De levenscyclus van een apparaatdubbel is gekoppeld aan de bijbehorende [apparaat-id](iot-hub-devguide-identity-registry.md). Apparaatdubbels worden impliciet gemaakt en verwijderd wanneer een apparaat-id is gemaakt of verwijderd uit IoT Hub.

Een apparaatdubbel is een JSON-document met:

* **Tags**. Een gedeelte van het JSON-document dat de back-end van de oplossing kan lezen en schrijven naar. Tags zijn niet zichtbaar voor apps op apparaten.

* **Gewenste eigenschappen**. Dit wordt gebruikt, samen met de gerapporteerde eigenschappen om te synchroniseren apparaatconfiguratie of voorwaarden. De back-end oplossing gewenste eigenschappen kunt instellen en deze kan worden gelezen door de apparaat-app. De apparaat-app kan ook met het ontvangen van meldingen van wijzigingen in de gewenste eigenschappen.

* **Gerapporteerde eigenschappen**. Samen met de gewenste eigenschappen gebruikt om te synchroniseren apparaatconfiguratie of voorwaarden. De apparaat-app gerapporteerde eigenschappen kunt instellen, en de back-end oplossing kunt lezen en query's voor uitvoert.

* **Eigenschappen van apparaat-id**. De hoofdmap van het apparaat dubbele JSON-document bevat de alleen-lezen eigenschappen van de bijbehorende apparaat-id die zijn opgeslagen in de [id-register](iot-hub-devguide-identity-registry.md).

![Schermopname van het apparaat](./media/iot-hub-devguide-device-twins/twin.png)

Het volgende voorbeeld ziet u een apparaatdubbel JSON-document:

```json
{
    "deviceId": "devA",
    "etag": "AAAAAAAAAAc=", 
    "status": "enabled",
    "statusReason": "provisioned",
    "statusUpdateTime": "0001-01-01T00:00:00",
    "connectionState": "connected",
    "lastActivityTime": "2015-02-30T16:24:48.789Z",
    "cloudToDeviceMessageCount": 0, 
    "authenticationType": "sas",
    "x509Thumbprint": {     
        "primaryThumbprint": null, 
        "secondaryThumbprint": null 
    }, 
    "version": 2, 
    "tags": {
        "$etag": "123",
        "deploymentLocation": {
            "building": "43",
            "floor": "1"
        }
    },
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata" : {...},
            "$version": 1
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            },
            "batteryLevel": 55,
            "$metadata" : {...},
            "$version": 4
        }
    }
}
```

In het hoofdobject zijn de apparaat-id-eigenschappen en container-objecten voor `tags` , en beide `reported` en `desired` eigenschappen. De `properties` container bevat bepaalde onderdelen van de alleen-lezen (`$metadata`, `$etag`, en `$version`) die worden beschreven in de [dubbele Apparaatmetagegevens](iot-hub-devguide-device-twins.md#device-twin-metadata) en [optimistische gelijktijdigheid](iot-hub-devguide-device-twins.md#optimistic-concurrency) de secties.

### <a name="reported-property-example"></a>Voorbeeld van de gerapporteerde eigenschap

In het vorige voorbeeld de apparaatdubbel bevat een `batteryLevel` eigenschap die moet worden gerapporteerd door de apparaat-app. Deze eigenschap maakt het mogelijk is om te zoeken en op apparaten die zijn gebaseerd op de laatste gerapporteerde accu werkt. Andere voorbeelden zijn onder meer de apparaat-app-rapportage apparaatmogelijkheden of opties voor netwerkconnectiviteit.

> [!NOTE]
> Gerapporteerde eigenschappen vereenvoudigen van scenario's waarin de back-end van de oplossing is geïnteresseerd in de laatste bekende waarde van een eigenschap. Gebruik [apparaat-naar-cloud-berichten](iot-hub-devguide-messages-d2c.md) als de back-end van de oplossing moet verwerken telemetrie van apparaten in de vorm van reeksen voorzien van een tijdstempel gebeurtenissen, zoals een tijdreeks.

### <a name="desired-property-example"></a>Voorbeeld van de gewenste eigenschap

In het vorige voorbeeld de `telemetryConfig` apparaatdubbel-gewenste en gerapporteerde eigenschappen worden gebruikt door de back-end van de oplossing en de apparaat-app om te synchroniseren van de configuratie van de telemetrie voor dit apparaat. Bijvoorbeeld:

1. De back-end oplossing Hiermee stelt u de gewenste eigenschap met de waarde van de gewenste configuratie. Dit is het gedeelte van het document met de gewenste eigenschap is ingesteld:

   ```json
   "desired": {
       "telemetryConfig": {
           "sendFrequency": "5m"
       },
       ...
   },
   ```

2. De apparaat-app wordt op de hoogte gesteld van de wijziging onmiddellijk als verbonden of op de eerste opnieuw verbinding te maken. De apparaat-app vervolgens de bijgewerkte configuratie rapporten (of een fout voorwaarde met behulp van de `status` eigenschap). Dit is het gedeelte van de gerapporteerde eigenschappen:

   ```json
   "reported": {
       "telemetryConfig": {
           "sendFrequency": "5m",
           "status": "success"
       }
       ...
   }
   ```

3. De back-end oplossing kunt de resultaten van de configuratiebewerking bijhouden op veel apparaten door [uitvoeren van query's](iot-hub-devguide-query-language.md) dubbele apparaten.

> [!NOTE]
> De voorgaande fragmenten zijn voorbeelden, geoptimaliseerd voor de leesbaarheid van een manier om de configuratie van een apparaat en de status ervan te coderen. IoT Hub legt een specifieke schema voor het dubbele apparaat gewenst en eigenschappen in de dubbele apparaten gerapporteerde.
> 

Dubbele kunt u langlopende bewerkingen zoals het firmware-updates synchroniseren. Zie voor meer informatie over het gebruik van eigenschappen voor het synchroniseren en een langdurige bewerking bijhouden op apparaten [gebruik gewenste eigenschappen om apparaten te configureren](tutorial-device-twins.md).

## <a name="back-end-operations"></a>Back-end-bewerkingen

De back-end van de oplossing is van invloed op het dubbele apparaat met behulp van de volgende atomische bewerkingen, die toegankelijk is via HTTPS:

* **Ophalen van de apparaatdubbel op ID**. Met deze bewerking wordt het apparaat dubbele document, inclusief tags en eigenschappen van het gewenste en gerapporteerd.

* **Gedeeltelijk bijwerken apparaatdubbel**. Met deze bewerking kunt de back-end oplossing voor gedeeltelijk tags of in een apparaatdubbel-gewenste eigenschappen bijwerken. De gedeeltelijke update wordt uitgedrukt als een JSON-document dat een eigenschap bijgewerkt of toegevoegd. Eigenschappen ingesteld op `null` worden verwijderd. Het volgende voorbeeld wordt een nieuwe gewenste eigenschap met de waarde `{"newProperty": "newValue"}`, overschrijft de bestaande waarde van `existingProperty` met `"otherNewValue"`, en verwijdert u `otherOldProperty`. Er zijn geen andere wijzigingen zijn aangebracht aan de bestaande gewenste eigenschappen of labels:

   ```json
   {
       "properties": {
           "desired": {
               "newProperty": {
                   "nestedProperty": "newValue"
               },
               "existingProperty": "otherNewValue",
               "otherOldProperty": null
           }
       }
   }
   ```

* **Gewenste eigenschappen vervangen**. Met deze bewerking kunt u de back-end oplossing voor het volledig overschrijft alle bestaande gewenste eigenschappen en vervangen door een nieuwe JSON-document voor `properties/desired`.

* **Vervang tags**. Met deze bewerking kunt u de back-end oplossing voor het volledig overschrijft alle bestaande tags en vervangen door een nieuwe JSON-document voor `tags`.

* **Dubbele meldingen ontvangen**. Met deze bewerking kunt de back-end oplossing om te worden geïnformeerd wanneer het dubbele wordt gewijzigd. Om dit te doen, moet uw IoT-oplossing een route te maken en in te stellen de gegevensbron gelijk zijn aan *twinChangeEvents*. Standaard bestaat geen dergelijke routes vooraf, zodat er geen dubbele meldingen worden verzonden. Als de wijzigingssnelheid te hoog is, of om andere redenen, zoals interne fouten, worden de IoT-Hub slechts één melding alle wijzigingen bevat verzonden. Als uw toepassing moet betrouwbare controle en logboekregistratie van alle tussenliggende statussen, moet u apparaat-naar-cloud-berichten. Het meldingsbericht dubbele bevat eigenschappen en de hoofdtekst.

   - Eigenschappen

   | Naam | Waarde |
   | --- | --- |
   $content-type | application/json |
   $iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
   $iothub-bericht-bron | twinChangeEvents |
   $content-codering | utf-8 |
   deviceId | ID van het apparaat |
   HubName | Naam van IoT Hub |
   operationTimestamp | [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) timestamp van bewerking |
   iothub-bericht-schema | deviceLifecycleNotification |
   opType | "replaceTwin" of "updateTwin" |

   Systeemeigenschappen bericht worden voorafgegaan door de `$` symbool.

   - Hoofdtekst
        
   Deze sectie bevat alle wijzigingen in de apparaatdubbel in een JSON-indeling. Het maakt gebruik van dezelfde indeling als een patch, met het verschil dat deze alle dubbele secties kan bevatten: labels, properties.reported, properties.desired en dat deze de elementen '$metadata bevat'. Bijvoorbeeld:

   ```json
   {
       "properties": {
           "desired": {
               "$metadata": {
                   "$lastUpdated": "2016-02-30T16:24:48.789Z"
               },
               "$version": 1
           },
           "reported": {
               "$metadata": {
                   "$lastUpdated": "2016-02-30T16:24:48.789Z"
               },
               "$version": 1
           }
       }
   }
   ```

Ondersteuning voor alle voorgaande bewerkingen [optimistische gelijktijdigheid](iot-hub-devguide-device-twins.md#optimistic-concurrency) en vereisen dat de **ServiceConnect** machtiging, zoals gedefinieerd in [beheren van toegang tot IoT Hub](iot-hub-devguide-security.md).

Naast deze bewerkingen is de back-end oplossing kunt doen:

* De dubbele apparaten met behulp van de SQL-achtige query [IoT Hub-querytaal](iot-hub-devguide-query-language.md).

* Bewerkingen uitvoeren op grote sets met behulp van apparaatdubbels [taken](iot-hub-devguide-jobs.md).

## <a name="device-operations"></a>Apparaatbewerkingen

De apparaat-app is van invloed op het dubbele apparaat met behulp van de volgende atomische bewerkingen:

* **Ophalen van de apparaatdubbel**. Met deze bewerking wordt het apparaat dubbele-document (met inbegrip van de labels en gewenste en gerapporteerde eigenschappen) voor de momenteel verbonden apparaat.

* **Gedeeltelijk gerapporteerde eigenschappen bij te werken**. Met deze bewerking kunt het gedeeltelijke update van de gerapporteerde eigenschappen van de momenteel verbonden apparaat. Deze bewerking wordt de dezelfde JSON-update-indeling dat de oplossing back-end wordt gebruikt voor een gedeeltelijke update van gewenste eigenschappen.

* **Gewenste eigenschappen observeren**. De momenteel verbonden apparaat kunt kiezen om te worden geïnformeerd over updates voor de gewenste eigenschappen wanneer ze zich voordoen. Het apparaat ontvangt de dezelfde vorm van update (gedeeltelijke of volledige vervanging) dat door de back-end van de oplossing wordt uitgevoerd.

De voorgaande behoeft de **DeviceConnect** machtiging, zoals gedefinieerd in [toegang tot IoT Hub regelen](iot-hub-devguide-security.md).

De [Azure IoT device SDK's](iot-hub-devguide-sdks.md) gemakkelijk te gebruiken van de voorgaande bewerkingen van veel talen en platforms. Zie voor meer informatie over de details van IoT-Hub primitieven voor synchronisatie van de gewenste eigenschappen [apparaat opnieuw verbinden met flow](iot-hub-devguide-device-twins.md#device-reconnection-flow).

## <a name="tags-and-properties-format"></a>Indeling van tags en eigenschappen

Labels en gewenste eigenschappen, gerapporteerde eigenschappen zijn JSON-objecten met de volgende beperkingen:

* Alle sleutels in JSON-objecten zijn hoofdlettergevoelig 64 bytes UNICODE UTF-8-tekenreeksen. Toegestane tekens uitsluiten Unicode-tekens (segmenten C0 en C1), en `.`, `$`, en SP

* Alle waarden in de JSON-objecten van de volgende JSON-typen zijn: boolean, getal, string, object. Matrices zijn niet toegestaan. De maximale waarde voor gehele getallen is 4503599627370495 en de minimale waarde voor gehele getallen-4503599627370496 is.

* Alle JSON-objecten in de gewenste en gerapporteerde eigenschappen-tags kunnen hebben een maximale diepte van 5. Bijvoorbeeld, geldt het volgende object:

   ```json
   {
       ...
       "tags": {
           "one": {
               "two": {
                   "three": {
                       "four": {
                           "five": {
                               "property": "value"
                           }
                       }
                   }
               }
           }
       },
       ...
   }
   ```

* Alle tekenreekswaarden kunnen maximaal 512 bytes lang zijn.

## <a name="device-twin-size"></a>Grootte van de dubbele apparaat

IoT Hub dwingt een beperking van de grootte van 8KB op elk van de respectieve totale waarden van `tags`, `properties/desired`, en `properties/reported`, met uitzondering van alleen-lezen-elementen.

De grootte wordt berekend door het tellen van alle tekens, met uitzondering van Unicode-tekens (segmenten C0 en C1) en die zich buiten de tekenreeksconstanten.

IoT Hub is geweigerd door een fout. alle bewerkingen die de grootte van deze documenten boven de limiet wilt verhogen.

## <a name="device-twin-metadata"></a>Metagegevens van de dubbele apparaten

IoT Hub onderhoudt de tijdstempel van de laatste update voor elk JSON-object in apparaatdubbel gewenst en gerapporteerde eigenschappen. De tijdstempels in UTC en gecodeerd in de [ISO8601](https://en.wikipedia.org/wiki/ISO_8601) indeling `YYYY-MM-DDTHH:MM:SS.mmmZ`.

Bijvoorbeeld:

```json
{
    ...
    "properties": {
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": {
                        "$lastUpdated": "2016-03-30T16:24:48.789Z"
                    },
                    "$lastUpdated": "2016-03-30T16:24:48.789Z"
                },
                "$lastUpdated": "2016-03-30T16:24:48.789Z"
            },
            "$version": 23
        },
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            "batteryLevel": "55%",
            "$metadata": {
                "telemetryConfig": {
                    "sendFrequency": "5m",
                    "status": {
                        "$lastUpdated": "2016-03-31T16:35:48.789Z"
                    },
                    "$lastUpdated": "2016-03-31T16:35:48.789Z"
                }
                "batteryLevel": {
                    "$lastUpdated": "2016-04-01T16:35:48.789Z"
                },
                "$lastUpdated": "2016-04-01T16:24:48.789Z"
            },
            "$version": 123
        }
    }
    ...
}
```

Deze informatie wordt opgeslagen op elk niveau (niet alleen de knooppunten van de JSON-structuur) als u wilt behouden van updates die objectsleutels te verwijderen.

## <a name="optimistic-concurrency"></a>Optimistische gelijktijdige uitvoering

Tags, gewenste en gerapporteerde eigenschappen alle ondersteuning van optimistische gelijktijdigheid.
Tags zijn een ETag volgens [RFC7232](https://tools.ietf.org/html/rfc7232), die de JSON-weergave van de tag vertegenwoordigt. U kunt ETags in voorwaardelijke bewerkingen van de back-end oplossing gebruiken om te zorgen voor consistentie.

Apparaatdubbel gewenst en gerapporteerde eigenschappen geen ETags, maar hebben een `$version` waarde die kan worden gegarandeerd incrementeel zijn. Op dezelfde manier naar een ETag, kan de versie worden gebruikt door de partij bijwerken voor het afdwingen van consistentie van updates. Bijvoorbeeld, een apparaat-app voor een gerapporteerde eigenschap of de back-end oplossing voor een gewenste eigenschap.

Versies zijn ook handig wanneer een observing agent (zoals de apparaat-app de gewenste eigenschappen geobserveerd) op elkaar stappen tussen het resultaat van een bewerking ophalen en een melding van updates afstemmen moet. De [apparaat opnieuw verbinden met flow sectie](iot-hub-devguide-device-twins.md#device-reconnection-flow) vindt u meer informatie.

## <a name="device-reconnection-flow"></a>Apparaat opnieuw verbinden met flow

IoT Hub blijven niet behouden updatemeldingen gewenste eigenschappen voor niet-verbonden apparaten. Betekent dit dat het document volledige gewenste eigenschappen, naast het abonneren op updatemeldingen moet worden opgehaald door een apparaat dat verbinding maakt. Gezien de mogelijkheid van stappen tussen updatemeldingen en volledige ophalen, moet de volgende stroom worden gewaarborgd:

1. Apparaat-app maakt verbinding met een IoT-hub.
2. Apparaat-app Hiermee abonneert u zich voor de gewenste eigenschappen updatemeldingen.
3. Apparaat-app Hiermee haalt u het volledige document voor de gewenste eigenschappen.

De apparaat-app kunt negeren alle meldingen met `$version` kleiner dan of gelijk is dan de versie van het volledige document opgehaald. Deze aanpak is mogelijk omdat de IoT Hub garandeert dat versies altijd verhogen.

> [!NOTE]
> Deze logica wordt al geïmplementeerd de [Azure IoT device SDK's](iot-hub-devguide-sdks.md). Deze beschrijving is nuttig als de apparaat-app niet van Azure IoT device SDK's gebruiken en het MQTT-interface moet rechtstreeks programma.
> 

## <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is

Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* De [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) artikel beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.

* De [bandbreedtebeperking en quota's](iot-hub-devguide-quotas-throttling.md) artikel beschrijft de quota die betrekking hebben op de IoT Hub-service en de beperkingsgedrag u kunt verwachten wanneer u de service gebruiken.

* De [apparaat en de service Azure IoT-SDK's](iot-hub-devguide-sdks.md) artikel geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.

* De [IoT Hub-querytaal voor apparaatdubbels, taken en berichtroutering](iot-hub-devguide-query-language.md) artikel beschrijft de IoT Hub-querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.

* De [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) artikel vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu hebt u geleerd over apparaatdubbels, bent u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Begrijpen en gebruiken van moduledubbels in IoT Hub](iot-hub-devguide-module-twins.md)
* [Een rechtstreekse methode aanroepen op een apparaat](iot-hub-devguide-direct-methods.md)
* [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md)

Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, Zie de volgende zelfstudies voor IoT Hub:

* [Het gebruik van de apparaatdubbel](iot-hub-node-node-twin-getstarted.md)
* [Apparaatdubbeleigenschappen gebruiken](tutorial-device-twins.md)
* [Apparaatbeheer met Azure IoT Toolkit voor Visual Studio Code](iot-hub-device-management-iot-toolkit.md)