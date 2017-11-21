---
title: Azure IoT-apparaat SDK voor C - serialisatiefunctie | Microsoft Docs
description: Het gebruik van de serialisatiefunctie-bibliotheek in de Azure-IoT-apparaat-SDK voor C apps voor apparaten die communiceren met een IoT-hub maken.
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: defbed34-de73-429c-8592-cd863a38e4dd
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 09/06/2016
ms.author: obloch
ms.openlocfilehash: d8b9e147b68d16c6c166e92cbabf5b5b63e23e8d
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IoT-apparaat SDK voor C: meer informatie over de serialisatiefunctie
De [eerst artikel](iot-hub-device-sdk-c-intro.md) in deze reeks geïntroduceerd de **Azure IoT-device SDK voor C**. Het volgende artikel opgegeven een gedetailleerdere beschrijving van de [ **IoTHubClient**](iot-hub-device-sdk-c-iothubclient.md). In dit artikel is voltooid dekking van de SDK door te geven van een gedetailleerdere beschrijving van de resterende component: de **serialisatiefunctie** bibliotheek.

De inleidende artikel wordt beschreven hoe u de **serialisatiefunctie** bibliotheek gebeurtenissen te verzenden en ontvangen van berichten uit IoT Hub. In dit artikel we die discussie uitbreiden door te geven van een uitgebreidere uitleg van het model van uw gegevens met de **serialisatiefunctie** macrotaal. Het artikel ook meer informatie over hoe de bibliotheek berichten serialiseert bevat (en in sommige gevallen hoe u kunt bepalen het gedrag voor serialisatie). We beschrijven ook bepaalde parameters die kunt u wijzigen om te bepalen van de grootte van de modellen die u maakt.

Ten slotte revisits het artikel sommige onderwerpen in de vorige artikelen zoals bericht en de verwerking van de eigenschap. Als we je uitzoeken deze functies werken op dezelfde manier met behulp van de **serialisatiefunctie** bibliotheek als met de **IoTHubClient** bibliotheek.

Alles dat wordt beschreven in dit artikel is gebaseerd op de **serialisatiefunctie** SDK-voorbeelden. Als u volgen wilt, raadpleegt u de **simplesample\_amqp** en **simplesample\_http** toepassingen die zijn opgenomen in de Azure IoT-device SDK voor C.

