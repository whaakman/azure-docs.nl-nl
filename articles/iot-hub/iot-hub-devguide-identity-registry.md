---
title: Inzicht in de Azure IoT Hub-identiteitsregister | Microsoft Docs
description: Handleiding voor ontwikkelaars - beschrijving van de id-register van IoT Hub en hoe u deze gebruiken om uw apparaten te beheren. Bevat informatie over het importeren en exporteren van apparaat-id's in één bulkbewerking.
author: wesmc7777
manager: philmea
ms.author: wesmc
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 08/29/2018
ms.openlocfilehash: 935635c474190413545d1a2731c367a691bfa56d
ms.sourcegitcommit: 15e9613e9e32288e174241efdb365fa0b12ec2ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/28/2019
ms.locfileid: "57010257"
---
# <a name="understand-the-identity-registry-in-your-iot-hub"></a>Inzicht in het identiteitenregister van uw IoT-hub

Elke IoT hub is een id-register waarin informatie over de apparaten en de modules die verbinding mogen maken met de IoT-hub. Voordat een apparaat of de module verbinding met een IoT-hub maken kan, moet er een vermelding voor het apparaat of de module in het id-register van de IoT-hub. Een apparaat of de module moet ook worden geverifieerd bij de IoT-hub op basis van referenties die zijn opgeslagen in het id-register.

De ID van de apparaat- of module die zijn opgeslagen in het id-register is hoofdlettergevoelig.

Op hoog niveau is het id-register een REST-compatibele verzameling apparaat of de module identity-resources. Wanneer u een vermelding in het id-register toevoegen, maakt IoT-Hub u een set met resources per apparaat, zoals de wachtrij met berichten die onderweg zijn cloud-naar-apparaat.

Gebruik het id-register als u wilt:

* Apparaten inrichten of modules die verbinding met uw IoT-hub maken.
* Per-apparaat/per-module toegang tot het apparaat of de module gerichte eindpunten van uw hub beheren.

> [!NOTE]
> * Het id-register bevat niet alle toepassingsspecifieke-metagegevens.
> * Moduledubbel voor identiteits- en -module is in openbare preview. Onder de functie wordt ondersteund op de identiteit van de module wanneer deze algemeen beschikbaar.
>

## <a name="identity-registry-operations"></a>Registerbewerkingen voor identiteit

De id-register van IoT Hub wordt aangegeven dat de volgende bewerkingen:

* Apparaat- of -module maken
* Apparaat-of-module bijwerken
* Apparaat of de module identiteit op ID opgehaald
* Apparaat-of-module verwijderen
* Lijst van maximaal 1000 identiteiten
* Apparaat-id's exporteren naar Azure blob-opslag
* Apparaat-id's importeren uit Azure blob-opslag

