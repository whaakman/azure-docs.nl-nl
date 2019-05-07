---
title: Over het installeren van IoT Edge op Kubernetes | Microsoft Docs
description: Meer informatie over het installeren van IoT Edge op een lokale ontwikkelingsomgeving cluster met Kubernetes
author: kgremban
manager: philmea
ms.author: veyalla
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.custom: seodec18
ms.openlocfilehash: 66aca7be9a2df93d846d7e78bc64c93279afc2d1
ms.sourcegitcommit: f6ba5c5a4b1ec4e35c41a4e799fb669ad5099522
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/06/2019
ms.locfileid: "65160693"
---
# <a name="how-to-install-iot-edge-on-kubernetes-preview"></a>Over het installeren van IoT Edge op Kubernetes (Preview)

IoT Edge kunt integreren met Kubernetes als een robuuste, maximaal beschikbare infrastructuur-laag gebruiken. Registratie van een IoT-Edge *aangepaste resourcedefinitie* (CRD) met de Kubernetes API-Server. Daarnaast biedt deze blade een *Operator* (IoT Edge-agent) dat voor overeenstemming zorgt met gewenste status cloud beheerd met de status van het lokale cluster. 

Levensduur van de module wordt beheerd door de scheduler Kubernetes, houdt de beschikbaarheid van de module en kiest de plaatsing. IoT Edge beheert het edge-toepassingsplatform uitgevoerd op de voorgrond, continu afstemmen van de gewenste status die is opgegeven in IoT Hub met de status op het edge-cluster. Het edge-toepassingsmodel is nog steeds de vertrouwde model op basis van IoT Edge-modules en routes. De IoT Edge agent-upoperator *automatische* vertaling naar de Kubernetes-natives constructs, zoals schillen, implementaties, services, enzovoort.

Hier volgt een diagram architectuur op hoog niveau:

![kubernetes-arch](./media/how-to-install-iot-edge-kubernetes/k8s-arch.png)

Elk onderdeel van de edge-implementatie is afgestemd op een Kubernetes-naamruimte die specifiek zijn voor het apparaat, waardoor het mogelijk om te delen tussen meerdere edge-apparaten en hun implementaties dezelfde clusterresources.

>[!NOTE]
>IoT Edge op Kubernetes is in [preview-versie](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="install-locally-for-a-quick-test-environment"></a>Lokaal installeren voor een snelle test-omgeving

### <a name="prerequisites"></a>Vereisten

* Kubernetes 1,10 of hoger. Als u een bestaande installatie hebt, kunt u [Minikube](https://kubernetes.io/docs/setup/minikube/) voor een lokaal cluster-omgeving. 

* [Helm](https://helm.sh/docs/using_helm/#quickstart-guide), het pakketbeheerprogramma voor Kubernetes.

* [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) voor het weergeven en interactie met het cluster.

### <a name="setup-steps"></a>Instellingsstappen

1. Start **Minikube**

    ``` shell
    minikube start
    ```

1. Initialiseren van de **Helm** serveronderdeel (*tiller*) in uw cluster

    ``` shell
    helm init
    ```

1. IoT Edge-opslagplaats toevoegen en bijwerken van de helm-installatie

    ``` shell
    helm repo add edgek8s https://edgek8s.blob.core.windows.net/helm/
    helm repo update
    ```

1. [Maken van een IoT-Hub](../iot-hub/iot-hub-create-through-portal.md), [een IoT Edge-apparaat registreren](how-to-register-device-portal.md), en noteer de verbindingsreeks.

1. Iotedged en IoT Edge-agent installeren in uw cluster

    ```shell
    helm install \
    --name k8s-edge1 \
    --set "deviceConnectionString=replace-with-device-connection-string" \
    edgek8s/edge-kubernetes
    ```
1. Open het Kubernetes-dashboard in de browser

    ```shell
    minikube dashboard
    ```

    Onder de cluster-naamruimten, ziet u een voor de IoT Edge-apparaat na de overeenkomst *msiot -\<iothub-naam >-\<edgedevice-name >*. De IoT Edge-agent en iotedged schillen moet actief en werkend in deze naamruimte.

1. Toevoegen van een gesimuleerde temperatuur sensor-module met behulp van de stappen in de [implementeren van een module](quickstart-linux.md#deploy-a-module) sectie van de Quick Start. IoT Edge module management wordt gedaan via de portal voor IoT Hub net als elk ander IoT Edge-apparaat. Lokale wijzigingen aanbrengen in de moduleconfiguratie via Kubernetes-hulpprogramma's wordt niet aanbevolen omdat ze mogelijk overschreven.

1. Binnen een paar seconden, het vernieuwen de **schillen** pagina onder de naamruimte van de edge-apparaat in het dashboard wordt een lijst met de IoT Edge hub en gesimuleerde sensor schillen alsof ze worden uitgevoerd met de IoT Edge hub pod gegevens opnemen in IoT Hub.

## <a name="clean-up-resources"></a>Resources opschonen

Als u wilt verwijderen van alle resources die via de edge-implementatie gemaakt, gebruikt u de volgende opdracht uit met de naam die wordt gebruikt in stap 5 van de vorige sectie.

``` shell
helm delete --purge k8s-edge1
```

## <a name="next-steps"></a>Volgende stappen

### <a name="deploy-as-a-highly-available-edge-gateway"></a>Als een maximaal beschikbare edge-gateway implementeren 

Het edge-apparaat in een Kubernetes-cluster kan worden gebruikt als een IoT-gateway voor downstream-apparaten. Deze kan worden geconfigureerd om te zijn tegen knooppuntfout manier wordt perimeterbeveiliging geboden hoge beschikbaarheid voor het edge-implementaties. Raadpleeg deze [gedetailleerd overzicht](https://github.com/Azure-Samples/iotedge-gateway-on-kubernetes) het gebruik van IoT Edge in dit scenario.