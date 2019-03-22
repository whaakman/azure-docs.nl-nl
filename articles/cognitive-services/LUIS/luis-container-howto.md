---
title: Docker-containers
titleSuffix: Language Understanding - Azure Cognitive Services
description: De LUIS-container in een docker-container, wordt uw app getrainde of gepubliceerd geladen en biedt toegang tot de voorspellingen van de query van de API-eindpunten van de container.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: article
ms.date: 03/19/2019
ms.author: diberry
ms.openlocfilehash: 1cf5fb00e9f1a202fe7ad46253f916e3e6bee7a7
ms.sourcegitcommit: ab6fa92977255c5ecbe8a53cac61c2cd2a11601f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/20/2019
ms.locfileid: "58295569"
---
# <a name="install-and-run-luis-docker-containers"></a>Installeren en uitvoeren van LUIS docker-containers
 
De container Language Understanding (LUIS) uw Language Understanding getrainde of gepubliceerde model wordt geladen, ook weten als een [LUIS-app](https://www.luis.ai), in een docker-container en biedt toegang tot de voorspellingen van de query uit van de container-API eindpunten. U kunt het verzamelen van Logboeken voor query's uit de container en deze terug naar het Azure Language Understanding-model voor het verbeteren van de nauwkeurigheid van de app uploaden.

De volgende video ziet u met behulp van deze container.

[![Demonstratie van de container voor Cognitive Services](./media/luis-container-how-to/luis-containers-demo-video-still.png)](https://aka.ms/luis-container-demo)

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten

Om uit te voeren de LUIS-container, moet u het volgende hebt: 

|Vereist|Doel|
|--|--|
|Docker-Engine| U moet de Docker-Engine zijn geïnstalleerd op een [hostcomputer](#the-host-computer). Docker biedt pakketten die de Docker-omgeving configureren op [macOS](https://docs.docker.com/docker-for-mac/), [Windows](https://docs.docker.com/docker-for-windows/), en [Linux](https://docs.docker.com/engine/installation/#supported-platforms). Zie voor een uitleg van de basisprincipes van Docker en containers, de [dockeroverzicht](https://docs.docker.com/engine/docker-overview/).<br><br> Docker moet worden geconfigureerd, zodat de containers om te verbinden met en facturering gegevens verzenden naar Azure. <br><br> **Op Windows**, Docker moet ook worden geconfigureerd ter ondersteuning van Linux-containers.<br><br>|
|Vertrouwd zijn met Docker | U hebt een basiskennis hebt van Docker-kernconcepten zoals registers, -opslagplaatsen, containers, en containerinstallatiekopieën, evenals kennis van basic `docker` opdrachten.| 
|Language Understanding (LUIS) resource en de bijbehorende app |Als u wilt gebruiken in de container, moet u het volgende hebben:<br><br>* A [ _Language Understanding_ Azure-resource](luis-how-to-azure-subscription.md), samen met de bijbehorende eindpuntsleutel en het eindpunt URI (die wordt gebruikt als het eindpunt van de facturering).<br>* Een getraind of gepubliceerde app verpakt als een gekoppelde invoer voor de container met de bijbehorende App-ID.<br>* De ontwerp-sleutel voor het downloaden van het app-pakket, als u dit vanuit de API doen.<br><br>Deze vereisten worden gebruikt voor de opdrachtregelargumenten doorgeven aan de volgende variabelen:<br><br>**{AUTHORING_KEY}** : Deze sleutel wordt gebruikt voor het ophalen van de app-pakket van de LUIS-service in de cloud en de logboeken voor query's uploaden naar de cloud. De indeling is `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx`.<br><br>**{APPLICATION_ID}** : Deze ID wordt gebruikt om de App te selecteren. De indeling is `xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx`.<br><br>**{ENDPOINT_KEY}**: Deze sleutel wordt gebruikt voor het starten van de container. U vindt de eindpuntsleutel op twee plaatsen. De eerste is de Azure-portal binnen de _Language Understanding_ lijst met resources van sleutels. De eindpuntsleutel is ook beschikbaar in de LUIS-portal op de sleutels en het eindpunt instellingenpagina. Gebruik niet de starter-sleutel.<br><br>**{BILLING_ENDPOINT}**: De facturering eindpuntwaarde is beschikbaar op de pagina van de taal begrijpen overzicht van de Azure portal. Een voorbeeld is: `https://westus.api.cognitive.microsoft.com/luis/v2.0`.<br><br>De [ontwerpen en eindpuntsleutel](luis-boundaries.md#key-limits) hebben verschillende doeleinden. Gebruik deze niet door elkaar. |

### <a name="the-host-computer"></a>De hostcomputer

[!INCLUDE [Request access to private preview](../../../includes/cognitive-services-containers-host-computer.md)]

### <a name="container-requirements-and-recommendations"></a>Containervereisten en aanbevelingen

Deze container biedt ondersteuning voor minimale en aanbevolen waarden voor de instellingen:

|Container| Minimum | Aanbevolen | TPS<br>(Minimum, Maximum)|
|-----------|---------|-------------|--|
|LUIS|1 core, 2 GB geheugen|1 core, 4 GB geheugen|20,40|

* Elke core moet ten minste 2,6 GHz (gigahertz) of sneller.
* TPS - transacties per seconde

Kernen en geheugen komen overeen met de `--cpus` en `--memory` instellingen die worden gebruikt als onderdeel van de `docker run` opdracht.

## <a name="get-the-container-image-with-docker-pull"></a>Met de installatiekopie van de container ophalen `docker pull`

Gebruik de [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) opdracht voor het downloaden van een containerinstallatiekopie uit de `mcr.microsoft.com/azure-cognitive-services/luis` opslagplaats:

```
docker pull mcr.microsoft.com/azure-cognitive-services/luis:latest
```

Gebruik de [ `docker pull` ](https://docs.docker.com/engine/reference/commandline/pull/) opdracht om een containerinstallatiekopie te downloaden.

Voor een volledige beschrijving van de beschikbare labels, zoals `latest` in de voorgaande opdracht gebruikt, Zie [LUIS](https://go.microsoft.com/fwlink/?linkid=2043204) op Docker Hub.

[!INCLUDE [Tip for using docker list](../../../includes/cognitive-services-containers-docker-list-tip.md)]


## <a name="how-to-use-the-container"></a>Het gebruik van de container

Als de container op de [hostcomputer](#the-host-computer), de volgende procedure gebruiken om te werken met de container.

![Proces voor het gebruik van de container Language Understanding (LUIS)](./media/luis-container-how-to/luis-flow-with-containers-diagram.jpg)

1. [Pakket exporteren](#export-packaged-app-from-luis) voor de container van LUIS-portal of LUIS APIs.
1. Pakketbestand verplaatsen naar de vereiste **invoer** map op de [hostcomputer](#the-host-computer). Wijzig, alter, of niet decomprimeren van LUIS-pakketbestand.
1. [Uitvoeren van de container](##run-the-container-with-docker-run), met de vereiste _invoer koppelpunt_ en instellingen voor facturering. Meer [voorbeelden](luis-container-configuration.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 
1. [Uitvoeren van query's van de container voorspelling eindpunt](#query-the-containers-prediction-endpoint). 
1. Wanneer u klaar bent met de container [importeren van de eindpunt-logboeken](#import-the-endpoint-logs-for-active-learning) uit de uitvoer koppelen aan de LUIS-portal en [stoppen](#stop-the-container) de container.
1. Gebruik LUIS portal [actief leren](luis-how-to-review-endoint-utt.md) op de **bekijken eindpunt uitingen** pagina voor het verbeteren van de app.

De app die wordt uitgevoerd in de container kan niet worden gewijzigd. In volgorde de wijziging van de app in de container, moet u instellen dat de app in de LUIS-service met de [LUIS](https://www.luis.ai) portal of gebruik de LUIS [API's ontwerpen](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c2f). Vervolgens trainen en/of publiceren, klikt u vervolgens een nieuw pakket downloaden en voer de container opnieuw uit.

De LUIS-app in de container kan niet worden geëxporteerd naar de LUIS-service. Alleen de logboeken van de query's kunnen worden geüpload. 

## <a name="export-packaged-app-from-luis"></a>App-pakket exporteren van LUIS

De LUIS-container is een getraind of gepubliceerde LUIS-app voor het beantwoorden van query's de voorspelling van de gebruiker uitingen vereist. Gebruik om de LUIS-app downloaden, ofwel het getrainde of het gepubliceerde pakket API. 

De standaardlocatie is de `input` submap ten opzichte van waar u uitvoeren de `docker run` opdracht.  

Plaats het pakketbestand in een map en deze map als de invoer koppelpunt verwijzen tijdens het uitvoeren van de docker-container. 

### <a name="package-types"></a>Pakkettypen

De invoer Koppelmap mag de **productie**, **fasering**, en **Trained** versies van de app tegelijkertijd. Alle pakketten zijn gekoppeld. 

|Pakkettype|API-query-eindpunt|Query-beschikbaarheid|De indeling van de pakket-bestandsnaam|
|--|--|--|--|
|Getraind|Get, Post|Alleen container|`{APPLICATION_ID}_v{APPLICATION_VERSION}.gz`|
|Faseren|Get, Post|Azure en de container|`{APPLICATION_ID}_STAGING.gz`|
|Productie|Get, Post|Azure en de container|`{APPLICATION_ID}_PRODUCTION.gz`|

> [!IMPORTANT]
> Wijzig, alter, of niet decomprimeren van de LUIS-pakketbestanden.

### <a name="packaging-prerequisites"></a>Verpakking-vereisten

Voordat u een LUIS-toepassing verpakken, moet u het volgende hebt:

|Verpakking-vereisten|Details|
|--|--|
|Azure _Language Understanding_ bronexemplaar|Ondersteunde regio 's<br><br>VS-West (```westus```)<br>West Europe (```westeurope```)<br>Australië-Oost (```australiaeast```)|
|Getrainde of gepubliceerd LUIS-app|Zonder [afhankelijkheden niet-ondersteunde](#unsupported-dependencies). |
|Toegang tot de [hostcomputer](#the-host-computer)van bestandssysteem |De computer moet toestaan een [invoer koppelpunt](luis-container-configuration.md#mount-settings).|
  
### <a name="export-app-package-from-luis-portal"></a>App-pakket exporteren vanuit LUIS-portal

De LUIS [portal](https://www.luis.ai) biedt de mogelijkheid om de app getrainde of gepubliceerd pakket exporteren. 

### <a name="export-published-apps-package-from-luis-portal"></a>Gepubliceerde app-pakket exporteren vanuit LUIS-portal

De gepubliceerde app-pakket is beschikbaar via de **mijn Apps** pagina. 

1. Aanmelden bij de LUIS [portal](https://www.luis.ai).
1. Schakel het selectievakje aan de linkerkant van de naam van de app in de lijst. 
1. Selecteer de **exporteren** item uit de contextuele werkbalk boven de lijst.
1. Selecteer **exporteren voor container (GZIP)**.
1. Selecteer de omgeving van **productiesite** of **Staging-site**.
1. Het pakket wordt gedownload vanuit de browser.

![Exporteren van het gepubliceerde pakket voor de container in de App-pagina exporteren menu](./media/luis-container-how-to/export-published-package-for-container.png)

### <a name="export-trained-apps-package-from-luis-portal"></a>Ervaren app-pakket exporteren vanuit LUIS-portal

De ervaren app-pakket is beschikbaar via de **versies** pagina. 

1. Aanmelden bij de LUIS [portal](https://www.luis.ai).
1. Selecteer de app in de lijst. 
1. Selecteer **beheren** in de navigatiebalk van de app.
1. Selecteer **versies** in de linker navigatiebalk.
1. Schakel het selectievakje aan de linkerkant van de versienaam van de in de lijst.
1. Selecteer de **exporteren** item uit de contextuele werkbalk boven de lijst.
1. Selecteer **exporteren voor container (GZIP)**.
1. Het pakket wordt gedownload vanuit de browser.

![Exporteren van het getrainde pakket voor de container in op de pagina versies Export menu](./media/luis-container-how-to/export-trained-package-for-container.png)


### <a name="export-published-apps-package-from-api"></a>Gepubliceerde app-pakket uit API exporteren

De volgende REST-API-methode gebruiken om een LUIS-app die u hebt al [gepubliceerd](luis-how-to-publish-app.md). Vervangen door uw eigen juiste waarden voor de tijdelijke aanduidingen in de API-aanroep met behulp van de tabel hieronder de HTTP-specificatie.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{APPLICATION_ID} | De toepassings-ID van de gepubliceerde LUIS-app. |
|{APPLICATION_ENVIRONMENT} | De omgeving van de gepubliceerde LUIS-app. Gebruik een van de volgende waarden:<br/>```PRODUCTION```<br/>```STAGING``` |
|{AUTHORING_KEY} | De ontwerphandleiding sleutel van de LUIS-account voor de gepubliceerde LUIS-app.<br/>U krijgt uw authoring sleutel uit de **gebruikersinstellingen** pagina op de LUIS-portal. |
|{AZURE_REGION} | De juiste Azure-regio:<br/><br/>```westus``` -VS-west<br/>```westeurope``` -West-Europa<br/>```australiaeast``` -Australië-Oost |

Gebruik de volgende CURL-opdracht voor het downloaden van het gepubliceerde pakket, waarbij u uw eigen waarden vervangt:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/slot/{APPLICATION_ENVIRONMENT}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_{APPLICATION_ENVIRONMENT}.gz
```

Als dit lukt, is het antwoord een LUIS-pakketbestand. Sla het bestand in de opslaglocatie die is opgegeven voor de invoer koppelen van de container. 

### <a name="export-trained-apps-package-from-api"></a>Ervaren app-pakket uit API exporteren

De volgende REST-API-methode gebruiken om een LUIS-toepassing die u hebt al [getrainde](luis-how-to-train.md). Vervangen door uw eigen juiste waarden voor de tijdelijke aanduidingen in de API-aanroep met behulp van de tabel hieronder de HTTP-specificatie.

```http
GET /luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip HTTP/1.1
Host: {AZURE_REGION}.api.cognitive.microsoft.com
Ocp-Apim-Subscription-Key: {AUTHORING_KEY}
```

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{APPLICATION_ID} | De toepassings-ID van het getrainde LUIS-toepassing. |
|{APPLICATION_VERSION} | De versie van de toepassing van het getrainde LUIS-toepassing. |
|{AUTHORING_KEY} | De ontwerphandleiding sleutel van de LUIS-account voor de gepubliceerde LUIS-app.<br/>U krijgt uw authoring sleutel uit de **gebruikersinstellingen** pagina op de LUIS-portal.  |
|{AZURE_REGION} | De juiste Azure-regio:<br/><br/>```westus``` -VS-west<br/>```westeurope``` -West-Europa<br/>```australiaeast``` -Australië-Oost |

Gebruik de volgende CURL-opdracht het getrainde pakket te downloaden:

```bash
curl -X GET \
https://{AZURE_REGION}.api.cognitive.microsoft.com/luis/api/v2.0/package/{APPLICATION_ID}/versions/{APPLICATION_VERSION}/gzip  \
 -H "Ocp-Apim-Subscription-Key: {AUTHORING_KEY}" \
 -o {APPLICATION_ID}_v{APPLICATION_VERSION}.gz
```

Als dit lukt, is het antwoord een LUIS-pakketbestand. Sla het bestand in de opslaglocatie die is opgegeven voor de invoer koppelen van de container. 

## <a name="run-the-container-with-docker-run"></a>De container met uitvoeren `docker run`

Gebruik de [docker uitvoeren](https://docs.docker.com/engine/reference/commandline/run/) opdracht uit te voeren van de container. De opdracht maakt gebruik van de volgende parameters:

| Tijdelijke aanduiding | Value |
|-------------|-------|
|{ENDPOINT_KEY} | Deze sleutel wordt gebruikt voor het starten van de container. Gebruik niet de starter-sleutel. |
|{BILLING_ENDPOINT} | De facturering eindpuntwaarde is beschikbaar op de pagina van de taal begrijpen overzicht van de Azure portal.|

Deze parameters vervangen door uw eigen waarden in het volgende voorbeeld `docker run` opdracht.

```bash
docker run --rm -it -p 5000:5000 --memory 4g --cpus 2 \
--mount type=bind,src=c:\input,target=/input \
--mount type=bind,src=c:\output,target=/output \
mcr.microsoft.com/azure-cognitive-services/luis \
Eula=accept \
Billing={BILLING_ENDPOINT} \
ApiKey={ENDPOINT_KEY}
```

> [!Note] 
> De voorgaande opdracht maakt gebruik van de map uit de `c:` station om te voorkomen van machtigingsconflicten op Windows. Als u gebruiken van een specifieke map als de invoermap wilt, moet u mogelijk de docker verlenen machtiging-service. De voorgaande docker-opdracht maakt gebruik van de backslash `\`, als een voortzetting van regel tekens. Vervang of verwijder deze op basis van uw [hostcomputer](#the-host-computer) vereisten van het besturingssysteem. Wijzig de volgorde van de argumenten niet, tenzij u bekend bent met docker-containers.


Met deze opdracht:

* Een container uitvoert van de LUIS containerinstallatiekopie
* LUIS-app uit de invoer koppelpunt op c:\input, bevindt zich op containerhost geladen
* Twee CPU-kernen en 4 GB (Gigabyte) aan geheugen worden toegewezen
* Gebruikt TCP-poort 5000 en wijst er een pseudo-TTY voor de container
* Container en LUIS slaat Logboeken als u wilt koppelen aan c:\output, bevindt zich op containerhost uitvoeren
* De container worden automatisch verwijderd nadat deze is afgesloten. De containerinstallatiekopie is nog steeds beschikbaar op de hostcomputer. 

Meer [voorbeelden](luis-container-configuration.md#example-docker-run-commands) van de `docker run` opdrachten zijn beschikbaar. 

> [!IMPORTANT]
> De `Eula`, `Billing`, en `ApiKey` opties moeten worden opgegeven voor het uitvoeren van de container; anders wordt de container niet start.  Zie voor meer informatie, [facturering](#billing).
> De waarde ApiKey is de **sleutel** pagina van de sleutels en de eindpunten in de portal LUIS en is ook beschikbaar op de pagina sleutels in Azure Language Understanding Resource.  

[!INCLUDE [Running multiple containers on the same host](../../../includes/cognitive-services-containers-run-multiple-same-host.md)]

## <a name="query-the-containers-prediction-endpoint"></a>Query uitvoeren op het eindpunt voorspelling van de container

De container biedt eindpunt van de voorspelling query op basis van REST API's. Eindpunten voor gepubliceerde (fasering of productie)-apps hebben een _verschillende_ route dan eindpunten voor getrainde apps. 

Gebruikmaken van de host `https://localhost:5000`, voor de container met API's. 

|Pakkettype|Methode|Route|Queryparameters|
|--|--|--|--|
|Gepubliceerd|[Ophalen](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee78), [Post](https://westus.dev.cognitive.microsoft.com/docs/services/5819c76f40a6350ce09de1ac/operations/5819c77140a63516d81aee79)|/luis/v2.0/apps/{appId}?|q={q}<br>& voor fasering<br>[&timezoneOffset]<br>[& uitgebreide]<br>[&log]<br>|
|Getraind|Get, Post|/ luis/v2.0/apps/{appId}/versions/{versionId}?|q={q}<br>[&timezoneOffset]<br>[& uitgebreide]<br>[&log]|

De queryparameters configureren hoe en wat wordt geretourneerd in antwoord op de query:

|Queryparameter|Type|Doel|
|--|--|--|
|`q`|string|Utterance van de gebruiker.|
|`timezoneOffset`|getal|U kunt de timezoneOffset [wijzigt de tijdzone](luis-concept-data-alteration.md#change-time-zone-of-prebuilt-datetimev2-entity) die worden gebruikt door de vooraf gedefinieerde entiteit datetimeV2.|
|`verbose`|booleaans|Retourneert alle intents en hun scores wanneer ingesteld op true. Standaard is ingesteld op false, die alleen het belangrijkste doel retourneert.|
|`staging`|booleaans|Query retourneert uit de resultaten van de omgeving voor fasering als is ingesteld op true. |
|`log`|booleaans|Logboekregistratie van query's, die later kunnen worden gebruikt voor [actief leren](luis-how-to-review-endoint-utt.md). De standaardinstelling is true.|

### <a name="query-published-app"></a>Gepubliceerde app query

Er is een voorbeeld van de CURL-opdracht voor het uitvoeren van query's de container voor een gepubliceerde app:

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}?q=turn%20on%20the%20lights&staging=false&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
Om te maken van query's naar de **Staging-** omgeving, wijzigen de **staging-** queryreekswaarde parameter op ' True ': 

`staging=true`

### <a name="query-trained-app"></a>Getrainde app query

Een voorbeeld CURL-opdracht voor het uitvoeren van query's de container voor een getraind app is: 

```bash
curl -X GET \
"http://localhost:5000/luis/v2.0/apps/{APPLICATION_ID}/versions/{APPLICATION_VERSION}?q=turn%20on%20the%20lights&timezoneOffset=0&verbose=false&log=true" \
-H "accept: application/json"
```
De naam van maximaal 10 tekens heeft en alleen de tekens die zijn toegestaan in een URL bevat. 

## <a name="import-the-endpoint-logs-for-active-learning"></a>Importeren van de eindpunt-logboeken voor actief leren

Als een output-koppelpunt is opgegeven voor de container LUIS, worden app-query-logboekbestanden opgeslagen in de uitvoermap, waar {INSTANCE_ID} is voor de container-ID. Het querylogboek app bevat de query, antwoord en tijdstempels voor elke voorspellingsquery verzonden naar de LUIS-container. 

De volgende locatie ziet u de structuur van de geneste map voor de logboekbestanden van de container.
`
/output/luis/{INSTANCE_ID}/
`
 
Vanuit de portal LUIS, selecteert u uw app en vervolgens **eindpunt-logboeken importeren** om deze logboeken te uploaden. 

![Importeren van de logboekbestanden van de container voor actief leren](./media/luis-container-how-to/upload-endpoint-log-files.png)

Nadat het logboek is geüpload, [bekijken van het eindpunt](https://docs.microsoft.com/azure/cognitive-services/luis/luis-concept-review-endpoint-utterances) uitingen in de LUIS-portal.

## <a name="stop-the-container"></a>De container stoppen

Als u wilt afsluiten van de container, in de opdrachtregelomgeving waarop de container wordt uitgevoerd, drukt u op **Ctrl + C**.

## <a name="troubleshooting"></a>Problemen oplossen

Als u de container wordt uitgevoerd met een uitvoer [koppelen](luis-container-configuration.md#mount-settings) en logboekregistratie is ingeschakeld, wordt de container genereert logboekbestanden die tot het oplossen van problemen die optreden tijdens het starten of uitvoeren van de container. 

## <a name="containers-api-documentation"></a>API-documentatie van de container

[!INCLUDE [Container's API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

## <a name="billing"></a>Billing

De LUIS container verzendt factuurgegevens naar Azure, met behulp van een _Language Understanding_ resource voor uw Azure-account. 

[!INCLUDE [Container's Billing Settings](../../../includes/cognitive-services-containers-how-to-billing-info.md)]

Zie voor meer informatie over deze opties [containers configureren](luis-container-configuration.md).

## <a name="unsupported-dependencies"></a>Niet-ondersteunde afhankelijkheden

U kunt een LUIS-toepassing gebruiken als het **bevat geen** een van de volgende afhankelijkheden:

Niet-ondersteunde app-configuraties|Details|
|--|--|
|Niet-ondersteunde container culturen| Duits (nl-nl)<br>Nederlands (nl-NL)<br>Japans (ja-JP)<br>|
|Niet-ondersteunde domeinen|Vooraf gemaakte domeinen, met inbegrip van vooraf gedefinieerde domein intenties en entiteiten|
|Niet-ondersteunde entiteiten voor alle culturen|[KeyPhrase](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-keyphrase) vooraf gemaakte entiteiten voor alle culturen|
|Niet-ondersteunde entiteiten voor de cultuur Engels (en-US)|[GeographyV2](https://docs.microsoft.com/azure/cognitive-services/luis/luis-reference-prebuilt-geographyv2) vooraf gemaakte entiteiten|
|Spraak voorbereiden|Externe afhankelijkheden worden niet ondersteund in de container.|
|Sentimentanalyse|Externe afhankelijkheden worden niet ondersteund in de container.|
|Bing spellingcontrole|Externe afhankelijkheden worden niet ondersteund in de container.|

## <a name="summary"></a>Samenvatting

In dit artikel hebt u geleerd concepten en werkstroom voor het downloaden, installeren en uitvoeren van containers met Language Understanding (LUIS). Samenvatting:

* Language Understanding (LUIS) biedt een Linux-container voor Docker die eindpunt query voorspellingen van uitingen.
* Containerinstallatiekopieën worden gedownload uit het Microsoft Container Registry (MCR).
* Containerinstallatiekopieën uitvoeren in Docker.
* U kunt de REST-API gebruiken om op te vragen van de containereindpunten door de host-URI van de container op te geven.
* Bij het instantiëren van een container, moet u informatie over facturering opgeven.

> [!IMPORTANT]
> Cognitive Services-containers zijn geen licentie om uit te voeren zonder verbinding met Azure voor het meten. Klanten moeten de containers om te communiceren factureringsgegevens met de softwarelicentiecontrole-service te allen tijde inschakelen. Cognitive Services-containers verzenden gegevens van de klant (bijvoorbeeld, de afbeelding of tekst die wordt geanalyseerd) niet naar Microsoft.

## <a name="next-steps"></a>Volgende stappen

* Beoordeling [containers configureren](luis-container-configuration.md) voor configuratie-instellingen
* Raadpleeg [probleemoplossing](troubleshooting.md) het oplossen van problemen met betrekking tot LUIS-functionaliteit.
* Meer [Cognitive Services-Containers](../cognitive-services-container-support.md)
