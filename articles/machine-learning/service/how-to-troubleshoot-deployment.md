---
title: Handleiding voor het oplossen van problemen met de implementatie
titleSuffix: Azure Machine Learning service
description: Informatie over hoe tijdelijke oplossing, is opgelost en veelvoorkomende problemen oplossen de Docker-implementatie met AKS en ACI met behulp van Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: haining
author: hning86
ms.reviewer: jmartens
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 6bd3bc86aa828ab28462de9d45f660889634cbd7
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53100511"
---
# <a name="troubleshooting-azure-machine-learning-service-aks-and-aci-deployments"></a>Problemen met Azure Machine Learning-service AKS en ACI-implementaties oplossen

In dit artikel leert u hoe u kunt omzeilen of oplossen van de veelvoorkomende implementatiefouten Docker met Azure Container Instances (ACI) en Azure Kubernetes Service (AKS) met behulp van Azure Machine Learning-service.

Bij het implementeren van een model in Azure Machine Learning-service, wordt in het systeem een aantal taken uitvoert. Dit is een complexe reeks gebeurtenissen en soms zich problemen voordoen. De implementatietaken zijn:

1. Registreer het model in de werkruimte model-register.

2. Bouw een Docker-installatiekopie, met inbegrip van:
    1. Download het geregistreerde model uit het register. 
    2. Maak een docker-bestand, met een Python-omgeving op basis van de afhankelijkheden die u in de omgeving yaml-bestand opgeeft.
    3. Uw modelbestanden en het scoring-script dat u opgeeft in de dockerfile toevoegen.
    4. Bouw een nieuwe Docker-installatiekopie met behulp van het bestand dockerfile.
    5. Registreer de Docker-installatiekopie met de Azure Container Registry die zijn gekoppeld aan de werkruimte.

3. De Docker-installatiekopie implementeren naar Azure Container exemplaar (ACI)-service of naar Azure Kubernetes Service (AKS).

4. Starten van een nieuwe container (of containers) in ACI of AKS. 

Meer informatie over dit proces in de [Modelbeheer](concept-model-management-and-deployment.md) inleiding.

## <a name="before-you-begin"></a>Voordat u begint

Als u een probleem ondervindt, het eerste wat te doen is de Implementatietaak opdelen (vorige beschreven) in afzonderlijke stappen het probleem te isoleren. 

Dit is vooral handig als u de `Webservice.deploy` -API of `Webservice.deploy_from_model` API, omdat deze functies de bovengenoemde stappen in één actie groeperen. Deze API's zijn meestal heel gemakkelijk, maar u kunt het opsplitsen van de stappen bij het oplossen van vervangen met de onderstaande API-aanroepen.

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
                                                      execution_script="score.py",
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
Als systeem kan niet aan het bouwen van de Docker-installatiekopie, is de `image.wait_for_creation()` aanroep is mislukt met bepaalde foutberichten die enkele of er aanwijzingen kunnen bieden. U vindt ook meer informatie over de fouten van het logboek van de build installatiekopie. Hieronder wordt een voorbeeld van code waarin wordt beschreven hoe u voor het detecteren van het logboek-uri van de installatiekopie-build.

```python
# if you already have the image object handy
print(image.image_build_log_uri)

# if you only know the name of the image (note there might be multiple images with the same name but different version number)
print(ws.images()['myimg'].image_build_log_uri)

# list logs for all images in the workspace
for name, img in ws.images().items():
    print (img.name, img.version, img.image_build_log_uri)
```
De uri van de installatiekopie-logboek is een SAS-URL die verwijst naar een logboekbestand die zijn opgeslagen in de Azure blob-opslag. Alleen kopiëren en plakken de uri in een browservenster en u kunnen downloaden en weergeven van het logboekbestand.


## <a name="service-launch-fails"></a>Service starten mislukt
Nadat de installatiekopie wordt is gemaakt, wordt het systeem probeert te starten van een container in ACI of AKS, afhankelijk van de configuratie van uw implementatie. Het is in het algemeen aanbevolen om te proberen een ACI-implementatie eerst, omdat het is een eenvoudigere implementatie van één container. Op deze manier u kunt vervolgens sprake is van een AKS-specifieke probleem.

Als onderdeel van de container starten van proces, de `init()` functie in uw scoring-script wordt aangeroepen door het systeem. Als er niet-onderschepte uitzonderingen in de `init()` functioneren, ziet u mogelijk **CrashLoopBackOff** fout in het foutbericht. Hieronder volgen enkele tips voor hulp bij het oplossen van het probleem.

### <a name="inspect-the-docker-log"></a>Inspecteer de Docker-logboek
U kunt gedetailleerde berichten voor Docker-engine van het serviceobject afdrukken.

```python
# if you already have the service object handy
print(service.get_logs())

# if you only know the name of the service (note there might be multiple services with the same name but different version number)
print(ws.webservices()['mysvc'].get_logs())
```

