---
title: Meer informatie over moduledubbels Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - moduledubbels gebruiken om de status en configuratie van gegevens tussen IoT-Hub en uw apparaten te synchroniseren
author: chrissie926
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: 8f567ba43c1657783f9898863aef980627800481
ms.sourcegitcommit: e0834ad0bad38f4fb007053a472bde918d69f6cb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37436316"
---
# <a name="understand-and-use-module-twins-in-iot-hub"></a>Begrijpen en gebruiken van moduledubbels in IoT Hub

In dit artikel wordt ervan uitgegaan dat u hebt gelezen [begrijpen en gebruiken van apparaatdubbels in IoT Hub] [ lnk-devguide-device-twins] eerste. In IoT-Hub, kunt onder per apparaat-id, u maximaal 20 module identiteiten maken. De identiteit van elke module genereert impliciet een moduledubbel. Vergelijkbaar met dubbele apparaten, moduledubbels zijn JSON-documenten waarin statusinformatie van module met inbegrip van metagegevens, configuraties en voorwaarden. Azure IoT Hub onderhoudt een moduledubbel voor elke module die u met IoT Hub verbindt. 

De IoT Hub apparaat-SDK's aan de kant van het apparaat kunt u maken van modules die elk een onafhankelijke verbinding met IoT Hub wordt geopend. Hiermee kunt u afzonderlijke naamruimten voor de verschillende onderdelen op uw apparaat gebruikt. U hebt bijvoorbeeld een Verkoopautomaat waarvoor drie verschillende sensoren. De sensor wordt bepaald door de verschillende afdelingen in uw bedrijf. U kunt een module voor elke sensor maken. Op deze manier elke afdeling is alleen het verzenden van taken of directe methoden aan de sensor die ze beheren, voorkomen van conflicten en gebruikersfouten.

 Moduledubbel voor identiteits- en -module biedt dezelfde functionaliteit als de apparaat-id en het dubbele apparaat, maar op een fijnere granulatie. Deze weer specifieker kunt apparaten, zoals besturingssysteem op basis van apparaten of firmware-apparaten beheren van meerdere onderdelen, als u wilt isoleren, configuratie en voorwaarden voor elk van deze onderdelen. Module identiteits- en moduledubbels bieden een scheiding van problemen bij het werken met IoT-apparaten waarvoor modulaire software-onderdelen. We gericht op de ondersteuning van alle apparaat dubbele functionaliteit op het niveau van module dubbele door module dubbele algemene beschikbaarheid. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Dit artikel wordt beschreven:

* De structuur van de moduledubbel: *tags*, *gewenste* en *gerapporteerde eigenschappen*.
* De bewerkingen die de modules en back-ends op moduledubbels kunnen uitvoeren.

Raadpleeg [richtlijnen voor communicatie van apparaat-naar-cloud] [ lnk-d2c-guidance] voor informatie over het gebruik van de gerapporteerde eigenschappen, apparaat-naar-cloud-berichten of bestand uploaden.
Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen] [ lnk-c2d-guidance] voor informatie over het gebruik van de gewenste eigenschappen, directe methoden of cloud-naar-apparaat-berichten.

## <a name="module-twins"></a>Moduledubbels
Moduledubbels opslaan van informatie met betrekking tot de module die:

* Modules op het apparaat en IoT-Hub kunnen gebruiken om te synchroniseren van de voorwaarden van de module en de configuratie.
* De back-end oplossing kunt gebruiken voor query- en doel langlopende bewerkingen.

De levenscyclus van een moduledubbel is gekoppeld aan de bijbehorende [module identiteit][lnk-identity]. Modules dubbels worden impliciet gemaakt en verwijderd wanneer de identiteit van een module is gemaakt of verwijderd uit IoT Hub.

Een moduledubbel is een JSON-document met:

