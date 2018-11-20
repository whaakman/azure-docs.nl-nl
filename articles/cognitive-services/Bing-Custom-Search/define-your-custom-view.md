---
title: Een aangepaste weergave - Bing Custom Search definiëren
titlesuffix: Azure Cognitive Services
description: Beschrijft hoe u site en verticale search-services maken
services: cognitive-services
author: aahill
manager: cgronlun
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: conceptual
ms.date: 09/28/2017
ms.author: aahi
ms.openlocfilehash: ab1b8b011a6523fe3760c233c83d59fd9768ad0c
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163297"
---
# <a name="configure-your-custom-search-experience"></a>Uw aangepaste zoekervaring configureren

Een aangepaste zoekinstantie kunt u de zoekfunctie om op te nemen alleen inhoud van websites die uw gebruikers het belangrijkst aanpassen. In plaats van een zoekopdracht in het hele web uitvoert, doorzoekt Bing alleen het segment de status van de web-waarin u bent geïnteresseerd. Als u een aangepaste weergave van het web wilt maken, gebruikt u de [portal](https://customsearch.ai) van Bing Aangepaste zoekopdrachten. Zie voor meer informatie over het aanmelden bij de portal [maken van uw eerste exemplaar van de Bing Custom Search](https://docs.microsoft.com/azure/cognitive-services/bing-custom-search/quick-start). 

De portal kunt u een search-exemplaar dat Hiermee geeft u de domeinen, subpagina's en webpagina's die u wilt dat Bing om te zoeken naar en die u niet wilt dat deze om te zoeken naar maken. Naast het opgeven van de URL's van de inhoud die u kent, kunt u ook vragen suggestie voor inhoud die u mogelijk wilt toevoegen aan uw weergave in de portal. 

Hier volgen de manieren waarop u een segment van het web kunt definiëren: 

1.  Het domein. Een segment van het domein bevat alle inhoud binnen een internetdomein gevonden. Bijvoorbeeld, www.microsoft.com. Als 'www' zorgt ervoor dat de Bing zoeken ook subdomeinen van het domein. Bijvoorbeeld, als u microsoft.com opgeeft, resultaten Bing ook uit support.microsoft.com of technet.microsoft.com.  
  
2.  (Sub). Een segment subpagina bevat alle inhoud gevonden in de (sub) en paden eronder. U kunt maximaal twee subpagina's in het pad opgeven. Bijvoorbeeld, www.microsoft.com/en-us/windows/  
  
3.  Webpagina. Een segment webpagina kunt u alleen die webpagina opnemen in een aangepast zoeken. U kunt eventueel opgeven of u wilt opnemen subpagina's.

Alle domeinen, subpagina's en webpagina's die u opgeeft moet zijn openbare en geïndexeerd door Bing. Als u eigenaar bent van een openbare site die u wilt opnemen in het zoekvak en deze nog niet worden geïndexeerd door Bing, raadpleegt u de Bing [webbeheerder documentatie](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) voor meer informatie over het ophalen van Bing het indexeren. Zie ook de documentatie van de beheerder voor meer informatie over het ophalen van Bing de verkende site bijwerkt als de index verouderd is.

## <a name="adding-slices-to-your-custom-search"></a>Segmenten toe te voegen aan uw aangepaste zoekopdrachten

Wanneer u uw aangepaste zoekinstantie definiëren, geeft u de actieve en geblokkeerde domeinen, subpagina's en webpagina's die u wilt zoeken naar of niet te zoeken.  

- Actief: Een lijst met domeinen, subpagina's of webpagina's om op te nemen in het zoekvak.  
  
- Geblokkeerd: Een lijst van het domein, subpagina's of webpagina's moeten worden uitgesloten van de zoekopdracht. De items die u blok inhoud te vinden onder de domeinen en subpagina's zijn moet die worden vermeld in de lijst met actieve.

Voor toegang tot elke lijst, klik op de tabbladen actief en wordt geblokkeerd in uw exemplaar voor aangepast zoeken. 

<a name="active-and-blocked-lists"></a>
## <a name="active-and-blocked-lists"></a>Actief en wordt een lijst met geblokkeerde 

Als u wilt een segment van de website die u wilt dat Bing om te zoeken naar opgeven, klikt u op de **Active** tabblad en de lijst met de domeinen, subpagina's en webpagina's om te zoeken. U kunt een segment rechtstreeks aan de lijst toevoegen of toevoegen van meer dan één segment door het uploaden van een tekstbestand met het uploadpictogram.

Details voor het uploaden van bestand: 

- Uploaden van het bestand is alleen beschikbaar voor segmenten toe te voegen aan de lijst met actieve, u deze segmenten toevoegen aan de lijst met geblokkeerde niet gebruiken.  
  
- Maak een tekstbestand en geef een enkel domein, subpagina of webpagina per regel. Het volledige uploadproces wordt geweigerd als er een fout optreedt.  
  
- Als de lijst met geblokkeerde bevat het domein, de subpagina of de webpagina die u hebt opgegeven in het uploadbestand, wordt de service, wordt deze verwijderd uit de lijst geblokkeerd en wordt deze toegevoegd aan de lijst met actieve.  
  
- De service negeert dubbele waarden in het uploadbestand.

Als u wilt bewerken of verwijderen van segmenten, gebruikt u de opties onder de **besturingselementen** kolom. 

Op deze manier kunt u segmenten toevoegen aan de lijst met geblokkeerde (tenzij u een uploadbestand niet gebruiken om op te geven van de segmenten).

## <a name="pinned-list"></a>Lijst met vastgehouden

In de portal kunt u een specifieke webpagina vastmaken aan de bovenkant van de zoekresultaten voor wanneer de gebruiker een specifieke zoekterm invoert. De **vastgehouden** tabblad bevat een lijst met query term en webpagina-paren die de webpagina die wordt weergegeven als het beste resultaat voor een specifieke query opgeven. Bovendien wordt alleen geïndexeerde webpagina's worden weergegeven in zoekopdrachten. Zie voor meer informatie over het vastmaken van resultaten [positie aanpassen](#adjustrank). 

Resultaten vast te maken is niet beschikbaar voor de afbeeldingen zoeken en optreedt in video's zoeken.

## <a name="website-suggestions"></a>Suggesties voor website

Na het toevoegen van segmenten aan de lijst met actieve, genereert de service-website en subpagina suggesties die u mogelijk wilt toevoegen aan uw zoekopdracht. De **u wilt toevoegen** sectie bevat de suggesties. De instellingenpagina voor het exemplaar bevat in deze sectie alleen als suggesties beschikbaar zijn. 

Als u wilt toevoegen suggesties aan uw lijst met actieve, klikt u op het pictogram +.  Omdat de service suggesties op basis van uw instellingen genereert, moet u op **vernieuwen** na het toevoegen van de suggesties. 

## <a name="preview-pane"></a>Voorbeeldweergave

U kunt testen van uw search-exemplaar met behulp van het voorbeeldvenster aan de rechterkant op zoekopdrachten en resultaten te bekijken. 

1. Selecteer **mijn instantie**
2. Selecteer een filter veilig zoeken en wat de markt om te zoeken naar (Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)).
3. Voer een query en druk op enter of klik op het zoekpictogram om de resultaten van de huidige configuratie weer te geven. 

