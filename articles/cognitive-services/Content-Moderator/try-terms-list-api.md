---
title: Gemiddelde tekst met aangepaste term lijsten in Azure inhoud beheerder | Microsoft Docs
description: Aangepaste term lijsten in de console inhoud beheerder API Test-Drive.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 2542e4590781879408aafe8d072eceef157e02c9
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344552"
---
# <a name="moderate-with-custom-term-lists-in-the-api-console"></a>Gemiddelde met aangepaste term lijsten in de API-console

De standaard globale lijst met termen in Azure inhoud beheerder is voldoende voor de meest inhoud toezicht behoeften. Mogelijk moet u echter scherm van de termen die specifiek voor uw organisatie zijn. Bijvoorbeeld, kan u concurrent labelnamen voor verder onderzoek. 

Gebruik de [lijst Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f) om aangepaste lijsten van termen te gebruiken met de tekst toezicht API te maken. De **tekst - scherm** bewerking scant de tekst voor taalgebruik en ook de tekst op basis van aangepaste en gedeelde zwarte lijsten vergeleken.

> [!NOTE]
> Er is een maximumlimiet van **geeft een lijst van 5 term** met elke lijst **niet meer dan 10.000 voorwaarden**.
>

De lijst met Management API kunt u de volgende taken uitvoeren:
- Een lijst maken.
- Voorwaarden toevoegen aan een lijst.
- Scherm voorwaarden op basis van de voorwaarden in een lijst.
- Voorwaarden verwijdert uit een lijst.
- Een lijst te verwijderen.
- Lijst met gegevens bewerken.
- De index vernieuwen, zodat de wijzigingen in de lijst zijn opgenomen in een nieuwe scan.

## <a name="use-the-api-console"></a>De API-console gebruiken

Voordat u kunt de API in de online-console test-drive, moet u de abonnementssleutel van uw. Deze sleutel bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie voor meer informatie [overzicht](overview.md).

## <a name="refresh-search-index"></a>Vernieuwen van de search-index

Nadat u wijzigingen in een lijst van de termijn aanbrengt, moet u de index om wijzigingen te worden opgenomen in toekomstige scans vernieuwen. Deze stap is vergelijkbaar met hoe een zoekmachine op het bureaublad (indien ingeschakeld) of een zoekmachine voortdurend vernieuwd de index als nieuwe bestanden of pagina's wilt opnemen.

1.  In de [Term lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), selecteer in het menu links **Term bevat**, en selecteer vervolgens **zoekindex vernieuwen**. 

  De **Term bevat - zoekindex vernieuwen** pagina wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Term lijsten - zoekindex vernieuwen pagina Regioselectie](images/test-drive-region.png)

  De **Term bevat - zoekindex vernieuwen** API-console wordt geopend.

3.  In de **listId** en voer de id van de lijst. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

  ![Een lijst met term API - vak van vernieuwen Search-Index console antwoord inhoud](images/try-terms-list-refresh-1.png)

## <a name="create-a-term-list"></a>Maak een lijst van de termijn
1.  Ga naar de [Term lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f). 

  De **Term bevat - maken** pagina wordt geopend.

2.  Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Term lijsten - pagina Regioselectie maken](images/test-drive-region.png)

  De **Term bevat - maken** API-console wordt geopend.
 
3.  In de **Ocp-Apim-Subscription-Key** Voer de abonnementssleutel van uw.

4.  In de **aanvraagtekst** Geef waarden voor **naam** (bijvoorbeeld MyList) en **beschrijving**.

  ![Term lijsten - maken console aanvraag hoofdtekstnaam en beschrijving](images/try-terms-list-create-1.png)

5.  Sleutel / waarde-paar tijdelijke aanduidingen gebruiken meer beschrijvende metagegevens toewijzen aan de lijst.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
              "Category": "Competitors",
              "Type": "Exclude"
           }
        }

  Lijst met metagegevens als sleutel-waardeparen en geen daadwerkelijke voorwaarden toevoegen.
 
