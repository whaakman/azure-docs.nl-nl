---
title: Met GPU Azure-containerinstanties implementeren
description: Informatie over het implementeren van Azure container instances om uit te voeren op GPU-resources.
services: container-instances
author: dlepow
manager: jeconnoc
ms.service: container-instances
ms.topic: article
ms.date: 11/29/2018
ms.author: danlep
ms.openlocfilehash: 2cbfb21469df45f29a70b5d10d8c99ecd894c30c
ms.sourcegitcommit: 7862449050a220133e5316f0030a259b1c6e3004
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/22/2018
ms.locfileid: "53755016"
---
# <a name="deploy-container-instances-that-use-gpu-resources"></a>Containerinstanties die gebruikmaken van GPU-resources implementeren

Voor het uitvoeren van bepaalde rekenintensieve workloads op Azure Container Instances, implementeert u uw [containergroepen](container-instances-container-groups.md) met *GPU resources*. De containerinstanties in de groep hebben toegang tot een of meer NVIDIA Tesla-GPU's tijdens het uitvoeren van containerworkloads zoals CUDA- en deep learning-toepassingen.

Zoals u in dit artikel, kunt u GPU resources toevoegen wanneer u een containergroep met behulp van implementeert een [YAML-bestand](container-instances-multi-container-yaml.md) of [Resource Manager-sjabloon](container-instances-multi-container-group.md).

