---
title: Spraak containers installeren
titleSuffix: Azure Cognitive Services
description: Installeren en uitvoeren van containers van spraak. Spraak-naar-tekst transcribes audiostreams naar tekst in realtime die uw toepassingen, hulpprogramma's of apparaten kunnen gebruiken of weergeven. Tekst naar spraak converteert invoertekst naar menselijke kunstmatige spraak.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 06/19/2019
ms.author: dapine
ms.openlocfilehash: 6df40909e2072ac2126344ba5b696c9e520e9955
ms.sourcegitcommit: f56b267b11f23ac8f6284bb662b38c7a8336e99b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/28/2019
ms.locfileid: "67434150"
---
# <a name="install-and-run-speech-service-containers"></a>Installeren en uitvoeren van containers voor spraak-Service

Spraak-containers bieden klanten kunnen maken van een architectuur van de spraak-toepassing die is geoptimaliseerd om te profiteren van zowel mogelijkheden van de robuuste cloud en edge plaats. 

De twee spraak-containers zijn **spraak-naar-tekst** en **tekst naar spraak**. 

|Function|Functies|Latest|
|-|-|--|
|Spraak-naar-tekst| <li>Transcribes continue realtime spraak of batch audio-opnamen in tekst met tussenliggende resultaten genereren.|1.1.2|
|Tekst naar spraak| <li>Converteert tekst naar natuurlijk klinkende spraak. met tekst zonder opmaak invoer of spraak synthese Markup Language (SSML). |1.1.0|

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

U moet voldoen aan de volgende vereisten voordat u met behulp van spraak containers:

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> **Op Windows**, Docker moet ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.| 
|Spraak-resource |Als u wilt deze containers gebruiken, moet u het volgende hebben:<br><br>Een _spraak_ Azure-resource om de bijbehorende facturering sleutel en facturering URI van het eindpunt te verkrijgen. Beide waarden zijn beschikbaar op de Azure-portal **spraak** overzicht en sleutels pagina's en zijn vereist voor het starten van de container.<br><br>**{BILLING_KEY}** : bronsleutel<br><br>**{BILLING_ENDPOINT_URI}** : voorbeeld van de eindpunt-URI is: `https://westus.api.cognitive.microsoft.com/sts/v1.0`|

## <a name="request-access-to-the-container-registry"></a>Aanvraag voor toegang tot de container registry

