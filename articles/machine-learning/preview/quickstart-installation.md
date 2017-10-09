---
title: Snelstartgids voor het installeren van Azure Machine Learning-services | Microsoft Docs
description: Deze snelstartgids laat zien hoe u Azure Machine Learning-resources kunt maken en hoe het installeren van Azure Machine Learning Workbench in zijn werk gaat.
services: machine-learning
author: hning86
ms.author: haining, raymondl, chhavib
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: hero-article
ms.date: 09/20/2017
ms.translationtype: HT
ms.sourcegitcommit: c3a2462b4ce4e1410a670624bcbcec26fd51b811
ms.openlocfilehash: 91d2f47a528050f644973044f96c0354b91dba25
ms.contentlocale: nl-nl
ms.lasthandoff: 09/25/2017

---

# <a name="create-azure-machine-learning-preview-accounts-and-install-azure-machine-learning-workbench"></a>Preview-accounts maken voor Azure Machine Learning en Azure Machine Learning Workbench installeren
Azure Machine Learning is een geïntegreerde end-to-end oplossing voor gegevenswetenschap en geavanceerde analyse voor professionele gegevenswetenschappers. Hiermee kunnen ze gegevens voorbereiden, experimenten ontwikkelen en modellen in de cloud implementeren.

In deze snelstartgids wordt beschreven hoe u accounts voor experimenten en modelbeheer kunt maken in de preview van Azure Machine Learning-services. Ook ziet u hoe u de Azure Machine Learning Workbench-bureaubladtoepassing en CLI-hulpprogramma's kunt installeren. Volg vervolgens een korte rondleiding door de functies van de preview van Azure Machine Learning met behulp van de tijdloze [iris-dataset](https://en.wikipedia.org/wiki/iris_flower_data_set) om een model op te bouwen voor het bepalen van de soort van een iris op basis van enkele van zijn fysieke kenmerken.  

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

## <a name="prerequisites"></a>Vereisten
Momenteel kan de Azure Machine Learning Workbench alleen worden geïnstalleerd op de volgende besturingssystemen: Windows 10, Windows Server 2016 en macOS Sierra.

