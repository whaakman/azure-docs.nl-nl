---
title: Gemiddelde van afbeeldingen met behulp van aangepaste lijsten in Azure inhoud beheerder | Microsoft Docs
description: Afbeelding van aangepaste lijsten in de console inhoud beheerder API Test-Drive.
services: cognitive-services
author: sanjeev3
manager: mikemcca
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: article
ms.date: 08/05/2017
ms.author: sajagtap
ms.openlocfilehash: 2d714f017be16d978ffbb877a2b7e78e1caf9169
ms.sourcegitcommit: 95d9a6acf29405a533db943b1688612980374272
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/23/2018
ms.locfileid: "35344555"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Gemiddelde met lijsten van de aangepaste installatiekopie in de API-console

U gebruikt de [lijst Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) in Azure inhoud beheerder voor het maken van aangepaste lijsten van afbeeldingen. De aangepaste lijsten met afbeeldingen gebruiken met de installatiekopie toezicht-API. De bewerking van de installatiekopie van toezicht evalueert uw installatiekopie. Als u aangepaste lijsten maakt, wordt deze door de bewerking ook vergelijkt met de afbeeldingen in uw aangepaste lijsten. Aangepaste lijsten kunt u de installatiekopie van het toestaan of blokkeren.

> [!NOTE]
> Er is een maximumlimiet van **5 installatiekopie lijsten** met elke lijst **niet meer dan 10.000 afbeeldingen**.
>

U de lijst beheer-API gebruiken voor de volgende taken uitvoeren:

- Een lijst maken.
- Installatiekopieën toevoegen aan een lijst.
- Installatiekopieën van het scherm op basis van de afbeeldingen in een lijst.
- Installatiekopieën van een lijst verwijderen.
- Een lijst te verwijderen.
- Lijst met gegevens bewerken.
- De index vernieuwen, zodat de wijzigingen in de lijst zijn opgenomen in een nieuwe scan.

## <a name="use-the-api-console"></a>De API-console gebruiken
Voordat u kunt de API in de online-console test-drive, moet u de abonnementssleutel van uw. Dit bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie voor meer informatie [overzicht](overview.md).

## <a name="refresh-search-index"></a>Vernieuwen van de search-index

Nadat u wijzigingen in een lijst met afbeeldingen aanbrengt, moet u de index om wijzigingen te worden opgenomen in toekomstige scans vernieuwen. Deze stap is vergelijkbaar met hoe een zoekmachine op het bureaublad (indien ingeschakeld) of een zoekmachine voortdurend vernieuwd de index als nieuwe bestanden of pagina's wilt opnemen.

1.  In de [installatiekopie lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), selecteer in het menu links **afbeelding vindt u een**, en selecteer vervolgens **zoekindex vernieuwen**.

  De **afbeelding geeft een lijst - zoekindex vernieuwen** pagina wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 
 
    ![Afbeelding van lijsten: Search-Index pagina Regioselectie vernieuwen](images/test-drive-region.png)

    De **afbeelding geeft een lijst - zoekindex vernieuwen** API-console wordt geopend.

3.  In de **listId** en voer de id van de lijst. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

  ![De installatiekopie van een lijst met - vak van vernieuwen Search-Index console antwoord inhoud](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Een lijst met afbeeldingen maken

1.  Ga naar de [installatiekopie lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

  De **afbeelding geeft een lijst - maken** pagina wordt geopend. 

3.  Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.

    ![Afbeelding van lijsten: maken pagina regio selecteren](images/test-drive-region.png)

    De **afbeelding geeft een lijst - maken** API-console wordt geopend.
 
4.  In de **Ocp-Apim-Subscription-Key** Voer de abonnementssleutel van uw.

5.  In de **aanvraagtekst** Geef waarden voor **naam** (bijvoorbeeld MyList) en **beschrijving**.

  ![Afbeelding van lijsten: maken van de console aanvraag hoofdtekstnaam en beschrijving](images/try-terms-list-create-1.png)

6.  Sleutel / waarde-paar tijdelijke aanduidingen gebruiken meer beschrijvende metagegevens toewijzen aan de lijst.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
             "Category": "Competitors",
             "Type": "Exclude"
           }
        }

  Lijst met metagegevens als sleutel-waardeparen en niet de werkelijke installatiekopieën toevoegen.
 
7.  Selecteer **Verzenden**. De lijst wordt gemaakt. Opmerking de **ID** waarde die is gekoppeld aan de nieuwe lijst. In dat geval moet u deze ID in voor de beheerfuncties van andere installatiekopie-lijst.

  ![Afbeelding van lijsten: console antwoord inhoud vak geeft de lijst-ID maken](images/try-terms-list-create-2.png)
 
