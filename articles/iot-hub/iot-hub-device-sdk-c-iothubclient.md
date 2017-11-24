---
title: Azure IoT-apparaat SDK voor C - IoTHubClient | Microsoft Docs
description: Het gebruik van de bibliotheek IoTHubClient in de Azure-IoT-apparaat-SDK voor C apps voor apparaten die communiceren met een IoT-hub maken.
services: iot-hub
documentationcenter: 
author: olivierbloch
manager: timlt
editor: 
ms.assetid: 828cf2bf-999d-4b8a-8a28-c7c901629600
ms.service: iot-hub
ms.devlang: cpp
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/29/2017
ms.author: obloch
ms.openlocfilehash: 8428857bcd444f99ba2c0f6b31ff662d5596b591
ms.sourcegitcommit: 933af6219266cc685d0c9009f533ca1be03aa5e9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/18/2017
---
# <a name="azure-iot-device-sdk-for-c--more-about-iothubclient"></a>Azure IoT-apparaat SDK voor C: meer informatie over IoTHubClient
De [eerst artikel](iot-hub-device-sdk-c-intro.md) in deze reeks geïntroduceerd de **Azure IoT-device SDK voor C**. Dit artikel worden beschreven dat er twee architectuur lagen SDK zijn. Aan de basis is de **IoTHubClient** bibliotheek die rechtstreeks communicatie met IoT Hub beheert. Er is ook de **serialisatiefunctie** bibliotheek die op deze voortbouwt om serialisatie-services te bieden. In dit artikel bieden we aanvullende details over de **IoTHubClient** bibliotheek.

Het vorige artikel wordt beschreven hoe u de **IoTHubClient** bibliotheek voor het verzenden van gebeurtenissen naar IoT Hub en ontvangen van berichten. In dit artikel wordt uitgebreid die discussie waarin wordt uitgelegd hoe nauwkeuriger beheren *wanneer* verzenden en ontvangen van gegevens, inleiding tot u de **lager niveau API's**. Ook wordt uitgelegd hoe u eigenschappen koppelen aan gebeurtenissen (en berichten ophalen) met de eigenschap afhandeling van functies in de **IoTHubClient** bibliotheek. Ten slotte, bieden we extra uitleg van de verschillende manieren voor het afhandelen van berichten uit IoT Hub is ontvangen.

Het artikel tot slot die betrekking hebben op een aantal verschillende onderwerpen, waaronder meer over het apparaat en het wijzigen van het gedrag van de **IoTHubClient** via configuratieopties.

We gebruiken de **IoTHubClient** SDK-voorbeelden in deze onderwerpen waarin wordt uitgelegd. Als u volgen wilt, raadpleegt u de **iothub\_client\_voorbeeld\_http** en **iothub\_client\_voorbeeld\_amqp** toepassingen die zijn opgenomen in de Azure IoT-device SDK voor C. alles dat wordt beschreven in de volgende secties wordt uitgelegd in deze voorbeelden.

