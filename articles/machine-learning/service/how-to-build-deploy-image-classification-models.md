---
title: Bouw en implementeer een installatiekopie classificatie-model met behulp van Azure Machine Learning-pakket voor de visie van de Computer.
description: Informatie over het bouwen, te trainen, te testen en te implementeren van een visie installatiekopie classificatie computermodel met het Azure Machine Learning-pakket voor Computer Vision.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: netahw
author: nhaiby
ms.date: 04/23/2018
ms.openlocfilehash: bd9f01e76c68fa41616818251b5b54553059cbcc
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="build-and-deploy-image-classification-models-with-azure-machine-learning"></a>Bouw en implementeer de installatiekopie classificatiemodellen kunnen met Azure Machine Learning

Informatie over het gebruik in dit artikel **Azure Machine Learning-pakket voor Computer Vision** (AMLPCV) te trainen, testen en implementeren van een installatiekopie van classificatie-model. 

Een groot aantal problemen in het domein van de visie computer kan worden opgelost met de classificatie van de installatiekopie. Deze problemen zijn voor het bouwen van modellen die antwoorden op vragen zoals:
+ _Een OBJECT aanwezig in de afbeelding is? Bijvoorbeeld 'aquaduct', 'auto', 'verzenden', enzovoort_
+ _Welke klasse ogen ziekte ernst wordt door deze geduld retinal scan evinced?_

Bij het maken en implementeren van dit model met AMLPCV, gaat u de volgende stappen:
1. Het maken van de gegevensset
2. Visualisatie van de installatiekopie en aantekening
3. Afbeelding uitdiepen
4. Modeldefinitie diep Neurale netwerk (DNN)
5. Training van classificatie
6. Evaluatie en visualisatie
7. Implementatie-webservice
8. Testen van de Load-webservice

