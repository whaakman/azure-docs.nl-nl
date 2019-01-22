---
title: Azure Machine Learning-workloads uitvoeren met geautomatiseerde machine learning-(AutoML) op Apache Spark in Azure HDInsight
description: Leer hoe u Azure Machine Learning-workloads uitvoeren met geautomatiseerde machine learning-(AutoML) op Apache Spark in Azure HDInsight.
services: hdinsight
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: howto
ms.date: 01/14/2019
ms.openlocfilehash: 58852ee5de97bef043f8c0b67d9e90cdc057912a
ms.sourcegitcommit: 9999fe6e2400cf734f79e2edd6f96a8adf118d92
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/22/2019
ms.locfileid: "54440220"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Azure Machine Learning-workloads uitvoeren met geautomatiseerde machine learning-(AutoML) op Apache Spark in Azure HDInsight

Azure Machine Learning is een analyseplatform voor samenwerking, slepen-en-neerzetten-hulpprogramma die kunt u bouwen, testen en implementeren van predictive analytics-oplossingen voor uw gegevens. Azure Machine Learning publiceert modellen als webservices die eenvoudig kunnen worden gebruikt door aangepaste apps of BI-hulpprogramma's zoals Excel. Geautomatiseerde machine learning-(AutoML) kunt maken van hoge kwaliteit machine learning-modellen met behulp van intelligente automatisering en optimalisatie. AutoML besluit het juiste algoritme en hyper parameters voor specifieke typen.

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Azure Machine Learning installeren op een HDInsight-cluster

> [!Note]
> De Azure ML-werkruimte is momenteel beschikbaar in de volgende regio's: VS-Oost, eastus2 en westcentralus. Het HDInsight-cluster moet ook worden gemaakt in een van deze regio's.

Raadpleeg voor algemene zelfstudies van Azure Machine Learning en geautomatiseerde machine learning, [zelfstudie: Uw eerste gegevenswetenschapexperiment maken in Azure Machine Learning Studio](../../machine-learning/studio/create-experiment.md) en [zelfstudie: Gebruik automatische machine learning om uw regressiemodel](../../machine-learning/service/tutorial-auto-train-models.md).
AzureML installeren op uw Azure HDInsight-cluster, voert u het script action - [install_aml](https://commonartifacts.blob.core.windows.net/automl/install_aml.sh) - op de hoofdknooppunten en het worker-knooppunten van een cluster met HDInsight 3.6 Spark 2.3.0 (aanbevolen). Deze scriptactie kan worden uitgevoerd als onderdeel van de cluster-proces voor het maken of op een bestaand cluster via Azure portal.

Lees voor meer informatie over scriptacties [aanpassen Linux gebaseerde HDInsight-clusters met scriptacties](../hdinsight-hadoop-customize-cluster-linux.md). Naast het installeren van Azure Machine Learning-pakketten en afhankelijkheden, downloadt het script ook een voorbeeld van een Jupyter-Notebook (via pad `HdiNotebooks/PySpark` van het standaardarchief). Deze Jupyter-Notebook ziet u hoe u van een geautomatiseerde machine learning-classificatie voor een eenvoudige indeling.

> [!Note]
> Azure Machine Learning-pakketten zijn geïnstalleerd in Python3 conda-omgeving. De geïnstalleerde Jupyter-notebook moet worden uitgevoerd met behulp van de kernel PySpark3.

## <a name="authentication-for-workspace"></a>Verificatie voor werkruimte

Werkruimte maken en experiment verzenden vereisen een verificatietoken. Dit token kan worden gegenereerd met een [Azure AD-toepassing](../../active-directory/develop/app-objects-and-service-principals.md). Een [Azure AD-gebruiker](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) kan ook worden gebruikt voor het genereren van het verificatietoken vereist als multi-factor authentication is niet ingeschakeld voor het account.  

Het volgende codefragment maakt u een verificatie-token met een **Azure AD-toepassing**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                username = '<Azure AD Application ID>',
                password = '<Azure AD Application Key>'
                )
```
Het volgende codefragment maakt u een verificatie-token met een **Azure AD-gebruiker**.

```python
from azure.common.credentials import UserPassCredentials
credentials = UserPassCredentials('user@domain.com','my_smart_password')
```

## <a name="loading-dataset"></a>Het laden van gegevensset

Machine learning in Spark maakt gebruik van geautomatiseerde **gegevensstromen**, welke lazily geëvalueerd, onveranderbare bewerkingen op gegevens zijn.  Een gegevensstroom kan een gegevensset laden van een blob met openbare leestoegang, of van een blob-URL met een SAS-token.

```python
import azureml.dataprep as dprep

dataflow_public = dprep.read_csv(path='https://commonartifacts.blob.core.windows.net/automl/UCI_Adult_train.csv')

dataflow_with_token = dprep.read_csv(path='https://dpreptestfiles.blob.core.windows.net/testfiles/read_csv_duplicate_headers.csv?st=2018-06-15T23%3A01%3A42Z&se=2019-06-16T23%3A01%3A00Z&sp=r&sv=2017-04-17&sr=b&sig=ugQQCmeC2eBamm6ynM7wnI%2BI3TTDTM6z9RPKj4a%2FU6g%3D')
```

U kunt ook het gegevensarchief registreren met de werkruimte met behulp van een eenmalige registratie.

## <a name="experiment-submission"></a>Verzending van experiment

In de configuratie van geautomatiseerde machine learning, de eigenschap `spark_context` moet worden ingesteld voor het pakket uit te voeren op de gedistribueerde modus. De eigenschap `concurrent_iterations`, dit is het maximum aantal iteraties parallel worden uitgevoerd, moet worden ingesteld op een getal kleiner dan de executor-cores voor de Spark-app.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de reden achter automatische leerprocessen [Release modellen op gelijke tred met behulp van Microsoft machine learning geautomatiseerde!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/).
* [AutoML project van Microsoft Research](https://www.microsoft.com/research/project/automl/)