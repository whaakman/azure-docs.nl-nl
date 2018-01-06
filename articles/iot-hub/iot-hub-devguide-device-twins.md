---
title: Azure IoT Hub apparaat horende begrijpen | Microsoft Docs
description: Handleiding voor ontwikkelaars - gebruik apparaat horende status en configuratie van gegevens tussen IoT Hub en uw apparaten synchroniseren
services: iot-hub
documentationcenter: .net
author: fsautomata
manager: timlt
editor: 
ms.assetid: 8a3da072-a5bf-46e5-8de4-24cdbb2a03fa
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/19/2017
ms.author: elioda
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3b2b2877efe5f898b5759c03ac0ddcf3ecc03901
ms.sourcegitcommit: 1d423a8954731b0f318240f2fa0262934ff04bd9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/05/2018
---
# <a name="understand-and-use-device-twins-in-iot-hub"></a>Begrijpen en gebruiken van apparaat horende in IoT-Hub

*Apparaat horende* zijn JSON-documenten die status apparaatgegevens, met inbegrip van metagegevens, configuraties en voorwaarden opslaan. Azure IoT Hub onderhoudt een apparaat twin voor elk apparaat dat u verbinding met IoT Hub maakt. Dit artikel wordt beschreven:


* De structuur van het apparaat twin: *labels*, *gewenste* en *eigenschappen gerapporteerd*.
* De bewerkingen die apparaat-apps en back-ends op het apparaat horende kunnen uitvoeren.

Apparaat horende te gebruiken:

* Apparaatspecifieke metagegevens niet opslaan in de cloud. Bijvoorbeeld, de implementatielocatie van een snoep-machine.
* Huidige status rapportgegevens zoals beschikbaar mogelijkheden en de voorwaarden van de app op uw apparaat. Bijvoorbeeld, een apparaat is verbonden met uw IoT-hub over mobiel of Wi-Fi.
* De status van langlopende werkstromen tussen apparaattoepassing en back-endserver voor apps worden gesynchroniseerd. Wanneer de oplossing voor back-end geeft u de nieuwe firmwareversie voor het installeren en de apparaattoepassing rapporteert de verschillende stadia van het updateproces.
* De metagegevens van apparaten, de configuratie of de status opvragen.

Raadpleeg [apparaat-naar-cloud communicatie richtlijnen] [ lnk-d2c-guidance] voor instructies over het gebruik van de gerapporteerde eigenschappen, apparaat-naar-cloud-berichten of bestand uploaden.
Raadpleeg [Cloud-naar-apparaat communicatie richtlijnen] [ lnk-c2d-guidance] voor hulp bij het gebruik van eigenschappen van de gewenste rechtstreekse methoden of cloud-naar-apparaat-berichten.

## <a name="device-twins"></a>Apparaat horende
Apparaat horende apparaatgerelateerde gegevens opslaan die:

* Apparaat- en back-ends kunnen gebruiken om apparaat-voorwaarden en configuratie te synchroniseren.
* De back-end oplossing kunt gebruiken voor query- en doel langlopende bewerkingen.

De levenscyclus van een apparaat-twin is gekoppeld aan de bijbehorende [apparaat-id][lnk-identity]. Apparaat horende worden impliciet gemaakt en verwijderd wanneer een apparaat-id is gemaakt of verwijderd uit IoT Hub.

Een apparaat-twin is een JSON-document met:

* **Labels**. Een gedeelte van de JSON-document dat de back-end oplossing kunt lezen en schrijven naar. Labels zijn niet zichtbaar voor apparaat-apps.
* **Eigenschappen van de gewenste**. Gebruikt samen met de eigenschappen van de gerapporteerde synchroniseren apparaatconfiguratie of voorwaarden. De back-end oplossing kunt gewenste eigenschappen instellen en ze kan worden gelezen door de app voor het apparaat. De apparaat-app kan ook ontvangen van meldingen van wijzigingen in de gewenste eigenschappen.
* **Eigenschappen gerapporteerd**. Gebruikt samen met de gewenste eigenschappen voor het synchroniseren van apparaatconfiguratie of voorwaarden. De app apparaat gemelde eigenschappen kunt instellen en de back-end oplossing kan lezen en hierin zoeken.
* **Identiteitseigenschappen van apparaat**. De hoofdmap van het apparaat twin JSON-document bevat de eigenschappen voor alleen-lezen van de bijbehorende apparaat-id opgeslagen in de [identiteitsregister][lnk-identity].

