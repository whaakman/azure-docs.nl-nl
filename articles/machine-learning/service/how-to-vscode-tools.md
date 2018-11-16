---
title: Met behulp van Visual Studio Code-hulpprogramma's voor AI-extensie met Azure Machine Learning
description: Meer informatie over Visual Studio Code-hulpprogramma's voor AI en hoe u training en een machine learning en deep learning-modellen met Azure Machine Learning-service in VS Code implementeren.
services: machine-learning
ms.service: machine-learning
ms.component: core
ms.topic: conceptual
ms.author: jmartens
author: j-martens
ms.reviewer: jmartens
ms.date: 10/1/2018
ms.openlocfilehash: a298a49c5bdf28b30da05c1b6296fc211ec14c1d
ms.sourcegitcommit: a4e4e0236197544569a0a7e34c1c20d071774dd6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2018
ms.locfileid: "51713143"
---
# <a name="vs-code-tools-for-ai-get-started-with-azure-machine-learning-from-visual-studio-code"></a>VS Code-hulpprogramma's voor AI: aan de slag met Azure Machine Learning van Visual Studio Code

In dit artikel leert u over Visual Studio Code (VS-Code)-extensie, **hulpprogramma's voor AI**, en hoe u training en een machine learning en deep learning-modellen met Azure Machine Learning-service in VS Code implementeren.

Gebruik de hulpprogramma's voor AI-extensie in Visual Studio code de Azure Machine Learning-service gebruiken voor het voorbereiden van uw gegevens, trainen en testen ML-modellen op lokale en externe compute-doelen, de modellen te implementeren en bijhouden van aangepaste metrische gegevens en experimenten.

## <a name="prerequisite"></a>Vereiste

