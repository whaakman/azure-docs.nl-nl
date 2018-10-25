---
title: Azure IoT device-SDK voor C - Serializer | Microsoft Docs
description: Het gebruik van de Serializer-bibliotheek in de Azure IoT-device-SDK voor C om te maken van apps op apparaten die communiceren met een IoT-hub.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 09/06/2016
ms.author: yizhon
ms.openlocfilehash: 0a7e30be374ae5095e206ce0e519e51bb58f1f00
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024855"
---
# <a name="azure-iot-device-sdk-for-c--more-about-serializer"></a>Azure IoT device-SDK voor C: meer informatie over serialisatiefunctie

De eerste artikel van deze serie geïntroduceerd de [Inleiding tot Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). Het volgende artikel opgegeven een gedetailleerde beschrijving van de [Azure IoT device-SDK voor C--IoTHubClient](iot-hub-device-sdk-c-iothubclient.md). In dit artikel wordt de dekking van de SDK door te geven van een gedetailleerde beschrijving van het resterende onderdeel: de **serializer** bibliotheek.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Het inleidende artikel wordt beschreven hoe u de **serializer** bibliotheek om gebeurtenissen te verzenden en ontvangen van berichten uit IoT Hub. In dit artikel, we die discussie uitbreiden door te geven van een uitgebreidere uitleg over hoe u uw gegevens met het model de **serializer** macrotaal. Het artikel ook bevat meer informatie over hoe de bibliotheek berichten serialiseert (en in sommige gevallen hoe kunt u de serialisatie-gedrag beheren). Sommige parameters die kunt u wijzigen om te bepalen van de grootte van de modellen die u zult ook wordt beschreven.

Ten slotte revisits het artikel enkele onderwerpen aan bod in de vorige artikelen, zoals bericht en de verwerking van de eigenschap. Als we u uitzoeken, deze functies werken op dezelfde manier met behulp van de **serializer** bibliotheek als ze dit met doen de **IoTHubClient** bibliotheek.

Alles dat wordt beschreven in dit artikel is gebaseerd op de **serializer** SDK-voorbeelden. Als u volgen wilt, raadpleegt u de **simplesample\_amqp** en **simplesample\_http** toepassingen die zijn opgenomen in de Azure IoT-device-SDK voor c

