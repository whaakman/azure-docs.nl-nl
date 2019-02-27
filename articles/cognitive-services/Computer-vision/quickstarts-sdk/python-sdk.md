---
title: 'Quickstart: Python-SDK'
titleSuffix: Azure Cognitive Services
description: In deze quickstart leert u hoe u de Python SDK gebruikt voor veelvoorkomende taken.
services: cognitive-services
author: PatrickFarley
manager: nitinme
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: quickstart
ms.date: 02/15/2019
ms.author: pafarley
ms.openlocfilehash: 3043067f326f782c51be38382070ae0db0e90f4d
ms.sourcegitcommit: f7be3cff2cca149e57aa967e5310eeb0b51f7c77
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/15/2019
ms.locfileid: "56314166"
---
# <a name="azure-cognitive-services-computer-vision-sdk-for-python"></a>Azure Cognitive Services Computer Vision SDK voor Python

De Computer Vision-service geeft ontwikkelaars toegang tot geavanceerde algoritmen voor het verwerken van afbeeldingen en het retourneren van informatie. Met Computer Vision-algoritmen kunt u de inhoud van een afbeelding op verschillende manieren analyseren, afhankelijk van de visuele kenmerken waarin u geïnteresseerd bent. U kunt met Computer Vision bijvoorbeeld bepalen of een afbeelding inhoud voor volwassenen of ongepaste inhoud bevat, alle gezichten in een afbeelding zoeken of handgeschreven of gedrukte tekst ophalen. De service werkt met populaire afbeeldingsindelingen, zoals JPEG en PNG. 

U kunt Computer Vision in uw toepassing voor het volgende gebruiken:

- Analyseren van afbeeldingen voor inzicht
- Extraheren van tekst uit afbeeldingen
- Miniaturen genereren

Zoekt u meer documentatie?

