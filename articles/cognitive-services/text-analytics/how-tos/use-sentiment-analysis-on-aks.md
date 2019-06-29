---
title: Azure Kubernetes-Services worden uitgevoerd
titleSuffix: Text Analytics - Azure Cognitive Services
description: De text analytics-containers met de installatiekopie van het sentiment-analyse met de Azure Kubernetes-Services implementeren en testen in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 4d5e1da01be531550915a38bed17dd8e57be907a
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67454948"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-services-aks"></a>Een Sentimentanalyse-container implementeren op Azure Kubernetes Services (AKS)

Informatie over het implementeren van de Cognitive Services [Tekstanalyse](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) container met de installatiekopie van het Sentimentanalyse voor Azure Kubernetes Services (AKS). Deze procedure verklaart het maken van een Text Analytics-resource, het maken van een bijbehorende Sentimentanalyse-installatiekopie en de mogelijkheid om uit te oefenen deze indeling van de twee vanuit een browser. Met behulp van containers, kan de ontwikkelaar aandacht weg van beheer van infrastructuur in plaats daarvan gericht op de ontwikkeling van toepassingen verplaatsen.

## <a name="prerequisites"></a>Vereisten

Deze procedure moet diverse hulpprogramma's die moeten worden geïnstalleerd en lokaal uitvoeren. Gebruik niet Azure Cloud shell.

* Gebruik een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Teksteditor, bijvoorbeeld: [Visual Studio Code](https://code.visualstudio.com/download).
* Installeer de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
* Installeer de [Kubernetes-CLI](https://kubernetes.io/docs/tasks/tools/install-kubectl/).
* Een Azure-resource met de juiste prijscategorie. Niet alle Prijscategorieën werken met deze container:
    * **Tekstanalyse** resource met F0 of standaardprijzen alleen lagen.
    * **Cognitive Services** resource met de S0 prijscategorie.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics Containers on Azure Kubernetes Services (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-text-analytics-container-to-an-aks-cluster"></a>Text Analytics-Container implementeren in een AKS-Cluster

1. Open de Azure CLI en meld u aan bij Azure

    ```azurecli
    az login
    ```

1. Aanmelden bij het AKS-cluster (vervangen door de `your-cluster-name` en `your-resource-group` met de juiste waarden)

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Nadat u deze opdracht wordt uitgevoerd, wordt een bericht dat lijkt op het volgende:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Als u meerdere abonnementen beschikbaar in uw Azure-account hebt en de `az aks get-credentials` opdracht met een fout retourneert, een veelvoorkomend probleem is dat u het juiste abonnement. Eenvoudig instellen van de context van het gebruik van hetzelfde abonnement dat u hebt gemaakt met de resources met Azure CLI-sessie en probeer het opnieuw.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Open de teksteditor naar keuze, (in dit voorbeeld wordt __Visual Studio Code__):

    ```azurecli
    code .
    ```

1. Maak een nieuw bestand met de naam in de teksteditor _sentiment.yaml_ en plak de volgende YAML erin:

    ```yaml
    apiVersion: apps/v1beta1
    kind: Deployment
    metadata:
      name: sentiment
    spec:
      template:
        metadata:
          labels:
            app: sentiment-app
        spec:
          containers:
          - name: sentiment
            image: mcr.microsoft.com/azure-cognitive-services/sentiment
            ports:
            - containerPort: 5000
            env:
            - name: EULA
              value: "accept"
            - name: billing
              value: "https://westus2.api.cognitive.microsoft.com/"
            - name: apikey
              value: "16c12e3419f54ba49a3222177cef781d"
     
    --- 
    apiVersion: v1
    kind: Service
    metadata:
      name: sentiment
    spec:
      type: LoadBalancer
      ports:
      - port: 5000
      selector:
        app: sentiment-app
    ```

1. Sla het bestand op en sluit de teksteditor.
1. Uitvoeren van de Kubernetes `apply` opdracht met de _sentiment.yaml_ als het doel:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Nadat u de opdracht heeft de implementatieconfiguratie is toegepast, wordt een bericht dat lijkt op de volgende uitvoer:

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Controleer of de schil is geïmplementeerd:

    ```console
    kubectl get pods
    ```

    Hiermee wordt de actieve status van de schil uitvoer:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Controleer of de service beschikbaar is en het IP-adres ophalen:

    ```console
    kubectl get services
    ```

    Dit wordt de actieve status van de uitvoer de _sentiment_ service in de schil:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the Sentiment Analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Volgende stappen

* Meer [Cognitive Services-Containers](../../cognitive-services-container-support.md)
* Gebruik de [Tekstanalyse verbonden Service](../vs-text-connected-service.md)