* **Tags**. Een gedeelte van het JSON-document dat de back-end van de oplossing kan lezen en schrijven naar. Tags zijn niet zichtbaar voor modules op het apparaat. Tags zijn ingesteld voor het uitvoeren van query's doel.
* **Gewenste eigenschappen**. Gebruikt samen met de gerapporteerde eigenschappen om te synchroniseren van configuratie van de module of voorwaarden. De back-end oplossing gewenste eigenschappen kunt instellen en deze kan worden gelezen door de module-app. De module-app kan ook met het ontvangen van meldingen van wijzigingen in de gewenste eigenschappen.
* **Gerapporteerde eigenschappen**. Gebruikt samen met de gewenste eigenschappen voor het synchroniseren van configuratie van de module of voorwaarden. De module app gerapporteerde eigenschappen kunt instellen en de back-end oplossing kunt lezen en query's voor uitvoert.
* **Identiteitseigenschappen van de module**. De hoofdmap van de module dubbele JSON-document bevat de alleen-lezen eigenschappen van de overeenkomstige module-id die zijn opgeslagen in de [id-register][lnk-identity].

![][img-module-twin]

Het volgende voorbeeld ziet u een moduledubbel JSON-document:

```json
{
    "deviceId": "devA",
    "moduleId": "moduleA",
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
            }
            "batteryLevel": 55,
            "$metadata" : {...},
            "$version": 4
        }
    }
}
```

In het hoofdobject zijn de module identiteitseigenschappen en container-objecten voor `tags` , en beide `reported` en `desired` eigenschappen. De `properties` container bevat bepaalde onderdelen van de alleen-lezen (`$metadata`, `$etag`, en `$version`) die worden beschreven in de [Module dubbele metagegevens] [ lnk-module-twin-metadata] en [ Optimistische gelijktijdigheid] [ lnk-concurrency] secties.

### <a name="reported-property-example"></a>Voorbeeld van de gerapporteerde eigenschap
In het vorige voorbeeld de moduledubbel bevat een `batteryLevel` eigenschap die moet worden gerapporteerd door de module-app. Deze eigenschap maakt het mogelijk is om te zoeken en modules op basis van de laatste gerapporteerde accuniveau gehandeld. Andere voorbeelden zijn onder meer de rapportage-module mogelijkheden voor app-module of opties voor netwerkconnectiviteit.

> [!NOTE]
> Gerapporteerde eigenschappen vereenvoudigen van scenario's waarin de back-end van de oplossing is geïnteresseerd in de laatste bekende waarde van een eigenschap. Gebruik [apparaat-naar-cloud-berichten] [ lnk-d2c] als de back-end van de oplossing moet verwerken module telemetrie in de vorm van reeksen voorzien van een tijdstempel gebeurtenissen, zoals een tijdreeks.

### <a name="desired-property-example"></a>Voorbeeld van de gewenste eigenschap
In het vorige voorbeeld de `telemetryConfig` moduledubbel gewenst en gerapporteerde eigenschappen worden gebruikt door de back-end van de oplossing en de module-app om te synchroniseren van de configuratie van de telemetrie voor deze module. Bijvoorbeeld:

1. De back-end oplossing Hiermee stelt u de gewenste eigenschap met de waarde van de gewenste configuratie. Dit is het gedeelte van het document met de gewenste eigenschap is ingesteld:

    ```json
    ...
    "desired": {
        "telemetryConfig": {
            "sendFrequency": "5m"
        },
        ...
    },
    ...
    ```

2. De module-app wordt op de hoogte gesteld van de wijziging onmiddellijk als verbonden of op de eerste opnieuw verbinding te maken. De module-app vervolgens de bijgewerkte configuratie rapporten (of een fout voorwaarde met behulp van de `status` eigenschap). Dit is het gedeelte van de gerapporteerde eigenschappen:

    ```json
    ...
    "reported": {
        "telemetryConfig": {
            "sendFrequency": "5m",
            "status": "success"
        }
        ...
    }
    ...
    ```

