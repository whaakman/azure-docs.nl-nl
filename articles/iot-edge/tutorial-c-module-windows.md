---
title: Zelfstudie voor C-module ontwikkelen voor Windows - Azure IoT Edge | Microsoft Docs
description: Deze zelfstudie leert u hoe u een IoT Edge-module maken met C-code en deze implementeren in een Windows-apparaat met IoT Edge
services: iot-edge
author: shizn
manager: philmea
ms.author: xshi
ms.date: 04/23/2019
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: ee64e5a49bf2825c83c74167d7eb75aa3dc59387
ms.sourcegitcommit: 509e1583c3a3dde34c8090d2149d255cb92fe991
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/27/2019
ms.locfileid: "66239824"
---
# <a name="tutorial-develop-a-c-iot-edge-module-for-windows-devices"></a>Zelfstudie: Een C IoT Edge-module voor Windows-apparaten ontwikkelen

Visual Studio gebruiken voor het ontwikkelen van C-code en deze implementeren in een Windows-apparaat met Azure IoT Edge. 

U kunt Azure IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio gebruiken voor het maken van een IoT Edge-module die gebaseerd op de .NET Core-SDK 2.1.
> * Gebruik Visual Studio en Docker om te maken van een Docker-installatiekopie en deze publiceren naar uw register.
> * De module implementeren op uw IoT Edge-apparaat.
> * Gegenereerde gegevens weergeven.

De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheid gegevens te reduceren die worden gecommuniceerd naar en worden opgeslagen in de cloud. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="solution-scope"></a>Bereik van de oplossing

In deze zelfstudie laat zien hoe u voor het ontwikkelen van een module in **C** met behulp van **Visual Studio 2017**, en hoe u implementeert een **Windows-apparaat**. Als u modules voor Linux-apparaten ontwikkelt, gaat u naar [ontwikkelen van een C IoT Edge-module voor Linux-apparaten](tutorial-c-module.md) in plaats daarvan. 

Gebruik de volgende tabel om te begrijpen van de opties voor het ontwikkelen en implementeren van modules in C op Windows-apparaten: 

| C | Visual Studio Code | Visual Studio 2017 | 
| -- | ------------------ | ------------------ |
| **Windows AMD64** |  | ![C-modules voor WinAMD64 in Visual Studio ontwikkelen](./media/tutorial-c-module/green-check.png) |

## <a name="prerequisites"></a>Vereisten

