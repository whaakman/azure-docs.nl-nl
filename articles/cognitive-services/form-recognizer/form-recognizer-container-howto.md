---
title: Installeren en uitvoeren van container - formulier-herkenning
titleSuffix: Azure Cognitive Services
description: Meer informatie over het gebruik van de Form Recognizer-container voor het parseren van formulier- en tabelgegevens.
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: form-recognizer
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: f38752928832b7dee6a7e55f1d25374a64391bbe
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67441884"
---
# <a name="install-and-run-form-recognizer-containers"></a>Installeren en formulier herkenning containers uitvoeren
Herkenning van Azure formulier is van toepassing machine learning-technologie om te identificeren en extraheren van sleutel / waarde-paren en tabellen uit formulieren. Het wordt gekoppeld aan waarden en de items in tabel met de sleutel / waarde-paren en gestructureerde gegevens vindt u de relaties in het oorspronkelijke bestand vervolgens weergeeft. 

Verminder complexiteit en een aangepast formulier herkenning model eenvoudig te integreren in uw automation-werkstroomproces of andere toepassing, kunt u het model met behulp van een eenvoudige REST-API aanroepen. Maximaal vijf form-documenten (of een leeg formulier en twee ingevuld in formulieren) zijn nodig, dus u resultaten snel, nauwkeurig krijgt kunt, en die zijn afgestemd op uw specifieke inhoud. Er is geen zware handmatige tussenkomst of uitgebreide data science-expertise is nodig. En er is geen vereist labeling van gegevens of gegevens aantekening.