![][img-twin]

Het volgende voorbeeld ziet u een apparaat twin JSON-document:

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
                    }
                    "batteryLevel": 55,
                    "$metadata" : {...},
                    "$version": 4
                }
            }
        }

In het hoofdobject het apparaat identiteitseigenschappen en containerobjecten voor `tags` en beide `reported` en `desired` eigenschappen. De `properties` container bevat sommige alleen-lezen-elementen (`$metadata`, `$etag`, en `$version`) dat wordt beschreven in de [twin Apparaatmetagegevens] [ lnk-twin-metadata] en [ Optimistische gelijktijdigheid] [ lnk-concurrency] secties.

### <a name="reported-property-example"></a>Voorbeeld van de gerapporteerde eigenschap
In het vorige voorbeeld de apparaat-twin bevat een `batteryLevel` eigenschap die is gerapporteerd door de app voor het apparaat. Deze eigenschap kan doorzoeken en op apparaten die zijn gebaseerd op het niveau van de laatste gemelde batterij werkt. Andere voorbeelden zijn de apparaat-app apparaat rapportagemogelijkheden of opties voor netwerkconnectiviteit.

> [!NOTE]
> Eigenschappen van de gerapporteerde vereenvoudigen scenario's waarin de back-end van de oplossing is geïnteresseerd in de laatst bekende waarde van een eigenschap. Gebruik [apparaat-naar-cloudberichten] [ lnk-d2c] als de back-end van de oplossing moet verwerken van telemetriegegevens in de vorm van reeksen voorzien van een tijdstempel gebeurtenissen, zoals een tijdreeks.

### <a name="desired-property-example"></a>Voorbeeld van de gewenste eigenschap
In het vorige voorbeeld de `telemetryConfig` apparaat twin gewenst en gemelde eigenschappen worden gebruikt door de back-end van de oplossing en de apparaat-app voor het synchroniseren van de telemetrie-configuratie voor dit apparaat. Bijvoorbeeld:

1. De back-end oplossing stelt de gewenste eigenschap met de waarde van de gewenste configuratie. Dit is het gedeelte van het document met de gewenste eigenschap is ingesteld:
   
        ...
        "desired": {
            "telemetryConfig": {
                "sendFrequency": "5m"
            },
            ...
        },
        ...
2. De apparaat-app ontvangt een melding van de wijziging onmiddellijk als verbonden of op de eerste opnieuw verbinding te maken. De apparaat-app vervolgens verslag uitbrengt de bijgewerkte configuratie (of een fout voorwaarde met behulp van de `status` eigenschap). Dit is het gedeelte van de gerapporteerde eigenschappen:
   
        ...
        "reported": {
            "telemetryConfig": {
                "sendFrequency": "5m",
                "status": "success"
            }
            ...
        }
        ...
3. De back-end oplossing kunt volgen de resultaten van de configuratie-bewerking op veel apparaten door [opvragen] [ lnk-query] horende apparaten.

> [!NOTE]
> De voorgaande codefragmenten zijn voorbeelden, geoptimaliseerd voor de leesbaarheid van een manier voor het coderen van de configuratie van een apparaat en de status ervan. IoT Hub legt een specifieke schema voor de apparaat-twin gewenst en eigenschappen in de horende apparaten gerapporteerd.
> 
> 

U kunt horende langlopende bewerkingen zoals firmware-updates te synchroniseren. Zie voor meer informatie over het gebruik van eigenschappen om te synchroniseren en een langdurige bewerking bijhouden op apparaten [gebruik gewenst eigenschappen voor het configureren van apparaten][lnk-twin-properties].

## <a name="back-end-operations"></a>Back-end-bewerkingen
De back-end van de oplossing is van invloed op het apparaat twin met behulp van de volgende atomische bewerkingen, die toegankelijk is via HTTPS:

