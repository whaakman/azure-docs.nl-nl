---
title: Het apparaat met Azure IoT SDK voor C | Microsoft Docs
description: Aan de slag met het apparaat met Azure IoT SDK voor C en informatie over het maken van apps voor apparaten die communiceren met een IoT-hub.
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: e448b061-6bdd-470a-a527-15ec03cca7b9
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/25/2017
ms.author: obloch
ms.openlocfilehash: 99a430810b915f4ca06d9c07182319eff1fa299e
ms.sourcegitcommit: f1c1789f2f2502d683afaf5a2f46cc548c0dea50
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/18/2018
---
# <a name="azure-iot-device-sdk-for-c"></a>Azure IoT-apparaat SDK voor C

De **Azure IoT-device SDK** is een set van bibliotheken die zijn ontworpen voor het vereenvoudigen van het proces van het verzenden van berichten naar en ontvangen van berichten uit de **Azure IoT Hub** service. Er zijn verschillende variaties van de SDK, elke die gericht is op een specifiek platform, maar in dit artikel beschrijft de **Azure IoT-device SDK voor C**.

Het apparaat met Azure IoT SDK voor C is geschreven in C ANSI (C99) om te maximaliseren draagbaarheid. Deze functie stelt de bibliotheken geschikt bewerkingen uitvoeren op meerdere platforms en apparaten, vooral wanneer het minimaliseren van schijf en geheugengebruik is een prioriteit.

