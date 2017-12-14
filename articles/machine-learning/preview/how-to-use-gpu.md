---
title: Het gebruik van GPU voor Azure Machine Learning | Microsoft Docs
description: In dit artikel wordt beschreven hoe grafische verwerken eenheden (GPU) gebruiken voor het trainen van diep neural netwerken in Azure Machine Learning-Workbench.
services: machine-learning
author: rastala
ms.author: roastala
manager: jhubbard
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 09/14/2017
ms.openlocfilehash: ce1557aed09384b0d7a0b65aabd473fe72ab740c
ms.sourcegitcommit: 922687d91838b77c038c68b415ab87d94729555e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/13/2017
---
# <a name="how-to-use-gpu-in-azure-machine-learning"></a>Het gebruik van GPU in Azure Machine Learning
Grafische verwerken Unit (GPU) wordt veel gebruikt voor het verwerken van rekenkracht taken die doorgaans kunnen ontstaan bij het trainen van bepaalde diepe neural network-modellen. Met behulp van GPU's, kunt u de trainingstijd van de modellen aanzienlijk verkorten. In dit document, u informatie over het configureren van Azure ML-Workbench om te gebruiken [DSVM (gegevens wetenschappelijke virtuele Machine)](https://docs.microsoft.com/azure/machine-learning/data-science-virtual-machine/overview) uitgerust met GPU's als doel kan worden uitgevoerd. 

## <a name="prerequisites"></a>Vereisten
- Om de stap in deze handleiding instructies, moet u eerst [installeren Azure ML-Workbench](quickstart-installation.md).
- U moet toegang hebben tot computers die zijn uitgerust met NVidia GPU's.
    - Rechtstreeks op de lokale computer (Windows of Mac OS) kunt u uw scripts uitvoeren met GPU's.
    - U kunt ook scripts uitvoeren in een Docker-container op een computer met een GPU's.

## <a name="execute-in-local-environment-with-gpus"></a>Uitvoeren in _lokale_ omgeving met GPU's
U kunt Azure ML-Workbench installeren op een computer is uitgerust met een GPU's en voeren op _lokale_ omgeving. Dit kan zijn:
- Een fysieke Windows- of Mac OS-machine.
- Een Windows-VM (virtuele Machine) zoals een Windows-DSVM ingericht met behulp van de sjabloon voor virtuele machines van Azure NC-serie.

In dit geval zijn er geen speciale configuratie vereist in de Azure ML-Workbench. Zorg ervoor dat u hebt de benodigde stuurprogramma's, toolkits en GPU ingeschakeld machine learning-bibliotheken die lokaal zijn geïnstalleerd. Gewoon uitgevoerd tegen _lokale_ omgeving waar de Python-runtime rechtstreeks toegang de lokale GPU-hardware tot.

1. Open AML Workbench. Ga naar **bestand** en **opdrachtprompt openen**. 
2. Installeren vanaf de opdrachtregel, GPU-functionaliteit van grondige learning framework zoals Microsoft cognitieve Toolkit, TensorFlow en enzovoort. Bijvoorbeeld:

```batch
REM install latest TensorFlow with GPU support
C:\MyProj> pip install tensorflow-gpu

REM install Microsoft Cognitive Toolkit 2.1 (1-bit SGD) with GPU support on Windows
C:\MyProj> pip install https://cntk.ai/PythonWheel/GPU-1bit-SGD/cntk-2.1-cp35-cp35m-win_amd64.whl
```

3. Python-code die gebruikmaakt van de deep bibliotheken leren schrijven.
4. Kies _lokale_ zoals omgeving berekenen en voer de Python-code.

```batch
REM execute Python script in local environment
C:\MyProj> az ml experiment submit -c local my-deep-learning-script.py
```

## <a name="execute-in-docker-environment-on-linux-vm-with-gpus"></a>Uitvoeren in _docker_ omgeving op Linux-VM met GPU's
Azure ML-Workbench bieden ook ondersteuning voor uitvoering in Docker in een Azure Linux VM. U hebt hier een geweldige optie rekenkracht taken uitvoeren op een stukje krachtige virtuele hardware en betaalt alleen voor het gebruik door het uitschakelen van wanneer u klaar bent. In principe is het mogelijk te GPU's gebruiken op een virtuele Linux-machine, de DSVM op basis van Ubuntu wordt geleverd met de vereiste CUDA stuurprogramma's en bibliotheken die vooraf zijn geïnstalleerd, waardoor de opzet veel gemakkelijker wordt. Volg de onderstaande stappen te volgen:

### <a name="create-a-ubuntu-based-linux-data-science-virtual-machine-in-azure"></a>Maken van een Linux-gegevens op basis van Ubuntu wetenschappelijke virtuele Machine in Azure
1. Open uw webbrowser en Ga naar de [Azure-portal](https://portal.azure.com)

2. Selecteer **+ nieuw** aan de linkerkant van de portal.

3. Zoek naar 'Data wetenschappelijke virtuele Machine voor Linux (Ubuntu)' in de marketplace.

4. Klik op **maken** een Ubuntu DSVM maken.

5. Vul de **basisbeginselen** formulier met de vereiste informatie.
Als u de locatie voor uw virtuele machine, houd er rekening mee dat GPU VM's alleen beschikbaar in bepaalde Azure-regio's, bijvoorbeeld zijn **Zuid-centraal VS**. Zie [producten die beschikbaar zijn in elke regio compute](https://azure.microsoft.com/regions/services/).
Klik op OK om op te slaan de **basisbeginselen** informatie.

6. Kies de grootte van de virtuele machine. Selecteer een van de grootte met NC voorafgegaan VM's die zijn uitgerust met chips NVidia GPU.  Klik op **Alles weergeven** om te zien van de volledige lijst indien nodig. Meer informatie over [GPU uitgerust Azure Virtual machines](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-gpu).

7. De overige instellingen voltooien en de aankoopgegevens controleren. Klik op inkoop voor het maken van de virtuele machine. Let op het IP-adres dat is toegewezen aan de virtuele machine. 

### <a name="create-a-new-project-in-azure-ml-workbench"></a>Maak een nieuw Project in Azure ML-Workbench 
U kunt de _classificeren MNIST TensorFlow met_ bijvoorbeeld of de _MNIST classificeren gegevensset met CNTK_ voorbeeld.

### <a name="create-a-new-compute-target"></a>Maak een nieuw Compute-doel
Start de opdrachtregel van Azure ML-Workbench. Voer de volgende opdracht. De tijdelijke aanduiding voor tekst uit het onderstaande voorbeeld vervangen door uw eigen waarden voor de naam, IP-adres, gebruikersnaam en wachtwoord. 

```batch
C:\MyProj> az ml computetarget attach remotedocker --name "my_dsvm" --address "my_dsvm_ip_address" --username "my_name" --password "my_password" 
```

### <a name="configure-azure-ml-workbench-to-access-gpu"></a>Azure ML-Workbench voor toegang tot GPU configureren
Ga terug naar het project en open **bestandsweergave**, en treffers het **vernieuwen** knop. Nu u twee nieuwe configuratiebestanden ziet `my_dsvm.compute` en `my_dsvm.runconfig`.
 
Open de `my_dsvm.compute`. Wijzig de `baseDockerImage` naar `microsoft/mmlspark:plus-gpu-0.7.9` en voeg een nieuwe regel `nvidiaDocker: true`. Het bestand moet dus hebben dat deze twee regels:
 
```yaml
...
baseDockerImage: microsoft/mmlspark:plus-gpu-0.9.9
nvidiaDocker: true
```
 
Open nu `my_dsvm.runconfig`, wijzigen `Framework` waarde uit de `PySpark` naar `Python`. Als u deze regel niet ziet, toevoegen, omdat de standaardwaarde is `PySpark`.

```yaml
"Framework": "Python"
```
### <a name="reference-deep-learning-framework"></a>Verwijzing grondige Learning Framework 
Open `conda_dependencies.yml` bestand en zorg ervoor dat u gebruikt de GPU-versie van de deep learning framework Python-pakketten. De voorbeeldprojecten lijst versies van de CPU, dus moet u deze wijziging hebt aangebracht.

Voorbeeld voor TensorFlow: 
```
name: project_environment
dependencies:
  - python=3.5.2
  # latest TensorFlow library with GPU support
  - tensorflow-gpu
```

Voorbeeld voor Microsoft cognitieve Toolkit:
```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip: 
    # use the Linux build of Microsoft Cognitive Toolkit 2.1 with GPU support
    - https://cntk.ai/PythonWheel/GPU/cntk-2.1-cp35-cp35m-linux_x86_64.whl
```

U kunt ook de 1 bits SGD-versie van de Microsoft cognitieve Toolkit waarmee prestatieverbeteringen op meerdere GPU virtuele machines gebruiken. Houd er rekening mee [de licentievereiste voor 1-bits-SGD](https://docs.microsoft.com/cognitive-toolkit/cntk-1bit-sgd-license).

```yaml
name: project_environment
dependencies:
  - python=3.5.2
  - pip:    
    # use the Linux build of the Microsoft Cognitive Toolkit 2.1 with 1-bit SGD and GPU support
    - https://cntk.ai/PythonWheel/GPU-1bit-SGD/cntk-2.1-cp35-cp35m-linux_x86_64.whl
```

### <a name="execute"></a>Uitvoeren
U bent nu klaar om uit te voeren van uw Python-scripts. U kunt ze uitvoeren binnen de Azure ML-Workbench door middel de `my_dsvm` context, of u deze kunt starten vanaf de opdrachtregel:
 
```batch
C:\myProj> az ml experiment submit -c my_dsvm my_tensorflow_or_cntk_script.py
```
 
Om te bevestigen dat de GPU wordt gebruikt, controleert u de uitvoer van de uitvoeren als u wilt weergegeven dat lijkt op het volgende:

```
name: Tesla K80
major: 3 minor: 7 memoryClockRate (GHz) 0.8235
pciBusID 06c3:00:00.0
Total memory: 11.17GiB
Free memory: 11.11GiB
```

Gefeliciteerd. Uw script om alleen deze om de kracht van GPU via een Docker-container!

## <a name="next-steps"></a>Volgende stappen
Bekijk een voorbeeld van het gebruik van GPU te diep neurale netwerk trainingsmateriaal op Galerie van Azure ML versnellen.
