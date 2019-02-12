---
title: Installeren en uitvoeren van containers
titlesuffix: Face - Azure Cognitive Services
description: Het downloaden, installeren en uitvoeren van containers voor gezicht in deze zelfstudie met stapsgewijze instructies.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: article
ms.date: 02/11/2019
ms.author: diberry
ms.openlocfilehash: 7a41bfaada64528e90f43064b34c394f9a9b8f8f
ms.sourcegitcommit: 39397603c8534d3d0623ae4efbeca153df8ed791
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/12/2019
ms.locfileid: "56099085"
---
# <a name="install-and-run-containers"></a>Containers installeren en uitvoeren

Face biedt een gestandaardiseerde Linux-container voor Docker, met de naam gezicht, detecteert menselijke gezichten in afbeeldingen en-kenmerken, zoals gezichtsoriëntatiepunten (zoals hartstukken en ogen), geslacht, leeftijd en andere machine voorspeld gezichtskenmerken identificeert. Naast detectie controleren Face of twee gezichten in dezelfde afbeelding of andere installatiekopieën zijn hetzelfde met behulp van een betrouwbaarheidsscore of gezichten op een database om te zien als een gelijkende vergelijken of identieke face al bestaat. Deze kunt soortgelijke gezichten ook organiseren in groepen, met behulp van gedeelde visuele kenmerken.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met behulp van containers voor Face-API:

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> **Op Windows**, Docker moet ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.| 
|Face-API-resource |Als u wilt gebruiken in de container, moet u het volgende hebben:<br><br>Een _Face-API_ Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden beschikbaar zijn op de Face-API-overzicht en sleutels van de Azure portal-pagina's en zijn vereist om te starten van de container.<br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is: `https://westus.api.cognitive.microsoft.com/text/analytics/v2.0`|


## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en geheugen toewijzen voor elke container Face-API.

| Container | Minimum | Aanbevolen |
|-----------|---------|-------------|
|Face | 1 core, 2 GB geheugen | 1 core, 4 GB geheugen |

Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

## <a name="get-the-container-image-with-docker-pull"></a>Met de installatiekopie van de container ophalen `docker pull`

Containerinstallatiekopieën voor Face-API zijn beschikbaar. 

| Container | Opslagplaats |
|-----------|------------|
| Face | `containerpreview.azurecr.io/microsoft/cognitive-services-face:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-face-container"></a>Docker pull voor de Face-container

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-face:latest
```

## <a name="how-to-use-the-container"></a>Het gebruik van de container

Als de container op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-with-docker-run), facturering met de vereiste instellingen. Meer [voorbeelden](./face-resource-container-config.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>De container met uitvoeren `docker run`

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren op een van de drie containers. De opdracht maakt gebruik van de volgende parameters:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de pagina van de Azure portal-Face-API-sleutels.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de pagina van de Face-API overzicht van de Azure portal.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-face \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Met deze opdracht:

* Een container face kan worden uitgevoerd van de container-installatiekopie
* Één CPU-kern en 4 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer. 

Meer [voorbeelden](./face-resource-container-config.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

De container biedt eindpunt van de voorspelling query op basis van REST API's. 

Gebruikmaken van de host https://localhost:5000, voor de container met API's.

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container wordt uitgevoerd met een uitvoer [koppelen](./face-resource-container-config.md#mount-settings) en logboekregistratie is ingeschakeld, wordt de container genereert logboekbestanden die tot het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container. 

## <a name="containers-api-documentation"></a>API-documentatie van de container

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="billing"></a>Billing

De Face-API containers verzenden factuurgegevens naar Azure, met behulp van een _Face-API_ resource voor uw Azure-account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](./face-resource-container-config.md).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en uitvoeren van containers voor Face-API. Samenvatting:

* Face-API biedt drie Linux-containers voor Docker, encapsulating sleuteltermextractie, taaldetectie en sentimentanalyse.
* Containerinstallatiekopieën worden gedownload uit het Microsoft Container Registry (MCR) in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in containers Face-API door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](face-resource-container-config.md) voor configuratie-instellingen
* Beoordeling [Face overzicht](Overview.md) voor meer informatie over het opsporen en identificeren van gezichten  
* Raadpleeg de [Face-API](//westus.dev.cognitive.microsoft.com/docs/services/563879b61984550e40cbbe8d/operations/563879b61984550f30395236) voor meer informatie over de methoden die wordt ondersteund door de container.
* Raadpleeg [Veelgestelde vragen (FAQ)](FAQ.md) het oplossen van problemen met betrekking tot Face-functionaliteit.
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
