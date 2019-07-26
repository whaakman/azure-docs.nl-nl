---
author: larryfr
ms.service: machine-learning
ms.topic: include
ms.date: 07/19/2019
ms.author: larryfr
ms.openlocfilehash: 03b40b1f334816f08d7e169f8ff78bdb7c06c4de
ms.sourcegitcommit: e72073911f7635cdae6b75066b0a88ce00b9053b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68348558"
---
De vermeldingen in de `inferenceconfig.json` document structuur met de para meters voor de [InferenceConfig](https://docs.microsoft.com/python/api/azureml-core/azureml.core.model.inferenceconfig?view=azure-ml-py) -klasse. De volgende tabel beschrijft de toewijzing tussen entiteiten in het JSON-document en de para meters voor de-methode:

| JSON-entiteit | Methode parameter | Description |
| ----- | ----- | ----- |
| `entryScript` | `entry_script` | Het pad naar het lokale bestand dat de code bevat die moet worden uitgevoerd voor de installatie kopie. |
| `runtime` | `runtime` | Welke runtime moet worden gebruikt voor de installatie kopie. De huidige ondersteunde Runtimes zijn ' Spark-py ' en ' python '. |
| `condaFile` | `conda_file` | Optioneel. Het pad naar het lokale bestand met een Conda-omgevings definitie die moet worden gebruikt voor de installatie kopie. |
| `extraDockerFileSteps` | `extra_docker_file_steps` | Optioneel. Pad naar lokaal bestand met extra docker-stappen die moeten worden uitgevoerd bij het instellen van de installatie kopie. |
| `sourceDirectory` | `source_directory` | Optioneel. Pad naar mappen met alle bestanden om de installatie kopie te maken. |
| `enableGpu` | `enable_gpu` | Optioneel. Hiermee wordt aangegeven of GPU-ondersteuning in de installatie kopie moet worden ingeschakeld. De GPU-installatie kopie moet worden gebruikt voor Microsoft Azure service. Bijvoorbeeld Azure Container Instances, Azure Machine Learning compute, Azure Virtual Machines en Azure Kubernetes service. De standaard waarde is False. |
| `baseImage` | `base_image` | Optioneel. Een aangepaste installatie kopie die moet worden gebruikt als basis installatie kopie. Als er geen basis installatie kopie wordt opgegeven, wordt de basis installatie kopie gebruikt op basis van de opgegeven runtime parameter. |
| `baseImageRegistry` | `base_image_registry` | Optioneel. Het REGI ster van de installatie kopie met de basis installatie kopie. |
| `cudaVersion` | `cuda_version` | Optioneel. De versie van CUDA die moet worden geïnstalleerd voor installatie kopieën waarvoor GPU-ondersteuning is vereist. De GPU-installatie kopie moet worden gebruikt op Microsoft Azure Services. Bijvoorbeeld Azure Container Instances, Azure Machine Learning compute, Azure Virtual Machines en Azure Kubernetes service. Ondersteunde versies zijn 9,0, 9,1 en 10,0. Als ' enable_gpu ' is ingesteld, wordt de standaard waarde van ' 9,1 ' gebruikt. |
| `description` | `description` |  Een beschrijving van deze afbeelding. |

De volgende JSON is een voor beeld van een configuratie voor het afgebruiken van de CLI:

```json
{
    "entryScript": "score.py",
    "runtime": "python",
    "condaFile": "myenv.yml",
    "extraDockerfileSteps": null,
    "sourceDirectory": null,
    "enableGpu": false,
    "baseImage": null,
    "baseImageRegistry": null
}
```