---
title: Gesimuleerde antwoorden van een API met Azure Portal | Microsoft Docs
description: Deze zelfstudie laat zien hoe u met API Management (APIM) beleid kunt instellen voor een API zodat deze een gesimuleerd antwoord retourneert. Deze methode maakt het voor ontwikkelaars mogelijk om verder te gaan met het implementeren en testen van de instantie van API Management wanneer de back-end niet beschikbaar is voor het verzenden van echte antwoorden.
services: api-management
documentationcenter: ''
author: vladvino
manager: cfowler
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.custom: mvc
ms.topic: tutorial
ms.date: 11/27/2017
ms.author: apimpm
ms.openlocfilehash: 4383ce3788f6fade5299d69ef99b80221c58d9e7
ms.sourcegitcommit: d98d99567d0383bb8d7cbe2d767ec15ebf2daeb2
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 05/10/2018
ms.locfileid: "33936980"
---
# <a name="mock-api-responses"></a>Gesimuleerde antwoorden van een API

Back-end API's worden geïmporteerd in een APIM API of handmatig gemaakt en beheerd. Met de stappen in deze zelfstudie leert u hoe u APIM kunt gebruiken om een lege API te maken en deze handmatig te beheren. Deze zelfstudie laat zien hoe u beleid kunt instellen voor een API zodat deze een gesimuleerd antwoord retourneert. Deze methode maakt het voor ontwikkelaars mogelijk om verder te gaan met het implementeren en testen van de instantie van APIM, zelfs wanneer de back-end niet beschikbaar is voor het verzenden van echte antwoorden. De mogelijkheid voor het maken van gesimuleerde antwoorden kan nuttig zijn bij een aantal scenario's:

+ Als de API-façade eerst is ontworpen en de implementatie van de back-end later komt. Of als de back-end parallel wordt ontwikkeld.
+ Wanneer de back-end tijdelijk niet operationeel is of niet kan worden geschaald.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een test-API maken 
> * Een bewerking aan de test-API toevoegen
> * Antwoordsimulatie inschakelen
> * De gesimuleerde API testen

![Gesimuleerd bewerkingsantwoord](./media/mock-api-responses/mock-api-responses01.png)

## <a name="prerequisites"></a>Vereisten

Lees de volgende snelstartgids: [Een Azure API Management-exemplaar maken](get-started-create-service-instance.md).

## <a name="create-a-test-api"></a>Een test-API maken 

De stappen in deze sectie laten zien hoe u een lege API zonder back-end maakt. Ook ziet u hoe u een bewerking kunt toevoegen aan de API. Bij het aanroepen van de bewerking na het voltooien van de stappen in deze sectie, treedt een fout op. U ontvangt geen foutmeldingen na voltooiing van de stappen in de sectie 'Antwoordsimulatie inschakelen'.

1. Selecteer **API's** bij **API MANAGEMENT**.
2. Selecteer **+ API toevoegen** in het linkermenu.
3. Selecteer **Lege API** uit de lijst.
4. Voer '*Test API*' in als **weergavenaam**.
5. Voer '*Onbeperkt*' in bij **Producten**.
6. Selecteer **Maken**.

## <a name="add-an-operation-to-the-test-api"></a>Een bewerking aan de test-API toevoegen

1. Selecteer de API die u in de vorige stap hebt gemaakt.
2. Klik op **+ Bewerking toevoegen**.

    ![Gesimuleerd bewerkingsantwoord](./media/mock-api-responses/mock-api-responses02.png)

    |Instelling|Waarde|Beschrijving|
    |---|---|---|
    |**URL** (HTTP-woord)|GET|U kunt kiezen uit een van de vooraf gedefinieerde HTTP-woorden.|
    |**URL** |*/test*|Een URL-pad voor de API. |
    |**Weergavenaam**|*Testaanroep*|Deze naam wordt weergegeven in de **ontwikkelaarsportal**.|
    |**Beschrijving**||Geef een beschrijving van de bewerking die wordt gebruikt voor documentatie voor de ontwikkelaars met behulp van deze API in de **ontwikkelaarsportal**.|
    |Tabblad **Query**||U kunt queryparameters toevoegen. Naast het invoeren van een naam en beschrijving, kunt u waarden opgeven die kunnen worden toegewezen aan deze parameter. Een van de waarden kan worden gemarkeerd als standaardwaarde (optioneel).|
    |Tabblad **Aanvraag**||U kunt aanvraaginhoudstypen, voorbeelden en schema's definiëren. |
    |Tabblad **Antwoord**|Zie de volgende stappen in deze tabel.|Definieer antwoordstatuscodes, inhoudstypen, voorbeelden en schema's.|

3. Selecteer het **Antwoord**-tabblad onder de velden voor de URL, Weergavenaam en Beschrijving.
4. Klik op **+ Antwoord toevoegen**.
5. Selecteer **200 OK** uit de lijst.
6. Selecteer **+ Weergave toevoegen** onder de kop **Representaties** aan de rechterkant.
7. Voer '*application/json*' in het zoekvak in en selecteer het inhoudstype **application/json**.
8. Voer in het tekstvak **Voorbeeld** '*{'sampleField': 'test'}*' in.
9. Selecteer **Opslaan**.

## <a name="enable-response-mocking"></a>Antwoordsimulatie inschakelen

1. Selecteer de API die u in de stap 'Een test-API maken' hebt gemaakt.
2. Selecteer de testbewerking die u hebt toegevoegd.
2. Klik in het venster aan de rechterkant op het tabblad **Ontwerpen**.
3. Klik in het venster **Binnenkomende verwerking** op het potlood.
4. Selecteer in het tabblad **Simuleren** **Statische antwoorden** voor **Gedrag simuleren**.
5. In het **API Management wordt het volgende antwoord geretourneerd:** text box, type **200 OK, application/json**. Deze selectie geeft aan dat uw API het voorbeeldantwoord zou moeten retourneren dat u hebt gedefinieerd in de vorige sectie.
6. Selecteer **Opslaan**.

## <a name="test-the-mocked-api"></a>De gesimuleerde API testen

1. Selecteer de API die u in de stap 'Een test-API maken' hebt gemaakt.
2. Open het tabblad **Test**.
3. Zorg ervoor dat de **Testaanroep**-API is geselecteerd.

    > [!TIP]
    > Een gele balk met de tekst **Simuleren is ingeschakeld** geeft aan dat reacties die zijn geretourneerd door API Management een simulatiebeleid en niet een daadwerkelijk antwoord van de back-end verzenden.

3. Selecteer **Verzenden** om een testaanroep uit te voeren.
4. Het **HTTP-antwoord** geeft de JSON weer die is opgegeven als een voorbeeld in de eerste sectie van de zelfstudie.

## <a name="video"></a>Video

> [!VIDEO https://www.youtube.com/embed/i9PjUAvw7DQ]
> 
> 

## <a name="next-steps"></a>Volgende stappen
In deze zelfstudie heeft u het volgende geleerd:

> [!div class="checklist"]
> * Een test-API maken
> * Een bewerking aan de test-API toevoegen
> * Antwoordsimulatie inschakelen
> * De gesimuleerde API testen

Ga door naar de volgende zelfstudie:

> [!div class="nextstepaction"]
> [Een gepubliceerde API transformeren en beveiligen](transform-api.md)