* [SDK-naslagdocumentatie](https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision)
* [Documentatie over Cognitive Services Computer Vision](https://docs.microsoft.com/azure/cognitive-services/computer-vision/)

## <a name="prerequisites"></a>Vereisten

* Azure-abonnement: [maak een gratis account][azure_sub]
* Azure [Computer Vision-resource][computervision_resource]
* [Python 3.6+][python]

Als u een Computer Vision-API-account nodig hebt, kunt u er met deze [Azure CLI][azure_cli]-opdracht een maken:

```Bash
RES_REGION=westeurope 
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

az cognitiveservices account create \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --location $RES_REGION \
    --kind ComputerVision \
    --sku S1 \
    --yes
```

## <a name="installation"></a>Installatie

Installeer de Azure Cognitive Services Computer Vision SDK met [pip][pip], eventueel in een [virtuele omgeving][venv].

### <a name="configure-a-virtual-environment-optional"></a>Een virtuele omgeving configureren (optioneel)

Hoewel dit niet is vereist, kunt u met een [virtuele omgeving][virtualenv] uw basissysteem en Azure SDK-omgevingen van elkaar geïsoleerd houden. Voer de volgende opdrachten uit om een virtuele omgeving te configureren en vervolgens te openen met [venv][venv], bijvoorbeeld `cogsrv-vision-env`:

```Bash
python3 -m venv cogsrv-vision-env
source cogsrv-vision-env/bin/activate
```

### <a name="install-the-sdk"></a>De SDK installeren

Installeer het [pakket][pypi_computervision] voor de Azure Cognitive Services Computer Vision SDK voor Python met [pip][pip]:

```Bash
pip install azure-cognitiveservices-vision-computervision
```

## <a name="authentication"></a>Verificatie

Zodra u uw Computer Vision-resource hebt gemaakt, hebt u de **regio** en een van de **accountsleutels** nodig om een instantie van het clientobject te maken.

Gebruik deze waarden bij het maken van de instantie van het [ComputerVisionAPI][ref_computervisionclient]-clientobject. 

### <a name="get-credentials"></a>Referenties ophalen

Gebruik het [Azure CLI][cloud_shell]-codefragment hieronder om twee omgevingsvariabelen in te vullen met de **regio** en een van de **sleutels** van het Computer Vision-account (u vindt deze waarden ook in de [Azure-portal][azure_portal]). Het codefragment is geformatteerd voor de Bash-shell.

```Bash
RES_GROUP=<resourcegroup-name>
ACCT_NAME=<computervision-account-name>

export ACCOUNT_REGION=$(az cognitiveservices account show \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query location \
    --output tsv)

export ACCOUNT_KEY=$(az cognitiveservices account keys list \
    --resource-group $RES_GROUP \
    --name $ACCT_NAME \
    --query key1 \
    --output tsv)
```

### <a name="create-client"></a>Client maken

Nadat u de omgevingsvariabelen `ACCOUNT_REGION` en `ACCOUNT_KEY` hebt ingevuld, kunt u het [ComputerVisionAPI][ref_computervisionclient]-clientobject maken.

```Python
from azure.cognitiveservices.vision.computervision import ComputerVisionAPI
from azure.cognitiveservices.vision.computervision.models import VisualFeatureTypes
from msrest.authentication import CognitiveServicesCredentials

import os
region = os.environ['ACCOUNT_REGION']
key = os.environ['ACCOUNT_KEY']

credentials = CognitiveServicesCredentials(key)
client = ComputerVisionAPI(region, credentials)
```

## <a name="usage"></a>Gebruik

Nadat u een [ComputerVisionAPI][ref_computervisionclient]-clientobject hebt geïnitialiseerd, kunt u het volgende doen:

* Een afbeelding analyseren: U kunt een afbeelding analyseren op bepaalde kenmerken, zoals gezichten, kleuren en labels.   
* Miniaturen genereren: Maak een aangepaste JPEG-afbeelding voor gebruik als een miniatuur van de oorspronkelijke afbeelding.
* Beschrijving van een afbeelding ophalen: Haal een beschrijving van de afbeelding op aan de hand van het onderwerpdomein. 

Zie [Wat is Computer Vision?][computervision_docs] voor meer informatie over deze service.

## <a name="examples"></a>Voorbeelden

In de volgende gedeelten vindt u enkele codefragmenten die betrekking hebben op een aantal van de meest algemene Computer Vision-taken, met inbegrip van:

* [Een afbeelding analyseren](#analyze-an-image)
* [Lijst met onderwerpdomeinen ophalen](#get-subject-domain-list)
* [Een afbeelding op domein analyseren](#analyze-an-image-by-domain)
* [Tekstbeschrijving van een afbeelding ophalen](#get-text-description-of-an-image)
* [Handgeschreven tekst uit een afbeelding ophalen](#get-text-from-image)
* [Miniatuur genereren](#generate-thumbnail)

### <a name="analyze-an-image"></a>Een afbeelding analyseren

U kunt een afbeelding analyseren op bepaalde kenmerken met [`analyze_image`][ref_computervisionclient_analyze_image]. Gebruik de eigenschap [`visual_features`][ref_computervision_model_visualfeatures] om de soorten analyse in te stellen die u op de afbeelding wilt uitvoeren. Veelvoorkomende waarden zijn `VisualFeatureTypes.tags` en `VisualFeatureTypes.description`.

```Python
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"

image_analysis = client.analyze_image(url,visual_features=[VisualFeatureTypes.tags])

for tag in image_analysis.tags:
    print(tag)
```

### <a name="get-subject-domain-list"></a>Lijst met onderwerpdomeinen ophalen

Bekijk de onderwerpdomeinen die zijn gebruikt om uw afbeelding te analyseren met [`list_models`][ref_computervisionclient_list_models]. Deze domeinnamen worden gebruikt bij het [analyseren van een afbeelding op domein](#analyze-an-image-by-domain). Een voorbeeld van een domein is `landmarks`.

```Python
models = client.list_models()

for x in models.models_property:
    print(x)
```

### <a name="analyze-an-image-by-domain"></a>Een afbeelding op domein analyseren

U kunt een afbeelding op onderwerpdomein analyseren met [`analyze_image_by_domain`][ref_computervisionclient_analyze_image_by_domain]. Haal de [lijst met ondersteunde onderwerpdomeinen](#get-subject-domain-list) op zodat u de juiste domeinnaam kunt gebruiken.  

```Python
domain = "landmarks"
url = "https://upload.wikimedia.org/wikipedia/commons/thumb/1/12/Broadway_and_Times_Square_by_night.jpg/450px-Broadway_and_Times_Square_by_night.jpg"
language = "en"

analysis = client.analyze_image_by_domain(domain, url, language)

for landmark in analysis.result["landmarks"]:
    print(landmark["name"])
    print(landmark["confidence"])
```

### <a name="get-text-description-of-an-image"></a>Tekstbeschrijving van een afbeelding ophalen

U kunt een op taal gebaseerde tekstbeschrijving van een afbeelding ophalen met [`describe_image`][ref_computervisionclient_describe_image]. Vraag verschillende beschrijvingen op met de eigenschap `max_description` als u een tekstanalyse uitvoert voor trefwoorden die aan de afbeelding zijn gekoppeld. Voorbeelden van een tekstbeschrijving voor de volgende afbeelding zijn onder meer `a train crossing a bridge over a body of water`, `a large bridge over a body of water` en `a train crossing a bridge over a large body of water`.

```Python
domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

analysis = client.describe_image(url, max_descriptions, language)

for caption in analysis.captions:
    print(caption.text)
    print(caption.confidence)
```

### <a name="get-text-from-image"></a>Tekst uit een afbeelding ophalen

U kunt handgeschreven of gedrukte tekst uit een afbeelding ophalen. Hiervoor zijn twee aanroepen aan de SDK nodig: [`recognize_text`][ref_computervisionclient_recognize_text] en [`get_text_operation_result`][ref_computervisionclient_get_text_operation_result]. De aanroep recognize_text is asynchroon. Controleer eerst in de resultaten van de aanroep get_text_operation_result of de eerste aanroep is voltooid met [`TextOperationStatusCodes`][ref_computervision_model_textoperationstatuscodes] voordat u de tekstgegevens extraheert. De resultaten bevatten de tekst en de coördinaten van het begrenzingsvak voor de tekst. 

```Python
url = "https://azurecomcdn.azureedge.net/cvt-1979217d3d0d31c5c87cbd991bccfee2d184b55eeb4081200012bdaf6a65601a/images/shared/cognitive-services-demos/read-text/read-1-thumbnail.png"
mode = TextRecognitionMode.handwritten
raw = True
custom_headers = None
numberOfCharsInOperationId = 36

# SDK call
rawHttpResponse = client.recognize_text(url, mode, custom_headers,  raw)

# Get ID from returned headers
operationLocation = rawHttpResponse.headers["Operation-Location"]
idLocation = len(operationLocation) - numberOfCharsInOperationId
operationId = operationLocation[idLocation:]

# SDK call
result = client.get_text_operation_result(operationId)

# Get data
if result.status == TextOperationStatusCodes.succeeded:

    for line in result.recognition_result.lines:
        print(line.text)
        print(line.bounding_box)
```

### <a name="generate-thumbnail"></a>Miniatuur genereren

U kunt een miniatuur (JPG) van een afbeelding genereren met [`generate_thumbnail`][ref_computervisionclient_generate_thumbnail]. De miniatuur hoeft niet dezelfde verhoudingen te hebben als de oorspronkelijke afbeelding. 

In dit voorbeeld wordt het pakket [Pillow][pypi_pillow] gebruikt om de nieuwe miniatuurafbeelding lokaal op te slaan.

```Python
from PIL import Image
import io

width = 50
height = 50
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"

thumbnail = client.generate_thumbnail(width, height, url)

for x in thumbnail:
    image = Image.open(io.BytesIO(x))

image.save('thumbnail.jpg')
```

## <a name="troubleshooting"></a>Problemen oplossen

### <a name="general"></a>Algemeen

Wanneer u met het clientobject [ComputerVisionAPI][ref_computervisionclient] communiceert met behulp van de SDK voor Python, wordt de klasse [`ComputerVisionErrorException`][ref_computervision_computervisionerrorexception] gebruikt voor het retourneren van fouten. Fouten die door de service worden geretourneerd, komen overeen met de HTTP-statuscodes die voor REST API-aanvragen worden geretourneerd.

Als u bijvoorbeeld probeert een afbeelding met een ongeldige sleutel te analyseren, wordt de fout `401` geretourneerd. In het volgende codefragment wordt de [fout][ref_httpfailure] mooi verwerkt door de uitzondering vast te leggen en aanvullende informatie over de fout weer te geven.

```Python

domain = "landmarks"
url = "http://www.public-domain-photos.com/free-stock-photos-4/travel/san-francisco/golden-gate-bridge-in-san-francisco.jpg"
language = "en"
max_descriptions = 3

try:
    analysis = client.describe_image(url, max_descriptions, language)

    for caption in analysis.captions:
        print(caption.text)
        print(caption.confidence)
except HTTPFailure as e:
    if e.status_code == 401:
        print("Error unauthorized. Make sure your key and region are correct.")
    else:
        raise
```

### <a name="handle-transient-errors-with-retries"></a>Tijdelijke fouten verwerken met nieuwe pogingen

Bij het werken met de [ComputerVisionAPI][ref_computervisionclient]-client kunnen er tijdelijke fouten optreden die worden veroorzaakt door [frequentielimieten][computervision_request_units] die worden afgedwongen door de service, of andere tijdelijke problemen zoals netwerkstoringen. Raadpleeg voor meer informatie over het verwerken van dit soort fouten [Patroon voor opnieuw proberen][azure_pattern_retry] in de handleiding Cloudontwerppatronen en de verwante [Patroon Circuitonderbreker][azure_pattern_circuit_breaker].

## <a name="next-steps"></a>Volgende stappen

### <a name="more-sample-code"></a>Meer voorbeeldcode

Enkele voorbeelden voor de Computer Vision SDK voor Python zijn beschikbaar in de GitHub-opslagplaats van de SDK. Deze voorbeelden bieden voorbeeldcode voor aanvullende scenario's die vaak voorkomen bij het werken met Computer Vision:

* [recognize_text][recognize-text]

### <a name="additional-documentation"></a>Aanvullende documentatie

Bekijk de [Azure Computer Vision-documentatie][computervision_docs] op docs.microsoft.com voor uitgebreidere documentatie over de Computer Vision-service.

<!-- LINKS -->
[pip]: https://pypi.org/project/pip/
[python]: https://www.python.org/downloads/

[azure_cli]: https://docs.microsoft.com/cli/azure
[azure_pattern_circuit_breaker]: https://docs.microsoft.com/azure/architecture/patterns/circuit-breaker
[azure_pattern_retry]: https://docs.microsoft.com/azure/architecture/patterns/retry
[azure_portal]: https://portal.azure.com
[azure_sub]: https://azure.microsoft.com/free/

[cloud_shell]: https://docs.microsoft.com/azure/cloud-shell/overview

[venv]: https://docs.python.org/3/library/venv.html
[virtualenv]: https://virtualenv.pypa.io

[source_code]: https://github.com/Azure/azure-sdk-for-python/tree/master/azure-cognitiveservices-vision-computervision

[pypi_computervision]:https://pypi.org/project/azure-cognitiveservices-vision-computervision/
[pypi_pillow]:https://pypi.org/project/Pillow/

[ref_computervision_sdk]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision?view=azure-python
[ref_computervision_computervisionerrorexception]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.computervisionerrorexception?view=azure-python
[ref_httpfailure]: https://docs.microsoft.com/python/api/msrest/msrest.exceptions.httpoperationerror?view=azure-python


[computervision_resource]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/vision-api-how-to-topics/howtosubscribe

[computervision_docs]: https://docs.microsoft.com/azure/cognitive-services/computer-vision/home

[ref_computervisionclient]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python


[ref_computervisionclient_analyze_image]: https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#analyze-image-url--visual-features-none--details-none--language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_list_models]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#list-models-custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_analyze_image_by_domain]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#analyze-image-by-domain-model--url--language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_describe_image]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#describe-image-url--max-candidates--1---language--en---custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_recognize_text]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#recognize-text-url--mode--custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_get_text_operation_result]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#get-text-operation-result-operation-id--custom-headers-none--raw-false----operation-config-
[ref_computervisionclient_generate_thumbnail]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.computervisionapi?view=azure-python#generate-thumbnail-width--height--url--smart-cropping-false--custom-headers-none--raw-false--callback-none----operation-config-


[ref_computervision_model_visualfeatures]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.visualfeaturetypes?view=azure-python

[ref_computervision_model_textoperationstatuscodes]:https://docs.microsoft.com/python/api/azure-cognitiveservices-vision-computervision/azure.cognitiveservices.vision.computervision.models.textoperationstatuscodes?view=azure-python

[computervision_request_units]:https://azure.microsoft.com/pricing/details/cognitive-services/computer-vision/

[recognize-text]:https://github.com/Azure-Samples/cognitive-services-python-sdk-samples/blob/master/samples/vision/computer_vision_samples.py

