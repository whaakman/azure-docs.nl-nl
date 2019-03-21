---
title: Gebruik Visual Studio Code met
titleSuffix: Azure Machine Learning service
description: Informatie over het installeren van Azure Machine Learning voor Visual Studio Code en een eenvoudig experiment maken in Azure Machine Learning.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: shwinne
author: swinner95
ms.date: 12/04/2018
ms.custom: seodec18
ms.openlocfilehash: 5e7e405bdb251bbebc5da81253eea2414b434cb8
ms.sourcegitcommit: 2d0fb4f3fc8086d61e2d8e506d5c2b930ba525a7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 03/18/2019
ms.locfileid: "57838307"
---
# <a name="get-started-with-azure-machine-learning-for-visual-studio-code"></a>Aan de slag met Azure Machine Learning voor Visual Studio Code

In dit artikel leert u hoe u voor het installeren van de **Azure Machine Learning voor Visual Studio Code** extensie en uw eerste experiment te maken met Azure Machine Learning-service in Visual Studio Code (VS-Code).

Gebruik de Azure Machine Learning-extensie in Visual Studio code de Azure Machine Learning-service gebruiken voor voorbereiden van uw gegevens, trainen en testmachine learning-modellen op lokale en externe compute-doelen, de modellen te implementeren en bijhouden van aangepaste metrische gegevens en experimenten.

## <a name="prerequisites"></a>Vereisten


