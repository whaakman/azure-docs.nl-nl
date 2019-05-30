---
title: Voorbeelden van Jupyter-notebooks
titleSuffix: Azure Machine Learning service
description: Zoek en voorbeeld van de Jupyter-notebooks gebruiken om te ontdekken van de service Azure Machine Learning Python-SDK.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: sample
author: sdgilley
ms.author: sgilley
ms.reviewer: sgilley
ms.date: 05/29/2019
ms.custom: seodec18
ms.openlocfilehash: ea4d5a807c25ea0406b49dac8a83ef1a34e0e8b3
ms.sourcegitcommit: 51a7669c2d12609f54509dbd78a30eeb852009ae
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/30/2019
ms.locfileid: "66391809"
---
# <a name="use-jupyter-notebooks-to-explore-azure-machine-learning-service"></a>Jupyter-notebooks gebruiken om Azure Machine Learning Service te verkennen

De [Azure Machine Learning-laptops opslagplaats](https://github.com/azure/machinelearningnotebooks) bevat de meest recente Azure Machine Learning Python SDK-voorbeelden. Deze laptops Juypter zijn ontworpen om u te helpen u bij het verkennen van de SDK en fungeren als modellen voor uw eigen machine learning-projecten.

In dit artikel laat zien hoe u toegang tot de opslagplaats van de volgende omgevingen:

- [Azure Machine Learning-Notebook VM](#azure-machine-learning-notebook-vm)
- [Breng uw eigen notebookserver](#bring-your-own-jupyter-notebook-server)
- [Virtuele Machine voor Datatechnologie](#data-science-virtual-machine)
- [Azure Notebooks](#azure-notebooks)

> [!NOTE]
> Nadat u de opslagplaats hebt gekloond, vindt u de zelfstudie notebooks onder in de **zelfstudies** map en functiespecifieke notebooks onder in de **How-to-naar-gebruik-azureml** map.

## <a name="azure-machine-learning-notebook-vm"></a>Azure Machine Learning-Notebook VM

De eenvoudigste manier om aan de slag met de voorbeelden is voltooid de [cloud-gebaseerde notebook snelstartgids](quickstart-run-cloud-notebook.md). Als voltooid, hebt u een vooraf geladen met de SDK en de voorbeeldopslagplaats toegewezen notebook-server. Er is geen downloads of installatie nodig.

## <a name="bring-your-own-jupyter-notebook-server"></a>Breng uw eigen Jupyter-Notebook-server

Als u dit doen om uw eigen notebook-server voor lokale ontwikkeling wilt, volgt u deze stappen:

[!INCLUDE [aml-your-server](../../../includes/aml-your-server.md)]

Deze instructies installeren de base die nodig zijn voor de Quick Start- en zelfstudie notebooks SDK-pakketten. Andere voorbeeldnotitieblokken moet u mogelijk om extra onderdelen te installeren. Zie voor meer informatie, [installeren van de Azure Machine Learning-SDK voor Python](https://docs.microsoft.com/python/api/overview/azure/ml/install).

## <a name="data-science-virtual-machine"></a>Data Science Virtual Machine

De Data Science Virtual Machine (DSVM) is een aangepaste VM-installatiekopie, speciaal gebouwd voor gegevenswetenschap. Als u [maken van een DSVM](how-to-configure-environment.md#dsvm), de SDK en de notebook-server zijn geïnstalleerd en geconfigureerd voor u. U moet echter nog steeds een werkruimte maken en de voorbeeldopslagplaats klonen.

[!INCLUDE [aml-dsvm-server](../../../includes/aml-dsvm-server.md)]

## <a name="azure-notebooks"></a>Azure Notebooks

Op [Azure notitieblokken](https://notebooks.azure.com/), de SDK en de notebook-server zijn geïnstalleerd en geconfigureerd voor u. Azure-notitieblokken biedt een volledig beheerde, lichtgewicht omgeving om te verkennen.

Voor toegang tot de opslagplaats met voorbeelden op Azure-Notebooks, gaat u naar uw Azure Machine Learning-werkruimte via de [Azure-portal](https://portal.azure.com). Uit de **overzicht** sectie, selecteer **aan de slag met Azure-notitieblokken**.

## <a name="next-steps"></a>Volgende stappen

Verken de [voorbeeld notitieblokken](https://aka.ms/aml-notebooks) om te ontdekken welke Azure Machine Learning service kunt doen, of probeer deze zelfstudie:

- [Een model voor de classificatie van afbeeldingen trainen en implementeren met MNIST](tutorial-train-models-with-aml.md)

- [Gegevens voorbereiden en geautomatiseerde machine learning gebruiken om een regressiemodel met de gegevensset voor taxi's in NYC te trainen](tutorial-data-prep.md)