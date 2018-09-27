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
ms.openlocfilehash: 9a07fa2010eef22c4d1477641d07dee70ab5a9cb
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47227427"
---
# <a name="how-to-manage-connectivity-and-reliable-messaging-using-azure-iot-hub-device-sdks"></a>Over het beheren van verbindingen en betrouwbare uitwisseling van berichten met behulp van Azure IoT Hub apparaat-SDK 's

Deze handleiding bevat op hoog niveau richtlijnen voor het ontwerpen van flexibele apparaattoepassingen, door te profiteren van de connectiviteit en betrouwbare berichtfuncties in Azure IoT device SDK's. Het doel van dit artikel is voor het beantwoorden van vragen en verwerken van deze scenario's:

- beheren van een uitgevallen netwerkverbinding
- schakelen tussen verschillende netwerkverbindingen beheren
- opnieuw verbinden vanwege de tijdelijke verbindingsfouten service beheren

Implementatiegegevens kunnen variëren per taal, Zie gekoppelde API-documentatie of specifieke SDK voor meer informatie.

- [C/Python/iOS SDK](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Node-SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)


## <a name="designing-for-resiliency"></a>Ontwerpen voor flexibiliteit

IoT-apparaten zijn vaak afhankelijk van niet-doorlopende en/of instabiel netwerkverbindingen zoals GSM of satelliet. Bovendien tijdens interactie met cloud-gebaseerde services, kunnen fouten optreden vanwege tijdelijke omstandigheden, zoals onregelmatige servicebeschikbaarheid en -infrastructuur-of toepassingsniveau fouten (vaak aangeduid als tijdelijke fouten). Er moet een toepassing die wordt uitgevoerd op een apparaat voor het beheren van de verbinding en mechanismen voor opnieuw verbinden, evenals de logica voor opnieuw proberen voor verzenden/ontvangen berichten. Daarnaast afhankelijk de vereisten van de strategie voor opnieuw proberen van geheugenlatentie de IoT-scenario deelneemt aan het apparaat, en het context en de mogelijkheden van het apparaat.

De Azure IoT Hub apparaat-SDK's erop gericht om verbinding te maken en gecommuniceerd tussen cloud-naar-apparaat- en apparaat-naar-cloud door te geven van een robuuste en uitgebreide manier om verbinding te maken en verzenden/ontvangen van berichten naar en van Azure IoT Hub te vereenvoudigen. Ontwikkelaars kunnen de bestaande implementatie voor het ontwikkelen van een strategie voor de juiste opnieuw proberen voor een bepaald scenario ook wijzigen.

De relevante SDK-functies die ondersteuning bieden voor connectiviteit en betrouwbare uitwisseling van berichten worden in de volgende secties besproken.

## <a name="connection-and-retry"></a>Verbinding en probeer het opnieuw

In deze sectie biedt een overzicht van de patronen opnieuw en probeer het opnieuw beschikbaar bij het beheren van verbindingen, advies voor implementaties voor het gebruik van beleid voor verschillende nieuwe pogingen in uw, apparaattoepassing en de relevante API's voor de apparaat-SDK's.

### <a name="error-patterns"></a>Fout-patronen
Fouten bij het verbinden kunnen gebeuren in veel niveaus:

-  Fouten in, zoals een niet-verbonden socket en naam resolutie-netwerk
- Op protocolniveau fouten voor HTTP, AMQP en MQTT transport, zoals koppelingen ontkoppeld of sessies is verlopen
- Op toepassingsniveau fouten die het resultaat zijn van een lokale fouten zoals ongeldige referenties of service gedrag zoals quotum overschrijdt of beperking

