---
title: 'Snelstartgids: Uw eerste exemplaar voor Bing Custom Search maken'
titlesuffix: Azure Cognitive Services
description: Als u Bing Custom Search wilt gebruiken, moet u een exemplaar voor aangepaste zoekopdrachten maken dat uw weergave of segment van het internet definieert. Het exemplaar bevat instellingen die de openbare domeinen, websites en webpagina's beschrijven die u wilt doorzoeken met Bing, plus eventuele instellingen voor het aanpassen van de rangschikking.
services: cognitive-services
author: brapel
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: quickstart
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 866d32aa4de45076fcbc4e413d8c2e67d5346878
ms.sourcegitcommit: 6f59cdc679924e7bfa53c25f820d33be242cea28
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/05/2018
ms.locfileid: "48816220"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Snelstartgids: Uw eerste exemplaar voor Bing Custom Search maken
Als u Bing Custom Search wilt gebruiken, moet u een exemplaar voor aangepaste zoekopdrachten maken dat uw weergave of segment van het internet definieert. Het exemplaar bevat instellingen die de openbare domeinen, websites en webpagina's beschrijven die u wilt doorzoeken met Bing, plus eventuele instellingen voor het aanpassen van de rangschikking. Gebruik voor het maken van het exemplaar de [portal](https://customsearch.ai) van Bing Custom Search. 

## <a name="create-a-custom-search-instance"></a>Een exemplaar voor aangepaste zoekopdrachten maken

Een exemplaar voor aangepaste zoekopdrachten met Bing maken:

1.  Haal een sleutel op voor de Aangepaste zoekopdrachten-API. Zie [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Klik op de knop **Sign in** en meld u aan bij de portal met een Microsoft-account (MSA). 
    - Als u geen MSA hebt, klikt u op **Get Started**. De portal vraagt om toestemming voor toegang tot uw gegevens. Klik op **Ja**.
    - Ga akkoord met de voorwaarden van Cognitive Services. Selecteer **I agree** en klik op **Agree**.  
3.  Na het aanmelden, klikt u op **New Instance** en voert u een naam in voor het exemplaar. Gebruik een naam die zinvol is en het type inhoud beschrijft dat de zoekopdracht retourneert. U kunt de naam overigens altijd wijzigen. 
4.  Voer op het tabblad **Active** onder **Search Experience** de URL in van een of meer websites die u wilt opnemen in de zoekopdracht.
5.  Controleer of het exemplaar resultaten retourneert door een query in te voeren in het voorbeeldvenster aan de rechterkant. Als deze geen resultaten oplevert, geeft u een andere website op. Bing retourneert alleen resultaten voor openbare websites die zijn geïndexeerd door de service.
6.  Klik op **Publish** om configuratiewijzigingen te publiceren naar de productieomgeving. Wanneer u hierom wordt gevraagd, klikt u op **Publish** om te bevestigen.
7.  Klik op **Production** > **Endpoints** en kopieer de waarde voor **Custom Configuration ID**. U hebt deze id nodig om de Aangepaste zoekopdrachten-API aan te roepen.

## <a name="next-steps"></a>Volgende stappen

U kunt blijven werken met het exemplaar voor aangepaste zoekopdrachten dat u net hebt gemaakt door de instructies in deze handleidingen te volgen:

- [Aangepaste zoekervaring configureren](./define-your-custom-view.md)
- [Aangepaste zoekopdrachten aanroepen](./search-your-custom-view.md)
- [Aangepaste zoekopdrachten delen](./share-your-custom-search.md)
- [Gehoste UI-ervaring configureren](./hosted-ui.md)
- [Decoratiemarkeringen gebruiken om tekst te markeren](./hit-highlighting.md)
- [Bladeren door webpagina's](./page-webpages.md)
