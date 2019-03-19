---
title: Azure Machine Learning implementeert op een apparaat - Azure IoT Edge | Microsoft Docs
description: In deze zelfstudie maakt u een Azure Machine Learning-model maken en vervolgens als een module aan een edge-apparaat implementeren
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 03/07/2019
ms.topic: tutorial
ms.service: iot-edge
services: iot-edge
ms.custom: mvc, seodec18
ms.openlocfilehash: 985f1f73fbfc8c75df8393615fca32f5d1c08b9d
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "58078309"
---
# <a name="tutorial-deploy-azure-machine-learning-as-an-iot-edge-module-preview"></a>Zelfstudie: Azure Machine Learning als een IoT Edge-module implementeren (preview)

U kunt IoT Edge-modules gebruiken voor het implementeren van code die uw bedrijfslogica rechtstreeks op uw IoT Edge-apparaten implementeert. Deze zelfstudie laat u stapsgewijs zien hoe u een Azure Machine Learning-module kunt implementeren waarmee wordt voorspeld wanneer een apparaat mislukt op basis van gesimuleerde machinetemperatuurgegevens. Zie [Documentatie voor Azure Machine Learning](../machine-learning/service/how-to-deploy-to-iot.md) voor meer informatie over de service Azure Machine Learning op IoT Edge.

De Azure Machine Learning-module die u in deze zelfstudie maakt, leest de omgevingsgegevens die door uw apparaat zijn gegenereerd en markeert de berichten als afwijkend of niet.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een Azure Machine Learning-module maken
> * Een module-container naar een Azure-containerregister zenden
> * Een Azure Machine Learning-module implementeren op uw IoT Edge-apparaat
> * Gegenereerde gegevens weergeven

>[!NOTE]
>Azure Machine Learning-modules in Azure IoT Edge zijn in de openbare preview.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]


## <a name="prerequisites"></a>Vereisten

Een Azure IoT Edge-apparaat:

* U kunt een virtuele machine van Azure gebruiken als een IoT Edge-apparaat met de volgende stappen in de Quick Start voor [Linux](quickstart-linux.md).
* De Azure Machine Learning-module biedt geen ondersteuning voor Windows-containers.
* De Azure Machine Learning-module biedt geen ondersteuning voor ARM-processors.

Cloudresources:

* Een gratis of standaard [IoT Hub](../iot-hub/iot-hub-create-through-portal.md)-laag in Azure.
* Een Azure Machine Learning-werkruimte. Volg de instructies in [gebruik van de Azure-portal aan de slag met Azure Machine Learning](../machine-learning/service/quickstart-get-started.md) een te maken en informatie over het gebruik van deze.
   * Noteer de naam van de werkruimte, resourcegroep en abonnements-ID. Deze waarden, allemaal verkrijgbaar in de werkruimte overzicht in Azure portal. U gebruikt deze waarden later in de zelfstudie een laptop met een Azure verbinden met de werkruimteresources van uw. 


### <a name="disable-process-identification"></a>Procesidentificatie uitschakelen

>[!NOTE]
>
> In de preview-fase biedt Azure Machine Learning geen ondersteuning voor de beveiligingsfunctie voor procesidentificatie die standaard is ingeschakeld voor IoT Edge.
> Hieronder ziet u hoe u deze functie kunt uitschakelen. Dit is echter niet geschikt voor productieomgevingen. Deze stappen zijn alleen nodig op Linux-apparaten. 

Als u wilt uitschakelen serverproces-id op uw IoT Edge-apparaat, moet u het IP-adres en poort voor **workload_uri** en **management_uri** in de **verbinding** sectie van de configuratie van de IoT Edge-daemon.

Haal eerst het IP-adres op. Voer in de opdrachtregel `ifconfig` in en kopieer het IP-adres van de **docker0**-interface.

Bewerk het configuratiebestand voor de IoT Edge-daemon:

```cmd/sh
sudo nano /etc/iotedge/config.yaml
```

Werk de sectie **verbinding maken** van de configuratie bij met uw IP-adres. Bijvoorbeeld:
```yaml
connect:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Voer in de sectie **luisteren** van de configuratie dezelfde adressen in. Bijvoorbeeld:

```yaml
listen:
  management_uri: "http://172.17.0.1:15580"
  workload_uri: "http://172.17.0.1:15581"