### <a name="debug-the-docker-image-locally"></a>Fouten opsporen in lokaal de Docker-installatiekopie
Enkele voorbeelden van momenten het Docker-logboek is niet voldoende informatie over wat er mis verzenden. U kunt Ga een stap verder en haal de ingebouwde Docker-installatiekopie, een lokale container starten en fouten opsporen in rechtstreeks worden uitgevoerd in de live-container interactief. Voor het starten van een lokale container, moet u een Docker-engine lokaal worden uitgevoerd en het is veel eenvoudiger als u ook hebt [azure-cli](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) geïnstalleerd.

Eerst moeten we weten van de installatiekopie-locatie:

```python
# print image location
print(image.image_location)
```

De installatiekopie-locatie is deze indeling: `<acr-name>.azurecr.io/<image-name>:<version-number>`, zoals `myworkpaceacr.azurecr.io/myimage:3`. 

Nu gaat u naar uw opdrachtregelvenster. Als u azure-cli is geïnstalleerd hebt, kunt u de volgende opdrachten voor het aanmelden bij de ACR (Azure Container Registry) die zijn gekoppeld aan de werkruimte waar de installatiekopie is opgeslagen typen. 

```sh
# log on to Azure first if you haven't done so before
$ az login

# make sure you set the right subscription in case you have access to multiple subscriptions
$ az account set -s <subscription_name_or_id>

# now let's log in to the workspace ACR
# note the acr-name is the domain name WITHOUT the ".azurecr.io" postfix
# e.g.: az acr login -n myworkpaceacr
$ az acr login -n <acr-name>
```
Als u geen azure-cli is geïnstalleerd, kunt u `docker login` opdracht uit om aan te melden bij de ACR. Maar u moet eerst de gebruikersnaam en het wachtwoord van de ACR van Azure-portal ophalen.

Zodra u zich hebt aangemeld bij de ACR, u kunt omlaag de Docker-installatiekopie en een container lokaal starten en start vervolgens een bash-sessie voor foutopsporing met behulp van de `docker run` opdracht:

```sh
# note the image_id is <acr-name>.azurecr.io/<image-name>:<version-number>
# for example: myworkpaceacr.azurecr.io/myimage:3
$ docker run -it <image_id> /bin/bash
```

Als u een bash-sessie de container die wordt uitgevoerd start, vindt u de scoring-scripts in de `/var/azureml-app` map. Vervolgens kunt u een Python-sessie voor het fouten opsporen in uw scoring scripts starten. 

```sh
# enter the directory where scoring scripts live
cd /var/azureml-app

# find what Python packages are installed in the python environment
pip freeze

# sanity-check on score.py
# you might want to edit the score.py to trigger init().
# as most of the errors happen in init() when you are trying to load the model.
python score.py
```
Als u een teksteditor voor het wijzigen van uw scripts nodig hebt, kunt u vim, nano, Emacs of uw favoriete editor installeren.

```sh
# update package index
apt-get update

# install a text editor of your choice
apt-get install vim
apt-get install nano
apt-get install emacs

# launch emacs (for example) to edit score.py
emacs score.py

# exit the container bash shell
exit
```

U kunt ook de webservice lokaal worden opgestart en HTTP-verkeer te verzenden. De Flask-server in de Docker-container wordt uitgevoerd op poort 5001. U kunt toewijzen aan een andere poorten die beschikbaar zijn op de hostcomputer.
```sh
# you can find the scoring API at: http://localhost:8000/score
$ docker run -p 8000:5001 <image_id>
```

## <a name="function-fails-getmodelpath"></a>Functie mislukt: get_model_path()
Vaak, in de `init()` functie in het scoring-script `Model.get_model_path()` functie aangeroepen om een modelbestand of map van de modelbestanden niet vinden in de container. Dit is vaak een bron van de fout als de modelbestand of map niet kan worden gevonden. De eenvoudigste manier om op te sporen van deze fout is om uit te voeren de volgende Python-code in de Container-shell:

```python
from azureml.core.model import Model
print(Model.get_model_path(model_name='my-best-model'))
```

Dit zou afdrukken van het lokale pad (relatief aan `/var/azureml-app`) in de container waar de scoring-script verwacht de modelbestand of map zoeken. Vervolgens kunt u controleren of het bestand of map inderdaad waarop is naar verwachting.


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
**Houd er rekening mee**: retourneren foutmeldingen vanuit de `run(input_data)` aanroep moet worden gedaan voor het opsporen van fouten in doel alleen. Een goed idee om dit te doen in een productieomgeving om beveiligingsredenen niet mogelijk.


## <a name="next-steps"></a>Volgende stappen

Meer informatie over implementatie: 
* [Over het implementeren en waar](how-to-deploy-and-where.md)

* [Zelfstudie: Trainen en implementeren van modellen](tutorial-train-models-with-aml.md)
