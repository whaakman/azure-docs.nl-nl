---
title: Gemiddeld installatiekopieën met aangepaste lijsten en de API-console - Content Moderator
titlesuffix: Azure Content Moderator
description: U kunt de lijst met Management-API in Azure Content Moderator aangepaste lijsten van installatiekopieën maken.
services: cognitive-services
author: sanjeev3
manager: cgronlun
ms.service: cognitive-services
ms.component: content-moderator
ms.topic: conceptual
ms.date: 01/10/2019
ms.author: sajagtap
ms.openlocfilehash: c42fdb037e1803db8255518f5c7ae7b2abd90c60
ms.sourcegitcommit: c61777f4aa47b91fb4df0c07614fdcf8ab6dcf32
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/14/2019
ms.locfileid: "54259356"
---
# <a name="moderate-with-custom-image-lists-in-the-api-console"></a>Gemiddelde met aangepaste afbeeldingslijsten in de API-console

U gebruikt de [lijst Management API](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672) in Azure Content Moderator om aangepaste lijsten van installatiekopieën te maken. Gebruik de aangepaste lijsten van afbeeldingen met de afbeeldingen-API voor beheer. De bewerking van de toezicht installatiekopie evalueert uw installatiekopie. Als u aangepaste lijsten maakt, wordt deze door de bewerking ook vergeleken met de afbeeldingen in uw aangepaste lijsten. U kunt aangepaste lijsten blokkeren of toestaan dat de installatiekopie.

> [!NOTE]
> Er is een maximumlimiet van **5 afbeeldingslijsten** waarbij elke lijst **niet meer dan 10.000 afbeeldingen mag bevatten**.
>

In dat geval gebruikt u de lijst Management-API in de volgende taken uitvoeren:

- Een lijst maken.
- Afbeeldingen toevoegen aan een lijst.
- Installatiekopieën van het scherm op basis van de afbeeldingen in een lijst.
- Afbeeldingen uit een lijst verwijderen.
- Een lijst verwijderen.
- Lijstgegevens bewerken.
- De index vernieuwen zodat wijzigingen in de lijst worden gebruikt in een nieuwe scan.

## <a name="use-the-api-console"></a>De API-console gebruiken
Voordat u de API in de online-console uitproberen kan, moet u de abonnementssleutel van uw. Dit bevindt zich op de **instellingen** tabblad, in de **Ocp-Apim-Subscription-Key** vak. Zie [Overzicht](overview.md) voor meer informatie.

## <a name="refresh-search-index"></a>Search-index vernieuwen

Nadat u wijzigingen in een lijst met afbeeldingen aanbrengt, moet u de index voor de wijzigingen moeten worden opgenomen in toekomstige scans vernieuwen. Deze stap is vergelijkbaar met hoe een zoekmachine op het bureaublad (indien ingeschakeld) of een zoekmachine voortdurend Hiermee vernieuwt u de index zodanig dat nieuwe bestanden of pagina's.

1.  In de [installatiekopie lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), selecteert u in het menu links **afbeelding geeft een lijst**, en selecteer vervolgens **zoekindex vernieuwen**.

  De **afbeelding geeft een lijst - zoekindex vernieuwen** pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 
 
    ![Lijst met installatiekopieën - zoekindex vernieuwen pagina regio selecteren](images/test-drive-region.png)

    De **afbeelding geeft een lijst - zoekindex vernieuwen** API-console wordt geopend.

3.  In de **listId** voert u de lijst-ID. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

  ![Lijst met installatiekopieën - vak met de Search-Index vernieuwen console antwoord inhoud](images/try-image-list-refresh-1.png)


## <a name="create-an-image-list"></a>Een installatiekopie maken

1.  Ga naar de [installatiekopie lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672).

  De **afbeelding geeft een lijst - maken** pagina wordt geopend. 

3.  Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.

    ![Afbeelding van lijsten: maken van de selectie van de regio](images/test-drive-region.png)

    De **afbeelding geeft een lijst - maken** API-console wordt geopend.
 
4.  In de **Ocp-Apim-Subscription-Key** voert u de abonnementssleutel van uw.

5.  In de **aanvraagtekst** vak, voer waarden in voor **naam** (bijvoorbeeld MyList) en **beschrijving**.

  ![Afbeelding van lijsten: maken van de console aanvraag hoofdtekst van de naam en beschrijving](images/try-terms-list-create-1.png)

6.  Sleutel / waarde-paar tijdelijke aanduidingen gebruiken meer beschrijvende metagegevens toewijzen aan uw lijst.

        {
           "Name": "MyExclusionList",
           "Description": "MyListDescription",
           "Metadata": 
           {
             "Category": "Competitors",
             "Type": "Exclude"
           }
        }

  Metagegevens van de lijst als sleutel-waardeparen en niet de werkelijke installatiekopieën toevoegen.
 