3. De back-end oplossing kunt volgen de resultaten van de configuratiebewerking in veel modules door [uitvoeren van query's] [ lnk-query] moduledubbels.

> [!NOTE]
> De voorgaande fragmenten zijn voorbeelden, geoptimaliseerd voor de leesbaarheid van een manier om het coderen van een configuratie van de module en de status ervan. IoT Hub legt een specifieke schema voor de moduledubbel gewenst en eigenschappen in de moduledubbels gerapporteerde.
> 
> 

## <a name="back-end-operations"></a>Back-end-bewerkingen
De back-end van de oplossing is van invloed op de moduledubbel met behulp van de volgende atomische bewerkingen, die toegankelijk is via HTTPS:

* **Ophalen van de moduledubbel op ID**. Deze bewerking wordt de module dubbele document, inclusief tags en eigenschappen van het gewenste en gerapporteerde geretourneerd.
* **Gedeeltelijk bijwerken moduledubbel**. Met deze bewerking kunt de back-end oplossing de tags of de gewenste eigenschappen in een moduledubbel gedeeltelijk bijwerken. De gedeeltelijke update wordt uitgedrukt als een JSON-document dat een eigenschap bijgewerkt of toegevoegd. Eigenschappen ingesteld op `null` worden verwijderd. Het volgende voorbeeld wordt een nieuwe gewenste eigenschap met de waarde `{"newProperty": "newValue"}`, overschrijft de bestaande waarde van `existingProperty` met `"otherNewValue"`, en verwijdert u `otherOldProperty`. Er zijn geen andere wijzigingen zijn aangebracht aan de bestaande gewenste eigenschappen of labels:

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
* **Dubbele meldingen ontvangen**. Met deze bewerking kunt de back-end oplossing om te worden geïnformeerd wanneer het dubbele wordt gewijzigd. Om dit te doen, moet uw IoT-oplossing een route te maken en in te stellen de gegevensbron gelijk zijn aan *twinChangeEvents*. Standaard geen dubbele meldingen worden verzonden, dat wil zeggen, er zijn geen dergelijke routes vooraf bestaan. Als de wijzigingssnelheid te hoog is, of om andere redenen, zoals interne fouten, worden de IoT-Hub slechts één melding alle wijzigingen bevat verzonden. Als uw toepassing moet betrouwbare controle en logboekregistratie van alle tussenliggende statussen, moet u apparaat-naar-cloud-berichten. Het meldingsbericht dubbele bevat eigenschappen en de hoofdtekst.

    - Eigenschappen

    | Naam | Waarde |
    | --- | --- |
    $content-type | application/json |
    $iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
    $iothub-bericht-bron | twinChangeEvents |
    $content-codering | utf-8 |
    deviceId | ID van het apparaat |
    moduleId | ID van de module |
    HubName | Naam van IoT Hub |
    operationTimestamp | [ISO8601] timestamp van bewerking |
    iothub-bericht-schema | deviceLifecycleNotification |
    opType | "replaceTwin" of "updateTwin" |

    Systeemeigenschappen bericht worden voorafgegaan door de `'$'` symbool.

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

Ondersteuning voor alle voorgaande bewerkingen [optimistische gelijktijdigheid] [ lnk-concurrency] en vereisen dat de **ServiceConnect** machtiging, zoals gedefinieerd in de [Security] [ lnk-security] artikel.

Naast deze bewerkingen is de back-end oplossing kunt doen:

* De moduledubbels met behulp van de SQL-achtige query [IoT Hub-querytaal][lnk-query].

## <a name="module-operations"></a>Modulebewerkingen
De module-app is van invloed op de moduledubbel met behulp van de volgende atomische bewerkingen:

* **Ophalen van de moduledubbel**. Met deze bewerking wordt het module-twin-document (met inbegrip van de labels en gewenste en gerapporteerde eigenschappen) voor de momenteel verbonden module.
* **Gedeeltelijk gerapporteerde eigenschappen bij te werken**. Met deze bewerking kunt het gedeeltelijke update van de gerapporteerde eigenschappen van de momenteel verbonden module. Deze bewerking wordt de dezelfde JSON-update-indeling dat de oplossing back-end wordt gebruikt voor een gedeeltelijke update van gewenste eigenschappen.
* **Gewenste eigenschappen observeren**. De momenteel verbonden module kunt kiezen om te worden geïnformeerd over updates voor de gewenste eigenschappen wanneer ze zich voordoen. De module ontvangt de dezelfde vorm van update (gedeeltelijke of volledige vervanging) dat door de back-end van de oplossing wordt uitgevoerd.

De voorgaande behoeft de **ModuleConnect** machtiging, zoals gedefinieerd in de [Security] [ lnk-security] artikel.

De [Azure IoT device SDK's] [ lnk-sdks] gemakkelijk te gebruiken van de voorgaande bewerkingen van veel talen en platforms.

## <a name="tags-and-properties-format"></a>Indeling van tags en eigenschappen
Labels en gewenste eigenschappen, gerapporteerde eigenschappen zijn JSON-objecten met de volgende beperkingen:

* Alle sleutels in JSON-objecten zijn hoofdlettergevoelig 64 bytes UNICODE UTF-8-tekenreeksen. Toegestane tekens uitsluiten Unicode-tekens (segmenten C0 en C1), en `'.'`, `' '`, en `'$'`.
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

* Alle tekenreekswaarden mag maximaal 4 KB lang.

## <a name="module-twin-size"></a>Omvang van integratiemodule twin
IoT Hub dwingt een beperking van de grootte van 8KB op elk van de respectieve totale waarden van `tags`, `properties/desired`, en `properties/reported`, met uitzondering van alleen-lezen-elementen.
De grootte wordt berekend door het tellen van alle tekens, met uitzondering van Unicode-tekens (segmenten C0 en C1) en die zich buiten de tekenreeksconstanten.
IoT Hub is geweigerd door een fout. alle bewerkingen die de grootte van deze documenten boven de limiet wilt verhogen.

## <a name="module-twin-metadata"></a>Module dubbele metagegevens
IoT Hub onderhoudt de tijdstempel van de laatste update voor elk JSON-object in de moduledubbel gewenst en gerapporteerde eigenschappen. De tijdstempels in UTC en gecodeerd in de [ISO8601] indeling `YYYY-MM-DDTHH:MM:SS.mmmZ`.
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
Tags zijn een ETag volgens [RFC7232], die de JSON-weergave van de tag vertegenwoordigt. U kunt ETags in voorwaardelijke bewerkingen van de back-end oplossing gebruiken om te zorgen voor consistentie.

Moduledubbel gewenst en gerapporteerde eigenschappen geen ETags, maar hebben een `$version` waarde die kan worden gegarandeerd incrementeel zijn. Op dezelfde manier naar een ETag, kan de versie worden gebruikt door de partij bijwerken voor het afdwingen van consistentie van updates. Bijvoorbeeld, een module-app voor een gerapporteerde eigenschap of de back-end oplossing voor een gewenste eigenschap.

Versies zijn ook handig wanneer een observing agent (zoals de module app inachtneming van de gewenste eigenschappen) op elkaar stappen tussen het resultaat van een bewerking ophalen en een melding van updates afstemmen moet. De sectie [apparaat opnieuw verbinden met flow] [lnk netsplitsing] vindt u meer informatie. 

## <a name="next-steps"></a>Volgende stappen
Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, Zie de volgende zelfstudies voor IoT Hub:

* [Aan de slag met IoT Hub identiteits- en module moduledubbel met behulp van .NET-back-end en .NET-apparaat][lnk-module-twin-tutorial]

<!-- links and images -->

[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md
[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232

[lnk-module-twin-tutorial]: iot-hub-csharp-csharp-module-twin-getstarted.md
[lnk-module-twin-metadata]: iot-hub-devguide-module-twins.md#module-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[img-module-twin]: media/iot-hub-devguide-device-twins/module-twin.jpg
