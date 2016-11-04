## Typische uitvoer
Hieronder volgt een voorbeeld van de uitvoer geschreven naar het logboekbestand door de Hello World-sjabloon. Voor de leesbaarheid zijn tekens voor nieuwe regels en tabs toegevoegd:

```
[{
    "time": "Mon Apr 11 13:48:07 2016",
    "content": "Log started"
}, {
    "time": "Mon Apr 11 13:48:48 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:48:55 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:01 2016",
    "properties": {
        "helloWorld": "from Azure IoT Gateway SDK simple sample!"
    },
    "content": "aGVsbG8gd29ybGQ="
}, {
    "time": "Mon Apr 11 13:49:04 2016",
    "content": "Log stopped"
}]
```

## Codefragmenten
Dit deel behandelt enkele belangrijke onderdelen van de code in de Hello World-sjabloon.

### Het maken van de gateway
De ontwikkelaar moet het *gatewayproces* schrijven. Dit programma maakt de interne infrastructuur (de broker), laadt modules en stelt alles in om correct te werken. De SDK biedt de **Gateway_Create_From_JSON**-functie waarmee u een gateway vanuit een JSON-bestand kunt opstarten. Om de functie **Gateway_Create_From_JSON** te gebruiken, moet u dit pad doorgeven aan een JSON-bestand dat de te laden modules specificeert. 

U vindt de code voor het gatewayproces in de Hello World-sjabloon in het bestand [main.c][lnk-main-c]. Het stuk hieronder geeft een verkorte versie van de gatewayprocescode weer voor een betere leesbaarheid. Het programma maakt een gateway en wacht dan tot de gebruiker op **ENTER** drukt alvorens het de gateway afbreekt. 

```
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

Het JSON-instellingenbestand bevat een lijst met te laden modules. Elke module moet het volgende specificeren:

* **module_name**: een unieke modulenaam.
* **module_path**: het pad naar de bibliotheek met de module. Voor Linux is dit een .so-bestand, voor Windows een .dll-bestand.
* **args**: alle configuratie-informatie die de module nodig heeft.

Het JSON-bestand bevat ook de koppelingen tussen de modules die worden doorgegeven aan de broker. Een koppeling heeft twee eigenschappen:

* **bron**: de naam van een module van het gedeelte `modules`, of '\*'.
* **sink**: de naam van een module van het gedeelte `modules`

Elke koppeling definieert de route en richting van een bericht. Berichten van module `source` moeten worden geleverd aan module `sink`. De `source` kan worden ingesteld op '\*'. Hiermee wordt aangegeven dat berichten van alle modules worden ontvangen door `sink`.

Het volgende voorbeeld toont het JSON-instellingenbestand gebruikt om de Hello World-sjabloon op Linux te configureren. Elk bericht dat wordt gemaakt door module `hello_world` wordt gebruikt door module `logger`. Of een module een argument nodig heeft, is afhankelijk van het ontwerp van de module. In dit voorbeeld neemt de loggermodule een argument dat het pad is naar het uitvoerbestand en de Hello World-module neemt geen argumenten:

```
{
    "modules" :
    [ 
        {
            "module name" : "logger",
            "module path" : "./modules/logger/liblogger_hl.so",
            "args" : {"filename":"log.txt"}
        },
        {
            "module name" : "hello_world",
            "module path" : "./modules/hello_world/libhello_world_hl.so",
            "args" : null
        }
    ],
    "links" :
    [
        {
            "source" : "hello_world",
            "sink" : "logger"
        }
    ]
}
```

### Hello World-module berichtpublicatie
U vindt de code gebruikt door de “hello world”-module om berichten te publiceren in het bestand ['hello_world.c'][lnk-helloworld-c]. Het onderstaande fragment toont een gewijzigde versie met bijkomende opmerkingen en bepaalde foutafhandelingscode die is verwijderd voor een betere leesbaarheid:

```
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

### Hello World-module berichtverwerking
De Hello World-module moet nooit berichten verwerken die andere modules publiceren voor de broker. Dit maakt implementatie van de berichtcallback in de Hello World-module een no-op-functie.

```
static void HelloWorld_Receive(MODULE_HANDLE moduleHandle, MESSAGE_HANDLE messageHandle)
{
    /* No action, HelloWorld is not interested in any messages. */
}
```

### Loggermodule berichtpublicatie en -verwerking
De loggermodule ontvangt berichten van de broker en schrijft ze weg naar een bestand. Deze module publiceert nooit berichten. Daarom roept de code van de loggermodule nooit de functie **Broker_Publish** aan.

De functie **Logger_Recieve** in het bestand [logger.c][lnk-logger-c] is de callback die de broker aanroept om berichten aan de loggermodule te leveren. Het onderstaande fragment toont een gewijzigde versie met bijkomende opmerkingen en bepaalde foutafhandelingscode die is verwijderd voor een betere leesbaarheid:

```
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

## Volgende stappen
Zie voor meer informatie over het gebruik van de Gateway SDK:

* [IoT Gateway SDK – apparaat-naar-cloudberichten verzenden met een gesimuleerd apparaat met Linux][lnk-gateway-simulated].
* [Azure IoT Gateway SDK][lnk-gateway-sdk] op GitHub.

<!-- Links -->
[lnk-main-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/samples/hello_world/src/main.c
[lnk-helloworld-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/hello_world/src/hello_world.c
[lnk-logger-c]: https://github.com/Azure/azure-iot-gateway-sdk/blob/master/modules/logger/src/logger.c
[lnk-gateway-sdk]: https://github.com/Azure/azure-iot-gateway-sdk/
[lnk-gateway-simulated]: ../articles/iot-hub/iot-hub-linux-gateway-sdk-simulated-device.md

<!--HONumber=Sep16_HO4-->


