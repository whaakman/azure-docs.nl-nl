---
title: De Azure IoT device-SDK voor C | Microsoft Docs
description: Aan de slag met Azure IoT device-SDK voor C en informatie over het maken van apps op apparaten die communiceren met een IoT-hub.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 08/25/2017
ms.author: yizhon
ms.openlocfilehash: d1d7c5df1b49a1f8c2fe4fbae4d8c8fdbd481e0e
ms.sourcegitcommit: d61faf71620a6a55dda014a665155f2a5dcd3fa2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/04/2019
ms.locfileid: "54053905"
---
# <a name="azure-iot-device-sdk-for-c"></a>Azure IoT device-SDK voor C

De **Azure IoT device-SDK** is een set bibliotheken die zijn ontworpen voor het vereenvoudigen van het proces van het verzenden van berichten naar en ontvangen van berichten uit de **Azure IoT Hub** service. Er zijn verschillende variaties van de SDK, elke die gericht is op een specifiek platform, maar in dit artikel beschrijft de **Azure IoT device-SDK voor C**.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

De Azure IoT device-SDK voor C is geschreven in C ANSI (C99) voor een maximale portabiliteit. Deze functie kunt u de bibliotheken geschikt zijn voor het werken met meerdere platforms en apparaten, met name wanneer de schijf worden geminimaliseerd en geheugengebruik is een prioriteit.

