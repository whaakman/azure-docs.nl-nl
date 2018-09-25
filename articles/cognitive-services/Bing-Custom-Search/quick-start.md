---
title: Een eerste Bing Custom Search instantie maken - Microsoft Cognitive Services
description: Voor het gebruik van Bing Custom Search, moet u een exemplaar voor aangepast zoeken die de weergave of het segment de status van het web definieert maken. Het exemplaar bevat instellingen waarmee de openbare domeinen, subsites en webpagina's die u wilt dat Bing om te zoeken naar en rangschikking van aanpassingen.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 25d622772fe47ffad001834d476e612f8c606904
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46981646"
---
# <a name="create-your-first-bing-custom-search-instance"></a>Uw eerste Bing Custom Search-exemplaar maken
Voor het gebruik van Bing Custom Search, moet u een exemplaar voor aangepast zoeken die de weergave of het segment de status van het web definieert maken. Het exemplaar bevat instellingen waarmee de openbare domeinen, websites en webpagina's die u wilt dat Bing om te zoeken naar en rangschikking van aanpassingen. Gebruik voor het maken van het exemplaar van de Bing Custom Search [portal](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Een exemplaar voor aangepast zoeken maken

Een Bing Custom Search-exemplaar maken:

1.  Een sleutel ophalen voor Custom Search-API. Zie [probeer de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Klik op de **aanmelden** knop en aanmelden bij de portal met een Microsoft-account (MSA). 
    - Als u een MSA hebt, klikt u op **maken van een Microsoft-account**. De portal gevraagd of u machtigingen voor toegang tot uw gegevens. Klik op **Ja**.
    - Ga akkoord met de Cognitive Services-voorwaarden. Controleer **ik ga akkoord** en klikt u op **akkoord**.  
3.  Na het aanmelden, klikt u op **nieuw exemplaar** en de naam van het exemplaar. Gebruik een naam die zinvol is en een beschrijving van het type inhoud dat de zoekopdracht retourneert. U kunt de naam op elk gewenst moment wijzigen. 
4.  Op de **Active** tabblad onder **zoekervaring**, voer de URL van een of meer websites die u wilt opnemen in uw zoekopdracht.
5.  Om te bevestigen dat uw exemplaar resultaten retourneert, voert u een query in het voorbeeldvenster aan de rechterkant. Als er geen resultaten zijn, geeft u een nieuwe website. Bing retourneert resultaten alleen voor openbare websites die deze heeft geïndexeerd.
6.  Klik op **publiceren** om configuratiewijzigingen te publiceren naar productie. Wanneer u hierom wordt gevraagd, klikt u op **publiceren** om te bevestigen.
7.  Klik op **productie** > **eindpunten** en kopieer de **aangepaste configuratie-ID**. U moet deze ID door aan de Custom Search-API aanroepen.

## <a name="next-steps"></a>Volgende stappen

Blijven werken met het exemplaar voor aangepast zoeken dat u zojuist hebt gemaakt met de instructies in deze handleidingen:

- [Uw aangepaste zoekervaring configureren](./define-your-custom-view.md)
- [Aanroepen van uw aangepaste zoekopdrachten](./search-your-custom-view.md)
- [Delen van uw aangepaste zoekopdrachten](./share-your-custom-search.md)
- [Configureren van uw gehoste gebruikersinterface-ervaring](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