De apparaat-SDK's detecteren van fouten in alle drie niveaus.  OS-gerelateerde fouten en hardware zijn niet gedetecteerd en verwerkt door de apparaat-SDK's.  Het ontwerp is gebaseerd op [de tijdelijke fouten afhandelen richtlijnen](/azure/architecture/best-practices/transient-faults#general-guidelines) van Azure Architecture Center.

### <a name="retry-patterns"></a>Patronen opnieuw proberen

Het algehele proces voor het opnieuw wanneer er fouten worden gedetecteerd is: 
1. De SDK detecteert de fout en de bijbehorende fout in het netwerk, protocol, of de toepassing.
2. Op basis van het fouttype, probeer de SDK gebruikt de fout filteren om te bepalen als moet worden uitgevoerd.  Als een **onherstelbare fout** wordt geïdentificeerd door de SDK bewerkingen (verbinding en het versturen en ontvangen) wordt gestopt en de SDK aan de gebruiker weten. Een onherstelbare fout is een fout die de SDK kunt identificeren en te bepalen dat deze niet worden hersteld, bijvoorbeeld, verificatie of een ongeldig eindpunt fout.
3. Als een **onherstelbare fout** is geïdentificeerd, de SDK begint om opnieuw te proberen met behulp van het beleid voor opnieuw proberen dat is opgegeven totdat een gedefinieerde time-out is verlopen.
4. Wanneer de opgegeven time-out is verlopen, de SDK probeert verbinding te maken of verzendt, stopt en de gebruiker wordt geïnformeerd.
5.  De SDK kan de gebruiker te koppelen van een callback voor het ontvangen van de status verandert. 

Beleid voor drie opnieuw proberen zijn beschikbaar:
- **Exponentieel uitstel met jitter**: dit is het standaardbeleid voor opnieuw proberen die worden toegepast.  Het is doorgaans agressief aan het begin, vertraagt de en vervolgens komt binnen via een maximale vertraging die niet is overschreden.  Het ontwerp is gebaseerd op [richtlijnen van Azure Architecture Center voor opnieuw proberen](https://docs.microsoft.com/azure/architecture/best-practices/retry-service-specific).
- **Aangepaste nieuwe poging**: U kunt een beleid voor aangepaste nieuwe poging implementeren en deze invoeren in het RetryPolicy, afhankelijk van de taal die u kiest. U kunt een beleid voor opnieuw proberen dat geschikt is voor uw scenario ontwerpen.  Dit is niet beschikbaar op de C-SDK.
- **Er geen nieuwe**: Er is een optie voor het instellen van beleid voor opnieuw proberen te 'geen nieuwe poging doen,"waardoor de logica voor opnieuw proberen is uitgeschakeld.  De SDK probeert verbinding maken met één keer en verzend een bericht eenmaal, ervan uitgaande dat de verbinding tot stand is gebracht. Dit beleid meestal worden gebruikt in gevallen waarbij er problemen voor bandbreedte of de kosten.   Als deze optie is gekozen, worden berichten die niet voldoen aan voor het verzenden van gaan verloren en kunnen niet worden hersteld. 

### <a name="retry-policy-apis"></a>API's voor beleid voor opnieuw proberen

   | SDK | Methode SetRetryPolicy | Beleidsimplementaties | Begeleiding bij implementatie |
   |-----|----------------------|--|--|
   |  C/Python/iOS  | [IOTHUB_CLIENT_RESULT IoTHubClient_SetRetryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/2018-05-04/iothub_client/inc/iothub_client.h#L188)        | **Standaard**: [IOTHUB_CLIENT_RETRY_EXPONENTIAL_BACKOFF](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Aangepast:** gebruik beschikbaar [retryPolicy](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)<BR>**Er zijn geen nieuwe poging:** [IOTHUB_CLIENT_RETRY_NONE](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#connection-retry-policies)  | [C/Python/iOS-implementatie](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/connection_and_messaging_reliability.md#)  |
   | Java| [SetRetryPolicy](https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device._device_client_config.setretrypolicy?view=azure-java-stable)        | **Standaard**: [ExponentialBackoffWithJitter klasse](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)<BR>**Aangepast:** implementeren [RetryPolicy-interface](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/RetryPolicy.java)<BR>**Er zijn geen nieuwe poging:** [NoRetry klasse](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/src/main/java/com/microsoft/azure/sdk/iot/device/transport/NoRetry.java)  | [Java-toepassing](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md) |[.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
   | .NET| [DeviceClient.SetRetryPolicy](/dotnet/api/microsoft.azure.devices.client.deviceclient.setretrypolicy?view=azure-dotnet#Microsoft_Azure_Devices_Client_DeviceClient_SetRetryPolicy_Microsoft_Azure_Devices_Client_IRetryPolicy) | **Standaard**: [ExponentialBackoff klasse](/dotnet/api/microsoft.azure.devices.client.exponentialbackoff?view=azure-dotnet)<BR>**Aangepast:** implementeren [IRetryPolicy-interface](https://docs.microsoft.com/dotnet/api/microsoft.azure.devices.client.iretrypolicy?view=azure-dotnet)<BR>**Er zijn geen nieuwe poging:** [NoRetry klasse](/dotnet/api/microsoft.azure.devices.client.noretry?view=azure-dotnet) | [Implementatie van C#](https://github.com/Azure/azure-iot-sdk-csharp) |
   | Knooppunt| [SetRetryPolicy](/javascript/api/azure-iot-device/client?view=azure-iot-typescript-latest#azure_iot_device_Client_setRetryPolicy) | **Standaard**: [ExponentialBackoffWithJitter klasse](/javascript/api/azure-iot-common/exponentialbackoffwithjitter?view=azure-iot-typescript-latest)<BR>**Aangepast:** implementeren [RetryPolicy-interface](/javascript/api/azure-iot-common/retrypolicy?view=azure-iot-typescript-latest)<BR>**Er zijn geen nieuwe poging:** [NoRetry klasse](/javascript/api/azure-iot-common/noretry?view=azure-iot-typescript-latest) | [Knooppunt-implementatie](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them) |
   

Hieronder vindt u codevoorbeelden met daarin deze stroom. 

#### <a name="net-implementation-guidance"></a>.NET-Implementatiehandleiding

Het volgende codevoorbeeld laat zien hoe om te definiëren en instellen van het standaardbeleid voor opnieuw proberen:

   ```csharp
   # define/set default retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(int.MaxValue, TimeSpan.FromMilliseconds(100), TimeSpan.FromSeconds(10), TimeSpan.FromMilliseconds(100));
   SetRetryPolicy(retryPolicy);
   ```

Om te voorkomen dat een hoog CPU-gebruik, de nieuwe pogingen wordt vertraagd als de code niet onmiddellijk (bijvoorbeeld wanneer er is geen netwerk of de route naar de bestemming) zodat de minimale tijd voor het uitvoeren van de volgende poging 1 seconde is. 

Als de service met een beperking-fout reageert, wordt het beleid voor opnieuw proberen is anders en kan niet worden gewijzigd via de openbare API:

   ```csharp
   # throttled retry policy
   RetryPolicy retryPolicy = new ExponentialBackoff(RetryCount, TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(60), TimeSpan.FromSeconds(5));
   SetRetryPolicy(retryPolicy);
   ```

Het mechanisme voor opnieuw proberen gestopt nadat `DefaultOperationTimeoutInMilliseconds`, die momenteel is ingesteld op vier minuten.

#### <a name="other-languages-implementation-guidance"></a>Andere talen-Implementatiehandleiding
Bekijk de volgende implementatie-documentatie voor andere talen.  Voorbeelden van het gebruik van beleid voor opnieuw proberen die API 's u in de opslagplaats vindt.
- [C/Python/iOS SDK](https://github.com/azure/azure-iot-sdk-c)
- [.NET SDK](https://github.com/Azure/azure-iot-sdk-csharp/blob/master/iothub/device/devdoc/requirements/retrypolicy.md)
- [Java SDK](https://github.com/Azure/azure-iot-sdk-java/blob/master/device/iot-device-client/devdoc/requirement_docs/com/microsoft/azure/iothub/retryPolicy.md)
- [Node-SDK](https://github.com/Azure/azure-iot-sdk-node/wiki/Connectivity-and-Retries#types-of-errors-and-how-to-detect-them)

## <a name="next-steps"></a>Volgende stappen
- [Apparaat- en service-SDK's gebruiken](.\iot-hub-devguide-sdks.md)
- [De IoT-apparaat-SDK voor C gebruiken](.\iot-hub-device-sdk-c-intro.md)
- [Ontwikkelen voor beperkte apparaten](.\iot-hub-devguide-develop-for-constrained-devices.md)
- [Ontwikkelen voor mobiele apparaten](.\iot-hub-how-to-develop-for-mobile-devices.md)
- [Oplossen van apparaat wordt verbroken](iot-hub-troubleshoot-connectivity.md)
