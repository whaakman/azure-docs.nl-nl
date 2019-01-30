---
title: 'Gemiddeld installatiekopieën met de API-Console: Content Moderator'
titlesuffix: Azure Cognitive Services
description: De afbeeldingen-API voor beheer in Azure Content Moderator gebruiken om te scannen en revisie toezicht werkstromen voor de inhoud van initiëren.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: f4c74e668e3f0b96f8350447424b7d6d06e34f16
ms.sourcegitcommit: 95822822bfe8da01ffb061fe229fbcc3ef7c2c19
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/29/2019
ms.locfileid: "55218730"
---
# <a name="moderate-images-from-the-api-console"></a>Gemiddeld installatiekopieën uit de API-console

Gebruik de [afbeeldingen-API voor beheer van](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) in Azure Content Moderator om te scannen en revisie toezicht werkstromen voor de inhoud van initiëren. De beheer-taak scant uw taalgebruik en vergelijkt die met de aangepaste en gedeelde zwarte lijsten.

## <a name="use-the-api-console"></a>De API-console gebruiken
Voordat u de API in de online-console uitproberen kan, moet u de abonnementssleutel van uw. Dit bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie [Overzicht](overview.md) voor meer informatie.

1.  Ga naar [installatiekopie Afbeeldingstoezicht-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c).

  De **afbeelding: Evalueer** image toezicht op pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Probeer de installatiekopie - evalueren van de selectie van de regio](images/test-drive-region.png)
  
  De **afbeelding: Evalueer** API-console wordt geopend.

3. In de **Ocp-Apim-Subscription-Key** voert u de abonnementssleutel van uw.

  ![Probeer de installatiekopie - console-abonnementssleutel evalueren](images/try-image-api-1.PNG)

4. In de **aanvraagtekst** vak, de standaardinstallatiekopie van het voorbeeld te gebruiken of geef een afbeelding te scannen. U kunt de afbeelding zelf als binair indienen bit-gegevens of een openbaar toegankelijke URL voor een afbeelding opgeven. 

  In dit voorbeeld gebruikt u het pad dat in de **aanvraagtekst** vak en selecteer vervolgens **verzenden**. 

   ![Probeer installatiekopie - console-aanvraagtekst evalueren](images/try-image-api-2.PNG)

  Dit is de installatiekopie via deze URL:

  ![Probeer de installatiekopie - console voorbeeldafbeelding evalueren](images/sample-image.jpg) 

5. Selecteer **Verzenden**.

6. De API retourneert een kans score voor elke classificatie. Retourneert ook een bepaling van de vraag of de installatiekopie voldoet aan de voorwaarden (**waar** of **false**). 

  ![Probeer de installatiekopie - console kans score evalueren en bepaling van voorwaarde](images/try-image-api-3.PNG)

## <a name="face-detection"></a>Gezichtsdetectie

De Afbeeldingstoezicht-API kunt u gezichten zoeken in een afbeelding. Deze optie kan nuttig zijn wanneer u vragen over privacy leiden hebt en wilt voorkomen dat een bepaald gezicht wordt geplaatst op uw platform. 

1.  In de [installatiekopie Afbeeldingstoezicht-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), in het menu links onder **installatiekopie**, selecteer **gezichten zoeken**. 

  De **afbeelding - gezichten zoeken** pagina wordt geopend.

2.  Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Probeer installatiekopie - selectie van gezichten pagina regio zoeken](images/test-drive-region.png)

  De **afbeelding - gezichten zoeken** API-console wordt geopend.

3. Geef een afbeelding te scannen. U kunt de afbeelding zelf als binair indienen bit-gegevens, of een openbaar toegankelijke URL naar een afbeelding opgeven. Dit voorbeeld bevat koppelingen naar een afbeelding die wordt gebruikt in een artikel CNN.

  ![Probeer de installatiekopie - voorbeeldafbeelding gezichten zoeken](images/try-image-api-face-image.jpg)

  ![Probeer installatiekopie - voorbeeld van een aanvraag gezichten zoeken](images/try-image-api-face-request.png)

4. Selecteer **Verzenden**. In dit voorbeeld de API vindt twee gezichten en retourneert de coördinaten in de afbeelding.

   ![Probeer de installatiekopie - vak met gezichten voorbeeld antwoord inhoud zoeken](images/try-image-api-face-response.png)

## <a name="text-detection-via-ocr-capability"></a>Detectie van tekst via OCR-mogelijkheid

U kunt de Content Moderator OCR-mogelijkheid gebruiken voor het detecteren van tekst in afbeeldingen.

1. In de [installatiekopie Afbeeldingstoezicht-API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), in het menu links onder **installatiekopie**, selecteer **OCR**. 

  De **afbeelding - OCR** pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Installatiekopie - OCR pagina regio selecteren](images/test-drive-region.png)

  De **afbeelding - OCR** API-console wordt geopend.

3. In de **Ocp-Apim-Subscription-Key** voert u de abonnementssleutel van uw.

4. In de **aanvraagtekst** vak, gebruikt u de standaardinstallatiekopie van het voorbeeld. Dit is dezelfde installatiekopie die wordt gebruikt in de vorige sectie.

5. Selecteer **Verzenden**. Geëxtraheerde tekst wordt weergegeven in JSON:

  ![Installatiekopie - vak van OCR voorbeeld antwoord inhoud](images/try-image-api-ocr.PNG)

## <a name="next-steps"></a>Volgende stappen

De REST-API in uw code te gebruiken of beginnen met de [installatiekopie toezicht .NET snelstartgids](image-moderation-quickstart-dotnet.md) om te integreren in uw toepassing.
