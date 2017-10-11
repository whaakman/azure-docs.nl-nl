---
title: Inzicht in het identiteitenregister van Azure IoT Hub | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de id-register van IoT Hub en het gebruik ervan in uw apparaten kunt beheren. Bevat informatie over het importeren en exporteren van apparaat-id's in bulk.
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0706eccd-e84c-4ae7-bbd4-2b1a22241147
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b6e9c7b71fa6fc78f97c0144c735fc44778181d8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/18/2017
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a>Inzicht in het identiteitenregister van uw IoT-hub

Elke iothub heeft een register-id's die worden opgeslagen informatie over de apparaten die verbinding mogen maken met de IoT-hub. Voordat een apparaat verbinding naar een iothub maken kan, moet er een vermelding voor het apparaat in het identiteitenregister van de IoT-hub. Een apparaat moet ook worden geverifieerd met de IoT-hub die op basis van de referenties die zijn opgeslagen in het identiteitsregister.

De apparaat-ID die is opgeslagen in het identiteitenregister is hoofdlettergevoelig.

Op een hoog niveau is het identiteitsregister een REST-compatibele verzameling apparaatbronnen identiteit. Wanneer u een vermelding in het identiteitenregister toevoegt, maakt de IoT Hub een set resources zoals de wachtrij met onderweg cloud-naar-apparaat-berichten per apparaat.

### <a name="when-to-use"></a>Wanneer gebruikt u dit?

Gebruik de id-register wanneer u moet:

* Inrichten van apparaten die verbinding met uw IoT-hub maken.
* Per apparaat toegang tot uw hub apparaat gerichte eindpunten beheren.

> [!NOTE]
> De register-id's bevat geen toepassingsspecifieke metagegevens.

## <a name="identity-registry-operations"></a>Registerbewerkingen voor identiteit

De id-register van IoT Hub bevat de volgende bewerkingen:

* Apparaat-id maken
* Apparaat-ID bijwerken
* Apparaat-id door-ID ophalen
* Verwijderen van apparaat-id
* Lijst van maximaal 1000 identiteiten
* Exporteren van alle identiteiten naar Azure blob-opslag
* Identiteiten importeren uit Azure blob-opslag

Alle deze bewerkingen kunt optimistische gelijktijdigheid van taken, zoals opgegeven in [RFC7232][lnk-rfc7232].

> [!IMPORTANT]
> De enige manier om het ophalen van alle identiteiten in het identiteitenregister een IoT-hub is met de [exporteren] [ lnk-export] functionaliteit.

Een id-register van IoT-Hub:

* Bevat geen metagegevens voor de toepassing.
* Zoals een woordenboek zijn toegankelijk via de **deviceId** als de sleutel.
* Ondersteunt geen expressieve query's.

Een IoT-oplossing heeft doorgaans een afzonderlijk archief oplossings-specifieke die toepassingsspecifieke metagegevens bevat. Het archief oplossings-specifieke in een oplossing voor smart gebouw legt bijvoorbeeld de ruimte bevindt waarin een temperatuursensor wordt geïmplementeerd.

> [!IMPORTANT]
> Alleen gebruiken de register-id's voor beheer van apparaten en de inrichting van bewerkingen. Hoge doorvoersnelheid operations tijdens runtime moeten niet afhankelijk is van het uitvoeren van bewerkingen in het identiteitsregister. De status van de verbinding van een apparaat controleren voordat een opdracht verzonden is bijvoorbeeld niet een ondersteunde patroon. Controleer de [tarieven beperking] [ lnk-quotas] voor het identiteitenregister en de [apparaat heartbeat] [ lnk-guidance-heartbeat] patroon.

## <a name="disable-devices"></a>Apparaten uitschakelen

U kunt apparaten uitschakelen door het bijwerken van de **status** eigenschap van een identiteit in het identiteitsregister. Normaal gesproken gebruikt u deze eigenschap in twee scenario's:

