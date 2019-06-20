---
title: Containers installeren en uitvoeren
titlesuffix: Face - Azure Cognitive Services
description: Downloaden, installeren en uitvoeren van containers voor gezicht in deze zelfstudie met stapsgewijze instructies.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: beb9818be05fbb9a9e9c958dccb2e375f7685bd0
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67272799"
---
# <a name="install-and-run-face-containers"></a>Installeren en uitvoeren van de Face-containers

Azure Cognitive Services Face biedt een gestandaardiseerde Linux-container voor Docker die menselijke gezichten in afbeeldingen detecteert. Het identificeert ook kenmerken, waaronder gezichtsoriëntatiepunten, zoals hartstukken en onder ogen krijgt, geslacht, leeftijd en andere machine voorspeld gezichtskenmerken. Naast detectie, kunt Face controleren als twee gezichten in dezelfde afbeelding of andere installatiekopieën dezelfde met een betrouwbaarheidsscore zijn. Face kunt ook gezichten op een database om te zien of er al een gezicht gelijkende of identieke bestaat vergelijken. Deze kunt ook vergelijkbare gezichten in groepen indelen met behulp van gedeelde visuele kenmerken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u de Face-API-containers gebruiken.

|Vereist|Doel|
|--|--|
|Docker-Engine| De Docker-Engine moet zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> Op Windows, worden Docker ook geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | Moet u een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers en containerinstallatiekopieën. U moet ook kennis van basic `docker` opdrachten.| 
|Azure `Cognitive Services` resource |Voor het gebruik van de container, moet u het volgende hebben:<br><br>Een Azure Cognitive Services-resource en de bijbehorende facturering sleutel en de facturering URI van het eindpunt. Beide waarden zijn beschikbaar op de **overzicht** en **sleutels** pagina's voor de resource. Deze moeten de container te starten. Voeg de `face/v1.0` routering naar het eindpunt-URI, zoals wordt weergegeven in het volgende BILLING_ENDPOINT_URI-voorbeeld: <br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is `https://westus.api.cognitive.microsoft.com/face/v1.0`|

## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

[!INCLUDE [Request access to private container registry](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en geheugen toewijzen voor elke container Face-API.

| Container | Minimum | Aanbevolen | Transacties per seconde<br>(Minimum, maximum)|
|-----------|---------|-------------|--|
|Face | 1 core, 2 GB geheugen | 1 core, 4 GB geheugen |10, 20|

* Elke core moet ten minste 2,6 GHz of sneller.
* Transacties per seconde (TPS).

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

## <a name="get-the-container-image-with-docker-pull"></a>De installatiekopie van de container met docker pull ophalen

Containerinstallatiekopieën voor de Face-API zijn beschikbaar. 

| Container | Opslagplaats |
|-----------|------------|
| Face | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker pull voor de Face-container

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="use-the-container"></a>De container gebruiken

Nadat de container wordt op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-with-docker-run) facturering met de vereiste instellingen. Meer [voorbeelden](./face-resource-container-config.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>Uitvoeren van de container met docker uitvoeren

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren op een van de drie containers. De opdracht maakt gebruik van de volgende parameters.

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de Azure `Cognitive Services` **sleutels** pagina. |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de Azure `Cognitive Services` **overzicht** pagina. Een voorbeeld is `https://westus.api.cognitive.microsoft.com/face/v1.0`.|

Voeg de `face/v1.0` routering naar het eindpunt-URI, zoals wordt weergegeven in het voorgaande voorbeeld van BILLING_ENDPOINT_URI. 

Deze parameters vervangen door uw eigen waarden in de volgende `docker run` opdracht:

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Met deze opdracht:

* Hiermee wordt een container face installatiekopie van de container.
* Één CPU-kern en 4 GB geheugen worden toegewezen.
* Gebruikt TCP-poort 5000 en wijst er een pseudo TTY voor de container.
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer. 

Meer [voorbeelden](./face-resource-container-config.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container of de container start niet. Zie voor meer informatie, [facturering](#billing).

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]


## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

De container biedt eindpunt van de voorspelling query op basis van REST API's. 

Gebruikmaken van de host `https://localhost:5000`, voor de container met API's.


<!--  ## Validate container is running -->

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container wordt uitgevoerd met een uitvoer [koppelen](./face-resource-container-config.md#mount-settings) en logboekregistratie is ingeschakeld, wordt de container genereert logboekbestanden die tot het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container. 


## <a name="billing"></a>Billing

De Face-API-containers voor het verzenden van factureringsgegevens naar Azure met behulp van een resource Face-API voor uw Azure-account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](./face-resource-container-config.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en uitvoeren van de Face-API-containers. Samenvatting:

* De Face-API biedt drie Linux-containers voor Docker sleuteltermextractie, taaldetectie en sentimentanalyse.
* Containerinstallatiekopieën worden gedownload van Azure Container Registry.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in containers Face-API door de host-URI van de container op te geven.
* Wanneer u een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers worden niet gelicentieerd voor het uitvoeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden geen gegevens van de klant, zoals de afbeelding of tekst die wordt geanalyseerd, naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Zie voor configuratie-instellingen, [containers configureren](face-resource-container-config.md).
* Zie voor meer informatie over het opsporen en identificeren van gezichten, [Face overzicht](Overview.md).
* Zie voor meer informatie over de methoden die wordt ondersteund door de container de [Face-API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236).
* Zie voor het gebruik van meer Cognitive Services-containers, [Cognitive Services-containers](../cognitive-services-container-support.md).
