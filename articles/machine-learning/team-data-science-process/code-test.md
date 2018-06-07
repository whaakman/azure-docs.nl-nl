---
title: Gegevens wetenschappelijke code testen in Azure met UCI volwassenen inkomsten voorspelling gegevensset - Team gegevens wetenschap proces en Visual Studio Team Services
description: Gegevens wetenschappelijke code testen met UCI volwassenen inkomsten voorspelling gegevens
services: machine-learning, team-data-science-process
documentationcenter: ''
author: weig
manager: deguhath
editor: cgronlun
ms.assetid: b8fbef77-3e80-4911-8e84-23dbf42c9bee
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/19/2018
ms.author: weig
ms.openlocfilehash: f9465eadfea97ffb721a2bac2c9845460d566fd1
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/01/2018
ms.locfileid: "34655159"
---
# <a name="data-science-code-testing-with-the-uci-adult-income-prediction-dataset"></a>Gegevens wetenschappelijke code testen met de UCI volwassenen inkomsten voorspelling gegevensset
Dit artikel bevat voorlopige richtlijnen voor het testen van de code in een wetenschappelijke werkstroom. De tests biedt gegevenswetenschappers een systematische en efficiënte manier om de kwaliteit en het verwachte resultaat van de code. We gebruiken een Team gegevens wetenschap proces (TDSP) [project die gebruikmaakt van de gegevensset UCI volwassenen inkomsten](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) die we eerder gepubliceerd om weer te geven hoe het testen van code kan worden gedaan. 

## <a name="introduction-on-code-testing"></a>Inleiding op code testen
'Eenheid testen' is een zet procedure voor het ontwikkelen van de software. Maar voor gegevenswetenschap, is het vaak niet duidelijk welk die middelen en hoe moet u testen code voor verschillende fasen van een levensduur van het wetenschappelijke gegevens, zoals:

* Gegevensvoorbereiding
* Data quality onderzoek
* Modelleren
* Model-implementatie 

Dit artikel vervangt de term 'eenheid testen' met "code testen." Het verwijst naar het testen van de functies die kunnen beoordelen als code voor een bepaalde stap van de levenscyclus van een wetenschappelijke gegevens levert resultaten 'zoals verwacht." De persoon die dat de test definieert wat schrijft 'zoals verwacht,' is afhankelijk van de uitkomst van de functie--bijvoorbeeld kwaliteit van gegevens controleren of model.

Dit artikel bevat verwijzingen als nuttige informatiebronnen.

## <a name="visual-studio-team-services-for-the-testing-framework"></a>Visual Studio Team Services voor het testen-framework
Dit artikel wordt beschreven hoe u en automatiseren met behulp van Visual Studio Team Services (VSTS) testen. U kunt besluiten alternatieve hulpprogramma's gebruiken. Ook laten we zien hoe een automatische build instellen met behulp van VSTS en bouwen van agents. Voor agents build gebruiken we wetenschappelijke virtuele Machines van Azure Data (DSVMs).

## <a name="flow-of-code-testing"></a>Stroom van de code testen
De algemene werkstroom testen code in een wetenschappelijke gegevensproject ziet er als volgt: 

![Stroomdiagram van de code testen](./media/code-test/test-flow-chart.PNG)

    
## <a name="detailed-steps"></a>Gedetailleerde stappen

Gebruik de volgende stappen voor het instellen en testen van code en een geautomatiseerde build uitvoeren met behulp van een agent build en VSTS:

1. Maak een project in de Visual Studio-bureaubladtoepassing:

    ![Scherm 'Nieuw project maken' in Visual Studio](./media/code-test/create_project.PNG)

   Nadat u uw project hebt gemaakt, vindt u dat in Solution Explorer in het rechterdeelvenster:
    
    ![Stappen voor het maken van een project](./media/code-test/create_python_project_in_vs.PNG)

    ![Solution Explorer](./media/code-test/solution_explorer_in_vs.PNG)

3. De projectcode van uw in de opslagplaats VSTS project code-feed: 

    ![Project code opslagplaats](./media/code-test/create_repo.PNG)

4. Stel dat u eerder hebt gedaan met sommige gegevens voorbereiding werk, zoals gegevensopname functie-engineering en label kolommen maken. U wilt controleren of dat uw code genereert de resultaten die u verwacht. Hier volgt een code die u gebruiken kunt om te controleren of de code voor de verwerking van gegevens goed werkt:

    * Controleer of de kolomnamen rechts zijn:
    
      ![Code voor het afstemmen van kolomnamen](./media/code-test/check_column_names.PNG)

    * Controleer of responsniveaus rechts zijn:

      ![Code voor de overeenkomende niveaus](./media/code-test/check_response_levels.PNG)

    * Controleer of de reactie percentage redelijke is:

      ![Code voor het percentage van de reactie](./media/code-test/check_response_percentage.PNG)

    * Controleer de ontbrekende frequentie van elke kolom in de gegevens:
    
      ![Code voor een ontbrekende snelheid](./media/code-test/check_missing_rate.PNG)


5. Nadat u hebt het verwerken van gegevens en de functie engineering werk gedaan en u een goede model hebben getraind, zorg er dan voor dat het model dat u getraind nieuwe gegevenssets kunt beoordelen correct. U kunt de volgende twee tests gebruiken om te controleren van de voorspelling niveaus en distributie van labelwaarden:

    * Controleer de voorspelling niveaus:
    
      ![Code voor het controleren van de voorspelling niveaus](./media/code-test/check_prediction_levels.PNG)

    * Controleer de distributie van de voorspelling waarden:

      ![Code voor het controleren van de voorspelling waarden](./media/code-test/check_prediction_values.PNG)

