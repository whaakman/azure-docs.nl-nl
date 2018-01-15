---
title: 'Azure Analysis Services-zelfstudie - Les 10: Partities maken | Microsoft Docs'
description: In deze les wordt beschreven hoe u partities maakt in de zelfstudie over Azure Analysis Services.
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
ms.openlocfilehash: a73836b784a5e86d01df51fb83d619890d56502a
ms.sourcegitcommit: 176c575aea7602682afd6214880aad0be6167c52
ms.translationtype: HT
ms.contentlocale: nl-NL
ms.lasthandoff: 01/09/2018
---
# <a name="create-partitions"></a>Partities maken

In deze les gaat u partities maken om de tabel FactInternetSales op te splitsen in kleinere, logische delen die onafhankelijk van andere partities kunnen worden verwerkt (vernieuwd). Elke tabel die u in een model opneemt, heeft standaard één partitie, met daarin alle kolommen en rijen van de tabel. Voor de tabel FactInternetSales willen we de gegevens per jaar opsplitsen; één partitie voor elk van vijf jaren in de tabel. Elke partitie kan vervolgens onafhankelijk worden verwerkt. Zie [Partities](https://docs.microsoft.com/sql/analysis-services/tabular-models/partitions-ssas-tabular) voor meer informatie. 
  
Geschatte tijd voor het voltooien van deze les: **15 minuten**  
  
## <a name="prerequisites"></a>Vereisten  
Dit onderwerp maakt deel uit van een zelfstudie over het ontwerpen van een tabellair model. De lessen van de zelfstudie moeten op volgorde worden uitgevoerd. Voordat u de taken in deze les gaat uitvoeren, moet u de vorige les hebben voltooid: [Les 9: Hiërarchieën maken](../tutorials/aas-lesson-9-create-hierarchies.md).  
  
## <a name="create-partitions"></a>Partities maken  
  
#### <a name="to-create-partitions-in-the-factinternetsales-table"></a>Partities maken in de tabel FactInternetSales:  
  
1.  Vouw in Tabular Model Explorer het gedeelte **Tables** uit en klik vervolgens met de rechtermuisknop op **FactInternetSales** > **Partitions**.  
  
2.  Klik in Partition Manager op **Copy** en wijzig de naam vervolgens in **FactInternetSales2010**.
  
    Omdat de partitie alleen de rijen voor een bepaalde periode moet bevatten, namelijk voor het jaar 2010, moet u de query-expressie aanpassen.
  
4.  Klik op **Design** om Query-editor te openen en klik vervolgens op de query **FactInternetSales2010**.

5.  Klik in het voorbeeld op de pijl-omlaag in de kolomkop **OrderDate** en klik vervolgens op **Date/Time Filters** > **Between**.

    ![aas-lesson10-query-editor](../tutorials/media/aas-lesson10-query-editor.png)

6.  Laat in het dialoogvenster Filter Rows bij **Show rows where: OrderDate** de instelling **is after or equal to** staan en voer in het datumveld de datum **1/1/2010** in. Laat de operator **And** ingeschakeld, selecteer **is before**, typ **1/1/2011** in het datumveld en klik ten slotte op **OK**.

    ![aas-lesson10-filter-rows](../tutorials/media/aas-lesson10-filter-rows.png)
    
    In Query-editor ziet u, bij TOEGEPASTE STAPPEN, een andere stap met de naam Filtered Rows. Dit filter is om alleen orderdatums uit 2010 te selecteren.

8.  Klik op **Import**.

    In Partition Manager is nu de component Filtered Rows toegevoegd aan de query-expressie.

    ![aas-lesson10-query](../tutorials/media/aas-lesson10-query.png)
  
    Deze instructie bepaalt dat deze partitie alleen gegevens moet weergeven uit de rijen waarvoor de OrderDate in het kalenderjaar 2010 valt, zoals ingesteld in de component Filtered Rows.  
  
  
#### <a name="to-create-a-partition-for-the-2011-year"></a>Een partitie maken voor het jaar 2011:  
  
1.  Klik in de lijst met partities op de partitie **FactInternetSales2010** die u hebt gemaakt en klik vervolgens op **Copy**.  Wijzig de partitienaam in **FactInternetSales2011**. 

    U hoeft Query-editor niet te gebruiken om een nieuwe component Filtered Rows te maken. Omdat u een kopie van de query voor 2010 hebt gemaakt, hoeft u alleen maar een kleine wijziging door te voeren in de query voor 2011.
  
2.  Om met deze partitie alleen de rijen voor het jaar 2011 weer te geven, moet u bij **Query Expression** de jaren in de component Filtered Rows vervangen door respectievelijk **2011** en **2012**, zoals hier:  
  
    ```  
    let
        Source = #"SQL/localhost;AdventureWorksDW2014",
        dbo_FactInternetSales = Source{[Schema="dbo",Item="FactInternetSales"]}[Data],
        #"Removed Columns" = Table.RemoveColumns(dbo_FactInternetSales,{"OrderDateKey", "DueDateKey", "ShipDateKey"}),
        #"Filtered Rows" = Table.SelectRows(#"Removed Columns", each [OrderDate] >= #datetime(2011, 1, 1, 0, 0, 0) and [OrderDate] < #datetime(2012, 1, 1, 0, 0, 0))
    in
        #"Filtered Rows"
   
    ```  
  
#### <a name="to-create-partitions-for-2012-2013-and-2014"></a>Partities maken voor 2012, 2013 en 2014:  
  
- Volg de vorige stappen om partities te maken voor 2012, 2013 en 2014, en wijzig vervolgens de jaartallen in de component Filtered Rows, zodat alleen rijen voor dat jaar worden opgenomen. 
  

## <a name="delete-the-factinternetsales-partition"></a>De partitie FactInternetSales verwijderen
We hebben nu partities voor elk jaar en kunnen dus de partitie FactInternetSales verwijderen, zodat er geen overlap ontstaat als er bij het verwerken van partities de optie Process all kiezen.

#### <a name="to-delete-the-factinternetsales-partition"></a>De partitie FactInternetSales verwijderen:
-  Klik op de partitie FactInternetSales en klik vervolgens op **Delete**.



## <a name="process-partitions"></a>Partities verwerken  
In Partition Manager kunt u in de kolom **Last Processed** van de nieuwe partities zien dat deze partities nog nooit zijn verwerkt. Wanneer u partities maakt, moet u een bewerking Process Partitions of Process Table uitvoeren om de gegevens in deze partities te vernieuwen.  
  
#### <a name="to-process-the-factinternetsales-partitions"></a>De FactInternetSales-partities verwerken:  
  
1.  Klik op **OK** om Partition Manager te sluiten.  
  
2.  Klik op de tabel **FactInternetSales** en klik vervolgens op het menu **Model** > **Process** > **Process Partitions**.  
  
3.  Controleer in het dialoogvenster Process Partitions of **Mode** is ingesteld op **Process Default**.  
  
4.  Schakel in de kolom **Process** het selectievakje in voor elk van de vijf partities die u hebt gemaakt en klik vervolgens op **OK**.  

    ![aas-lesson10-process-partitions](../tutorials/media/aas-lesson10-process-partitions.png)
  
    Als u wordt gevraagd om referenties, gebruikt u de Windows-gebruikersnaam en het wachtwoord die u hebt opgegeven in les 2.  
  
    In het dialoogvenster **Data Processing** ziet u informatie over de verwerking van de verschillende partities. U ziet dat voor elke partitie een ander aantal rijen wordt overgebracht. Elke partitie bevat immers alleen rijen voor het jaar dat is opgegeven in de component WHERE in de SQL-instructie. Zodra de verwerking is voltooid, kunt u het dialoogvenster Data Processing sluiten.  
  
    ![aas-lesson10-process-complete](../tutorials/media/aas-lesson10-process-complete.png)
  
 ## <a name="whats-next"></a>Volgende stappen
Ga naar de volgende les: [Les 11: Rollen maken](../tutorials/aas-lesson-11-create-roles.md). 