[CNTK](https://www.microsoft.com/cognitive-toolkit/) wordt gebruikt als het framework grondige learning training wordt lokaal uitgevoerd op een machine GPU ingeschakeld, zoals de ([grondige learning gegevens wetenschappelijke VM](https://azuremarketplace.microsoft.com/marketplace/apps/microsoft-ads.dsvm-deep-learning?tab=Overview)), en implementatie gebruikmaakt van de Azure ML uitoefening CLI.

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
> [De Jupyter-notebook ophalen](https://aka.ms/aml-packages/vision/notebooks/image_classification)

### <a name="load-the-sample-data"></a>De voorbeeldgegevens laden

Het volgende voorbeeld wordt een gegevensset die bestaan uit 63 tafelgerei installatiekopieën. Elke installatiekopie wordt aangeduid als onderdeel van een van de vier verschillende klassen (kom, kop, bestek, plaat). Het aantal afbeeldingen in dit voorbeeld is klein zodat dit voorbeeld kan snel worden uitgevoerd. In de praktijk moeten ten minste 100 afbeeldingen per klasse worden opgegeven. Alle installatiekopieën bevinden zich op *'... /sample_data/imgs_recycling / '*, in submappen aangeroepen 'kom', 'kop', 'bestek' en 'plaat'.

![Azure Machine Learning-gegevensset](media/how-to-build-deploy-image-classification-models/recycling_examples.jpg)

## <a name="storage-context"></a>Opslag-context

De context van de opslag wordt gebruikt om te bepalen waarin verschillende uitvoerbestanden zoals uitgebreide afbeeldingen of DNN modelbestanden wordt opgeslagen. Zie voor meer informatie over opslag contexten de [StorageContext documentatie](https://review.docs.microsoft.com/en-us/python/api/cvtk.core.context.storagecontext?view=azure-python&branch=smoke-test). 

De inhoud van de opslag hoeft normaal gesproken niet expliciet worden ingesteld. Echter om te voorkomen de 25 MB bereikt voor de projectgrootte die zijn opgelegd door de Azure Machine Learning-Workbench, de uitvoer directory instellen voor het Azure Machine Learning-pakket voor Computer Vision naar een locatie buiten het Azure Machine Learning-project ('... /.. /.. /.. / cvtk_output '). Zorg ervoor dat de map 'cvtk_output' verwijderen wanneer deze niet meer nodig is.


```python
import warnings
warnings.filterwarnings("ignore")
import json, numpy as np, os, timeit 
from azureml.logging import get_azureml_logger
from imgaug import augmenters
from IPython.display import display
from sklearn import svm
from cvtk import ClassificationDataset, CNTKTLModel, Context, Splitter, StorageContext
from cvtk.augmentation import augment_dataset
from cvtk.core.classifier import ScikitClassifier
from cvtk.evaluation import ClassificationEvaluation, graph_roc_curve, graph_pr_curve, graph_confusion_matrix, basic_plot
import matplotlib.pyplot as plt
%matplotlib inline

# Disable printing of logging messages
from azuremltkbase.logging import ToolkitLogger
ToolkitLogger.getInstance().setEnabled(False)

# Set storage context.
out_root_path = "../../../cvtk_output"
Context.create(outputs_path=out_root_path, persistent_path=out_root_path, temp_path=out_root_path)
```




    {
        "storage": {
            "outputs_path": "../../../cvtk_output",
            "persistent_path": "../../../cvtk_output",
            "temp_path": "../../../cvtk_output"
        }
    }



## <a name="create-a-dataset"></a>Een gegevensset maken

Zodra u hebt geïmporteerd, de afhankelijkheden en stelt u de context van de opslag, kunt u het object dataset.

Als u wilt dat object maken met Azure Machine Learning-pakket voor Computer Vision, bieden de hoofdmap van de installatiekopieën op de lokale schijf. Deze map moet dezelfde algemene structuur als de dataset tafelgerei die volgen, submappen met de werkelijke installatiekopieën bevatten:
- hoofdmap
    - label 1
    - Label 2
    - ...
    - label n
  
Een installatiekopie classificatie model trainen voor een andere dataset net zo eenvoudig is als het pad naar de hoofdmap wijzigen `dataset_location` in de volgende code verschillende afbeeldingen verwijzen.


```python
# Root image directory 
dataset_location = os.path.abspath(os.path.join(os.getcwd(), "../sample_data/imgs_recycling"))

dataset_name = 'recycling'
dataset = ClassificationDataset.create_from_dir(dataset_name, dataset_location)
print("Dataset consists of {} images with {} labels.".format(len(dataset.images), len(dataset.labels)))
print("Select information for image 2: name={}, label={}, unique id={}.".format(
    dataset.images[2].name, dataset.images[2]._labels[0].name, dataset.images[2]._storage_id))
```

    F1 2018-04-23 17:12:57,593 INFO azureml.vision:machine info {"is_dsvm": true, "os_type": "Windows"} 
    F1 2018-04-23 17:12:57,599 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Dataset consists of 63 images with 4 labels.
    Select information for image 2: name=msft-plastic-bowl20170725152154282.jpg, label=bowl, unique id=3.

Het object dataset biedt functionaliteit om het downloaden van afbeeldingen met behulp van de [Bing-API van zoekservice installatiekopie](https://azure.microsoft.com/services/cognitive-services/bing-image-search-api/). 

Er zijn twee soorten zoekquery's ondersteund: 
+ Gewone tekst query 's
+ Afbeelding URL-query 's

Deze query's samen met het label van de klasse moeten worden opgegeven in een bestand met JSON gecodeerde tekst. Bijvoorbeeld:

```json
{
    "bowl": [
                    "plastic bowl",
                    "../imgs_recycling/bowl"
    ],
    "cup": [
                    "plastic cup",
                    "../imgs_recycling/cup",
                    "http://cdnimg2.webstaurantstore.com/images/products/main/123662/268841/dart-solo-ultra-clear-conex-tp12-12-oz-pet-plastic-cold-cup-1000-case.jpg"
    ],
    "cutlery": [
                    "plastic cutlery",
                    "../imgs_recycling/cutlery",
                    "http://img4.foodservicewarehouse.com/Prd/1900SQ/Fineline_2514-BO.jpg"
    ],
    "plate": [
                    "plastic plate",
                    "../imgs_recycling/plate"
    ]
}
```

Bovendien moet u expliciet een Context-object bevat de Bing installatiekopie zoeken-API-sleutel maken. Hiervoor moet een Bing installatiekopie zoeken-API-abonnement.

## <a name="visualize-and-annotate-images"></a>Visualiseren en aantekeningen toevoegen aan installatiekopieën

U kunt de afbeeldingen en de juiste labels in de dataset-object op met de volgende widget visualiseren. 

Als de 'Widget Javascript is niet gevonden'-fout optreedt, voert u deze opdracht voor het oplossen van het:
<br>`jupyter nbextension enable --py --sys-prefix widgetsnbextension`


```python
from ui_utils.ui_annotation import AnnotationUI
annotation_ui = AnnotationUI(dataset, Context.get_global_context())
display(annotation_ui.ui)
```

![Azure Machine Learning-gegevensset](media/how-to-build-deploy-image-classification-models/image_annotation.png)

## <a name="augment-images"></a>Afbeeldingen verbeteren

De [ `augmentation` module](https://docs.microsoft.com/en-us/python/api/cvtk.augmentation) biedt functionaliteit om het uitbreiden van een dataset-object met behulp van de transformaties die zijn beschreven in de [imgaug](https://github.com/aleju/imgaug) bibliotheek. Afbeelding transformaties kunnen worden gegroepeerd in één pijplijn in dat geval alle transformaties in de pijplijn tegelijkertijd worden toegepast elke installatiekopie. 

Als u wilt verschillende uitdiepen stappen afzonderlijk toepassen, of op een andere manier kunt u meerdere pijplijnen definiëren en ze doorgeven aan de *augment_dataset* functie. Zie voor meer informatie en voorbeelden van afbeelding uitdiepen de [imgaug documentatie](https://github.com/aleju/imgaug).

Uitgebreide afbeeldingen toevoegen aan de trainingset is met name handig voor kleine gegevenssets. Aangezien het DNN training-proces langzamer vanwege het toegenomen aantal training afbeeldingen is, raden we aan dat u experimenteren zonder uitdiepen start.


```python
# Split the dataset into train and test  
train_set_orig, test_set = dataset.split(train_size = 0.66, stratify = "label")
print("Number of training images = {}, test images = {}.".format(train_set_orig.size(), test_set.size()))
```

    F1 2018-04-23 17:13:01,780 INFO azureml.vision:splitter splitting a dataset 
    F1 2018-04-23 17:13:01,805 INFO azureml.vision:dataset creating dataset for scenario=classification 
    F1 2018-04-23 17:13:01,809 INFO azureml.vision:dataset creating dataset for scenario=classification 
    Number of training images = 41, test images = 22.
    


```python
augment_train_set = False

if augment_train_set:
    aug_sequence = augmenters.Sequential([
            augmenters.Fliplr(0.5),             # horizontally flip 50% of all images
            augmenters.Crop(percent=(0, 0.1)),  # crop images by 0-10% of their height/width
        ])
    train_set = augment_dataset(train_set_orig, [aug_sequence])
    print("Number of original training images = {}, with augmented images included = {}.".format(train_set_orig.size(), train_set.size()))
else:
    train_set = train_set_orig  
```

## <a name="define-dnn-models"></a>DNN modellen definiëren

De volgende pretrained Deep Neural Network-modellen worden ondersteund met dit pakket: 
+ AlexNet
+ Resnet 18
+ Resnet 34
+ Resnet 50
+ Resnet 101
+ Resnet 152

Deze DNNs kunnen worden gebruikt als classificatie, of als featurizer. 

Meer informatie over de netwerken vindt u [hier](https://github.com/Microsoft/CNTK/blob/master/PretrainedModels/Image.md), en is een algemene inleiding tot naar Transfer Learning [hier](https://blog.slavv.com/a-gentle-intro-to-transfer-learning-2c0b674375a0).

De standaard installatiekopie classificatieparameters voor dit pakket zijn 224 x 224 pixelresolutie en een DNN Resnet 18. Deze parameters zijn geselecteerd voor de goede werking op een groot aantal taken. Nauwkeurigheid kan vaak worden verbeterd, bijvoorbeeld door het verhogen van de installatiekopie resolutie en 500 x 500 pixels en/of een dieper model (Resnet 50) selecteren. Echter als u de parameters kan worden geleverd op een aanzienlijke toename in de trainingstijd. Zie het artikel op [hoe nauwkeuriger](https://docs.microsoft.com/azure/machine-learning/service/how-to-improve-accuracy-for-computer-vision-models).


```python
# Default parameters (224 x 224 pixels resolution, Resnet-18)
lr_per_mb = [0.05]*7 + [0.005]*7 +  [0.0005]
mb_size = 32
input_resoluton = 224
base_model_name = "ResNet18_ImageNet_CNTK"

# Suggested parameters for 500 x 500 pixels resolution, Resnet-50
# (see in the Appendix "How to improve accuracy", last row in table)
# lr_per_mb   = [0.01] * 7 + [0.001] * 7 + [0.0001]
# mb_size    = 8
# input_resoluton = 500
# base_model_name = "ResNet50_ImageNet_CNTK"

# Initialize model
dnn_model = CNTKTLModel(train_set.labels,
                       base_model_name=base_model_name,
                       image_dims = (3, input_resoluton, input_resoluton))
```

    Successfully downloaded ResNet18_ImageNet_CNTK
    

## <a name="train-the-classifier"></a>De classificatie trainen

U kunt een van de volgende methoden voor het vooraf getraind DNN.

  - **DNN verfijning**, die de DNN rechtstreeks uitvoeren van de classificatie traint. Tijdens de training DNN traag is, leidt het doorgaans tot de beste resultaten omdat het gewicht van alle netwerk tijdens de training waarmee nauwkeurigheid kunnen worden verbeterd.

  - **DNN featurization**, die wordt uitgevoerd de DNN als-is op te halen van een lagere dimensionale weergave van een installatiekopie (512, 2048 of 4096 zwevend). Deze weergave wordt vervolgens gebruikt als invoer voor het trainen van een afzonderlijke classificatie. Omdat de DNN ongewijzigd blijft, wordt deze methode veel sneller vergeleken met DNN verfijning, maar nauwkeurigheid niet goed is. Echter kan een externe classificatie zoals een lineaire SVM training (zoals weergegeven in de volgende code) een sterke referentiepunt te bieden, en helpen bij het begrijpen van de haalbaarheid van een probleem.
  
TensorBoard kan worden gebruikt om de voortgang van de training te visualiseren. TensorBoard activeren:
1. De parameter toevoegen `tensorboard_logdir=PATH` zoals weergegeven in de volgende code
1. Start de TensorBoard-client met de opdracht `tensorboard --logdir=PATH` in een nieuwe console.
1. Open een webbrowser volgens de instructies van TensorBoard, dat standaard localhost:6006 is. 


```python
# Train either the DNN or a SVM as classifier 
classifier_name = "dnn"

if classifier_name.lower() == "dnn":  
    dnn_model.train(train_set, lr_per_mb = lr_per_mb, mb_size = mb_size, eval_dataset=test_set) #, tensorboard_logdir=r"tensorboard"
    classifier = dnn_model
elif classifier_name.lower() == "svm":
    learner = svm.LinearSVC(C=1.0, class_weight='balanced', verbose=0)
    classifier = ScikitClassifier(dnn_model, learner = learner)
    classifier.train(train_set)
else:
    raise Exception("Classifier unknown: " + classifier)   
```

    F1 2018-04-23 17:13:28,238 INFO azureml.vision:Fit starting in experiment  1541466320 
    F1 2018-04-23 17:13:28,239 INFO azureml.vision:model starting training for scenario=classification 
    <class 'int'>
    1 worker
    Training transfer learning model for 15 epochs (epoch_size = 41).
    non-distributed mode
    Training 15741700 parameters in 53 parameter tensors.
    Training 15741700 parameters in 53 parameter tensors.
    Learning rate per minibatch: 0.05
    Momentum per minibatch: 0.9
    PROGRESS: 0.00%
    Finished Epoch[1 of 15]: [Training] loss = 2.820586 * 41, metric = 68.29% * 41 5.738s (  7.1 samples/s);
    Evaluation Set Error :: 29.27%
    Finished Epoch[2 of 15]: [Training] loss = 0.286728 * 41, metric = 9.76% * 41 0.752s ( 54.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[3 of 15]: [Training] loss = 0.206938 * 41, metric = 4.88% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 41.46%
    Finished Epoch[4 of 15]: [Training] loss = 0.098931 * 41, metric = 2.44% * 41 0.785s ( 52.2 samples/s);
    Evaluation Set Error :: 48.78%
    Finished Epoch[5 of 15]: [Training] loss = 0.046547 * 41, metric = 0.00% * 41 0.724s ( 56.6 samples/s);
    Evaluation Set Error :: 43.90%
    Finished Epoch[6 of 15]: [Training] loss = 0.059709 * 41, metric = 4.88% * 41 0.636s ( 64.5 samples/s);
    Evaluation Set Error :: 34.15%
    Finished Epoch[7 of 15]: [Training] loss = 0.005817 * 41, metric = 0.00% * 41 0.710s ( 57.7 samples/s);
    Evaluation Set Error :: 14.63%
    Learning rate per minibatch: 0.005
    Finished Epoch[8 of 15]: [Training] loss = 0.014917 * 41, metric = 0.00% * 41 0.649s ( 63.2 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[9 of 15]: [Training] loss = 0.040539 * 41, metric = 2.44% * 41 0.777s ( 52.8 samples/s);
    Evaluation Set Error :: 9.76%
    Finished Epoch[10 of 15]: [Training] loss = 0.024606 * 41, metric = 0.00% * 41 0.626s ( 65.5 samples/s);
    Evaluation Set Error :: 7.32%
    PROGRESS: 0.00%
    Finished Epoch[11 of 15]: [Training] loss = 0.004225 * 41, metric = 0.00% * 41 0.656s ( 62.5 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[12 of 15]: [Training] loss = 0.004364 * 41, metric = 0.00% * 41 0.702s ( 58.4 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[13 of 15]: [Training] loss = 0.007974 * 41, metric = 0.00% * 41 0.721s ( 56.9 samples/s);
    Evaluation Set Error :: 4.88%
    Finished Epoch[14 of 15]: [Training] loss = 0.000655 * 41, metric = 0.00% * 41 0.711s ( 57.7 samples/s);
    Evaluation Set Error :: 4.88%
    Learning rate per minibatch: 0.0005
    Finished Epoch[15 of 15]: [Training] loss = 0.024865 * 41, metric = 0.00% * 41 0.688s ( 59.6 samples/s);
    Evaluation Set Error :: 4.88%
    Stored trained model at ../../../cvtk_output\model_trained\ImageClassification.model
    F1 2018-04-23 17:13:45,097 INFO azureml.vision:Fit finished in experiment  1541466320 
    


```python
# Plot how the training and test accuracy increases during gradient descent. 
if classifier_name == "dnn":
    [train_accs, test_accs, epoch_numbers] = classifier.train_eval_accs
    plt.xlabel("Number of training epochs") 
    plt.ylabel("Classification accuracy") 
    train_plot = plt.plot(epoch_numbers, train_accs, 'r-', label = "Training accuracy")
    test_plot = plt.plot(epoch_numbers, test_accs, 'b-.', label = "Test accuracy")
    plt.legend()
```

![PNG](media/how-to-build-deploy-image-classification-models/output_17_0.png)


## <a name="evaluate-and-visualize-model-performance"></a>Evalueren en visualiseren model prestaties

U kunt de prestaties van het getrainde model op een onafhankelijke testgegevensset met de module evaluatie evalueren. Enkele van de evaluatie van metrische gegevens die wordt berekend:
 
+ Nauwkeurigheid (standaard klasse gemiddeld)
+ PR curve
+ ROC-curve
+ Gebied onder curve
+ Verwarring matrix


```python
# Run the classifier on all test set images
ce = ClassificationEvaluation(classifier, test_set, minibatch_size = mb_size)

# Compute Accuracy and the confusion matrix
acc = ce.compute_accuracy()
print("Accuracy = {:2.2f}%".format(100*acc))
cm  = ce.compute_confusion_matrix()
print("Confusion matrix = \n{}".format(cm))

# Show PR curve, ROC curve, and confusion matrix
fig, ((ax1, ax2, ax3)) = plt.subplots(1,3)
fig.set_size_inches(18, 4)
graph_roc_curve(ce, ax=ax1)
graph_pr_curve(ce, ax=ax2)
graph_confusion_matrix(ce, ax=ax3)
plt.show()
```

    F1 2018-04-23 17:14:37,449 INFO azureml.vision:evaluation doing evaluation for scenario=classification 
    F1 2018-04-23 17:14:37,450 INFO azureml.vision:model scoring dataset for scenario=classification 
    Accuracy = 95.45%
    Confusion matrix = 
    [[ 0  1  0  1]
     [ 0  7  0  0]
     [ 0  0  2  0]
     [ 0  0  0 11]]
    


![PNG](media/how-to-build-deploy-image-classification-models/output_20_1.png)



```python
# Results viewer UI
labels = [l.name for l in dataset.labels] 
pred_scores = ce.scores #classification scores for all images and all classes
pred_labels = [labels[i] for i in np.argmax(pred_scores, axis=1)]

from ui_utils.ui_results_viewer import ResultsUI
results_ui = ResultsUI(test_set, Context.get_global_context(), pred_scores, pred_labels)
display(results_ui.ui)
```

![Azure Machine Learning-gegevensset](media/how-to-build-deploy-image-classification-models/Image_Classification_Results.png)


```python
# Precision / recall curve UI
precisions, recalls, thresholds = ce.compute_precision_recall_curve() 
thresholds = list(thresholds)
thresholds.append(thresholds[-1])
from ui_utils.ui_precision_recall import PrecisionRecallUI
pr_ui = PrecisionRecallUI(100*precisions[::-1], 100*recalls[::-1], thresholds[::-1])
display(pr_ui.ui) 
```

![Azure Machine Learning-gegevensset](media/how-to-build-deploy-image-classification-models/image_precision_curve.png)

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

**Maak en stel uw implementatieomgeving**

U hoeft alleen te uw implementatieomgeving eenmaal worden ingesteld. Als u een nog geen hebt, kunt u uw implementatieomgeving nu met gebruik van instellen [deze instructies](https://docs.microsoft.com/azure/machine-learning/desktop-workbench/deployment-setup-configuration#environment-setup). 

Volg de lokale of cluster implementatie installatiestappen correct op basis van uw behoeften.
+ Lokale implementaties worden ondersteund voor Linux en Windows 10-computers, maar niet voor de virtuele machine van Windows gegevens wetenschap of de grondige Learning VM. 
+ Implementaties van een cluster omgeving worden ondersteund voor Linux- en Windows. 

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
# ##### OPTIONAL###### 
# Interactive CLI setup helper, including model management account and deployment environment.
# If you haven't setup you CLI before or if you want to change you CLI settings, you can use this block to help you interactively.
#
# UNCOMMENT THE FOLLOWING LINES IF YOU HAVE NOT CREATED OR SET THE MODEL MANAGEMENT ACCOUNT AND DEPLOYMENT ENVIRONMENT
#
# from azuremltkbase.deployment import CliSetup
# CliSetup().run()
```


```python
# # Optional. Persist your model on disk and reuse it later for deployment. 
# from cvtk import TFFasterRCNN, Context
# import os
# save_model_path = os.path.join(Context.get_global_context().storage.persistent_path, "saved_classifier.model")
# # Save model to disk
# dnn_model.serialize(save_model_path)
# # Load model from disk
# dnn_model = CNTKTLModel.deserialize(save_model_path)
```


```python
from cvtk.operationalization import AMLDeployment

# set deployment name
deployment_name = "wsdeployment"

# Optional Azure Machine Learning deployment cluster name (environment name) and resource group name
# If you don't provide here. It will use the current deployment environment (you can check with CLI command "az ml env show").
azureml_rscgroup = "<resource group>"
cluster_name = "<cluster name>"

# If you provide the cluster information, it will use the provided cluster to deploy. 
# Example: deploy_obj = AMLDeployment(deployment_name=deployment_name, associated_DNNModel=dnn_model,
#                            aml_env="cluster", cluster_name=cluster_name, resource_group=azureml_rscgroup, replicas=1)

# Create deployment object
deploy_obj = AMLDeployment(deployment_name=deployment_name, aml_env="cluster", associated_DNNModel=dnn_model, replicas=1)

# Check if the deployment name exists, if yes remove it first.
if deploy_obj.is_existing_service():
    AMLDeployment.delete_if_service_exist(deployment_name)
    
# Create the web service
print("Deploying to Azure cluster...")
deploy_obj.deploy()
print("Deployment DONE")
```

### <a name="consume-the-web-service"></a>De web-service gebruiken 

Zodra u het model als een webservice implementeert, kunt u installatiekopieën score met de webservice met behulp van een van deze methoden:

- De webservice rechtstreeks met de implementatie van object met behulp van beoordelen `deploy_obj.score_image(image_path_or_url)`

- Gebruik de eindpunt-URL en de Service servicesleutel (geen voor de implementatie van de lokale) met: `AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

- De HTTP-aanvragen rechtstreeks naar de webservice-eindpunt score vormen. Deze optie is bedoeld voor ervaren gebruikers.

### <a name="score-with-existing-deployment-object"></a>Score met bestaande implementatieobject

```
deploy_obj.score_image(image_path_or_url)
```


```python
# Score with existing deployment object

# Score local image with file path
print("Score local image with file path")
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url and remove image resizing
print("Score image url")
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url)
print("serialized_result_in_json:", serialized_result_in_json)

# Score image url with added paramters. Add softmax to score.
print("Score image url with added paramters. Add softmax to score")
from cvtk.utils.constants import ClassificationRESTApiParamters
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224], parameters={ClassificationRESTApiParamters.ADD_SOFTMAX:True})
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Time image scoring
import timeit

for img_index, img_obj in enumerate(test_set.images[:10]):
    print("Calling API for image {} of {}: {}...".format(img_index, len(test_set.images), img_obj.name))
    tic = timeit.default_timer()
    return_json = deploy_obj.score_image(img_obj.storage_path, image_resize_dims=[224,224])
    print("   Time for API call: {:.2f} seconds".format(timeit.default_timer() - tic))
    print(return_json)
```

### <a name="score-with-service-endpoint-url-and-service-key"></a>Met de service-eindpunt-url en servicesleutel beoordelen

`AMLDeployment.score_existing_service_with_image(image_path_or_url, service_endpoint_url, service_key=None)`

```python
# Import related classes and functions
from cvtk.operationalization import AMLDeployment

service_endpoint_url = "" # please replace with your own service url
service_key = "" # please replace with your own service key
# score local image with file path
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key)
print("serialized_result_in_json:", serialized_result_in_json)

# score image url
image_path_or_url = "https://cvtkdata.blob.core.windows.net/publicimages/microsoft_logo.jpg"
print("Image source:",image_path_or_url)
serialized_result_in_json = AMLDeployment.score_existing_service_with_image(image_path_or_url,service_endpoint_url, service_key = service_key, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```

### <a name="score-endpoint-with-http-request-directly"></a>Score-eindpunt met HTTP-aanvraag rechtstreeks

De volgende voorbeeldcode vormt de HTTP-aanvraag rechtstreeks in Python. Echter, kunt u dit doen in andere programmeertalen.


```python
def score_image_list_with_http(images, service_endpoint_url, service_key=None, parameters={}):
    """Score image list with http request

    Args:
        images(list): list of (input image file path, base64 image string, url or buffer)
        service_endpoint_url(str): endpoint url
        service_key(str): service key, None for local deployment.
        parameters(dict): service additional paramters in dictionary


    Returns:
        result (list): list of serialized result 
    """
    import requests
    from io import BytesIO
    import base64
    routing_id = ""

    if service_key is None:
        headers = {'Content-Type': 'application/json',
                   'X-Marathon-App-Id': routing_id}
    else:
        headers = {'Content-Type': 'application/json',
                   "Authorization": ('Bearer ' + service_key), 'X-Marathon-App-Id': routing_id}
    payload = []
    for image in images:
        encoded = None
        # read image
        with open(image,'rb') as f:
            image_buffer = BytesIO(f.read()) ## Getting an image file represented as a BytesIO object
        # convert your image to base64 string
        encoded = base64.b64encode(image_buffer.getvalue())
        image_request = {"image_in_base64": "{0}".format(encoded), "parameters": parameters}
        payload.append(image_request)
    body = json.dumps(payload)
    r = requests.post(service_endpoint_url, data=body, headers=headers)
    try:
        result = json.loads(r.text)
    except:
        raise ValueError("Incorrect output format. Result cant not be parsed: " + r.text)
    return result

# Test with images
images = [test_set.images[0].storage_path, test_set.images[1].storage_path] # A list of local image files
score_image_list_with_http(images, service_endpoint_url, service_key)
```

### <a name="parse-serialized-result-from-web-service"></a>Parseren van geserialiseerde resultaat van de webservice

De uitvoer van de webservice is een JSON-tekenreeks. Deze JSON-tekenreeks met verschillende DNN modelklassen kunnen worden geparseerd.


```python
image_path_or_url = test_set.images[0].storage_path
print("Image source:",image_path_or_url)
serialized_result_in_json = deploy_obj.score_image(image_path_or_url, image_resize_dims=[224,224])
print("serialized_result_in_json:", serialized_result_in_json)
```


```python
# Parse result from json string
import numpy as np
parsed_result = CNTKTLModel.parse_serialized_result(serialized_result_in_json)
print("Parsed result:", parsed_result)
# Map result to image class
class_index = np.argmax(np.array(parsed_result))
print("Class index:", class_index)
dnn_model.class_map
print("Class label:", dnn_model.class_map[class_index])
```


## <a name="next-steps"></a>Volgende stappen

Meer informatie over Azure Machine Learning-pakket voor Computer Vision in deze artikelen:

+ Meer informatie over hoe [verbeteren de nauwkeurigheid van dit model](how-to-improve-accuracy-for-computer-vision-models.md).

+ Lees de [overzicht van het pakket en informatie over het installeren van deze](https://aka.ms/aml-packages/vision).

+ Verken de [documentatie verwijst naar](https://docs.microsoft.com/python/api/overview/azure-machine-learning/computer-vision) voor dit pakket.

+ Meer informatie over [andere Python-pakketten voor Azure Machine Learning](reference-python-package-overview.md).
