---
title: "Bing aangepaste zoekactie: Aangepaste zoekactie webpagina's maken | Microsoft Docs"
description: Beschrijft hoe een exemplaar van de aangepaste zoekactie configureren en integreren met een webpagina
services: cognitive-services
author: brapel
manager: ehansen
ms.service: cognitive-services
ms.component: bing-custom-search
ms.topic: article
ms.date: 10/16/2017
ms.author: v-brapel
ms.openlocfilehash: c1431ec852cab943e00d3933ef4f0500a4fdb151
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344912"
---
# <a name="build-a-custom-search-web-page"></a>Een aangepaste zoekactie webpagina bouwen
Bing aangepaste zoeken kunt u voor het maken van de ervaringen op maat gemaakte zoeken naar onderwerpen die u interesseren. Bijvoorbeeld, als u een Vechtsport-website die u een zoekfunctie kunt beschikt, kunt u de domeinen, subsites en webpagina's met Bing worden gezocht. Uw gebruikers zien zoekresultaten wilt weergeven die zijn toegesneden op de inhoud die ze interesseren in plaats van paginering via algemene zoekresultaten wilt weergeven die mogelijk niet relevant inhoud bevatten. 

Deze zelfstudie laat zien hoe een exemplaar van de aangepaste zoekactie configureren en integreren met een nieuwe webpagina.

De taken besproken zijn:

> [!div class="checklist"]
> - Maak een aangepaste zoekactie-exemplaar
> - Actieve vermeldingen toevoegen
> - Geblokkeerde vermeldingen toevoegen
> - Vastgemaakte items toevoegen
> - Aangepaste zoekactie integreren in een webpagina

