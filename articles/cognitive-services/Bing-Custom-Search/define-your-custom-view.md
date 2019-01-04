---
title: Configureren van uw ervaring Bing Custom Search | Microsoft Docs
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
ms.openlocfilehash: ac5ba80740c47dd71a30bb20aab4a54829eac822
ms.sourcegitcommit: 4eeeb520acf8b2419bcc73d8fcc81a075b81663a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/19/2018
ms.locfileid: "53597797"
---
# <a name="configure-your-bing-custom-search-experience"></a>De ervaring van uw Bing Custom Search configureren

Een aangepaste zoekinstantie kunt u de zoekfunctie om op te nemen alleen inhoud van websites die uw gebruikers het belangrijkst aanpassen. Bing zoeken in plaats van het uitvoeren van een zoekopdracht in de hele web, alleen de segmenten van de website die u interesseren. Als u een aangepaste weergave van het web wilt maken, gebruikt u de [portal](https://customsearch.ai) van Bing Aangepaste zoekopdrachten.

De portal kunt u een search-exemplaar dat Hiermee geeft u het delen van het web maken: domeinen, subpagina's en webpagina's, die u wilt dat Bing om te zoeken, en die u niet wilt dat deze om te zoeken. De portal kan ook voorstellen voor inhoud die u wilt opnemen.

Bij het definiëren van de segmenten van de web-, gebruikt u de volgende:

| De segmentnaam van het | Description                                                                                                                                                                                                                                                                                                |
|------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Domain     | Een segment van het domein bevat alle inhoud binnen een internetdomein gevonden. Bijvoorbeeld `www.microsoft.com`. Als u weglaat `www.` zorgt ervoor dat de Bing zoeken ook subdomeinen van het domein. Als u bijvoorbeeld `microsoft.com`, Bing retourneert ook resultaten van `support.microsoft.com` of `technet.microsoft.com`. |
| (Sub)    | Een segment subpagina bevat alle inhoud gevonden in de (sub) en paden eronder. U kunt maximaal twee subpagina's in het pad opgeven. Bijvoorbeeld: `www.microsoft.com/en-us/windows/`                                                                                                                       |
| Webpagina    | Een segment webpagina kunt u alleen die webpagina opnemen in een aangepast zoeken. U kunt eventueel opgeven of u wilt opnemen subpagina's.                                                                                                                                                                                  |

> [!IMPORTANT]
> Alle domeinen, subpagina's en webpagina's die u opgeeft moet zijn openbare en geïndexeerd door Bing. Als u eigenaar bent van een openbare site die u wilt opnemen in het zoekvak en deze nog niet worden geïndexeerd door Bing, raadpleegt u de Bing [webbeheerder documentatie](https://www.bing.com/webmaster/help/webmaster-guidelines-30fba23a) voor meer informatie over het ophalen van Bing het indexeren. Zie ook de documentatie van de beheerder voor meer informatie over het ophalen van Bing de verkende site bijwerkt als de index verouderd is.

## <a name="add-slices-of-the-web-to-your-custom-search-instance"></a>Segmenten van de website toevoegen aan uw exemplaar voor aangepast zoeken

Wanneer u uw aangepaste zoekinstantie maakt, kunt u het delen van het web: domeinen, subpagina's en webpagina's, die u wilt hebben opgenomen of geblokkeerd in de zoekresultaten. 

Als u weet dat de segmenten die u wilt opnemen in uw exemplaar voor aangepast zoeken, deze toevoegen aan uw exemplaar van **Active** lijst. 

Als u niet zeker weet welke segmenten om op te nemen, kunt u zoekquery's verzenden naar Bing in de **Preview** deelvenster en selecteer de segmenten die u wilt. Om dit te doen: 

1. Selecteer 'Bing' in de vervolgkeuzelijst in het voorbeeldvenster en voer een zoekopdracht

2. Klik op **site toevoegen** naast het resultaat dat u wilt opnemen. Klik vervolgens op OK.

>[!NOTE]
> [!INCLUDE[publish or revert](./includes/publish-revert.md)]

<a name="active-and-blocked-lists"></a>

### <a name="customize-your-search-experience-with-active-and-blocked-lists"></a>Aanpassen van uw zoekervaring met lijsten met actieve en geblokkeerd 

U kunt de lijst met actieve en geblokkeerde segmenten openen door te klikken op de **Active** en **geblokkeerd** tabbladen in uw exemplaar voor aangepast zoeken. Toegevoegd aan de lijst met actieve segmenten worden opgenomen in uw aangepaste zoekopdrachten. Geblokkeerde segmenten wordt niet worden doorzocht en wordt niet weergegeven in de lijst met zoekresultaten.

Als u wilt de segmenten van de gewenste Bing om te zoeken naar web opgeven, klikt u op de **Active** tabblad en een of meer URL's toe te voegen. Als u wilt bewerken of verwijderen van URL's, gebruikt u de opties onder de **besturingselementen** kolom. 

Wanneer het toevoegen van URL's voor de **Active** lijst die u kunt één URL's of meerdere URL's tegelijk toevoegen door het uploaden van een tekstbestand met het uploadpictogram.

![De Bing Custom Search actief tabblad](media/file-upload-icon.png)

Upload een bestand, een tekstbestand maken en geef een enkel domein, subpagina of webpagina per regel. Het bestand worden geweigerd als deze is niet juist opgemaakt.

> [!NOTE]
> * U kunt alleen een bestand uploadt naar de **Active** lijst. U deze niet gebruiken voor het toevoegen van segmenten de **geblokkeerd** lijst.  
> * Als de **geblokkeerd** lijst bevat een domein, subpagina of een webpagina die u hebt opgegeven in het uploadbestand, wordt deze verwijderd uit de **geblokkeerd** lijst en toegevoegd aan de **Active** lijst .
> * Dubbele vermeldingen in het uploadbestand worden genegeerd door Bing Custom Search. 

### <a name="get-website-suggestions-for-your-search-experience"></a>Profiteer van tips van de website voor uw zoekervaring

Na het toevoegen van websegmenten de **Active** weergeven, de Bing Custom Search portal-website en subpagina suggesties aan de onderkant van het tabblad wordt gegenereerd. Dit zijn de segmenten die door Bing Custom Search als dat u wilt opnemen. Klik op **vernieuwen** bijgewerkte suggesties na het bijwerken van instellingen voor uw aangepaste zoekinstantie ophalen. In deze sectie is alleen zichtbaar als suggesties beschikbaar zijn.

## <a name="search-for-images-and-videos"></a>Zoeken naar afbeeldingen en video 's

U kunt zoeken naar afbeeldingen en video's op soortgelijke wijze aan webinhoud met behulp van de [afbeeldingen zoeken-API van Bing aangepaste](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-images-api-v7-reference) of de [Video zoeken-API van Bing aangepaste](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-videos-api-v7-reference). U kunt deze resultaten met weergeven de [gebruikersinterface die wordt gehost](hosted-ui.md), of de API's. 

Deze API's zijn vergelijkbaar met de niet-aangepaste [Bing afbeeldingen zoeken](../Bing-Image-Search/overview.md) en [Bing video's zoeken](../Bing-Video-Search/search-the-web.md) API's, maar het gehele web doorzoeken en hoeven niet de `customConfig` queryparameter. Zie deze documentatiesets voor meer informatie over het werken met afbeeldingen en video's. 

## <a name="test-your-search-instance-with-the-preview-pane"></a>Test uw search-exemplaar met het voorbeeldvenster

U kunt uw exemplaar van de search testen met behulp van het voorbeeldvenster aan de rechterkant van de portal voor zoekopdrachten en de resultaten te bekijken. 

1. Selecteer onder het zoekvak **mijn exemplaar**. U kunt de resultaten van uw zoekervaring vergelijken met Bing, door te selecteren **Bing**. 
2. Selecteer een filter veilig zoeken en die de markt om te zoeken naar (Zie [queryparameters](https://docs.microsoft.com/rest/api/cognitiveservices/bing-custom-search-api-v7-reference#query-parameters)).
3. Voer een query en druk op enter of klik op het zoekpictogram om de resultaten van de huidige configuratie weer te geven. U kunt uw search-type u uitvoeren door te klikken op wijzigen **Web**, **installatiekopie**, of **Video** overeenkomende resultaten ophalen. 

<a name="adjustrank"></a>

## <a name="adjust-the-rank-of-specific-search-results"></a>De positie van specifieke zoekresultaten aanpassen

De portal kunt u de positie zoeken van inhoud van specifieke domeinen, subpagina's en webpagina's aanpassen. Na het verzenden van een zoekopdracht in het voorbeeldvenster, bevat elk zoekresultaat een lijst van aanpassingen, die kunt u voor deze:  

|            |                                                                                                                                                                      |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Blok      | Hiermee wordt het domein, subpagina of webpagina aan de lijst met geblokkeerde verplaatst. Bing sluit inhoud uit de geselecteerde site wordt weergegeven in de lijst met zoekresultaten.                    |
| Versterking      | Verhoogt de inhoud van het domein of de (sub) moet hoger zijn in de lijst met zoekresultaten.                                                                                        |
| Niveau verlagen     | Inhoud van het domein of lager in de lijst met zoekresultaten subpagina verlagen. U selecteren of te verlagen van de inhoud van het domein of subpagina dat de webpagina die deel uitmaakt. |
| Vastmaken aan de bovenkant | Verplaatst het domein, subpagina of een webpagina aan de **vastgehouden** lijst. Dit zorgt ervoor dat de webpagina wordt weergegeven als de belangrijkste zoekresultaten voor een bepaalde zoekquery.                   |

Aanpassen van de positie is niet beschikbaar voor de afbeelding of video zoekopdrachten.

### <a name="boosting-and-demoting-search-results"></a>Versterking en degraderen van zoekresultaten

U kunt heel stimuleren, vergroten, of degraderen van een domein of subpagina de **Active** lijst. Standaard worden alle segmenten zonder aanpassingen rangorde toegevoegd. Segmenten van de website die zijn heel heeft de klantenopbrengst of Boosted zijn hoger gerangschikt in de lijst met zoekresultaten (met de volgorde van de super boost is hoger dan boost). Items die zijn gedegradeerd worden gerangschikt lager in de lijst met zoekresultaten.

U kunt heel stimuleren, verhogen of verlagen van items met behulp van de **Trefwoordenrangschikking aanpassen** besturingselementen in de **Active** lijst, of door met behulp van de Boost en besturingselementen in het voorbeeldvenster te degraderen. De service wordt het segment wordt toegevoegd aan uw lijst met actieve en past u de volgorde daarvan.

> [!NOTE] 
> Versterking en degraderen van domeinen en subpagina's is een van de vele methoden die Bing Custom Search gebruikt om te bepalen van de volgorde van de lijst met zoekresultaten. Vanwege andere factoren die invloed op de positie van verschillende webinhoud, zijn de gevolgen van het aanpassen van de positie kunnen variëren. Gebruik het voorbeeldvenster om te testen van de gevolgen van het aanpassen van de positie van de zoekresultaten. 

Super stimuleren, verhogen en verlagen zijn niet beschikbaar voor de installatiekopie en de video zoeken.

## <a name="pin-slices-to-the-top-of-search-results"></a>Pincode segmenten aan het begin van zoekresultaten

De portal kunt u ook vastmaken URL's naar de bovenkant van zoekresultaten voor specifieke zoektermen, met behulp van de **vastgehouden** tabblad. Voer een URL en een query om op te geven van de webpagina die wordt weergegeven als het beste resultaat. Houd er rekening mee dat u maximaal één webpagina per zoekquery vastmaken kunt en alleen geïndexeerde webpagina's worden weergegeven in zoekopdrachten. Resultaten vast te maken is niet beschikbaar voor de afbeelding of video zoekopdrachten.

U kunt een webpagina aan het begin vastmaken op twee manieren:

* In de **vastgehouden** tabblad, voer de URL van de webpagina als u wilt vastmaken aan de bovenkant en de bijbehorende query.

* In de **Preview** deelvenster, voer een zoekopdracht en klik op zoeken. De webpagina die u wilt vastmaken voor uw query en klikt u op zoek **vastmaken aan de bovenkant**. de webpagina en de query wordt toegevoegd aan de **vastgehouden** lijst.

### <a name="specify-the-pins-match-condition"></a>Voorwaarde voor overeenkomst van de pincode opgeven

Standaard webpagina's zijn alleen vastgemaakt aan het begin van zoekresultaten wanneer de query-tekenreeks van de gebruiker exact overeenkomt met een die worden vermeld in de **vastgehouden** lijst. U kunt dit gedrag wijzigen door op te geven op een van de volgende criteria voor overeenkomst:

> [!NOTE]
> Alle vergelijkingen tussen zoekquery van de gebruiker en de zoekquery van de pincode zijn niet hoofdlettergevoelig.

| Waarde | Description                                                                          |
|---------------|----------------------------------------------------------------------------------|
| Begint met | De pincode is een overeenkomst als de queryreeks van de gebruiker met de querytekenreeks van de pincode begint |
| Eindigt op   | De pincode is een overeenkomst als queryreeks van de gebruiker met de pincode van de query-tekenreeks eindigt.  |
| Contains    | De pincode is een overeenkomst als de query-tekenreeks van de gebruiker de pincode queryreeks bevat.   |


Als u wilt wijzigen van de pincode-voorwaarde voor overeenkomst, klikt u op het speldpictogram bewerken. In de **Query-voorwaarde voor overeenkomst** kolom, klikt u op de vervolgkeuzelijst en selecteert u de nieuwe voorwaarde te gebruiken. Klik vervolgens op de opslagbewerking pictogram van de wijziging op te slaan.

### <a name="change-the-order-of-your-pinned-sites"></a>De volgorde van uw vastgemaakte sites wijzigen

U wijzigt de volgorde van uw pincodes, u kunt slepen en neerzetten het ze of hun volgordenummer bewerken door te klikken op het pictogram 'bewerken' in de **besturingselementen** kolom van de **vastgehouden** lijst.

Als meerdere pincodes voldoen aan een voorwaarde voor overeenkomst, Bing Custom Search gebruikt met het hoogste niveau in de lijst met één.

## <a name="view-statistics"></a>Statistische gegevens weergeven

Als u geabonneerd op aangepaste zoekopdrachten op het juiste niveau (Zie de [prijspagina's](https://azure.microsoft.com/pricing/details/cognitive-services/bing-custom-search/)), een **statistieken** tabblad wordt toegevoegd aan uw productie-exemplaren. Het tabblad statistieken bevat meer informatie over hoe uw Custom Search-eindpunten worden gebruikt, zoals aanroepvolume, top-query's, geografische verdeling, responscodes en veilig zoeken. U kunt informatie over het gebruik van de opgegeven besturingselementen voor filteren.

## <a name="usage-guidelines"></a>Richtlijnen voor het gebruik

- Voor elk exemplaar voor aangepast zoeken, het maximum aantal rangschikking van aanpassingen die u mogelijk wilt maken voor **Active** en **geblokkeerd** segmenten is beperkt tot 400.
- Een segment toe te voegen aan de actieve of geblokkeerd tabbladen telt als één positie aanpassing.
- Versterking en aantal degraderen als twee rangorde aanpassingen.
- Voor elk exemplaar voor aangepast zoeken is het maximum aantal pincodes dat mag u beperkt tot 200.

## <a name="next-steps"></a>Volgende stappen

- [Aangepaste zoekopdrachten aanroepen](./search-your-custom-view.md)
- [Gehoste UI-ervaring configureren](./hosted-ui.md)
- [Decoratiemarkeringen gebruiken om tekst te markeren](./hit-highlighting.md)
- [Bladeren door webpagina's](./page-webpages.md)