U vindt de [ **Azure IoT-device SDK voor C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats en de weergave details van de API in de [C-API-referentiemateriaal](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="the-modeling-language"></a>De modelleertaal
De [inleidende artikel](iot-hub-device-sdk-c-intro.md) in deze reeks geïntroduceerd de **Azure IoT-device SDK voor C** modelleertaal via het voorbeeld in de **simplesample\_amqp** toepassing:

```
BEGIN_NAMESPACE(WeatherStation);

DECLARE_MODEL(ContosoAnemometer,
WITH_DATA(ascii_char_ptr, DeviceId),
WITH_DATA(double, WindSpeed),
WITH_ACTION(TurnFanOn),
WITH_ACTION(TurnFanOff),
WITH_ACTION(SetAirResistance, int, Position)
);

END_NAMESPACE(WeatherStation);
```

Zoals u ziet, is de modelleertaal gebaseerd op C macro's. De definitie van de met beginnen altijd **BEGIN\_NAAMRUIMTE** en altijd eindigen met **END\_NAAMRUIMTE**. Het is gebruikelijk als naam voor de naamruimte voor uw bedrijf of, net zoals in dit voorbeeld wordt het project dat u werkt.

Wat komt in de naamruimte model definities zijn. In dit geval is er één model voor een anemometer. Nogmaals het model kunt naam geven, maar doorgaans deze voor het apparaat of een type gegevens dat u wilt uitwisselen met IoT Hub wordt genoemd.  

Modellen een definitie bevatten van de gebeurtenissen kunt u inkomend naar IoT Hub (de *gegevens*) en de berichten uit IoT Hub kunt u ontvangen (de *acties*). Als u in het voorbeeld zien kunt, hebben gebeurtenissen een type en een naam; acties hebben een naam en optionele parameters (elk met een type).

Wat wordt niet gedemonstreerd in dit voorbeeld zijn extra gegevenstypen die worden ondersteund door de SDK. Aan bod om het dialoogvenster.

> [!NOTE]
> IoT Hub verwijst naar de gegevens die een apparaat naar als verzendt *gebeurtenissen*, terwijl de modelleertaal naar als verwijst *gegevens* (gedefinieerd met behulp van **WITH_DATA**). Evenzo IoT Hub verwijst naar de gegevens die u kunt naar apparaten als verzenden *berichten*, terwijl de modelleertaal naar als verwijst *acties* (gedefinieerd met behulp van **WITH_ACTION**). Let erop dat deze voorwaarden door elkaar kunnen worden gebruikt in dit artikel.
> 
> 

### <a name="supported-data-types"></a>Ondersteunde gegevenstypen
De volgende gegevenstypen worden ondersteund in modellen die zijn gemaakt met de **serialisatiefunctie** bibliotheek:

| Type | Beschrijving |
| --- | --- |
| dubbele |dubbele precisiegetal met drijvende komma |
| int |32-bits geheel getal |
| Float |drijvende-kommagetal met enkele precisie |
| lang |lang geheel getal |
| int8\_t |8-bits geheel getal |
| Int16\_t |16-bits geheel getal |
| Int32\_t |32-bits geheel getal |
| Int64\_t |64-bits geheel getal |
| BOOL |Booleaanse waarde |
| ASCII\_char\_ptr |ASCII-tekenreeks |
| EDM\_DATUM\_TIJD\_OFFSET |datum tijdverschil |
| EDM\_GUID |GUID |
| EDM\_BINAIRE |Binaire |
| DECLAREREN\_STRUCT |complex gegevenstype |

Laten we beginnen met het laatste gegevenstype. De **DECLARE\_STRUCT** kunt u definiëren van complexe gegevenstypen, dat wil zeggen groepen van de andere primitieve typen. Deze groeperingen kunnen we voor het definiëren van een model dat er als volgt uit:

```
DECLARE_STRUCT(TestType,
double, aDouble,
int, aInt,
float, aFloat,
long, aLong,
int8_t, aInt8,
uint8_t, auInt8,
int16_t, aInt16,
int32_t, aInt32,
int64_t, aInt64,
bool, aBool,
ascii_char_ptr, aAsciiCharPtr,
EDM_DATE_TIME_OFFSET, aDateTimeOffset,
EDM_GUID, aGuid,
EDM_BINARY, aBinary
);

DECLARE_MODEL(TestModel,
WITH_DATA(TestType, Test)
);
```

Het model bevat een één-gebeurtenis van het type **TestType**. **TestType** is een complex type met verschillende leden die gezamenlijk primitieve typen ondersteund aantonen door de **serialisatiefunctie** modelleertaal.

Met een model als dit schrijven we code om gegevens te verzenden met IoT-Hub die wordt weergegeven als volgt:

```
TestModel* testModel = CREATE_MODEL_INSTANCE(MyThermostat, TestModel);

testModel->Test.aDouble = 1.1;
testModel->Test.aInt = 2;
testModel->Test.aFloat = 3.0f;
testModel->Test.aLong = 4;
testModel->Test.aInt8 = 5;
testModel->Test.auInt8 = 6;
testModel->Test.aInt16 = 7;
testModel->Test.aInt32 = 8;
testModel->Test.aInt64 = 9;
testModel->Test.aBool = true;
testModel->Test.aAsciiCharPtr = "ascii string 1";

time_t now;
time(&now);
testModel->Test.aDateTimeOffset = GetDateTimeOffset(now);

EDM_GUID guid = { { 0x00, 0x01, 0x02, 0x03, 0x04, 0x05, 0x06, 0x07, 0x08, 0x09, 0x0A, 0x0B, 0x0C, 0x0D, 0x0E, 0x0F } };
testModel->Test.aGuid = guid;

unsigned char binaryArray[3] = { 0x01, 0x02, 0x03 };
EDM_BINARY binaryData = { sizeof(binaryArray), &binaryArray };
testModel->Test.aBinary = binaryData;

SendAsync(iotHubClientHandle, (const void*)&(testModel->Test));
```

In principe we een waarde toewijst aan elk lid van de **Test** structuur en vervolgens aan te roepen **SendAsync** verzenden de **Test** gebeurtenis van de gegevens naar de cloud. **SendAsync** is een helperfunctie die een één-gebeurtenis naar IoT Hub verzendt:

```
void SendAsync(IOTHUB_CLIENT_LL_HANDLE iotHubClientHandle, const void *dataEvent)
{
    unsigned char* destination;
    size_t destinationSize;
    if (SERIALIZE(&destination, &destinationSize, *(const unsigned char*)dataEvent) ==
    {
        // null terminate the string
        char* destinationAsString = (char*)malloc(destinationSize + 1);
        if (destinationAsString != NULL)
        {
            memcpy(destinationAsString, destination, destinationSize);
            destinationAsString[destinationSize] = '\0';
            IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromString(destinationAsString);
            if (messageHandle != NULL)
            {
                IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)0);

                IoTHubMessage_Destroy(messageHandle);
            }
            free(destinationAsString);
        }
        free(destination);
    }
}
```

Deze functie de gegeven gebeurtenis serialiseert en verzendt het naar IoT Hub met **IoTHubClient\_SendEventAsync**. Dit is de dezelfde code die wordt besproken in de vorige artikelen (**SendAsync** kapselt de logica in een handige functie).

Een andere hulpfunctie gebruikt in de vorige code is **GetDateTimeOffset**. Deze functie worden de opgegeven tijd omgezet in een waarde van het type **EDM\_datum\_tijd\_OFFSET**:

```
EDM_DATE_TIME_OFFSET GetDateTimeOffset(time_t time)
{
    struct tm newTime;
    gmtime_s(&newTime, &time);
    EDM_DATE_TIME_OFFSET dateTimeOffset;
    dateTimeOffset.dateTime = newTime;
    dateTimeOffset.fractionalSecond = 0;
    dateTimeOffset.hasFractionalSecond = 0;
    dateTimeOffset.hasTimeZone = 0;
    dateTimeOffset.timeZoneHour = 0;
    dateTimeOffset.timeZoneMinute = 0;
    return dateTimeOffset;
}
```

Als u deze code uitvoert, wordt het volgende bericht verzonden naar IoT Hub:

```
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Houd er rekening mee dat de serialisatie JSON die de indeling die is gegenereerd door de **serialisatiefunctie** bibliotheek. Let ook op dat elk lid van het geserialiseerde JSON-object overeenkomt met de leden van de **TestType** die is gedefinieerd in het model. De ook exact overeenkomen met waarden die worden gebruikt in de code. Echter, houd er rekening mee dat de binaire gegevens base64-gecodeerd is: 'AQID' is de base64-codering {0x01, 0x02, 0x03}.

In dit voorbeeld wordt het voordeel van het gebruik van de **serialisatiefunctie** bibliotheek--Hierdoor kunnen wij JSON verzenden naar de cloud, zonder expliciet omgaan met serialisatie in onze toepassing. Alle we hebben te hoeven maken over de waarden van de Gegevensgebeurtenissen in het model en vervolgens het aanroepen van eenvoudige API's voor het verzenden van gebeurtenissen naar de cloud configureert.

Met deze informatie kunt definiëren we modellen die het bereik van de ondersteunde gegevenstypen, met inbegrip van complexe typen (we kan zelfs complexe typen binnen andere complexe typen bevatten) bevatten. Echter, hij geserialiseerd JSON die worden gegenereerd door het bovenstaande voorbeeld wordt een belangrijk punt. *Hoe* we verzenden van gegevens met de **serialisatiefunctie** bibliotheek bepaalt precies hoe de JSON is samengesteld. Dat bepaald punt wat aan bod naast is.

## <a name="more-about-serialization"></a>Meer informatie over de serialisatie
Highlights van de vorige sectie een voorbeeld van uitvoer wordt gegenereerd door de **serialisatiefunctie** bibliotheek. In deze sectie wordt uitgelegd hoe de gegevens voor het serialiseert van de bibliotheek en hoe u dit gedrag met behulp van de serialisatie-API's kunt bepalen.

Om door te gaan de bespreking van de serialisatie, werkt we met een nieuw model op basis van een thermostaat. Eerst gaan we enkele achtergrond bieden op het scenario dat we te adres proberen.

We willen een thermostaat die temperatuur en vochtigheid meet model. Elk gegevensitem gaat anders naar IoT Hub worden verzonden. Standaard wordt de ingresses thermostaat een gebeurtenis temperatuur elke 2 minuten; een gebeurtenis vochtigheid is ingressed om de 15 minuten. Wanneer een gebeurtenis ingressed is, moet een tijdstempel dat de tijd bevat dat de bijbehorende temperatuur of vochtigheid is gemeten bevatten.

Dit scenario wordt opgegeven, moet ziet u twee verschillende manieren om de gegevens te modelleren en het effect wordt uitgelegd dat modellering op de geserialiseerde uitvoer heeft.

### <a name="model-1"></a>Model 1
Dit is de eerste versie van een model die ondersteuning biedt voor het vorige scenario:

```
BEGIN_NAMESPACE(Contoso);

DECLARE_STRUCT(TemperatureEvent,
int, Temperature,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_STRUCT(HumidityEvent,
int, Humidity,
EDM_DATE_TIME_OFFSET, Time);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureEvent, Temperature),
WITH_DATA(HumidityEvent, Humidity)
);

END_NAMESPACE(Contoso);
```

Opmerking dat het model twee Gegevensgebeurtenissen bevat: **temperatuur** en **vochtigheid**. In tegenstelling tot eerdere voorbeelden, het type van elke gebeurtenis is een structuur gedefinieerd met behulp van **DECLARE\_STRUCT**. **TemperatureEvent** bevat een temperatuurmeting en een tijdstempel; **HumidityEvent** een meting vochtigheid en een tijdstempel bevat. Dit model biedt ons een natuurlijke manier voor het modelleren van de gegevens voor het scenario dat hierboven wordt beschreven. Wanneer er een gebeurtenis naar de cloud verzenden, sturen ofwel we een temperatuur/tijdstempel of een combinatie van vochtigheid/tijdstempel.

We kunnen een gebeurtenis temperatuur sturen naar de cloud met code zoals de volgende:

```
time_t now;
time(&now);
thermostat->Temperature.Temperature = 75;
thermostat->Temperature.Time = GetDateTimeOffset(now);

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

We je vastgelegde waarden gebruiken voor de temperatuur en vochtigheid in de voorbeeldcode, maar Stel dat we daadwerkelijk deze waarden ophaalt van de bijbehorende sensoren op thermostaat steekproeven.

De bovenstaande maakt gebruik van code de **GetDateTimeOffset** helper die eerder is geïntroduceerd. Om redenen die wissen hoger fungeren zal, scheidt u deze code expliciet de taak van het serialiseren en het verzenden van de gebeurtenis. De vorige code serialiseert de temperatuur-gebeurtenis in een buffer. Vervolgens **sendMessage** is een Help-functie (opgenomen in **simplesample\_amqp**) die de gebeurtenis naar IoT Hub verzendt:

```
static void sendMessage(IOTHUB_CLIENT_HANDLE iotHubClientHandle, const unsigned char* buffer, size_t size)
{
    static unsigned int messageTrackingId;
    IOTHUB_MESSAGE_HANDLE messageHandle = IoTHubMessage_CreateFromByteArray(buffer, size);
    if (messageHandle != NULL)
    {
        IoTHubClient_SendEventAsync(iotHubClientHandle, messageHandle, sendCallback, (void*)(uintptr_t)messageTrackingId);

        IoTHubMessage_Destroy(messageHandle);
    }
    free((void*)buffer);
}
```

Deze code is een subset van de **SendAsync** helper beschreven in de vorige sectie, zodat we won't gaan eroverheen opnieuw hier.

Wanneer we de vorige code voor het verzenden van de gebeurtenis temperatuur uitvoert, wordt deze geserialiseerde vorm van de gebeurtenis verzonden naar IoT Hub:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

We bij het verzenden van een temperatuur van type **TemperatureEvent** en die struct bevat een **temperatuur** en **tijd** lid. Dit is rechtstreeks weerspiegeld in de geserialiseerde gegevens.

Op deze manier kunnen we een gebeurtenis vochtigheid met deze code verzenden:

```
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

De geserialiseerde vorm dat wordt verzonden naar IoT Hub wordt als volgt weergegeven:

```
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Dit is opnieuw, zoals verwacht.

Met dit model, kunt u voorstellen dat hoe meer gebeurtenissen kunnen eenvoudig worden toegevoegd. U meer structuren met definiëren **DECLARE\_STRUCT**, en de bijbehorende gebeurtenis opnemen in het model met **WITH\_gegevens**.

Nu gaan we het model wijzigen zodat deze dezelfde gegevens bevat, maar met een andere structuur.

### <a name="model-2"></a>Model 2
Houd rekening met deze alternatieve model een:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

In dit geval wordt geëlimineerd de **DECLARE\_STRUCT** macro's en de gegevensitems van ons scenario met eenvoudige typen van de modelleertaal gewoon definieert.

Alleen voor het moment dat we negeren de **tijd** gebeurtenis. Met deze Reserveer is hier de code voor inkomend **temperatuur**:

```
time_t now;
time(&now);
thermostat->Temperature = 75;

unsigned char* destination;
size_t destinationSize;
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code verzendt de volgende gebeurtenis in de geserialiseerde naar IoT Hub:

```
{"Temperature":75}
```

En de code voor het verzenden van de gebeurtenis vochtigheid ziet er als volgt:

```
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code verzendt dit naar IoT Hub:

```
{"Humidity":45}
```

Tot nu toe zijn er nog geen verrassingen. Nu gaan we wijzigen hoe we de macro SERIALISEREN gebruiken.

De **SERIALISEREN** macro kunnen meerdere Gegevensgebeurtenissen als argumenten. Dit kan we voor het serialiseren van de **temperatuur** en **vochtigheid** gebeurtenis samen en verzend dit naar IoT Hub in één aanroep:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Geschat mogelijk dat het resultaat van deze code is dat twee gebeurtenissen worden verzonden naar IoT Hub:

[

{{'Temperatuur': 75},

{{'Vochtigheid': 45}

]

Met andere woorden, u verwacht dat deze code hetzelfde als het verzenden van is **temperatuur** en **vochtigheid** afzonderlijk. Het is net ten behoeve van beide gebeurtenissen die moeten worden doorgegeven **SERIALISEREN** in dezelfde aanroep. Dat is echter niet het geval is. De bovenstaande code verzendt in plaats daarvan deze één gebeurtenis naar IoT Hub:

{{'Temperatuur': 75, 'vochtigheid': 45}

Dit kan het lijken vreemd omdat het model definieert **temperatuur** en **vochtigheid** als twee *afzonderlijke* gebeurtenissen:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Het herstelpunt niet we meer deze gebeurtenissen model waar **temperatuur** en **vochtigheid** zich in dezelfde structuur:

```
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Als we dit model gebruikt, het normaal zou zijn gemakkelijker te begrijpen hoe **temperatuur** en **vochtigheid** in hetzelfde geserialiseerde bericht zou worden verzonden. Echter niet altijd duidelijk waarom het op die manier werkt wanneer u beide Gegevensgebeurtenissen die moeten worden doorgegeven **SERIALISEREN** met model 2.

Dit gedrag is gemakkelijker te begrijpen als u het uitgangspunten die weet de **serialisatiefunctie** bibliotheek aan te brengen. Zinnig van deze daarvoor gaat u terug naar het model:

```
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Dit model in objectgeoriënteerde voorwaarden zien. In dit geval wordt een fysiek apparaat (thermostaat) bent modelleren en dat het apparaat bevat kenmerken, zoals **temperatuur** en **vochtigheid**.

We kunnen de hele status van het model met bijvoorbeeld de volgende code sturen:

```
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ervan uitgaande dat de waarden van temperatuur, vochtigheid en tijd zijn ingesteld, zien we een gebeurtenis als volgt verzonden naar IoT Hub:

```
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Soms u mogelijk alleen wilt verzenden *sommige* eigenschappen van het model in de cloud (dit is vooral van toepassing als uw model een groot aantal Gegevensgebeurtenissen bevat). Is het nuttig om alleen een subset van van Gegevensgebeurtenissen, zoals in het vorige voorbeeld verzenden:

```
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Hiermee wordt precies dezelfde geserialiseerde gebeurtenis gegenereerd als we hebt gedefinieerd een **TemperatureEvent** met een **temperatuur** en **tijd** lid, net zoals we met model 1. In dit geval we kunnen u exact dezelfde geserialiseerde gebeurtenis genereren met behulp van een ander model (model 2), omdat we aangeroepen **SERIALISEREN** op een andere manier.

Het belangrijkste is dat als u meerdere Gegevensgebeurtenissen doorgeeft **SERIALISEREN,** en vervolgens wordt ervan uitgegaan dat elke gebeurtenis is een eigenschap in een JSON-object.

De aanbevolen aanpak is afhankelijk van u en hoe u nadenken over uw model. Als u 'gebeurtenissen' naar de cloud verzendt en elke gebeurtenis een gedefinieerde set eigenschappen bevat, maakt de eerste benadering veel zin. In dat geval gebruikt u **DECLARE\_STRUCT** voor het definiëren van de structuur van elke gebeurtenis en plaatst deze in uw model met de **WITH\_gegevens** macro. U verzendt vervolgens elke gebeurtenis zoals we hebben gedaan in het eerste voorbeeld hierboven. In deze benadering zou u alleen een enkele gegevensgebeurtenis doorgeven **SERIALISATIEFUNCTIE**.

Als u over uw model op een wijze objectgeoriënteerde nadenkt, klikt u vervolgens de tweede oplossing kan aan de behoeften van u. In dit geval de elementen gedefinieerd met behulp van **WITH\_gegevens** 'Eigenschappen' van het object zijn. Geeft u de subset van gebeurtenissen naar **SERIALISEREN** die u tevreden bent, afhankelijk van hoeveel van uw 'van het object' status die u wilt verzenden naar de cloud.

Clusterservice aanpak is goed of fout. Alleen zich bewust zijn van hoe de **serialisatiefunctie** bibliotheek werkt en kies de modellering benadering die het beste past bij uw behoeften.

## <a name="message-handling"></a>Afhandeling van berichten
Tot nu toe in dit artikel heeft het verzenden van gebeurtenissen naar IoT Hub alleen besproken en nog niet opgelost ontvangen van berichten. De reden voor deze bewerking die wat er moet is weten over het ontvangen van berichten wordt grotendeels gedekt een [eerder artikel](iot-hub-device-sdk-c-intro.md). Intrekken van dat artikel berichten worden verwerkt door de callback-functie van een bericht te registreren:

```
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

U schrijft vervolgens de callbackfunctie die wordt aangeroepen wanneer een bericht wordt ontvangen:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT IoTHubMessage(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    IOTHUBMESSAGE_DISPOSITION_RESULT result;
    const unsigned char* buffer;
    size_t size;
    if (IoTHubMessage_GetByteArray(message, &buffer, &size) != IOTHUB_MESSAGE_OK)
    {
        printf("unable to IoTHubMessage_GetByteArray\r\n");
        result = EXECUTE_COMMAND_ERROR;
    }
    else
    {
        /*buffer is not zero terminated*/
        char* temp = malloc(size + 1);
        if (temp == NULL)
        {
            printf("failed to malloc\r\n");
            result = EXECUTE_COMMAND_ERROR;
        }
        else
        {
            memcpy(temp, buffer, size);
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

Deze implementatie van **IoTHubMessage** de specifieke functie aanroept voor elke actie in het model. Bijvoorbeeld, als het model is gedefinieerd met deze actie:

```
WITH_ACTION(SetAirResistance, int, Position)
```

U moet een functie met deze handtekening definiëren:

```
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** wordt aangeroepen wanneer het bericht wordt verzonden naar het apparaat.

We nog niet uitgelegd is hoe de geserialiseerde versie van bericht eruit nog. Met andere woorden, als u wilt verzenden een **SetAirResistance** bericht naar uw apparaat, hoe ziet dit eruit?

Als u een bericht naar een apparaat verzendt, wilt u dit doen via de Azure IoT service SDK. U moet nog steeds weten welke tekenreeks om te verzenden naar een bepaalde actie aanroepen. De algemene indeling voor het verzenden van een bericht wordt weergegeven als volgt:

```
{"Name" : "", "Parameters" : "" }
```

U verzendt een geserialiseerde JSON-object met twee eigenschappen: **naam** is de naam van de actie (bericht) en **Parameters** bevat de parameters van deze actie.

Bijvoorbeeld, om aan te roepen **SetAirResistance** kunt u dit bericht verzendt naar een apparaat:

```
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

De naam van de moet exact overeenkomen met een actie die is gedefinieerd in het model. De parameternamen moeten ook overeenkomen met. Let ook op hoofdletters en kleine letters. **Naam** en **Parameters** zijn altijd hoofdletters. Zorg ervoor dat overeenkomt met het geval van uw naam voor de actie en de parameters in het model. In dit voorbeeld is de naam van de 'SetAirResistance' en niet 'setairresistance'.

De twee andere acties **TurnFanOn** en **TurnFanOff** kan worden aangeroepen door deze berichten verzenden naar een apparaat:

```
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Deze sectie beschreven alles wat u weten wanneer gebeurtenissen verzenden en ontvangen berichten moet met de **serialisatiefunctie** bibliotheek. Voordat u doorgaat, gaan we betrekking op bepaalde parameters kunt u configureren die bepalen hoe groot is voor het model.

## <a name="macro-configuration"></a>Macro-configuratie
Als u de **serialisatiefunctie** bibliotheek een belangrijk onderdeel van de SDK moet denken is gevonden in de bibliotheek voor het azure-c-gedeeld-hulpprogramma.
Als u de Azure-iot-sdk-c-opslagplaats vanuit GitHub met de optie--recursieve hebt gekloond, zult u deze gedeelde hulpprogrammabibliotheek:

```
.\\c-utility
```

Als u niet de bibliotheek hebt gekloond, kun je [hier](https://github.com/Azure/azure-c-shared-utility).

In de tapewisselaar gedeelde hulpprogramma vindt u de volgende map:

```
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Deze map bevat een Visual Studio-oplossing aangeroepen **macro\_utils\_h\_generator.sln**:

  ![](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.PNG)

Het programma in deze oplossing genereert de **macro\_utils.h** bestand. Er is een standaardmacro\_utils.h-bestand met de SDK is opgenomen. Deze oplossing kunt u sommige parameters te wijzigen en vervolgens opnieuw maken het header-bestand op basis van deze parameters.

De twee belangrijkste parameters met betrokken zijn **nArithmetic** en **nMacroParameters** die zijn gedefinieerd in deze twee regels gevonden in de macro\_utils.tt:

```
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>

```

Deze waarden zijn de standaardparameters opgenomen met de SDK. Elke parameter heeft de volgende zin:

* nMacroParameters – bepaalt hoeveel parameters die u kunt hebben in een DECLARE\_macrodefinitie MODEL.
* nArithmetic: Hiermee bepaalt u het totale aantal leden in een model is toegestaan.

De reden dat deze parameters belangrijk zijn is omdat ze bepalen hoe lang het model kan worden. Neem bijvoorbeeld een modeldefinitie voor dit:

```
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Zoals eerder vermeld **DECLARE\_MODEL** is slechts een C macro. De namen van het model en de **WITH\_gegevens** instructie (nog een andere macro) zijn parameters van **DECLARE\_MODEL**. **nMacroParameters** definieert hoeveel parameters kunnen worden opgenomen in **DECLARE\_MODEL**. Hiermee definieert effectief, hoeveel gegevens gebeurtenis en actie declaraties u kunt hebben. Als zodanig met de standaardlimiet van 124 betekent dit dat u een model met een combinatie van ongeveer 60 acties en Gegevensgebeurtenissen kunt definiëren. Als u deze limiet overschrijdt probeert, ontvangt u compilatiefouten die er ongeveer als volgt:

  ![](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.PNG)

De **nArithmetic** parameter meer over de interne werking van de macrotaal dan uw toepassing is.  Hiermee kunt u het totale aantal leden die u in uw model hebben kunt, inclusief **DECLARE_STRUCT** macro's. Als u beginnen te zien compilatiefouten zoals dit en probeer u moet verhogen **nArithmetic**:

   ![](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.PNG)

Als u wilt dat deze parameters wijzigt, wijzigt u de waarden in de macro\_utils.tt bestand, het compileren van de macro\_utils\_h\_generator.sln oplossing, en voer het programma voor compilatie. Als u dit doet, een nieuwe macro\_utils.h bestand is gegenereerd en worden in de.\\ algemene\\inc directory.

Om te kunnen gebruiken van de nieuwe versie van de macro\_utils.h, verwijdert u de **serialisatiefunctie** NuGet-pakket van uw oplossing en in plaats daarvan zijn de **serialisatiefunctie** Visual Studio-project. Hierdoor wordt uw code te compileren met de broncode van de serialisatiefunctie-bibliotheek. Dit omvat de bijgewerkte macro\_utils.h. Als u dit doen wilt voor **simplesample\_amqp**, begin met het verwijderen van het NuGet-pakket voor de serialisatiefunctie-bibliotheek van de oplossing:

   ![](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.PNG)

Voeg vervolgens dit project toe aan uw Visual Studio-oplossing:

> . \\c\\serialisatiefunctie\\bouwen\\windows\\serializer.vcxproj
> 
> 

Wanneer u bent klaar, ziet uw oplossing als volgt:

   ![](media/iot-hub-device-sdk-c-serializer/05-serializer-project.PNG)

Nu wanneer het compileren van uw oplossing, de bijgewerkte macro\_utils.h is opgenomen in uw binary.

Houd er rekening mee dat deze waarden hoog genoeg verhogen compiler overschrijden kunt. Tot op dit moment de **nMacroParameters** is de belangrijkste parameter waarmee worden betrokken. De specificatie C99 geeft aan dat een minimum van 127 parameters zijn toegestaan in een macrodefinitie van de. De Microsoft-compiler volgt de specificatie exact (en kent een limiet van 127), zodat het niet mogelijk verhogen **nMacroParameters** tot meer dan de standaardwaarde. Andere compilers kunt u mogelijk om dit te doen (bijvoorbeeld de compiler GNU een hogere limiet ondersteunt).

Tot nu toe hebben we behandeld bijna alles wat u moet weten over het schrijven van code die de **serialisatiefunctie** bibliotheek. Vóór de sluiting, bezoekt u gaan we een aantal onderwerpen uit eerdere artikelen die u vraagt zich misschien af over.

## <a name="the-lower-level-apis"></a>De API's van lager niveau
De voorbeeldtoepassing waarop dit artikel gericht is **simplesample\_amqp**. In dit voorbeeld gebruikt de op een hoger niveau (de niet-' LLE ") API's voor het verzenden van gebeurtenissen en ontvangen van berichten. Als u deze API's gebruikt, wordt een achtergrond-thread wordt uitgevoerd die zorgt voor zowel het verzenden van gebeurtenissen en ontvangen van berichten. U kunt echter de lagere niveaus (ES) API's gebruiken deze achtergrondthread elimineren en expliciete controle over wanneer u gebeurtenissen verzenden of ontvangen van berichten vanuit de cloud.

Zoals beschreven in een [vorige artikel](iot-hub-device-sdk-c-iothubclient.md), er is een reeks functies die uit de API's van een hoger niveau bestaat:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_vernietigen

Deze API's worden uitgelegd **simplesample\_amqp**.

Er is ook een vergelijkbare reeks API's van lager niveau.

* IoTHubClient\_LLE\_CreateFromConnectionString
* IoTHubClient\_LLE\_SendEventAsync
* IoTHubClient\_LLE\_SetMessageCallback
* IoTHubClient\_LLE\_vernietigen

Houd er rekening mee dat de API's van lager niveau werken op dezelfde manier zoals beschreven in de vorige artikelen. U kunt de eerste reeks API's gebruiken als u wilt dat een achtergrond-thread voor het afhandelen van gebeurtenissen verzenden en ontvangen berichten. U kunt de tweede reeks API's gebruiken als u wilt dat expliciete controle over wanneer u gegevens verzenden en ontvangen van IoT Hub. Een reeks API's werken net zo goed met de **serialisatiefunctie** bibliotheek.

Voor een voorbeeld van hoe de niveau-API's worden gebruikt met de **serialisatiefunctie** bibliotheek, Zie de **simplesample\_http** toepassing.

## <a name="additional-topics"></a>Extra onderwerpen
Enkele andere onderwerpen opgemerkt zijn opnieuw eigendom verwerken, met behulp van alternatieve inloggegevens en configuratieopties. Dit zijn alle onderwerpen in een [vorige artikel](iot-hub-device-sdk-c-iothubclient.md). Het belangrijkste is dat al deze functies op dezelfde manier als met werken de **serialisatiefunctie** bibliotheek als met de **IoTHubClient** bibliotheek. Bijvoorbeeld, als u wilt de eigenschappen koppelen aan een gebeurtenis uit het model, u **IoTHubMessage\_eigenschappen** en **kaart**\_**AddorUpdate**, net zoals eerder beschreven:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Of de gebeurtenis is gegenereerd op basis van de **serialisatiefunctie** bibliotheek of gemaakt handmatig met behulp van de **IoTHubClient** bibliotheek maakt niet uit.

Referenties voor de alternatieve apparaat, met behulp van **IoTHubClient\_LLE\_maken** net zo goed werkt als **IoTHubClient\_CreateFromConnectionString** voor het toewijzen van een **IOTHUB\_CLIENT\_verwerken**.

Ten slotte, als u de **serialisatiefunctie** bibliotheek, kunt u configuratieopties met instellen **IoTHubClient\_LLE\_SetOption** net zoals u deed bij gebruik van de **IoTHubClient** bibliotheek.

Een functie die uniek is voor de **serialisatiefunctie** bibliotheek zijn de initialisatie van de API's. Voordat u met de bibliotheek werken beginnen kunt, moet u aanroepen **serialisatiefunctie\_init**:

```
serializer_init(NULL);
```

Dit wordt gedaan vlak voordat u aanroepen **IoTHubClient\_CreateFromConnectionString**.

Op dezelfde manier als u klaar bent met de bibliotheek werken, is de laatste aanroep u maakt het **serialisatiefunctie\_deinit**:

```
serializer_deinit();
```

Anders wordt alle hierboven vermelde-functies werken op dezelfde manier de **serialisatiefunctie** bibliotheek als in de **IoTHubClient** bibliotheek. Zie voor meer informatie over deze onderwerpen, de [vorige artikel](iot-hub-device-sdk-c-iothubclient.md) in deze reeks.

## <a name="next-steps"></a>Volgende stappen
Dit artikel wordt beschreven in detail de unieke aspecten van de **serialisatiefunctie** bibliotheek opgenomen in de **Azure IoT-device SDK voor C**. Met de informatie verstrekt er is een goed inzicht in het gebruik van modellen gebeurtenissen verzenden en ontvangen van berichten uit IoT Hub.

Hiermee ook de reeks driedelige over het ontwikkelen van toepassingen met is de **Azure IoT-device SDK voor C**. Dit moet onvoldoende informatie om niet alleen u op weg maar bieden u een grondig begrip over de werking van de API's. Voor meer informatie zijn er enkele voorbeelden in de SDK niet ingegaan. Anders wordt de [SDK-documentatie](https://github.com/Azure/azure-iot-sdk-c) is een goede bron voor meer informatie.

Zie voor meer informatie over het ontwikkelen voor IoT Hub, de [Azure IoT SDK's][lnk-sdks].

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
