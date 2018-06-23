---
title: Definieer en het gebruik van werkstromen in Azure inhoud beheerder | Microsoft Docs
description: Informatie over het maken van aangepaste werkstromen op basis van de beleidsregels van uw inhoud.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 01/07/2018
ms.author: sajagtap
ms.openlocfilehash: dfe3ba8a2ef1bcbc69ef585b504a9367d9420bf0
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344479"
---
# <a name="define-test-and-use-workflows"></a>Definiëren, testen en het gebruik van werkstromen

U kunt de API's en Azure inhoud beheerder workflow designer gebruiken om aangepaste werkstromen en drempelwaarden op basis van uw inhoud beleidsregels te definiëren.

Werkstromen "verbinding maken met' de inhoud beheerder API met behulp van connectors. U kunt andere API's gebruiken als een connector voor die API beschikbaar is. In dit voorbeeld maakt gebruik van de inhoud beheerder connector die is opgenomen in de standaardinstelling.

## <a name="browse-to-the-workflows-section"></a>Blader naar de sectie werkstromen

Op de **instellingen** tabblad **werkstromen**.

  ![Werkstromen instellen](images/2-workflows-0.png)

## <a name="start-a-new-workflow"></a>Een nieuwe werkstroom starten

Selecteer **-werkstroom toevoegen**.

  ![Een werkstroom toevoegen](images/2-workflows-1.png)

## <a name="assign-a-name-and-description"></a>Wijs een naam en beschrijving

Naam van de werkstroom, voer een beschrijving in en kies of de werkstroom afbeeldingen of tekst verwerkt.

  ![Werkstroomnaam en beschrijving](images/ocr-workflow-step-1.PNG)

## <a name="define-the-evaluation-criteria-condition"></a>Definieer de evaluatiecriteria ('voorwaarde')

In de volgende schermafbeelding ziet u de velden en de If-Then-Else selecties die u nodig hebt om te definiëren voor uw werkstromen. Kies een connector. In dit voorbeeld wordt **inhoud beheerder**. Afhankelijk van de connector die u kiest, worden de beschikbare opties voor uitvoer wijzigen.

  ![Werkstroom voorwaarde definiëren](images/ocr-workflow-step-2-condition.PNG)

Nadat u de connector en de uitvoer die u kiest, selecteert u een operator en de waarde voor de voorwaarde.

## <a name="define-the-action-to-take"></a>Definieer de actie te ondernemen

Selecteer de actie moet worden uitgevoerd en de voorwaarde om te voldoen. Het volgende voorbeeld maakt u een installatiekopie onderzoek, een label toegewezen `a`, gemarkeerd voor de voorwaarde die wordt weergegeven. U kunt ook meerdere voorwaarden om de resultaten die u wilt combineren. Voegt u desgewenst een alternatief pad voor (Else).

  ![Werkstroomactie definiëren](images/ocr-workflow-step-3-action.PNG)

## <a name="save-your-workflow"></a>De werkstroom opslaan

Ten slotte de werkstroom opslaan en noteer de naam van de werkstroom. U moet de naam niet aan een taak toezicht starten met de revisie-API.

## <a name="test-the-workflow"></a>De werkstroom testen

Nu dat u een aangepaste werkstroom gedefinieerd, kunt u deze met voorbeeldinhoud testen.

Selecteer het bijbehorende **werkstroom uitvoeren** knop.

  ![Werkstroom testen](images/ocr-workflow-step-6-list.PNG)

### <a name="upload-a-file"></a>Bestand uploaden

Sla de [voorbeeldafbeelding](https://moderatorsampleimages.blob.core.windows.net/samples/sample5.png) naar uw lokale schijf. Als u wilt testen van de werkstroom, selecteer **bestanden kiezen** en upload de installatiekopie.

  ![Installatiekopiebestand uploaden](images/ocr-workflow-step-7-upload.PNG)

### <a name="track-the-workflow"></a>Bijhouden van de werkstroom

Het bijhouden van de werkstroom zoals deze wordt uitgevoerd.

  ![Bijhouden van de werkstroom kan worden uitgevoerd](images/ocr-workflow-step-4-test.PNG)

### <a name="review-any-images-flagged-for-human-moderation"></a>Bekijk alle installatiekopieën die zijn gemarkeerd voor menselijke toezicht

Om te zien van de afbeelding bekijken, gaat u naar de **installatiekopie** tabblad onder **controleren**.

  ![Afbeeldingen beoordelen](images/ocr-sample-image-workflow1.PNG)

## <a name="next-steps"></a>Volgende stappen 

Als u wilt de werkstroom vanaf code aanroepen, met behulp van aangepaste werkstromen met de [ `Job` API console Quick Start](../try-review-api-job.md) en de [.NET SDK Quick Start](../moderation-jobs-quickstart-dotnet.md).
