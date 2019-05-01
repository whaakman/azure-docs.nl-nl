---
title: Zelfstudie voor C-module ontwikkelen voor Linux - Azure IoT Edge | Microsoft Docs
description: Deze zelfstudie leert u hoe u een IoT Edge-module maken met C-code en deze implementeren in een Linux-apparaat met IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: f5f6b6473a3cbe078ae2357e1f908788e54af25d
ms.sourcegitcommit: 44a85a2ed288f484cc3cdf71d9b51bc0be64cc33
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/28/2019
ms.locfileid: "64573419"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-linux-devices"></a>Zelfstudie: Een C IoT Edge-module voor Linux-apparaten ontwikkelen

Gebruik Visual Studio Code ontwikkelen van C-code en deze implementeren naar een Linux-apparaat met Azure IoT Edge. 

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge-module in C te maken
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren in een containerregister
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven

De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Bereik van de oplossing

In deze zelfstudie laat zien hoe u voor het ontwikkelen van een module in **C** met behulp van **Visual Studio Code**, en hoe u implementeert een **Linux apparaat**. Als u modules voor Windows-apparaten ontwikkelt, gaat u naar [ontwikkelen van een C IoT Edge-module voor Windows-apparaten](tutorial-c-module-windows.md) in plaats daarvan.

Gebruik de volgende tabel om te begrijpen van de opties voor het ontwikkelen en implementeren van modules in C op Linux: 

