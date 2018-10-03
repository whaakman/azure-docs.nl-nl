---
title: Gegevensverzameling inschakelen voor modellen in productie - Azure Machine Learning
description: Meer informatie over het verzamelen van Azure Machine Learning invoermodel gegevens in een Azure Blob-opslag.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: marthalc
author: marthalc
ms.date: 09/24/2018
ms.openlocfilehash: 61776c5122abb73dd34ef2ad11f6a44ab22f6a6b
ms.sourcegitcommit: 1981c65544e642958917a5ffa2b09d6b7345475d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/03/2018
ms.locfileid: "48238835"
---
# <a name="collect-data-for-models-in-production"></a>Verzamelen van gegevens voor modellen in productie

In dit artikel leert u hoe u invoer om modelgegevens te verzamelen via de Azure Machine Learning-services die u hebt geïmplementeerd in Azure Kubernetes Cluster (AKS) in een Azure Blob-opslag. 

Eenmaal is ingeschakeld, wordt deze gegevens die u verzamelt helpt u:
* Groepen gegevens bewaken tijdens de productiegegevens krijgt uw model

* Neem betere beslissingen op het opnieuw trainen of het model optimaliseren

* Opnieuw trainen van uw model met de verzamelde gegevens

## <a name="what-is-collected-and-where-does-it-go"></a>Wat wordt er verzameld en waar gaat het?

De volgende gegevens kunnen worden verzameld:
* Model **invoer** gegevens van webservices die zijn geïmplementeerd in Azure Kubernetes Cluster (AKS) (spraak, afbeeldingen en video zijn **niet** verzameld) 
  
* Model voorspellingen met invoergegevens van de productie.

> [!Note]
> Vooraf aggregatie of vooraf berekeningen op deze gegevens zijn niet deel uit van de service op dit moment.   

De uitvoer wordt opgeslagen in een Azure-Blob. Omdat de gegevens in een Azure-Blob wordt toegevoegd, kunt u vervolgens uw favoriete hulpprogramma voor de analyse uitvoeren. 

Het pad naar de uitvoergegevens in de blob met de volgende deze syntaxis:

```
/modeldata/<subscriptionid>/<resourcegroup>/<workspace>/<webservice>/<model>/<version>/<identifier>/<year>/<month>/<day>/data.csv
# example: /modeldata/1a2b3c4d-5e6f-7g8h-9i10-j11k12l13m14/myresourcegrp/myWorkspace/aks-w-collv9/best_model/10/inputs/2018/12/31/data.csv
```

## <a name="prerequisites"></a>Vereisten

- Een Azure-abonnement. Als u nog geen abonnement hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) voordat u begint.

- Een Azure Machine Learning-service-werkruimte en een lokale map met uw scripts en de Azure Machine Learning-SDK voor Python geïnstalleerd. Informatie over het verkrijgen van deze vereisten met behulp van de [het configureren van een ontwikkelomgeving](how-to-configure-environment.md) document.

- Een getraind model voor machine learning om te worden geïmplementeerd naar Azure Kubernetes Service (AKS). Als u niet hebt, raadpleegt u de [classificeringsmodel voor de installatiekopie van de trein](tutorial-train-models-with-aml.md) zelfstudie.

- Een [AKS-cluster](how-to-deploy-to-aks.md).

- De volgende module geïnstalleerd en afhankelijkheden [in uw omgeving](how-to-configure-environment.md):
  + Op Linux:
    ```shell
    sudo apt-get install libxml++2.6-2v5
    pip install azureml-monitoring
    ```

  + In Windows:
    ```shell
    pip install azureml-monitoring
    ```

## <a name="enable-data-collection"></a>Gegevensverzameling inschakelen
Het verzamelen van gegevens kan worden ingeschakeld, ongeacht het model wordt geïmplementeerd via Azure Machine Learning-Service of andere hulpprogramma's. 

Als u wilt inschakelen, moet u naar:

1. Open het scoring-bestand. 

1. Voeg de volgende code toe aan de bovenkant van het bestand:

   ```python 
   from azureml.monitoring import ModelDataCollector
   ```

