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
ms.openlocfilehash: 1f9b689ac6127bc2f7d1e810356ae9a23b8e0996
ms.sourcegitcommit: 2d961702f23e63ee63eddf52086e0c8573aec8dd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/07/2018
ms.locfileid: "44162390"
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

1.  Open een internetbrowser.
2.  Navigeer naar de aangepaste zoekopdrachten [portal](https://customsearch.ai).
3.  Aanmelden bij de portal met een Microsoft-account (MSA). Als u een MSA hebt, klikt u op **maken van een Microsoft-account**. Als het de eerste keer is met behulp van de portal, wordt u gevraagd voor machtigingen voor toegang tot uw gegevens. Klik op **Ja**.
4.  Na het aanmelden, klikt u op **nieuwe aangepaste zoekopdrachten**. In de **maken van een nieuw exemplaar voor aangepast zoeken** venster, Geef een naam die zinvol is en een beschrijving van het type inhoud dat de zoekopdracht retourneert. U kunt de naam op elk gewenst moment wijzigen.
 
    ![Schermopname van het maken van een nieuwe aangepaste exemplaar zoekvak](../media/newCustomSrch.png)

5.  Klik op OK, een URL en of u wilt opnemen subpagina's van de basispagina opgeeft:

    ![Schermopname van de pagina definitie URL](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Actieve vermeldingen toe te voegen
Als u wilt opnemen in resultaten van specifieke sites of URL's, voeg deze toe aan de **Active** tabblad.

1.  In de **Definition Editor**, klikt u op de **Active** tabblad en voer de URL van een of meer sites die u wilt opnemen in uw zoekopdracht.

    ![Schermopname van het actieve tabblad definitie-Editor](../media/customSrchEditor.png)

2.  Om te bevestigen dat uw exemplaar resultaten retourneert, voert u een query in het voorbeeldvenster aan de rechterkant. Bing retourneert resultaten alleen voor openbare websites die deze heeft geïndexeerd.

## <a name="add-blocked-entries"></a>Geblokkeerde vermeldingen toe te voegen
Als u wilt uitsluiten resultaten van specifieke sites of URL's, voeg deze toe aan de **geblokkeerd** tabblad.

1. In de **Definition Editor**, klikt u op de **geblokkeerd** tabblad en voer de URL van een of meer sites die u wilt uitsluiten van uw zoekopdracht.

    ![Schermopname van de definitie van de Editor tabblad geblokkeerd](../media/blockedCustomSrch.png)


2. Om te bevestigen dat uw exemplaar niet van resultaten op het geblokkeerde sites retourneren, voert u een query in het voorbeeldvenster aan de rechterkant. 

## <a name="add-pinned-entries"></a>Vastgemaakte items toevoegen
Als u wilt vastmaken van een specifieke webpagina aan het begin van de lijst met zoekresultaten, toevoegen de webpagina's en query term voor de **vastgehouden** tabblad.  De **vastgehouden** tabblad bevat een lijst van webpagina's en query term-paren die de webpagina die wordt weergegeven als het beste resultaat voor een specifieke query opgeven. De zoekterm van de gebruiker moet exact overeenkomen met de queryterm vastgemaakte voor de webpagina om te worden vastgemaakt aan het begin.

1. In de **Definition Editor**, klikt u op de **vastgehouden** tabblad en voer de termijn van webpagina's en query van de webpagina die moet worden geretourneerd als het beste resultaat.

    ![Schermopname van de definitie van de Editor tabblad vastgemaakt](../media/pinnedCustomSrch.png)

2. Voer ter bevestiging dat uw exemplaar van de opgegeven webpagina als het beste resultaat retourneert de zoekterm die u hebt vastgemaakt in het voorbeeldvenster aan de rechterkant.

## <a name="configure-hosted-ui"></a>Gehoste gebruikersinterface configureren
Custom Search biedt een gehoste-gebruikersinterface voor het renderen van de JSON-antwoord van uw exemplaar voor aangepast zoeken.  Voor het definiëren van uw gebruikersinterface-ervaring:

1. Klik op de **gebruikersinterface die wordt gehost** tabblad.
2. Selecteer een indeling.

    ![Schermopname van de gebruikersinterface die wordt gehost Selecteer stap lay-out](./media/custom-search-hosted-ui-select-layout.png)

3. Selecteer een kleurenthema.

    ![Schermopname van de gebruikersinterface die wordt gehost Selecteer stap lay-out](./media/custom-search-hosted-ui-select-color-theme.png)

4. Geef extra configuratieopties.

    ![Schermopname van de stap van de aanvullende configuraties die worden gehost UI](./media/custom-search-hosted-ui-additional-configurations.png)

5. Plak uw **abonnementssleutel**. Zie [probeer de Cognitive Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Schermopname van de stap van de aanvullende configuraties die worden gehost UI](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE [publish or revert](../includes/publish-revert.md)]

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
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Voeg een regel break-element en een div-element om te fungeren als een container.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. Uit de **gebruikersinterface die wordt gehost** tabblad, blader naar de sectie met de titel **gebruiken van de gebruikersinterface**. Kopieer het JavaScript-fragment.

    ![Schermopname van de gebruikersinterface die wordt gehost opslaan de knop](./media/custom-search-hosted-ui-consuming-ui.png)

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
                src="https://ui.customsearch.ai/api/ux/render?customConfig=<YOUR-CUSTOM-CONFIG-ID>&market=en-US&safeSearch=Moderate">
        </script>
    </div>
    ```

6. In de **Solution Explorer**, klik met de rechtermuisknop op **wwwroot** en klikt u op **weergeven in Browser**.

    ![Schermopname van solution explorer-weergave in de Browser te selecteren in het contextmenu wwwroot](./media/custom-search-webapp-view-in-browser.png)

Uw nieuwe aangepaste zoekopdrachten-webpagina moet er ongeveer als volgt uitzien:

![Schermopname van de webpagina aangepast zoeken](./media/custom-search-webapp-browse-index.png)

Uitvoeren van een zoekopdracht resultaten als volgt wordt weergegeven.

![Schermopname van het aangepaste zoekresultaten](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> - Een exemplaar voor aangepast zoeken die zijn gemaakt
> - Actieve vermeldingen toegevoegd
> - Geblokkeerde vermeldingen toegevoegd
> - Vastgemaakte vermeldingen toegevoegd
> - Aangepaste zoekopdrachten geïntegreerd in webpagina 's

U kunt nu doorgaan met het aanroepen van Bing Custom Search-eindpunten via een programma.

> [!div class="nextstepaction"]
> [Bing Custom Search-eindpunt (C#) aanroepen](../call-endpoint-csharp.md)