U vindt de [ **Azure IoT device-SDK voor C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats en de weergave details van de API in de [C-API-verwijzing](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

## <a name="the-modeling-language"></a>De modelleertaal

De [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md) artikel van deze serie geïntroduceerd de **Azure IoT device-SDK voor C** modelleertaal via het voorbeeld dat is opgegeven in de **simplesample\_ amqp** toepassing:

```C
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

Zoals u ziet, is de modelleertaal gebaseerd op C macro's. U eerst uw definitie met **BEGIN\_NAAMRUIMTE** en eindigen op altijd **END\_NAAMRUIMTE**. Het is gebruikelijk om de naam van de naamruimte voor uw bedrijf of, zoals in dit voorbeeld wordt het project waaraan u werkt.

Wat komt binnen de naamruimte model definities zijn. In dit geval is er één model voor een anemometer. Nogmaals, het model kan worden met de naam, maar doorgaans het model met de naam voor het apparaat of het type van de gegevens die u wilt gebruiken om te wisselen met IoT Hub.  

Modellen een definitie bevatten van de gebeurtenissen kunt u inkomend verkeer naar IoT Hub (de *gegevens*) en de berichten uit IoT Hub kunt u ontvangen (de *acties*). Zoals u in het voorbeeld zien kunt, hebben gebeurtenissen een type en een naam. acties hebben een naam en optionele parameters (elk met een type).

Wat wordt niet gedemonstreerd in dit voorbeeld zijn extra gegevenstypen die worden ondersteund door de SDK. Aan de orde dat gaan.

> [!NOTE]
> IoT Hub verwijst naar de gegevens die een apparaat naar het bestand als verzendt *gebeurtenissen*, terwijl de modelleertaal naar het bestand als verwijst *gegevens* (gedefinieerd met behulp van **WITH_DATA**). Evenzo, IoT-Hub verwijst naar de gegevens die u kunt naar apparaten als verzenden *berichten*, terwijl de modelleertaal naar het bestand als verwijst *acties* (gedefinieerd met behulp van **WITH_ACTION**). Let erop dat deze bepalingen door elkaar kunnen worden gebruikt in dit artikel.
> 
> 

## <a name="supported-data-types"></a>Ondersteunde gegevenstypen

De volgende gegevenstypen worden ondersteund in modellen die zijn gemaakt met de **serializer** bibliotheek:

| Type | Beschrijving |
| --- | --- |
| double |dubbele precisiegetal met drijvende komma |
| int |32-bits geheel getal zijn |
| float |enkele precisiegetal met drijvende komma |
| lang |lange geheel getal zijn |
| int8\_t |8-bits geheel getal zijn |
| int16\_t |16-bits geheel getal zijn |
| Int32\_t |32-bits geheel getal zijn |
| int64\_t |64-bits geheel getal zijn |
| BOOL |booleaans |
| ASCII\_char\_ptr |ASCII-tekenreeks |
| EDM\_DATUM\_TIJD\_OFFSET |datum time-offset |
| EDM\_GUID |GUID |
| EDM\_BINAIRE |binaire bestanden |
| DECLAREER\_STRUCT |Complex gegevenstype |

Laten we beginnen met het laatste gegevenstype. De **DECLARE\_STRUCT** kunt u complexe gegevenstypen, groeperingen van de andere primitieve typen definiëren. Deze groeperingen kunnen we voor het definiëren van een model dat er als uitzien volgt:

```C
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

Het model bevat een eenmalige gebeurtenis van het type **TestType**. **TestType** is een complex type met meerdere leden die gezamenlijk de primitieve typen die worden ondersteund demonstreren door de **serializer** modelleren van taal.

Met een model als dit kunnen we code om gegevens te verzenden naar IoT-Hub die wordt weergegeven als volgt schrijven:

```C
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

In feite, wijzen we een waarde op elk lid van de **Test** structuur en klikt u vervolgens aan te roepen **SendAsync** voor het verzenden van de **Test** gebeurtenis van de gegevens naar de cloud. **SendAsync** is een helperfunctie waarmee een één gebeurtenis naar IoT Hub verzonden:

```C
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

Deze functie de opgegeven gebeurtenis serialiseert en verzendt ze naar IoT Hub met behulp van **IoTHubClient\_SendEventAsync**. Dit is dezelfde als de code die wordt besproken in de vorige artikelen (**SendAsync** kapselt de logica in een handige functie).

Een van andere Help-functie die wordt gebruikt in de vorige code is **GetDateTimeOffset**. Deze functie transformeert de opgegeven tijd naar een waarde van het type **EDM\_datum\_tijd\_OFFSET**:

```C
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

```C
{"aDouble":1.100000000000000, "aInt":2, "aFloat":3.000000, "aLong":4, "aInt8":5, "auInt8":6, "aInt16":7, "aInt32":8, "aInt64":9, "aBool":true, "aAsciiCharPtr":"ascii string 1", "aDateTimeOffset":"2015-09-14T21:18:21Z", "aGuid":"00010203-0405-0607-0809-0A0B0C0D0E0F", "aBinary":"AQID"}
```

Houd er rekening mee dat de serialisatie in JSON, dit de indeling is die zijn gegenereerd door de **serializer** bibliotheek. Let ook op dat elk lid van het geserialiseerde JSON-object komt overeen met de leden van de **TestType** die we hebben gedefinieerd in het model. De waarden ook exact overeenkomen met die in de code worden gebruikt. Echter, houd er rekening mee dat de binaire gegevens base64-gecodeerd is: "AQID" is de met base64-codering van {0x01, 0x02, 0x03}.

In dit voorbeeld ziet u het voordeel van het gebruik van de **serializer** bibliotheek--Hierdoor kunnen we voor het verzenden van JSON naar de cloud, zonder dat u hoeft te bekommeren expliciet serialisatie in onze toepassing. Alles wat we hoeft te bekommeren om is instellen van de waarden van de Gegevensgebeurtenissen in het model en eenvoudige API's voor het verzenden van gebeurtenissen naar de cloud vervolgens aan te roepen.

Met deze gegevens definiëren we modellen die het bereik van de ondersteunde gegevenstypen, met inbegrip van complexe typen (we kunnen zelfs complexe typen binnen andere complexe typen bevatten). Echter, de geserialiseerde JSON die worden gegenereerd door het bovenstaande voorbeeld wordt een belangrijk punt. *Hoe* we bij het verzenden van gegevens met de **serializer** bibliotheek bepaalt precies hoe de JSON heeft een onjuiste indeling. Dat bepaald tijdstip aan bod vervolgens is.

## <a name="more-about-serialization"></a>Meer informatie over de serialisatie

De vorige sectie wordt een voorbeeld van uitvoer wordt gegenereerd door de **serializer** bibliotheek. In deze sectie wordt uitgelegd hoe gegevens in de bibliotheek worden serialiseert en hoe u dit gedrag met behulp van de serialisatie-API's kunt beheren.

Als u wilt gaan de discussie over serialisatie, werkt we met een nieuw model op basis van een thermostaat. Eerst gaan we enige ervaring te bieden op het scenario dat we te adres proberen.

We willen een thermostaat die temperatuur en vochtigheid meet model. Elke hoeveelheid gegevens gaat anders naar IoT Hub worden verzonden. Standaard wordt de ingresses thermostaat een temperatuur gebeurtenis om de 2 minuten; een gebeurtenis vochtigheid is ingressed om de 15 minuten. Wanneer een gebeurtenis ingressed wordt, moet deze een timestamp die hier de tijd wordt dat de bijbehorende temperatuur of vochtigheid werd gemeten bevatten.

In dit scenario worden gegeven, we laten zien twee verschillende manieren om de gegevens te modelleren en leggen we uit het effect dat modellering op de geserialiseerde uitvoer heeft.

### <a name="model-1"></a>Model 1

Dit is de eerste versie van een model dat ondersteuning biedt voor het voorgaande scenario:

```C
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

Houd er rekening mee dat het model twee Gegevensgebeurtenissen bevat: **temperatuur** en **vochtigheid**. In tegenstelling tot eerdere voorbeelden, het type van elke gebeurtenis is een structuur die is gedefinieerd met behulp van **DECLARE\_STRUCT**. **TemperatureEvent** bevat een temperatuurmeting en een tijdstempel; **HumidityEvent** bevat een meting vochtigheid en een tijdstempel. Dit model biedt ons een natuurlijke manier om de gegevens voor de hierboven beschreven scenario te modelleren. Wanneer er een gebeurtenis naar de cloud verzonden, sturen ofwel we een temperatuur/tijdstempel of een paar vochtigheid/tijdstempel.

We kunnen een gebeurtenis temperatuur sturen naar de cloud met behulp van code, zoals het volgende:

```C
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

We u vastgelegde waarden gebruiken voor de temperatuur en vochtigheid in de voorbeeldcode, maar Stel dat we bent daadwerkelijk bij het ophalen van deze waarden door de bijbehorende sensoren op thermostaat steekproeven.

De bovenstaande maakt gebruik van code de **GetDateTimeOffset** helper die eerder is geïntroduceerd. Om redenen die duidelijk later worden, wordt deze code expliciet de taak van het serialiseren en het verzenden van de gebeurtenis gescheiden. De vorige code serialiseert de temperatuur-gebeurtenis in een buffer. Vervolgens **sendMessage** is van een Help-functie (opgenomen in **simplesample\_amqp**) die de gebeurtenis verzonden naar IoT Hub:

```C
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

Deze code is een subset van de **SendAsync** helper in de vorige sectie wordt beschreven, zodat we wordt niet hier erboven opnieuw.

Wanneer we de vorige code voor het verzenden van de gebeurtenis temperatuur uitvoert, wordt deze geserialiseerde vorm van de gebeurtenis verzonden naar IoT Hub:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

We bij het verzenden van een temperatuur van het type is **TemperatureEvent** en die struct bevat een **temperatuur** en **tijd** lid. Dit is de geserialiseerde gegevens rechtstreeks weergegeven.

We kunnen op dezelfde manier een gebeurtenis vochtigheid met deze code sturen:

```C
thermostat->Humidity.Humidity = 45;
thermostat->Humidity.Time = GetDateTimeOffset(now);
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

De geserialiseerde vorm die wordt verzonden naar IoT Hub weergegeven als volgt:

```C
{"Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Dit is het opnieuw, zoals verwacht.

Met dit model kunt u zien hoe aanvullende gebeurtenissen kunnen eenvoudig worden toegevoegd. U definieert meer structuren met **DECLARE\_STRUCT**, en de bijbehorende gebeurtenis in het model door te nemen **WITH\_gegevens**.

Nu gaan we het model wijzigen zodat deze dezelfde gegevens zijn opgenomen, maar met een andere structuur.

### <a name="model-2"></a>Model 2

Houd rekening met deze alternatieve model hierboven:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

In dit geval we hebt verwijderd de **DECLARE\_STRUCT** macro's en de gegevensitems die in ons scenario met behulp van eenvoudige typen van de modelleertaal gewoon wilt definiëren.

Alleen voor het moment dat, negeert de **tijd** gebeurtenis. Met deze aside is hier de code voor inkomend verkeer **temperatuur**:

```C
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

Deze code wordt de volgende geserialiseerde gebeurtenis verzonden naar IoT Hub:

```C
{"Temperature":75}
```

En de code voor het verzenden van de gebeurtenis vochtigheid als volgt weergegeven:

```C
thermostat->Humidity = 45;
if (SERIALIZE(&destination, &destinationSize, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Deze code verzendt dit naar IoT Hub:

```C
{"Humidity":45}
```

Er zijn tot nu toe nog steeds geen verrassingen. Nu gaan we wijzigen hoe we de macro SERIALISEREN gebruiken.

De **SERIALISEREN** macro kunnen meerdere Gegevensgebeurtenissen als argumenten. Hierdoor kunnen we het serialiseren van de **temperatuur** en **vochtigheid** gebeurtenis samen en verzend dit naar IoT Hub in één aanroep:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

U kunt gokken dat het resultaat van deze code is dat twee gebeurtenissen worden verzonden naar IoT Hub:

[{"Temperatuur": 75}, {"Vochtigheid": 45}]

Met andere woorden, u denkt dat deze code hetzelfde als het verzenden van is **temperatuur** en **vochtigheid** afzonderlijk. Dit is alleen nuttig om door te geven van beide gebeurtenissen naar **SERIALISEREN** in dezelfde aanroep. Dat is echter niet het geval. In plaats daarvan verzendt de bovenstaande code deze één gebeurtenis naar IoT Hub:

{"Temperatuur": 75, "vochtigheid": 45}

Dit lijkt vreemd omdat ons model definieert **temperatuur** en **vochtigheid** als twee *afzonderlijke* gebeurtenissen:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Meer op het punt niet we deze gebeurtenissen model waarbij **temperatuur** en **vochtigheid** zich in dezelfde structuur:

```C
DECLARE_STRUCT(TemperatureAndHumidityEvent,
int, Temperature,
int, Humidity,
);

DECLARE_MODEL(Thermostat,
WITH_DATA(TemperatureAndHumidityEvent, TemperatureAndHumidity),
);
```

Als we dit model gebruikt, het normaal zou zijn gemakkelijker te begrijpen hoe **temperatuur** en **vochtigheid** in hetzelfde geserialiseerde bericht worden verzonden. Maar het is niet altijd duidelijk waarom het op deze manier werkt als u beide Gegevensgebeurtenissen die moeten worden gehaald **SERIALISEREN** met 2-model.

Dit gedrag is gemakkelijker te begrijpen als u de veronderstellingen dat weet de **serializer** bibliotheek maakt. Aan het zinvol zijn van deze we gaan terug naar ons model:

```C
DECLARE_MODEL(Thermostat,
WITH_DATA(int, Temperature),
WITH_DATA(int, Humidity),
WITH_DATA(EDM_DATE_TIME_OFFSET, Time)
);
```

Dit model in termen van objectgeoriënteerde programmeertechnologieën beschouwen. In dit geval wordt een fysiek apparaat (een thermostaat) bent modelleren en dat het apparaat bevat kenmerken, zoals **temperatuur** en **vochtigheid**.

We kunnen de volledige status van het model met code, zoals de volgende sturen:

```C
if (SERIALIZE(&destination, &destinationSize, thermostat->Temperature, thermostat->Humidity, thermostat->Time) == IOT_AGENT_OK)
{
    sendMessage(iotHubClientHandle, destination, destinationSize);
}
```

Ervan uitgaande dat de waarden van de temperatuur, vochtigheid en tijd zijn ingesteld, zien we dat een gebeurtenis als volgt verzonden naar IoT Hub:

```C
{"Temperature":75, "Humidity":45, "Time":"2015-09-17T18:45:56Z"}
```

Soms kan alleen wilt u voor het verzenden van *sommige* eigenschappen van het model naar de cloud (dit is vooral van toepassing als uw model een groot aantal Gegevensgebeurtenissen bevat). Het is handig voor het verzenden van alleen een subset van van Gegevensgebeurtenissen, zoals in het eerdere voorbeeld:

```C
{"Temperature":75, "Time":"2015-09-17T18:45:56Z"}
```

Dit exact dezelfde geserialiseerde gebeurtenis genereert als wanneer we hadden gedefinieerd een **TemperatureEvent** met een **temperatuur** en **tijd** lid, net zoals we met het model 1. In dit geval konden we precies dezelfde geserialiseerde gebeurtenis genereren met behulp van een ander model (model 2), omdat we met de naam **SERIALISEREN** op een andere manier.

Wat belangrijk is dat als u meerdere Gegevensgebeurtenissen naar doorgeeft **SERIALISEREN,** en vervolgens wordt ervan uitgegaan dat elke gebeurtenis is een eigenschap in een JSON-object.

De beste benadering is afhankelijk van u en hoe u uw model vindt. Als u "events" naar de cloud verzendt en elke gebeurtenis een gedefinieerde set eigenschappen bevat, klikt u vervolgens vormen de eerste benadering een logische combinatie. In dat geval gebruikt u **DECLARE\_STRUCT** voor het definiëren van de structuur van elke gebeurtenis en deze vervolgens opnemen in uw model met de **WITH\_gegevens** macro. U verzendt vervolgens elke gebeurtenis zoals we hebben gedaan in het eerste voorbeeld. In deze benadering zou u alleen een één gebeurtenis doorgeven **SERIALIZER**.

Als u uw model in een objectgeoriënteerde manier vindt, klikt u vervolgens de tweede methode kan naar eigen inzicht. In dit geval de elementen gedefinieerd met behulp van **WITH\_gegevens** ' Eigenschappen ' van het object. Geeft u de subset van de gebeurtenissen **SERIALISEREN** die u wilt, afhankelijk van hoeveel van uw ' van ' Objectstatus u wilt verzenden naar de cloud.

Clusterservice aanpak is gegaan of naar rechts. Alleen rekening met hoe de **serializer** bibliotheek werkt, en kies de benadering van modellering die het beste past bij uw behoeften.

## <a name="message-handling"></a>Afhandeling van berichten

Tot nu toe in dit artikel is verzonden gebeurtenissen naar IoT Hub alleen besproken en nog niet opgelost ontvangen van berichten. De reden hiervoor is dat wat we wilt weten over het ontvangen van berichten is grotendeels aan bod gekomen in het artikel [Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md). Intrekken van dat artikel berichten worden verwerkt door een retouraanroepfunctie bericht registreren:

```C
IoTHubClient_SetMessageCallback(iotHubClientHandle, IoTHubMessage, myWeather)
```

U schrijft vervolgens de callbackfunctie die wordt aangeroepen wanneer een bericht wordt ontvangen:

```C
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

Deze implementatie van **IoTHubMessage** roept de specifieke functie voor elke actie in het model. Bijvoorbeeld, als uw model is gedefinieerd met deze actie:

```C
WITH_ACTION(SetAirResistance, int, Position)
```

U moet een functie met een handtekening definiëren:

```C
EXECUTE_COMMAND_RESULT SetAirResistance(ContosoAnemometer* device, int Position)
{
    (void)device;
    (void)printf("Setting Air Resistance Position to %d.\r\n", Position);
    return EXECUTE_COMMAND_SUCCESS;
}
```

**SetAirResistance** wordt aangeroepen wanneer dat bericht is verzonden naar uw apparaat.

Wat we nog niet uitgelegd is hoe de geserialiseerde versie is van het bericht eruit nog. Met andere woorden, als u wilt dat voor het verzenden van een **SetAirResistance** bericht naar uw apparaat, hoe ziet dit eruit?

Als u een bericht naar een apparaat verzendt, zou u dit doen via de Azure IoT service SDK. U moet wel weten welke tekenreeks die moet worden verzonden om aan te roepen een bepaalde actie. De algemene indeling voor het verzenden van een bericht weergegeven als volgt:

```C
{"Name" : "", "Parameters" : "" }
```

U verzendt een geserialiseerde JSON-object met twee eigenschappen: **naam** is de naam van de actie (bericht) en **Parameters** bevat de parameters van de actie.

Bijvoorbeeld, om aan te roepen **SetAirResistance** kunt u dit bericht verzenden naar een apparaat:

```C
{"Name" : "SetAirResistance", "Parameters" : { "Position" : 5 }}
```

De actienaam van de moet exact overeenkomen met een actie die is gedefinieerd in uw model. De parameternamen van de moeten ook overeenkomen met. Let ook op hoofdletters en kleine letters. **Naam** en **Parameters** zijn altijd hoofdletters. Zorg ervoor dat u het hoofdlettergebruik van de naam van de actie en de parameters in uw model. In dit voorbeeld is de actienaam van de 'SetAirResistance' en niet 'setairresistance'.

De twee andere acties **TurnFanOn** en **TurnFanOff** kunnen worden aangeroepen door deze berichten verzenden naar een apparaat:

```C
{"Name" : "TurnFanOn", "Parameters" : {}}
{"Name" : "TurnFanOff", "Parameters" : {}}
```

Deze sectie wordt beschreven in alles wat u weten wanneer gebeurtenissen verzenden en ontvangen berichten moet met de **serializer** bibliotheek. Voordat u doorgaat, gaan we omvatten bepaalde parameters kunt u configureren die bepalen hoe groot uw model is.

## <a name="macro-configuration"></a>Configuratie van de macro

Als u de **Serializer** -bibliotheek een belangrijk onderdeel van de SDK moet rekening houden met is gevonden in de bibliotheek voor het azure-c-gedeeld-hulpprogramma.

Als u de Azure-iot-sdk-c-opslagplaats van GitHub met behulp van de optie--recursieve hebt gekloond, wordt u deze hulpprogrammabibliotheek, gedeelde hier vinden:

```C
.\\c-utility
```

Als u niet de bibliotheek hebt gekloond, u vindt deze [hier](https://github.com/Azure/azure-c-shared-utility).

In de tapewisselaar gedeelde hulpprogramma vindt u de volgende map:

```C
azure-c-shared-utility\\macro\_utils\_h\_generator.
```

Deze map bevat een Visual Studio-oplossing met de naam **macro\_utils\_h\_generator.sln**:

  ![Schermafbeelding van de Visual Studio-oplossing maco_utils_h_generator](media/iot-hub-device-sdk-c-serializer/01-macro_utils_h_generator.png)

Het programma in deze oplossing genereert de **macro\_utils.h** bestand. Er is een standaardmacro\_utils.h-bestand en de SDK is opgenomen. Deze oplossing kunt u enkele parameters te wijzigen en vervolgens opnieuw de header-bestand op basis van deze parameters.

De twee belangrijkste parameters zorgen met zijn **nArithmetic** en **nMacroParameters** die zijn gedefinieerd in deze twee regels gevonden in de macro\_utils.tt:

```C
<#int nArithmetic=1024;#>
<#int nMacroParameters=124;/*127 parameters in one macro deﬁnition in C99 in chapter 5.2.4.1 Translation limits*/#>
```

Deze waarden zijn de standaardparameters en de SDK is opgenomen. Elke parameter heeft de volgende betekenis:

* nMacroParameters: Hiermee bepaalt u het aantal parameters die u kunt in één DECLARE hebben\_macrodefinitie MODEL.
* nArithmetic: Hiermee bepaalt u het totale aantal leden dat is toegestaan in een model.

De reden dat deze parameters belangrijk zijn is omdat ze bepalen hoe groot uw model mag zijn. Bijvoorbeeld, houd rekening met de definitie van dit model:

```C
DECLARE_MODEL(MyModel,
WITH_DATA(int, MyData)
);
```

Zoals eerder vermeld **DECLARE\_MODEL** is slechts een macro C. De namen van het model en de **WITH\_gegevens** instructie (nog een andere macro) worden parameters van **DECLARE\_MODEL**. **nMacroParameters** definieert het aantal parameters kunnen worden opgenomen in **DECLARE\_MODEL**. Hiermee definieert effectief, hoeveel gegevens gebeurtenis en actie declaraties u kan hebben. Daarom met de standaardlimiet van 124 betekent dit dat u een model met een combinatie van ongeveer 60 acties en data-gebeurtenissen kunt definiëren. Als u deze limiet overschrijdt probeert, ontvangt u compilatiefouten die er ongeveer als volgt:

  ![Schermafbeelding van de Macro Parameters compilatiefouten](media/iot-hub-device-sdk-c-serializer/02-nMacroParametersCompilerErrors.png)

De **nArithmetic** parameter wordt informatie over de interne werking van de macrotaal dan uw toepassing.  Hiermee kunt u het totale aantal leden die u in uw model hebben kunt, met inbegrip van **DECLARE_STRUCT** macro's. Als u start compilatiefouten zoals dit ziet, dan u verhogen proberen moet **nArithmetic**:

   ![Schermafbeelding van rekenkundige compilatiefouten](media/iot-hub-device-sdk-c-serializer/03-nArithmeticCompilerErrors.png)

Als u deze parameters wijzigt wilt, wijzigt u de waarden in de macro\_utils.tt bestand, het compileren van de macro\_utils\_h\_generator.sln oplossing, en voer het programma gecompileerd. Als u dit doet, een nieuwe macro\_utils.h bestand is gegenereerd en wordt geplaatst in de.\\ algemene\\inc directory.

Als u wilt gebruiken van de nieuwe versie van de macro\_utils.h, verwijdert de **serializer** NuGet-pakket van uw oplossing en in plaats daarvan zijn de **serializer** Visual Studio-project. Hierdoor kunnen uw code te compileren met de broncode van de serializer-bibliotheek. Dit omvat de bijgewerkte macro\_utils.h. Als u dit doen wilt voor **simplesample\_amqp**, starten door het verwijderen van het NuGet-pakket voor de bibliotheek serializer van de oplossing:

   ![Schermafbeelding van het verwijderen van het NuGet-pakket voor de serializer-bibliotheek](media/iot-hub-device-sdk-c-serializer/04-serializer-github-package.png)

Voeg vervolgens dit project toe aan uw Visual Studio-oplossing:

> . \\c\\serializer\\bouwen\\windows\\serializer.vcxproj
> 
> 

Wanneer u klaar bent, moet uw oplossing er als volgt:

   ![Schermafbeelding van de simplesample_amqp Visual Studio-oplossing](media/iot-hub-device-sdk-c-serializer/05-serializer-project.png)

Nu bij het compileren van uw oplossing, de bijgewerkte macro\_utils.h is opgenomen in het binaire bestand.

Houd er rekening mee dat het verhogen van deze waarden hoog genoeg compiler limieten kunt overschrijden. Tot op dit moment de **nMacroParameters** is de belangrijkste parameter waarmee u zorgen te maken. De specificaties C99 geeft aan dat een minimum van 127 parameters zijn toegestaan in een macrodefinitie van de. De Microsoft-compiler volgt de specificatie precies (en heeft een limiet van 127), zodat u het niet mogelijk om te verhogen **nMacroParameters** naast de standaardomgeving. Andere compilers mogelijk kunt u dit niet doet, (bijvoorbeeld de compiler GNU ondersteunt voor een hogere limiet).

Tot nu toe hebben we besproken vrijwel alles wat u moet weten over het schrijven van code met de **serializer** bibliotheek. Vóór de sluiting, gaan we terug naar bepaalde onderwerpen uit de vorige artikelen die u vraagt zich misschien af over.

## <a name="the-lower-level-apis"></a>De API's van lager niveau
De voorbeeldtoepassing op die in dit artikel gericht is **simplesample\_amqp**. In dit voorbeeld wordt de hoger niveau (de niet -**LLE**) API's voor het verzenden van gebeurtenissen en ontvangen van berichten. Als u deze API's gebruikt, wordt een achtergrond-thread wordt uitgevoerd die zorgt dat zowel het verzenden van gebeurtenissen en ontvangen van berichten. U kunt echter de op lagere niveaus (ES) API's gebruiken om te verwijderen van deze thread op de achtergrond en expliciete controle over wanneer u gebeurtenissen verzenden of ontvangen van berichten vanuit de cloud.

Zoals beschreven in een [vorige artikel](iot-hub-device-sdk-c-iothubclient.md), er is een set functies die uit de API's van een hoger niveau bestaat:

* IoTHubClient\_CreateFromConnectionString
* IoTHubClient\_SendEventAsync
* IoTHubClient\_SetMessageCallback
* IoTHubClient\_vernietigen

Deze API's worden geïllustreerd in **simplesample\_amqp**.

Er is ook een vergelijkbare set API's op lagere niveaus.

* IoTHubClient\_LLE\_CreateFromConnectionString
* IoTHubClient\_LLE\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_LLE\_vernietigen

Houd er rekening mee dat de API's van lager niveau exact dezelfde manier werken zoals beschreven in de vorige artikelen. U kunt de eerste set met API's gebruiken als u wilt dat een achtergrond-thread voor het afhandelen van gebeurtenissen verzenden en ontvangen berichten. U kunt de tweede set API's gebruiken als u wilt dat expliciete controle over wanneer u gegevens verzenden en ontvangen van IoT Hub. Een set API's werken net zo goed met de **serializer** bibliotheek.

Voor een voorbeeld van hoe de niveau-API's worden gebruikt met de **serializer** bibliotheek, Zie de **simplesample\_http** toepassing.

## <a name="additional-topics"></a>Extra onderwerpen
Enkele andere onderwerpen opgemerkt zijn opnieuw eigendom afhandelen, met behulp van alternatieve apparaatreferenties en configuratie-opties. Dit zijn alle onderwerpen aan bod een [vorige artikel](iot-hub-device-sdk-c-iothubclient.md). Het belangrijkste is punt is dat al deze functies op dezelfde manier als met werken de **serializer** bibliotheek als ze dit met doen de **IoTHubClient** bibliotheek. Bijvoorbeeld, als u wilt de eigenschappen koppelen aan een gebeurtenis uit het model, u **IoTHubMessage\_eigenschappen** en **kaart**\_**AddorUpdate**, op dezelfde manier als eerder beschreven:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Of de gebeurtenis is gegenereerd op basis van de **serializer** bibliotheek of gemaakt handmatig met behulp van de **IoTHubClient** bibliotheek maakt niet uit.

Voor de referenties van het alternatieve apparaat, met behulp van **IoTHubClient\_LLE\_maken** werkt net zo goed als **IoTHubClient\_CreateFromConnectionString** voor bij het toewijzen van een **IOTHUB\_CLIENT\_verwerken**.

Ten slotte, als u de **serializer** bibliotheek, kunt u configuratie-opties met instellen **IoTHubClient\_LLE\_SetOption** net als bij het gebruik van de **IoTHubClient** bibliotheek.

Een functie die uniek is voor de **serializer** bibliotheek worden de initialisatie van de API's. Voordat u met de bibliotheek werken beginnen kunt, moet u aanroepen **serializer\_init**:

```C
serializer_init(NULL);
```

Dit wordt gedaan voordat u aanroepen **IoTHubClient\_CreateFromConnectionString**.

Op dezelfde manier als u klaar bent met de bibliotheek werken, de laatste aanroep die u maakt, is het **serializer\_deinit**:

```C
serializer_deinit();
```

Anders alle van de bovenstaande functies werken op dezelfde manier de **serializer** bibliotheek als in de **IoTHubClient** bibliotheek. Zie voor meer informatie over deze onderwerpen, de [vorige artikel](iot-hub-device-sdk-c-iothubclient.md) in deze reeks.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven in detail de unieke aspecten van de **serializer** bibliotheek die deel uitmaken van de **Azure IoT device-SDK voor C**. Met de informatie die er is een goed begrip van hoe u modules gebruiken voor het verzenden van gebeurtenissen en ontvangen van berichten uit IoT Hub.

Hiermee ook de driedelige serie over het ontwikkelen van toepassingen met is de **Azure IoT device-SDK voor C**. Dit moet voldoende informatie niet alleen aan de slag, maar geeft u een grondig begrip van de werking van de API's. Voor meer informatie, moet u er een paar voorbeelden zijn in de SDK die hier niet wordt gedekt. Anders wordt de [Azure IoT SDK-documentatie](https://github.com/Azure/azure-iot-sdk-c) is een goede bron voor meer informatie.

Zie voor meer informatie over het ontwikkelen van IoT Hub, de [Azure IoT SDK's](iot-hub-devguide-sdks.md).

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md).