7.  Selecteer **Verzenden**. De lijst wordt gemaakt. Houd er rekening mee de **ID** waarde die is gekoppeld aan de nieuwe lijst. In dat geval moet u deze ID in voor de beheerfuncties van andere installatiekopie-lijst.

  ![Afbeelding van lijsten: console antwoord maken en de inhoud in de lijst-ID wordt weergegeven](images/try-terms-list-create-2.png)
 
8.  Afbeeldingen vervolgens toevoegen aan MyList. Selecteer in het menu links **installatiekopie**, en selecteer vervolgens **afbeelding toevoegen**.

  De **Image - installatiekopie toevoegen** pagina wordt geopend. 

9. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.

  ![Installatiekopie - installatiekopie van de selectie van de regio toevoegen](images/test-drive-region.png)

  De **Image - installatiekopie toevoegen** API-console wordt geopend.
 
10. In de **listId** vak, voer de lijst-ID die u hebt gegenereerd en voer vervolgens de URL van de installatiekopie die u wilt toevoegen. Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

11. Selecteer om te controleren dat de afbeelding is toegevoegd aan de lijst, in het menu links **installatiekopie**, en selecteer vervolgens **installatiekopie-id ophalen**.

  De **afbeelding - installatiekopie-id ophalen** API-console wordt geopend.
  
12. In de **listId** vak, voer de lijst-ID en voer vervolgens de abonnementssleutel van uw. Selecteer **Verzenden**.

  ![Installatiekopie - Get alle installatiekopie-id's console antwoord vak inhoud geeft een lijst van de installatiekopieën die u hebt ingevoerd](images/try-image-list-create-11.png)
 
10. Een paar meer installatiekopieën toevoegen. Nu u een aangepaste lijst met installatiekopieën hebt gemaakt, kunt u proberen [evalueren installatiekopieën](try-image-api.md) met behulp van de lijst met aangepaste afbeeldingen. 

## <a name="delete-images-and-lists"></a>Verwijderen van installatiekopieën en lijsten

Het is eenvoudig een installatiekopie of een lijst te verwijderen. De API kunt u de volgende taken uitvoeren:

- Een installatiekopie verwijderen. (**Image - verwijderen**)
- Alle installatiekopieën in een lijst verwijderen zonder te verwijderen van de lijst. (**Afbeelding: verwijdert u alle installatiekopieën**)
- Een lijst en alle inhoud verwijderen. (**Afbeeldingslijsten - verwijderen**)

In dit voorbeeld wordt een één installatiekopie verwijderd:

1. In de [installatiekopie lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), selecteert u in het menu links **installatiekopie**, en selecteer vervolgens **verwijderen**. 

  De **Image - verwijderen** pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie. 

  ![Installatiekopie - selectie verwijderen pagina-regio](images/test-drive-region.png)
 
  De **Image - verwijderen** API-console wordt geopend.
 
3.  In de **listId** voert u de ID van de lijst als u wilt verwijderen van een afbeelding uit.  Dit is het dat wordt geretourneerd de **afbeelding - installatiekopie-id ophalen** console voor MyList. Voer vervolgens de **ImageId** van de afbeelding te verwijderen. 

In ons voorbeeld wordt de lijst-ID is **58953**, de waarde voor **ContentSource**. De installatiekopie-ID is **59021**, de waarde voor **ContentIds**.

4.  Voer de abonnementssleutel van uw en selecteer vervolgens **verzenden**.

5.  Om te controleren dat de installatiekopie is verwijderd, gebruikt u de **afbeelding - installatiekopie-id ophalen** console.
 
## <a name="change-list-information"></a>Lijst met gegevens wijzigen

U kunt een lijst met de naam en beschrijving bewerken en metagegevensitems toevoegen.

1.  In de [installatiekopie lijst Management API-verwijzing](https://westus.dev.cognitive.microsoft.com/docs/services/57cf755e3f9b070c105bd2c2/operations/57cf755e3f9b070868a1f672), selecteert u in het menu links **afbeelding geeft een lijst**, en selecteer vervolgens **Details van de Update**. 

  De **afbeelding geeft een lijst - Details van de Update** pagina wordt geopend.

2. Voor **Open API testconsole**, selecteer de regio die het beste past bij uw locatie.  

    ![Lijst met installatiekopieën - Details van de Update pagina regio selecteren](images/test-drive-region.png)

    De **afbeelding geeft een lijst - Details van de Update** API-console wordt geopend.
 
3.  In de **listId** vak, voer de lijst-ID en voer vervolgens de abonnementssleutel van uw.

4.  In de **aanvraagtekst** vak, voer de benodigde aanpassingen en selecteer vervolgens de **verzenden** knop op de pagina.

  ![Lijst met installatiekopieën - Details van de Update-console aanvraag hoofdtekst bewerkingen](images/try-terms-list-change-1.png)
 

## <a name="next-steps"></a>Volgende stappen

De REST-API in uw code te gebruiken of beginnen met de [afbeelding geeft een lijst van de Snelstartgids voor .NET](image-lists-quickstart-dotnet.md) om te integreren in uw toepassing.