6. Alle functies samen in een pythonscript aangeroepen testen put **test_funcs.py**:

    ![Python-script voor test-functies](./media/code-test/create_file_test_func.PNG)


7. Nadat de test-codes zijn voorbereid, kunt u de testomgeving in Visual Studio kunt instellen.

   Maak een Python-bestand aangeroepen **test1.py**. In dit bestand maakt u een klasse die bevat alle tests die u wilt doen. Het volgende voorbeeld ziet u zes tests voorbereid:
    
    ![Python-bestand met een lijst met testen in een klasse](./media/code-test/create_file_test1_class.PNG)

8. Deze tests kunnen automatisch worden gedetecteerd als u **codetest.testCase** achter de klassenaam van de. Test Explorer openen in het rechter deelvenster en selecteer **alles uitvoeren**. Alle tests sequentieel worden uitgevoerd en zien als de test geslaagd is.

    ![De tests worden uitgevoerd](./media/code-test/run_tests.PNG)

9. Controleer in uw code in de project-opslagplaats met behulp van de Git-opdrachten. Uw meest recente werk wordt binnenkort in VSTS vermeld.

    ![GIT-opdrachten voor het inchecken van code](./media/code-test/git_check_in.PNG)

    ![Meest recente werk in VSTS](./media/code-test/git_check_in_most_recent_work.PNG)

10. Instellen van automatische bouwen en testen in VSTS:

    a. Selecteer in de opslagplaats project **bouwen en de vrijgave van**, en selecteer vervolgens **+ nieuw** voor het maken van een nieuwe buildproces.

       ![Selecties voor het starten van een nieuwe buildproces](./media/code-test/create_new_build.PNG)

    b. Volg de aanwijzingen om uw code bronlocatie, projectnaam, opslagplaats en vertakking informatie te selecteren.
    
       ![Bron, name, -opslagplaats en vertakking informatie](./media/code-test/fill_in_build_info.PNG)

    c. Selecteer een sjabloon. Omdat er geen Python-projectsjabloon, beginnen met het selecteren **leeg proces**. 

       ![Lijst met sjablonen en een knop 'Leeg proces'](./media/code-test/start_empty_process_template.PNG)

    d. Naam van de build en selecteer de agent. U kunt hier de standaardwaarde als u een DSVM gebruiken wilt voor het voltooien van de buildproces. Zie voor meer informatie over de instelling agents [bouwen en de vrijgave van agents](https://docs.microsoft.com/en-us/vsts/build-release/concepts/agents/agents?view=vsts).
    
       ![Build en agent selecties](./media/code-test/select_agent.PNG)

    e. Selecteer **+** in het linkerdeelvenster, een taak toevoegen voor deze fase build. Omdat we gaan het Python-script uitvoeren **test1.py** voor het voltooien van alle controles uitgevoerd met deze taak is met behulp van een PowerShell-opdracht Python-code uit te voeren.
    
       ![Deelvenster 'Taken toevoegen' met PowerShell geselecteerd](./media/code-test/add_task_powershell.PNG)

    f. In de details van PowerShell, vult u de vereiste informatie, zoals de naam en versie van PowerShell. Kies **Inline-Script** als het type. 
    
       In het vak onder **Inline-Script**, typt u **python test1.py**. Controleer of dat de omgevingsvariabele juist is ingesteld voor Python. Als u een andere versie of kernel van Python nodig hebt, kunt u expliciet het pad opgeven zoals weergegeven in de afbeelding: 
    
       ![PowerShell-details](./media/code-test/powershell_scripts.PNG)

    g. Selecteer **opslaan en wachtrij** voor het voltooien van de procedure voor het maken van definitie.

       ![Knop "Opslaan & wachtrij"](./media/code-test/save_and_queue_build_definition.PNG)

Nu telkens wanneer een nieuwe doorvoer wordt doorgegeven voor de opslagplaats code, wordt het buildproces automatisch gestart. (Hier gebruiken we master als de opslagplaats, maar u kunt elke vertakking definiëren.) Het proces wordt uitgevoerd de **test1.py** bestand in de agentcomputer om ervoor te zorgen dat alles gedefinieerd in de code correct wordt uitgevoerd. 

Als er waarschuwingen zijn correct ingesteld, wordt u geïnformeerd in e-mailbericht wanneer de build is voltooid. U kunt ook de status van de build in VSTS controleren. Als dit mislukt, kunt u Controleer de details van de build en weten welke stuk verbroken is.

![E-mailmeldingen van build-geslaagd](./media/code-test/email_build_succeed.PNG)

![VSTS melding van build-geslaagd](./media/code-test/vs_online_build_succeed.PNG)

## <a name="next-steps"></a>Volgende stappen
* Zie de [UCI inkomsten voorspelling opslagplaats](https://github.com/Azure/MachineLearningSamples-TDSPUCIAdultIncome) voor concrete voorbeelden van de eenheidstests voor wetenschap gegevensscenario.
* Volg de voorgaande overzicht en voorbeelden van het scenario UCI inkomsten voorspelling in uw eigen gegevens wetenschap-projecten.

## <a name="references"></a>Verwijzingen
* [Team Data Science Process](https://aka.ms/tdsp)
* [Hulpprogramma's voor Visual Studio testen](https://www.visualstudio.com/vs/features/testing-tools/)
* [VSTS testen Resources](https://www.visualstudio.com/team-services/)
* [Gegevens wetenschappelijke virtuele Machines](https://azure.microsoft.com/services/virtual-machines/data-science-virtual-machines/)