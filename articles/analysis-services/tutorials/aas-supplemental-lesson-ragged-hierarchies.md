---
title: "Azure Analysis Services-zelfstudie - Aanvullende les: Onregelmatige hiërarchieën | Microsoft Docs"
description: "In deze les wordt beschreven hoe u onregelmatige hiërarchieën herstelt in de zelfstudie over Azure Analysis Services."
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
ms.openlocfilehash: c5c4a687ffe512b15372d152b517834771e46328
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="supplemental-lesson---ragged-hierarchies"></a>Aanvullende les: Onregelmatige hiërarchieën

In deze aanvullende les gaat u een probleem oplossen dat vaak optreedt bij het maken van een draaitabel van hiërarchieën die op verschillende niveaus lege waarden (leden) bevatten. Dit is bijvoorbeeld het geval als een hoge manager in een organisatie zowel afdelingsmanagers als niet-managers als direct ondergeschikten heeft. Of bij een geografische hiërarchie die bestaat uit land-provincie-stad, waarbij voor sommige steden geen bovenliggende provincie is ingevoerd. Wanneer een hiërarchie lege leden heeft, worden deze vaak overgebracht naar andere, afwijkende niveaus waardoor er een onregelmatige hiërarchie ontstaat.

![aas-lesson-detail-ragged-hierarchies-table](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

Tabellaire modellen op het compatibiliteitsniveau 1400 hebben een extra eigenschap **Hide Members** voor hiërarchieën. Bij de instelling **Default** voor deze eigenschap wordt ervan uitgegaan dat er op geen enkel niveau lege leden bestaan. Bij de instelling **Hide blank members** worden lege leden uitgesloten van de hiërarchie wanneer deze wordt toegevoegd aan een draaitabel of -rapport.  
  
Geschatte tijd voor het voltooien van deze les: **20 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Deze aanvullende les maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. U kunt de taken in deze aanvullende les pas uitvoeren nadat u alle voorgaande lessen hebt afgerond of het voorbeeldproject Adventure Works Internet Sales hebt voltooid. 

Als u tijdens de zelfstudie het project AW Internet Sales hebt gemaakt, bevat het model nog geen gegevens of hiërarchieën die onregelmatig zijn. Om deze aanvullende les te voltooien, moet u eerst het probleem introduceren door een paar extra tabellen toe te voegen, en relaties, berekende kolommen, een meting en een nieuwe hiërarchie Organization te maken. Dat gedeelte neemt ongeveer 15 minuten in beslag. Vervolgens hebt u nog maar een paar minuten nodig om het probleem op te lossen.  

## <a name="add-tables-and-objects"></a>Tabellen en objecten toevoegen
  
### <a name="to-add-new-tables-to-your-model"></a>Nieuwe tabellen toevoegen aan uw model
  
1.  Vouw in Tabular Model Explorer de optie **Data Sources** uit, klik met de rechtermuisknop op uw verbinding en klik vervolgens op **Import New Tables**.
  
2.  Selecteer in Navigator **DimEmployee** en **FactResellerSales**, en klik vervolgens op **OK**.

3.  Klik in Query-editor op **Import**.

4.  Maak de volgende [relaties](../tutorials/aas-lesson-4-create-relationships.md):

    | Tabel 1           | Kolom       | Filterrichting   | Tabel 2     | Kolom      | Actief |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Standaard            | DimDate     | Date        | Ja    |
    | FactResellerSales | DueDate      | Standaard            | DimDate     | Date        | Nee     |
    | FactResellerSales | ShipDateKey  | Standaard            | DimDate     | Date        | Nee     |
    | FactResellerSales | ProductKey   | Standaard            | DimProduct  | ProductKey  | Ja    |
    | FactResellerSales | EmployeeKey  | Naar beide tabellen | DimEmployee | EmployeeKey | Ja    |

5. Maak in de tabel **DimEmployee** de volgende [berekende kolommen](../tutorials/aas-lesson-5-create-calculated-columns.md): 

    **Pad** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **FullName** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Level1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Level2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],2,1)) 
    ```

    **Level3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],3,1)) 
    ```

    **Level4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],4,1)) 
    ```

    **Level5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],5,1)) 
    ```

6.  Maak in de tabel **DimEmployee** een [hiërarchie](../tutorials/aas-lesson-9-create-hierarchies.md) met de naam **Organization**. Voeg de volgende kolommen in deze volgorde toe: **Level1**, **Level2**, **Level3**, **Level4**, **Level5**.

7.  Maak in de tabel **FactResellerSales** de volgende [meting](../tutorials/aas-lesson-6-create-measures.md):

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  Gebruik [Analyse in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) om Excel te openen en automatisch een draaitabel te maken.

9.  Ga naar **Draaitabelvelden**, voeg de hiërarchie **Organization** uit de tabel **DimEmployee** toe aan **Rijen**, en de meting **ResellerTotalSales** uit de tabel **FactResellerSales** aan **Waarden**.

    ![aas-lesson-detail-ragged-hierarchies-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    Zoals u ziet in de draaitabel, bevat de hiërarchie onregelmatige rijen. Er zijn veel rijen waarin lege leden worden weergegeven.

## <a name="to-fix-the-ragged-hierarchy-by-setting-the-hide-members-property"></a>Het probleem met de onregelmatige hiërarchie oplossen door de eigenschap Hide Members in te stellen

1.  Ga naar **Tabular Model Explorer** en vouw **Tables** > **DimEmployee** > **Hierarchies** > **Organization** uit.

2.  Selecteer **Hide blank members** bij **Properties** > **Hide Members**. 

    ![aas-lesson-detail-ragged-hierarchies-hidemembers](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Ga terug naar Excel en vernieuw de draaitabel. 

    ![aas-lesson-detail-ragged-hierarchies-pivottable-refresh](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Dat ziet er al veel beter uit.

## <a name="see-also"></a>Zie ook   
[Les 9: Hiërarchieën maken](../tutorials/aas-lesson-9-create-hierarchies.md)  
[Aanvullende les: Dynamische beveiliging](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Aanvullende les: Detailrijen](../tutorials/aas-supplemental-lesson-detail-rows.md)  
