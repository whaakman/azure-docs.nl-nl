---
title: Azure IoT device-SDK voor C - IoTHubClient | Microsoft Docs
description: Het gebruik van de IoTHubClient-bibliotheek in de Azure IoT-device-SDK voor C om te maken van apps op apparaten die communiceren met een IoT-hub.
author: yzhong94
ms.service: iot-hub
services: iot-hub
ms.devlang: c
ms.topic: conceptual
ms.date: 08/29/2017
ms.author: yizhon
ms.openlocfilehash: dd3b693271326c85688a275a65b67ad6257220e3
ms.sourcegitcommit: f6050791e910c22bd3c749c6d0f09b1ba8fccf0c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2018
ms.locfileid: "50024755"
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Azure IoT device-SDK voor C: meer informatie over IoTHubClient

[Azure IoT device-SDK voor C](iot-hub-device-sdk-c-intro.md) is het eerste artikel in deze reeks Maak kennis met de **Azure IoT device-SDK voor C**. Dit artikel wordt uitgelegd dat er twee architectuur lagen in de SDK zijn. Tegen het basisrekentarief is de **IoTHubClient** bibliotheek die rechtstreeks de communicatie met IoT Hub beheert. Er is ook de **serializer** bibliotheek die op deze voortbouwt om serialisatie-services te bieden. In dit artikel krijgt u extra details op de **IoTHubClient** bibliotheek.

[!INCLUDE [iot-hub-basic](../../includes/iot-hub-basic-partial.md)]

Het vorige artikel wordt beschreven hoe u de **IoTHubClient** bibliotheek gebeurtenissen verzenden naar IoT Hub en kunnen berichten ontvangen. In dit artikel is een uitbreiding die discussie door een uitleg over het beheren van nauwkeuriger te *wanneer* verzenden en ontvangen van gegevens, introduceert u de **op lager niveau API's**. Ook wordt uitgelegd hoe u eigenschappen koppelen aan gebeurtenissen (en deze worden opgehaald van berichten) met behulp van de eigenschap voor het verwerken van functies in de **IoTHubClient** bibliotheek. Ten slotte, bieden we meer uitleg over de verschillende manieren voor het afhandelen van berichten ontvangen van IoT Hub.

Het artikel tot slot biedt die betrekking hebben op een aantal verschillende onderwerpen, waaronder meer informatie over apparaatreferenties en het wijzigen van het gedrag van de **IoTHubClient** door middel van configuratie-opties.

We gebruiken de **IoTHubClient** SDK-voorbeelden waarin wordt uitgelegd in deze onderwerpen. Als u volgen wilt, raadpleegt u de **iothub\_client\_voorbeeld\_http** en **iothub\_client\_voorbeeld\_amqp**toepassingen die zijn opgenomen in de Azure IoT-device-SDK voor c alles in de volgende secties wordt beschreven in deze voorbeelden wordt aangetoond.

