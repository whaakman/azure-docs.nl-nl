---
title: Met de Linux gegevens wetenschappelijke virtuele Machine in Azure voor gegevenswetenschap | Microsoft Docs
description: Het uitvoeren van verschillende algemene gegevens wetenschappelijke taken met de Linux-VM wetenschappelijke van gegevens.
services: machine-learning
documentationcenter: 
author: bradsev
manager: cgronlun
editor: cgronlun
ms.assetid: 34ef0b10-9270-474f-8800-eecb183bbce4
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/10/2017
ms.author: bradsev;paulsh
ms.openlocfilehash: 650b11d66f3ca32266b9842af77c909e125b4e4d
ms.sourcegitcommit: d03907a25fb7f22bec6a33c9c91b877897e96197
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/12/2017
---
# <a name="data-science-with-a-linux-data-science-virtual-machine-on-azure"></a>Met een Linux gegevens wetenschappelijke virtuele Machine in Azure voor gegevenswetenschap
In dit scenario laat zien hoe u verschillende algemene gegevens wetenschappelijke taken met de virtuele Linux-gegevens van wetenschappelijke machine uitvoeren. Linux gegevens wetenschappelijke virtuele Machine (DSVM) is beschikbaar in Azure die vooraf worden geïnstalleerd met een verzameling hulpprogramma's die doorgaans gebruikt voor gegevensanalyse en machine learning op de installatiekopie van een virtuele machine. De belangrijke softwareonderdelen worden gespecificeerd in de [Linux gegevens wetenschap-Machine inrichten](linux-dsvm-intro.md) onderwerp. De VM-installatiekopie kunt eenvoudig aan de slag gegevenswetenschap in minuten, zonder te installeren en configureren van elk van de hulpprogramma's afzonderlijk doen. U kunt eenvoudig opschalen van de virtuele machine, indien nodig en stop de toepassing als deze niet in gebruik. Deze resource wordt dus elastische en betaalbare.

De gegevens wetenschappelijke taken gedemonstreerd in dit scenario de stappen die worden beschreven in de [Team gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/). Dit proces biedt een systematische aanpak voor gegevenswetenschap waarmee teams van gegevenswetenschappers tot effectief samenwerken gedurende de levenscyclus van het bouwen van intelligente toepassingen. Het proces van de wetenschappelijke gegevens biedt ook een iteratieve framework voor gegevenswetenschap die kan worden gevolgd door een persoon.

Analyseren we de [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) gegevensset in dit scenario. Dit is een set van e-mailberichten die zijn gemarkeerd als spam of ham (wat betekent dat ze zijn geen spam), en het bevat ook een aantal statistieken over de inhoud van de e-mailberichten. De statistieken die is opgenomen worden in de volgende maar één sectie beschreven.

## <a name="prerequisites"></a>Vereisten
Voordat u een virtuele Machine voor Linux gegevens wetenschappelijke gebruiken kunt, moet u het volgende hebben:

