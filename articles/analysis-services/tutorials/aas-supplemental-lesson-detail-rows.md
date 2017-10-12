---
title: 'Azure Analysis Services-zelfstudie - Aanvullende les: Detailrijen | Microsoft Docs'
description: In deze les wordt beschreven hoe u een detailrijenexpressie maakt in de zelfstudie over Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.openlocfilehash: 9995ad39d9e3fd1a211c513d4097398e99eefc54
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="supplemental-lesson---detail-rows"></a>Aanvullende les: Detailrijen

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In deze aanvullende les gaat u DAX Editor gebruiken voor het definiëren van een aangepaste detailrijenexpressie. Een detailrijenexpressie is een eigenschap van een meting die eindgebruikers meer informatie biedt over de cumulatieve resultaten van een meting. 
  
Geschatte tijd voor het voltooien van deze les: **10 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Deze aanvullende les maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. U kunt de taken in deze aanvullende les pas uitvoeren nadat u alle voorgaande lessen hebt afgerond of het voorbeeldproject Adventure Works Internet Sales hebt voltooid.  
  
## <a name="what-do-we-need-to-solve"></a>Wat moeten we oplossen?
Laten we eens kijken naar de details van onze meting InternetTotalSales, voordat we een detailrijenexpressie gaan toevoegen.

1.  Klik in SSDT op het menu **Model** > **Analyze in Excel** om Excel te openen met een lege draaitabel.
  
2.  Ga naar **Draaitabelvelden** en voeg de meting **InternetTotalSales** uit de tabel FactInternetSales toe aan **Waarden**, **CalendarYear** uit de tabel DimDate aan **Kolommen** en **EnglishCountryRegionName** aan **Rijen**. Onze draaitabel toont nu geaggregeerde resultaten van de meting InternetTotalSales per regio en per jaar. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. Dubbelklik in de draaitabel op een geaggregeerde waarde voor een jaar en een regionaam. In dit voorbeeld hebben we gedubbelklikt op de waarde voor Australië en het jaar 2014. Er wordt een nieuw blad geopend met gegevens, maar dit zijn niet echt nuttige gegevens.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Wat we graag willen zien is een tabel met kolommen en rijen met gegevens die bijdragen aan het geaggregeerde resultaat van onze meting InternetTotalSales. Dit kan door een detailrijenexpressie toe te voegen als een eigenschap van de meting.

## <a name="add-a-detail-rows-expression"></a>Een detailrijenexpressie maken

#### <a name="to-create-a-detail-rows-expression"></a>Een detailrijenexpressie maken: 
  
1. Klik in SSDT, in het metingenraster van de tabel FactInternetSales, op de meting **InternetTotalSales**. 

2. Ga naar **Properties** > **Detail Rows Expression** en klik op de knop hieronder om DAX Editor te openen.

    ![aas-lesson-detail-rows-ellipse](../tutorials/media/aas-lesson-detail-rows-ellipse.png)

3. Voer in DAX Editor de volgende expressie in:

    ```
    SELECTCOLUMNS(
    FactInternetSales,
    "Sales Order Number", FactInternetSales[SalesOrderNumber],
    "Customer First Name", RELATED(DimCustomer[FirstName]),
    "Customer Last Name", RELATED(DimCustomer[LastName]),
    "City", RELATED(DimGeography[City]),
    "Order Date", FactInternetSales[OrderDate],
    "Internet Total Sales", [InternetTotalSales]
    )

    ```

    Deze expressie bevat namen, kolommen en meetresultaten uit de tabel FactInternetSales en er worden gerelateerde tabellen geretourneerd wanneer een gebruiker dubbelklikt op een geaggregeerd resultaat in een draaitabel of -rapport.

4. Ga terug naar Excel, verwijder van het blad dat we hebben gemaakt in stap 3 en dubbelklik op een geaggregeerde waarde. Nu we een detailrijenexpressie hebben gedefinieerd voor de meting, wordt er een nieuw blad geopend met gegevens waar we heel veel aan hebben.

    ![aas-lesson-detail-rows-detailsheet](../tutorials/media/aas-lesson-detail-rows-detailsheet.png)

5. Implementeer het model opnieuw.

  
## <a name="see-also"></a>Zie ook  
[SELECTCOLUMNS Function (DAX)](https://msdn.microsoft.com/library/mt761759.aspx)  (SELECTCOLUMNS, functie (DAX))  
[Aanvullende les: Dynamische beveiliging](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Aanvullende les: Onregelmatige hiërarchieën](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)  
