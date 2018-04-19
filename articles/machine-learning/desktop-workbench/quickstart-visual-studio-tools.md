---
title: QuickStart-artikel voor Visual Studio Tools voor Machine Learning in Azure | Microsoft Docs
description: In dit artikel wordt beschreven hoe u aan de slag kunt gaan met Visual Studio Tools voor Machine Learning, van het maken van een experiment, via het trainen van een model tot het operationaliseren van een webservice.
services: machine-learning
author: ahgyger
ms.author: ahgyger
manager: haining
ms.reviewer: garyericson, jasonwhowell, mldocs
ms.service: machine-learning
ms.workload: data-services
ms.custom: mvc
ms.topic: quickstart
ms.date: 11/15/2017
ms.openlocfilehash: bbcb2ea5a7ceeb976f590393608cc29c67d9a49e
ms.sourcegitcommit: 3f33787645e890ff3b73c4b3a28d90d5f814e46c
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/03/2018
---
# <a name="visual-studio-tools-for-ai"></a>Visual Studio Tools voor AI
Visual Studio Tools voor AI is een ontwikkelingsextensie voor het bouwen, testen en implementeren van deep learning- en AI-oplossingen. Het zorgt voor naadloze integratie met Azure Machine Learning, met name een uitvoeringsgeschiedenisweergave, met gedetailleerde informatie over de prestaties van vorige trainingen en aangepaste metrische gegevens. Het biedt een voorbeeldverkennerweergave waarmee ontwikkelaars door voorbeelden kunnen bladeren en snel nieuwe projecten opstarten met [Microsoft Cognitive Toolkit (vroeger bekend als CNTK)](http://www.microsoft.com/en-us/cognitive-toolkit), [Google TensorFlow](https://www.tensorflow.org) en andere deep learning-frameworks. Tot slot biedt het een verkenner voor compute-doelen, waarmee u taken kunt verzenden voor het trainen van modellen op externe omgevingen zoals Azure Virtual Machines of Linux-servers met GPU. Het programma biedt ook toegang tot [Azure Batch AI (preview-versie)](https://docs.microsoft.com/azure/batch-ai/).
 
## <a name="getting-started"></a>Aan de slag 
U moet eerst [Visual Studio](https://www.visualstudio.com/downloads/) downloaden en installeren om aan de slag te kunnen gaan. Wanneer u Visual Studio hebt geopend, voert u de volgende stappen uit:
1. Klik op de menubalk in Visual Studio en selecteer Uitbreidingen en updates...
2. Klik op het tabblad Online en selecteer Zoeken in Visual Studio Marketplace.
3. Ga naar Visual Studio voor AI. 
3. Klik op de knop **Downloaden**. 
4. Start Visual Studio na de installatie opnieuw. 

Wanneer Visual Studio opnieuw is geladen, is de extensie actief. [Meer informatie over het zoeken naar extensies](hhttps://docs.microsoft.com/visualstudio/ide/finding-and-using-visual-studio-extensions).

> [!NOTE]
> Voor Visual Studio Tools voor AI is Visual Studio 2015 of 2017 vereist in de editie voor professionals of bedrijven. De Apple OSX-versie wordt niet ondersteund. 


## <a name="exploring-project-samples"></a>Voorbeelden van projecten verkennen
Visual Studio Tools voor AI wordt geleverd met een voorbeeldverkenner. Met de voorbeeldverkenner kunt u gemakkelijk voorbeelden ontdekken en ze met een paar klikken uitproberen. Ga als volgt te werk om de verkenner te openen:   
1. Klik in de menubalk op **AI-hulpprogramma’s**.
2. Klik op Azure Machine Learning-galerie.

Er wordt een tabblad geopend met alle Azure ML-voorbeelden.

## <a name="creating-a-new-project-from-the-sample-explorer"></a>Een nieuw project maken vanuit de voorbeeldverkenner 
U kunt door verschillende voorbeelden bladeren en daar meer informatie over krijgen. We gaan bladeren totdat we het voorbeeld 'Classifying Iris' (Iris classificeren) hebben gevonden. Ga als volgt te werk om een nieuw project te maken op basis van dit voorbeeld:
1. Klik op de knop **Installeren** in het projectvoorbeeld om een nieuw dialoogvenster te openen. 
2. Selecteer een resourcegroep, een account en een werkruimte.
3. U kunt het projecttype op Algemeen laten staan.
4. Voer een pad en een naam in voor het project, en druk vervolgens op Enter. 
5. Er wordt een dialoogvenster geopend waarin u wordt gevraagd om een oplossing op te slaan. Klik op Opslaan. 

Zodra dit is voltooid, wordt er een nieuw project geopend in een nieuwe instantie van Visual Studio. 

> [!TIP]
> U moet zijn aangemeld om toegang te krijgen tot uw Azure-resource. Voer vanuit de ingesloten terminal 'az login' in en volg de instructies op. 

## <a name="submitting-experiment-with-the-new-project"></a>Experiment met het nieuwe project verzenden
Met het nieuwe project open in Visual Studio verzendt u een taak naar een compute-doel (een lokale of virtuele machine met Docker).
Ga als volgt te werk om de taak te verzenden: 
1. Klik in Solution Explorer met de rechtermuisknop op het bestand dat u wilt verzenden en selecteer **Instellen als opstartbestand**.
2. Selecteer de naam van het project, klik er met de rechtermuisknop op en selecteer **Taak verzenden...**
3. Er wordt een nieuw dialoogvenster geopend, waarin u het cluster (of het compute-doel) kunt kiezen om het script uit te voeren.
4. Klik op **Verzenden**

Wanneer de taak is verzonden, wordt de voortgang van de uitvoeringen weergegeven in de ingesloten terminal.

## <a name="view-list-of-jobs"></a>Lijst met taken bekijken
Wanneer de taak is verzonden, kunt u een lijst met de taken weergeven vanuit de uitvoeringsgeschiedenis.
1. Klik in **Server Explorer** op **AI-hulpprogramma's**.
2. Selecteer vervolgens **Azure Machine Learning**
3. Klik op het menu **Taken**.

In Job Explorer worden alle verzonden experimenten voor dit project weergegeven. 

## <a name="view-job-details"></a>Taakdetails weergeven
Met Job Explorer geopend klikt u op de eerste uitvoering in de lijst.
Hierdoor worden de volgende panelen geladen: Taakoverzicht en Logboeken en uitvoer.

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Het configureren van Azure Machine Learning voor het werken met een IDE](./how-to-configure-your-IDE.md)
