---
title: 'Azure Analysis Services-zelfstudie - Les 5: Berekende kolommen maken | Microsoft Docs'
description: In deze les wordt beschreven hoe u berekende kolommen maakt in de zelfstudie over Azure Analysis Services.
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
ms.date: 09/20/2017
ms.author: owend
ms.openlocfilehash: eab74fbadc6a143ca5a2bc57a1762539a6d489c1
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2017
---
# <a name="lesson-5-create-calculated-columns"></a>Les 5: Berekende kolommen maken

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In deze les maakt u gegevens in het model door berekende kolommen toe te voegen. U kunt berekende kolommen maken (als aangepaste kolommen) wanneer u Get Data uitvoert, met behulp van Query-editor of in de ontwerpfunctie voor modellen. De laatste methode gaan we verderop toepassen. Zie [Berekende kolommen](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns) voor meer informatie.
  
U gaat vijf nieuwe berekende kolommen maken in drie verschillende tabellen. De stappen verschillen enigszins per taak, om aan te tonen dat er meerdere manieren zijn om kolommen te maken, ze een andere naam te geven en ze op diverse locaties in een tabel weer te geven.  

Dit is trouwens ook de les waarin we voor het eerst DAX (Data Analysis Expressions) gaan gebruiken. DAX is een speciale taal voor het maken van in hoge mate aanpasbare formule-expressies voor tabellaire modellen. In deze zelfstudie gebruikt u DAX voor het maken van berekende kolommen, metingen en rolfilters. Zie [DAX in tabellaire modellen](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular) voor meer informatie. 
  
Geschatte tijd voor het voltooien van deze les: **15 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 4: Relaties maken](../tutorials/aas-lesson-4-create-relationships.md). 
  
## <a name="create-calculated-columns"></a>Berekende kolommen maken  
  
#### <a name="create-a-monthcalendar-calculated-column-in-the-dimdate-table"></a>Een berekende kolom MonthCalendar maken in de tabel DimDate:  
  
1.  Klik op het menu **Model** > **Model View** > **Data View**.  
  
    Berekende kolommen kunnen alleen worden gemaakt met behulp van de ontwerpfunctie voor modellen in de gegevensweergave.  
  
2.  Klik in de ontwerpfunctie voor modellen op de tabel (het tabblad) **DimDate**.  
  
3.  Klik met de rechtermuisknop op de kolomkop **CalendarQuarter** en klik vervolgens op **Insert Column**.  
  
    Er wordt links van de kolom **Calendar Quarter** een nieuwe kolom ingevoegd met de naam **Calculated Column 1**.  
  
4.  Typ op de formulebalk boven de tabel de volgende DAX-formule. De functie Automatisch aanvullen zorgt ervoor dat u makkelijk de volledig gekwalificeerde namen van kolommen en tabellen kunt invullen en toont bovendien de functies die beschikbaar zijn.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Vervolgens worden er voor alle rijen in de berekende kolom waarden ingevuld. Als u omlaag door de tabel schuift, ziet u dat rijen verschillende waarden kunnen hebben voor deze kolom, op basis van de gegevens in elke rij.    
  
5.  Wijzig de naam van deze kolom in **MonthCalendar**. 

    ![aas-lesson5-newcolumn](../tutorials/media/aas-lesson5-newcolumn.png) 
  
De berekende kolom MonthCalendar bevat een sorteerbare naam voor de maanden.  
  
#### <a name="create-a-dayofweek-calculated-column-in-the-dimdate-table"></a>Een berekende kolom DayOfWeek maken in de tabel DimDate:  
  
1.  Zorg dat de tabel **DimDate** nog actief is en klik vervolgens op het menu **Column** en **Add Column**.  
  
2.  Typ op de formulebalk de volgende formule:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Druk op Enter als de formule klaar is. De nieuwe kolom wordt aan de rechterkant van de tabel toegevoegd.  
  
3.  Wijzig de naam van de kolom in **DayOfWeek**.  
  
4.  Klik op de kolomkop en sleep de kolom tussen de kolom **EnglishDayNameOfWeek** en de kolom **DayNumberOfMonth**.  
  
    > [!TIP]  
    > U kunt makkelijker navigeren door kolommen in de tabel te verplaatsen.  
  
De berekende kolom DayOfWeek bevat een sorteerbare naam voor de dagen van de week.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-the-dimproduct-table"></a>Een berekende kolom ProductSubcategoryName maken in de tabel DimProduct:  
  
  
1.  Schuif in de tabel **DimProduct** helemaal naar de rechterkant van de tabel. U ziet dat de meest rechtse kolom de naam **Add Column** (cursief) heeft. Klik op de kolomkop van deze kolom.  
  
2.  Typ op de formulebalk de volgende formule:  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Wijzig de naam van de kolom in **ProductSubcategoryName**.  
  
De berekende kolom ProductSubcategoryName wordt gebruikt voor het maken van een hiërarchie in de tabel DimProduct, die gegevens bevat uit de kolom EnglishProductSubcategoryName in de tabel DimProductSubcategory. Hiërarchieën kunnen niet meer dan één tabel omvatten. U gaat later hiërarchieën maken in les 9.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-the-dimproduct-table"></a>Een berekende kolom ProductCategoryName maken in de tabel DimProduct:  
  
1.  Zorg dat de tabel **DimProduct** nog actief is en klik vervolgens op het menu **Column** en **Add Column**.  
  
2.  Typ op de formulebalk de volgende formule:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Wijzig de naam van de kolom in **ProductCategoryName**.  
  
De berekende kolom ProductCategoryName wordt gebruikt voor het maken van een hiërarchie in de tabel DimProduct, die gegevens bevat uit de kolom EnglishProductCategoryName in de tabel DimProductCategory. Hiërarchieën kunnen niet meer dan één tabel omvatten.  
  
#### <a name="create-a-margin-calculated-column-in-the-factinternetsales-table"></a>Een berekende kolom Margin maken in de tabel FactInternetSales:  
  
1.  Selecteer in de ontwerpfunctie voor modellen de tabel **FactInternetSales**.  
  
2.  Maak een nieuwe berekende kolom tussen de kolommen **SalesAmount** en **TaxAmt**.  
  
3.  Typ op de formulebalk de volgende formule:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Wijzig de naam van de kolom in **Margin**.  
 
      ![aas-lesson5-newmargin](../tutorials/media/aas-lesson5-newmargin.png)
      
    De berekende kolom Margin wordt gebruikt voor het analyseren van winstmarge voor elke verkoop.  
  
## <a name="whats-next"></a>Volgende stappen
[Les 6: Metingen maken](../tutorials/aas-lesson-6-create-measures.md).
  
  
  
