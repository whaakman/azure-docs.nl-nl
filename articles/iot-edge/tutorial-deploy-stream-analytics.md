---
title: 'Zelfstudie: ASA-taken implementeren op Azure IoT Edge-apparaten | Microsoft Docs'
description: In deze zelfstudie implementeert u Azure Stream Analytics als module op een Iot Edge-apparaat
author: kgremban
manager: timlt
ms.author: kgremban
ms.date: 09/21/2018
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc
ms.openlocfilehash: de856aed32b596209f7af2861b76ed6648d0caca
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/10/2018
ms.locfileid: "49067819"
---
# <a name="tutorial-deploy-azure-stream-analytics-as-an-iot-edge-module-preview"></a>Zelfstudie: Azure Stream Analytics implementeren als een IoT Edge-module (preview)

Veel IoT-oplossingen maken gebruik van analyseservices om inzicht te krijgen in gegevens wanneer deze binnenkomen vanaf IoT-apparaten. Met Azure IoT Edge kunt u de logica van [Azure Stream Analytics][azure-stream] verplaatsen naar het apparaat zelf. Door telemetriestromen te verwerken kunt u de hoeveelheid geüploade gegevens verkleinen en tijd besparen bij het reageren op inzichten waarvoor een actie kan worden uitgevoerd.

Azure IoT Edge en Azure Stream Analytics zijn geïntegreerd, zodat u een Azure Stream Analytics-taak kunt maken in Azure Portal en deze vervolgens zonder aanvullende code kunt implementeren als een IoT Edge-module.  

Azure Stream Analytics biedt een rijk gestructureerde querysyntaxis voor gegevensanalyse, zowel in de cloud als op IoT Edge-apparaten. Raadpleeg de [Azure Stream Analytics-documentatie](../stream-analytics/stream-analytics-edge.md) voor meer informatie over Azure Stream Analytics in IoT Edge.

Met de Stream Analytics-module in deze zelfstudie wordt gedurende 30 seconden (lopende telling) de gemiddelde temperatuur gemeten. Wanneer het gemiddelde de 70 bereikt, wordt met de module voor het apparaat een waarschuwing verzonden dat er actie moet worden ondernomen. In dit geval bestaat deze actie uit het opnieuw instellen van de gesimuleerde temperatuursensor. In een productieomgeving kunt u deze functie gebruiken om een computer uit te schakelen of om voorzorgsmaatregelen te nemen wanneer de temperatuur een gevaarlijk niveau bereikt. 

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Stream Analytics-taak maken om gegevens te verwerken.
> * Verbinding maken tussen de nieuwe Azure Stream Analytics-taak en andere IoT Edge-modules.
> * De Azure Stream Analytics-taak implementeren op een IoT Edge-apparaat vanuit Azure Portal.

<center>
![Architectuurdiagram voor zelfstudie](./media/tutorial-deploy-stream-analytics/ASATutorialDiagram.png)
</center>