2. Declareer de variabelen van uw gegevens verzameling in uw `init()` functie:

    ```python
    global inputs_dc, prediction_dc
    inputs_dc = ModelDataCollector("best_model", identifier="inputs", feature_names=["feat1", "feat2", "feat3". "feat4", "feat5", "feat6"])
    prediction_dc = ModelDataCollector("best_model", identifier="predictions", feature_names=["prediction1", "prediction2"])
    ```

    *CorrelationId* is een optionele parameter, u hoeft niet instellen als uw model niet worden vereist. Met een correlationId in plaats, helpt u om eenvoudiger met andere gegevens. (Voorbeelden zijn onder meer: LoanNumber, klantnummer, enz.)
    
    *Id* wordt later gebruikt voor het bouwen van de mappenstructuur in uw Blob, kan worden gebruikt om te delen 'raw' gegevens ten opzichte van "verwerkt".

3.  Voeg de volgende regels code aan de `run(input_df)` functie:

    ```python
    data = np.array(data)
    result = model.predict(data)
    inputs_dc.collect(data) #this call is saving our input data into Azure Blob
    prediction_dc.collect(result) #this call is saving our input data into Azure Blob
    ```

4. Verzamelen van gegevens is **niet** automatisch ingesteld op **waar** wanneer u een service in AKS implementeert, dus u moet uw configuratiebestand bijwerken zoals: 

    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True)
    ```
    Application Insights voor het bewaken van de service kan ook worden ingeschakeld door deze configuratie te wijzigen:
    ```python
    aks_config = AksWebservice.deploy_configuration(collect_model_data=True, enable_app_insights=True)
    ``` 

5. [Nieuwe installatiekopie maken en implementeren van uw service.](how-to-deploy-to-aks.md) 


Als u al een service met de afhankelijkheden die zijn geïnstalleerd hebt uw **omgevingsbestand** en **scoring-bestand**, verzamelen van gegevens door inschakelen:

1. Ga naar [Azure-Portal](https://portal.azure.com).

1. Open de werkruimte.

1. Ga naar **implementaties** -> **Selecteer service** -> **bewerken**.

   ![Service bewerken](media/how-to-enable-data-collection/EditService.png)

1. In **geavanceerde instellingen**, schakel **inschakelen modelgegevensverzameling**. 

   ![Schakel het verzamelen van gegevens](media/how-to-enable-data-collection/CheckDataCollection.png)

   In dit venster, u kunt ook 'Diagnose van Appinsights inschakelen' om bij te houden van de status van uw service.  

1. Selecteer **Update** de wijziging toepassen.


## <a name="disable-data-collection"></a>Gegevens verzamelen uitschakelen
U kunt stoppen met het verzamelen van gegevens elk gewenst moment. Python-code of de Azure-portal gebruiken om te verzamelen van gegevens uitschakelen.

+ Optie 1 - uitschakelen in Azure portal: 
  1. Meld u aan bij de [Azure-portal](https://portal.azure.com).

  1. Open de werkruimte.

  1. Ga naar **implementaties** -> **Selecteer service** -> **bewerken**.

     ![Service bewerken](media/how-to-enable-data-collection/EditService.png)

  1. In **geavanceerde instellingen**, schakel **inschakelen modelgegevensverzameling**. 

     ![Schakel het verzamelen van gegevens](media/how-to-enable-data-collection/UncheckDataCollection.png) 

  1. Selecteer **Update** de wijziging toepassen.

* Optie 2: Python gebruiken om te verzamelen van gegevens uitschakelen:

  ```python 
  ## replace <service_name> with the name of the web service
  <service_name>.update(collect_model_data=False)
  ```

## <a name="example-notebook"></a>Voorbeeld van de notebook

De `00.Getting Started/12.enable-data-collection-for-models-in-aks.ipynb` notebook concepten in dit artikel laat zien.  

Dit notitieblok ophalen:
 
[!INCLUDE [aml-clone-in-azure-notebook](../../../includes/aml-clone-for-examples.md)]