U vindt de [ **Azure IoT-device SDK voor C** ](https://github.com/Azure/azure-iot-sdk-c) GitHub-opslagplaats en de weergave details van de API in de [C-API-referentiemateriaal](https://azure.github.io/azure-iot-sdk-c/index.html).

## <a name="the-lower-level-apis"></a>De API's van lager niveau
Het vorige artikel wordt beschreven voor de werking van de **IotHubClient** binnen de context van de **iothub\_client\_voorbeeld\_amqp** toepassing. Bijvoorbeeld, worden bij het initialiseren van de tapewisselaar met deze code beschreven.

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

Het beschreven verzenden van gebeurtenissen met de functieaanroep van deze ook.

```
IoTHubClient_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message);
```

Het artikel wordt ook beschreven hoe u berichten ontvangen via het registreren van een callback-functie.

```
int receiveContext = 0;
IoTHubClient_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext);
```

Het artikel hebt u ook geleerd hoe u met code zoals de volgende bronnen vrij te maken.

```
IoTHubClient_Destroy(iotHubClientHandle);
```

Er zijn echter aanvullende functies aan elk van deze API's:

* IoTHubClient\_LLE\_CreateFromConnectionString
* IoTHubClient\_LLE\_SendEventAsync
* IoTHubClient\_LLE\_SetMessageCallback
* IoTHubClient\_LLE\_vernietigen

Deze functies alle opnemen 'LLE' in de naam van de API. De parameters van elk van deze functies zijn dan die identiek is aan hun niet LLE collega's. Het gedrag van deze functies verschilt echter op één belangrijk.

Als u aanroept **IoTHubClient\_CreateFromConnectionString**, de onderliggende bibliotheken maken in een nieuwe thread die op de achtergrond uitgevoerd. Deze thread gebeurtenissen verzendt en ontvangt berichten uit IoT Hub. Er is geen dergelijke thread gemaakt bij het werken met de API's van 'LLE'. Het maken van de achtergrondthread is ten behoeve van de ontwikkelaar. U hoeft niet te hoeven maken over expliciet gebeurtenissen verzenden en ontvangen van berichten uit IoT Hub--gebeurt automatisch op de achtergrond. Daarentegen bieden 'LLE' API's u een expliciete controle over de communicatie met IoT Hub, als u deze nodig hebt.

Om dit beter te begrijpen, bekijk een voorbeeld:

Als u aanroept **IoTHubClient\_SendEventAsync**, wat u daadwerkelijk doet de gebeurtenis is als in een buffer. De achtergrondthread die is gemaakt bij het aanroepen van **IoTHubClient\_CreateFromConnectionString** voortdurend bewaakt deze buffer en alle gegevens die deze bevat naar IoT Hub verzendt. Dit gebeurt op de achtergrond op hetzelfde moment de hoofdthread uitvoeren van andere taken.

Op dezelfde manier als het registreren van een callback-functie voor berichten met **IoTHubClient\_SetMessageCallback**, bent u de SDK om de achtergrondthread die de callback-functie aanroepen wanneer een bericht ontvangen, onafhankelijk van de hoofdthread is instructie.

De API's van 'LLE' Maak een achtergrond-thread. In plaats daarvan moet een nieuwe API worden aangeroepen om expliciet verzenden en ontvangen van gegevens uit IoT Hub. Dit wordt geïllustreerd in het volgende voorbeeld.

De **iothub\_client\_voorbeeld\_http** toepassing die opgenomen in de SDK ziet u de API's van lager niveau. In dit voorbeeld verzenden we gebeurtenissen naar IoT Hub met bijvoorbeeld de volgende code:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Message_%d_From_IoTHubClient_LL_Over_HTTP", i);
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));

IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

De eerste drie regels maken het bericht en de laatste regel verzendt de gebeurtenis. Echter, zoals eerder vermeld, 'verzenden' de gebeurtenis betekent dat de gegevens gewoon wordt geplaatst in een buffer. Niets op het netwerk worden verzonden wanneer we aanroepen **IoTHubClient\_LLE\_SendEventAsync**. Om te daadwerkelijk inkomend de gegevens naar IoT Hub, moet u aanroepen **IoTHubClient\_LLE\_DoWork**, zoals in dit voorbeeld:

```
while (1)
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Deze code (van de **iothub\_client\_voorbeeld\_http** toepassing) herhaaldelijk roept **IoTHubClient\_LLE\_DoWork**. Elke keer **IoTHubClient\_LLE\_DoWork** is aangeroepen, stuurt sommige gebeurtenissen uit de buffer naar IoT Hub en het ophalen van een bericht in de wachtrij worden verzonden naar het apparaat. Het laatste geval betekent dat als er een callback-functie voor berichten geregistreerd, klikt u vervolgens de callback is aangeroepen (ervan uitgaande dat alle berichten worden in de wachtrij geplaatst). We zou hebben een callbackfunctie geregistreerd met bijvoorbeeld de volgende code:

```
IoTHubClient_LL_SetMessageCallback(iotHubClientHandle, ReceiveMessageCallback, &receiveContext)
```

De reden dat **IoTHubClient\_LLE\_DoWork** heet vaak in een lus is dat elke keer dat deze wordt aangeroepen, verzendt *sommige* buffer opgeslagen gebeurtenissen naar IoT Hub en haalt *de volgende* bericht staat in de wachtrij voor het apparaat. Elke aanroep wordt niet gegarandeerd alle buffer opgeslagen gebeurtenissen verzenden of om op te halen alle berichten in de wachtrij. Als u wilt verzendt, worden alle gebeurtenissen in de buffer en ga vervolgens door op met de andere verwerking dat kunt u deze lus vervangen door de volgende code:

```
IOTHUB_CLIENT_STATUS status;

