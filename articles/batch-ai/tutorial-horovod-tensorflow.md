---
title: Zelfstudie - Gedistribueerde training met Azure Batch AI en Horovod | Microsoft Docs
description: Zelfstudie - Een gedistribueerde model trainen met Horovod met behulp van de service Azure Batch AI en Azure CLI.
services: batch-ai
author: johnwu10
manager: jeconnoc
ms.service: batch-ai
ms.topic: tutorial
ms.date: 09/03/2018
ms.author: danlep
ms.custom: mvc
ROBOTS: NOINDEX
ms.openlocfilehash: 45255845d8645391ee33471830ac2ef27870a40d
ms.sourcegitcommit: c37122644eab1cc739d735077cf971edb6d428fe
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/14/2018
ms.locfileid: "53408626"
---
# <a name="tutorial-train-a-distributed-model-with-horovod"></a>Zelfstudie: Een gedistribueerde model trainen met Horovod

[!INCLUDE [batch-ai-retiring](../../includes/batch-ai-retiring.md)]

In deze zelfstudie traint u een gedistribueerd deep learning-model door het parallel uit te voeren op meerdere knooppunten in een Batch AI-cluster. Batch AI is een beheerde service voor het trainen van machine learning- en AI-modellen op grote schaal over GPU-clusters van Azure. 

In deze zelfstudie wordt een algemene Batch AI-werkstroom geïntroduceerd en werkt u met Batch AI-resources via Azure CLI. De behandelde onderwerpen zijn onder meer:

> [!div class="checklist"]
> * Een Batch AI-werkruimte, experiment en cluster instellen
> * Een Azure-bestandsshare instellen voor invoer en uitvoer
> * Een deep learning-model parallelliseren met Horovod
> * Een trainingstaak verzenden
> * De taak bewaken
> * De resultaten van de training ophalen