>[!NOTE]
>Azure Stream Analytics-modules voor IoT Edge zijn in de [openbare preview](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt uw ontwikkelcomputer of een virtuele machine gebruiken als een Edge-apparaat door de stappen te volgen in de snelstart voor [Linux-](quickstart-linux.md) of [Windows-apparaten](quickstart.md).

Cloudresources:

* Een gratis [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure. 


## <a name="create-an-azure-stream-analytics-job"></a>Een Azure Stream Analytics-taak maken

In deze sectie maakt u een Azure Stream Analytics-taak om gegevens uit uw IoT-hub te halen, de verzonden telemetriegegevens van uw apparaat te doorzoeken, en de resultaten vervolgens door te sturen naar een Azure Blob-opslagcontainer. 

### <a name="create-a-storage-account"></a>Create a storage account

Wanneer u een Azure Stream Analytics-taak maakt die op een IoT Edge-apparaat moet worden uitgevoerd, moet deze worden opgeslagen op een manier die door het apparaat kan worden aangeroepen. U kunt een bestaand Azure-opslagaccount gebruiken of een nieuw maken. 

1. Ga in de Azure-portal achtereenvolgens naar **Een resource maken** > **Opslag** > **Opslagaccount - blob, bestand, tabel, wachtrij**. 

1. Geef de volgende waarden op om uw opslagaccount te maken:

   | Veld | Waarde |
   | ----- | ----- |
   | Naam | Voer een unieke naam in voor het opslagaccount. | 
   | Locatie | Kies een locatie dicht bij u in de buurt. |
   | Abonnement | Kies het hetzelfde abonnement als uw IoT-hub. |
   | Resourcegroep | Het wordt aangeraden om dezelfde resourcegroep te gebruiken voor alle test-resources die u maakt met de snelstartgidsen en zelfstudies voor IoT Edge, zoals **IoTEdgeResources**. |

1. Houd de standaardwaarden voor de andere velden aan en selecteer **Maken**. 

### <a name="create-a-new-job"></a>Een nieuwe taak maken

1. Ga in de Azure-portal naar **Een resource maken** > **Internet of Things** > **Stream Analytics-taak**.

1. Geef de volgende waarden op om de taak te maken:

   | Veld | Waarde |
   | ----- | ----- |
   | Taaknaam | Geef de taak een naam. Bijvoorbeeld **IoTEdgeJob**. | 
   | Abonnement | Kies het hetzelfde abonnement als uw IoT-hub. |
   | Resourcegroep | Het wordt aangeraden om dezelfde resourcegroep te gebruiken voor alle test-resources die u maakt met de snelstartgidsen en zelfstudies voor IoT Edge, zoals **IoTEdgeResources**. |
   | Locatie | Kies een locatie dicht bij u in de buurt. | 
   | Hostingomgeving | Selecteer **Edge**. |
 
1. Selecteer **Maken**.

### <a name="configure-your-job"></a>Uw taak configureren

Zodra uw Stream Analytics-taak is gemaakt in de Azure-portal, kunt u deze configureren met invoer, uitvoer en een query die moeten worden uitgevoerd op de gegevens die worden doorgegeven. 

Met behulp van de drie elementen invoer, uitvoer en query wordt in deze sectie een taak gemaakt waarbij temperatuurgegevens van het IoT Edge-apparaat worden ontvangen. Deze gegevens worden in een tijdvenster met een lopende telling van 30 seconden geanalyseerd. Als de gemiddelde temperatuur in dit tijdvenster de 70 graden overschrijdt, wordt een waarschuwing naar het IoT Edge-apparaat verzonden. In de volgende sectie, waarin u de taak gaat implementeren, moet u opgeven waar de gegevens precies vandaan komen en waar ze naartoe gaan.  

1. Navigeer naar uw Stream Analytics-taak in de Azure-portal. 

1. Onder **Taaktopologie** selecteert u **Invoer** en vervolgens **Stroominvoer toevoegen**.

   ![Azure Stream Analytics-invoer](./media/tutorial-deploy-stream-analytics/asa_input.png)

1. Kies **Edge Hub** in de vervolgkeuzelijst.

1. Voer in het deelvenster **Nieuwe invoer** **temperatuur** in als de invoeralias. 

1. Houd de standaardwaarden voor de andere velden aan en selecteer **Opslaan**.

1. Open onder **Taaktopologie** de optie **Uitvoer** en selecteer vervolgens **Toevoegen**.

   ![Azure Stream Analytics-uitvoer](./media/tutorial-deploy-stream-analytics/asa_output.png)

1. Kies **Edge Hub** in de vervolgkeuzelijst.

1. Voer in het deelvenster **Nieuwe uitvoer** **waarschuwing** in als de uitvoeralias. 

1. Houd de standaardwaarden voor de andere velden aan en selecteer **Opslaan**.

1. Selecteer **Query** onder **Taaktopologie**.

1. Vervang de standaardtekst door de volgende query. De SQL-code verzendt een opdracht voor opnieuw instellen naar de waarschuwingsuitvoer als de gemiddelde temperatuur van de machine in een tijdvenster van 30 seconden de 70 graden bereikt. De opdracht voor opnieuw instellen is vooraf in de sensor geprogrammeerd als actie die kan worden uitgevoerd. 

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

1. Selecteer **Opslaan**.

### <a name="configure-iot-edge-settings"></a>Instellingen voor IoT Edge configureren

Als u uw Stream Analytics-taak wilt voorbereiden om te worden geïmplementeerd op een IoT Edge-apparaat, moet u de taak aan een container in een opslagaccount koppelen. Wanneer u uw taak gaat implementeren, wordt de taakdefinitie geëxporteerd naar de opslagcontainer. 

1. Bij **Configureren** selecteert u **Opslagaccountinstellingen**.

1. Selecteer **Opslagaccount toevoegen**. 

1. Selecteer in de vervolgkeuzelijst uw **opslagaccount**.

1. Selecteer in het veld **Container** de optie **Nieuw maken** en geef een naam op voor de opslagcontainer. 

1. Selecteer **Opslaan**. 

## <a name="deploy-the-job"></a>De taak implementeren

U bent nu klaar om de Azure Stream Analytics-taak te implementeren op uw IoT Edge-apparaat. 

In deze sectie gaat u de wizard **Modules instellen** in de Azure-portal gebruiken om een *distributiemanifest* te maken. Het distributiemanifest is een JSON-bestand waarin alle modules worden beschreven die op een apparaat worden geïmplementeerd, waarin de containerregisters worden beschreven waarin installatiekopieën van de modules worden bewaard, hoe de modules moeten worden beheerd en hoe de modules met elkaar kunnen communiceren. Uw IoT Edge-apparaat haalt het distributiemanifest op uit IoT Hub en gebruikt vervolgens de informatie die zich daarin bevindt, om alle modules die eraan zijn toegewezen te implementeren en te configureren. 

In deze zelfstudie gaat u twee modules implementeren. De eerste is **tempSensor**: dit is een module die een temperatuur- en vochtigheidssensor simuleert. De tweede is uw Stream Analytics-taak. De sensormodule levert de gegevensstroom aan die door de taakquery wordt geanalyseerd. 

1. Ga in Azure Portal, in uw IoT-hub, naar **IoT Edge** en open vervolgens de detailpagina van het IoT Edge-apparaat.

1. Selecteer **Modules instellen**.  

1. Als u de tempSensor-module al eerder op dit apparaat hebt geïmplementeerd, wordt deze mogelijk automatisch ingevuld. Als dit niet het geval is, voegt u de module toe met behulp van de volgende stappen:

   1. Klik op **Toevoegen** en selecteer **IoT Edge-module**.
   1. Typ **tempSensor** als de naam.
   1. Voer **mcr.microsoft.com/azureiotedge-simulated-temperature-sensor:1.0** in als de URI voor de installatiekopie. 
   1. Laat de overige instellingen ongewijzigd en selecteer **Opslaan**.

1. Voeg uw Azure Stream Analytics Edge-taak toe met behulp van de volgende stappen:

   1. Klik op **Toevoegen** en selecteer **Azure Stream Analytics-module**.
   1. Selecteer uw abonnement en de Azure Stream Analytics-taak die u hebt gemaakt. 
   1. Selecteer **Opslaan**.

1. Zodra uw Stream Analytics-taak is gepubliceerd naar de opslagcontainer die u hebt gemaakt, klikt u op de naam van de module om te zien hoe een Stream Analytics-module is opgebouwd. 

   De URI van de installatiekopie verwijst naar een standaardinstallatiekopie van Azure Stream Analytics. Dit is de dezelfde installatiekopie die wordt gebruikt voor elke taak die op een IoT Edge-apparaat wordt geïmplementeerd. 

   De moduledubbel wordt geconfigureerd met een gewenste eigenschap genaamd **ASAJobInfo**. De waarde van deze eigenschap verwijst naar de taakdefinitie in uw opslagcontainer. Deze eigenschap bepaalt hoe de installatiekopie van Stream Analytics wordt geconfigureerd met uw specifieke taakgegevens. 

1. Sluit de modulepagina.

1. Noteer de naam van uw Stream Analytics-module omdat u deze in de volgende stap nodig hebt, en selecteer daarna **Volgende** om door te gaan.

1. Vervang de standaardwaarde in **Routes** door de volgende code. Werk alle drie de instanties van _{moduleName}_ bij met de naam van de Azure Stream Analytics-module. 

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

   De routes die u hier declareert bepalen de gegevensstroom die via het IoT Edge-apparaat gaat. De telemetriegegevens van tempSensor worden naar IoT Hub verzonden en naar de **temperatuurinvoer** die is geconfigureerd in de Stream Analytics-taak. De **waarschuwingsuitvoerberichten** worden verzonden naar IoT Hub en naar de tempSensor-module om de opdracht voor opnieuw instellen te activeren. 

1. Selecteer **Volgende**.

1. Selecteer in de stap **Implementatie beoordelen** de optie **Verzenden**.

1. Ga terug naar de detailpagina van het apparaat en selecteer vervolgens **Vernieuwen**.  

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
1. Bekijk alle systeemlogboeken en metrische gegevens. Gebruik de naam van de Stream Analytics-module:

   ```cmd/sh
   iotedge logs -f {moduleName}  
   ```

U ziet nu dat de temperatuur van de machine geleidelijk stijgt totdat deze gedurende 30 seconden 70 graden is. Vervolgens worden met de Stream Analytics-module de beginwaarden ingesteld. De temperatuur van de machine zakt nu weer naar 21 graden. 

   ![Docker-logboek][9]

## <a name="clean-up-resources"></a>Resources opschonen 

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat. 

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen. 
 
[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

[!INCLUDE [iot-edge-clean-up-local-resources](../../includes/iot-edge-clean-up-local-resources.md)]


## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een Azure Stream Analytics-taak geconfigureerd om gegevens van uw IoT Edge-apparaat te analyseren. Vervolgens hebt u deze Azure Stream Analytics-module op een IoT Edge-apparaat geladen om temperatuurstijgingen lokaal te verwerken en erop te reageren, en de samengevoegde gegevensstroom te verzenden naar de cloud. Ga verder met de andere zelfstudies om te zien hoe u met Azure IoT Edge meer oplossingen voor uw bedrijf kunt maken.

> [!div class="nextstepaction"] 
> [Een Azure Machine Learning-model implementeren als een module][lnk-ml-tutorial]

<!-- Images. -->
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

