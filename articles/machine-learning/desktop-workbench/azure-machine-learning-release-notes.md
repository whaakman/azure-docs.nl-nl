---
title: Wat is er nieuw in Azure Machine Learning
description: In dit document worden de updates voor Azure Machine Learning.
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.service: machine-learning
ms.workload: data-services
ms.topic: reference
ms.date: 03/28/2018
ms.openlocfilehash: 1eb4e7859ae62fca4e4a11f69abda3f767ab9596
ms.sourcegitcommit: e221d1a2e0fb245610a6dd886e7e74c362f06467
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/07/2018
---
# <a name="whats-new-in-azure-machine-learning"></a>Wat is nieuw in Azure Machine Learning?

In dit artikel meer informatie over de nieuwe releases van [Azure Machine Learning](../service/overview-what-is-azure-ml.md). 


## <a name="2018-05-sprint-5"></a>2018-05 (sprint 5)

Met deze versie van Azure Machine Learning, kunt u het volgende doen:
+ Featurize afbeeldingen met een quantized versie van ResNet 50, een classificatie op basis van deze functies, trainen en [model implementeren op een FPGA op Azure](../service/how-to-deploy-fpga-web-service.md) voor inferencing zeer lage latentie.

+ Snel bouwen en implementeren van maximaal nauwkeurige machine learning en grondige learning-modellen met [aangepaste Azure Machine Learning-pakketten](../service/reference-python-package-overview.md) voor de volgende gebieden:
  + [Computer vision](../service/how-to-build-deploy-image-classification-models.md)
  + [Tekstanalyse](../service/how-to-build-deploy-text-classification-models.md)
  + [Prognose](../service/how-to-build-deploy-forecast-models.md)

## <a name="2018-03-sprint-4"></a>2018-03 (sprint 4)
**Versienummer**: 0.1.1801.24353 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([uw versie vinden](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))


Veel van de volgende updates worden gedaan als directe resultaten van uw feedback. Zorg ervoor dat ze afkomstig zijn.

**Belangrijke nieuwe functies en wijzigingen**

- Ondersteuning voor het uitvoeren van scripts op externe Ubuntu VM's op uw eigen omgeving naast extern docker systeemeigen gebaseerd worden uitgevoerd.
- Nieuwe omgeving ervaring in de Workbench-App kunt u compute-doelen maken en uitvoeren van de configuraties naast onze ervaring op basis van CLI.
![Tabblad omgevingen](media/azure-machine-learning-release-notes/environment-page.png)
- Geschiedenis van aanpasbare uitvoeren rapporten ![afbeelding van nieuwe uitvoeren Geschiedenisrapporten](media/azure-machine-learning-release-notes/new-run-history-reports.png)

**Gedetailleerde Updates**

Hier volgt een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

### <a name="workbench-ui"></a>Workbench gebruikersinterface
- Aanpasbare rapporten van de geschiedenis uitvoeren
  - Configuratie van de verbeterde grafiek voor rapporten van de geschiedenis uitvoeren
    - De gebruikte toegangspunten kan worden gewijzigd.
    - Op het hoogste niveau filters kunnen worden toegevoegd en gewijzigd ![Filters toevoegen](media/azure-machine-learning-release-notes/add-filters.jpg)
    - Grafieken en statistieken kunnen worden toegevoegd of gewijzigd (en slepen en neerzetten gerangschikt).
    ![Nieuwe grafieken](media/azure-machine-learning-release-notes/configure-charts.png)

  - CRUD voor rapporten van de geschiedenis uitvoeren
  - Alle bestaande uitvoeringsgeschiedenis lijstweergave configuraties aan serverzijde rapporten, die fungeert als pijplijnen op wordt uitgevoerd vanuit de geselecteerde toegangspunten verplaatst.

- Tabblad omgevingen
  - Nieuwe compute-doel toevoegen en voer configuratiebestanden aan uw project eenvoudig ![nieuw Compute-doel](media/azure-machine-learning-release-notes/add-new-environments.png)
  - Beheren en de configuratiebestanden die met behulp van een eenvoudige, op basis van formulieren UX bijwerken
  - Nieuwe knop voor het voorbereiden van uw omgeving voor uitvoering

- Verbeterde prestaties aan de lijst van bestanden in de zijbalk

### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Azure Machine Learning Workbench kunt u nu om te zoeken naar een kolom met behulp van een bekende kolomnaam.


### <a name="experimentation"></a>Experiment
- Azure Machine Learning Workbench ondersteunt nu uw scripts systeemeigen uitgevoerd op uw eigen omgeving python of pyspark. Voor deze mogelijkheid gebruiker maakt en beheert hun eigen omgeving op de externe virtuele machine en gebruiken van Azure Machine Learning Workbench hun scripts uitvoeren op die zijn gericht. Zie [configureren van Azure Machine Learning experimenteren Service](experimentation-service-configuration.md) 

### <a name="model-management"></a>Modelbeheer
- Ondersteuning voor het aanpassen van de Containers geïmplementeerd: kan de container-afbeelding aanpassen zodat de installatie van externe bibliotheken met apt get, enzovoort. Het is niet meer beperkt tot pip installeerbare bibliotheken. Zie de [documentatie](model-management-custom-container.md) voor meer informatie.
  - Gebruik de `--docker-file myDockerStepsFilename` vlag en de bestandsnaam de naam met het manifest, image of opdrachten voor het maken van service.
  - Houd er rekening mee dat de basisinstallatiekopie Ubuntu is en kan niet worden gewijzigd.
  - Voorbeeld: 
  
      ```shell
      $ az ml image create -n myimage -m mymodel.pkl -f score.py --docker-file mydockerstepsfile
      ```