## <a name="prerequisites"></a>Vereisten
- Als u wilt volgen samen met de zelfstudie, moet u een abonnementssleutel voor de API van zoekservice aangepaste Bing.  Als u een sleutel, Zie [cognitieve Services probeer](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search).
- Als u Visual Studio 2017 nog niet hebt geïnstalleerd, kunt u het downloaden en de **gratis** [Community Edition van Visual Studio 2017](https://www.visualstudio.com/downloads/) gebruiken.

## <a name="create-a-custom-search-instance"></a>Maak een aangepaste zoekactie-exemplaar
Een exemplaar Bing aangepaste zoekactie maken:

1.  Open een internetbrowser.
2.  Navigeer naar de aangepaste zoekactie [portal](https://customsearch.ai).
3.  Aanmelden bij de portal met een Microsoft-account (MSA). Als u een MSA hebt, klikt u op **maken van een Microsoft-account**. Als de eerste keer met de portal is, wordt u gevraagd om machtigingen voor toegang tot uw gegevens. Klik op **Ja**.
4.  Na het aanmelden, klikt u op **nieuwe aangepaste zoekopdracht**. In de **Maak een nieuw exemplaar van de aangepaste zoekactie** venster, voer een naam die zinvol is en een beschrijving van het type inhoud dat de zoekopdracht retourneert. U kunt de naam op elk gewenst moment wijzigen.
 
    ![Schermopname van het maken van een nieuwe aangepaste exemplaar zoekvak](../media/newCustomSrch.png)

5.  Klik op OK, geeft u een URL en of u wilt opnemen subpagina's van de basispagina:

    ![Schermopname van de pagina definitie URL](../media/newCustomSrch1-a.png)

## <a name="add-active-entries"></a>Actieve vermeldingen toevoegen
Om de resultaten van bepaalde sites of URL's opnemen, voeg deze toe aan de **Active** tabblad.

1.  In de **Definition Editor**, klikt u op de **Active** tabblad en voer de URL van een of meer sites die u wilt opnemen in uw zoekopdracht.

    ![Schermopname van het actieve tabblad definitie-Editor](../media/customSrchEditor.png)

2.  Om te bevestigen dat uw exemplaar resultaten retourneert, voert u een query in het voorbeeldvenster aan de rechterkant. Bing retourneert resultaten alleen voor openbare sites die het heeft geïndexeerd.

## <a name="add-blocked-entries"></a>Geblokkeerde vermeldingen toevoegen
Als u wilt uitsluiten resultaten van bepaalde sites of URL's, voeg deze toe aan de **geblokkeerd** tabblad.

1. In de **Definition Editor**, klikt u op de **geblokkeerd** tabblad en voer de URL van een of meer sites die u wilt uitsluiten van de zoekopdracht.

    ![Schermopname van de Editor voor de definitie van tabblad geblokkeerd](../media/blockedCustomSrch.png)


2. Om te bevestigen dat uw exemplaar niet van resultaten op de geblokkeerde websites retourneren, voert u een query in het voorbeeldvenster aan de rechterkant. 

## <a name="add-pinned-entries"></a>Vastgemaakte items toevoegen
Om een specifieke webpagina boven aan de lijst met zoekresultaten, voeg de term webpagina en de query voor de **vastgehouden** tabblad.  De **vastgehouden** tabblad bevat een lijst van webpagina's en query term-waardeparen die op de webpagina die wordt weergegeven als het eerste resultaat voor een specifieke query opgeven. De zoekterm van de gebruiker moet exact overeenkomen met de queryterm vastgemaakt voor de webpagina om te worden vastgemaakt aan de bovenkant.

1. In de **Definition Editor**, klikt u op de **vastgehouden** tabblad en voer de term webpagina's en query van de webpagina die moet worden geretourneerd als het eerste resultaat.

    ![Schermopname van de Editor voor de definitie van tabblad vastgemaakt](../media/pinnedCustomSrch.png)

2. Om te bevestigen dat uw exemplaar van de webpagina van de opgegeven als het eerste resultaat retourneert, voert u de zoekterm die u hebt vastgemaakt in het voorbeeldvenster aan de rechterkant.

## <a name="configure-hosted-ui"></a>Gehoste UI configureren
Aangepaste Search biedt een gehoste UI is opgegeven voor het weergeven van de JSON-antwoord van uw aangepaste zoekactie-exemplaar.  UI-mogelijkheden definiëren:

1. Klik op de **UI gehost** tabblad.
2. Selecteer een indeling.

    ![Schermopname van de gebruikersinterface gehost selecteren stap lay-out](./media/custom-search-hosted-ui-select-layout.png)

3. Selecteer een kleurenthema.

    ![Schermopname van de gebruikersinterface gehost selecteren stap lay-out](./media/custom-search-hosted-ui-select-color-theme.png)

4. Geef extra configuratieopties.

    ![Schermopname van de stap van de aanvullende configuraties die worden gehost UI](./media/custom-search-hosted-ui-additional-configurations.png)

5. Plak uw **abonnementssleutel**. Zie [probeer cognitieve Services](https://azure.microsoft.com/try/cognitive-services/?api=bing-custom-search-api).

    ![Schermopname van de stap van de aanvullende configuraties die worden gehost UI](./media/custom-search-hosted-ui-subscription-key.png)

[!INCLUDE[publish or revert](../includes/publish-revert.md)]

<a name="consuminghostedui"></a>
## <a name="consuming-hosted-ui"></a>Gehoste gebruikersinterface gebruiken
Er zijn twee manieren gebruiken voor de gehoste-gebruikersinterface.  

- Optie 1: Het opgegeven JavaScript-fragment integreren met uw toepassing.
- Optie 2: Gebruik het HTML-eindpunt opgegeven.

De rest van deze zelfstudie ziet u **optie 1: Javascript-fragment**.  

## <a name="set-up-your-visual-studio-solution"></a>Instellen van uw Visual Studio-oplossing
1. Open **Visual Studio** op uw computer.
2. Selecteer in het menu **Bestand** de optie **Nieuw** en kies vervolgens **Project**.
3. In de **nieuw Project** Selecteer **Visual C# / Web / ASP.NET-webtoepassing Core**, Geef uw project en klik vervolgens op **OK**.
   
    ![Schermopname van het venster Nieuw project](./media/custom-search-new-project.png)

4. In de **nieuwe webtoepassing voor ASP.NET Core** Selecteer **webtoepassing** en klik op **OK**.
    
    ![Schermopname van het venster Nieuw project](./media/custom-search-new-webapp.png)

## <a name="edit-indexcshtml"></a>Index.cshtml bewerken
1. In de **Solution Explorer**, vouw **pagina's** en dubbelklikt u op **index.cshtml** het bestand te openen.

    ![Schermopname van solution explorer met de uitgevouwen-pagina's en index.cshtml geselecteerd](./media/custom-search-visual-studio-webapp-solution-explorer-index.png)

2. In index.cshtml, verwijdert u alle informatie van regel 7 en daaronder wordt gestart.
    
    ``` razor
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }    
    ```

3. Een lijn break-element en een div om te fungeren als een container toevoegen.

    ``` html
    @page
    @model IndexModel
    @{
        ViewData["Title"] = "Home page";
    }
    <br />
    <div id="customSearch"></div>
    ```

4. Van de **UI gehost** tabblad, blader naar het gedeelte **gebruiken van de gebruikersinterface**. Kopieer de JavaScript-fragment.

    ![Schermopname van de gebruikersinterface gehost opslaan knop](./media/custom-search-hosted-ui-consuming-ui.png)

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

6. In de **Solution Explorer**, klik met de rechtermuisknop op **wwwroot** en klik op **weergeven in Browser**.

    ![Schermopname van solution explorer weergave in de Browser te selecteren in het contextmenu wwwroot](./media/custom-search-webapp-view-in-browser.png)

Uw nieuwe aangepaste zoekactie webpagina moet er ongeveer als volgt uitzien:

![Schermopname van het aangepaste zoekactie webpagina](./media/custom-search-webapp-browse-index.png)

Uitvoeren van een zoekopdracht wordt gerenderd resultaten als volgt.

![Schermopname van het aangepaste zoekresultaten](./media/custom-search-webapp-results.png)

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie hebt u het volgende gedaan:

> [!div class="checklist"]
> - Een aangepaste zoekactie exemplaar gemaakt
> - Toegevoegde actieve meldingen
> - Geblokkeerde vermeldingen
> - Vastgemaakte vermeldingen
> - Aangepaste zoekactie geïntegreerd in een webpagina

U kunt nu doorgaan met het aanroepen van Bing aangepaste zoekactie eindpunten programmatisch.

> [!div class="nextstepaction"]
> [Bing aangepaste zoekactie eindpunt (C#) aanroepen](../call-endpoint-csharp.md)