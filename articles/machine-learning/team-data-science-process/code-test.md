---
title: Data science code testen op Azure met UCI volwassenen inkomsten voorspelling gegevensset - Team Data Science Process en Visual Studio Team Services
description: Data science code testen met UCI volwassenen inkomsten voorspellingsgegevens
services: machine-learning, team-data-science-process
documentationcenter: ''
author: weig
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.component: team-data-science-process
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2018
ms.author: weig
ms.openlocfilehash: 46d156ce09b1ebcdcceb27ede6e7fa1595d30da6
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/02/2018
ms.locfileid: "39439494"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Data science code testen met de gegevensset UCI volwassenen inkomsten voorspelling
Dit artikel bevat voorlopige richtlijnen voor het testen van code in een werkstroom voor datatechnologie. Dergelijke test biedt gegevenswetenschappers een systematische en efficiënte manier om te controleren of de kwaliteit en de verwachte resultaten van hun code. We gebruiken een Team Data Science Process (TDSP) [project die gebruikmaakt van de gegevensset UCI volwassenen inkomsten](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) die we eerder gepubliceerd om weer te geven hoe het testen van code kan worden gedaan. 

## <a name="introduction-on-code-testing"></a>Inleiding over het testen van code
'Eenheidstesten' is een zich al geruime tijd procedure voor het ontwikkelen van software. Maar voor datatechnologie, is het vaak niet duidelijk welke die middelen en hoe u moet testen van code voor verschillende fasen van een volledige wetenschappelijke levensduur, zoals:

* Gegevensvoorbereiding
* Data quality onderzoek
* Modelleren
* Modelimplementatie 

In dit artikel vervangt de term 'eenheid testen"met"code testen." Deze gegevensset verwijst naar testen als de functies die u helpen te beoordelen als code voor een bepaalde stap van een volledige wetenschappelijke levensduur levert resultaten 'zoals verwacht." De persoon die dat de test definieert wat schrijft 'zoals verwacht,"is afhankelijk van het resultaat van de functie--bijvoorbeeld kwaliteitscontrole van de gegevens of modellen.

Dit artikel bevat verwijzingen naar als handige resources.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Visual Studio Team Services voor het testen framework
In dit artikel wordt beschreven hoe u uitvoeren en testen met behulp van Visual Studio Team Services (VSTS) te automatiseren. U kunt andere hulpprogramma's gebruiken. We laten ook zien hoe u een automatische build instellen met behulp van VSTS en build-agents. Voor de build-agents gebruiken we Azure Data Science Virtual Machines (Dsvm).

## <a name="flow-of-code-testing"></a>Stroom van het testen van code
De algemene werkstroom testen in een data science-project de code ziet er als volgt: 