* Een **Azure-abonnement**. Als u nog geen een, Zie [maken van uw gratis Azure-account vandaag](https://azure.microsoft.com/free/).
* Een [ **gegevenswetenschap Linux VM**](https://azure.microsoft.com/marketplace/partners/microsoft-ads/linux-data-science-vm). Zie voor meer informatie over het inrichten van deze virtuele machine [Linux gegevens wetenschap-Machine inrichten](linux-dsvm-intro.md).
* [X2Go](http://wiki.x2go.org/doku.php) geïnstalleerd op uw computer en een XFCE-sessie hebt geopend. Voor informatie over het installeren en configureren van een **X2Go client**, Zie [installeren en configureren van de client X2Go](linux-dsvm-intro.md#installing-and-configuring-x2go-client).
* Voor een soepeler schuiven ervaring, schakelt u de vlag gfx.xrender.enabled in over: config in virtuele machines FireFox-browser. [Zie meer hier. ](https://www.reddit.com/r/firefox/comments/4nfmvp/ff_47_unbearable_slow_over_remote_x11/). Denk ook na over schakelen *mousewheel.enable_pixel_scrolling* op False. [Hier instructies.](https://support.mozilla.org/en-US/questions/981140)
* Een **AzureML account**. Als u er nog geen hebt, zich aanmelden voor een nieuw wachtwoord op de [AzureML-startpagina](https://studio.azureml.net/). Er is een gratis gebruik laag kunt u aan de slag.

## <a name="download-the-spambase-dataset"></a>De gegevensset spambase downloaden
De [spambase](https://archive.ics.uci.edu/ml/datasets/spambase) gegevensset is een relatief klein set gegevens die alleen 4601 voorbeelden bevat. Dit is een handige grootte moet worden gebruikt wanneer dat sommige van de belangrijkste functies van de VM van de wetenschappelijke gegevens zoals deze blijft de resourcevereisten matige te demonstreren.

> [!NOTE]
> In dit scenario is gemaakt op een D2 v2-formaat Linux gegevens wetenschappelijke virtuele Machine. Deze grootte DSVM is kan afhandelen van de procedures in dit scenario.
>
>

Als u meer opslagruimte nodig hebt, kunt u extra schijven maken en deze koppelt aan uw virtuele machine. Deze schijven gebruik permanente Azure-opslag, zodat hun gegevens bewaard, zelfs wanneer de server vanwege het formaat is ingericht of wordt afgesloten. Volg de instructies in om een schijf toevoegen en koppel deze aan uw virtuele machine, [een schijf toevoegen aan een Linux-VM](../../virtual-machines/linux/add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Deze stappen de Azure-opdrachtregelinterface (Azure CLI), die al is geïnstalleerd op de DSVM gebruiken. Deze procedures is dus mogelijk volledig uit de virtuele machine zelf. Een andere optie voor de opslagruimte vergroten is met [Azure files](../../storage/files/storage-how-to-use-files-linux.md).

Open een terminalvenster en voer deze opdracht voor het downloaden van de gegevens:

    wget http://archive.ics.uci.edu/ml/machine-learning-databases/spambase/spambase.data

Het gedownloade bestand heeft geen koprij, dus we maken van een ander bestand dat u beschikt over een koptekst. Deze opdracht voor het maken van een bestand met de juiste koppen uitvoeren:

    echo 'word_freq_make, word_freq_address, word_freq_all, word_freq_3d,word_freq_our, word_freq_over, word_freq_remove, word_freq_internet,word_freq_order, word_freq_mail, word_freq_receive, word_freq_will,word_freq_people, word_freq_report, word_freq_addresses, word_freq_free,word_freq_business, word_freq_email, word_freq_you, word_freq_credit,word_freq_your, word_freq_font, word_freq_000, word_freq_money,word_freq_hp, word_freq_hpl, word_freq_george, word_freq_650, word_freq_lab,word_freq_labs, word_freq_telnet, word_freq_857, word_freq_data,word_freq_415, word_freq_85, word_freq_technology, word_freq_1999,word_freq_parts, word_freq_pm, word_freq_direct, word_freq_cs, word_freq_meeting,word_freq_original, word_freq_project, word_freq_re, word_freq_edu,word_freq_table, word_freq_conference, char_freq_semicolon, char_freq_leftParen,char_freq_leftBracket, char_freq_exclamation, char_freq_dollar, char_freq_pound, capital_run_length_average,capital_run_length_longest, capital_run_length_total, spam' > headers

Vervolgens samenvoegen van de twee bestanden samen met de opdracht:

    cat spambase.data >> headers
    mv headers spambaseHeaders.data

De gegevensset heeft verschillende typen statistieken op elk e-mailbericht:

* Kolommen, zoals ***word\_freq\_WORD*** geven het percentage woorden in de e-mail die overeenkomen met *WORD*. Bijvoorbeeld, als *word\_freq\_zorg* is 1, wordt 1% van alle woorden in het e-mailbericht zijn *zorg*.
* Kolommen, zoals ***char\_freq\_CHAR*** geven het percentage van alle tekens in de e-mail die waren *CHAR*.
* ***kapitaal\_uitvoeren\_lengte\_langste*** is de langste lengte van een reeks van hoofdletters.
* ***kapitaal\_uitvoeren\_lengte\_gemiddelde*** is de gemiddelde duur van alle reeksen hoofdletters.
* ***kapitaal\_uitvoeren\_lengte\_totale*** is de totale lengte van alle reeksen hoofdletters.
* ***spam*** geeft aan of het e-mailbericht wordt beschouwd als spam of niet (1 = spam, 0 = niet spam).

## <a name="explore-the-dataset-with-microsoft-r-open"></a>De gegevensset met Microsoft R Open verkennen
We gaan de gegevens controleren en voer enkele eenvoudige machine learning-met R. De wetenschappelijke gegevens VM wordt geleverd met [Microsoft R Open](https://mran.revolutionanalytics.com/open/) vooraf zijn geïnstalleerd. De multithreaded math-bibliotheken in deze versie van R bieden betere prestaties dan verschillende single thread-versies. Microsoft R Open biedt ook reproduceerbaarheid met behulp van een momentopname van de opslagplaats van het pakket CRAN.

Als u exemplaren van de codevoorbeelden in dit scenario gebruikt, kloon de **Azure-Machine-Learning--Gegevenswetenschap** opslagplaats met git, dat vooraf is geïnstalleerd op de virtuele machine. Vanaf de opdrachtregel git uitvoeren:

    git clone https://github.com/Azure/Azure-MachineLearning-DataScience.git

Open een terminalvenster en een nieuwe R-sessie starten met de interactieve R-console.

> [!NOTE]
> U kunt ook RStudio gebruiken voor de volgende procedures. Geef voor het installeren van RStudio deze opdracht in een terminal:`./Desktop/DSVM\ tools/installRStudio.sh`
>
>

Als u wilt de gegevens importeren en instellen van de omgeving, uitvoeren:

    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

Samenvattende statistieken over elke kolom bekijken:

    summary(data)

Voor een andere weergave van de gegevens:

    str(data)

Hiermee geeft u het type van elke variabele en de eerste paar waarden in de gegevensset.

De *spam* kolom als een geheel getal is gelezen, maar het is daadwerkelijk een categorische variabele (of factor). Instellen van het type:

    data$spam <- as.factor(data$spam)

Sommige experimentele analyses gebruikt u de [ggplot2](http://ggplot2.org/) inpakken, een populaire grafische-bibliotheek voor R dat al is geïnstalleerd op de virtuele machine. Opmerking van de gegevens van de samenvatting dat eerder is weergegeven, dat we samenvattende statistieken over de frequentie van het teken uitroepteken hebben. Laten we tekenen die frequenties met de volgende opdrachten:

    library(ggplot2)
    ggplot(data) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Aangezien de balk nul, het tekengebied scheeftrekken is, gaan we deze verwijderen uit:

    email_with_exclamation = data[data$char_freq_exclamation > 0, ]
    ggplot(email_with_exclamation) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Er is een niet-triviale dichtheid dan 1, waarin u geïnteresseerd. Bekijk alleen die gegevens in:

    ggplot(data[data$char_freq_exclamation > 1, ]) + geom_histogram(aes(x=char_freq_exclamation), binwidth=0.25)

Vervolgens splitsen door spam tegenover ham:

    ggplot(data[data$char_freq_exclamation > 1, ], aes(x=char_freq_exclamation)) +
    geom_density(lty=3) +
    geom_density(aes(fill=spam, colour=spam), alpha=0.55) +
    xlab("spam") +
    ggtitle("Distribution of spam \nby frequency of !") +
    labs(fill="spam", y="Density")

Deze voorbeelden moeten u vergelijkbare curve van de andere kolommen om te verkennen van de gegevens in deze kunt inschakelen.

## <a name="train-and-test-an-ml-model"></a>Trainen en te testen ML-model
Nu gaan we een aantal machine learning-modellen voor het classificeren van de e-mailberichten in de gegevensset bevat van span of ham trainen. We een decision tree-model en een willekeurige forest-model in deze sectie trainen en vervolgens de juistheid van hun voorspellingen.

> [!NOTE]
> Het pakket rpart (recursieve partitioneren en Regressiestructuren) is gebruikt in de volgende code is al geïnstalleerd op de VM, wetenschappelijke gegevens.
>
>

Eerste, gaan we de gegevensset te splitsen in trainings- en testset sets:

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

En maak vervolgens een beslissingsstructuur voor het classificeren van de e-mailberichten.

    require(rpart)
    model.rpart <- rpart(spam ~ ., method = "class", data = trainSet)
    plot(model.rpart)
    text(model.rpart)

Dit is het resultaat:

![1](./media/linux-dsvm-walkthrough/decision-tree.png)

Om te bepalen hoe goed worden uitgevoerd op de trainingset, kunt u de volgende code gebruiken:

    trainSetPred <- predict(model.rpart, newdata = trainSet, type = "class")
    t <- table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

Om te bepalen hoe goed worden uitgevoerd op de testset:

    testSetPred <- predict(model.rpart, newdata = testSet, type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy

U kunt ook gaan we een willekeurige forest-model. Willekeurige forests tal van beslissingsstructuren trainen en uitvoer van een klasse die de modus van de classificaties uit alle van de afzonderlijke beslissingsstructuren. Ze bieden een krachtigere machine learning-benadering als voor de neiging van een boomstructuur besluit naar een gegevensset training overfit worden gecorrigeerd.

    require(randomForest)
    trainVars <- setdiff(colnames(data), 'spam')
    model.rf <- randomForest(x=trainSet[, trainVars], y=trainSet$spam)

    trainSetPred <- predict(model.rf, newdata = trainSet[, trainVars], type = "class")
    table(`Actual Class` = trainSet$spam, `Predicted Class` = trainSetPred)

    testSetPred <- predict(model.rf, newdata = testSet[, trainVars], type = "class")
    t <- table(`Actual Class` = testSet$spam, `Predicted Class` = testSetPred)
    accuracy <- sum(diag(t))/sum(t)
    accuracy


## <a name="deploy-a-model-to-azure-ml"></a>Implementeren van een model voor Azure ML
[Azure Machine Learning Studio](https://studio.azureml.net/) (AzureML) is een cloudservice waarmee u gemakkelijk kunnen bouwen en implementeren van predictive analytics-modellen. Een van de nice functies van AzureML is de mogelijkheid voor het publiceren van een R-functie als een webservice. Het AzureML-R-pakket maakt implementatie eenvoudig om gewoon via onze R-sessie op de DSVM.

Als u wilt implementeren de decision tree code uit de vorige sectie, moet u zich aanmelden bij Azure Machine Learning Studio. U moet uw werkruimte-ID en een verificatietoken aan te melden. Deze waarden vinden en het initialiseren van de AzureML-variabelen met hen:

Selecteer **instellingen** op in het menu links. Opmerking uw **WERKRUIMTE-ID**. ![2](./media/linux-dsvm-walkthrough/workspace-id.png)

Selecteer **autorisatie Tokens** uit de overhead menu en de opmerking uw **primaire autorisatie Token**.![ 3](./media/linux-dsvm-walkthrough/workspace-token.png)

Load de **AzureML** van het pakket en stelt u de waarden van variabelen met uw token en werkruimte-ID in uw R-sessie op de DSVM:

    require(AzureML)
    wsAuth = "<authorization-token>"
    wsID = "<workspace-id>"


Laten we vereenvoudigen het model om deze demonstratie gemakkelijker te implementeren. Kies de drie variabelen in de beslissingsstructuur die het dichtst bij de hoofdmap en bouwen van een nieuwe boomstructuur met behulp van alleen die drie variabelen:

    colNames <- c("char_freq_dollar", "word_freq_remove", "word_freq_hp", "spam")
    smallTrainSet <- trainSet[, colNames]
    smallTestSet <- testSet[, colNames]
    model.rpart <- rpart(spam ~ ., method = "class", data = smallTrainSet)

Er moet een voorspellingsfunctie die ervoor zorgt dat de functies als invoer de voorspelde waarden geretourneerd:

    predictSpam <- function(char_freq_dollar, word_freq_remove, word_freq_hp) {
        predictDF <- predict(model.rpart, data.frame("char_freq_dollar" = char_freq_dollar,
        "word_freq_remove" = word_freq_remove, "word_freq_hp" = word_freq_hp))
        return(colnames(predictDF)[apply(predictDF, 1, which.max)])
    }

De functie predictSpam publiceren naar het AzureML met behulp van de **publishWebService** functie:

    spamWebService <- publishWebService("predictSpam",
        "spamWebService",
        list("char_freq_dollar"="float", "word_freq_remove"="float","word_freq_hp"="float"),
        list("spam"="int"),
        wsID, wsAuth)

Deze functie heeft de **predictSpam** werkt, maakt u een webservice die met de naam **spamWebService** gedefinieerd met in- en uitgangen en retourneert informatie over het nieuwe eindpunt.

Details van de gepubliceerde webservice, met inbegrip van de API-eindpunt weergeven en toegangssleutels met de opdracht:

    spamWebService[[2]]

Uitproberen op de eerste 10 rijen van de test instellen:

    consumeDataframe(spamWebService$endpoints[[1]]$PrimaryKey, spamWebService$endpoints[[1]]$ApiLocation, smallTestSet[1:10, 1:3])


## <a name="use-other-tools-available"></a>Andere hulpprogramma's gebruiken
De resterende secties laten zien hoe u een aantal van de hulpprogramma's geïnstalleerd op de Linux-VM wetenschappelijke van gegevens. Hier volgt de lijst met hulpprogramma's beschreven:

* XGBoost
* Python
* Jupyterhub
* Rammelaar
* PostgreSQL & Squirrel SQL
* SQL Server-datawarehouse

## <a name="xgboost"></a>XGBoost
[XGBoost](https://xgboost.readthedocs.org/en/latest/) is een hulpprogramma waarmee een snelle en nauwkeurige gestimuleerd structuur-implementatie.

    require(xgboost)
    data <- read.csv("spambaseHeaders.data")
    set.seed(123)

    rnd <- runif(dim(data)[1])
    trainSet = subset(data, rnd <= 0.7)
    testSet = subset(data, rnd > 0.7)

    bst <- xgboost(data = data.matrix(trainSet[,0:57]), label = trainSet$spam, nthread = 2, nrounds = 2, objective = "binary:logistic")

    pred <- predict(bst, data.matrix(testSet[, 0:57]))
    accuracy <- 1.0 - mean(as.numeric(pred > 0.5) != testSet$spam)
    print(paste("test accuracy = ", accuracy))

XGBoost kunnen ook aanroepen vanuit python of een opdrachtregel.

## <a name="python"></a>Python
Voor de ontwikkeling met behulp van Python, zijn de distributies Anaconda Python 2.7 en 3.5 geïnstalleerd in de DSVM.

> [!NOTE]
> Bevat de distributie Anaconda [Condas](http://conda.pydata.org/docs/index.html), die kan worden gebruikt voor het maken van aangepaste omgevingen voor Python die verschillende versies en/of geïnstalleerd in deze pakketten.
>
>

Laten we enkele van de gegevensset spambase gelezen en de e-mailberichten classificeren met ondersteuning voor virtuele machines vector in scikit-informatie:

    import pandas
    from sklearn import svm    
    data = pandas.read_csv("spambaseHeaders.data", sep = ',\s*')
    X = data.ix[:, 0:57]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Maken van voorspellingen:

    clf.predict(X.ix[0:20, :])

Om weer te geven hoe u een eindpunt voor met AzureML publiceert, gaan we een eenvoudigere model maken de drie variabelen zoals toen we het model R eerder hebt gepubliceerd.

    X = data.ix[["char_freq_dollar", "word_freq_remove", "word_freq_hp"]]
    y = data.ix[:, 57]
    clf = svm.SVC()
    clf.fit(X, y)

Het model om naar te publiceren voor met AzureML:

    # Publish the model.
    workspace_id = "<workspace-id>"
    workspace_token = "<workspace-token>"
    from azureml import services
    @services.publish(workspace_id, workspace_token)
    @services.types(char_freq_dollar = float, word_freq_remove = float, word_freq_hp = float)
    @services.returns(int) # 0 or 1
    def predictSpam(char_freq_dollar, word_freq_remove, word_freq_hp):
        inputArray = [char_freq_dollar, word_freq_remove, word_freq_hp]
        return clf.predict(inputArray)

    # Get some info about the resulting model.
    predictSpam.service.url
    predictSpam.service.api_key

    # Call the model
    predictSpam.service(1, 1, 1)

> [!NOTE]
> Dit is alleen beschikbaar voor python 2.7 en nog niet wordt ondersteund op 3.5. Voer met **/anaconda/bin/python2.7**.
>
>

## <a name="jupyterhub"></a>Jupyterhub
De Anaconda-distributie in de DSVM wordt geleverd met een Jupyter-notebook een omgeving met meerdere platforms voor het delen van Python, R of Julia code en -analyse. De Jupyter-notebook toegankelijk is via JupyterHub. U zich aanmelden met uw lokale Linux-gebruikersnaam en wachtwoord op ***https://\<VM DNS-naam of IP-adres\>: 8000 /***. Alle configuratiebestanden voor JupyterHub zijn gevonden in map **/etc/jupyterhub**.

Verschillende voorbeeldquery notitieblokken zijn al geïnstalleerd op de virtuele machine:

* Zie de [IntroToJupyterPython.ipynb](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroToJupyterPython.ipynb) voor een voorbeeld Python-notebook.
* Zie [IntroTutorialinR](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IntroTutorialinR.ipynb) voor een voorbeeld van een **R** notebook.
* Zie de [IrisClassifierPyMLWebService](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Data-Science-Virtual-Machine/Samples/Notebooks/IrisClassifierPyMLWebService.ipynb) voor een ander voorbeeld **Python** notebook.

> [!NOTE]
> De taal Julia is ook beschikbaar via de opdrachtregel op de VM, wetenschappelijke gegevens Linux.
>
>

## <a name="rattle"></a>Rammelaar
[Rattle](https://cran.r-project.org/web/packages/rattle/index.html) (de R analytische hulpprogramma voor meer informatie over eenvoudig) is een grafisch hulpprogramma R voor gegevensanalyse. Er is een intuïtieve interface waarmee u gemakkelijk om te laden, verkennen, en gegevens transformeren en bouwen en evalueren van modellen.  Het artikel [Rattle: A Data Mining GUI voor R](https://journal.r-project.org/archive/2009-2/RJournal_2009-2_Williams.pdf) biedt een overzicht over de functies.

Installeren en Rammelaar beginnen met de volgende opdrachten:

    if(!require("rattle")) install.packages("rattle")
    require(rattle)
    rattle()

> [!NOTE]
> De installatie is niet vereist op de DSVM. Maar Rammelaar u mogelijk gevraagd te extra pakketten installeren wanneer deze wordt geladen.
>
>

Rammelaar maakt gebruik van een tabblad-gebaseerde interface. De meeste van de tabbladen komen overeen met de stappen in de [gegevens wetenschap proces](https://azure.microsoft.com/documentation/learning-paths/data-science-process/), zoals het laden van gegevens of het verkennen. Het proces van de wetenschappelijke gegevens loopt van links naar rechts de tabbladen. Maar het laatste tabblad bevat een logboek van het R-opdrachten door Rammelaar worden uitgevoerd.

Laden en configureren van de gegevensset:

* Als u wilt laden van het bestand, selecteer de **gegevens** tabblad vervolgens
* Kies de selector naast **Filename** en kies **spambaseHeaders.data**.
* Het bestand laden. Selecteer **Execute** in de bovenste rij knoppen. U ziet een overzicht van elke kolom, met inbegrip van het geïdentificeerde gegevenstype, of u nu invoer, een doel of andere typen van de variabele en het aantal unieke waarden.
* Rammelaar geconstateerd correct de **spam** kolom als doel. Selecteer de kolom spam en stel vervolgens de **gegevens doeltype** naar **Categoric**.

De gegevens verkennen:

* Selecteer de **verkennen** tabblad.
* Klik op **samenvatting**, klikt u vervolgens **Execute**, om informatie over de typen variabelen en sommige samenvattende statistieken te zien.
* Als u wilt weergeven van andere typen statistieken over elke variabele, selecteer andere opties zoals **Beschrijf** of **basisbeginselen**.

De **verkennen** tabblad kunt u veel begrijpelijke manier mee waarnemingspunten genereren. Om te tekenen een histogram van de gegevens:

* Selecteer **distributies**.
* Controleer **Histogram** voor **word_freq_remove** en **word_freq_you**.
* Selecteer **uitvoeren**. Hier ziet u beide waarnemingspunten dichtheid in een grafiekvenster van één, waarbij duidelijk is dat het woord "u" vaker in e-mailberichten staat dan 'verwijderen'.

De correlatie waarnemingspunten zijn ook interessante. Een te maken:

* Kies **correlatie** als de **Type**, klikt u vervolgens
* Selecteer **uitvoeren**.
* Rammelaar waarschuwt u raadt maximaal 40 variabelen. Selecteer **Ja** om weer te geven van de grafiek.

Er zijn enkele interessante correlaties die afkomstig van zijn: 'technologie' wordt ten zeerste gecorreleerd met 'HP' en 'labs', bijvoorbeeld. Dit wordt ook sterk gecorreleerd met '650', omdat het netnummer van de gegevensset donors 650.

De numerieke waarden van de correlatie tussen woorden zijn beschikbaar in het venster verkennen. Opmerking, bijvoorbeeld 'technologie' op de negatieve worden gecorreleerd met "uw" en 'geld' interessant is.

Rammelaar kan de gegevensset voor het afhandelen van enkele veelvoorkomende problemen transformeren. Zo kunnen u oorspronkelijke functies, ontbrekende waarden rekenen uitschieters verwerken en variabelen of opmerkingen met ontbrekende gegevens verwijderen. Rammelaar kunt ook regels van de koppeling tussen opmerkingen en/of variabelen identificeren. Deze tabbladen zijn buiten het bereik van deze Introductie.

Rammelaar kunt ook cluster analyse uitvoeren. Laten we uitsluiten sommige functies om de uitvoer eenvoudiger te lezen. Op de **gegevens** Kies **negeren** naast elk van de variabelen behalve deze tien items:

* word_freq_hp
* word_freq_technology
* word_freq_george
* word_freq_remove
* word_freq_your
* word_freq_dollar
* word_freq_money
* capital_run_length_longest
* word_freq_business
* ongewenste e-mail

Ga vervolgens terug naar de **Cluster** Kies **KMeans**, en stel de *aantal clusters* tot en met 4. Vervolgens **uitvoeren**. De resultaten worden weergegeven in het uitvoervenster. Een cluster met hoge frequentie van 'george' en 'hp' heeft en is waarschijnlijk een legitieme zakelijke-e-mailadres.

Een eenvoudige decision tree machine learning-model bouwen:

* Selecteer de **Model** tabblad
* Kies **structuur** als de **Type**.
* Selecteer **Execute** om weer te geven van de structuur in tekstvorm in het uitvoervenster.
* Selecteer de **tekenen** knop om een grafische versie weer te geven. Dit lijkt erg op de structuur wordt verkregen eerder met behulp van *rpart*.

Een van de nice functies van Rammelaar is de mogelijkheid voor het uitvoeren van verschillende methoden van de machine learning en deze snel te evalueren. Hier volgt de procedure:

* Kies **alle** voor de **Type**.
* Selecteer **uitvoeren**.
* Na voltooiing klikt u op enkel **Type**, bijvoorbeeld **SVM**, en bekijk de resultaten.
* U kunt ook vergelijken de prestaties van de modellen in de validatiesamenvatting ingesteld met de **Evaluate** tabblad. Bijvoorbeeld, de **fout Matrix** selectie ziet u de matrix verwarring, algemene fout en gemiddelde klasse fout voor elk model op de set validatie.
* U kunt ook tekenen ROC-curve, analyse van de gevoeligheid en andere soorten model evaluaties doen.

Zodra u klaar bent voor het bouwen van modellen, selecteert u de **logboek** tabblad om de R-code uitvoeren met Rammelaar tijdens uw sessie. U kunt selecteren de **exporteren** knop op te slaan.

> [!NOTE]
> Er is een fout in de huidige release van Rammelaar. Voor het script wijzigen of dit de stappen herhalen later gebruiken, moet u een teken # vóór * exporteren... dit logboek * in de tekst van het logboek.
>
>

## <a name="postgresql--squirrel-sql"></a>PostgreSQL & Squirrel SQL
De DSVM wordt geleverd met PostgreSQL geïnstalleerd. PostgreSQL is een geavanceerde, open-source relationele database. Deze sectie wordt beschreven hoe u onze gegevensset spam laden in PostgreSQL en deze vervolgens een query.

Voordat u de gegevens laden kunt, moet u wachtwoordverificatie van de localhost toestaan. Bij een opdrachtprompt:

    sudo gedit /var/lib/pgsql/data/pg_hba.conf

Aan de onderkant van het configuratiebestand zijn verschillende regels die de toegestane verbindingen worden in detail beschreven:

    # "local" is for Unix domain socket connections only
    local   all             all                                     trust
    # IPv4 local connections:
    host    all             all             127.0.0.1/32            ident
    # IPv6 local connections:
    host    all             all             ::1/128                 ident

De regel 'IPv4 lokale verbindingen' voor het gebruik van md5 in plaats van ident, zodat we kunnen aanmelden met een gebruikersnaam en wachtwoord wijzigen:

    # IPv4 local connections:
    host    all             all             127.0.0.1/32            md5

De service en start postgres:

    sudo systemctl restart postgresql

Psql, een interactief terminal voor PostgreSQL als de gebruiker ingebouwde postgres, start de volgende opdracht uit te voeren vanaf een opdrachtprompt:

    sudo -u postgres psql

Maak een nieuw gebruikersaccount met de dezelfde gebruikersnaam als het Linux-account u momenteel bent aangemeld als en wijs hieraan een wachtwoord:

    CREATE USER <username> WITH CREATEDB;
    CREATE DATABASE <username>;
    ALTER USER <username> password '<password>';
    \quit

Meld u vervolgens naar psql als uw gebruikers:

    psql

En de gegevens in een nieuwe database te importeren:

    CREATE DATABASE spam;
    \c spam
    CREATE TABLE data (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer);
    \copy data FROM /home/<username>/spambase.data DELIMITER ',' CSV;
    \quit

Nu gaan we Verken de gegevens en uitvoeren van sommige query's met behulp van **Squirrel SQL**, een grafisch hulpprogramma waarmee u communiceren met databases via een JDBC-stuurprogramma.

Start SQL Squirrel in het menu toepassingen om te beginnen. Het stuurprogramma instellen:

* Selecteer **Windows**, klikt u vervolgens **stuurprogramma's weergeven**.
* Met de rechtermuisknop op **PostgreSQL** en selecteer **stuurprogramma wijzigen**.
* Selecteer **Extra klasse pad**, klikt u vervolgens **toevoegen**.
* Voer ***/usr/share/java/jdbcdrivers/postgresql-9.4.1208.jre6.jar*** voor de **bestandsnaam** en
* Selecteer **Open**.
* Lijst met stuurprogramma's kiezen en selecteer vervolgens **org.postgresql.Driver** in **klassenaam**, en selecteer **OK**.

De verbinding met de lokale server instellen:

* Selecteer **Windows**, klikt u vervolgens **aliassen weergeven.**
* Kies de  **+**  knop om te maken van een nieuwe alias.
* Naam *Spam database*, kies **PostgreSQL** in de **stuurprogramma** vervolgkeuzelijst.
* De URL ingesteld op *jdbc:postgresql://localhost/spam*.
* Voer uw *gebruikersnaam* en *wachtwoord*.
* Klik op **OK**.
* Openen van de **verbinding** venster, dubbelklikt u op de ***Spam database*** alias.
* Selecteer **Verbinden**.

Sommige query's uitvoeren:

* Selecteer de **SQL** tabblad.
* Voer een eenvoudige query zoals `SELECT * from data;` in het tekstvak query aan de bovenkant van het SQL-tabblad.
* Druk op **Ctrl + Enter** uit te voeren. Standaard Squirrel SQL de eerste 100 rijen geretourneerd uit uw query.

Er zijn veel meer query's dat om te bekijken van deze gegevens kan worden uitgevoerd. Bijvoorbeeld, hoe gaat de frequentie van het woord *zorg* verschillen tussen spam en ham?

    SELECT avg(word_freq_make), spam from data group by spam;

Of wat zijn de kenmerken van de e-mail die vaak bevatten *3d*?

    SELECT * from data order by word_freq_3d desc;

De meeste e-mailberichten waarvoor een hoge exemplaar van *3d* zijn blijkbaar spam, zodat het mogelijk een handige functie voor het bouwen van een Voorspellend model voor het classificeren van de e-mailberichten.

Als u uitvoeren van machine learning met gegevens die zijn opgeslagen in een PostgreSQL-database wilt, kunt u overwegen [MADlib](http://madlib.incubator.apache.org/).

## <a name="sql-server-data-warehouse"></a>SQL Server-datawarehouse
Azure SQL Data Warehouse is een schaalbare clouddatabase die geschikt is voor het verwerken van grote hoeveelheden relationele en/of niet-relationele gegevens. Zie voor meer informatie [wat is Azure SQL Data Warehouse?](../../sql-data-warehouse/sql-data-warehouse-overview-what-is.md)

Als u wilt verbinding maken met het datawarehouse en de tabel maakt, voer de volgende opdracht uit vanaf de opdrachtprompt:

    sqlcmd -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -I

Klik op het sqlcmd-opdrachtprompt:

    CREATE TABLE spam (word_freq_make real, word_freq_address real, word_freq_all real, word_freq_3d real,word_freq_our real, word_freq_over real, word_freq_remove real, word_freq_internet real,word_freq_order real, word_freq_mail real, word_freq_receive real, word_freq_will real,word_freq_people real, word_freq_report real, word_freq_addresses real, word_freq_free real,word_freq_business real, word_freq_email real, word_freq_you real, word_freq_credit real,word_freq_your real, word_freq_font real, word_freq_000 real, word_freq_money real,word_freq_hp real, word_freq_hpl real, word_freq_george real, word_freq_650 real, word_freq_lab real,word_freq_labs real, word_freq_telnet real, word_freq_857 real, word_freq_data real,word_freq_415 real, word_freq_85 real, word_freq_technology real, word_freq_1999 real,word_freq_parts real, word_freq_pm real, word_freq_direct real, word_freq_cs real, word_freq_meeting real,word_freq_original real, word_freq_project real, word_freq_re real, word_freq_edu real,word_freq_table real, word_freq_conference real, char_freq_semicolon real, char_freq_leftParen real,char_freq_leftBracket real, char_freq_exclamation real, char_freq_dollar real, char_freq_pound real, capital_run_length_average real, capital_run_length_longest real, capital_run_length_total real, spam integer) WITH (CLUSTERED COLUMNSTORE INDEX, DISTRIBUTION = ROUND_ROBIN);
    GO

Gegevens kopiëren met bcp:

    bcp spam in spambaseHeaders.data -q -c -t  ',' -S <server-name>.database.windows.net -d <database-name> -U <username> -P <password> -F 1 -r "\r\n"

> [!NOTE]
> De regeleinden in het gedownloade bestand zijn Windows-stijl, maar bcp verwacht UNIX-stijl, daarom moet u vertelt bcp die met de vlag - r.
>
>

En de query met sqlcmd:

    select top 10 spam, char_freq_dollar from spam;
    GO

U kunt ook een query met Squirrel SQL. Uitvoeren van gelijksoortige stappen voor PostgreSQL, met behulp van het stuurprogramma Microsoft MSSQL Server JDBC die kunnen worden gevonden in ***/usr/share/java/jdbcdrivers/sqljdbc42.jar***.

## <a name="next-steps"></a>Volgende stappen
Zie voor een overzicht van onderwerpen die helpt u stapsgewijs door de taken die het proces voor Gegevenswetenschap in Azure omvatten, [Team gegevens wetenschap proces](http://aka.ms/datascienceprocess).

Zie voor een beschrijving van andere end-to-end-scenario's die de stappen in de procedure van wetenschappelijke gegevens Team voor specifieke scenario's laten zien [Team gegevens wetenschap proces scenario's](../team-data-science-process/walkthroughs.md). De scenario's ook te laten zien hoe cloud en on-premises hulpprogramma's en services combineren in een werkstroom of pijplijn een intelligente toepassing maken.
