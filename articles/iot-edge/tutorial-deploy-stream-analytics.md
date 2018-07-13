---
title: 'Zelfstudie: ASA-taken implementeren op Azure IoT Edge-apparaten | Microsoft Docs'
description: Azure Stream Analytics als module implementeren op een Iot Edge-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 06/25/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: 0790f504c978b4302812cffc9b655e817c156da3
ms.sourcegitcommit: 0a84b090d4c2fb57af3876c26a1f97aac12015c5
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2018
ms.locfileid: "38540169"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module---preview"></a>Zelfstudie: Azure Stream Analytics implementeren als een IoT Edge-module (preview)

Veel IoT-oplossingen maken gebruik van analyseservices om inzicht te krijgen in gegevens wanneer deze binnenkomen vanaf IoT-apparaten. Met Azure IoT Edge kunt u de logica van [Azure Stream Analytics][azure-stream] verplaatsen naar het apparaat zelf. Door telemetriestromen te verwerken kunt u de hoeveelheid geüploade gegevens verkleinen en tijd besparen bij het reageren op inzichten waarvoor een actie kan worden uitgevoerd.

Azure IoT Edge en Azure Stream Analytics zijn geïntegreerd, zodat u een Azure Stream Analytics-taak kunt maken in Azure Portal en deze vervolgens zonder aanvullende code kunt implementeren als een IoT Edge-module.  

Azure Stream Analytics biedt een rijk gestructureerde querysyntaxis voor gegevensanalyse, zowel in de cloud als op IoT Edge-apparaten. Raadpleeg de [Azure Stream Analytics-documentatie](../stream-analytics/stream-analytics-edge.md) voor meer informatie over Azure Stream Analytics in IoT Edge.

Met de Stream Analytics-module in deze zelfstudie wordt gedurende 30 seconden (lopende telling) de gemiddelde temperatuur gemeten. Wanneer het gemiddelde de 70 bereikt, wordt met de module voor het apparaat een waarschuwing verzonden dat er actie moet worden ondernomen. In dit geval bestaat deze actie uit het opnieuw instellen van de gesimuleerde temperatuursensor. In een productieomgeving kunt u deze functie gebruiken om een computer uit te schakelen of om voorzorgsmaatregelen te nemen wanneer de temperatuur een gevaarlijk niveau bereikt. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Stream Analytics-taak maken om gegevens te verwerken.
> * Verbinding maken tussen de nieuwe Azure Stream Analytics-taak en andere IoT Edge-modules.
> * De Azure Stream Analytics-taak implementeren op een IoT Edge-apparaat vanuit Azure Portal.

