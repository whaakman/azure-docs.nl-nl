---
title: 'Gemiddeld inhoud met behulp van onlinebeoordelingen door mensen met de API-console: Content Moderator'
titlesuffix: Azure Cognitive Services
description: Bewerkingen van de beoordeling van de lees-API gebruiken om te maken van afbeelding of tekst beoordelingen voor menselijk toezicht.
services: cognitive-services
author: sanjeev3
manager: nitinme
ms.service: cognitive-services
ms.subservice: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: d963cdd484727ac46aed1915643dd92462e2c643
ms.sourcegitcommit: 90cec6cccf303ad4767a343ce00befba020a10f6
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 02/07/2019
ms.locfileid: "55857805"
---
# <a name="create-reviews-from-the-api-console"></a>Beoordelingen van de API-console maken

Gebruik van de bekijken API [bewerkingen controleren](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) afbeelding of tekst beoordelingen voor menselijk toezicht maken. Het beoordelingsprogramma menselijke moderators gebruiken om de inhoud te controleren. Deze bewerking op basis van uw bedrijfslogica na toezicht gebruiken. Gebruik deze nadat u uw inhoud met behulp van de installatiekopie van het Content Moderator of tekst-API's of andere Cognitive Services-API's hebt gescand. 

Nadat een menselijke moderator de labels automatisch toegewezen en voorspellingsgegevens controleert en een beslissing laatste toezicht verzendt, verzendt de API controleren alle gegevens naar uw API-eindpunt.

## <a name="use-the-api-console"></a>De API-console gebruiken
Als u wilt Maak een proefrit met de API met behulp van de online-console, moet u enkele waarden invoeren in de console:

- **teamName**: De teamnaam dat u hebt gemaakt bij het instellen van uw account lees-hulpprogramma. 
- **ContentId**: Deze reeks wordt doorgegeven aan de API en die zijn geretourneerd via de callback. De ContentId is handig voor het koppelen van interne id's of metagegevens met de resultaten van een taak voor beheer.
- **Metadata**: Aangepaste sleutel / waarde-paren tijdens de callback die aan uw API-eindpunt geretourneerd. Als de sleutel een korte code die is gedefinieerd in het beoordelingsprogramma is, wordt deze weergegeven als een label.
- **Ocp-Apim-Subscription-Key**: Zich op de **instellingen** tabblad. Zie [Overzicht](overview.md) voor meer informatie.

De eenvoudigste manier om toegang tot een testconsole is van de **referenties** venster.

1.  In de **referenties** venster [revisie API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  De **controleren - maken** pagina wordt geopend.

2.  Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.

  ![Controleren - pagina Regioselectie maken](images/test-drive-region.png)

  De **controleren - maken** API-console wordt geopend.
  
3.  Geef waarden op voor de vereiste queryparameters, inhoudstype en de abonnementssleutel van uw. In de **aanvraagtekst** geeft u de inhoud (bijvoorbeeld installatiekopie-locatie), metagegevens en andere informatie die is gekoppeld aan de inhoud.

  ![Controleren - console queryparameters, headers en -vak voor hoofdtekst van aanvraag](images/test-drive-review-1.PNG)
  
4.  Selecteer **Verzenden**. De ID van een toegangsbeoordeling is gemaakt. Kopieer deze ID in de volgende stappen gebruiken.

  ![Controleren - console antwoord inhoud vak geeft de revisie-ID maken](images/test-drive-review-2.PNG)
  
5.  Selecteer **ophalen**, en open vervolgens de API door de knop die overeenkomt met uw regio te selecteren. Voer op de resulterende pagina, de waarden voor **teamName**, **ReviewID**, en **abonnementssleutel**. Selecteer de **verzenden** knop op de pagina. 

  ![Controleren - console Get resultaten maken](images/test-drive-review-3.PNG)
  
6.  Hier ziet u de resultaten van de scan.

  ![Controleren - vak console antwoord inhoud maken](images/test-drive-review-4.PNG)
  
7.  Selecteer op het Dashboard met Content Moderator, **revisie** > **installatiekopie**. De installatiekopie die u hebt gescand wordt weergegeven, klaar te maken voor menselijke beoordeling.

  ![Controleren of hulpprogramma installatiekopie van een voetbal](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Volgende stappen

De REST-API in uw code te gebruiken of beginnen met de [Snelstartgids voor .NET beoordelingen](moderation-reviews-quickstart-dotnet.md) om te integreren in uw toepassing.
