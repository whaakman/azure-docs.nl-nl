---
title: Maak een eerste exemplaar van de Bing aangepaste zoekactie - cognitieve Microsoft-Services
description: Voor het gebruik van aangepaste Bing-zoekactie, moet u een aangepaste zoekactie-exemplaar dat de weergave of het segment van het web definieert. Het exemplaar bevat instellingen die opgeeft van de openbare domeinen, subsites en webpagina's die u wilt dat Bing om te zoeken en ranking aanpassingen.
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 05/07/2017
ms.author: v-brapel
ms.openlocfilehash: 35f0bca01de1c2087f6ae30949cca9b03192b838
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344908"
---
# <a name="create-your-first-bing-custom-search-instance"></a>Uw eerste Bing aangepaste zoekactie-exemplaar maken
Voor het gebruik van aangepaste Bing-zoekactie, moet u een aangepaste zoekactie-exemplaar dat de weergave of het segment van het web definieert. Het exemplaar bevat instellingen die opgeeft van de openbare domeinen, websites en webpagina's die u wilt dat Bing om te zoeken en ranking aanpassingen. Gebruik voor het maken van het exemplaar van de aangepaste Bing zoekactie [portal](https://customsearch.ai). 

## <a name="create-a-custom-search-instance"></a>Maak een aangepaste zoekactie-exemplaar

Een exemplaar Bing aangepaste zoekactie maken:

1.  Een sleutel voor aangepaste Search API niet ophalen. Zie [probeer cognitieve Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
2.  Aanmelden bij de portal met een Microsoft-account (MSA). Klik op de **aanmelden** knop. Als dit de eerste keer dat het portal volgen met de onderstaande aanvullende stappen, anders verder met stap 3.
    - Als u een MSA hebt, klikt u op **maken van een Microsoft-account**. De portal wordt u gevraagd om machtigingen voor toegang tot uw gegevens. Klik op **Ja**.
    - Ga akkoord met de servicevoorwaarden cognitieve. Controleer **ik ga akkoord** en klik op **akkoord**.  
3.  Na het aanmelden, klikt u op **nieuw exemplaar** en de naam van het exemplaar. Gebruik een naam die zinvol is en een beschrijving van het type inhoud dat de zoekopdracht retourneert. U kunt de naam op elk gewenst moment wijzigen. 
4.  Op de **Active** tabblad **zoekervaring**, voer de URL van een of meer sites die u wilt opnemen in uw zoekopdracht.
5.  Om te bevestigen dat uw exemplaar resultaten retourneert, voert u een query in het voorbeeldvenster aan de rechterkant. Als er geen resultaten zijn, geeft u een nieuwe site. Bing retourneert resultaten alleen voor openbare sites die het heeft geïndexeerd.
6.  Klik op **publiceren** voor het publiceren van configuratiewijzigingen naar productie. Wanneer u wordt gevraagd, klikt u op **publiceren** om te bevestigen.
7.  Klik op **productie** > **eindpunten** en kopieer de **aangepaste configuratie-ID**. U moet deze ID door aan de aangepaste zoeken-API aanroepen.

## <a name="next-steps"></a>Volgende stappen

Blijven werken met het aangepaste zoekactie-exemplaar dat u zojuist hebt gemaakt door de volgende instructies in deze handleidingen:

- [Uw aangepaste zoekervaring configureren](./define-your-custom-view.md)
- [Uw aangepaste zoekopdracht aanroepen](./search-your-custom-view.md)
- [Uw aangepaste zoekopdracht delen](./share-your-custom-search.md)
- [Configureer uw gehoste UI-mogelijkheden](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)