while ((IoTHubClient_LL_GetSendStatus(iotHubClientHandle, &status) == IOTHUB_CLIENT_OK) && (status == IOTHUB_CLIENT_SEND_STATUS_BUSY))
{
    IoTHubClient_LL_DoWork(iotHubClientHandle);
    ThreadAPI_Sleep(1000);
}
```

Deze code roept **IoTHubClient\_LLE\_DoWork** totdat alle gebeurtenissen in de buffer met IoT Hub zijn verzonden. Houd er rekening mee dat dit ook impliceert niet alle berichten in de wachtrij is ontvangen. Deel van de reden hiervoor is dat als deterministische een actie controleren of er 'alle' berichten niet. Wat gebeurt er als u 'alle' berichten ophalen, maar wordt nog een naar het apparaat verzonden onmiddellijk na? Er is een betere manier om te gaan met die met een geprogrammeerde time-out. De callback-functie van het bericht kan bijvoorbeeld een timer ingesteld telkens wanneer deze wordt aangeroepen. U kunt vervolgens schrijven logica om door te gaan met de verwerking als u bijvoorbeeld geen berichten ontvangen in de afgelopen *X* seconden.

Wanneer u bent klaar ingressing gebeurtenissen en ontvangen van berichten, zorg ervoor dat de bijbehorende functie om resources op te schonen.

```
IoTHubClient_LL_Destroy(iotHubClientHandle);
```

In feite is er slechts één set API's voor het verzenden en ontvangen van gegevens met een achtergrond-thread en een andere set API's die hetzelfde zonder de achtergrondthread doet. Veel van ontwikkelaars verkiezen boven de niet - LLE API's, maar de API's van lager niveau zijn nuttig wanneer de ontwikkelaar van de expliciete controle over Netwerkoverdrachten wil. Bijvoorbeeld: op sommige apparaten verzamelen van gegevens gedurende een bepaalde tijd alleen ingangsgebeurtenissen tussenpozen (bijvoorbeeld één keer een uur of één keer per dag). De API's van lager niveau bieden de mogelijkheid tot expliciet besturingselement bij het verzenden en ontvangen van gegevens uit IoT Hub. Anderen wordt gewoon de voorkeur aan waarmee de API's van lager niveau. Alles wordt uitgevoerd op de hoofdthread in plaats van sommige gebeurt werk op de achtergrond.

Het model dat u kiest, moet u consistent in welke API's die u gebruikt. Als u met het aanroepen van Start **IoTHubClient\_LLE\_CreateFromConnectionString**, zorg ervoor dat u alleen de bijbehorende lager niveau API's gebruiken voor een vervolgzelfstudie werk:

* IoTHubClient\_LLE\_SendEventAsync
* IoTHubClient\_LLE\_SetMessageCallback
* IoTHubClient\_LLE\_vernietigen
* IoTHubClient\_LLE\_DoWork

Het omgekeerde geldt ook. Als u met opstart **IoTHubClient\_CreateFromConnectionString**, gebruikt u de niet - LLE API's voor alle verdere verwerking.

Zie het apparaat Azure IoT SDK voor C de **iothub\_client\_voorbeeld\_http** toepassing voor een compleet voorbeeld van de API's van lager niveau. De **iothub\_client\_voorbeeld\_amqp** toepassing voor een compleet voorbeeld van de niet - LLE API's kan worden verwezen.

## <a name="property-handling"></a>Verwerking van de eigenschap
Tot nu toe wanneer we verzenden van gegevens hebt die worden beschreven, hebt we is verwijst naar de hoofdtekst van het bericht. Neem bijvoorbeeld deze code:

```
EVENT_INSTANCE message;
sprintf_s(msgText, sizeof(msgText), "Hello World");
message.messageHandle = IoTHubMessage_CreateFromByteArray((const unsigned char*)msgText, strlen(msgText));
IoTHubClient_LL_SendEventAsync(iotHubClientHandle, message.messageHandle, SendConfirmationCallback, &message)
```

In dit voorbeeld verzendt een bericht naar IoT Hub met de tekst "Hallo wereld". IoT Hub kan echter ook eigenschappen die moeten worden gekoppeld aan elk bericht. Eigenschappen zijn naam/waarde-paren die kunnen worden gekoppeld aan het bericht. Bijvoorbeeld, kunnen we de vorige code voor het toevoegen van een eigenschap aan het bericht wijzigen:

```
MAP_HANDLE propMap = IoTHubMessage_Properties(message.messageHandle);
sprintf_s(propText, sizeof(propText), "%d", i);
Map_AddOrUpdate(propMap, "SequenceNumber", propText);
```

Begin met het aanroepen van **IoTHubMessage\_eigenschappen** en door te geven de ingang van ons bericht. Wat we terughalen is een **kaart\_verwerken** verwijzing waarmee we beginnen met het toevoegen van eigenschappen. De laatste wordt bereikt door het aanroepen van **kaart\_AddOrUpdate**, wat een verwijzing naar een kaart duurt\_INGANG, de eigenschapsnaam en de waarde van eigenschap. Met deze API kunnen we zoveel eigenschappen als we wilt toevoegen.

Wanneer de gebeurtenis wordt gelezen uit **Event Hubs**, de ontvanger kan opsommen van de eigenschappen en de bijbehorende waarden ophalen. Bijvoorbeeld in .NET zou hiertoe toegang tot de [verzameling eigenschappen op het object EventData](https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.eventdata.properties.aspx).

In het vorige voorbeeld koppelt we eigenschappen aan een gebeurtenis die we naar IoT Hub sturen. Eigenschappen kunnen ook worden gekoppeld aan berichten uit IoT Hub is ontvangen. Als we halen eigenschappen op van een bericht willen, kunnen we de volgende code gebruiken in onze retouraanroepfunctie bericht:

```
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

