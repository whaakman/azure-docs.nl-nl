---
title: Application Insights inschakelen voor de Azure Machine Learning-service in productie
description: Informatie over het instellen van Application Insights voor Azure Machine Learning-service voor implementatie voor Azure Kubernetes Service
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 962090340cad6bcd95245cffe16c25f08a98226b
ms.sourcegitcommit: d372d75558fc7be78b1a4b42b4245f40f213018c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2018
ms.locfileid: "51300758"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Uw Azure Machine Learning-modellen in productie met Application Insights bewaken

In dit artikel leert u hoe u Azure Application Insights instellen voor uw Azure Machine Learning-service. Application Insights biedt u de mogelijkheid om te controleren:
* Tarieven, reactietijden en foutpercentages aanvragen.
* Afhankelijkheidsrelaties, reactietijden en foutpercentages.
* Uitzonderingen.

[Meer informatie over Application Insights](../../application-insights/app-insights-overview.md). 

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.
* Een Azure Machine Learning-werkruimte en een lokale map met uw scripts en de Azure Machine Learning-SDK voor Python geïnstalleerd. Zie voor meer informatie over het verkrijgen van deze vereisten, [het configureren van een ontwikkelomgeving](how-to-configure-environment.md).
* Een getraind model voor machine learning om te worden geïmplementeerd naar Azure Kubernetes Service (AKS). Als u niet hebt, raadpleegt u de [Train installatiekopie classificeringsmodel](tutorial-train-models-with-aml.md) zelfstudie.
* Een [AKS-cluster](how-to-deploy-to-aks.md).

## <a name="enable-and-disable-in-the-portal"></a>In- of uitschakelen in de portal

U kunt in- en uitschakelen van Application Insights in Azure portal.

### <a name="enable"></a>Inschakelen

1. In de [Azure-portal](https://portal.azure.com), opent u uw werkruimte.

1. Op de **implementaties** tabblad, selecteert u de service waar u Application Insights inschakelen.

   [![Lijst met services op het tabblad implementaties](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Selecteer **Bewerken**.

   [![Knop bewerken](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. In **geavanceerde instellingen**, selecteer de **diagnose van AppInsights inschakelen** selectievakje.

   [![Het selectievakje is ingeschakeld voor het inschakelen van diagnostische gegevens](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png#lightbox)

1. Selecteer **Update** aan de onderkant van het scherm om de wijzigingen toe te passen. 

### <a name="disable"></a>Uitschakelen
1. In de [Azure-portal](https://portal.azure.com), opent u uw werkruimte.
1. Selecteer **implementaties**, selecteert u de service en selecteer **bewerken**.

   [![Knop bewerken](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. In **geavanceerde instellingen**, schakel de **diagnose van AppInsights inschakelen** selectievakje. 

   [![Het selectievakje is uitgeschakeld voor het inschakelen van diagnostische gegevens](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Selecteer **Update** aan de onderkant van het scherm om de wijzigingen toe te passen. 

## <a name="enable-and-disable-from-the-sdk"></a>In- en uitschakelen van de SDK

### <a name="update-a-deployed-service"></a>Een geïmplementeerde service bijwerken
1. Identificeren van de service in uw werkruimte. De waarde voor `ws` is de naam van uw werkruimte.

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Bijwerken van uw service en Application Insights inschakelen. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Aangepaste logboektraceringen in uw service
Als u wilt dat aangepaste logtraceringen, volgt u de [standard implementatieproces voor AKS](how-to-deploy-to-aks.md). Vervolgens:

1. Het scoring-bestand bijwerken door toevoeging van afdrukken instructies.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. De AKS-configuratie bijwerken.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [De installatiekopie van het bouwen en implementeren het](how-to-deploy-to-aks.md).  

### <a name="disable-tracking-in-python"></a>Bijhouden van wijzigingen in Python uitschakelen

Als u wilt uitschakelen Application Insights, gebruik de volgende code:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Gegevens evalueren
Van uw service-gegevens worden opgeslagen in uw Application Insights-account, in dezelfde resourcegroep bevinden als uw Azure Machine Learning-service.
Om dit te bekijken:
1. Ga naar de service Machine Learning-werkruimte in de [Azure-portal](https://portal.azure.com) en klik op de koppeling van Application Insights.

    [![AppInsightsLoc](media/how-to-enable-app-insights/AppInsightsLoc.png)](./media/how-to-enable-app-insights/AppInsightsLoc.png#lightbox)

1. Selecteer de **overzicht** tabblad om te bekijken van een set van metrische gegevens voor uw service.

   [![Overzicht](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Als u wilt zoeken in uw aangepaste traceringen, selecteer **Analytics**.
4. Selecteer in de schemasectie **traceringen**. Selecteer vervolgens **uitvoeren** uw query uit te voeren. Gegevens moeten worden weergegeven in een tabelindeling en moet worden toegewezen aan uw aangepaste aanroepen in de scoring-bestand. 

   [![Aangepaste traceringen](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Zie voor meer informatie over het gebruik van Application Insights, [wat is Application Insights?](../../application-insights/app-insights-overview.md).
    

## <a name="example-notebook"></a>Voorbeeld van de notebook

De [00. aan Started/13.enable-app-insights-in-production-service.ipynb](https://github.com/Azure/MachineLearningNotebooks/tree/master/01.getting-started/13.enable-app-insights) notebook concepten in dit artikel laat zien.  Dit notitieblok ophalen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen
U kunt ook gegevens verzamelen over uw modellen in productie. Lees het artikel [verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md). 


## <a name="other-references"></a>Verwijzingen naar andere
* [Azure Monitor voor containers](https://docs.microsoft.com/azure/monitoring/monitoring-container-insights-overview?toc=%2fazure%2fmonitoring%2ftoc.json)
