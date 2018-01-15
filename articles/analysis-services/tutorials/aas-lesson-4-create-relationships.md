---
title: 'Azure Analysis Services-zelfstudie - Les 4: Relaties maken | Microsoft Docs'
description: In deze les wordt beschreven hoe u relaties maakt in de zelfstudie over Azure Analysis Services.
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
ms.openlocfilehash: 57b3a172445047291f0aea5b1616b9dcbf6bf745
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-relationships"></a>Relaties maken

In deze les gaat u de relaties controleren die automatisch zijn gemaakt tijdens het importeren van gegevens en nieuwe relaties toevoegen tussen verschillende tabellen. Een relatie is een verbinding tussen twee tabellen die bepaalt hoe de gegevens in deze tabellen moeten worden gecorreleerd. De tabel DimProduct en de tabel DimProductSubcategory hebben bijvoorbeeld een relatie die is gebaseerd op het feit dat elk product tot een subcategorie behoort. Zie [Relaties](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular) voor meer informatie.
  
Geschatte tijd voor het voltooien van deze les: **10 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 3: Als gegevenstabel markeren](../tutorials/aas-lesson-3-mark-as-date-table.md). 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Bestaande relaties controleren en nieuwe relaties toevoegen  
Bij het importeren van gegevens met behulp van Get Data zijn zeven tabellen opgehaald uit de database AdventureWorksDW2014. Over het algemeen is het zo dat bij het importeren van gegevens uit een relationele gegevensbron, bestaande relaties automatisch samen met de gegevens worden geïmporteerd. Er moeten relaties bestaan tussen de tabellen in de gegevensbron om ervoor te zorgen dat met Get Data automatisch relaties worden gemaakt in het gegevensmodel.

Voordat u verdergaat met het ontwerpen van het model, moet u controleren of deze relaties tussen tabellen juist zijn gemaakt. Voor deze zelfstudie gaat u ook drie nieuwe relaties toevoegen.  

  
#### <a name="to-review-existing-relationships"></a>Bestaande relaties controleren:  
  
1.  Klik op het menu **Model** > **Model View** > **Diagram View**.  

    De ontwerpfunctie voor modellen wordt nu weergegeven in de diagramweergave. Dit is een grafische weergave waarin alle geïmporteerde tabellen met lijnen met elkaar zijn verbonden. De lijnen tussen de tabellen geven de relaties aan die automatisch zijn gemaakt bij het importeren van de gegevens.
    
    ![aas-lesson4-diagram](../tutorials/media/aas-lesson4-diagram.png)
  
    > [!NOTE]
    > Als u geen relaties tussen tabellen ziet, betekent dit waarschijnlijk dat er geen relaties zijn tussen deze tabellen in de gegevensbron.

    Voeg zo veel tabellen als mogelijk toe met behulp van de MiniMap-besturingselementen in de rechterbenedenhoek van de ontwerpfunctie. U kunt tabellen ook naar andere locaties slepen, tabellen dichter bij elkaar zetten of ze in een bepaalde volgorde plaatsen. Het verplaatsen van tabellen heeft geen invloed op de relaties tussen de tabellen. Als u alle kolommen in een bepaalde tabel wilt weergeven, sleept u een tabelrand om de tabel groter of kleiner te maken.  
  
2.  Klik op de ononderbroken lijn tussen de tabel **DimCustomer** en de tabel **DimGeography**. De ononderbroken lijn tussen deze twee tabellen geeft aan dat dit een actieve relatie is, wat inhoudt dat de relatie standaard wordt gebruikt bij het berekenen van DAX formules.  
  
    U ziet dat de kolom **GeographyKey** in de tabel **DimCustomer** en de kolom **GeographyKey** in de tabel **DimGeography** nu allebei in een kader worden weergegeven. Dit zijn de kolommen die worden gebruikt in de relatie. De eigenschappen van de relatie worden nu ook weergegeven in het venster **Properties**.  
  
    > [!TIP]  
    > U kunt de relaties tussen alle tabellen ook in tabelvorm weergeven met behulp van het dialoogvenster Manage Relationships. Klik hiervoor in Tabular Model Explorer met de rechtermuisknop op **Relationships** > **Manage Relationships**.
  
