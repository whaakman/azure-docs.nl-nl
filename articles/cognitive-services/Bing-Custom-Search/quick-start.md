---
title: 'Quickstart: Uw eerste Bing Custom Search-exemplaar maken | Microsoft Docs'
titlesuffix: Azure Cognitive Services
description: Gebruik dit artikel om een aangepast Bing-exemplaar te maken waarmee u domeinen en webpagina's kunt doorzoeken die u hebt gedefinieerd.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: bing-custom-search
ms.topic: quickstart
ms.date: 06/18/2019
ms.author: aahi
ms.openlocfilehash: 6949824f598194456837544526223b823dcfc3e5
ms.sourcegitcommit: a52d48238d00161be5d1ed5d04132db4de43e076
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/20/2019
ms.locfileid: "67273347"
---
# <a name="quickstart-create-your-first-bing-custom-search-instance"></a>Quickstart: Uw eerste Bing Custom Search-exemplaar maken

Als u Bing Custom Search wilt gebruiken, moet u een exemplaar voor aangepaste zoekopdrachten maken dat uw weergave of segment van het internet definieert. Dit exemplaar bevat de openbare domeinen, websites en webpagina's die u wilt doorzoeken, plus eventuele gewenste aanpassingen van de rangschikking. 

Gebruik de [Bing Custom Search-portal](https://customsearch.ai) om het exemplaar te maken. 

![Een afbeelding van de Bing Custom Search-portal](media/blockedCustomSrch.png)

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [cognitive-services-bing-custom-search-prerequisites](../../../includes/cognitive-services-bing-custom-search-signup-requirements.md)]

## <a name="create-a-custom-search-instance"></a>Een exemplaar voor aangepaste zoekopdrachten maken

Een exemplaar voor aangepaste zoekopdrachten met Bing maken:

1. Klik op de webpagina van de [Bing Custom Search-portal](https://customsearch.ai) op **Aan de slag** en meld u aan met uw Microsoft-account.

2. Klik op **Nieuw exemplaar** en voer een beschrijvende naam in. U kunt de naam van het exemplaar overigens altijd wijzigen.
 
3. Voer op het tabblad **Active** onder **Search Experience** de URL in van een of meer websites die u wilt opnemen in de zoekopdracht. 

    > [!NOTE]
    > Bing Custom Search-exemplaren retourneren alleen resultaten voor domeinen en voor webpagina's die openbaar zijn en die zijn geïndexeerd door Bing.

4. Gebruik de rechterzijde van de Bing Custom Search-portal als u een query wilt invoeren en de zoekresultaten wilt bekijken die zijn geretourneerd door uw exemplaar van Bing Custom Search. Als er geen resultaten worden geretourneerd, probeer dan een andere URL in te voeren.  

5. Klik op **Publiceren** om uw wijzigingen naar de productieomgeving te publiceren, en werk de eindpunten van het exemplaar bij.

6.  Klik op de **productie** tabblad onder **eindpunten**, en kopieer uw **aangepaste configuratie-ID**. U hebt deze id nodig om de Custom Search-API aan te roepen waarbij u de id aan de queryparameter `customconfig=` in uw aanroepen moet toevoegen.


## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Snelstart: Uw Bing Custom Search-eindpunt aanroepen](./call-endpoint-csharp.md)
