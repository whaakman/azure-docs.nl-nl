---
title: 'Bing Custom Search: Een webpagina aangepast zoeken maken | Microsoft Docs'
description: Beschrijft hoe u voor het configureren van een exemplaar voor aangepast zoeken en te integreren met een webpagina
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: 8bc1520325afc256ac62cc1f1dfaf24c53da4b83
ms.sourcegitcommit: 32d218f5bd74f1cd106f4248115985df631d0a8c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/24/2018
ms.locfileid: "46979995"
---
# <a name="build-a-custom-search-web-page"></a>Een Aangepaste zoekopdrachten-webpagina bouwen

Bing aangepaste zoekopdrachten kunt maken op maat gemaakte zoekervaringen voor onderwerpen waarin u geïnteresseerd bent. Bijvoorbeeld, als u over een gevechtssporten-website die u een zoekfunctie kunt, kunt u de domeinen, subsites en webpagina's die Bing zoeken. Uw gebruikers te zien die zijn afgestemd op de inhoud die ze in plaats van een wisselbestand via algemene zoekresultaten die mogelijk niet relevant inhoud bevatten die het belangrijkst zoekresultaten. 

Deze zelfstudie wordt gedemonstreerd hoe u een exemplaar voor aangepast zoeken configureert en integreren deze in een nieuwe webpagina.

De taken die gedekt zijn:

> [!div class="checklist"]
> - Een exemplaar voor aangepast zoeken maken
> - Actieve vermeldingen toe te voegen
> - Geblokkeerde vermeldingen toe te voegen
> - Vastgemaakte items toevoegen
> - Aangepaste zoekopdrachten integreren met een webpagina

## <a name="prerequisites"></a>Vereisten