|Function|Functies|
|-|-|
|Form Recognizer| <li>PDF-, PNG en JPG-bestanden worden verwerkt<li>Treinen aangepaste modellen met een minimum van 5 vormen van dezelfde lay-out <li>Sleutel / waarde-paren en de gegevens worden geëxtraheerd <li>Maakt gebruik van de functie Azure Cognitive Services Computer Vision-API herkent tekst om te detecteren en extraheer gedrukte tekst uit afbeeldingen in formulieren<li>Aantekening of labels vereist niet|

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Voordat u een formulier herkenning containers gebruiken, moet u voldoen aan de volgende vereisten:

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> Op Windows, moet Docker ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën en kennis van basic `docker` opdrachten.|
|De Azure CLI| Installeer de [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest) op de host.|
|Computer Vision-API-resource| Voor het verwerken van gescande documenten en afbeeldingen, moet u een Computer Vision-resource. U hebt toegang tot de functie tekst herkennen als een Azure resource (de REST-API of SDK) of een *cognitive services-herkennen-tekst* [container](../Computer-vision/computer-vision-how-to-install-containers.md##get-the-container-image-with-docker-pull). De kosten voor de gebruikelijke facturering van toepassing. <br><br>In de sleutel en de facturering eindpunten doorgeven voor de Computer Vision-bron (Azure-cloud of Cognitive Services-container). Gebruik deze sleutel en het eindpunt van de facturering als {COMPUTER_VISION_API_KEY} en {COMPUTER_VISION_BILLING_ENDPOINT_URI}.<br><br> Als u de *cognitive services-herkennen-tekst* container, zorg ervoor dat:<br><br>Uw Computer Vision-sleutel voor de herkenning van formulier-container is de sleutel die is opgegeven in de Computer Vision `docker run` opdracht voor het *cognitive services-herkennen-tekst* container.<br>Uw facturering-eindpunt is van de container-eindpunt (bijvoorbeeld `https://localhost:5000`). Als u de Computer Vision-container en de container formulier herkenning samen op dezelfde host, ze kunnen niet beide worden gestart met de standaardpoort *5000*.  |  
|Formulier herkenning resource |Voor het gebruik van deze containers, moet u het volgende hebben:<br><br>Een _formulier herkenning_ Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden zijn beschikbaar op de Azure-portal **formulier herkenning overzicht** en **formulier herkenning overzicht sleutels** pagina's, en beide waarden zijn vereist voor het starten van de container.<br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is `https://westus.api.cognitive.microsoft.com/forms/v1.0`| 

## <a name="request-access-to-the-container-registry"></a>Aanvraag voor toegang tot de container registry

U moet eerst invullen en verzenden de [Cognitive Services formulier herkenning Containers toegang krijgen tot aanvraagformulier](https://aka.ms/FormRecognizerRequestAccess) toegang vragen tot de container. In dat geval ook ondertekent u voor Computer Vision. U hoeft niet te registreren voor het aanvraagformulier voor de Computer Vision-afzonderlijk. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De minimale en aanbevolen CPU-kernen en geheugen toewijzen voor elke container formulier herkenning worden in de volgende tabel beschreven:

| Container | Minimum | Aanbevolen |
|-----------|---------|-------------|
|cognitive-services-form-recognizer | 2 kernen, 4 GB geheugen | 4 core, 8 GB geheugen |

* Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.
* TPS - transacties per seconde
* Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

> [!Note]
> De minimale en aanbevolen waarden zijn gebaseerd op de Docker-limieten en *niet* resources van de host de computer.

## <a name="get-the-container-image-with-the-docker-pull-command"></a>Ophalen van de installatiekopie van de container met de docker pull-opdracht

Containerinstallatiekopieën voor herkenning van formulier zijn beschikbaar in de volgende opslagplaats:

| Container | Opslagplaats |
|-----------|------------|
| cognitive-services-form-recognizer | `containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest` |

Als u wilt gebruiken de `cognitive-services-recognize-text` [container](../Computer-vision/computer-vision-how-to-install-containers.md##get-the-container-image-with-docker-pull), in plaats van de service formulier herkenning, zorg ervoor dat u de `docker pull` opdracht met de containernaam van de juiste: 

```
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text:latest
```

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="docker-pull-for-the-form-recognizer-container"></a>Docker pull voor de herkenning van formulier-container

#### <a name="form-recognizer"></a>Form Recognizer

Als u de container formulier herkenning, gebruikt u de volgende opdracht uit:

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer:latest
```

## <a name="how-to-use-the-container"></a>Het gebruik van de container

Nadat de container wordt op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-by-using-the-docker-run-command), met de facturering instellingen vereist, maar niet in gebruik. Meer [voorbeelden](form-recognizer-container-configuration.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar.
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-by-using-the-docker-run-command"></a>De container uitvoeren met behulp van de docker opdracht uitvoeren

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren op een van de drie containers. De opdracht maakt gebruik van de volgende parameters:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container. Het is beschikbaar in Azure portal **formulier herkenning sleutels** pagina.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de Azure-portal **formulier herkenning overzicht** pagina.|
|{COMPUTER_VISION_API_KEY}| De sleutel beschikbaar is op de Azure-portal **Computer Vision-API-sleutels** pagina.|
|{COMPUTER_VISION_ENDPOINT_URI}|De facturering-eindpunt. Als u een cloud-gebaseerde Computer Vision-resource, de URI-waarde is beschikbaar op de Azure-portal **Computer Vision-API-overzicht** pagina. Als u een `cognitive-services-recognize-text` container, gebruikt u de facturering eindpunt-URL die wordt doorgegeven aan de container in de `docker run` opdracht.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

### <a name="form-recognizer"></a>Form Recognizer

```bash
docker run --rm -it -p 5000:5000 --memory 8g --cpus 2 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY} \
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

Met deze opdracht:

* Hiermee wordt een container formulier herkenning installatiekopie van de container.
* 2 CPU-kernen en 8 GB (Gigabyte) aan geheugen worden toegewezen.
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container.
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer.
* Hiermee wordt een /input en een volume/output naar de container gekoppeld.

[!INCLUDE [Running multiple containers on the same host H2](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

### <a name="run-separate-containers-as-separate-docker-run-commands"></a>Afzonderlijke containers worden uitgevoerd als afzonderlijke docker-opdrachten uitvoeren

Voor de combinatie van formulier herkenning en herkenning van tekst die lokaal wordt gehost op dezelfde host worden opgenomen, gebruikt u de volgende twee voorbeeld Docker-CLI-opdrachten:

De eerste container worden uitgevoerd op poort 5000. 

```bash 
docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
--mount type=bind,source=c:\input,target=/input  \
--mount type=bind,source=c:\output,target=/output \
containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
FormRecognizer:ComputerVisionApiKey={COMPUTER_VISION_API_KEY} \
FormRecognizer:ComputerVisionEndpointUri={COMPUTER_VISION_ENDPOINT_URI}
```

De tweede container op poort 5001 worden uitgevoerd.


```bash 
docker run --rm -it -p 5001:5000 --memory 4g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text \
Eula=accept \
Billing={COMPUTER_VISION_ENDPOINT_URI} \
ApiKey={COMPUTER_VISION_API_KEY}
```
Elke volgende container moet zich op een andere poort. 

### <a name="run-separate-containers-with-docker-compose"></a>Voer afzonderlijke containers met Docker Compose

Zie voor de herkenning van formulier en tekst herkenning combinatie die lokaal wordt gehost op dezelfde host, het volgende voorbeeld Docker Compose YAML-bestand. De tekst-herkenning `{COMPUTER_VISION_API_KEY}` moet hetzelfde zijn voor zowel de `formrecognizer` en `ocr` containers. De `{COMPUTER_VISION_ENDPOINT_URI}` wordt uitsluitend gebruikt in de `ocr` container, omdat de `formrecognizer` container maakt gebruik van de `ocr` naam en poort. 

```docker
version: '3.3'
services:   
  ocr:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-recognize-text"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{COMPUTER_VISION_ENDPOINT_URI}"
      apikey: {COMPUTER_VISION_API_KEY}  

  formrecognizer:
    image: "containerpreview.azurecr.io/microsoft/cognitive-services-form-recognizer"
    deploy:
      resources:
        limits:
          cpus: '2'
          memory: 8g
        reservations:
          cpus: '1'
          memory: 4g
    environment:
      eula: accept
      billing: "{BILLING_ENDPOINT_URI}"
      apikey: {BILLING_KEY}
      FormRecognizer__ComputerVisionApiKey: {COMPUTER_VISION_API_KEY}
      FormRecognizer__ComputerVisionEndpointUri: "http://ocr:5000"
      FormRecognizer__SyncProcessTaskCancelLimitInSecs: 75
    links:
      - ocr
    volumes:
      - type: bind
        source: c:\output
        target: /output
      - type: bind
        source: c:\input
        target: /input
    ports:
      - "5000:5000"  
```


> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey`, evenals de `FormRecognizer:ComputerVisionApiKey` en `FormRecognizer:ComputerVisionEndpointUri` opties moet worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start. Zie voor meer informatie, [facturering](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

|Container|Eindpunt|
|--|--|
|form-recognizer|http://localhost:5000


### <a name="form-recognizer"></a>Form Recognizer

De container biedt op basis van websocket query eindpunt API's, die u via toegang tot [formulier herkenning services SDK-documentatie](https://docs.microsoft.com/azure/cognitive-services/form-recognizer/).

Het formulier herkenning SDK gebruikt standaard de online services. Voor het gebruik van de container, moet u de van de initialisatiemethode wijzigen. Zie de onderstaande voorbeelden.

#### <a name="for-c"></a>VoorC#

Wijzigen van het gebruik van deze aanroep van de initialisatie van Azure-cloud:

```C#
var config = FormRecognizerConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

op deze aanroep waarin het eindpunt van de container:

```C#
var config = FormRecognizerConfig.FromEndpoint("ws://localhost:5000/formrecognizer/v1.0-preview/custom", "YourSubscriptionKey");
```

#### <a name="for-python"></a>Voor Python

Wijzigen van het gebruik van deze aanroep van de initialisatie van Azure-cloud:

```python
formrecognizer_config = formrecognizersdk.FormRecognizerConfig(subscription=formrecognizer_key, region=service_region)
```

op deze aanroep waarin het eindpunt van de container:

```python
formrecognizer_config = formrecognizersdk.FormRecognizerConfig(subscription=formrecognizer_key, endpoint="ws://localhost:5000/formrecognizer/v1.0-preview/custom"
```

### <a name="form-recognizer"></a>Form Recognizer

De container biedt REST-eindpunt API's, die u kunt vinden op de [Form-herkenning API](https://westus2.dev.cognitive.microsoft.com/docs/services/form-recognizer-api/operations/AnalyzeWithCustomModel) pagina.


[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]


## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer u de container uitvoert, wordt de container gebruikt **stdout** en **stderr** naar uitvoerinformatie die nuttig is voor het oplossen van problemen die zich voordoen bij het starten of uitvoeren van de container.

## <a name="billing"></a>Billing

De containers formulier herkenning factureringsgegevens verzenden naar Azure met behulp van een _formulier herkenning_ resource voor uw Azure-account.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](form-recognizer-container-configuration.md).

<!--blogs/samples/video courses -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en formulier herkenning containers uitvoeren. Samenvatting:

* Formulier herkenning biedt een Linux-container voor Docker.
* Containerinstallatiekopieën worden gedownload van het privé containerregister in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de REST SDK gebruiken om aan te roepen van bewerkingen in formulier herkenning container door de host-URI van de container op te geven.
* Wanneer u een container, moet u de factureringsgegevens opgeven.

> [!IMPORTANT]
>  Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden gegevens van de klant (bijvoorbeeld, de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](form-recognizer-container-configuration.md) voor configuratie-instellingen.
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md).