## <a name="2018-01-sprint-3"></a>2018-01 (sprint 3) 
**Versienummer**: 0.1.1712.18263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([uw versie vinden](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Hier volgen de updates en verbeteringen in deze sprint. Veel van deze updates worden gedaan als direct resultaat van feedback van gebruikers. 


Hier volgt een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

- Updates voor de verificatie-stack zorgt ervoor dat de selectie en -account voor aanmelding bij het opstarten

### <a name="workbench"></a>Workbench
- Mogelijkheid om te installeren/verwijderen van de app vanuit programma's toevoegen/verwijderen
- Updates voor de verificatie-stack zorgt ervoor dat de selectie en -account voor aanmelding bij het opstarten
- Verbeterde ervaring voor eenmalige aanmelding op (SSO) in Windows
- Gebruikers die deel uitmaken van meerdere tenants met andere referenties zich nu aanmelden bij de Workbench

### <a name="ui"></a>GEBRUIKERSINTERFACE
- Algemene verbeteringen en oplossingen voor problemen

### <a name="notebooks"></a>Laptops
- Algemene verbeteringen en oplossingen voor problemen

### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Verbeterde automatische suggesties tijdens het uitvoeren van transformaties door voorbeeld
- Verbeterde algoritme voor het patroon frequentie inspector
- Voorbeeldgegevens en feedback verzenden tijdens het uitvoeren van de voorbeeld-transformaties ![afbeelding van de koppeling van de feedback verzenden op afgeleid kolom transformatie](media/azure-machine-learning-release-notes/SendFeedbackFromDeriveColumn.png)
- Spark Runtime-verbeteringen
- Scala is Pyspark vervangen
- Vaste onvermogen om de gegevens niet van toepassing sluiten voor de Time Series-Inspector 
- De tijd van de loopt vast voor de uitvoering van de gegevens Prep voor HDI opgelost

### <a name="model-management-cli-updates"></a>Model Management CLI-updates 
  - Eigendom van het abonnement is niet meer vereist voor het inrichten van resources. Inzender aan de resourcegroep worden voor toegang tot voldoende zijn voor het instellen van de implementatieomgeving.
  - Ingeschakelde lokale omgeving gratis abonnementen instellen 

## <a name="2017-12-sprint-2-qfe"></a>2017-12 (sprint 2 QFE) 
**Versienummer**: 0.1.1711.15323 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([uw versie vinden](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Dit is de release QFE (Quick Fix Engineering) van een secundaire versie. Deze worden verschillende telemetrie problemen en helpt het productteam om beter te begrijpen hoe het product wordt gebruikt. De kennis kunt in toekomstige inspanningen voor het verbeteren van de productervaring gaan. 

Bovendien zijn er twee belangrijke updates:

- Een bug vast gegevens prep die voorkomen dat de time series-inspector weergeven in voorbereiding gegevenspakketten.
- In het opdrachtregelprogramma hoeft niet langer te worden van een Azure-abonnement eigenaar voor het inrichten van Machine Learning Compute ACS-clusters. 

## <a name="2017-12-sprint-2"></a>2017-12 (sprint 2)
**Versienummer**: 0.1.1711.15263 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([uw versie vinden](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Welkom bij de derde Azure Machine Learning-update. Deze update bevat verbeteringen in de workbench-app, de opdrachtregelinterface (CLI) en de back-end-services. Hartelijk dank voor het verzenden van Messenger en frowns. Veel van de volgende updates worden gedaan als directe resultaten van uw feedback. 

**Belangrijke nieuwe functies**
- [Ondersteuning voor SQL Server en Azure SQL-database als een gegevensbron](data-prep-appendix2-supported-data-sources.md#types) 
- [Grondige kennis op Spark met MMLSpark met GPU-ondersteuning](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- [Alle AML containers zijn compatibel met Azure IoT randapparaten wanneer geïmplementeerd (geen extra stappen vereist)](http://aka.ms/aml-iot-edge-blog)
- Geregistreerd model lijst en details weergaven beschikbaar Azure-portal
- Toegang tot compute-doelen met SSH-sleutel gebaseerde verificatie naast gebruikersnaam/wachtwoord gebaseerde toegang. 
- Nieuwe patroon frequentie Inspector in de gegevens voorbereiden ervaring. 

**Gedetailleerde Updates** Hieronder volgt een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

### <a name="installer"></a>Installatieprogramma
- Installatieprogramma kan self update zodat die fouten oplossingen en nieuwe functies kunnen worden ondersteund zonder dat gebruiker opnieuw te installeren

### <a name="workbench-authentication"></a>Workbench verificatie
- Meerdere oplossingen voor verificatiesysteem. Laat ons weten als u nog steeds problemen met aanmelden ondervinden.
- UI-wijzigingen die makkelijker te vinden de Manager van de Proxy-instellingen.

### <a name="workbench"></a>Workbench
- Weergave van alleen-lezen bestand heeft nu lichte blauwe achtergrond
- Verplaatste knop Bewerken rechts detecteerbaar meer.
- 'dsource', 'dprep' en 'ipynb' bestandsindelingen kunnen nu worden gerenderd onbewerkte tekst zonder opmaak
- De workbench heeft nu een nieuwe bewerken ervaring dat de gebruiker naar het gebruik van externe IDE voor scripts bewerken en Workbench alleen gebruiken voor het bewerken van de bestandstypen die u een rijke bewerken-ervaring (zoals laptops, gegevensbronnen, voorbereiding gegevenspakketten hebt)
- Bij het laden van de lijst met werkruimten en projecten waaraan de gebruiker toegang tot heeft is aanzienlijk sneller nu

### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Een patroon frequentie Inspector om weer te geven van de tekenreeks patronen in een kolom. U kunt ook uw gegevens met behulp van deze patronen filteren. Hiermee geeft u een weergave vergelijkbaar met de waarde telt inspector. Het verschil is dat patroon frequentie het aantal van de unieke patronen van de gegevens in plaats van het aantal unieke gegevens toont. U kunt ook filteren in- of alle rijen die voldoen aan een bepaald patroon.

![Afbeelding van patroon frequentie inspector op nummer Product](media/azure-machine-learning-release-notes/pattern-inspector-product-number.png)

- Verbeterde prestaties bij het aanbevelen van rand aanvragen om te bekijken in de transformatie 'kolom afgeleid door voorbeeld'

- [Ondersteuning voor SQL Server en Azure SQL-database als een gegevensbron](data-prep-appendix2-supported-data-sources.md#types) 

![Afbeelding van het maken van een nieuwe SQL server-gegevensbron](media/azure-machine-learning-release-notes/sql-server-data-source.png)

- 'In een oogopslag'-weergave van het aantal rijen en kolommen ingeschakeld

![Afbeelding van de kolomaantal rijen in een respijtperiode](media/azure-machine-learning-release-notes/row-col-count.png)

- Gegevens prep is ingeschakeld in alle compute-contexten
- Gegevensbronnen die gebruikmaken van een SQL Server-database zijn ingeschakeld in alle compute-contexten
- Gegevens voorbereiden raster kolommen kunnen worden gefilterd door het gegevenstype
- Opgelost probleem met het converteren van meerdere kolommen tot datum
- Probleem opgelost die gebruiker kan uitvoerkolom selecteren als een bron in kolom afgeleid door voorbeeld als gebruiker gewijzigd uitvoerkolomnaam in de geavanceerde modus.

### <a name="job-execution"></a>Uitvoeren van taak
U kunt nu maken en toegang tot een remotedocker of cluster compute-doel SSH-sleutel verificatie op basis van deze stappen te volgen:
- Een compute-doel met de volgende opdracht in de CLI koppelen

    ```azure-cli
    $ az ml computetarget attach remotedocker --name "remotevm" --address "remotevm_IP_address" --username "sshuser" --use-azureml-ssh-key
    ```
>[!NOTE]
>-k optie (of---azureml-ssh-sleutel gebruiken) in de opdracht geeft om te genereren en gebruiken van SSH-sleutel.

- Azure ML-Workbench gaat genereren van een openbare sleutel en uitvoer die in de console. Meld u aan bij de compute-doelserver met behulp van de dezelfde gebruikersnaam en ~/.ssh/authorized_keys toevoegen aan deze openbare sleutel.

- U kunt dit doel compute voorbereiden en deze gebruiken voor uitvoering en Azure ML-Workbench gebruikt deze sleutel voor verificatie.  

Zie voor meer informatie over het maken van de compute-doelen [configureren van Azure Machine Learning experimenteren Service](experimentation-service-configuration.md)

### <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools voor AI
- Ondersteuning toegevoegd voor [Visual Studio Tools for AI](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.vstoolsai-vs2017). 

### <a name="command-line-interface-cli"></a>Opdrachtregelinterface (CLI)
- Toegevoegd `az ml datasource create` kan naar het bestand voor een gegevensbron maken vanaf de opdrachtregel

### <a name="model-management-and-operationalization"></a>Model Management en uitoefening
- [Alle AML containers zijn compatibel met Azure IoT randapparaten wanneer geoperationaliseerd (geen extra stappen vereist)](http://aka.ms/aml-iot-edge-blog) 
- Verbeteringen van foutberichten in de CLI o16n
- Oplossingen voor problemen in het beheerportal model UX  
- Consistente letter hoofdlettergebruik voor model management kenmerken in de detailpagina
- Score berekenen aanroepen time-out realtime ingesteld op 60 seconden
- Geregistreerd model lijst en details weergaven die beschikbaar zijn in de Azure portal

![details van het model in de portal](media/azure-machine-learning-release-notes/model-list.jpg)

![overzicht van het model in de portal](media/azure-machine-learning-release-notes/model-overview-portal.jpg)

### <a name="mmlspark"></a>MMLSpark
- Deep Learning in Spark met [GPU-ondersteuning](https://github.com/Azure/mmlspark/blob/master/docs/gpu-setup.md)
- Ondersteuning voor Resource Manager-sjablonen voor eenvoudig resources implementeren
- Ondersteuning voor het ecosysteem SparklyR
- [AZTK-integratie](https://github.com/Azure/aztk/wiki/Spark-on-Azure-for-Python-Users#optional-set-up-mmlspark)

### <a name="sample-projects"></a>Voorbeeldprojecten
- [IRIS](https://github.com/Azure/MachineLearningSamples-Iris) en [MMLSpark](https://github.com/Azure/mmlspark) voorbeelden bijgewerkt met de nieuwe versie van Azure ML-SDK

### <a name="breaking-changes"></a>Wijzigingen op te splitsen
- Gepromoveerd de `--type` in switch `az ml computetarget attach` naar een subopdracht. 

    - `az ml computetarget attach --type remotedocker` is nu `az ml computetarget attach remotedocker`
    - `az ml computetarget attach --type cluster` is nu `az ml computetarget attach cluster`

## <a name="2017-11-sprint-1"></a>2017-11 (sprint 1) 
**Versienummer**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([uw versie vinden](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

We hebben verbeteringen rond de beveiliging, stabiliteit en onderhoud in de workbench-app, de CLI en de back-end-services-laag aangebracht in deze release. Hartelijk dank voor het verzenden van Messenger en frowns. Veel van de onderstaande updates zijn aangebracht tijdens het direct resultaten van uw feedback. Houden afkomstig!

### <a name="notable-new-features"></a>Belangrijke nieuwe functies
- Azure ML is nu beschikbaar in twee nieuwe Azure-regio's: **West-Europa** en **Zuidoost-Azië**. Die lid worden van de vorige gebieden van **VS-Oost 2**, **West-Centraal VS**, en **Australië-Oost**, regio's tot vijf brengen van het totale aantal geïmplementeerd.
- We Python code syntaxismarkering in de Workbench-app om het gemakkelijker te lezen en bewerken van broncode Python ingeschakeld. 
- U kunt nu uw favoriete IDE rechtstreeks vanuit een bestand in plaats van vanaf het hele project starten.  Een bestand openen in de Workbench en vervolgens te klikken op 'Bewerken' Start uw IDE (momenteel tegenover Code en PyCharm worden ondersteund) naar het huidige bestand en het project.  U kunt ook klikken op de pijl naast de knop bewerken om te bewerken van het bestand in de teksteditor Workbench.  Bestanden zijn alleen-lezen, totdat u klikt u op bewerken, zo wordt voorkomen dat per ongeluk worden gewijzigd.
- De populaire tekengebied bibliotheek `matplotlib` versie 2.1.0 wordt nu geleverd bij de Workbench-app.
- We de versie van .NET Core bijgewerkt naar 2.0 voor de prep-engine. Deze verwijderd de vereiste voor openssl brew installeren tijdens de installatie van de app op Mac OS. Deze ook de weg wordt vrijgemaakt voor meer informatie vinden over interessante gegevens prep functies die u in de nabije toekomst worden geleverd. 
- We hebben een startpagina versiespecifiek app ingeschakeld zodat u meer relevante releaseopmerkingen ophalen en wordt u gevraagd op basis van uw huidige versie van de app bijwerken.
- Als de naam van uw lokale gebruiker een spatie erin heeft, kan de toepassing nu worden geïnstalleerd. 

### <a name="detailed-updates"></a>Gedetailleerde Updates
Hieronder ziet u een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

#### <a name="installer"></a>Installatieprogramma
- App-installatieprogramma ruimt nu naar de installatiemap gemaakt door een oudere versie van de app.
- Een fout die leidt installatieprogramma op 100% op Mac OS hoge Sierra te zitten vast.
- Er is nu een directe koppeling naar de map installatieprogramma voor de gebruiker om te controleren installatieprogramma Logboeken als de installatie is mislukt.
- Installeer werkt nu voor gebruikers die ruimte in hun gebruikersnaam hebben.

#### <a name="workbench-authentication"></a>Workbench verificatie
- Ondersteuning voor verificatie in de Proxy-Manager.
- Logboekregistratie in nu is geslaagd als de gebruiker zich achter een firewall. 
- Als de gebruiker heeft experimenteren accounts in meerdere Azure-regio's, en als één regio gebeurt niet beschikbaar is, wordt de app niet meer loopt vast.
- Wanneer verificatie is niet voltooid en het verificatiedialoogvenster nog steeds zichtbaar is, wordt de app niet langer probeert werkruimte laden vanuit de lokale cache.

#### <a name="workbench-app"></a>Workbench App
- Syntaxismarkering voor Python-code is ingeschakeld in de teksteditor.
- De knop bewerken in de teksteditor kunt u het bestand bewerken in een IDE (VS Code en PyCharm worden ondersteund) of in de ingebouwde teksteditor.
- Er is een teksteditor in de modus alleen-lezen standaard. 
- Knop visuele status nu wijzigingen opslaan uitgeschakeld nadat het huidige bestand opgeslagen en daarom niet meer gewijzigd is.
- Hiermee slaat u Workbench _alle_ -bestanden opgeslagen wanneer u een run initieert.
- Workbench onthoudt dat laatst gebruikte werkruimte op de lokale computer, zodat deze automatisch wordt geopend.
- Nu slechts één exemplaar van Workbench mag worden uitgevoerd. Eerder kunnen meerdere exemplaren worden gestart die problemen veroorzaakt als het wordt uitgevoerd op hetzelfde project.
- Nieuwe naam bestandsmenu 'Open Project...' aan 'Toevoegt bestaande map als Project...' 
- Tabblad overschakelen is nu veel sneller.
- Help-koppelingen worden toegevoegd aan het dialoogvenster IDE configureren.
- Het feedbackformulier onthoudt nu het e-mailadres dat u de laatste keer hebt.
- Messenger- en frowns wordt tekst nu groter, zodat u kunt meer feedback sturen! 
- De `--owner` help-tekst in te schakelen `az ml workspace create` is opgelost.
- We hebben een 'Over' in het dialoogvenster zodat gebruikers eenvoudig bekijken en kopiëren van het versienummer van de app toegevoegd.
- Een 'Een functie voorgesteld' menu-item is toegevoegd aan het menu Help.
- Accountnaam experimenteren is nu zichtbaar in de app titelbalk, voorafgaand aan de naam van de app 'Azure Machine Learning Workbench'.
- De startpagina van een versiespecifiek app weergegeven nu op basis van de versie van de app gedetecteerd.

#### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Externe website kunnen niet meer worden geladen vanuit de kaart Inspector om te voorkomen dat mogelijke beveiligingsproblemen.
- Histogram en aantal inspectors heeft nu de optie voor het weergeven van de grafiek in logaritmische schaal.
- Wanneer een berekening wordt uitgevoerd, kwaliteit gegevensbalk nu ziet u een andere kleur aangegeven van de status 'berekenen'.
- Kolom metrische gegevens worden nu statistieken voor categorische waarde kolommen weergeven.
- Het laatste teken in naam van de gegevensbron niet meer wordt afgebroken.
- Prep gegevenspakket blijft nu geopend wanneer u overschakelt van tabbladen, waardoor de merkbare prestaties verbeteren.
- In de gegevensbron bij het schakelen tussen de gegevensweergave en metrische gegevens weergeven, verandert de volgorde van kolommen nu meer.
- Openen van een ongeldig `.dprep` of `.dsource` bestand wordt niet langer Workbench vastlopen.
- Prep gegevenspakket kunnen nu relatief pad gebruikt voor uitvoer in _schrijven naar CSV_ transformeren.
- _Houd kolom_ transformatie staat nu de gebruiker toe te voegen extra kolommen als bewerkt.
- _Vervang deze_ menu nu daadwerkelijk start _waarde vervangen_ in het dialoogvenster.
- _Vervang de waarde_ transformeren nu functies zoals verwacht in plaats van de fout ArgumentOutOfRangeException.
- Prep gegevenspakket gebruikt nu absoluut pad op bij verwijzingen naar gegevensbestanden buiten de projectmap, waardoor het mogelijk om uit te voeren van het pakket in de lokale context met het absolute pad naar het gegevensbestand.
- _Volledige_ als een strategie voor steekproeven wordt nu ondersteund bij gebruik van Azure blob als gegevensbron.
- Python-code (van prep gegevenspakket) gegenereerd zijn nu voor zowel CR als LF, zodat u beschrijvende in Windows.
- _Kies de metrische gegevens_ dropdown eigenschap nu wordt verborgen wanneer u overschakelt naar de gegevensweergave.
- Workbench kan zich nu proces parketvloeren bestanden zelfs bij het gebruik van Python-runtime. Eerder kan alleen Spark worden gebruikt bij het verwerken van parketvloeren bestanden. 
- Filteren van waarden in een kolom met _datum_ gegevenstype wordt niet meer gegevens prep engine vastloopt.
- Metriekweergave respecteert nu steekproeven strategie voor updates.
- Externe taken nu steekproef werkt goed.

#### <a name="job-execution"></a>Uitvoeren van taak
- Argument is nu opgenomen in de uitvoeringsgeschiedenis record.
- Taken gestarte CLI nu weergegeven in deelvenster voor historie taak uitvoeren automatisch.
- Deelvenster van de taak wordt nu opdrachten aangemaakt door gastgebruikers toegevoegd aan de Azure AD-tenant.
- Deelvenster taak annuleren en verwijderingsacties stabieler zijn.
- Wanneer op de knop uitvoeren klikt, wordt nu foutbericht geactiveerd als de configuratiebestanden in een onjuiste notatie.
- Taken in de CLI gestarte verstoort afsluitende app niet meer.
- Taken gestarte CLI nu blijft spit uit de standard out zelfs na een uur van de uitvoering.
- Betere foutberichten worden weergegeven wanneer de prep gegevenspakket uitvoeren in Python/PySpark mislukt.
- `az ml experiment clean` ruimt nu Docker-afbeeldingen in ook externe VM.
- `az ml experiment clean` nu werkt goed voor lokale doel op Mac OS.
- Foutberichten wanneer die gericht is op lokale of externe Docker wordt uitgevoerd, worden opgeschoond omhoog en gemakkelijker te lezen.
- Betere foutbericht wordt weergegeven wanneer de naam van het hoofdknooppunt voor HDInsight-cluster niet correct opgemaakt is als gekoppeld als een doel kan worden uitgevoerd.
- Betere foutbericht wordt weergegeven wanneer het geheim is niet gevonden in de referentie-service. 
- MMLSpark-bibliotheek is voor ondersteuning van Apache Spark 2.2 bijgewerkt.
- MMLSpark omvatten nu een onderwerp codering transformatie (Mesh codering) voor medische documenten.
- `matplotlib` versie 2.1.0 is nu verzonden out van het vak met Workbench.

#### <a name="jupyter-notebook"></a>Jupyter Notebook
- Zoekactie voor namen notebook nu werkt goed in de weergave laptops.
- U kunt nu een laptop in de weergave laptops verwijderen.
- Nieuwe magic `%upload_artifact` wordt toegevoegd voor het uploaden van bestanden die in de Notebook uitvoeringsomgeving in het gegevensarchief uitvoeringsgeschiedenis wordt geproduceerd.
- Kernelfouten worden nu in de status van de Notebook taak voor het opsporen van eenvoudiger opgehaald.
- Jupyter server nu correct wordt afgesloten wanneer de gebruiker zich aanmeldt bij de app.

#### <a name="azure-portal"></a>Azure Portal
- Account experimenteren en Model-Management-account kunnen nu worden gemaakt in twee nieuwe Azure-regio's: West-Europa en Zuidoost-Azië.
- Model Management account DevTest plan nu is alleen beschikbaar wanneer dit het eerste item in het abonnement worden gemaakt. 
- Help-koppeling in de Azure portal is bijgewerkt om te verwijzen naar de juiste documentatiepagina.
- Beschrijvingsveld wordt verwijderd uit de detailpagina Docker-installatiekopie, omdat deze niet van toepassing.
- Informatie over de instellingen voor AppInsights en automatisch schalen zoals worden toegevoegd aan de pagina met details.
- Model management-pagina wordt nu gerenderd zelfs als cookies van derden zijn uitgeschakeld in de browser. 

#### <a name="operationalization"></a>Uitoefening
- Webservice met 'score' in de naam ervan niet meer is mislukt.
- Gebruiker kan nu met het maken van een implementatieomgeving met alleen Inzender toegang tot een Azure-resourcegroep of het abonnement. Eigenaar toegang tot het hele abonnement is niet meer nodig.
- Uitoefening CLI nu leuk vindt tabblad automatisch aanvullen op Linux.
- Bouw afbeeldingsservice ondersteunt nu installatiekopieën van het gebouw voor Azure IoT services/apparaten.

#### <a name="sample-projects"></a>Voorbeeldprojecten
- [_Classificeren van Iris_ ](./tutorial-classifying-iris-part-1.md) voorbeeldproject:
    - `iris_pyspark.py` is gewijzigd in `iris_spark.py`.
    - `iris_score.py` is gewijzigd in `score_iris.py`.
    - `iris.dprep` en `iris.dsource` bijgewerkt op basis van de meest recente gegevens prep engine-updates.
    - `iris.ipynb` Laptop is gewijzigd om te werken in HDInsight-cluster.
    - Uitvoeringsgeschiedenis is ingeschakeld in `iris.ipynb` Notebook cel.
- [_Geavanceerde Prep van gegevens met behulp van de sharegegevens fiets_ ](./tutorial-bikeshare-dataprep.md) voorbeeldproject 'Foutwaarde verwerken' stap vast.
- [_MMLSpark op volwassenen telling gegevens_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) voorbeeldproject `docker.runconfig` bijgewerkt van JSON naar YAML indeling.
- [_Gedistribueerde Hyperparameter afstemmen_ ](./scenario-distributed-tuning-of-hyperparameters.md) voorbeeldproject`docker.runconfig` bijgewerkt van JSON naar YAML indeling.
- Nieuwe voorbeeldproject [ _installatiekopie classificatie met CNTK_](./scenario-image-classification-using-cntk.md).


## <a name="2017-10-sprint-0"></a>2017 10 (sprint 0) 
**Versienummer**: 0.1.1710.31013 &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;([uw versie vinden](../service/known-issues-and-troubleshooting-guide.md#find-the-workbench-build-number))

Welkom bij de eerste update van Azure Machine Learning Workbench na de initiële openbare preview op de Conferentie Microsoft Ignite 2017. De belangrijkste updates in deze release zijn de betrouwbaarheid en stabilization worden opgelost.  Enkele van de kritieke problemen die we behandeld:

### <a name="new-features"></a>Nieuwe functies
- Mac OS hoge Sierra wordt nu ondersteund.

### <a name="bug-fixes"></a>Oplossingen voor problemen
#### <a name="workbench-experience"></a>Workbench ervaring
- Slepen en neerzetten is een bestand in de Workbench zorgt ervoor dat de Workbench vastloopt.
- Het terminalvenster in VS-Code is geconfigureerd als een IDE voor Workbench worden niet herkend door _az ml_ opdrachten.

#### <a name="workbench-authentication"></a>Workbench verificatie
We een aantal updates voor het verbeteren van verschillende aanmelding en verificatie problemen die worden gerapporteerd aangebracht.
- Verificatievenster houdt verschijnen-up, met name wanneer de verbinding met Internet is niet stabiel is.
- Problemen met verbeterde betrouwbaarheid rond verificatietokens.
- In sommige gevallen wordt verificatievenster tweemaal verschijnt.
- Hoofdvenster Workbench nog steeds weergegeven 'verificatie' bericht wanneer de verificatie is voltooid en het pop-updialoogvenster al gesloten.
- Als er geen verbinding met Internet, verschijnt de verificatiedialoog met een leeg scherm.

#### <a name="data-preparation"></a>Gegevensvoorbereiding 
- Wanneer een specifieke waarde is gefilterd, worden fouten en de ontbrekende waarden ook gefilterd.
- Wijzigen van een strategie voor steekproeven Hiermee verwijdert u de volgende bestaande joinbewerkingen.
- Vervangen van een ontbrekende waarde wordt transformatie geen rekening NaN.
- De typeverwijzing datum genereert uitzondering wanneer null-waarde gedetecteerd.

#### <a name="job-execution"></a>Uitvoeren van taak
- Er is geen duidelijke foutmelding wanneer de taakuitvoering is mislukt voor het uploaden van de projectmap omdat de limiet is overschreden.
- Als de werkmap voor Python-script van de gebruiker wordt gewijzigd, worden de bestanden die zijn geschreven naar uitvoer mappen worden niet bijgehouden. 
- Als het actief Azure-abonnement anders dan het huidige project behoort is, resulteert de verzending van de taak een fout 403.
- Wanneer Docker niet aanwezig is, wordt er is geen duidelijke foutbericht wordt geretourneerd als gebruiker probeert te Docker gebruiken als een doel kan worden uitgevoerd.
- .runconfig bestand wordt niet automatisch opgeslagen wanneer de gebruiker klikt op _uitvoeren_ knop.

#### <a name="jupyter-notebook"></a>Jupyter Notebook
- Laptop-server starten niet als gebruiker met bepaalde typen aanmelding gebruikt.
- Foutberichten voor laptop-server doen niet surface in logboeken zichtbaar voor gebruiker.

#### <a name="azure-portal"></a>Azure Portal
- Het donkere thema van Azure portal zorgt ervoor dat Model-Management-blade een zwart vak wordt weergegeven.

#### <a name="operationalization"></a>Uitoefening
- Hergebruik van een manifest voor het bijwerken van een webservice zorgt ervoor dat een nieuwe Docker-installatiekopie gebouwd met een willekeurige naam.
- Weblogboeken service kunnen niet worden opgehaald uit Kubernetes cluster.
- Misleidend foutbericht wordt afgedrukt wanneer de gebruiker probeert te maken van een Model-Management-account of een ML Compute-account en problemen met machtigingen optreedt.

## <a name="next-steps"></a>Volgende stappen

Lees het overzicht voor [Azure Machine Learning](../service/overview-what-is-azure-ml.md).
