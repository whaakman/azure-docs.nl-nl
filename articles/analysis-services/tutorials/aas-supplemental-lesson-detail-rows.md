---
title: 'Azure Analysis Services-zelfstudie - Aanvullende les: Detailrijen | Microsoft Docs'
description: In deze les wordt beschreven hoe u een detailrijenexpressie maakt in de zelfstudie over Azure Analysis Services.
author: minewiskan
manager: kfile
ms.service: azure-analysis-services
ms.topic: conceptual
ms.date: 07/03/2018
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: 626258488afec4b3c3f025ae85bd3b5866aa0cf3
ms.sourcegitcommit: 86cb3855e1368e5a74f21fdd71684c78a1f907ac
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/03/2018
ms.locfileid: "37443974"
---
# <a name="supplemental-lesson---detail-rows"></a>Aanvullende les: Detailrijen

In deze aanvullende les gaat u DAX Editor gebruiken voor het definiëren van een aangepaste detailrijenexpressie. Een detailrijenexpressie is een eigenschap van een meting die eindgebruikers meer informatie biedt over de cumulatieve resultaten van een meting. 
  
Geschatte tijd voor het voltooien van deze les: **10 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Deze aanvullende les maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. U kunt de taken in deze aanvullende les pas uitvoeren nadat u alle voorgaande lessen hebt afgerond of het voorbeeldproject Adventure Works Internet Sales hebt voltooid.  
  
## <a name="whats-the-issue"></a>Wat is het probleem?
Laten we kijken naar de details van de meting InternetTotalSales, voordat u een Detailrijenexpressie toe te voegen.

1.  Klik in SSDT op het menu **Model** > **Analyze in Excel** om Excel te openen met een lege draaitabel.
  
2.  Ga naar **Draaitabelvelden** en voeg de meting **InternetTotalSales** uit de tabel FactInternetSales toe aan **Waarden**, **CalendarYear** uit de tabel DimDate aan **Kolommen** en **EnglishCountryRegionName** aan **Rijen**. De draaitabel maakt nu een samengevoegde resultaten van de meting InternetTotalSales per regio en per jaar. 

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-pivottable.png)

3. Dubbelklik in de draaitabel op een geaggregeerde waarde voor een jaar en een regionaam. De waarde voor Australië en het jaar 2014. Er wordt een nieuw blad geopend met gegevens, maar dit zijn niet echt nuttige gegevens.

    ![aas-lesson-detail-rows-pivottable](../tutorials/media/aas-lesson-detail-rows-sheet.png)
  
Het doel hier is een tabel met kolommen en rijen met gegevens die aan het geaggregeerde resultaat van de meting InternetTotalSales bijdragen. Om dit te doen, voeg u een Detailrijenexpressie toe als een eigenschap van de meting.

## <a name="add-a-detail-rows-expression"></a>Een detailrijenexpressie maken

#### <a name="to-create-a-detail-rows-expression"></a>Een detailrijenexpressie maken: 
  
1. Klik in het metingenraster van de tabel FactInternetSales op de meting **InternetTotalSales**. 

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
[Aanvullende les: dynamische beveiliging](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Aanvullende les: Onregelmatige hiërarchieën](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   
 