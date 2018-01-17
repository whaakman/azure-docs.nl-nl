---
title: 'Azure Analysis Services-zelfstudie - Les 2: Gegevens ophalen | Microsoft Docs'
description: In deze les wordt beschreven hoe u gegevens ophaalt en importeert voor een tabellair model in Azure Analysis Services.
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
ms.openlocfilehash: 138f9f6e85d5e206c8b09d5c93822cfef5dd1246
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="get-data"></a>Gegevens ophalen

In deze les gebruikt u Get Data in SSDT om verbinding te maken met de AdventureWorks-voorbeelddatabase, gegevens te selecteren, een voorbeeld van de gegevens te bekijken en ze te filteren, en ze ten slotte in uw modelwerkruimte te importeren.  
  
Met behulp van Get Data kunt u gegevens importeren uit een groot aantal gegevensbronnen: Azure SQL Database, Oracle, Sybase, OData-Feed, Teradata, bestanden en meer. U kunt ook query's uitvoeren op gegevens met een formule-expressie van Power Query M.

> [!NOTE]
> De taken en afbeeldingen in deze zelfstudie laten zien hoe u verbinding maakt met een AdventureWorksDW2014-database op een on-premises server. In sommige gevallen kan een AdventureWorks-database in Azure afwijken.
  
Geschatte tijd voor het voltooien van deze les: **10 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 1: Een nieuw project voor een tabellair model maken](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Een verbinding maken  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw2014-database"></a>Een verbinding maken met de database AdventureWorksDW2014:  
  
1.  Klik in Tabular Model Explorer met de rechtermuisknop op **Data Sources** > **Import from Data Source**.  
  
    Hiermee wordt de wizard Get Data gestart, die u stapsgewijs begeleidt bij het maken van een verbinding met een gegevensbron. Als u Tabular Model Explorer niet ziet, dubbelklikt u in **Solution Explorer** op **Model.bim** om het model te openen in de ontwerpfunctie. 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  Klik in Get Data op **Database** > **SQL Server database** > **Connect**.  
  
3.  Typ in het dialoogvenster **SQL Server database** in het vak **Server** de naam van de server waarop u de database AdventureWorksDW2014 hebt geïnstalleerd en klik vervolgens op **Connect**.  

4.  Wanneer u om referenties wordt gevraagd, moet u de referenties opgeven die door Analysis Services worden gebruikt om bij het importeren en verwerken van gegevens verbinding te maken met de gegevensbron. Selecteer **Impersonate Account** in de lijst **Impersonation Mode**, geef referenties op en klik op **Connect**. Het verdient aanbeveling een account te gebruiken waarvan het wachtwoord niet verloopt.

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > De veiligste methode om verbinding te maken met een gegevensbron is met behulp van een Windows-gebruikersaccount en wachtwoord.
  
5.  Selecteer in Navigator de database **AdventureWorksDW2014** en klik vervolgens op **OK**. Hiermee wordt de verbinding met de database tot stand gebracht. 
  
6.  Schakel in Navigator het selectievakje van de volgende tabellen in : **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** en **FactInternetSales**.  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
Als u op OK klikt, wordt Query Editor geopend. In de volgende sectie gaat u de gegevens selecteren die u wilt importeren.

  
## <a name="filter-the-table-data"></a>De tabelgegevens filteren  
Tabellen in de voorbeelddatabase AdventureWorksDW2014 bevatten gegevens die niet nodig zijn voor het model. Indien mogelijk moet u alle onnodige gegevens uitfilteren om ruimte te besparen die anders door het model in het geheugen wordt gebruikt. U gaat een aantal van de kolommen uit tabellen uitfilteren, zodat deze niet worden geïmporteerd in de werkruimtedatabase, of in de modeldatabase nadat deze is geïmplementeerd. 
  
#### <a name="to-filter-the-table-data-before-importing"></a>De tabelgegevens filteren voordat u gaat importeren:  
  
1.  Selecteer de tabel **DimCustomer** in Query Editor. U ziet een weergave van de tabel DimCustomer uit de gegevensbron (de voorbeelddatabase AdventureWorksDW2014). 
  
2.  Selecteer de kolommen **SpanishEducation**, **FrenchEducation**, **SpanishOccupation** en **FrenchOccupation** (Ctrl ingedrukt houden en klikken), klik met de rechtermuisknop en klik op **Remove Columns**. 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    Aangezien de waarden voor deze kolommen niet relevant zijn voor de analyse van internetverkopen, is het niet nodig om deze kolommen te importeren. Door overbodige kolommen te verwijderen, wordt uw model kleiner en efficiënter.  

    > [!TIP]
    > Als u een fout maakt, kunt u een back-up maken door een stap te verwijderen in **TOEGEPASTE STAPPEN**.   
    
    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-step.png)

  
4.  Filter de resterende tabellen door de volgende kolommen te verwijderen uit elke tabel:  
    
    **DimDate**
    
      |Kolom|  
      |--------|  
      |**DateKey**|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Kolom|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Kolom|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Kolom|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Kolom|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      Geen kolommen verwijderd.
  
## <a name="Import"></a>De geselecteerde tabellen en kolomgegevens importeren  
U hebt nu de overbodige gegevens uitgefilterd en kunt dus de rest van de gegevens gaan importeren. De wizard zorgt ervoor dat de tabelgegevens samen met eventuele relaties tussen tabellen worden geïmporteerd. Er worden nieuwe tabellen en kolommen gemaakt in het model en gegevens die u hebt uitgefilterd, worden niet geïmporteerd.  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a>De geselecteerde tabellen en kolomgegevens importeren:  
  
1.  Bekijk de selecties. Als alles er goed uitziet, klikt u op **Import**. In het dialoogvenster Data Processing ziet u de voortgangsstatus van het importeren van gegevens uit de gegevensbron naar de database in de werkruimte.
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  Klik op **Sluiten**.  

  
## <a name="save-your-model-project"></a>Het modelproject opslaan  
Het is belangrijk om uw modelproject vaak op te slaan.  
  
#### <a name="to-save-the-model-project"></a>Het modelproject opslaan:  
  
-   Klik op **File** > **Save All**.  
  
## <a name="whats-next"></a>Volgende stappen
[Les 3: Als gegevenstabel markeren](../tutorials/aas-lesson-3-mark-as-date-table.md).

  
  
