---
title: Wat is de Azure Machine Learning-service?
description: Verklaart de basisconcepten van machine learning in de cloud, beschrijft waarvoor u dit kunt gebruiken en definieert termen voor machine learning. Overzicht van Azure Machine Learning, een geïntegreerde end-to-end gegevenswetenschapoplossing die professionele gegevenswetenschappers in staat stelt geavanceerde analyseapplicaties te ontwikkelen, hiermee te experimenteren en deze op cloudschaal te implementeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: overview
ms.reviewer: jmartens
author: garyericson
ms.author: garye
ms.date: 09/24/2018
ms.openlocfilehash: 1dac11b8ad71a936b33742b52c95ac998176baf7
ms.sourcegitcommit: da3459aca32dcdbf6a63ae9186d2ad2ca2295893
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2018
ms.locfileid: "51244935"
---
# <a name="what-is-azure-machine-learning-service-preview"></a>Wat is Azure Machine Learning-service (preview)?

Azure Machine Learning-service (Preview) is een cloudservice die u kunt gebruiken om machine Learning-modellen te ontwikkelen en te implementeren. Door Azure Machine Learning-service te gebruiken, kunt u uw modellen volgen terwijl u ze bouwt, leert, implementeert en beheert, allemaal op de grote schaal die de cloud biedt.

## <a name="what-is-machine-learning"></a>Wat is machine learning?

Machine learning is een techniek van gegevenswetenschap die computers in staat stelt om op basis van bestaande gegevens, toekomstig gedrag, resultaten en trends te voorspellen. Met behulp van machine learning kunnen computers leren zonder dat ze expliciet worden geprogrammeerd.

Prognoses of voorspellingen op basis van machine learning kunnen apps en apparaten slimmer maken. Wanneer u bijvoorbeeld online winkelt, helpt machine learning op basis van wat u eerder hebt gekocht, bij het aanraden van andere producten waarvoor u mogelijk belangstelling hebt. Of wanneer uw creditcard wordt gebruikt, vergelijkt machine learning de transactie met een database van transacties om fraude te detecteren. En wanneer uw robotstofzuiger een kamer stofzuigt, helpt machine learning bij het besluiten wanneer de taak klaar is.

## <a name="what-is-azure-machine-learning-service"></a>Wat is Azure Machine Learning-service?

Azure Machine Learning-service biedt een omgeving in de cloud die u kunt gebruiken om machine learning-modellen te ontwikkelen, trainen, testen, implementeren, beheren en volgen.