```

Opslaan en sluiten van het configuratiebestand.

Maken van een omgevingsvariabele IOTEDGE_HOST met het adres management_uri (om in te stellen deze permanent, toe te voegen aan `/etc/environment`). Bijvoorbeeld:

```cmd/sh
export IOTEDGE_HOST="http://172.17.0.1:15580"
```

Start de IoT Edge-service de wijzigingen van kracht te laten worden.

```cmd/sh
sudo systemctl restart iotedge
```

## <a name="create-and-deploy-azure-machine-learning-module"></a>Maken en implementeren van Azure Machine Learning-module

In deze sectie maakt u getrainde machine learning modelbestanden converteren en container in een Azure Machine Learning-service. Alle onderdelen die vereist zijn voor de Docker-installatiekopie bevinden zich in de [AI-werkset voor Azure IoT Edge Git-repo](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial). Volg deze stappen voor het uploaden van die opslagplaats naar Microsoft Azure-notitieblokken voor het maken van de container en pusht u deze naar Azure Container Registry.


1. Navigeer naar uw Azure-notitieblokken-projecten. Krijgt u hun vanuit uw werkruimte van de service Azure Machine Learning in de [Azure-portal](https://portal.azure.com) of door aan te melden bij [Microsoft Azure-notitieblokken](https://notebooks.azure.com/home/projects) met uw Azure-account.

2. Selecteer **GitHub-opslagplaats uploaden**.

3. Geef de naam van de volgende GitHub-opslagplaats: `Azure/ai-toolkit-iot-edge`. Schakel de **openbare** vak als u wilt het privé houden van uw project. Selecteer **importeren**. 

4. Nadat het importeren is voltooid, gaat u naar de nieuwe **ai-werkset-iot-edge** project en open de **IoT Edge anomaly detection zelfstudie** map. 

5. Controleren of uw project wordt uitgevoerd. Als dit niet het geval is, selecteert u **worden uitgevoerd op de gratis Compute**.

   ![Uitgevoerd op de gratis compute](./media/tutorial-deploy-machine-learning/run-on-free-compute.png)

6. Open de **aml_config/config.json** bestand.

7. Het configuratiebestand zodat de waarden voor uw Azure-abonnement-ID, een resourcegroep die in uw abonnement en de naam van uw Azure Machine Learning-service-werkruimte bewerken. U kunt deze waarden uit de **overzicht** sectie van uw werkruimte in Azure. 

8. Sla het configuratiebestand.

9. Open de **00-anomaliedetectie-detectie-tutorial.ipynb** bestand.

10. Wanneer u hierom wordt gevraagd, selecteert u de **Python 3.6** kernel Selecteer **ingesteld Kernel**.

11. De eerste cel in het notitieblok volgens de instructies in de opmerkingen bewerken. Gebruik de dezelfde resourcegroep bevinden, abonnements-ID en de naam van de werkruimte die u hebt toegevoegd in het configuratiebestand.

12. Uitvoeren van de cellen in de notebook door ze te selecteren en te selecteren **uitvoeren** of drukken `Shift + Enter`.

    >[!TIP]
    >Sommige van de cellen in de anomaliedetectie-detectie zelfstudie notebook zijn optioneel, omdat ze resources dat sommige gebruikers mogelijk of kunnen niet nog hebt, zoals een IoT-Hub maken. Als u de resourcegegevens van een bestaande in de eerste cel, ontvangt u fouten als u de cellen die nieuwe resources niet maken omdat er dubbele resources niet gemaakt door Azure wordt uitgevoerd. Dit is prima, en u kunt de fouten negeren of volledig die optioneel gedeeltes overslaan. 

Na het voltooien van de stappen in het notitieblok, hebt u een model voor afwijkingsdetectie, ingebouwde als een Docker-container-installatiekopie, getraind en die installatiekopie naar Azure Container Registry gepusht. Vervolgens het model getest en ten slotte deze geïmplementeerd naar uw IoT Edge-apparaat. 

## <a name="view-container-repository"></a>Containeropslagplaats weergeven

Controleer dat uw containerinstallatiekopie is gemaakt en opgeslagen in Azure container registry die is gekoppeld aan uw machine learning-omgeving. De notebook die u hebt gebruikt in de vorige sectie worden automatisch opgegeven installatiekopie van de container en de registerreferenties voor uw IoT Edge-apparaat, maar u moet weten waar ze zijn opgeslagen, zodat u kunt de gegevens zelf vinden later opnieuw. 

1. In de [Azure-portal](https://portal.azure.com), gaat u naar de service Machine Learning-werkruimte. 

2. De **overzicht** sectie vindt u de details van de werkruimte als ook alle bijbehorende resources. Selecteer de **register** waarde, de naam van uw werkruimte gevolgd door willekeurige getallen moet. 

3. Selecteer in het containerregister **opslagplaatsen**. Er is een opslagplaats met de naam **tempanomalydetection** die is gemaakt door de notebook die u in de vorige sectie hebt uitgevoerd. 

4. Selecteer **tempanomalydetection**. U ziet dat de opslagplaats een tag heeft: **1**. 

   Nu dat u de naam van het containerregister, de naam van de opslagplaats en het label weet, weet u het pad van de volledige installatiekopie van de container. Afbeeldingspaden er als volgt uitzien  **\<registry_name\>.azurecr.io/tempanomalydetection:1**. Pad van de installatiekopie kunt u deze container implementeren in IoT Edge-apparaten. 

5. Selecteer in het containerregister **toegangssleutels**. U ziet een aantal van de referenties voor toegang, met inbegrip van **aanmeldingsserver** en de **gebruikersnaam**, en **wachtwoord** voor een gebruiker met beheerdersrechten.

   Deze referenties kunnen worden opgenomen in het manifest van de implementatie van uw IoT-Edge Apparaattoegang geven tot pull-containerinstallatiekopieën uit het register. 

Nu weet u waar de Machine Learning-container-installatiekopie is opgeslagen. De volgende sectie begeleid om te zien hoe deze wordt uitgevoerd als een geïmplementeerde module op uw IoT Edge-apparaat. 

## <a name="view-generated-data"></a>Gegenereerde gegevens weergeven

U kunt berichten weergeven die worden gegenereerd vanaf elke IoT Edge-module, en u kunt berichten weergeven die worden bezorgd bij de IoT Edge-hub.

### <a name="view-data-on-your-iot-edge-device"></a>Gegevens weergeven op het IoT Edge-apparaat

Op het IoT Edge-apparaat kunt u de berichten weergeven die worden verzonden vanaf elke afzonderlijke module.

U wilt gebruiken `sudo` voor verhoogde machtigingen om uit te voeren `iotedge` opdrachten. Meld u af en weer bent aangemeld met het apparaat automatisch bijgewerkt uw machtigingen.

1. Geef alle modules op het IoT Edge-apparaat weer.

   ```cmd/sh
   iotedge list
   ```

2. Geef de berichten weer die zijn verzonden vanaf een specifiek apparaat. Gebruik de modulenaam uit de uitvoer van de vorige opdracht.

   ```cmd/sh
   iotedge logs <module_name> -f
   ```

### <a name="view-data-arriving-at-your-iot-hub"></a>Gegevens weergeven die binnenkomen bij de IoT-hub

U kunt de apparaat-naar-cloud-berichten die de IoT-hub ontvangt, weergeven met behulp van de [Azure IoT Hub Toolkit-extensie voor Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vsciot-vscode.azure-iot-toolkit) (voorheen Azure IoT Toolkit-extensie).

In de volgende stappen ziet u hoe u Visual Studio Code kunt instellen om apparaat-naar-cloud-berichten te controleren die binnenkomen bij de IoT-hub.

1. Selecteer in Visual Studio Code **IoT Hub-apparaten**.

2. Selecteer in het menu **...** en vervolgens **Set IoT Hub-verbindingsreeksen**.

   ![IoT Hub-verbindingsreeks instellen](./media/tutorial-deploy-machine-learning/set-connection.png)

3. Voer in het tekstvak dat bovenaan de pagina wordt geopend de iothubowner-verbindingsreeks in voor uw IoT Hub. Uw IoT Edge-apparaat moet worden weergegeven in de lijst met IoT Hub-apparaten.

4. Selecteer **...** opnieuw en vervolgens **Controle D2C-bericht starten**.

5. Bekijk de berichten die de tempSensor elke vijf seconden verzendt. De hoofdtekst van het bericht bevat een eigenschap genaamd **anomaliedetectie**, waarmee u de machinelearningmodule met een waarde waar of ONWAAR. De eigenschap **AzureMLResponse** bevat de waarde ‘OK’ als het model is uitgevoerd.

   ![Reactie van Azure Machine Learning-service in de hoofdtekst van het bericht](./media/tutorial-deploy-machine-learning/ml-output.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u van plan bent door te gaan met het volgende aanbevolen artikel, kunt u de resources en configuraties die u hebt gemaakt behouden en opnieuw gebruiken. U kunt ook hetzelfde IoT Edge-apparaat blijven gebruiken als een testapparaat.

Anders kunt u de lokale configuraties en Azure-resources die u in dit artikel hebt gemaakt, verwijderen om kosten te voorkomen.

[!INCLUDE [iot-edge-clean-up-cloud-resources](../../includes/iot-edge-clean-up-cloud-resources.md)]

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u een IoT Edge-module geïmplementeerd die werd aangedreven door Azure Machine Learning. U kunt doorgaan met elke andere zelfstudie om te leren waarmee Azure IoT Edge u nog meer kan helpen uw gegevens om te zetten in bedrijfsinzichten.

> [!div class="nextstepaction"]
> [Afbeeldingen classificeren met Custom Vision Service](tutorial-deploy-custom-vision.md)

