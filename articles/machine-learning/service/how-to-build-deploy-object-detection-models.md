---
title: Bouw en implementeer een objectmodel detectie met behulp van Azure Machine Learning-pakket voor de visie van de Computer.
description: Informatie over het bouwen, trainen, testen en implementeren van een computer vision-detectie objectmodel met het Azure Machine Learning-pakket voor Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 06/01/2018
ms.openlocfilehash: 62cc37d8c462d0fc1831de7b50a85738d6e63a17
ms.sourcegitcommit: 59fffec8043c3da2fcf31ca5036a55bbd62e519c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/04/2018
ms.locfileid: "34727771"
---
# <a name="build-and-deploy-object-detection-models-with-azure-machine-learning"></a>Bouw en implementeer object detectie modellen met Azure Machine Learning

Informatie over het gebruik in dit artikel **Azure Machine Learning-pakket voor Computer Vision** om te trainen, testen en implementeren van een [sneller R CNN](https://arxiv.org/abs/1506.01497) objectmodel voor detectie. 

Een groot aantal problemen in het domein van de visie computer kan worden opgelost met object-detectie. Deze problemen zijn voor het bouwen van modellen die een variabele aantal objecten niet op een installatiekopie vinden. 

Bij het maken en implementeren van dit model met dit pakket, gaat u de volgende stappen:
1.  Het maken van de gegevensset
2.  Modeldefinitie diep Neurale netwerk (DNN)
3.  Model trainen
4.  Evaluatie en visualisatie
5.  Implementatie-webservice
6.  Testen van de Load-webservice

In dit voorbeeld TensorFlow wordt gebruikt als het framework grondige learning, training wordt lokaal uitgevoerd op een machine GPU ingeschakeld, zoals de [grondige learning gegevens wetenschappelijke VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview), en implementatie gebruikmaakt van de Azure ML uitoefening CLI.

Raadpleeg de [pakket naslagdocumentatie](https://aka.ms/aml-packages/vision) voor de gedetailleerde naslaginformatie voor elke module en de klasse.

## <a name="prerequisites"></a>Vereisten

1. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

1. De volgende accounts en de toepassing moeten worden ingesteld en geïnstalleerd:
   - Een Azure Machine Learning Experimenten-account 
   - Een Azure Machine Learning-Model Management-account
   - Azure Machine Learning Workbench moet zijn geïnstalleerd

   Als deze drie zijn nog niet gemaakt of geïnstalleerd, voert u de [Quick Start Azure Machine Learning en de Workbench](../service/quickstart-installation.md) artikel. 

1. Het Azure Machine Learning-pakket voor de visie van de Computer moet worden geïnstalleerd. Meer informatie over hoe [hier in dit pakket installeren](https://aka.ms/aml-packages/vision).

## <a name="sample-data-and-notebook"></a>Voorbeeldgegevens en notebook

### <a name="get-the-jupyter-notebook"></a>De Jupyter-notebook ophalen

Download de notebook om uit te voeren van het voorbeeld beschreven hier zelf.

> [!div class="nextstepaction"]
> [De Jupyter-notebook ophalen](https://aka.ms/aml-packages/vision/notebooks/object_detection)

### <a name="load-the-sample-data"></a>De voorbeeldgegevens laden

Voor deze demo is een gegevensset van boodschappen binnen koelkasten opgegeven, wordt die bestaan uit 30 installatiekopieën en 8 klassen (eggBox, joghurt ketchup, mushroom, mustard, oranje, squash en water). Er is een aantekening-xml-bestand met dezelfde naam voor elk jpg-afbeelding. 

De volgende afbeelding ziet de aanbevolen mapstructuur. 

![Mapstructuur](media/how-to-build-deploy-object-detection-models/data_directory.JPG)

## <a name="image-annotation"></a>Afbeelding aantekening

Object locaties hoeven te trainen en evalueren van de detectie van een object van aantekeningen voorzien. [LabelImg](https://tzutalin.github.io/labelImg) is een hulpprogramma voor open-source aantekening die aantekeningen toevoegen aan installatiekopieën kan worden gebruikt. LabelImg schrijft een xml-bestand per afbeelding in Pascal VOS-indeling kan worden gelezen door dit pakket. 

## <a name="storage-context"></a>Opslag-Context
De context van de opslag wordt gebruikt om te bepalen waar de verschillende uitvoerbestanden zoals DNN modelbestanden worden opgeslagen. Zie voor meer informatie de [StorageContext documentatie](https://docs.microsoft.com/en-us/python/api/cvtk.core.context.storagecontext?view=azure-ml-py-latest). De inhoud van de opslag hoeft normaal gesproken niet expliciet worden ingesteld. Om te voorkomen de Workbench project limiet van 25 MB, wordt echter de uitvoer-directory om te verwijzen naar een locatie buiten het project AML instellen ('... /.. /.. /.. / cvtk_output '). Zorg ervoor dat de map 'cvtk_output' verwijderen wanneer deze niet meer nodig is.


```python
import warnings
warnings.filterwarnings("ignore")
import os, time
from cvtk.core import Context, ObjectDetectionDataset, TFFasterRCNN
from cvtk.utils import detection_utils
from matplotlib import pyplot as plt

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

# Initialize the context object
out_root_path = "../../../cvtk_output"
Context.create(outputs_path=out_root_path, persistent_path=out_root_path, temp_path=out_root_path)

# Display the images
%matplotlib inline
```

## <a name="create-a-dataset"></a>Een gegevensset maken

Maak een CVTK gegevensset die uit een set installatiekopieën met hun respectieve omsluitende vak aantekeningen bestaat. In dit voorbeeld wordt de koelkast afbeeldingen die beschikbaar zijn in de '... de map/levensmiddelen-sample_data/training"worden gebruikt. Alleen JPEG-afbeeldingen worden ondersteund.


```python
image_folder = "../sample_data/foods/train"
data_train = ObjectDetectionDataset.create_from_dir(dataset_name='training_dataset', data_dir=image_folder,
                                                    annotations_dir="Annotations", image_subdirectory='JPEGImages')

# Show some statistics of the training image, and also give one example of the ground truth rectangle annotations
data_train.print_info()
_ = data_train.images[2].visualize_bounding_boxes(image_size = (10,10))
```

    F1 2018-05-25 23:12:21,727 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-05-25 23:12:21,733 INFO azureml.vision:dataset creating dataset for scenario=detection 
    Dataset name: training_dataset
    Total classes: 8, total images: 25
    Label-wise object counts:
        Label eggBox: 20 objects
        Label joghurt: 20 objects
        Label ketchup: 20 objects
        Label mushroom: 20 objects
        Label mustard: 20 objects
        Label orange: 20 objects
        Label squash: 40 objects
        Label water: 20 objects
    Bounding box width and height distribution:
        Bounding box widths  0/5/25/50/75/95/100-percentile: 54/61/79/117/133/165/311 pixels
        Bounding box heights 0/5/25/50/75/95/100-percentile: 48/58/75/124/142/170/212 pixels
    


![PNG](media/how-to-build-deploy-object-detection-models/output_6_1.JPG)


## <a name="define-a-model"></a>Een model definiëren

In dit voorbeeld wordt het model sneller R CNN gebruikt. Verschillende parameters kunnen worden opgegeven bij het definiëren van dit model. De betekenis van deze parameters, evenals de parameters gebruikt voor het trainen van (Zie volgende sectie) vindt u in beide CVTK API documenten of op de [Tensorflow object detectiewebsite](https://github.com/tensorflow/models/tree/master/research/object_detection). Meer informatie over sneller R CNN model vindt u op [deze koppeling](https://docs.microsoft.com/en-us/cognitive-toolkit/Object-Detection-using-Faster-R-CNN#technical-details). Dit model is gebaseerd op snelle R-CNN en meer informatie hierover vindt u [hier](https://docs.microsoft.com/en-us/cognitive-toolkit/Object-Detection-using-Fast-R-CNN#algorithm-details).


```python
score_threshold = 0.0       # Threshold on the detection score, use to discard lower-confidence detections.
max_total_detections = 300  # Maximum number of detections. A high value slows down training but might increase accuracy.
my_detector = TFFasterRCNN(labels=data_train.labels, 
                           score_threshold=score_threshold, 
                           max_total_detections=max_total_detections)
```

## <a name="train-the-model"></a>Het model trainen

Het model Îles Cocos getraind sneller R-CNN met ResNet50 wordt gebruikt als startpunt voor training. 

Het aantal stappen training in de code is ingesteld op 350 voor het trainen van de detectie, zodat de training sneller uitgevoerd (~ 5 minuten met GPU). In de praktijk, moet u deze op minstens 10 keer het aantal afbeeldingen in de trainingset instellen.

In dit voorbeeld is het aantal detectie training stappen ingesteld op 350 voor snelle training. In de praktijk kan is een goede vuistregel echter te stellen de stappen op 10 of meer keer het aantal afbeeldingen in de trainingset.

Er zijn twee belangrijke parameters voor training:
- Het aantal stappen voor het trainen van het model dat wordt vertegenwoordigd door het argument num_seps. Elke stap traint het model met een minibatch van batchgrootte een.
- Leren belastingtarie(f)(ven), die kan worden ingesteld door initial_learning_rate

```python
print("tensorboard --logdir={}".format(my_detector.train_dir))

# to get good results, use a larger value for num_steps, e.g., 5000.
num_steps = 350
learning_rate = 0.001 # learning rate

start_train = time.time()
my_detector.train(dataset=data_train, num_steps=num_steps, 
                  initial_learning_rate=learning_rate)
end_train = time.time()
print(end_train-start_train)
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\train
    F1 2018-05-25 23:12:22,764 INFO azureml.vision:Fit starting in experiment  1125722225 
    F1 2018-05-25 23:12:22,767 INFO azureml.vision:model starting trainging for scenario=detection 
    Using existing checkpoint file that's saved at 'C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\models\detection\faster_rcnn_resnet50_coco_2018_01_28\model.ckpt.index'.
    TFRecords creation started.
    F1 2018-05-25 23:12:22,773 INFO On image 0 of 25
    TFRecords creation completed.
    Training started.
    Training progressing: step 0 ...
    Training progressing: step 100 ...
    Training progressing: step 200 ...
    Training progressing: step 300 ...
    F1 2018-05-25 23:18:02,730 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:18:10,722 INFO 2953 ops in the final graph.
    F1 2018-05-25 23:18:24,244 INFO azureml.vision:Fit finished in experiment  1125722225 
    Training completed.
    361.604615688324
    

TensorBoard kan worden gebruikt om de voortgang van de training te visualiseren. TensorBoard gebeurtenissen bevinden zich in de map die is opgegeven door het modelobject train_dir kenmerk. Als u wilt weergeven TensorBoard, de volgende stappen uit:
1. Kopieer de afdruk die begint met 'tensorboard--logdir' aan een opdrachtregel en voer deze uit. 
2. Kopieer de URL van de geretourneerde vanaf de opdrachtregel naar een webbrowser om de TensorBoard weer te geven. 

De TensorBoard moet eruitzien als in de volgende schermafbeelding. Het duurt enkele minuten voor de map training worden ingevuld. Dus als TensorBoard voorkomt stappen up correct de eerste keer probeert herhalende 1-2.  

![tensorboard](media/how-to-build-deploy-object-detection-models/tensorboard.JPG)

## <a name="evaluate-the-model"></a>Het model evalueren

De methode 'evalueren' wordt gebruikt om het model beoordelen. Deze functie moet een object ObjectDetectionDataset als invoer. De gegevensset evaluatie kan worden gemaakt met behulp van dezelfde functie als het abonnement voor de gegevensset training gebruikt. De ondersteunde meetwaarde is gemiddelde precisie zoals is gedefinieerd voor de [PASCAL VOS uitdaging](http://host.robots.ox.ac.uk/pascal/VOC/pubs/everingham10.pdf).  


```python
image_folder = "../sample_data/foods/test"
data_val = ObjectDetectionDataset.create_from_dir(dataset_name='val_dataset', data_dir=image_folder)
eval_result = my_detector.evaluate(dataset=data_val)
```

    F1 2018-05-25 23:18:24,253 INFO azureml.vision:dataset creating dataset for scenario=detection 
    F1 2018-05-25 23:18:24,286 INFO On image 0 of 5
    F1 2018-05-25 23:18:29,300 INFO Starting evaluation at 2018-05-26-03:18:29
    F1 2018-05-25 23:18:32,403 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:33,158 INFO Detection visualizations written to summary with tag image-0.
    F1 2018-05-25 23:18:33,518 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:34,342 INFO Detection visualizations written to summary with tag image-1.
    F1 2018-05-25 23:18:34,714 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:35,470 INFO Detection visualizations written to summary with tag image-2.
    F1 2018-05-25 23:18:35,835 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:36,654 INFO Detection visualizations written to summary with tag image-3.
    F1 2018-05-25 23:18:37,010 INFO Creating detection visualizations.
    F1 2018-05-25 23:18:37,798 INFO Detection visualizations written to summary with tag image-4.
    F1 2018-05-25 23:18:37,804 INFO Running eval batches done.
    F1 2018-05-25 23:18:37,805 INFO # success: 5
    F1 2018-05-25 23:18:37,806 INFO # skipped: 0
    F1 2018-05-25 23:18:38,119 INFO Writing metrics to tf summary.
    F1 2018-05-25 23:18:38,121 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/eggBox: 1.000000
    F1 2018-05-25 23:18:38,205 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/joghurt: 0.942857
    F1 2018-05-25 23:18:38,206 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/ketchup: 1.000000
    F1 2018-05-25 23:18:38,207 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mushroom: 1.000000
    F1 2018-05-25 23:18:38,208 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/mustard: 1.000000
    F1 2018-05-25 23:18:38,209 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/orange: 1.000000
    F1 2018-05-25 23:18:38,210 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/squash: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/PerformanceByCategory/AP@0.5IOU/water: 1.000000
    F1 2018-05-25 23:18:38,211 INFO PASCAL/Precision/mAP@0.5IOU: 0.992857
    F1 2018-05-25 23:18:38,253 INFO Metrics written to tf summary.
    F1 2018-05-25 23:18:38,254 INFO Finished evaluation!
    

De evaluatieresultaten kunnen in een schone indeling worden afgedrukt.


```python
# print out the performance metric values
for label_obj in data_train.labels:
    label = label_obj.name
    key = 'PASCAL/PerformanceByCategory/AP@0.5IOU/' + label
    print('{0: <15}: {1: <3}'.format(label, round(eval_result[key], 2)))
print('{0: <15}: {1: <3}'.format("overall:", round(eval_result['PASCAL/Precision/mAP@0.5IOU'], 2))) 
```

    joghurt        : 0.94
    squash         : 1.0
    mushroom       : 1.0
    eggBox         : 1.0
    ketchup        : 1.0
    mustard        : 1.0
    water          : 1.0
    orange         : 1.0
    overall:       : 0.99
    

Op deze manier kunt u de nauwkeurigheid van het model op de trainingset berekenen. Dit zorgt ervoor dat training geconvergeerd naar een goede oplossing doen. De nauwkeurigheid van de trainingset na de geslaagde training is vaak dicht bij 100%.

Evaluatieresultaten kunnen ook worden weergegeven uit TensorBoard, met inbegrip van de waarden van de kaart en afbeeldingen met voorspelde omsluitende vakken. Kopieer de afdruk van de volgende code in een opdrachtregelvenster om de client TensorBoard te starten. Hier wordt een poortwaarde 8008 gebruikt om conflicten met de standaardwaarde van 6006, die werd gebruikt voor het weergeven van trainingsstatus te voorkomen.


```python
print("tensorboard --logdir={} --port=8008".format(my_detector.eval_dir))
```

    tensorboard --logdir=C:\Users\lixun\Desktop\AutoDL\CVTK\Src\API\cvtk_output\temp_faster_rcnn_resnet50\models\eval --port=8008
    

## <a name="score-an-image"></a>Een installatiekopie van een beoordelen

Wanneer u tevreden met de prestaties van het getrainde model bent, kan het modelobject 'score' functie kan worden gebruikt voor de beoordeling van nieuwe afbeeldingen. De geretourneerde scores kunnen worden weergegeven met de functie 'visualiseren'. 


```python
image_path = data_val.images[1].storage_path
detections_dict = my_detector.score(image_path)
path_save = out_root_path + "/scored_images/scored_image_preloaded.jpg"
ax = detection_utils.visualize(image_path, detections_dict, image_size=(8, 12))
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

![PNG](media/how-to-build-deploy-object-detection-models/output_20_0.JPG)

##  <a name="save-the-model"></a>Sla het model

Het getrainde model worden opgeslagen op schijf en weer in het geheugen geladen, zoals wordt weergegeven in de volgende codevoorbeelden.


```python
save_model_path = out_root_path + "/frozen_model/faster_rcnn.model" # Please save your model to outside of your AML workbench project folder because of the size limit of AML project
my_detector.save(save_model_path)
```

    F1 2018-05-25 23:18:55,166 INFO Graph Rewriter optimizations enabled
    Converted 275 variables to const ops.
    F1 2018-05-25 23:19:03,867 INFO 2953 ops in the final graph.
    

## <a name="load-the-saved-model-for-scoring"></a>Het opgeslagen model voor score berekenen niet laden

Voor het gebruik van de opgeslagen model, het model niet laden in het geheugen met de functie 'load'. U moet slechts één keer laden. 

```python
my_detector_loaded = TFFasterRCNN.load(save_model_path)
```

Nadat het model is geladen, kan deze worden gebruikt voor het verkrijgen van een afbeelding of een lijst met afbeeldingen. Voor een enkele afbeelding, een woordenlijst geretourneerd met sleutels zoals 'detection_boxes', 'detection_scores' en 'num_detections'. Als de invoer voor een lijst met afbeeldingen, is een lijst van woordenlijst die wordt geretourneerd, met één woordenlijst die overeenkomt met een installatiekopie. 


```python
detections_dict = my_detector_loaded.score(image_path)
```

De gedetecteerde objecten met scores hierboven 0,5, met inbegrip van labels, scores en coördinaten kan worden afgedrukt.


```python
look_up = dict((v,k) for k,v in my_detector.class_map.items())
n_obj = 0
for i in range(detections_dict['num_detections']):
    if detections_dict['detection_scores'][i] > 0.5:
        n_obj += 1
        print("Object {}: label={:11}, score={:.2f}, location=(top: {:.2f}, left: {:.2f}, bottom: {:.2f}, right: {:.2f})".format(
            i, look_up[detections_dict['detection_classes'][i]], 
            detections_dict['detection_scores'][i], 
            detections_dict['detection_boxes'][i][0],
            detections_dict['detection_boxes'][i][1], 
            detections_dict['detection_boxes'][i][2],
            detections_dict['detection_boxes'][i][3]))    
        
print("\nFound {} objects in image {}.".format(n_obj, image_path))           
```

    Object 0: label=squash     , score=0.99, location=(top: 0.74, left: 0.30, bottom: 0.84, right: 0.42)
    Object 1: label=squash     , score=0.98, location=(top: 0.27, left: 0.21, bottom: 0.37, right: 0.33)
    Object 2: label=orange     , score=0.98, location=(top: 0.31, left: 0.39, bottom: 0.37, right: 0.48)
    Object 3: label=joghurt    , score=0.98, location=(top: 0.57, left: 0.29, bottom: 0.67, right: 0.39)
    Object 4: label=eggBox     , score=0.97, location=(top: 0.41, left: 0.53, bottom: 0.49, right: 0.69)
    Object 5: label=water      , score=0.95, location=(top: 0.23, left: 0.51, bottom: 0.37, right: 0.57)
    Object 6: label=mustard    , score=0.88, location=(top: 0.61, left: 0.47, bottom: 0.66, right: 0.57)
    Object 7: label=ketchup    , score=0.80, location=(top: 0.62, left: 0.62, bottom: 0.68, right: 0.72)
    
    Found 8 objects in image ../sample_data/foods/test\JPEGImages\10.jpg.
    

Net zoals de scores visualiseren voordat.


```python
path_save = out_root_path + "/scored_images/scored_image_frozen_graph.jpg"
ax = detection_utils.visualize(image_path, detections_dict, path_save=path_save, image_size=(8, 12))
# ax.get_figure() # use this code extract the returned image
```

![PNG](media/how-to-build-deploy-object-detection-models/output_30_0.JPG)

## <a name="operationalization-deploy-and-consume"></a>Uitoefening: implementeren en gebruiken

Uitoefening is het proces van het publiceren van modellen en code als webservices en het verbruik van deze services levert geen resultaten voor bedrijven. 

Nadat uw model is getraind, kunt u dit model implementeren als een webservice voor verbruik met [Azure Machine Learning CLI](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/cli-for-azure-machine-learning). Uw modellen kunnen worden toegepast op uw lokale computer of een Azure Container Service (ACS)-cluster. ACS gebruikt, kunt u uw webservice handmatig schalen of gebruik de functie voor automatisch schalen.

**Meld u aan met Azure CLI**

Met behulp van een [Azure](https://azure.microsoft.com/) account met een geldig abonnement, meld u aan met de volgende CLI-opdracht:
<br>`az login`

+ Als u wilt overschakelen naar een andere Azure-abonnement, moet u de opdracht gebruiken:
<br>`az account set --subscription [your subscription name]`

+ Als het huidige model-management-account weergeven, gebruikt u de opdracht:
  <br>`az ml account modelmanagement show`

**Maak en stel uw clusteromgeving voor implementatie**

U hoeft alleen te uw implementatieomgeving eenmaal worden ingesteld. Als u een nog geen hebt, kunt u uw implementatieomgeving nu met gebruik van instellen [deze instructies](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Als uw omgeving actieve implementatie weergeven, gebruikt u de volgende CLI-opdracht:
<br>`az ml env show`
   
Azure CLI voorbeeldopdracht maken en implementatieomgeving instellen

```CLI
az provider register -n Microsoft.MachineLearningCompute
az provider register -n Microsoft.ContainerRegistry
az provider register -n Microsoft.ContainerService
az ml env setup --cluster -n [your environment name] -l [Azure region e.g. westcentralus] [-g [resource group]]
az ml env set -n [environment name] -g [resource group]
az ml env cluster
```
    
### <a name="manage-web-services-and-deployments"></a>Web-services en -implementaties beheren

De volgende API's kunnen worden gebruikt voor modellen als webservices implementeren en beheren van implementaties die webservices te beheren.

|Taak|API|
|----|----|
|Implementatieobject maken|`deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model, aml_env="cluster")`
|-Webservice implementeren|`deploy_obj.deploy()`|
|Score-afbeelding|`deploy_obj.score_image(local_image_path_or_image_url)`|
|Webservice verwijderen|`deploy_obj.delete()`|
|Een installatiekopie docker zonder webservice maken|`deploy_obj.build_docker_image()`|
|Lijst van de bestaande implementatie|`AMLDeployment.list_deployment()`|
|Als de service met de implementatienaam van de bestaat verwijderen|`AMLDeployment.delete_if_service_exist(deployment_name)`|

**API-documentatie:** raadpleegt u de [pakket naslagdocumentatie](https://aka.ms/aml-packages/vision) voor de gedetailleerde naslaginformatie voor elke module en de klasse.

**CLI-verwijzing:** voor meer geavanceerde bewerkingen die zijn gerelateerd aan implementatie, naar verwijzen de [management CLI reference model](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/model-management-cli-reference).

**Implementatiebeheer in Azure-portal**: U kunt bijhouden en beheren van uw implementaties in de [Azure-portal](https://ms.portal.azure.com/). Vinden uw pagina voor het serviceaccount van Machine Learning-Model Management met de naam van de Azure-portal. Ga naar de pagina van de account Model Management > Model Management > Services.

```python
# ##### OPTIONAL - Interactive CLI setup helper ###### 
# # Interactive CLI setup helper, including model management account and deployment environment.
# # If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
# # UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT

# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Create deployment object
# It will use the current deployment environment (you can check it with CLI command "az ml env show").
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=my_detector, replicas=1)

# Alternatively, you can provide azure machine learning deployment cluster name (environment name) and resource group name
# to deploy your model. It will use the provided cluster to deploy. To do that, please uncomment the following lines to create 
# the deployment object.

# azureml_rscgroup = "<resource group>"
# cluster_name = "<cluster name>"
# deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=my_detector,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# create the webservice
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>De web-service gebruiken

Nadat u de webservice hebt gemaakt, kunt u afbeeldingen met de geïmplementeerde webservice beoordelen. U hebt verschillende mogelijkheden:

   - U kunt de webservice met het implementatieobject met rechtstreeks beoordelen: deploy_obj.score_image(image_path_or_url) 
   - Of u kunt de Service-eindpunt-url en de Service-sleutel (geen voor de implementatie van de lokale) gebruiken met: AMLDeployment.score_existing_service_with_image (image_path_or_url, service_endpoint_url, service_key = None)
   - Vormen uw http-aanvragen rechtstreeks naar de webservice-eindpunt beoordelen (voor ervaren gebruikers).

### <a name="score-with-existing-deployment-object"></a>Score met bestaande implementatieobject
```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = data_train.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])

```


```python
# Time image scoring
import timeit

num_images = 3
for img_index, img_obj in enumerate(data_train.images[:num_images]):
    print("Calling API for image {} of {}: {}...".format(img_index, num_images, img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Met de service-eindpunt-url en servicesleutel beoordelen
```
    AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)
```


```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "http://xxx" # please replace with your own service url
service_key = "xxx" # please replace with your own service key

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json[:50])
```

### <a name="score-endpoint-with-http-request-directly"></a>Score-eindpunt met HTTP-aanvraag rechtstreeks
Hieronder vindt u enkele voorbeeldcode om de http-aanvraag rechtstreeks in Python. U kunt dit doen in andere programmeertalen.


```python
def score_image_with_http(image, service_endpoint_url, service_key=None, parameters={}):
    """Score local image with http request

    Args:
        image (str): Image file path
        service_endpoint_url(str): web service endpoint url
        service_key(str): Service key. None for local deployment.
        parameters (dict): Additional request paramters in dictionary. Default is {}.


    Returns:
        str: serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    import json

    if service_key is None:
        headers = {'Content-Type': 'application/json'}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key)}
    payload = []
    encoded = None
    
    # Read image
    with open(image,'rb') as f:
        image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        
    # Convert your image to base64 string
    # image_in_base64 : "b'{base64}'"
    encoded = base64.b64encode(image_buffer.getvalue())
    image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
    payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
        json.loads(result[0])
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result[0]

```

### <a name="parse-serialized-result-from-webservice"></a>Parseren van geserialiseerde resultaat van de webservice
Het resultaat van de webservice is in json-tekenreeks die kan worden geparseerd.


```python
image_path_or_url = image_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json[:50])
```


```python
# Parse result from json string
import numpy as np
parsed_result = TFFasterRCNN.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
```


```python
ax = detection_utils.visualize(image_path, parsed_result)
path_save = "../../../cvtk_output/scored_images/scored_image_web.jpg"
path_save_dir = os.path.dirname(os.path.abspath(path_save))
os.makedirs(path_save_dir, exist_ok=True)
ax.get_figure().savefig(path_save)
```

## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Machine Learning-pakket voor Computer Vision in deze artikelen:

+ Lees de [overzicht van het pakket en informatie over het installeren van deze](https://aka.ms/aml-packages/vision).

+ Verken de [documentatie verwijst naar](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) voor dit pakket.

+ Meer informatie over [andere Python-pakketten voor Azure Machine Learning](reference-python-package-overview.md).