> [!IMPORTANT]
> Deze functie is momenteel in preview en sommige [gelden beperkingen](#preview-limitations). Preview-versies worden beschikbaar gesteld op voorwaarde dat u akkoord gaat met de [aanvullende gebruiksvoorwaarden][terms-of-use]. Sommige aspecten van deze functie worden mogelijk nog gewijzigd voordat de functie algemeen beschikbaar wordt.

## <a name="preview-limitations"></a>Beperkingen voor Preview

Preview-versie gelden de volgende beperkingen bij het gebruik van GPU-bronnen in groepen met containers. 

**Ondersteunde regio's**:

* VS-Oost (VS-Oost)
* VS-West 2 (westus2)
* Zuid-centraal VS (southcentralus)
* West-Europa (Europa West)
* Noord-Europa (northeurope)
* Oost-Azië (Oost-Aziatische)
* Centraal-India (centralindia)

Ondersteuning wordt gedurende een periode voor extra regio's toegevoegd.

**Ondersteunde typen besturingssystemen**: Alleen voor Linux

**Aanvullende beperkingen**: GPU-resources kunnen niet worden gebruikt bij het implementeren van een containergroep in een [virtueel netwerk](container-instances-vnet.md).

## <a name="about-gpu-resources"></a>Over GPU-resources

### <a name="count-and-sku"></a>Aantal en de SKU

Geef voor het gebruik van GPU's in een containerexemplaar, een *GPU resource* met de volgende informatie:

* **Aantal** -het aantal GPU's: **1**, **2**, of **4**.
* **SKU** -de GPU-SKU: **K80**, **P100**, of **V100**. Elke SKU wordt toegewezen aan de GPU NVIDIA Tesla in een de volgende Azure-met GPU VM-families:

  | SKU | VM-serie |
  | --- | --- |
  | K80 | [NC](../virtual-machines/linux/sizes-gpu.md#nc-series) |
  | P100 | [NCv2](../virtual-machines/linux/sizes-gpu.md#ncv2-series) |
  | V100 | [NCv3](../virtual-machines/linux/sizes-gpu.md#ncv3-series) |

### <a name="cpu-and-memory"></a>CPU en geheugen

Bij het implementeren van resources voor GPU, stelt u CPU en geheugenbronnen geschikt is voor de werkbelasting, tot de maximale waarden in de volgende tabel weergegeven. Deze waarden zijn momenteel groter is dan de CPU en geheugenlimieten in containerexemplaren zonder GPU-resources.  

| GPU-SKU | GPU-aantal | CPU |  Geheugen (GB) |
| --- | --- | --- | --- |
| K80 | 1 | 6 | 56 |
| K80 | 2 | 12 | 112 |
| K80 | 4 | 24 | 224 |
| P100 | 1 | 6 | 112 |
| P100 | 2 | 12 | 224 |
| P100 | 4 | 24 | 448 |
| V100 | 1 | 6 | 112 |
| V100 | 2 | 12 | 224 |
| V100 | 4 | 24 | 448 |

### <a name="things-to-know"></a>Dingen die u moet weten

* **Implementatietijd** -het maken van een containergroep met GPU-resources duurt maximaal **8-10 minuten**. Dit komt door de extra tijd voor het inrichten en configureert u een GPU-VM in Azure. 

* **Prijzen** - vergelijkbaar met containergroepen zonder GPU-resources, Azure facturen voor de resources die worden gebruikt via de *duur* van de containergroep van een met GPU-resources. De duur wordt berekend op basis van de tijd voor het ophalen van uw eerste containerinstallatiekopie totdat de containergroep wordt beëindigd. Deze omvatten niet de tijd voor het implementeren van de containergroep.

  Prijzen zijn voor containergroepen met GPU-bronnen dan voor containergroepen zonder hoger. Zie [prijsinformatie](https://azure.microsoft.com/pricing/details/container-instances/).

* **CUDA-stuurprogramma's** - Container instances met GPU-resources zijn al ingericht met NVIDIA CUDA-stuurprogramma's en verbeteren van de container, zodat u containerinstallatiekopieën kunt gebruiken die is ontwikkeld voor CUDA-werkbelastingen.

## <a name="yaml-example"></a>YAML-voorbeeld

Een manier om toe te voegen GPU-resources is het een containergroep implementeren met behulp van een [YAML-bestand](container-instances-multi-container-yaml.md). De volgende YAML kopiëren naar een nieuw bestand met de naam *gpu-implementeren-aci.yaml*, sla het bestand. Deze YAML wordt gemaakt van de containergroep van een met de naam *gpucontainergroup* een containerinstantie met een K80-GPU op te geven. Het exemplaar wordt uitgevoerd een voorbeeldtoepassing CUDA vector toevoegen. De resourceaanvragen zijn voldoende zijn om uit te voeren van de werkbelasting.

```YAML
additional_properties: {}
apiVersion: '2018-10-01'
name: gpucontainergroup
properties:
  containers:
  - name: gpucontainer
    properties:
      image: k8s-gcrio.azureedge.net/cuda-vector-add:v0.1
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
          gpu:
            count: 1
            sku: K80
  osType: Linux
  restartPolicy: OnFailure
```

Implementeren van de containergroep met de [az container maken] [ az-container-create] opdracht op te geven de naam van de YAML-bestand voor de `--file` parameter. Moet u opgeven de naam van een resourcegroep en een locatie voor de containergroep zoals *eastus* die ondersteuning biedt voor GPU-resources.  

```azurecli
az container create --resource-group myResourceGroup --file gpu-deploy-aci.yaml --location eastus
```

Het duurt enkele minuten om de implementatie te voltooien. Vervolgens wordt de container wordt gestart en wordt uitgevoerd een optelbewerking CUDA-vector. Voer de [az container logs] [ az-container-logs] opdracht om de uitvoer weer te geven:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergroup --container-name gpucontainer
```

Uitvoer:

```Console
[Vector addition of 50000 elements]
Copy input data from the host memory to the CUDA device
CUDA kernel launch with 196 blocks of 256 threads
Copy output data from the CUDA device to the host memory
Test PASSED
Done
```

## <a name="resource-manager-template-example"></a>Voorbeeld van de Resource Manager-sjabloon

Een andere manier om een containergroep met GPU-resources implementeren met behulp van is een [Resource Manager-sjabloon](container-instances-multi-container-group.md). Beginnen met het maken van een bestand met de naam `gpudeploy.json`, kopieer vervolgens de volgende JSON naar het. In dit voorbeeld implementeert een containerinstantie met een V100 GPU die compatibel is met een [TensorFlow](https://www.tensorflow.org/versions/r1.1/get_started/mnist/beginners) trainingstaak op basis van de [MNIST gegevensset](http://yann.lecun.com/exdb/mnist/). De resourceaanvragen zijn voldoende zijn om uit te voeren van de werkbelasting.

```JSON
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "containerGroupName": {
        "type": "string",
        "defaultValue": "gpucontainergrouprm",
        "metadata": {
          "description": "Container Group name."
        }
      }
    },
    "variables": {
      "containername": "gpucontainer",
      "containerimage": "microsoft/samples-tf-mnist-demo:gpu"
    },
    "resources": [
      {
        "name": "[parameters('containerGroupName')]",
        "type": "Microsoft.ContainerInstance/containerGroups",
        "apiVersion": "2018-10-01",
        "location": "[resourceGroup().location]",
        "properties": {
            "containers": [
            {
              "name": "[variables('containername')]",
              "properties": {
                "image": "[variables('containerimage')]",
                "resources": {
                  "requests": {
                    "cpu": 4.0,
                    "memoryInGb": 12.0,
                    "gpu": {
                        "count": 1,
                        "sku": "V100"
                  }
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "restartPolicy": "OnFailure"
        }
      }
    ]
}
```

Implementeer de sjabloon met de [az group deployment maken] [ az-group-deployment-create] opdracht. Moet u opgeven de naam van een resourcegroep die is gemaakt in een regio zoals *eastus* die ondersteuning biedt voor GPU-resources.

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file gpudeploy.json
```

Het duurt enkele minuten om de implementatie te voltooien. Vervolgens wordt de container wordt gestart en de TensorFlow-taak wordt uitgevoerd. Voer de [az container logs] [ az-container-logs] opdracht om de uitvoer weer te geven:

```azurecli
az container logs --resource-group myResourceGroup --name gpucontainergroup --container-name gpucontainer
```

Uitvoer:

```Console
2018-10-25 18:31:10.155010: I tensorflow/core/platform/cpu_feature_guard.cc:137] Your CPU supports instructions that this TensorFlow binary was not compiled to use: SSE4.1 SSE4.2 AVX AVX2 FMA
2018-10-25 18:31:10.305937: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1030] Found device 0 with properties:
name: Tesla K80 major: 3 minor: 7 memoryClockRate(GHz): 0.8235
pciBusID: ccb6:00:00.0
totalMemory: 11.92GiB freeMemory: 11.85GiB
2018-10-25 18:31:10.305981: I tensorflow/core/common_runtime/gpu/gpu_device.cc:1120] Creating TensorFlow device (/device:GPU:0) -> (device: 0, name: Tesla K80, pci bus id: ccb6:00:00.0, compute capability: 3.7)
2018-10-25 18:31:14.941723: I tensorflow/stream_executor/dso_loader.cc:139] successfully opened CUDA library libcupti.so.8.0 locally
Successfully downloaded train-images-idx3-ubyte.gz 9912422 bytes.
Extracting /tmp/tensorflow/input_data/train-images-idx3-ubyte.gz
Successfully downloaded train-labels-idx1-ubyte.gz 28881 bytes.
Extracting /tmp/tensorflow/input_data/train-labels-idx1-ubyte.gz
Successfully downloaded t10k-images-idx3-ubyte.gz 1648877 bytes.
Extracting /tmp/tensorflow/input_data/t10k-images-idx3-ubyte.gz
Successfully downloaded t10k-labels-idx1-ubyte.gz 4542 bytes.
Extracting /tmp/tensorflow/input_data/t10k-labels-idx1-ubyte.gz
Accuracy at step 0: 0.097
Accuracy at step 10: 0.6993
Accuracy at step 20: 0.8208
Accuracy at step 30: 0.8594
...
Accuracy at step 990: 0.969
Adding run metadata for 999
```

## <a name="clean-up-resources"></a>Resources opschonen

Omdat met behulp van GPU-resources zijn mogelijk dure, moet u zorgen dat uw containers onverwacht wordt niet uitgevoerd gedurende lange perioden. Bewaken van uw containers in Azure portal of Controleer de status van de containergroep van een met de [az container show] [ az-container-show] opdracht. Bijvoorbeeld:

```azurecli
az container show --resource-group myResourceGroup --name gpucontainergroup --output table
```

Wanneer u klaar bent met de containerinstanties werkt u hebt gemaakt, verwijdert deze met de volgende opdrachten:

```azurecli
az container delete --resource-group myResourceGroup --name gpucontainergroup -y
az container delete --resource-group myResourceGroup --name gpucontainergrouprm -y
```

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het implementeren van een container groep met een [YAML-bestand](container-instances-multi-container-yaml.md) of [Resource Manager-sjabloon](container-instances-multi-container-group.md).
* Meer informatie over [GPU VM-grootten geoptimaliseerd](../virtual-machines/linux/sizes-gpu.md) in Azure.


<!-- IMAGES -->
[aci-vnet-01]: ./media/container-instances-vnet/aci-vnet-01.png

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create
[az-container-show]: /cli/azure/container#az-container-show
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
