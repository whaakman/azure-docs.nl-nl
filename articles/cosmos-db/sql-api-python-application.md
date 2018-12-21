---
title: Zelfstudie voor Python Flask-webtoepassingen voor Azure Cosmos DB
description: Bekijk een databasezelfstudie over het gebruik van Azure Cosmos DB voor het opslaan van en de toegang tot gegevens uit een Python Flask-webtoepassing die wordt gehost in Azure. Oplossingen voor het ontwikkelen van toepassingen zoeken.
keywords: Toepassingsontwikkeling, python flask, python-webtoepassing, python-webontwikkeling
services: cosmos-db
author: SnehaGunda
ms.service: cosmos-db
ms.component: cosmosdb-sql
ms.devlang: python
ms.topic: tutorial
ms.date: 02/23/2017
ms.author: sngun
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: a2c7b7c5dc68bb96e0b54dc89f2c170cdb20c792
ms.sourcegitcommit: 9fb6f44dbdaf9002ac4f411781bf1bd25c191e26
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2018
ms.locfileid: "53081901"
---
# <a name="build-a-python-flask-web-application-using-azure-cosmos-db"></a>Een Python Flask-webtoepassing bouwen met Azure Cosmos DB

> [!div class="op_single_selector"]
> * [.NET](sql-api-dotnet-application.md)
> * [Java](sql-api-java-application.md)
> * [Node.js](sql-api-nodejs-application.md)
> * [Python](sql-api-python-application.md)
> * [Xamarin](mobile-apps-with-xamarin.md)
> 

In deze zelfstudie wordt uitgelegd hoe u Azure Cosmos DB gebruikt om gegevens uit een Python Flask-webtoepassing die wordt gehost op Azure App Service, op te slaan en te openen. Voor deze zelfstudie wordt ervan uitgegaan dat u enige ervaring hebt met het gebruik van Python- en Azure-websites.

In deze zelfstudie komen de volgende onderwerpen aan bod:

1. Een Azure Cosmos DB-account maken en inrichten.
2. Een Python Flask-toepassing maken.
3. Verbinding maken met Azure Cosmos DB en deze gebruiken via uw webtoepassing.
4. De webtoepassing implementeren in Azure App Service.

In deze zelfstudie bouwt u een eenvoudige stemtoepassing waarmee u kunt stemmen.

![Schermopname van de stemtoepassing die is gemaakt met deze databasezelfstudie](./media/sql-api-python-application/cosmos-db-pythonr-run-application.png)

## <a name="database-tutorial-prerequisites"></a>Vereisten voor de databasezelfstudie
Voordat u de instructies in dit artikel uitvoert, moet het volgende zijn geïnstalleerd:

* [Een Azure-abonnement](https://azure.microsoft.com/free/?WT.mc_id=A261C142F). 

  [!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

* [Visual Studio 2017](https://www.visualstudio.com/downloads/) waarbij **Azure-ontwikkeling** en **Python-ontwikkeling** zijn ingeschakeld. U kunt controleren of deze vereisten zijn geïnstalleerd en deze installeren door het **installatieprogramma voor Visual Studio** lokaal te openen.   
* [Microsoft Azure SDK voor Python 2.7](https://azure.microsoft.com/downloads/). 
* [Python 2.7](https://www.python.org/downloads/windows/). U kunt de 32-bits of 64-bits installatie gebruiken.

> [!IMPORTANT]
> Als u Python 2.7 voor het eerst installeert, moet u in het scherm Customize Python 2.7.13 (Python 2.7.13 aanpassen) de optie **Add python.exe to Path** (Python.exe aan het pad toevoegen) selecteren.
> 
> ![Schermopname van het scherm Customize Python 2.7.11 (Python 2.7.11 aanpassen), waarin u Add python.exe to Path (Python.exe aan het pad toevoegen) moet selecteren](./media/sql-api-python-application/cosmos-db-python-install.png)
> 
> 

* [Microsoft Visual C++ Compiler voor Python 2.7](https://www.microsoft.com/en-us/download/details.aspx?id=44266).

## <a name="step-1-create-an-azure-cosmos-db-database-account"></a>Stap 1: een Azure Cosmos DB-databaseaccount maken
Begin met het maken van een Azure Cosmos DB-account. Als u al een account hebt of de Azure Cosmos DB-emulator gebruikt voor deze zelfstudie, kunt u direct doorgaan naar [Stap 2: een nieuwe Python Flask-webtoepassing maken](#step-2-create-a-new-python-flask-web-application).

[!INCLUDE [cosmos-db-create-dbaccount](../../includes/cosmos-db-create-dbaccount.md)]

<br/>
U kunt nu zien hoe u een compleet nieuwe Python Flask-toepassing maakt.

## <a name="step-2-create-a-new-python-flask-web-application"></a>Stap 2: een nieuwe Python Flask-webtoepassing maken
1. Wijs in het menu **Bestand** van Visual Studio de optie **Nieuw** aan en klik vervolgens op **Project**.
   
    Het dialoogvenster **Nieuw project** wordt weergegeven.
2. Vouw in het linkerdeelvenster achtereenvolgens **Sjablonen** en **Python** uit en klik vervolgens op **Web**. 
3. Selecteer in het middelste deelvenster de optie **Flask Web Project** (Flask-webproject), typ in het vak **Naam** de tekst **tutorial** en klik vervolgens op **OK**. Houd er rekening mee dat Python-pakketnamen uitsluitend kleine letters mogen bevatten, zoals beschreven in de [Style Guide for Python Code](https://www.python.org/dev/peps/pep-0008/#package-and-module-names) (Stijlgids voor Python-code).
   
    Als u nog niet bekend bent met Python Flask, dit is een ontwikkelframework voor webtoepassingen waarmee u sneller webtoepassingen in Python kunt maken.
   
    ![Schermopname van het venster Nieuw project in Visual Studio met aan de linkerkant Python gemarkeerd, in het midden het geselecteerde Python Flask-webproject en de naam tutorial in het naamvak](./media/sql-api-python-application/image9.png)
4. Klik in het venster **Python Tools for Visual Studio** (Python-hulpprogramma's voor Visual Studio) op **Install into a virtual environment** (Installeren in een virtuele omgeving). 
   
    ![Schermopname van de databasezelfstudie - Venster met Python Tools for Visual Studio](./media/sql-api-python-application/python-install-virtual-environment.png)
5. Selecteer in het venster **Virtuele omgeving toevoegen** de optie Python 2.7 of Python 3.5 in het vak Een interpreter selecteren, accepteer de andere standaardinstellingen en klik op **Maken**. Hiermee stelt u de vereiste virtuele Python-omgeving voor uw project in.
   
    ![Schermopname van de databasezelfstudie - Venster met Python Tools for Visual Studio](./media/sql-api-python-application/image10_A.png)
   
    Wanneer de omgeving is geïnstalleerd, verschijnt de volgende tekst in het uitvoervenster: `Successfully installed Flask-0.10.1 Jinja2-2.8 MarkupSafe-0.23 Werkzeug-0.11.5 itsdangerous-0.24 'requirements.txt' was installed successfully.`.

## <a name="step-3-modify-the-python-flask-web-application"></a>Stap 3: de Python Flask-webtoepassing wijzigen
### <a name="add-the-python-flask-packages-to-your-project"></a>De Python Flask-pakketten aan uw project toevoegen
Zodra het project is ingesteld, moet u de vereiste Flask-pakketten toevoegen aan uw project, inclusief pydocumentdb, het Python-pakket voor de Azure Cosmos DB SQL-API.

1. Open het bestand **requirements.txt** in Solution Explorer en vervang de inhoud door het volgende:
   
        flask==0.9
        flask-mail==0.7.6
        sqlalchemy==0.7.9
        flask-sqlalchemy==0.16
        sqlalchemy-migrate==0.7.2
        flask-whooshalchemy==0.55a
        flask-wtf==0.8.4
        pytz==2013b
        flask-babel==0.8
        flup
        pydocumentdb>=1.0.0
2. Sla het bestand **requirements.txt** op. 
3. Klik in Solution Explorer met de rechtermuisknop op **env** en klik op **Install from requirements.txt** (Installeren vanuit requirements.txt).
   
    ![Schermopname waarin env (Python 2.7) is geselecteerd en de optie Install from requirements.txt (Installeren vanuit requirements.txt) in de lijst is gemarkeerd](./media/sql-api-python-application/cosmos-db-python-install-from-requirements.png)
   
    Zodra de installatie is voltooid, wordt in het uitvoervenster het volgende weergegeven:
   
        Successfully installed Babel-2.3.2 Tempita-0.5.2 WTForms-2.1 Whoosh-2.7.4 blinker-1.4 decorator-4.0.9 flask-0.9 flask-babel-0.8 flask-mail-0.7.6 flask-sqlalchemy-0.16 flask-whooshalchemy-0.55a0 flask-wtf-0.8.4 flup-1.0.2 pydocumentdb-1.6.1 pytz-2013b0 speaklater-1.3 sqlalchemy-0.7.9 sqlalchemy-migrate-0.7.2
   
   > [!NOTE]
   > In uitzonderlijke gevallen wordt er een fout in het uitvoervenster weergegeven. Als dit gebeurt, controleert u of de fout verband houdt met de opschoonbewerking. Soms mislukt de opschoonbewerking, maar wordt de installatie wel gewoon voltooid (schuif naar boven in het uitvoervenster om dit te controleren). U kunt uw installatie controleren door [de virtuele omgeving te verifiëren](#verify-the-virtual-environment). Als de installatie is mislukt maar de controle is geslaagd is, kunt u zonder problemen doorgaan.
   > 
   > 

### <a name="verify-the-virtual-environment"></a>De virtuele omgeving verifiëren
Zorg ervoor dat alles juist is geïnstalleerd.

1. Bouw de oplossing op door op **Ctrl**+**Shift**+**B** te drukken.
2. Zodra de opbouwbewerking is voltooid, start u de website door op **F5** te drukken. Hiermee wordt de Flask-ontwikkelaarsserver en uw webbrowser gestart. De volgende pagina wordt weergegeven:
   
    ![Het lege Python Flask-webontwikkelingsproject in een browser](./media/sql-api-python-application/image12.png)
3. Stop de foutopsporing voor de website door in Visual Studio op **Shift**+**F5** te drukken.

### <a name="create-database-collection-and-document-definitions"></a>Database-, verzamelings- en documentdefinities maken
U kunt nu de stemtoepassing maken door nieuwe bestanden toe te voegen en andere bestanden bij te werken.

1. Klik in Solution Explorer met de rechtermuisknop op het project **tutorial** en klik vervolgens op **Toevoegen** en **Nieuw item**. Selecteer **het lege Python-bestand** en noem het bestand **forms.py**.  
2. Kopieer de volgende code naar het bestand forms.py en sla het bestand vervolgens op:

```python
from flask_wtf import Form
from wtforms import RadioField

class VoteForm(Form):
    deploy_preference  = RadioField('Deployment Preference', choices=[
        ('Web Site', 'Web Site'),
        ('Cloud Service', 'Cloud Service'),
        ('Virtual Machine', 'Virtual Machine')], default='Web Site')
```


### <a name="add-the-required-imports-to-viewspy"></a>De vereiste imports toevoegen aan views.py
1. Vouw in Solution Explorer de map **tutorial** uit en open het bestand **views.py**. 
2. Voeg de volgende importinstructies boven aan het bestand **views.py** toe en sla het bestand vervolgens op. Hiermee worden de PythonSDK van Azure Cosmos DB en de Flask-pakketten geïmporteerd.
   
    ```python
    from forms import VoteForm
    import config_cosmos
    import pydocumentdb.document_client as document_client
    ```

### <a name="create-database-collection-and-document"></a>De database, de verzameling en het document maken
* Voeg de volgende code toe aan het eind van het bestand **views.py**. Hiermee wordt de database gemaakt die door het formulier wordt gebruikt. Verwijder de bestaande code in **views.py** niet. U kunt de code gewoon aan het eind toevoegen.

```python
@app.route('/create')
def create():
    """Renders the contact page."""
    client = document_client.DocumentClient(config_cosmos.COSMOSDB_HOST, {'masterKey': config_cosmos.COSMOSDB_KEY})

    # Attempt to delete the database.  This allows this to be used to recreate as well as create
    try:
        db = next((data for data in client.ReadDatabases() if data['id'] == config_cosmos.COSMOSDB_DATABASE))
        client.DeleteDatabase(db['_self'])
    except:
        pass

    # Create database
    db = client.CreateDatabase({ 'id': config_cosmos.COSMOSDB_DATABASE })

    # Create collection
    collection = client.CreateCollection(db['_self'],{ 'id': config_cosmos.COSMOSDB_COLLECTION })

    # Create document
    document = client.CreateDocument(collection['_self'],
        { 'id': config_cosmos.COSMOSDB_DOCUMENT,
          'Web Site': 0,
          'Cloud Service': 0,
          'Virtual Machine': 0,
          'name': config_cosmos.COSMOSDB_DOCUMENT 
        })

    return render_template(
       'create.html',
        title='Create Page',
        year=datetime.now().year,
        message='You just created a new database, collection, and document.  Your old votes have been deleted')
```


### <a name="read-database-collection-document-and-submit-form"></a>De database, de verzameling en het document lezen en het formulier verzenden
* Voeg de volgende code toe aan het eind van het bestand **views.py**. Deze code wordt gebruikt om het formulier in te stellen en de database, de verzameling en het documenten te lezen. Verwijder de bestaande code in **views.py** niet. U kunt de code gewoon aan het eind toevoegen.

```python
@app.route('/vote', methods=['GET', 'POST'])
def vote(): 
    form = VoteForm()
    replaced_document ={}
    if form.validate_on_submit(): # is user submitted vote  
        client = document_client.DocumentClient(config_cosmos.COSMOSDB_HOST, {'masterKey': config_cosmos.COSMOSDB_KEY})

        # Read databases and take first since id should not be duplicated.
        db = next((data for data in client.ReadDatabases() if data['id'] == config_cosmos.COSMOSDB_DATABASE))

        # Read collections and take first since id should not be duplicated.
        coll = next((coll for coll in client.ReadCollections(db['_self']) if coll['id'] == config_cosmos.COSMOSDB_COLLECTION))

        # Read documents and take first since id should not be duplicated.
        doc = next((doc for doc in client.ReadDocuments(coll['_self']) if doc['id'] == config_cosmos.COSMOSDB_DOCUMENT))

        # Take the data from the deploy_preference and increment our database
        doc[form.deploy_preference.data] = doc[form.deploy_preference.data] + 1
        replaced_document = client.ReplaceDocument(doc['_self'], doc)

        # Create a model to pass to results.html
        class VoteObject:
            choices = dict()
            total_votes = 0

        vote_object = VoteObject()
        vote_object.choices = {
            "Web Site" : doc['Web Site'],
            "Cloud Service" : doc['Cloud Service'],
            "Virtual Machine" : doc['Virtual Machine']
        }
        vote_object.total_votes = sum(vote_object.choices.values())

        return render_template(
            'results.html', 
            year=datetime.now().year, 
            vote_object = vote_object)

    else :
        return render_template(
            'vote.html', 
            title = 'Vote',
            year=datetime.now().year,
            form = form)
```


### <a name="create-the-html-files"></a>De HTML-bestanden maken
1. Klik in de map **tutorial** in Solution Explorer met de rechtermuisknop op de map **templates** en klik achtereenvolgens op **Toevoegen** en **Nieuw item**. 
2. Selecteer **HTML-pagina** en typ in het naamvak **create.html**. 
3. Herhaal de eerste twee stappen om twee extra HTML-bestanden te maken: results.html en vote.html.
4. Voeg in het bestand **create.html** de volgende code toe aan het `<body>`-element. Er wordt een bericht weergegeven dat er een nieuwe database, een nieuwe verzameling en een nieuw document is gemaakt.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>{{ title }}.</h2>
    <h3>{{ message }}</h3>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```
5. Voeg in het bestand **results.html** de volgende code toe aan het `<body`-element. De resultaten van de poll worden weergegeven.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Results of the vote</h2>
        <br />
   
    {% for choice in vote_object.choices %}
    <div class="row">
        <div class="col-sm-5">{{choice}}</div>
            <div class="col-sm-5">
                <div class="progress">
                    <div class="progress-bar" role="progressbar" aria-valuenow="{{vote_object.choices[choice]}}" aria-valuemin="0" aria-valuemax="{{vote_object.total_votes}}" style="width: {{(vote_object.choices[choice]/vote_object.total_votes)*100}}%;">
                                {{vote_object.choices[choice]}}
                </div>
            </div>
            </div>
    </div>
    {% endfor %}
   
    <br />
    <a class="btn btn-primary" href="{{ url_for('vote') }}">Vote again?</a>
    {% endblock %}
    ```
6. Voeg in het bestand **vote.html** de volgende code toe aan het `<body`-element. De poll wordt weergegeven en de stemmen worden geaccepteerd. Bij het registreren van de stemmen wordt de besturing overgegeven aan het bestand views.py, waar de uitgebrachte stem wordt herkend en het document dienovereenkomstig wordt toegevoegd.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>What is your favorite way to host an application on Azure?</h2>
    <form action="" method="post" name="vote">
        {{form.hidden_tag()}}
            {{form.deploy_preference}}
            <button class="btn btn-primary" type="submit">Vote</button>
    </form>
    {% endblock %}
    ```
7. In de map **templates** vervangt u de inhoud van het bestand **index.html** door de volgende inhoud. Deze pagina fungeert als de landingspagina voor uw toepassing.
   
    ```html
    {% extends "layout.html" %}
    {% block content %}
    <h2>Python + Azure Cosmos DB Voting Application.</h2>
    <h3>This is a sample Cosmos DB voting application using PyDocumentDB</h3>
    <p><a href="{{ url_for('create') }}" class="btn btn-primary btn-large">Create/Clear the Voting Database &raquo;</a></p>
    <p><a href="{{ url_for('vote') }}" class="btn btn-primary btn-large">Vote &raquo;</a></p>
    {% endblock %}
    ```

### <a name="add-a-configuration-file-and-change-the-initpy"></a>Een configuratiebestand toevoegen en de \_\_init\_\_.py wijzigen
1. Klik in Solution Explorer met de rechtermuisknop op het project **tutorial** en klik achtereenvolgens op **Toevoegen** en **Nieuw item**, selecteer **het lege Python-bestand** en noem het bestand **config_cosmos.py**. Dit configuratiebestand is nodig voor de formulieren in Flask. U kunt dit bestand ook gebruiken om een geheime sleutel te verstrekken. Deze sleutel is echter niet nodig voor deze zelfstudie.
2. Voeg de volgende code toe aan het bestand config_cosmos.py. In de volgende stap moet u de waarden wijzigen voor **COSMOSDB\_HOST** en **COSMOSDB\_KEY**.
   
    ```python
    CSRF_ENABLED = True
    SECRET_KEY = 'you-will-never-guess'
   
    COSMOSDB_HOST = 'https://YOUR_COSMOSDB_NAME.documents.azure.com:443/'
    COSMOSDB_KEY = 'YOUR_SECRET_KEY_ENDING_IN_=='
   
    COSMOSDB_DATABASE = 'voting database'
    COSMOSDB_COLLECTION = 'voting collection'
    COSMOSDB_DOCUMENT = 'voting document'
    ```
3. Navigeer in de [Azure-portal](https://portal.azure.com/) naar de pagina **Sleutels** door op **Bladeren** en **Azure Cosmos DB-accounts** te klikken. Dubbelklik op de naam van het account dat u wilt gebruiken en klik vervolgens op de knop **Sleutels** in het gebied **Essentials**. Kopieer op de pagina **Sleutels** de **URI**-waarde en plak deze in het bestand **config.py** als waarde voor de eigenschap **COSMOSDB\_HOST**. 
4. Terug in de Azure-portal kopieert u op de pagina **Sleutels** de waarde van de **primaire sleutel** of de **secundaire sleutel** en plakt u deze in het bestand **config_cosmos.py** als waarde voor de eigenschap **COSMOSDB\_KEY**.
5. Voeg in het bestand **\_\_init\_\_.py** de volgende regels toe om het lezen van het configuratiebestand en basislogboekregistratie op te nemen: 
   
        app.config.from_object('config_cosmos')
        logging.basicConfig(level=logging.INFO,format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
        logger = logging.getLogger(__name__)
   
    De inhoud van het bestand is nu:
   
    ```python
    import logging
    from flask import Flask
    app = Flask(__name__)
    app.config.from_pyfile('config_cosmos')
    logging.basicConfig(level=logging.INFO,format='%(asctime)s - %(name)s - %(levelname)s - %(message)s')
    logger = logging.getLogger(__name__)

    import tutorial.views
    ```
6. Zodra alle bestanden zijn toegevoegd, ziet Solution Explorer er als volgt uit:
   
    ![Schermopname van het Solution Explorer-venster in Visual Studio](./media/sql-api-python-application/cosmos-db-python-solution-explorer.png)

## <a name="step-4-run-your-web-application-locally"></a>Stap 4: de webtoepassing lokaal uitvoeren
1. Bouw de oplossing op door op **Ctrl**+**Shift**+**B** te drukken.
2. Zodra de opbouwbewerking is voltooid, start u de website door op **F5** te drukken. Uw scherm ziet er nu als volgt uit:
   
    ![Schermopname van de Python + Azure Cosmos DB-stemtoepassing in een webbrowser](./media/sql-api-python-application/cosmos-db-pythonr-run-application.png)
3. Klik op **Create/Clear the Voting Database** (De stemdatabase maken/wissen) om de database te genereren.
   
    ![Schermopname van de Create Page (Pagina maken) maken van de webtoepassing – ontwikkelingsgegevens](./media/sql-api-python-application/cosmos-db-python-run-create-page.png)
4. Klik vervolgens op **Vote** (Stemmen) en selecteer uw optie.
   
    ![Schermopname van de webtoepassing met een vraag waarvoor kan worden gestemd](./media/sql-api-python-application/cosmos-db-vote.png)
5. Voor elke stem die u uitbrengt, wordt de desbetreffende teller verhoogd.
   
    ![Schermopname van de pagina met stemresultaten](./media/sql-api-python-application/cosmos-db-voting-results.png)
6. Stop de foutopsporing voor het project door op Shift + F5 te drukken.

## <a name="step-5-deploy-the-web-application-to-azure"></a>Stap 5: de webtoepassing implementeren in Azure
Nu de volledige toepassing correct werkt met Azure Cosmos DB, gaan we een web.config-bestand maken, de bestanden op de server bijwerken zodat deze overeenkomen met de lokale omgeving en vervolgens de voltooide app in Azure bekijken. Deze procedure is specifiek voor Visual Studio 2017. Zie [Publiceren naar Azure App Service](/visualstudio/python/publishing-to-azure) als u een andere versie van Visual Studio gebruikt.

1. Klik in Visual Studio **Solution Explorer** met de rechtermuisknop op het project en selecteer **Toevoegen> Nieuw item...**. Selecteer in het dialoogvenster dat wordt weergegeven, de sjabloon **Azure web.config (Fast CGI)** en selecteer **OK**. Hiermee wordt een `web.config`-bestand in de hoofdmap van uw project gemaakt. 

2. Wijzig de `<system.webServer>`-sectie in `web.config` zodat het pad overeenkomt met de Python-installatie. Voor Python 2.7 x64 ziet de vermelding er bijvoorbeeld als volgt uit:
    
    ```xml
    <system.webServer>
        <handlers>
            <add name="PythonHandler" path="*" verb="*" modules="FastCgiModule" scriptProcessor="D:\home\Python27\python.exe|D:\home\Python27\wfastcgi.py" resourceType="Unspecified" requireAccess="Script"/>
        </handlers>
    </system.webServer>
    ```

3. Stel de vermelding `WSGI_HANDLER` in `web.config` in op `tutorial.app` zodat deze overeenkomt met uw projectnaam. 

    ```xml
    <!-- Flask apps only: change the project name to match your app -->
    <add key="WSGI_HANDLER" value="tutorial.app"/>
    ```

4. Vouw in Visual Studio **Solution Explorer** de map **zelfstudie** uit, klik met de rechtermuisknop op de map `static`, selecteer **Toevoegen > Nieuw item...**, selecteer de sjabloon 'Azure static files web.config' en selecteer **OK**. Met deze actie wordt er nog een `web.config` gemaakt in de map `static` waarmee de Python-verwerking voor deze map wordt uitgeschakeld. Met deze configuratie worden aanvragen voor statische bestanden verzonden naar de standaardwebserver in plaats van met de Python-toepassing.

5. Sla de bestanden op en klik in Solution Explorer met de rechtermuisknop op het project (zorg ervoor dat de toepassing niet meer lokaal wordt uitgevoerd) en selecteer **Publiceren**.  
   
     ![Schermopname van de geselecteerde zelfstudie in Solution Explorer met de optie Publiceren gemarkeerd](./media/sql-api-python-application/image20.png)
6. Selecteer **Microsoft Azure App Service** in het dialoogvenster **Publiceren**, selecteer **Nieuwe maken** en klik vervolgens op **Publiceren**.
   
    ![Schermopname van het venster Publish Web (Publiceren naar het web) waarbij Microsoft Azure App Service is gemarkeerd](./media/sql-api-python-application/cosmos-db-python-publish.png)
7. Voer in het dialoogvenster **App Service maken** de naam van uw web-app en het **abonnement**, de **resourcegroep** en het **App Service-abonnement** in en klik op **Maken**.
   
    ![Schermopname van het venster Microsoft Azure Web Apps (Web-apps van Microsoft Azure)](./media/sql-api-python-application/cosmos-db-python-create-app-service.png)
8. Na een paar seconden kopieert Visual Studio uw bestanden naar de server en wordt 'De pagina kan niet worden weergegeven omdat er een interne serverfout is opgetreden' weergegeven. op de pagina `http://<your app service>.azurewebsites.net/`.

9. Open in de Azure-portal uw nieuwe App Service-account en schuif in het navigatiemenu omlaag naar de sectie **Ontwikkelprogramma's**, selecteer **Extensies** en klik vervolgens op **+ Toevoegen**.

10. Schuif op de pagina **Extensie kiezen** omlaag naar de meest recente Python 2.7-installatie en selecteer de optie x86 bits of x64 bits en klik op **OK** om de juridische voorwaarden te accepteren.  
   
11. Gebruik de Kudu-console, waarnaar u kunt bladeren bij `https://<your app service name>.scm.azurewebsites.net/DebugConsole`, om de pakketten te installeren die worden vermeld in het bestand `requirements.txt` van uw app. Ga hiervoor in de diagnostische Kudu-console naar de Python-map `D:\home\Python27` en voer de volgende opdracht uit zoals wordt beschreven in de sectie [Kudu-console](/visualstudio/python/managing-python-on-azure-app-service#azure-app-service-kudu-console):

    ```
    D:\home\Python27>python -m pip install --upgrade -r /home/site/wwwroot/requirements.txt
    ```          

12. Start de App Service opnieuw in de Azure-portal nadat u de nieuwe pakketten hebt geïnstalleerd door op de knop **Opnieuw starten** te drukken. 

    > [!Tip] 
    > Als u wijzigingen aanbrengt in het bestand `requirements.txt` van uw app, moet u de Kudu-console nogmaals gebruiken om de pakketten te installeren die nu in dit bestand worden vermeld. 

13. Wanneer u de serveromgeving volledig hebt geconfigureerd, vernieuwt u de pagina in de browser, waarna de web-app wordt weergegeven.

    ![Resultaten van het publiceren van Bottle-, Flask- en Django-apps naar App Service](./media/sql-api-python-application/python-published-app-services.png)

    > [!Tip] 
    > Als de webpagina niet wordt weergegeven of als het bericht 'De pagina kan niet worden weergegeven omdat er een interne serverfout is opgetreden.' nog steeds wordt weergegeven, opent u het bestand web.config in Kudo, voegt u ` <httpErrors errorMode="Detailed"></httpErrors>` toe aan de sectie system.webServer en vernieuwt u de pagina. Hierdoor wordt er gedetailleerde foutuitvoer naar de browser verzonden. 

## <a name="troubleshooting"></a>Problemen oplossen
Als dit de eerste Python-app is die u op uw computer uitvoert, moet u ervoor zorgen dat de variabele PATH de volgende mappen (of gelijkwaardige installatielocaties) bevat:

    C:\Python27\site-packages;C:\Python27\;C:\Python27\Scripts;

Als er een foutbericht wordt weergegeven op uw stempagina en u het project een andere naam dan **zelfstudie** hebt gegeven, moet u ervoor zorgen dat **\_\_init\_\_.py** verwijst naar de juiste projectnaam in de regel: `import tutorial.view`.

## <a name="next-steps"></a>Volgende stappen
Gefeliciteerd! U hebt zojuist uw eerste Python-webtoepassing met Azure Cosmos DB gemaakt en deze gepubliceerd naar Azure.

Als u de functionaliteit van uw webtoepassing wilt uitbreiden, bekijkt u de beschikbare API's in de [Azure Cosmos DB Python SDK](sql-api-sdk-python.md).

Zie [Python-ontwikkelaarscentrum](https://azure.microsoft.com/develop/python/) voor meer informatie over Azure, Visual Studio en Python. 

Zie [The Flask Mega-Tutorial, Part I: Hello, World!](https://blog.miguelgrinberg.com/post/the-flask-mega-tutorial-part-i-hello-world) (Engelstalig) voor aanvullende zelfstudies over Python Flask.  