U moet eerst invullen en verzenden de [Cognitive Services spraak Containers aanvraagformulier](https://aka.ms/speechcontainerspreview/) toegang vragen tot de container. 

[!INCLUDE [Request access to the container registry](../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Host Computer requirements](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="advanced-vector-extension-support"></a>Ondersteuning voor geavanceerde Vector-extensie

De **host** is de computer met de docker-container. De host moet ondersteuning bieden voor [geavanceerde Vector extensies](https://en.wikipedia.org/wiki/Advanced_Vector_Extensions#CPUs_with_AVX2) (AVX2). Deze ondersteuning op Linux-hosts met de volgende opdracht, kunt u controleren: 

```console
grep -q avx2 /proc/cpuinfo && echo AVX2 supported || echo No AVX2 support detected
```

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

De volgende tabel beschrijft de minimale en aanbevolen CPU-kernen en geheugen toewijzen voor elke container spraak.

| Container | Minimum | Aanbevolen |
|-----------|---------|-------------|
|cognitive-services-speech-to-text | 2 kernen<br>2 GB geheugen  | 4 kern<br>4 GB geheugen  |
|cognitive-services-text-to-speech | 1 core, 0,5 GB geheugen| 2 kernen, 1 GB geheugen |

* Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

**Houd er rekening mee**; De minimale en aanbevolen zijn op basis van Docker-limieten, *niet* resources van de host de computer. Spraak-naar-tekst containers geheugen toewijzen gedeelten van een grote taalmodel en het is bijvoorbeeld _aanbevolen_ dat het hele bestand past in het geheugen, namelijk een extra 4-6 GB. Ook kan de eerste uitvoering van een container langer duren, omdat het modellen wisselgeheugen wordt geplaatst in het geheugen.

## <a name="get-the-container-image-with-docker-pull"></a>Met de installatiekopie van de container ophalen `docker pull`

Containerinstallatiekopieën voor spraak zijn beschikbaar.

| Container | Opslagplaats |
|-----------|------------|
| cognitive-services-speech-to-text | `containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest` |
| cognitive-services-text-to-speech | `containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest` |

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]

### <a name="language-locale-is-in-container-tag"></a>Landinstelling voor de taal is in container-code

De `latest` label worden de `en-us` landinstellingen en `jessarus` stem.

#### <a name="speech-to-text-locales"></a>Spraak naar tekst landinstellingen

Alle tags, met uitzondering van `latest` zijn in de volgende indeling hebben: waar de `<culture>` geeft aan dat de container landinstellingen:

```
<major>.<minor>.<patch>-<platform>-<culture>-<prerelease>
```

De volgende code is een voorbeeld van de indeling:

```
1.1.2-amd64-en-us-preview
```

De volgende tabel bevat de ondersteunde landinstellingen voor **spraak-naar-tekst** in de 1.1.2 versie van de container:

|Landinstelling voor de taal|Tags|
|--|--|
|Chinees|`zh-cn`|
|Nederlands |`en-us`<br>`en-gb`<br>`en-au`<br>`en-in`|
|Frans |`fr-ca`<br>`fr-fr`|
|Duits|`de-de`|
|Italiaans|`it-it`|
|Japans|`ja-jp`|
|Koreaans|`ko-kr`|
|Portugees|`pt-br`|
|Spaans|`es-es`<br>`es-mx`|

#### <a name="text-to-speech-locales"></a>Landinstellingen van tekst naar spraak

Alle tags, met uitzondering van `latest` zijn in de volgende indeling hebben: waar de `<culture>` geeft aan dat de landinstelling en de `<voice>` geeft aan dat de stem van de container:

```
<major>.<minor>.<patch>-<platform>-<culture>-<voice>-<prerelease>
```

De volgende code is een voorbeeld van de indeling:

```
1.1.0-amd64-en-us-jessarus-preview
```

De volgende tabel bevat de ondersteunde landinstellingen voor **tekst naar spraak** in de 1.1.0 versie van de container:

|Landinstelling voor de taal|Tags|Ondersteunde stemmen|
|--|--|--|
|Chinees|`zh-cn`|huihuirus<br>kangkang-apollo<br>yaoyao-apollo|
|Nederlands |`en-au`|Catherine<br>hayleyrus|
|Nederlands |`en-gb`|george-apollo<br>hazelrus<br>susan-apollo|
|Nederlands |`en-in`|heera-apollo<br>priyarus<br>ravi-apollo<br>|
|Nederlands |`en-us`|jessarus<br>benjaminrus<br>jessa24krus<br>zirarus<br>guy24krus|
|Frans|`fr-ca`|caroline<br>harmonierus|
|Frans|`fr-fr`|hortenserus<br>julie-apollo<br>paul-apollo|
|Duits|`de-de`|hedda<br>heddarus<br>stefan-apollo|
|Italiaans|`it-it`|cosimo-apollo<br>luciarus|
|Japans|`ja-jp`|ayumi-apollo<br>harukarus<br>ichiro-apollo|
|Koreaans|`ko-kr`|heamirus|
|Portugees|`pt-br`|daniel-apollo<br>heloisarus|
|Spaans|`es-es`|elenarus<br>laura-apollo<br>pablo-apollo<br>|
|Spaans|`es-mx`|hildarus<br>raul-apollo|

### <a name="docker-pull-for-the-speech-containers"></a>Docker pull voor de spraak-containers

#### <a name="speech-to-text"></a>Spraak-naar-tekst

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text:latest
```

#### <a name="text-to-speech"></a>Tekst naar spraak

```Docker
docker pull containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech:latest
```

## <a name="how-to-use-the-container"></a>Het gebruik van de container

Als de container op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

1. [Uitvoeren van de container](#run-the-container-with-docker-run), met de facturering instellingen vereist, maar niet in gebruik. Meer [voorbeelden](speech-container-configuration.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar.
1. [Query van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint).

## <a name="run-the-container-with-docker-run"></a>De container met uitvoeren `docker run`

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren op een van de drie containers. De opdracht maakt gebruik van de volgende parameters:

**Tijdens de Preview-versie**, de facturering instellingen moeten geldig voor het starten van de container, maar worden niet in rekening gebracht voor gebruik.

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{BILLING_KEY} | Deze sleutel wordt gebruikt voor het starten van de container en is beschikbaar op de pagina van de spraak-sleutels van de Azure portal.  |
|{BILLING_ENDPOINT_URI} | Het eindpunt van de facturering URI-waarde is beschikbaar op de pagina van de spraak-overzicht van de Azure portal.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

### <a name="text-to-speech"></a>Tekst naar spraak

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 1 \
containerpreview.azurecr.io/microsoft/cognitive-services-text-to-speech \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

### <a name="speech-to-text"></a>Spraak-naar-tekst

```bash
docker run --rm -it -p 5000:5000 --memory 2g --cpus 2 \
containerpreview.azurecr.io/microsoft/cognitive-services-speech-to-text \
Eula=accept \
Billing={BILLING_ENDPOINT_URI} \
ApiKey={BILLING_KEY}
```

Met deze opdracht:

* Een spraak-container op de installatiekopie van de container wordt uitgevoerd
* 2 CPU-kernen en 2 GB (Gigabyte) geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer.

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

|Container|Eindpunt|
|--|--|
|Spraak-naar-tekst|ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1|
|Tekst naar spraak|http://localhost:5000/speech/synthesize/cognitiveservices/v1|

### <a name="speech-to-text"></a>Spraak-naar-tekst

De container biedt eindpunt van de query op basis van websocket API's, die toegankelijk zijn via de [spraak SDK](index.yml).

De spraak-SDK gebruikt standaard online spraakservices. Voor het gebruik van de container, moet u de van de initialisatiemethode wijzigen. Zie de onderstaande voorbeelden.

#### <a name="for-c"></a>VoorC#

Wijzigen van het gebruik van deze aanroep van de initialisatie van Azure-cloud:

```C#
var config = SpeechConfig.FromSubscription("YourSubscriptionKey", "YourServiceRegion");
```

met behulp van het eindpunt van de container op deze aanroep:

```C#
var config = SpeechConfig.FromEndpoint(
    new Uri("ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1"),
    "YourSubscriptionKey");
```

#### <a name="for-python"></a>Voor Python

In plaats van deze aanroep van de initialisatie van Azure-cloud

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, region=service_region)
```

met behulp van het eindpunt van de container op deze aanroep:

```python
speech_config = speechsdk.SpeechConfig(
    subscription=speech_key, endpoint="ws://localhost:5000/speech/recognition/dictation/cognitiveservices/v1")
```

### <a name="text-to-speech"></a>Tekst naar spraak

De container biedt REST-eindpunt API's die kunnen worden gevonden [hier](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech) en voorbeelden vindt [hier](https://azure.microsoft.com/resources/samples/cognitive-speech-tts/).

[!INCLUDE [Validate container is running - Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="stop-the-container"></a>De container stoppen

[!INCLUDE [How to stop the container](../../../includes/cognitive-services-containers-stop.md)]

## <a name="troubleshooting"></a>Problemen oplossen

Wanneer u de container uitvoert, wordt de container gebruikt **stdout** en **stderr** naar uitvoerinformatie die is het handig om het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container.

## <a name="billing"></a>Billing

Het verzenden van de containers spraak factuurgegevens naar Azure, met behulp van een _spraak_ resource voor uw Azure-account.

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](speech-container-configuration.md).

<!--blogs/samples/video coures -->

[!INCLUDE [Discoverability of more container information](../../../includes/cognitive-services-containers-discoverability.md)]

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en spraak containers uitvoeren. Samenvatting:

* Spraak biedt twee Linux-containers voor Docker, spraak naar tekst en tekst naar spraak encapsulating.
* Containerinstallatiekopieën worden gedownload van het privé containerregister in Azure.
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API of de SDK gebruiken om aan te roepen van bewerkingen in spraak containers door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
>  Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden klantgegevens (zoals de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](speech-container-configuration.md) voor configuratie-instellingen
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
