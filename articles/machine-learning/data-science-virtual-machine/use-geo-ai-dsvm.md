---
title: Met behulp van de Geo kunstmatige Intelligence gegevens wetenschappelijke virtuele Machine - Azure | Microsoft Docs
description: Het gebruik van een geografisch AI virtuele Machine in Azure.
keywords: deep leren, AI en hulpmiddelen voor wetenschappelijke gegevens, gegevens wetenschappelijke virtuele machine, georuimtelijke analytics
services: machine-learning
documentationcenter: ''
author: gopitk
manager: cgronlun
ms.assetid: ''
ms.service: machine-learning
ms.component: data-science-vm
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 03/05/2018
ms.author: gokuma
ms.openlocfilehash: f346b086a0269f247d64edf9346b01849ba3d0ee
ms.sourcegitcommit: 9cdd83256b82e664bd36991d78f87ea1e56827cd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/16/2018
---
# <a name="using-the-geo-artificial-intelligence-data-science-virtual-machine"></a>Gebruik van geografisch kunstmatige Intelligence gegevens wetenschappelijke virtuele Machine

Gebruik de Geo AI gegevens wetenschappelijke VM ophalen van gegevens voor analyse, het uitvoeren van worsteling gegevens en het bouwen van modellen voor AI-toepassingen die georuimtelijke informatie gebruiken. Nadat u hebt uw geografisch AI gegevens wetenschappelijke VM ingericht en ArcGIS Pro aangemeld met uw account ArcGIS, kunt u de interactie met ArcGIS desktop- en ArcGis online starten. U kunt ook toegang tot ArcGIS van Python-interfaces en een R taal bridge vooraf is geconfigureerd op de VM van de wetenschap Geo-gegevens. Uitgebreide AI om toepassingen te bouwen, kunt u het combineren met de machine learning en diep leren frameworks en andere gegevens wetenschappelijke software beschikbaar is op de wetenschappelijke gegevens VM.  


## <a name="configuration-details"></a>Configuratiegegevens

De bibliotheek Python [arcpy](http://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm), die wordt gebruikt voor interface met ArcGIS is geïnstalleerd in de globale hoofdmap conda omgeving van VM wetenschappelijke gegevens dat is gevonden op ```c:\anaconda```. 

- Als u Python in een opdrachtprompt uitvoert, voert u ```activate``` activeren in de conda hoofdmap Python-omgeving. 
- Als u een IDE- of Jupyter-notebook gebruikt, selecteert u de omgeving of voor kernel om te controleren of dat u zich in de juiste conda-omgeving. 

De R-bridge ArcGIS wordt geïnstalleerd als een R-bibliotheek met de naam [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) in de belangrijkste Microsoft-R exemplaar van de zelfstandige server zich bevindt op ```C:\Program Files\Microsoft\ML Server\R_SERVER```. Visual Studio, RStudio en Jupyter zijn al vooraf geconfigureerd voor het gebruik van deze omgeving voor R en hebben toegang tot de ```arcgisbinding``` R-bibliotheek. 


## <a name="geo-ai-data-science-vm-samples"></a>Voorbeelden van geo AI gegevens wetenschappelijke VM

Naast de ML en deep voorbeelden op basis van het framework van de basis gegevens wetenschappelijke virtuele machine learning is een reeks voorbeelden georuimtelijke ook beschikbaar als onderdeel van de virtuele machine Geo AI gegevens wetenschap. Deze voorbeelden kunt u snel het ontwikkelen van AI-toepassingen met georuimtelijke gegevens en de ArcGIS-software. 


1. [Ophalen van vermeld met georuimtelijke analytics met behulp van Python](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/Python%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): een inleidende voorbeeld waarin wordt beschreven hoe u wilt werken met georuimtelijke gegevens met behulp van de interface Python naar ArcGIS geleverd door de [arcpy](http://pro.arcgis.com/en/pro-app/arcpy/main/arcgis-pro-arcpy-reference.htm) bibliotheek. U ziet ook hoe u traditionele machine learning-met georuimtelijke gegevens combineren en het resultaat op een kaart in ArcGIS visualiseren. 

2. [Ophalen van vermeld met georuimtelijke analytics met R](https://github.com/Azure/DataScienceVM/blob/master/Notebooks/ArcGIS/R%20walkthrough%20ArcGIS%20Data%20analysis%20and%20ML.ipynb): een inleidende voorbeeldtoepassing die u laat hoe zien werken met georuimtelijke gegevens met behulp van de interface R naar ArcGIS geleverd door de [arcgisbinding](https://github.com/R-ArcGIS/r-bridge) bibliotheek. 

3. [Pixel niveau land classificatie gebruiken](https://github.com/Azure/pixel_level_land_classification): een zelfstudie ziet u het maken van een model diep neurale netwerk die een installatiekopie van een lucht als invoer accepteert en retourneert een label land behandeld. Voorbeelden van land behandeld labels zijn 'forested' of 'water'. Het model retourneert deze een label voor elke pixel in de installatiekopie. Het model is gebouwd met behulp van Microsoft open source [cognitieve Toolkit (CNTK)](https://www.microsoft.com/en-us/cognitive-toolkit/) grondige learning framework. Het voorbeeld ziet u hoe moet worden uitgebreid met de training op [Azure Batch AI](https://docs.microsoft.com/azure/batch-ai/) en de modelvoorspellingen in ArcGIS Pro-software gebruiken. 


## <a name="next-steps"></a>Volgende stappen

Aanvullende voorbeelden die gebruikmaken van de VM van de wetenschappelijke gegevens zijn beschikbaar:

* [Voorbeelden](dsvm-samples-and-walkthroughs.md)

