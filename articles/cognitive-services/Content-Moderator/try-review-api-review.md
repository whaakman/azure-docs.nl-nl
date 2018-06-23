---
title: Gemiddelde van inhoud met behulp van menselijke beoordelingen in Azure inhoud beheerder | Microsoft Docs
description: Informatie over het maken van menselijke beoordelingen in de console inhoud beheerder API.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: e9faf595e65ba4475a743e4cb45919fd30fbd6e8
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344473"
---
# <a name="create-reviews-from-the-api-console"></a>Beoordelingen van de API-console maken

De bekijken API gebruiken [bewerkingen controleren](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4) afbeelding of tekst beoordelingen voor menselijke toezicht te maken. Menselijke moderators gebruiken de Controleer de inhoud van het hulpprogramma controleren. Deze bewerking op basis van uw bedrijfslogica na controle gebruiken. Deze gebruiken nadat u uw inhoud hebt gescand met behulp van de installatiekopie van de beheerder van de inhoud of tekst API's of andere cognitieve Services-API's. 

Nadat een menselijke beheerder de labels automatisch toegewezen en de gegevens van de voorspelling bekijkt en een beslissing laatste controle verstuurt, verzendt de API Controleer alle gegevens naar uw API-eindpunt.

## <a name="use-the-api-console"></a>De API-console gebruiken
Als u wilt de API test-drive met behulp van de online-console, moet u enkele waarden invoeren in de console:

- **teamName**: de teamnaam die u hebt gemaakt bij het instellen van uw account controleren-hulpprogramma. 
- **ContentId**: deze reeks is doorgegeven aan de API en geretourneerd via de retouraanroep. De ContentId is nuttig voor het koppelen van interne id's of metagegevens met de resultaten van een taak toezicht.
- **Metagegevens**: aangepaste sleutel-waardeparen die aan uw API-eindpunt worden geretourneerd tijdens de callback. Als de sleutel een korte code die is gedefinieerd in het hulpprogramma controleren is, wordt deze weergegeven als een label.
- **OCP-Apim-Subscription-Key**: zich op de **instellingen** tabblad. Zie voor meer informatie [overzicht](overview.md).

De eenvoudigste manier om toegang tot een test-console is van de **referenties** venster.

1.  In de **referenties** Selecteer [revisie API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/580519463f9b070e5c591178/operations/580519483f9b0709fc47f9c4).

  De **bekijken - maken** pagina wordt geopend.

2.  Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.

  ![Bekijk - pagina Regioselectie maken](images/test-drive-region.png)

  De **bekijken - maken** API-console wordt geopend.
  
3.  Geef waarden voor de vereiste queryparameters type inhoud en de abonnementssleutel van uw. In de **aanvraagtekst** geeft u de inhoud (bijvoorbeeld bevinden), metagegevens en andere informatie die is gekoppeld aan de inhoud.

  ![Bekijk - console queryparameters, kopteksten en aanvraag hoofdtekst vak maken](images/test-drive-review-1.PNG)
  
4.  Selecteer **Verzenden**. Een revisie-ID is gemaakt. Kopieer deze ID moet worden gebruikt in de volgende stappen uit.

  ![Bekijk - console antwoord inhoud vak geeft de revisie-ID maken](images/test-drive-review-2.PNG)
  
5.  Selecteer **ophalen**, en open vervolgens de API door de knop die overeenkomt met uw regio te selecteren. Voer de waarden voor op de resulterende pagina **teamName**, **ReviewID**, en **abonnementssleutel**. Selecteer de **verzenden** knop op de pagina. 

  ![Bekijk - Get-resultaten voor console maken](images/test-drive-review-3.PNG)
  
6.  Hier ziet u de resultaten van de scan.

  ![Bekijk - vak console antwoord inhoud maken](images/test-drive-review-4.PNG)
  
7.  Selecteer op het Dashboard inhoud beheerder **revisie** > **installatiekopie**. De gescande afbeelding wordt weergegeven, gereed voor menselijke revisie.

  ![Hulpprogramma voor de installatiekopie van een voetbal controleren](images/test-drive-review-5.PNG)

## <a name="next-steps"></a>Volgende stappen

De REST API gebruiken in uw code of starten met de [beoordelingen .NET Quick Start](moderation-reviews-quickstart-dotnet.md) integreren met uw toepassing.