Voor deze zelfstudie wordt een objectdetectiemodel zo gewijzigd dat het parallel wordt uitgevoerd met [Horovod](https://github.com/uber/horovod). Het model wordt getraind op de [CIFAR-10-gegevensset](https://www.cs.toronto.edu/~kriz/cifar.html) met installatiekopieën. De trainingstaak wordt uitgevoerd op een cluster met 24 vCPU's en 4 GPU's, en duurt ongeveer 60 minuten.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Als u ervoor kiest om de CLI lokaal te installeren en te gebruiken, moet u Azure CLI 2.0.38 of hoger gebruiken voor deze zelfstudie. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren](/cli/azure/install-azure-cli) als u de CLI wilt installeren of een upgrade wilt uitvoeren. 

## <a name="why-use-horovod"></a>Waarom moet ik Horovod gebruiken?

Horovod is een framework voor gedistribueerde training voor Tensorflow, Keras en PyTorch en wordt gebruikt voor deze zelfstudie. Met Horovod en slechts enkele regels programmacode kunt u een trainingsscript dat is ontworpen om te worden uitgevoerd op één GPU converteren naar een script dat efficiënt wordt uitgevoerd op een gedistribueerd systeem.

Naast Horovod biedt Batch AI ook ondersteuning voor gedistribueerde training met verschillende andere populaire open-source frameworks. Lees de licentievoorwaarden van het framework dat u wilt gebruiken voor het trainen van modellen in een productieomgeving.

## <a name="prepare-the-batch-ai-environment"></a>De Batch AI-omgeving voorbereiden

### <a name="create-a-resource-group"></a>Een resourcegroep maken

Maak een resourcegroep met de naam `batchai.horovod` in de regio `eastus` met behulp van de opdracht `az group create`. U gebruikt de resourcegroep voor het implementeren van Batch AI-resources.

```azurecli-interactive
az group create --name batchai.horovod --location eastus
```

### <a name="create-a-workspace"></a>Een werkruimte maken

Maak een Batch AI-werkruimte met de `az batchai workspace create`-opdracht. Een werkruimte is een verzameling op het hoogste niveau van andere Batch AI-resources. Met de volgende opdracht maakt u een werkruimte met de naam `batchaidev` in de resourcegroep.

```azurecli-interactive
az batchai workspace create --resource-group batchai.horovod --workspace batchaidev 
```

### <a name="create-an-experiment"></a>Een experiment maken

In een Batch AI-experiment worden een of meer taken gegroepeerd zodat u ze samen kunt beheren en er query's op kunt uitvoeren. Met de volgende `az batchai experiment create`-opdracht maakt u een experiment met de naam `cifar` in de werkruimte en resourcegroep.

```azurecli-interactive
az batchai experiment create --resource-group batchai.horovod --workspace batchaidev --name cifar 
```

## <a name="set-up-a-gpu-cluster"></a>Een GPU-cluster instellen

Stel vervolgens een GPU-cluster voor de uitvoering van het experiment in. Batch AI biedt allerlei mogelijkheden voor het aanpassen van clusters voor specifieke behoeften.

Met de volgende `az batchai cluster create`-opdracht maakt u een cluster met vier knooppunten met de naam `nc6cluster` in de werkruimte en resourcegroep. Standaard wordt op de VM's in het cluster een Ubuntu Server-installatiekopie voor containerhosttoepassingen uitgevoerd. Voor de clusterknooppunten in dit voorbeeld wordt een grootte van `Standard_NC6` gebruikt, met één NVIDIA Tesla K80-GPU.

```azurecli-interactive
az batchai cluster create --resource-group batchai.horovod --workspace batchaidev --name nc6cluster --vm-priority dedicated  --vm-size Standard_NC6 --target 4 --generate-ssh-keys
```

Voer de `az batchai cluster show`-opdracht uit om de status van het cluster weer te geven. Het duurt meestal een paar minuten voordat het cluster volledig is ingericht.

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --output table
```

Als u begint met het maken van een cluster, heeft het cluster eerst de status `resizing`. Ga door met de volgende stappen terwijl de status van het cluster verandert. U kunt de trainingstaak uitvoeren zodra het cluster de status `steady` heeft en de knooppunten `idle` zijn. Bijvoorbeeld:

```
Name        Resource Group    Workspace    VM Size       State      Idle    Running    Preparing    Leaving    Unusable
----------  ----------------  -----------  ------------  -------  ------  ---------  -----------  ---------  ----------
nc6cluster  batchai.horovod  batchaidev   STANDARD_NC6  steady        4          0            0          0           0
```

## <a name="set-up-storage"></a>Opslag instellen

Gebruik de `az storage account create`-opdracht om een opslagaccount voor uw trainingsscript en de uitvoer van de training te maken.

```azurecli-interactive
az storage account create --resource-group batchai.horovod --name mystorageaccount --location eastus --sku Standard_LRS
```

Maak een Azure-bestandsshare met de naam `myshare` in het account met behulp van de `az storage share create`-opdracht:

```azurecli-interactive
az storage share create --name myshare --account-name mystorageaccount
```

In de praktijk kan dezelfde opslag worden gebruikt voor meerdere taken en experimenten. Om alles overzichtelijk te houden, maakt u een map in de bestandsshare voor het opslaan van bestanden met betrekking tot dit specifieke experiment. Met de volgende `az storage directory create`-opdracht maakt u een map met de naam `cifar`.

```azurecli-interactive
az storage directory create --name cifar --share-name myshare --account-name mystorageaccount
```

De volgende stap is het voorbereiden van het daadwerkelijke trainingsscript, dat u vervolgens naar de zojuist gemaakte map uploadt.

## <a name="create-the-training-script"></a>Het trainingsscript maken

Voor dit experiment voert u een Python-script uit waarin enkele wijzigingen zijn aangebracht om parallel naast Horovod een objectdetectiemodel uit te voeren. Het [oorspronkelijke model](https://raw.githubusercontent.com/keras-team/keras/master/examples/cifar10_cnn.py) maakt gebruik van Keras met een back-end van TensorFlow. 

Maak met behulp van uw favoriete teksteditor een bestand met de naam `cifar_cnn_distributed.py` en de volgende inhoud in een werkmap in uw shell. Wijzigingen in de oorspronkelijke broncode zijn voorzien van commenteer met het prefix `HOROVOD`.

```python
from __future__ import print_function
import keras
from keras.datasets import cifar10
from keras.preprocessing.image import ImageDataGenerator
from keras.models import Sequential
from keras.layers import Dense, Dropout, Activation, Flatten
from keras.layers import Conv2D, MaxPooling2D
import tensorflow as tf
import horovod.keras as hvd
import os
from keras import backend as K
import math
import argparse 

# HOROVOD: initialize Horovod.
hvd.init()

# HOROVOD: pin GPU to be used to process local rank (one GPU per process)
config = tf.ConfigProto()
config.gpu_options.allow_growth = True
config.gpu_options.visible_device_list = str(hvd.local_rank())
K.set_session(tf.Session(config=config))

batch_size = 32
num_classes = 10
# HOROVOD: adjust number of epochs based on number of GPUs.
epochs = int(math.ceil(100.0 / hvd.size()))

data_augmentation = True
num_predictions = 20
# BATCH AI: change save directory to mounted storage path
parser = argparse.ArgumentParser()
parser.add_argument("-d", "--dir", help="directory to save model to")
args = parser.parse_args()
save_dir = os.path.join(args.dir, 'saved_models')
model_name = 'keras_cifar10_trained_model.h5'

# The data, split between train and test sets:
(x_train, y_train), (x_test, y_test) = cifar10.load_data()
print('x_train shape:', x_train.shape)
print(x_train.shape[0], 'train samples')
print(x_test.shape[0], 'test samples')

# Convert class vectors to binary class matrices.
y_train = keras.utils.to_categorical(y_train, num_classes)
y_test = keras.utils.to_categorical(y_test, num_classes)

model = Sequential()
model.add(Conv2D(32, (3, 3), padding='same',
                 input_shape=x_train.shape[1:]))
model.add(Activation('relu'))
model.add(Conv2D(32, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Conv2D(64, (3, 3), padding='same'))
model.add(Activation('relu'))
model.add(Conv2D(64, (3, 3)))
model.add(Activation('relu'))
model.add(MaxPooling2D(pool_size=(2, 2)))
model.add(Dropout(0.25))

model.add(Flatten())
model.add(Dense(512))
model.add(Activation('relu'))
model.add(Dropout(0.5))
model.add(Dense(num_classes))
model.add(Activation('softmax'))

# HOROVOD: adjust learning rate based on number of GPUs.
opt = keras.optimizers.rmsprop(lr=0.0001 * hvd.size(), decay=1e-6)

# HOROVOD: add Horovod Distributed Optimizer.
opt = hvd.DistributedOptimizer(opt)

# Let's train the model using RMSprop
model.compile(loss='categorical_crossentropy',
              optimizer=opt,
              metrics=['accuracy'])

callbacks = [
    # HOROVOD: broadcast initial variable states from rank 0 to all other processes.
    # This is necessary to ensure consistent initialization of all workers when
    # training is started with random weights or restored from a checkpoint.
    hvd.callbacks.BroadcastGlobalVariablesCallback(0),
]

# HOROVOD: save checkpoints only on worker 0 to prevent other workers from corrupting them.
if hvd.rank() == 0:
    callbacks.append(keras.callbacks.ModelCheckpoint('./checkpoint-{epoch}.h5'))

x_train = x_train.astype('float32')
x_test = x_test.astype('float32')
x_train /= 255
x_test /= 255

if not data_augmentation:
    print('Not using data augmentation.')
    model.fit(x_train, y_train,
              batch_size=batch_size,
              epochs=epochs,
              validation_data=(x_test, y_test),
              shuffle=True)
else:
    print('Using real-time data augmentation.')
    # This will do preprocessing and realtime data augmentation:
    datagen = ImageDataGenerator(
        featurewise_center=False,  # set input mean to 0 over the dataset
        samplewise_center=False,  # set each sample mean to 0
        featurewise_std_normalization=False,  # divide inputs by std of the dataset
        samplewise_std_normalization=False,  # divide each input by its std
        zca_whitening=False,  # apply ZCA whitening
        zca_epsilon=1e-06,  # epsilon for ZCA whitening
        rotation_range=0,  # randomly rotate images in the range (degrees, 0 to 180)
        width_shift_range=0.1,  # randomly shift images horizontally (fraction of total width)
        height_shift_range=0.1,  # randomly shift images vertically (fraction of total height)
        shear_range=0.,  # set range for random shear
        zoom_range=0.,  # set range for random zoom
        channel_shift_range=0.,  # set range for random channel shifts
        fill_mode='nearest',  # set mode for filling points outside the input boundaries
        cval=0.,  # value used for fill_mode = "constant"
        horizontal_flip=True,  # randomly flip images
        vertical_flip=False,  # randomly flip images
        rescale=None,  # set rescaling factor (applied before any other transformation)
        preprocessing_function=None,  # set function that will be applied on each input
        data_format=None,  # image data format, either "channels_first" or "channels_last"
        validation_split=0.0)  # fraction of images reserved for validation (strictly between 0 and 1)

    # Compute quantities required for feature-wise normalization
    # (std, mean, and principal components if ZCA whitening is applied).
    datagen.fit(x_train)

    # Fit the model on the batches generated by datagen.flow().
    model.fit_generator(datagen.flow(x_train, y_train,
                                     batch_size=batch_size),
                        epochs=epochs,
                        validation_data=(x_test, y_test),
                        workers=4)

# Save model and weights
if not os.path.isdir(save_dir):
    os.makedirs(save_dir)
model_path = os.path.join(save_dir, model_name)
model.save(model_path)
print('Saved trained model at %s ' % model_path)

# Score trained model.
scores = model.evaluate(x_test, y_test, verbose=1)
print('Test loss:', scores[0])
print('Test accuracy:', scores[1])
```

Zoals dit voorbeeld laat zien, zijn er slechts enkele wijzigingen in het model nodig om gedistribueerde training met behulp van het Horovod-framework mogelijk te maken. 

Houd er rekening mee dat voor deze demo en dit script slechts een relatief klein model en een relatief kleine gegevensset worden gebruikt, waardoor dit gedistribueerde model niet per se een aanzienlijke prestatieverbetering oplevert. De ware kracht van gedistribueerde training wordt pas duidelijk als u een veel groter model en een grotere gegevensset gebruikt.

## <a name="upload-the-training-script"></a>Het trainingsscript uploaden

Zodra het script gereed is, moet u het uploaden naar de eerder gemaakte map in de bestandsshare. Met de volgende `az storage file upload`-opdracht wordt het script vanuit de lokale werkmap geüpload naar de juiste locatie.

```azurecli-interactive
az storage file upload --path cifar --share-name myshare --source cifar_cnn_distributed.py --account-name mystorageaccount
```

## <a name="submit-the-training-job"></a>De trainingstaak verzenden

Nadat u de bovenstaande stappen hebt voltooid, maakt u een trainingstaak. In Batch AI gebruikt u een `job.json`-bestand om de parameters voor het uitvoeren van een taak te definiëren. Maak met behulp van uw favoriete teksteditor een taakconfiguratiebestand met de naam `job.json` met de volgende inhoud.

```json
{
    "$schema": "https://raw.githubusercontent.com/Azure/BatchAI/master/schemas/2018-05-01/job.json",
    "properties": {
        "nodeCount": 4,
        "horovodSettings": {
            "pythonScriptFilePath": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar/cifar_cnn_distributed.py",
            "commandLineArgs": "--dir=$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar"
        },
        "stdOutErrPathPrefix": "$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare/cifar",
        "mountVolumes": {
            "azureFileShares": [
                {
                    "azureFileUrl": "https://<AZURE_BATCHAI_STORAGE_ACCOUNT>.file.core.windows.net/myshare",
                    "relativeMountPath": "myshare"
                }
            ]
        },
        "jobPreparation": {
            "commandLine": "apt update; apt install mpi-default-dev mpi-default-bin -y; pip install keras horovod"
        },
        "containerSettings": {
            "imageSourceRegistry": {
                "image": "tensorflow/tensorflow:1.8.0-gpu"
            }
        }
    }
}
```

Uitleg van eigenschappen:

| Eigenschap | Beschrijving |
| --------- | --------- |
| `nodeCount` | Het aantal knooppunten dat u wilt reserveren voor de taak. Hier wordt de taak parallel uitgevoerd op `4` knooppunten. |
| `horovodSettings` | In het veld `pythonScriptFilePath` definieert u het pad naar het Horovod-script. Dit script bevindt zich in de eerder gemaakte map `cifar`. In het veld `commandLineArgs` geeft u de opdrachtregelargumenten voor de uitvoering van het script op. Het enige vereiste argument voor dit experiment is de map waarin het model moet worden opgeslagen. `$AZ_BATCHAI_JOB_MOUNT_ROOT/myshare` is het pad waar de bestandsshare is gekoppeld. | 
| `stdOutErrPathPrefix` | Het pad voor het opslaan van de uitvoer en logboeken van de taak. In dit voorbeeld is dat dezelfde map: `cifar`. |
| `jobPreparation` | Eventuele speciale instructies om de omgeving voor te bereiden voor het uitvoeren van de taak. Voor dit script moeten de aangegeven MPI- en Horovod-pakketten zijn geïnstalleerd. |
| `containerSettings` | Instellingen voor de container waarop de taak wordt uitgevoerd. Voor deze taak wordt een Docker-container gebruikt die is gemaakt met `tensorflow`.

Maak de taak met behulp van de configuratie en de `az batchai job create`-opdracht. Met de volgende opdracht wordt een nieuwe taak met de naam `cifar_distributed` in de wachtrij geplaatst, waarbij gebruik wordt gemaakt van alle resources die tot nu zijn ingesteld. 

```azurecli-interactive
az batchai job create --cluster nc6cluster --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --experiment cifar --config-file job.json --storage-account-name mystorageaccount
```

Als de knooppunten momenteel in gebruik zijn, kan het enige tijd duren voordat de taak wordt uitgevoerd. Gebruik de `az batchai job show`-opdracht om de uitvoeringsstatus van de taak weer te geven.

```azurecli-interactive
az batchai job show --experiment cifar --name cifar_distributed --resource-group batchai.horovod --workspace batchaidev --query "executionState"
```

### <a name="visualize-the-distributed-training"></a>De gedistribueerde training visualiseren

Zodra de taak wordt uitgevoerd, gebruikt u de `az batchai cluster show`-opdracht opnieuw om de status van de clusterknooppunten weer te geven. 

```azurecli-interactive
az batchai cluster show --name nc6cluster --workspace batchaidev --resource-group batchai.horovod --query "nodeStateCounts"
```

De uitvoer ziet er ongeveer als volgt uit, waarbij alle vier de knooppunten de status Actief hebben. Dit resultaat geeft aan dat alle vier de knooppunten momenteel worden gebruikt in de gedistribueerde training.

```
{
  "idleNodeCount": 0,
  "leavingNodeCount": 0,
  "preparingNodeCount": 0,
  "runningNodeCount": 4,
  "unusableNodeCount": 0
}
```

## <a name="monitor-the-job"></a>De taak bewaken

### <a name="list-output-files"></a>Uitvoerbestanden weergeven

Gebruik, terwijl de taak wordt uitgevoerd, de `az batchai job file list`-opdracht om een lijst met de gegenereerde uitvoerbestanden weer te geven.

```azurecli-interactive
az batchai job file list --experiment cifar --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev --output table
```

Voor dit specifieke experiment ziet de uitvoer er ongeveer als volgt uit. De algemene uitvoer voor de taak wordt vastgelegd in `stdout.txt`. Eventuele fouten bij uitvoering van de hoofdtaak worden vastgelegd in `stderr.txt`. De overige bestanden zijn de uitvoer-, fout- en taakvoorbereidingslogboeken van elk afzonderlijk knooppunt.

```
Name                                                    Type       Size  Modified
------------------------------------------------------  ------  -------  -------------------------
execution-tvm-676767296_1-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_2-20180718t174802z-p.log        file      15094  2018-07-18T22:41:55+00:00
execution-tvm-676767296_3-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
execution-tvm-676767296_4-20180718t174802z-p.log        file       8801  2018-07-18T22:41:28+00:00
stderr-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file        238  2018-07-18T22:41:50+00:00
stderr.txt                                              file       7653  2018-07-18T22:46:32+00:00
stdout-job_prep-tvm-676767296_1-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout-job_prep-tvm-676767296_2-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_3-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:54+00:00
stdout-job_prep-tvm-676767296_4-20180718t174802z-p.txt  file      13651  2018-07-18T22:41:55+00:00
stdout.txt                                              file    2316480  2018-07-18T22:46:32+00:00
```

### <a name="stream-an-output-file"></a>Een uitvoerbestand streamen

Gebruik de `az batchai job file stream`-opdracht voor het streamen van de inhoud van een bestand. In het volgende voorbeeld wordt het hoofduitvoeringslogboek gestreamd.

```azurecli-interactive
az batchai job file stream --experiment cifar --file-name stdout.txt --job cifar_distributed --resource-group batchai.horovod --workspace batchaidev
```

Met deze opdracht wordt, terwijl de taak wordt uitgevoerd, de standaarduitvoer van de trainingstaak gestreamd. De uitvoer ziet er ongeveer als volgt uit.

```
...
50000 train samples
10000 test samples
Using real-time data augmentation.
Epoch 1/25


   1/1563 [..............................] - ETA: 2:42:25 - loss: 2.3334 - acc: 0.0312   1/1563 [..............................] - ETA: 2:30:42 - loss: 2.2973 - acc: 0.0938
   1/1563 [..............................] - ETA: 30:36 - loss: 2.3175 - acc: 0.1250
   1/1563 [..............................] - ETA: 2:32:58 - loss: 2.3489 - acc: 0.0625
   2/1563 [..............................] - ETA: 1:21:59 - loss: 2.3230 - acc: 0.0625

   2/1563 [..............................]   2/1563 [..............................] - ETA: 1:16:09 - loss: 2.2913 - acc: 0.0938 - ETA: 1:17:15 - loss: 2.3147 - acc: 0.0781
   2/1563 [..............................] - ETA: 16:07 - loss: 2.3678 - acc: 0.0938
   3/1563 [..............................] - ETA: 55:05 - loss: 2.3232 - acc: 0.0938  
   3/1563 [..............................] - ETA: 51:57 - loss: 2.3185 - acc: 0.1146  
   3/1563 [..............................] - ETA: 51:12 - loss: 2.3179 - acc: 0.1042  
   3/1563 [..............................] - ETA: 11:13 - loss: 2.3504 - acc: 0.0833
   4/1563 [..............................] - ETA: 39:43 - loss: 2.3224 - acc: 0.1094
   4/1563 [..............................] - ETA: 42:09 - loss: 2.3049 - acc: 0.1250
   4/1563 [..............................] - ETA: 39:15 - loss: 2.3089 - acc: 0.1094
   4/1563 [..............................] - ETA: 9:16 - loss: 2.3316 - acc: 0.1016 
   5/1563 [..............................] - ETA: 39:51 - loss: 2.3153 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:58 - loss: 2.3197 - acc: 0.1125
   5/1563 [..............................] - ETA: 37:35 - loss: 2.3148 - acc: 0.1062
   5/1563 [..............................] - ETA: 13:38 - loss: 2.3263 - acc: 0.1062
   6/1563 [..............................] - ETA: 35:48 - loss: 2.3168 - acc: 0.1198

   6/1563 [..............................]   6/1563 [..............................] - ETA: 34:13 - loss: 2.3142 - acc: 0.1198 - ETA: 33:51 - loss: 2.3162 - acc: 0.1042
   6/1563 [..............................] - ETA: 13:54 - loss: 2.3225 - acc: 0.1094
   7/1563 [..............................] - ETA: 30:53 - loss: 2.3181 - acc: 0.1071

   7/1563 [..............................]   7/1563 [..............................] - ETA: 29:32 - loss: 2.3149 - acc: 0.1161 - ETA: 29:13 - loss: 2.3140 - acc: 0.0938
   7/1563 [..............................] - ETA: 12:09 - loss: 2.3174 - acc: 0.1205
   8/1563 [..............................] - ETA: 26:04 - loss: 2.3113 - acc: 0.1133
   8/1563 [..............................] - ETA: 27:15 - loss: 2.3169 - acc: 0.1133
   8/1563 [..............................] - ETA: 10:51 - loss: 2.3152 - acc: 0.1172
...
```

Via het script wordt het model in 25 tijdvakken getraind of de gegevensset voor de training doorlopen. Dit proces duurt ongeveer 60 minuten. 

## <a name="retrieve-the-results"></a>De resultaten ophalen

Wanneer het script is voltooid en alles goed is gegaan, is de nauwkeurigheid van de validatie ongeveer 70-75% en is het getrainde model opgeslagen in de bestandsshare op `cifar/saved_models/keras_cifar10_trained_model.h5`. 

De training van het model maakt gewoonlijk deel uit van een grotere werkstroom. U kunt het getrainde model bijvoorbeeld in een andere toepassing beschikbaar maken. Als u het getrainde model lokaal wilt downloaden, gebruikt u de `az storage file download`-opdracht. 

```azurecli-interactive
az storage file download --path cifar/saved_models/keras_cifar10_trained_model.h5 --share-name myshare --account-name mystorageaccount
```
## <a name="clean-up-resources"></a>Resources opschonen

Wanneer de taken zijn uitgevoerd, is het raadzaam om alle clusters te verkleinen tot `0 nodes` zodat er geen kosten in rekening worden gebracht voor rekentijd terwijl u de clusters niet gebruikt. Gebruik de volgende `az batchai cluster resize`-opdracht. 

```azurecli-interactive
az batchai cluster resize --name nc6cluster --resource-group batchai.horovod --target 0 --workspace batchaidev
```

U kunt het formaat later wijzigen naar 1 of meer knooppunten om uw taken uit te voeren. 

Als u niet van plan bent de werkruimte en het opslagaccount in de toekomst nog te gebruiken, verwijdert u de resourcegroep met de `az group delete`-opdracht. Door een resourcegroep te verwijderen, worden alle resources van die groep verwijderd.

```azurecli-interactive
az group delete --name batchai.horovod
```

## <a name="next-steps"></a>Volgende stappen

In deze zelfstudie hebt u het volgende geleerd:

> [!div class="checklist"]
> * Een Batch AI-werkruimte, experiment en cluster instellen
> * Een Azure-bestandsshare instellen voor invoer en uitvoer
> * Een model parallelliseren met behulp van Horovod
> * Een trainingstaak verzenden
> * De taak bewaken
> * De resultaten van de training ophalen

Zie de recepten op GitHub voor voorbeelden van het gebruik van Batch AI met verschillende frameworks.

> [!div class="nextstepaction"]
> [Batch AI-recepten](https://github.com/Azure/BatchAI/tree/master/recipes)
