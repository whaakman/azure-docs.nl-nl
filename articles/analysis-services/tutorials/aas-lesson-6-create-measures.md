---
title: 'Azure Analysis Services-zelfstudie - Les 6: Metingen maken| Microsoft Docs'
description: In deze les wordt beschreven hoe u metingen maakt in de zelfstudie over Azure Analysis Services.
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: kfile
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/08/2018
ms.author: owend
ms.openlocfilehash: fa47d4ea9aa019464e465c051b016dac7c224dc9
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-measures"></a>Metingen maken

In deze les maakt u metingen voor het model. Net als bij de berekende kolommen die u in de vorige les hebt gemaakt, wordt voor het maken van een meting ook een DAX-formule gebruikt. In tegenstelling tot berekende kolommen worden metingen echter geëvalueerd op basis van een *filter* dat de gebruiker selecteert, zoals een bepaalde kolom of een slicer die is toegevoegd aan het veld Rijlabels in een draaitabel. Vervolgens wordt voor elke cel in het filter een waarde berekend door de toegepaste meting. Metingen zijn krachtige, flexibele berekeningen die u in bijna alle tabellaire modellen zult gebruiken om dynamische berekeningen ui te voeren op numerieke gegevens. Zie [Metingen](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular) voor meer informatie.
  
Voor het maken van metingen gebruikt u het *metingenraster*. Elke tabel heeft standaard een leeg metingenraster, maar meestal hoeft u niet voor elke tabel metingen te maken. Het raster wordt weergegeven onder een tabel in de gegevensweergave van de ontwerpfunctie voor modellen. Als u het raster voor een tabel wilt verbergen of weergeven, klikt u in het menu **Table** op **Show Measure Grid**.  
  
U kunt een meting maken door te klikken op een lege cel in het raster en vervolgens een DAX-formule te typen op de formulebalk. Wanneer u op Enter drukt om de formule te voltooien, wordt de meting weergegeven in de cel. U kunt ook metingen maken met behulp van een standaardaggregatiefunctie. U klikt dan op een kolomkop en vervolgens op de knop AutoSom (**∑**) op de werkbalk. Metingen die zijn gemaakt met de functie AutoSom worden in het metingenraster direct onder de kolom weergegeven, maar kunnen desgewenst worden verplaatst.  
  
In deze les gaat u metingen maken door een DAX-formule in te voeren op de formulebalk en via de functie AutoSom.  
  
Geschatte tijd voor het voltooien van deze les: **30 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 5: Berekende kolommen maken](../tutorials/aas-lesson-5-create-calculated-columns.md).  
  
## <a name="create-measures"></a>Metingen maken  
  
#### <a name="to-create-a-dayscurrentquartertodate-measure-in-the-dimdate-table"></a>Een meting DaysCurrentQuarterToDate maken in de tabel DimDate:  
  
1.  Klik in de ontwerpfunctie voor modellen op de tabel **DimDate**.  
  
2.  Klik in het metingenraster op de lege cel linksboven.  
  
3.  Typ op de formulebalk de volgende formule:  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    U ziet dat in de cel linksboven nu de naam van een meting staat, **DaysCurrentQuarterToDate**, gevolgd door het resultaat, **92**. Het resultaat is op dit moment niet relevant omdat er geen gebruikersfilter is toegepast.
    
      ![aas-lesson6-newmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    In tegenstelling tot berekende kolommen kunt u in formules voor het maken van een meting de naam van de meting typen, gevolgd door een dubbele punt en de formule-expressie.

  
#### <a name="to-create-a-daysincurrentquarter-measure-in-the-dimdate-table"></a>Een meting DaysInCurrentQuarter maken in de tabel DimDate:  
  
1.  Zorg dat de tabel **DimDate** nog steeds actief is in de ontwerpfunctie voor modellen en klik in het metingenraster op de lege cel onder de meting die u hebt gemaakt.  
  
2.  Typ op de formulebalk de volgende formule:  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Bij het maken van een vergelijking tussen een onvolledige periode en de vorige periode, moet de formule het aandeel berekenen van de periode die is verstreken en dit vergelijken met hetzelfde aandeel in de vorige periode. In dit geval kan met [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] het aandeel voor de huidige periode worden vastgesteld.  
  
#### <a name="to-create-an-internetdistinctcountsalesorder-measure-in-the-factinternetsales-table"></a>Een meting InternetDistinctCountSalesOrder maken in de tabel FactInternetSales  
  
1.  Klik op de tabel **FactInternetSales**.   
  
2.  Klik op de kolomkop **SalesOrderNumber**.  
  
3.  Klik op de werkbalk op de pijl-omlaag naast de knop AutoSom (**∑**) en selecteer vervolgens **DistinctCount**.  
  
    De functie AutoSom maakt automatisch een meting voor de geselecteerde kolom met behulp van de standaardaggregatieformule DistinctCount.  
    
       ![aas-lesson6-newmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  Klik in het metingenraster op de nieuwe meting en ga vervolgens in het venster **Properties** naar **Measure Name** en wijzig de naam van de meting in **InternetDistinctCountSalesOrder**. 
 
  
#### <a name="to-create-additional-measures-in-the-factinternetsales-table"></a>Aanvullende metingen maken in de tabel FactInternetSales:  
  
1.  Maak met behulp van de functie AutoSom de volgende metingen:  

    |Kolom|Naam van meting|AutoSom (∑)|Formule|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Count|=COUNTA([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Sum|=SUM([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Sum|=SUM([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Sum|=SUM([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Sum|=SUM([SalesAmount])|  
    |Margin|InternetTotalMargin|Sum|=SUM([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Sum|=SUM([TaxAmt])|  
    |Freight|InternetTotalFreight|Sum|=SUM([Freight])|  
  
2.  Maak in deze volgorde de volgende metingen door op een lege cel in het metingenraster te klikken en de formules in te voeren op de formulebalk:  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
Metingen die zijn gemaakt voor de tabel FactInternetSales kunnen worden gebruikt om essentiële financiële gegevens te analyseren (zoals omzet, kosten en winstmarge) voor items die zijn gedefinieerd met het filter dat de gebruiker heeft geselecteerd.  
  
## <a name="whats-next"></a>Volgende stappen
[Les 7: Key Performance Indicators maken](../tutorials/aas-lesson-7-create-key-performance-indicators.md).  

  