* Tijdens een inrichtingsproces orchestration. Zie voor meer informatie [apparaten inrichten][lnk-guidance-provisioning].
* Als u om welke reden overwegen een apparaat is geknoeid of niet-geautoriseerde is geworden.

## <a name="import-and-export-device-identities"></a>Importeren en exporteren van apparaat-id 's

U kunt apparaat-id's in bulk exporteren uit het register van de identiteit van een IoT-hub, met behulp van asynchrone bewerkingen op de [IoT Hub resource provider-eindpunt][lnk-endpoints]. Uitvoer zijn langlopende taken die gebruikmaken van een klant opgegeven blob-container apparaat identiteitsgegevens gelezen uit het identiteitenregister op te slaan.

U kunt de apparaat-id's in bulk in het identiteitsregister een IoT-hub, importeren met behulp van asynchrone bewerkingen op de [IoT Hub resource provider-eindpunt][lnk-endpoints]. Invoer zijn langlopende taken die gebruikmaken van gegevens in een klant opgegeven blob-container apparaat identiteit om gegevens te schrijven in het identiteitsregister.

* Zie voor gedetailleerde informatie over het importeren en exporteren API's, [resourceprovider IoT Hub REST-API's][lnk-resource-provider-apis].
* Zie voor meer informatie over het uitvoeren van importeren en exporteren van taken, [bulksgewijs beheer van IoT Hub apparaat-id's][lnk-bulk-identity].

## <a name="device-provisioning"></a>Mobiele apparaten inrichten

Het apparaatgegevens die een bepaalde IoT-oplossing zijn opgeslagen, is afhankelijk van de specifieke vereisten van deze oplossing. Maar ten minste een oplossing moet opslaan, apparaat-id's en verificatiesleutels. Azure IoT Hub bevat een identity-registry die waarden voor elk apparaat zoals-id's en verificatiesleutels statuscodes kunt opslaan. Een oplossing kunt u andere Azure-services zoals tabelopslag, blob-opslag of Cosmos-DB gebruiken voor het opslaan van een extra apparaatgegevens.

*Mobiele apparaten inrichten* is het proces van de initiële gegevens toe te voegen aan de worden opgeslagen in uw oplossing. Zodat een nieuw apparaat verbinding maken met uw hub, moet u een apparaat-ID en sleutels toevoegen aan de id-register van IoT Hub. Als onderdeel van het proces van inrichting moet u mogelijk apparaatspecifieke gegevens in andere archieven oplossing initialiseren.

## <a name="device-heartbeat"></a>Apparaat heartbeat

De id-register van IoT Hub bevat een veld met de naam **connectionState**. Gebruik alleen de **connectionState** veld tijdens de ontwikkeling en foutopsporing. IoT-oplossingen moeten het veld niet opvragen tijdens runtime. Bijvoorbeeld, niet zoeken de **connectionState** veld om te controleren of een apparaat is verbonden alvorens u een bericht cloud-naar-apparaat of een SMS-bericht verzenden.

Als uw IoT-oplossing weten moet als een apparaat is verbonden, implementeert u de *heartbeat patroon*.

Het apparaat verzendt apparaat-naar-cloudberichten ten minste één keer voor elke vaste hoeveelheid tijd (bijvoorbeeld ten minste één keer per uur) in het patroon heartbeat. Dus zelfs als een apparaat geen gegevens heeft verzenden, het nog steeds een leeg apparaat-naar-cloud bericht verzenden (meestal met een eigenschap die deze als een heartbeat identificeert) Aan de service onderhoudt de oplossing een kaart met de laatste heartbeat ontvangen voor elk apparaat. Als de oplossing niet binnen de verwachte tijd van het apparaat een heartbeat-bericht ontvangt, wordt ervan uitgegaan dat er een probleem met het apparaat is.

Een complexere implementatie kan bevat de gegevens uit [operations bewaking] [ lnk-devguide-opmon] om apparaten die zijn probeert verbinding te maken of communiceren, maar mislukken te identificeren. Wanneer u het patroon heartbeat implementeert, Controleer of [IoT Hub quota en vertragingen][lnk-quotas].

> [!NOTE]
> Als een IoT-oplossing maakt gebruik van de verbindingsstatus uitsluitend om te bepalen of cloud-naar-apparaat-berichten verzenden en berichten niet worden uitgezonden naar grote verzamelingen van apparaten, kunt u overwegen de eenvoudiger *korte verlooptijd* patroon. Dit patroon bereikt hetzelfde resultaat als een apparaat verbinding status register via de heartbeat-patroon, terwijl het efficiënter wordt onderhouden. Als u een bericht bevestigingen aanvraagt, IoT-Hub u kunt een melding over welke apparaten kunnen geen berichten ontvangen en die niet zijn.

## <a name="device-lifecycle-notifications"></a>Meldingen over de levenscyclus van apparaat

IoT Hub kan uw IoT-oplossing melden wanneer een apparaat-id wordt gemaakt of verwijderd door het verzenden van meldingen over de levenscyclus van apparaat. Om dit te doen, moet uw IoT-oplossing het maken van een route en stelt u de gegevensbron op *DeviceLifecycleEvents*. Standaard geen lifecycle meldingen worden verzonden, dat wil zeggen, geen dergelijke routes vooraf bestaan. De melding omvat eigenschappen en hoofdtekst.

Eigenschappen: Eigenschappen bericht worden voorafgegaan door de `'$'` symbool.

| Naam | Waarde |
| --- | --- |
$content-type | application/json |
$iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
$iothub-bericht-bron | deviceLifecycleEvents |
$content-codering | UTF-8 |
opType | **createDeviceIdentity** of **deleteDeviceIdentity** |
hubName | Naam van de IoT-Hub |
deviceId | ID van het apparaat |
operationTimestamp | ISO8601 tijdstempel van bewerking |
bericht-iothub-schema | deviceLifecycleNotification |

Hoofdtekst: In deze sectie is in JSON-indeling en de twin van de gemaakte apparaat-id vertegenwoordigt. Bijvoorbeeld:

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "etag":"AAAAAAAAAAE=",
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

## <a name="reference-topics"></a>De onderwerpen waarnaar wordt verwezen:

De volgende onderwerpen met naslaginformatie bieden u meer informatie over het identiteitsregister.

## <a name="device-identity-properties"></a>Identiteitseigenschappen van apparaat

Apparaat-id's worden weergegeven als JSON-documenten met de volgende eigenschappen:

| Eigenschap | Opties | Beschrijving |
| --- | --- | --- |
| deviceId |vereist alleen-lezen op updates |Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens + `{'-', ':', '.', '+', '%', '_', '#', '*', '?', '!', '(', ')', ',', '=', '@', ';', '$', '''}`. |
| generationId |vereist alleen-lezen |Een IoT hub is gegenereerd, hoofdlettergevoelig tekenreeks maximaal 128 tekens. Deze waarde wordt gebruikt om te onderscheiden van apparaten met dezelfde **deviceId**wanneer ze zijn verwijderd en opnieuw gemaakt. |
| ETag |vereist alleen-lezen |Een tekenreeks voor een zwakke ETag voor de identiteit van het apparaat als per [RFC7232][lnk-rfc7232]. |
| auth |Optioneel |Een samengestelde-object met de informatie en beveiliging materiaal verificatie. |
| auth.symkey |Optioneel |Een samengestelde-object met een primaire en secundaire sleutel, opgeslagen in base64-indeling. |
| status |Vereist |Een indicator toegang. Kan **ingeschakeld** of **uitgeschakelde**. Als **ingeschakeld**, het apparaat verbinding mag maken. Als **uitgeschakelde**, dit apparaat geen toegang tot een willekeurig eindpunt apparaat gerichte. |
| statusReason |Optioneel |Een 128 tekens lang tekenreeks waarin de reden voor de status van het apparaat-id wordt opgeslagen. Alle UTF-8 tekens zijn toegestaan. |
| statusUpdateTime |alleen-lezen |Een tijdelijke aanduiding met de datum en tijd van de laatste statusupdate. |
| connectionState |alleen-lezen |Een veld dat aangeeft verbindingsstatus: beide **verbonden** of **verbroken**. Dit veld wordt de Iothub-weergave van de status van het apparaat. **Belangrijke**: dit veld mag alleen worden gebruikt voor ontwikkeling/foutopsporing doeleinden. De verbindingsstatus wordt alleen voor apparaten met behulp van protocollen MQTT of AMQP zijn bijgewerkt. Bovendien is het gebaseerd op protocolniveau pings (MQTT pings of AMQP pings) en er een maximale vertraging van slechts 5 minuten. Daarom kunnen er valse positieven, zoals apparaten gerapporteerd als verbonden, maar die niet zijn verbonden. |
| connectionStateUpdatedTime |alleen-lezen |De datum en tijd van laatste status van de verbinding met een tijdelijke indicator is bijgewerkt. |
| lastActivityTime |alleen-lezen |Een tijdelijke aanduiding weer met de datum en de laatste keer dat het apparaat is verbonden, ontvangen of een bericht verzonden. |

> [!NOTE]
> Verbindingsstatus kan alleen bestaan uit de Iothub-weergave van de status van de verbinding. Updates voor deze status kunnen worden vertraagd, afhankelijk van netwerkomstandigheden en configuraties.

## <a name="additional-reference-material"></a>Aanvullende referentiemateriaal

Er zijn andere onderwerpen waarnaar wordt verwezen in de IoT Hub developer guide:

* [IoT-hubeindpunten] [ lnk-endpoints] beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.
* [Bandbreedtebeperking en quota's] [ lnk-quotas] beschrijving van de quota en beperking gedrag die betrekking hebben op de service IoT Hub.
* [Azure IoT-SDKs voor apparaat en de service] [ lnk-sdks] geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apps voor het apparaat en de service die communiceren met IoT Hub-taal.
* [IoT Hub-querytaal] [ lnk-query] beschrijft de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaat horende en taken.
* [Ondersteuning voor IoT Hub MQTT] [ lnk-devguide-mqtt] meer informatie over IoT Hub ondersteuning biedt voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

Nu u het gebruik van de id-register van IoT Hub hebt geleerd, kunt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Toegang beheren met IoT Hub][lnk-devguide-security]
* [Horende apparaten gebruiken om de status en configuraties te synchroniseren][lnk-devguide-device-twins]
* [Een directe methode aangeroepen voor een apparaat][lnk-devguide-directmethods]
* [Taken plannen op meerdere apparaten][lnk-devguide-jobs]

Als u wilt uitproberen enkele concepten die in dit artikel wordt beschreven, kunt u mogelijk geïnteresseerd in de volgende IoT Hub-zelfstudie:

* [Aan de slag met Azure IoT Hub][lnk-getstarted-tutorial]

<!-- Links and images -->

[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-sdks]: iot-hub-devguide-sdks.md
[lnk-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-guidance-provisioning]: iot-hub-devguide-identity-registry.md#device-provisioning
[lnk-guidance-heartbeat]: iot-hub-devguide-identity-registry.md#device-heartbeat
[lnk-rfc7232]: https://tools.ietf.org/html/rfc7232
[lnk-bulk-identity]: iot-hub-bulk-identity-mgmt.md
[lnk-export]: iot-hub-devguide-identity-registry.md#import-and-export-device-identities
[lnk-devguide-opmon]: iot-hub-operations-monitoring.md

[lnk-devguide-security]: iot-hub-devguide-security.md
[lnk-devguide-device-twins]: iot-hub-devguide-device-twins.md
[lnk-devguide-directmethods]: iot-hub-devguide-direct-methods.md
[lnk-devguide-jobs]: iot-hub-devguide-jobs.md

[lnk-getstarted-tutorial]: iot-hub-csharp-csharp-getstarted.md
