---
title: Application insights voor Azure Machine Learning-service in productie inschakelen
description: Informatie over het instellen van Application Insights voor Azure Machine Learning-Service die zijn geïmplementeerd in een Azure Kubernetes Service
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 10/01/2018
ms.openlocfilehash: 8fbddd7bca8750317490c35473730ff11f07374c
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48242623"
---
# <a name="monitor-your-azure-machine-learning-models-in-production-with-application-insights"></a>Uw Azure Machine Learning-modellen in productie met Application Insights bewaken

In dit artikel leert u hoe u voor het instellen van **Application Insights** voor uw **Azure Machine Learning** service. Eenmaal is ingeschakeld, Application Insights biedt u de mogelijkheid om te controleren:
* Tarieven, reactietijden en foutpercentages aanvragen
* Afhankelijkheidsrelaties, reactietijden en foutpercentages
* Uitzonderingen

Meer informatie over Application Insights [hier](../../application-insights/app-insights-overview.md). 

## <a name="prerequisites"></a>Vereisten
* Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.
* Een Azure Machine Learning-werkruimte en een lokale map met uw scripts en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.
* Een getraind model voor machine learning om te worden geïmplementeerd naar Azure Kubernetes Service (AKS). Als u niet hebt, raadpleegt u de [classificeringsmodel voor de installatiekopie van de trein](tutorial-train-models-with-aml.md) zelfstudie.
* Een [AKS-cluster](how-to-deploy-to-aks.md).

## <a name="enable--disable-in-the-portal"></a>& Uitschakelen in de portal inschakelen

U kunt in- en uitschakelen van Application Insights in Azure portal.

### <a name="enable"></a>Inschakelen

1. In [Azure-portal](https://portal.azure.com), opent u uw werkruimte.

1. Ga in uw implementaties en selecteert u de service waar u Application Insights inschakelen.

   [![Implementaties](media/how-to-enable-app-insights/Deployments.PNG)](./media/how-to-enable-app-insights/Deployments.PNG#lightbox)

3. Klik op **bewerken** en Ga naar **geavanceerde instellingen**.

   [![Bewerken](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

4. In **geavanceerde instellingen** Selecteer **diagnostische gegevens van Application Insights inschakelen**.

   [![Bewerken](media/how-to-enable-app-insights/AdvancedSettings.png)](./media/how-to-enable-app-insights/AdvancedSettings.png)#lightbox)

1. Selecteer **Update** aan de onderkant van het scherm om de wijzigingen toe te passen. 

### <a name="disable"></a>Uitschakelen
Als u wilt uitschakelen Application Insights in Azure portal, moet u de volgende stappen uitvoeren:

1. Meld u aan bij Azure portal op https://portal.azure.com.
1. Ga naar uw werkruimte.
1. Kies **implementaties**, klikt u vervolgens **Selecteer service**, en vervolgens **bewerken**.

   [![Bewerken](media/how-to-enable-app-insights/Edit.PNG)](./media/how-to-enable-app-insights/Edit.PNG#lightbox)

1. In **geavanceerde instellingen**, schakelt u de optie **diagnose van AppInsights inschakelen**. 

   [![Schakel het selectievakje](media/how-to-enable-app-insights/uncheck.png)](./media/how-to-enable-app-insights/uncheck.png#lightbox)

1. Selecteer **Update** aan de onderkant van het scherm om de wijzigingen toe te passen. 

## <a name="enable--disable-from-the-sdk"></a>Inschakelen en uitschakelen van de SDK

### <a name="update-a-deployed-service"></a>Een geïmplementeerde service bijwerken
1. Identificeren van de service in uw werkruimte (ws = naam van uw werkruimte)

    ```python
    aks_service= Webservice(ws, "my-service-name")
    ```
2. Bijwerken van uw service en Application Insights inschakelen. 

    ```python
    aks_service.update(enable_app_insights=True)
    ```

### <a name="log-custom-traces-in-your-service"></a>Aangepaste logboektraceringen in uw service
Als u wilt dat aangepaste logtraceringen, volgt u de [standard implementatieproces voor AKS](how-to-deploy-to-aks.md) en kunt u:

1. Scoring-bestand bijwerken door toevoeging van afdrukken instructies.
    
    ```python
    print ("model initialized" + time.strftime("%H:%M:%S"))
    ```

2. Aks-configuratie bijwerken.
    
    ```python
    aks_config = AksWebservice.deploy_configuration(enable_app_insights=True)
    ```

3. [Installatiekopie maken en deze implementeren.](how-to-deploy-to-aks.md)  

### <a name="disable-tracking-in-python"></a>Bijhouden van wijzigingen in Python uitschakelen

Als u wilt uitschakelen Application Insights, gebruik de volgende code:

```python 
## replace <service_name> with the name of the web service
<service_name>.update(enable_app_insights=False)
```
    

## <a name="evaluate-data"></a>Gegevens evalueren
De gegevens van uw service wordt opgeslagen in uw Application Insights-account in de werkruimte van uw Azure Machine Learning-service wordt op dezelfde resourcegroep.
Om dit te bekijken:
1. Ga naar de resourcegroep in de [Azure-portal](https://portal.azure.com) en klikt u op in uw Application Insights-resource. 
2. De **overzicht** tabblad ziet u de standaardset van metrische gegevens voor uw service.

   [![Overzicht](media/how-to-enable-app-insights/overview.png)](./media/how-to-enable-app-insights/overview.png#lightbox)

3. Om te zoeken in uw aangepaste traceringen klikt u op **Analytics**.
4. Klik in de schemasectie op **traceringen** en vervolgens **uitvoeren** uw query. Gegevens moeten worden weergegeven in een tabelindeling omlaag hieronder en moet worden toegewezen aan uw aangepaste aanroepen in de scoring-bestand. 

   [![Aangepaste traceringen](media/how-to-enable-app-insights/logs.png)](./media/how-to-enable-app-insights/logs.png#lightbox)

Klik op [hier](../../application-insights/app-insights-overview.md) voor meer informatie over het gebruik van Application Insights.
    

## <a name="example-notebook"></a>Voorbeeld van de Notebook

De `00.Getting Started/13.enable-app-insights-in-production-service.ipynb` notebook concepten in dit artikel laat zien.  Dit notitieblok ophalen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]

## <a name="next-steps"></a>Volgende stappen
U kunt ook gegevens verzamelen over uw modellen in productie. Lees het artikel [verzamelen van gegevens voor modellen in productie](how-to-enable-data-collection.md) 
