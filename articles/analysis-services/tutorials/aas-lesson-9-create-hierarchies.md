---
title: "Azure Analysis Services-zelfstudie - Les 9: Hiërarchieën maken | Microsoft Docs"
description: 
services: analysis-services
documentationcenter: 
author: minewiskan
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
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: d628dc621335acf231342a6d9186079de16e85f4
ms.contentlocale: nl-nl
ms.lasthandoff: 06/03/2017

---
<a id="lesson-9-create-hierarchies" class="xliff"></a>

# Les 9: Hiërarchieën maken

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In deze les gaat u hiërarchieën maken. Hiërarchieën zijn groepen van kolommen die op niveaus zijn gerangschikt. Zo kan een hiërarchie Geografie onderliggende niveaus bevatten voor land, provincie en stad. Hiërarchieën kunnen los van andere kolommen worden weergegeven in een clienttoepassing voor rapportagedoeleinden. Hierdoor is het makkelijker voor gebruikers om door een hiërarchie te navigeren of deze op te nemen in een rapport. Zie [Hiërarchieën](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular) voor meer informatie.
  
U maakt hiërarchieën met de *diagramweergave* van de ontwerpfunctie voor modellen. De gegevensweergave is niet geschikt voor het maken en beheren van hiërarchieën.  
  
Geschatte tijd voor het voltooien van deze les: **20 minuten**  
  
<a id="prerequisites" class="xliff"></a>

## Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 8: Perspectieven maken](../tutorials/aas-lesson-8-create-perspectives.md).  
  
<a id="create-hierarchies" class="xliff"></a>

## Hiërarchieën maken  
  
<a id="to-create-a-category-hierarchy-in-the-dimproduct-table" class="xliff"></a>

#### Een hiërarchie Category maken in de tabel DimProduct:  
  
1.  Klik in de ontwerpfunctie voor modellen (in de diagramweergave) met de rechtermuisknop op de tabel **DimProduct** > **Create Hierarchy**. Er wordt onderaan het tabelvenster een nieuwe hiërarchie weergegeven. Wijzig de naam van de hiërarchie in **Category**.  
  
2.  Sleep de kolom **ProductCategoryName** naar de nieuwe hiërarchie **Category**.  
  
3.  Klik in de hiërarchie **Category** met de rechtermuisknop op **ProductCategoryName** > **Rename** en typ vervolgens **Category**.  
  
    > [!NOTE]  
    > Als u de naam van een kolom wijzigt in een hiërarchie, wordt de naam van die kolom niet gewijzigd in de onderliggende tabel. Een kolom in een hiërarchie is alleen maar een weergave van de kolom in de tabel.  
  
4.  Sleep de kolom **ProductSubcategoryName** naar de hiërarchie **Category**. Wijzig de naam in **Subcategory**. 
  
5.  Klik met de rechtermuisknop op de kolom **ModelName** > **Add to hierarchy** en selecteer vervolgens **Category**. Wijzig de naam in **Model**.

6.  Voeg als laatste **EnglishProductName** toe aan de hiërarchie. Wijzig de naam in **Product**.  

    ![aas-lesson9-category](../tutorials/media/aas-lesson9-category.png)
  
<a id="to-create-hierarchies-in-the-dimdate-table" class="xliff"></a>

#### Hiërarchieën maken in de tabel DimDate:  
  
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
  
<a id="whats-next" class="xliff"></a>

 ## Volgende stappen
[Les 10: Partities maken](../tutorials/aas-lesson-10-create-partitions.md). 
  
  