[ ![Azure Machine Learning-serviceworkflow](./media/overview-what-is-azure-ml/aml.png) ] (./media/overview-what-is-azure-ml/aml.png#lightbox)

Azure Machine Learning-service ondersteunt open-source technologieën volledig, zodat u tienduizenden open-source Python-pakketten kunt gebruiken met machine learning-componenten als TensorFlow en scikit-learn.
Uitgebreide hulpprogramma’s als [Jupyter notebooks](http://jupyter.org) of de [Visual Studio Code Tools for AI](https://visualstudio.microsoft.com/downloads/ai-tools-vscode/) maken het eenvoudig interactief gegevens te verkennen, te transformeren en dan modellen te ontwikkelen en te testen.
Azure Machine Learning-service omvat ook functies die [modelgeneratie automatiseren en afstemmen](tutorial-auto-train-models.md)om u te helpen met gemak, efficiënt en nauwkeurig modellen te maken.

Met Azure Machine Learning-service kunt u op uw lokale machine beginnen met trainen en dan opschalen naar de cloud. Met systeemeigen ondersteuning voor [Azure Batch AI](https://azure.microsoft.com/services/batch-ai/) en [geavanceerde hyperparameter afstemservices](how-to-tune-hyperparameters.md) kunt u sneller betere modellen bouwen met gebruik van de kracht van de cloud. 

Al u het juiste model hebt, kunt u het eenvoudig implementeren in een container zoals Docker. Dit betekent dat het eenvoudig is [Azure Container Instances](how-to-deploy-to-aci.md) of [Azure Kubernetes Service](how-to-deploy-to-aks.md) te implementeren, of u kunt de container gebruiken in uw eigen implementaties, zowel on-premises als in de cloud.
U kunt de geïmplementeerde modellen beheren en meerdere uitvoeringen volgen terwijl u experimenteert om de beste oplossing te vinden.

[!INCLUDE [aml-preview-note](../../../includes/aml-preview-note.md)]

## <a name="what-can-i-do-with-azure-machine-learning-service"></a>Wat kan ik doen met de Azure Machine Learning-service?

Azure Machine Learning-service kan voor u automatisch een model genereren en het automatisch afstemmen.
Zie [Zelfstudie: Automatisch een classificatiemodel trainen met Azure Automated Machine Learning](tutorial-auto-train-models.md) voor een voorbeeld.

Of met de Azure Machine Learning <a href="https://aka.ms/aml-sdk" target="_blank">SDK</a> voor Python kunt u samen met open-source Python-pakketten uiterst nauwkeurige machine learning- en deep learning-modellen bouwen en trainen in een Azure Machine Learning-servicewerkruimte.
U kunt kiezen uit veel machine learning-componenten die beschikbaar zijn in open-source Python-pakketten, zoals de volgende:

- <a href="http://scikit-learn.org/stable/" target="_blank">Scikit-learn</a>
- <a href="https://www.tensorflow.org" target="_blank">Tensorflow</a>
- <a href="https://pytorch.org" target="_blank">PyTorch</a>
- <a href="https://www.microsoft.com/en-us/cognitive-toolkit/" target="_blank">CNTK</a>
- <a href="http://mxnet.io" target="_blank">MXNet</a>

Als u eenmaal een model hebt, kunt u het gebruiken om een container (zoals Docker) te maken die lokaal kan worden geïmplementeerd om te testen en dan als productie-webservice in [Azure Container Instances](how-to-deploy-to-aci.md) of [Azure Kubernetes Service](how-to-deploy-to-aks.md).

U kunt dan uw geïmplementeerde modellen beheren met de [Azure Portal](https://portal.azure.com/) of de [Azure Machine Learning CLI-extensie](reference-azure-machine-learning-cli.md).
U kunt de metrische gegevens van een model evalueren, nieuwe versies van het model opnieuw trainen en implementeren en dat allemaal terwijl u de experimenten van het model volgt.

Bekijk [Volgende stappen](#next-steps) hieronder om te beginnen met Azure Machine Learning-service.

## <a name="how-is-azure-machine-learning-service-different-from-studio"></a>Waarin verschilt Azure Machine Learning-service van Studio?

Azure Machine Learning Studio is een visuele werkruimte voor samenwerking met slepen en neerzetten waar u machine learning-oplossingen kunt bouwen, testen en implementeren zonder dat u code hoeft te programmeren. Er wordt gebruikgemaakt van vooraf gebouwde en geconfigureerde machine learning-algoritmes en modules voor gegevensverwerking.

Gebruik Machine Learning Studio wanneer u snel en eenvoudig wilt experimenteren met machine learning-modellen en de ingebouwde machine learning-algoritmen voldoende zijn voor uw oplossingen.

Gebruik Machine Learning-service als u werkt in een Python-omgeving, u meer controle wenst over uw machine learning-algoritmen of als u open-source machine learning-bibliotheken wilt gebruiken.

> [!NOTE]
> Modellen die zijn gemaakt in Azure Machine Learning Studio kunnen niet worden geïmplementeerd of beheerd door Azure Machine Learning-service.

## <a name="free-trial"></a>Gratis proefversie
Als u geen abonnee bent, kunt u [gratis een Azure-account openen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F). U ontvangt tegoed wat u kunt uitgeven aan Azure-services. Als uw tegoed op is, kunt u het account behouden en de [gratis Azure-services](https://azure.microsoft.com/free/) gebruiken. Er worden nooit kosten in rekening gebracht bij uw creditcard tenzij u de instellingen expliciet wijzigt en aangeeft dat u wilt betalen. U kunt ook [de voordelen voor MSDN-abonnees activeren](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/?WT.mc_id=A261C142F): bij uw MSDN-abonnement ontvangt u elke maand een tegoed dat u kunt gebruiken voor betaalde Azure-services.

## <a name="next-steps"></a>Volgende stappen

- Een machine learning-servicewerkruimte maken met het artikel [Azure Portal gebruiken om te beginnen](quickstart-get-started.md)
 
- Volg de volledige zelfstudie, [Train een installatiekopieclassificeringsmodel met Azure Machine Learning](tutorial-train-models-with-aml.md) om te leren hoe u modellen traint en implementeert met Azure Machine Learning-service

- Zie [Zelfstudie: automatisch een classificeringsmodel trainen met Azure Automated Machine Learning](tutorial-auto-train-models.md) voor informatie over toestaan dat Azure Machine Learning automatisch uw model genereert en afstemt

- Leer meer over [Machine Learning-pijplijnen](/azure/machine-learning/service/concept-ml-pipelines) om uw Machine Learning-scenario’s te bouwen, te optimaliseren en te beheren.

- Zie [Azure Machine Learning-service, architectuur en concepten](concept-azure-machine-learning-architecture.md) voor een technisch, uitgebreid overzicht van de service

- Zie [Andere machine learning-producten van Microsoft](./overview-more-machine-learning.md) voor meer informatie over andere machine learning-producten van Microsoft


<!-- 

An intro to AML or an end-to-end quickstart video could go here.

In this 9-minute video, learn how you can benefit your app. You'll learn about key features and what a typical workflow looks like. 

>[!VIDEO https://channel9.msdn.com/Events/Connect/2016/138/player]
 
+ 0-3 minutes covers key features and use-cases.
+ 3-4 minutes covers service provisioning. 
+ 4-6 minutes covers Import Data wizard used to create an index using the built-in real estate dataset.

-->