>[!NOTE]
>Azure Stream Analytics-modules voor IoT Edge zijn in de [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Vereisten

* Een IoT-hub
* Het IoT Edge-apparaat dat u hebt gemaakt en geconfigureerd in de quickstart voor [Windows][lnk-quickstart-win] of [Linux][lnk-quickstart-lin]. 

## <a name="create-an-azure-stream-analytics-job"></a>Een Azure Stream Analytics-taak maken

In deze sectie maakt u een Azure Stream Analytics-taak om gegevens uit uw IoT-hub te halen, de verzonden telemetriegegevens van uw apparaat te doorzoeken, en de resultaten vervolgens door te sturen naar een Azure Blob-opslagcontainer. Raadpleeg de sectie Overzicht in de [Stream Analytics-documentatie][azure-stream] voor meer informatie. 

### <a name="create-a-storage-account"></a>Create a storage account

Voor Azure Stream Analytics-taken is een Azure Storage-account vereist om te fungeren als eindpunt voor de taakuitvoer. In het voorbeeld in deze sectie wordt het type Blob-opslag gebruikt. Raadpleeg de sectie Blobs in de [Azure Storage-documentatie][azure-storage] voor meer informatie.

1. Ga in Azure Portal naar **Een resource maken**, voer **Opslagaccount** in het tekstvak in en selecteer vervolgens **Opslagaccount: blob, bestand, tabel, wachtrij**.

2. Voer in het deelvenster **Opslagaccount maken** een naam in voor het opslagaccount, selecteer dezelfde locatie als waar de IoT-hub is opgeslagen, selecteer dezelfde resourcegroep als de IoT-hub en selecteer vervolgens **Maken**. Noteer deze naam voor later gebruik.

    ![Create a storage account][1]


### <a name="create-a-stream-analytics-job"></a>Een Stream Analytics-taak maken

1. Ga in Azure Portal naar **Een Resource maken** > **Internet of Things**, en selecteer vervolgens **Stream Analytics-taak**.

2. Voer in het deelvenster **Nieuwe Stream Analytics-taak** de volgende stappen uit:

   1. Typ in het vak **Taaknaam** een naam voor de taak.
   
   2. Gebruik dezelfde **Resourcegroep** en **Locatie** als de IoT-hub. 

      > [!NOTE]
      > Momenteel worden Azure Stream Analytics-taken in IoT Edge niet ondersteund in de regio VS - west 2. 

   3. Selecteer onder **Hostingomgeving** de optie **Edge**.
    
3. Selecteer **Maken**.

4. Open in de gemaakte taak onder **Taaktopologie** de optie **Invoer**.

   ![Azure Stream Analytics-invoer](./media/tutorial-deploy-stream-analytics/asa_input.png)

5. Selecteer **Stroominvoer toevoegen**. Selecteer vervolgens **Edge Hub**.

6. Voer in het deelvenster **Nieuwe invoer** **temperatuur** in als de invoeralias. 

7. Selecteer **Opslaan**.

8. Open onder **Taaktopologie** de optie **Uitvoer**.

   ![Azure Stream Analytics-uitvoer](./media/tutorial-deploy-stream-analytics/asa_output.png)

9. Selecteer **Toevoegen**. Selecteer vervolgens **Edge Hub**.

10. Voer in het deelvenster **Nieuwe uitvoer** **waarschuwing** in als de uitvoeralias. 

11. Selecteer **Opslaan**.

12. Selecteer onder **Taaktopologie** de optie **Query** en vervang de standaardtekst door de volgende query waarmee een waarschuwing wordt gegenereerd wanneer de gemiddelde temperatuur van de machine 70 graden bereikt in een tijdvenster van 30 seconden:

    ```sql
    SELECT  
        'reset' AS command 
    INTO 
       alert 
    FROM 
       temperature TIMESTAMP BY timeCreated 
    GROUP BY TumblingWindow(second,30) 
    HAVING Avg(machine.temperature) > 70
    ```

13. Selecteer **Opslaan**.

14. Selecteer onder **Configureren** de optie **IoT Edge-instellingen**.

15. Selecteer in de vervolgkeuzelijst uw **opslagaccount**.

16. Selecteer in het veld **Container** de optie **Nieuw maken** en geef een naam op voor de opslagcontainer. 

17. Selecteer **Opslaan**. 


## <a name="deploy-the-job"></a>De taak implementeren

U bent nu klaar om de Azure Stream Analytics-taak te implementeren op uw IoT Edge-apparaat.

1. Ga in Azure Portal, in uw IoT-hub, naar **IoT Edge** en open vervolgens de detailpagina van het IoT Edge-apparaat.

2. Selecteer **Modules instellen**.  

   Als u de tempSensor-module al eerder op dit apparaat hebt geïmplementeerd, wordt deze mogelijk automatisch ingevuld. Als dit niet het geval is, voegt u de module toe met behulp van de volgende stappen:

   1. Klik op **Toevoegen** en selecteer **IoT Edge-module**.
   2. Typ **tempSensor** als de naam.
   3. Voer **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** in als de URI voor de installatiekopie. 
   4. Laat de overige instellingen ongewijzigd.
   5. Selecteer **Opslaan**.

3. Voeg uw Azure Stream Analytics Edge-taak toe met behulp van de volgende stappen:

   1. Klik op **Toevoegen** en selecteer **Azure Stream Analytics-module**.
   2. Selecteer uw abonnement en de Azure Stream Analytics-taak die u hebt gemaakt. 
   3. Selecteer **Opslaan**.

4. Selecteer **Volgende**.

5. Vervang de standaardwaarde in **Routes** door de volgende code. Werk _{moduleName}_ bij met de naam van de Azure Stream Analytics-module. De module moet dezelfde naam hebben als de taak waarmee deze is gemaakt. 

    ```json
    {
        "routes": {
            "telemetryToCloud": "FROM /messages/modules/tempSensor/* INTO $upstream",
            "alertsToCloud": "FROM /messages/modules/{moduleName}/* INTO $upstream",
            "alertsToReset": "FROM /messages/modules/{moduleName}/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")",
            "telemetryToAsa": "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/{moduleName}/inputs/temperature\")"
        }
    }
    ```

6. Selecteer **Volgende**.

7. Selecteer in de stap **Implementatie beoordelen** de optie **Verzenden**.

8. Ga terug naar de detailpagina van het apparaat en selecteer vervolgens **Vernieuwen**.  

    U ziet nu dat de nieuwe Stream Analytics-module wordt uitgevoerd, samen met de IoT Edge-agentmodule en de IoT Edge-hub.

    ![Module-uitvoer][7]

## <a name="view-data"></a>Gegevens weergeven

U kunt nu naar het IoT Edge-apparaat gaan om de interactie tussen de Azure Stream Analytics-module en de tempSensor-module te controleren.

1. Controleer of alle modules worden uitgevoerd in Docker:

   ```cmd/sh
   iotedge list  
   ```
<!--
   ![Docker output][8]
-->
2. Bekijk alle systeemlogboeken en metrische gegevens. Gebruik de naam van de Stream Analytics-module:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

U ziet nu dat de temperatuur van de machine geleidelijk stijgt totdat deze gedurende 30 seconden 70 graden is. Vervolgens worden met de Stream Analytics-module de beginwaarden ingesteld. De temperatuur van de machine zakt nu weer naar 21 graden. 

   ![Docker-logboek][9]

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

<!--
   ![Delete](./media/iot-edge-quickstarts-clean-up-resources/iot-edge-delete-resource-group.png)
-->
4. U wordt gevraagd om het verwijderen van de resourcegroep te bevestigen. Typ de naam van de resourcegroep nogmaals om te bevestigen en klik op **Verwijderen**. Na enkele ogenblikken worden de resourcegroep en alle resources in de groep verwijderd.

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Stream Analytics-taak geconfigureerd om gegevens van uw IoT Edge-apparaat te analyseren. Vervolgens hebt u deze Azure Stream Analytics-module op een IoT Edge-apparaat geladen om temperatuurstijgingen lokaal te verwerken en erop te reageren, en de samengevoegde gegevensstroom te verzenden naar de cloud. Ga verder met de andere zelfstudies om te zien hoe u met Azure IoT Edge meer oplossingen voor uw bedrijf kunt maken.

> [!div class="nextstepaction"] 
> [Een Azure Machine Learning-model implementeren als een module][lnk-ml-tutorial]

<!-- Images. -->
[1]: ./media/tutorial-deploy-stream-analytics/storage.png
[4]: ./media/tutorial-deploy-stream-analytics/add_device.png
[5]: ./media/tutorial-deploy-stream-analytics/asa_job.png
[6]: ./media/tutorial-deploy-stream-analytics/set_module.png
[7]: ./media/tutorial-deploy-stream-analytics/module_output2.png
[8]: ./media/tutorial-deploy-stream-analytics/docker_output.png
[9]: ./media/tutorial-deploy-stream-analytics/docker_log.png
[10]: ./media/tutorial-deploy-stream-analytics/storage_settings.png
[11]: ./media/tutorial-deploy-stream-analytics/temp_module.png


<!-- Links -->
[lnk-what-is-iot-edge]: what-is-iot-edge.md
[lnk-module-dev]: module-development.md
[iot-hub-get-started-create-hub]: ../../includes/iot-hub-get-started-create-hub.md
[azure-iot]: https://docs.microsoft.com/azure/iot-hub/
[azure-storage]: https://docs.microsoft.com/azure/storage/
[azure-stream]: https://docs.microsoft.com/azure/stream-analytics/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-quickstart-win]: quickstart.md
[lnk-quickstart-lin]: quickstart-linux.md
[lnk-module-tutorial]: tutorial-csharp-module.md
[lnk-ml-tutorial]: tutorial-deploy-machine-learning.md

