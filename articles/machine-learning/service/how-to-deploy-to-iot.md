---
title: Modellen implementeren op IoT Edge-apparaten van Azure Machine Learning-service | Microsoft Docs
description: Leer hoe u een model implementeren vanuit de Azure Machine Learning-service naar Azure IoT Edge-apparaten. Een model implementeert naar een edge-apparaat, kunt u mogelijk om gegevens op het apparaat in plaats van deze naar de cloud verzonden en er wordt gewacht op een antwoord te beoordelen.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: shipatel
author: shivanipatel
manager: cgronlun
ms.reviewer: larryfr
ms.date: 09/24/2018
ms.openlocfilehash: 66370aec76044454ab4f11eb432fe2e9b0cdb9cf
ms.sourcegitcommit: 609c85e433150e7c27abd3b373d56ee9cf95179a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48248582"
---
# <a name="prepare-to-deploy-models-on-iot-edge"></a>Voorbereidingen voor het implementeren van modellen op IoT Edge

In dit document leert u hoe u de Azure Machine Learning-service gebruiken voor het voorbereiden van een getraind model voor implementatie naar een Azure IoT Edge-apparaat.

Een Azure IoT Edge-apparaat is een op Linux of Windows-apparaat waarop de Azure IoT Edge-runtime wordt uitgevoerd. Machine learning-modellen kunnen worden geïmplementeerd op deze apparaten als IoT Edge-modules. Een model implementeert naar een IoT Edge-apparaat staat toe dat het apparaat rechtstreeks gebruik van het model, in plaats van dat voor het verzenden van gegevens naar de cloud voor verwerking. U krijgt snellere reactietijden en minder gegevens worden overgebracht.

Voordat een model implementeert naar een edge-apparaat, gebruikt u de stappen in dit document om voor te bereiden voor het model en het apparaat.

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

* Een werkruimte van Azure Machine Learning-service. Als u wilt maken van een gebruikt u de stappen in de [aan de slag met Azure Machine Learning-service](quickstart-get-started.md) document.

* Een ontwikkelomgeving. Zie voor meer informatie de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.

* Een [Azure IoT Hub](../../iot-hub/iot-hub-create-through-portal.md) in uw Azure-abonnement. 

