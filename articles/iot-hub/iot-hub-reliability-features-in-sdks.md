---
title: Over het beheren van verbindingen en betrouwbare uitwisseling van berichten met behulp van Azure IoT Hub apparaat-SDK 's
description: Meer informatie over het verbeteren van de connectiviteit van uw apparaten en bij het gebruik van de apparaat-SDK's van de Azure IoT Hub-berichten
services: iot-hub
keywords: ''
author: yzhong94
ms.author: yizhon
ms.date: 07/07/2018
ms.topic: article
ms.service: iot-hub
documentationcenter: ''
manager: timlt
ms.devlang: na
ms.custom: mvc
ms.openlocfilehash: 8951680ca9488dabffd02ee084e3f6827122276e
ms.sourcegitcommit: 5d837a7557363424e0183d5f04dcb23a8ff966bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52957449"
---
# <a name="manage-connectivity-and-reliable-messaging-by-using-azure-iot-hub-device-sdks"></a>Connectiviteit en betrouwbare uitwisseling van berichten met behulp van Azure IoT Hub apparaat-SDK's beheren

Dit artikel vindt u op hoog niveau hulp bij het ontwerpen van apparaattoepassingen die toleranter zijn. Hier ziet u hoe u kunt profiteren van de connectiviteit en betrouwbare berichtfuncties in Azure IoT device SDK's. Het doel van deze handleiding is voor het beheren van de volgende scenario's:

- Oplossen van een uitgevallen netwerkverbinding
- Schakelen tussen verschillende netwerkverbindingen
- Opnieuw verbinding maken vanwege fouten in de tijdelijke verbindingsfouten service

Implementatiegegevens kunnen variëren per taal. Zie voor meer informatie, de API-documentatie of de specifieke SDK:

- [C/Python/iOS SDK](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Node-SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="designing-for-resiliency"></a>Ontwerpen voor flexibiliteit

IoT-apparaten zijn vaak afhankelijk van niet-doorlopende of instabiel Netwerkverbindingen (bijvoorbeeld GSM of satelliet). Fouten kunnen optreden wanneer apparaten interactie met cloudservices vanwege onregelmatige service beschikbaarheid en infrastructuur op serverniveau of tijdelijke fouten hebben. Een toepassing die wordt uitgevoerd op een apparaat is voor het beheren van de mechanismen voor verbinding en opnieuw verbinden met de logica voor opnieuw proberen voor het verzenden en ontvangen van berichten. Ook afhankelijk de vereisten van de strategie voor opnieuw proberen van geheugenlatentie van het apparaat IoT-scenario, context, mogelijkheden.

De Azure IoT Hub apparaat-SDK's erop gericht om verbinding te maken en gecommuniceerd tussen cloud-naar-apparaat- en apparaat-naar-cloud te vereenvoudigen. Deze SDK's bieden een krachtige manier om verbinding maken met Azure IoT Hub en een uitgebreide set met opties voor het verzenden en ontvangen van berichten. Ontwikkelaars kunnen de bestaande implementatie voor het aanpassen van een betere strategie voor opnieuw proberen voor een bepaald scenario ook wijzigen.

De relevante SDK-functies die ondersteuning bieden voor connectiviteit en betrouwbare uitwisseling van berichten worden in de volgende secties besproken.

## <a name="connection-and-retry"></a>Verbinding en probeer het opnieuw

In deze sectie biedt een overzicht van de patronen opnieuw en probeer het opnieuw beschikbaar bij het beheren van verbindingen. Het advies voor implementaties voor het gebruik van een beleid voor verschillende nieuwe pogingen in uw apparaattoepassing details en een lijst met relevante API's van de apparaat-SDK's.

### <a name="error-patterns"></a>Fout-patronen
Fouten bij het verbinden kunnen gebeuren op veel niveaus:

- Netwerkfouten: verbinding verbroken socket en de naam resolutie-fouten
- Protocolniveau fouten voor HTTP, AMQP en MQTT transport: koppelingen ontkoppeld of verlopen sessies
- Op toepassingsniveau fouten die het resultaat zijn van een lokale fouten: ongeldige referenties of het servicegedrag (bijvoorbeeld, het quotum overschrijden of beperking)

De apparaat-SDK's detecteren van fouten op alle drie niveaus. OS-gerelateerde fouten en hardware zijn niet gedetecteerd en verwerkt door de apparaat-SDK's. Het ontwerp van de SDK is gebaseerd op [de tijdelijke fouten afhandelen richtlijnen](/azure/architecture/best-practices/transient-faults#general-guidelines) van het Azure Architecture Center.

### <a name="retry-patterns"></a>Patronen opnieuw proberen

De volgende stappen beschrijven het proces opnieuw wanneer er fouten worden gedetecteerd:

1. De SDK detecteert de fout en de bijbehorende fout in het netwerk, het protocol of de toepassing.
1. De SDK gebruikt de fout-filter om te bepalen welk fouttype en bepalen of een nieuwe poging nodig is.
1. Als de SDK identificeert een **onherstelbare fout**, bewerkingen, zoals verbinding, verzenden en ontvangen zijn gestopt. De SDK meldt de gebruiker. Voorbeelden van onherstelbare fouten zijn een verificatiefout en een ongeldig eindpunt-fout.
1. Als de SDK identificeert een **onherstelbare fout**, het opnieuw probeert op basis van uw beleid voor de opgegeven opnieuw proberen totdat de opgegeven time-out is verstreken.
1. Wanneer de opgegeven time-out is verlopen, stopt de SDK probeert verbinding te maken of verzenden. Deze waarschuwt de gebruiker.
1. De SDK kan de gebruiker te koppelen van een callback voor het ontvangen van de status verandert.

De SDK's bieden dat drie beleidsregels voor opnieuw proberen:

- **Exponentieel uitstel met jitter**: dit standaardbeleid voor opnieuw proberen is doorgaans agressief aan het begin en vertragen na verloop van tijd totdat een maximale vertraging is bereikt. Het ontwerp is gebaseerd op [richtlijnen van Azure Architecture Center voor opnieuw proberen](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific).
- **Aangepaste nieuwe poging**: voor sommige talen SDK kunt u een beleid voor aangepaste opnieuw proberen dat beter geschikt is voor uw scenario en deze vervolgens invoeren in het RetryPolicy kunt ontwerpen. Aangepaste nieuwe poging is niet beschikbaar in de C-SDK.
- **Er geen nieuwe**: U kunt instellen dat beleid voor opnieuw proberen te 'geen nieuwe poging doen,"waardoor de logica voor opnieuw proberen is uitgeschakeld. De SDK probeert verbinding maken met één keer en verzend een bericht eenmaal, ervan uitgaande dat de verbinding tot stand is gebracht. Dit beleid wordt doorgaans gebruikt in scenario's met de bandbreedte of kosten betreft. Als u deze optie kiest, worden de berichten die niet voldoen aan voor het verzenden van gaan verloren en kunnen niet worden hersteld.

### <a name="retry-policy-apis"></a>API's voor beleid voor opnieuw proberen

   | SDK | Methode SetRetryPolicy | Beleidsimplementaties | Begeleiding bij implementatie |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Standaard**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Aangepast:** gebruik beschikbaar [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Er zijn geen nieuwe poging:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [C/Python/iOS-implementatie](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._device_client_config.setretrypolicy?view=azure-java-stable)        | **Standaard**: [ExponentialBackoffWithJitter klasse](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Aangepast:** implementeren [RetryPolicy-interface](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Er zijn geen nieuwe poging:** [NoRetry klasse](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Java-toepassing](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |[.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_SetRetryPolicy_Microsoft_Azure_Devices_Client_IRetryPolicy) | **Standaard**: [ExponentialBackoff klasse](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Aangepast:** implementeren [IRetryPolicy-interface](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet)<BR>**Er zijn geen nieuwe poging:** [NoRetry klasse](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [Implementatie van C#](https://github.com/Azure/azure-iot-sdk-csharp) |
   | Knooppunt| [SetRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest#azure_iot_device_Client_setRetryPolicy) | **Standaard**: [ExponentialBackoffWithJitter klasse](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Aangepast:** implementeren [RetryPolicy-interface](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Er zijn geen nieuwe poging:** [NoRetry klasse](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Knooppunt-implementatie](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |

Voorbeelden van de volgende code ziet u deze stroom:

#### <a name="net-implementation-guidance"></a>.NET-Implementatiehandleiding

Het volgende codevoorbeeld laat zien hoe om te definiëren en instellen van het standaardbeleid voor opnieuw proberen:

   ```csharp
   # define/set default retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Om te voorkomen dat een hoog CPU-gebruik, worden de nieuwe pogingen als de code onmiddellijk mislukt beperkt. Bijvoorbeeld: wanneer er is geen netwerk of de route naar de bestemming. De minimale tijd voor het uitvoeren van de volgende poging is 1 seconde.

Als de service met een beperking-fout reageert, wordt het beleid voor opnieuw proberen is anders en kan niet worden gewijzigd via de openbare API:

   ```csharp
   # throttled retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5));
   SetRetryPolicy(retryPolicy);
   ```

Het mechanisme voor opnieuw proberen na stopt `DefaultOperationTimeoutInMilliseconds`, die momenteel is ingesteld op vier minuten.

#### <a name="other-languages-implementation-guidance"></a>Andere talen-Implementatiehandleiding

Voor codevoorbeelden in andere talen, raadpleegt u de volgende documenten voor de implementatie. De opslagplaats bevat voorbeelden van met het gebruik van beleid voor opnieuw proberen API's.

- [C/Python/iOS SDK](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Node-SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Volgende stappen
- [Apparaat- en service-SDK's gebruiken](./iot-hub-devguide-sdks.md)
- [De IoT-apparaat-SDK voor C gebruiken](./iot-hub-device-sdk-c-intro.md)
- [Ontwikkelen voor beperkte apparaten](./iot-hub-devguide-develop-for-constrained-devices.md)
- [Ontwikkelen voor mobiele apparaten](./iot-hub-how-to-develop-for-mobile-devices.md)
- [Oplossen van apparaat wordt verbroken](iot-hub-troubleshoot-connectivity.md)
