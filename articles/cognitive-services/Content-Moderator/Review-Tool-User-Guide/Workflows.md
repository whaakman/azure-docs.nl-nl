---
title: Definiëren en gebruiken van werkstromen voor inhoudstoezicht - Content Moderator
titlesuffix: Azure Cognitive Services
description: U kunt de API's en Azure Content Moderator workflow designer gebruiken om aangepaste werkstromen en drempelwaarden op basis van uw inhoud beleidsregels te definiëren.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: 265d5a2bd6a256d287e6ae164e883ade783f960f
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54262401"
---
# <a name="define-test-and-use-workflows"></a>Definiëren, testen en werkstromen gebruiken

U kunt de API's en Azure Content Moderator workflow designer gebruiken om aangepaste werkstromen en drempelwaarden op basis van uw inhoud beleidsregels te definiëren.

Werkstromen 'verbinding maken' naar het Content Moderator-API met behulp van connectors. U kunt andere API's gebruiken als een connector voor die API beschikbaar is. In dit voorbeeld maakt gebruik van de Content Moderator-connector die is opgenomen in de standaardinstelling.

## <a name="browse-to-the-workflows-section"></a>Blader naar de sectie werkstromen

Op de **instellingen** tabblad **werkstromen**.

  ![Werkstromen instellen](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Een nieuwe werkstroom starten

Selecteer **werkstroom toevoegen**.

  ![Een werkstroom toevoegen](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Toewijzen van een naam en beschrijving

Naam van uw werkstroom, voer een beschrijving in en kies of de workflow afbeeldingen of tekst voert.

  ![Naam van de werkstroom en beschrijving](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>De van evaluatiecriteria ('voorwaarde') definiëren

In de volgende schermafbeelding ziet u de velden en de If-dan-anders selecties die u nodig hebt om te definiëren voor uw werkstromen. Kies een connector. In dit voorbeeld wordt **Content Moderator**. Afhankelijk van de connector die u kiest, worden de beschikbare opties voor uitvoer wijzigen.

  ![Werkstroom voorwaarde definiëren](images/ocr-workflow-step-2-condition.PNG)

Nadat u de connector en de uitvoer die u hebt gekozen, selecteert u een operator en de waarde voor de voorwaarde.

## <a name="define-the-action-to-take"></a>De actie te ondernemen definiëren

Selecteer de actie moet worden uitgevoerd en de voorwaarde om te voldoen. Het volgende voorbeeld maakt u een beoordeling van de installatiekopie, een label toegewezen `a`, gemarkeerd voor de voorwaarde die wordt weergegeven. U kunt ook meerdere voorwaarden om de resultaten die u wilt combineren. Hiermee voegt u desgewenst een alternatieve (Else)-pad.

  ![Werkstroomactie definiëren](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>De werkstroom opslaan

Ten slotte wordt de werkstroom opslaan, en noteer de Werkstroomnaam van de. U moet de naam om te beginnen een beheer-taak met behulp van de API controleren.

## <a name="test-the-workflow"></a>De werkstroom testen

Nu dat u een aangepaste werkstroom hebt gedefinieerd, kunt u deze met voorbeeldinhoud testen.

Selecteer de bijbehorende **werkstroom uitvoeren** knop.

  ![Werkstroom testen](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Bestand uploaden

Sla de [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) naar uw lokale schijf. Als u wilt testen van de werkstroom, selecteer **bestand(en) Kies** en upload de installatiekopie.

  ![Installatiekopiebestand uploaden](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>Bijhouden van de werkstroom

Bijhouden van de werkstroom als deze wordt uitgevoerd.

  ![Bijhouden van uitvoering](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>Controleer alle afbeeldingen die is gemarkeerd voor menselijk toezicht

Om te zien van de afbeelding bekijken, gaat u naar de **installatiekopie** tabblad onder **bekijken**.

  ![Afbeeldingen beoordelen](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Volgende stappen 

Voor het aanroepen van de werkstroom van code, met behulp van aangepaste werkstromen met de [ `Job` console-API-snelstart](../try-review-api-job.md) en de [.NET SDK-snelstartgids](../moderation-jobs-quickstart-dotnet.md).
