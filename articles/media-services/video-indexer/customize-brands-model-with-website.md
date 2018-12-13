---
title: De website van de Video Indexer gebruiken voor het aanpassen van een model merken - Azure
titlesuffix: Azure Media Services
description: In dit artikel laat zien hoe een merken model met de Video Indexer-website aanpassen.
services: media-services
author: anikaz
manager: johndeu
ms.service: media-services
ms.topic: article
ms.date: 12/03/2018
ms.author: anzaman
ms.openlocfilehash: 2522ede85c290fa238c0d5a5604d2dfbab984cdc
ms.sourcegitcommit: 7fd404885ecab8ed0c942d81cb889f69ed69a146
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/12/2018
ms.locfileid: "53285289"
---
# <a name="customize-a-brands-model-with-the-video-indexer-website"></a>Een model merken met de Video Indexer-website aanpassen

Video Indexer biedt ondersteuning voor detectie merk van spraak en tekst visual tijdens het indexeren en indexeren van video en audio-inhoud. De mogelijkheid voor het detecteren van merk vermeldingen van producten, services, identificeert en bedrijven voorgesteld door de Bing merken database. Bijvoorbeeld, als Microsoft wordt vermeld in een video of audio-inhoud of als deze weergegeven in de visual tekst in een video wordt, Video Indexer wordt dit gedetecteerd als een onderdeel in de inhoud. Een aangepast merken-model kunt u aangeven of u Video Indexer merken van de Bing detecteert merken database, uitsluiten van bepaalde merken wordt gedetecteerd (in wezen het maken van een zwarte lijst met merken) en merken die deel van uw model uitmaken moeten bevatten die mogelijk niet in de database van de Bing merken (in wezen het maken van een technisch overzicht van merken).

Zie voor een gedetailleerd overzicht [overzicht](customize-brands-model-overview.md).

U kunt de Video Indexer-website wilt maken, gebruikt en aangepaste merken modellen gedetecteerd in een video bewerken, zoals beschreven in dit onderwerp. U kunt ook de API, zoals beschreven in [aanpassen merken model met behulp van API's](customize-brands-model-with-api.md).

## <a name="edit-the-settings-of-the-brands-model"></a>De instellingen van het model merken bewerken  

U hebt de mogelijkheid om in te stellen of er gewenste merken uit de database van de Bing merken worden gedetecteerd. Hiervoor moet u de instellingen van uw model merken bewerken.

1. Ga naar de [Video Indexer](https://www.videoindexer.ai/)-website en meld u aan.
2. Voor het aanpassen van een model in uw account, klikt u op de **modelaanpassing inhoud** knop in de rechterbovenhoek van de pagina.
 
   ![Inhoudsmodel aanpassen](./media/content-model-customization/content-model-customization.png) 
3. Als u wilt bewerken merken, selecteer de **merken** tabblad.

    ![Merken model aanpassen](./media/customize-brand-model/customize-brand-model.png)
4. Controleer de **merken voorgesteld door Bing weergeven** optie, als u wilt gebruiken voor Video Indexer om op te nemen merken voorgesteld door Bing. Laat de optie uitgeschakeld als u niet wilt dat Video Indexer voor het detecteren van merken voorgesteld door Bing in uw inhoud. 

## <a name="include-brands-in-the-model"></a>Merken opnemen in het model

De **opnemen merken** sectie geeft de aangepaste merken die u wilt gebruiken voor Video Indexer om te detecteren, zelfs als ze niet worden voorgesteld door Bing.  

### <a name="add-a-brand"></a>Een onderdeel toevoegen

1. Klik op ' + branding toevoegen '.

    ![Merken model aanpassen](./media/customize-brand-model/add-brand.png)

    Geef een naam (vereist), een categorie (optioneel), een beschrijving (optioneel) en verwijzen naar de URL in (optioneel).
    Het categorieveld is bedoeld om u te helpen u uw merken taggen. Dit veld wordt weergegeven als van het merk *tags* bij het gebruik van de Video Indexer-API's. Bijvoorbeeld, worden het merk 'Azure' gemarkeerd of aangemerkt als 'Cloud'.

    Het veld van de referentie-URL mag referentie-website voor het merk, zoals een koppeling naar de Wikipedia-pagina.
2. Klik op 'Toevoegen merk' en u ziet dat het merk is toegevoegd aan de **opnemen merken** lijst.

### <a name="edit-a-brand"></a>Een merk bewerken

1. Klik op het potloodpictogram naast de merken die u wilt bewerken.

    U kunt de categorie, de beschrijving of de referentie-URL van een merk bijwerken. U kunt de naam van een merk niet wijzigen omdat de namen van merken, uniek zijn. Als u nodig hebt om de naam van het merk te wijzigen, verwijdert u het hele merk (Zie volgende sectie) en maakt u een nieuw onderdeel met de nieuwe naam.
2. Klik op de **bijwerken** knop het merk bijwerken met de nieuwe informatie.

### <a name="delete-a-brand"></a>Een onderdeel verwijderen

1. Klik op het prullenbakpictogram naast de merken die u wilt verwijderen.
2. Klik op 'Verwijderen' en het merk wordt niet meer weergegeven uw *opnemen merken* lijst.

## <a name="exclude-brands-from-the-model"></a>Merken uitsluiten van het model

De **uitsluiten merken** sectie vertegenwoordigt de merken die u wilt gebruiken voor Video Indexer niet om te detecteren.

### <a name="add-a-brand"></a>Een onderdeel toevoegen

1. Klik op ' + branding toevoegen '.

    Geef een naam (vereist), categorie (optioneel).
2. Klik op 'Toevoegen merk' en u ziet dat het merk is toegevoegd aan de *uitsluiten merken* lijst.

### <a name="edit-a-brand"></a>Een merk bewerken

1. Klik op het potloodpictogram naast de merken die u wilt bewerken.

    U kunt alleen de categorie van een merk bijwerken. U kunt de naam van een merk niet wijzigen omdat de namen van merken, uniek zijn. Als u nodig hebt om de naam van het merk te wijzigen, verwijdert u het hele merk (Zie volgende sectie) en maakt u een nieuw onderdeel met de nieuwe naam.
2. Klik op de **bijwerken** knop het merk bijwerken met de nieuwe informatie.

### <a name="delete-a-brand"></a>Een onderdeel verwijderen

1. Klik op het prullenbakpictogram naast de merken die u wilt verwijderen.
2. Klik op 'Verwijderen' en het merk wordt niet meer weergegeven uw *uitsluiten merken* lijst.

## <a name="next-steps"></a>Volgende stappen

[Pas merken model met behulp van API 's](customize-brands-model-with-api.md)
