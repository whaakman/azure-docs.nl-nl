---
title: Installeren en uitvoeren van containers - Anomaliedetectie detector
titleSuffix: Azure Cognitive Services
description: Gebruik geavanceerde algoritmen afwijkingen Detector-API's om te identificeren van afwijkingen in uw time series-gegevens.
services: cognitive-services
author: IEvangelist
ms.service: cognitive-services
ms.subservice: anomaly-detection
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 704ea3ecbc309c2328e120d5b6776e659944d254
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67275025"
---
# <a name="install-and-run-anomaly-detector-containers"></a>Installeren en uitvoeren van containers voor de detectie van afwijkingen

De detectie van afwijkingen heeft de volgende container: 

|Function|Functies|
|-|-|
|Anomaly Detector| <li> Afwijkingen detecteert wanneer deze zich in realtime voordoen. <li> Detecteert afwijkingen in uw gegevensset als een batch. <li> Infereert het verwachte normale bereik van uw gegevens. <li> Ondersteunt anomaliedetectie detectie gevoeligheid correctie voor een beter aanpassen aan uw gegevens. |

Raadpleeg voor gedetailleerde informatie over de API's:
* [Meer informatie over Anomaliedetectie Detector API-service](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met behulp van de detectie van afwijkingen containers:

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> **Op Windows**, Docker moet ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.| 
|Anomaliedetectie Detector resource |Als u wilt deze containers gebruiken, moet u het volgende hebben:<br><br>Een _Anomaliedetectie Detector_ Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden zijn beschikbaar op de pagina overzicht van de detectie van afwijkingen en sleutels van de Azure portal en zijn verplicht om de container te starten.<br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is: `https://westus2.api.cognitive.microsoft.com`|

## <a name="request-access-to-the-container-registry"></a>Aanvraag voor toegang tot de container registry

U moet eerst invullen en verzenden de [Anomaliedetectie Detector Container aanvraagformulier](https://aka.ms/adcontainer) toegang vragen tot de container.

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

<!--* [Azure IoT Edge](https://docs.microsoft.com/azure/iot-edge/). For instructions of deploying Anomaly Detector module in IoT Edge, see [How to deploy Anomaly Detector module in IoT Edge](how-to-deploy-anomaly-detector-module-in-iot-edge.md).-->

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en geheugen toewijzen voor de detectie van afwijkingen container.

| QPS (query's per seconde) | Minimum | Aanbevolen |
|-----------|---------|-------------|
| 10 QPS | 4 core, 1GB geheugen | 8-core 2GB geheugen |
| 20 QPS | 8 kernen, 2GB geheugen | 16-core 4GB geheugen |

Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

## <a name="get-the-container-image-with-docker-pull"></a>Met de installatiekopie van de container ophalen `docker pull`

Gebruik de [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) opdracht om een containerinstallatiekopie te downloaden.

| Container | Opslagplaats |
|-----------|------------|
| cognitive-services-anomaly-detector | `containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest` |

<!--
For a full description of available tags, such as `latest` used in the preceding command, see [anomaly-detector](https://go.microsoft.com/fwlink/?linkid=2083827&clcid=0x409) on Docker Hub.
-->
[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


### <a name="docker-pull-for-the-anomaly-detector-container"></a>Docker pull voor de container Anomaliedetectie Detector

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest
```

## <a name="how-to-use-the-container"></a>Het gebruik van de container

Als de container op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-with-docker-run), facturering met de vereiste instellingen. Meer [voorbeelden](anomaly-detector-container-configuration.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>De container met uitvoeren `docker run`

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren op een van de drie containers. De opdracht maakt gebruik van de volgende parameters:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de pagina van de Anomaliedetectie Detector sleutels van de Azure portal.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de pagina van de Anomaliedetectie Detector overzicht van de Azure portal.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-anomaly-detector:latest \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Met deze opdracht:

* Een container voor detectie van afwijkingen kan worden uitgevoerd van de container-installatiekopie
* Één CPU-kern en 4 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer. 

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

### <a name="running-multiple-containers-on-the-same-host"></a>Meerdere containers uitvoeren op dezelfde host

Als u van plan bent om uit te voeren van meerdere containers met blootgestelde poorten, zorg ervoor dat elke container uitvoeren met een andere poort. Bijvoorbeeld: de eerste container op poort 5000- en de tweede container uitvoeren op poort 5001.

Vervang de `<container-registry>` en `<container-name>` met de waarden van de containers die u gebruikt. Deze hoeven niet te zijn van dezelfde container. U kunt de container voor detectie van afwijkingen en de LUIS-container die samen worden uitgevoerd op de HOST hebben of u kunt meerdere Anomaliedetectie Detector containers die worden uitgevoerd. 

De eerste container worden uitgevoerd op poort 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

De tweede container op poort 5001 worden uitgevoerd.


```bash 
docker run --rm -it -p 5000:5001 --memory 4g --cpus 1 \
<container-registry>/microsoft/<container-name> \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Elke volgende container moet zich op een andere poort. 

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

De container biedt eindpunt van de voorspelling query op basis van REST API's. 

Gebruikmaken van de host https://localhost:5000, voor de container met API's.

<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container wordt uitgevoerd met een uitvoer [koppelen](anomaly-detector-container-configuration.md#mount-settings) en logboekregistratie is ingeschakeld, wordt de container genereert logboekbestanden die tot het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container. 

## <a name="billing"></a>Billing

De detectie van afwijkingen containers verzenden factuurgegevens naar Azure, met behulp van een _Anomaliedetectie Detector_ resource voor uw Azure-account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](anomaly-detector-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en Anomaliedetectie Detector containers uitvoeren. Samenvatting:

* Detectie van afwijkingen biedt een Linux-container voor Docker, detectie van afwijkingen encapsulating met batch vs streaming, verwachte bereik Deductie en gevoeligheid afstemmen.
* Containerinstallatiekopieën worden gedownload van een toegewezen voor containers preview van persoonlijke Azure Container Registry.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in de Anomaliedetectie Detector containers door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de tijd reeksgegevens die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](anomaly-detector-container-configuration.md) voor configuratie-instellingen
* [Meer informatie over Anomaliedetectie Detector API-service](https://go.microsoft.com/fwlink/?linkid=2080698&clcid=0x409)
