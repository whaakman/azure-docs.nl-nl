---
title: Handleiding voor het oplossen van problemen met de implementatie
titleSuffix: Azure Machine Learning service
description: Leer hoe u omzeilen, oplossen en veelvoorkomende problemen oplossen de Docker-implementatie met AKS en ACI met behulp van Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
author: chris-lauren
ms.author: clauren
ms.reviewer: jmartens
ms.date: 05/02/2018
ms.custom: seodec18
ms.openlocfilehash: 0fba7c2f5a46e0c5d0e3c5fdd65a03bb77f148d9
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67074997"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Problemen met Azure Machine Learning-service AKS en ACI-implementaties oplossen

Leer hoe u omzeilen of oplossen van veelvoorkomende fouten van Docker-implementatie met Azure Container Instances (ACI) en Azure Kubernetes Service (AKS) met behulp van Azure Machine Learning-service.

Bij het implementeren van een model in Azure Machine Learning-service, wordt in het systeem een aantal taken uitvoert. De implementatietaken zijn:

1. Registreer het model in de werkruimte model-register.

2. Bouw een Docker-installatiekopie, met inbegrip van:
    1. Download het geregistreerde model uit het register. 
    2. Maak een docker-bestand, met een Python-omgeving op basis van de afhankelijkheden die u in de omgeving yaml-bestand opgeeft.
    3. Uw modelbestanden en het scoring-script dat u opgeeft in de dockerfile toevoegen.
    4. Bouw een nieuwe Docker-installatiekopie met behulp van het bestand dockerfile.
    5. Registreer de Docker-installatiekopie met de Azure Container Registry die zijn gekoppeld aan de werkruimte.

    > [!IMPORTANT]
    > Afhankelijk van uw code, het maken van installatiekopieën vinden automatisch plaats zonder uw invoer.

3. De Docker-installatiekopie implementeren naar Azure Container exemplaar (ACI)-service of naar Azure Kubernetes Service (AKS).

4. Starten van een nieuwe container (of containers) in ACI of AKS. 

Meer informatie over dit proces in de [Modelbeheer](concept-model-management-and-deployment.md) inleiding.

## <a name="before-you-begin"></a>Voordat u begint

Als u een probleem ondervindt, het eerste wat te doen is de Implementatietaak opdelen (vorige beschreven) in afzonderlijke stappen het probleem te isoleren.

