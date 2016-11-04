---
title: Django en MySQL in Azure met Python Tools 2.2 for Visual Studio
description: Leer hoe u de Python Tools for Visual Studio gebruikt om een Django-web-app te maken waarin gegevens worden opgeslagen in een MySQL-database, en deze vervolgens in Azure App Service-web-apps te implementeren.
services: app-service\web
documentationcenter: python
author: huguesv
manager: wpickett
editor: ''

ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: python
ms.topic: get-started-article
ms.date: 07/07/2016
ms.author: huvalo

---
# Django en MySQL in Azure met Python Tools 2.2 for Visual Studio
[!INCLUDE [tabs](../../includes/app-service-web-get-started-nav-tabs.md)]

In deze zelfstudie gebruikt u [Python Tools for Visual Studio](PTVS.md) om een eenvoudige poll-web-app te maken met een van de PTVS-voorbeeldsjablonen. U leert hoe u een MySQL-service gebruikt die wordt gehost in Azure, hoe u de web-app kunt configureren voor het gebruik van MySQL en hoe u de web-app publiceert naar [Azure App Service-web-apps](http://go.microsoft.com/fwlink/?LinkId=529714).

> [!NOTE]
> De informatie in deze zelfstudie is ook beschikbaar in de volgende video:
> 
> [PTVS 2.1: Django-app met MySQL][video]
> 
> 

Raadpleeg het [Python Developer Center] voor meer artikelen over het ontwikkelen van Azure App Service-web-apps met PTVS met behulp van Bottle-, Flask- en Django-webframeworks, met Azure Table Storage-, MySQL- en SQL Database-services. Dit artikel is gericht op App Service, maar voor het ontwikkelen van [Azure Cloud Services] volgt u soortgelijk stappen.

## Vereisten
* Visual Studio 2015
* [Python 2.7 32-bits] of [Python 3.4 32-bits]
* [Python Tools 2.2 for Visual Studio]
* [Python Tools 2.2 for Visual Studio Samples VSIX]
* [Azure SDK-tools voor VS 2015]
* Django 1.9 of hoger

[!INCLUDE [create-account-and-websites-note](../../includes/create-account-and-websites-note.md)]

<!-- This note should not render as part of the the previous include. -->

> [!NOTE]
> Als u aan de slag wilt met Azure App Service voordat u zich aanmeldt voor een Azure-account, gaat u naar [App Service uitproberen](http://go.microsoft.com/fwlink/?LinkId=523751). Hier kunt u direct een tijdelijke web-app maken in App Service. Er is geen creditcard vereist en u bent nergens toe verplicht.
> 
> 

## Het project maken
In deze sectie maakt u een Visual Studio-project met behulp van een voorbeeldsjabloon. U maakt een virtuele omgeving en installeert de vereiste pakketten. U maakt een lokale database met behulp van sqlite. Vervolgens voert u de toepassing lokaal uit.

1. Selecteer in Visual Studio **File**, **New Project**.
2. De projectsjablonen uit de [Python Tools 2.2 for Visual Studio Samples VSIX] zijn beschikbaar onder **Python**, **Samples**. Selecteer **Polls Django Web Project** en klik op OK om het project te maken.
   
    ![Het dialoogvenster New Project](./media/web-sites-python-ptvs-django-mysql/PollsDjangoNewProject.png)
3. U wordt gevraagd om externe pakketten te installeren. Selecteer **Install into a virtual environment**.
   
    ![Het dialoogvenster External Packages](./media/web-sites-python-ptvs-django-mysql/PollsDjangoExternalPackages.png)
4. Selecteer **Python 2.7** of **Python 3.4** als basisinterpreter.
   
    ![Het dialoogvenster Add Virtual Environment](./media/web-sites-python-ptvs-django-mysql/PollsCommonAddVirtualEnv.png)
5. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer **Python**. Selecteer vervolgens **Django Migrate**.  Selecteer vervolgens **Django Create Superuser**.
6. Hiermee wordt in de projectmap een Django-beheerconsole en een sqlite-database gemaakt. Volg de aanwijzingen voor het maken van een gebruiker.
7. Controleer of de toepassing werkt, door te drukken op `F5`.
8. Klik in de navigatiebalk bovenaan op **Log in**.
   
    ![Django-navigatiebalk](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalMenu.png)
9. Voer de referenties in voor de gebruiker die u hebt gemaakt tijdens het synchroniseren van de database.
   
    ![Aanmeldscherm](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserLocalLogin.png)
10. Klik op **Create Sample Polls**.
    
     ![Voorbeeld-polls maken](./media/web-sites-python-ptvs-django-mysql/PollsDjangoCommonBrowserNoPolls.png)
11. Klik op een poll en stem.
    
     ![Stemmen in voorbeeld-polls](./media/web-sites-python-ptvs-django-mysql/PollsDjangoSqliteBrowser.png)

## Een MySQL-database maken
Voor de database maakt u een gehoste ClearDB MySQL-database in Azure.

Als alternatief kunt u uw eigen virtuele machine maken die wordt uitgevoerd in Azure, en vervolgens MySQL zelf installeren en beheren.

U kunt een database maken met een gratis abonnement. Ga hiervoor als volgt te werk.

1. Meld u aan bij de [Azure Portal].
2. Klik boven in het navigatiedeelvenster achtereenvolgens op **NIEUW**, **Gegevens en opslag** en **MySQL-database**. 
3. Configureer de nieuwe MySQL-database door een nieuwe resourcegroep te maken en hiervoor de juiste locatie te selecteren.
4. Nadat de MySQL-database is gemaakt, klikt u op de databaseblade op **Eigenschappen**.
5. Gebruik de knop Kopiëren om de waarde van **VERBINDINGSREEKS** op het klembord te plaatsen.

## Het project configureren
In deze sectie configureert u de web-app voor het gebruik van de MySQL-database die u zojuist hebt gemaakt. U kunt ook extra Python-pakketten installeren die vereist zijn voor het gebruik van MySQL-databases met Django. Vervolgens voert u de web-app lokaal uit.

1. Open in Visual Studio **settings.py** vanuit de map *ProjectName*. Plak de verbindingsreeks tijdelijk in de editor. De verbindingsreeks heeft de volgende indeling:
   
        Database=<NAME>;Data Source=<HOST>;User Id=<USER>;Password=<PASSWORD>
   
    Wijzig de standaarddatabase **ENGINE** voor het gebruik van MySQL en stel de waarden voor **NAME**, **USER**, **PASSWORD** en **HOST** van de **CONNECTIONSTRING** in.
   
        DATABASES = {
            'default': {
                'ENGINE': 'django.db.backends.mysql',
                'NAME': '<Database>',
                'USER': '<User Id>',
                'PASSWORD': '<Password>',
                'HOST': '<Data Source>',
                'PORT': '',
            }
        }
2. Klik onder **Python-omgevingen** in Solution Explorer met de rechtermuisknop op de virtuele omgeving en selecteer **Install Python Package**.
3. Installeer het pakket `mysqlclient` met behulp van **pip**.
   
    ![Het dialoogvenster Install Package](./media/web-sites-python-ptvs-django-mysql/PollsDjangoMySQLInstallPackage.png)
4. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer **Python**. Selecteer vervolgens **Django Migrate**.  Selecteer vervolgens **Django Create Superuser**.
   
    Hiermee maakt u de tabellen voor de MySQL-database die u in de vorige sectie hebt gemaakt. Volg de aanwijzingen voor het maken van een gebruiker. De gebruiker hoeft niet dezelfde te zijn als de gebruiker in de sqlite-database die in de eerste sectie van dit artikel is gemaakt.
5. Voer de toepassing uit met `F5`. Polls die zijn gemaakt met **Create Sample Polls** en de gegevens die zijn ingediend via stemmen, worden geserialiseerd in de MySQL-database.

## De web-app publiceren naar Azure App Service
De Azure SDK voor .NET biedt een eenvoudige manier om uw web-app in Azure App Service te implementeren.

1. Klik in **Solution Explorer** met de rechtermuisknop op het projectknooppunt en selecteer **Publiceren**.
   
    ![Het dialoogvenster Publish Web](./media/web-sites-python-ptvs-django-mysql/PollsCommonPublishWebSiteDialog.png)
2. Klik op **Microsoft Azure App Service**.
3. Klik op **Nieuw** om een nieuwe web-app te maken.
4. Vul de volgende velden in en klik op **Maken**:
   
   * **Web-appnaam**
   * **App Service-plan**
   * **Resourcegroep**
   * **Regio**
   * Laat **Databaseserver** ingesteld op **Geen database**.
5. Accepteer alle overige standaardwaarden en klik op **Publiceren**.
6. De gepubliceerde web-app wordt automatisch geopend in uw webbrowser. De web-app hoort nu correct te werken met behulp van de **MySQL**-database gehost in Azure.
   
    ![Webbrowser](./media/web-sites-python-ptvs-django-mysql/PollsDjangoAzureBrowser.png)
   
    Gefeliciteerd. U hebt uw op MySQL gebaseerde web-app gepubliceerd naar Azure.

## Volgende stappen
Volg deze koppelingen voor meer informatie over Python Tools for Visual Studio, Django en MySQL.

* [Documentatie bij Python Tools for Visual Studio]
  * [Webprojecten]
  * [Cloudserviceprojecten]
  * [Foutopsporing op afstand in Microsoft Azure]
* [Documentatie bij Django]
* [MySQL]

Raadpleeg het [Python Developer Center](/develop/python/) voor meer informatie.

<!--Link references-->

[Python Developer Center]: /develop/python/
[Azure Cloud Services]: ../cloud-services-python-ptvs.md

<!--External Link references-->

[Azure Portal]: https://portal.azure.com
[Python Tools for Visual Studio]: http://aka.ms/ptvs
[Python Tools 2.2 for Visual Studio]: http://go.microsoft.com/fwlink/?LinkID=624025
[Python Tools 2.2 for Visual Studio Samples VSIX]: http://go.microsoft.com/fwlink/?LinkID=624025
[Azure SDK-tools voor VS 2015]: http://go.microsoft.com/fwlink/?LinkId=518003
[Python 2.7 32-bits]: http://go.microsoft.com/fwlink/?LinkId=517190 
[Python 3.4 32-bits]: http://go.microsoft.com/fwlink/?LinkId=517191
[Documentatie bij Python Tools for Visual Studio]: http://aka.ms/ptvsdocs
[Foutopsporing op afstand in Microsoft Azure]: http://go.microsoft.com/fwlink/?LinkId=624026
[Webprojecten]: http://go.microsoft.com/fwlink/?LinkId=624027
[Cloudserviceprojecten]: http://go.microsoft.com/fwlink/?LinkId=624028
[Documentatie bij Django]: https://www.djangoproject.com/
[MySQL]: http://www.mysql.com/
[video]: http://youtu.be/oKCApIrS0Lo



<!--HONumber=ago16_HO4-->