U vindt de [ **Azure IoT device-SDK voor C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats en de weergave details van de API in de [C-API-verwijzing](https://docs.microsoft.com/azure/iot-hub/iot-c-sdk-ref/).

## <a name="the-lower-level-apis"></a>De API's van lager niveau

Het vorige artikel wordt beschreven voor de werking van de **IotHubClient** binnen de context van de **iothub\_client\_voorbeeld\_amqp** toepassing. Bijvoorbeeld, deze wordt uitgelegd hoe u de bibliotheek met behulp van deze code initialiseren.

```C
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Het is ook dat wordt beschreven hoe u gebeurtenissen verzenden met behulp van deze aanroep van de functie.

```C
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Het artikel wordt ook beschreven hoe u berichten ontvangen met een retouraanroepfunctie registreren.

```C
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Het artikel hebt u ook geleerd hoe u met behulp van code, zoals de volgende bronnen vrij te maken.

```C
IoTHubClient_Destroy(iotHubClientHandle);
```

Er zijn aanvullende functies voor elk van deze API's:

* IoTHubClient\_LLE\_CreateFromConnectionString
* IoTHubClient\_LLE\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_LLE\_vernietigen

Deze zijn allen inclusief functies **LLE** in naam van de API. Andere de **LLE** deel van de naam, de parameters van elk van deze functies zijn identiek aan hun tegenhangers niet alles. Echter, verschilt het gedrag van deze functies op één belangrijk.

Als u aanroept **IoTHubClient\_CreateFromConnectionString**, de onderliggende bibliotheken maakt u een nieuwe thread die wordt uitgevoerd op de achtergrond. Deze thread gebeurtenissen verzendt en ontvangt berichten van IoT-Hub. Er is geen deze thread is gemaakt bij het werken met de **LLE** API's. Het maken van de achtergrondthread is nuttig voor de ontwikkelaar. U hebt geen zorgen te hoeven maken over het expliciet gebeurtenissen verzenden en ontvangen van berichten uit IoT Hub--gebeurt automatisch op de achtergrond. Daarentegen de **LLE** API's bieden u expliciete controle over de communicatie met IoT Hub, als u deze nodig hebt.

Dit concept beter begrijpen, kunt u een voorbeeld laten we kijken:

Als u aanroept **IoTHubClient\_SendEventAsync**, wat u daadwerkelijk doet de gebeurtenis is plaatsen in een buffer. De thread op de achtergrond gemaakt bij het aanroepen van **IoTHubClient\_CreateFromConnectionString** voortdurend controleert deze buffer en alle gegevens die deze bevat naar IoT Hub verzendt. Dit gebeurt op de achtergrond op hetzelfde moment dat de hoofdthread ander werk uitvoert.

Op dezelfde manier als het registreren van een retouraanroepfunctie voor berichten met **IoTHubClient\_SetMessageCallback**, bent u de SDK om de achtergrondthread die de callbackfunctie aanroepen wanneer een bericht is instructie ontvangen, onafhankelijk van de hoofdthread.

De **LLE** API's een achtergrond-thread niet maken. In plaats daarvan moet een nieuwe API worden aangeroepen voor het expliciet verzenden en ontvangen van gegevens van IoT-Hub. Dit wordt geïllustreerd in het volgende voorbeeld.

De **iothub\_client\_voorbeeld\_http** toepassing die opgenomen in de SDK ziet u de API's van lager niveau. In dit voorbeeld verzenden we gebeurtenissen naar IoT Hub met code, zoals het volgende:

```C
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

De eerste drie regels maken het bericht en de laatste regel verzendt de gebeurtenis. Zoals eerder vermeld, moet u echter verzendt dat de gebeurtenis betekent dat de gegevens gewoon in een buffer wordt geplaatst. Niets op het netwerk wordt verzonden wanneer we noemen **IoTHubClient\_LLE\_SendEventAsync**. In de volgorde aan daadwerkelijk inkomend verkeer de gegevens uit naar IoT Hub, moet u aanroepen **IoTHubClient\_LLE\_DoWork**, zoals in dit voorbeeld:

```C
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(100);
}
```

Deze code (van de **iothub\_client\_voorbeeld\_http** toepassing) herhaaldelijk aanroept **IoTHubClient\_LLE\_DoWork**. Telkens wanneer **IoTHubClient\_LLE\_DoWork** is aangeroepen, stuurt het sommige gebeurtenissen uit de buffer naar IoT Hub en het ophalen van een bericht in de wachtrij worden verzonden naar het apparaat. Het laatste geval betekent dat als we een retouraanroepfunctie voor berichten geregistreerd, klikt u vervolgens de retouraanroep wordt aangeroepen (ervan uitgaande dat er berichten zijn in de wachtrij). Zou dat we een retouraanroepfunctie hebt geregistreerd met code, zoals het volgende:

```C
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

De reden dat **IoTHubClient\_LLE\_DoWork** vaak wordt aangeroepen in een lus is dat elke keer dat deze wordt aangeroepen, verzendt deze *sommige* buffer gebeurtenissen naar IoT Hub en haalt *de volgende* bericht dat in de wachtrij geplaatst voor het apparaat. Elke aanroep is niet noodzakelijkerwijs alle gebufferde gebeurtenissen verzenden of om op te halen van alle berichten in de wachtrij. Als u wilt verzenden van alle gebeurtenissen in de buffer en vervolgens gaat u verder met andere verwerking dat kunt u deze lus vervangen door de volgende code:

```C
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(100);
}
```

Deze code wordt de **IoTHubClient\_LLE\_DoWork** totdat alle gebeurtenissen in de buffer zijn verzonden naar IoT Hub. Houd er rekening mee dat dit ook betekent niet dat alle berichten in de wachtrij zijn ontvangen. Onderdeel van de reden hiervoor is dat als deterministisch een actie voor 'alle' berichten controleren niet. Wat gebeurt er als u 'alle' van de berichten ophalen, maar vervolgens een andere naam op het apparaat wordt verzonden zodra? Er is een betere manier om op te lossen die met een geprogrammeerde time-out. De bericht-callback-functie kan bijvoorbeeld een timer opnieuw telkens wanneer de toepassing wordt aangeroepen. Vervolgens kunt u logica om door te gaan met de verwerking als u bijvoorbeeld geen berichten zijn ontvangen in de afgelopen *X* seconden.

Wanneer u klaar ingressing gebeurtenissen en ontvangen van berichten, zorg ervoor dat de bijbehorende functie voor het opschonen van resources.

```C
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

In feite is er slechts één set API's voor het verzenden en ontvangen van gegevens met een achtergrond-thread en een andere reeks API's die hetzelfde zonder de achtergrondthread doet. Veel ontwikkelaars verkiezen boven de niet - LLE API's, maar de API's van lager niveau zijn nuttig wanneer de ontwikkelaar wil expliciete controle over het netwerk. Bijvoorbeeld: sommige apparaten gegevens gedurende een bepaalde tijd alleen binnenkomende gebeurtenissen verzamelen met opgegeven tussenpozen (bijvoorbeeld één keer een uur of één keer per dag). De API's van lager niveau bieden u de mogelijkheid om te expliciet bepalen bij het verzenden en ontvangen van gegevens van IoT-Hub. Anderen wordt gewoon de voorkeur aan dat de op lager niveau API's bieden. Alles wat er gebeurt op de hoofdthread in plaats van enkele gebeurt werk op de achtergrond.

Het model dat u kiest, moet u consistent in welke API's die u gebruikt. Als u door het aanroepen van begint **IoTHubClient\_LLE\_CreateFromConnectionString**, zorg ervoor dat u alleen de bijbehorende op lagere niveaus-API's gebruiken voor follow-up werk:

* IoTHubClient\_LLE\_SendEventAsync
* IoTHubClient\_LL\_SetMessageCallback
* IoTHubClient\_LLE\_vernietigen
* IoTHubClient\_LLE\_DoWork

Het omgekeerde geldt ook. Als u met begint **IoTHubClient\_CreateFromConnectionString**, klikt u vervolgens de niet - LLE API's gebruiken voor alle verdere verwerking.

In de Azure IoT device-SDK voor C, Zie de **iothub\_client\_voorbeeld\_http** toepassing voor een compleet voorbeeld van de API's van lager niveau. De **iothub\_client\_voorbeeld\_amqp** toepassing kan worden verwezen voor een compleet voorbeeld van de niet - LLE API's.

## <a name="property-handling"></a>Afhandeling van eigenschap

Tot nu toe wanneer we verzenden van gegevens hebt die worden beschreven, we hebben zijn die verwijzen naar de hoofdtekst van het bericht. Bijvoorbeeld, houd rekening met deze code:

```C
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

In dit voorbeeld verzendt een bericht naar IoT Hub met de tekst "Hallo wereld". IoT Hub kan echter ook eigenschappen die moeten worden gekoppeld aan elk bericht. Eigenschappen zijn naam/waarde-paren die kunnen worden gekoppeld aan het bericht. Bijvoorbeeld, wijzigen we de vorige code voor het koppelen van een eigenschap aan het bericht:

```C
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

We beginnen door het aanroepen van **IoTHubMessage\_eigenschappen** en daaraan de ingang van ons bericht. Wat we teruggaan is een **kaart\_verwerken** verwijzing die kan we beginnen met het toevoegen van eigenschappen. De laatste wordt bereikt door het aanroepen van **kaart\_AddOrUpdate**, die een verwijzing naar een kaart neemt\_INGANG, de eigenschapsnaam en de waarde van eigenschap. Met deze API kunnen we zo veel eigenschappen als we willen toevoegen.

Wanneer de gebeurtenis wordt gelezen uit **Event Hubs**, de ontvanger kan opsommen van de eigenschappen en de bijbehorende waarden ophalen. Bijvoorbeeld, in .NET dit zou worden bereikt door het openen van de [verzameling eigenschappen op het EventData-object](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

In het vorige voorbeeld hebt we eigenschappen koppelen aan een gebeurtenis die we naar IoT Hub verzenden. Eigenschappen kunnen ook worden gekoppeld aan berichten ontvangen van IoT Hub. Als we halen eigenschappen op van een bericht willen, kunnen we code, zoals het volgende gebruiken in onze retouraanroepfunctie bericht:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .

    // Retrieve properties from the message
    MAP_HANDLE mapProperties = IoTHubMessage_Properties(message);
    if (mapProperties != NULL)
    {
        const char*const* keys;
        const char*const* values;
        size_t propertyCount = 0;
        if (Map_GetInternals(mapProperties, &keys, &values, &propertyCount) == MAP_OK)
        {
            if (propertyCount > 0)
            {
                printf("Message Properties:\r\n");
                for (size_t index = 0; index < propertyCount; index++)
                {
                    printf("\tKey: %s Value: %s\r\n", keys[index], values[index]);
                }
                printf("\r\n");
            }
        }
    }

    . . .
}
```

De aanroep van **IoTHubMessage\_eigenschappen** retourneert de **kaart\_verwerken** verwijzing. Vervolgens geven we dat verwijzing naar **kaart\_GetInternals** verkrijgen van een verwijzing naar een matrix met de naam/waarde-paren (evenals een aantal van de eigenschappen). Op dat moment is het een kwestie van het inventariseren van eigenschappen van het ophalen om de waarden die we willen.

U hebt geen eigenschappen in uw toepassing gebruiken. Echter, als u deze instellen op gebeurtenissen of deze worden opgehaald van berichten, moet de **IoTHubClient** -bibliotheek kunnen eenvoudig.

## <a name="message-handling"></a>Afhandeling van berichten

Zoals eerder gezegd, als berichten uit IoT Hub de **IoTHubClient** bibliotheek reageert door een geregistreerde retouraanroepfunctie aan te roepen. Er is een retourparameter van deze functie die aanvullende uitleg verdient. Hier volgt een fragment van de callbackfunctie in de **iothub\_client\_voorbeeld\_http** voorbeeldtoepassing:

```C
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Houd er rekening mee dat het retourtype is **IOTHUBMESSAGE\_toestand\_resultaat** en in dit geval wordt geretourneerd **IOTHUBMESSAGE\_GEACCEPTEERDE**. Er zijn andere waarden we van deze functie kan retourneren die wijzigen hoe de **IoTHubClient** bibliotheek reageert op de bericht-callback. Hier zijn de opties.

* **IOTHUBMESSAGE\_GEACCEPTEERDE** : het bericht met succes is verwerkt. De **IoTHubClient** bibliotheek wordt niet de callback-functie opnieuw met hetzelfde bericht worden aangeroepen.

* **IOTHUBMESSAGE\_afgewezen** : het bericht is niet verwerkt en er is geen wens om dit te doen in de toekomst. De **IoTHubClient** bibliotheek mag niet de callback-functie opnieuw met hetzelfde bericht worden aangeroepen.

* **IOTHUBMESSAGE\_ABANDONED** : het bericht is niet verwerkt, maar de **IoTHubClient** bibliotheek moet de callbackfunctie opnieuw met hetzelfde bericht aanroepen.

Voor de eerste twee retourcodes, de **IoTHubClient** bibliotheek verzendt een bericht naar IoT Hub waarmee wordt aangegeven dat het bericht moet worden verwijderd uit de apparaatwachtrij en niet opnieuw worden bezorgd. Het uiteindelijke resultaat is hetzelfde (het bericht is verwijderd uit de apparaatwachtrij), maar nog steeds of het bericht is geaccepteerd of geweigerd wordt vastgelegd.  Dit verschil op te nemen is handig om afzenders van het bericht die kunnen luisteren naar feedback en zoek uit of een apparaat heeft geaccepteerd of een bepaald bericht geweigerd.

In het laatste geval wordt ook een bericht verzonden naar IoT Hub, maar Hiermee wordt aangegeven dat het bericht moet opnieuw worden bezorgd. Meestal zult u een bericht afbreken als u er een fout optreden, maar wilt u het bericht te verwerken opnieuw proberen. Daarentegen is afwijzen van een bericht geschikt als er een onherstelbare fout opgetreden (of als u besluit hoeft dat u niet wilt dat het bericht te verwerken).

In elk geval worden op de hoogte van de verschillende retourcodes zodat u kunt het gewenste gedrag werkdag de **IoTHubClient** bibliotheek.

## <a name="alternate-device-credentials"></a>Alternatieve apparaatreferenties

Zoals hierboven is uitgelegd, het eerste wat te doen als u werkt met de **IoTHubClient** bibliotheek is op te halen een **IOTHUB\_CLIENT\_verwerken** met een aanroep van zoals het volgende:

```C
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

De argumenten voor **IoTHubClient\_CreateFromConnectionString** zijn de apparaatverbindingsreeks en een parameter die aangeeft van het protocol dat we gebruiken om te communiceren met IoT Hub. De verbindingsreeks van het apparaat heeft een indeling die wordt weergegeven als volgt:

```C
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Er zijn vier soorten informatie in deze tekenreeks: de naam van IoT Hub, IoT Hub-achtervoegsel, apparaat-ID en gedeelde toegangssleutel. Ophalen van de volledig gekwalificeerde domeinnaam (FQDN) van een IoT-hub wanneer u uw IoT hub-exemplaar in Azure portal maakt: dit geeft u de naam van de IoT hub (het eerste deel van de FQDN-naam) en het achtervoegsel in IoT hub (de rest van de FQDN-naam). U profiteert de apparaat-ID en de gedeelde toegangssleutel wanneer u uw apparaat bij IoT Hub registreren (zoals beschreven in de [vorige artikel](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** beschikt u over een manier om de bibliotheek te initialiseren. Als u liever, kunt u een nieuwe **IOTHUB\_CLIENT\_verwerken** met behulp van deze afzonderlijke parameters in plaats van de verbindingsreeks van het apparaat. Dit wordt bereikt door de volgende code:

```C
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Dit gebeurt hetzelfde als **IoTHubClient\_CreateFromConnectionString**.

Er lijken misschien overduidelijk dat u wilt gebruiken **IoTHubClient\_CreateFromConnectionString** in plaats van deze uitgebreidere methode van de initialisatie. Houd er rekening mee, echter dat een apparaat in IoT Hub te registreren krijgt u een apparaat-ID en de apparaatsleutel (niet een verbindingsreeks). De *apparatenverkenner* SDK-hulpprogramma is geïntroduceerd in de [vorige artikel](iot-hub-device-sdk-c-intro.md) maakt gebruik van bibliotheken in de **Azure IoT service SDK** om te maken van de verbindingsreeks van de apparaat-ID , sleutelwaarde en de IoT Hub-hostnaam. Dus aanroepen **IoTHubClient\_LLE\_maken** wellicht beter is omdat u de stap voor het genereren van een verbindingsreeks. Gebruik de methode is handig.

## <a name="configuration-options"></a>Configuratie-opties

Tot nu toe alles beschreven over de manier waarop de **IoTHubClient** bibliotheek werkt weerspiegelt het standaardgedrag ervan. Er zijn echter een aantal opties die u instellen kunt om te wijzigen van de werking van de bibliotheek. Dit wordt bereikt door gebruik te maken van de **IoTHubClient\_LLE\_SetOption** API. Bekijk het volgende voorbeeld:

```C
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Er zijn een aantal opties die vaak worden gebruikt:

* **SetBatching** (bool) – als **waar**, en vervolgens de gegevens die worden verzonden naar IoT Hub wordt verzonden in batches. Als **false**, en moeten afzonderlijk worden verzonden. De standaardwaarde is **false**. Houd er rekening mee dat de **SetBatching** optie is alleen van toepassing op het HTTPS-protocol en niet op de protocollen MQTT- of AMQP.

* **Time-out** (niet-ondertekende int): deze waarde wordt weergegeven in milliseconden. Als het verzenden van een HTTPS-aanvraag of een reactie ontvangen langer duurt dan de opgegeven tijd, en vervolgens de verbinding een time-out optreedt.

De batchverwerkingsindeling optie is belangrijk. Standaard wordt in de bibliotheek ingresses gebeurtenissen afzonderlijk (één gebeurtenis is wat u doorgeeft aan **IoTHubClient\_LLE\_SendEventAsync**). Als de batchverwerkingsindeling optie **waar**, de bibliotheek verzamelt zo veel gebeurtenissen zoals deze kan uit de buffer (maximaal de maximale berichtgrootte die IoT Hub worden geaccepteerd).  De batch gebeurtenis wordt verzonden naar IoT Hub in één HTTPS-aanroep (de afzonderlijke gebeurtenissen zijn gebundeld in een JSON-matrix). Inschakelen van batchverwerking doorgaans resulteert in grote prestatiewinst omdat u bent netwerk retouren verminderen. Bovendien vermindert bandbreedte omdat u bij het verzenden van een set van HTTPS-headers met een gebeurtenis batch in plaats van een set van headers voor elke afzonderlijke gebeurtenis. Tenzij u een specifieke reden anders hebt, moet doorgaans u om in te schakelen via batchverwerking uitvoeren.

## <a name="next-steps"></a>Volgende stappen

Dit artikel wordt beschreven in de details van het gedrag van de **IoTHubClient** bibliotheek gevonden in de **Azure IoT device-SDK voor C**. Met deze informatie, moet u een goed begrip van de mogelijkheden van de **IoTHubClient** bibliotheek. Het tweede artikel in deze reeks is [Azure IoT device-SDK voor C - Serializer](iot-hub-device-sdk-c-serializer.md), waarmee u dergelijke gegevens op de **serializer** bibliotheek.

Zie voor meer informatie over het ontwikkelen van IoT Hub, de [Azure IoT SDK's](iot-hub-devguide-sdks.md).

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie [AI implementeren op edge-apparaten met Azure IoT Edge](../iot-edge/tutorial-simulate-device-linux.md).