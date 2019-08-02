---
title: Met behulp van de Geo kunstmatige intelligentie Data Science Virtual Machine - Azure | Microsoft Docs
description: Informatie over het gebruik van de Geo AI Data Science Virtual Machine voor het analyseren van gegevens en bouw modellen op basis van georuimtelijke gegevens.
keywords: deep learning, AI, hulpprogramma's voor data science, virtuele machine voor datatechnologie, georuimtelijke analyses
services: machine-learning
documentationcenter: ''
author: vijetajo
manager: cgronlun
ms.custom: seodec18
ms.assetid: ''
ms.service: machine-learning
ms.subservice: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: vijetaj
ms.openlocfilehash: 9bca7089e6137b3780e3d22f50887e880be29d8e
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68565073"
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Met behulp van de Geo kunstmatige intelligentie Data Science Virtual Machine

De Geo AI Data Science VM gebruiken voor het ophalen van gegevens voor analyse, het uitvoeren van data wrangling en bouw modellen voor AI-toepassingen die georuimtelijke gegevens gebruiken. Nadat u hebt uw Geo AI Data Science VM ingericht en ArcGIS Pro aangemeld met uw ArcGIS-account, kunt u de interactie met ArcGIS-desktop- en ArcGis online kunt starten. U kunt ook toegang tot ArcGIS van Python-interfaces en een R-taal bridge vooraf is geconfigureerd op de Geo-Data Science VM. Voor het bouwen van uitgebreide AI-toepassingen, kunt u het combineren met de machine learning- en deep learning-frameworks en andere gegevens wetenschappelijke software die beschikbaar is op de Data Science VM.  


## <a name="configuration-details"></a>Configuratiegegevens

De Python-clientbibliotheek [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), die wordt gebruikt voor interactie met ArcGIS is geïnstalleerd in de algemene root conda-omgeving van de Data Science VM die is gevonden op ```c:\anaconda```. 

- Als u Python in een opdrachtprompt uitvoert, voert u ```activate``` activeren in de conda basis Python-omgeving. 
- Als u een IDE- of Jupyter-notebook gebruikt, kunt u de omgeving of de kernel om te controleren of dat u zich in de juiste conda-omgeving selecteren. 

De R-brug en ArcGIS wordt geïnstalleerd als een R-bibliotheek met de naam [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) in de belangrijkste Microsoft R server zelfstandige-exemplaar dat zich bevindt in ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio en Jupyter zijn al vooraf geconfigureerd voor het gebruik van deze R-omgeving en hebben toegang tot de ```arcgisbinding``` R-bibliotheek. 


## <a name="geo-ai-data-science-vm-samples"></a>Voorbeelden van de geo AI Data Science VM

Naast de ML en deep learning framework op basis van voorbeelden van de basis Data Science VM, is ook een reeks georuimtelijke voorbeelden beschikbaar als onderdeel van de Geo AI Data Science VM. Aan de hand van deze voorbeelden kunnen u snel het ontwikkelen van AI-toepassingen met behulp van georuimtelijke gegevens en de ArcGIS-software. 


1. [Krijg de voor bereiding van georuimtelijke analyse met python](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): Een inleidend voor beeld waarin wordt getoond hoe u met georuimtelijke gegevens werkt met behulp van de Python-interface, die wordt ArcGIS door de [arcpy](https://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) -bibliotheek. U ziet ook hoe u traditionele machine learning met georuimtelijke gegevens combineren en het resultaat op een kaart in ArcGIS visualiseren. 

2. De [voor bereiding van georuimtelijke analyse met R](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): Een voor beeld van een inleidende informatie over hoe u met georuimtelijke gegevens kunt werken met behulp van de R-interface voor ArcGIS van de [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) -bibliotheek. 

3. [Classificatie voor land gebruik op pixel niveau](https://github.com/Azure/pixel_level_land_classification): Een zelf studie waarin wordt getoond hoe u een diepe Neural-netwerk model maakt dat een lucht afbeelding als invoer accepteert en een omslag label retourneert. Voorbeelden van land-cover labels zijn "forested" of "water". Het model geretourneerd die een label voor elke pixel in de afbeelding. Het model is gebouwd met behulp van Microsoft open source [Cognitive Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) deep learning-frameworks. 


## <a name="next-steps"></a>Volgende stappen

Aanvullende voorbeelden van die gebruikmaken van de Data Science VM zijn hier beschikbaar:

* [Voorbeelden](dsvm-samples-and-walkthroughs.md)

