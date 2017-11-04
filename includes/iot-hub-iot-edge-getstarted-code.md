## <a name="typical-output"></a>Typische uitvoer

Het volgende voorbeeld ziet u de uitvoer naar het logboekbestand geschreven door de Hallo wereld-voorbeeld. De uitvoer is voor de leesbaarheid voorzien van opmaak:

```json
[{
    "time": "Mon Apr 11 13:42:50 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:42:50 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:42:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:43:00 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:45:00 2016",
    "content": "Log stopped"
}]
```

## <a name="code-snippets"></a>Codefragmenten

In deze sectie worden enkele belangrijke onderdelen van de code in het voorbeeldprogramma Hello\_World behandeld.

### <a name="iot-edge-gateway-creation"></a>Rand IoT gateway maken

Voor het maken van een gateway implementeren een *gateway proces*. Dit programma maakt de interne infrastructuur (broker), wordt de rand van de IoT-modules geladen en het proces gateway configureert. IoT Edge biedt de functie **Gateway\_Create\_From\_JSON**, waarmee u een gateway vanuit een JSON-bestand kunt opstarten. Gebruik de **Gateway\_maken\_van\_JSON** werkt, het pad naar een JSON-bestand dat Hiermee geeft u aan de rand van de IoT-modules laden doorgeven.

U vindt de code voor het gateway-proces in de *Hallo wereld* voorbeeld in de [main.c] [ lnk-main-c] bestand. Voor een betere leesbaarheid bevat het fragment hieronder een verkorte versie van de code voor het verwerken van de gateway. Het voorbeeldprogramma maakt een gateway en wacht dan tot de gebruiker op **Enter** drukt om de gateway weer te verwijderen.

```C
int main(int argc, char** argv)
{
    GATEWAY_HANDLE gateway;
    if ((gateway = Gateway_Create_From_JSON(argv[1])) == NULL)
    {
        printf("failed to create the gateway from JSON\n");
    }
    else
    {
        printf("gateway successfully created from JSON\n");
        printf("gateway shall run until ENTER is pressed\n");
        (void)getchar();
        Gateway_LL_Destroy(gateway);
    }
    return 0;
}
```

De instellingen voor JSON-bestand bevat een lijst van de rand van de IoT-modules laden en de koppelingen tussen de modules. Elke IoT rand module moet a: opgeven