+ Visual Studio Code moet worden geïnstalleerd. VS Code is een toegankelijke, krachtige broncode-editor die wordt uitgevoerd op het bureaublad. Het wordt geleverd met ingebouwde ondersteuning voor Python en meer.  [Informatie over het installeren van VS Code](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 of hoger installeren](https://www.anaconda.com/download/).

+ Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://aka.ms/AMLfree) aan voordat u begint.

## <a name="install-vs-code-tools-for-ai-extension"></a>VS Code-hulpprogramma's voor AI-extensie installeren

Wanneer u installeert de **hulpprogramma's voor AI** extensie, twee meer uitbreidingen worden automatisch geïnstalleerd (als u toegang tot internet hebt). Ze zijn het [Azure-Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extensie en de [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) extensie

Als u wilt werken met Azure Machine Learning, moeten we VS Code omzetten in een Python IDE. Werken met [Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python), vereist de Microsoft Python-extensie, die wordt geïnstalleerd met hulpprogramma's voor AI automatisch. De extensie voor VS Code maakt een uitstekende IDE, en werkt in elk besturingssysteem met tal van Python-interpreters van. Het maakt gebruik van alle van de kracht van VS Code voor het automatisch aanvullen en IntelliSense, linten, foutopsporing en Eenheidstesten, naast de mogelijkheid om eenvoudig schakelen tussen de Python-omgevingen, met inbegrip van virtuele en conda-omgevingen. Bekijk deze rondleiding door bewerkt, wordt uitgevoerd, en Python-code opsporen van fouten, Zie de [Python Hello World-zelfstudie](https://code.visualstudio.com/docs/python/python-tutorial)

**De hulpprogramma's voor AI-extensie installeren:**

1. Start VS Code.

1. In een browser, gaat u naar: https://aka.ms/vscodetoolsforai. 

1. Klik in die webpagina op **installeren**. 

1. Klik op het tabblad extensie **installeren**.

1. Er wordt een Welkom tabblad geopend in VS Code voor de extensie en het Azure-symbool wordt toegevoegd aan de activiteitenbalk.

   ![Azure-pictogram in de activiteitenbalk Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. In het dialoogvenster, klikt u op **aanmelden** en volg de aanwijzing op het scherm om te verifiëren met Azure. 
   
   De extensie Azure-Account, die is geïnstalleerd, samen met de VS Code-hulpprogramma's voor AI, kunt u verifiëren met uw Azure-account. Zie de lijst met opdrachten in de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) pagina.

> [!Tip] 
> Bekijk de [IntelliCode-extensie voor Visual Studio Code (preview)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode biedt een verscheidenheid aan functies AI-ondersteuning voor IntelliSense in Python, zoals het afleiden van de meest relevante automatisch aanvullen op basis van de huidige context van de code.

## <a name="install-the-sdk"></a>De SDK installeren

1. Zorg ervoor dat Python 3.5 of hoger is geïnstalleerd en wordt herkend door VS Code. Als u deze nu installeren, start VS Code opnieuw op en selecteert u een Python-interpreter met behulp van instructies op https://code.visualstudio.com/docs/python/python-tutorial.

1. In VS Code, open het opdrachtenpalet **Ctrl + Shift + P**.

1. Type 'Azure ML-SDK installeren' om te vinden van de pip installeren opdracht voor de SDK. Een lokale persoonlijke Python-omgeving is gemaakt met de Visual Studio Code-vereisten voor het werken met Azure Machine Learning.
   ![Installeren](./media/vscode-tools-for-ai/install-sdk.png)

1. Geef de Python-interpreter te gebruiken in de geïntegreerde terminal-venster, of druk je op **Enter** gebruiken de standaard Python-interpreter.

   ![Installeren](./media/vscode-tools-for-ai/python.png)

## <a name="get-started-with-azure-machine-learning"></a>Aan de slag met Azure Machine Learning

Voordat u training en implementeren van ML-modellen met behulp van VS Code, die u wilt maken een [werkruimte van Azure Machine Learning-service](concept-azure-machine-learning-architecture.md#workspace) in de cloud om uw modellen als hulpbronnen bevatten. Leer hoe u een te maken en uw eerste experiment maken in deze werkruimte.

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure: Machine Learning zijbalk wordt weergegeven.

   ![Installeren](./media/vscode-tools-for-ai/createworkspace.gif)

1. Met de rechtermuisknop op uw Azure-abonnement en selecteer **werkruimte maken**. Er wordt een lijst weergegeven. Naam van het abonnement is 'OpenMind Studio' in de bewegende afbeelding en de werkruimte is 'MyWorkspace'. 

1. Selecteer een bestaande resourcegroep in de lijst of maak een nieuwe met behulp van de wizard in de command palette.

1. Typ in het veld een unieke en duidelijke naam voor uw nieuwe werkruimte. De werkruimte is de naam 'MyWorkspace' in de schermafbeeldingen.

1. Druk op enter en de nieuwe werkruimte is gemaakt. Deze wordt weergegeven in de structuurweergave onder de naam van het abonnement.

1. Met de rechtermuisknop op de naam van de werkruimte en kies **Experiment maken** in het contextmenu.  Experimenten bijhouden van uw uitvoerbewerkingen met Azure Machine Learning.

1. Voer in het veld een naam in uw experiment. In de schermafbeeldingen, het experiment is met de naam 'MNIST'.
 
1. Druk op enter en wordt de nieuw experiment gemaakt. Deze wordt weergegeven in de structuurweergave onder de Werkruimtenaam van de.

1. Met de rechtermuisknop op de naam van het experiment en kies **koppelen van een lokale map**. Deze map moet uw lokale Python-scripts bevatten. De map vervolgens gekoppeld aan het experiment in de cloud. 

   Elk van uw experiment nu met uw experiment uitgevoerd, zodat al uw belangrijke metrische gegevens worden opgeslagen in de geschiedenis van het experiment en de modellen die u training krijgen automatisch worden geüpload naar Azure Machine Learning en opgeslagen in uw experiment metrische gegevens en Logboeken.

   [![Koppelen van een map in VS Code](./media/vscode-tools-for-ai/attachfolder.gif)](./media/vscode-tools-for-ai/attachfolder.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Sneltoetsen gebruiken

De Azure Machine Learning-functies in VS Code zijn, zoals de meeste van VS Code, toegankelijk is vanaf het toetsenbord. De belangrijkste toetsencombinatie om te weten is Ctrl + Shift + P, vindt u de Command Palette. Hier kunt hebt u toegang tot alle functionaliteit van VS Code, met inbegrip van sneltoetsen voor de meest voorkomende bewerkingen.

[![Sneltoetsen voor VS Code-hulpprogramma's voor AI](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Volgende stappen

U kunt nu Visual Studio Code gebruiken om te werken met Azure Machine Learning.

Meer informatie over het [maken van de compute-doelen, trainen en implementeren van modellen in Visual Studio Code](how-to-vscode-train-deploy.md).