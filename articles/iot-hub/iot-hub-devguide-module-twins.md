---
title: Azure IoT Hub module horende begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - gebruik module horende status en configuratie van gegevens tussen IoT Hub en uw apparaten synchroniseren
author: chrissie926
manager: ''
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 04/26/2018
ms.author: menchi
ms.openlocfilehash: 71d762b6f1c199db17058ac107aad7a0b3260ae7
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34633492"
---
# <a name="understand-and-use-module-twins-in-iot-hub"></a>Begrijpen en gebruiken van module horende in IoT-Hub

In dit artikel wordt ervan uitgegaan dat u hebt gelezen [begrijpen en gebruiken van apparaat horende in IoT-Hub] [ lnk-devguide-device-twins] eerste. Onder per apparaat-id in IoT Hub, kunt u maximaal 20 module identiteiten maken. De identiteit van elke module genereert impliciet een twin module. Lijkt veel op het apparaat horende, module horende zijn JSON-documenten die module informatie over metagegevens, configuraties en voorwaarden inclusief opslaat. Azure IoT Hub onderhoudt een module-twin voor elke module die u verbinding met IoT Hub maakt. 

Het apparaat met IoT Hub SDK's inschakelen aan de kant apparaat voor het maken van de modules die elk een onafhankelijke verbinding met IoT Hub wordt geopend. Hiermee kunt u afzonderlijke naamruimten gebruiken voor andere onderdelen op uw apparaat. U hebt bijvoorbeeld een snoep-machine met drie verschillende sensoren. Elke sensor wordt beheerd door verschillende afdelingen in uw bedrijf. U kunt een module voor elke sensor maken. Op deze manier elke afdeling kan alleen taken of directe methoden verzenden naar de sensor die Hiermee worden beheerd, voorkomen van conflicten en gebruikersfouten.

 Module identiteits- en module twin biedt dezelfde mogelijkheden als apparaat-id en het apparaat twin maar fijner samenvattingen. Deze weer specifieker kan apparaten, zoals besturingssysteem op basis van apparaten of firmware-apparaten beheren van meerdere onderdelen, als u wilt isoleren, configuratie en voorwaarden voor elk van deze onderdelen. Identiteit van de module en module horende bieden een scheiding van problemen bij het werken met IoT-apparaten waarvoor modulaire software-onderdelen. We gericht op de ondersteuning van alle apparaat twin functionaliteit op module twin niveau door de module twin algemene beschikbaarheid. 

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-whole.md)]

Dit artikel wordt beschreven:

* De structuur van de module-twin: *labels*, *gewenste* en *eigenschappen gerapporteerd*.
* De bewerkingen die de modules en back-ends voor module horende kunnen uitvoeren.

Raadpleeg [apparaat-naar-cloud communicatie richtlijnen] [ lnk-d2c-guidance] voor instructies over het gebruik van de gerapporteerde eigenschappen, apparaat-naar-cloud-berichten of bestand uploaden.
Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen] [ lnk-c2d-guidance] voor hulp bij het gebruik van eigenschappen van de gewenste rechtstreekse methoden of cloud-naar-apparaat-berichten.

## <a name="module-twins"></a>Module horende
Module horende module-gerelateerde informatie opslaan die:

* Modules op het apparaat en IoT-Hub kunnen gebruiken voor het synchroniseren van de voorwaarden van de module en configuratie.
* De back-end oplossing kunt gebruiken voor query- en doel langlopende bewerkingen.

De levenscyclus van een module-twin is gekoppeld aan de bijbehorende [module identiteit][lnk-identity]. Modules horende worden impliciet gemaakt en verwijderd wanneer de identiteit van een module is gemaakt of verwijderd uit IoT Hub.

Een module-twin is een JSON-document met:

