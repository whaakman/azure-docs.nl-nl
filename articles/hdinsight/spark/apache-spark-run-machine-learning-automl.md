---
title: Azure Machine Learning-workloads uitvoeren met geautomatiseerde machine learning-(AutoML) op Apache Spark in Azure HDInsight
description: Leer hoe u Azure Machine Learning-workloads uitvoeren met geautomatiseerde machine learning-(AutoML) op Apache Spark in Azure HDInsight.
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.service: hdinsight
ms.topic: conceptual
ms.date: 01/14/2019
ms.openlocfilehash: 5135de0fc87af227073f96c653d928ace1a50fd0
ms.sourcegitcommit: d4dfbc34a1f03488e1b7bc5e711a11b72c717ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/13/2019
ms.locfileid: "64917034"
---
# <a name="run-azure-machine-learning-workloads-with-automated-machine-learning-automl-on-apache-spark-in-azure-hdinsight"></a>Azure Machine Learning-workloads uitvoeren met geautomatiseerde machine learning-(AutoML) op Apache Spark in Azure HDInsight

Azure Machine Learning vereenvoudigt en versnelt de gebouw, training en implementatie van machine learning-modellen. In geautomatiseerde machine learning-(AutoML), moet u beginnen met trainingsgegevens waarvoor een gedefinieerd doel-functie en vervolgens doorlopen en combinaties van algoritmen en functie van selecties automatisch het beste model selecteren voor uw gegevens op basis van de scores training. HDInsight kan klanten voor het inrichten van clusters met honderden knooppunten. AutoML op Spark wordt uitgevoerd in een HDInsight-cluster kan gebruikers het gebruik van rekencapaciteit op deze knooppunten trainingstaken uitvoeren op een uitbreidbare manier, en om meerdere trainingstaken parallel uit te voeren. Hierdoor kunnen gebruikers AutoML experimenten uitvoeren terwijl u de compute deelt met hun big data-workloads.
 

## <a name="install-azure-machine-learning-on-an-hdinsight-cluster"></a>Azure Machine Learning installeren op een HDInsight-cluster

Raadpleeg voor algemene zelfstudies van geautomatiseerde machine learning, [zelfstudie: Gebruik automatische machine learning om uw regressiemodel](../../machine-learning/service/tutorial-auto-train-models.md).
Alle nieuwe HDInsight Spark-clusters worden geleverd vooraf worden geïnstalleerd met AzureML-AutoML SDK. U kunt aan de slag met AutoML op HDInsight met deze [voorbeeld Jupyter-notebook](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/azure-hdi). Deze Jupyter-Notebook ziet u hoe u van een geautomatiseerde machine learning-classificatie voor een eenvoudige indeling.

> [!Note]
> Azure Machine Learning-pakketten zijn geïnstalleerd in Python3 conda-omgeving. De geïnstalleerde Jupyter-notebook moet worden uitgevoerd met behulp van de kernel PySpark3.

U kunt ook Zeppelin-notebooks gebruiken AutoML ook gebruiken.

> [!Note]
> Zeppelin is een [bekende probleem](https://community.hortonworks.com/content/supportkb/207822/the-livypyspark3-interpreter-uses-python-2-instead.html) waar PySpark3 de juiste versie van Python niet ophalen. Gebruik de gedocumenteerde tijdelijke oplossing.

## <a name="authentication-for-workspace"></a>Verificatie voor werkruimte

Werkruimte maken en experiment verzenden vereisen een verificatietoken. Dit token kan worden gegenereerd met een [Azure AD-toepassing](../../active-directory/develop/app-objects-and-service-principals.md). Een [Azure AD-gebruiker](https://docs.microsoft.com/python/azure/python-sdk-azure-authenticate?view=azure-python) kan ook worden gebruikt voor het genereren van het verificatietoken vereist als multi-factor authentication is niet ingeschakeld voor het account.  

Het volgende codefragment maakt u een verificatie-token met een **Azure AD-toepassing**.

```python
from azureml.core.authentication import ServicePrincipalAuthentication
auth_sp = ServicePrincipalAuthentication(
                tenant_id = '<Azure Tenant ID>',
                service_principal_id = '<Azure AD Application ID>',
                service_principal_password = '<Azure AD Application Key>'
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

In de [geautomatiseerde machine learning-configuratie](https://docs.microsoft.com/python/api/azureml-train-automl/azureml.train.automl.automlconfig), de eigenschap `spark_context` moet worden ingesteld voor het pakket uit te voeren op de gedistribueerde modus. De eigenschap `concurrent_iterations`, dit is het maximum aantal iteraties parallel worden uitgevoerd, moet worden ingesteld op een getal kleiner dan de executor-cores voor de Spark-app.

## <a name="next-steps"></a>Volgende stappen

* Zie voor meer informatie over de reden achter automatische leerprocessen [Release modellen op gelijke tred met behulp van Microsoft machine learning geautomatiseerde!](https://azure.microsoft.com/blog/release-models-at-pace-using-microsoft-s-automl/)
* Zie voor meer informatie over het gebruik van Azure ML geautomatiseerde ML-mogelijkheden [nieuwe geautomatiseerde mogelijkheden voor machine learning in Azure Machine Learning-service](https://azure.microsoft.com/blog/new-automated-machine-learning-capabilities-in-azure-machine-learning-service/)
* [AutoML project van Microsoft Research](https://www.microsoft.com/research/project/automl/)