+ Als u nog geen Azure-abonnement hebt, maakt u een gratis account voordat u begint. Probeer nog vandaag de [gratis of betaalde versie van de Azure Machine Learning Service](https://aka.ms/AMLFree).

+ Visual Studio Code moet worden geïnstalleerd. VS Code is een toegankelijke, krachtige broncode-editor die wordt uitgevoerd op het bureaublad. Het wordt geleverd met ingebouwde ondersteuning voor Python en meer.  [Informatie over het installeren van VS Code](https://code.visualstudio.com/docs/setup/setup-overview).

+ [Python 3.5 of hoger installeren](https://www.anaconda.com/download/).


## <a name="install-the-azure-machine-learning-for-vs-code-extension"></a>De Azure Machine Learning voor VS Code-extensie installeren

Wanneer u installeert de **Azure Machine Learning** extensie, twee meer uitbreidingen worden automatisch geïnstalleerd (als u toegang tot internet hebt). Ze zijn het [Azure-Account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extensie en de [Microsoft Python](https://marketplace.visualstudio.com/items?itemName=ms-python.python) extensie

Als u wilt werken met Azure Machine Learning, moeten we VS Code omzetten in een Python IDE. Werken met [Python in Visual Studio Code](https://code.visualstudio.com/docs/languages/python), vereist de Microsoft Python-extensie, die automatisch wordt geïnstalleerd met de Azure Machine Learning-extensie. De extensie voor VS Code maakt een uitstekende IDE, en werkt in elk besturingssysteem met tal van Python-interpreters van. Het maakt gebruik van alle van de kracht van VS Code voor het automatisch aanvullen en IntelliSense, linten, foutopsporing en Eenheidstesten, naast de mogelijkheid om eenvoudig schakelen tussen de Python-omgevingen, met inbegrip van virtuele en conda-omgevingen. Bekijk deze rondleiding door bewerkt, wordt uitgevoerd, en Python-code opsporen van fouten, Zie de [Python Hello World-zelfstudie](https://code.visualstudio.com/docs/python/python-tutorial)

**De Azure Machine Learning-extensie installeren:**

1. Start VS Code.

1. In een browser, gaat u naar: [Azure Machine Learning voor Visual Studio Code (Preview)](https://aka.ms/vscodetoolsforai) extensie

1. Klik in die webpagina op **installeren**. 

1. Klik op het tabblad extensie **installeren**.

1. Er wordt een Welkom tabblad geopend in VS Code voor de extensie en het Azure-symbool wordt toegevoegd aan de activiteitenbalk.

   ![Azure-pictogram in de activiteitenbalk Visual Studio Code](./media/vscode-tools-for-ai/azure-activity-bar.png)

1. In het dialoogvenster, klikt u op **aanmelden** en volg de aanwijzing op het scherm om te verifiëren met Azure. 
   
   De extensie Azure-Account, die samen met de Azure Machine Learning voor VS Code-extensie is geïnstalleerd, kunt u verifiëren met uw Azure-account. Zie de lijst met opdrachten in de [Azure-accountextensie](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) pagina.

> [!Tip] 
> Bekijk de [IntelliCode-extensie voor Visual Studio Code (preview)](https://go.microsoft.com/fwlink/?linkid=2006060). IntelliCode biedt een verscheidenheid aan functies AI-ondersteuning voor IntelliSense in Python, zoals het afleiden van de meest relevante automatisch aanvullen op basis van de huidige context van de code.

## <a name="azure-ml-sdk-installation"></a>Azure ML-SDK-installatie

1. Zorg ervoor dat Python 3.5 of hoger is geïnstalleerd en wordt herkend door VS Code. Als u deze nu installeren, start VS Code opnieuw op en selecteert u een Python-interpreter met behulp van instructies op https://code.visualstudio.com/docs/python/python-tutorial.

1. Geef de Python-interpreter te gebruiken in de geïntegreerde terminal-venster, of druk je op **Enter** gebruiken de standaard Python-interpreter.

   ![Kies de-interpreter](./media/vscode-tools-for-ai/python.png)

1. In de rechterbenedenhoek van het venster, wordt een melding weergegeven dat aangeeft dat de SDK van Azure ML automatisch wordt geïnstalleerd.    Een lokale persoonlijke Python-omgeving is gemaakt met de Visual Studio Code-vereisten voor het werken met Azure Machine Learning.

   ![Azure Machine Learning-SDK voor Python installeren](./media/vscode-tools-for-ai/runtimedependencies.png)

## <a name="get-started-with-azure-machine-learning"></a>Aan de slag met Azure Machine Learning

Voordat u training en implementeren van machine learning-modellen met behulp van VS Code, die u wilt maken een [werkruimte van Azure Machine Learning-service](concept-azure-machine-learning-architecture.md#workspace) in de cloud om uw modellen als hulpbronnen bevatten. Leer hoe u een te maken en uw eerste experiment maken in deze werkruimte.

1. Klik op het Azure-pictogram in de activiteitenbalk Visual Studio Code. De Azure Machine Learning-zijbalk wordt weergegeven.

   [![Installeren](./media/vscode-tools-for-ai/CreateaWorkspace.gif)](./media/vscode-tools-for-ai/CreateaWorkspace.gif#lightbox)


1. Met de rechtermuisknop op uw Azure-abonnement en selecteer **werkruimte maken**. Er wordt een lijst weergegeven. Naam van het abonnement is 'Gratis' en de werkruimte is 'TeamWorkspace' in de animatie. 

1. Selecteer een bestaande resourcegroep in de lijst of maak een nieuwe met behulp van de wizard in de Command Palette.

1. Typ in het veld een unieke en duidelijke naam voor uw nieuwe werkruimte. De werkruimte is de naam 'TeamWorkspace' in de schermafbeeldingen.

1. Druk op enter en de nieuwe werkruimte is gemaakt. Deze wordt weergegeven in de structuurweergave onder de naam van het abonnement.

1. Met de rechtermuisknop op het knooppunt van het Experiment en kies **Experiment maken** in het contextmenu.  Experimenten bijhouden van uw uitvoerbewerkingen met Azure Machine Learning.

1. Voer in het veld een naam in uw experiment. In de schermafbeeldingen, het experiment is met de naam 'MNIST'.
 
1. Druk op enter en wordt de nieuw experiment gemaakt. Deze wordt weergegeven in de structuurweergave onder de Werkruimtenaam van de.

1. U kunt met de rechtermuisknop op een Experiment in een werkruimte en selecteer 'Instellen als actieve Experiment'. De **'Active'** experiment is het experiment u momenteel gebruikt en uw map openen in VS Code wordt gekoppeld aan dit experiment in de cloud. Deze map moet uw lokale Python-scripts bevatten.

   Elk van uw experiment nu met uw experiment uitgevoerd, zodat al uw belangrijke metrische gegevens worden opgeslagen in de geschiedenis van het experiment en de modellen die u training krijgen automatisch worden geüpload naar Azure Machine Learning en opgeslagen in uw experiment metrische gegevens en Logboeken.

   [![Koppelen van een map in VS Code](./media/vscode-tools-for-ai/CreateAnExperiment.gif)](./media/vscode-tools-for-ai/CreateAnExperiment.gif#lightbox)

### <a name="use-keyboard-shortcuts"></a>Sneltoetsen gebruiken

De Azure Machine Learning-functies in VS Code zijn, zoals de meeste van VS Code, toegankelijk is vanaf het toetsenbord. De belangrijkste toetsencombinatie om te weten is Ctrl + Shift + P, vindt u de Command Palette. Hier kunt hebt u toegang tot alle functionaliteit van VS Code, met inbegrip van sneltoetsen voor de meest voorkomende bewerkingen.

[![Sneltoetsen voor Azure Machine Learning voor VS Code](./media/vscode-tools-for-ai/commands.gif)](./media/vscode-tools-for-ai/commands.gif#lightbox)

## <a name="next-steps"></a>Volgende stappen

U kunt nu Visual Studio Code gebruiken om te werken met Azure Machine Learning.

Meer informatie over het [maken van de compute-doelen, trainen en implementeren van modellen in Visual Studio Code](how-to-vscode-train-deploy.md).
