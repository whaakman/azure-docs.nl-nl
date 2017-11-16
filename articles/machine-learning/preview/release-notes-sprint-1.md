---
title: Azure ML-Workbench release-opmerkingen voor sprint 1 November 2017
description: In dit document worden de updates voor de sprint 1-release van Azure ML
services: machine-learning
author: hning86
ms.author: haining
manager: mwinkle
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.topic: article
ms.date: 11/06/2017
ms.openlocfilehash: a4945c77be5763ffeda328184149f712572937c0
ms.sourcegitcommit: afc78e4fdef08e4ef75e3456fdfe3709d3c3680b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2017
---
# <a name="sprint-1---november-2017"></a>Sprint 1 - november 2017 

**Versienummer: 0.1.1710.31013**

Welkom bij de tweede update van Azure Machine Learning-Workbench. We blijven de verbeteringen rond de beveiliging, stabiliteit en onderhoud te maken in de workbench-app, de CLI en de back-end-services-laag. Hartelijk dank voor het verzenden van Messenger en frowns. Veel van de onderstaande updates zijn aangebracht tijdens het direct resultaten van uw feedback. Zorg ervoor dat ze afkomstig zijn.

## <a name="notable-new-features"></a>Belangrijke nieuwe functies
- Azure ML is nu beschikbaar in twee nieuwe Azure-regio's: **West-Europa** en **Zuidoost-Azië**. Die lid worden van de vorige gebieden van **VS-Oost 2**, **West-Centraal VS**, en **Australië-Oost**, regio's tot vijf brengen van het totale aantal geïmplementeerd.
- We Python code syntaxismarkering in de Workbench-app om het gemakkelijker te lezen en bewerken van broncode Python ingeschakeld. 
- U kunt nu uw favoriete IDE rechtstreeks vanuit een bestand in plaats van vanaf het hele project starten.  Een bestand openen in de Workbench en vervolgens te klikken op 'Bewerken' Start uw IDE (momenteel tegenover Code en PyCharm worden ondersteund) naar het huidige bestand en het project.  U kunt ook klikken op de pijl naast de knop bewerken om te bewerken van het bestand in de teksteditor Workbench.  Bestanden zijn alleen-lezen, totdat u klikt u op bewerken, zo wordt voorkomen dat per ongeluk worden gewijzigd.
- De populaire tekengebied bibliotheek `matplotlib` versie 2.1.0 wordt nu geleverd bij de Workbench-app.
- We de versie van .NET Core bijgewerkt naar 2.0 voor de prep-engine. Deze verwijderd de vereiste voor openssl brew installeren tijdens de installatie van de app op Mac OS. Deze ook de weg wordt vrijgemaakt voor meer informatie vinden over interessante gegevens prep functies die u in de nabije toekomst worden geleverd. 
- We hebben een startpagina versiespecifiek app ingeschakeld zodat u meer relevante releaseopmerkingen ophalen en wordt u gevraagd op basis van uw huidige versie van de app bijwerken.
- Als de naam van uw lokale gebruiker een spatie erin heeft, kan de toepassing nu worden geïnstalleerd. 

## <a name="detailed-updates"></a>Gedetailleerde Updates
Hieronder ziet u een lijst met gedetailleerde updates voor elk onderdeel gebied in Azure Machine Learning in deze sprint.

### <a name="installer"></a>Installatieprogramma
- App-installatieprogramma ruimt nu naar de installatiemap gemaakt door een oudere versie van de app.
- Een fout die leidt installatieprogramma op 100% op Mac OS hoge Sierra te zitten vast.
- Er is nu een directe koppeling naar de map installatieprogramma voor de gebruiker om te controleren installatieprogramma Logboeken als de installatie is mislukt.
- Installeer werkt nu voor gebruikers die ruimte in hun gebruikersnaam hebben.

### <a name="workbench-authentication"></a>Workbench verificatie
- Ondersteuning voor verificatie in de Proxy-Manager.
- Logboekregistratie in nu is geslaagd als de gebruiker zich achter een firewall. 
- Als de gebruiker heeft experimenteren accounts in meerdere Azure-regio's, en als één regio gebeurt niet beschikbaar is, wordt de app niet meer loopt vast.
- Wanneer verificatie is niet voltooid en het verificatiedialoogvenster nog steeds zichtbaar is, wordt de app niet langer probeert werkruimte laden vanuit de lokale cache.

### <a name="workbench-app"></a>Workbench App
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

### <a name="data-preparation"></a>Gegevensvoorbereiding 
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