Selecteer het type van de zoekopdracht uit te voeren, klikt u op **Web** om op te halen van webresultaten, **installatiekopie** om op te halen van de resultaten van de installatiekopie, of **Video** video resultaten ophalen. 

Met behulp van het voorbeeldvenster, u kunt ook Bing resultaten bekijken door te selecteren **Bing** in plaats van **mijn exemplaar**. Dit kan nuttig zijn om te vergelijken van resultaten van uw zoekervaring in de resultaten van Bing zijn.

<a name="adjustrank"></a>
## <a name="adjust-rank"></a>Positie aanpassen

De portal kunt u aanpassen positie voor het bewerken van de resultaten die Bing retourneert. Voer een zoekterm in het voorbeeldvenster en voer de query uit. Het voorbeeldvenster lijst het zoekresultaat voor de query. Rechts van elk resultaat de lijst is van aanpassingen, kunt u maken. 

- Blokkeren. Hiermee wordt het domein, subpagina of webpagina aan de lijst met geblokkeerde verplaatst. U selecteert het niveau te blokkeren. Bing sluit inhoud van de geselecteerde site in de lijst met zoekresultaten.  
  
- Geef een boost. Verhoogt de inhoud van het domein of de subpagina hoger in de lijst met zoekresultaten. U selecteren of te verhogen van de inhoud van het domein of subpagina dat de webpagina die deel uitmaakt. [Meer lezen](#boosting-and-demoting)  
  
- Niveau verlagen. Inhoud van het domein of lager in de lijst met zoekresultaten subpagina verlagen. U selecteren of te verlagen van de inhoud van het domein of subpagina dat de webpagina die deel uitmaakt. [Meer informatie](#boosting-and-demoting).  
  
- Pincode naar boven. Definieer de webpagina die wordt weergegeven aan de bovenkant van de resultaten als de query-tekenreeks van de gebruiker overeenkomt met dat de pincodes querytekenreeks op basis van de voorwaarde van de pincode. De lijst met actieve heeft geen bevatten van de webpagina die u kunt vastmaken. [Meer informatie](#pin-to-top).

Aanpassen van de positie is niet beschikbaar voor de afbeeldingen zoeken en optreedt in video's zoeken.

## <a name="boosting-and-demoting"></a>Versterking en degraderen

U kunt heel stimuleren, vergroten, of degraderen van een domein of in de lijst met actieve subpagina. Standaard worden alle segmenten met hetzelfde gewicht toegevoegd. Items die zijn heel heeft de klantenopbrengst of heeft de Klantenopbrengst zijn hoger gerangschikt in de lijst met zoekresultaten (met de volgorde van de super boost is hoger dan boost). Items die zijn gedegradeerd worden gerangschikt lager in de lijst met zoekresultaten.

Het is belangrijk te weten dat heel verhogen, verhogen en verlagen geven gewicht varianten met de domeinen of subpagina's. Dit is slechts een van de vele signalen die door de kerntechnologie wordt gebruikt om de volgorde van de resultaten te bepalen. Dit betekent dat hun effect voor een specifieke query kan niet worden gegarandeerd als vele andere factoren mogelijk van invloed op de algehele rangorde van de webresultaten.  Om te bepalen van de mogelijke gevolgen heeft die versterking of degraderen op de kerntechnologie, test de zoekervaring met behulp van het voorbeeldvenster.

U kunt heel stimuleren, vergroten, of degraderen van items met behulp van de besturingselementen Trefwoordenrangschikking aanpassen in de lijst met actieve of met behulp van de Boost en degraderen van besturingselementen in het voorbeeldvenster. De service wordt het segment wordt toegevoegd aan uw lijst met actieve en past u de volgorde daarvan.

Super stimuleren, verhogen en verlagen zijn niet beschikbaar voor de ervaringen van de afbeeldingen zoeken en video's zoeken.

## <a name="pin-to-top"></a>Vastmaken aan de bovenkant

Als u wilt een webpagina aan het begin van de lijst met zoekresultaten voor een specifieke query vastmaken, moet u een van de volgende opties kiezen:

1.  In de **vastgehouden** tabblad, voer de URL van de webpagina als u wilt vastmaken aan het begin van de resultaten en de query die wordt geactiveerd de vast te maken.  
  
2.  In de **Preview** in het deelvenster een queryterm en klik op zoeken. Identificeer vervolgens de webpagina in de resultaten die u vastmaken aan het begin van de resultaten wilt als de gebruiker dezelfde query voert. Klik vervolgens op **vastmaken aan de bovenkant**. De service wordt toegevoegd voor de webpagina en de query naar de **vastgehouden** lijst. 

U kunt uw pincodes in volgen de **vastgehouden** tabblad. De pincodes worden weergegeven als\<query\>, \<webpagina\>' paren. 

Voor een specifieke query, kunt u maximaal één webpagina aan het begin van de resultaten vastmaken.

Pincodes zijn niet beschikbaar voor het zoeken voor afbeeldingen en video's zoeken-ervaringen.

### <a name="specifying-the-pins-match-condition"></a>Voorwaarde voor overeenkomst van de pincode op te geven

De webpagina is vastgemaakt aan de bovenkant van de resultaten alleen als de querytekenreeks van de gebruiker overeenkomt met de pincode van de query-tekenreeks op basis van de voorwaarde van de pincode. Een pincode wordt standaard gekoppeld alleen als de query-tekenreeks van de pincode en de queryreeks van de gebruiker exact overeenkomen. U kunt het standaardgedrag wijzigen door op te geven op een van de volgende criteria voor overeenkomst.

- Begint met &mdash; de pincode is een overeenkomst als de queryreeks van de gebruiker met de querytekenreeks van de pincode begint.
- Eindigt met &mdash; de pincode is een overeenkomst als queryreeks van de gebruiker met de pincode van de query-tekenreeks eindigt.
- Bevat &mdash; de pincode is een overeenkomst als de query-tekenreeks van de gebruiker de pincode queryreeks bevat.

Als u wilt wijzigen van de pincode voorwaarde, klikt u op het bewerkingspictogram van de pincode (er ongeveer zo uitziet als een potlood). In de **Query-voorwaarde voor overeenkomst** kolom, klikt u op de vervolgkeuzelijst en selecteert u de nieuwe voorwaarde te gebruiken. Klik vervolgens op de opslagbewerking pictogram van de wijziging op te slaan.

Alle vergelijkingen zijn niet hoofdlettergevoelig.

### <a name="changing-the-order-of-the-pins"></a>De volgorde van de pincodes te wijzigen

U wijzigt de volgorde van uw pincodes, u kunt slepen en neerzetten van de pincode of u kunt bewerken van de pincode en het volgordenummer van de wijzigen. Klik op de pincode in de lijst te verplaatsen, houd de muisknop ingedrukt en sleep de pincode de pincode die u wilt vervangen te slepen en neerzetten een pincode, en laat de muisknop los. U ziet dat de volgorde van de pincode is gewijzigd.

U kunt ook de volgorde van de pincode bewerken. In de **besturingselementen** kolom, klikt u op het bewerkingspictogram van de pincode (er ongeveer zo uitziet als een potlood). Voer het nummer van de waar u de pincode weergegeven in de lijst en druk op enter of klik op Opslaan pictogram. Bijvoorbeeld, als de pincode momenteel zesde in de lijst is en u wilt dat de tweede, het ordernummer te wijzigen twee (2).

Als meerdere pincodes voldoen aan de voorwaarde voor overeenkomst, bepaalt de volgorde van de pincodes welke pincode Bing gebruikt. Bijvoorbeeld, als pincodes twee en drie voldoen aan de voorwaarde voor overeenkomst, Bing maakt gebruik van twee pincode.

## <a name="use-bing-to-specify-slices"></a>Gebruik Bing om op te geven van segmenten

Er zijn een aantal manieren om op te geven van de segmenten van de website die gezamenlijk uw aangepaste zoekopdrachten. Als u weet dat u wilt opnemen in uw exemplaar segmenten, deze toevoegen aan uw exemplaar van **Active** lijst. Voor informatie over het toevoegen van items die u wilt de **Active** lijst zelf kunt, Zie [lijsten met actieve en geblokkeerde websites](#active-and-blocked-lists).

Maar als u niet zeker weet welke segmenten om op te nemen, kunt u uitvoeren Bing query's de **Preview** deelvenster en zien wat Bing retourneert. Vervolgens kunt u de segmenten die u wilt opnemen in uw aangepaste zoekopdrachten. Zo moet u waarschijnlijk meerdere querytermen om ervoor te zorgen dat u bij het identificeren van alle segmenten die u wilt gebruiken voor uw exemplaar uitvoeren. 

Volg deze stappen voor het gebruik van Bing segmenten toevoegen aan uw aangepaste zoekinstantie. 

1.  Aanmelden bij Bing Custom Search [portal](https://customsearch.ai).
2.  Maak een exemplaar of Selecteer een exemplaar om bij te werken.
3.  Selecteer in het voorbeeldvenster aan de rechterkant, Bing in de vervolgkeuzelijst.
4.  Voer een zoekterm die relevant is voor uw exemplaar in het zoekvak.
5.  Klik op **site toevoegen** naast het resultaat dat u wilt opnemen.
6.  Klik op de knop **OK**.

[!INCLUDE[publish or revert](./includes/publish-revert.md)]

## <a name="view-statistics"></a>Statistieken weergeven

Als u geabonneerd op aangepaste zoekopdrachten op het juiste niveau (Zie de [prijspagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), een **statistieken** tabblad wordt toegevoegd aan uw productie-exemplaren. Het tabblad statistieken bevat meer informatie over hoe uw Custom Search-eindpunten worden gebruikt, zoals aanroepvolume, top-query's, geografische verdeling, responscodes en veilig zoeken. U kunt informatie over het gebruik van de opgegeven besturingselementen voor filteren.

## <a name="understanding-quota"></a>Understanding quotum

- Voor elk exemplaar voor aangepast zoeken, het maximum aantal rangschikking van aanpassingen die u mogelijk wilt maken voor **Active** en **geblokkeerd** segmenten is beperkt tot 400.
- Een segment toe te voegen aan de actieve of geblokkeerd tabbladen telt als één positie aanpassing.
- Versterking en aantal degraderen als twee rangorde aanpassingen.
- Voor elk exemplaar voor aangepast zoeken is het maximum aantal pincodes dat mag u beperkt tot 200.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste zoekopdrachten aanroepen](./search-your-custom-view.md)
- [Gehoste UI-ervaring configureren](./hosted-ui.md)
- [Decoratiemarkeringen gebruiken om tekst te markeren](./hit-highlighting.md)
- [Bladeren door webpagina's](./page-webpages.md)