6.  Selecteer **Verzenden**. De lijst wordt gemaakt. Opmerking de **ID** waarde die is gekoppeld aan de nieuwe lijst. In dat geval moet u deze ID in voor de beheerfuncties van andere term-lijst.

  ![Term lijsten - console antwoord inhoud vak geeft de lijst-ID maken](images/try-terms-list-create-2.png)
 
7.  Voorwaarden toevoegen aan MyList. In het menu links onder **Term**, selecteer **begrip toevoegen**. 

  De **benaming - Term toevoegen** pagina wordt geopend. 

8.  Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Term - Term pagina Regioselectie toevoegen](images/test-drive-region.png)

  De **benaming - Term toevoegen** API-console wordt geopend.
 
9.  In de **listId** vak en selecteer een waarde voor de lijst-ID invoeren die u hebt gegenereerd **taal**. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

  ![Term - Term console queryparameters toevoegen](images/try-terms-list-create-3.png)
 
10. Selecteer om te controleren dat de term is toegevoegd aan de lijst, in het menu links **Term**, en selecteer vervolgens **ophalen van alle voorwaarden**. 

  De **Term - alle voorwaarden ophalen** API-console wordt geopend.

11. In de **listId** vak, voert u de ID van de lijst en voer vervolgens de abonnementssleutel van uw. Selecteer **Verzenden**.

12. In de **antwoordinhoud** vak, controleert u of de voorwaarden die u hebt ingevoerd.

  ![Term - alle Get-termen console antwoord vak inhoud een lijst met de voorwaarden die u hebt ingevoerd](images/try-terms-list-create-4.png)
 
13. Enkele meer voorwaarden toevoegen. Nu u een aangepaste lijst met termen hebt gemaakt, proberen [tekst scannen](try-text-api.md) met behulp van de lijst met aangepaste term. 

## <a name="delete-terms-and-lists"></a>Voorwaarden en lijsten verwijderen

Het is eenvoudig een term of een lijst te verwijderen. U kunt de API gebruiken voor de volgende taken uitvoeren:

- Een term verwijderen. (**Benaming - verwijderen**)
- De voorwaarden in een lijst verwijderen zonder te verwijderen van de lijst. (**Benaming - verwijderen van alle voorwaarden**)
- Een lijst en alle inhoud verwijderd. (**Term lijsten - Delete**)

In dit voorbeeld wordt een enkele term verwijderd.

1.  In de [Term lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), selecteer in het menu links **Term**, en selecteer vervolgens **verwijderen**. 

  De **benaming - verwijderen** wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Term - selectie van regio pagina verwijderen](images/test-drive-region.png)

  De **benaming - verwijderen** API-console wordt geopend.
  
3.  In de **listId** en voer de ID van de lijst die u wilt verwijderen van een term uit. Deze ID is het aantal (in ons voorbeeld **122**) die wordt geretourneerd als de **Term bevat - Details ophalen** console voor MyList. Voer de term in en selecteer een taal.
 
  ![Term - Delete-console queryparameters](images/try-terms-list-delete-1.png)

4.  Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

5.  Om te controleren dat de term is verwijderd, gebruikt de **Term bevat - ophalen van alle** console.

  ![Term lijsten: ophalen van alle console antwoord inhoud vak geeft aan dat de term wordt verwijderd](images/try-terms-list-delete-2.png)
 
## <a name="change-list-information"></a>Informatie over de lijst wijzigen

U kunt de naam en beschrijving van een lijst bewerkt en metagegevensitems toevoegen.

1.  In de [Term lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f67f), selecteer in het menu links **Term bevat**, en selecteer vervolgens **Details van de Update**. 

  De **Term bevat - Details van de Update** pagina wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Term lijsten - Details van de Update pagina Regioselectie](images/test-drive-region.png)

  De **Term bevat - Details van de Update** API-console wordt geopend.

3.  In de **listId** vak, voert u de ID van de lijst en voer vervolgens de abonnementssleutel van uw.

4.  In de **aanvraagtekst** vak, bewerk de instellingen en selecteer vervolgens **verzenden**.

  ![Term lijsten - Details van de Update console aanvraag hoofdtekst bewerkingen](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Volgende stappen

De REST API gebruiken in uw code of starten met de [Term bevat .NET Quick Start](term-lists-quickstart-dotnet.md) integreren met uw toepassing.
