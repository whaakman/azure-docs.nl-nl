---
title: Cluster met een webgebruikersinterface Azure Kubernetes beheren
description: Met behulp van de webgebruikersinterface Kubernetes in Azure Container Service
services: container-service
author: bburns
manager: timlt
ms.service: container-service
ms.topic: article
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: d5a3906e5e26c9ed0a6cf356000c3d81f00de72c
ms.sourcegitcommit: 5d3e99478a5f26e92d1e7f3cec6b0ff5fbd7cedf
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2017
---
# <a name="using-the-kubernetes-web-ui-with-azure-container-service"></a>Met behulp van de webgebruikersinterface Kubernetes met Azure Container Service

[!INCLUDE [aks-preview-redirect.md](../../../includes/aks-preview-redirect.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).


Ook wordt ervan uitgegaan dat u de Azure CLI 2.0 hebt en `kubectl` hulpprogramma's geïnstalleerd.

U kunt testen, hebt u de `az` hulpprogramma is geïnstalleerd door te voeren:

```console
$ az --version
```

Als u hebt de `az` hulpprogramma is geïnstalleerd, er zijn instructies [hier](https://github.com/azure/azure-cli#installation).

U kunt testen, hebt u de `kubectl` hulpprogramma is geïnstalleerd door te voeren:

```console
$ kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Overzicht

### <a name="connect-to-the-web-ui"></a>Verbinding maken met de webgebruikersinterface
U kunt de webgebruikersinterface Kubernetes starten door te voeren:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Dit moet een webbrowser die is geconfigureerd om te communiceren met een beveiligde proxy uw lokale computer verbinden met de webgebruikersinterface Kubernetes openen.

### <a name="create-and-expose-a-service"></a>Maken en weergeven van een service
1. Klik in het Kubernetes web UI op **maken** knop in het bovenste rechtervenster.

    ![Kubernetes maken gebruikersinterface](./media/container-service-kubernetes-ui/create.png)

    Een dialoogvenster weergegeven waarin u kunt starten maken van de toepassing.

2. Geef deze de naam `hello-nginx`. Gebruik de [ `nginx` container in Docker](https://hub.docker.com/_/nginx/) en drie replica's van deze webservice implementeren.

    ![Dialoogvenster maken Kubernetes schil](./media/container-service-kubernetes-ui/nginx.png)

3. Onder **Service**, selecteer **externe** en voert u poort 80.

    Deze instelling taakverdelingen verkeer naar de drie replica's.

    ![Dialoogvenster maken Kubernetes Service](./media/container-service-kubernetes-ui/service.png)

4. Klik op **implementeren** voor het implementeren van deze containers en -services.

    ![Kubernetes implementeren](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Uw containers weergeven
Nadat u op **implementeren**, ziet u de gebruikersinterface een weergave van uw service omdat het wordt geïmplementeerd:

![Kubernetes Status](./media/container-service-kubernetes-ui/status.png)

U kunt de status van elk object Kubernetes in de cirkel onder aan de linkerkant van de gebruikersinterface zien **gehele product**. Als het een gedeeltelijk volledige cirkel, klikt u vervolgens implementeert het object nog steeds. Wanneer een object volledig is geïmplementeerd, wordt een groen vinkje weergegeven:

![Kubernetes geïmplementeerd](./media/container-service-kubernetes-ui/deployed.png)

Zodra u alles wordt uitgevoerd, klikt u op een van uw gehele product voor informatie over de webservice.

![Kubernetes gehele product](./media/container-service-kubernetes-ui/pods.png)

In de **gehele product** weergave ziet u informatie over de containers in schil, evenals de CPU en geheugen resources gebruikt door deze containers:

![Kubernetes Resources](./media/container-service-kubernetes-ui/resources.png)

Als u de resources niet ziet, moet u wellicht wacht een paar minuten voordat de bewakingsgegevens worden doorgegeven.

Klik op een overzicht van de logboeken voor de container **logboeken bekijken**.

![Kubernetes Logboeken](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Uw service weergeven
De UI Kubernetes gecreëerd naast uw containers wordt uitgevoerd, een externe `Service` die voorziet in een load balancer om verkeer naar de containers in uw cluster.

Klik in het navigatiedeelvenster links op **Services** om weer te geven van alle services (er moet slechts één).

![Kubernetes Services](./media/container-service-kubernetes-ui/service-deployed.png)

In deze weergave ziet u een extern eindpunt (IP-adres) dat is toegewezen aan uw service.
Als u op dat IP-adres, ziet u het Nginx-container die achter de load balancer.

![nginx weergeven](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Uw service vergroten of verkleinen
U kunt niet alleen uw objecten in de gebruikersinterface weergeven, bewerken en de objecten Kubernetes API bijwerken.

Klik eerst **implementaties** in het navigatiedeelvenster links om te zien van de implementatie voor uw service.

Wanneer u zich in die weergave, klikt u op de replicaset en klik vervolgens op **bewerken** in de bovenste navigatiebalk:

![Kubernetes bewerken](./media/container-service-kubernetes-ui/edit.png)

Bewerk de `spec.replicas` veld `2`, en klik op **Update**.

Dit zorgt ervoor dat het aantal replica's op twee door een van uw gehele product te verwijderen.

 