- Als u wilt volgen, samen met de zelfstudie, moet u een abonnementssleutel voor de Bing Custom Search-API.  Als u een sleutel, Zie [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken.

## <a name="create-a-custom-search-instance"></a>Een exemplaar voor aangepast zoeken maken

Een Bing Custom Search-exemplaar maken:

1. Open een internetbrowser.  
  
2. Navigeer naar de aangepaste zoekopdrachten [portal](https://customsearch.ai).  
  
3. Aanmelden bij de portal met een Microsoft-account (MSA). Als u een MSA hebt, klikt u op **maken van een Microsoft-account**. Als het de eerste keer is met behulp van de portal, wordt u gevraagd voor machtigingen voor toegang tot uw gegevens. Klik op **Ja**.  
  
4. Na het aanmelden, klikt u op **nieuwe aangepaste zoekopdrachten**. In de **maken van een nieuw exemplaar voor aangepast zoeken** venster, Geef een naam die zinvol is en een beschrijving van het type inhoud dat de zoekopdracht retourneert. U kunt de naam op elk gewenst moment wijzigen.  
  
  ![Schermopname van het maken van een nieuwe aangepaste exemplaar zoekvak](../media/newCustomSrch.png)  
  
5. Klik op OK, een URL en of u wilt opnemen subpagina's van de URL opgeeft.  
  
  ![Schermafbeelding van de URL van de definitie van pagina](../media/newCustomSrch1-a.png)  


## <a name="add-active-entries"></a>Actieve vermeldingen toe te voegen

Als u wilt opnemen in resultaten van bepaalde websites of URL's, voeg deze toe aan de **Active** tabblad.

1.  Op de **configuratie** pagina, klikt u op de **Active** tabblad en voer de URL van een of meer websites die u wilt opnemen in uw zoekopdracht.

    ![Schermopname van het actieve tabblad definitie-Editor](../media/customSrchEditor.png)

2.  Om te bevestigen dat uw exemplaar resultaten retourneert, voert u een query in het voorbeeldvenster aan de rechterkant. Bing retourneert alleen resultaten voor openbare websites die deze heeft geïndexeerd.

## <a name="add-blocked-entries"></a>Geblokkeerde vermeldingen toe te voegen

Als u wilt uitsluiten resultaten van bepaalde websites of URL's, voeg deze toe aan de **geblokkeerd** tabblad.

1. Op de **configuratie** pagina, klikt u op de **geblokkeerd** tabblad en voer de URL van een of meer websites die u wilt uitsluiten van uw zoekopdracht.

    ![Schermafbeelding van de definitie van de Editor tabblad geblokkeerd](../media/blockedCustomSrch.png)


2. Om te bevestigen dat uw exemplaar niet van resultaten op het geblokkeerde websites retourneren, voert u een query in het voorbeeldvenster aan de rechterkant. 

## <a name="add-pinned-entries"></a>Vastgemaakte items toevoegen

Als u wilt vastmaken van een specifieke webpagina aan het begin van de lijst met zoekresultaten, toevoegen de webpagina's en query term voor de **vastgehouden** tabblad. De **vastgehouden** tabblad bevat een lijst van webpagina's en query term-paren die de webpagina die wordt weergegeven als het beste resultaat voor een specifieke query opgeven. De webpagina is vastgemaakt alleen als de querytekenreeks van de gebruiker overeenkomt met de pincode van de query-tekenreeks op basis van de voorwaarde van de pincode. [Meer informatie](../define-your-custom-view.md#pin-to-top).

1. Op de **configuratie** pagina, klikt u op de **vastgehouden** tabblad en voer de termijn van webpagina's en query van de webpagina die u laten retourneren als de bovenste resultaat wilt.  
  
2. Standaard queryreeks van de gebruiker exact overeenkomen met de queryreeks van uw pincode voor Bing om terug te keren van de webpagina als het beste resultaat. Om te wijzigen van de voorwaarde voor overeenkomst, bewerkt u de pincode (Klik op het potloodpictogram), klikt u op Exact in de **Query-voorwaarde voor overeenkomst** kolom en selecteert u de voorwaarde voor overeenkomst die geschikt is voor uw toepassing.  
  
    ![Schermafbeelding van de definitie van de Editor vastgemaakt tabblad](../media/pinnedCustomSrch.png)
  
3. Voer ter bevestiging dat uw exemplaar van de opgegeven webpagina als het beste resultaat retourneert de zoekterm die u hebt vastgemaakt in het voorbeeldvenster aan de rechterkant.

## <a name="configure-hosted-ui"></a>Gehoste gebruikersinterface configureren

Custom Search biedt een gehoste-gebruikersinterface voor het renderen van de JSON-antwoord van uw exemplaar voor aangepast zoeken. Voor het definiëren van uw gebruikersinterface-ervaring:

1. Klik op de **gebruikersinterface die wordt gehost** tabblad.  
  
2. Selecteer een indeling.  
  
  ![Schermafbeelding van de gebruikersinterface die wordt gehost Selecteer stap lay-out](./media/custom-search-hosted-ui-select-layout.png)  
  
3. Selecteer een kleurenthema.  
  
  ![Schermafbeelding van de gebruikersinterface die wordt gehost kleurthema selecteren](./media/custom-search-hosted-ui-select-color-theme.png)  

  Als u nodig hebt voor het afstemmen van de kleurthema beter integreren met uw web-app, klikt u op **aanpassen thema**. Niet alle kleur configuraties van toepassing op alle lay-out-thema's. Als u wilt een kleur wijzigen, voert u de kleur van de hexadecimale RGB-waarde (bijvoorbeeld #366eb8) in het bijbehorende tekstvak. Of klik op de kleurknop en klik vervolgens op de tint die bij u past. Altijd Denk over toegankelijkheid bij het selecteren van kleuren.
  
  ![Schermafbeelding van de gebruikersinterface die wordt gehost kleurthema aanpassen](./media/custom-search-hosted-ui-customize-color-theme.png)  

  
4. Geef extra configuratieopties.  
  
  ![Schermafbeelding van de stap van de aanvullende configuraties die worden gehost UI](./media/custom-search-hosted-ui-additional-configurations.png)  
  
  Als u wilt ophalen, geavanceerde configuraties, klikt u op **geavanceerde configuraties weergeven**. Deze configuraties, zoals toegevoegd *koppelingsdoel* aan opties voor het doorzoeken van Web *filters inschakelen* afbeeldings- en Video-opties, en *tijdelijke aanduiding vak Zoeken* aan diverse Opties.

  ![Schermafbeelding van de stap van de geavanceerde configuraties gebruikersinterface die wordt gehost](./media/custom-search-hosted-ui-advanced-configurations.png)  
  
5. Selecteer uw abonnementssleutels in de vervolgkeuzelijsten. Of u kunt de abonnementssleutel handmatig invoeren. Zie voor meer informatie over het ophalen van de sleutels [Cognitive Services proberen](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).  
  
  ![Schermafbeelding van de stap van de aanvullende configuraties die worden gehost UI](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE[publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Gehoste gebruikersinterface verbruikt

Er zijn twee manieren gebruiken voor de gehoste-gebruikersinterface.  

- Optie 1: Het opgegeven JavaScript-fragment in uw toepassing integreren.
- Optie 2: Gebruik de HTML-eindpunt opgegeven.

De rest van deze zelfstudie ziet u **optie 1: Javascript-fragment**.  

## <a name="set-up-your-visual-studio-solution"></a>Uw Visual Studio-oplossing instellen

1. Open **Visual Studio** op uw computer.  
  
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.  
  
3. In de **nieuw Project** venster **Visual C# / Web-/ ASP.NET Core-webtoepassing**, Geef uw project de naam en klik vervolgens op **OK**.  
  
  ![Schermopname van het venster Nieuw project](./media/custom-search-new-project.png)  
  
4. In de **nieuwe ASP.NET Core-webtoepassing** venster **webtoepassing** en klikt u op **OK**.  
  
  ![Schermopname van het venster Nieuw project](./media/custom-search-new-webapp.png)  

## <a name="edit-indexcshtml"></a>Index.cshtml bewerken

1. In de **Solution Explorer**, vouw **pagina's** en dubbelklikt u op **index.cshtml** om het bestand te openen.  
  
  ![Schermopname van solution explorer met pagina's uitgebreid en index.cshtml geselecteerd](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)  
  
2. In index.cshtml, alles begin van regel 7 en hieronder worden verwijderd.  
  
  ```razor
  @page
  @model IndexModel
  @{
      ViewData["Title"] = "Home page";
  }    
  ```  
  
3. Voeg een regel break-element en een div-element om te fungeren als een container.  
  
  ```html
  @page
  @model IndexModel
  @{
      ViewData["Title"] = "Home page";
  }
  <br />
  <div id="customSearch"></div>
  ```  
  
4. In de **gebruikersinterface die wordt gehost** pagina, blader naar de sectie met de titel **gebruiken van de gebruikersinterface**. Klik op de *eindpunten* voor toegang tot de JavaScript-fragment. U kunt ook op het codefragment ophalen door te klikken op **productie** en vervolgens de **gebruikersinterface die wordt gehost** tabblad.
  
  <!-- Get new screenshot after prod gets new bits
  ![Screenshot of the Hosted UI save button](./media/custom-search-hosted-ui-consuming-ui.png)  
  -->
  
5. Plak het script-element in de container die u hebt toegevoegd.  
  
  ``` html
  @page
  @model IndexModel
  @{
      ViewData["Title"] = "Home page";
  }
  <br />
  <div id="customSearch">
      <script type="text/javascript" 
          id="bcs_js_snippet"
          src="https://ui.customsearch.ai /api/ux/rendering-js?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate&version=latest&q=">
      </script>
  </div>
  ```  
  
6. In de **Solution Explorer**, klik met de rechtermuisknop op **wwwroot** en klikt u op **weergeven in Browser**.  
  
  ![Schermopname van solution explorer-weergave in de Browser te selecteren in het contextmenu wwwroot](./media/custom-search-webapp-view-in-browser.png)  

Uw nieuwe aangepaste zoekopdrachten-webpagina moet er ongeveer als volgt uitzien:

![Schermafbeelding van de webpagina aangepast zoeken](./media/custom-search-webapp-browse-index.png)

Uitvoeren van een zoekopdracht wordt resultaten als volgt weergegeven:

![Schermafbeelding van de aangepaste zoekresultaten](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Bing Custom Search-eindpunt (C#) aanroepen](../call-endpoint-csharp.md)