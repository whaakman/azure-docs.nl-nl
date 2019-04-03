---
title: 'Quickstart: Lijst met tekst naar spraak stemmen, Python - spraakservices'
titleSuffix: Azure Cognitive Services
description: In deze snelstartgids leert u hoe u de volledige lijst met standaard- en neurale stemmen voor een regio/het eindpunt met behulp van Python. De lijst wordt geretourneerd als JSON en stem beschikbaarheid kan variëren per regio.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 03/22/2019
ms.author: erhopf
ms.openlocfilehash: 66bda68b1313a7c172e273671bc3a03503d08e0d
ms.sourcegitcommit: a60a55278f645f5d6cda95bcf9895441ade04629
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/03/2019
ms.locfileid: "58876577"
---
# <a name="quickstart-get-the-list-of-text-to-speech-voices-using-python"></a>Quickstart: De lijst met tekst naar spraak met behulp van Python-stemmen

In deze snelstartgids leert u hoe u de volledige lijst met standaard- en neurale stemmen voor een regio/het eindpunt met behulp van Python. De lijst wordt geretourneerd als JSON en stem beschikbaarheid kan variëren per regio. Zie voor een lijst met ondersteunde regio's, [regio's](regions.md).

Deze snelstartgids moet een [Azure Cognitive Services-account](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) met een resource Speech Services. Als u geen account hebt, kunt u de [gratis proefversie](get-started.md) gebruiken om een abonnementssleutel op te halen.

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* Python 2.7.x of 3.x
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download) of uw favoriete teksteditor
* De sleutel van een Azure-abonnement voor de spraakservices

## <a name="create-a-project-and-import-required-modules"></a>Een project maken en de vereiste modules importeren

Maak een nieuw Python-project met uw favoriete IDE of editor. Kopieer dit codefragment naar uw project in een bestand met de naam `get-voices.py`.

```python
import requests
```

> [!NOTE]
> Als u deze modules nog niet hebt gebruikt, moet u ze installeren voordat u het programma uitvoert. Voer voor het installeren van deze pakketten voert u `pip install requests` uit.

Aanvragen voor HTTP-aanvragen voor de Text to Speech-service wordt gebruikt.

## <a name="set-the-subscription-key-and-create-a-prompt-for-tts"></a>Stel de abonnementssleutel en een prompt voor TTS maken

In de volgende gedeelten maakt u methoden voor verwerking van autorisatie, de Text to Speech-API aanroepen en valideren van het antwoord. Laten we beginnen met het maken van een klasse. Dit is waar gaan we onze methoden voor token exchange en het aanroepen van de Text to Speech-API.

```python
class GetVoices(object):
    def __init__(self, subscription_key):
        self.subscription_key = subscription_key
        self.access_token = None
```

De `subscription_key` is de unieke sleutel vanuit de Azure-portal.

## <a name="get-an-access-token"></a>Een toegangstoken opvragen

Dit eindpunt moet een toegangstoken voor verificatie. Als u een toegangstoken, is een exchange vereist. In dit voorbeeld uitwisselt uw abonnementssleutel Speech Services voor een access token met de `issueToken` eindpunt.

In dit voorbeeld wordt ervan uitgegaan dat uw abonnement spraakservices in de regio VS-West. Als u een andere regio, werk de waarde voor `fetch_token_url`. Zie voor een volledige lijst [regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#rest-apis).

Kopieer deze code in de `GetVoices` klasse:

```python
def get_token(self):
    fetch_token_url = "https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken"
    headers = {
        'Ocp-Apim-Subscription-Key': self.subscription_key
    }
    response = requests.post(fetch_token_url, headers=headers)
    self.access_token = str(response.text)
```

> [!NOTE]
> Zie voor meer informatie over verificatie [verifiëren met een toegangstoken](https://docs.microsoft.com/azure/cognitive-services/authentication#authenticate-with-an-authentication-token).

## <a name="make-a-request-and-save-the-response"></a>Een aanvraag indienen en het antwoord opslaan

Hier gaat u naar de aanvraag voor het samenstellen en opslaan van de lijst met geretourneerde stemmen. Eerst moet u om in te stellen de `base_url` en `path`. In dit voorbeeld wordt ervan uitgegaan dat u het eindpunt van de VS-West. Als de bron naar een andere regio is geregistreerd, moet u bijwerken de `base_url`. Zie voor meer informatie, [Speech Services-regio's](https://docs.microsoft.com/azure/cognitive-services/speech-service/regions#text-to-speech).

Voeg vervolgens de vereiste headers voor de aanvraag. Ten slotte maakt u een aanvraag naar de service. Als de aanvraag geslaagd is, en een 200-statuscode is geretourneerd, wordt het antwoord wordt geschreven naar het bestand.

Kopieer deze code in de `GetVoices` klasse:

```python
def get_voices(self):
    base_url = 'https://westus.tts.speech.microsoft.com'
    path = '/cognitiveservices/voices/list'
    get_voices_url = base_url + path
    headers = {
        'Authorization': 'Bearer ' + self.access_token
    }
    response = requests.get(get_voices_url, headers=headers)
    if response.status_code == 200:
        with open('voices.json', 'wb') as voices:
            voices.write(response.content)
            print("\nStatus code: " + str(response.status_code) + "\nvoices.json is ready to view.\n")
    else:
        print("\nStatus code: " + str(
            response.status_code) + "\nSomething went wrong. Check your subscription key and headers.\n")
```

## <a name="put-it-all-together"></a>Alles samenvoegen

U bent bijna klaar. De laatste stap is het instantiëren van de klasse en aanroepen van uw functies.

```python
if __name__ == "__main__":
    subscription_key = "YOUR_KEY_HERE"
    app = GetVoices(subscription_key)
    app.get_token()
    app.get_voices()
```

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Dat is alles, u kunt het voorbeeld uitvoert. Ga vanaf de opdrachtregel (of terminalsessie) naar de projectmap en voer het volgende uit:

```console
python get-voices.py
```

## <a name="clean-up-resources"></a>Resources opschonen

Denk eraan dat u eventuele vertrouwelijke informatie, zoals abonnementssleutels, verwijdert uit de broncode van de voorbeeld-app.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Python-voorbeelden op GitHub verkennen](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/Samples-Http/Python)

## <a name="see-also"></a>Zie ook

* [Naslaginformatie voor de Text to Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-apis)
* [Het maken van aangepaste spraakstijlen](how-to-customize-voice-font.md)
* [Record stem voorbeelden voor het maken van een aangepaste spraak](record-custom-voice-samples.md)
