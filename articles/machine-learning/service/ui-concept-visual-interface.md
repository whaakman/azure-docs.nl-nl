---
title: Visuele interface
titleSuffix: Azure Machine Learning service
description: Meer informatie over de voorwaarden, de concepten en de werkstroom die gezamenlijk de visuele interface (preview) voor Azure Machine Learning-service.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: sgilley
author: sdgilley
ms.date: 05/15/2019
ms.openlocfilehash: be07e0f3438ea93312d4eb440e7e63b8f98e11b8
ms.sourcegitcommit: 41ca82b5f95d2e07b0c7f9025b912daf0ab21909
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "67077382"
---
# <a name="what-is-the-visual-interface-for-azure-machine-learning-service"></a>Wat is de visuele interface voor Azure Machine Learning-service? 

De visuele interface (preview) voor Azure Machine Learning-service kunt u gegevens voorbereiden, trainen, testen, implementeren, beheren en bijhouden van machine learning-modellen zonder code te schrijven.

Er is niets te programmeren, visueel verbinding [gegevenssets](#dataset) en [modules](#module) te maken van uw model.

Uw Azure Machine Learning-service maakt gebruik van de visuele interface [werkruimte](concept-workspace.md) aan:

+ Artefacten van schrijven [experimenteren](#experiment) wordt uitgevoerd in de werkruimte.
+ Toegang tot [gegevenssets](#dataset).
+ Gebruik de [rekenresources](#compute) in de werkruimte om uit te voeren van het experiment. 
+ Registreren [modellen](concept-azure-machine-learning-architecture.md#models).
+ [Implementeer](#deployment) modellen als webservices op-resources in de werkruimte COMPUTE.

![Overzicht van de visuele interface](media/ui-concept-visual-interface/overview.png)

## <a name="workflow"></a>Werkstroom

De visuele interface biedt een interactieve, visuele canvas snel bouwen, testen, en ze opnieuw testen op een model. 

+ U slepen en neerzetten [modules](#module) naar het canvas.
+ De modules met elkaar verbinden om een [experimenteren](#experiment).
+ Voer het experiment met behulp van de compute-resource van de Service Machine Learning-werkruimte.
+ Uw modelontwerp herhalen door het experiment bewerken en nogmaals uit te voeren.
+ Wanneer u klaar bent, zet uw **trainingsexperiment** naar een **Voorspellend experiment**.
+ [Implementeer](#deployment) de Voorspellend experiment als een web-service zodat het model kan worden geopend door anderen.

## <a name="experiment"></a>Experiment

Een experiment maken vanaf het begin of een bestaand voorbeeldexperiment als sjabloon gebruiken.  Telkens wanneer die u een experiment uitvoert worden artefacten opgeslagen in uw werkruimte.

Een experiment bestaat uit gegevenssets en analytische modules, die u met elkaar verbinden in een model. Een geldig experiment heeft de volgende kenmerken:

* Gegevenssets mogen alleen worden verbonden met modules.
* Modules mogen worden verbonden met gegevenssets of andere modules.
* Alle ingangspoorten voor modules moeten een verbinding met de gegevensstroom hebben.
* Alle vereiste parameters voor elke module moeten worden ingesteld.

Zie voor een voorbeeld van een eenvoudig experiment [Quick Start: Voorbereiden en visualiseren van gegevens zonder code te schrijven in Azure Machine Learning](ui-quickstart-run-experiment.md).

Zie voor een volledig overzicht van een predictive analytics-oplossing, [zelfstudie: Auto's prijs op met de visuele interface voorspellen](ui-tutorial-automobile-price-train-score.md).

## <a name="dataset"></a>Gegevensset

Een gegevensset bestaat uit gegevens die is geüpload naar de visuele interface te gebruiken in het modelleringsproces. Er zijn een aantal voorbeeldgegevenssets opgenomen om te experimenteren met en u kunt meer gegevenssets uploaden als u deze nodig hebt.

## <a name="module"></a>Module

Een module is een algoritme dat u met uw gegevens kunt uitvoeren. De visuele interface heeft een aantal modules, variërend van ingress-functies voor training, beoordeling en van validatieprocessen.

Een module kan een reeks parameters hebben waarmee u de interne algoritmen van de module kunt configureren. Wanneer u een module op het canvas selecteert, worden de parameters van de module weergegeven in het deelvenster met eigenschappen aan de rechterkant van het canvas. U kunt de parameters in dit deelvenster wijzigen om het model af te stemmen.

![Module-eigenschappen](media/ui-concept-visual-interface/properties.png)

Zie voor hulp bij het navigeren door de bibliotheek met machine learning-algoritmen, [overzicht van algoritmen en modules](../algorithm-module-reference/module-reference.md)

## <a name="compute"></a> COMPUTE-resources

Gebruik de rekenresources van uw werkruimte om uit te voeren van uw experiment of de host-uw geïmplementeerde modellen als webservices. De ondersteunde compute-doelen zijn:


| COMPUTE-doel | Training | Implementatie |
| ---- |:----:|:----:|
| Azure Machine Learning-Computing | ✓ | |
| Azure Kubernetes Service | | ✓ |

COMPUTE-doelen zijn gekoppeld aan uw Machine Learning [werkruimte](concept-workspace.md). Beheren van uw compute-doelen in uw werkruimte in de [Azure-portal](https://portal.azure.com).

## <a name="deployment"></a>Implementatie

Wanneer uw predictive analytics-model klaar is, kunt u deze implementeren als een webservice rechtstreeks vanuit de visuele interface.

De webservices bieden een interface tussen een toepassing en het scoring-model. Een externe toepassing kan communiceren met de scoremodel in realtime. Een aanroep van een webservice retourneert voorspellingsresultaten naar een externe toepassing. Tijdens een aanroep aan een webservice wordt een API-sleutel doorgegeven die is gemaakt tijdens de implementatie van de webservice. De webservice is gebaseerd op REST, een populaire architectuur voor webprogrammering.

Zie voor meer informatie over het implementeren van uw model, [zelfstudie: Implementeren van een machine learning-model met de visuele interface](ui-tutorial-automobile-price-deploy.md).

## <a name="next-steps"></a>Volgende stappen

* Leer de basisprincipes van voorspellende analyses en machine learning met [Quick Start: Voorbereiden en visualiseren van gegevens zonder code te schrijven in Azure Machine Learning](ui-quickstart-run-experiment.md).
* Gebruik een van de voorbeelden en aanpassen met suite uw behoeften:
    * [Voorbeeld 1: regressie: Prijs voorspellen](ui-sample-regression-predict-automobile-price-basic.md)
    * [Voorbeeld 2: regressie: Prijs voorspellen en algoritmen te vergelijken](ui-sample-regression-predict-automobile-price-compare-algorithms.md)
    * [Voorbeeld 3 - indeling: Kredietrisico voorspellen](ui-sample-classification-predict-credit-risk-basic.md)
    * [Voorbeeld 4 - classificatie: Kredietrisico (kosten gevoelige) voorspellen](ui-sample-classification-predict-credit-risk-cost-sensitive.md)
    * [Voorbeeld 5 - indeling: Verloop, appetency en up-selling te voorspellen](ui-sample-classification-predict-churn.md)
