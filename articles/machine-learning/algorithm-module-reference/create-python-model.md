---
title: 'Python-Model maken: Moduleverwijzing'
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van het model van de Python-Model maken in Azure Machine Learning-service te maken van aangepaste modellen of gegevensverwerking module.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 05/06/2019
ROBOTS: NOINDEX
ms.openlocfilehash: ea9b50cede3e2d264ca0476b6a987ca6896c3c79
ms.sourcegitcommit: 4b9c06dad94dfb3a103feb2ee0da5a6202c910cc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/02/2019
ms.locfileid: "65029743"
---
# <a name="create-python-model"></a>Python-Model maken

In dit artikel wordt beschreven hoe u de **Python-Model maken** module voor het maken van een ongetrainde model uit een Python-script. 

U kunt het model baseren op een cursist die is opgenomen in een Python-pakket in de Azure Machine Learning-omgeving. 

Nadat u het model hebt gemaakt, kunt u [Train Model](train-model.md) trainen het model op een gegevensset, zoals elke andere cursist in Azure Machine Learning. Het getrainde model kan worden doorgegeven aan [Score Model](score-model.md) het model gebruiken om voorspellingen te doen. Het getrainde model vervolgens kan worden opgeslagen en de scoring werkstroom kan worden gepubliceerd als een webservice.

> [!WARNING]
> Het is momenteel niet mogelijk om door te geven van de beoordeelde resultaten van een Python-model te [Evaluate Model](evaluate-model.md). Als u een model te evalueren wilt, kunt u aangepaste Python en voer het script met behulp van de [Execute Python Script](execute-python-script.md) module.  


## <a name="how-to-configure-create-python-model"></a>Het configureren van Python-Model maken

Gebruik van deze module vereist tussenliggende of deskundige kennis van Python. De module biedt ondersteuning voor gebruik van een cursist die is opgenomen in de Python-pakketten die al is geïnstalleerd in Azure Machine Learning. Zie lijst met vooraf geïnstalleerde Python-pakket in [Execute Python Script](execute-python-script.md).
  

Dit artikel wordt beschreven hoe u de **Python-Model maken** met een eenvoudig experiment. Hieronder vindt u de grafiek van het experiment.

![maken-python-model](./media/module/aml-create-python-model.png)

1.  Klik op **Python-Model maken**, bewerkt u het script voor het implementeren van uw modellen of gegevensbeheer verwerken. U kunt het model baseren op een cursist die is opgenomen in een Python-pakket in de Azure Machine Learning-omgeving.


    Hieronder volgt een voorbeeldcode van twee klassen Naive Bayes classificatie met behulp van de populaire *sklearn* pakket.

```Python

# The script MUST define a class named AzureMLModel.
# This class MUST at least define the following three methods:
    # __init__: in which self.model must be assigned,
    # train: which trains self.model, the two input arguments must be pandas DataFrame,
    # predict: which generates prediction result, the input argument and the prediction result MUST be pandas DataFrame.
# The signatures (method names and argument names) of all these methods MUST be exactly the same as the following example.


import pandas as pd
from sklearn.naive_bayes import GaussianNB


class AzureMLModel:
    def __init__(self):
        self.model = GaussianNB()
        self.feature_column_names = list()

    def train(self, df_train, df_label):
        self.feature_column_names = df_train.columns.tolist()
        self.model.fit(df_train, df_label)

    def predict(self, df):
        return pd.DataFrame(
            {'Scored Labels': self.model.predict(df[self.feature_column_names]), 
             'probabilities': self.model.predict_proba(df[self.feature_column_names])[:, 1]}
        )


```


2. Verbinding maken met de **Python-Model maken** module die u zojuist hebt gemaakt naar een **Train Model** en **Score-Model**

3. Als u evalueren van het model wilt, voegt u toe een [Execute Python Script](execute-python-script.md) en bewerken van het Python-script voor het implementeren van evaluatie.

Hieronder vindt u een evaluatie-voorbeeldcode.

```Python


# The script MUST contain a function named azureml_main
# which is the entry point for this module.

# imports up here can be used to 
import pandas as pd

# The entry point function can contain up to two input arguments:
#   Param<dataframe1>: a pandas.DataFrame
#   Param<dataframe2>: a pandas.DataFrame
def azureml_main(dataframe1 = None, dataframe2 = None):
    
    from sklearn.metrics import accuracy_score, precision_score, recall_score, roc_auc_score, roc_curve
    import pandas as pd
    import numpy as np
    
    scores = dataframe1.ix[:, ("income", "Scored Labels", "probabilities")]
    ytrue = np.array([0 if val == '<=50K' else 1 for val in scores["income"]])
    ypred = np.array([0 if val == '<=50K' else 1 for val in scores["Scored Labels"]])    
    probabilities = scores["probabilities"]
    
    accuracy, precision, recall, auc = \
    accuracy_score(ytrue, ypred),\
    precision_score(ytrue, ypred),\
    recall_score(ytrue, ypred),\
    roc_auc_score(ytrue, probabilities)
    
    metrics = pd.DataFrame();
    metrics["Metric"] = ["Accuracy", "Precision", "Recall", "AUC"];
    metrics["Value"] = [accuracy, precision, recall, auc]
    
    return metrics,

```