Er zijn een grote verscheidenheid aan platforms waarop de SDK is getest (Zie de [Azure Certified voor IoT-apparaatcatalogus](https://catalog.azureiotsuite.com/) voor meer informatie). Hoewel dit artikel overzichten van de voorbeeldcode die wordt uitgevoerd op het Windows-platform bevat, is de code die wordt beschreven in dit artikel is vrijwel identiek in het bereik van de ondersteunde platforms.

De volgende video biedt een overzicht van de Azure IoT SDK voor C:

>[!VIDEO https://channel9.msdn.com/Shows/Internet-of-Things-Show/Azure-IoT-C-SDK-insights/Player]

Dit artikel vindt u de architectuur van de Azure IoT-device-SDK voor c Hierin wordt beschreven hoe u de apparaatbibliotheek initialiseren, gegevens verzenden naar IoT Hub en berichten ontvangen van het. De informatie in dit artikel moet voldoende zijn om aan de slag met de SDK, maar biedt ook verwijzingen naar aanvullende informatie over de bibliotheken.

## <a name="sdk-architecture"></a>SDK-architectuur

U vindt de [ **Azure IoT device-SDK voor C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats en de weergave details van de API in de [C-API-verwijzing](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

De meest recente versie van de bibliotheken vindt u de **master** vertakking van de opslagplaats:

  ![Schermafbeelding van de master-vertakking van de opslagplaats](./media/iot-hub-device-sdk-c-intro/RepoMasterBranch.png)

* De core-implementatie van de SDK is in de **iothub\_client** map met de implementatie van de API-laag in de SDK: de **IoTHubClient** bibliotheek. De **IoTHubClient** bibliotheek bevat API's implementeren onbewerkte berichten voor het verzenden van berichten naar IoT Hub en ontvangen van berichten uit IoT Hub. Wanneer u deze bibliotheek, u bent verantwoordelijk voor het implementeren van bericht serialisatie, maar andere gegevens om te communiceren met IoT Hub worden verwerkt voor u.

* De **serializer** map bevat ondersteunende functies en voorbeelden die laten hoe u gegevens serialiseren zien voor verzenden voor Azure IoT Hub met behulp van de clientbibliotheek. Het gebruik van de serializer is niet verplicht en uw gemak wordt geleverd. Gebruik de **serializer** bibliotheek, definieert u een model dat Hiermee geeft u de gegevens worden verzonden naar IoT Hub en de berichten die u verwacht te ontvangen van deze. Als het model is gedefinieerd, wordt u door de SDK biedt met een API-gebied waarmee u eenvoudig kunt werken met de apparaat-naar-cloud-en cloud-naar-apparaat zonder dat u de details van de serialisatie. De bibliotheek is afhankelijk van andere open source-bibliotheken die implementeren met behulp van protocollen zoals MQTT naar AMQP transport.

* De **IoTHubClient** bibliotheek, is afhankelijk van andere open source-bibliotheken:

  * De [Azure C gedeeld hulpprogramma](https://github.com/Azure/azure-c-shared-utility) bibliotheek waarmee u algemene functionaliteit voor eenvoudige taken (zoals tekenreeksen, lijst manipulatie en i/o-) die nodig zijn voor verschillende Azure-gerelateerde C-SDK's.

  * De [Azure uAMQP](https://github.com/Azure/azure-uamqp-c) library, die een client-side '-implementatie van AMQP geoptimaliseerd voor apparaten met beperkte resource is.

  * De [Azure uMQTT](https://github.com/Azure/azure-umqtt-c) library, die is een implementatie van het MQTT-protocol voor algemeen gebruik-bibliotheek en geoptimaliseerd voor apparaten met beperkte resource.

Gebruik van deze bibliotheken is gemakkelijker te begrijpen door te kijken voorbeeldcode. De volgende secties helpen u bij meerdere van de voorbeeldtoepassingen die zijn opgenomen in de SDK. In dit scenario moet bieden u een goed idee voor de verschillende mogelijkheden van de architectuur lagen van de SDK en een inleiding tot de werking van de API's.

## <a name="before-you-run-the-samples"></a>Voordat u de voorbeelden uitvoeren

Voordat u de voorbeelden in de Azure IoT-device-SDK voor C uitvoeren kunt, moet u [maken van een exemplaar van de IoT Hub-service](iot-hub-create-through-portal.md) in uw Azure-abonnement. Voltooi de volgende taken:

* Uw ontwikkelomgeving voorbereiden
* Apparaatreferenties ophalen.

### <a name="prepare-your-development-environment"></a>Uw ontwikkelomgeving voorbereiden

Pakketten zijn beschikbaar voor voorkomende platformen (zoals NuGet voor Windows of apt_get voor Debian en Ubuntu) en de voorbeelden gebruiken deze pakketten indien beschikbaar. In sommige gevallen moet u voor het compileren van de SDK voor of op uw apparaat. Als u nodig hebt voor het compileren van de SDK, Zie [uw ontwikkelomgeving voorbereiden](https://github.com/Azure/azure-iot-sdk-c/blob/master/doc/devbox_setup.md) in de GitHub-opslagplaats.

U de voorbeeldcode van de toepassing, een kopie van de SDK te downloaden vanuit GitHub. Uw kopie van de bron van de **master** vertakking van de [GitHub-opslagplaats](https://github.com/Azure/azure-iot-sdk-c).


### <a name="obtain-the-device-credentials"></a>Haal de apparaatreferenties op

Nu dat u de voorbeeld-broncode hebt, wordt het volgende wat te doen is een set apparaatreferenties ophalen. Voor het apparaat toegang krijgen tot een IoT-hub, moet u eerst het apparaat toevoegen aan de id-register van IoT Hub. Als u uw apparaat toevoegt, krijgt u een set referenties van het apparaat die u nodig hebt voor het apparaat te kunnen verbinding maken met de IoT-hub. De voorbeeldtoepassingen die worden beschreven in de volgende sectie verwacht dat deze referenties in de vorm van een **apparaatverbindingsreeks**.

Er zijn verschillende open-source-hulpprogramma's voor het beheren van uw IoT-hub.

* Een Windows-toepassing met de naam [apparatenverkenner](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

* Een platformoverschrijdende Visual Studio Code-extensie met de naam [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).

* Een Python platformoverschrijdende CLI met de naam [de IoT-extensie voor Azure CLI](https://github.com/Azure/azure-iot-cli-extension).

Deze zelfstudie wordt gebruikgemaakt van de grafische *apparatenverkenner* hulpprogramma. U kunt de *Azure IoT-hulpprogramma's voor VS Code* als u in VS Code ontwikkelt. U kunt ook de *de IoT-extensie voor Azure CLI 2.0* hulpprogramma als u liever een CLI-hulpprogramma te gebruiken.

Het hulpprogramma device explorer gebruikt de Azure IoT-service-bibliotheken voor het uitvoeren van verschillende functies op IoT-Hub, zoals het toevoegen van apparaten. Als u het hulpprogramma device explorer op een apparaat, krijgt u een verbindingsreeks voor uw apparaat. U hebt deze verbindingsreeks om uit te voeren van de voorbeeldtoepassingen nodig.

Als u niet bekend met het hulpprogramma device explorer bent, geeft de volgende procedure wordt beschreven hoe u deze gebruiken om een apparaat toevoegen en het verkrijgen van een apparaat-verbindingsreeks.

1. Zie voor het installeren van het hulpprogramma device explorer [over het gebruik van de Device Explorer voor IoT Hub-apparaten](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/tools/DeviceExplorer).

1. Als u het programma uitvoert, ziet u deze interface:

  ![Schermafbeelding van de dubbele Device Explorer](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinConfigTab.png)

1. Voer uw **IoT Hub-verbindingsreeks** in het eerste veld en klikt u op **Update**. Deze stap configureert u het hulpprogramma, zodat deze met IoT Hub communiceren kan. 

De **Connection String** kunt u vinden onder **IoT Hub-Service** > **instellingen** > **beleid voor gedeelde toegang**  >  **iothubowner**.

1. Wanneer de IoT Hub-verbindingsreeks is geconfigureerd, klikt u op de **Management** tabblad:

  ![Device Explorer dubbele / Management-schermafbeelding](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinManagementTab.png)

Dit tabblad is waar u de apparaten die zijn geregistreerd in uw IoT-hub beheren.

1. U maakt een apparaat door te klikken op de **maken** knop. Er wordt een dialoogvenster weergegeven met een reeks vooraf gevulde sleutels (primaire en secundaire). Voer een **apparaat-ID** en klik vervolgens op **maken**.

  ![Schermafbeelding van het apparaat maken](./media/iot-hub-device-sdk-c-intro/CreateDevice.png)

1. Wanneer het apparaat wordt gemaakt, wordt er door de apparaten updates lijst met alle geregistreerde apparaten, inclusief de functie die u zojuist hebt gemaakt. Als u met de rechtermuisknop op het nieuwe apparaat, ziet u dit menu:

  ![Device Explorer dubbele met de rechtermuisknop op resultaat](./media/iot-hub-device-sdk-c-intro/DeviceExplorerTwinManagementTab_RightClick.png)

1. Als u ervoor kiest **Kopieer de verbindingsreeks voor het geselecteerde apparaat**, de verbindingsreeks naar het Klembord is gekopieerd. Bewaar een kopie van de verbindingsreeks van het apparaat. U hebt deze nodig bij het uitvoeren van de voorbeeldtoepassingen die worden beschreven in de volgende secties.

Wanneer u de bovenstaande stappen hebt voltooid, bent u klaar om te beginnen met het uitvoeren van code. De meeste voorbeelden hebben een constante aan de bovenkant van de belangrijkste bron-bestand waarmee u een verbindingsreeks invoeren. Bijvoorbeeld, de bijbehorende regel van de **iothub\_client\_voorbeeld\_mqtt** toepassing als volgt wordt weergegeven.

```c
static const char* connectionString = "[device connection string]";
```

## <a name="use-the-iothubclient-library"></a>De bibliotheek IoTHubClient gebruiken

Binnen de **iothub\_client** map in de [azure-iot-sdk-c](https://github.com/azure/azure-iot-sdk-c) opslagplaats, er is een **voorbeelden** map met een toepassing met de naam **iothub\_client\_voorbeeld\_mqtt**.

De Windows-versie van de **iothub\_client\_voorbeeld\_mqtt** toepassing bevat de volgende Visual Studio-oplossing:

  ![Visual Studio Solution Explorer](./media/iot-hub-device-sdk-c-intro/iothub-client-sample-mqtt.png)

> [!NOTE]
> Als u dit project in Visual Studio 2017 openen, accepteert u de aanwijzingen voor het stelt u het project naar de nieuwste versie.

Deze oplossing bevat een enkel project. Er zijn vier NuGet-pakketten geïnstalleerd in deze oplossing:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.umqtt

Moet u altijd de **Microsoft.Azure.C.SharedUtility** pakket wanneer u met de SDK werkt. In dit voorbeeld wordt het MQTT-protocol, dus u moet bevatten de **Microsoft.Azure.umqtt** en **Microsoft.Azure.IoTHub.MqttTransport** pakketten (Er zijn gelijkwaardig pakketten voor AMQP- en HTTPS). Omdat in het voorbeeld wordt de **IoTHubClient** bibliotheek, moet u ook de **Microsoft.Azure.IoTHub.IoTHubClient** -pakket in uw oplossing.

U vindt de implementatie voor de voorbeeldtoepassing in de **iothub\_client\_voorbeeld\_mqtt.c** bronbestand.

De volgende stappen gebruikt deze voorbeeldtoepassing om te zien hoe u wat is vereist voor het gebruik van de **IoTHubClient** bibliotheek.

### <a name="initialize-the-library"></a>Initialiseren van de bibliotheek

> [!NOTE]
> Voordat u begint te werken met de bibliotheken, moet u mogelijk initialisaties platform-specifieke uitvoeren. Als u van plan bent AMQP in Linux gebruiken moet u bijvoorbeeld de OpenSSL-bibliotheek initialiseren. De voorbeelden in de [GitHub-opslagplaats](https://github.com/Azure/azure-iot-sdk-c) Roep de hulpprogrammafunctie **platform\_init** wanneer de client wordt gestart en roep de **platform\_deinit**functie voordat u afsluit. Deze functies zijn gedefinieerd in de platform.h header-bestand. Bekijk de definities van deze functies voor uw doelplatform in de [opslagplaats](https://github.com/Azure/azure-iot-sdk-c) om te bepalen of u moet de van de platform-specifieke initialisatiecode opnemen in uw client.

Als u wilt werken met de bibliotheken, moet u eerst een IoT Hub-client-ingang toewijzen:

```c
if ((iotHubClientHandle = 
  IoTHubClient_LL_CreateFromConnectionString(connectionString, MQTT_Protocol)) == NULL)
{
    (void)printf("ERROR: iotHubClientHandle is NULL!\r\n");
}
else
{
    ...
```

U kunt doorgeven een kopie van de apparaatverbindingsreeks die u hebt verkregen via het hulpprogramma device explorer op deze functie. U wijst ook de te gebruiken communicatieprotocol. In dit voorbeeld maakt gebruik van MQTT, maar AMQP- en HTTPS zijn ook opties.

Wanneer u hebt een geldig **IOTHUB\_CLIENT\_verwerken**, u kunt beginnen met het aanroepen van de API's om te verzenden en ontvangen van berichten naar en van IoT-Hub.

### <a name="send-messages"></a>Berichten verzenden

De voorbeeldtoepassing stelt u een lus berichten te verzenden naar uw IoT-hub. Het volgende codefragment:

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

Telkens wanneer u een bericht stuurt, geeft u een verwijzing naar een retouraanroepfunctie die wordt aangeroepen wanneer de gegevens worden verzonden. In dit voorbeeld de callbackfunctie is aangeroepen **SendConfirmationCallback**. Het volgende fragment toont deze retouraanroepfunctie:

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

Houd er rekening mee de aanroep van de **IoTHubMessage\_vernietigen** werken wanneer u klaar bent met het bericht. Deze functie kan de resources toegewezen tijdens het maken van het bericht.

### <a name="receive-messages"></a>Berichten ontvangen

Ontvangen van een bericht is een asynchrone bewerking. Registreer eerst de callback om aan te roepen wanneer het apparaat een bericht weergegeven ontvangt:

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

De laatste parameter is een ongeldig aanwijzer naar wat u wilt. Er is een aanwijzer naar een geheel getal zijn, maar wordt een verwijzing naar een meer complexe gegevensstructuur in het voorbeeld. Deze parameter zorgt ervoor dat de callback-functie om te worden uitgevoerd op gedeelde staat met de aanroeper van deze functie.

Wanneer het apparaat een bericht ontvangt, wordt de geregistreerde callback-functie aangeroepen. Deze callbackfunctie opgehaald:

* De bericht-id en de correlatie-id van het bericht.
* De inhoud van het bericht.
* Aangepaste eigenschappen van het bericht.

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

Gebruik de **IoTHubMessage\_GetByteArray** functie om op te halen van het bericht dat in dit voorbeeld een tekenreeks is.

### <a name="uninitialize-the-library"></a>Initialisatie van de bibliotheek

Wanneer u klaar bent met het verzenden van gebeurtenissen en die berichten ontvangt, kunt u initialisatie van de IoT-bibliotheek. Om dit te doen, voert u de volgende aanroep van de functie:

```c
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

Deze aanroep maakt u de resources die eerder zijn toegewezen door de **IoTHubClient\_CreateFromConnectionString** functie.

Zoals u ziet, is het eenvoudig te verzenden en ontvangen van berichten met de **IoTHubClient** bibliotheek. De bibliotheek verwerkt de details van de communicatie met IoT Hub, met inbegrip van welk protocol moet worden gebruikt (dit is vanuit het perspectief van de ontwikkelaar, een eenvoudige configuratie-optie).

De **IoTHubClient** bibliotheek bevat ook nauwkeurige controle over hoe u het serialiseren van de gegevens die uw apparaat naar IoT Hub verzendt. In sommige gevallen is dit niveau van controle een voordeel, maar in andere gevallen is een implementatiedetail in die u niet wilt worden betrokken. Als dit het geval is, kunt u overwegen met behulp van de **serializer** library, die in de volgende sectie wordt beschreven.

## <a name="use-the-serializer-library"></a>Gebruik de serializer-bibliotheek

Conceptueel gezien de **serializer** bibliotheek bevindt zich boven de **IoTHubClient** bibliotheek in de SDK. Hierbij de **IoTHubClient** -bibliotheek voor de onderliggende communicatie met IoT Hub, maar het voegt mogelijkheden voor modellen die het ongemak van het omgaan met bericht serialisatie van de ontwikkelaar verwijderen. Hoe dit werkt bibliotheek aanbevolen wordt geïllustreerd door een voorbeeld.

In de **serializer** map in de [azure-iot-sdk-c-opslagplaats](https://github.com/Azure/azure-iot-sdk-c), is een **voorbeelden** map met een toepassing met de naam **simplesample\_mqtt**. De Windows-versie van dit voorbeeld bevat de volgende Visual Studio-oplossing:

  ![Visual Studio-oplossing voor het mqtt-voorbeeld](./media/iot-hub-device-sdk-c-intro/simplesample_mqtt.png)

> [!NOTE]
> Als u dit project in Visual Studio 2017 openen, accepteert u de aanwijzingen voor het stelt u het project naar de nieuwste versie.

Net als bij het vorige voorbeeld bevat deze een enkele NuGet-pakketten:

* Microsoft.Azure.C.SharedUtility
* Microsoft.Azure.IoTHub.MqttTransport
* Microsoft.Azure.IoTHub.IoTHubClient
* Microsoft.Azure.IoTHub.Serializer
* Microsoft.Azure.umqtt

U kunt de meeste van deze pakketten in het vorige voorbeeld hebt gezien, maar **Microsoft.Azure.IoTHub.Serializer** is er nieuw. Dit pakket is vereist wanneer u de **serializer** bibliotheek.

U vindt de implementatie van de voorbeeldtoepassing in de **simplesample\_mqtt.c** bestand.

De volgende secties helpen u via de belangrijkste onderdelen van dit voorbeeld.

### <a name="initialize-the-library"></a>Initialiseren van de bibliotheek

Aan de slag met de **serializer** bibliotheek, aanroepen van de initialisatie van de API's:

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

De aanroep van de **serializer\_init** functie is een eenmalige aanroep en de onderliggende bibliotheek wordt geïnitialiseerd. Roep vervolgens de **IoTHubClient\_LLE\_CreateFromConnectionString** functie, dit is dezelfde API als in de **IoTHubClient** voorbeeld. Deze aanroep Hiermee stelt u de verbindingsreeks van uw apparaat (deze aanroep is ook waar u het protocol kiest u wilt gebruiken). In dit voorbeeld maakt gebruik van MQTT als het transport, maar kan gebruikmaken van AMQP of HTTPS.

Roep ten slotte de **maken\_MODEL\_exemplaar** functie. **WeatherStation** is de naamruimte van het model en **ContosoAnemometer** is de naam van het model. Zodra de model-exemplaar is gemaakt, kunt u deze kunt gebruiken om te beginnen met het verzenden en ontvangen van berichten. Het is echter belangrijk te begrijpen wat een model is.

### <a name="define-the-model"></a>Het model definiëren

Een model in de **serializer** bibliotheek definieert de berichten die uw apparaat naar IoT Hub verzenden kunt en de berichten, zogenaamde *acties* in de modelleertaal, die kan ontvangen. U definieert een model met behulp van een set van C-macro's als in de **simplesample\_mqtt** voorbeeldtoepassing:

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

De **beginnen\_NAAMRUIMTE** en **END\_NAAMRUIMTE** macro's beide duren voordat de naamruimte van het model als een argument. Verwacht wordt dat alles tussen deze macro's is de definitie van het model of modellen en de gegevensstructuren die gebruikmaken van de modellen.

In dit voorbeeld wordt één model met de naam **ContosoAnemometer**. Dit model definieert twee soorten gegevens die uw apparaat naar IoT Hub verzenden kunt: **Apparaat-id** en **windsnelheid**. Het definieert ook drie acties (berichten) die uw apparaat kan ontvangen: **TurnFanOn**, **TurnFanOff**, en **SetAirResistance**. Elk gegevenselement heeft een type en elke actie heeft een naam (en optioneel een set parameters).

De gegevens en acties die zijn gedefinieerd in het model definiëren van een API-gebied die u kunt gebruiken om berichten te verzenden naar IoT Hub en reageren op berichten die naar het apparaat verzonden. Door een voorbeeld van het gebruik van dit model best begrijpen.

### <a name="send-messages"></a>Berichten verzenden

Het model definieert de gegevens die u naar IoT Hub verzenden kunt. In dit voorbeeld, houdt dat in dat een van de twee items gedefinieerd met behulp van de **WITH_DATA** macro. Er zijn verschillende stappen die nodig zijn voor het verzenden van **DeviceId** en **windsnelheid** waarden naar een IoT hub. De eerste is het instellen van de gegevens die u wilt verzenden:

```c
myWeather->DeviceId = "myFirstDevice";
myWeather->WindSpeed = avgWindSpeed + (rand() % 4 + 2);
```

Het model dat u eerder hebt gedefinieerd, kunt u de waarden instellen door in te stellen van de leden van een **struct**. Vervolgens serialiseren van het bericht dat u wilt verzenden:

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

Deze code serialiseert de apparaat-naar-cloud naar een buffer (waarnaar wordt verwezen door **bestemming**). De code roept vervolgens de **sendMessage** functie voor het verzenden van het bericht naar IoT Hub:

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

De tweede naar de laatste parameter van **IoTHubClient\_LLE\_SendEventAsync** is een verwijzing naar een callbackfunctie die wordt aangeroepen wanneer de gegevens wordt verzonden. Dit is de callback-functie in het voorbeeld:

```c
void sendCallback(IOTHUB_CLIENT_CONFIRMATION_RESULT result, void* userContextCallback)
{
    unsigned int messageTrackingId = (unsigned int)(uintptr_t)userContextCallback;

    (void)printf("Message Id: %u Received.\r\n", messageTrackingId);

    (void)printf("Result Call Back Called! Result is: %s \r\n", ENUM_TO_STRING(IOTHUB_CLIENT_CONFIRMATION_RESULT, result));
}
```

De tweede parameter is een verwijzing naar de context van de gebruiker; de dezelfde wijzer doorgegeven aan **IoTHubClient\_LLE\_SendEventAsync**. In dit geval wordt de context is een eenvoudige Prestatiemeter, maar mag elke gewenste.

Dat is alles er is met het verzenden van apparaat-naar-cloud-berichten. Het enige wat nodespecid dekking is berichten ontvangen.

### <a name="receive-messages"></a>Berichten ontvangen

Ontvangen een bericht werkt op dezelfde manier naar de berichten manier werken in de **IoTHubClient** bibliotheek. Registreer eerst een retouraanroepfunctie bericht:

```c
if (IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, 
  IoTHubMessage, myWeather) != IOTHUB_CLIENT_OK)
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

Deze code wordt de standaardtekst--dit is hetzelfde voor elke oplossing. Deze functie ontvangt het bericht en zorgt dat deze de juiste werkt alleen in de aanroep van routering **EXECUTE\_opdracht**. De aangeroepen functie is op dit moment afhankelijk van de definitie van de acties in uw model.

Wanneer u een actie in het model definieert, bent u vereist voor het implementeren van een functie die wordt aangeroepen wanneer het apparaat ontvangt vervolgens het bijbehorende bericht. Bijvoorbeeld, als uw model is gedefinieerd met deze actie:

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

Houd er rekening mee hoe de naam van de functie komt overeen met de naam van de actie in het model en dat de parameters van de functie overeenkomen met de opgegeven parameters voor de actie. De eerste parameter is altijd vereist en bevat een verwijzing naar het exemplaar van uw model.

Wanneer het apparaat een bericht dat overeenkomt met deze handtekening ontvangt, wordt de bijbehorende functie aangeroepen. Daarom, afgezien van dat u hoeft te bevatten de standaardcode van **IoTHubMessage**, ontvangen van berichten is alleen een kwestie van het definiëren van een eenvoudige functie voor elke actie die is gedefinieerd in uw model.

### <a name="uninitialize-the-library"></a>Initialisatie van de bibliotheek

Wanneer u klaar bent met het verzenden van gegevens en berichten ontvangt, kunt u initialisatie van de IoT-bibliotheek:

```c
...
        DESTROY_MODEL_INSTANCE(myWeather);
    }
    IoTHubClient_LL_Destroy(iotHubClientHandle);
}
serializer_deinit();
```

Elk van deze drie functies worden uitgelijnd met de drie initialisatie-functies die eerder zijn beschreven. Deze API's, zorgt u ervoor dat u eerder toegewezen bronnen vrij.

## <a name="next-steps"></a>Volgende stappen

In dit artikel de basisbeginselen van het gebruik van de bibliotheken in waarvoor het **Azure IoT device-SDK voor C**. Dit u geleverd met voldoende gegevens om te begrijpen wat inbegrepen bij de SDK, de architectuur en hoe u aan de slag met Windows-voorbeelden. Het volgende artikel blijft de beschrijving van de SDK door een uitleg te [meer informatie over de bibliotheek IoTHubClient](iot-hub-device-sdk-c-iothubclient.md).

Zie voor meer informatie over het ontwikkelen van IoT Hub, de [Azure IoT SDK's](iot-hub-devguide-sdks.md).

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md)