De aanroep van **IoTHubMessage\_eigenschappen** retourneert de **kaart\_verwerken** verwijzing. We deze verwijzing naar geeft **kaart\_GetInternals** verkrijgen van een verwijzing naar een matrix met de naam/waarde-paren (evenals een aantal van de eigenschappen). Op dat moment is een kwestie van het inventariseren van de eigenschappen te krijgen tot de waarden die we willen.

U hoeft niet te gebruiken van eigenschappen in uw toepassing. Echter, als u deze instelt op gebeurtenissen of deze worden opgehaald van berichten, moet de **IoTHubClient** bibliotheek kunt u gemakkelijk.

## <a name="message-handling"></a>Afhandeling van berichten
Zoals eerder gezegd, wanneer berichten uit IoT Hub de **IoTHubClient** bibliotheek antwoordt door een geregistreerde callback-functie wordt aangeroepen. Er is een retourparameter van deze functie die een extra verklaring verdient. Hier volgt een fragment van de callbackfunctie in de **iothub\_client\_voorbeeld\_http** voorbeeldtoepassing:

```
static IOTHUBMESSAGE_DISPOSITION_RESULT ReceiveMessageCallback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
{
    . . .
    return IOTHUBMESSAGE_ACCEPTED;
}
```

Denk eraan dat het retourtype **IOTHUBMESSAGE\_toestand\_resultaat** en in dit geval wordt teruggeplaatst **IOTHUBMESSAGE\_GEACCEPTEERDE**. Er zijn andere waarden we kunnen van deze functie worden geretourneerd die wijzigen hoe de **IoTHubClient** bibliotheek reageert op de bericht-callback. Hier vindt u de opties.

* **IOTHUBMESSAGE\_GEACCEPTEERDE** – het bericht is verwerkt. De **IoTHubClient** bibliotheek wordt niet de callback-functie opnieuw met hetzelfde bericht worden aangeroepen.
* **IOTHUBMESSAGE\_geweigerd** : het bericht is niet verwerkt en er is geen wenst te doen in de toekomst. De **IoTHubClient** bibliotheek mag niet de callback-functie opnieuw met hetzelfde bericht worden aangeroepen.
* **IOTHUBMESSAGE\_ABANDONED** – het bericht is niet verwerkt, maar de **IoTHubClient** bibliotheek moet de callback-functie opnieuw met hetzelfde bericht worden aangeroepen.