Er zijn een breed scala aan platforms waarop de SDK is getest (Zie de [Azure gecertificeerd voor IoT-apparaat catalogus](https://catalog.azureiotsuite.com/) voor meer informatie). Hoewel dit artikel scenario's van voorbeeldcode uitgevoerd op het Windows-platform bevat, is de code die wordt beschreven in dit artikel is identiek in het bereik van de ondersteunde platforms.

In dit artikel maakt u kennis met de architectuur van het apparaat met Azure IoT SDK voor C. Dit laat zien hoe de apparaatbibliotheek initialiseren en berichten ontvangen gegevens verzenden naar IoT Hub. De informatie in dit artikel moet voldoende aan de slag met de SDK, maar bevat ook koppelingen naar aanvullende informatie over de bibliotheken.

## <a name="sdk-architecture"></a>SDK-architectuur

U vindt de [ **Azure IoT-device SDK voor C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats en de weergave details van de API in de [C-API-referentiemateriaal](https://azure.github.io/azure-iot-sdk-c/index.html).

De meest recente versie van de bibliotheken vindt u in de **master** branche van de opslagplaats:

  ![](media/iot-hub-device-sdk-c-intro/01-MasterBranch.PNG)

* De core-implementatie van de SDK is in de **iothub\_client** map met de implementatie van de onderste laag API in de SDK: de **IoTHubClient** bibliotheek. De **IoTHubClient** bibliotheek bevat API's implementeren onbewerkte berichten voor het verzenden van berichten naar IoT Hub en ontvangen van berichten uit IoT Hub. Wanneer u deze bibliotheek, bent u verantwoordelijk voor het implementeren van bericht serialisatie, maar andere details voor de communicatie met IoT Hub worden afgehandeld.
* De **serialisatiefunctie** map hulpfuncties en voorbeelden die laten hoe u gegevens serialiseren zien voordat naar Azure IoT Hub worden verzonden met behulp van de clientbibliotheek bevat. Het gebruik van de serializer is niet verplicht en uw gemak is opgegeven. Gebruik de **serialisatiefunctie** wanneer u een model waarin de gegevens worden verzonden naar IoT Hub en de berichten die u verwacht te ontvangen van deze bibliotheek, definiëren. Als het model is gedefinieerd, biedt de SDK u een API-gebied waarmee u eenvoudig werken met de apparaat-naar-cloud-en cloud-naar-apparaat zonder dat u de details van de serialisatie. De bibliotheek is afhankelijk van andere open-source bibliotheken die implementeren met behulp van protocollen zoals MQTT en AMQP transport.
* De **IoTHubClient** bibliotheek, is afhankelijk van andere open-source bibliotheken:
  * De [Azure C gedeeld hulpprogramma](https://github.com/Azure/azure-c-shared-utility) bibliotheek, waardoor algemene functionaliteit voor basistaken (zoals tekenreeksen, bewerkingen en i/o) over diverse Azure-gerelateerde C SDK's nodig.
  * De [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) bibliotheek die een client-side '-implementatie van AMQP die zijn geoptimaliseerd voor resource beperkte apparaten.
  * De [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) bibliotheek is een algemene bibliotheek implementatie van het protocol MQTT en geoptimaliseerd voor resource beperkte apparaten.

Gebruik van deze bibliotheken is gemakkelijker te begrijpen door te kijken voorbeeldcode. De volgende secties helpt u stapsgewijs door verschillende van de voorbeeldtoepassingen die zijn opgenomen in de SDK. In dit scenario moet bieden u een goed idee voor de verschillende mogelijkheden van de architectuur van de SDK en een inleiding tot de werking van de API's.

## <a name="before-you-run-the-samples"></a>Voordat u de controles uitvoeren

Voordat u de voorbeelden in de Azure IoT-device SDK voor C uitvoeren kunt, moet u [geen exemplaar maken van de service IoT Hub](iot-hub-create-through-portal.md) in uw Azure-abonnement. Voltooi de volgende taken:

* Uw ontwikkelomgeving voorbereiden
* Apparaat-referenties ophalen.

### <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

Pakketten zijn beschikbaar voor algemene platforms (zoals NuGet voor Windows of apt_get voor Debian en Ubuntu) en de voorbeelden gebruiken deze pakketten indien beschikbaar. In sommige gevallen moet u Compileer de SDK voor of op uw apparaat. Als u Compileer de SDK wilt, Zie [uw ontwikkelingsomgeving voorbereiden](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) in de GitHub-opslagplaats.

Als u de voorbeeldcode van de toepassing, een kopie van de SDK te downloaden vanuit GitHub. Een exemplaar van de bron van de **master** vertakking van de [GitHub-opslagplaats](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-the-device-credentials"></a>De referenties van het apparaat ophalen

Nu dat u de voorbeeldcode van de bron hebt, is het volgende wat te doen om op te halen van een set referenties apparaat. Voor een apparaat kunnen toegang krijgen tot een IoT-hub, moet u eerst het apparaat toevoegen aan de id-register van IoT Hub. Als u uw apparaat toevoegt, krijgt u een set referenties voor apparaten die u nodig hebt voor het apparaat verbinding kunnen maken met de IoT-hub. De voorbeeldtoepassingen die wordt besproken in de volgende sectie verwacht dat deze referenties in de vorm van een **apparaat verbindingsreeks**.

Er zijn verschillende open-source hulpprogramma's voor het beheren van uw IoT-hub.

* Een Windows-toepassing aangeroepen [apparaat explorer](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).
* Een platformoverschrijdende CLI van Python-hulpprogramma aangeroepen [de IoT-extensie voor Azure CLI 2.0](https://github.com/Azure/azure-iot-cli-extension).

Deze zelfstudie wordt gebruikgemaakt van de grafische *apparaat explorer* hulpprogramma. U kunt ook de *de IoT-extensie voor Azure CLI 2.0* hulpprogramma als u liever een CLI-hulpprogramma te gebruiken.

Het programma voor apparaat explorer gebruikt de bibliotheken van de service Azure IoT voor diverse functies uitvoeren op IoT Hub, inclusief het toevoegen van apparaten. Als u het apparaat explorer-hulpprogramma gebruiken voor het toevoegen van een apparaat, krijgt u een verbindingsreeks voor uw apparaat. U moet deze verbindingsreeks om uit te voeren van de voorbeeldtoepassingen.

Als u niet bekend met het programma voor apparaat explorer bent, geeft de volgende procedure wordt beschreven hoe gebruikt om een apparaat toevoegt en een apparaat-verbindingsreeks ophalen.

Zie het installeren van het programma voor apparaat explorer [de Explorer-apparaat gebruiken voor IoT Hub-apparaten](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

Wanneer u het programma uitvoert, ziet u deze interface:

  ![](media/iot-hub-device-sdk-c-intro/03-DeviceExplorer.PNG)

Voer uw **IoT Hub-verbindingsreeks** in het eerste veld en klikt u op **Update**. Deze stap configureert u het hulpprogramma zodat deze met IoT Hub communiceren kan.

Wanneer de IoT Hub-verbindingsreeks is geconfigureerd, klikt u op de **Management** tabblad:

  ![](media/iot-hub-device-sdk-c-intro/04-ManagementTab.PNG)

Dit tabblad is waar het beheren van de apparaten in uw IoT-hub geregistreerd.

U maakt een apparaat door te klikken op de **maken** knop. Er wordt een dialoogvenster weergegeven met een reeks vooraf ingestelde sleutels (primair en secundair). Voer een **apparaat-ID** en klik vervolgens op **maken**.

  ![](media/iot-hub-device-sdk-c-intro/05-CreateDevice.PNG)

Wanneer het apparaat is gemaakt, weergeven de apparaten updates met alle geregistreerde apparaten, inclusief de waarschuwing die u zojuist hebt gemaakt. Als u met de rechtermuisknop op het nieuwe apparaat, ziet u dit menu:

  ![](media/iot-hub-device-sdk-c-intro/06-RightClickDevice.PNG)

Als u ervoor kiest **Kopieer de verbindingsreeks voor geselecteerde apparaat**, de verbindingsreeks van het apparaat naar het Klembord is gekopieerd. Houd een kopie van de verbindingsreeks van het apparaat. U moet bij het uitvoeren van de voorbeeldtoepassingen die in de volgende secties beschreven.

Wanneer u de bovenstaande stappen hebt voltooid, bent u klaar om te beginnen met het uitvoeren van sommige code. Beide voorbeelden hebben een constante aan de bovenkant van de belangrijkste bron-bestand waarmee u een verbindingsreeks invoeren. Bijvoorbeeld, de bijbehorende regel van de **iothub\_client\_voorbeeld\_mqtt** toepassing wordt weergegeven als volgt.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-the-iothubclient-library"></a>Gebruik de IoTHubClient-bibliotheek

Binnen de **iothub\_client** map in de [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) -opslagplaats, er is een **voorbeelden** map waarin een toepassing met de naam **iothub\_client\_voorbeeld\_mqtt**.

De Windows-versie van de **iothub\_client\_voorbeeld\_mqtt** toepassing bevat de volgende Visual Studio-oplossing:

  ![](media/iot-hub-device-sdk-c-intro/12-iothub-client-sample-mqtt.PNG)

> [!NOTE]
> Als u dit project in Visual Studio 2017 opent, accepteert u de aanwijzingen voor het stelt u het project naar de nieuwste versie.

Deze oplossing bevat een project. Er zijn vier NuGet-pakketten in deze oplossing is geïnstalleerd:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Moet u altijd de **Microsoft.Azure.C.SharedUtility** wanneer u met de SDK werkt-pakket. Dit voorbeeld wordt de protocollen MQTT-protocol gebruikt, dus u moet bevatten de **Microsoft.Azure.umqtt** en **Microsoft.Azure.IoTHub.MqttTransport** pakketten (Er zijn equivalent pakketten voor AMQP- en HTTPS). Omdat in dit voorbeeld worden de **IoTHubClient** bibliotheek, moet u ook de **Microsoft.Azure.IoTHub.IoTHubClient** -pakket in uw oplossing.

U vindt de uitvoering voor de voorbeeldtoepassing in de **iothub\_client\_voorbeeld\_mqtt.c** bronbestand.

De volgende stappen gebruikt deze voorbeeldtoepassing u stapsgewijs door het wat nodig is om het gebruik van de **IoTHubClient** bibliotheek.

### <a name="initialize-the-library"></a>De bibliotheek initialiseren

> [!NOTE]
> Voordat u begint te werken met de bibliotheken, moet u wellicht initialisaties platform-specifieke uitvoeren. Als u wilt gebruiken AMQP op Linux moet u bijvoorbeeld de OpenSSL-bibliotheek initialiseren. De voorbeelden in de [GitHub-opslagplaats](https://github.com/Azure/azure-iot-sdk-c) Roep de hulpprogrammafunctie **platform\_init** wanneer de client wordt gestart en roep de **platform\_deinit**functie voordat u afsluit. Deze functies zijn gedefinieerd in het platform.h header-bestand. Bekijk de definities van deze functies voor uw doelplatform in de [opslagplaats](https://github.com/Azure/azure-iot-sdk-c) om te bepalen of u moet de van de platform-specifieke initialisatiecode opnemen in de client.

Als u wilt werken met de bibliotheken, moet u eerst een IoT Hub client ingang toewijzen:

```c
if ((iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

U doorgeven een kopie van de apparaat-verbindingsreeks die u hebt verkregen via het programma voor apparaat explorer aan deze functie. U ook opgeven de te gebruiken communicatieprotocol. In dit voorbeeld wordt MQTT maar AMQP- en HTTPS zijn ook opties.

Wanneer u hebt een geldige **IOTHUB\_CLIENT\_verwerken**, kunt u beginnen met het aanroepen van de API's om te verzenden en ontvangen van berichten naar en van IoT-Hub.

### <a name="send-messages"></a>Berichten verzenden

De voorbeeldtoepassing stelt een lus berichten verzenden naar uw IoT-hub. Het volgende codefragment:

- Hiermee maakt u een bericht.
- Voegt een eigenschap toe aan het bericht.
- Verzendt een bericht.

Maak eerst een bericht weergegeven:

```c
size_t iterator = 0;
do
{
    if (iterator < MESSAGE_COUNT)
    {
        sprintf_s(msgText, sizeof(msgText), "{\"deviceId\":\"myFirstDevice\",\"windSpeed\":%.2f}", avgWindSpeed + (rand() % 4 + 2));
        if ((messages[iterator].messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText))) == NULL)
        {
            (void)printf("ERROR: iotHubMessageHandle is NULL!\r\n");
        }
        else
        {
            messages[iterator].messageTrackingId = iterator;
            MAP_HANDLE propMap = IoTHubMessage_Properties(messages[iterator].messageHandle);
            (void)sprintf_s(propText, sizeof(propText), "PropMsg_%zu", iterator);
            if (Map_AddOrUpdate(propMap, "PropName", propText) != MAP_OK)
            {
                (void)printf("ERROR: Map_AddOrUpdate Failed!\r\n");
            }

            if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messages[iterator].messageHandle, SendConfirmationCallback, &messages[iterator]) != IOTHUB_CLIENT_OK)
            {
                (void)printf("ERROR: IoTHubClient_LL_SendEventAsync..........FAILED!\r\n");
            }
            else
            {
                (void)printf("IoTHubClient_LL_SendEventAsync accepted message [%d] for transmission to IoT Hub.\r\n", (int)iterator);
            }
        }
    }
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1);

    iterator++;
} while (g_continueRunning);
```

Telkens wanneer u een bericht verzendt, Geef een verwijzing naar een callbackfunctie die wordt aangeroepen wanneer de gegevens worden verzonden. In dit voorbeeld wordt de callbackfunctie wordt aangeroepen **SendConfirmationCallback**. Het volgende fragment toont deze callback-functie:

```c
static void SendConfirmationCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    EVENT_INSTANCE* eventInstance = (EVENT_INSTANCE*)userContextCallback;
    (void)printf("Confirmation[%d] received for message tracking id = %zu with result = %s\r\n", callbackCounter, eventInstance->messageTrackingId, ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
    /* Some device specific action code goes here... */
    callbackCounter++;
    IoTHubMessage_Destroy(eventInstance->messageHandle);
}
```

Let op de aanroep van de **IoTHubMessage\_vernietigen** wanneer u klaar bent met het bericht. Deze functie maakt tijdens het maken van het bericht toegewezen resources vrij.

### <a name="receive-messages"></a>Berichten ontvangen

Het bericht is een asynchrone bewerking. Ten eerste kunt u de callback om aan te roepen wanneer het apparaat een bericht ontvangt registreren:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext) != IOTHUB_CLIENT_OK)
{
    (void)printf("ERROR: IoTHubClient_LL_SetMessageCallback..........FAILED!\r\n");
}
else
{
    (void)printf("IoTHubClient_LL_SetMessageCallback...successful.\r\n");
...
```

De laatste parameter is een ongeldig aanwijzer naar elke gewenste. In het voorbeeld is een verwijzing naar een geheel getal, maar wordt een verwijzing naar een complexere gegevensstructuur. Deze parameter kunt de functie voor retouraanroepen bewerkingen uitvoeren op gedeelde staat met de aanroeper van deze functie.

Wanneer het apparaat een bericht ontvangt, wordt de geregistreerde callback-functie wordt aangeroepen. Deze functie voor retouraanroepen opgehaald:

* De bericht-id en de correlatie-id van het bericht.
* De inhoud van het bericht.
* Alle aangepaste eigenschappen van het bericht.

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    int* counter = (int*)userContextCallback;
    const char* buffer;
    size_t size;
    MAP_HANDLE mapProperties;
    const char* messageId;
    const char* correlationId;

    // Message properties
    if ((messageId = IoTHubMessage_GetMessageId(message)) == NULL)
    {
        messageId = "<null>";
    }

    if ((correlationId = IoTHubMessage_GetCorrelationId(message)) == NULL)
    {
        correlationId = "<null>";
    }

    // Message content
    if (IoTHubMessage_GetByteArray(message, (const unsigned char**)&buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        (void)printf("unable to retrieve the message data\r\n");
    }
    else
    {
        (void)printf("Received Message [%d]\r\n Message ID: %s\r\n Correlation ID: %s\r\n Data: <<<%.*s>>> & Size=%d\r\n", *counter, messageId, correlationId, (int)size, buffer, (int)size);
        // If we receive the work 'quit' then we stop running
        if (size == (strlen("quit") * sizeof(char)) && memcmp(buffer, "quit", size) == 0)
        {
            g_continueRunning = false;
        }
    }

    // Retrieve properties from the message
    mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                size_t index;

                printf(" Message Properties:\r\n");
                for (index = 0; index < propertyCount; index++)
                {
                    (void)printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                (void)printf("\r\n");
            }
        }
    }

    /* Some device specific action code goes here... */
    (*counter)++;
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Gebruik de **IoTHubMessage\_GetByteArray** functie voor het ophalen van het bericht dat in dit voorbeeld een tekenreeks is.

### <a name="uninitialize-the-library"></a>Initialisatie van de bibliotheek

Wanneer u gebeurtenissen verzenden en ontvangen berichten bent klaar, kunt u initialisatie van de IoT-bibliotheek. Voert de volgende functie-oproep om dit te doen:

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Deze aanroep is vrijgemaakt van de resources die eerder zijn toegewezen door de **IoTHubClient\_CreateFromConnectionString** functie.

Zoals u ziet, is het eenvoudig verzenden en ontvangen berichten met de **IoTHubClient** bibliotheek. De bibliotheek verwerkt de details van de communicatie met IoT Hub, met inbegrip van de te gebruiken protocol (vanuit het perspectief van de ontwikkelaar, is dit een eenvoudige configuratieoptie).

De **IoTHubClient** bibliotheek biedt ook nauwkeurige controle over hoe u voor het serialiseren van de gegevens die uw apparaat met IoT Hub verzendt. In sommige gevallen is dit niveau van controle een voordeel, maar er is een implementatie details die u niet wilt betrokken zijn bij. Als dit het geval is, kunt u met behulp van de **serialisatiefunctie** bibliotheek, die in de volgende sectie wordt beschreven.

## <a name="use-the-serializer-library"></a>Gebruik de serialisatiefunctie-bibliotheek

Conceptueel gezien de **serialisatiefunctie** bibliotheek bevindt zich boven de **IoTHubClient** bibliotheek in de SDK. Dit maakt gebruik van de **IoTHubClient** -bibliotheek voor de onderliggende communicatie met IoT Hub, maar het voegt modellering mogelijkheden die de last van omgaan met bericht serialisatie van de ontwikkelaar verwijderen. Hoe deze bibliotheek werkt best wordt geïllustreerd door een voorbeeld.

In de **serialisatiefunctie** map in de [azure-iot-sdk-c-opslagplaats](https://github.com/Azure/azure-iot-sdk-c), is een **voorbeelden** map waarin een toepassing met de naam **simplesample\_mqtt**. De Windows-versie van dit voorbeeld bevat de volgende Visual Studio-oplossing:

  ![](media/iot-hub-device-sdk-c-intro/14-simplesample_mqtt.PNG)

> [!NOTE]
> Als u dit project in Visual Studio 2017 opent, accepteert u de aanwijzingen voor het stelt u het project naar de nieuwste versie.

Net als bij het vorige voorbeeld, bevat deze een enkele NuGet-pakketten:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

U kunt de meeste van deze pakketten in het vorige voorbeeld hebt gezien, maar **Microsoft.Azure.IoTHub.Serializer** is er nieuw. Dit pakket is vereist wanneer u de **serialisatiefunctie** bibliotheek.

U vindt de uitvoering van de voorbeeldtoepassing in de **simplesample\_mqtt.c** bestand.

De volgende secties vindt u via de belangrijkste onderdelen van dit voorbeeld.

### <a name="initialize-the-library"></a>De bibliotheek initialiseren

Aan de slag met de **serialisatiefunctie** bibliotheek, aanroepen van de initialisatie van de API's:

```c
if (serializer_init(NULL) != SERIALIZER_OK)
{
    (void)printf("Failed on serializer_init\r\n");
}
else
{
    IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle = IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol);
    srand((unsigned int)time(NULL));
    int avgWindSpeed = 10;

    if (iotHubClientHandle == NULL)
    {
        (void)printf("Failed on IoTHubClient_LL_Create\r\n");
    }
    else
    {
        ContosoAnemometer* myWeather = CREATE_MODEL_INSTANCE(WeatherStation, ContosoAnemometer);
        if (myWeather == NULL)
        {
            (void)printf("Failed on CREATE_MODEL_INSTANCE\r\n");
        }
        else
        {
...
```

De aanroep van de **serialisatiefunctie\_init** functie is een eenmalige aanroep en de onderliggende bibliotheek wordt geïnitialiseerd. Roep vervolgens de **IoTHubClient\_LLE\_CreateFromConnectionString** functie, die is de API hetzelfde als in de **IoTHubClient** voorbeeld. Deze aanroep Hiermee stelt u de verbindingsreeks van uw apparaat (deze aanroep is ook waar u het protocol kiest u wilt gebruiken). Dit voorbeeld MQTT als het transport gebruikt, maar kan AMQP of HTTPS gebruiken.

Tenslotte roept de **maken\_MODEL\_exemplaar** functie. **WeatherStation** is de naamruimte van het model en **ContosoAnemometer** is de naam van het model. Zodra de model-exemplaar is gemaakt, kunt u het verzenden en ontvangen van berichten. Het is echter belangrijk om te begrijpen wat een model is.

### <a name="define-the-model"></a>Het model definiëren

Een model in de **serialisatiefunctie** bibliotheek definieert de berichten die uw apparaat met IoT Hub verzenden kunt en de berichten, zogenaamde *acties* in de modelleertaal die het kan ontvangen. U definieert een model met behulp van een set van C macro's als in de **simplesample\_mqtt** voorbeeldtoepassing:

```c
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(int, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

De **BEGINT\_NAAMRUIMTE** en **END\_NAAMRUIMTE** macro's zowel de naamruimte van het model als een argument nemen. Verwacht wordt dat alles tussen deze macro's is de definitie van het model of modellen en de gegevensstructuren die de modellen gebruiken.

In dit voorbeeld is een enkelvoudig model aangeroepen **ContosoAnemometer**. Dit model definieert twee soorten gegevens waarmee u uw apparaat met IoT Hub verzenden kunt: **DeviceId** en **windsnelheid**. Definieert ook drie acties (berichten) die het apparaat kan ontvangen: **TurnFanOn**, **TurnFanOff**, en **SetAirResistance**. Elk gegevenselement in de heeft een type en elke actie heeft een naam (en desgewenst een set parameters).

De gegevens en acties die zijn gedefinieerd in het model definiëren een API-gebied die u kunt gebruiken voor het verzenden van berichten naar IoT Hub en reageren op berichten die naar het apparaat verzonden. Door een voorbeeld van het gebruik van dit model best begrijpen.

### <a name="send-messages"></a>Berichten verzenden

Het model definieert de gegevens die u met IoT Hub verzenden kunt. In dit voorbeeld dat betekent een van de twee gegevensitems gedefinieerd met behulp van de **WITH_DATA** macro. Er zijn verschillende stappen nodig om te verzenden **DeviceId** en **windsnelheid** waarden naar een iothub. De eerste is het instellen van de gegevens die u wilt verzenden:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

Het model dat u eerder hebt gedefinieerd, kunt u de waarden instellen door leden van een **struct**. Vervolgens serialiseren het bericht dat u wilt verzenden:

```c
unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, myWeather->DeviceId, myWeather->WindSpeed) != CODEFIRST_OK)
{
    (void)printf("Failed to serialize\r\n");
}
else
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
    free(destination);
}
```

Deze code de apparaat-naar-cloud naar een buffer serialiseert (waarnaar wordt verwezen door **bestemming**). De code roept vervolgens de **sendMessage** functie het bericht te verzenden naar IoT Hub:

```c
static void sendMessage(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle == NULL)
    {
        printf("unable to create a new IoTHubMessage\r\n");
    }
    else
    {
        if (IoTHubClient_LL_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId) != IOTHUB_CLIENT_OK)
        {
            printf("failed to hand over the message to IoTHubClient");
        }
        else
        {
            printf("IoTHubClient accepted the message for delivery\r\n");
        }
        IoTHubMessage_Destroy(messageHandle);
    }
    messageTrackingId++;
}
```


De tweede op de laatste parameter van **IoTHubClient\_LLE\_SendEventAsync** is een verwijzing naar een callback-functie die wordt aangeroepen wanneer de gegevens is verzonden. Hier volgt de callback-functie in de steekproef:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

De tweede parameter is een aanwijzer naar gebruikerscontext; de aanwijzer dezelfde doorgegeven aan **IoTHubClient\_LLE\_SendEventAsync**. In dit geval de context is een eenvoudige Prestatiemeter gebruiken, maar dit alles kan zijn.

Dit is alles wat er is naar het verzenden van apparaat-naar-cloud-berichten. Het enige dat links ten aanzien van is berichten ontvangen.

### <a name="receive-messages"></a>Berichten ontvangen

Ontvangen een bericht werkt op dezelfde manier naar de berichten manier werken in de **IoTHubClient** bibliotheek. U registreert eerst een callback-functie van het bericht:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
{
    printf("unable to IoTHubClient_SetMessageCallback\r\n");
}
else
{
...
```

