---
title: 'Bing aangepaste zoekactie: Een aangepaste weergave definiëren | Microsoft Docs'
description: Hierin wordt beschreven hoe u een site en verticale search-services maken
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 09/28/2017
ms.author: v-brapel
ms.openlocfilehash: 8ffe3087df398d6310828e41d0c6992199fafbed
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344915"
---
# <a name="configure-your-custom-search-experience"></a>Uw aangepaste zoekervaring configureren
Een exemplaar van de aangepaste zoekactie kunt u de zoekfunctie zodanig dat alleen inhoud van websites die uw gebruikers interesseren aanpassen. In plaats van een zoekopdracht in de hele website uitvoert, doorzoekt Bing alleen het segment van het web waarin u geïnteresseerd bent.
Voor het maken van de aangepaste weergave van het web gebruikt u de aangepaste Bing-zoekopdracht [portal](https://customsearch.ai). Zie voor meer informatie over het aanmelden bij de portal [maken van uw eerste exemplaar van Bing aangepaste zoekactie](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). De portal maakt u een zoekopdracht-exemplaar waarin de domeinen, subpagina's en webpagina's die u wilt dat Bing om te zoeken en die u niet wilt dat deze om te zoeken. Naast het opgeven van de URL's van de inhoud die u kent, kunt u ook de portal voor het voorstellen van inhoud die u mogelijk wilt toevoegen aan uw weergave vragen. Hier volgen de manieren waarop u een segment van het web kunt definiëren: 

1.  Een domein. Een segment domein bevat alle inhoud in een internetdomein gevonden. Bijvoorbeeld www.microsoft.com. Als weggelaten 'www' zorgt ervoor dat de Bing ook zoeken subdomeinen van het domein. Bijvoorbeeld, als u microsoft.com opgeeft, retourneert Bing ook resultaten van support.microsoft.com of technet.microsoft.com.
2.  Subpagina. Een segment subpagina bevat alle inhoud gevonden in de subpagina en paden eronder. U kunt maximaal twee subpagina's in het pad opgeven. Bijvoorbeeld: www.microsoft.com/en-us/windows/ 
3.  Webpagina. Een segment webpagina kunt u alleen die webpagina opnemen in een aangepaste zoekactie. Desgewenst kunt u opgeven of subpagina's opnemen.

Alle domeinen, subpagina's en webpagina's die u opgeeft moet openbaar en geïndexeerde door Bing. Als u een openbare site die u wilt opnemen in de zoekopdracht bezit en Bing nog niet worden geïndexeerd, controleert u de Bing [webbeheerder documentatie](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) voor meer informatie over het ophalen van Bing het indexeren. Zie ook de documentatie van de beheerder voor meer informatie over het ophalen van Bing uw verkende site bijwerkt als de index verouderd is.

## <a name="adding-slices-to-your-custom-search"></a>Segmenten toe te voegen aan uw aangepaste zoekopdracht
Wanneer u uw exemplaar van de aangepaste zoekactie definieert, opgeven u de actieve en geblokkeerde domeinen, subpagina's en webpagina's die u wilt zoeken of zoeken niet.  

- Actieve: Een lijst met domeinen, subpagina's of op webpagina's worden in de zoekopdracht opnemen. 
- Geblokkeerd: Een lijst met het domein, subpagina's of webpagina's moeten worden uitgesloten van de zoekopdracht. De items die u blok inhoud gevonden onder de domeinen en subpagina's zijn moet weergegeven in de actieve lijst.

Voor toegang tot elke lijst, klik op de tabbladen actief en wordt geblokkeerd in uw exemplaar van de aangepaste zoekactie. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Actieve en een lijst met geblokkeerde 
Als u een segment van de website die u wilt dat Bing om te zoeken, klikt u op de **Active** tabblad en weergeven van de domeinen, subpagina's en webpagina's om te zoeken. U kunt een segment rechtstreeks toevoegen aan de lijst of meer dan één segment toevoegen door het uploaden van een tekstbestand met het pictogram uploaden.

Bestand uploaden details: 

- Uploaden van het bestand is alleen beschikbaar voor segmenten toe te voegen aan de actieve lijst, u deze segmenten toevoegen aan de lijst met geblokkeerde niet gebruiken. 
- Maak een tekstbestand en geef een enkel domein, subpagina of webpagina per regel. Het uploaden van de hele wordt geweigerd als er een fout optreedt. 
- Als de lijst met geblokkeerde bevat het domein, de subpagina of de webpagina die u hebt opgegeven in het uploadbestand, wordt de service wordt deze verwijderd uit de lijst met geblokkeerde en voegt het toe aan de actieve lijst. 
- De service negeert duplicaten in het uploadbestand.

Als u wilt bewerken of verwijderen van segmenten, gebruik de opties onder de kolom besturingselementen. 

Op deze manier kunt u segmenten toevoegen aan de lijst geblokkeerd (behalve wanneer u een uploadbestand niet gebruiken om op te geven van de segmenten).

## <a name="pinned-list"></a>Lijst met vastgehouden
De portal kunt u ook een specifieke webpagina op de bovenkant van het zoekresultaat vast als de gebruiker een bepaalde zoekterm invoert. De **vastgehouden** tabblad bevat een lijst met query term / webpagina-paren die de webpagina die wordt weergegeven als het eerste resultaat voor een specifieke query opgeven. De zoekterm van de gebruiker moet exact overeenkomen met de queryterm vastgemaakt.
Zie voor meer informatie over het vastmaken van resultaten [positie aanpassen](#adjustrank).

Resultaten vastmaken is niet beschikbaar voor de installatiekopie zoekfunctie.

## <a name="site-suggestions"></a>Suggesties voor site
Na het toevoegen van segmenten aan de lijst met actieve, genereert de service site en subpagina suggesties die u kunt toevoegen aan uw zoekopdracht. De **u wilt toevoegen** sectie bevat de suggesties. De instellingenpagina exemplaar bevat deze sectie alleen als suggesties beschikbaar. 

Suggesties toevoegen aan de lijst met actieve: klik op het pictogram +.  Aangezien de service wordt gegenereerd op basis van uw instellingen suggesties, moet u op **vernieuwen** na het toevoegen van de suggesties. 

## <a name="preview-pane"></a>Voorbeeldvenster
U kunt testen uit uw search-exemplaar met het voorbeeldvenster aan de rechterkant zoekopdrachten en resultaten te bekijken. Selecteer **mijn exemplaar**, selecteert u een veilige zoekfilter en wat de markt om te zoeken (Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters). Voer een query en druk op enter, of klik op het zoekpictogram om de resultaten van de huidige configuratie weer te geven. Web-resultaten klikt u op **Web**, klikt u op de installatiekopie-resultaten te zien **installatiekopie**. 

 Het voorbeeldvenster gebruikt, kunt u ook Bing resultaten bekijken door te selecteren **Bing** in plaats van **mijn exemplaar**. Dit is nuttig om de resultaten van de zoekfunctie om de resultaten van Bing te vergelijken.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Positie aanpassen
De portal kunt u aanpassen om te bewerken van de resultaten die Bing retourneert positie. Geef een zoekterm in het voorbeeldvenster en voer de query. Het voorbeeldvenster lijst van de zoekresultaten voor de query. Aan de rechterkant van elk resultaat de lijst is van aanpassingen, kunt u. 

- Blok. Hiermee wordt het domein, de subpagina of de webpagina aan de lijst met geblokkeerde verplaatst. U selecteert het niveau om te blokkeren. Bing uitsluiten van inhoud van de geselecteerde site in de zoekresultaten. 
- Versterking. Verhoogt de inhoud van het domein of subpagina hoger in de zoekresultaten. U selecteren of te verbeteren van de inhoud van het domein of subpagina dat de webpagina die deel uitmaakt.
- Degraderen. Selecteert, wordt de inhoud van het domein of subpagina lager in de zoekresultaten. U selecteren of te degraderen van de inhoud van het domein of subpagina dat de webpagina die deel uitmaakt. 
- Pincode naar boven. Definieer de webpagina die wordt weergegeven aan de bovenkant van de resultaten als de gebruikersquery term exact overeenkomt met de zoekterm die u hebt gebruikt. De actieve lijst heeft geen bevat de webpagina die u kunt vastmaken. 

Aanpassen van de positie is niet beschikbaar voor de installatiekopie zoekfunctie.

## <a name="boosting-and-demoting"></a>Versterking en degraderen
U kunt super verhogen, verhogen, of degraderen van een domein of in de lijst met actieve subpagina. Standaard worden alle segmenten toegevoegd met hetzelfde gewicht. Items die Super boosted of Boosted zijn hoger gerangschikt in de zoekresultaten (met super versterking ranking hoger is dan versterking). Items die zijn gedegradeerd zijn lager in de zoekresultaten gerangschikt.

Het is belangrijk te weten dat super verhogen, de en geven gewicht VARIANT's naar de domeinen of subpagina's degraderen. Dit is slechts een van de vele signalen gebruikt door de ranker om de volgorde van de resultaten te bepalen. Dit betekent dat de gevolgen voor een specifieke query kan niet worden gegarandeerd als veel andere factoren mogelijk van invloed op de algemene volgorde van de webresultaten.  Om te bepalen van de mogelijke gevolgen heeft die versterking of degraderen op de ranker, test de zoekfunctie met behulp van het voorbeeldvenster.

U kunt super verhogen, verhogen, of items degraderen met behulp van de besturingselementen rangschikking aanpassen in de actieve lijst of met behulp van de versterking en besturingselementen in het voorbeeldvenster degraderen. De service wordt het segment wordt toegevoegd aan de lijst met actieve en past u de volgorde dienovereenkomstig.

Super verhogen, de en degraderen wijzigingen worden automatisch opgeslagen en direct weer voor het eindpunt van uw aangepaste zoekactie. 

Super verhogen, de en degraderen zijn niet beschikbaar voor de installatiekopie zoekfunctie.

## <a name="pin-to-top"></a>Vastmaken aan de bovenkant
Als u wilt een webpagina op de bovenkant van de zoekresultaten voor een specifieke query vastmaken, moet u een van de volgende opties kiezen:

1.  Voer de URL van de webpagina als u wilt vastmaken aan de bovenkant van de resultaten en de exacte query die de vastmaken activeren in het tabblad vastgehouden. 
2.  Voer een zoekterm in het voorbeeldvenster en klik vervolgens op zoeken. Identificeer vervolgens de webpagina in de resultaten die u vastmaken aan de bovenkant van de resultaten wilt als de gebruiker dezelfde query voert. Klik vervolgens op pincode naar boven. De service wordt de webpagina en de query toegevoegd aan de lijst met vastgehouden. 

U kunt uw pincodes op het tabblad vastgehouden bijhouden. De pincodes worden weergegeven als\<query\>, \<webpagina\>' paren. 

De webpagina is vastgemaakt alleen als de gebruiker query exact overeenkomt met uw query. 

U kunt maximaal één webpagina op de bovenkant van de resultaten vastmaken voor een specifieke query.

Pincodes zijn niet beschikbaar voor de installatiekopie zoekfunctie.

## <a name="use-bing-to-specify-slices"></a>Gebruik Bing om op te geven van segmenten
Er zijn verschillende manieren om op te geven van de segmenten van de website die gezamenlijk uw aangepaste zoekopdracht. Als u de segmenten die u wilt opnemen in uw exemplaar weet, kunt u deze aan uw exemplaar actieve lijst toevoegen. Zie voor informatie over het toevoegen van items aan de lijst met actieve zelf, [actieve en geblokkeerd lijsten](#active-and-blocked-lists).
Maar als u niet zeker weet welke segmenten om op te nemen, kunt u Bing query's uitvoeren in het voorbeeldvenster en Zie wat Bing retourneert. Vervolgens kunt u de segmenten die u wilt opnemen in uw aangepaste zoekopdracht. Zo moet u waarschijnlijk meerdere querytermen om ervoor te zorgen dat u alle segmenten die u voor uw exemplaar wilt identificeren uitvoeren. 

Volg deze stappen voor het gebruik van Bing segmenten toevoegen aan uw aangepaste zoekactie-exemplaar. 
1.  Aanmelden bij Bing aangepaste zoekactie [portal](https://customsearch.ai).
2.  Geen exemplaar maken of Selecteer een exemplaar om bij te werken.
3.  Selecteer in het voorbeeldvenster aan de rechterkant Bing uit de vervolgkeuzelijst.
4.  Voer een zoekterm die relevant is voor uw exemplaar in het zoekvak.
5.  Klik op **site toevoegen** naast het resultaat dat u wilt opnemen.
6.  Klik op de knop **OK**.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Statistieken weergeven
Als u zich hebt geabonneerd aangepaste zoekactie op het juiste niveau (Zie de [prijzen van pagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), een **statistieken** tabblad toegevoegd aan uw productie-exemplaren. Het tabblad statistieken bevat gedetailleerde informatie over hoe uw aangepaste zoekactie eindpunten zijn gebruikt, inclusief Belvolume, top-query's, geografische verdeling, reactiecodes en veilig zoeken. U kunt details met de opgegeven besturingselementen filteren.

## <a name="understanding-quota"></a>Understanding quotum
- Voor elk exemplaar van de aangepaste zoekactie, het maximum aantal ranking aanpassingen die u kan aanbrengen in **Active** en **geblokkeerd** segmenten is beperkt tot 400.
- Een segment toevoegen aan de actieve of geblokkeerd tabbladen telt als een ranking-aanpassing.
- Versterking en aantal degraderen als twee ranking aanpassingen.
- Voor elk exemplaar aangepaste zoekactie is het maximum aantal pincodes waardoor u mogelijk beperkt tot 200.

## <a name="next-steps"></a>Volgende stappen

- [Uw aangepaste zoekopdracht aanroepen](./search-your-custom-view.md)
- [Configureer uw gehoste UI-mogelijkheden](./hosted-ui.md)
- [Decoration markeringen gebruiken om te markeren van tekst](./hit-highlighting.md)
- [Pagina webpagina 's](./page-webpages.md)