* **Labels**. Een gedeelte van de JSON-document dat de back-end oplossing kunt lezen en schrijven naar. Labels zijn niet zichtbaar voor modules op het apparaat. Labels zijn ingesteld voor het uitvoeren van query's doel.
* **Eigenschappen van de gewenste**. Gebruikt samen met gerapporteerde eigenschappen voor het synchroniseren van moduleconfiguratie of voorwaarden. De back-end oplossing gewenste eigenschappen kunt instellen en ze kan worden gelezen door de module-app. De module-app kan ook ontvangen van meldingen van wijzigingen in de gewenste eigenschappen.
* **Eigenschappen gerapporteerd**. Gebruikt samen met de gewenste eigenschappen voor het synchroniseren van moduleconfiguratie of voorwaarden. De module app gerapporteerde eigenschappen kunt instellen, en de back-end oplossing kan lezen en hierin zoeken.
* **Identiteitseigenschappen van de module**. De hoofdmap van de module twin JSON-document bevat de eigenschappen voor alleen-lezen van de identiteit van de bijbehorende module opgeslagen in de [identiteitsregister][lnk-identity].

![][img-module-twin]

Het volgende voorbeeld ziet u een module-twin JSON-document:

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

In het hoofdobject de module identiteitseigenschappen en containerobjecten voor `tags` en beide `reported` en `desired` eigenschappen. De `properties` container bevat sommige alleen-lezen-elementen (`$metadata`, `$etag`, en `$version`) dat wordt beschreven in de [Module twin metagegevens] [ lnk-module-twin-metadata] en [ Optimistische gelijktijdigheid] [ lnk-concurrency] secties.

### <a name="reported-property-example"></a>Voorbeeld van de gerapporteerde eigenschap
In het vorige voorbeeld wordt de module-twin bevat een `batteryLevel` eigenschap die is gerapporteerd door de module-app. Deze eigenschap kan doorzoeken en modules op basis van het laatste gemelde niveau niet bewerken. Andere voorbeelden zijn de module app module rapportagemogelijkheden of opties voor netwerkconnectiviteit.

> [!NOTE]
> Eigenschappen van de gerapporteerde vereenvoudigen scenario's waarin de back-end van de oplossing is geïnteresseerd in de laatst bekende waarde van een eigenschap. Gebruik [apparaat-naar-cloudberichten] [ lnk-d2c] als de back-end van de oplossing moet verwerken module telemetrie in de vorm van reeksen voorzien van een tijdstempel gebeurtenissen, zoals een tijdreeks.

### <a name="desired-property-example"></a>Voorbeeld van de gewenste eigenschap
In het vorige voorbeeld de `telemetryConfig` module twin gewenst en gerapporteerde eigenschappen worden gebruikt door de back-end van de oplossing en de module-app voor het synchroniseren van de configuratie van de telemetrie voor deze module. Bijvoorbeeld:

1. De back-end oplossing stelt de gewenste eigenschap met de waarde van de gewenste configuratie. Dit is het gedeelte van het document met de gewenste eigenschap is ingesteld:

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

2. De module app ontvangt een melding van de wijziging onmiddellijk als verbonden of op de eerste opnieuw verbinding te maken. De bijgewerkte configuratie in de module-app vervolgens verslag uitbrengt (of een fout voorwaarde met behulp van de `status` eigenschap). Dit is het gedeelte van de gerapporteerde eigenschappen:

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

3. De back-end oplossing kunt houden de resultaten van de bewerking configuratie over veel modules door [opvragen] [ lnk-query] module horende.

> [!NOTE]
> De voorgaande codefragmenten zijn voorbeelden, geoptimaliseerd voor de leesbaarheid van een manier voor het coderen van de moduleconfiguratie van een en de status ervan. IoT Hub legt een specifieke schema voor de module-twin gewenst en eigenschappen in de module horende gerapporteerd.
> 
> 

## <a name="back-end-operations"></a>Back-end-bewerkingen
De back-end van de oplossing is van invloed op de module-twin met behulp van de volgende atomische bewerkingen, die toegankelijk is via HTTPS:

* **Module-twin door-ID ophalen**. Deze bewerking wordt de module twin document, inclusief tags en eigenschappen van het gewenste en gerapporteerd.
* **Gedeeltelijk bijwerken module twin**. Deze bewerking kunt de back-end oplossing de codes of de gewenste eigenschappen in een module-twin gedeeltelijk bijwerken. De gedeeltelijke update wordt uitgedrukt als een JSON-document dat wordt toegevoegd of updates van een eigenschap. Eigenschappen ingesteld op `null` worden verwijderd. Het volgende voorbeeld wordt een nieuwe gewenste eigenschap met de waarde `{"newProperty": "newValue"}`, overschrijft de bestaande waarde van `existingProperty` met `"otherNewValue"`, en verwijdert u `otherOldProperty`. Er zijn geen andere wijzigingen zijn aangebracht aan de bestaande gewenste eigenschappen of labels:

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

* **Gewenste eigenschappen vervangen**. Deze bewerking kunt u de back-end oplossing voor het volledig overschrijven alle bestaande gewenste eigenschappen en vervangen door een nieuwe JSON-document voor `properties/desired`.
* **Vervang labels**. Deze bewerking kunt u de back-end oplossing voor het volledig overschrijven alle bestaande tags en vervangen door een nieuwe JSON-document voor `tags`.
* **Dubbele meldingen ontvangen**. Deze bewerking kunt de back-end oplossing wilt worden gewaarschuwd als de twin is gewijzigd. Om dit te doen, moet uw IoT-oplossing het maken van een route en stelt u de gegevensbron op *twinChangeEvents*. Standaard geen twin meldingen worden verzonden, dat wil zeggen, geen dergelijke routes vooraf bestaan. Als de wijzigingssnelheid te hoog is, of om andere redenen als interne fouten, kan de IoT-Hub slechts één melding waarin alle wijzigingen verzenden. Als uw toepassing moet betrouwbare voor controle en logboekregistratie van alle tussenliggende statussen, moet u apparaat-naar-cloud-berichten. Het meldingsbericht twin bevat eigenschappen en hoofdtekst.

    - Eigenschappen

    | Naam | Waarde |
    | --- | --- |
    $content-type | application/json |
    $iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
    $iothub-bericht-bron | twinChangeEvents |
    $content-codering | utf-8 |
    deviceId | ID van het apparaat |
    moduleId | ID van de module |
    hubName | Naam van de IoT-Hub |
    operationTimestamp | [ISO8601] tijdstempel van bewerking |
    bericht-iothub-schema | deviceLifecycleNotification |
    opType | 'replaceTwin' of 'updateTwin' |

    Bericht Systeemeigenschappen worden voorafgegaan door de `'$'` symbool.

    - Hoofdtekst
        
    Deze sectie bevat alle twin wijzigingen in een JSON-indeling. Met het verschil dat deze alle twin secties kan bevatten, wordt dezelfde indeling als een patch: labels, properties.reported properties.desired en dat deze de elementen '$metadata bevat'. Bijvoorbeeld:

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

Ondersteuning voor alle voorgaande bewerkingen [optimistische gelijktijdigheid] [ lnk-concurrency] en vereisen de **ServiceConnect** toestemming hebben, zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

Naast deze bewerkingen kan de back-end oplossing:

* De module horende met behulp van de SQL-achtige query [IoT Hub-querytaal][lnk-query].

## <a name="module-operations"></a>Modulebewerkingen
De module-app is van invloed op de module-twin met behulp van de volgende atomische bewerkingen:

* **Ophalen van de module twin**. Deze bewerking wordt het module-twin-document (inclusief tags en gewenste en gerapporteerde Systeemeigenschappen) voor de momenteel verbonden module.
* **De eigenschappen van de gerapporteerde gedeeltelijk bijwerken**. Deze bewerking kunt de gedeeltelijke update van de gerapporteerde eigenschappen van de momenteel verbonden module. Deze bewerking wordt de dezelfde JSON-update-indeling dat de oplossing voor back-end gebruikt voor een gedeeltelijke update van de gewenste eigenschappen.
* **Houd rekening met de eigenschappen van de gewenste**. De momenteel verbonden module kunt van updates voor de gewenste eigenschappen informeren wanneer ze zich voordoen. De module ontvangt dezelfde vorm van update (gedeeltelijke of volledige vervanging) uitgevoerd door de back-end oplossing.