U schrijft vervolgens de callbackfunctie die wordt aangeroepen wanneer een bericht wordt ontvangen:

```c
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = IOTHUBMESSAGE_ABANDONED;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = IOTHUBMESSAGE_ABANDONED;
        }
        else
        {
            (void)memcpy(temp, buffer, size);
            temp[size] = '\0';
            EXECUTE_COMMAND_RESULT executeCommandResult = EXECUTE_COMMAND(userContextCallback, temp);
            result =
                (executeCommandResult == EXECUTE_COMMAND_ERROR) ? IOTHUBMESSAGE_ABANDONED :
                (executeCommandResult == EXECUTE_COMMAND_SUCCESS) ? IOTHUBMESSAGE_ACCEPTED :
                IOTHUBMESSAGE_REJECTED;
            free(temp);
        }
    }
    return result;
}
```

Deze code is standaardtekst--hetzelfde is voor een oplossing. Deze functie het bericht ontvangt, en zorgt voor de routering naar de betreffende functie via de aanroep van de het **EXECUTE\_opdracht**. De aangeroepen functie is op dit moment hangt af van de definitie van de acties in uw model.

Wanneer u een actie in het model definieert, bent u verplicht voor het implementeren van een functie die wordt aangeroepen wanneer het apparaat het bijbehorende bericht ontvangt. Bijvoorbeeld, als het model is gedefinieerd met deze actie:

```c
WITH_ACTION(SetAirResistance, int, Position)
```

Een functie met deze handtekening definiëren:

```c
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

Houd er rekening mee hoe de naam van de functie overeenkomt met de naam van de actie in het model en dat de parameters van de functie overeenkomen met de opgegeven parameters voor de actie. De eerste parameter is altijd vereist en bevat een verwijzing naar het exemplaar van het model.

Wanneer het apparaat een bericht dat overeenkomt met deze handtekening ontvangt, wordt de overeenkomende functie is aangeroepen. Daarom in combinatie met de code van de standaardtekst opnemen **IoTHubMessage**, ontvangen van berichten is slechts een kwestie van het definiëren van een eenvoudige functie voor elke actie in het model is gedefinieerd.

### <a name="uninitialize-the-library"></a>Initialisatie van de bibliotheek

Wanneer u klaar bent met het verzenden van gegevens en berichten ontvangt kunt u initialisatie van de IoT-bibliotheek:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Elk van deze drie functies wordt uitgelijnd met de drie initialisatie van de functies die eerder zijn beschreven. Het aanroepen van deze API's zorgt ervoor dat u eerder toegewezen resources vrij.

## <a name="next-steps"></a>Volgende stappen

In dit artikel de basisbeginselen van het gebruik van de bibliotheken in waarvoor de **Azure IoT-device SDK voor C**. Dit u geleverd met voldoende gegevens om te begrijpen wat opgenomen in de SDK, de architectuur en hoe u aan de slag met de Windows-voorbeelden. Het volgende artikel blijft de beschrijving van de SDK door uitleg over [meer informatie over de bibliotheek IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

Zie voor meer informatie over het ontwikkelen voor IoT Hub, de [Azure IoT SDK's][lnk-sdks].

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op Edge-apparaten met Azure IoT Edge][lnk-iotedge]

[lnk-file upload]: iot-hub-csharp-csharp-file-upload.md
[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
