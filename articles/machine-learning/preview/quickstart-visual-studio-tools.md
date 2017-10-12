---
title: Snelstartartikel voor Visual Studio Code-hulpprogramma's voor Machine Learning in Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u aan de slag kunt gaan met Visual Studio Code-hulpprogramma's voor Machine Learning, van het maken van een experiment, via het trainen van een model tot het operationaliseren van een webservice.
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: get-started-article
ms.date: 09/12/2017
ms.openlocfilehash: 680c1afab1af31cfef51b1c82d2db49f452ba6ab
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="visual-studio-code-tools-for-ai"></a>Visual Studio Code-hulpprogramma's voor AI
Visual Studio Code-hulpprogramma's voor AI is een ontwikkelingsextensie voor het bouwen, testen en implementeren van deep learning- en AI-oplossingen. Het zorgt voor naadloze integratie met Azure Machine Learning, met name een uitvoeringsgeschiedenisweergave, met gedetailleerde informatie over de prestaties van vorige trainingen en aangepaste metrische gegevens. Het biedt een voorbeeldverkennerweergave waarmee ontwikkelaars door voorbeelden kunnen bladeren en snel nieuwe projecten opstarten met [Microsoft Cognitive Toolkit (vroeger bekend als CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit), [Google TensorFlow](https://www.tensorflow.org) en andere deep learning-frameworks. Tot slot biedt het een verkenner voor compute-doelen, waarmee u taken kunt verzenden voor het trainen van modellen op externe omgevingen zoals Azure Virtual Machines of Linux-servers met GPU. 
 
## <a name="getting-started"></a>Aan de slag 
Om te beginnen moet u eerst [Visual Studio Code](https://code.visualstudio.com/Download) downloaden en installeren. Wanneer u Visual Studio Code open hebt, voert u de volgende stappen uit:
1. Klik op het pictogram van de extensie in de activiteitenbalk. 
2. Zoek naar "Visual Studio Code-hulpprogramma's voor AI". 
3. Klik op de knop **Installeren**. 
4. Klik na de installatie op de knop **Opnieuw laden**. 

Wanneer Visual Studio Code opnieuw is geladen, is de extensie actief. [Meer informatie over het installeren van extensies](https://code.visualstudio.com/docs/editor/extension-gallery).

## <a name="exploring-project-samples"></a>Voorbeelden van projecten verkennen
Visual Studio Code-hulpprogramma's voor AI wordt geleverd met een voorbeeldverkenner. Met de voorbeeldverkenner kunt u gemakkelijk voorbeelden ontdekken en ze met een paar klikken uitproberen. Ga als volgt te werk om de verkenner te openen:   
1. Open het opdrachtenpalet (Beeld > **Opdrachtenpalet** of **Ctrl+Shift+P**).
2. Voer "AI-voorbeeld" in. 
3. U krijgt een aanbeveling voor 'AI: Open Azure ML voorbeeldverkenner'. Selecteer deze en druk op Enter. 

U kunt ook op het pictogram van de voorbeeldverkenner klikken.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Een nieuw project maken vanuit de voorbeeldverkenner 
U kunt door verschillende voorbeelden bladeren en daar meer informatie over krijgen. We gaan bladeren totdat we het voorbeeld 'Classifying Iris' (Iris classificeren) hebben gevonden. Maak als volgt een nieuw project op basis van dit voorbeeld:
1. Klik op de installatieknop van het projectvoorbeeld en let op de voorgestelde opdrachten waarmee u door de stappen voor het maken ven een nieuw project wordt geleid. 
2. Kies een naam voor het project, bijvoorbeeld 'Iris'.
3. Kies een mappad waar u uw project wilt maken en druk op Enter. 
4. Selecteer een bestaande werkruimte en druk op Enter.

Het project wordt gemaakt.

> [!TIP]
> U moet aangemeld zijn om toegang te krijgen tot uw Azure-resource. Voer vanuit de ingesloten terminal 'az login' in en volg de instructies op. 

## <a name="submitting-experiment-with-the-new-project"></a>Experiment met het nieuwe project verzenden
Met het nieuwe project open in Visual Studio Code, verzenden we een taak naar ons andere compute-doel (lokale en virtuele machine met docker).
Visual Studio Code-hulpprogramma's voor AI biedt meerdere methoden om een experiment te verzenden. 
1. Contextmenu (klik rechts) - **AI: Taak verzenden**.
2. Vanuit het opdrachtenpalet: 'AI: Taak verzenden'.
3. U kunt de opdracht ook direct uitvoeren met behulp van Azure CLI, Machine Learning Commands, met behulp van de ingesloten terminal.

Open iris_sklearn.py, klik met de rechtermuisknop en selecteer **AI: Taak verzenden**.
1. Selecteer uw platform: 'Azure Machine Learning'.
2. Selecteer de uitvoeringsconfiguratie: 'Docker-Python'.

> [!NOTE]
> Als dit de eerste keer is dat u een taak verzendt, krijgt u een bericht met de mededeling dat er geen machine learning-configuratie is gevonden, maar dat er een wordt gemaakt. Er wordt een JSON-bestand geopend; sla dit op (**Ctrl+S**).

Wanneer de taak is verzonden, wordt de voortgang van de uitvoeringen weergegeven in de ingesloten terminal. 

## <a name="view-list-of-jobs"></a>Lijst met taken bekijken
Wanneer de taken zijn verzonden, kunt u een lijst weergeven met de taken vanuit de uitvoeringsgeschiedenis.
1. Open het opdrachtenpalet (Beeld > **Opdrachtenpalet** of **Ctrl+Shift+P**).
2. Voer 'AI-lijst' in.
3. U krijgt een aanbeveling voor 'AI: taken weergeven'. Selecteer deze en druk op Enter.
4. Selecteer het platform 'Azure Machine Learning'.

De takenlijstweergave wordt geopend en toont alle uitvoeringen met een aantal gerelateerde gegevens.

## <a name="view-job-details"></a>Taakdetails weergeven
Met de takenlijstweergave nog open, klikt u op de eerste uitvoering in de lijst.
U kunt dieper ingaan op de resultaten van een taak door op de bovenste **taak-ID** te klikken om gedetailleerde informatie te bekijken. 

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Het configureren van Azure Machine Learning voor het werken met een IDE](./how-to-configure-your-IDE.md)
