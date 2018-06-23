---
title: Gemiddelde van afbeeldingen met Azure inhoud beheerder | Microsoft Docs
description: Controle van de afbeelding in de console inhoud beheerder API Test-Drive.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: fec54826c70ae10e56c68406f629c56639985295
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344500"
---
# <a name="moderate-images-from-the-api-console"></a>Gemiddeld installatiekopieën van de API-console

Gebruik de [installatiekopie toezicht API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c) in Azure inhoud beheerder om te scannen en revisie toezicht werkstromen voor de installatiekopie van inhoud initiëren. De taak toezicht scant de inhoud voor taalgebruik en vergelijkt die met aangepaste en gedeelde zwarte lijsten.

## <a name="use-the-api-console"></a>De API-console gebruiken
Voordat u kunt de API in de online-console test-drive, moet u de abonnementssleutel van uw. Dit bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie voor meer informatie [overzicht](overview.md).

1.  Ga naar [installatiekopie toezicht API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c).

  De **afbeelding: evalueren** installatiekopie toezicht wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Probeer de installatiekopie - pagina Regioselectie evalueren](images/test-drive-region.png)
  
  De **afbeelding: evalueren** API-console wordt geopend.

3. In de **Ocp-Apim-Subscription-Key** Voer de abonnementssleutel van uw.

  ![Probeer de installatiekopie - console abonnementssleutel evalueren](images/try-image-api-1.PNG)

4. In de **aanvraagtekst** vak of geef een afbeelding voor het scannen van de standaardinstallatiekopie van het voorbeeld gebruiken. U kunt de afbeelding zelf als binair indienen bit gegevens of een openbaar toegankelijke URL opgeven voor een afbeelding. 

  In dit voorbeeld gebruikt u het pad dat is opgegeven in de **aanvraagtekst** vak en selecteer vervolgens **verzenden**. 

   ![Probeer de installatiekopie - console aanvraagtekst evalueren](images/try-image-api-2.PNG)

  Dit is de installatiekopie op die URL:

  ![Probeer de installatiekopie - console voorbeeldafbeelding evalueren](images/sample-image.jpg) 

5. Selecteer **Verzenden**.

6. De API retourneert een score kans voor elke classificatie. Retourneert ook bepalen of de installatiekopie van het voldoet aan de voorwaarden (**true** of **false**). 

  ![Probeer de installatiekopie - console kans score evalueren en bepaling van voorwaarde](images/try-image-api-3.PNG)

## <a name="face-detection"></a>Gezichtsherkenning

De afbeelding toezicht API kunt u vlakken niet vinden in een afbeelding. Deze optie kan nuttig zijn wanneer u privacyproblemen hebt en wilt voorkomen dat een specifieke gezicht wordt gepost op uw platform. 

1.  In de [installatiekopie toezicht API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), in het menu links onder **installatiekopie**, selecteer **vinden bespreekt**. 

  De **afbeelding - vinden bespreekt** pagina wordt geopend.

2.  Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Probeer de installatiekopie - vlakken pagina Regioselectie vinden](images/test-drive-region.png)

  De **afbeelding - vinden bespreekt** API-console wordt geopend.

3. Geef een afbeelding om te scannen. U kunt de afbeelding zelf als binair indienen bit gegevens of geef een openbaar toegankelijke URL voor een afbeelding. Dit voorbeeld bevat koppelingen naar een afbeelding die wordt gebruikt in een artikel CNN.

  ![Probeer de installatiekopie - voorbeeldafbeelding vlakken vinden](images/try-image-api-face-image.jpg)

  ![Probeer de installatiekopie - vlakken voorbeeld van een aanvraag zoeken](images/try-image-api-face-request.png)

4. Selecteer **Verzenden**. In dit voorbeeld wordt de API twee vlakken vindt en retourneert de coördinaten in de installatiekopie.

   ![Probeer de installatiekopie - vak van vlakken voorbeeld antwoord inhoud zoeken](images/try-image-api-face-response.png)

## <a name="text-detection-via-ocr-capability"></a>Detectie van tekst via OCR mogelijkheid

U kunt de mogelijkheid inhoud beheerder OCR gebruiken voor het detecteren van tekst in de afbeeldingen.

1. In de [installatiekopie toezicht API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf753a3f9b070c105bd2c1/operations/57cf753a3f9b070868a1f66c), in het menu links onder **installatiekopie**, selecteer **OCR**. 

  De **afbeelding - OCR** pagina wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Afbeelding - OCR pagina Regioselectie](images/test-drive-region.png)

  De **afbeelding - OCR** API-console wordt geopend.

3. In de **Ocp-Apim-Subscription-Key** Voer de abonnementssleutel van uw.

4. In de **aanvraagtekst** gebruikt u de standaardinstallatiekopie van het voorbeeld. Dit is dezelfde installatiekopie die wordt gebruikt in de vorige sectie.

5. Selecteer **Verzenden**. De uitgepakte tekst wordt weergegeven in JSON:

  ![Afbeelding van-vak van OCR voorbeeld antwoord inhoud](images/try-image-api-ocr.PNG)

## <a name="next-steps"></a>Volgende stappen

De REST API gebruiken in uw code of starten met de [installatiekopie toezicht .NET Quick Start](image-moderation-quickstart-dotnet.md) integreren met uw toepassing.