Alle deze bewerkingen kunt optimistische gelijktijdigheid van taken, zoals opgegeven in [RFC7232](https://tools.ietf.org/html/rfc7232).

> [!IMPORTANT]
> De enige manier om op te halen van alle identiteiten in een IoT-hub-identiteitsregister is met de [exporteren](iot-hub-devguide-identity-registry.md#import-and-export-device-identities) functionaliteit.

Een id-register van IoT-Hub:

* Bevat niet alle metagegevens van de toepassing.
* Toegankelijk is, zoals een woordenlijst met behulp van de **deviceId** of **moduleId** als de sleutel.
* Biedt geen ondersteuning voor expressieve query's.

Een IoT-oplossing is doorgaans een afzonderlijk archief oplossingsspecifieke die toepassingsspecifieke metagegevens bevat. Het archief oplossingsspecifieke in een oplossing voor slimme gebouwen legt bijvoorbeeld de ruimte bevindt waarin een temperatuursensor is geïmplementeerd.

> [!IMPORTANT]
> Gebruik alleen het id-register voor het beheer van apparaten en bewerkingen wordt ingericht. Hoge doorvoer bewerkingen tijdens runtime moeten niet afhankelijk is van het uitvoeren van bewerkingen in het id-register. Controle van de status van de verbinding van een apparaat voor het verzenden van een opdracht is bijvoorbeeld niet een ondersteunde patroon. Controleer de [tarieven beperking](iot-hub-devguide-quotas-throttling.md) voor het id-register en de [apparaat heartbeat](iot-hub-devguide-identity-registry.md#device-heartbeat) patroon.

## <a name="disable-devices"></a>Apparaten uitschakelen

U kunt apparaten uitschakelen door het bijwerken van de **status** eigenschap van een identiteit in het id-register. Meestal gebruikt u deze eigenschap in twee scenario's:

* Tijdens een inrichtingsproces orchestration. Zie voor meer informatie, [Device Provisioning](iot-hub-devguide-identity-registry.md#device-provisioning).

* Als u om een bepaalde reden, denkt een apparaat is geknoeid of niet-geautoriseerde is geworden.

Deze functie is niet beschikbaar voor modules.

## <a name="import-and-export-device-identities"></a>Importeren en exporteren van apparaat-id 's

Gebruik de asynchrone bewerkingen op de [IoT Hub-resource provider-eindpunt](iot-hub-devguide-endpoints.md) apparaat-id's in bulk exporteren vanuit een IoT-hub-identiteitsregister. Uitvoer worden langlopende taken die gebruikmaken van een klant verstrekte blob-container op te slaan van apparaat-id gegevens die in het id-register gelezen.

Gebruik de asynchrone bewerkingen op de [IoT Hub-eindpunt voor resource provider](iot-hub-devguide-endpoints.md) apparaatidentiteiten bulksgewijs importeren naar een IoT-hub-identiteitsregister. Invoer zijn langdurige taken die gebruikmaken van gegevens in een door de klant verstrekte blobcontainer identiteitsgegevens voor apparaat schrijven in het id-register.

Zie voor meer informatie over het importeren en exporteren API's, [IoT-Hub resourceprovider REST-API's](/rest/api/iothub/iothubresource). Zie voor meer informatie over het uitvoeren van importeren en exporteren van taken, [bulksgewijs beheren van IoT Hub apparaat-id's](iot-hub-bulk-identity-mgmt.md).

## <a name="device-provisioning"></a>Apparaten inrichten

Het apparaatgegevens die een opgegeven IoT-oplossing zijn opgeslagen, is afhankelijk van de specifieke vereisten van die oplossing. Maar als een minimum, een oplossing voor apparaat-id's en verificatiesleutels moet opslaan. Azure IoT Hub bevat een register van identiteiten die waarden voor elk apparaat, zoals id's en verificatiesleutels statuscodes kan opslaan. Een oplossing kunt u andere Azure-services zoals tabelopslag, blob-opslag of Cosmos DB gebruiken voor het opslaan van een extra apparaatgegevens.

*Apparaatinrichting* is het proces van het toevoegen van de initiële gegevens voor de stores in uw oplossing. Als u wilt een nieuw apparaat verbinding maken met uw hub inschakelen, moet u een apparaat-ID en sleutels toevoegen aan de id-register van IoT Hub. Als onderdeel van het proces van inrichting moet u mogelijk apparaatspecifieke gegevens in andere archieven oplossing initialiseren. U kunt ook de Azure IoT Hub Device Provisioning Service gebruiken om in te schakelen zero-touch, just-in-time inrichting naar een of meer IoT-hubs zonder menselijke tussenkomst. Zie voor meer informatie, de [documentatie over service inrichten](https://azure.microsoft.com/documentation/services/iot-dps).

## <a name="device-heartbeat"></a>Apparaat heartbeat

De id-register van IoT Hub bevat een veld met de naam **connectionState**. Gebruik alleen de **connectionState** veld tijdens de ontwikkeling en foutopsporing. IoT-oplossingen moeten geen query uitvoeren op het veld tijdens de uitvoering. Bijvoorbeeld: kan geen query uitvoeren op de **connectionState** veld om te controleren of een apparaat is verbonden, voordat u een cloud-naar-apparaat-bericht of een SMS-bericht verzenden. Het is raadzaam zich abonneert op de [ **apparaat verbroken** gebeurtenis](iot-hub-event-grid.md#event-types) op Event Grid waarschuwingen ontvangen en controleren van de verbindingsstatus van het apparaat. Gebruik deze [zelfstudie](iot-hub-how-to-order-connection-state-events.md) voor informatie over het apparaat is verbonden en het apparaat verbroken gebeurtenissen uit IoT Hub in uw IoT-oplossing integreren.

Als uw IoT-oplossing nodig heeft om te weten als een apparaat is verbonden, kunt u implementeren de *heartbeat patroon*.
In het heartbeat-patroon maakt verzendt het apparaat apparaat-naar-cloud-berichten ten minste één keer voor elke vaste hoeveelheid tijd (bijvoorbeeld ten minste één keer per uur). Dus zelfs als een apparaat beschikt niet over alle gegevens worden verzonden, stuurt nog steeds een lege apparaat-naar-cloud-bericht (meestal met een eigenschap die deze als een heartbeat identificeert). Aan de service voor deze oplossing wordt een kaart met de laatste heartbeat ontvangen voor elk apparaat. Als de oplossing niet binnen de verwachte tijd van het apparaat een heartbeat-bericht ontvangt, wordt ervan uitgegaan dat er een probleem met het apparaat is.

Een meer complexe implementatie kan bijvoorbeeld de gegevens van [Azure Monitor](../azure-monitor/index.yml) en [Azure Resource Health](../service-health/resource-health-overview.md) om apparaten te identificeren die probeert verbinding te maken of communiceren, maar mislukt, controleert u [Bewaken met diagnostische gegevens](iot-hub-monitor-resource-health.md) handleiding. Wanneer u het patroon heartbeat implementeert, Controleer of [IoT Hub-quota en vertragingen in](iot-hub-devguide-quotas-throttling.md).

> [!NOTE]
> Als een IoT-oplossing maakt gebruik van de verbindingsstatus uitsluitend om te bepalen of cloud-naar-apparaat-berichten verzenden en berichten niet op grote sets apparaten worden uitgezonden, kunt u overwegen de eenvoudiger *korte verlooptijd* patroon. Dit patroon geven hetzelfde resultaat als een apparaat verbinding status register met behulp van de heartbeat-patroon, terwijl u efficiënter te beheren. Als u een bericht bevestigingen aangevraagd, IoT-Hub u kunt een melding over welke apparaten kunnen om berichten te ontvangen en die niet zijn.

## <a name="device-and-module-lifecycle-notifications"></a>Apparaat- en -module meldingen over de levenscyclus

IoT Hub kunnen uw IoT-oplossing waarschuwen wanneer een identiteit die wordt gemaakt of verwijderd door het verzenden van meldingen over de levenscyclus. Om dit te doen, moet uw IoT-oplossing een route te maken en in te stellen de gegevensbron gelijk zijn aan *DeviceLifecycleEvents* of *ModuleLifecycleEvents*. Standaard geen lifecycle-meldingen worden verzonden, dat wil zeggen, er zijn geen dergelijke routes vooraf bestaan. De melding bevat eigenschappen en hoofdtekst.

Eigenschappen: Systeemeigenschappen bericht worden voorafgegaan door de `$` symbool.

Melding voor apparaat:

| Name | Value |
| --- | --- |
|$content-type | application/json |
|$iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
|$iothub-bericht-bron | deviceLifecycleEvents |
|$content-encoding | utf-8 |
|opType | **createDeviceIdentity** of **deleteDeviceIdentity** |
|hubName | Naam van IoT Hub |
|deviceId | ID van het apparaat |
|operationTimestamp | ISO8601-timestamp van bewerking |
|iothub-bericht-schema | deviceLifecycleNotification |

Hoofdtekst: In deze sectie wordt in JSON-indeling en vertegenwoordigt het dubbele van de gemaakte apparaat-id. Bijvoorbeeld:

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
Melding voor module:

| Name | Value |
| --- | --- |
$content-type | application/json |
$iothub-enqueuedtime |  Tijd waarop de melding is verzonden |
$iothub-bericht-bron | moduleLifecycleEvents |
$content-encoding | utf-8 |
opType | **createModuleIdentity** of **deleteModuleIdentity** |
hubName | Naam van IoT Hub |
moduleId | ID van de module |
operationTimestamp | ISO8601-timestamp van bewerking |
iothub-bericht-schema | moduleLifecycleNotification |

Hoofdtekst: Deze sectie wordt in JSON-indeling en vertegenwoordigt het dubbele van de identiteit van de gemaakte module. Bijvoorbeeld:

```json
{
    "deviceId":"11576-ailn-test-0-67333793211",
    "moduleId":"tempSensor",
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

## <a name="device-identity-properties"></a>Eigenschappen van apparaat-id

Apparaat-id's worden weergegeven als JSON-documenten met de volgende eigenschappen:

| Eigenschap | Opties | Description |
| --- | --- | --- |
| deviceId |vereist, alleen-lezen op updates |Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens, plus bepaalde speciale tekens: `- . + % _ # * ? ! ( ) , = @ $ '`. |
| generationId |vereist, alleen-lezen |Een IoT hub is gegenereerd, hoofdlettergevoelig tekenreeks van maximaal 128 tekens lang. Deze waarde wordt gebruikt om u te onderscheiden van apparaten met dezelfde **deviceId**, wanneer ze zijn verwijderd en opnieuw gemaakt. |
| etag |vereist, alleen-lezen |Een tekenreeks voor een zwakke ETag voor de apparaat-id als per [RFC7232](https://tools.ietf.org/html/rfc7232). |
| auth |optioneel |Een samengestelde-object met verificatie-informatie en beveiliging materiaal. |
| auth.symkey |optioneel |Een samengestelde-object met een primaire en secundaire sleutel, die zijn opgeslagen in Base 64-indeling. |
| status |vereist |Een indicator toegang. Kan **ingeschakeld** of **uitgeschakelde**. Als **ingeschakeld**, het apparaat is toegestaan om verbinding te maken. Als **uitgeschakelde**, dit apparaat geen toegang tot een willekeurig apparaat gerichte-eindpunt. |
| statusReason |optioneel |Een 128 tekens lang tekenreeks waarin de reden voor de status van het apparaat-id. Alle UTF-8 tekens zijn toegestaan. |
| statusUpdateTime |alleen-lezen |Een tijdelijke aanduiding met de datum en tijd van de laatste statusupdate. |
| connectionState |alleen-lezen |Een veld verbindingsstatus aangeeft: beide **verbonden** of **verbroken**. Dit veld geeft de IoT Hub-weergave van de status van het apparaat verbinding. **Belangrijke**: Dit veld moet alleen worden gebruikt voor ontwikkeling/foutopsporing doeleinden. Status van de verbinding wordt alleen voor apparaten die gebruikmaken van MQTT- of AMQP bijgewerkt. Bovendien is gebaseerd op protocolniveau pings (MQTT pings of AMQP pings) en er een maximale vertraging van slechts vijf minuten. Daarom kunnen er fout-positieven, zoals apparaten gerapporteerd als verbonden, maar die niet zijn verbonden. |
| connectionStateUpdatedTime |alleen-lezen |Een tijdelijke indicator, met de datum en tijd van laatste status van de verbinding is bijgewerkt. |
| lastActivityTime |alleen-lezen |Een tijdelijke aanduiding die laat zien de datum en de laatste keer dat het apparaat verbinding, ontvangen of een bericht verzonden. |

> [!NOTE]
> Verbindingsstatus kan alleen bestaan uit de IoT Hub-weergave van de status van de verbinding. Updates voor deze status kunnen worden uitgesteld, afhankelijk van het netwerk en configuraties.

> [!NOTE]
> Het apparaat SDK's bieden momenteel geen ondersteuning met behulp van de `+` en `#` tekens in de **deviceId**.

## <a name="module-identity-properties"></a>Identiteitseigenschappen van module

Module-id's worden weergegeven als JSON-documenten met de volgende eigenschappen:

| Eigenschap | Opties | Description |
| --- | --- | --- |
| deviceId |vereist, alleen-lezen op updates |Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens, plus bepaalde speciale tekens: `- . + % _ # * ? ! ( ) , = @ $ '`. |
| moduleId |vereist, alleen-lezen op updates |Een hoofdlettergevoelige tekenreeks (maximaal 128 tekens lang) van ASCII-7-bits alfanumerieke tekens, plus bepaalde speciale tekens: `- . + % _ # * ? ! ( ) , = @ $ '`. |
| generationId |vereist, alleen-lezen |Een IoT hub is gegenereerd, hoofdlettergevoelig tekenreeks van maximaal 128 tekens lang. Deze waarde wordt gebruikt om u te onderscheiden van apparaten met dezelfde **deviceId**, wanneer ze zijn verwijderd en opnieuw gemaakt. |
| etag |vereist, alleen-lezen |Een tekenreeks voor een zwakke ETag voor de apparaat-id als per [RFC7232](https://tools.ietf.org/html/rfc7232). |
| auth |optioneel |Een samengestelde-object met verificatie-informatie en beveiliging materiaal. |
| auth.symkey |optioneel |Een samengestelde-object met een primaire en secundaire sleutel, die zijn opgeslagen in Base 64-indeling. |
| status |vereist |Een indicator toegang. Kan **ingeschakeld** of **uitgeschakelde**. Als **ingeschakeld**, het apparaat is toegestaan om verbinding te maken. Als **uitgeschakelde**, dit apparaat geen toegang tot een willekeurig apparaat gerichte-eindpunt. |
| statusReason |optioneel |Een 128 tekens lang tekenreeks waarin de reden voor de status van het apparaat-id. Alle UTF-8 tekens zijn toegestaan. |
| statusUpdateTime |alleen-lezen |Een tijdelijke aanduiding met de datum en tijd van de laatste statusupdate. |
| connectionState |alleen-lezen |Een veld verbindingsstatus aangeeft: beide **verbonden** of **verbroken**. Dit veld geeft de IoT Hub-weergave van de status van het apparaat verbinding. **Belangrijke**: Dit veld moet alleen worden gebruikt voor ontwikkeling/foutopsporing doeleinden. Status van de verbinding wordt alleen voor apparaten die gebruikmaken van MQTT- of AMQP bijgewerkt. Bovendien is gebaseerd op protocolniveau pings (MQTT pings of AMQP pings) en er een maximale vertraging van slechts vijf minuten. Daarom kunnen er fout-positieven, zoals apparaten gerapporteerd als verbonden, maar die niet zijn verbonden. |
| connectionStateUpdatedTime |alleen-lezen |Een tijdelijke indicator, met de datum en tijd van laatste status van de verbinding is bijgewerkt. |
| lastActivityTime |alleen-lezen |Een tijdelijke aanduiding die laat zien de datum en de laatste keer dat het apparaat verbinding, ontvangen of een bericht verzonden. |

> [!NOTE]
> Het apparaat SDK's bieden momenteel geen ondersteuning met behulp van de `+` en `#` tekens in de **deviceId** en **moduleId**.

## <a name="additional-reference-material"></a>Meer referentiemateriaal dat beschikbaar is

Er zijn andere onderwerpen met naslaginformatie in de IoT Hub developer guide:

* [IoT Hub-eindpunten](iot-hub-devguide-endpoints.md) beschrijft de verschillende eindpunten die elke IoT-hub voor runtime- en beheerbewerkingen toont.

* [Bandbreedtebeperking en quota's](iot-hub-devguide-quotas-throttling.md) beschrijving van de quota en beperkingen van problemen die betrekking hebben op de IoT Hub-service.

* [Azure IoT-apparaat en service-SDK's](iot-hub-devguide-sdks.md) geeft een lijst van de verschillende SDK's kunt u bij het ontwikkelen van apparaat- en service-apps die communiceren met IoT Hub-taal.

* [IoT Hub-querytaal](iot-hub-devguide-query-language.md) beschrijving van de querytaal kunt u gegevens ophalen uit IoT Hub over uw apparaatdubbels en taken.

* [IoT Hub MQTT-ondersteuning](iot-hub-mqtt-support.md) vindt u meer informatie over IoT Hub-ondersteuning voor het MQTT-protocol.

## <a name="next-steps"></a>Volgende stappen

U hebt geleerd hoe u de id-register van IoT Hub gebruikt, kunt u mogelijk geïnteresseerd in de volgende onderwerpen van IoT Hub developer guide:

* [Toegang tot IoT Hub regelen](iot-hub-devguide-security.md)

* [Apparaatdubbels gebruiken voor het synchroniseren van de status en -configuraties](iot-hub-devguide-device-twins.md)

* [Een rechtstreekse methode aanroepen op een apparaat](iot-hub-devguide-direct-methods.md)

* [Taken op meerdere apparaten plannen](iot-hub-devguide-jobs.md)

Als u wilt uitproberen enkele concepten die worden beschreven in dit artikel, raadpleegt u de volgende zelfstudie voor IoT-Hub:

* [Aan de slag met Azure IoT Hub](quickstart-send-telemetry-dotnet.md)

Om te verkennen met behulp van de IoT Hub Device Provisioning Service inschakelen zero-touch, just-in-time inrichting, Zie: 

* [Azure IoT Hub Device Provisioning Service](https://azure.microsoft.com/documentation/services/iot-dps)