Voor de eerste twee retourcodes de **IoTHubClient** bibliotheek verzendt een bericht naar een IoT-Hub die aangeeft dat het bericht moet worden verwijderd uit de apparaatwachtrij en niet opnieuw worden bezorgd. Het uiteindelijke resultaat is hetzelfde (het bericht is verwijderd uit de apparaatwachtrij), maar Hiermee wordt aangegeven of het bericht is goedgekeurd of geweigerd nog steeds wordt vastgelegd.  Dit verschil Recording is handig voor afzenders van het bericht die luisteren naar feedback en weten als een apparaat heeft geaccepteerd of een bepaald bericht afgewezen.

In het laatste geval wordt ook een bericht verstuurd naar IoT Hub, maar Hiermee wordt aangegeven dat het bericht opnieuw moet worden bezorgd. Meestal zult u een bericht afbreken als u er een fout optreden maar wilt u proberen het bericht om opnieuw te verwerken. Daarentegen is weigeren van een bericht geschikt als er een onherstelbare fout optreden (of als u gewoon besluit dat u niet wilt dat het bericht te verwerken).

In elk geval worden op de hoogte van de andere retourcodes zodat u kunt het gedrag van de gewenste roepen de **IoTHubClient** bibliotheek.

## <a name="alternate-device-credentials"></a>Alternatieve apparaat referenties
Zoals hierboven is uitgelegd, het eerste wat te doen als u werkt met de **IoTHubClient** bibliotheek is op te halen een **IOTHUB\_CLIENT\_verwerken** met een aanroep van zoals het volgende:

```
IOTHUB_CLIENT_HANDLE iotHubClientHandle;
iotHubClientHandle = IoTHubClient_CreateFromConnectionString(connectionString, AMQP_Protocol);
```

De argumenten voor **IoTHubClient\_CreateFromConnectionString** zijn de verbindingsreeks van het apparaat en een parameter die aangeeft van het protocol dat we gebruiken om te communiceren met IoT Hub. De verbindingsreeks van het apparaat heeft een indeling die wordt weergegeven als volgt:

```
HostName=IOTHUBNAME.IOTHUBSUFFIX;DeviceId=DEVICEID;SharedAccessKey=SHAREDACCESSKEY
```

Er zijn vier stukjes informatie in deze tekenreeks: naam van de IoT Hub, achtervoegsel IoT Hub, apparaat-ID en gedeelde toegangssleutel. U kunt de volledig gekwalificeerde domeinnaam (FQDN) van een IoT-hub verkrijgen wanneer u uw IoT hub-instantie in de Azure-portal maakt, Hierdoor krijgt u de naam van de IoT hub (het eerste deel van de FQDN-naam) en het achtervoegsel voor de IoT hub (de rest van de FQDN-naam). U krijgt de apparaat-ID en de gedeelde toegangssleutel wanneer u uw apparaat met IoT Hub registreren (zoals beschreven in de [vorige artikel](iot-hub-device-sdk-c-intro.md)).

**IoTHubClient\_CreateFromConnectionString** biedt u één manier om de bibliotheek te initialiseren. Als u liever, kunt u een nieuwe **IOTHUB\_CLIENT\_verwerken** met behulp van deze afzonderlijke parameters in plaats van de verbindingsreeks van het apparaat. Dit wordt bereikt met de volgende code:

```
IOTHUB_CLIENT_CONFIG iotHubClientConfig;
iotHubClientConfig.iotHubName = "";
iotHubClientConfig.deviceId = "";
iotHubClientConfig.deviceKey = "";
iotHubClientConfig.iotHubSuffix = "";
iotHubClientConfig.protocol = HTTP_Protocol;
IOTHUB_CLIENT_HANDLE iotHubClientHandle = IoTHubClient_LL_Create(&iotHubClientConfig);
```