3.  Controleer of de volgende relaties zijn gemaakt tijdens het importeren van de tabellen uit de database AdventureWorksDW:  
  
    |Actief|Tabel|Gerelateerde opzoektabel|  
    |----------|---------|------------------------|  
    |Ja|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Ja|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Ja|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Ja|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Ja|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Als een van de relaties ontbreekt, controleert u of het model de volgende tabellen bevat: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory en FactInternetSales. Als tabellen uit dezelfde verbonden gegevensbron op verschillende momenten worden geïmporteerd, worden eventuele relaties tussen deze tabellen niet overgenomen en moeten deze handmatig worden gemaakt. Als er geen relaties worden weergegeven, betekent dit dat er zijn geen relaties zijn in de gegevensbron. U kunt ze handmatig maken in het gegevensmodel.

### <a name="take-a-closer-look"></a>Diagramweergave
In de diagramweergave ziet u een pijl, een sterretje en een nummer op de lijnen die de relatie tussen tabellen aangeven.

![aas-lesson4-line](../tutorials/media/aas-lesson4-line.png)

De pijl geeft de filterrichting aan. Het sterretje betekent dat deze tabel de veel-zijde vertegenwoordigt van de kardinaliteit van de relatie. Het nummer één betekent dat deze tabel de een-zijde van de relatie voorstelt. Als u een relatie wilt bewerken, bijvoorbeeld om de filterrichting van de relatie of de kardinaliteit te wijzigen, dubbelklikt u op de relatielijn om het dialoogvenster Edit Relationship te openen.

![aas-lesson4-edit](../tutorials/media/aas-lesson4-edit.png)

Deze functies zijn bedoeld voor geavanceerde gegevensmodellering en vallen buiten het bereik van deze zelfstudie. Ga voor meer informatie naar [Bi-directional cross filters - tabular models - Analysis Services](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services) (Bidirectionele kruisfilters voor tabellaire modellen in Analysis Services).

In sommige gevallen moet u mogelijk aanvullende relaties maken tussen tabellen in het model om ondersteuning in te bouwen voor bepaalde bedrijfslogica. Voor deze zelfstudie moet u drie extra relaties maken tussen de tabel FactInternetSales en de tabel DimDate.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Nieuwe relaties tussen tabellen toevoegen:  
  
1.  Ga in de ontwerpfunctie voor modellen naar de tabel **FactInternetSales** en sleep de kolom **OrderDate** naar de kolom **Date** in de tabel **DimDate**.  

    Er wordt een ononderbroken lijn weergegeven om aan te geven dat u een actieve relatie hebt gemaakt tussen de kolom **OrderDate** in de tabel **FactInternetSales** en de kolom **Date** in de tabel **Date**. 
  
      ![aas-lesson4-new](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > Wanneer u relaties maakt, worden de kardinaliteit en de filterrichting tussen de primaire tabel en de gerelateerde opzoektabel automatisch geselecteerd.  
  
2.  Sleep de kolom **DueDate** in de tabel **FactInternetSales** naar de kolom **Date** in de tabel **DimDate**.  
  
    Er wordt een stippellijn weergegeven om aan te geven dat u een inactieve relatie hebt gemaakt tussen de kolom **DueDate** in de tabel **FactInternetSales** en de kolom **Date** in de tabel **Date**. U kunt meerdere relaties maken tussen tabellen, maar er kan altijd maar één relatie actief zijn. Inactieve relaties kunnen actief worden gemaakt om speciale aggregaties uit te voeren in aangepaste DAX-expressies.  
  
3.  We gaan ter afsluiting nog één relatie maken. Sleep de kolom **ShipDate** in de tabel **FactInternetSales** naar de kolom **Date** in de tabel **DimDate**.  
    
     ![aas-lesson4-newinactive](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Volgende stappen
[Les 5: Berekende kolommen maken](../tutorials/aas-lesson-5-create-calculated-columns.md).
  
  
  