### <a name="job-execution"></a>Uitvoeren van taak
- Argument is nu opgenomen in de uitvoeringsgeschiedenis record.
- Taken gestarte CLI nu weergegeven in deelvenster voor historie taak uitvoeren automatisch.
- Deelvenster van de taak wordt nu opdrachten aangemaakt door gastgebruikers toegevoegd aan de AAD-tenant.
- Deelvenster taak annuleren en verwijderingsacties stabieler zijn.
- Wanneer op de knop uitvoeren klikt, wordt nu foutbericht geactiveerd als de configuratiebestanden in een onjuiste notatie.
- Taken in de CLI gestarte verstoort afsluitende app niet meer.
- Taken gestarte CLI nu blijft spit uit de standard out zelfs na een uur van de uitvoering.
- Betere foutberichten worden weergegeven wanneer de prep gegevenspakket uitvoeren in Python/PySpark mislukt.
- `az ml experiment clean`ruimt nu Docker-afbeeldingen in ook externe VM.
- `az ml experiment clean`nu werkt goed voor lokale doel op Mac OS.
- Foutberichten wanneer die gericht is op lokale of externe Docker wordt uitgevoerd, worden opgeschoond omhoog en gemakkelijker te lezen.
- Betere foutbericht wordt weergegeven wanneer de naam van het hoofdknooppunt voor HDInsight-cluster niet correct opgemaakt is als gekoppeld als een doel kan worden uitgevoerd.
- Betere foutbericht wordt weergegeven wanneer het geheim is niet gevonden in de referentie-service. 
- MMLSpark-bibliotheek is voor ondersteuning van Apache Spark 2.2 bijgewerkt.
- MMLSpark omvatten nu een onderwerp codering transformatie (Mesh codering) voor medische documenten.
- `matplotlib`versie 2.1.0 is nu verzonden out van het vak met Workbench.

### <a name="jupyter-notebook"></a>Jupyter Notebook
- Zoekactie voor namen notebook nu werkt goed in de weergave laptops.
- U kunt nu een laptop in de weergave laptops verwijderen.
- Nieuwe magic `%upload_artifact` wordt toegevoegd voor het uploaden van bestanden die in de Notebook uitvoeringsomgeving in het gegevensarchief uitvoeringsgeschiedenis wordt geproduceerd.
- Kernelfouten worden nu in de status van de Notebook taak voor het opsporen van eenvoudiger opgehaald.
- Jupyter server nu correct wordt afgesloten wanneer de gebruiker zich aanmeldt bij de app.

### <a name="azure-portal"></a>Azure Portal
- Account experimenteren en Model-Management-account kunnen nu worden gemaakt in twee nieuwe Azure-regio's: West-Europa en Zuidoost-Azië.
- Model Management account DevTest plan nu is alleen beschikbaar wanneer dit het eerste item in het abonnement worden gemaakt. 
- Help-koppeling in de Azure portal is bijgewerkt om te verwijzen naar de juiste documentatiepagina.
- Beschrijvingsveld wordt verwijderd uit de detailpagina Docker-installatiekopie, omdat deze niet van toepassing.
- Informatie over de instellingen voor AppInsights en automatisch schalen zoals worden toegevoegd aan de pagina met details.
- Model management-pagina wordt nu gerenderd zelfs als cookies van derden zijn uitgeschakeld in de browser. 

### <a name="operationalization"></a>Uitoefening
- Webservice met 'score' in de naam ervan niet meer is mislukt.
- Gebruiker kan nu met het maken van een implementatieomgeving met alleen Inzender toegang tot een Azure-resourcegroep of het abonnement. Eigenaar toegang tot het hele abonnement is niet meer nodig.
- Uitoefening CLI nu leuk vindt tabblad automatisch aanvullen op Linux.
- Bouw afbeeldingsservice ondersteunt nu installatiekopieën van het gebouw voor Azure IoT services/apparaten.

### <a name="sample-projects"></a>Voorbeeldprojecten
- [_Classificeren van Iris_ ](./tutorial-classifying-iris-part-1.md) voorbeeldproject:
    - `iris_pyspark.py`is gewijzigd in `iris_spark.py`.
    - `iris_score.py`is gewijzigd in `score_iris.py`.
    - `iris.dprep`en `iris.dsource` bijgewerkt op basis van de meest recente gegevens prep engine-updates.
    - `iris.ipynb`Laptop is gewijzigd om te werken in HDInsight-cluster.
    - Uitvoeringsgeschiedenis is ingeschakeld in `iris.ipynb` Notebook cel.
- [_Geavanceerde Prep van gegevens met behulp van de sharegegevens fiets_ ](./tutorial-bikeshare-dataprep.md) voorbeeldproject 'Foutwaarde verwerken' stap vast.
- [_MMLSpark op volwassenen telling gegevens_ ](https://github.com/Azure/MachineLearningSamples-mmlspark) voorbeeldproject `docker.runconfig` bijgewerkt van JSON naar YAML indeling.
- [_Gedistribueerde Hyperparameter afstemmen_ ](./scenario-distributed-tuning-of-hyperparameters.md) voorbeeldproject`docker.runconfig` bijgewerkt van JSON naar YAML indeling.
- Nieuwe voorbeeldproject [ _installatiekopie classificatie met CNTK_](./scenario-image-classification-using-cntk.md).