| C | Visual Studio Code | Visual Studio 2017 | 
| - | ------------------ | ------------------ |
| **Linux AMD64** | ![VS Code gebruiken voor C-modules op Linux AMD64](./media/tutorial-c-module/green-check.png) | ![Gebruik Visual Studio 2017 voor C-modules op Linux AMD64](./media/tutorial-c-module/green-check.png) |
| **Linux ARM32** | ![VS Code gebruiken voor C-modules op Linux ARM32](./media/tutorial-c-module/green-check.png) | ![Gebruik Visual Studio 2017 voor C-modules op Linux ARM32](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u zijn gebleven door middel van de vorige zelfstudie over het instellen van uw ontwikkelomgeving voor ontwikkeling van Linux-containers: [Ontwikkelen van IoT Edge-modules voor Linux-apparaten](tutorial-develop-for-linux.md). Door deze zelfstudie is voltooid, hebt u de volgende vereisten voldaan: 

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een [Linux-apparaat met Azure IoT Edge](quickstart-linux.md)
* Een containerregister, bijvoorbeeld [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio Code](https://code.visualstudio.com/) geconfigureerd met de [hulpprogramma's voor Azure IoT](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-tools).
* [Docker CE](https://docs.docker.com/install/) geconfigureerd voor het uitvoeren van Linux-containers.

Voor het ontwikkelen van een IoT Edge-module in C, installeert u de volgende aanvullende vereisten op uw ontwikkelcomputer: 

* [C/C++-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) voor Visual Studio Code (Engelstalig).

## <a name="create-a-module-project"></a>Een module-project maken

De volgende stappen uit voor het maken van een IoT Edge-module-project voor C met behulp van Visual Studio Code en de hulpprogramma's voor Azure IoT-extensie. Zodra u een projectsjabloon gemaakt hebt, voegt u nieuwe code toe zodat de module berichten op basis van de gerapporteerde eigenschappen filtert. 

### <a name="create-a-new-project"></a>Een nieuw project maken

Maak een C-oplossingssjabloon die u met uw eigen code kunt aanpassen.

1. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen.

2. Typ in het opdrachtenpalet de opdracht **Azure: Aanmelden** in, voer deze uit en volg de instructies om u aan te melden bij uw Azure-account. Als u zich al hebt aangemeld, kunt u deze stap overslaan.

3. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** in en voer deze uit. Volg de aanwijzingen in het opdrachtpalet om uw oplossing te maken.

   | Veld | Value |
   | ----- | ----- |
   | Map selecteren | Kies de locatie op uw ontwikkelcomputer waar VS Code de oplossingsbestanden moet maken. |
   | Een naam opgeven voor de oplossing | Voer een beschrijvende naam voor de oplossing in of accepteer de standaardnaam **EdgeSolution**. |
   | Modulesjabloon selecteren | Kies **C Module**. |
   | Een modulenaam opgeven | Geef de module de naam **CModule**. |
   | Opslagplaats voor Docker-afbeeldingen voor de module opgeven | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. De containerafbeelding wordt vooraf gevuld vanuit de naam die u in de laatste stap hebt opgegeven. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/cmodule. |
 
   ![Opslagplaats voor Docker-installatiekopieën opgeven](./media/tutorial-c-module/repository.png)

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

In het omgevingsbestand worden de referenties voor het containerregister opgeslagen. Deze referenties worden gedeeld met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen.

1. Open in VS Code Explorer het .env-bestand.
2. Werk de velden **gebruikersnaam** en **wachtwoord** bij met de waarden die u hebt gekopieerd uit het Azure-containerregister.
3. Sla dit bestand op.

### <a name="select-your-target-architecture"></a>Selecteer uw doel-architectuur

Op dit moment kunt C-modules voor Linux AMD64- en Linux ARM32v7 apparaten ontwikkelen met Visual Studio Code. U moet selecteren welke architectuur die u hebt geconfigureerd met elke oplossing, omdat de container is gemaakt en uitgevoerd anders voor elk architectuurtype. De standaardwaarde is Linux AMD64. 

1. Open het opdrachtenpalet en zoek **Azure IoT Edge: Doelplatform standaard ingesteld voor Edge-oplossing**, of Selecteer het pictogram van de snelkoppeling in de zijbalk aan de onderkant van het venster. 

2. Selecteer de doel-architectuur in de lijst met opties in het opdrachtenpalet. Voor deze zelfstudie gebruiken we een virtuele Ubuntu-machine als de IoT Edge-apparaat, zodat de standaardwaarde wordt **amd64**. 

### <a name="update-the-module-with-custom-code"></a>De module bijwerken met aangepaste code

De code van de standaard-module ontvangt berichten van een wachtrij en geeft ze via een uitvoerwachtrij. We gaan enkele aanvullende code toevoegen zodat de module berichten aan de rand worden verwerkt voordat ze worden doorgestuurd naar IoT Hub. De module bijwerken zodat deze de temperatuurgegevens in elk bericht analyseert en alleen het bericht naar IoT Hub verzonden als de temperatuur van een bepaalde drempelwaarde overschrijdt. 

1. De gegevens van de sensor in dit scenario worden in JSON-indeling aangeleverd. Als u berichten wilt filteren in een JSON-indeling, moet u een JSON-bibliotheek voor C importeren. In deze zelfstudie wordt Parson gebruikt.

   1. Download de [Parson GitHub-opslagplaats](https://github.com/kgabis/parson). Kopieer de bestanden **parson.c** en **parson.h** naar de map **CModule**.

   2. Open **modules** > **CModule** > **CMakeLists.txt**. Importeer boven in het bestand de Parson-bestanden als een bibliotheek met de naam **my_parson**.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Voeg **my_parson** toe aan de lijst met bibliotheken in de functie **target_link_libraries** van CMakeLists.txt.

   4. Sla het bestand **CMakeLists.txt** op.

   5. Open **modules** > **CModule** > **main.c**. Onder aan de lijst met instructies, toevoegen van een nieuw account om op te nemen `parson.h` voor JSON-ondersteuning:

      ```c
      #include "parson.h"
      ```

1. Voeg in het bestand **main.c** na de sectie include een globale variabele toe met de naam `temperatureThreshold`. Deze variabele bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden.

    ```c
    static double temperatureThreshold = 25;
    ```

1. Zoek de `CreateMessageInstance` functie in main.c. Vervang de binnenste if-else-instructie met de volgende code waarmee een paar regels van de functionaliteit wordt toegevoegd: 

   ```c
   if ((messageInstance->messageHandle = IoTHubMessage_Clone(message)) == NULL)
   {
       free(messageInstance);
       messageInstance = NULL;
   }
   else
   {
       messageInstance->messageTrackingId = messagesReceivedByInput1Queue;
       MAP_HANDLE propMap = IoTHubMessage_Properties(messageInstance->messageHandle);
       if (Map_AddOrUpdate(propMap, "MessageType", "Alert") != MAP_OK)
       {
          printf("ERROR: Map_AddOrUpdate Failed!\r\n");
       }
   }
   ```

   De nieuwe regels met code in de else-instructie wordt een nieuwe eigenschap toevoegen aan het bericht dat het bericht als een waarschuwing van labels. Deze code worden alle berichten labels als waarschuwingen, omdat we voegen functionaliteit dat alleen berichten naar IoT Hub verzendt als ze hoge temperaturen rapporteren toe. 

1. Vervang de volledige functie `InputQueue1Callback` door de volgende code. Deze functie implementeert het feitelijke berichtenfilter. Wanneer een bericht wordt ontvangen, wordt gecontroleerd of de gemelde temperatuur hoger is dan de drempelwaarde. Zo ja, verzendt deze het bericht via de uitvoerwachtrij. Als dat niet het geval is, klikt u vervolgens het bericht worden genegeerd. 

    ```c
    static IOTHUBMESSAGE_DISPOSITION_RESULT InputQueue1Callback(IOTHUB_MESSAGE_HANDLE message, void* userContextCallback)
    {
        IOTHUBMESSAGE_DISPOSITION_RESULT result;
        IOTHUB_CLIENT_RESULT clientResult;
        IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle = (IOTHUB_MODULE_CLIENT_LL_HANDLE)userContextCallback;

        unsigned const char* messageBody;
        size_t contentSize;

        if (IoTHubMessage_GetByteArray(message, &messageBody, &contentSize) != IOTHUB_MESSAGE_OK)
        {
            messageBody = "<null>";
        }

        printf("Received Message [%zu]\r\n Data: [%s]\r\n",
                messagesReceivedByInput1Queue, messageBody);

        // Check if the message reports temperatures higher than the threshold
        JSON_Value *root_value = json_parse_string(messageBody);
        JSON_Object *root_object = json_value_get_object(root_value);
        double temperature;
        if (json_object_dotget_value(root_object, "machine.temperature") != NULL && (temperature = json_object_dotget_number(root_object, "machine.temperature")) > temperatureThreshold)
        {
            printf("Machine temperature %f exceeds threshold %f\r\n", temperature, temperatureThreshold);
            // This message should be sent to next stop in the pipeline, namely "output1".  What happens at "outpu1" is determined
            // by the configuration of the Edge routing table setup.
            MESSAGE_INSTANCE *messageInstance = CreateMessageInstance(message);
            if (NULL == messageInstance)
            {
                result = IOTHUBMESSAGE_ABANDONED;
            }
            else
            {
                printf("Sending message (%zu) to the next stage in pipeline\n", messagesReceivedByInput1Queue);

                clientResult = IoTHubModuleClient_LL_SendEventToOutputAsync(iotHubModuleClientHandle, messageInstance->messageHandle, "output1", SendConfirmationCallback, (void *)messageInstance);
                if (clientResult != IOTHUB_CLIENT_OK)
                {
                    IoTHubMessage_Destroy(messageInstance->messageHandle);
                    free(messageInstance);
                    printf("IoTHubModuleClient_LL_SendEventToOutputAsync failed on sending msg#=%zu, err=%d\n", messagesReceivedByInput1Queue, clientResult);
                    result = IOTHUBMESSAGE_ABANDONED;
                }
                else
                {
                    result = IOTHUBMESSAGE_ACCEPTED;
                }
            }
        }
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

1. Voeg een `moduleTwinCallback`-functie toe. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

    ```c
    static void moduleTwinCallback(DEVICE_TWIN_UPDATE_STATE update_state, const unsigned char* payLoad, size_t size, void* userContextCallback)
    {
        printf("\r\nTwin callback called with (state=%s, size=%zu):\r\n%s\r\n",
            ENUM_TO_STRING(DEVICE_TWIN_UPDATE_STATE, update_state), size, payLoad);
        JSON_Value *root_value = json_parse_string(payLoad);
        JSON_Object *root_object = json_value_get_object(root_value);
        if (json_object_dotget_value(root_object, "desired.TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_dotget_number(root_object, "desired.TemperatureThreshold");
        }
        if (json_object_get_value(root_object, "TemperatureThreshold") != NULL) {
            temperatureThreshold = json_object_get_number(root_object, "TemperatureThreshold");
        }
    }
    ```

1. Zoek de `SetupCallbacksForModule` functie. De functie vervangen door de volgende code die wordt toegevoegd een **of als** instructie om te controleren of de moduledubbel is bijgewerkt.

   ```c
   static int SetupCallbacksForModule(IOTHUB_MODULE_CLIENT_LL_HANDLE iotHubModuleClientHandle)
   {
       int ret;

       if (IoTHubModuleClient_LL_SetInputMessageCallback(iotHubModuleClientHandle, "input1", InputQueue1Callback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetInputMessageCallback(\"input1\")..........FAILED!\r\n");
           ret = __FAILURE__;
       }
       else if (IoTHubModuleClient_LL_SetModuleTwinCallback(iotHubModuleClientHandle, moduleTwinCallback, (void*)iotHubModuleClientHandle) != IOTHUB_CLIENT_OK)
       {
           printf("ERROR: IoTHubModuleClient_LL_SetModuleTwinCallback(default)..........FAILED!\r\n");
           ret = __FAILURE__;
       }
       else
       {
           ret = 0;
       }

       return ret;
   }
   ```

1. Sla het bestand main.c op.

1. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. 

1. Voeg de moduledubbel CModule toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie `moduleContent` in, na de moduledubbel `$edgeHub`:

   ```json
   "CModule": {
       "properties.desired":{
           "TemperatureThreshold":25
       }
   }
   ```

   ![Dubbele CModule toevoegen aan implementatiesjabloon](./media/tutorial-c-module/module-twin.png)

1. Sla het bestand **deployment.template.json** op.

## <a name="build-and-push-your-module"></a>Bouw en stuur uw module

In de vorige sectie hebt u een oplossing die het IoT Edge en code toegevoegd aan de CModule die filtert berichten waarin de gerapporteerde machine temperatuur hoger is binnen de aanvaardbare grenzen. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen.

1. Open de terminal van VS Code en selecteer **weergave** > **Terminal**.

1. Meld u aan Docker met de volgende opdracht in de terminal. Meld u aan met de gebruikersnaam, wachtwoord en aanmeldingsserver van het Azure container registry. U vindt deze waarden uit de **toegangssleutels** gedeelte van uw register in Azure portal.
     
   ```bash
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

2. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **Build and Push IoT Edge solution**.

   De opdracht build- en push start drie bewerkingen. Eerst een nieuwe map wordt gemaakt in de oplossing met de naam **config** die de volledige implementatie oplossing voor het manifest, gebouwd om van gegevens in de sjabloon voor de implementatie en andere bestanden bevat. Ten tweede, deze wordt uitgevoerd `docker build` om de containerinstallatiekopie op basis van het dockerfile dat geschikt is voor uw doel-architectuur te bouwen. Vervolgens wordt deze uitgevoerd `docker push` naar de opslagplaats voor installatiekopieën pushen naar uw containerregister.

## <a name="deploy-modules-to-device"></a>Modules op apparaat implementeren

Gebruik de Verkenner van Visual Studio Code en de hulpprogramma's voor Azure IoT-extensie aan het project module implementeren in uw IoT Edge-apparaat. U hebt al een implementatie-manifest voorbereid voor uw scenario, de **deployment.json** bestand in de config-map. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

Zorg ervoor dat uw IoT Edge-apparaat actief en werkend is. 

1. Vouw in de Visual Studio Code explorer de **Azure IoT Hub-apparaten** sectie voor uw overzicht van IoT-apparaten.

2. Klik met de rechtermuisknop op de naam van het IoT Edge-apparaat en selecteer **Implementatie voor één apparaat maken**.

3. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**. Gebruik niet het bestand deployment.template.json.

4. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **CModule** wordt uitgevoerd, samen met de module **TempSensor** en de modules **$edgeAgent** en **$edgeHub**.

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart.

U kunt de status van uw IoT Edge-apparaat bekijken via de sectie **Azure IoT Hub Devices** van de Visual Studio Code explorer. Vouw de details van uw apparaat uit voor een overzicht van de modules die worden geïmplementeerd en uitgevoerd.

1. In de Visual Studio Code-Verkenner met de rechtermuisknop op de naam van uw IoT Edge-apparaat en selecteer **Start Monitoring D2C berichten**.

2. Bekijk de berichten die binnenkomen in uw IoT-Hub. Het duurt even voor de berichten binnenkomen, omdat het IoT Edge-apparaat moet de nieuwe implementatie ontvangen en alle modules te starten. We de wijzigingen in de code CModule wacht totdat de temperatuur machine 25 graden is bereikt voordat het verzenden van berichten. Het berichttype ook toegevoegd **waarschuwing** in de berichten die deze temperatuur drempelwaarde bereikt. 

   ![Berichten weergeven die binnenkomen in IoT Hub](./media/tutorial-c-module/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>De moduledubbel bewerken

We de moduledubbel CModule in het manifest van de implementatie hebt gebruikt voor het instellen van de drempelwaarde voor de temperatuur op 25 graden. De moduledubbel kunt u de functionaliteit wijzigen zonder de code van de module bijwerken.

1. Vouw de gegevens van uw IoT Edge-apparaat om te zien van de actieve modules in Visual Studio Code. 

2. Met de rechtermuisknop op **CModule** en selecteer **bewerken moduledubbel**. 

3. Zoek **TemperatureThreshold** in de gewenste eigenschappen. De waarde ervan wijzigen naar een nieuwe temperatuur 5 graden tot 10 graden hoger is dan de meest recente gemelde temperatuur. 

4. Sla het bestand van de dubbele module.

5. Met de rechtermuisknop op een willekeurige plaats in het deelvenster en selecteer Bewerken moduledubbel **Update moduledubbel**. 

5. De binnenkomende berichten voor apparaat-naar-cloud bewaken. U ziet de berichten die niet meer totdat de nieuwe temperatuur drempelwaarde is bereikt. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat.

Anders kunt u de lokale configuraties en de Azure-resources dat u in dit artikel gebruikt om kosten te vermijden verwijderen.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u gereed voor het bouwen van uw eigen modules bent, vindt u meer informatie over [ontwikkelen van uw eigen IoT Edge-modules](module-development.md) of hoe u [modules met Visual Studio Code ontwikkelen](how-to-vs-code-develop-module.md). U kunt doorgaan naar de volgende zelfstudies voor meer informatie over hoe Azure IoT Edge kunnen helpen bij het implementeren van Azure cloudservices om te verwerken en analyseren van gegevens aan de rand.

> [!div class="nextstepaction"]
> [Functies](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Machine Learning](tutorial-deploy-machine-learning.md)
> [Custom Vision Service](tutorial-deploy-custom-vision.md)

