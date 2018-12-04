---
title: Azure Machine Learning-Services in Azure-notitieblokken gebruiken | Microsoft Docs
description: Een overzicht van de voorbeeld-notebooks voor Azure Machine Learning-Services die u met Azure-Notebooks gebruiken kunt.
services: app-service
documentationcenter: ''
author: kraigb
manager: douge
ms.assetid: 0dc4fc31-ae1c-422c-ac34-7b025e6651b4
ms.service: notebooks
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/04/2018
ms.author: kraigb
ms.openlocfilehash: fabd89f7591abafd68b318921fb7723f3eb7373d
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2018
ms.locfileid: "52856098"
---
# <a name="use-azure-machine-learning-services-in-a-notebook"></a>Azure Machine Learning-Services in een notitieblok gebruiken

Azure-notitieblokken afkomstig is vooraf geconfigureerd met de noodzakelijke omgeving om te werken met [Azure Machine Learning Services](/azure/machine-learning/service/). U kunt eenvoudig een voorbeeldproject klonen bij uw account laptops om te verkennen tal van scenario's voor Machine Learning.

## <a name="clone-the-sample-into-your-account"></a>Het voorbeeld klonen bij uw account

1. Meld u aan bij [Azure notitieblokken](https://notebooks.azure.com/).
1. Selecteer **Mijn projecten** om te navigeren naar het dashboard projecten.
1. Selecteer de **GitHub-opslagplaats uploaden** (de pijl-omhoog) knop open de **Github-opslagplaats uploaden** pop-upvenster.
1. Voer in het pop-upvenster `Azure/MachineLearningNotebooks` in **GitHub-opslagplaats**, Geef een naam op voor het project in **projectnaam** bieden een id in, zoals 'Azure ML-Services, **Project-ID**, schakel **openbare** als u wilt, selecteert u **importeren**.

    ![Voorbeeld van Azure Machine Learning-Notebook in uw account notitieblokken importeren](media/azureml-import-project.png)

1. Na een minuut of twee gaat Azure-notitieblokken automatisch u naar het nieuwe project-dashboard.

## <a name="run-a-sample-notebook"></a>Uitvoeren op een voorbeeld-notebook

1. Selecteer **00 - configuration.ipynb** naar het gedeelte over configuratie van de notebook start en volg de instructies voor het maken van een Azure Machine Learning-werkruimte.

    - Omdat Azure notitieblokken al de vereiste pakketten voor Python bevat, kunt u alleen het codefragment in stap 2 van de vereisten om te controleren of de Azure ML-SDK-versie uitvoeren.

1. Zodra de configuratie is voltooid, selecteert u **01.getting gestart** om te navigeren naar de map met 13 verschillende voorbeeldnotitieblokken, die elk is spreken voor zich.

## <a name="next-steps"></a>Volgende stappen

De documentatie van Azure Machine Learning-Services bevat tal van andere bronnen die u begeleiden bij het werken met Machine Learning-Service in notitieblokken:

- [Snelstartgids: Gebruik Python aan de slag met Azure Machine Learning](/azure/machine-learning/service/quickstart-create-workspace-with-python.md)
- [Zelfstudie #1: Een model van de classificatie installatiekopie met de service Azure Machine Learning te trainen](/azure/machine-learning/service/tutorial-train-models-with-aml.md)
- [Zelfstudie #2: Een classificeringsmodel installatiekopie in Azure Container exemplaar (ACI) implementeren](/azure/machine-learning/service/tutorial-deploy-models-with-aml.md)
- [Zelfstudie: Een classificatie model trainen met geautomatiseerde machine learning in Azure Machine Learning-service](/azure/machine-learning/service/tutorial-auto-train-models.md)

Zie ook de documentatie voor de [Azure Machine Learning-SDK voor Python](/python/api/overview/azure/ml/intro.md?view=azure-ml-py).