8.  Afbeeldingen vervolgens toevoegen aan MyList. Selecteer in het menu links **installatiekopie**, en selecteer vervolgens **afbeelding toevoegen**.

  De **Image - installatiekopie toe te voegen** pagina wordt geopend. 

9. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.

  ![Afbeelding: installatiekopie pagina Regioselectie toevoegen](images/test-drive-region.png)

  De **Image - installatiekopie toe te voegen** API-console wordt geopend.
 
10. In de **listId** vak, voer de lijst-ID die u hebt gegenereerd en voer vervolgens de URL van de installatiekopie die u wilt toevoegen. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

11. Selecteer om te controleren of de afbeelding is toegevoegd aan de lijst, in het menu links **installatiekopie**, en selecteer vervolgens **installatiekopie-id ophalen**.

  De **afbeelding - installatiekopie-id ophalen** API-console wordt geopend.
  
12. In de **listId** vak, voert u de ID van de lijst en voer vervolgens de abonnementssleutel van uw. Selecteer **Verzenden**.

  ![Afbeelding - Get alle installatiekopie-id's console antwoord inhoud in de lijst staan de installatiekopieën die u hebt ingevoerd](images/try-image-list-create-11.png)
 
10. Installatiekopieën van een paar meer toevoegen. Nu u een aangepaste lijst met afbeeldingen hebt gemaakt, proberen [evalueren installatiekopieën](try-image-api.md) met behulp van de lijst van de aangepaste installatiekopie. 

## <a name="delete-images-and-lists"></a>Verwijderen van afbeeldingen en lijsten

Verwijderen van een afbeelding of een lijst is eenvoudig. De API kunt u de volgende taken uitvoeren:

- Een installatiekopie verwijderen. (**Afbeelding: verwijderen**)
- De afbeeldingen in een lijst verwijderen zonder te verwijderen van de lijst. (**Afbeelding: verwijdert u alle installatiekopieën**)
- Een lijst en alle inhoud verwijderd. (**Installatiekopie lijsten - Delete**)

In dit voorbeeld wordt een één installatiekopie verwijderd:

1. In de [installatiekopie lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), selecteer in het menu links **installatiekopie**, en selecteer vervolgens **verwijderen**. 

  De **afbeelding: verwijderen** pagina wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie. 

  ![Afbeelding - selectie van regio pagina verwijderen](images/test-drive-region.png)
 
  De **afbeelding: verwijderen** API-console wordt geopend.
 
3.  In de **listId** en voer de ID van de lijst voor het verwijderen van een afbeelding uit.  Dit is het aantal geretourneerd in de **afbeelding - installatiekopie-id ophalen** console voor MyList. Voer vervolgens de **ImageId** van de afbeelding te verwijderen. 

In ons voorbeeld wordt de lijst-ID is **58953**, de waarde voor **ContentSource**. De installatiekopie-ID is **59021**, de waarde voor **ContentIds**.

4.  Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

5.  Om te controleren dat de installatiekopie is verwijderd, gebruikt de **afbeelding - installatiekopie-id ophalen** console.
 
## <a name="change-list-information"></a>Informatie over de lijst wijzigen

U kunt de naam en beschrijving van een lijst bewerkt en metagegevensitems toevoegen.

1.  In de [installatiekopie lijst Management API-referentiemateriaal](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), selecteer in het menu links **afbeelding vindt u een**, en selecteer vervolgens **Details van de Update**. 

  De **afbeelding geeft een lijst - Details van de Update** pagina wordt geopend.

2. Voor **Open API testen console**, selecteer de regio die het beste past bij uw locatie.  

    ![De installatiekopie van een lijst met - Details van de Update pagina Regioselectie](images/test-drive-region.png)

    De **afbeelding geeft een lijst - Details van de Update** API-console wordt geopend.
 
3.  In de **listId** vak, voert u de ID van de lijst en voer vervolgens de abonnementssleutel van uw.

4.  In de **aanvraagtekst** vak, bewerk de instellingen en selecteer vervolgens de **verzenden** knop op de pagina.

  ![De installatiekopie van een lijst met - Details van de Update console aanvraag hoofdtekst bewerkingen](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Volgende stappen

De REST API gebruiken in uw code of starten met de [afbeelding vindt u een .NET-Quick Start](image-lists-quickstart-dotnet.md) integreren met uw toepassing.