![Stroomdiagram van het testen van code](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Gedetailleerde stappen

Gebruik de volgende stappen uit om te testen van code en een geautomatiseerde build op te zetten met behulp van een build-agent en VSTS en:

1. Een project maken in de Visual Studio-bureaubladtoepassing:

    ![Scherm 'Nieuw project maken' in Visual Studio](./media/code-test/create_project.PNG)

   Nadat u uw project maakt, moet u deze vinden in Solution Explorer in het rechterdeelvenster:
    
    ![Stappen voor het maken van een project](./media/code-test/create_python_project_in_vs.PNG)

    ![Solution Explorer](./media/code-test/solution_explorer_in_vs.PNG)

1. De projectcode van uw-feed in de codeopslagplaats van de VSTS-project: 

    ![Codeopslagplaats project](./media/code-test/create_repo.PNG)

1. Stel dat u sommige gegevensvoorbereidingswerkzaamheden, zoals gegevensopname, feature-engineering en het maken van label kolommen hebben gedaan. U wilt controleren of dat uw code genereert de resultaten die u verwacht. Dit is wat code die u gebruiken kunt om te testen of de code van de gegevensverwerking correct werkt:

    * Controleer of de namen van kolommen rechts zijn:
    
      ![Code voor het afstemmen van de namen van kolommen](./media/code-test/check_column_names.PNG)

    * Controleer of responsniveaus rechts zijn:

      ![Code voor die overeenkomen met niveaus](./media/code-test/check_response_levels.PNG)

    * Controleer of de antwoord-percentage redelijke is:

      ![Code voor het percentage van de reactie voor](./media/code-test/check_response_percentage.PNG)

    * Controleer de ontbrekende snelheid van elke kolom in de gegevens:
    
      ![Code voor een ontbrekende snelheid](./media/code-test/check_missing_rate.PNG)


1. Nadat u hebt de gegevensverwerking en functie-engineering werk gedaan en u een goed model hebt getraind, zorg ervoor dat het model dat u getraind correct nieuwe gegevenssets kunt beoordelen. U kunt de volgende twee tests uit om te controleren of de voorspelling niveaus en distributie van de labelwaarden:

    * Controleer de voorspelling niveaus:
    
      ![Code voor het controleren van voorspelling niveaus](./media/code-test/check_prediction_levels.PNG)

    * Controleer de distributie van voorspelling waarden:

      ![Code voor het controleren van de waarden voor voorspelling](./media/code-test/check_prediction_values.PNG)

1. Alle functies samen in een pythonscript met de naam test put **test_funcs.py**:

    ![Python-script voor test-functies](./media/code-test/create_file_test_func.PNG)


1. Nadat de test-codes zijn voorbereid, kunt u de testomgeving in Visual Studio instellen.

   Maak een Python-bestand met de naam **test1.py**. In dit bestand, maakt u een klasse met alle tests die u wilt doen. Het volgende voorbeeld ziet u zes tests voorbereid:
    
    ![Python-bestand met een lijst met tests in een klasse](./media/code-test/create_file_test1_class.PNG)

1. Deze tests kunnen automatisch worden gedetecteerd als u **codetest.testCase** achter de klassenaam. Test Explorer openen in het rechterdeelvenster en selecteer **alles uitvoeren**. Alle tests achter elkaar wordt uitgevoerd en zien als de test geslaagd of niet is.

    ![De tests worden uitgevoerd](./media/code-test/run_tests.PNG)

1. Controleer in uw code in de projectopslagplaats met behulp van Git-opdrachten. Uw meest recente werk worden binnenkort in VSTS weergegeven.

    ![GIT-opdrachten voor het controleren van in de code](./media/code-test/git_check_in.PNG)

    ![Meest recente werk in VSTS](./media/code-test/git_check_in_most_recent_work.PNG)

1. Instellen van automatische bouwen en testen in VSTS:

    a. Selecteer in de projectopslagplaats **Build and Release**, en selecteer vervolgens **+ nieuw** om een nieuwe buildproces te maken.

       ![Selections for starting a new build process](./media/code-test/create_new_build.PNG)

    b. Volg de aanwijzingen om uw code bronlocatie, naam van het project, opslagplaats en vertakking informatie te selecteren.
    
       ![Source, name, repository, and branch information](./media/code-test/fill_in_build_info.PNG)

    c. Selecteer een sjabloon. Omdat er geen Python projectsjabloon, maken, starten door het selecteren van **leeg proces**. 

       ![List of templates and "Empty process" button](./media/code-test/start_empty_process_template.PNG)

    d. De naam van de build en selecteer de agent. U kunt hier de standaardwaarde als u een DSVM gebruiken wilt voor het voltooien van het bouwproces. Zie voor meer informatie over de instelling agents [bouwen en uitbrengen agents](https://docs.microsoft.com/vsts/build-release/concepts/agents/agents?view=vsts).
    
       ![Build and agent selections](./media/code-test/select_agent.PNG)

    e. Selecteer **+** in het linkerdeelvenster, een taak wordt toegevoegd voor deze build-fase. Omdat we gaan de Python-script uit te voeren **test1.py** voor het voltooien van alle controles, deze taak een PowerShell-opdracht gebruikt om het Python-code uitvoeren.
    
       !["Add tasks" pane with PowerShell selected](./media/code-test/add_task_powershell.PNG)

    f. In de details van PowerShell, vult u de vereiste gegevens, zoals de naam en versie van PowerShell. Kies **Inline Script** als het type. 
    
       In the box under **Inline Script**, you can type **python test1.py**. Make sure the environment variable is set up correctly for Python. If you need a different version or kernel of Python, you can explicitly specify the path as shown in the figure: 
    
       ![PowerShell details](./media/code-test/powershell_scripts.PNG)

    g. Selecteer **opslaan en in de wachtrij** naar het build-definitie-proces te voltooien.

       !["Save & queue" button](./media/code-test/save_and_queue_build_definition.PNG)

Nu telkens wanneer een nieuwe doorvoer wordt doorgestuurd naar de opslagplaats, wordt het bouwproces automatisch gestart. (Hier gebruiken we master als de opslagplaats, maar u kunt een vertakking definiëren.) Het proces wordt uitgevoerd de **test1.py** bestand in de agentcomputer om ervoor te zorgen dat alles gedefinieerd in de code correct wordt uitgevoerd. 

Als meldingen correct zijn ingesteld, u krijgt een bericht in e-mailbericht wanneer de build is voltooid. U kunt ook de status van de build in VSTS controleren. Als dit mislukt, kunt u de details van de build en weten welk stuk verbroken is.

![E-mailmelding van de build-geslaagd](./media/code-test/email_build_succeed.PNG)

![VSTS-melding van slagen van de build](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Volgende stappen
* Zie de [UCI inkomsten voorspelling opslagplaats](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) voor concrete voorbeelden van eenheidstests voor data science-scenario's.
* Volg de voorgaande overzicht en voorbeelden van het scenario UCI inkomsten voorspelling in uw eigen data science-projecten.

## <a name="references"></a>Verwijzingen
* [Team Data Science Process](https://aka.ms/tdsp)
* [Hulpprogramma's voor Visual Studio testen](https://www.visualstudio.com/vs/features/testing-tools/)
* [Resources voor VSTS testen](https://www.visualstudio.com/team-services/)
* [Virtuele Machines voor Datatechnologie](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)