Dit hetzelfde als bewerkstelligt **IoTHubClient\_CreateFromConnectionString**.

Het lijkt misschien duidelijk dat u wilt gebruiken **IoTHubClient\_CreateFromConnectionString** in plaats van deze uitgebreidere methode van de initialisatie. Houd er rekening mee echter, wanneer u een apparaat in IoT Hub registreren wat er een apparaat-ID en de apparaatsleutel (niet een verbindingsreeks is). De *apparaat explorer* SDK hulpprogramma geïntroduceerd in de [vorige artikel](iot-hub-device-sdk-c-intro.md) maakt gebruik van bibliotheken in de **Azure IoT service SDK** maken van de verbindingsreeks van het apparaat van de apparaat-ID, de sleutel voor apparaat en IoT Hub-hostnaam. Aanroepen van dus **IoTHubClient\_LLE\_maken** wellicht de voorkeur omdat u de stap voor het genereren van een verbindingsreeks. Gebruik de methode is handig.

## <a name="configuration-options"></a>Configuratie-opties
Tot nu toe alles beschreven over de manier waarop de **IoTHubClient** bibliotheek werkt weerspiegelt het standaardgedrag. Er zijn echter een aantal opties die u instellen kunt op de werking van de bibliotheek wijzigen. Dit wordt bereikt door het gebruik van de **IoTHubClient\_LLE\_SetOption** API. Bekijk het volgende voorbeeld:

```
unsigned int timeout = 30000;
IoTHubClient_LL_SetOption(iotHubClientHandle, "timeout", &timeout);
```

Er zijn een aantal opties die vaak worden gebruikt:

* **SetBatching** (bool) – als **true**, en vervolgens de gegevens die worden verzonden naar IoT Hub wordt verzonden in batches. Als **false**, en vervolgens berichten afzonderlijk worden verzonden. De standaardwaarde is **false**. Houd er rekening mee dat de **SetBatching** optie is alleen van toepassing op het HTTPS-protocol en niet op de protocollen MQTT of AMQP zijn.
* **Time-out** (niet-ondertekende int): deze waarde wordt weergegeven in milliseconden. Als stuurt een HTTPS-aanvraag of antwoord ontvangen duurt langer dan de opgegeven tijd, klikt u vervolgens de verbinding een time-out optreedt.

De optie batchen is belangrijk. Standaard wordt in de bibliotheek ingresses gebeurtenissen afzonderlijk (één gebeurtenis is wat u doorgeeft aan **IoTHubClient\_LLE\_SendEventAsync**). Als de optie batchen **true**, de bibliotheek verzamelt zoveel gebeurtenissen zoals kan het uit de buffer (maximaal de maximale berichtgrootte die IoT Hub accepteert).  De gebeurtenis batch wordt verzonden naar IoT Hub in één HTTPS-aanroep (de afzonderlijke gebeurtenissen zijn gebundeld in een JSON-matrix). Inschakelen van batchverwerking doorgaans resulteert in grote prestatiewinst omdat u bent netwerk retourbewerkingen verminderen. Bovendien vermindert bandbreedte omdat u bij het verzenden van een set met HTTPS-headers met een gebeurtenis batch in plaats van een set headers voor elke afzonderlijke gebeurtenis. Tenzij u een specifieke reden hebt om iets anders, moet doorgaans u om in te schakelen batchverwerking.

## <a name="next-steps"></a>Volgende stappen
Dit artikel wordt beschreven in de details van het gedrag van de **IoTHubClient** bibliotheek gevonden in de **Azure IoT-device SDK voor C**. Met deze informatie kunt u hebt een goed begrip van de mogelijkheden van de **IoTHubClient** bibliotheek. De [volgende artikel](iot-hub-device-sdk-c-serializer.md) vergelijkbare details bevat over de **serialisatiefunctie** bibliotheek.

Zie voor meer informatie over het ontwikkelen voor IoT Hub, de [Azure IoT SDK's][lnk-sdks].

Als u wilt de mogelijkheden van IoT Hub verder verkennen, Zie:

* [AI implementeren op de edge-apparaten met Azure IoT rand][lnk-iotedge]

[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: ../iot-edge/tutorial-simulate-device-linux.md
