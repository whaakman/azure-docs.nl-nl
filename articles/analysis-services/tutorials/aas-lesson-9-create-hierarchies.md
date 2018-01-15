---
title: "Azure Analysis Services-zelfstudie - Les 9: Hiërarchieën maken | Microsoft Docs"
description: 
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
ms.openlocfilehash: 041096b1a93fdc671939b6d6715a7836d1977e3c
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-hierarchies"></a>Hiërarchieën maken

In deze les gaat u hiërarchieën maken. Hiërarchieën zijn groepen kolommen gerangschikt op niveaus. Zo kan een hiërarchie Geografie onderliggende niveaus bevatten voor land, provincie en stad. Hiërarchieën kunnen los van andere kolommen worden weergegeven in een clienttoepassing voor rapportagedoeleinden. Hierdoor is het makkelijker voor gebruikers om door een hiërarchie te navigeren of deze op te nemen in een rapport. Zie [Hiërarchieën](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular) voor meer informatie.
  
U maakt hiërarchieën met de *diagramweergave* van de ontwerpfunctie voor modellen. De gegevensweergave is niet geschikt voor het maken en beheren van hiërarchieën.  
  
Geschatte tijd voor het voltooien van deze les: **20 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 8: Perspectieven maken](../tutorials/aas-lesson-8-create-perspectives.md).  
  
## <a name="create-hierarchies"></a>Hiërarchieën maken  
  
#### <a name="to-create-a-category-hierarchy-in-the-dimproduct-table"></a>Een hiërarchie Category maken in de tabel DimProduct:  
  
1.  Klik in de ontwerpfunctie voor modellen (in de diagramweergave) met de rechtermuisknop op de tabel **DimProduct** > **Create Hierarchy**. Er wordt onderaan het tabelvenster een nieuwe hiërarchie weergegeven. Wijzig de naam van de hiërarchie in **Category**.  
  
2.  Sleep de kolom **ProductCategoryName** naar de nieuwe hiërarchie **Category**.  
  
3.  Klik in de hiërarchie **Category** met de rechtermuisknop op **ProductCategoryName** > **Rename** en typ vervolgens **Category**.  
  
    > [!NOTE]  
    > Als u de naam van een kolom wijzigt in een hiërarchie, wordt de naam van die kolom niet gewijzigd in de onderliggende tabel. Een kolom in een hiërarchie is alleen maar een weergave van de kolom in de tabel.  
  
4.  Sleep de kolom **ProductSubcategoryName** naar de hiërarchie **Category**. Wijzig de naam in **Subcategory**. 
  
5.  Klik met de rechtermuisknop op de kolom **ModelName** > **Add to hierarchy** en selecteer vervolgens **Category**. Wijzig de naam in **Model**.

6.  Voeg als laatste **EnglishProductName** toe aan de hiërarchie. Wijzig de naam in **Product**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="to-create-hierarchies-in-the-dimdate-table"></a>Hiërarchieën maken in de tabel DimDate:  
  
1.  Maak in de tabel **DimDate** een hiërarchie met de naam **Calendar**.  
  
3.  Voeg de volgende kolommen in deze volgorde toe:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  Maak in de tabel **DimDate** een hiërarchie met de naam **Fiscal**. Voeg de volgende kolommen in deze volgorde toe:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Maak ten slotte in de tabel **DimDate** een hiërarchie **ProductionCalendar**. Voeg de volgende kolommen in deze volgorde toe:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Volgende stappen
[Les 10: Partities maken](../tutorials/aas-lesson-10-create-partitions.md). 
  
  
