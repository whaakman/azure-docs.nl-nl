---
title: Model van de antwoorden van een API met de Azure portal | Microsoft Docs
description: Deze zelfstudie laat zien hoe beleid instellen voor een API zodat deze een mocked antwoord retourneert met API Management (APIM). Deze methode endables ontwikkelaars verdergaan met het implementeren en testen van het exemplaar van API Management in geval de back-end is niet beschikbaar voor echte antwoorden verzenden.
services: api-management
documentationcenter: 
author: juliako
manager: cfowler
editor: 
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: e485071b026c52eb23532639546ad475fc92cde3
ms.sourcegitcommit: b854df4fc66c73ba1dd141740a2b348de3e1e028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2017
---
# <a name="mock-api-responses"></a>Antwoorden mock-API

Backend APIs worden geïmporteerd in een API APIM of gemaakt en beheerd handmatig. De stappen in deze zelfstudie ziet u hoe APIM gebruiken om te maken van een lege API en handmatig beheren. De zelfstudie laat zien hoe beleid instellen voor een API zodat deze een mocked antwoord retourneert. Deze methode kan ontwikkelaars verdergaan met het implementeren en testen van het exemplaar APIM, zelfs als de back-end niet beschikbaar is voor echte antwoorden verzenden. Mogelijkheid voor het model maken van antwoorden kan nuttig zijn bij een aantal scenario's:

+ Als de API-façade eerst is ontworpen en de implementatie van de back-end later afkomstig is. Of de back-end parallel wordt ontwikkeld.
+ Wanneer de back-end is tijdelijk niet operationele of kan niet worden geschaald.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een test-API maken 
> * Een bewerking aan de test API toevoegen
> * Antwoord mocking inschakelen
> * De mocked API testen

![Mocked bewerkingsantwoord](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Vereisten

Voltooi de volgende Snelstartgids: [Azure API Management-exemplaar maken](get-started-create-service-instance.md).

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="create-a-test-api"></a>Een test-API maken 

De stappen in deze sectie laten zien hoe een leeg API maken met geen back-end. Ook ziet u hoe een bewerking toevoegen aan de API. Het aanroepen van de bewerking na het voltooien van de stappen in deze sectie, treedt er een fout op. U ontvangt geen fouten na voltooiing van de stappen in de sectie 'Antwoord mocking inschakelen'.

1. Selecteer **API's** uit onder **API MANAGEMENT**.
2. Selecteer in het menu links **+ API toevoegen**.
3. Selecteer **leeg API** uit de lijst.
4. Voer '*API testen*' voor **weergavenaam**.
5. Voer '*onbeperkte*' voor **producten**.
6. Selecteer **Maken**.

## <a name="add-an-operation-to-the-test-api"></a>Een bewerking aan de test API toevoegen

1. Selecteer de API die u in de vorige stap hebt gemaakt.
2. Klik op **+ toevoegbewerking**.

    ![Mocked bewerkingsantwoord](./media/mock-api-responses/mock-api-responses02.png)

    |Instelling|Waarde|Beschrijving|
    |---|---|---|
    |**URL** (HTTP-term)|GET|U kunt kiezen uit een van de vooraf gedefinieerde HTTP-termen.|
    |**URL** |*/Test*|Een URL-pad voor de API. |
    |**Weergavenaam**|*De aanroep van de test*|De naam die wordt weergegeven in de **ontwikkelaarsportal**.|
    |**Beschrijving**||Geef een beschrijving van de bewerking die wordt gebruikt voor documentatie voor de ontwikkelaars met behulp van deze API in de **ontwikkelaarsportal**.|
    |**Query** tabblad||U kunt query-parameters kunt toevoegen. Naast het bieden van een naam en beschrijving, kunt u de waarden die kunnen worden toegewezen aan deze parameter opgeven. Een van de waarden kan worden als standaardwaarde gemarkeerd (optioneel).|
    |**Aanvraag** tabblad||U kunt inhoud aanvraagtypen, voorbeelden en schema's definiëren. |
    |**Antwoord** tabblad|Zie de volgende procedure in deze tabel.|Antwoordstatuscodes, inhoudstypen, voorbeelden en schema's definiëren.|

3. Selecteer de **antwoord** tabblad in de URL weergegeven naam en beschrijving velden.
4. Klik op **+ reactie toevoegen**.
5. Selecteer **200 OK** uit de lijst.
6. Onder de **verklaringen** kop aan de rechterkant, selecteer **+ weergave toevoegen**.
7. Voer '*application/json*' in het zoekvak en selecteer de **application/json** inhoudstype.
8. In de **voorbeeld** tekst Voer '*{'sampleField': 'test'}*'.
9. Selecteer **Opslaan**.

## <a name="enable-response-mocking"></a>Antwoord mocking inschakelen

1. Selecteer de API die u in de stap 'Een test-API maken' hebt gemaakt.
2. Selecteer de testbewerking die u hebt toegevoegd.
2. Klik in het venster aan de rechterkant de **ontwerp** tabblad.
3. In de **binnenkomend verkeer** venster, klik op het potloodpictogram.
4. In de **Mocking** tabblad **statische reacties** voor **Mocking gedrag**.
5. In de **API Management wordt het volgende antwoord:** in het tekstvak **200 OK, application/json**. Deze selectie geeft aan dat uw API resultaat moet geven het antwoord voorbeeld die u hebt gedefinieerd in de vorige sectie.
6. Selecteer **Opslaan**.

## <a name="test-the-mocked-api"></a>De mocked API testen

1. Selecteer de API die u in de stap 'Een test-API maken' hebt gemaakt.
2. Open de **Test** tabblad.
3. Zorg ervoor dat de **testen aanroep** API is geselecteerd.

    > [!TIP]
    > Een gele balk met de tekst **Mocking is ingeschakeld** geeft aan dat reacties geretourneerd van de API Management verstuurt een mocking beleid en niet een antwoord van de werkelijke back-end.

3. Selecteer **verzenden** waarmee een test aanroepen.
4. De **HTTP-antwoord** de JSON die is opgegeven als een voorbeeld van een in de eerste sectie van de zelfstudie wordt weergegeven.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een test-API maken
> * Een bewerking aan de test API toevoegen
> * Antwoord mocking inschakelen
> * De mocked API testen

Ga naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Transformeren en een gepubliceerde API beveiligen](transform-api.md)