Het verbreken van de implementatie in taken is handig als u de [Webservice.deploy()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-workspace--name--model-paths--image-config--deployment-config-none--deployment-target-none-) -API of [Webservice.deploy_from_model()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#deploy-from-model-workspace--name--models--image-config--deployment-config-none--deployment-target-none-) API, als beide functies de bovengenoemde stappen uitvoeren als een één actie. Normaal gesproken deze API's zijn handig, maar u kunt het opsplitsen van de stappen bij het oplossen van vervangen met de onderstaande API-aanroepen.

1. Registreer het model. Hier volgt een voorbeeld van code:

    ```python
    # register a model out of a run record
    model = best_run.register_model(model_name='my_best_model', model_path='outputs/my_model.pkl')

    # or, you can register a file or a folder of files as a model
    model = Model.register(model_path='my_model.pkl', model_name='my_best_model', workspace=ws)
    ```

2. Bouw de installatiekopie. Hier volgt een voorbeeld van code:

    ```python
    # configure the image
    image_config = ContainerImage.image_configuration(runtime="python",
                                                      entry_script="score.py",
                                                      conda_file="myenv.yml")

    # create the image
    image = Image.create(name='myimg', models=[model], image_config=image_config, workspace=ws)

    # wait for image creation to finish
    image.wait_for_creation(show_output=True)
    ```

3. De installatiekopie implementeert als service. Hier volgt een voorbeeld van code:

    ```python
    # configure an ACI-based deployment
    aci_config = AciWebservice.deploy_configuration(cpu_cores=1, memory_gb=1)

    aci_service = Webservice.deploy_from_image(deployment_config=aci_config, 
                                               image=image, 
                                               name='mysvc', 
                                               workspace=ws)
    aci_service.wait_for_deployment(show_output=True)    
    ```

Nadat u het implementatieproces in afzonderlijke taken hebt gesplitst, kunnen we kijken naar enkele van de meest voorkomende fouten.

## <a name="image-building-fails"></a>De installatiekopie van het bouwen van mislukt

Als de Docker-installatiekopie kan niet worden gemaakt, de [image.wait_for_creation()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.image.image(class)?view=azure-ml-py#wait-for-creation-show-output-false-) of [service.wait_for_deployment()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py#wait-for-deployment-show-output-false-) aanroep is mislukt met bepaalde foutberichten die enkele of er aanwijzingen kunnen bieden. U vindt ook meer informatie over de fouten van het logboek van de build installatiekopie. Hieronder wordt een voorbeeld van code waarin wordt beschreven hoe u voor het detecteren van het logboek-uri van de installatiekopie-build.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images.items():
    print (img.name, img.version, img.image_build_log_uri)
```

De uri van de installatiekopie-logboek is een SAS-URL die verwijst naar een logboekbestand die zijn opgeslagen in de Azure blob-opslag. Alleen kopiëren en plakken de uri in een browservenster en u kunnen downloaden en weergeven van het logboekbestand.

### <a name="azure-key-vault-access-policy-and-azure-resource-manager-templates"></a>Azure Key Vault-toegangsbeleid en Azure Resource Manager-sjablonen

De build van de installatiekopie kan ook mislukken vanwege een probleem met het toegangsbeleid voor Azure Key Vault. Deze situatie kan zich voordoen wanneer u een Azure Resource Manager-sjabloon te maken van de werkruimte en de bijbehorende resources (met inbegrip van Azure Key Vault), meerdere keren gebruikt. Bijvoorbeeld, met behulp van de sjabloon meerdere keren met dezelfde parameters als onderdeel van een continue integratie en implementatiepijplijn.

De meeste bewerkingen van resources maken via sjablonen idempotent zijn, maar Key Vault wist het toegangsbeleid telkens wanneer die de sjabloon wordt gebruikt. Wissen van het beleid einden toegang naar de Key Vault voor een bestaande werkruimte die wordt gebruikt. Deze voorwaarde resulteert in fouten wanneer u probeert te maken van nieuwe afbeeldingen. Hier volgen enkele voorbeelden van de fouten die u kunt ontvangen:

__Portal__:
```text
Create image "myimage": An internal server error occurred. Please try again. If the problem persists, contact support.
```

__SDK__:
```python
image = ContainerImage.create(name = "myimage", models = [model], image_config = image_config, workspace = ws)
Creating image
Traceback (most recent call last):
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 341, in create
    resp.raise_for_status()
  File "C:\Python37\lib\site-packages\requests\models.py", line 940, in raise_for_status
    raise HTTPError(http_error_msg, response=self)
requests.exceptions.HTTPError: 500 Server Error: Internal Server Error for url: https://eastus.modelmanagement.azureml.net/api/subscriptions/<subscription-id>/resourceGroups/<resource-group>/providers/Microsoft.MachineLearningServices/workspaces/<workspace-name>/images?api-version=2018-11-19

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "C:\Python37\lib\site-packages\azureml\core\image\image.py", line 346, in create
    'Content: {}'.format(resp.status_code, resp.headers, resp.content))
azureml.exceptions._azureml_exception.WebserviceException: Received bad response from Model Management Service:
Response Code: 500
Headers: {'Date': 'Tue, 26 Feb 2019 17:47:53 GMT', 'Content-Type': 'application/json', 'Transfer-Encoding': 'chunked', 'Connection': 'keep-alive', 'api-supported-versions': '2018-03-01-preview, 2018-11-19', 'x-ms-client-request-id': '3cdcf791f1214b9cbac93076ebfb5167', 'x-ms-client-session-id': '', 'Strict-Transport-Security': 'max-age=15724800; includeSubDomains; preload'}
Content: b'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}'
```

__CLI__:
```text
ERROR: {'Azure-cli-ml Version': None, 'Error': WebserviceException('Received bad response from Model Management Service:\nResponse Code: 500\nHeaders: {\'Date\': \'Tue, 26 Feb 2019 17:34:05
GMT\', \'Content-Type\': \'application/json\', \'Transfer-Encoding\': \'chunked\', \'Connection\': \'keep-alive\', \'api-supported-versions\': \'2018-03-01-preview, 2018-11-19\', \'x-ms-client-request-id\':
\'bc89430916164412abe3d82acb1d1109\', \'x-ms-client-session-id\': \'\', \'Strict-Transport-Security\': \'max-age=15724800; includeSubDomains; preload\'}\nContent:
b\'{"code":"InternalServerError","statusCode":500,"message":"An internal server error occurred. Please try again. If the problem persists, contact support"}\'',)}
```

Als u wilt voorkomen dat dit probleem, raden we aan een van de volgende methoden:

* Implementeer niet de sjabloon meerdere keren voor dezelfde parameters. Of verwijder de bestaande resources voordat u ze opnieuw maken met de sjabloon.
* Het toegangsbeleid van Key Vault onderzoekt en vervolgens deze beleidsregels gebruiken om in te stellen de `accessPolicies` eigenschap van de sjabloon.
* Controleren of de Key Vault-resource al bestaat. Als dit het geval is, niet opnieuw maken door de sjabloon. Bijvoorbeeld een parameter waarmee u het maken van de Key Vault-resource uitschakelen als deze al bestaat kunt toevoegen.

## <a name="debug-locally"></a>Lokaal fouten opsporen

Als u problemen hebt met het implementeren van een model ACI of AKS, probeert u deze als een lokale webservice implementeert. Met behulp van een lokale webservice gemakkelijker problemen op te lossen. De Docker-installatiekopie met het model is gedownload en gestart op uw lokale systeem.

> [!IMPORTANT]
> Lokale webservice-implementaties vereisen een werkende installatie van Docker op uw lokale systeem. Docker moet worden uitgevoerd voordat u een lokale webservice implementeert. Zie voor meer informatie over het installeren en met behulp van Docker [ https://www.docker.com/ ](https://www.docker.com/).

> [!WARNING]
> Lokale webservice-implementaties worden niet ondersteund voor productiescenario's.

Voor het implementeren van lokaal, uw code te gebruiken wijzigen `LocalWebservice.deploy_configuration()` om de implementatieconfiguratie van een te maken. Gebruik vervolgens `Model.deploy()` om de service te implementeren. Het volgende voorbeeld wordt een model geïmplementeerd (die deel uitmaken van de `model` variabele) als een lokale webservice:

```python
from azureml.core.model import InferenceConfig,Model
from azureml.core.webservice import LocalWebservice

# Create inference configuration. This creates a docker image that contains the model.
inference_config = InferenceConfig(runtime= "python", 
                                   entry_script="score.py",
                                   conda_file="myenv.yml")

# Create a local deployment, using port 8890 for the web service endpoint
deployment_config = LocalWebservice.deploy_configuration(port=8890)
# Deploy the service
service = Model.deploy(ws, "mymodel", [model], inference_config, deployment_config)
# Wait for the deployment to complete
service.wait_for_deployment(True)
# Display the port that the web service is available on
print(service.port)
```

Op dit moment kunt u werken met de service als normaal. De volgende code toont bijvoorbeeld het verzenden van gegevens naar de service:

```python
import json

test_sample = json.dumps({'data': [
    [1,2,3,4,5,6,7,8,9,10], 
    [10,9,8,7,6,5,4,3,2,1]
]})

test_sample = bytes(test_sample,encoding = 'utf8')

prediction = service.run(input_data=test_sample)
print(prediction)
```

### <a name="update-the-service"></a>De service bijwerken

Tijdens het lokale testen, moet u mogelijk bijwerken de `score.py` bestand toe te voegen logboekregistratie of probeert op te lossen problemen die u hebt ontdekt. Laden van wijzigingen in de `score.py` bestand, gebruik `reload()`. Bijvoorbeeld de volgende code wordt het script voor de service opnieuw geladen en verzendt die gegevens vervolgens naar deze. De gegevens wordt berekend met behulp van de bijgewerkte `score.py` bestand:

```python
service.reload()
print(service.run(input_data=test_sample))
```

> [!NOTE]
> Het script opnieuw is geladen vanuit de locatie die is opgegeven door de `InferenceConfig` object dat wordt gebruikt door de service.

Het model, de Conda-afhankelijkheden of de configuratie van de implementatie wilt wijzigen, gebruikt u [update()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#update--args-). Het model dat wordt gebruikt door de service-updates in het volgende voorbeeld:

```python
service.update([different_model], inference_config, deployment_config)
```

### <a name="delete-the-service"></a>De service verwijderen

Als u wilt verwijderen van de service, gebruikt u [delete()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice%28class%29?view=azure-ml-py#delete--).

### <a id="dockerlog"></a> Inspecteer de Docker-logboek

U kunt gedetailleerde berichten voor Docker-engine van het serviceobject afdrukken. U kunt het logboek weergeven voor ACI en AKS lokale implementaties. Het volgende voorbeeld ziet u hoe u de logboeken afdrukken.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices['mysvc'].get_logs())
```

## <a name="service-launch-fails"></a>Service starten mislukt

Nadat de installatiekopie wordt is gemaakt, wordt het systeem probeert te starten van een container met behulp van de implementatieconfiguratie van uw. Als onderdeel van de container starten van proces, de `init()` functie in uw scoring-script wordt aangeroepen door het systeem. Als er niet-onderschepte uitzonderingen in de `init()` functioneren, ziet u mogelijk **CrashLoopBackOff** fout in het foutbericht.

Gebruik de informatie in de [inspecteren van het Docker-logboek](#dockerlog) sectie om te controleren of de logboeken.

## <a name="function-fails-getmodelpath"></a>Functie mislukt: get_model_path()

Vaak, in de `init()` functie in het scoring-script, [Model.get_model_path()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.model?view=azure-ml-py#get-model-path-model-name--version-none---workspace-none-) functie aangeroepen om een modelbestand of map van de modelbestanden niet vinden in de container. Als het modelbestand of map kan niet worden gevonden, wordt de functie mislukt. De eenvoudigste manier om op te sporen van deze fout is om uit te voeren de volgende Python-code in de Container-shell:

```python
import logging
logging.basicConfig(level=logging.DEBUG)
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

In dit voorbeeld wordt het lokale pad (relatief aan `/var/azureml-app`) in de container waar de scoring-script verwacht de modelbestand of map zoeken. Vervolgens kunt u controleren of het bestand of map inderdaad waarop is naar verwachting.

Instellen van het niveau van logboekregistratie voor FOUTOPSPORING, kan dit ertoe leiden dat de aanvullende gegevens moeten worden vastgelegd, die mogelijk nuttig bij het identificeren van de fout.

## <a name="function-fails-runinputdata"></a>Functie mislukt: run(input_data)

Als de service is geïmplementeerd, maar deze loopt vast bij het plaatsen van gegevens naar het scoring-eindpunt, kunt u fout-instructie in afvangen toevoegen uw `run(input_data)` functie zodat het gedetailleerde foutbericht wordt weergegeven in plaats daarvan retourneert. Bijvoorbeeld:

```python
def run(input_data):
    try:
        data = json.loads(input_data)['data']
        data = np.array(data)
        result = model.predict(data)
        return json.dumps({"result": result.tolist()})
    except Exception as e:
        result = str(e)
        # return error message back to the client
        return json.dumps({"error": result})
```

**Opmerking**: Retourneren van foutberichten van de `run(input_data)` aanroep moet worden gedaan voor het opsporen van fouten in doel alleen. Uit veiligheidsoverwegingen moet u niet retourneren foutberichten op deze manier in een productieomgeving.

## <a name="http-status-code-503"></a>HTTP-statuscode 503

Azure Kubernetes Service-implementaties ondersteuning voor automatisch schalen, waardoor de replica's moeten worden toegevoegd ter ondersteuning van extra belasting. Het automatisch schalen is echter ontworpen voor het afhandelen van **geleidelijke** wijzigingen in de belasting. Als u grote pieken in aanvragen per seconde ontvangen, kunnen clients een HTTP-statuscode 503 ontvangen.

Er zijn twee dingen die kunnen helpen voorkomen dat 503 statuscodes:

* Wijziging in het gebruiksniveau op welke automatisch schalen nieuwe replica's maakt.
    
    Standaard doelverbruik voor automatisch schalen is ingesteld op 70%, wat betekent dat de service kan omgaan met pieken in het aantal aanvragen per seconde (RPS) tot 30%. U kunt de streefgebruikspercentage aanpassen door in te stellen de `autoscale_target_utilization` op een lagere waarde.

    > [!IMPORTANT]
    > Deze wijziging leidt niet tot de replica's moet worden gemaakt *sneller*. In plaats daarvan worden gemaakt met een lagere drempelwaarde voor gebruik. In plaats van te wachten totdat de service 70 is % gebruikt, wordt de waarde wijzigen aan 30% replica's moet worden gemaakt wanneer er gebruik van 30% optreedt.
    
    Als de web-service maakt al gebruik van de huidige maximale replica's en er nog steeds 503 statuscodes, verhoogt u de `autoscale_max_replicas` waarde naar het maximum aantal replica's vergroten.

* Wijzig het minimum aantal replica's. De minimale replica's vergroten biedt een grotere groep voor het afhandelen van de binnenkomende pieken.

    Instellen om te vergroten het minimum aantal replica's, `autoscale_min_replicas` op een hogere waarde. U kunt de vereiste replica's berekenen met behulp van de volgende code, waarden vervangen door waarden die specifiek zijn voor uw project:

    ```python
    from math import ceil
    # target requests per second
    targetRps = 20
    # time to process the request (in seconds)
    reqTime = 10
    # Maximum requests per container
    maxReqPerContainer = 1
    # target_utilization. 70% in this example
    targetUtilization = .7

    concurrentRequests = targetRps * reqTime / targetUtilization

    # Number of container replicas
    replicas = ceil(concurrentRequests / maxReqPerContainer)
    ```

    > [!NOTE]
    > Als u aanvraag pieken groter is ontvangt dan de nieuwe minimale replica's kunnen worden verwerkt, ontvangt u mogelijk 503s opnieuw. Als het verkeer naar uw service toeneemt moet u mogelijk bijvoorbeeld de minimale replica's vergroten.

Voor meer informatie over het instellen `autoscale_target_utilization`, `autoscale_max_replicas`, en `autoscale_min_replicas` , Zie de [AksWebservice](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.akswebservice?view=azure-ml-py) -Moduleverwijzing voorkomen.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over implementatie:

* [Over het implementeren en waar](how-to-deploy-and-where.md)
* [Zelfstudie: Trainen en implementeren van modellen](tutorial-train-models-with-aml.md)