## <a name="log-in-to-the-azure-portal"></a>Aanmelden bij Azure Portal
Meld u aan bij [Azure Portal](https://portal.azure.com/).

## <a name="create-azure-machine-learning-accounts"></a>Accounts maken voor Azure Machine Learning
Gebruik de Azure-portal om accounts in te richten voor Azure Machine Learning. 
1. Selecteer de knop **Nieuw** (+) in de linkerbovenhoek van de portal.

2. Typ 'Machine Learning' in de zoekbalk. Selecteer het zoekresultaat met de naam **Machine Learning Experimenten (preview)**.  Klik op het sterpictogram om van deze selectie een favoriet te maken in uw Azure-portal.

   ![Azure Machine Learning zoeken](media/quickstart-installation/portal-more-services.png)

3. Klik op **+ Toevoegen** om een nieuw account te configureren voor Machine Learning Experimenten. Het gedetailleerde formulier wordt geopend.

   ![Machine Learning Experimenten-account](media/quickstart-installation/portal-create-experimentation.png)

4. Vul het formulier Machine Learning Experimenten in met de volgende gegevens:

   Instelling|Voorgestelde waarde|Beschrijving
   ---|---|---
   Naam experimenteeraccount | _Unieke naam_ |Kies een unieke naam die uw account identificeert. U kunt uw eigen naam gebruiken of de naam van een afdeling of project die het experiment identificeert. De naam moet 2 tot 32 tekens lang zijn en mag alleen alfanumerieke tekens en afbreekstreepjes '-' bevatten. 
   Abonnement | _Uw abonnement_ |Het Azure-abonnement dat u wilt gebruiken voor uw experiment. Als u meerdere abonnementen hebt, kiest u het juiste abonnement waarin de resource zal worden gefactureerd.
   Resourcegroep | _Uw resourcegroep_ | U kunt een nieuwe resourcegroepnaam maken of een bestaande naam uit uw abonnement gebruiken.
   Locatie | _De regio het dichtst bij uw gebruikers_ | Kies de locatie die zich het dichtst bij uw gebruikers en de gegevensresources bevindt.
   Aantal seats | 2 | Typ het aantal seats. Deze selectie beïnvloedt de [prijzen](https://azure.microsoft.com/pricing/details/machine-learning/). De eerste twee seats zijn gratis. Gebruik twee seats voor deze snelstartgids. U kunt het aantal seats later naar behoefte bijwerken in de Azure-portal.
   Opslagaccount | _Unieke naam_ | Kies **Nieuw maken** en geef een naam op om een nieuw Azure-opslagaccount te maken, of kies **Bestaande gebruiken** en selecteer uw bestaande opslagaccount in de vervolgkeuzelijst. Het opslagaccount is vereist en wordt gebruikt voor het bewaren van projectartefacten en uitvoeringsgeschiedenisgegevens. 
   Werkruimte voor Experimenten-account | _Unieke naam_ | Geef een naam op voor de nieuwe werkruimte. De naam moet 2 tot 32 tekens lang zijn en mag alleen alfanumerieke tekens en afbreekstreepjes '-' bevatten.
   Eigenaar van de werkruimte toewijzen | _Uw account_ | Selecteer uw eigen account als de eigenaar van de werkruimte.
   Modelbeheer-account maken | *controleren* | Als onderdeel van het maken van een Experimenten-account, hebt u de optie om ook het Machine Learning Modelbeheer-account te maken. Deze bron wordt gebruikt wanneer u klaar bent om uw modellen te implementeren en beheren als realtime webservices. Wij raden aan het Modelbeheer-account tegelijk met het Experimenten-account te maken.
   Accountnaam | _Unieke naam_ | Kies een unieke naam die uw Modelbeheer-account identificeert. U kunt uw eigen naam gebruiken of de naam van een afdeling of project die het experiment identificeert. De naam moet 2 tot 32 tekens lang zijn en mag alleen alfanumerieke tekens en afbreekstreepjes '-' bevatten. 
   Prijscategorie voor Modelbeheer | **DEVTEST** | Klik op **Geen prijscategorie geselecteerd** om de prijscategorie voor uw nieuwe Modelbeheer-account op te geven. Selecteer om kosten te besparen de prijscategorie **DEVTEST** als deze beschikbaar is voor uw abonnement (beperkte beschikbaarheid), en anders de prijscategorie S1. Klik op **Selecteren** om de geselecteerde prijscategorie op te slaan. 
   Vastmaken aan dashboard | _controleren_ | Selecteer de optie **Vastmaken aan dashboard** om uw Azure Machine Learning Experimenten-account gemakkelijk te kunnen volgen op de dashboardpagina van uw Azure-portal.

5. Klik op **Maken** om te beginnen met het aanmaakproces.

6. Klik rechtsboven in de werkbalk van de Azure-portal op **Meldingen** (pictogram van een klok) om het implementatieproces te bewaken. 

   De melding luidt 'De implementatie wordt uitgevoerd...'. De status wordt gewijzigd in 'Implementatie is voltooid' wanneer de implementatie is uitgevoerd. De pagina Machine Learning Experimenten-account wordt geopend als de implementatie is geslaagd.
   
   ![Meldingen in Azure-portal](media/quickstart-installation/portal-notification.png)

Afhankelijk van het besturingssysteem dat u op uw lokale computer gebruikt, volgt u nu de aanwijzingen in een van de volgende twee secties om Azure Machine Learning Workbench op uw computer te installeren. 

## <a name="install-azure-machine-learning-workbench-on-windows"></a>Azure Machine Learning Workbench installeren op Windows
Installeer de Azure Machine Learning Workbench op uw computer met Windows 10, Windows Server 2016 of later.

1. Download het nieuwste installatieprogramma van Azure Machine Learning Workbench, **[AmlWorkbenchSetup.msi](https://aka.ms/azureml-wb-msi)**.

2. Dubbelklik in de Verkenner op het gedownloade installatieprogramma _AmlWorkbenchSetup.msi_.

   >[!IMPORTANT]
   >Download het installatieprogramma volledig op schijf en voer het vanaf daar uit. Start het niet rechtstreeks vanuit de downloadwidget van uw browser.

3. Voltooi de installatie door de instructies op het scherm te volgen.

   Het installatieprogramma downloadt alle vereiste afhankelijke onderdelen, zoals Python, Miniconda en andere gerelateerde bibliotheken. Het kan ongeveer een half uur duren voordat het installeren van alle onderdelen is voltooid. 

4. Azure Machine Learning Workbench is nu geïnstalleerd in de volgende map:
   
   `C:\Users\<user>\AppData\Local\AmlWorkbench`

## <a name="install-azure-machine-learning-workbench-on-macos"></a>Azure Machine Learning Workbench installeren op macOS
Installeer de Azure Machine Learning Workbench op uw computer met macOS Sierra.

1. Installeer de openssl-bibliotheek met [Homebrew](http://brew.sh). Zie [Prerequisite for .NET Core on Mac](https://docs.microsoft.com/dotnet/core/macos-prerequisites) (Vereisten voor .NET Core op de Mac) voor meer informatie.
   ```
   # install Homebrew first if you don't have it already
   /usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"

   # install latest openssl needed for .NET Core 1.x
   brew update
   brew install openssl
   mkdir -p /usr/local/lib
   ln -s /usr/local/opt/openssl/lib/libcrypto.1.0.0.dylib /usr/local/lib/
   ln -s /usr/local/opt/openssl/lib/libssl.1.0.0.dylib /usr/local/lib/
   ```

2. Download het nieuwste installatieprogramma van Azure Machine Learning Workbench, **[AmlWorkbenchSetup.dmg](https://aka.ms/azureml-wb-dmg)**.

   >[!IMPORTANT]
   >Download het installatieprogramma volledig op schijf en voer het vanaf daar uit. Start het niet rechtstreeks vanuit de downloadwidget van uw browser.

3. Dubbelklik op het gedownloade installatieprogramma _AmlWorkbench.dmg_ vanuit de Finder.

4. Voltooi de installatie door de instructies op het scherm te volgen.

   Het installatieprogramma downloadt alle vereiste afhankelijke onderdelen, zoals Python, Miniconda en andere gerelateerde bibliotheken. Het kan ongeveer een half uur duren voordat het installeren van alle onderdelen is voltooid. 

5. Azure Machine Learning Workbench is nu geïnstalleerd in de volgende map: 

   _/Applications/AmlWorkbench.app_

## <a name="run-azure-machine-learning-workbench-to-log-in-the-first-time"></a>Voer Azure Machine Learning Workbench om u de eerste keer aan te melden
1. Klik op de knop  **Workbench starten** op het laatste scherm van het installatieprogramma wanneer het installatieproces voltooid is. Als u het installatieprogramma hebt gesloten, zoekt u de snelkoppeling naar de Machine Learning Workbench op het bureaublad of in het menu Start naar de naam **Azure Machine Learning Workbench** om de app te starten.

2. Meld u aan bij de Workbench met hetzelfde account dat u eerder hebt gebruikt voor het inrichten van uw Azure-resources. 

3. Wanneer de aanmelding is geslaagd, zoekt de Workbench naar de Machine Learning Experimenten-accounts die u eerder hebt gemaakt. Alle Azure-abonnementen waartoe u toegang hebt met uw referenties, worden doorzocht. Wanneer er ten minste één Experimenten-account wordt gevonden, wordt de Workbench geopend met dat account. Vervolgens worden de werkruimten en projecten in het account vermeld. 

   >[!TIP]
   > Als u toegang hebt tot meer dan één Experimenten-Account hebt, kunt u overschakelen naar een ander account door op het avatar-pictogram in de linkerbenedenhoek van de Workbench-app te klikken.

Zie [Deployment Environment Setup](deployment-setup-configuration.md) (Implementatieomgeving instellen) voor het maken van een omgeving voor het implementeren van uw webservices.

## <a name="create-a-new-project"></a>Een nieuw project maken
1. Start de Azure ML Workbench-app en meld u aan. 

2. Klik op **Bestand** --> **Nieuw project** (of klik op **+** in het deelvenster **PROJECTEN**). 

3. Vul de velden **Projectnaam** en **Projectmap** in. De **Projectbeschrijving** is optioneel, maar wel nuttig. Laat het veld **Visualstudio.com GIT Repository URL** voorlopig leeg. Kies een werkruimte en selecteer **Classifying Iris** (Iris classificeren) als de projectsjabloon.

   >[!TIP]
   >U kunt het tekstveld voor de Git-repo desgewenst ook invullen met de URL van een Git-repo die wordt gehost in een [VSTS](https://www.visualstudio.com)-project (Visual Studio Team Service). Deze Git-repo moet al bestaan en moet leeg zijn, zonder master branch. U moet ook schrijftoegang hebben tot de repo. Door nu een Git-repo toe te voegen, kunt u later Roaming and Sharing-scenario's inschakelen. [Meer informatie](using-git-ml-project.md).

4. Klik op de knop **Maken** om het project te maken. Er wordt een nieuw project voor u gemaakt en geopend. U kunt nu de introductiepagina van het project, gegevensbronnen, notitieblokken en broncodebestanden verkennen. 

    >[!TIP]
    >U kunt ook het project ook openen in VS Code of een andere editor door gewoon een koppeling naar een IDE (Integrated Development Environment) te configureren en de projectmap daarin te openen. [Meer informatie](how-to-configure-your-IDE.md). 

## <a name="run-a-python-script"></a>Een Python-script uitvoeren
We gaan een script uitvoeren op uw lokale computer. 

1. Elk project wordt geopend in een eigen **Projectdashboard**-pagina. Selecteer `local` als doel voor de uitvoering vanuit de opdrachtbalk aan de bovenkant van de toepassing, links van de knop Uitvoeren, en `iris_sklearn.py` als het uit te voeren script.  Er zijn een nog enkele andere bestanden opgenomen in het voorbeeld, die u later kunt bekijken. 

   ![img](media/quickstart-installation/run_control.png)

2. Voer in het tekstveld **Argumenten** `0.01` in. Dit getal wordt in de code gebruikt om de regularisatiesnelheid in te stellen, een waarde die wordt gebruikt om te configureren hoe het lineaire-regressiemodel wordt getraind. 

3. Klik op de knop **Uitvoeren** om te beginnen met het uitvoeren van `iris_sklearn.py` op uw computer. 

   Deze code gebruikt het [logistic regression](https://en.wikipedia.org/wiki/logistic_regression)-algoritme van de populaire Python [scikit learn](http://scikit-learn.org/stable/index.html)-bibliotheek om het model op te bouwen.

4. Het deelvenster **Taken** schuift van rechts naar buiten als het nog niet zichtbaar is en er wordt een `iris_sklearn`-taak toegevoegd in het deelvenster. De status verandert van **Indienen** in **Uitvoeren** als de taak wordt gestart en vervolgens binnen een paar seconden in **Voltooid**. 

5. Gefeliciteerd! U hebt een Python-script uitgevoerd in Azure ML-Workbench.

6. Herhaal stappen 2-4 verschillende keren. Gebruik elke keer verschillende argumentwaarden tussen `10` en `0.001`.

## <a name="view-run-history"></a>Uitvoeringsgeschiedenis weergeven
1. Navigeer naar de weergave **Uitvoeringen** en klik op **iris_sklearn.py** de lijst met uitvoeringen. Het uitvoeringsgeschiedenisdashboard voor `iris_sklearn.py` wordt geopend. Alle uitvoeringen op `iris_sklearn.py` worden weergegeven. 

   ![img](media/quickstart-installation/run_view.png)

2. In het uitvoeringsgeschiedenisdashboard worden ook de belangrijkste metrische gegevens, een set standaardgrafieken en een lijst met metrische gegevens voor elke uitvoering weergegeven. U kunt deze weergave aanpassen door de configuraties te sorteren, te filteren en aan te passen door op het configuratiepictogram of het filterpictogram te klikken.

   ![img](media/quickstart-installation/run_dashboard.png)

3. Klik op een voltooide uitvoering om een gedetailleerde weergave te zien van die specifieke uitvoering, met aanvullende metrische gegevens, de bestanden die bij de uitvoering zijn geproduceerd en andere logboeken die mogelijk nuttig zijn.

## <a name="next-steps"></a>Volgende stappen
U hebt nu een Azure Machine Learning Experimenten-account en een Azure Machine Learning Modelbeheer-account gemaakt. U hebt de bureaublad-app en de opdrachtregelinterface van Azure Machine Learning Workbench geïnstalleerd. U hebt een nieuw project gemaakt, een model gemaakt door een script uit te voeren, en de uitvoeringsgeschiedenis van het script verkend.

Voor een uitgebreidere ervaring met deze werkstroom, met inbegrip van het implementeren van uw Iris-model als een webservice, volgt u de volledige zelfstudie Classifying Iris (Iris classificeren) die gedetailleerde stappen bevat voor het [gegevensvoorbereiding](tutorial-classifying-iris-part-1.md), [experimenteren](tutorial-classifying-iris-part-2.md) en [modelbeheer](tutorial-classifying-iris-part-3.md). 

> [!div class="nextstepaction"]
> [Zelfstudie Iris classificeren](tutorial-classifying-iris-part-1.md)

