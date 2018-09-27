---
title: Gemiddeld tekst met aangepaste terminologielijsten - Content Moderator
titlesuffix: Azure Cognitive Services
description: Maak een proefrit met aangepaste terminologielijsten in de Content Moderator-API-console.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 99df9fda2cc56f169a61ec215a976de28fc13d27
ms.sourcegitcommit: ad08b2db50d63c8f550575d2e7bb9a0852efb12f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2018
ms.locfileid: "47220275"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>Gemiddelde met aangepaste terminologielijsten in de API-console

De standaard globale lijst met voorwaarden in Azure Content Moderator is voldoende voor de meest content moderation behoeften. U moet echter mogelijk scherm van de termen die specifiek voor uw organisatie zijn. Bijvoorbeeld, kunt u code concurrent namen voor verder onderzoek. 

Gebruik de [lijst Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) om aangepaste lijsten van voorwaarden voor gebruik met de tekst Afbeeldingstoezicht-API te maken. De **tekst - scherm** bewerking scant uw tekst taalgebruik en ook vergelijkt tekst tegen aangepaste en gedeelde zwarte lijsten.

> [!NOTE]
> Er is een maximumlimiet van **geeft een lijst van 5 term** met elke lijst **niet meer dan 10.000 voorwaarden**.
>

De lijst met Management-API kunt u de volgende taken uitvoeren:
- Een lijst maken.
- Voorwaarden toevoegen aan een lijst.
- Scherm voorwaarden op basis van de voorwaarden in een lijst.
- Gebruiksvoorwaarden verwijderen uit een lijst.
- Een lijst verwijderen.
- Bewerk de gegevens weer te geven.
- De index vernieuwen zodat wijzigingen in de lijst zijn opgenomen in een nieuwe scan.

## <a name="use-the-api-console"></a>De API-console gebruiken

Voordat u de API in de online-console uitproberen kan, moet u de abonnementssleutel van uw. Deze sleutel bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie voor meer informatie, [overzicht](overview.md).

## <a name="refresh-search-index"></a>Search-index vernieuwen

Nadat u wijzigingen aan een terminologielijst met aanbrengt, moet u de index voor de wijzigingen moeten worden opgenomen in toekomstige scans vernieuwen. Deze stap is vergelijkbaar met hoe een zoekmachine op het bureaublad (indien ingeschakeld) of een zoekmachine voortdurend Hiermee vernieuwt u de index zodanig dat nieuwe bestanden of pagina's.

1.  In de [Term lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), selecteer in het menu links **geeft een lijst van Term**, en selecteer vervolgens **zoekindex vernieuwen**. 

  De **Term bevat - zoekindex vernieuwen** pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Terminologielijsten - zoekindex vernieuwen pagina regio selecteren](images/test-drive-region.png)

  De **Term bevat - zoekindex vernieuwen** API-console wordt geopend.

3.  In de **listId** voert u de lijst-ID. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

  ![Terminologielijsten API - vak met de Search-Index vernieuwen console antwoord inhoud](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Een terminologielijst maken
1.  Ga naar de [Term lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

  De **Term bevat - maken** pagina wordt geopend.

2.  Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Lijsten termijn: de selectie van de regio maken](images/test-drive-region.png)

  De **Term bevat - maken** API-console wordt geopend.
 
3.  In de **Ocp-Apim-Subscription-Key** voert u de abonnementssleutel van uw.

4.  In de **aanvraagtekst** vak, voer waarden in voor **naam** (bijvoorbeeld MyList) en **beschrijving**.

  ![Lijsten termijn: maken van de console aanvraag hoofdtekst van de naam en beschrijving](images/try-terms-list-create-1.png)

5.  Sleutel / waarde-paar tijdelijke aanduidingen gebruiken meer beschrijvende metagegevens toewijzen aan uw lijst.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
              "Category": "Competitors",
              "Type": "Exclude"
           }
        }

  Metagegevens van de lijst als sleutel-waardeparen en geen voorwaarden toevoegen.
 
