---
title: Zelfstudie voor Azure IoT Edge C | Microsoft Docs
description: In deze zelfstudie ziet u hoe u een IoT Edge-module met C-code maakt en deze implementeert op een Edge-apparaat
services: iot-edge
author: shizn
manager: timlt
ms.author: xshi
ms.date: 07/30/2018
ms.topic: tutorial
ms.service: iot-edge
ms.custom: mvc
ms.openlocfilehash: c9d1931f1b78bb19f5e321a19baca45265ea7ab4
ms.sourcegitcommit: 96f498de91984321614f09d796ca88887c4bd2fb
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39413158"
---
# <a name="tutorial-develop-a-c-iot-edge-module-and-deploy-to-your-simulated-device"></a>Zelfstudie: Een IoT Edge-module in C ontwikkelen en implementeren op uw gesimuleerde apparaat

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. In deze zelfstudie leert u een IoT Edge-module te maken die sensorgegevens filtert. In deze zelfstudie leert u het volgende:    

> [!div class="checklist"]
> * Visual Studio Code gebruiken om een IoT Edge-module in C te maken
> * Visual Studio Code en Docker gebruiken om een Docker-installatiekopie te maken en deze te publiceren in een containerregister 
> * De module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven


De IoT Edge-module die u maakt in deze zelfstudie filtert de temperatuurgegevens die door uw apparaat worden gegenereerd. Er worden alleen gegevens upstream gezonden als de temperatuur boven een opgegeven drempelwaarde komt. Dit soort analyse is nuttig om de hoeveelheidgegevens die worden gecommuniceerd naar en opgeslagen in de cloud te reduceren. 

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).

Cloudresources:

* Een standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 

Ontwikkelingsresources:

* [Visual Studio Code](https://code.visualstudio.com/). 
* [C/C++-extensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.cpptools) voor Visual Studio Code (Engelstalig).
* [Azure IoT Edge-extensie](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-edge) voor Visual Studio Code.
* [Docker CE](https://docs.docker.com/install/). 


## <a name="create-a-container-registry"></a>Een containerregister maken
In deze zelfstudie gebruikt u de Azure IoT Edge-extensie voor VS Code om een module te bouwen en maakt u een **containerinstallatiekopie** van de bestanden. Vervolgens pusht u deze installatiekopie naar een **register** waarin uw installatiekopieën worden opgeslagen en beheerd. Tot slot implementeert u de installatiekopie uit het register voor uitvoering op uw IoT Edge-apparaat.  

Voor deze zelfstudie kunt u elk register gebruiken dat compatibel is met Docker. Twee populaire Docker-registerservices die beschikbaar zijn in de cloud zijn [Azure Container Registry](https://docs.microsoft.com/azure/container-registry/) en [Docker Hub](https://docs.docker.com/docker-hub/repos/#viewing-repository-tags). In deze zelfstudie wordt Azure Container Registry gebruikt. 

1. Selecteer in [Azure Portal](https://portal.azure.com) de optie **Een resource maken** > **Containers** > **Azure Container Registry**.
2. Geef een naam op voor het register, kies een abonnement, kies een resourcegroep en stel de SKU in op **Basis**. 
3. Selecteer **Maken**.
4. Zodra het containerregister is gemaakt, gaat u er naartoe en selecteert u **Toegangssleutels**. 
5. Stel de **Gebruiker met beheerdersrechten** in op **Inschakelen**.
6. Kopieer de waarden voor **Aanmeldingsserver**, **Gebruikersnaam** en **wachtwoord**. U gebruikt deze waarden later in deze zelfstudie wanneer u de Docker-installatiekopie publiceert naar uw register en wanneer u de registerreferenties toevoegt aan de Edge-runtime. 

## <a name="create-an-iot-edge-module-project"></a>Een IoT Edge-moduleproject creëren
In de volgende stappen ziet u hoe u een IoT Edge-moduleproject op basis van .NET core 2.0 maakt met behulp van Visual Studio Code en de Azure IoT Edge-extensie.
1. Selecteer in Visual Studio Code **View** > **Integrated Terminal** om de met VS code geïntegreerde terminal te openen.
2. Selecteer **View** > **Command Palette** om het VS Code-opdrachtpalet te openen. 
3. Typ in het opdrachtpalet de opdracht **Azure: Sign in** en voer deze uit. Volg vervolgens de instructies om u aan te melden bij uw Azure-account. Als u zich al hebt aangemeld, kunt u deze stap overslaan.
4. Typ in het opdrachtpalet de opdracht **Azure IoT Edge: New IoT Edge solution** en voer deze uit. Geef in het opdrachtpalet de volgende informatie op om de oplossing te maken: 
   1. Selecteer de map waarin u de oplossing wilt maken. 
   2. Geef een naam op voor de oplossing of houd de standaardnaam **EdgeSolution** aan.
   3. Kies **C Module** als de modulesjabloon. 
   4. Geef de module de naam **CModule**. 
   5. Geef het Azure Container Registry dat u in de vorige sectie hebt gemaakt, op als de opslagplaats voor installatiekopieën voor de eerste module. Vervang **localhost:5000** door de gekopieerde waarde voor de aanmeldingsserver. De uiteindelijke tekenreeks ziet er als volgt uit: **\<registernaam\>.azurecr.io/cmodule**.
 
4. In het VS Code-venster wordt de werkruimte van de IoT Edge-oplossing geladen. Er is een map **modules**, een map **.vscode**, een sjabloonbestand voor het distributiemanifest en een **.env**-bestand. De standaardcode voor de module wordt geïmplementeerd als een pijpmodule. 

5. Als u berichten wilt filteren in een JSON-indeling, moet er een JSON-bibliotheek worden geïmporteerd. U kunt een willekeurige JSON-bibliotheek kiezen of uw eigen bibliotheek schrijven om JSON in uw C-module te parseren. In de onderstaande stappen wordt [Parson](https://github.com/kgabis/parson) als voorbeeld gebruikt.
   1. Download **parson.c** en **parson.h** in de [Parson Github-opslagplaats](https://github.com/kgabis/parson). Plak de twee bestanden in de map **CModule**.
   2. Open **modules** > **CModule** > **CMakeLists.txt**. Voeg de onderstaande regels toe om de Parson-bibliotheek als my_parson te importeren.

      ```
      add_library(my_parson
          parson.c
          parson.h
      )
      ```

   3. Voeg in `target_link_libraries` in **CMakeLists.txt** `my_parson` eraan toe.
   4. Open **modules** > **CModule** > **main.c**. Voeg onder aan de sectie include de onderstaande code toe om `parson.h` voor JSON-ondersteuning op te nemen:

      ```c
      #include "parson.h"
      ```

6. Voeg na de sectie include de globale variabele `temperatureThreshold` toe. Deze variabele bepaalt de waarde die de gemeten temperatuur moet overschrijden voordat de gegevens naar IoT Hub worden verzonden. 

    ```c
    static double temperatureThreshold = 25;
    ```

7. Vervang de hele functie `CreateMessageInstance` door de onderstaande code. Deze functie wijst een context voor de callback toe. 

    ```c
    static MESSAGE_INSTANCE* CreateMessageInstance(IOTHUB_MESSAGE_HANDLE message)
    {
        MESSAGE_INSTANCE* messageInstance = (MESSAGE_INSTANCE*)malloc(sizeof(MESSAGE_INSTANCE));
        if (NULL == messageInstance)
        {
            printf("Failed allocating 'MESSAGE_INSTANCE' for pipelined message\r\n");
        }
        else
        {
            memset(messageInstance, 0, sizeof(*messageInstance));

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
        }

        return messageInstance;
    }
    ```

8. Vervang de hele functie `InputQueue1Callback` door de onderstaande code. Deze functie implementeert het feitelijke berichtenfilter. 

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

9. Voeg een `moduleTwinCallback`-functie toe. Deze methode ontvangt updates van de gewenste eigenschappen van de moduledubbel en updatet de variabele **temperatureThreshold** naar dezelfde waarde. Alle modules hebben hun eigen moduledubbel, waardoor u rechtstreeks in de cloud de code kunt configureren die in een module wordt uitgevoerd.

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

10. Voeg een nieuwe callback voor `moduleTwinCallback` in functie `SetupCallbacksForModule` toe. Vervang de hele functie `SetupCallbacksForModule` door de onderstaande code.

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

11. Sla dit bestand op.

## <a name="build-your-iot-edge-solution"></a>Uw eigen IoT Edge-oplossing bouwen

In de vorige sectie hebt u een IoT Edge-oplossing gemaakt en code toegevoegd aan de CModule waarmee berichten worden gefilterd waarin de gemelde temperatuur van de computer onder de aanvaardbare drempelwaarde komt. Nu moet u de oplossing bouwen als een containerinstallatiekopie en deze naar het containerregister pushen. 

1. Meld u aan bij Docker door de volgende opdracht in te voeren in de geïntegreerde terminal van Visual Studio Code, zodat u de module-installatiekopie naar het ACR kunt pushen: 
     
   ```csh/sh
   docker login -u <ACR username> -p <ACR password> <ACR login server>
   ```
   Gebruik de gebruikersnaam, het wachtwoord en de aanmeldingsserver die u in de eerste sectie hebt gekopieerd uit het Azure-containerregister. Of haal deze opnieuw op in de sectie **Toegangssleutels** van het register in Azure Portal.

2. Open in VS Code Explorer het bestand **deployment.template.json** in de werkruimte van de IoT Edge-oplossing. Dit bestand vertelt `$edgeAgent` dat er twee modules moeten worden geïmplementeerd: **tempSensor** en **CModule**. De waarde `CModule.image` is ingesteld op een installatiekopie met de versie Linux amd64. Zie [Informatie over het gebruiken, configureren en hergebruiken van IoT Edge-modules](module-composition.md) voor meer informatie over distributiemanifesten.

3. Het bestand **deployment.template.json** heeft een sectie **registryCredentials** waarin de referenties van het Docker-register zijn opgeslagen. De actuele gebruikersnaam en het bijbehorende wachtwoord zijn opgeslagen in het .env-bestand dat niet wordt herkend in Git.

4. Voeg de moduledubbel CModule toe aan het distributiemanifest. Voeg de volgende JSON-inhoud onder aan de sectie `moduleContent` in, na de moduledubbel `$edgeHub`: 
    ```json
        "CModule": {
            "properties.desired":{
                "TemperatureThreshold":25
            }
        }
    ```

4. Sla dit bestand op.
5. Klik in VS Code Explorer met de rechtermuisknop op het bestand **deployment.template.json** en selecteer **IoT Edge-oplossing bouwen**. 

Wanneer u Visual Studio Code de opdracht geeft om uw oplossing te bouwen, wordt eerst een `deployment.json`-bestand gemaakt in een nieuwe **configuratiemap** op basis van de informatie in het distributiesjabloon. Vervolgens worden twee opdrachten uitgevoerd in de geïntegreerde terminal: `docker build` en `docker push`. Met deze twee opdrachten wordt uw code gebouwd, `CModule.dll` opgeslagen in een container, en vervolgens naar het containerregister gepusht dat u hebt opgegeven toen u de oplossing initialiseerde. 

U kunt het volledige adres van de containerinstallatiekopie, inclusief de tag, zien in de geïntegreerde terminal van VS Code. Het adres van de installatiekopie is opgebouwd uit informatie uit het `module.json`-bestand, in de indeling **\<opslagplaats\>:\<versie\>-\<platform\>**. Voor deze zelfstudie ziet dit adres er als volgt uit: **registryname.azurecr.io/cmodule:0.0.1-amd64**.

## <a name="deploy-and-run-the-solution"></a>De oplossing implementeren en uitvoeren

1. Configureer de Azure IoT Toolkit-extensie met de verbindingsreeks voor uw IoT Hub: 
    1. Open de VS Code Explorer door **Verkenner** > **bekijken** te selecteren. 
    2. Klik in Explorer op **AZURE IOT HUB EXPLORER** en klik vervolgens op **...**. Klik op **IoT Hub selecteren**. Volg de instructies om u aan te melden bij uw Azure-account en kies uw IoT-hub. 
       Let op: u kunt dit ook instellen door te klikken op **IoT Hub-verbindingsreeks instellen**. Voer in het pop-upvenster de verbindingsreeks voor de IoT-hub in waarmee het IoT Edge-apparaat verbinding maakt.

2. Klik in Azure IoT Hub Devices Explorer met de rechtermuisknop op uw IoT Edge-apparaat. Selecteer vervolgens **Implementatie maken voor IoT Edge-apparaat**. Selecteer het bestand **deployment.json** in de **configuratiemap** en klik vervolgens op **Edge-distributiemanifest selecteren**.

3. Klik op de knop Vernieuwen. U ziet nu dat de nieuwe **CModule** wordt uitgevoerd, samen met de module **TempSensor** en de modules **$edgeAgent** en **$edgeHub**. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

1. Klik op **...** en selecteer **Controle D2C-berichten starten** om de gegevens te controleren die binnenkomen bij de IoT-hub.
2. Als u de D2C-berichten voor een specifiek apparaat wilt controleren, klikt u met de rechtermuisknop op dit apparaat in de lijst en selecteert u **Controle D2C-berichten starten**.
3. Voer de opdracht **Azure IoT Hub: Stop monitoring D2C message** uit in het opdrachtpalet om het controleren van berichten te stoppen. 
4. Als u de moduledubbel wilt weergeven of bewerken, klikt u met de rechtermuisknop op deze moduledubbel in de lijst en selecteert u **Moduledubbel bewerken**. Als u de moduledubbel wilt bijwerken, slaat u het dubbele JSON-bestand op, klikt u met de rechtermuisknop in de editor en selecteert u **Moduledubbel bijwerken**.
5. Als u Docker-logboeken wilt bekijken, kunt u [Docker](https://marketplace.visualstudio.com/items?itemName=PeterJausovec.vscode-docker) voor VS Code installeren. U kunt de actieve modules lokaal zoeken in Docker Explorer. Klik in het contextmenu op **Logboeken weergeven** om ze te bekijken in de geïntegreerde terminal.
 
## <a name="clean-up-resources"></a>Resources opschonen 

<!--[!INCLUDE [iot-edge-quickstarts-clean-up-resources](../../includes/iot-edge-quickstarts-clean-up-resources.md)] -->

Als u doorgaat met het volgende aanbevolen artikel, kunt u de resources en configuraties die u al hebt gemaakt, behouden en opnieuw gebruiken.

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 

> [!IMPORTANT]
> Het verwijderen van de Azure-resources en resourcegroep kan niet ongedaan worden gemaakt. Zodra u dit hebt uitgevoerd, zijn de resourcegroep en alle bijbehorende resources definitief verwijderd. Zorg ervoor dat u niet per ongeluk de verkeerde resourcegroep of resources verwijdert. Als u de IoT Hub in een bestaande resourcegroep hebt gemaakt met resources die u wilt behouden, moet u alleen de IoT Hub-resource zelf verwijderen in plaats van de resourcegroep te verwijderen.
>

Als u alleen de IoT-hub wilt verwijderen, voert u de volgende opdracht uit met behulp van de naam van de hub en van de resourcegroep:

```azurecli-interactive
az iot hub delete --name MyIoTHub --resource-group TestResources
```


Ga als volgt te werk om de hele resourcegroep te verwijderen op naam:

1. Meld u aan bij [Azure Portal](https://portal.azure.com) en klik op **Resourcegroepen**.

2. Typ in het tekstvak **Filteren op naam...** de naam van de resourcegroep die uw IoT Hub bevat. 

3. Klik rechts van de resourcegroep in de lijst met resultaten op **...** en vervolgens op **Resourcegroep verwijderen**.

4. U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Typ de naam van de resourcegroep nogmaals om te bevestigen en klik op **Verwijderen**. Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.



## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module gemaakt die code bevat voor het filteren van onbewerkte gegevens die worden gegenereerd door uw IoT Edge-apparaat. Als u klaar bent om uw eigen modules te bouwen, kunt u meer informatie krijgen over het [ontwikkelen van een C-module met Azure IoT Edge voor Visual Studio Code](how-to-develop-c-module.md). U kunt verdergaan met de volgende zelfstudies om te leren hoe Azure IoT Edge u nog meer kan helpen bij het omzetten van uw gegevens in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Gegevens opslaan met SQL Server-databases](tutorial-store-data-sql-server.md)

<!-- Links -->
[lnk-tutorial1-win]: quickstart.md
[lnk-tutorial1-lin]: quickstart-linux.md
