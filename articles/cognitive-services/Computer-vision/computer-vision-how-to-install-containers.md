---
title: Over het installeren en uitvoeren van containers - Computer Vision
titlesuffix: Azure Cognitive Services
description: Het downloaden, installeren en uitvoeren van containers voor Computer Vision in deze zelfstudie met stapsgewijze instructies.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: article
ms.date: 02/08/2019
ms.author: diberry
ms.custom: seodec18
ms.openlocfilehash: 58a22253efb6928c87937e4aba852dd93a3e4422
ms.sourcegitcommit: 943af92555ba640288464c11d84e01da948db5c0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/09/2019
ms.locfileid: "55978541"
---
# <a name="install-and-run-recognize-text-containers"></a>Installeren en te herkennen tekst containers uitvoeren

Het gedeelte tekst herkennen van Computer Vision-is ook beschikbaar als een Docker-container. Hiermee kunt u om te detecteren en extraheer gedrukte tekst uit afbeeldingen van verschillende objecten met verschillende oppervlakken en achtergronden, zoals ontvangsten, posters en visitekaartjes.  
> [!IMPORTANT]
> De tekst herkennen-container wordt op dit moment werkt alleen met Engels.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met behulp van containers tekst herkennen:

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> **Op Windows**, Docker moet ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.| 
|Herkennen tekst resource |Als u wilt gebruiken in de container, moet u het volgende hebben:<br><br>Een [ _tekst herkennen_ ](vision-api-how-to-topics/howtosubscribe.md) Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden zijn beschikbaar op herkennen tekst overzicht en sleutels van de Azure portal-pagina's en zijn vereist om te starten van de container.<br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is: `https://westus.api.cognitive.microsoft.com/vision/v2.0`|


## <a name="request-access-to-the-private-container-registry"></a>Aanvraag voor toegang tot de privécontainerregister

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-request-access.md)]

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-host-computer.md)]


### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en geheugen toewijzen voor elke container tekst herkennen.

| Container | Minimum | Aanbevolen |
|-----------|---------|-------------|
|Tekst herkennen|1 core, 8 GB geheugen, 0,5 TPS|2 kernen, 8 GB geheugen, 1 TPS|

Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.


## <a name="get-the-container-image-with-docker-pull"></a>Met de installatiekopie van de container ophalen `docker pull`

Containerinstallatiekopieën voor tekst herkennen zijn beschikbaar. 

| Container | Opslagplaats |
|-----------|------------|
|Tekst herkennen | `containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest` |

Gebruik de [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) opdracht om een containerinstallatiekopie te downloaden.


### <a name="docker-pull-for-the-recognize-text-container"></a>Docker pull voor de container tekst herkennen

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-rocognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

## <a name="how-to-use-the-container"></a>Het gebruik van de container

Als de container op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-with-docker-run), facturering met de vereiste instellingen. Meer [voorbeelden](computer-vision-resource-container-config.md) van de `docker run` opdrachten zijn beschikbaar. 
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint). 

## <a name="run-the-container-with-docker-run"></a>De container met uitvoeren `docker run`

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren van de container. De opdracht maakt gebruik van de volgende parameters:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de Azure-portal op de pagina herkennen tekst-sleutels.  |
|{BILLING_ENDPOINT_URI} | De facturering eindpunt URI-waarde.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Met deze opdracht:

* Een container herkennen kan worden uitgevoerd van de container-installatiekopie
* Één CPU-kern en 4 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer. 

Meer [voorbeelden](./computer-vision-resource-container-config.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

De container biedt eindpunt van de voorspelling query op basis van REST API's. 

Gebruikmaken van de host https://localhost:5000, voor de container met API's.

### <a name="asynchronous-text-recognition"></a>Asynchrone tekstherkenning

U kunt de `POST /vision/v2.0/recognizeText` en `GET /vision/v2.0/textOperations/*{id}*` bewerkingen in concert asynchroon herkennen gedrukte tekst in een afbeelding, vergelijkbaar met hoe de Computer Vision-service maakt gebruik van de bijbehorende REST-bewerkingen. De container tekst herkennen herkent alleen gedrukte tekst, niet handgeschreven tekst op dit moment, zodat de `mode` parameter normaal gesproken opgegeven voor de bewerking van de Computer Vision wordt genegeerd door de tekst herkennen-container.

### <a name="synchronous-text-recognition"></a>Synchrone tekstherkenning

U kunt de `POST /vision/v2.0/recognizeTextDirect` bewerking is synchroon gedrukte tekst in een afbeelding herkennen. Omdat deze bewerking synchroon is, de hoofdtekst van de aanvraag voor deze bewerking is hetzelfde als die voor de `POST /vision/v2.0/recognizeText` bewerking, maar het antwoord body voor deze bewerking hetzelfde als die die wordt geretourneerd is door de `GET /vision/v2.0/textOperations/*{id}*` bewerking.

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container wordt uitgevoerd met een uitvoer [koppelen](./computer-vision-resource-container-config.md#mount-settings) en logboekregistratie is ingeschakeld, wordt de container genereert logboekbestanden die tot het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container. 

## <a name="containers-api-documentation"></a>API-documentatie van de container

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="billing"></a>Billing

De tekst herkennen containers verzenden factuurgegevens naar Azure, met behulp van een _tekst herkennen_ resource voor uw Azure-account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](./computer-vision-resource-container-config.md).

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en tekst herkennen containers uitvoeren. Samenvatting:

* Herkennen tekst biedt een Linux-container voor Docker, tekst encapsulating herkennen.
* Containerinstallatiekopieën worden gedownload uit het Microsoft Container Registry (MCR) in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in containers tekst herkennen door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden gegevens van de klant (bijvoorbeeld, de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](computer-vision-resource-container-config.md) voor configuratie-instellingen
* Beoordeling [Computer Vision-overzicht](Home.md) voor meer informatie over het herkennen van afdrukken en handgeschreven tekst  
* Raadpleeg de [Computer Vision-API](//westus.dev.cognitive.microsoft.com/docs/services/5adf991815e1060e6355ad44/operations/56f91f2e778daf14a499e1fa) voor meer informatie over de methoden die wordt ondersteund door de container.
* Raadpleeg [Veelgestelde vragen (FAQ)](FAQ.md) het oplossen van problemen met betrekking tot de Computer Vision-functionaliteit.
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
