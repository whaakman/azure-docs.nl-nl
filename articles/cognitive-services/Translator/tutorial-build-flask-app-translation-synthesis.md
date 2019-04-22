---
title: 'Zelfstudie: Een Flask-app om te vertalen, bootsen en analyseren van tekst - Translator Text-API bouwen'
titleSuffix: Azure Cognitive Services
description: In deze zelfstudie bouwt u een Flask gebaseerde web-app die gebruikmaakt van Azure Cognitive Services voor tekst vertalen, sentiment analyseren en vertaalde tekst in spraak bootsen. Onze focus ligt op de Python-code en de Flask-routes waarmee de toepassing. We wordt niet veel tijd besteden aan het Javascript dat Hiermee bepaalt u de app, maar de bestanden voor u om te controleren.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: tutorial
ms.date: 04/02/2019
ms.author: erhopf
ms.openlocfilehash: 69e6797e91fc645e3bd3e3b300cea6852a662214
ms.sourcegitcommit: c174d408a5522b58160e17a87d2b6ef4482a6694
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 04/18/2019
ms.locfileid: "59007386"
---
# <a name="tutorial-build-a-flask-app-with-azure-cognitive-services"></a>Zelfstudie: Een Flask-app ontwikkelen met Azure Cognitive Services

In deze zelfstudie bouwt u een Flask web-app die gebruikmaakt van Azure Cognitive Services tekst vertalen, sentiment analyseren en vertaalde tekst in spraak bootsen. Onze focus ligt op de Python-code en de Flask-routes waarmee de toepassing, maar we zullen kunt gebruiken om te met HTML en Javascript dat de app verzamelt. Als u problemen ondervindt, laat het ons uitvoert weten met behulp van de feedbackknop hieronder.

Dit is wat deze zelfstudie wordt uitgelegd:

> [!div class="checklist"]
> * Azure-abonnementssleutels ophalen
> * Uw ontwikkelomgeving instellen en installeer afhankelijkheden
> * Maak een Flask-app
> * De Translator Text-API gebruiken om tekst te vertalen
> * Tekstanalyse gebruiken voor het analyseren van positieve/negatieve stemming van invoertekst en vertalingen
> * Speech Services gebruiken voor het converteren van vertaalde tekst in kunstmatige spraak
> * De Flask-app lokaal uitvoeren

> [!TIP]
> Als u wilt overslaan en alle de code in één keer het volledige voorbeeld, samen met build instructies zijn beschikbaar op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Flask-App-Tutorial).

## <a name="what-is-flask"></a>Wat is Flask?

Flask is een microframework voor het maken van webtoepassingen. Dit betekent dat Flask biedt u hulpprogramma's, bibliotheken en -technologieën waarmee u een webtoepassing kunt maken. Deze web-App kan worden sommige webpagina's, een blog, een wiki of go als essentiële als een Agendatoepassing webgebaseerde of een commerciële website.

Hier zijn enkele nuttige koppelingen voor degenen die gedetailleerde informatie over na deze zelfstudie wilt:

* [Flask-documentatie](http://flask.pocoo.org/)
* [Flask voor Dummies - een handleiding voor beginners naar Flask](https://codeburst.io/flask-for-dummies-a-beginners-guide-to-flask-part-uno-53aec6afc5b1)

## <a name="prerequisites"></a>Vereisten

Laten we bekijken de software en abonnement sleutels die u nodig hebt voor deze zelfstudie.

* [Python 3.5.2 gebruikt of hoger](https://www.python.org/downloads/)
* [Hulpprogramma's voor GIT](https://git-scm.com/downloads)
* Een IDE- of teksteditor, zoals [Visual Studio Code](https://code.visualstudio.com/) of [Atom](https://atom.io/)  
* [Chrome](https://www.google.com/chrome/browser/) of [Firefox](https://www.mozilla.org/firefox)
* Een **Translator tekst** abonnementssleutel (Houd er rekening mee dat u niet verplicht om een regio te selecteren.)
* Een **Tekstanalyse** abonnementssleutel in de **VS-West** regio.
* Een **spraakservices** abonnementssleutel in de **VS-West** regio.

## <a name="create-an-account-and-subscribe-to-resources"></a>Een account maken en zich abonneren op resources

Zoals eerder vermeld gaat u drie abonnementssleutels nodig voor deze zelfstudie. Dit betekent dat u wilt maken van een bron in uw Azure-account voor:
* Translator Text
* Tekstanalyse
* Spraakservices

Gebruik [een Cognitive Services-Account maken in Azure portal](https://docs.microsoft.com/azure/cognitive-services/cognitive-services-apis-create-account) voor stapsgewijze instructies om resources te maken.

> [!IMPORTANT]
> Voor deze zelfstudie maakt u uw resources in de regio VS-West. Als u een andere regio, moet u om aan te passen van de basis-URL in elk van uw Python-bestanden.

## <a name="set-up-your-dev-environment"></a>Uw ontwikkelaarsomgeving instellen

Voordat u uw Flask web-app bouwt, moet u een werkmap voor uw project maken en een paar Python-pakketten te installeren.

### <a name="create-a-working-directory"></a>Maken van een werkmap

1. Open vanaf de opdrachtregel (Windows) of terminal (Mac OS of Linux). Vervolgens maakt u een werkmap en sub-mappen voor uw project:  

   ```
   mkdir -p flask-cog-services/static/scripts && mkdir flask-cog-services/templates
   ```
2. Schakel naar de werkmap van uw project:  

   ```
   cd flask-cog-services
   ```

### <a name="create-and-activate-your-virtual-environment-with-virtualenv"></a>Maken en activeren van de virtuele omgeving met `virtualenv`

We gaan maken van een virtuele omgeving voor onze Flask-app via `virtualenv`. Met behulp van een virtuele omgeving zorgt ervoor dat u hebt een schone omgeving om te werken.

1. In uw werkmap en voer deze opdracht uit om een virtuele omgeving te maken: **macOS-/ Linux:**
   ```
   virtualenv venv --python=python3
   ```
   We hebben expliciet aangegeven dat de virtuele omgeving moet worden gebruikt voor het gebruik van Python 3. Dit zorgt ervoor dat gebruikers met meerdere installaties van Python van de juiste versie gebruikmaakt.

   **Windows CMD / Bash-Windows:**
   ```
   virtualenv venv
   ```
   Dingen om eenvoudig te houden, zijn we naamgeving van uw venv virtuele omgeving.

2. De opdrachten voor het activeren van de virtuele omgeving, hangt af van uw platform/shell:   

   | Platform | Shell | Opdracht |
   |----------|-------|---------|
   | macOS/Linux | Bash/zsh | `source venv/bin/activate` |
   | Windows | Bash | `source venv/Scripts/activate` |
   | | Opdrachtregel | `venv\Scripts\activate.bat` |
   | | PowerShell | `venv\Scripts\Activate.ps1` |

   Na het uitvoeren van deze opdracht, de opdrachtregel of terminal-sessie moet worden voorafgegaan door `venv`.

3. U kunt de sessie op elk gewenst moment uitschakelen door deze te typen in de opdrachtregel of terminal: `deactivate`.

> [!NOTE]
> Python heeft uitgebreide documentatie voor het maken en beheren van virtuele omgevingen, Zie [virtualenv](https://virtualenv.pypa.io/en/latest/).

### <a name="install-requests"></a>Installeren van aanvragen

Aanvragen is een populaire module die wordt gebruikt om HTTP 1.1-aanvragen te verzenden. Er is niet nodig queryreeksen handmatig toevoegen aan uw URL's of uw postgegevens formulier coderen.

1. Voer het volgende voor het installeren van aanvragen:

   ```
   pip install requests
   ```

> [!NOTE]
> Als u wilt meer informatie over aanvragen, Zie [aanvragen: HTTP gebruikt om een mens](http://docs.python-requests.org/en/master/).

### <a name="install-and-configure-flask"></a>Installeren en configureren van Flask

Vervolgens moeten we Flask installeren. Flask verwerkt de routering voor onze web-app, en kan we voor server-naar-server-aanroepen die onze abonnementssleutels van de eindgebruiker te verbergen.

1. Voer het volgende voor het installeren van Flask:
   ```
   pip install Flask
   ```
   Zorg ervoor dat flask is geïnstalleerd. Uitvoeren:
   ```
   flask --version
   ```
   De versie moet worden afgedrukt op de terminal. Iets anders betekent dat er iets is misgegaan.

2. Als u wilt de Flask-app uitvoeren, kunt u ofwel de flask-opdracht of het Python -m-switch met Flask gebruiken. Voordat u kunt dit doen moet u uw terminal zien welke app werken met door het exporteren van de `FLASK_APP` omgevingsvariabele:

   **macOS/Linux**:
   ```
   export FLASK_APP=app.py
   ```

   **Windows**:
   ```
   set FLASK_APP=app.py
   ```

## <a name="create-your-flask-app"></a>Flask-app maken

In deze sectie gaat u een barebones Flask-app die wordt geretourneerd van een HTML-bestand wanneer gebruikers de hoofdmap van uw app bereikt maken. Te veel tijd wilt kiezen uit elkaar de code niet uitgeven, komen we terug naar dit bestand later bijwerken.

### <a name="what-is-a-flask-route"></a>Wat is een Flask-route?

We staan nu even om te praten over '[routes](http://flask.pocoo.org/docs/1.0/api/#flask.Flask.route)'. Routering wordt gebruikt om een URL binden aan een specifieke functie. Route decorators flask gebruikt voor het registreren van functies tot specifieke URL's. Bijvoorbeeld, wanneer een gebruiker navigeert naar de hoofdmap (`/`) van onze web-app `index.html` wordt weergegeven.  

```python
@app.route('/')
def index():
    return render_template('index.html')
```

Laten we eens een meer voorbeeld hamer deze start.

```python
@app.route('/about')
def about():
    return render_template('about.html')
```

Deze code zorgt ervoor dat wanneer een gebruiker naar navigeert `http://your-web-app.com/about` die de `about.html` bestand wordt gerenderd.

Terwijl deze voorbeelden laten hoe u weergegeven html-pagina's voor een gebruiker zien, kunnen ook routes worden gebruikt voor het aanroepen van API's wanneer een knop wordt ingedrukt of een willekeurig aantal acties uitvoeren zonder te navigeren vanuit de startpagina. U ziet dit in actie bij het maken van routes voor vertaling, sentiment en spraaksynthese.

### <a name="get-started"></a>Aan de slag

1. Open het project in uw IDE en maak vervolgens een bestand met de naam `app.py` in de hoofdmap van uw werkmap. Vervolgens kopieert u deze code in `app.py` en op te slaan:

   ```python
   from flask import Flask, render_template, url_for, jsonify, request

   app = Flask(__name__)
   app.config['JSON_AS_ASCII'] = False

   @app.route('/')
   def index():
       return render_template('index.html')
   ```

   Dit codeblok geeft aan dat de app om weer te geven `index.html` wanneer een gebruiker navigeert naar de hoofdmap van uw web-app (`/`).

2. Vervolgens maken we de front-end voor onze web-app. Maak een bestand met de naam `index.html` in de `templates` directory. Kopieer deze code in `templates/index.html`.

   ```html
   <!doctype html>
   <html lang="en">
     <head>
       <!-- Required metadata tags -->
       <meta charset="utf-8">
       <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
       <meta name="description" content="Translate and analyze text with Azure Cognitive Services.">
       <!-- Bootstrap CSS -->
       <link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/css/bootstrap.min.css" integrity="sha384-Gn5384xqQ1aoWXA+058RXPxPg6fy4IWvTNh0E263XmFcJlSAwiGgFAW/dAiS6JXm" crossorigin="anonymous">
       <title>Translate and analyze text with Azure Cognitive Services</title>
     </head>
     <body>
       <div class="container">
         <h1>Translate, synthesize, and analyze text with Azure</h1>
         <p>This simple web app uses Azure for text translation, text-to-speech conversion, and sentiment analysis of input text and translations. Learn more about <a href="https://docs.microsoft.com/azure/cognitive-services/">Azure Cognitive Services</a>.
        </p>
        <!-- HTML provided in the following sections goes here. -->

        <!-- End -->
       </div>

       <!-- Required Javascript for this tutorial -->
       <script src="https://code.jquery.com/jquery-3.2.1.slim.min.js" integrity="sha384-KJ3o2DKtIkvYIK3UENzmM7KCkRr/rE9/Qpg6aAZGJwFDMVNA/GpGFF93hXpG5KkN" crossorigin="anonymous"></script>
       <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.3.1/jquery.min.js"></script>
       <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.12.9/umd/popper.min.js" integrity="sha384-ApNbgh9B+Y1QKtv3Rn7W3mgPxhU9K/ScQsAP7hUibX39j7fakFPskvXusvfa0b4Q" crossorigin="anonymous"></script>
       <script src="https://maxcdn.bootstrapcdn.com/bootstrap/4.0.0/js/bootstrap.min.js" integrity="sha384-JZR6Spejh4U02d8jOt6vLEHfe/JQGiRRSQQxSfFWpi1MquVdAyjUar5+76PVCmYl" crossorigin="anonymous"></script>
       <script type = "text/javascript" src ="static/scripts/main.js"></script>
     </body>
   </html>
   ```

3. Laten we de Flask-app testen. Voer vanaf de terminal:

   ```
   flask run
   ```

4. Open een browser en navigeer naar de URL die is opgegeven. Hier ziet u uw app met één pagina. Druk op **Ctrl + c** afsluiten van de app.

## <a name="translate-text"></a>Tekst vertalen

Nu dat u een beter beeld hebt van hoe een eenvoudige Flask-app we werkt:

* Sommige Python voor het aanroepen van de Translator Text-API en een antwoord schrijven
* Maak een Flask-route voor het aanroepen van de Python-code
* De HTML-code bijwerken met een gebied voor tekstinvoer en vertaling, een taalkeuze en knop vertalen
* Javascript waarmee gebruikers kunnen communiceren met de Flask-app uit de HTML-code schrijven

### <a name="call-the-translator-text-api"></a>De Translator Text-API aanroepen

Het eerste wat dat u moet doen is een functie voor het aanroepen van de API van Translator tekst schrijven. Deze functie neemt twee argumenten: `text_input` en `language_output`. Deze functie wordt aangeroepen wanneer een gebruiker op de knop vertalen in uw app. Het tekstgebied in de HTML-code wordt verzonden als de `text_input`, en de waarde van de taal selecteren in de HTML-code is verzonden als `language_output`.

1. Begin met het maken van een bestand met de naam `translate.py` in de hoofdmap van uw werkmap.
2. Vervolgens voegt u deze code toe `translate.py`. Deze functie neemt twee argumenten: `text_input` en `language_output`.
   ```python
   import os, requests, uuid, json

   # Don't forget to replace with your Cog Services subscription key!
   # If you prefer to use environment variables, see Extra Credit for more info.
   subscription_key = 'YOUR_TRANSLATOR_TEXT_SUBSCRIPTION_KEY'

   # Our Flask route will supply two arguments: text_input and language_output.
   # When the translate text button is pressed in our Flask app, the Ajax request
   # will grab these values from our web app, and use them in the request.
   # See main.js for Ajax calls.
   def get_translation(text_input, language_output):
       base_url = 'https://api.cognitive.microsofttranslator.com'
       path = '/translate?api-version=3.0'
       params = '&to=' + language_output
       constructed_url = base_url + path + params

       headers = {
           'Ocp-Apim-Subscription-Key': subscription_key,
           'Content-type': 'application/json',
           'X-ClientTraceId': str(uuid.uuid4())
       }

       # You can pass more than one object in body.
       body = [{
           'text' : text_input
       }]
       response = requests.post(constructed_url, headers=headers, json=body)
       return response.json()
   ```
3. Uw abonnementssleutel Translator tekst toevoegen en opslaan.

### <a name="add-a-route-to-apppy"></a>Een route naar toevoegen `app.py`

Vervolgens moet u een route maken in uw Flask-app die worden aangeroepen `translate.py`. Deze route wordt telkens wanneer een gebruiker op de knop vertalen in uw app worden aangeroepen.

Voor deze app uw route wilt accepteren `POST` aanvragen. Dit komt doordat de functie wordt verwacht dat de tekst te vertalen en een uitvoer-taal voor de vertaling.

Flask biedt ondersteunende functies om te parseren en beheren van elke aanvraag. In de code die is opgegeven, `get_json()` geeft als resultaat de gegevens uit de `POST` aanvraag als JSON. Klik vervolgens met behulp van `data['text']` en `data['to']`, de waarden voor de taal van tekst en uitvoer worden doorgegeven aan `get_translation()` functie beschikbaar is via `translate.py`. De laatste stap is het antwoord geretourneerd als JSON, omdat, moet u deze gegevens weergeven in uw web-app.

In de volgende secties zult u dit proces herhalen bij het maken van routes voor de analyse- en spraakherkenning synthese sentiment.

1. Open `app.py` en zoek de importinstructie boven aan `app.py` en voeg de volgende regel toe:

   ```python
   import translate
   ```
   Nu onze Flask-app de methode die beschikbaar zijn via gebruiken kunt `translate.py`.

2. Deze code kopiëren naar het einde van `app.py` en op te slaan:

   ```python
   @app.route('/translate-text', methods=['POST'])
   def translate_text():
       data = request.get_json()
       text_input = data['text']
       translation_output = data['to']
       response = translate.get_translation(text_input, translation_output)
       return jsonify(response)
   ```

### <a name="update-indexhtml"></a>Update `index.html`

Nu dat u een functie hebt voor het vertalen van tekst en een route in de Flask-app om aan te roepen, wordt de volgende stap is om te beginnen met het samenstellen van de HTML-code voor uw app. De HTML-code hieronder biedt een aantal dingen:

* Biedt een tekstgebied waar gebruikers te vertalen tekst kunnen invoeren.
* Bevat een taalkeuze.
* HTML-elementen voor het renderen van de gedetecteerde taal en het vertrouwen van de scores geretourneerd tijdens de conversie van bevat.
* Biedt een alleen-lezen tekstgebied waar de NAT-uitvoer wordt weergegeven.
* Tijdelijke aanduidingen voor sentiment-analyse- en spraakherkenning synthese code die u aan dit bestand later in de zelfstudie toevoegen zult bevat.

We werken `index.html`.

1. Open `index.html` en zoekt u deze codeopmerkingen:
   ```html
   <!-- HTML provided in the following sections goes here. -->

   <!-- End -->
   ```

2. De codeopmerkingen vervangen door dit blok HTML:
   ```html
   <div class="row">
     <div class="col">
       <form>
         <!-- Enter text to translate. -->
         <div class="form-group">
           <label for="text-to-translate"><strong>Enter the text you'd like to translate:</strong></label>
           <textarea class="form-control" id="text-to-translate" rows="5"></textarea>
         </div>
         <!-- Select output language. -->
         <div class="form-group">
           <label for="select-language"><strong>Translate to:</strong></label>
           <select class="form-control" id="select-language">
             <option value="ar">Arabic</option>
             <option value="ca">Catalan</option>
             <option value="zh-Hans">Chinese (Simplified)</option>
             <option value="zh-Hant">Chinese (Traditional)</option>
             <option value="hr">Croatian</option>
             <option value="en">English</option>
             <option value="fr">French</option>
             <option value="de">German</option>
             <option value="el">Greek</option>
             <option value="he">Hebrew</option>
             <option value="hi">Hindi</option>
             <option value="it">Italian</option>
             <option value="ja">Japanese</option>
             <option value="ko">Korean</option>
             <option value="pt">Portuguese</option>
             <option value="ru">Russian</option>
             <option value="es">Spanish</option>
             <option value="th">Thai</option>
             <option value="tr">Turkish</option>
             <option value="vi">Vietnamese</option>
           </select>
         </div>
         <button type="submit" class="btn btn-primary mb-2" id="translate">Translate text</button></br>
         <div id="detected-language" style="display: none">
           <strong>Detected language:</strong> <span id="detected-language-result"></span><br />
           <strong>Detection confidence:</strong> <span id="confidence"></span><br /><br />
         </div>

         <!-- Start sentiment code-->

         <!-- End sentiment code -->

       </form>
     </div>
     <div class="col">
       <!-- Translated text returned by the Translate API is rendered here. -->
       <form>
         <div class="form-group" id="translator-text-response">
           <label for="translation-result"><strong>Translated text:</strong></label>
           <textarea readonly class="form-control" id="translation-result" rows="5"></textarea>
         </div>

         <!-- Start voice font selection code -->

         <!-- End voice font selection code -->

       </form>

       <!-- Add Speech Synthesis button and audio element -->

       <!-- End Speech Synthesis button -->

     </div>
   </div>
   ```

De volgende stap is het schrijven van wat Javascript. Dit is de brug tussen uw HTML-code en de Flask-route.

### <a name="create-mainjs"></a>Maken `main.js`  

De `main.js` bestand is de brug tussen uw HTML-code en de Flask-route. Uw app gebruikt een combinatie van jQuery, Ajax en XMLHttpRequest weergave van de inhoud en ervoor `POST` aanvragen naar de Flask-routes.

Inhoud van de HTML-code wordt in de onderstaande code gebruikt om een aanvraag om de Flask-route te maken. Met name de inhoud van het tekstgebied en de taalkeuze zijn toegewezen aan variabelen en vervolgens doorgegeven in de aanvraag voor `translate-text`.

De code doorloopt het antwoord, en de HTML-code wordt bijgewerkt met de vertaling, de gedetecteerde taal en de betrouwbaarheidsscore.

1. Maak een bestand genaamd vanuit uw IDE `main.js` in de `static/scripts` directory.
2. Kopieer deze code in `static/scripts/main.js`:
   ```javascript
   //Initiate jQuery on load.
   $(function() {
     //Translate text with flask route
     $("#translate").on("click", function(e) {
       e.preventDefault();
       var translateVal = document.getElementById("text-to-translate").value;
       var languageVal = document.getElementById("select-language").value;
       var translateRequest = { 'text': translateVal, 'to': languageVal }

       if (translateVal !== "") {
         $.ajax({
           url: '/translate-text',
           method: 'POST',
           headers: {
               'Content-Type':'application/json'
           },
           dataType: 'json',
           data: JSON.stringify(translateRequest),
           success: function(data) {
             for (var i = 0; i < data.length; i++) {
               document.getElementById("translation-result").textContent = data[i].translations[0].text;
               document.getElementById("detected-language-result").textContent = data[i].detectedLanguage.language;
               if (document.getElementById("detected-language-result").textContent !== ""){
                 document.getElementById("detected-language").style.display = "block";
               }
               document.getElementById("confidence").textContent = data[i].detectedLanguage.score;
             }
           }
         });
       };
     });
     // In the following sections, you'll add code for sentiment analysis and
     // speech synthesis here.
   })
   ```

### <a name="test-translation"></a>Test vertaling

Laten we vertaling in de app te testen.

```
flask run
```

Ga naar het opgegeven serveradres. Typ tekst in het invoervak, selecteer dat een taal en druk op vertalen. U moet een vertaling. Als dit niet werkt, zorg ervoor dat u uw abonnementssleutel hebt toegevoegd.

> [!TIP]
> Als de wijzigingen worden niet weergegeven of als de app niet zoals werkt u verwacht dat het te proberen uw cache wissen of een privé-/ incognito-venster te openen.

Druk op **CTRL + c** kill van de app en Ga naar de volgende sectie.

## <a name="analyze-sentiment"></a>Stemming analyseren

De [Tekstanalyse-API](https://docs.microsoft.com/azure/cognitive-services/text-analytics/overview) kan worden gebruikt voor het uitvoeren van sentimentanalyse, sleuteltermen extraheren uit tekst of de brontaal detecteren. In deze app gaan we sentimentanalyse gebruiken om te bepalen of de opgegeven tekst positief, neutraal of negatief is. De API retourneert een numerieke score tussen 0 en 1. Scores dicht bij 1 geven een positieve stemming aan en scores dicht bij 0 geven een negatieve stemming aan.

In deze sectie gaat u een paar dingen doen:

* Sommige Python voor het aanroepen van de Tekstanalyse-API voor het uitvoeren van sentimentanalyse en een antwoord retourneren schrijven
* Maak een Flask-route voor het aanroepen van de Python-code
* De HTML-code bijwerken met een gebied voor sentiment scores, samen met de knop analyse uitvoeren
* Javascript waarmee gebruikers kunnen communiceren met de Flask-app uit de HTML-code schrijven

### <a name="call-the-text-analytics-api"></a>De Tekstanalyse-API aanroepen

Laten we een functie voor het aanroepen van de Tekstanalyse-API schrijven. Deze functie duurt vier argumenten: `input_text`, `input_language`, `output_text`, en `output_language`. Deze functie wordt aangeroepen wanneer een gebruiker op de knop uitvoeren sentiment-analyse in uw app. Gegevens die zijn opgegeven door de gebruiker uit de lijst met tekst oppervlakte en de taal, evenals de gedetecteerde taal en NAT-uitvoer worden geleverd bij elke aanvraag. Het antwoordobject bevat sentiment scores voor de bron en de vertaling. In de volgende secties vindt gaat u schrijven wat Javascript voor het parseren van het antwoord en worden gebruikt in uw app. Voorlopig laten we richt u zich bij aanroepen van de Tekstanalyse-API.

1. We gaan maken van een bestand met de naam `sentiment.py` in de hoofdmap van uw werkmap.
2. Vervolgens voegt u deze code toe `sentiment.py`.
   ```python
   import os, requests, uuid, json

   # Don't forget to replace with your Cog Services subscription key!
   subscription_key = 'YOUR_TEXT_ANALYTICS_SUBSCRIPTION_KEY'

   # Our Flask route will supply four arguments: input_text, input_language,
   # output_text, output_language.
   # When the run sentiment analysis button is pressed in our Flask app,
   # the Ajax request will grab these values from our web app, and use them
   # in the request. See main.js for Ajax calls.

   def get_sentiment(input_text, input_language, output_text, output_language):
       base_url = 'https://westus.api.cognitive.microsoft.com/text/analytics'
       path = '/v2.0/sentiment'
       constructed_url = base_url + path

       headers = {
           'Ocp-Apim-Subscription-Key': subscription_key,
           'Content-type': 'application/json',
           'X-ClientTraceId': str(uuid.uuid4())
       }

       # You can pass more than one object in body.
       body = {
           'documents': [
               {
                   'language': input_language,
                   'id': '1',
                   'text': input_text
               },
               {
                   'language': output_language,
                   'id': '2',
                   'text': output_text
               }
           ]
       }
       response = requests.post(constructed_url, headers=headers, json=body)
       return response.json()
   ```
3. Uw abonnementssleutel Tekstanalyse toevoegen en opslaan.

### <a name="add-a-route-to-apppy"></a>Een route naar toevoegen `app.py`

Laten we een route maken in uw Flask-app die worden aangeroepen `sentiment.py`. Deze route wordt telkens wanneer een gebruiker op de knop uitvoeren sentiment-analyse in uw app worden aangeroepen. Als de route voor vertaling, gaat deze route accepteren `POST` aanvragen omdat de argumenten verwacht.

1. Open `app.py` en zoek de importinstructie boven aan `app.py` en werk dit bij:

   ```python
   import translate, sentiment
   ```
   Nu onze Flask-app de methode die beschikbaar zijn via gebruiken kunt `sentiment.py`.

2. Deze code kopiëren naar het einde van `app.py` en op te slaan:
   ```python
   @app.route('/sentiment-analysis', methods=['POST'])
   def sentiment_analysis():
       data = request.get_json()
       input_text = data['inputText']
       input_lang = data['inputLanguage']
       output_text = data['outputText']
       output_lang =  data['outputLanguage']
       response = sentiment.get_sentiment(input_text, input_lang, output_text, output_lang)
       return jsonify(response)
   ```

### <a name="update-indexhtml"></a>Update `index.html`

Nu dat u een functie hebt voor het uitvoeren van sentimentanalyse en een route in uw Flask-app om aan te roepen, wordt de volgende stap is om te beginnen met het schrijven van de HTML-code voor uw app. De HTML-code hieronder biedt een aantal dingen:

* Hiermee voegt u een knop toe aan uw app sentimentanalyse kunt uitvoeren
* Voegt een element waarin wordt uitgelegd sentiment scoren
* Een element als u wilt weergeven van de scores gevoel wordt toegevoegd

1. Open `index.html` en zoekt u deze codeopmerkingen:
   ```html
   <!-- Start sentiment code-->

   <!-- End sentiment code -->
   ```

2. De codeopmerkingen vervangen door dit blok HTML:
   ```html
   <button type="submit" class="btn btn-primary mb-2" id="sentiment-analysis">Run sentiment analysis</button></br>
   <div id="sentiment" style="display: none">
      <p>Sentiment scores are provided on a 1 point scale. The closer the sentiment score is to 1, indicates positive sentiment. The closer it is to 0, indicates negative sentiment.</p>
      <strong>Sentiment score for input:</strong> <span id="input-sentiment"></span><br />
      <strong>Sentiment score for translation:</strong> <span id="translation-sentiment"></span>
   </div>
   ```

### <a name="update-mainjs"></a>Update `main.js`

Inhoud van de HTML-code wordt in de onderstaande code gebruikt om een aanvraag om de Flask-route te maken. Met name de inhoud van het tekstgebied en de taalkeuze zijn toegewezen aan variabelen en vervolgens doorgegeven in de aanvraag voor de `sentiment-analysis` route.

De code doorloopt het antwoord, en de HTML-code wordt bijgewerkt met de scores sentiment.

1. Maak een bestand genaamd vanuit uw IDE `main.js` in de `static` directory.

2. Kopieer deze code in `static/scripts/main.js`:
   ```javascript
   //Run sentinment analysis on input and translation.
   $("#sentiment-analysis").on("click", function(e) {
     e.preventDefault();
     var inputText = document.getElementById("text-to-translate").value;
     var inputLanguage = document.getElementById("detected-language-result").innerHTML;
     var outputText = document.getElementById("translation-result").value;
     var outputLanguage = document.getElementById("select-language").value;

     var sentimentRequest = { "inputText": inputText, "inputLanguage": inputLanguage, "outputText": outputText,  "outputLanguage": outputLanguage };

     if (inputText !== "") {
       $.ajax({
         url: "/sentiment-analysis",
         method: "POST",
         headers: {
             "Content-Type":"application/json"
         },
         dataType: "json",
         data: JSON.stringify(sentimentRequest),
         success: function(data) {
           for (var i = 0; i < data.documents.length; i++) {
             if (typeof data.documents[i] !== "undefined"){
               if (data.documents[i].id === "1") {
                 document.getElementById("input-sentiment").textContent = data.documents[i].score;
               }
               if (data.documents[i].id === "2") {
                 document.getElementById("translation-sentiment").textContent = data.documents[i].score;
               }
             }
           }
           for (var i = 0; i < data.errors.length; i++) {
             if (typeof data.errors[i] !== "undefined"){
               if (data.errors[i].id === "1") {
                 document.getElementById("input-sentiment").textContent = data.errors[i].message;
               }
               if (data.errors[i].id === "2") {
                 document.getElementById("translation-sentiment").textContent = data.errors[i].message;
               }
             }
           }
           if (document.getElementById("input-sentiment").textContent !== '' && document.getElementById("translation-sentiment").textContent !== ""){
             document.getElementById("sentiment").style.display = "block";
           }
         }
       });
     }
   });
   // In the next section, you'll add code for speech synthesis here.
   ```

### <a name="test-sentiment-analysis"></a>Test-sentimentanalyse

Laten we sentimentanalyse in de app te testen.

```
flask run
```

Ga naar het opgegeven serveradres. Typ tekst in het invoervak, selecteer dat een taal en druk op vertalen. U moet een vertaling. Druk vervolgens op de knop uitvoeren sentiment-analyse. U ziet twee scores. Als dit niet werkt, zorg ervoor dat u uw abonnementssleutel hebt toegevoegd.

> [!TIP]
> Als de wijzigingen worden niet weergegeven of als de app niet zoals werkt u verwacht dat het te proberen uw cache wissen of een privé-/ incognito-venster te openen.

Druk op **CTRL + c** kill van de app en Ga naar de volgende sectie.

## <a name="convert-text-to-speech"></a>Tekst naar spraak converteren

De [Text-to-speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech) waarmee uw app naar tekst converteren naar natuurlijke menselijke kunstmatige spraak. De service ondersteunt standaard, neural en aangepaste stemmen. Onze voorbeeldapp gebruikt enkele van de beschikbare stemmen voor een volledige lijst, Zie [ondersteunde talen](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#text-to-speech).

In deze sectie gaat u een paar dingen doen:

* Schrijven van sommige Python als u wilt converteren van tekst naar spraak met de Text to Speech-API
* Maak een Flask-route voor het aanroepen van de Python-code
* De HTML-code bijwerken met een knop om te converteren tekst naar spraak en een-element voor het afspelen van geluid
* Schrijven van Javascript waarmee gebruikers kunnen communiceren met de Flask-app

### <a name="call-the-text-to-speech-api"></a>De Text to Speech-API aanroepen

Laten we een functie als u wilt converteren van tekst naar spraak schrijven. Deze functie neemt twee argumenten: `input_text` en `voice_font`. Deze functie wordt aangeroepen wanneer een gebruiker op de Text to Speech knop converteren in uw app. `input_text` de vertaling uitvoer is geretourneerd door de aanroep te vertalen tekst, `voice_font` is de waarde van de selector voor het lettertype van spraak in de HTML-code.

1. We gaan maken van een bestand met de naam `synthesize.py` in de hoofdmap van uw werkmap.

2. Vervolgens voegt u deze code toe `synthesize.py`.
   ```Python
   import os, requests, time
   from xml.etree import ElementTree

   class TextToSpeech(object):
       def __init__(self, input_text, voice_font):
           subscription_key = 'YOUR_SPEECH_SERVICES_SUBSCRIPTION_KEY'
           self.subscription_key = subscription_key
           self.input_text = input_text
           self.voice_font = voice_font
           self.timestr = time.strftime('%Y%m%d-%H%M')
           self.access_token = None

       # This function performs the token exchange.
       def get_token(self):
           fetch_token_url = 'https://westus.api.cognitive.microsoft.com/sts/v1.0/issueToken'
           headers = {
               'Ocp-Apim-Subscription-Key': self.subscription_key
           }
           response = requests.post(fetch_token_url, headers=headers)
           self.access_token = str(response.text)

       # This function calls the TTS endpoint with the access token.
       def save_audio(self):
           base_url = 'https://westus.tts.speech.microsoft.com/'
           path = 'cognitiveservices/v1'
           constructed_url = base_url + path
           headers = {
               'Authorization': 'Bearer ' + self.access_token,
               'Content-Type': 'application/ssml+xml',
               'X-Microsoft-OutputFormat': 'riff-24khz-16bit-mono-pcm',
               'User-Agent': 'YOUR_RESOURCE_NAME',
           }
           # Build the SSML request with ElementTree
           xml_body = ElementTree.Element('speak', version='1.0')
           xml_body.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-us')
           voice = ElementTree.SubElement(xml_body, 'voice')
           voice.set('{http://www.w3.org/XML/1998/namespace}lang', 'en-US')
           voice.set('name', 'Microsoft Server Speech Text to Speech Voice {}'.format(self.voice_font))
           voice.text = self.input_text
           # The body must be encoded as UTF-8 to handle non-ascii characters.
           body = ElementTree.tostring(xml_body, encoding="utf-8")

           #Send the request
           response = requests.post(constructed_url, headers=headers, data=body)

           # Write the response as a wav file for playback. The file is located
           # in the same directory where this sample is run.
           return response.content
   ```
3. Uw abonnementssleutel Speech Services toevoegen en opslaan.

### <a name="add-a-route-to-apppy"></a>Een route naar toevoegen `app.py`

Laten we een route maken in uw Flask-app die worden aangeroepen `synthesize.py`. Deze route wordt telkens wanneer een gebruiker op de Text to Speech knop converteren in uw app worden aangeroepen. Deze route is, zoals de routes voor vertaling en sentimentanalyse, gaan om te accepteren `POST` aanvragen omdat de functie twee argumenten verwacht: de tekst om na te bootsen en de spraakstijl om te worden afgespeeld.

1. Open `app.py` en zoek de importinstructie boven aan `app.py` en werk dit bij:

   ```python
   import translate, sentiment, synthesize
   ```
   Nu onze Flask-app de methode die beschikbaar zijn via gebruiken kunt `synthesize.py`.

2. Deze code kopiëren naar het einde van `app.py` en op te slaan:

   ```Python
   @app.route('/text-to-speech', methods=['POST'])
   def text_to_speech():
       data = request.get_json()
       text_input = data['text']
       voice_font = data['voice']
       tts = synthesize.TextToSpeech(text_input, voice_font)
       tts.get_token()
       audio_response = tts.save_audio()
       return audio_response
   ```

### <a name="update-indexhtml"></a>Update `index.html`

Nu dat u een functie hebt voor het converteren van tekst naar spraak en een route in de Flask-app om aan te roepen, wordt de volgende stap is om te beginnen met het schrijven van de HTML-code voor uw app. De HTML-code hieronder biedt een aantal dingen:

* Biedt een selectie voice-omlaag
* Er wordt een knop als u wilt converteren van tekst naar spraak
* Voegt een audio-element, dat wordt gebruikt voor het afspelen van de kunstmatige spraak

1. Open `index.html` en zoekt u deze codeopmerkingen:
   ```html
   <!-- Start voice font selection code -->

   <!-- End voice font selection code -->
   ```

2. De codeopmerkingen vervangen door dit blok HTML:
   ```html
   <div class="form-group">
     <label for="select-voice"><strong>Select voice font:</strong></label>
     <select class="form-control" id="select-voice">
       <option value="(ar-SA, Naayf)">Arabic | Male | Naayf</option>
       <option value="(ca-ES, HerenaRUS)">Catalan | Female | HerenaRUS</option>
       <option value="(zh-CN, HuihuiRUS)">Chinese (Mainland) | Female | HuihuiRUS</option>
       <option value="(zh-CN, Kangkang, Apollo)">Chinese (Mainland) | Male | Kangkang, Apollo</option>
       <option value="(zh-HK, Tracy, Apollo)">Chinese (Hong Kong)| Female | Tracy, Apollo</option>
       <option value="(zh-HK, Danny, Apollo)">Chinese (Hong Kong) | Male | Danny, Apollo</option>
       <option value="(zh-TW, Yating, Apollo)">Chinese (Taiwan)| Female | Yaiting, Apollo</option>
       <option value="(zh-TW, Zhiwei, Apollo)">Chinese (Taiwan) | Male | Zhiwei, Apollo</option>
       <option value="(hr-HR, Matej)">Croatian | Male | Matej</option>
       <option value="(en-US, Jessa24kRUS)">English (US) | Female | Jessa24kRUS</option>
       <option value="(en-US, Guy24kRUS)">English (US) | Male | Guy24kRUS</option>
       <option value="(en-IE, Sean)">English (IE) | Male | Sean</option>
       <option value="(fr-FR, Julie, Apollo)">French | Female | Julie, Apollo</option>
       <option value="(fr-FR, HortenseRUS)">French | Female | Julie, HortenseRUS</option>
       <option value="(fr-FR, Paul, Apollo)">French | Male | Paul, Apollo</option>
       <option value="(de-DE, Hedda)">German | Female | Hedda</option>
       <option value="(de-DE, HeddaRUS)">German | Female | HeddaRUS</option>
       <option value="(de-DE, Stefan, Apollo)">German | Male | Apollo</option>
       <option value="(el-GR, Stefanos)">Greek | Male | Stefanos</option>
       <option value="(he-IL, Asaf)">Hebrew (Isreal) | Male | Asaf</option>
       <option value="(hi-IN, Kalpana, Apollo)">Hindi | Female | Kalpana, Apollo</option>
       <option value="(hi-IN, Hemant)">Hindi | Male | Hemant</option>
       <option value="(it-IT, LuciaRUS)">Italian | Female | LuciaRUS</option>
       <option value="(it-IT, Cosimo, Apollo)">Italian | Male | Cosimo, Apollo</option>
       <option value="(ja-JP, Ichiro, Apollo)">Japanese | Male | Ichiro</option>
       <option value="(ja-JP, HarukaRUS)">Japanese | Female | HarukaRUS</option>
       <option value="(ko-KR, HeamiRUS)">Korean | Female | Haemi</option>
       <option value="(pt-BR, HeloisaRUS)">Portuguese (Brazil) | Female | HeloisaRUS</option>
       <option value="(pt-BR, Daniel, Apollo)">Portuguese (Brazil) | Male | Daniel, Apollo</option>
       <option value="(pt-PT, HeliaRUS)">Portuguese (Portugal) | Female | HeliaRUS</option>
       <option value="(ru-RU, Irina, Apollo)">Russian | Female | Irina, Apollo</option>
       <option value="(ru-RU, Pavel, Apollo)">Russian | Male | Pavel, Apollo</option>
       <option value="(ru-RU, EkaterinaRUS)">Russian | Female | EkaterinaRUS</option>
       <option value="(es-ES, Laura, Apollo)">Spanish | Female | Laura, Apollo</option>
       <option value="(es-ES, HelenaRUS)">Spanish | Female | HelenaRUS</option>
       <option value="(es-ES, Pablo, Apollo)">Spanish | Male | Pablo, Apollo</option>
       <option value="(th-TH, Pattara)">Thai | Male | Pattara</option>
       <option value="(tr-TR, SedaRUS)">Turkish | Female | SedaRUS</option>
       <option value="(vi-VN, An)">Vietnamese | Male | An</option>
     </select>
   </div>
   ```

3. Zoek vervolgens naar deze codeopmerkingen:
   ```html
   <!-- Add Speech Synthesis button and audio element -->

   <!-- End Speech Synthesis button -->
   ```

4. De codeopmerkingen vervangen door dit blok HTML:

```html
<button type="submit" class="btn btn-primary mb-2" id="text-to-speech">Convert text-to-speech</button>
<div id="audio-playback">
  <audio id="audio" controls>
    <source id="audio-source" type="audio/mpeg" />
  </audio>
</div>
```

5. Zorg ervoor dat uw werk op te slaan.

### <a name="update-mainjs"></a>Update `main.js`

Inhoud van de HTML-code wordt in de onderstaande code gebruikt om een aanvraag om de Flask-route te maken. Met name de vertaling en de spraakstijl zijn toegewezen aan variabelen en vervolgens doorgegeven in de aanvraag voor de `text-to-speech` route.

De code doorloopt het antwoord, en de HTML-code wordt bijgewerkt met de scores sentiment.

1. Maak een bestand genaamd vanuit uw IDE `main.js` in de `static` directory.
2. Kopieer deze code in `static/scripts/main.js`:
   ```javascript
   // Convert text-to-speech
   $("#text-to-speech").on("click", function(e) {
     e.preventDefault();
     var ttsInput = document.getElementById("translation-result").value;
     var ttsVoice = document.getElementById("select-voice").value;
     var ttsRequest = { 'text': ttsInput, 'voice': ttsVoice }

     var xhr = new XMLHttpRequest();
     xhr.open("post", "/text-to-speech", true);
     xhr.setRequestHeader("Content-Type", "application/json");
     xhr.responseType = "blob";
     xhr.onload = function(evt){
       if (xhr.status === 200) {
         audioBlob = new Blob([xhr.response], {type: "audio/mpeg"});
         audioURL = URL.createObjectURL(audioBlob);
         if (audioURL.length > 5){
           var audio = document.getElementById("audio");
           var source = document.getElementById("audio-source");
           source.src = audioURL;
           audio.load();
           audio.play();
         }else{
           console.log("An error occurred getting and playing the audio.")
         }
       }
     }
     xhr.send(JSON.stringify(ttsRequest));
   });
   // Code for automatic language selection goes here.
   ```
3. U bent bijna klaar. Het laatste wat u gaan doen is een code toevoegen aan `main.js` een spraakstijl op basis van de taal voor vertaling automatisch wordt geselecteerd. Toevoegen van deze codeblok op `main.js`:
   ```javascript
   // Automatic voice font selection based on translation output.
   $('select[id="select-language"]').change(function(e) {
     if ($(this).val() == "ar"){
       document.getElementById("select-voice").value = "(ar-SA, Naayf)";
     }
     if ($(this).val() == "ca"){
       document.getElementById("select-voice").value = "(ca-ES, HerenaRUS)";
     }
     if ($(this).val() == "zh-Hans"){
       document.getElementById("select-voice").value = "(zh-HK, Tracy, Apollo)";
     }
     if ($(this).val() == "zh-Hant"){
       document.getElementById("select-voice").value = "(zh-HK, Tracy, Apollo)";
     }
     if ($(this).val() == "hr"){
       document.getElementById("select-voice").value = "(hr-HR, Matej)";
     }
     if ($(this).val() == "en"){
       document.getElementById("select-voice").value = "(en-US, Jessa24kRUS)";
     }
     if ($(this).val() == "fr"){
       document.getElementById("select-voice").value = "(fr-FR, HortenseRUS)";
     }
     if ($(this).val() == "de"){
       document.getElementById("select-voice").value = "(de-DE, HeddaRUS)";
     }
     if ($(this).val() == "el"){
       document.getElementById("select-voice").value = "(el-GR, Stefanos)";
     }
     if ($(this).val() == "he"){
       document.getElementById("select-voice").value = "(he-IL, Asaf)";
     }
     if ($(this).val() == "hi"){
       document.getElementById("select-voice").value = "(hi-IN, Kalpana, Apollo)";
     }
     if ($(this).val() == "it"){
       document.getElementById("select-voice").value = "(it-IT, LuciaRUS)";
     }
     if ($(this).val() == "ja"){
       document.getElementById("select-voice").value = "(ja-JP, HarukaRUS)";
     }
     if ($(this).val() == "ko"){
       document.getElementById("select-voice").value = "(ko-KR, HeamiRUS)";
     }
     if ($(this).val() == "pt"){
       document.getElementById("select-voice").value = "(pt-BR, HeloisaRUS)";
     }
     if ($(this).val() == "ru"){
       document.getElementById("select-voice").value = "(ru-RU, EkaterinaRUS)";
     }
     if ($(this).val() == "es"){
       document.getElementById("select-voice").value = "(es-ES, HelenaRUS)";
     }
     if ($(this).val() == "th"){
       document.getElementById("select-voice").value = "(th-TH, Pattara)";
     }
     if ($(this).val() == "tr"){
       document.getElementById("select-voice").value = "(tr-TR, SedaRUS)";
     }
     if ($(this).val() == "vi"){
       document.getElementById("select-voice").value = "(vi-VN, An)";
     }
   });
   ```

### <a name="test-your-app"></a>Uw app testen

Laten we spraaksynthese in de app te testen.

```
flask run
```

Ga naar het opgegeven serveradres. Typ tekst in het invoervak, selecteer dat een taal en druk op vertalen. U moet een vertaling. Vervolgens selecteert u een stem en druk op de Text to Speech knop converteren. de vertaling moet worden afgespeeld als kunstmatige spraak. Als dit niet werkt, zorg ervoor dat u uw abonnementssleutel hebt toegevoegd.

> [!TIP]
> Als de wijzigingen worden niet weergegeven of als de app niet zoals werkt u verwacht dat het te proberen uw cache wissen of een privé-/ incognito-venster te openen.

Dat is alles, hebt u een werkende app die wordt uitgevoerd vertalingen, sentiment en kunstmatige spraak analyseert. Druk op **CTRL + c** afsluiten van de app. Zorg ervoor dat u uitchecken de andere [Azure Cognitive Services](https://docs.microsoft.com/azure/cognitive-services/).

## <a name="get-the-source-code"></a>De broncode ophalen

De broncode voor dit project is beschikbaar op [GitHub](https://github.com/MicrosoftTranslator/Text-Translation-API-V3-Flask-App-Tutorial).

## <a name="next-steps"></a>Volgende stappen

* [ Naslaginformatie Translator Text-API](https://docs.microsoft.com/azure/cognitive-services/Translator/reference/v3-0-reference)
* [Naslaginformatie over de Text Analytics-API](https://westus.dev.cognitive.microsoft.com/docs/services/TextAnalytics.V2.0/operations/56f30ceeeda5650db055a3c7)
* [Naslaginformatie voor de Text to Speech-API](https://docs.microsoft.com/azure/cognitive-services/speech-service/rest-text-to-speech)
