---
title: Over het gebruik van GPU voor Azure Machine Learning | Microsoft Docs
description: In dit artikel wordt beschreven hoe u grafische verwerking van eenheden (GPU) gebruiken om te trainen deep neural networks in Azure Machine Learning Workbench.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.component: core
ms.workload: data-services
ms.topic: article
ms.date: 09/14/2017
ROBOTS: NOINDEX
ms.openlocfilehash: 8e8944a9bdfa2b75f6442e03d45dfb6977c2a122
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46974354"
---
# <a name="how-to-use-gpu-in-azure-machine-learning"></a>Over het gebruik van GPU in Azure Machine Learning

[!INCLUDE [workbench-deprecated](../../../includes/aml-deprecating-preview-2017.md)]

Grafische verwerking Unit (GPU) wordt veel gebruikt voor het verwerken van rekenintensief zijn taken die doorgaans kunnen ontstaan bij het trainen van bepaalde deep neural network-modellen. Met behulp van GPU's, kunt u de trainingstijd van de modellen aanzienlijk verkorten. In dit document leert u hoe het configureren van Azure ML Workbench gebruiken [DSVM (Data Science Virtual Machine)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) uitgerust met GPU's als doel voor uitvoering. 

## <a name="prerequisites"></a>Vereisten
- Als u wilt in deze gebruiksaanwijzing kunt doorlopen, moet u eerst [Azure ML Workbench installeren](quickstart-installation.md).
- U moet toegang hebben tot computers die zijn voorzien van NVidia GPU's.
    - Rechtstreeks op de lokale computer (Windows of Mac OS) kunt u uw scripts uitvoeren met GPU's.
    - U kunt ook scripts uitvoeren in een Docker-container op een Linux-machine met GPU's...

## <a name="execute-in-local-environment-with-gpus"></a>Uitvoeren in _lokale_ omgeving met GPU's
U kunt Azure ML Workbench installeren op een computer is uitgerust met GPU's en uitvoeren op basis van _lokale_ omgeving. Dit kan zijn:
- Een fysieke Windows- of macOS-computer.
- Een Windows-VM (virtuele Machine), zoals een Windows-DSVM inrichten met behulp van Azure NC-serie VM's-sjabloon.

In dit geval zijn er geen speciale configuratie vereist in Azure ML Workbench. Zorg ervoor dat u hebt alle benodigde stuurprogramma's, toolkits en met GPU machine learning-bibliotheken die lokaal is geïnstalleerd. Alleen uitvoeren op basis van _lokale_ omgeving waar de Python-runtime rechtstreeks toegang de lokale GPU-hardware tot.

1. Open AML-Workbench. Ga naar **bestand** en **Open Command Prompt**. 
2. Installeren vanaf de opdrachtregel, deep learning met GPU framework, zoals de Microsoft Cognitive Toolkit, TensorFlow en enzovoort. Bijvoorbeeld:

```batch
REM install latest TensorFlow with GPU support
C:\MyProj> pip install tensorflow-gpu

REM install Microsoft Cognitive Toolkit 2.5 with GPU support on Windows
C:\MyProj> pip install https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.5.1-cp35-cp35m-win_amd64.whl
```

3. Python-code die gebruikmaakt van de deep learning-bibliotheken schrijven.
4. Kies _lokale_ compute-omgeving en de Python-code worden uitgevoerd.

```batch
REM execute Python script in local environment
C:\MyProj> az ml experiment submit -c local my-deep-learning-script.py
```

## <a name="execute-in-docker-environment-on-linux-vm-with-gpus"></a>Uitvoeren in _docker_ omgeving op Linux-VM met GPU's
Azure ML Workbench bieden ook ondersteuning voor uitvoering in Docker in een Azure Linux-VM. U hebt hier een goede optie voor rekenintensieve taken uitvoeren op een stukje krachtige virtuele hardware en betaal alleen voor het gebruik door het uitschakelen van wanneer u klaar bent. In principe is het mogelijk te gebruiken van GPU's op elke Linux-machine, de Ubuntu gebaseerde DSVM wordt geleverd met de vereiste CUDA-stuurprogramma's en bibliotheken die vooraf zijn geïnstalleerd, waardoor de opzet veel gemakkelijker wordt. Volg de onderstaande stappen te volgen:

### <a name="create-a-ubuntu-based-linux-data-science-virtual-machine-in-azure"></a>Maken van een op basis van een Ubuntu Linux Data Science Virtual Machine in Azure
1. Open uw webbrowser en Ga naar de [Azure-portal](https://portal.azure.com)

2. Selecteer **+ nieuw** aan de linkerkant van de portal.

3. Zoeken naar 'Data Science Virtual Machine voor Linux (Ubuntu)' in de marketplace.

4. Klik op **maken** te maken van een Ubuntu-DSVM.

5. Vul de **basisbeginselen** formulier met de vereiste informatie.
Bij het selecteren van de locatie voor uw virtuele machine, houd er rekening mee dat GPU VM's alleen beschikbaar in bepaalde Azure-regio's, bijvoorbeeld zijn **Zuid-centraal VS**. Zie [compute producten beschikbaar per regio](https://azure.microsoft.com/regions/services/).
Klik op OK om op te slaan de **basisbeginselen** informatie.

6. Kies de grootte van de virtuele machine. Selecteer een van de grootte met NC voorafgegaan VM's die zijn uitgerust met NVidia GPU-chips.  Klik op **Alles weergeven** om te zien van de volledige lijst zo nodig. Meer informatie over [GPU uitgerust virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

7. De overige instellingen voltooien en te bekijken van de gegevens. Klik op de aankoop voor het maken van de virtuele machine. Noteer het IP-adres toegewezen aan de virtuele machine. 

### <a name="create-a-new-project-in-azure-ml-workbench"></a>Een nieuw Project maken in Azure ML Workbench 
Kunt u de _classificeren MNIST met behulp van TensorFlow_ bijvoorbeeld of de _MNIST classificeren gegevensset met CNTK_ voorbeeld.

### <a name="create-a-new-compute-target"></a>Maak een nieuwe Compute-doel
De opdrachtregel van Azure ML Workbench start. Voer de volgende opdracht. Vervang de tijdelijke aanduiding voor tekst van het onderstaande voorbeeld door uw eigen waarden voor de naam, IP-adres, gebruikersnaam en wachtwoord. 

```batch
C:\MyProj> az ml computetarget attach remotedocker --name "my_dsvm" --address "my_dsvm_ip_address" --username "my_name" --password "my_password" 
```

### <a name="configure-azure-ml-workbench-to-access-gpu"></a>Azure ML Workbench voor toegang tot GPU configureren
Ga terug naar het project en open **bestandsweergave**, en klik op de **vernieuwen** knop. Nu u twee nieuwe configuratiebestanden ziet `my_dsvm.compute` en `my_dsvm.runconfig`.
 
Open de `my_dsvm.compute`. Wijzig de `baseDockerImage` naar `microsoft/mmlspark:plus-gpu-0.9.9` en voeg een nieuwe regel toe `nvidiaDocker: true`. Het bestand hebt zodat deze twee regels:
 
```yaml
...
baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9
nvidiaDocker: true
```
 
Open nu `my_dsvm.runconfig`, wijzigen `Framework` waarde uit de `PySpark` naar `Python`. Als u deze regel niet ziet, toevoegen, omdat de standaardwaarde is `PySpark`.

```yaml
"Framework": "Python"
```
### <a name="reference-deep-learning-framework"></a>Naslaginformatie over Deep Learning Framework 
Open `conda_dependencies.yml` bestand en zorg ervoor dat u de GPU-versie van de deep learning framework Python-pakketten. De voorbeeldprojecten lijst versies van de CPU, dus moet u deze wijziging hebt aangebracht.

Voorbeeld voor TensorFlow: 
```
name: project_environment
dependencies:
  - python=3.5.2
  # latest TensorFlow library with GPU support
  - tensorflow-gpu
```

Voorbeeld voor Microsoft Cognitive Toolkit:
```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip: 
    # use the Linux build of Microsoft Cognitive Toolkit 2.5 with GPU support
    - https://cntk.ai/PythonWheel/GPU/cntk_gpu-2.5.1-cp35-cp35m-win_amd64.whl
```

### <a name="execute"></a>Uitvoeren
U bent nu klaar om uit te voeren van uw Python-scripts. U kunt ze uitvoeren binnen de Azure ML Workbench met behulp van de `my_dsvm` context, of u kunt starten vanaf de opdrachtregel:
 
```batch
C:\myProj> az ml experiment submit -c my_dsvm my_tensorflow_or_cntk_script.py
```
 
Om te controleren dat de GPU wordt gebruikt, Controleer de uitvoer uitvoeren om te zien er ongeveer als volgt uit:

```
name: Tesla K80
major: 3 minor: 7 memoryClockRate (GHz) 0.8235
pciBusID 06c3:00:00.0
Total memory: 11.17GiB
Free memory: 11.11GiB
```

Gefeliciteerd! Het script om alleen deze om de kracht van GPU via een Docker-container!

## <a name="next-steps"></a>Volgende stappen
Bekijk een voorbeeld van het gebruik van GPU te versnellen, deep neural network-training op Azure ML-galerie.
