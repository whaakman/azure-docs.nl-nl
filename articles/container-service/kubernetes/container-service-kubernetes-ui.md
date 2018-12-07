---
title: (AFGESCHAFT) Beheren Azure Kubernetes-cluster met web-UI
description: Met behulp van de Kubernetes-Webinterface in Azure Container Service
services: container-service
author: bburns
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 02/21/2017
ms.author: bburns
ms.custom: mvc
ms.openlocfilehash: c3a79b2e4fab807613a54d2792f5f5b97570293b
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2018
ms.locfileid: "52996139"
---
# <a name="deprecated-using-the-kubernetes-web-ui-with-azure-container-service"></a>(AFGESCHAFT) Met Azure Container Service met behulp van de Kubernetes-Webinterface

> [!TIP]
> Zie voor de bijgewerkte versie dit artikel die gebruikmaakt van Azure Kubernetes Service, [toegang tot het Kubernetes-web-dashboard in Azure Kubernetes Service (AKS)](../../aks/kubernetes-dashboard.md).

[!INCLUDE [ACS deprecation](../../../includes/container-service-kubernetes-deprecation.md)]

## <a name="prerequisites"></a>Vereisten
In dit scenario wordt ervan uitgegaan dat u hebt [gemaakt van een Kubernetes-cluster met behulp van Azure Container Service](container-service-kubernetes-walkthrough.md).


Ook wordt ervan uitgegaan dat u de Azure CLI hebt en `kubectl` hulpprogramma's zijn geïnstalleerd.

U kunt testen als u hebt de `az` hulpprogramma is geïnstalleerd door uit te voeren:

```console
$ az --version
```

Als u geen de `az` hulpprogramma is geïnstalleerd, worden er instructies [hier](https://github.com/azure/azure-cli#installation).

U kunt testen als u hebt de `kubectl` hulpprogramma is geïnstalleerd door uit te voeren:

```console
$ kubectl version
```

Als u geen `kubectl` geïnstalleerd, u kunt uitvoeren:

```console
$ az acs kubernetes install-cli
```

## <a name="overview"></a>Overzicht

### <a name="connect-to-the-web-ui"></a>Verbinding maken met de web-UI
U kunt de Kubernetes-Webinterface starten door te voeren:

```console
$ az acs kubernetes browse -g [Resource Group] -n [Container service instance name]
```

Dit moet een webbrowser die is geconfigureerd om te communiceren met een veilige proxy verbinding te maken van uw lokale computer met de Kubernetes-Webinterface openen.

### <a name="create-and-expose-a-service"></a>Maken en een service zichtbaar maken
1. Klik in het Kubernetes-Webinterface op **maken** knop in het bovenste venster.

    ![Kubernetes maken gebruikersinterface](./media/container-service-kubernetes-ui/create.png)

    Een dialoogvenster weergegeven waarin u kunt gaan maken van uw toepassing.

2. Geef deze de naam `hello-nginx`. Gebruik de [ `nginx` -container vanuit Docker](https://hub.docker.com/_/nginx/) en drie replica's van deze webservice implementeren.

    ![Kubernetes-schil maken dialoogvenster](./media/container-service-kubernetes-ui/nginx.png)

3. Onder **Service**, selecteer **externe** en voert u poort 80.

    Deze instelling worden de taken van verkeer naar de drie replica's.

    ![Dialoogvenster maken Kubernetes-Service](./media/container-service-kubernetes-ui/service.png)

4. Klik op **implementeren** om deze containers en services te implementeren.

    ![Kubernetes implementeren](./media/container-service-kubernetes-ui/deploy.png)

### <a name="view-your-containers"></a>Uw containers weergeven
Nadat u op **implementeren**, de gebruikersinterface ziet u een overzicht van uw service, zoals het wordt geïmplementeerd:

![Kubernetes-Status](./media/container-service-kubernetes-ui/status.png)

U kunt de status van elk object Kubernetes in de cirkel onder aan de linkerkant van de gebruikersinterface ziet **schillen**. Als het een gedeeltelijk volledige cirkel, klikt u vervolgens het object is nog steeds de implementatie. Wanneer een object is volledig is geïmplementeerd, wordt een groen vinkje weergegeven:

![Kubernetes geïmplementeerd](./media/container-service-kubernetes-ui/deployed.png)

Als alles wordt uitgevoerd, klikt u op een van uw schillen voor informatie over de actieve webservice.

![Kubernetes-schillen](./media/container-service-kubernetes-ui/pods.png)

In de **schillen** weergeven, kunt u informatie over de containers in de schil, evenals de CPU en geheugen resources die worden gebruikt door deze containers zien:

![Kubernetes-Resources](./media/container-service-kubernetes-ui/resources.png)

Als u de resources niet ziet, moet u mogelijk Wacht een paar minuten voor de gegevens worden doorgegeven.

Voor de logboeken voor de container, klikt u op **logboeken bekijken**.

![Kubernetes-Logboeken](./media/container-service-kubernetes-ui/logs.png)

### <a name="viewing-your-service"></a>Uw service bekijken
Naast het uitvoeren van uw containers, de UI van Kubernetes een externe heeft gemaakt `Service` die een load balancer om verkeer naar de containers in uw cluster inricht.

Klik in het linkernavigatiedeelvenster op **Services** om alle services (er moet slechts één) weer te geven.

![Kubernetes-Services](./media/container-service-kubernetes-ui/service-deployed.png)

In deze weergave ziet u een extern eindpunt (IP-adres) die is toegewezen aan uw service.
Als u dat IP-adres klikt, ziet u de Nginx-container die achter de load balancer.

![nginx weergeven](./media/container-service-kubernetes-ui/nginx-page.png)

### <a name="resizing-your-service"></a>Uw service vergroten of verkleinen
U kunt niet alleen de objecten in de gebruikersinterface weergeven, bewerken en bijwerken van de Kubernetes API-objecten.

Klik eerst op **implementaties** in het navigatiedeelvenster links om te zien van de implementatie voor uw service.

Wanneer u in die weergave bent, klikt u op de replicaset en klik vervolgens op **bewerken** in de bovenste navigatiebalk:

![Kubernetes bewerken](./media/container-service-kubernetes-ui/edit.png)

Bewerk de `spec.replicas` veld `2`, en klikt u op **Update**.

Dit zorgt ervoor dat het aantal replica's op twee door het verwijderen van een van uw schillen verwijderen.

 