Voordat u deze zelfstudie begint, moet u zijn gebleven door middel van de vorige zelfstudie over het instellen van uw ontwikkelomgeving voor ontwikkeling van Windows-containers: [Ontwikkelen van IoT Edge-modules voor Windows-apparaten](tutorial-develop-for-windows.md). Na het voltooien van deze zelfstudie hebt u de volgende vereisten voldaan: 

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een [Windows-apparaat met Azure IoT Edge](quickstart.md).
* Een containerregister, bijvoorbeeld [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/).
* [Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio?view=vs-2017), versie 15.7 of hoger, geconfigureerd met de [hulpprogramma's voor Azure IoT Edge](https://marketplace.visualstudio.com/items?itemName=vsc-iot.vsiotedgetools) extensie.
* [Docker CE](https://docs.docker.com/install/) geconfigureerd voor het uitvoeren van Windows-containers.
* De Azure IoT-SDK voor c 

## <a name="create-a-module-project"></a>Een module-project maken

De volgende stappen maakt u een IoT Edge-module-project dat gebaseerd op .NET Core 2.0 SDK met behulp van Visual Studio en de hulpprogramma's voor Azure IoT Edge-extensie. Zodra u een projectsjabloon gemaakt hebt, voegt u nieuwe code toe zodat de module berichten op basis van de gerapporteerde eigenschappen filtert. 

### <a name="create-a-new-project"></a>Een nieuw project maken

Maak een C-oplossingssjabloon die u met uw eigen code kunt aanpassen.

1. Visual Studio als beheerder uitvoeren.

2. Selecteer **Bestand** > **Nieuw** > **Project**. 

3. Selecteer in het venster Nieuw project, de **Azure IoT** type toepassingsproject en kies de **Azure IoT Edge** project. Wijzig de naam van het project en de oplossing iets beschrijvende als volgt aan bij **CTutorialApp**. Selecteer **OK** om het project te maken. 

   ![Een nieuw Azure IoT Edge-project maken](./media/tutorial-c-module-windows/new-project.png)

4. In de IoT Edge-toepassing en het modulevenster, configureert u uw project met de volgende waarden: 

   | Veld | Value |
   | ----- | ----- |
   | Platform voor toepassingen | Schakel het selectievakje **Linux Amd64**, en Controleer **WindowsAmd64**. |
   | Selecteer een sjabloon | Selecteer **C Module**. | 
   | Naam van de module-project | Geef de module de naam **CModule**. | 
   | Opslagplaats voor docker-installatiekopieën | Een opslagplaats voor afbeeldingen bevat de naam van het containerregister en de naam van uw containerafbeelding. Uw containerinstallatiekopie wordt vooraf ingevuld vanuit de waarde van de naam van de module project. Vervang **localhost:5000** door de waarde van de aanmeldingsserver uit uw Azure-containerregister. U vindt de aanmeldingsserver op de overzichtspagina van het containerregister in de Azure-portal. <br><br> De uiteindelijke opslagplaats voor de installatiekopie ziet er ongeveer als volgt uit: \<registernaam\>.azurecr.io/cmodule. |

   ![Configureren van uw project voor het doelapparaat en module-type containerregister](./media/tutorial-c-module-windows/add-application-and-module.png)

5. Selecteer **OK** de wijzigingen worden toegepast. 

### <a name="add-your-registry-credentials"></a>Uw registerreferenties toevoegen

Het manifest implementatie deelt de referenties voor uw container registry met de IoT Edge-runtime. De runtime heeft deze referenties nodig om uw persoonlijke installatiekopieën naar het IoT Edge-apparaat te halen. Gebruik de referenties van de **toegangssleutels** sectie van uw Azure container registry. 

1. Open in Visual Studio solution explorer, de **deployment.template.json** bestand. 

2. Zoek de **registryCredentials** eigenschap in de $edgeAgent gewenste eigenschappen. 

3. Werk de eigenschap met de referenties, deze indeling te volgen: 

   ```json
   "registryCredentials": {
     "<registry name>": {
       "username": "<username>",
       "password": "<password>",
       "address": "<registry name>.azurecr.io"
     }
   }

4. Save the deployment.template.json file. 

### Update the module with custom code

The default module code receives messages on an input queue and passes them along through an output queue. Let's add some additional code so that the module processes the messages at the edge before forwarding them to IoT Hub. Update the module so that it analyzes the temperature data in each message, and only sends the message to IoT Hub if the temperature exceeds a certain threshold. 


1. The data from the sensor in this scenario comes in JSON format. To filter messages in JSON format, import a JSON library for C. This tutorial uses Parson.

   1. Download the [Parson GitHub repository](https://github.com/kgabis/parson). Copy the **parson.c** and **parson.h** files into the **CModule** project.

   2. In Visual Studio, open the **CMakeLists.txt** file from the CModule project folder. At the top of the file, import the Parson files as a library called **my_parson**.

      ```
      add_library(my_parson        parson.c        parson.h    )
      ```

   3. Add **my_parson** to the list of libraries in the **target_link_libraries** section of the CMakeLists.txt file.

   4. Save the **CMakeLists.txt** file.

   5. Open **CModule** > **main.c**. At the bottom of the list of include statements, add a new one to include `parson.h` for JSON support:

      ```c
      #include "parson.h"
      ```

2.  In de **main.c** bestand, het toevoegen van een globale variabele met de naam `temperatureThreshold` naast de messagesReceivedByInput1Queue-variabele. Deze variabele bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden.

    ```c
    static double temperatureThreshold = 25;
    ```

3. Zoek de `CreateMessageInstance` functie in main.c. Vervang de binnenste if-else-instructie met de volgende code waarmee een paar regels van de functionaliteit wordt toegevoegd: 

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

4. Zoek de `InputQueue1Callback` functie en vervang de gehele functie door de volgende code. Deze functie implementeert het feitelijke berichtenfilter. Wanneer een bericht wordt ontvangen, wordt gecontroleerd of de gemelde temperatuur hoger is dan de drempelwaarde. Zo ja, verzendt deze het bericht via de uitvoerwachtrij. Als dat niet het geval is, klikt u vervolgens het bericht worden genegeerd. 

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

        // If temperature exceeds threshold, send to output1
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
        // If message does not exceed threshold, do not forward
        else
        {
            printf("Not sending message (%zu) to the next stage in pipeline.\r\n", messagesReceivedByInput1Queue);
            result = IOTHUBMESSAGE_ACCEPTED;
        }

        messagesReceivedByInput1Queue++;
        return result;
    }
    ```

5. Voeg een `moduleTwinCallback`-functie toe. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

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

6. Zoek de `SetupCallbacksForModule` functie. De functie vervangen door de volgende code die wordt toegevoegd een **of als** instructie om te controleren of de moduledubbel is bijgewerkt. 

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

7. Sla het bestand main.c op.

8. Open de **deployment.template.json** bestand. 

9. Voeg de moduledubbel CModule toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie `moduleContent` in, na de moduledubbel `$edgeHub`:

   ```json
   "CModule": {
       "properties.desired":{
           "TemperatureThreshold":25
       }
   }
   ```

   ![Dubbele CModule toevoegen aan implementatiesjabloon](./media/tutorial-c-module-windows/module-twin.png)

1. Sla het bestand **deployment.template.json** op.

## <a name="build-and-push-your-module"></a>Bouw en stuur uw module

In de vorige sectie hebt u een oplossing die het IoT Edge en code toegevoegd de **CModule** voor het filteren berichten wanneer de temperatuur gerapporteerde machine lager is dan de drempelwaarde. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Gebruik de volgende opdracht op uw ontwikkelcomputer aanmelden bij Docker. Meld u aan met de gebruikersnaam, wachtwoord en aanmeldingsserver van het Azure container registry. U vindt deze waarden uit de **toegangssleutels** gedeelte van uw register in Azure portal.

   ```cmd
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```

   U ontvangt mogelijk een beveiligingswaarschuwing voor het gebruik van aanbevelen `--password-stdin`. Hoewel deze aanbevolen procedure wordt aanbevolen voor productiescenario's, valt buiten het bereik van deze zelfstudie. Zie voor meer informatie de [dockeraanmelding](https://docs.docker.com/engine/reference/commandline/login/#provide-a-password-using-stdin) verwijzing.

2. In Visual Studio solution explorer met de rechtermuisknop op de naam van het project dat u wilt maken. De standaardnaam is **AzureIotEdgeApp1** en omdat u een Windows-module bouwt, de uitbreiding moet **Windows.Amd64**. 

3. Selecteer **maken en pushen van IoT Edge-Modules**. 

   De opdracht build- en push start drie bewerkingen. Eerst een nieuwe map wordt gemaakt in de oplossing met de naam **config** die de volledige implementatie oplossing voor het manifest, gebouwd om van gegevens in de sjabloon voor de implementatie en andere bestanden bevat. Ten tweede, deze wordt uitgevoerd `docker build` om de containerinstallatiekopie op basis van het dockerfile dat geschikt is voor uw doel-architectuur te bouwen. Vervolgens wordt deze uitgevoerd `docker push` naar de opslagplaats voor installatiekopieën pushen naar uw containerregister. 

## <a name="deploy-modules-to-device"></a>Modules op apparaat implementeren

De Visual Studio cloud explorer en de hulpprogramma's voor Azure IoT Edge-extensie gebruiken aan het project module implementeren in uw IoT Edge-apparaat. U hebt al een implementatie-manifest voorbereid voor uw scenario, de **deployment.json** bestand in de config-map. U hoeft nu alleen nog maar een apparaat te selecteren dat de implementatie moet ontvangen.

Zorg ervoor dat uw IoT Edge-apparaat actief en werkend is. 

1. Vouw in de Visual Studio cloud explorer, de resources voor uw overzicht van IoT-apparaten. 

2. Met de rechtermuisknop op de naam van de IoT Edge-apparaat dat u wilt ontvangen van de implementatie. 

3. Selecteer **implementatie maken-** .

4. Selecteer in de bestandenverkenner de **deployment.windows amd64** bestand in de map van de configuratie van uw oplossing. 

5. Vernieuwen van de cloud explorer om te zien van de geïmplementeerde modules vermeld in uw apparaat. 


## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

Als u het implementatiemanifest op uw IoT Edge-apparaat toepast, verzamelt de IoT Edge-runtime op het apparaat de informatie over de nieuwe implementatie en wordt deze uitgevoerd. Modules die worden uitgevoerd op het apparaat en die niet zijn opgenomen in het implementatiemanifest, worden gestopt. Alle modules die ontbreken op het apparaat worden gestart. 

U kunt de hulpprogramma's voor IoT Edge-extensie gebruiken om berichten te bekijken wanneer ze op uw IoT-Hub binnenkomen. 

1. Selecteer in de Visual Studio cloud explorer, de naam van uw IoT Edge-apparaat. 

2. In de **acties** in de lijst met **Start Monitoring ingebouwde gebeurtenis eindpunt**. 

3. Bekijk de berichten die binnenkomen in uw IoT-Hub. Het duurt even voor de berichten binnenkomen, omdat het IoT Edge-apparaat moet de nieuwe implementatie ontvangen en alle modules te starten. We de wijzigingen in de code CModule wacht totdat de temperatuur machine 25 graden is bereikt voordat het verzenden van berichten. Het berichttype ook toegevoegd **waarschuwing** in de berichten die deze temperatuur drempelwaarde bereikt. 

   ![Berichten weergeven die binnenkomen in IoT Hub](./media/tutorial-c-module-windows/view-d2c-message.png)

## <a name="edit-the-module-twin"></a>De moduledubbel bewerken

We de moduledubbel CModule gebruikt voor het instellen van de drempelwaarde voor de temperatuur op 25 graden. De moduledubbel kunt u de functionaliteit wijzigen zonder de code van de module bijwerken.

1. Open in Visual Studio, de **deployment.windows amd64.json** bestand. (Niet in het bestand deployment.template. Als er geen de implementatie in het configuratiebestand in solution explorer het manifest, selecteert u de **weergeven van alle bestanden** pictogram in de Verkenner.)

2. Het dubbele CModule vinden en wijzig de waarde van de **temperatureThreshold** parameter voor een nieuwe temperatuur 5 graden tot 10 graden hoger is dan de meest recente gemelde temperatuur. 

3. Sla de **deployment.windows amd64.json** bestand.

4. Volg de implementatiestappen opnieuw uit om te het manifest van de bijgewerkte implementatie toepassen op uw apparaat. 

5. De binnenkomende berichten voor apparaat-naar-cloud bewaken. U ziet de berichten die niet meer totdat de nieuwe temperatuur drempelwaarde is bereikt. 

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en de Azure-resources dat u in dit artikel gebruikt om kosten te vermijden verwijderen. 

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt met code voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Wanneer u gereed voor het bouwen van uw eigen modules bent, vindt u meer informatie over [ontwikkelen van uw eigen IoT Edge-modules](module-development.md) of hoe u [modules met Visual Studio ontwikkelen](how-to-visual-studio-develop-module.md). U kunt doorgaan naar de volgende zelfstudies voor meer informatie over hoe Azure IoT Edge kunnen helpen bij het implementeren van Azure cloudservices om te verwerken en analyseren van gegevens aan de rand.

> [!div class="nextstepaction"]
> [Functies](tutorial-deploy-function.md)
> [Stream Analytics](tutorial-deploy-stream-analytics.md)
> [Machine Learning](tutorial-deploy-machine-learning.md)
> [Custom Vision Service](tutorial-deploy-custom-vision.md)
