---
title: Voorbeelden van Jupyter-notebooks
titleSuffix: Azure Machine Learning service
description: Jupyter-voorbeeldnotebooks vinden en gebruiken om Azure Machine Learning Service in Python te ontdekken.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: cd88fd85ce6d18287c700a54e42b6237a42ea5c9
ms.sourcegitcommit: eea74d11a6d6ea6d187e90e368e70e46b76cd2aa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/03/2019
ms.locfileid: "65035382"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Jupyter-notebooks gebruiken om Azure Machine Learning Service te verkennen

We hebben een reeks Jupyter Python-notebooks ontwikkeld zodat u Azure Machine Learning Service gemakkelijk kunt verkennen. 

Ontdek hoe u de service gebruikt met de documentatie op deze site en hoe u de notebooks aan uw situatie aanpast. 

Gebruik een van de onderstaande paden om een notebookserver met deze voorbeeldnotebooks uit te voeren.  Zodra de server wordt uitgevoerd, vindt u notebooks voor zelfstudie in de map **tutorials**. U kunt ook de verschillende functies bekijken in de map **how-to-use-azureml**.

## <a name="a-managed-cloud-notebook-server"></a>Een beheerde cloud notebook-server

Het is eenvoudig aan de slag met uw eigen cloud-gebaseerde notebook-server. De voorbeeld-laptop en de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) al zijn geïnstalleerd en geconfigureerd voor u, zodra u deze cloudresource hebt gemaakt.  

[!INCLUDE [aml-azure-notebooks](../../../includes/aml-azure-notebooks.md)]

* De voorbeelden zijn beschikbaar op de webpagina van de notebook.

## <a name="a-data-science-virtual-machine-dsvm"></a>Een Data Science Virtual Machine (DSVM)

De [Azure Machine Learning SDK voor Python](https://aka.ms/aml-sdk) en de notebookserver zijn al voor u geïnstalleerd en geconfigureerd in een DSVM. 

Nadat u [een DSVM hebt gemaakt](how-to-configure-environment.md#dsvm), volgt u deze stappen op de DSVM om de notebooks uit te voeren.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="your-own-jupyter-notebook-server"></a>Uw eigen Jupyter-Notebook-server

Volg deze stappen om een lokale Jupyter Notebook-server te maken op uw computer.

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

De installatie-instructies installeert de pakketten die u wilt uitvoeren van de Quick Start- en -zelfstudie-notebooks.  Voor andere voorbeeldnotebooks moeten mogelijk extra onderdelen worden geïnstalleerd.  Zie [De Azure Machine Learning-SDK voor Python installeren](https://docs.microsoft.com/python/api/overview/azure/ml/install) voor meer informatie over deze onderdelen.

## <a name="azure-notebooks"></a>Azure Notebooks

De voorbeeld-notebooks en de [Azure Machine Learning-SDK voor Python](https://aka.ms/aml-sdk) al zijn geïnstalleerd en geconfigureerd voor u op [Azure notitieblokken](https://notebooks.azure.com/). De installatie en toekomstige updates worden automatisch beheerd via Azure-services.

Gebruik de [Azure-portal](https://portal.azure.com) aan de slag met Azure-Notebooks.  Uw werkruimte openen en naar de **overzicht** sectie, selecteer **aan de slag met Azure-notitieblokken**.

## <a name="next-steps"></a>Volgende stappen

+ Verken de voorbeeld-notebooks voor Azure Machine Learning-service in deze GitHub-opslagplaats: https://aka.ms/aml-notebooks

Volg deze zelfstudies:
+ [Een model voor de classificatie van afbeeldingen trainen en implementeren met MNIST](tutorial-train-models-with-aml.md)

+ [Gegevens voorbereiden en geautomatiseerde machine learning gebruiken om een regressiemodel met de gegevensset voor taxi's in NYC te trainen](tutorial-data-prep.md)