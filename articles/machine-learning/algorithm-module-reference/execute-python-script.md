---
title: 'Python-Script uitvoeren: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Python-Script uitvoeren in Azure Machine Learning-service Python-code uit te voeren.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/02/2019
ROBOTS: NOINDEX
ms.openlocfilehash: 6e61ed5a18e69b107b78bf2042de21d14cd6beb5
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029128"
---
# <a name="execute-python-script-module"></a>Module voor Python-Script uitvoeren

Dit artikel beschrijft een module van de visuele interface (preview) voor Azure Machine Learning-service.

Gebruik deze module Python-code uit te voeren. Zie voor meer informatie over de architectuur en ontwerp principes van Python [het volgende artikel.](https://docs.microsoft.com/azure/machine-learning/machine-learning-execute-python-scripts)

Met Python, kunt u taken die worden momenteel niet ondersteund door de bestaande modules, zoals uitvoeren:

+ Met behulp van gegevens te visualiseren `matplotlib`
+ Met behulp van Python-bibliotheken te inventariseren, gegevenssets en modellen in de werkruimte
+ Lezen en bewerken van gegevens uit bronnen die niet wordt ondersteund door het laden van de [importgegevens](./import-data.md) module
+ Uw eigen deep learning-code uitvoeren 


Azure Machine Learning maakt gebruik van de distributie Anaconda van Python, waaronder veel algemene hulpprogramma's voor gegevensverwerking. Anaconda versie zal automatisch worden bijgewerkt. Huidige versie is:
 -  Anaconda 4.5 + distributie voor Python 3.6 

De vooraf geïnstalleerde pakketten zijn:
-  asn1crypto==0.24.0
- attrs==19.1.0
- azure-common==1.1.18
- Azure-storage-blob 1.5.0 ==
- azure-storage-common==1.4.0
- certifi==2019.3.9
- cffi==1.12.2
- chardet==3.0.4
- cryptografie 2.6.1 ==
- distro==1.4.0
- idna==2.8
- jsonschema==3.0.1
- lightgbm==2.2.3
- more-itertools==6.0.0
- numpy==1.16.2
- pandas==0.24.2
- Pillow==6.0.0
- pip==19.0.3
- pyarrow 0.12.1 ==
- pycparser 2.19 ==
- pycryptodomex==3.7.3
- pyrsistent==0.14.11
- python-dateutil==2.8.0
- pytz==2018.9
- requests==2.21.0
- scikit-learn==0.20.3
- scipy==1.2.1
- setuptools==40.8.0
- zes 1.12.0 ==
- torch==1.0.1.post2
- torchvision==0.2.2.post3
- urllib3==1.24.1
- wheel==0.33.1 

 Voor het installeren van andere pakketten niet in de lijst met vooraf geïnstalleerde, bijvoorbeeld *scikit-diverse*, voeg de volgende code toe aan uw script: 

 ```python
import os
os.system(f"pip install scikit-misc")
```

## <a name="how-to-use"></a>Gebruiksinstructies

De **Execute Python Script** -module bevat de Python-voorbeeldcode die u als uitgangspunt gebruiken kunt. Het configureren van de **Execute Python Script** -module, bieden u een set van invoer- en Python-code uit te voeren de **Python-script** in het tekstvak.

1. Voeg de **Execute Python Script** module naar het experimentcanvas.

2. Toevoegen en verbinding maken op **Dataset1** geen gegevenssets van de interface die u wilt gebruiken voor invoer. Verwijzen naar deze gegevensset in uw Python-script als **DataFrame1**.

    Gebruik van een gegevensset is optioneel, als u wilt genereren van gegevens met behulp van Python, of gebruik van Python-code voor het importeren van de gegevens rechtstreeks in de module.

    Deze module biedt ondersteuning voor het toevoegen van een tweede gegevensset op **Dataset2**. Verwijzen naar de tweede gegevensset in uw Python-script als DataFrame2.

    Gegevenssets die zijn opgeslagen in Azure Machine Learning worden automatisch geconverteerd naar **pandas** data.frames wanneer met deze module wordt geladen.

    ![Python invoer kaart uitvoeren](media/module/python-module.png)

4. Toevoegen als u wilt opnemen nieuwe Python-pakketten of code, het ZIP-bestand met deze aangepaste resources op **Script bundel**. De invoer naar **Script bundel** moet worden een ZIP-bestand al geüpload naar uw werkruimte. 

    Elk bestand in het geüploade ZIP-archief kan worden gebruikt tijdens het experiment uitvoeren. Als het archief een mapstructuur bevat, de structuur wordt behouden, maar u moet een map genaamd vooraf **src** naar het pad.

5. In de **Python-script** in het tekstvak typt of plakt u geldige Python-script.

    De **Python-script** tekstvak is vooraf gevuld met enkele instructies in opmerkingen en voorbeeldcode voor toegang tot gegevens en uitvoer. **U moet bewerken of vervang deze code.** Zorg ervoor dat Python conventies over inspringing en hoofdlettergebruik.

    + Het script moet een functie met de naam bevatten `azureml_main` als het toegangspunt voor deze module.
    + De functie voor het item mag maximaal twee invoerargumenten: `Param<dataframe1>` en `Param<dataframe2>`
    + ZIP-bestanden die zijn verbonden met de derde invoerpoort zijn uitgepakt en opgeslagen in de map `.\Script Bundle`, die ook wordt toegevoegd aan het Python `sys.path`. 

    Dus als het zipbestand bevat `mymodule.py`, importeert u het met `import mymodule`.

    + Twee gegevenssets kunnen worden geretourneerd aan de interface, een reeks van het type moet `pandas.DataFrame`. U kunt andere uitvoer in uw Python-code maken en ze rechtstreeks naar Azure storage worden geschreven.

6. Voer het experiment uit of Selecteer de module en klikt u op **geselecteerde uitvoeren** alleen de Python-script uit te voeren.

    Alle van de gegevens en de code is geladen in een virtuele machine en uitgevoerd met behulp van de opgegeven Python-omgeving.

## <a name="results"></a>Resultaten

De resultaten van alle berekeningen uitgevoerd door de ingesloten Python-code moeten worden opgegeven als een pandas. DataFrame, wordt automatisch geconverteerd naar de indeling van de Azure Machine Learning-gegevensset, zodat u de resultaten met andere modules in het experiment gebruiken kunt.

De module retourneert twee gegevenssets:  
  
+ **Resultaten 1 gegevensset**, die is gedefinieerd door de eerste geretourneerde pandas dataframe in Python-script

+ **Resultaat van de gegevensset 2**, die is gedefinieerd door de tweede geretourneerde pandas dataframe in Python-script


## <a name="next-steps"></a>Volgende stappen

Zie de [set met beschikbare modules](module-reference.md) met Azure Machine Learning-service. 