* **Apparaat-twin ophalen met id**. Deze bewerking wordt het apparaat twin document, inclusief tags en eigenschappen van het gewenste en gerapporteerd.
* **Gedeeltelijk bijwerken apparaat twin**. Deze bewerking kunt de back-end oplossing gedeeltelijk de codes of de gewenste eigenschappen in een twin apparaat bijwerken. De gedeeltelijke update wordt uitgedrukt als een JSON-document dat wordt toegevoegd of updates van een eigenschap. Eigenschappen ingesteld op `null` worden verwijderd. Het volgende voorbeeld wordt een nieuwe gewenste eigenschap met de waarde `{"newProperty": "newValue"}`, overschrijft de bestaande waarde van `existingProperty` met `"otherNewValue"`, en verwijdert u `otherOldProperty`. Er zijn geen andere wijzigingen zijn aangebracht aan de bestaande gewenste eigenschappen of labels:
   
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
* **Gewenste eigenschappen vervangen**. Deze bewerking kunt u de back-end oplossing voor het volledig overschrijven alle bestaande gewenste eigenschappen en vervangen door een nieuwe JSON-document voor `properties/desired`.
* **Vervang labels**. Deze bewerking kunt u de back-end oplossing voor het volledig overschrijven alle bestaande tags en vervangen door een nieuwe JSON-document voor `tags`.
* **Dubbele meldingen ontvangen**. Deze bewerking kunt de back-end oplossing wilt worden gewaarschuwd als de twin is gewijzigd. Om dit te doen, moet uw IoT-oplossing het maken van een route en stelt u de gegevensbron op *twinChangeEvents*. Standaard geen twin meldingen worden verzonden, dat wil zeggen, geen dergelijke routes vooraf bestaan. Als de wijzigingssnelheid te hoog is, of om andere redenen als interne fouten, kan de IoT-Hub slechts één melding waarin alle wijzigingen verzenden. Dus als uw toepassing moet betrouwbare voor controle en logboekregistratie van alle tussenliggende statussen, nog steeds aanbevolen wordt dat u D2C berichten. Het meldingsbericht twin bevat eigenschappen en hoofdtekst.

    - Eigenschappen

    | Naam | Waarde |
    | --- | --- |
    $content-type | application/json |
    $iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
    $iothub-bericht-bron | twinChangeEvents |
    $content-codering | UTF-8 |
    deviceId | ID van het apparaat |
    hubName | Naam van de IoT-Hub |
    operationTimestamp | [ISO8601] tijdstempel van bewerking |
    bericht-iothub-schema | deviceLifecycleNotification |
    opType | 'replaceTwin' of 'updateTwin' |

    Bericht Systeemeigenschappen worden voorafgegaan door de `'$'` symbool.

    - Hoofdtekst
        
    Deze sectie bevat alle twin wijzigingen in een JSON-indeling. Met het verschil dat deze alle twin secties kan bevatten, wordt dezelfde indeling als een patch: labels, properties.reported properties.desired en dat deze de elementen '$metadata bevat'. Bijvoorbeeld:
    ```
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

* De apparaat-horende met behulp van de SQL-achtige query [IoT Hub-querytaal][lnk-query].
* Bewerkingen uitvoeren op grote gegevenssets apparaat horende met [taken][lnk-jobs].

## <a name="device-operations"></a>Apparaatbewerkingen
De app voor het apparaat is van invloed op het apparaat twin met behulp van de volgende atomische bewerkingen:

* **Ophalen van apparaat twin**. Deze bewerking wordt het apparaat twin-document (inclusief tags en gewenste en gerapporteerde Systeemeigenschappen) voor de momenteel verbonden apparaat.
* **De eigenschappen van de gerapporteerde gedeeltelijk bijwerken**. Deze bewerking kunt het gedeeltelijke bijwerken van de gerapporteerde eigenschappen van de momenteel verbonden apparaat. Deze bewerking wordt de dezelfde JSON-update-indeling dat de oplossing voor back-end gebruikt voor een gedeeltelijke update van de gewenste eigenschappen.
* **Houd rekening met de eigenschappen van de gewenste**. De momenteel verbonden apparaat kunt van updates voor de gewenste eigenschappen informeren wanneer ze zich voordoen. Het apparaat ontvangt de dezelfde vorm van update (gedeeltelijke of volledige vervanging) uitgevoerd door de back-end oplossing.

De voorgaande bewerkingen vereist de **DeviceConnect** toestemming hebben, zoals gedefinieerd in de [beveiliging] [ lnk-security] artikel.

De [apparaat Azure IoT SDK's] [ lnk-sdks] gemakkelijk te gebruiken van de voorgaande bewerkingen uit veel talen en platforms. Zie voor meer informatie over de details van IoT Hub primitieven voor synchronisatie van de gewenste eigenschappen [apparaat opnieuw verbinden stroom][lnk-reconnection].

## <a name="tags-and-properties-format"></a>Labels en eigenschappen indeling
Labels, gewenste eigenschappen en gerapporteerde eigenschappen zijn JSON-objecten met de volgende beperkingen:

* Alle sleutels in JSON-objecten zijn hoofdlettergevoelig 64 bytes UTF-8, UNICODE-tekenreeksen. Toegestane tekens Unicode-tekens (segmenten C0 en C1), uitsluiten en `'.'`, `' '`, en `'$'`.
* Alle waarden in de JSON-objecten van de volgende JSON-typen kunnen zijn: boolean, getal, string, object. Matrices zijn niet toegestaan. De maximale waarde voor de gehele getallen 4503599627370495 is en de minimale waarde voor gehele getallen-4503599627370496 is.
* Alle JSON-objecten in tags, gewenste en gerapporteerde eigenschappen kunnen een maximale diepte van 5 hebben. De volgende object is bijvoorbeeld geldig:

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

* Alle tekenreekswaarden mag maximaal 4 KB lang.

## <a name="device-twin-size"></a>De grootte van de apparaat-twin
IoT Hub worden afgedwongen voor een beperking van de grootte van 8KB op elk van de respectieve totale waarden van `tags`, `properties/desired`, en `properties/reported`, met uitzondering van alleen-lezen-elementen.
De grootte wordt berekend door de telling van alle tekens, met uitzondering van Unicode-tekens (segmenten C0 en C1) en spaties bevatten die niet tekenreeksconstanten zijn.
IoT Hub worden alle bewerkingen die u wilt de grootte van deze documenten boven de limiet verhogen geweigerd met een fout.

## <a name="device-twin-metadata"></a>Metagegevens van apparaten twin
IoT Hub onderhoudt het tijdstempel van de laatste update voor elk JSON-object in de apparaat-twin gewenst en eigenschappen gerapporteerd. De tijdstempels in UTC en gecodeerd in de [ISO8601] indeling `YYYY-MM-DDTHH:MM:SS.mmmZ`.
Bijvoorbeeld:

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

Deze informatie wordt opgeslagen op elk niveau (niet alleen de knooppunten van de JSON-structuur)-updates die objectsleutels verwijderen moet worden bewaard.

## <a name="optimistic-concurrency"></a>Optimistische gelijktijdigheid
Labels, gewenst en eigenschappen van alle ondersteuning optimistische gelijktijdigheid gerapporteerd.
Labels hebben een ETag conform [RFC7232], die staat voor de JSON-weergave van de tag. U kunt ETags in voorwaardelijke bijwerkbewerkingen van de back-end oplossing gebruiken om consistentie te garanderen.

Apparaat twin gewenst en gerapporteerde eigenschappen zijn niet ETags, maar hebben een `$version` is gegarandeerd incrementele waarde. Op dezelfde manier naar een ETag, de versie kan worden gebruikt door de partij bijwerken om af te dwingen van de consistentie van updates. Bijvoorbeeld, een apparaat-app voor een eigenschap gemeld of de back-end oplossing voor een gewenste eigenschap.

Versies zijn ook handig wanneer een observing agent (zoals de apparaat-app de gewenste eigenschappen observeren) op elkaar races tussen het resultaat van een bewerking ophalen en een melding van updates afstemmen moet. De sectie [apparaat opnieuw verbinden stroom] [ lnk-reconnection] vindt u meer informatie.

## <a name="device-reconnection-flow"></a>Opnieuw verbinden stroom van apparaat
IoT Hub behoudt updatemeldingen gewenste eigenschappen voor niet-verbonden apparaten niet. Betekent dit dat het document volledige gewenste eigenschappen naast abonneren op updatemeldingen moet worden opgehaald door een apparaat dat verbinding maakt. Gezien de mogelijkheid van races tussen updatemeldingen en volledige ophalen, ervoor de volgende stroom wordt gezorgd

1. App voor het apparaat verbindt met een IoT-hub.
2. App voor het apparaat is lid voor de gewenste eigenschappen updatemeldingen.
3. Apparaattoepassing haalt het volledige document van de gewenste eigenschappen.

De apparaat-app kunt negeren alle meldingen met `$version` of minder zijn dan de versie van het volledige document opgehaald. Deze aanpak is mogelijk omdat IoT Hub wordt gegarandeerd dat versies altijd verhogen.

> [!NOTE]
> Deze logica wordt al geïmplementeerd de [apparaat Azure IoT SDK's][lnk-sdks]. Deze beschrijving is handig als de app voor het apparaat een apparaat met Azure IoT SDK's gebruiken kan en de protocollen MQTT-interface moet rechtstreeks programma.
> 
> 

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal
Er zijn andere onderwerpen waarnaar wordt verwezen in de IoT Hub developer guide:

* De [IoT-hubeindpunten] [ lnk-endpoints] artikel beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.
* De [bandbreedtebeperking en quota's] [ lnk-quotas] artikel beschrijft de quota die betrekking hebben op de IoT Hub-service en het gedrag bandbreedteregeling kunt verwachten wanneer u de service gebruiken.
* De [apparaat en de service Azure IoT-SDK's] [ lnk-sdks] artikel bevat een overzicht van de verschillende SDK's kunt u bij het ontwikkelen van apps voor het apparaat en de service die communiceren met IoT Hub-taal.
* De [querytaal IoT Hub voor apparaat horende, taken en berichtroutering] [ lnk-query] artikel beschrijft de IoT Hub-querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat horende en taken.
* De [IoT Hub MQTT ondersteuning] [ lnk-devguide-mqtt] artikel vindt u meer informatie over ondersteuning voor het protocol MQTT IoT Hub.

## <a name="next-steps"></a>Volgende stappen
Nu hebt u geleerd over horende apparaat, hebt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Een directe methode aangeroepen voor een apparaat][lnk-methods]
* [Taken plannen op meerdere apparaten][lnk-jobs]

Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, kunt u mogelijk geïnteresseerd in de volgende zelfstudies met IoT Hub:

* [Het gebruik van de apparaat-twin][lnk-twin-tutorial]
* [Het gebruik van twin apparaateigenschappen][lnk-twin-properties]

<!-- links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-jobs]: iot-hub-devguide-jobs.md
[lnk-identity]: iot-hub-devguide-identity-registry.md
[lnk-d2c]: iot-hub-devguide-messages-d2c.md
[lnk-methods]: iot-hub-devguide-direct-methods.md
[lnk-security]: iot-hub-devguide-security.md
[lnk-c2d-guidance]: iot-hub-devguide-c2d-guidance.md
[lnk-d2c-guidance]: iot-hub-devguide-d2c-guidance.md

[ISO8601]: https://en.wikipedia.org/wiki/ISO_8601
[RFC7232]: https://tools.ietf.org/html/rfc7232
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md

[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-twin-tutorial]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-properties]: iot-hub-node-node-twin-how-to-configure.md
[lnk-twin-metadata]: iot-hub-devguide-device-twins.md#device-twin-metadata
[lnk-concurrency]: iot-hub-devguide-device-twins.md#optimistic-concurrency
[lnk-reconnection]: iot-hub-devguide-device-twins.md#device-reconnection-flow

[img-twin]: media/iot-hub-devguide-device-twins/twin.png