* Een getraind model. Zie voor een voorbeeld van hoe u een model te trainen, de [een model van de installatiekopie classificatie met Azure Machine Learning te trainen](tutorial-train-models-with-aml.md) document. Er is een vooraf getrainde model beschikbaar op de [AI-werkset voor Azure IoT Edge-GitHub-opslagplaats](https://github.com/Azure/ai-toolkit-iot-edge/tree/master/IoT%20Edge%20anomaly%20detection%20tutorial).

## <a name="prepare-the-iot-device"></a>De IoT-apparaat voorbereiden

Voor meer informatie over het registreren van uw apparaat en het installeren van de IoT-runtime, volg de stappen in de [Snelstartgids: uw eerste IoT Edge-module implementeren op een apparaat met Linux x64](../../iot-edge/quickstart-linux.md) document.

## <a name="register-the-model"></a>Registreer het model

Azure IoT Edge-modules zijn gebaseerd op containerinstallatiekopieën. Als u wilt uw model implementeert naar een IoT Edge-apparaat, gebruik de volgende stappen voor het registreren van uw model in de werkruimte van een Azure Machine Learning-service en een Docker-installatiekopie te maken. 

> [!IMPORTANT]
> Als u Azure Machine Learning gebruikt met het trainen van uw model die mogelijk al zijn geregistreerd in uw werkruimte, slaat u stap 3 in dit geval.

1. Initialiseren van de werkruimte en laad de config.json-bestand:

    ```python
    from azureml.core  import Workspace

    #Load existing workspace from the config file info.
    ws  = Workspace.from_config()
    ```    

1. Registreer het model in uw werkruimte. Vervang de standaardtekst door uw model pad, de naam, de labels en de beschrijving:

    ```python
    from azureml.core.model import Model
    model = Model.register(model_path = "model.pkl", # this path points to the local file
                        model_name = "best_model", # the model gets registered as this name
                        tags = {'attribute': "myattribute", 'classification': "myclassification"},
                        description = "My awesome model",
                        workspace = ws)
    ```    

1. Het geregistreerde model ophalen: 

    ```python
    from azureml.core.model import Model

    model_name = "best_model"
    model = Model(ws, model_name)                     
    ```    

## <a name="create-a-docker-image"></a>Maak een Docker-installatiekopie.

1. Maak een **scoring-script** met de naam `score.py`. Dit bestand wordt gebruikt om uit te voeren van het model in de afbeelding. Het omvat de volgende functies:

    * De `init()` functie, die doorgaans in een globale objecttoegang het model laadt. Deze functie wordt uitgevoerd slechts eenmaal wanneer de Docker-container wordt gestart. 

    * De `run(input_data)` -functie maakt gebruik van het model om te voorspellen van een waarde op basis van de ingevoerde gegevens. Invoer en uitvoer aan het run doorgaans gebruikt u JSON-serialisatie en het deserialiseren, maar andere indelingen worden ondersteund.

    Zie voor een voorbeeld: de [installatiekopie classificatie zelfstudie](tutorial-deploy-models-with-aml.md#make-script).

1. Maak een **omgevingsbestand** met de naam `myenv.yml`. Dit bestand is een specificatie Conda-omgeving en een lijst met alle afhankelijkheden die nodig zijn voor het script en het model. Zie voor een voorbeeld: de [installatiekopie classificatie zelfstudie](tutorial-deploy-models-with-aml.md#make-myenv).

1. Configureren van de Docker-installatiekopie met de `score.py` en `myenv.yml` bestanden:
    
    ```python
    from azureml.core.image import Image, ContainerImage
    
    #Image configuration
    image_config = ContainerImage.image_configuration( runtime = "python", 
                           execution_script = "score.py",
                           conda_file = "myenv.yml", 
                           tags = {"attributes", "calssification"},
                           description = "Image that contains my model",
                           
                        )
    ```    

1. De installatiekopie met behulp van de configuratie van het model en de installatiekopie maken:

    ```python
    image = ContainerImage.create (name = "myimage", 
                           models = [model], #this is the model object
                           image_config = image_config,
                           workspace = ws
                        )
    ```     

    Het maken van de installatiekopie duurt ongeveer vijf minuten.

## <a name="get-the-container-registry-credentials"></a>De container registerreferenties ophalen

Azure IoT moet de referenties voor de container registry om docker-installatiekopieën in Azure Machine Learning-service worden opgeslagen. Gebruik de volgende stappen uit om op te halen van de referenties:

1. Meld u aan bij [Azure Portal](https://portal.azure.com/signin/index).

1. Ga naar de werkruimte van uw Azure Machine Learning-service en selecteer __overzicht__. Om te gaan naar de container registry-instellingen, selecteer de __register__ koppeling.

    ![Een afbeelding van de container-register-item](./media/how-to-deploy-to-iot/findregisteredcontainer.png)

1. Selecteer één keer in het containerregister **toegangssleutels** en schakel vervolgens de gebruiker met beheerdersrechten.

    ![Een installatiekopie van het scherm van de sleutels toegang](./media/how-to-deploy-to-iot/findaccesskey.png)

1. Sla de waarden voor de aanmeldingsserver, gebruikersnaam en wachtwoord. 

   Deze referenties zijn nodig voor de IoT edge-Apparaattoegang tot afbeeldingen in uw persoonlijke containerregister.

## <a name="next-steps"></a>Volgende stappen

U hebt voorbereidingen voor implementatie. Gebruik nu de stappen in de [implementeren Azure IoT Edge-modules van de Azure-portal](../../iot-edge/how-to-deploy-modules-portal.md) document om te implementeren op uw edge-apparaat. Bij het configureren van de __registerinstellingen__ voor het apparaat, gebruikt u de referenties die u eerder hebt geconfigureerd.
