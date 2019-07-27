---
title: Azure Kubernetes-service uitvoeren-Text Analytics
titleSuffix: Azure Cognitive Services
description: Implementeer de Text Analytics-containers met de sentiment-analyse kopie naar de Azure Kubernetes-service en test deze in een webbrowser.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 06/21/2019
ms.author: dapine
ms.openlocfilehash: 44ee5fab5b4e8900b823453e5674fc9bdb5fe9ac
ms.sourcegitcommit: 7c4de3e22b8e9d71c579f31cbfcea9f22d43721a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/26/2019
ms.locfileid: "68552269"
---
# <a name="deploy-a-sentiment-analysis-container-to-azure-kubernetes-service"></a>Een sentiment Analysis-container implementeren in azure Kubernetes service

Meer informatie over het implementeren van de Azure Cognitive Services [Text Analytics](https://docs.microsoft.com/azure/cognitive-services/text-analytics/how-tos/text-analytics-how-to-install-containers) -container met de sentiment-analyse kopie naar Azure Kubernetes service (AKS). In deze procedure ziet u hoe u een Text Analytics resource maakt, hoe u een gekoppelde sentiment-analyse kopie maakt en hoe u deze indeling van de twee kunt volgen vanuit een browser. Door gebruik te maken van containers kunt u uw aandacht afleiden van het beheer van de infra structuur om te richten op het ontwikkelen van toepassingen.

## <a name="prerequisites"></a>Vereisten

Voor deze procedure zijn verschillende hulpprogram ma's vereist die moeten worden geïnstalleerd en lokaal worden uitgevoerd. Gebruik Azure Cloud Shell niet. U hebt het volgende nodig:

* Een Azure-abonnement. Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/) aan voordat u begint.
* Een tekst editor, bijvoorbeeld [Visual Studio code](https://code.visualstudio.com/download).
* De [Azure-cli](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) is geïnstalleerd.
* De [Kubernetes-cli](https://kubernetes.io/docs/tasks/tools/install-kubectl/) is geïnstalleerd.
* Een Azure-resource met de juiste prijs categorie. Niet alle prijs categorieën werken met deze container:
    * **Azure Text Analytics** resource met alleen F0 of Standard-prijs categorieën.
    * **Azure Cognitive Services** resource met de prijs categorie s0.

[!INCLUDE [Create a Cognitive Services Text Analytics resource](../includes/create-text-analytics-resource.md)]

[!INCLUDE [Create a Text Analytics container on Azure Kubernetes Service (AKS)](../../containers/includes/create-aks-resource.md)]

## <a name="deploy-a-text-analytics-container-to-an-aks-cluster"></a>Een Text Analytics-container implementeren in een AKS-cluster

1. Open de Azure CLI en meld u aan bij Azure.

    ```azurecli
    az login
    ```

1. Meld u aan bij het AKS-cluster. Vervang `your-cluster-name` en`your-resource-group` door de juiste waarden.

    ```azurecli
    az aks get-credentials -n your-cluster-name -g -your-resource-group
    ```

    Wanneer deze opdracht wordt uitgevoerd, wordt een bericht met de volgende strekking gerapporteerd:

    ```console
    Merged "your-cluster-name" as current context in /home/username/.kube/config
    ```

    > [!WARNING]
    > Als er meerdere abonnementen voor u beschikbaar zijn in uw Azure-account en `az aks get-credentials` de opdracht wordt geretourneerd met een fout, is een veelvoorkomend probleem dat u het verkeerde abonnement gebruikt. Stel de context van uw Azure CLI-sessie in op het gebruik van hetzelfde abonnement dat u hebt gemaakt voor de resources en probeer het opnieuw.
    > ```azurecli
    >  az account set -s subscription-id
    > ```

1. Open de gewenste tekst editor. In dit voor beeld wordt Visual Studio code gebruikt.

    ```azurecli
    code .
    ```

1. Maak in de tekst editor een nieuw bestand met de naam _sentiment. yaml_en plak de volgende YAML hierin. Vervang `billing/value` en`apikey/value` door uw eigen gegevens.

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
              value: # < Your endpoint >
            - name: apikey
              value: # < Your API Key >
     
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

1. Sla het bestand op en sluit de tekst editor.
1. Voer de opdracht `apply` Kubernetes uit met _sentiment. yaml_ als doel:

    ```console
    kuberctl apply -f sentiment.yaml
    ```

    Nadat de opdracht de implementatie configuratie heeft toegepast, wordt een bericht weer gegeven zoals in de volgende uitvoer:

    ```
    deployment.apps "sentiment" created
    service "sentiment" created
    ```
1. Controleer of de Pod is geïmplementeerd:

    ```console
    kubectl get pods
    ```

    De uitvoer voor de uitvoerings status van de Pod:

    ```
    NAME                         READY     STATUS    RESTARTS   AGE
    sentiment-5c9ccdf575-mf6k5   1/1       Running   0          1m
    ```

1. Controleer of de service beschikbaar is en haal het IP-adres op.

    ```console
    kubectl get services
    ```

    De uitvoer voor de uitvoerings status van de _sentiment_ -service in de Pod:

    ```
    NAME         TYPE           CLUSTER-IP    EXTERNAL-IP      PORT(S)          AGE
    kubernetes   ClusterIP      10.0.0.1      <none>           443/TCP          2m
    sentiment    LoadBalancer   10.0.100.64   168.61.156.180   5000:31234/TCP   2m
    ```

[!INCLUDE [Verify the sentiment analysis container instance](../includes/verify-sentiment-analysis-container.md)]

## <a name="next-steps"></a>Volgende stappen

* Meer [Cognitive Services containers](../../cognitive-services-container-support.md) gebruiken
* De [Text Analytics verbonden service](../vs-text-connected-service.md) gebruiken