6.  Selecteer **Verzenden**. De lijst wordt gemaakt. Houd er rekening mee de **ID** waarde die is gekoppeld aan de nieuwe lijst. In dat geval moet u deze ID in voor de beheerfuncties van andere term-lijst.

  ![Lijsten termijn - console antwoord maken en de inhoud in de lijst-ID wordt weergegeven](images/try-terms-list-create-2.png)
 
7.  Voorwaarden toevoegen aan MyList. In het menu links onder **Term**, selecteer **begrip toevoegen**. 

  De **termijn - Term toevoegen** pagina wordt geopend. 

8.  Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Termijn: regio de selectie van Term toevoegen](images/test-drive-region.png)

  De **termijn - Term toevoegen** API-console wordt geopend.
 
9.  In de **listId** vak, voer de lijst-ID die u hebt gegenereerd en selecteer een waarde voor **taal**. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

  ![Termijn: Term console queryparameters toevoegen](images/try-terms-list-create-3.png)
 
10. Selecteer om te controleren dat de term is toegevoegd aan de lijst, in het menu links **Term**, en selecteer vervolgens **krijgen alle voorwaarden**. 

  De **Term - alle voorwaarden ophalen** API-console wordt geopend.

11. In de **listId** vak, voer de lijst-ID en voer vervolgens de abonnementssleutel van uw. Selecteer **Verzenden**.

12. In de **antwoordinhoud** controleert u de voorwaarden die u hebt ingevoerd.

  ![Term - Get alle voorwaarden console antwoord vak inhoud een lijst met de voorwaarden die u hebt ingevoerd](images/try-terms-list-create-4.png)
 
13. Een paar meer voorwaarden toevoegen. Nu u een aangepaste lijst met voorwaarden hebt gemaakt, kunt u proberen [tekst scannen](try-text-api.md) met behulp van de lijst van de aangepaste periode. 

## <a name="delete-terms-and-lists"></a>Voorwaarden verwijderen en lijsten

Het is eenvoudig een abonnement of een lijst te verwijderen. U de API gebruiken om de volgende taken uitvoeren:

- Een term te verwijderen. (**Termijn - verwijderen**)
- De voorwaarden in een lijst verwijderen zonder te verwijderen van de lijst. (**Termijn: verwijderen van alle voorwaarden**)
- Een lijst en alle inhoud verwijderd. (**Terminologielijsten - verwijderen**)

In dit voorbeeld wordt een termijn van één verwijderd.

1.  In de [Term lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), selecteert u in het menu links **Term**, en selecteer vervolgens **verwijderen**. 

  De **termijn - verwijderen** wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Term - selectie verwijderen pagina-regio](images/test-drive-region.png)

  De **termijn - verwijderen** API-console wordt geopend.
  
3.  In de **listId** voert u de ID van de lijst die u wilt verwijderen van een term uit. Deze ID is het getal (in ons voorbeeld **122**) die wordt geretourneerd in de **Term bevat - Details ophalen** console voor MyList. Voer de term in en selecteer een taal.
 
  ![Term - verwijderen-console queryparameters](images/try-terms-list-delete-1.png)

4.  Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

5.  Om te controleren of de term is verwijderd, gebruikt u de **Term bevat - u alle** console.

  ![Een lijst met termijn: alle console antwoord inhoud vak ziet u dat de term is verwijderd ophalen](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Lijst met gegevens wijzigen

U kunt een lijst met de naam en beschrijving bewerken en metagegevensitems toevoegen.

1.  In de [Term lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), selecteert u in het menu links **geeft een lijst van Term**, en selecteer vervolgens **Details van de Update**. 

  De **Term bevat - Details van de Update** pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Terminologielijsten - Details van de Update pagina regio selecteren](images/test-drive-region.png)

  De **Term bevat - Details van de Update** API-console wordt geopend.

3.  In de **listId** vak, voer de lijst-ID en voer vervolgens de abonnementssleutel van uw.

4.  In de **aanvraagtekst** vak, voer de benodigde aanpassingen en selecteer vervolgens **verzenden**.

  ![Terminologielijsten - Details van de Update-console aanvraag hoofdtekst bewerkingen](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Volgende stappen

De REST-API in uw code te gebruiken of beginnen met de [Term geeft een lijst van de Snelstartgids voor .NET](term-lists-quickstart-dotnet.md) om te integreren in uw toepassing.