* **name**: een unieke naam voor de module.
* **loader**: een laadprogramma dat de gewenste module kan laden. Laadprogramma's zijn een uitbreidingspunt voor het laden van verschillende soorten modules. IoT-Edge biedt laadprogramma's voor gebruik met modules die zijn geschreven in systeemeigen C, Node.js, Java en .NET. Het Hallo wereld-voorbeeld gebruikt alleen de lader van de systeemeigen C omdat alle modules in dit voorbeeld dynamische tapewisselaars die zijn geschreven in c zijn Zie voor meer informatie over het gebruik van de rand van de IoT-modules die zijn geschreven in verschillende talen de [Node.js](https://github.com/Azure/iot-edge/blob/master/samples/nodejs_simple_sample/), [Java](https://github.com/Azure/iot-edge/tree/master/samples/java_sample), of [.NET](https://github.com/Azure/iot-edge/tree/master/samples/dotnet_binding_sample) voorbeelden.
    * **naam**: de naam van het laadprogramma gebruikt voor het laden van de module.
    * **entrypoint**: het pad naar de bibliotheek met de module. Deze bibliotheek is een bestand .so op Linux, op Windows deze bibliotheek is een DLL-bestand. Het toegangspunt is specifiek voor het type laadprogramma dat wordt gebruikt. Het toegangspunt voor Node.js-laadprogramma is een bestand .js. Het toegangspunt voor Java-laadprogramma is een klassepad en de naam van een klasse. Het toegangspunt voor .NET-laadprogramma is een assembly-naam en de naam van een klasse.

* **args**: alle configuratie-informatie die de module nodig heeft.

De volgende code toont de JSON die wordt gebruikt voor alle IoT-rand modules voor het Hallo wereld-voorbeeld declareren op Linux. Of een module argumenten nodig heeft, is afhankelijk van het ontwerp van de module. In dit voorbeeld wordt aan de loggermodule een argument doorgegeven dat bestaat uit het pad naar het uitvoerbestand. De module hello\_world heeft geen argumenten.

```json
"modules" :
[
    {
        "name" : "logger",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/logger/liblogger.so"
        }
        },
        "args" : {"filename":"log.txt"}
    },
    {
        "name" : "hello_world",
        "loader": {
          "name": "native",
          "entrypoint": {
            "module.path": "./modules/hello_world/libhello_world.so"
        }
        },
        "args" : null
    }
]
```

Het JSON-bestand bevat ook de koppelingen tussen de modules die worden doorgegeven aan de broker. Een koppeling heeft twee eigenschappen:

* **bron**: de naam van een module van de `modules` sectie of `\*`.
* **sink**: de naam van een module van het gedeelte `modules`.

Elke koppeling definieert de route en richting van een bericht. Berichten van de **bron** module geleverd worden aan de **sink** module. U kunt instellen de **bron** module die u wilt `\*`, wat aangeeft dat de **sink** module berichten ontvangt van elke module.

De volgende code toont de JSON die wordt gebruikt om koppelingen te configureren tussen de modules die worden gebruikt in het voorbeeld hello\_world op Linux. Elk bericht dat wordt gemaakt door de module `hello_world`, wordt gebruikt door de module `logger`.

```json
"links":
[
    {
        "source": "hello_world",
        "sink": "logger"
    }
]
```

### <a name="helloworld-module-message-publishing"></a>Publiceren van berichten door de module hello\_world

U vindt de code die door de module hello\_world wordt gebruikt om berichten te publiceren, in het bestand [hello_world.c][lnk-helloworld-c]. Voor een betere leesbaarheid bevat het volgende fragment een gewijzigde versie van de code, waarbij opmerkingen zijn toegevoegd en code voor foutafhandeling is weggelaten:

```C
int helloWorldThread(void *param)
{
    // create data structures used in function.
    HELLOWORLD_HANDLE_DATA* handleData = param;
    MESSAGE_CONFIG msgConfig;
    MAP_HANDLE propertiesMap = Map_Create(NULL);

    // add a property named "helloWorld" with a value of "from Azure IoT
    // Gateway SDK simple sample!" to a set of message properties that
    // will be appended to the message before publishing it. 
    Map_AddOrUpdate(propertiesMap, "helloWorld", "from Azure IoT Gateway SDK simple sample!")

    // set the content for the message
    msgConfig.size = strlen(HELLOWORLD_MESSAGE);
    msgConfig.source = HELLOWORLD_MESSAGE;

    // set the properties for the message
    msgConfig.sourceProperties = propertiesMap;

    // create a message based on the msgConfig structure
    MESSAGE_HANDLE helloWorldMessage = Message_Create(&msgConfig);

    while (1)
    {
        if (handleData->stopThread)
        {
            (void)Unlock(handleData->lockHandle);
            break; /*gets out of the thread*/
        }
        else
        {
            // publish the message to the broker
            (void)Broker_Publish(handleData->brokerHandle, helloWorldMessage);
            (void)Unlock(handleData->lockHandle);
        }

        (void)ThreadAPI_Sleep(5000); /*every 5 seconds*/
    }

    Message_Destroy(helloWorldMessage);

    return 0;
}
```

De Hallo\_world module nooit berichten verwerkt die andere modules IoT rand naar de broker publiceren. Om die reden is de implementatie van de bericht-callback in de module hello\_world een no-op-functie.

```C
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### <a name="logger-module-message-processing"></a>Berichtenlogboek module berichtverwerking

De loggermodule ontvangt berichten van de broker en schrijft deze weg naar een bestand. Deze module publiceert nooit berichten. Daarom roept de code van de loggermodule nooit de functie **Broker_Publish** aan.

De **Logger_Receive** werken in de [logger.c] [ lnk-logger-c] bestand is de callback van de broker wordt aangeroepen voor het leveren van berichten naar de module berichtenlogboek. Voor een betere leesbaarheid bevat het volgende fragment een gewijzigde versie, waarbij opmerkingen zijn toegevoegd en code voor foutafhandeling is weggelaten:

```C
static void Logger_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{

    time_t temp = time(NULL);
    struct tm* t = localtime(&temp);
    char timetemp[80] = { 0 };

    // Get the message properties from the message
    CONSTMAP_HANDLE originalProperties = Message_GetProperties(messageHandle); 
    MAP_HANDLE propertiesAsMap = ConstMap_CloneWriteable(originalProperties);

    // Convert the collection of properties into a JSON string
    STRING_HANDLE jsonProperties = Map_ToJSON(propertiesAsMap);

    //  base64 encode the message content
    const CONSTBUFFER * content = Message_GetContent(messageHandle);
    STRING_HANDLE contentAsJSON = Base64_Encode_Bytes(content->buffer, content->size);

    // Start the construction of the final string to be logged by adding
    // the timestamp
    STRING_HANDLE jsonToBeAppended = STRING_construct(",{\"time\":\"");
    STRING_concat(jsonToBeAppended, timetemp);

    // Add the message properties
    STRING_concat(jsonToBeAppended, "\",\"properties\":"); 
    STRING_concat_with_STRING(jsonToBeAppended, jsonProperties);

    // Add the content
    STRING_concat(jsonToBeAppended, ",\"content\":\"");
    STRING_concat_with_STRING(jsonToBeAppended, contentAsJSON);
    STRING_concat(jsonToBeAppended, "\"}]");

    // Write the formatted string
    LOGGER_HANDLE_DATA *handleData = (LOGGER_HANDLE_DATA *)moduleHandle;
    addJSONString(handleData->fout, STRING_c_str(jsonToBeAppended);
}
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel hebt u een eenvoudige rand IoT gateway die berichten naar een logboekbestand schrijft uitgevoerd. Voor het uitvoeren van een steekproef die berichten naar IoT Hub verzendt, Zie:

- [Rand van de IoT-apparaat-naar-cloud-berichten verzenden met een gesimuleerd apparaat met Linux][lnk-gateway-simulated-linux] 
- [Rand van de IoT-apparaat-naar-cloud-berichten verzenden met een gesimuleerd apparaat met Windows][lnk-gateway-simulated-windows].


<!-- Links -->
[lnk-main-c]: https://github.com/Azure/iot-edge/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/iot-edge/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/iot-edge/blob/master/modules/logger/src/logger.c
[lnk-iot-edge]: https://github.com/Azure/iot-edge/
[lnk-gateway-simulated-linux]: ../articles/iot-hub/iot-hub-linux-iot-edge-simulated-device.md
[lnk-gateway-simulated-windows]: ../articles/iot-hub/iot-hub-windows-iot-edge-simulated-device.md