De voorgaande bewerkingen vereist de **ModuleConnect** toestemming hebben, zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

De [apparaat Azure IoT SDK's] [ lnk-sdks] gemakkelijk te gebruiken van de voorgaande bewerkingen uit veel talen en platforms.

## <a name="tags-and-properties-format"></a>Labels en eigenschappen indeling
Labels, gewenste eigenschappen en gerapporteerde eigenschappen zijn JSON-objecten met de volgende beperkingen:

* Alle sleutels in JSON-objecten zijn hoofdlettergevoelig 64 bytes UTF-8, UNICODE-tekenreeksen. Toegestane tekens Unicode-tekens (segmenten C0 en C1), uitsluiten en `'.'`, `' '`, en `'$'`.
* Alle waarden in de JSON-objecten van de volgende JSON-typen kunnen zijn: boolean, getal, string, object. Matrices zijn niet toegestaan. De maximale waarde voor de gehele getallen 4503599627370495 is en de minimale waarde voor gehele getallen-4503599627370496 is.
* Alle JSON-objecten in tags, gewenste en gerapporteerde eigenschappen kunnen een maximale diepte van 5 hebben. De volgende object is bijvoorbeeld geldig:

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

## <a name="module-twin-size"></a>Grootte van de module-twin
IoT Hub worden afgedwongen voor een beperking van de grootte van 8KB op elk van de respectieve totale waarden van `tags`, `properties/desired`, en `properties/reported`, met uitzondering van alleen-lezen-elementen.
De grootte wordt berekend door de telling van alle tekens, met uitzondering van Unicode-tekens (segmenten C0 en C1) en spaties bevatten die niet tekenreeksconstanten zijn.
IoT Hub worden alle bewerkingen die u wilt de grootte van deze documenten boven de limiet verhogen geweigerd met een fout.

## <a name="module-twin-metadata"></a>Module twin metagegevens
IoT Hub onderhoudt het tijdstempel van de laatste update voor elk JSON-object in de module twin gewenst en eigenschappen gerapporteerd. De tijdstempels in UTC en gecodeerd in de [ISO8601] indeling `YYYY-MM-DDTHH:MM:SS.mmmZ`.
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

Deze informatie wordt opgeslagen op elk niveau (niet alleen de knooppunten van de JSON-structuur)-updates die objectsleutels verwijderen moet worden bewaard.

## <a name="optimistic-concurrency"></a>Optimistische gelijktijdige uitvoering
Labels, gewenst en eigenschappen van alle ondersteuning optimistische gelijktijdigheid gerapporteerd.
Labels hebben een ETag conform [RFC7232], die staat voor de JSON-weergave van de tag. U kunt ETags in voorwaardelijke bijwerkbewerkingen van de back-end oplossing gebruiken om consistentie te garanderen.

Module twin gewenst en gerapporteerde eigenschappen zijn niet ETags, maar hebben een `$version` is gegarandeerd incrementele waarde. Op dezelfde manier naar een ETag, de versie kan worden gebruikt door de partij bijwerken om af te dwingen van de consistentie van updates. Bijvoorbeeld, een module-app voor een eigenschap gemeld of de back-end oplossing voor een gewenste eigenschap.

Versies zijn ook handig wanneer een observing agent (zoals de module app de gewenste eigenschappen observeren) op elkaar races tussen het resultaat van een bewerking ophalen en een melding van updates afstemmen moet. De sectie [apparaat opnieuw verbinden stroom] [lnk opnieuw verbinden] vindt u meer informatie. 

## <a name="next-steps"></a>Volgende stappen
Als u wilt uitproberen enkele concepten die worden beschreven in dit artikel, Zie de volgende zelfstudies met IoT Hub:

* [Aan de slag met IoT Hub module identiteits- en module twin met .NET back-up en .NET-apparaat][lnk-module-twin-tutorial]

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
